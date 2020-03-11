---
title: ASP.NET Core ile .NET için Web arabirimi 'ni (OWıN) açın
author: ardalis
description: ASP.NET Core, Web uygulamalarının Web sunucularından ayrılmasıyla, .NET için açık Web arabirimi 'ni (OWıN) nasıl desteklediğini öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/18/2018
uid: fundamentals/owin
ms.openlocfilehash: 14b23ba6d284413e20417bbd4142e19a656350ac
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666686"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a>ASP.NET Core ile .NET için Web arabirimi 'ni (OWıN) açın

, [Steve Smith](https://ardalis.com/) ve [Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

ASP.NET Core, .NET için açık Web arabirimi 'Ni (OWıN) destekler. OWIN, Web uygulamalarının Web sunucularından ayrılmasıyla izin verir. Bu işlem, ara yazılım için istekleri ve ilişkili yanıtları işlemek üzere bir ardışık düzende kullanılması için standart bir yol tanımlar. ASP.NET Core uygulamalar ve ara yazılım, OWıN tabanlı uygulamalar, sunucular ve ara yazılım ile birlikte çalışabilir.

OWIN, farklı nesne modellerinin birlikte kullanılmasına izin veren bir ayrılmış katman sağlar. `Microsoft.AspNetCore.Owin` paketi iki bağdaştırıcı uygulaması sağlar:

* OWıN 'a ASP.NET Core 
* ASP.NET Core için OWıN

Bu, ASP.NET Core bir OWIN uyumlu sunucu/konak üzerinde barındırılmasına veya diğer OWIN uyumlu bileşenlerin ASP.NET Core üzerinde çalıştırılmasını sağlar.

> [!NOTE]
> Bu bağdaştırıcıların kullanılması bir performans maliyetiyle birlikte gelir. Yalnızca ASP.NET Core bileşenleri kullanan uygulamalar `Microsoft.AspNetCore.Owin` paketi veya bağdaştırıcıları kullanmaz.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a>ASP.NET Core ardışık düzeninde OWıN ara yazılımı çalıştırma

ASP.NET Core 'nin OWıN desteği `Microsoft.AspNetCore.Owin` paketinin bir parçası olarak dağıtılır. Bu paketi yükleyerek, OWıN desteğini projenize aktarabilirsiniz.

OWıN ara yazılımı, `Func<IDictionary<string, object>, Task>` arabirimi gerektiren [owın belirtimine](https://owin.org/spec/spec/owin-1.0.0.html)uyar ve belirli anahtarlar ayarlanır (`owin.ResponseBody`gibi). Aşağıdaki basit OWıN ara yazılımı "Merhaba Dünya" görüntüler:

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: https://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

Örnek imza bir `Task` döndürür ve OWIN için gereken `IDictionary<string, object>` kabul eder.

Aşağıdaki kod, `UseOwin` uzantısı yöntemiyle ASP.NET Core işlem hattına `OwinHello` ara yazılımı (yukarıda gösterilen) nasıl ekleneceğini gösterir.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

OWıN ardışık düzeninde gerçekleşecek diğer eylemleri yapılandırabilirsiniz.

> [!NOTE]
> Yanıt üst bilgileri yalnızca yanıt akışına ilk yazmaya başlamadan önce değiştirilmelidir.

> [!NOTE]
> Performans nedenleriyle `UseOwin` birden çok çağrı önerilmez. OWIN bileşenleri, birlikte gruplandırılmışsa en iyi şekilde çalışır.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        return async environment =>
        {
            // Do something before.
            await next(environment);
            // Do something after.
        };
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-core-hosting-on-an-owin-based-server"></a>OWIN tabanlı bir sunucuda barındırma ASP.NET Core kullanma

OWIN tabanlı sunucular ASP.NET Core uygulamaları barındırabilirler. Bu tür bir sunucu, .NET OWıN Web sunucusu olan [Nowin](https://github.com/Bobris/Nowin)'dir. Bu makalenin örneğinde, Nowin 'a başvuran bir proje ekledik ve bunu kendi kendine barındırma ASP.NET Core özellikli bir `IServer` oluşturmak için kullanır.

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`, bir `Features` özelliği ve `Start` yöntemi gerektiren bir arabirimdir.

`Start`, sunucuyu yapılandırmadan ve başlatmaktan sorumludur; bu durumda, ıveraddressesözelliğinden Ayrıştırılan adresleri belirleyen bir dizi Fluent API çağrı aracılığıyla yapılır. `_builder` değişkeninin akıcı yapılandırmasının, isteklerin, yöntemin içinde daha önce tanımlanan `appFunc` tarafından işleneceğini belirtir. Bu `Func` gelen istekleri işlemek için her istekte çağrılır.

Nowin Server 'ın kolayca ekleneceğini ve yapılandırılacağını kolaylaştırmak için bir `IWebHostBuilder` uzantısı da ekleyeceğiz.

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

Bu özel sunucuyu kullanarak bir ASP.NET Core uygulaması çalıştırmak için bu uygulamada *program.cs* içindeki uzantıyı çağırın:

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

[ASP.NET Core sunucuları](xref:fundamentals/servers/index)hakkında daha fazla bilgi edinin.

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>OWIN tabanlı bir sunucuda ASP.NET Core çalıştırın ve WebSockets desteğini kullanın

ASP.NET Core ile OWıN tabanlı sunucuların özelliklerinin nasıl yararlanılabilir, WebSockets gibi özelliklere erişim için bir örnektir. Önceki örnekte kullanılan .NET OWIN Web sunucusu, içinde yerleşik olarak bulunan ve bir ASP.NET Core uygulaması tarafından yararlanılabilir olabilen Web Yuvaları desteği içerir. Aşağıdaki örnekte, Web yuvalarını destekleyen ve WebSockets üzerinden sunucuya gönderilen her şeyi yankılayan basit bir Web uygulaması gösterilmektedir.

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

Bu [örnek](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) , öncekiyle aynı `NowinServer` kullanılarak yapılandırılır-tek fark, uygulamanın `Configure` yönteminde nasıl yapılandırıldığına ilişkin bir yöntemdir. [Basit bir WebSocket istemcisi](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) kullanan bir test, uygulamayı gösterir:

![Web yuvası test Istemcisi](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>OWIN ortamı

`HttpContext`kullanarak bir OWıN ortamı oluşturabilirsiniz.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>OWıN tuşları

OWIN, bir HTTP Isteği/yanıt değişimi boyunca bilgi iletmek için bir `IDictionary<string,object>` nesnesine bağlıdır. ASP.NET Core aşağıda listelenen anahtarları uygular. Bkz. [birincil belirtim, uzantılar](https://owin.org/#spec)ve [Owın anahtar kılavuzları ve ortak anahtarlar](https://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>İstek verileri (OWıN v 1.0.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owın. RequestScheme | `String` |  |
| owın. RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owın. RequestHeaders | `IDictionary<string,string[]>`  | |
| owın. Istek gövdesi | `Stream`  | |

### <a name="request-data-owin-v110"></a>İstek verileri (OWıN v 1.1.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owin.RequestId | `String` | İsteğe bağlı |

### <a name="response-data-owin-v100"></a>Yanıt verileri (OWIN v 1.0.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | İsteğe bağlı |
| owın. ResponseReasonPhrase | `String` | İsteğe bağlı |
| owın. ResponseHeaders | `IDictionary<string,string[]>`  | |
| owın. Yanıt gövdesi | `Stream`  | |

### <a name="other-data-owin-v100"></a>Diğer veriler (OWIN v 1.0.0)

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| owin.CallCancelled | `CancellationToken` |  |
| owın. Sürüm  | `String` | |   

### <a name="common-keys"></a>Ortak anahtarlar

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| SSL. ClientCertificate | `X509Certificate` |  |
| SSL. LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| Server. IsLocal  | `bool` | |    
| server.OnSendingHeaders  | `Action<Action<object>,object>` | |

### <a name="sendfiles-v030"></a>SendFiles v 0.3.0

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| SendFile. SendAsync | Bkz. [temsilci imzası](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | Istek başına |

### <a name="opaque-v030"></a>Donuk v 0.3.0

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| ş. Sürüm | `String` |  |
| ş. Yükseltmenizi | `OpaqueUpgrade` | Bkz. [temsilci imzası](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| ş. Ka | `Stream` |  |
| ş. Calliptal edildi | `CancellationToken` |  |

### <a name="websocket-v030"></a>WebSocket v 0.3.0

| Anahtar               | Değer (tür) | Açıklama |
| ----------------- | ------------ | ----------- |
| WebSocket. Sürüm | `String` |  |
| WebSocket. Ettiğinizde | `WebSocketAccept` | Bkz. [temsilci imzası](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| WebSocket. AcceptAlt |  | Spec dışı |
| WebSocket. Alt protokolü | `String` | Bkz. [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5,5 |
| WebSocket. SendAsync | `WebSocketSendAsync` | Bkz. [temsilci imzası](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | Bkz. [temsilci imzası](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | Bkz. [temsilci imzası](https://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| WebSocket. Calliptal edildi | `CancellationToken` |  |
| WebSocket. ClientCloseStatus | `int` | İsteğe bağlı |
| websocket.ClientCloseDescription | `String` | İsteğe bağlı |

## <a name="additional-resources"></a>Ek kaynaklar

* [Ara Yazılım](xref:fundamentals/middleware/index)
* [Sunucular](xref:fundamentals/servers/index)
