---
title: ASP.NET Core WebSockets desteği
author: rick-anderson
description: ASP.NET Core WebSockets kullanmaya başlayacağınızı öğrenin.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: ede8064b5e77024b843357d4715869b3495b9147
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153689"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="91850-103">ASP.NET Core WebSockets desteği</span><span class="sxs-lookup"><span data-stu-id="91850-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="91850-104">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Barış Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="91850-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="91850-105">Bu makalede, ASP.NET Core WebSockets kullanmaya başlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="91850-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="91850-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)), TCP bağlantıları üzerinden kalıcı iki yönlü iletişim kanalları sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="91850-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="91850-107">Sohbet, Pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişim yararlı uygulamalarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="91850-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="91850-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="91850-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="91850-109">Bkz: [sonraki adımlar](#next-steps) daha fazla bilgi için bölüm.</span><span class="sxs-lookup"><span data-stu-id="91850-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="91850-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="91850-110">Prerequisites</span></span>

* <span data-ttu-id="91850-111">ASP.NET Core 1.1 veya üstü</span><span class="sxs-lookup"><span data-stu-id="91850-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="91850-112">ASP.NET Core destekleyen herhangi bir işletim sistemi:</span><span class="sxs-lookup"><span data-stu-id="91850-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="91850-113">Windows 7 / Windows Server 2008 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="91850-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="91850-114">Linux</span><span class="sxs-lookup"><span data-stu-id="91850-114">Linux</span></span>
  * <span data-ttu-id="91850-115">MacOS</span><span class="sxs-lookup"><span data-stu-id="91850-115">macOS</span></span>
  
* <span data-ttu-id="91850-116">Uygulama IIS ile Windows üzerinde çalışıyorsa:</span><span class="sxs-lookup"><span data-stu-id="91850-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="91850-117">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="91850-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="91850-118">IIS 8 / 8 IIS Express</span><span class="sxs-lookup"><span data-stu-id="91850-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="91850-119">WebSockets, IIS'de etkin olması gerekir (bkz [IIS/IIS Express Destek](#iisiis-express-support) bölüm.)</span><span class="sxs-lookup"><span data-stu-id="91850-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="91850-120">Uygulama çalışıyorsa [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="91850-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="91850-121">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="91850-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="91850-122">Desteklenen tarayıcılar için bkz: https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="91850-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="91850-123">WebSockets kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="91850-123">When to use WebSockets</span></span>

<span data-ttu-id="91850-124">WebSockets, doğrudan bir yuva bağlantısı ile çalışmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="91850-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="91850-125">Örneğin, gerçek zamanlı oyun ile olası en iyi performansı için WebSockets kullanın.</span><span class="sxs-lookup"><span data-stu-id="91850-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="91850-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) daha zengin bir uygulama modeli gerçek zamanlı işlevselliği sağlar, ancak bunun için ASP.NET yalnızca çalışır 4.x, ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="91850-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer app model for real-time functionality, but it only runs on ASP.NET 4.x, not ASP.NET Core.</span></span> <span data-ttu-id="91850-127">SignalR ASP.NET Core sürümü, ASP.NET Core 2.1 sürümüyle için zamanlandı.</span><span class="sxs-lookup"><span data-stu-id="91850-127">An ASP.NET Core version of SignalR is scheduled for release with ASP.NET Core 2.1.</span></span> <span data-ttu-id="91850-128">Bkz: [ASP.NET Core 2.1 üst düzey planlama](https://github.com/aspnet/Announcements/issues/288).</span><span class="sxs-lookup"><span data-stu-id="91850-128">See [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span></span>

<span data-ttu-id="91850-129">SignalR Core serbest kadar WebSockets kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="91850-129">Until SignalR Core is released, WebSockets can be used.</span></span> <span data-ttu-id="91850-130">Ancak, SignalR sağladığı özellikleri sağlanan ve gerekir geliştirici tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="91850-130">However, features that SignalR provides must be provided and supported by the developer.</span></span> <span data-ttu-id="91850-131">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="91850-131">For example:</span></span>

* <span data-ttu-id="91850-132">Geniş bir alternatif taşıma yöntemleri için otomatik geri dönüş kullanarak tarayıcı sürümleri için destek.</span><span class="sxs-lookup"><span data-stu-id="91850-132">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="91850-133">Bir bağlantı düştüğünde otomatik yeniden bağlanma.</span><span class="sxs-lookup"><span data-stu-id="91850-133">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="91850-134">Sunucuda veya yöntemleri çağırma istemciler için destek.</span><span class="sxs-lookup"><span data-stu-id="91850-134">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="91850-135">Birden çok sunucuya ölçeklendirme desteği.</span><span class="sxs-lookup"><span data-stu-id="91850-135">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="91850-136">Nasıl kullanılacağını</span><span class="sxs-lookup"><span data-stu-id="91850-136">How to use it</span></span>

* <span data-ttu-id="91850-137">Yükleme [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paket.</span><span class="sxs-lookup"><span data-stu-id="91850-137">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="91850-138">Ara yazılımını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="91850-138">Configure the middleware.</span></span>
* <span data-ttu-id="91850-139">WebSocket istekleri kabul edin.</span><span class="sxs-lookup"><span data-stu-id="91850-139">Accept WebSocket requests.</span></span>
* <span data-ttu-id="91850-140">İleti gönderme ve alma.</span><span class="sxs-lookup"><span data-stu-id="91850-140">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="91850-141">Ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="91850-141">Configure the middleware</span></span>

<span data-ttu-id="91850-142">WebSockets Ara yazılımında eklemek `Configure` yöntemi `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="91850-142">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="91850-143">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="91850-143">The following settings can be configured:</span></span>

* <span data-ttu-id="91850-144">`KeepAliveInterval` -Sık "ping" çerçevelerine proxy'leri emin olmak için istemci bağlantıyı açık tutmak göndermek nasıl.</span><span class="sxs-lookup"><span data-stu-id="91850-144">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="91850-145">`ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu.</span><span class="sxs-lookup"><span data-stu-id="91850-145">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="91850-146">İleri düzey kullanıcılar, bu veri boyutuna göre performans ayarlaması için değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="91850-146">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="91850-147">WebSocket istekleri kabul</span><span class="sxs-lookup"><span data-stu-id="91850-147">Accept WebSocket requests</span></span>

<span data-ttu-id="91850-148">İstek yaşam döngüsü devamındaki herhangi bir yerde (daha sonra `Configure` yöntemi veya bir MVC eylem, örneğin) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul et.</span><span class="sxs-lookup"><span data-stu-id="91850-148">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="91850-149">Aşağıdaki örnek alanından daha sonra kullanımda `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="91850-149">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="91850-150">Web yuvası isteğini üzerinde herhangi bir URL adresi gelebilir, ancak bu örnek kod yalnızca isteklerini kabul `/ws`.</span><span class="sxs-lookup"><span data-stu-id="91850-150">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="91850-151">İleti gönderme ve alma</span><span class="sxs-lookup"><span data-stu-id="91850-151">Send and receive messages</span></span>

<span data-ttu-id="91850-152">`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantı yükseltir ve sağlayan bir [WebSocket](/dotnet/core/api/system.net.websockets.websocket) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="91850-152">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="91850-153">Kullanım `WebSocket` ileti gönderme ve alma için nesne.</span><span class="sxs-lookup"><span data-stu-id="91850-153">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="91850-154">Web yuvası isteğini kabul eden daha önce gösterilen kod iletir `WebSocket` nesnesine bir `Echo` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="91850-154">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="91850-155">Kod bir ileti alır ve hemen aynı iletiyi gönderir.</span><span class="sxs-lookup"><span data-stu-id="91850-155">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="91850-156">Gönderilen ve istemci bağlantı kapatana kadar bir döngüde alınan ileti:</span><span class="sxs-lookup"><span data-stu-id="91850-156">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="91850-157">Döngü başlamadan önce WebSocket bağlantısı kabul ederek, ara yazılım ardışık düzenini sona erer.</span><span class="sxs-lookup"><span data-stu-id="91850-157">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="91850-158">Yuva kapatma sırasında ardışık düzen unwinds.</span><span class="sxs-lookup"><span data-stu-id="91850-158">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="91850-159">Diğer bir deyişle, istek ardışık düzeninde WebSocket kabul edildiğinde ilerleyen durdurur.</span><span class="sxs-lookup"><span data-stu-id="91850-159">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="91850-160">Döngü tamamlandı ve yuvanın kapalı olduğunda, isteği yeniden ardışık düzen devam eder.</span><span class="sxs-lookup"><span data-stu-id="91850-160">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="91850-161">IIS/IIS Express desteği</span><span class="sxs-lookup"><span data-stu-id="91850-161">IIS/IIS Express support</span></span>

<span data-ttu-id="91850-162">Windows Server 2012 veya üzeri ve Windows 8 veya üzeri ile IIS/IIS Express 8 veya üzeri WebSocket protokolü için desteği vardır.</span><span class="sxs-lookup"><span data-stu-id="91850-162">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="91850-163">Windows Server 2012 veya sonraki sürümlerde WebSocket protokolü için desteği etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="91850-163">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="91850-164">Kullanım **rol ve Özellik Ekle** gelen Sihirbazı **Yönet** menüsü veya bağlantıyı **Sunucu Yöneticisi'ni**.</span><span class="sxs-lookup"><span data-stu-id="91850-164">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="91850-165">Seçin **rol tabanlı veya özellik tabanlı yükleme**.</span><span class="sxs-lookup"><span data-stu-id="91850-165">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="91850-166">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="91850-166">Select **Next**.</span></span>
1. <span data-ttu-id="91850-167">(Yerel sunucu varsayılan olarak seçilidir) uygun sunucuyu seçin.</span><span class="sxs-lookup"><span data-stu-id="91850-167">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="91850-168">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="91850-168">Select **Next**.</span></span>
1. <span data-ttu-id="91850-169">Genişletme **Web sunucusu (IIS)** içinde **rolleri** ağaç, genişletin **Web sunucusu**, genişletin ve ardından **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="91850-169">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="91850-170">Seçin **WebSocket Protokolü**.</span><span class="sxs-lookup"><span data-stu-id="91850-170">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="91850-171">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="91850-171">Select **Next**.</span></span>
1. <span data-ttu-id="91850-172">Ek özellikleri olmayan gerekirse seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="91850-172">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="91850-173">**Yükle**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="91850-173">Select **Install**.</span></span>
1. <span data-ttu-id="91850-174">Yükleme tamamlandığında seçin **Kapat** sihirbazdan çıkmak için.</span><span class="sxs-lookup"><span data-stu-id="91850-174">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="91850-175">Windows 8 veya sonraki sürümlerde WebSocket protokolü için desteği etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="91850-175">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="91850-176">Gidin **Denetim Masası** > **programları** > **programlar ve Özellikler** > **kapatma Windows özellikleri ya da kapalı** (sol tarafında ekranı).</span><span class="sxs-lookup"><span data-stu-id="91850-176">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="91850-177">Aşağıdaki düğümler açın: **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="91850-177">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="91850-178">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="91850-178">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="91850-179">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="91850-179">Select **OK**.</span></span>

<span data-ttu-id="91850-180">**WebSocket Socket.IO üzerinde node.js kullanırken devre dışı bırak**</span><span class="sxs-lookup"><span data-stu-id="91850-180">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="91850-181">WebSocket desteği kullanıyorsanız [Socket.IO](https://socket.io/) üzerinde [Node.js](https://nodejs.org/), varsayılan IIS WebSocket modülü kullanılarak devre dışı bırakma `webSocket` öğesinde *web.config* veya *applicationHost.config*. Bu adımda gerçekleştirilen değil WebSocket iletişim yerine Node.js ve uygulamayı işlemek IIS WebSocket modülü çalışır.</span><span class="sxs-lookup"><span data-stu-id="91850-181">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="91850-182">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="91850-182">Next steps</span></span>

<span data-ttu-id="91850-183">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) bu eşlik makaledir Yankı uygulama.</span><span class="sxs-lookup"><span data-stu-id="91850-183">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="91850-184">WebSocket bağlantılar sağlayan bir web sayfasına sahip ve sunucunun aldığı iletileri istemciye yeniden gönderir.</span><span class="sxs-lookup"><span data-stu-id="91850-184">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="91850-185">(Bunu ayarlanmamış yukarı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden uygulamayı çalıştırın ve gidin http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="91850-185">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="91850-186">Web sayfasının sol üst bağlantı durumunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="91850-186">The web page shows the connection status in the upper left:</span></span>

![Web sayfasının ilk durumu](websockets/_static/start.png)

<span data-ttu-id="91850-188">Seçin **Bağlan** gösterilen URL'yi bir Web yuvası isteğini göndermek için.</span><span class="sxs-lookup"><span data-stu-id="91850-188">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="91850-189">Sınama iletisi girin ve **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="91850-189">Enter a test message and select **Send**.</span></span> <span data-ttu-id="91850-190">İşiniz bittiğinde, seçin **Kapat yuva**.</span><span class="sxs-lookup"><span data-stu-id="91850-190">When done, select **Close Socket**.</span></span> <span data-ttu-id="91850-191">**İletişim günlük** bölüm raporları her açık, gönderme ve kapatma eylemi olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="91850-191">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web sayfasının ilk durumu](websockets/_static/end.png)
