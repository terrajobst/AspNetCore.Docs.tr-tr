---
title: ASP.NET Core WebSockets desteği
author: rick-anderson
description: ASP.NET core'da WebSockets kullanmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: b0f1aeff6c7a5777993459274293ba23f2d9dc12
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206746"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="c37ba-103">ASP.NET Core WebSockets desteği</span><span class="sxs-lookup"><span data-stu-id="c37ba-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="c37ba-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="c37ba-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="c37ba-105">Bu makalede, WebSockets içinde ASP.NET Core ile çalışmaya başlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c37ba-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="c37ba-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="c37ba-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="c37ba-107">Sohbet, Pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişim yararlanan uygulamalarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c37ba-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="c37ba-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c37ba-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="c37ba-109">Bkz: [sonraki adımlar](#next-steps) bölümünde daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="c37ba-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c37ba-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="c37ba-110">Prerequisites</span></span>

* <span data-ttu-id="c37ba-111">ASP.NET Core 1.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c37ba-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="c37ba-112">ASP.NET Core destekleyen tüm işletim Sistemleri:</span><span class="sxs-lookup"><span data-stu-id="c37ba-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="c37ba-113">Windows 7 / Windows Server 2008 veya üstü</span><span class="sxs-lookup"><span data-stu-id="c37ba-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="c37ba-114">Linux</span><span class="sxs-lookup"><span data-stu-id="c37ba-114">Linux</span></span>
  * <span data-ttu-id="c37ba-115">MacOS</span><span class="sxs-lookup"><span data-stu-id="c37ba-115">macOS</span></span>
  
* <span data-ttu-id="c37ba-116">IIS ile Windows uygulama çalışıyorsa:</span><span class="sxs-lookup"><span data-stu-id="c37ba-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="c37ba-117">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c37ba-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="c37ba-118">IIS 8 / 8 IIS Express</span><span class="sxs-lookup"><span data-stu-id="c37ba-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="c37ba-119">WebSockets etkinleştirilmelidir (bkz [IIS/IIS Express Destek](#iisiis-express-support) bölümüne.).</span><span class="sxs-lookup"><span data-stu-id="c37ba-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="c37ba-120">Uygulama çalışıyorsa [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="c37ba-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="c37ba-121">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="c37ba-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="c37ba-122">Desteklenen tarayıcılar için bkz: https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="c37ba-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="c37ba-123">Ne zaman WebSockets kullanılır?</span><span class="sxs-lookup"><span data-stu-id="c37ba-123">When to use WebSockets</span></span>

<span data-ttu-id="c37ba-124">WebSockets, doğrudan bir yuva bağlantı ile çalışması için kullanın.</span><span class="sxs-lookup"><span data-stu-id="c37ba-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="c37ba-125">Örneğin, gerçek zamanlı oyun ile mümkün olan en iyi performansı için WebSockets kullanın.</span><span class="sxs-lookup"><span data-stu-id="c37ba-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="c37ba-126">[ASP.NET Core SignalR](xref:signalr/introduction) , uygulamalara gerçek zamanlı web işlevselliği ekleme basitleştiren bir kitaplık.</span><span class="sxs-lookup"><span data-stu-id="c37ba-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="c37ba-127">Mümkün olduğunda WebSockets kullanır.</span><span class="sxs-lookup"><span data-stu-id="c37ba-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="c37ba-128">WebSockets kullanma</span><span class="sxs-lookup"><span data-stu-id="c37ba-128">How to use WebSockets</span></span>

* <span data-ttu-id="c37ba-129">Yükleme [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paket.</span><span class="sxs-lookup"><span data-stu-id="c37ba-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="c37ba-130">Ara yazılımını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="c37ba-130">Configure the middleware.</span></span>
* <span data-ttu-id="c37ba-131">WebSocket isteklerini kabul etmek.</span><span class="sxs-lookup"><span data-stu-id="c37ba-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="c37ba-132">İletileri gönderip yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="c37ba-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="c37ba-133">Ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="c37ba-133">Configure the middleware</span></span>

<span data-ttu-id="c37ba-134">WebSockets Ara yazılımında ekleme `Configure` yöntemi `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c37ba-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

<span data-ttu-id="c37ba-135">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="c37ba-135">The following settings can be configured:</span></span>

* <span data-ttu-id="c37ba-136">`KeepAliveInterval` -Nasıl "ping" çerçeveler proxy'leri emin olmak için istemci bağlantıyı açık tutma genellikle gönderilecek.</span><span class="sxs-lookup"><span data-stu-id="c37ba-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="c37ba-137">`ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu.</span><span class="sxs-lookup"><span data-stu-id="c37ba-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="c37ba-138">Gelişmiş kullanıcılar, bu verilerin boyutunu temel alan performans ayarı için değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="c37ba-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="c37ba-139">WebSocket isteklerini kabul etmek</span><span class="sxs-lookup"><span data-stu-id="c37ba-139">Accept WebSocket requests</span></span>

<span data-ttu-id="c37ba-140">İsteği yaşam döngüsünün sonraki yere (daha sonra `Configure` yöntemi veya örneğin bir MVC eylemi) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="c37ba-140">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="c37ba-141">Aşağıdaki örnek daha sonra buna dandır `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c37ba-141">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="c37ba-142">Web yuvası isteğini herhangi bir URL gelebilir, ancak bu örnek kod, yalnızca isteklerini kabul eder `/ws`.</span><span class="sxs-lookup"><span data-stu-id="c37ba-142">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="c37ba-143">İleti gönderin ve alın</span><span class="sxs-lookup"><span data-stu-id="c37ba-143">Send and receive messages</span></span>

<span data-ttu-id="c37ba-144">`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantısı yükseltir ve sağlayan bir [WebSocket](/dotnet/core/api/system.net.websockets.websocket) nesne.</span><span class="sxs-lookup"><span data-stu-id="c37ba-144">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="c37ba-145">Kullanım `WebSocket` ileti göndermek ve almak için nesne.</span><span class="sxs-lookup"><span data-stu-id="c37ba-145">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="c37ba-146">Web yuvası isteğini kabul eden daha önce gösterilen kod geçirir `WebSocket` nesnesini bir `Echo` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c37ba-146">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="c37ba-147">Kod, bir ileti alır ve hemen aynı iletiyi gönderir.</span><span class="sxs-lookup"><span data-stu-id="c37ba-147">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="c37ba-148">Gönderilen ve istemci bağlantı kapatana kadar bir döngüde alınan ileti:</span><span class="sxs-lookup"><span data-stu-id="c37ba-148">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="c37ba-149">Döngü başlamadan önce WebSocket bağlantısı kabul ederek, ara yazılım ardışık düzenini sona erer.</span><span class="sxs-lookup"><span data-stu-id="c37ba-149">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="c37ba-150">Yuva kapatıldıktan sonra işlem hattını geriye doğru izler.</span><span class="sxs-lookup"><span data-stu-id="c37ba-150">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="c37ba-151">Diğer bir deyişle, işlem hattı, WebSocket kabul edildiğinde ilerletme isteğini durdurur.</span><span class="sxs-lookup"><span data-stu-id="c37ba-151">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="c37ba-152">Döngü tamamlandıktan ve yuva kapalı olduğunda, isteği yeniden işlem hattı devam eder.</span><span class="sxs-lookup"><span data-stu-id="c37ba-152">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="c37ba-153">IIS/IIS Express desteği</span><span class="sxs-lookup"><span data-stu-id="c37ba-153">IIS/IIS Express support</span></span>

<span data-ttu-id="c37ba-154">Windows Server 2012 veya üzeri ve Windows 8 veya üzeri ile IIS/IIS Express 8 veya üzeri, WebSocket protokolü için desteği vardır.</span><span class="sxs-lookup"><span data-stu-id="c37ba-154">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="c37ba-155">WebSockets, IIS Express kullanırken her zaman etkindir.</span><span class="sxs-lookup"><span data-stu-id="c37ba-155">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="c37ba-156">IIS WebSockets etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="c37ba-156">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="c37ba-157">Windows Server 2012 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="c37ba-157">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="c37ba-158">Bu adımları IIS Express kullanırken gerekli değildir</span><span class="sxs-lookup"><span data-stu-id="c37ba-158">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="c37ba-159">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-159">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="c37ba-160">Seçin **rol tabanlı veya özellik tabanlı yükleme**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-160">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="c37ba-161">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-161">Select **Next**.</span></span>
1. <span data-ttu-id="c37ba-162">(Yerel sunucu varsayılan olarak seçilidir) uygun sunucuyu seçin.</span><span class="sxs-lookup"><span data-stu-id="c37ba-162">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="c37ba-163">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-163">Select **Next**.</span></span>
1. <span data-ttu-id="c37ba-164">Genişletin **Web sunucusu (IIS)** içinde **rolleri** ağacında, genişletme **Web sunucusu**ve ardından **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-164">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="c37ba-165">Seçin **WebSocket Protokolü**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-165">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="c37ba-166">Seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-166">Select **Next**.</span></span>
1. <span data-ttu-id="c37ba-167">Ek özelliklere ihtiyaç duyulmayan, seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-167">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="c37ba-168">**Yükle**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="c37ba-168">Select **Install**.</span></span>
1. <span data-ttu-id="c37ba-169">Yükleme tamamlandığında seçin **Kapat** sihirbazdan çıkmak için.</span><span class="sxs-lookup"><span data-stu-id="c37ba-169">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="c37ba-170">Windows 8 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="c37ba-170">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="c37ba-171">Bu adımları IIS Express kullanırken gerekli değildir</span><span class="sxs-lookup"><span data-stu-id="c37ba-171">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="c37ba-172">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="c37ba-172">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="c37ba-173">Aşağıdaki düğümler açın: **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-173">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="c37ba-174">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="c37ba-174">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="c37ba-175">Seçin **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-175">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="c37ba-176">Socket.io, Node.js dosyasını kullanırken WebSocket devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="c37ba-176">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="c37ba-177">WebSocket desteği kullanıyorsanız [socket.io](https://socket.io/) üzerinde [Node.js](https://nodejs.org/), varsayılan IIS WebSocket modülünü kullanarak devre dışı bırak `webSocket` öğesinde *web.config* veya *applicationHost.config*. Bu adım yapılamaz ve iletişim WebSocket yerine Node.js uygulaması işlemek IIS WebSocket modülü çalışır.</span><span class="sxs-lookup"><span data-stu-id="c37ba-177">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="c37ba-178">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="c37ba-178">Next steps</span></span>

<span data-ttu-id="c37ba-179">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) bu eşlik Yankı uygulama makaledir.</span><span class="sxs-lookup"><span data-stu-id="c37ba-179">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="c37ba-180">WebSocket bağlantılarını sağlayan bir web sayfasına sahip ve sunucu istemciye herhangi bir ileti aldıktan sonra yeniden gönderir.</span><span class="sxs-lookup"><span data-stu-id="c37ba-180">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="c37ba-181">(Bunu ayarlanmadı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden uygulamayı çalıştırın ve gidin http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="c37ba-181">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="c37ba-182">Web sayfasının sol üst bağlantı durumunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="c37ba-182">The web page shows the connection status in the upper left:</span></span>

![Web sayfasının ilk durumu](websockets/_static/start.png)

<span data-ttu-id="c37ba-184">Seçin **Connect** gösterilen URL'yi bir Web yuvası isteğini göndermek için.</span><span class="sxs-lookup"><span data-stu-id="c37ba-184">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="c37ba-185">Test iletisi girin ve seçin **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-185">Enter a test message and select **Send**.</span></span> <span data-ttu-id="c37ba-186">İşiniz bittiğinde, seçin **Kapat yuva**.</span><span class="sxs-lookup"><span data-stu-id="c37ba-186">When done, select **Close Socket**.</span></span> <span data-ttu-id="c37ba-187">**İletişim günlük** bölüm açık, her gönderme bildirir ve kapatma eylemi,'olmuyor.</span><span class="sxs-lookup"><span data-stu-id="c37ba-187">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web sayfasının ilk durumu](websockets/_static/end.png)
