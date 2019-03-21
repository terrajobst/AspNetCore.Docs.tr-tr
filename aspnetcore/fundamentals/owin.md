---
title: ASP.NET Core ile .NET (OWIN) için Web arabirimini açın
author: ardalis
description: ASP.NET Core açık Web arabirimi için .NET (web sunucularından ölçeklendirilebilmeleri web apps sağlar OWIN), nasıl desteklediğini öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/18/2018
uid: fundamentals/owin
ms.openlocfilehash: de8548fe2396102fe44a8e6a000b44d4eea63285
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209943"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a>ASP.NET Core ile .NET (OWIN) için Web arabirimini açın

Tarafından [Steve Smith](https://ardalis.com/) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core, .NET (OWIN) için açık Web arabirimi destekler. OWIN web uygulamalarının web sunucularından ölçeklendirilebilmeleri sağlar. Bu, bir işlem hattında isteklerini ve yanıtlarını ilişkili işlemek için kullanılan ara yazılımı için standart bir biçimde tanımlar. ASP.NET Core uygulamaları ve ara yazılımlar OWIN tabanlı uygulamalar, sunucuları ve ara yazılım ile çalışabilirler.

OWIN iki çerçeveleri ile birlikte kullanılmak üzere farklı nesne modeline izin veren bir decoupling katmanı sağlar. `Microsoft.AspNetCore.Owin` Paket iki bağdaştırıcı uygulamaları sağlar:

* OWIN için ASP.NET Core 
* OWIN ASP.NET core'a

Bu, ASP.NET Core, ASP.NET Core üzerinde çalıştırılacak diğer OWIN uyumlu bileşenler için ya da bir OWIN uyumlu sunucusu/ana bilgisayar üzerinde barındırılması sağlar.

> [!NOTE]
> Bu bağdaştırıcılar kullanarak performans maliyetine ile birlikte gelir. Yalnızca ASP.NET Core bileşenlerini kullanarak uygulama olmamalıdır kullanın `Microsoft.AspNetCore.Owin` paketi veya bağdaştırıcıları.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a>ASP.NET Core işlem hattında OWIN ara yazılımı çalıştırma

ASP.NET Core'nın OWIN desteği parçası olarak dağıtılan `Microsoft.AspNetCore.Owin` paket. Bu paketi yükleyerek projenize OWIN destek alabilirsiniz.

OWIN ara yazılımı uygun şekilde [OWIN belirtimi](http://owin.org/spec/spec/owin-1.0.0.html), gerektiren bir `Func<IDictionary<string, object>, Task>` arabirimi ve özel anahtarları ayarlanabilir (gibi `owin.ResponseBody`). Aşağıdaki basit OWIN ara yazılımı "Hello World" görüntüler:

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

Örnek imza döndürür bir `Task` ve kabul eden bir `IDictionary<string, object>` OWIN gerektirdiği.

Aşağıdaki kod nasıl ekleneceğini gösterir `OwinHello` (yukarıda ASP.NET Core işlem hattı ile gösterilen) bir ara yazılım `UseOwin` genişletme yöntemi.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

OWIN ardışık düzenini içinde gerçekleşmesi için diğer eylemleri yapılandırabilirsiniz.

> [!NOTE]
> Yanıt üst bilgilerini yalnızca yanıt akışa ilk Yazımdan önce düzeltilmelidir.

> [!NOTE]
> Birden çok çağrılar `UseOwin` Performans nedeniyle önerilmez. OWIN bileşenleri, gruplandırılmış, en iyi şekilde çalışır.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(async (next) =>
    {
        // do something before
        await OwinHello(new OwinEnvironment(HttpContext));
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-core-hosting-on-an-owin-based-server"></a>Bir OWIN tabanlı sunucu üzerinde kullanarak ASP.NET Core barındırma

OWIN tabanlı sunucular, ASP.NET Core uygulamaları barındırabilir. Bir sunucu [Nowin](https://github.com/Bobris/Nowin), bir .NET OWIN web sunucusu. Bu makalede örnek miyim Nowin atıfta bulunan ve oluşturmak için kullandığı bir proje dahil ettiğiniz bir `IServer` ASP.NET Core kendi kendine barındırma yeteneğine.

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer` gerektiren bir arabirim bir `Features` özelliği ve `Start` yöntemi.

`Start` Yapılandırma ve bu durumda bir dizi IServerAddressesFeature Ayrıştırılan adreslerini ayarlamak fluent API'si çağrısı yapılır sunucunun başlatmak için sorumludur. Unutmayın fluent yapılandırmasını `_builder` değişkeni, istek tarafından işleneceğini belirtir `appFunc` yönteminde tanımlanmış. Bu `Func` her istekte gelen istekleri işlemek için çağrılır.

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

Bu yerinde uzantı çağırmak *Program.cs* bu özel sunucu kullanarak ASP.NET Core uygulaması çalıştırmak için:

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

Daha fazla bilgi edinin [ASP.NET Core sunucuları](xref:fundamentals/servers/index).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>ASP.NET Core OWIN tabanlı bir sunucuda çalıştırın ve WebSockets desteğini kullanma

OWIN tabanlı nasıl sunucularının özelliklerinin başka bir örnek tarafından yararlanılabilir ASP.NET Core, WebSockets gibi özelliklere erişim. Önceki örnekte kullanılan .NET OWIN web sunucusunun Web yerleşik olan bir ASP.NET Core uygulaması tarafından yararlanılabilir yuva için destek sunar. Aşağıdaki örnek, Web yuvalarını destekliyorsa ve WebSockets üzerinden sunucusuna gönderilen her şeyi yankılayan basit web uygulaması gösterir.

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

Bu [örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) aynı kullanılarak yapılandırılan `NowinServer` nasıl uygulama yapılandırılan içinde önceki tek - tek fark, kendi `Configure` yöntemi. Kullanarak bir test [basit websocket istemcisi](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) uygulaması gösterir:

![Web yuvası Test İstemcisi](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>OWIN ortamı

Bir OWIN ortamı kullanarak oluşturabilirsiniz `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>OWIN anahtarları

OWIN bağımlı bir `IDictionary<string,object>` bir HTTP istek/yanıt exchange içinde bilgi iletişim kurmak için nesne. ASP.NET Core, aşağıda listelenen anahtarları uygular. Bkz: [birincil belirtimi, uzantıları](http://owin.org/#spec), ve [OWIN anahtar yönergeleri ve ortak anahtarları](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>İstek verileri (OWIN v1.0.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owın. RequestScheme | `String` |  |
| owın. RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owın. RequestProtocol  | `String` | |    
| owın. RequestHeaders | `IDictionary<string,string[]>`  | |
| owın. Includesearchresults: true | `Stream`  | |

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
| owın. ResponseBody | `Stream`  | |

### <a name="other-data-owin-v100"></a>Diğer veri (OWIN v1.0.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owın. CallCancelled | `CancellationToken` |  |
| owın. Sürüm  | `String` | |   

### <a name="common-keys"></a>Ortak anahtarlar

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| SSL. ClientCertificate | `X509Certificate` |  |
| SSL. LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| Sunucu. Uzak bağlantı noktası | `String` | |     
| Sunucu. YerelIPAdresi  | `String` | |    
| Sunucu. Yerel bağlantı noktası  | `String` | |    
| Sunucu. IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |

### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| sendfile. SendAsync | Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | İstek başına |

### <a name="opaque-v030"></a>Donuk v0.3.0

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| Donuk. Sürüm | `String` |  |
| Donuk. Yükseltme | `OpaqueUpgrade` | Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| Donuk. Stream | `Stream` |  |
| Donuk. CallCancelled | `CancellationToken` |  |

### <a name="websocket-v030"></a>WebSocket v0.3.0

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| websocket. Sürüm | `String` |  |
| websocket. Kabul et | `WebSocketAccept` | Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket. AcceptAlt |  | Non-spec |
| websocket. Geçersizdir | `String` | Bkz: [RFC6455 bölüm 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) adım 5.5 |
| websocket. SendAsync | `WebSocketSendAsync` | Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket. CallCancelled | `CancellationToken` |  |
| websocket. ClientCloseStatus | `int` | İsteğe Bağlı |
| websocket.ClientCloseDescription | `String` | İsteğe Bağlı |

## <a name="additional-resources"></a>Ek kaynaklar

* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Sunucular](xref:fundamentals/servers/index)
