---
title: ".NET (OWIN) için açık Web arabirimi"
author: ardalis
description: "ASP.NET Core açık Web arabirimi için .NET (hangi web uygulamalarının web sunucularından ayrılmış sağlar OWIN), nasıl desteklediği bulur."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/owin
ms.openlocfilehash: 91e59d8568434867e10869b4db22bce9935ce573
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>.NET (OWIN) için Web Arabirimi'ni açmak için giriş

Tarafından [Steve Smith](https://ardalis.com/) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core açık Web arabirimi .NET (OWIN) destekler. OWIN web uygulamalarının web sunucularından ayrılmış sağlar. Ardışık düzeninde istekleri ve ilişkili yanıtları işlemek için kullanılan ara yazılımı için standart bir biçimde tanımlar. ASP.NET Core uygulamaları ve ara yazılım OWIN tabanlı uygulamalar, sunucuları ve ara yazılımı ile çalışabilirler.

OWIN birlikte kullanılmak üzere farklı nesne modeline iki çerçeveleri izin veren bir decoupling katmanı sağlar. `Microsoft.AspNetCore.Owin` Paket iki bağdaştırıcı uygulamaları sağlar:
- OWIN için ASP.NET Çekirdeği 
- ASP.NET Core OWIN

Bir OWIN uyumlu sunucu/ana bilgisayar üstünde veya ASP.NET Core üzerinde çalıştırılacak diğer OWIN uyumlu bileşenlerine yönelik barındırılmasını ASP.NET Core sağlar.

Not: Bu bağdaştırıcılar kullanarak bir performans ile gelir. Yalnızca ASP.NET Core bileşenleri kullanan uygulamalar Owın paket ya da bağdaştırıcıları kullanmamalısınız.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>ASP.NET ardışık düzeninde OWIN ara yazılımı çalıştırma

ASP.NET Core'nın OWIN destek parçası olarak dağıtılan `Microsoft.AspNetCore.Owin` paket. Bu paketi yüklerken projenize OWIN destek alabilirsiniz.

OWIN ara yazılımı uyan için [OWIN belirtimi](http://owin.org/spec/spec/owin-1.0.0.html), gerektiren bir `Func<IDictionary<string, object>, Task>` arabirimi ve özel anahtarları ayarlanabilir (gibi `owin.ResponseBody`). Aşağıdaki basit OWIN ara yazılımı "Hello World" görüntüler:

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

Örnek imza döndüren bir `Task` ve kabul eden bir `IDictionary<string, object>` OWIN gerektirdiği.

Aşağıdaki kodu nasıl ekleneceğini gösterir `OwinHello` (yukarıda için ASP.NET ardışık düzenini ile gösterilen) Ara `UseOwin` genişletme yöntemi.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

OWIN ardışık düzenini içinde gerçekleşmesi için başka eylemler yapılandırabilirsiniz.

> [!NOTE]
> Yanıt Üstbilgileri yanıt akışa ilk Yazımdan önce yalnızca değiştirilmesi gerekir.

> [!NOTE]
> Birden çok çağrılar `UseOwin` performans nedenleriyle önerilmez. OWIN bileşenleri bir arada gruplandırılmış, en iyi şekilde çalışır.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>Bir OWIN tabanlı sunucu üzerinde kullanarak ASP.NET barındırma

OWIN tabanlı sunucular ASP.NET uygulamalarını barındırabilir. Bu tür bir sunucu [Nowin](https://github.com/Bobris/Nowin), bir .NET OWIN web sunucusu. Bu makalede örnek t Nowin başvuruyor ve oluşturmak için kullandığı bir proje dahil ettiğiniz bir `IServer` ASP.NET Core kendi kendine barındırma yeteneğine sahip.

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`gerektiren bir arabirim bir `Features` özelliği ve `Start` yöntemi.

`Start`Yapılandırma ve bu durumda bir dizi IServerAddressesFeature Ayrıştırılan adreslerini ayarlamak fluent API çağrısı yoluyla yapılır sunucu başlangıç sorumludur. Unutmayın fluent yapılandırmasını `_builder` değişkeni belirtir istekleri tarafından işlenecek `appFunc` yöntemi önceden tanımlanmış. Bu `Func` her istekte gelen istekleri işlemek üzere çağrılır.

Ayrıca ekleyeceğiz bir `IWebHostBuilder` Nowin sunucu ekleme ve yapılandırma kolaylaştırmak için uzantı.

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

Bunun yerine, tüm uzantısı çağırmak için bu özel sunucu kullanarak bir ASP.NET uygulamasını çalıştırılması için gerekli *Program.cs*:

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

ASP.NET hakkında daha fazla bilgi [sunucuları](servers/index.md).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Bir OWIN tabanlı sunucu üzerinde ASP.NET Core çalıştırın ve WebSockets desteğini kullanın

OWIN tabanlı nasıl sunucularındaki özellikleri başka bir örneği tarafından yararlanılabilir ASP.NET çekirdeği WebSockets gibi özellikleri erişimi olur. Önceki örnekte kullanılan .NET OWIN web sunucusu Web, yerleşik olan bir ASP.NET Core uygulama tarafından yararlanılabilir yuva desteği vardır. Aşağıdaki örnek, Web yuvalarını destekliyorsa ve geri WebSockets aracılığıyla sunucuya gönderilen her şeyi görüntülemektedir bir basit bir web uygulaması gösterir.

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

Bu [örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) aynı kullanılarak yapılandırılmış `NowinServer` önceki bir - yalnızca nasıl uygulama yapılandırılan içinde farktır kendi `Configure` yöntemi. Kullanarak bir test [basit websocket istemci](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) uygulaması gösterir:

![Web yuvası Test İstemcisi](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>OWIN ortamı

Bir OWIN ortamı kullanarak oluşturabileceğiniz `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>OWIN anahtarları

OWIN bağımlı bir `IDictionary<string,object>` bir HTTP istek/yanıt exchange genelinde bilgileri iletişim kurmak için nesne. ASP.NET Core aşağıda listelenen anahtarları uygular. Bkz: [birincil belirtimi, uzantıları](http://owin.org/#spec), ve [OWIN anahtarı kılavuzları ve ortak anahtarlar](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>İstek verileri (OWIN v1.0.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin.RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin.RequestHeaders | `IDictionary<string,string[]>`  | |
| owin.RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>İstek verileri (OWIN v1.1.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owin.RequestId | `String` | İsteğe Bağlı |

### <a name="response-data-owin-v100"></a>Yanıt verileri (OWIN v1.0.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | İsteğe Bağlı |
| owin.ResponseReasonPhrase | `String` | İsteğe Bağlı |
| owın. ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin.ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Diğer veri (OWIN v1.0.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owın. CallCancelled | `CancellationToken` |  |
| owin.Version  | `String` | |   


### <a name="common-keys"></a>Ortak anahtarlar

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| Sunucu. Yerel bağlantı noktası  | `String` | |    
| server.IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | İstek başına |


### <a name="opaque-v030"></a>Opaque v0.3.0

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| Donuk. Yükseltme | `OpaqueUpgrade` | Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| Donuk. Akış | `Stream` |  |
| Donuk. CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| websocket.Version | `String` |  |
| websocket. Kabul et | `WebSocketAccept` | Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket.AcceptAlt |  | Olmayan belirtimi |
| websocket.SubProtocol | `String` | Bkz: [RFC6455 bölüm 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) adım 5.5 |
| websocket.SendAsync | `WebSocketSendAsync` | Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket. CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | İsteğe Bağlı |
| websocket.ClientCloseDescription | `String` | İsteğe Bağlı |

## <a name="additional-resources"></a>Ek kaynaklar

* [Ara Yazılım](xref:fundamentals/middleware)
* [Sunucular](xref:fundamentals/servers/index)
