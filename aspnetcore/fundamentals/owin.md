---
title: ASP.NET Core ile .NET (OWIN) için Web arabirimini açın
author: ardalis
description: ASP.NET Core açık Web arabirimi için .NET (web sunucularından ölçeklendirilebilmeleri web apps sağlar OWIN), nasıl desteklediğini öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/18/2018
uid: fundamentals/owin
ms.openlocfilehash: 9d6ce79c15fe768c260c6361ac3babecab5f3f9b
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087295"
---
# <a name="open-web-interface-for-net-owin-with-aspnet-core"></a><span data-ttu-id="88fb1-103">ASP.NET Core ile .NET (OWIN) için Web arabirimini açın</span><span class="sxs-lookup"><span data-stu-id="88fb1-103">Open Web Interface for .NET (OWIN) with ASP.NET Core</span></span>

<span data-ttu-id="88fb1-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="88fb1-104">By [Steve Smith](https://ardalis.com/) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="88fb1-105">ASP.NET Core, .NET (OWIN) için açık Web arabirimi destekler.</span><span class="sxs-lookup"><span data-stu-id="88fb1-105">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="88fb1-106">OWIN web uygulamalarının web sunucularından ölçeklendirilebilmeleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="88fb1-106">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="88fb1-107">Bu, bir işlem hattında isteklerini ve yanıtlarını ilişkili işlemek için kullanılan ara yazılımı için standart bir biçimde tanımlar.</span><span class="sxs-lookup"><span data-stu-id="88fb1-107">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="88fb1-108">ASP.NET Core uygulamaları ve ara yazılımlar OWIN tabanlı uygulamalar, sunucuları ve ara yazılım ile çalışabilirler.</span><span class="sxs-lookup"><span data-stu-id="88fb1-108">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="88fb1-109">OWIN iki çerçeveleri ile birlikte kullanılmak üzere farklı nesne modeline izin veren bir decoupling katmanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="88fb1-109">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="88fb1-110">`Microsoft.AspNetCore.Owin` Paket iki bağdaştırıcı uygulamaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="88fb1-110">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>

* <span data-ttu-id="88fb1-111">OWIN için ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88fb1-111">ASP.NET Core to OWIN</span></span> 
* <span data-ttu-id="88fb1-112">OWIN ASP.NET core'a</span><span class="sxs-lookup"><span data-stu-id="88fb1-112">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="88fb1-113">Bu, ASP.NET Core, ASP.NET Core üzerinde çalıştırılacak diğer OWIN uyumlu bileşenler için ya da bir OWIN uyumlu sunucusu/ana bilgisayar üzerinde barındırılması sağlar.</span><span class="sxs-lookup"><span data-stu-id="88fb1-113">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="88fb1-114">Bu bağdaştırıcılar kullanarak performans maliyetine ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="88fb1-114">Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="88fb1-115">Yalnızca ASP.NET Core bileşenlerini kullanarak uygulama olmamalıdır kullanın `Microsoft.AspNetCore.Owin` paketi veya bağdaştırıcıları.</span><span class="sxs-lookup"><span data-stu-id="88fb1-115">Apps using only ASP.NET Core components shouldn't use the `Microsoft.AspNetCore.Owin` package or adapters.</span></span>

<span data-ttu-id="88fb1-116">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="88fb1-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-core-pipeline"></a><span data-ttu-id="88fb1-117">ASP.NET Core işlem hattında OWIN ara yazılımı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="88fb1-117">Running OWIN middleware in the ASP.NET Core pipeline</span></span>

<span data-ttu-id="88fb1-118">ASP.NET Core'nın OWIN desteği parçası olarak dağıtılan `Microsoft.AspNetCore.Owin` paket.</span><span class="sxs-lookup"><span data-stu-id="88fb1-118">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="88fb1-119">Bu paketi yükleyerek projenize OWIN destek alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="88fb1-119">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="88fb1-120">OWIN ara yazılımı uygun şekilde [OWIN belirtimi](http://owin.org/spec/spec/owin-1.0.0.html), gerektiren bir `Func<IDictionary<string, object>, Task>` arabirimi ve özel anahtarları ayarlanabilir (gibi `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="88fb1-120">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="88fb1-121">Aşağıdaki basit OWIN ara yazılımı "Hello World" görüntüler:</span><span class="sxs-lookup"><span data-stu-id="88fb1-121">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="88fb1-122">Örnek imza döndürür bir `Task` ve kabul eden bir `IDictionary<string, object>` OWIN gerektirdiği.</span><span class="sxs-lookup"><span data-stu-id="88fb1-122">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="88fb1-123">Aşağıdaki kod nasıl ekleneceğini gösterir `OwinHello` (yukarıda ASP.NET Core işlem hattı ile gösterilen) bir ara yazılım `UseOwin` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="88fb1-123">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET Core pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="88fb1-124">OWIN ardışık düzenini içinde gerçekleşmesi için diğer eylemleri yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="88fb1-124">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="88fb1-125">Yanıt üst bilgilerini yalnızca yanıt akışa ilk Yazımdan önce düzeltilmelidir.</span><span class="sxs-lookup"><span data-stu-id="88fb1-125">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="88fb1-126">Birden çok çağrılar `UseOwin` Performans nedeniyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="88fb1-126">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="88fb1-127">OWIN bileşenleri, gruplandırılmış, en iyi şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="88fb1-127">OWIN components will operate best if grouped together.</span></span>

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

## <a name="using-aspnet-core-hosting-on-an-owin-based-server"></a><span data-ttu-id="88fb1-128">Bir OWIN tabanlı sunucu üzerinde kullanarak ASP.NET Core barındırma</span><span class="sxs-lookup"><span data-stu-id="88fb1-128">Using ASP.NET Core Hosting on an OWIN-based server</span></span>

<span data-ttu-id="88fb1-129">OWIN tabanlı sunucular, ASP.NET Core uygulamaları barındırabilir.</span><span class="sxs-lookup"><span data-stu-id="88fb1-129">OWIN-based servers can host ASP.NET Core apps.</span></span> <span data-ttu-id="88fb1-130">Bir sunucu [Nowin](https://github.com/Bobris/Nowin), bir .NET OWIN web sunucusu.</span><span class="sxs-lookup"><span data-stu-id="88fb1-130">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="88fb1-131">Bu makalede örnek miyim Nowin atıfta bulunan ve oluşturmak için kullandığı bir proje dahil ettiğiniz bir `IServer` ASP.NET Core kendi kendine barındırma yeteneğine.</span><span class="sxs-lookup"><span data-stu-id="88fb1-131">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="88fb1-132">`IServer` gerektiren bir arabirim bir `Features` özelliği ve `Start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="88fb1-132">`IServer` is an interface that requires a `Features` property and a `Start` method.</span></span>

<span data-ttu-id="88fb1-133">`Start` Yapılandırma ve bu durumda bir dizi IServerAddressesFeature Ayrıştırılan adreslerini ayarlamak fluent API'si çağrısı yapılır sunucunun başlatmak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="88fb1-133">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="88fb1-134">Unutmayın fluent yapılandırmasını `_builder` değişkeni, istek tarafından işleneceğini belirtir `appFunc` yönteminde tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="88fb1-134">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="88fb1-135">Bu `Func` her istekte gelen istekleri işlemek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="88fb1-135">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="88fb1-136">Ayrıca ekleyeceğiz bir `IWebHostBuilder` Nowin sunucu ekleme ve yapılandırma kolaylaştırmak için uzantı.</span><span class="sxs-lookup"><span data-stu-id="88fb1-136">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="88fb1-137">Bu yerinde uzantı çağırmak *Program.cs* bu özel sunucu kullanarak ASP.NET Core uygulaması çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="88fb1-137">With this in place, invoke the extension in *Program.cs* to run an ASP.NET Core app using this custom server:</span></span>

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

<span data-ttu-id="88fb1-138">Daha fazla bilgi edinin [ASP.NET Core sunucuları](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="88fb1-138">Learn more about [ASP.NET Core Servers](xref:fundamentals/servers/index).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="88fb1-139">ASP.NET Core OWIN tabanlı bir sunucuda çalıştırın ve WebSockets desteğini kullanma</span><span class="sxs-lookup"><span data-stu-id="88fb1-139">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="88fb1-140">OWIN tabanlı nasıl sunucularının özelliklerinin başka bir örnek tarafından yararlanılabilir ASP.NET Core, WebSockets gibi özelliklere erişim.</span><span class="sxs-lookup"><span data-stu-id="88fb1-140">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="88fb1-141">Önceki örnekte kullanılan .NET OWIN web sunucusunun Web yerleşik olan bir ASP.NET Core uygulaması tarafından yararlanılabilir yuva için destek sunar.</span><span class="sxs-lookup"><span data-stu-id="88fb1-141">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="88fb1-142">Aşağıdaki örnek, Web yuvalarını destekliyorsa ve WebSockets üzerinden sunucusuna gönderilen her şeyi yankılayan basit web uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="88fb1-142">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="88fb1-143">Bu [örnek](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) aynı kullanılarak yapılandırılan `NowinServer` nasıl uygulama yapılandırılan içinde önceki tek - tek fark, kendi `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="88fb1-143">This [sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="88fb1-144">Kullanarak bir test [basit websocket istemcisi](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) uygulaması gösterir:</span><span class="sxs-lookup"><span data-stu-id="88fb1-144">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web yuvası Test İstemcisi](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="88fb1-146">OWIN ortamı</span><span class="sxs-lookup"><span data-stu-id="88fb1-146">OWIN environment</span></span>

<span data-ttu-id="88fb1-147">Bir OWIN ortamı kullanarak oluşturabilirsiniz `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="88fb1-147">You can construct an OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="88fb1-148">OWIN anahtarları</span><span class="sxs-lookup"><span data-stu-id="88fb1-148">OWIN keys</span></span>

<span data-ttu-id="88fb1-149">OWIN bağımlı bir `IDictionary<string,object>` bir HTTP istek/yanıt exchange içinde bilgi iletişim kurmak için nesne.</span><span class="sxs-lookup"><span data-stu-id="88fb1-149">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="88fb1-150">ASP.NET Core, aşağıda listelenen anahtarları uygular.</span><span class="sxs-lookup"><span data-stu-id="88fb1-150">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="88fb1-151">Bkz: [birincil belirtimi, uzantıları](http://owin.org/#spec), ve [OWIN anahtar yönergeleri ve ortak anahtarları](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="88fb1-151">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="88fb1-152">İstek verileri (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="88fb1-152">Request data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="88fb1-153">Anahtar</span><span class="sxs-lookup"><span data-stu-id="88fb1-153">Key</span></span>               | <span data-ttu-id="88fb1-154">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="88fb1-154">Value (type)</span></span> | <span data-ttu-id="88fb1-155">Açıklama</span><span class="sxs-lookup"><span data-stu-id="88fb1-155">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="88fb1-156">owın. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="88fb1-156">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="88fb1-157">owın. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="88fb1-157">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="88fb1-158">owin.RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="88fb1-158">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="88fb1-159">owin.RequestPath</span><span class="sxs-lookup"><span data-stu-id="88fb1-159">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="88fb1-160">owin.RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="88fb1-160">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="88fb1-161">owın. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="88fb1-161">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="88fb1-162">owın. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="88fb1-162">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="88fb1-163">owın. Includesearchresults: true</span><span class="sxs-lookup"><span data-stu-id="88fb1-163">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="88fb1-164">İstek verileri (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="88fb1-164">Request data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="88fb1-165">Anahtar</span><span class="sxs-lookup"><span data-stu-id="88fb1-165">Key</span></span>               | <span data-ttu-id="88fb1-166">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="88fb1-166">Value (type)</span></span> | <span data-ttu-id="88fb1-167">Açıklama</span><span class="sxs-lookup"><span data-stu-id="88fb1-167">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="88fb1-168">owin.RequestId</span><span class="sxs-lookup"><span data-stu-id="88fb1-168">owin.RequestId</span></span> | `String` | <span data-ttu-id="88fb1-169">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="88fb1-169">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="88fb1-170">Yanıt verileri (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="88fb1-170">Response data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="88fb1-171">Anahtar</span><span class="sxs-lookup"><span data-stu-id="88fb1-171">Key</span></span>               | <span data-ttu-id="88fb1-172">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="88fb1-172">Value (type)</span></span> | <span data-ttu-id="88fb1-173">Açıklama</span><span class="sxs-lookup"><span data-stu-id="88fb1-173">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="88fb1-174">owin.ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="88fb1-174">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="88fb1-175">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="88fb1-175">Optional</span></span> |
| <span data-ttu-id="88fb1-176">owin.ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="88fb1-176">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="88fb1-177">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="88fb1-177">Optional</span></span> |
| <span data-ttu-id="88fb1-178">owın. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="88fb1-178">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="88fb1-179">owın. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="88fb1-179">owin.ResponseBody</span></span> | `Stream`  | |

### <a name="other-data-owin-v100"></a><span data-ttu-id="88fb1-180">Diğer veri (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="88fb1-180">Other data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="88fb1-181">Anahtar</span><span class="sxs-lookup"><span data-stu-id="88fb1-181">Key</span></span>               | <span data-ttu-id="88fb1-182">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="88fb1-182">Value (type)</span></span> | <span data-ttu-id="88fb1-183">Açıklama</span><span class="sxs-lookup"><span data-stu-id="88fb1-183">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="88fb1-184">owın. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="88fb1-184">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="88fb1-185">owın. Sürüm</span><span class="sxs-lookup"><span data-stu-id="88fb1-185">owin.Version</span></span>  | `String` | |   

### <a name="common-keys"></a><span data-ttu-id="88fb1-186">Ortak anahtarlar</span><span class="sxs-lookup"><span data-stu-id="88fb1-186">Common keys</span></span>

| <span data-ttu-id="88fb1-187">Anahtar</span><span class="sxs-lookup"><span data-stu-id="88fb1-187">Key</span></span>               | <span data-ttu-id="88fb1-188">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="88fb1-188">Value (type)</span></span> | <span data-ttu-id="88fb1-189">Açıklama</span><span class="sxs-lookup"><span data-stu-id="88fb1-189">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="88fb1-190">SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="88fb1-190">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="88fb1-191">SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="88fb1-191">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="88fb1-192">server.RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="88fb1-192">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="88fb1-193">Sunucu. Uzak bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="88fb1-193">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="88fb1-194">Sunucu. YerelIPAdresi</span><span class="sxs-lookup"><span data-stu-id="88fb1-194">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="88fb1-195">Sunucu. Yerel bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="88fb1-195">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="88fb1-196">Sunucu. IsLocal</span><span class="sxs-lookup"><span data-stu-id="88fb1-196">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="88fb1-197">server.OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="88fb1-197">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |

### <a name="sendfiles-v030"></a><span data-ttu-id="88fb1-198">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="88fb1-198">SendFiles v0.3.0</span></span>

| <span data-ttu-id="88fb1-199">Anahtar</span><span class="sxs-lookup"><span data-stu-id="88fb1-199">Key</span></span>               | <span data-ttu-id="88fb1-200">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="88fb1-200">Value (type)</span></span> | <span data-ttu-id="88fb1-201">Açıklama</span><span class="sxs-lookup"><span data-stu-id="88fb1-201">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="88fb1-202">sendfile. SendAsync</span><span class="sxs-lookup"><span data-stu-id="88fb1-202">sendfile.SendAsync</span></span> | <span data-ttu-id="88fb1-203">Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="88fb1-203">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="88fb1-204">İstek başına</span><span class="sxs-lookup"><span data-stu-id="88fb1-204">Per Request</span></span> |

### <a name="opaque-v030"></a><span data-ttu-id="88fb1-205">Donuk v0.3.0</span><span class="sxs-lookup"><span data-stu-id="88fb1-205">Opaque v0.3.0</span></span>

| <span data-ttu-id="88fb1-206">Anahtar</span><span class="sxs-lookup"><span data-stu-id="88fb1-206">Key</span></span>               | <span data-ttu-id="88fb1-207">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="88fb1-207">Value (type)</span></span> | <span data-ttu-id="88fb1-208">Açıklama</span><span class="sxs-lookup"><span data-stu-id="88fb1-208">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="88fb1-209">Donuk. Sürüm</span><span class="sxs-lookup"><span data-stu-id="88fb1-209">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="88fb1-210">Donuk. Yükseltme</span><span class="sxs-lookup"><span data-stu-id="88fb1-210">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="88fb1-211">Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="88fb1-211">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="88fb1-212">Donuk. Stream</span><span class="sxs-lookup"><span data-stu-id="88fb1-212">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="88fb1-213">Donuk. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="88fb1-213">opaque.CallCancelled</span></span> | `CancellationToken` |  |

### <a name="websocket-v030"></a><span data-ttu-id="88fb1-214">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="88fb1-214">WebSocket v0.3.0</span></span>

| <span data-ttu-id="88fb1-215">Anahtar</span><span class="sxs-lookup"><span data-stu-id="88fb1-215">Key</span></span>               | <span data-ttu-id="88fb1-216">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="88fb1-216">Value (type)</span></span> | <span data-ttu-id="88fb1-217">Açıklama</span><span class="sxs-lookup"><span data-stu-id="88fb1-217">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="88fb1-218">websocket. Sürüm</span><span class="sxs-lookup"><span data-stu-id="88fb1-218">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="88fb1-219">websocket. Kabul et</span><span class="sxs-lookup"><span data-stu-id="88fb1-219">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="88fb1-220">Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="88fb1-220">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="88fb1-221">websocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="88fb1-221">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="88fb1-222">Non-spec</span><span class="sxs-lookup"><span data-stu-id="88fb1-222">Non-spec</span></span> |
| <span data-ttu-id="88fb1-223">websocket. Geçersizdir</span><span class="sxs-lookup"><span data-stu-id="88fb1-223">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="88fb1-224">Bkz: [RFC6455 bölüm 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) adım 5.5</span><span class="sxs-lookup"><span data-stu-id="88fb1-224">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="88fb1-225">websocket. SendAsync</span><span class="sxs-lookup"><span data-stu-id="88fb1-225">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="88fb1-226">Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="88fb1-226">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="88fb1-227">websocket.ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="88fb1-227">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="88fb1-228">Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="88fb1-228">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="88fb1-229">websocket.CloseAsync</span><span class="sxs-lookup"><span data-stu-id="88fb1-229">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="88fb1-230">Bkz: [temsilci imzası](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="88fb1-230">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="88fb1-231">websocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="88fb1-231">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="88fb1-232">websocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="88fb1-232">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="88fb1-233">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="88fb1-233">Optional</span></span> |
| <span data-ttu-id="88fb1-234">websocket.ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="88fb1-234">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="88fb1-235">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="88fb1-235">Optional</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="88fb1-236">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="88fb1-236">Additional resources</span></span>

* [<span data-ttu-id="88fb1-237">Ara Yazılım</span><span class="sxs-lookup"><span data-stu-id="88fb1-237">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="88fb1-238">Sunucular</span><span class="sxs-lookup"><span data-stu-id="88fb1-238">Servers</span></span>](xref:fundamentals/servers/index)
