---
title: ASP.NET Core WebSockets desteği
author: rick-anderson
description: ASP.NET core'da WebSockets kullanmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: fc3f70fb888797216b2ccc911a9f69eaae6ac01c
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577736"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="0435b-103">ASP.NET Core WebSockets desteği</span><span class="sxs-lookup"><span data-stu-id="0435b-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="0435b-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="0435b-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="0435b-105">Bu makalede, WebSockets içinde ASP.NET Core ile çalışmaya başlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="0435b-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="0435b-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="0435b-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="0435b-107">Sohbet, Pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişim yararlanan uygulamalarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="0435b-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="0435b-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0435b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="0435b-109">Bkz: [sonraki adımlar](#next-steps) bölümünde daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="0435b-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0435b-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="0435b-110">Prerequisites</span></span>

* <span data-ttu-id="0435b-111">ASP.NET Core 1.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="0435b-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="0435b-112">ASP.NET Core destekleyen tüm işletim Sistemleri:</span><span class="sxs-lookup"><span data-stu-id="0435b-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="0435b-113">Windows 7 / Windows Server 2008 veya üstü</span><span class="sxs-lookup"><span data-stu-id="0435b-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="0435b-114">Linux</span><span class="sxs-lookup"><span data-stu-id="0435b-114">Linux</span></span>
  * <span data-ttu-id="0435b-115">MacOS</span><span class="sxs-lookup"><span data-stu-id="0435b-115">macOS</span></span>
  
* <span data-ttu-id="0435b-116">IIS ile Windows uygulama çalışıyorsa:</span><span class="sxs-lookup"><span data-stu-id="0435b-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="0435b-117">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="0435b-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="0435b-118">IIS 8 / 8 IIS Express</span><span class="sxs-lookup"><span data-stu-id="0435b-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="0435b-119">IIS WebSockets etkinleştirildi (bkz [IIS/IIS Express Destek](#iisiis-express-support) bölümüne.)</span><span class="sxs-lookup"><span data-stu-id="0435b-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="0435b-120">Uygulama çalışıyorsa [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="0435b-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="0435b-121">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="0435b-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="0435b-122">Desteklenen tarayıcılar için bkz: https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="0435b-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="0435b-123">Ne zaman WebSockets kullanılır?</span><span class="sxs-lookup"><span data-stu-id="0435b-123">When to use WebSockets</span></span>

<span data-ttu-id="0435b-124">WebSockets, doğrudan bir yuva bağlantı ile çalışması için kullanın.</span><span class="sxs-lookup"><span data-stu-id="0435b-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="0435b-125">Örneğin, gerçek zamanlı oyun ile mümkün olan en iyi performansı için WebSockets kullanın.</span><span class="sxs-lookup"><span data-stu-id="0435b-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="0435b-126">[ASP.NET Core SignalR](xref:signalr/introduction) , uygulamalara gerçek zamanlı web işlevselliği ekleme basitleştiren bir kitaplık.</span><span class="sxs-lookup"><span data-stu-id="0435b-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="0435b-127">Mümkün olduğunda WebSockets kullanır.</span><span class="sxs-lookup"><span data-stu-id="0435b-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="0435b-128">WebSockets kullanma</span><span class="sxs-lookup"><span data-stu-id="0435b-128">How to use WebSockets</span></span>

* <span data-ttu-id="0435b-129">Yükleme [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paket.</span><span class="sxs-lookup"><span data-stu-id="0435b-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="0435b-130">Ara yazılımını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="0435b-130">Configure the middleware.</span></span>
* <span data-ttu-id="0435b-131">WebSocket isteklerini kabul etmek.</span><span class="sxs-lookup"><span data-stu-id="0435b-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="0435b-132">İletileri gönderip yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="0435b-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="0435b-133">Ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0435b-133">Configure the middleware</span></span>

<span data-ttu-id="0435b-134">WebSockets Ara yazılımında ekleme `Configure` yöntemi `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="0435b-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="0435b-135">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="0435b-135">The following settings can be configured:</span></span>

* <span data-ttu-id="0435b-136">`KeepAliveInterval` -Nasıl "ping" çerçeveler proxy'leri emin olmak için istemci bağlantıyı açık tutma genellikle gönderilecek.</span><span class="sxs-lookup"><span data-stu-id="0435b-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="0435b-137">`ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu.</span><span class="sxs-lookup"><span data-stu-id="0435b-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="0435b-138">Gelişmiş kullanıcılar, bu verilerin boyutunu temel alan performans ayarı için değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="0435b-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="0435b-139">WebSocket isteklerini kabul etmek</span><span class="sxs-lookup"><span data-stu-id="0435b-139">Accept WebSocket requests</span></span>

<span data-ttu-id="0435b-140">İsteği yaşam döngüsünün sonraki yere (daha sonra `Configure` yöntemi veya örneğin bir MVC eylemi) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="0435b-140">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="0435b-141">Aşağıdaki örnek daha sonra buna dandır `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="0435b-141">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="0435b-142">Web yuvası isteğini herhangi bir URL gelebilir, ancak bu örnek kod, yalnızca isteklerini kabul eder `/ws`.</span><span class="sxs-lookup"><span data-stu-id="0435b-142">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="0435b-143">İleti gönderin ve alın</span><span class="sxs-lookup"><span data-stu-id="0435b-143">Send and receive messages</span></span>

<span data-ttu-id="0435b-144">`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantısı yükseltir ve sağlayan bir [WebSocket](/dotnet/core/api/system.net.websockets.websocket) nesne.</span><span class="sxs-lookup"><span data-stu-id="0435b-144">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="0435b-145">Kullanım `WebSocket` ileti göndermek ve almak için nesne.</span><span class="sxs-lookup"><span data-stu-id="0435b-145">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="0435b-146">Web yuvası isteğini kabul eden daha önce gösterilen kod geçirir `WebSocket` nesnesini bir `Echo` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0435b-146">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="0435b-147">Kod, bir ileti alır ve hemen aynı iletiyi gönderir.</span><span class="sxs-lookup"><span data-stu-id="0435b-147">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="0435b-148">Gönderilen ve istemci bağlantı kapatana kadar bir döngüde alınan ileti:</span><span class="sxs-lookup"><span data-stu-id="0435b-148">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="0435b-149">Döngü başlamadan önce WebSocket bağlantısı kabul ederek, ara yazılım ardışık düzenini sona erer.</span><span class="sxs-lookup"><span data-stu-id="0435b-149">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="0435b-150">Yuva kapatıldıktan sonra işlem hattını geriye doğru izler.</span><span class="sxs-lookup"><span data-stu-id="0435b-150">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="0435b-151">Diğer bir deyişle, işlem hattı, WebSocket kabul edildiğinde ilerletme isteğini durdurur.</span><span class="sxs-lookup"><span data-stu-id="0435b-151">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="0435b-152">Döngü tamamlandıktan ve yuva kapalı olduğunda, isteği yeniden işlem hattı devam eder.</span><span class="sxs-lookup"><span data-stu-id="0435b-152">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="0435b-153">IIS/IIS Express desteği</span><span class="sxs-lookup"><span data-stu-id="0435b-153">IIS/IIS Express support</span></span>

<span data-ttu-id="0435b-154">Windows Server 2012 veya üzeri ve Windows 8 veya üzeri ile IIS/IIS Express 8 veya üzeri, WebSocket protokolü için desteği vardır.</span><span class="sxs-lookup"><span data-stu-id="0435b-154">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="0435b-155">Windows Server 2012 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="0435b-155">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="0435b-156">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="0435b-156">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="0435b-157">Seçin **rol tabanlı veya özellik tabanlı yükleme**.</span><span class="sxs-lookup"><span data-stu-id="0435b-157">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="0435b-158">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0435b-158">Select **Next**.</span></span>
1. <span data-ttu-id="0435b-159">(Yerel sunucu varsayılan olarak seçilidir) uygun sunucuyu seçin.</span><span class="sxs-lookup"><span data-stu-id="0435b-159">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="0435b-160">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0435b-160">Select **Next**.</span></span>
1. <span data-ttu-id="0435b-161">Genişletin **Web sunucusu (IIS)** içinde **rolleri** ağacında, genişletme **Web sunucusu**ve ardından **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="0435b-161">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="0435b-162">Seçin **WebSocket Protokolü**.</span><span class="sxs-lookup"><span data-stu-id="0435b-162">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="0435b-163">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0435b-163">Select **Next**.</span></span>
1. <span data-ttu-id="0435b-164">Ek özelliklere ihtiyaç duyulmayan, seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="0435b-164">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="0435b-165">**Yükle**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="0435b-165">Select **Install**.</span></span>
1. <span data-ttu-id="0435b-166">Yükleme tamamlandığında seçin **Kapat** sihirbazdan çıkmak için.</span><span class="sxs-lookup"><span data-stu-id="0435b-166">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="0435b-167">Windows 8 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="0435b-167">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="0435b-168">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="0435b-168">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="0435b-169">Aşağıdaki düğümler açın: **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="0435b-169">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="0435b-170">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="0435b-170">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="0435b-171">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="0435b-171">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="0435b-172">Socket.io, Node.js dosyasını kullanırken WebSocket devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="0435b-172">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="0435b-173">WebSocket desteği kullanıyorsanız [socket.io](https://socket.io/) üzerinde [Node.js](https://nodejs.org/), varsayılan IIS WebSocket modülünü kullanarak devre dışı bırak `webSocket` öğesinde *web.config* veya *applicationHost.config*. Bu adım yapılamaz ve iletişim WebSocket yerine Node.js uygulaması işlemek IIS WebSocket modülü çalışır.</span><span class="sxs-lookup"><span data-stu-id="0435b-173">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="0435b-174">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0435b-174">Next steps</span></span>

<span data-ttu-id="0435b-175">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) bu eşlik Yankı uygulama makaledir.</span><span class="sxs-lookup"><span data-stu-id="0435b-175">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="0435b-176">WebSocket bağlantılarını sağlayan bir web sayfasına sahip ve sunucu istemciye herhangi bir ileti aldıktan sonra yeniden gönderir.</span><span class="sxs-lookup"><span data-stu-id="0435b-176">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="0435b-177">(Bunu ayarlanmadı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden uygulamayı çalıştırın ve gidin http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="0435b-177">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="0435b-178">Web sayfasının sol üst bağlantı durumunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="0435b-178">The web page shows the connection status in the upper left:</span></span>

![Web sayfasının ilk durumu](websockets/_static/start.png)

<span data-ttu-id="0435b-180">Seçin **Connect** gösterilen URL'yi bir Web yuvası isteğini göndermek için.</span><span class="sxs-lookup"><span data-stu-id="0435b-180">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="0435b-181">Test iletisi girin ve seçin **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="0435b-181">Enter a test message and select **Send**.</span></span> <span data-ttu-id="0435b-182">İşiniz bittiğinde, seçin **Kapat yuva**.</span><span class="sxs-lookup"><span data-stu-id="0435b-182">When done, select **Close Socket**.</span></span> <span data-ttu-id="0435b-183">**İletişim günlük** bölüm açık, her gönderme bildirir ve kapatma eylemi,'olmuyor.</span><span class="sxs-lookup"><span data-stu-id="0435b-183">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web sayfasının ilk durumu](websockets/_static/end.png)
