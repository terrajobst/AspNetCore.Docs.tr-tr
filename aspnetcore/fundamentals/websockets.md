---
title: ASP.NET Core desteği WebSockets
author: rick-anderson
description: ASP.NET Core 'de WebSockets kullanmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/websockets
ms.openlocfilehash: 5d4d9b02bd45e6650aa56448a3663cad06b3b45e
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975451"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="51d6d-103">ASP.NET Core desteği WebSockets</span><span class="sxs-lookup"><span data-stu-id="51d6d-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="51d6d-104">[Tom Dykstra](https://github.com/tdykstra) ve [Andrew Stanton-nurte](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="51d6d-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="51d6d-105">Bu makalede, ASP.NET Core ' de WebSockets ile çalışmaya başlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="51d6d-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)), TCP bağlantıları üzerinden iki yönlü kalıcı iletişim kanalları sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="51d6d-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="51d6d-107">Bu, sohbet, pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişimden faydalanabilir uygulamalarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="51d6d-108">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([indirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="51d6d-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="51d6d-109">[Nasıl çalıştırılır?](#sample-app)</span><span class="sxs-lookup"><span data-stu-id="51d6d-109">[How to run](#sample-app).</span></span>

## <a name="signalr"></a><span data-ttu-id="51d6d-110">SignalR</span><span class="sxs-lookup"><span data-stu-id="51d6d-110">SignalR</span></span>

<span data-ttu-id="51d6d-111">[ASP.NET Core SignalR](xref:signalr/introduction) , uygulamalara gerçek zamanlı Web işlevselliği eklemeyi kolaylaştıran bir kitaplıktır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-111">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="51d6d-112">Mümkün olduğunda WebSockets kullanır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-112">It uses WebSockets whenever possible.</span></span>

<span data-ttu-id="51d6d-113">Çoğu uygulama için ham WebSockets üzerinden SignalR önerilir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-113">For most applications, we recommend SignalR over raw WebSockets.</span></span> <span data-ttu-id="51d6d-114">SignalR, WebSockets 'in kullanılamadığı ortamlar için taşıma geri dönüşü sağlar.</span><span class="sxs-lookup"><span data-stu-id="51d6d-114">SignalR provides transport fallback for environments where WebSockets is not available.</span></span> <span data-ttu-id="51d6d-115">Ayrıca, basit bir uzak yordam çağrısı uygulama modeli sağlar.</span><span class="sxs-lookup"><span data-stu-id="51d6d-115">It also provides a simple remote procedure call app model.</span></span> <span data-ttu-id="51d6d-116">Birçok senaryoda, SignalR 'nin ham WebSockets kullanmaya kıyasla önemli bir performans dezavantajı yoktur.</span><span class="sxs-lookup"><span data-stu-id="51d6d-116">And in most scenarios, SignalR has no significant performance disadvantage compared to using raw WebSockets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="51d6d-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="51d6d-117">Prerequisites</span></span>

* <span data-ttu-id="51d6d-118">ASP.NET Core 1,1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="51d6d-118">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="51d6d-119">ASP.NET Core destekleyen herhangi bir işletim sistemi:</span><span class="sxs-lookup"><span data-stu-id="51d6d-119">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="51d6d-120">Windows 7/Windows Server 2008 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="51d6d-120">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="51d6d-121">Linux</span><span class="sxs-lookup"><span data-stu-id="51d6d-121">Linux</span></span>
  * <span data-ttu-id="51d6d-122">macOS</span><span class="sxs-lookup"><span data-stu-id="51d6d-122">macOS</span></span>
  
* <span data-ttu-id="51d6d-123">Uygulama IIS ile Windows üzerinde çalışıyorsa:</span><span class="sxs-lookup"><span data-stu-id="51d6d-123">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="51d6d-124">Windows 8/Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="51d6d-124">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="51d6d-125">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="51d6d-125">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="51d6d-126">WebSockets etkinleştirilmelidir ( [IIS/IIS Express destek](#iisiis-express-support) bölümüne bakın.).</span><span class="sxs-lookup"><span data-stu-id="51d6d-126">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="51d6d-127">Uygulama [http. sys](xref:fundamentals/servers/httpsys)üzerinde çalışıyorsa:</span><span class="sxs-lookup"><span data-stu-id="51d6d-127">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="51d6d-128">Windows 8/Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="51d6d-128">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="51d6d-129">Desteklenen tarayıcılar için bkz https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="51d6d-129">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

::: moniker range="< aspnetcore-2.1"

## <a name="nuget-package"></a><span data-ttu-id="51d6d-130">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="51d6d-130">NuGet package</span></span>

<span data-ttu-id="51d6d-131">[Microsoft. AspNetCore. WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="51d6d-131">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>

::: moniker-end

## <a name="configure-the-middleware"></a><span data-ttu-id="51d6d-132">Ara yazılımı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="51d6d-132">Configure the middleware</span></span>


<span data-ttu-id="51d6d-133">WebSockets ara yazılımını `Configure` `Startup` sınıfının yöntemine ekleyin:</span><span class="sxs-lookup"><span data-stu-id="51d6d-133">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="51d6d-134">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="51d6d-134">The following settings can be configured:</span></span>

* <span data-ttu-id="51d6d-135">`KeepAliveInterval`-Proxy 'lerin bağlantının açık kalmasını sağlamak için istemciye "ping" çerçeveleri gönderme sıklığı.</span><span class="sxs-lookup"><span data-stu-id="51d6d-135">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="51d6d-136">Varsayılan değer iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-136">The default is two minutes.</span></span>
* <span data-ttu-id="51d6d-137">`ReceiveBufferSize`-Verileri almak için kullanılan arabelleğin boyutu.</span><span class="sxs-lookup"><span data-stu-id="51d6d-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="51d6d-138">Gelişmiş kullanıcıların, verilerin boyutuna bağlı olarak performans ayarlaması için bunu değiştirmesi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="51d6d-139">Varsayılan değer 4 KB 'tır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-139">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="51d6d-140">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="51d6d-140">The following settings can be configured:</span></span>

* <span data-ttu-id="51d6d-141">`KeepAliveInterval`-Proxy 'lerin bağlantının açık kalmasını sağlamak için istemciye "ping" çerçeveleri gönderme sıklığı.</span><span class="sxs-lookup"><span data-stu-id="51d6d-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="51d6d-142">Varsayılan değer iki dakikadır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-142">The default is two minutes.</span></span>
* <span data-ttu-id="51d6d-143">`ReceiveBufferSize`-Verileri almak için kullanılan arabelleğin boyutu.</span><span class="sxs-lookup"><span data-stu-id="51d6d-143">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="51d6d-144">Gelişmiş kullanıcıların, verilerin boyutuna bağlı olarak performans ayarlaması için bunu değiştirmesi gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-144">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="51d6d-145">Varsayılan değer 4 KB 'tır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-145">The default is 4 KB.</span></span>
* <span data-ttu-id="51d6d-146">`AllowedOrigins`-WebSocket istekleri için izin verilen kaynak üst bilgi değerleri listesi.</span><span class="sxs-lookup"><span data-stu-id="51d6d-146">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="51d6d-147">Varsayılan olarak, tüm kaynaklardan izin verilir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-147">By default, all origins are allowed.</span></span> <span data-ttu-id="51d6d-148">Ayrıntılar için aşağıdaki "WebSocket kaynak kısıtlaması" başlığına bakın.</span><span class="sxs-lookup"><span data-stu-id="51d6d-148">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

## <a name="accept-websocket-requests"></a><span data-ttu-id="51d6d-149">WebSocket isteklerini kabul et</span><span class="sxs-lookup"><span data-stu-id="51d6d-149">Accept WebSocket requests</span></span>

<span data-ttu-id="51d6d-150">Daha sonra istek yaşam döngüsünün (daha sonra `Configure` yöntemde veya bir eylem yönteminde daha sonra) bir yerde, bir WebSocket isteği olup olmadığını denetleyin ve WebSocket isteğini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-150">Somewhere later in the request life cycle (later in the `Configure` method or in an action method, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="51d6d-151">Aşağıdaki örnek daha sonra `Configure` yönteminde verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="51d6d-151">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="51d6d-152">Herhangi bir URL 'de bir WebSocket isteği gelebilir, ancak bu örnek kod yalnızca için `/ws`istekleri kabul eder.</span><span class="sxs-lookup"><span data-stu-id="51d6d-152">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

<span data-ttu-id="51d6d-153">WebSocket kullanırken, bağlantı süresince ara yazılım işlem hattını çalışır durumda tutmanız **gerekir** .</span><span class="sxs-lookup"><span data-stu-id="51d6d-153">When using a WebSocket, you **must** keep the middleware pipeline running for the duration of the connection.</span></span> <span data-ttu-id="51d6d-154">Ara yazılım ardışık düzeni bittikten sonra bir WebSocket iletisi göndermeye veya almaya çalışırsanız, aşağıdaki gibi bir özel durum alabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="51d6d-154">If you attempt to send or receive a WebSocket message after the middleware pipeline ends, you may get an exception like the following:</span></span>

```
System.Net.WebSockets.WebSocketException (0x80004005): The remote party closed the WebSocket connection without completing the close handshake. ---> System.ObjectDisposedException: Cannot write to the response body, the response has completed.
Object name: 'HttpResponseStream'.
```

<span data-ttu-id="51d6d-155">WebSocket 'e veri yazmak için bir arka plan hizmeti kullanıyorsanız, ara yazılım ardışık düzenini çalışır durumda tutmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-155">If you're using a background service to write data to a WebSocket, make sure you keep the middleware pipeline running.</span></span> <span data-ttu-id="51d6d-156">Bunu kullanarak <xref:System.Threading.Tasks.TaskCompletionSource%601>yapın.</span><span class="sxs-lookup"><span data-stu-id="51d6d-156">Do this by using a <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span></span> <span data-ttu-id="51d6d-157">' İ arka plan hizmetinize geçirin ve WebSocket ile bitirdiğinizde <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> bu hizmete çağrı yapın. `TaskCompletionSource`</span><span class="sxs-lookup"><span data-stu-id="51d6d-157">Pass the `TaskCompletionSource` to your background service and have it call <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> when you finish with the WebSocket.</span></span> <span data-ttu-id="51d6d-158">Ardından `await`, aşağıdaki örnekte gösterildiği gibi istek sırasında özelliği:<xref:System.Threading.Tasks.TaskCompletionSource%601.Task></span><span class="sxs-lookup"><span data-stu-id="51d6d-158">Then `await` the <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> property during the request, as shown in the following example:</span></span>

```csharp
app.Use(async (context, next) => {
    var socket = await context.WebSockets.AcceptWebSocketAsync();
    var socketFinishedTcs = new TaskCompletionSource<object>();

    BackgroundSocketProcessor.AddSocket(socket, socketFinishedTcs); 

    await socketFinishedTcs.Task;
});
```
<span data-ttu-id="51d6d-159">Bir eylem yönteminden çok yakında döndürüyseniz, WebSocket kapalı özel durumu da oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-159">The WebSocket closed exception can also happen if you return too soon from an action method.</span></span> <span data-ttu-id="51d6d-160">Bir eylem yönteminde bir yuvayı kabul ediyorsanız, işlem yönteminden dönmeden önce yuva kullanan kodun tamamlanmasını bekleyin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-160">If you accept a socket in an action method, wait for the code that uses the socket to complete before returning from the action method.</span></span>

<span data-ttu-id="51d6d-161">Önemli iş `Task.Wait()`parçacığı `Task.Result`sorunlarına neden olabileceği için, yuvanın tamamlanmasını beklemek için hiçbir şekilde, veya benzer engelleme çağrılarını kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="51d6d-161">Never use `Task.Wait()`, `Task.Result`, or similar blocking calls to wait for the socket to complete, as that can cause serious threading issues.</span></span> <span data-ttu-id="51d6d-162">Her zaman `await`kullanın.</span><span class="sxs-lookup"><span data-stu-id="51d6d-162">Always use `await`.</span></span>

## <a name="send-and-receive-messages"></a><span data-ttu-id="51d6d-163">İleti gönderme ve alma</span><span class="sxs-lookup"><span data-stu-id="51d6d-163">Send and receive messages</span></span>

<span data-ttu-id="51d6d-164">Yöntemi, TCP bağlantısını bir WebSocket bağlantısıyla yükseltir ve bir WebSocket nesnesi sağlar. [](/dotnet/core/api/system.net.websockets.websocket) `AcceptWebSocketAsync`</span><span class="sxs-lookup"><span data-stu-id="51d6d-164">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="51d6d-165">İleti göndermek ve almak için nesnesinikullanın.`WebSocket`</span><span class="sxs-lookup"><span data-stu-id="51d6d-165">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="51d6d-166">WebSocket isteğini kabul eden daha önce gösterilen kod, `WebSocket` nesneyi bir `Echo` yönteme geçirir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-166">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="51d6d-167">Kod bir ileti alır ve hemen aynı iletiyi geri gönderir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-167">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="51d6d-168">İletiler, istemci bağlantıyı kapatana kadar bir döngüde gönderilir ve alınır:</span><span class="sxs-lookup"><span data-stu-id="51d6d-168">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

<span data-ttu-id="51d6d-169">Döngüye başlamadan önce WebSocket bağlantısı kabul edildiğinde, ara yazılım ardışık düzeni sona erer.</span><span class="sxs-lookup"><span data-stu-id="51d6d-169">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="51d6d-170">Yuva kapatıldıktan sonra işlem hattı kaldırımları.</span><span class="sxs-lookup"><span data-stu-id="51d6d-170">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="51d6d-171">Diğer bir deyişle, WebSocket kabul edildiğinde isteğin işlem hattında ileri doğru hareket ettirilmesi durduruluyor.</span><span class="sxs-lookup"><span data-stu-id="51d6d-171">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="51d6d-172">Döngü bittiğinde ve yuva kapalıyken, istek ardışık düzeni yedekler.</span><span class="sxs-lookup"><span data-stu-id="51d6d-172">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="handle-client-disconnects"></a><span data-ttu-id="51d6d-173">İstemci bağlantısı kesilen işleme</span><span class="sxs-lookup"><span data-stu-id="51d6d-173">Handle client disconnects</span></span>

<span data-ttu-id="51d6d-174">İstemci bağlantı kaybı nedeniyle bağlantısı kesildiğinde sunucu otomatik olarak bilgilendirilmedi.</span><span class="sxs-lookup"><span data-stu-id="51d6d-174">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="51d6d-175">Sunucu, yalnızca istemci gönderirse, internet bağlantısı kaybedilmişse gerçekleştirilemez bir bağlantı kesme iletisi alır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-175">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="51d6d-176">Bu durumda bazı işlemleri gerçekleştirmek istiyorsanız, belirli bir zaman penceresinde istemciden hiçbir şey alınmadığında bir zaman aşımı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="51d6d-176">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="51d6d-177">İstemci her zaman ileti göndermiyor ve bağlantı boşta kaldığı için zaman aşımına uğramasını istemiyorsanız, istemcinin her X saniyede bir ping iletisi göndermesi için bir Zamanlayıcı kullanmasını sağlamak için bir Zamanlayıcı kullanmasını sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="51d6d-177">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="51d6d-178">Sunucusunda, bir ileti öncekinden sonra 2\*X saniye içinde gelmediyse, bağlantıyı sonlandırın ve istemcinin bağlantısının kesildiğini bildirin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-178">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="51d6d-179">Beklenen zaman aralığının iki kez, ping iletisini tutabilecek Ağ gecikmeleri için ek süre kalmasını bekleyin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-179">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="51d6d-180">WebSocket kaynak kısıtlaması</span><span class="sxs-lookup"><span data-stu-id="51d6d-180">WebSocket origin restriction</span></span>

<span data-ttu-id="51d6d-181">CORS tarafından sunulan korumalar WebSockets için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-181">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="51d6d-182">Tarayıcılar şunları **desteklemez**:</span><span class="sxs-lookup"><span data-stu-id="51d6d-182">Browsers do **not**:</span></span>

* <span data-ttu-id="51d6d-183">CORS ön uçuş istekleri gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-183">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="51d6d-184">WebSocket istekleri yapılırken `Access-Control` üst bilgilerde belirtilen kısıtlamalara saygı.</span><span class="sxs-lookup"><span data-stu-id="51d6d-184">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="51d6d-185">Ancak, tarayıcılar WebSocket istekleri verirken `Origin` üstbilgiyi gönderir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-185">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="51d6d-186">Yalnızca beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için uygulamalar bu üstbilgileri doğrulamak üzere yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-186">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="51d6d-187">Sunucunuzu "https://server.com " üzerinde barındırıyorsanız ve istemcinizi "https://client.com" üzerinde barındırıyorsanız, doğrulamak için WebSockets `AllowedOrigins` listesine "https://client.com" ekleyin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-187">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="51d6d-188">Üst bilgi istemci tarafından denetlenir ve `Referer` üst bilgi gibi erişilebilir. `Origin`</span><span class="sxs-lookup"><span data-stu-id="51d6d-188">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="51d6d-189">Bu üst bilgileri kimlik doğrulama mekanizması olarak kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="51d6d-189">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="51d6d-190">IIS/IIS Express desteği</span><span class="sxs-lookup"><span data-stu-id="51d6d-190">IIS/IIS Express support</span></span>

<span data-ttu-id="51d6d-191">IIS/IIS Express 8 veya üzeri ile Windows Server 2012 veya üzeri ve Windows 8 veya üzeri, WebSocket protokolü için destek içerir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-191">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="51d6d-192">IIS Express kullanılırken WebSockets her zaman etkindir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-192">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="51d6d-193">IIS üzerinde WebSockets etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="51d6d-193">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="51d6d-194">Windows Server 2012 veya sonraki sürümlerde WebSocket protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="51d6d-194">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="51d6d-195">IIS Express kullanılırken bu adımlar gerekli değildir</span><span class="sxs-lookup"><span data-stu-id="51d6d-195">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="51d6d-196">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="51d6d-196">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="51d6d-197">**Rol tabanlı veya özellik tabanlı yükleme**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-197">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="51d6d-198">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-198">Select **Next**.</span></span>
1. <span data-ttu-id="51d6d-199">Uygun sunucuyu seçin (yerel sunucu varsayılan olarak seçilidir).</span><span class="sxs-lookup"><span data-stu-id="51d6d-199">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="51d6d-200">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-200">Select **Next**.</span></span>
1. <span data-ttu-id="51d6d-201">**Roller** ağacında **Web sunucusu (IIS)** öğesini genişletin, **Web sunucusu**' nu genişletin ve ardından **uygulama geliştirme**' yi genişletin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-201">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="51d6d-202">**WebSocket protokolünü**seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-202">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="51d6d-203">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-203">Select **Next**.</span></span>
1. <span data-ttu-id="51d6d-204">Ek özellikler gerekmiyorsa, **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-204">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="51d6d-205">**Yükle**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-205">Select **Install**.</span></span>
1. <span data-ttu-id="51d6d-206">Yükleme tamamlandığında sihirbazdan çıkmak için **Kapat** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-206">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="51d6d-207">Windows 8 veya sonraki sürümlerde WebSocket protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="51d6d-207">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="51d6d-208">IIS Express kullanılırken bu adımlar gerekli değildir</span><span class="sxs-lookup"><span data-stu-id="51d6d-208">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="51d6d-209">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="51d6d-209">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="51d6d-210">Aşağıdaki düğümleri açın: **Internet Information Services** > **World Wide Web**Servicesuygulama > **geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="51d6d-210">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="51d6d-211">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="51d6d-211">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="51d6d-212">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-212">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="51d6d-213">Node. js üzerinde socket.io kullanırken WebSocket 'i devre dışı bırakma</span><span class="sxs-lookup"><span data-stu-id="51d6d-213">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="51d6d-214">[Socket.io](https://socket.io/) içindeki WebSocket desteğini [Node. js](https://nodejs.org/)üzerinde kullanıyorsanız, `webSocket` *Web. config* veya *ApplicationHost. config*içindeki öğesini kullanarak varsayılan IIS WebSocket modülünü devre dışı bırakın. Bu adım gerçekleştirilmemişse, IIS WebSocket modülü Node. js ve uygulama yerine WebSocket iletişimini işlemeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-214">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="sample-app"></a><span data-ttu-id="51d6d-215">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="51d6d-215">Sample app</span></span>

<span data-ttu-id="51d6d-216">Bu makaleye eşlik eden [örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) bir Echo uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-216">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="51d6d-217">WebSocket bağlantısı yapan bir Web sayfasına sahiptir ve sunucu istemciye geri aldığı tüm iletileri daha sonra sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="51d6d-217">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="51d6d-218">Uygulamayı bir komut isteminden çalıştırın (IIS Express ile Visual Studio 'dan çalışacak şekilde ayarlanmamış) ve adresine http://localhost:5000 gidin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-218">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="51d6d-219">Web sayfası, sol üstteki bağlantı durumunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="51d6d-219">The web page shows the connection status in the upper left:</span></span>

![Web sayfasının ilk durumu](websockets/_static/start.png)

<span data-ttu-id="51d6d-221">Gösterilen URL 'ye WebSocket isteği göndermek için **Bağlan** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-221">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="51d6d-222">Bir sınama iletisi girin ve **Gönder**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-222">Enter a test message and select **Send**.</span></span> <span data-ttu-id="51d6d-223">İşiniz bittiğinde **yuvayı kapat**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="51d6d-223">When done, select **Close Socket**.</span></span> <span data-ttu-id="51d6d-224">**Iletişim günlüğü** bölümünde her açık, gönder ve Kapat eylemi gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="51d6d-224">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web sayfasının ilk durumu](websockets/_static/end.png)

