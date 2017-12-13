---
title: ".NET (OWIN) için açık Web arabirimi"
author: ardalis
description: "ASP.NET Core açık Web arabirimi için .NET (hangi web uygulamalarının web sunucularından ayrılmış sağlar OWIN), nasıl desteklediği bulur."
keywords: "ASP.NET Core, .NET, OWIN için açık Web arabirimi"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2ee970a1c9cd05ebee76b92c3e2c7c6c6cc6ef8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="466fa-104">.NET (OWIN) için Web Arabirimi'ni açmak için giriş</span><span class="sxs-lookup"><span data-stu-id="466fa-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="466fa-105">Tarafından [Steve Smith](https://ardalis.com/) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="466fa-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="466fa-106">ASP.NET Core açık Web arabirimi .NET (OWIN) destekler.</span><span class="sxs-lookup"><span data-stu-id="466fa-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="466fa-107">OWIN web uygulamalarının web sunucularından ayrılmış sağlar.</span><span class="sxs-lookup"><span data-stu-id="466fa-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="466fa-108">Ardışık düzeninde istekleri ve ilişkili yanıtları işlemek için kullanılan ara yazılımı için standart bir biçimde tanımlar.</span><span class="sxs-lookup"><span data-stu-id="466fa-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="466fa-109">ASP.NET Core uygulamaları ve ara yazılım OWIN tabanlı uygulamalar, sunucuları ve ara yazılımı ile çalışabilirler.</span><span class="sxs-lookup"><span data-stu-id="466fa-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="466fa-110">OWIN birlikte kullanılmak üzere farklı nesne modeline iki çerçeveleri izin veren bir decoupling katmanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="466fa-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="466fa-111">`Microsoft.AspNetCore.Owin` Paket iki bağdaştırıcı uygulamaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="466fa-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="466fa-112">OWIN için ASP.NET Çekirdeği</span><span class="sxs-lookup"><span data-stu-id="466fa-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="466fa-113">ASP.NET Core OWIN</span><span class="sxs-lookup"><span data-stu-id="466fa-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="466fa-114">Bir OWIN uyumlu sunucu/ana bilgisayar üstünde veya ASP.NET Core üzerinde çalıştırılacak diğer OWIN uyumlu bileşenlerine yönelik barındırılmasını ASP.NET Core sağlar.</span><span class="sxs-lookup"><span data-stu-id="466fa-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="466fa-115">Not: Bu bağdaştırıcılar kullanarak bir performans ile gelir.</span><span class="sxs-lookup"><span data-stu-id="466fa-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="466fa-116">Yalnızca ASP.NET Core bileşenleri kullanan uygulamalar Owın paket ya da bağdaştırıcıları kullanmamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="466fa-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

<span data-ttu-id="466fa-117">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="466fa-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="466fa-118">ASP.NET ardışık düzeninde OWIN ara yazılımı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="466fa-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="466fa-119">ASP.NET Core'nın OWIN destek parçası olarak dağıtılan `Microsoft.AspNetCore.Owin` paket.</span><span class="sxs-lookup"><span data-stu-id="466fa-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="466fa-120">Bu paketi yüklerken projenize OWIN destek alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="466fa-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="466fa-121">OWIN ara yazılımı uyan için [OWIN belirtimi](http://owin.org/spec/spec/owin-1.0.0.html), gerektiren bir `Func<IDictionary<string, object>, Task>` arabirimi ve özel anahtarları ayarlanabilir (gibi `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="466fa-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="466fa-122">Aşağıdaki basit OWIN ara yazılımı "Hello World" görüntüler:</span><span class="sxs-lookup"><span data-stu-id="466fa-122">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="466fa-123">Örnek imza döndüren bir `Task` ve kabul eden bir `IDictionary<string, object>` OWIN gerektirdiği.</span><span class="sxs-lookup"><span data-stu-id="466fa-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="466fa-124">Aşağıdaki kodu nasıl ekleneceğini gösterir `OwinHello` (yukarıda için ASP.NET ardışık düzenini ile gösterilen) Ara `UseOwin` genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="466fa-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

<span data-ttu-id="466fa-125">OWIN ardışık düzenini içinde gerçekleşmesi için başka eylemler yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="466fa-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="466fa-126">Yanıt Üstbilgileri yanıt akışa ilk Yazımdan önce yalnızca değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="466fa-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="466fa-127">Birden çok çağrılar `UseOwin` performans nedenleriyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="466fa-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="466fa-128">OWIN bileşenleri bir arada gruplandırılmış, en iyi şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="466fa-128">OWIN components will operate best if grouped together.</span></span>

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

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="466fa-129">Bir OWIN tabanlı sunucu üzerinde kullanarak ASP.NET barındırma</span><span class="sxs-lookup"><span data-stu-id="466fa-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="466fa-130">OWIN tabanlı sunucular ASP.NET uygulamalarını barındırabilir.</span><span class="sxs-lookup"><span data-stu-id="466fa-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="466fa-131">Bu tür bir sunucu [Nowin](https://github.com/Bobris/Nowin), bir .NET OWIN web sunucusu.</span><span class="sxs-lookup"><span data-stu-id="466fa-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="466fa-132">Bu makalede örnek t Nowin başvuruyor ve oluşturmak için kullandığı bir proje dahil ettiğiniz bir `IServer` ASP.NET Core kendi kendine barındırma yeteneğine sahip.</span><span class="sxs-lookup"><span data-stu-id="466fa-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="466fa-133">`IServer`gerektiren bir arabirim bir `Features` özelliği ve `Start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="466fa-133">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="466fa-134">`Start`Yapılandırma ve bu durumda bir dizi IServerAddressesFeature Ayrıştırılan adreslerini ayarlamak fluent API çağrısı yoluyla yapılır sunucu başlangıç sorumludur.</span><span class="sxs-lookup"><span data-stu-id="466fa-134">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="466fa-135">Unutmayın fluent yapılandırmasını `_builder` değişkeni belirtir istekleri tarafından işlenecek `appFunc` yöntemi önceden tanımlanmış.</span><span class="sxs-lookup"><span data-stu-id="466fa-135">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="466fa-136">Bu `Func` her istekte gelen istekleri işlemek üzere çağrılır.</span><span class="sxs-lookup"><span data-stu-id="466fa-136">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="466fa-137">Ayrıca ekleyeceğiz bir `IWebHostBuilder` Nowin sunucu ekleme ve yapılandırma kolaylaştırmak için uzantı.</span><span class="sxs-lookup"><span data-stu-id="466fa-137">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

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

<span data-ttu-id="466fa-138">Bunun yerine, tüm uzantısı çağırmak için bu özel sunucu kullanarak bir ASP.NET uygulamasını çalıştırılması için gerekli *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="466fa-138">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

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

<span data-ttu-id="466fa-139">ASP.NET hakkında daha fazla bilgi [sunucuları](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="466fa-139">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="466fa-140">Bir OWIN tabanlı sunucu üzerinde ASP.NET Core çalıştırın ve WebSockets desteğini kullanın</span><span class="sxs-lookup"><span data-stu-id="466fa-140">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="466fa-141">OWIN tabanlı nasıl sunucularındaki özellikleri başka bir örneği tarafından yararlanılabilir ASP.NET çekirdeği WebSockets gibi özellikleri erişimi olur.</span><span class="sxs-lookup"><span data-stu-id="466fa-141">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="466fa-142">Önceki örnekte kullanılan .NET OWIN web sunucusu Web, yerleşik olan bir ASP.NET Core uygulama tarafından yararlanılabilir yuva desteği vardır.</span><span class="sxs-lookup"><span data-stu-id="466fa-142">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="466fa-143">Aşağıdaki örnek, Web yuvalarını destekliyorsa ve geri WebSockets aracılığıyla sunucuya gönderilen her şeyi görüntülemektedir bir basit bir web uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="466fa-143">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

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

<span data-ttu-id="466fa-144">Bu [örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) aynı kullanılarak yapılandırılmış `NowinServer` önceki bir - yalnızca nasıl uygulama yapılandırılan içinde farktır kendi `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="466fa-144">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="466fa-145">Kullanarak bir test [basit websocket istemci](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) uygulaması gösterir:</span><span class="sxs-lookup"><span data-stu-id="466fa-145">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Web yuvası Test İstemcisi](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="466fa-147">OWIN ortamı</span><span class="sxs-lookup"><span data-stu-id="466fa-147">OWIN environment</span></span>

<span data-ttu-id="466fa-148">Bir OWIN ortamı kullanarak oluşturabileceğiniz `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="466fa-148">You can construct a OWIN environment using the `HttpContext`.</span></span>

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="466fa-149">OWIN anahtarları</span><span class="sxs-lookup"><span data-stu-id="466fa-149">OWIN keys</span></span>

<span data-ttu-id="466fa-150">OWIN bağımlı bir `IDictionary<string,object>` bir HTTP istek/yanıt exchange genelinde bilgileri iletişim kurmak için nesne.</span><span class="sxs-lookup"><span data-stu-id="466fa-150">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="466fa-151">ASP.NET Core aşağıda listelenen anahtarları uygular.</span><span class="sxs-lookup"><span data-stu-id="466fa-151">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="466fa-152">Bkz: [birincil belirtimi, uzantıları](http://owin.org/#spec), ve [OWIN anahtarı kılavuzları ve ortak anahtarlar](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="466fa-152">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="466fa-153">İstek verileri (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="466fa-153">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="466fa-154">Anahtar</span><span class="sxs-lookup"><span data-stu-id="466fa-154">Key</span></span>               | <span data-ttu-id="466fa-155">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="466fa-155">Value (type)</span></span> | <span data-ttu-id="466fa-156">Açıklama</span><span class="sxs-lookup"><span data-stu-id="466fa-156">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="466fa-157">owın. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="466fa-157">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="466fa-158">owın. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="466fa-158">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="466fa-159">owın. RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="466fa-159">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="466fa-160">owın. RequestPath</span><span class="sxs-lookup"><span data-stu-id="466fa-160">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="466fa-161">owın. RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="466fa-161">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="466fa-162">owın. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="466fa-162">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="466fa-163">owın. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="466fa-163">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="466fa-164">owın. RequestBody</span><span class="sxs-lookup"><span data-stu-id="466fa-164">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="466fa-165">İstek verileri (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="466fa-165">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="466fa-166">Anahtar</span><span class="sxs-lookup"><span data-stu-id="466fa-166">Key</span></span>               | <span data-ttu-id="466fa-167">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="466fa-167">Value (type)</span></span> | <span data-ttu-id="466fa-168">Açıklama</span><span class="sxs-lookup"><span data-stu-id="466fa-168">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="466fa-169">owın. RequestId</span><span class="sxs-lookup"><span data-stu-id="466fa-169">owin.RequestId</span></span> | `String` | <span data-ttu-id="466fa-170">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="466fa-170">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="466fa-171">Yanıt verileri (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="466fa-171">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="466fa-172">Anahtar</span><span class="sxs-lookup"><span data-stu-id="466fa-172">Key</span></span>               | <span data-ttu-id="466fa-173">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="466fa-173">Value (type)</span></span> | <span data-ttu-id="466fa-174">Açıklama</span><span class="sxs-lookup"><span data-stu-id="466fa-174">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="466fa-175">owın. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="466fa-175">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="466fa-176">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="466fa-176">Optional</span></span> |
| <span data-ttu-id="466fa-177">owın. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="466fa-177">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="466fa-178">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="466fa-178">Optional</span></span> |
| <span data-ttu-id="466fa-179">owın. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="466fa-179">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="466fa-180">owın. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="466fa-180">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="466fa-181">Diğer veri (OWIN v1.0.0)</span><span class="sxs-lookup"><span data-stu-id="466fa-181">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="466fa-182">Anahtar</span><span class="sxs-lookup"><span data-stu-id="466fa-182">Key</span></span>               | <span data-ttu-id="466fa-183">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="466fa-183">Value (type)</span></span> | <span data-ttu-id="466fa-184">Açıklama</span><span class="sxs-lookup"><span data-stu-id="466fa-184">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="466fa-185">owın. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="466fa-185">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="466fa-186">owın. Sürüm</span><span class="sxs-lookup"><span data-stu-id="466fa-186">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="466fa-187">Ortak anahtarlar</span><span class="sxs-lookup"><span data-stu-id="466fa-187">Common Keys</span></span>

| <span data-ttu-id="466fa-188">Anahtar</span><span class="sxs-lookup"><span data-stu-id="466fa-188">Key</span></span>               | <span data-ttu-id="466fa-189">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="466fa-189">Value (type)</span></span> | <span data-ttu-id="466fa-190">Açıklama</span><span class="sxs-lookup"><span data-stu-id="466fa-190">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="466fa-191">SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="466fa-191">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="466fa-192">SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="466fa-192">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="466fa-193">Sunucu. UzakIPAdresi</span><span class="sxs-lookup"><span data-stu-id="466fa-193">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="466fa-194">Sunucu. Uzak bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="466fa-194">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="466fa-195">Sunucu. YerelIPAdresi</span><span class="sxs-lookup"><span data-stu-id="466fa-195">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="466fa-196">Sunucu. Yerel bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="466fa-196">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="466fa-197">Sunucu. IsLocal</span><span class="sxs-lookup"><span data-stu-id="466fa-197">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="466fa-198">Sunucu. OnSendingHeaders</span><span class="sxs-lookup"><span data-stu-id="466fa-198">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="466fa-199">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="466fa-199">SendFiles v0.3.0</span></span>

| <span data-ttu-id="466fa-200">Anahtar</span><span class="sxs-lookup"><span data-stu-id="466fa-200">Key</span></span>               | <span data-ttu-id="466fa-201">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="466fa-201">Value (type)</span></span> | <span data-ttu-id="466fa-202">Açıklama</span><span class="sxs-lookup"><span data-stu-id="466fa-202">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="466fa-203">sendfile. SendAsync</span><span class="sxs-lookup"><span data-stu-id="466fa-203">sendfile.SendAsync</span></span> | <span data-ttu-id="466fa-204">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="466fa-204">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="466fa-205">İstek başına</span><span class="sxs-lookup"><span data-stu-id="466fa-205">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="466fa-206">Donuk v0.3.0</span><span class="sxs-lookup"><span data-stu-id="466fa-206">Opaque v0.3.0</span></span>

| <span data-ttu-id="466fa-207">Anahtar</span><span class="sxs-lookup"><span data-stu-id="466fa-207">Key</span></span>               | <span data-ttu-id="466fa-208">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="466fa-208">Value (type)</span></span> | <span data-ttu-id="466fa-209">Açıklama</span><span class="sxs-lookup"><span data-stu-id="466fa-209">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="466fa-210">Donuk. Sürüm</span><span class="sxs-lookup"><span data-stu-id="466fa-210">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="466fa-211">Donuk. Yükseltme</span><span class="sxs-lookup"><span data-stu-id="466fa-211">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="466fa-212">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="466fa-212">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="466fa-213">Donuk. Akış</span><span class="sxs-lookup"><span data-stu-id="466fa-213">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="466fa-214">Donuk. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="466fa-214">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="466fa-215">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="466fa-215">WebSocket v0.3.0</span></span>

| <span data-ttu-id="466fa-216">Anahtar</span><span class="sxs-lookup"><span data-stu-id="466fa-216">Key</span></span>               | <span data-ttu-id="466fa-217">Değer (tür)</span><span class="sxs-lookup"><span data-stu-id="466fa-217">Value (type)</span></span> | <span data-ttu-id="466fa-218">Açıklama</span><span class="sxs-lookup"><span data-stu-id="466fa-218">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="466fa-219">websocket. Sürüm</span><span class="sxs-lookup"><span data-stu-id="466fa-219">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="466fa-220">websocket. Kabul et</span><span class="sxs-lookup"><span data-stu-id="466fa-220">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="466fa-221">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="466fa-221">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="466fa-222">websocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="466fa-222">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="466fa-223">Olmayan belirtimi</span><span class="sxs-lookup"><span data-stu-id="466fa-223">Non-spec</span></span> |
| <span data-ttu-id="466fa-224">websocket. SubProtocol</span><span class="sxs-lookup"><span data-stu-id="466fa-224">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="466fa-225">Bkz: [RFC6455 bölüm 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) adım 5.5</span><span class="sxs-lookup"><span data-stu-id="466fa-225">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="466fa-226">websocket. SendAsync</span><span class="sxs-lookup"><span data-stu-id="466fa-226">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="466fa-227">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="466fa-227">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="466fa-228">websocket. ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="466fa-228">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="466fa-229">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="466fa-229">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="466fa-230">websocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="466fa-230">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="466fa-231">Bkz: [temsilci imza](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="466fa-231">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="466fa-232">websocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="466fa-232">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="466fa-233">websocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="466fa-233">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="466fa-234">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="466fa-234">Optional</span></span> |
| <span data-ttu-id="466fa-235">websocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="466fa-235">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="466fa-236">İsteğe Bağlı</span><span class="sxs-lookup"><span data-stu-id="466fa-236">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="466fa-237">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="466fa-237">Additional Resources</span></span>

* [<span data-ttu-id="466fa-238">Ara yazılım</span><span class="sxs-lookup"><span data-stu-id="466fa-238">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="466fa-239">Sunucular</span><span class="sxs-lookup"><span data-stu-id="466fa-239">Servers</span></span>](servers/index.md)
