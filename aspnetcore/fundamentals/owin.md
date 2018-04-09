---
title: ASP.NET çekirdeği ile .NET (OWIN) için Web Arabirimi'ni açmak
author: ardalis
description: ASP.NET Core açık Web arabirimi için .NET (hangi web uygulamalarının web sunucularından ayrılmış sağlar OWIN), nasıl desteklediği bulur.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/owin
ms.openlocfilehash: 7c379ddbcc1f66ba9f7a4d0a50a4607394fed0af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a><span data-ttu-id="8290d-103">ASP.NET çekirdeği ile .NET (OWIN) için Web Arabirimi'ni açmak</span><span class="sxs-lookup"><span data-stu-id="8290d-103">Open Web Interface for .NET (OWIN) with ASP.NET Core</span></span>

<span data-ttu-id="8290d-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8290d-104">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8290d-105">ASP.NET Core açık Web arabirimi .NET (OWIN) destekler.</span><span class="sxs-lookup"><span data-stu-id="8290d-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="8290d-106">OWIN web uygulamalarının web sunucularından ayrılmış sağlar.</span><span class="sxs-lookup"><span data-stu-id="8290d-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="8290d-107">Ardışık düzeninde istekleri ve ilişkili yanıtları işlemek için kullanılan ara yazılımı için standart bir biçimde tanımlar.</span><span class="sxs-lookup"><span data-stu-id="8290d-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="8290d-108">ASP.NET Core uygulamaları ve ara yazılım OWIN tabanlı uygulamalar, sunucuları ve ara yazılımı ile çalışabilirler.</span><span class="sxs-lookup"><span data-stu-id="8290d-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="8290d-109">OWIN birlikte kullanılmak üzere farklı nesne modeline iki çerçeveleri izin veren bir decoupling katmanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="8290d-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="8290d-110">`Microsoft.AspNetCore.Owin` Paket iki bağdaştırıcı uygulamaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="8290d-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="8290d-111">OWIN için ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="8290d-111">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="8290d-112">ASP.NET Core OWIN</span><span class="sxs-lookup"><span data-stu-id="8290d-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="8290d-113">Bir OWIN uyumlu sunucu/ana bilgisayar üstünde veya ASP.NET Core üzerinde çalıştırılacak diğer OWIN uyumlu bileşenlerine yönelik barındırılmasını ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="8290d-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="8290d-114">Not: Bu bağdaştırıcılar kullanarak bir performans ile gelir.</span><span class="sxs-lookup"><span data-stu-id="8290d-114">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="8290d-115">Yalnızca ASP.NET Core bileşenleri kullanan uygulamalar Owın paket ya da bağdaştırıcıları kullanmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="8290d-115">Applications using only ASP.NET Core components shouldn't use the Owin package or adapters.</span></span>

<span data-ttu-id="8290d-116">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8290d-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="8290d-117">ASP.NET ardışık düzeninde OWIN ara yazılımı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="8290d-117">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="8290d-118">ASP.NET Core'nın OWIN destek parçası olarak dağıtılan `Microsoft.AspNetCore.Owin` paket.</span><span class="sxs-lookup"><span data-stu-id="8290d-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="8290d-119">Bu paketi yüklerken projenize OWIN destek alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8290d-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="8290d-120">OWIN ara yazılımı uyan için [OWIN belirtimi](http://owin.org/spec/spec/owin-1.0.0.html), gerektiren bir `Func<IDictionary<string, object>, Task>` arabirimi ve özel anahtarları ayarlanabilir (gibi `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="8290d-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="8290d-121">Aşağıdaki basit OWIN ara yazılımı "Hello World" görüntüler:</span><span class="sxs-lookup"><span data-stu-id="8290d-121">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="8290d-122">Örnek imza döndüren bir `Task` ve kabul eden bir `IDictionary<string, object>` OWIN gerektirdiği.</span><span class="sxs-lookup"><span data-stu-id="8290d-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="8290d-123">Aşağıdaki kodu nasıl ekleneceğini gösterir `OwinHello` (yukarıda için ASP.NET ardışık düzenini ile gösterilen) Ara `UseOwin` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8290d-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="8290d-124">OWIN ardışık düzenini içinde gerçekleşmesi için başka eylemler yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8290d-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="8290d-125">Yanıt Üstbilgileri yanıt akışa ilk Yazımdan önce yalnızca değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="8290d-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="8290d-126">Birden çok çağrılar `UseOwin` performans nedenleriyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="8290d-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="8290d-127">OWIN bileşenleri bir arada gruplandırılmış, en iyi şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="8290d-127">OWIN components will operate best if grouped together.</span></span>

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

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="8290d-128">Bir OWIN tabanlı sunucu üzerinde kullanarak ASP.NET barındırma</span><span class="sxs-lookup"><span data-stu-id="8290d-128">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="8290d-129">OWIN tabanlı sunucular ASP.NET uygulamalarını barındırabilir.</span><span class="sxs-lookup"><span data-stu-id="8290d-129">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="8290d-130">Bu tür bir sunucu [Nowin](https://github.com/Bobris/Nowin), bir .NET OWIN web sunucusu.</span><span class="sxs-lookup"><span data-stu-id="8290d-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="8290d-131">Bu makalede örnek t Nowin başvuruyor ve oluşturmak için kullandığı bir proje dahil ettiğiniz bir `IServer` ASP.NET Core kendi kendine barındırma yeteneğine sahip.</span><span class="sxs-lookup"><span data-stu-id="8290d-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="8290d-132">`IServer` gerektiren bir arabirim bir `Features` özelliği ve `Start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8290d-132">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="8290d-133">`Start` Yapılandırma ve bu durumda bir dizi IServerAddressesFeature Ayrıştırılan adreslerini ayarlamak fluent API çağrısı yoluyla yapılır sunucu başlangıç sorumludur.</span><span class="sxs-lookup"><span data-stu-id="8290d-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="8290d-134">Unutmayın fluent yapılandırmasını `_builder` değişkeni belirtir istekleri tarafından işlenecek `appFunc` yöntemi önceden tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="8290d-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="8290d-135">Bu `Func` her istekte gelen istekleri işlemek üzere çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8290d-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="8290d-136">Ayrıca ekleyeceğiz bir `IWebHostBuilder` Nowin sunucu ekleme ve yapılandırma kolaylaştırmak için uzantı.</span><span class="sxs-lookup"><span data-stu-id="8290d-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="8290d-137">Bunun yerine, tüm uzantısı çağırmak için bu özel sunucu kullanarak bir ASP.NET uygulamasını çalıştırılması için gerekli *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="8290d-137">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

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

<span data-ttu-id="8290d-138">ASP.NET hakkında daha fazla bilgi [sunucuları](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8290d-138">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="8290d-139">Bir OWIN tabanlı sunucu üzerinde ASP.NET Core çalıştırın ve WebSockets desteğini kullanın</span><span class="sxs-lookup"><span data-stu-id="8290d-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="8290d-140">OWIN tabanlı nasıl sunucularındaki özellikleri başka bir örneği tarafından yararlanılabilir ASP.NET çekirdeği WebSockets gibi özellikleri erişimi olur.</span><span class="sxs-lookup"><span data-stu-id="8290d-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="8290d-141">Önceki örnekte kullanılan .NET OWIN web sunucusu Web, yerleşik olan bir ASP.NET Core uygulama tarafından yararlanılabilir yuva desteği vardır.</span><span class="sxs-lookup"><span data-stu-id="8290d-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="8290d-142">Aşağıdaki örnek, Web yuvalarını destekliyorsa ve geri WebSockets aracılığıyla sunucuya gönderilen her şeyi görüntülemektedir bir basit bir web uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="8290d-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="8290d-143">Bu [örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) aynı kullanılarak yapılandırılmış `NowinServer` önceki bir - yalnızca nasıl uygulama yapılandırılan içinde farktır kendi `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8290d-143">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="8290d-144">Kullanarak bir test [basit websocket istemci](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) uygulaması gösterir:</span><span class="sxs-lookup"><span data-stu-id="8290d-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web yuvası Test İstemcisi](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="8290d-146">OWIN ortamı</span><span class="sxs-lookup"><span data-stu-id="8290d-146">OWIN environment</span></span>

<span data-ttu-id="8290d-147">Bir OWIN ortamı kullanarak oluşturabileceğiniz `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="8290d-147">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="8290d-148">OWIN anahtarları</span><span class="sxs-lookup"><span data-stu-id="8290d-148">OWIN keys</span></span>

<span data-ttu-id="8290d-149">OWIN bağımlı bir `IDictionary<string,object>` bir HTTP istek/yanıt exchange genelinde bilgileri iletişim kurmak için nesne.</span><span class="sxs-lookup"><span data-stu-id="8290d-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="8290d-150">ASP.NET Core aşağıda listelenen anahtarları uygular.</span><span class="sxs-lookup"><span data-stu-id="8290d-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="8290d-151">Bkz: [birincil belirtimi, uzantıları](http://owin.org/#spec), ve [OWIN anahtarı kılavuzları ve ortak anahtarlar](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="8290d-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="8290d-152">İstek verileri (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="8290d-152">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="8290d-153">Anahtar</span><span class="sxs-lookup"><span data-stu-id="8290d-153">Key</span></span>               | <span data-ttu-id="8290d-154">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="8290d-154">Value (type)</span></span> | <span data-ttu-id="8290d-155">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8290d-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8290d-156">owin.RequestScheme</span><span class="sxs-lookup"><span data-stu-id="8290d-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="8290d-157">owin.RequestMethod</span><span class="sxs-lookup"><span data-stu-id="8290d-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="8290d-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="8290d-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="8290d-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="8290d-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="8290d-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="8290d-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="8290d-161">owin.RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="8290d-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="8290d-162">owin.RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="8290d-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="8290d-163">owin.RequestBody</span><span class="sxs-lookup"><span data-stu-id="8290d-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="8290d-164">İstek verileri (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="8290d-164">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="8290d-165">Anahtar</span><span class="sxs-lookup"><span data-stu-id="8290d-165">Key</span></span>               | <span data-ttu-id="8290d-166">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="8290d-166">Value (type)</span></span> | <span data-ttu-id="8290d-167">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8290d-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8290d-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="8290d-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="8290d-169">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="8290d-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="8290d-170">Yanıt verileri (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="8290d-170">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="8290d-171">Anahtar</span><span class="sxs-lookup"><span data-stu-id="8290d-171">Key</span></span>               | <span data-ttu-id="8290d-172">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="8290d-172">Value (type)</span></span> | <span data-ttu-id="8290d-173">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8290d-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8290d-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="8290d-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="8290d-175">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="8290d-175">Optional</span></span> |
| <span data-ttu-id="8290d-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="8290d-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="8290d-177">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="8290d-177">Optional</span></span> |
| <span data-ttu-id="8290d-178">owın. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="8290d-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="8290d-179">owin.ResponseBody</span><span class="sxs-lookup"><span data-stu-id="8290d-179">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="8290d-180">Diğer veri (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="8290d-180">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="8290d-181">Anahtar</span><span class="sxs-lookup"><span data-stu-id="8290d-181">Key</span></span>               | <span data-ttu-id="8290d-182">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="8290d-182">Value (type)</span></span> | <span data-ttu-id="8290d-183">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8290d-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8290d-184">owın. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="8290d-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="8290d-185">owin.Version</span><span class="sxs-lookup"><span data-stu-id="8290d-185">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="8290d-186">Ortak anahtarlar</span><span class="sxs-lookup"><span data-stu-id="8290d-186">Common keys</span></span>

| <span data-ttu-id="8290d-187">Anahtar</span><span class="sxs-lookup"><span data-stu-id="8290d-187">Key</span></span>               | <span data-ttu-id="8290d-188">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="8290d-188">Value (type)</span></span> | <span data-ttu-id="8290d-189">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8290d-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8290d-190">ssl.ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="8290d-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="8290d-191">ssl.LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="8290d-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="8290d-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="8290d-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="8290d-193">server.RemotePort</span><span class="sxs-lookup"><span data-stu-id="8290d-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="8290d-194">server.LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="8290d-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="8290d-195">Sunucu. Yerel bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="8290d-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="8290d-196">server.IsLocal</span><span class="sxs-lookup"><span data-stu-id="8290d-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="8290d-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="8290d-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="8290d-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="8290d-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="8290d-199">Anahtar</span><span class="sxs-lookup"><span data-stu-id="8290d-199">Key</span></span>               | <span data-ttu-id="8290d-200">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="8290d-200">Value (type)</span></span> | <span data-ttu-id="8290d-201">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8290d-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8290d-202">sendfile.SendAsync</span><span class="sxs-lookup"><span data-stu-id="8290d-202">sendfile.SendAsync</span></span> | <span data-ttu-id="8290d-203">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8290d-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="8290d-204">İstek başına</span><span class="sxs-lookup"><span data-stu-id="8290d-204">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="8290d-205">Opaque v0.3.0</span><span class="sxs-lookup"><span data-stu-id="8290d-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="8290d-206">Anahtar</span><span class="sxs-lookup"><span data-stu-id="8290d-206">Key</span></span>               | <span data-ttu-id="8290d-207">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="8290d-207">Value (type)</span></span> | <span data-ttu-id="8290d-208">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8290d-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8290d-209">opaque.Version</span><span class="sxs-lookup"><span data-stu-id="8290d-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="8290d-210">Donuk. Yükseltme</span><span class="sxs-lookup"><span data-stu-id="8290d-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="8290d-211">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8290d-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="8290d-212">Donuk. Akış</span><span class="sxs-lookup"><span data-stu-id="8290d-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="8290d-213">Donuk. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="8290d-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="8290d-214">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="8290d-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="8290d-215">Anahtar</span><span class="sxs-lookup"><span data-stu-id="8290d-215">Key</span></span>               | <span data-ttu-id="8290d-216">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="8290d-216">Value (type)</span></span> | <span data-ttu-id="8290d-217">Açıklama</span><span class="sxs-lookup"><span data-stu-id="8290d-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="8290d-218">websocket.Version</span><span class="sxs-lookup"><span data-stu-id="8290d-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="8290d-219">websocket. Kabul et</span><span class="sxs-lookup"><span data-stu-id="8290d-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="8290d-220">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8290d-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="8290d-221">websocket.AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="8290d-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="8290d-222">Olmayan belirtimi</span><span class="sxs-lookup"><span data-stu-id="8290d-222">Non-spec</span></span> |
| <span data-ttu-id="8290d-223">websocket.SubProtocol</span><span class="sxs-lookup"><span data-stu-id="8290d-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="8290d-224">Bkz: [RFC6455 bölüm 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) adım 5.5</span><span class="sxs-lookup"><span data-stu-id="8290d-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="8290d-225">websocket.SendAsync</span><span class="sxs-lookup"><span data-stu-id="8290d-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="8290d-226">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8290d-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="8290d-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="8290d-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="8290d-228">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8290d-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="8290d-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="8290d-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="8290d-230">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="8290d-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="8290d-231">websocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="8290d-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="8290d-232">websocket.ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="8290d-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="8290d-233">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="8290d-233">Optional</span></span> |
| <span data-ttu-id="8290d-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="8290d-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="8290d-235">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="8290d-235">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="8290d-236">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8290d-236">Additional resources</span></span>

* [<span data-ttu-id="8290d-237">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="8290d-237">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="8290d-238">Sunucular</span><span class="sxs-lookup"><span data-stu-id="8290d-238">Servers</span></span>](xref:fundamentals/servers/index)
