---
title: ASP.NET Core WebSockets desteği
author: rick-anderson
description: ASP.NET core'da WebSockets kullanmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/10/2019
uid: fundamentals/websockets
ms.openlocfilehash: 4c49a5349c0718e5c59f30e6d51caf7a43fa0454
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458452"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="b3389-103">ASP.NET Core WebSockets desteği</span><span class="sxs-lookup"><span data-stu-id="b3389-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="b3389-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="b3389-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="b3389-105">Bu makalede, WebSockets içinde ASP.NET Core ile çalışmaya başlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="b3389-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="b3389-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="b3389-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="b3389-107">Sohbet, Pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişim yararlanan uygulamalarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b3389-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="b3389-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b3389-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="b3389-109">[Çalıştırma](#sample-app).</span><span class="sxs-lookup"><span data-stu-id="b3389-109">[How to run](#sample-app).</span></span>

## <a name="signalr"></a><span data-ttu-id="b3389-110">SignalR</span><span class="sxs-lookup"><span data-stu-id="b3389-110">SignalR</span></span>

<span data-ttu-id="b3389-111">[ASP.NET Core SignalR](xref:signalr/introduction) , uygulamalara gerçek zamanlı web işlevselliği ekleme basitleştiren bir kitaplık.</span><span class="sxs-lookup"><span data-stu-id="b3389-111">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="b3389-112">Mümkün olduğunda WebSockets kullanır.</span><span class="sxs-lookup"><span data-stu-id="b3389-112">It uses WebSockets whenever possible.</span></span>

<span data-ttu-id="b3389-113">Çoğu uygulama için ham WebSockets üzerinden SignalR öneririz.</span><span class="sxs-lookup"><span data-stu-id="b3389-113">For most applications, we recommend SignalR over raw WebSockets.</span></span> <span data-ttu-id="b3389-114">SignalR taşıma geri dönüş WebSockets kullanılabilir olduğu ortamlar için sağlar.</span><span class="sxs-lookup"><span data-stu-id="b3389-114">SignalR provides transport fallback for environments where WebSockets is not available.</span></span> <span data-ttu-id="b3389-115">Ayrıca, bir basit uzaktan yordam çağrısı uygulama modeli sağlar.</span><span class="sxs-lookup"><span data-stu-id="b3389-115">It also provides a simple remote procedure call app model.</span></span> <span data-ttu-id="b3389-116">Ve çoğu senaryoda, SignalR ham WebSockets kullanmaya kıyasla hiçbir önemli performans dezavantajı vardır.</span><span class="sxs-lookup"><span data-stu-id="b3389-116">And in most scenarios, SignalR has no significant performance disadvantage compared to using raw WebSockets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3389-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b3389-117">Prerequisites</span></span>

* <span data-ttu-id="b3389-118">ASP.NET Core 1.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="b3389-118">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="b3389-119">ASP.NET Core destekleyen tüm işletim Sistemleri:</span><span class="sxs-lookup"><span data-stu-id="b3389-119">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="b3389-120">Windows 7 / Windows Server 2008 veya üstü</span><span class="sxs-lookup"><span data-stu-id="b3389-120">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="b3389-121">Linux</span><span class="sxs-lookup"><span data-stu-id="b3389-121">Linux</span></span>
  * <span data-ttu-id="b3389-122">macOS</span><span class="sxs-lookup"><span data-stu-id="b3389-122">macOS</span></span>
  
* <span data-ttu-id="b3389-123">IIS ile Windows uygulama çalışıyorsa:</span><span class="sxs-lookup"><span data-stu-id="b3389-123">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="b3389-124">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="b3389-124">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="b3389-125">IIS 8 / 8 IIS Express</span><span class="sxs-lookup"><span data-stu-id="b3389-125">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="b3389-126">WebSockets etkinleştirilmelidir (bkz [IIS/IIS Express Destek](#iisiis-express-support) bölümüne.).</span><span class="sxs-lookup"><span data-stu-id="b3389-126">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="b3389-127">Uygulama çalışıyorsa [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="b3389-127">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="b3389-128">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="b3389-128">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="b3389-129">Desteklenen tarayıcılar için bkz: https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="b3389-129">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

::: moniker range="< aspnetcore-2.1"

## <a name="nuget-package"></a><span data-ttu-id="b3389-130">NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="b3389-130">NuGet package</span></span>

<span data-ttu-id="b3389-131">Yükleme [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paket.</span><span class="sxs-lookup"><span data-stu-id="b3389-131">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>

::: moniker-end

## <a name="configure-the-middleware"></a><span data-ttu-id="b3389-132">Ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="b3389-132">Configure the middleware</span></span>


<span data-ttu-id="b3389-133">WebSockets Ara yazılımında ekleme `Configure` yöntemi `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="b3389-133">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b3389-134">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="b3389-134">The following settings can be configured:</span></span>

* <span data-ttu-id="b3389-135">`KeepAliveInterval` -Nasıl "ping" çerçeveler proxy'leri emin olmak için istemci bağlantıyı açık tutma genellikle gönderilecek.</span><span class="sxs-lookup"><span data-stu-id="b3389-135">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="b3389-136">İki dakika varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="b3389-136">The default is two minutes.</span></span>
* <span data-ttu-id="b3389-137">`ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu.</span><span class="sxs-lookup"><span data-stu-id="b3389-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="b3389-138">Gelişmiş kullanıcılar, bu verilerin boyutunu temel alan performans ayarı için değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="b3389-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="b3389-139">Varsayılan değer 4 KB'dir.</span><span class="sxs-lookup"><span data-stu-id="b3389-139">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b3389-140">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="b3389-140">The following settings can be configured:</span></span>

* <span data-ttu-id="b3389-141">`KeepAliveInterval` -Nasıl "ping" çerçeveler proxy'leri emin olmak için istemci bağlantıyı açık tutma genellikle gönderilecek.</span><span class="sxs-lookup"><span data-stu-id="b3389-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="b3389-142">İki dakika varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="b3389-142">The default is two minutes.</span></span>
* <span data-ttu-id="b3389-143">`ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu.</span><span class="sxs-lookup"><span data-stu-id="b3389-143">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="b3389-144">Gelişmiş kullanıcılar, bu verilerin boyutunu temel alan performans ayarı için değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="b3389-144">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="b3389-145">Varsayılan değer 4 KB'dir.</span><span class="sxs-lookup"><span data-stu-id="b3389-145">The default is 4 KB.</span></span>
* <span data-ttu-id="b3389-146">`AllowedOrigins` -WebSocket istekleri için izin verilen çıkış noktaları üstbilgi değerleri listesi.</span><span class="sxs-lookup"><span data-stu-id="b3389-146">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="b3389-147">Varsayılan olarak, tüm çıkış noktaları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="b3389-147">By default, all origins are allowed.</span></span> <span data-ttu-id="b3389-148">"WebSocket kaynak kısıtlama" Ayrıntılar için aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="b3389-148">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

## <a name="accept-websocket-requests"></a><span data-ttu-id="b3389-149">WebSocket isteklerini kabul etmek</span><span class="sxs-lookup"><span data-stu-id="b3389-149">Accept WebSocket requests</span></span>

<span data-ttu-id="b3389-150">İsteği yaşam döngüsünün sonraki yere (daha sonra `Configure` yöntemi veya örneğin bir eylem yöntemi) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="b3389-150">Somewhere later in the request life cycle (later in the `Configure` method or in an action method, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="b3389-151">Aşağıdaki örnek daha sonra buna dandır `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="b3389-151">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="b3389-152">Web yuvası isteğini herhangi bir URL gelebilir, ancak bu örnek kod, yalnızca isteklerini kabul eder `/ws`.</span><span class="sxs-lookup"><span data-stu-id="b3389-152">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

<span data-ttu-id="b3389-153">Bir WebSocket kullanırken, **gerekir** bağlantının süresi için ara yazılım ardışık düzenini tutun.</span><span class="sxs-lookup"><span data-stu-id="b3389-153">When using a WebSocket, you **must** keep the middleware pipeline running for the duration of the connection.</span></span> <span data-ttu-id="b3389-154">Ara yazılım ardışık düzenini sona erdikten sonra WebSocket ileti gönderileceği denerseniz, aşağıdaki gibi bir özel durum karşılaşabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="b3389-154">If you attempt to send or receive a WebSocket message after the middleware pipeline ends, you may get an exception like the following:</span></span>

```
System.Net.WebSockets.WebSocketException (0x80004005): The remote party closed the WebSocket connection without completing the close handshake. ---> System.ObjectDisposedException: Cannot write to the response body, the response has completed.
Object name: 'HttpResponseStream'.
```

<span data-ttu-id="b3389-155">Bir Web yuvası için veri yazmak için bir arka plan hizmeti kullanıyorsanız, çalışan bir ara yazılım ardışık düzenini tutmak emin olun.</span><span class="sxs-lookup"><span data-stu-id="b3389-155">If you're using a background service to write data to a WebSocket, make sure you keep the middleware pipeline running.</span></span> <span data-ttu-id="b3389-156">Kullanarak bunu bir <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span><span class="sxs-lookup"><span data-stu-id="b3389-156">Do this by using a <xref:System.Threading.Tasks.TaskCompletionSource%601>.</span></span> <span data-ttu-id="b3389-157">Geçirmek `TaskCompletionSource` arka plan için hizmet ve bu çağrı <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> ile WebSocket bitirdiğinizde.</span><span class="sxs-lookup"><span data-stu-id="b3389-157">Pass the `TaskCompletionSource` to your background service and have it call <xref:System.Threading.Tasks.TaskCompletionSource%601.TrySetResult%2A> when you finish with the WebSocket.</span></span> <span data-ttu-id="b3389-158">Ardından `await` <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> aşağıdaki örnekte gösterildiği gibi istek sırasında özelliği:</span><span class="sxs-lookup"><span data-stu-id="b3389-158">Then `await` the <xref:System.Threading.Tasks.TaskCompletionSource%601.Task> property during the request, as shown in the following example:</span></span>

```csharp
app.Use(async (context, next) => {
    var socket = await context.WebSockets.AcceptWebSocketAsync();
    var socketFinishedTcs = new TaskCompletionSource<object>();

    BackgroundSocketProcessor.AddSocket(socket, socketFinishedTcs); 

    await socketFinishedTcs.Task;
});
```
<span data-ttu-id="b3389-159">Çok yakında bir eylem yönteminden dönerseniz kapalı WebSocket özel durum de oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="b3389-159">The WebSocket closed exception can also happen if you return too soon from an action method.</span></span> <span data-ttu-id="b3389-160">Bir eylem yöntemi bir yuvaya kabul ederseniz, eylem yönteminden döndürmeden önce tamamlamak için yuva kullanan kod için bekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3389-160">If you accept a socket in an action method, wait for the code that uses the socket to complete before returning from the action method.</span></span>

<span data-ttu-id="b3389-161">Hiçbir zaman kullanmayın `Task.Wait()`, `Task.Result`, veya iş parçacığı oluşturma sorunları, ciddi neden olabileceği yuva tamamlamak beklenecek benzer engelleme çağrıları.</span><span class="sxs-lookup"><span data-stu-id="b3389-161">Never use `Task.Wait()`, `Task.Result`, or similar blocking calls to wait for the socket to complete, as that can cause serious threading issues.</span></span> <span data-ttu-id="b3389-162">Her zaman `await`.</span><span class="sxs-lookup"><span data-stu-id="b3389-162">Always use `await`.</span></span>

## <a name="send-and-receive-messages"></a><span data-ttu-id="b3389-163">İleti gönderin ve alın</span><span class="sxs-lookup"><span data-stu-id="b3389-163">Send and receive messages</span></span>

<span data-ttu-id="b3389-164">`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantısı yükseltir ve sağlayan bir [WebSocket](/dotnet/core/api/system.net.websockets.websocket) nesne.</span><span class="sxs-lookup"><span data-stu-id="b3389-164">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="b3389-165">Kullanım `WebSocket` ileti göndermek ve almak için nesne.</span><span class="sxs-lookup"><span data-stu-id="b3389-165">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="b3389-166">Web yuvası isteğini kabul eden daha önce gösterilen kod geçirir `WebSocket` nesnesini bir `Echo` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b3389-166">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="b3389-167">Kod, bir ileti alır ve hemen aynı iletiyi gönderir.</span><span class="sxs-lookup"><span data-stu-id="b3389-167">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="b3389-168">Gönderilen ve istemci bağlantı kapatana kadar bir döngüde alınan ileti:</span><span class="sxs-lookup"><span data-stu-id="b3389-168">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

<span data-ttu-id="b3389-169">Döngü başlamadan önce WebSocket bağlantısı kabul ederek, ara yazılım ardışık düzenini sona erer.</span><span class="sxs-lookup"><span data-stu-id="b3389-169">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="b3389-170">Yuva kapatıldıktan sonra işlem hattını geriye doğru izler.</span><span class="sxs-lookup"><span data-stu-id="b3389-170">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="b3389-171">Diğer bir deyişle, işlem hattı, WebSocket kabul edildiğinde ilerletme isteğini durdurur.</span><span class="sxs-lookup"><span data-stu-id="b3389-171">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="b3389-172">Döngü tamamlandıktan ve yuva kapalı olduğunda, isteği yeniden işlem hattı devam eder.</span><span class="sxs-lookup"><span data-stu-id="b3389-172">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="handle-client-disconnects"></a><span data-ttu-id="b3389-173">Tanıtıcı istemci bağlantısını keser</span><span class="sxs-lookup"><span data-stu-id="b3389-173">Handle client disconnects</span></span>

<span data-ttu-id="b3389-174">İstemci bağlantı kaybı nedeniyle kestiğinde sunucu otomatik olarak haberdar değildir.</span><span class="sxs-lookup"><span data-stu-id="b3389-174">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="b3389-175">Yalnızca istemci, internet bağlantısı kaybolursa, yapılamaz gönderirse, sunucunun bir bağlantı kesme iletisi alır.</span><span class="sxs-lookup"><span data-stu-id="b3389-175">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="b3389-176">Bu durum oluştuğunda bazı işlemler yapması istiyorsanız, hiçbir şey belirli bir zaman penceresi içinde bir istemciden alındıktan sonra bir zaman aşımını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b3389-176">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="b3389-177">İstemci her zaman ileti gönderme değildir ve yalnızca bağlantı boşta gittiği zaman aşımına uğramak üzere istemiyorsanız, istemci her X saniyede bir ping ileti göndermek için bir zamanlayıcı kullanmak vardır.</span><span class="sxs-lookup"><span data-stu-id="b3389-177">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="b3389-178">Bir ileti 2 içinde geldiyseniz henüz sunucuda\*X saniye sonra önceki bir, bağlantı ve istemci bağlantısı kesildi rapor sonlandırın.</span><span class="sxs-lookup"><span data-stu-id="b3389-178">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="b3389-179">Ping yapılacak tutabilir ağ gecikmeleri için ek süre bırakmak iki kez beklenen zaman aralığı için bekleyin.</span><span class="sxs-lookup"><span data-stu-id="b3389-179">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="b3389-180">WebSocket kaynak kısıtlama</span><span class="sxs-lookup"><span data-stu-id="b3389-180">WebSocket origin restriction</span></span>

<span data-ttu-id="b3389-181">CORS tarafından sağlanan korumaları WebSockets için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="b3389-181">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="b3389-182">Tarayıcılar **değil**:</span><span class="sxs-lookup"><span data-stu-id="b3389-182">Browsers do **not**:</span></span>

* <span data-ttu-id="b3389-183">CORS uçuş öncesi istekler gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="b3389-183">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="b3389-184">Belirtilen kısıtlamalarını dikkate `Access-Control` WebSocket istekleri yaparken üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="b3389-184">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="b3389-185">Ancak, tarayıcılar gönderebilirsiniz `Origin` WebSocket istekleri gönderirken, üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="b3389-185">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="b3389-186">Uygulamalar, beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için bu üstbilgileri doğrulamak için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="b3389-186">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="b3389-187">Sunucunuz koyduysanız "https://server.com"ve istemci üzerindeki barındırma"https://client.com", Ekle "https://client.com" için `AllowedOrigins` WebSockets doğrulamak için listesi.</span><span class="sxs-lookup"><span data-stu-id="b3389-187">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="b3389-188">`Origin` Üst bilgisi, istemcinin ve gibi denetlenir `Referer` başlık sahte.</span><span class="sxs-lookup"><span data-stu-id="b3389-188">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="b3389-189">Yapmak **değil** bir kimlik doğrulama mekanizması bu üst bilgi kullan.</span><span class="sxs-lookup"><span data-stu-id="b3389-189">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="b3389-190">IIS/IIS Express desteği</span><span class="sxs-lookup"><span data-stu-id="b3389-190">IIS/IIS Express support</span></span>

<span data-ttu-id="b3389-191">Windows Server 2012 veya üzeri ve Windows 8 veya üzeri ile IIS/IIS Express 8 veya üzeri, WebSocket protokolü için desteği vardır.</span><span class="sxs-lookup"><span data-stu-id="b3389-191">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="b3389-192">WebSockets, IIS Express kullanırken her zaman etkindir.</span><span class="sxs-lookup"><span data-stu-id="b3389-192">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="b3389-193">IIS WebSockets etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="b3389-193">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="b3389-194">Windows Server 2012 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="b3389-194">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="b3389-195">Bu adımları IIS Express kullanırken gerekli değildir</span><span class="sxs-lookup"><span data-stu-id="b3389-195">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="b3389-196">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="b3389-196">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="b3389-197">Seçin **rol tabanlı veya özellik tabanlı yükleme**.</span><span class="sxs-lookup"><span data-stu-id="b3389-197">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="b3389-198">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b3389-198">Select **Next**.</span></span>
1. <span data-ttu-id="b3389-199">(Yerel sunucu varsayılan olarak seçilidir) uygun sunucuyu seçin.</span><span class="sxs-lookup"><span data-stu-id="b3389-199">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="b3389-200">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b3389-200">Select **Next**.</span></span>
1. <span data-ttu-id="b3389-201">Genişletin **Web sunucusu (IIS)** içinde **rolleri** ağacında, genişletme **Web sunucusu**ve ardından **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="b3389-201">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="b3389-202">Seçin **WebSocket Protokolü**.</span><span class="sxs-lookup"><span data-stu-id="b3389-202">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="b3389-203">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b3389-203">Select **Next**.</span></span>
1. <span data-ttu-id="b3389-204">Ek özelliklere ihtiyaç duyulmayan, seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="b3389-204">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="b3389-205">**Yükle**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b3389-205">Select **Install**.</span></span>
1. <span data-ttu-id="b3389-206">Yükleme tamamlandığında seçin **Kapat** sihirbazdan çıkmak için.</span><span class="sxs-lookup"><span data-stu-id="b3389-206">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="b3389-207">Windows 8 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="b3389-207">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="b3389-208">Bu adımları IIS Express kullanırken gerekli değildir</span><span class="sxs-lookup"><span data-stu-id="b3389-208">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="b3389-209">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="b3389-209">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="b3389-210">Aşağıdaki düğümler açın: **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="b3389-210">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="b3389-211">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="b3389-211">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="b3389-212">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="b3389-212">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="b3389-213">Socket.io, Node.js dosyasını kullanırken WebSocket devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="b3389-213">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="b3389-214">WebSocket desteği kullanıyorsanız [socket.io](https://socket.io/) üzerinde [Node.js](https://nodejs.org/), varsayılan IIS WebSocket modülünü kullanarak devre dışı bırak `webSocket` öğesinde *web.config* veya *applicationHost.config*. Bu adım yapılamaz ve iletişim WebSocket yerine Node.js uygulaması işlemek IIS WebSocket modülü çalışır.</span><span class="sxs-lookup"><span data-stu-id="b3389-214">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="sample-app"></a><span data-ttu-id="b3389-215">Örnek uygulama</span><span class="sxs-lookup"><span data-stu-id="b3389-215">Sample app</span></span>

<span data-ttu-id="b3389-216">[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) bu eşlik Yankı uygulama makaledir.</span><span class="sxs-lookup"><span data-stu-id="b3389-216">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="b3389-217">WebSocket bağlantılarını sağlayan bir web sayfasına sahip ve sunucu istemciye herhangi bir ileti aldıktan sonra yeniden gönderir.</span><span class="sxs-lookup"><span data-stu-id="b3389-217">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="b3389-218">(Bunu ayarlanmadı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden uygulamayı çalıştırın ve gidin http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="b3389-218">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="b3389-219">Web sayfasının sol üst bağlantı durumunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="b3389-219">The web page shows the connection status in the upper left:</span></span>

![Web sayfasının ilk durumu](websockets/_static/start.png)

<span data-ttu-id="b3389-221">Seçin **Connect** gösterilen URL'yi bir Web yuvası isteğini göndermek için.</span><span class="sxs-lookup"><span data-stu-id="b3389-221">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="b3389-222">Test iletisi girin ve seçin **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="b3389-222">Enter a test message and select **Send**.</span></span> <span data-ttu-id="b3389-223">İşiniz bittiğinde, seçin **Kapat yuva**.</span><span class="sxs-lookup"><span data-stu-id="b3389-223">When done, select **Close Socket**.</span></span> <span data-ttu-id="b3389-224">**İletişim günlük** bölüm açık, her gönderme bildirir ve kapatma eylemi,'olmuyor.</span><span class="sxs-lookup"><span data-stu-id="b3389-224">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web sayfasının ilk durumu](websockets/_static/end.png)

