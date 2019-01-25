---
title: ASP.NET Core WebSockets desteği
author: rick-anderson
description: ASP.NET core'da WebSockets kullanmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/17/2019
uid: fundamentals/websockets
ms.openlocfilehash: 76acb9c96ed5e8bbbaf39eeb6cb23307bb44fb8d
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836864"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="949fa-103">ASP.NET Core WebSockets desteği</span><span class="sxs-lookup"><span data-stu-id="949fa-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="949fa-104">Tarafından [Tom Dykstra](https://github.com/tdykstra) ve [Andrew Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="949fa-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="949fa-105">Bu makalede, WebSockets içinde ASP.NET Core ile çalışmaya başlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="949fa-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="949fa-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) üzerinden TCP bağlantıları kalıcı iki yönlü iletişim kanalı sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="949fa-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="949fa-107">Sohbet, Pano ve oyun uygulamaları gibi hızlı, gerçek zamanlı iletişim yararlanan uygulamalarda kullanılır.</span><span class="sxs-lookup"><span data-stu-id="949fa-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="949fa-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="949fa-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="949fa-109">Bkz: [sonraki adımlar](#next-steps) bölümünde daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="949fa-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="949fa-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="949fa-110">Prerequisites</span></span>

* <span data-ttu-id="949fa-111">ASP.NET Core 1.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="949fa-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="949fa-112">ASP.NET Core destekleyen tüm işletim Sistemleri:</span><span class="sxs-lookup"><span data-stu-id="949fa-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="949fa-113">Windows 7 / Windows Server 2008 veya üstü</span><span class="sxs-lookup"><span data-stu-id="949fa-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="949fa-114">Linux</span><span class="sxs-lookup"><span data-stu-id="949fa-114">Linux</span></span>
  * <span data-ttu-id="949fa-115">macOS</span><span class="sxs-lookup"><span data-stu-id="949fa-115">macOS</span></span>
  
* <span data-ttu-id="949fa-116">IIS ile Windows uygulama çalışıyorsa:</span><span class="sxs-lookup"><span data-stu-id="949fa-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="949fa-117">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="949fa-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="949fa-118">IIS 8 / 8 IIS Express</span><span class="sxs-lookup"><span data-stu-id="949fa-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="949fa-119">WebSockets etkinleştirilmelidir (bkz [IIS/IIS Express Destek](#iisiis-express-support) bölümüne.).</span><span class="sxs-lookup"><span data-stu-id="949fa-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="949fa-120">Uygulama çalışıyorsa [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="949fa-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="949fa-121">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="949fa-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="949fa-122">Desteklenen tarayıcılar için bkz: https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="949fa-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="949fa-123">Ne zaman WebSockets kullanılır?</span><span class="sxs-lookup"><span data-stu-id="949fa-123">When to use WebSockets</span></span>

<span data-ttu-id="949fa-124">WebSockets, doğrudan bir yuva bağlantı ile çalışması için kullanın.</span><span class="sxs-lookup"><span data-stu-id="949fa-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="949fa-125">Örneğin, gerçek zamanlı oyun ile mümkün olan en iyi performansı için WebSockets kullanın.</span><span class="sxs-lookup"><span data-stu-id="949fa-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="949fa-126">[ASP.NET Core SignalR](xref:signalr/introduction) , uygulamalara gerçek zamanlı web işlevselliği ekleme basitleştiren bir kitaplık.</span><span class="sxs-lookup"><span data-stu-id="949fa-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="949fa-127">Mümkün olduğunda WebSockets kullanır.</span><span class="sxs-lookup"><span data-stu-id="949fa-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="949fa-128">WebSockets kullanma</span><span class="sxs-lookup"><span data-stu-id="949fa-128">How to use WebSockets</span></span>

* <span data-ttu-id="949fa-129">Yükleme [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paket.</span><span class="sxs-lookup"><span data-stu-id="949fa-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="949fa-130">Ara yazılımını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="949fa-130">Configure the middleware.</span></span>
* <span data-ttu-id="949fa-131">WebSocket isteklerini kabul etmek.</span><span class="sxs-lookup"><span data-stu-id="949fa-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="949fa-132">İletileri gönderip yeniden açın.</span><span class="sxs-lookup"><span data-stu-id="949fa-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="949fa-133">Ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="949fa-133">Configure the middleware</span></span>

<span data-ttu-id="949fa-134">WebSockets Ara yazılımında ekleme `Configure` yöntemi `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="949fa-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="949fa-135">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="949fa-135">The following settings can be configured:</span></span>

* <span data-ttu-id="949fa-136">`KeepAliveInterval` -Nasıl "ping" çerçeveler proxy'leri emin olmak için istemci bağlantıyı açık tutma genellikle gönderilecek.</span><span class="sxs-lookup"><span data-stu-id="949fa-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="949fa-137">İki dakika varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="949fa-137">The default is two minutes.</span></span>
* <span data-ttu-id="949fa-138">`ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu.</span><span class="sxs-lookup"><span data-stu-id="949fa-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="949fa-139">Gelişmiş kullanıcılar, bu verilerin boyutunu temel alan performans ayarı için değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="949fa-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="949fa-140">Varsayılan değer 4 KB'dir.</span><span class="sxs-lookup"><span data-stu-id="949fa-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="949fa-141">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="949fa-141">The following settings can be configured:</span></span>

* <span data-ttu-id="949fa-142">`KeepAliveInterval` -Nasıl "ping" çerçeveler proxy'leri emin olmak için istemci bağlantıyı açık tutma genellikle gönderilecek.</span><span class="sxs-lookup"><span data-stu-id="949fa-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="949fa-143">İki dakika varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="949fa-143">The default is two minutes.</span></span>
* <span data-ttu-id="949fa-144">`ReceiveBufferSize` -Veri almak için kullanılan arabellek boyutu.</span><span class="sxs-lookup"><span data-stu-id="949fa-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="949fa-145">Gelişmiş kullanıcılar, bu verilerin boyutunu temel alan performans ayarı için değiştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="949fa-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="949fa-146">Varsayılan değer 4 KB'dir.</span><span class="sxs-lookup"><span data-stu-id="949fa-146">The default is 4 KB.</span></span>
* <span data-ttu-id="949fa-147">`AllowedOrigins` -WebSocket istekleri için izin verilen çıkış noktaları üstbilgi değerleri listesi.</span><span class="sxs-lookup"><span data-stu-id="949fa-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="949fa-148">Varsayılan olarak, tüm çıkış noktaları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="949fa-148">By default, all origins are allowed.</span></span> <span data-ttu-id="949fa-149">"WebSocket kaynak kısıtlama" Ayrıntılar için aşağıya bakın.</span><span class="sxs-lookup"><span data-stu-id="949fa-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="949fa-150">WebSocket isteklerini kabul etmek</span><span class="sxs-lookup"><span data-stu-id="949fa-150">Accept WebSocket requests</span></span>

<span data-ttu-id="949fa-151">İsteği yaşam döngüsünün sonraki yere (daha sonra `Configure` yöntemi veya örneğin bir MVC eylemi) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="949fa-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="949fa-152">Aşağıdaki örnek daha sonra buna dandır `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="949fa-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="949fa-153">Web yuvası isteğini herhangi bir URL gelebilir, ancak bu örnek kod, yalnızca isteklerini kabul eder `/ws`.</span><span class="sxs-lookup"><span data-stu-id="949fa-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="949fa-154">İleti gönderin ve alın</span><span class="sxs-lookup"><span data-stu-id="949fa-154">Send and receive messages</span></span>

<span data-ttu-id="949fa-155">`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantısı yükseltir ve sağlayan bir [WebSocket](/dotnet/core/api/system.net.websockets.websocket) nesne.</span><span class="sxs-lookup"><span data-stu-id="949fa-155">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="949fa-156">Kullanım `WebSocket` ileti göndermek ve almak için nesne.</span><span class="sxs-lookup"><span data-stu-id="949fa-156">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="949fa-157">Web yuvası isteğini kabul eden daha önce gösterilen kod geçirir `WebSocket` nesnesini bir `Echo` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="949fa-157">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="949fa-158">Kod, bir ileti alır ve hemen aynı iletiyi gönderir.</span><span class="sxs-lookup"><span data-stu-id="949fa-158">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="949fa-159">Gönderilen ve istemci bağlantı kapatana kadar bir döngüde alınan ileti:</span><span class="sxs-lookup"><span data-stu-id="949fa-159">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="949fa-160">Döngü başlamadan önce WebSocket bağlantısı kabul ederek, ara yazılım ardışık düzenini sona erer.</span><span class="sxs-lookup"><span data-stu-id="949fa-160">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="949fa-161">Yuva kapatıldıktan sonra işlem hattını geriye doğru izler.</span><span class="sxs-lookup"><span data-stu-id="949fa-161">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="949fa-162">Diğer bir deyişle, işlem hattı, WebSocket kabul edildiğinde ilerletme isteğini durdurur.</span><span class="sxs-lookup"><span data-stu-id="949fa-162">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="949fa-163">Döngü tamamlandıktan ve yuva kapalı olduğunda, isteği yeniden işlem hattı devam eder.</span><span class="sxs-lookup"><span data-stu-id="949fa-163">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="handle-client-disconnects"></a><span data-ttu-id="949fa-164">Tanıtıcı istemci bağlantısını keser</span><span class="sxs-lookup"><span data-stu-id="949fa-164">Handle client disconnects</span></span>

<span data-ttu-id="949fa-165">İstemci bağlantı kaybı nedeniyle kestiğinde sunucu otomatik olarak haberdar değildir.</span><span class="sxs-lookup"><span data-stu-id="949fa-165">The server is not automatically informed when the client disconnects due to loss of connectivity.</span></span> <span data-ttu-id="949fa-166">Yalnızca istemci, internet bağlantısı kaybolursa, yapılamaz gönderirse, sunucunun bir bağlantı kesme iletisi alır.</span><span class="sxs-lookup"><span data-stu-id="949fa-166">The server receives a disconnect message only if the client sends it, which can't be done if the internet connection is lost.</span></span> <span data-ttu-id="949fa-167">Bu durum oluştuğunda bazı işlemler yapması istiyorsanız, hiçbir şey belirli bir zaman penceresi içinde bir istemciden alındıktan sonra bir zaman aşımını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="949fa-167">If you want to take some action when that happens, set a timeout after nothing is received from the client within a certain time window.</span></span>

<span data-ttu-id="949fa-168">İstemci her zaman ileti gönderme değildir ve yalnızca bağlantı boşta gittiği zaman aşımına uğramak üzere istemiyorsanız, istemci her X saniyede bir ping ileti göndermek için bir zamanlayıcı kullanmak vardır.</span><span class="sxs-lookup"><span data-stu-id="949fa-168">If the client isn't always sending messages and you don't want to timeout just because the connection goes idle, have the client use a timer to send a ping message every X seconds.</span></span> <span data-ttu-id="949fa-169">Bir ileti 2 içinde geldiyseniz henüz sunucuda\*X saniye sonra önceki bir, bağlantı ve istemci bağlantısı kesildi rapor sonlandırın.</span><span class="sxs-lookup"><span data-stu-id="949fa-169">On the server, if a message hasn't arrived within 2\*X seconds after the previous one, terminate the connection and report that the client disconnected.</span></span> <span data-ttu-id="949fa-170">Ping yapılacak tutabilir ağ gecikmeleri için ek süre bırakmak iki kez beklenen zaman aralığı için bekleyin.</span><span class="sxs-lookup"><span data-stu-id="949fa-170">Wait for twice the expected time interval to leave extra time for network delays that might hold up the ping message.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="949fa-171">WebSocket kaynak kısıtlama</span><span class="sxs-lookup"><span data-stu-id="949fa-171">WebSocket origin restriction</span></span>

<span data-ttu-id="949fa-172">CORS tarafından sağlanan korumaları WebSockets için geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="949fa-172">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="949fa-173">Tarayıcılar **değil**:</span><span class="sxs-lookup"><span data-stu-id="949fa-173">Browsers do **not**:</span></span>

* <span data-ttu-id="949fa-174">CORS uçuş öncesi istekler gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="949fa-174">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="949fa-175">Belirtilen kısıtlamalarını dikkate `Access-Control` WebSocket istekleri yaparken üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="949fa-175">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="949fa-176">Ancak, tarayıcılar gönderebilirsiniz `Origin` WebSocket istekleri gönderirken, üst bilgisi.</span><span class="sxs-lookup"><span data-stu-id="949fa-176">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="949fa-177">Uygulamalar, beklenen kaynaklardan gelen WebSockets izin verildiğinden emin olmak için bu üstbilgileri doğrulamak için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="949fa-177">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="949fa-178">Sunucunuz koyduysanız "https://server.com"ve istemci üzerindeki barındırma"https://client.com", Ekle "https://client.com" için `AllowedOrigins` WebSockets doğrulamak için listesi.</span><span class="sxs-lookup"><span data-stu-id="949fa-178">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptionsAO&highlight=6-7)]

> [!NOTE]
> <span data-ttu-id="949fa-179">`Origin` Üst bilgisi, istemcinin ve gibi denetlenir `Referer` başlık sahte.</span><span class="sxs-lookup"><span data-stu-id="949fa-179">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="949fa-180">Yapmak **değil** bir kimlik doğrulama mekanizması bu üst bilgi kullan.</span><span class="sxs-lookup"><span data-stu-id="949fa-180">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="949fa-181">IIS/IIS Express desteği</span><span class="sxs-lookup"><span data-stu-id="949fa-181">IIS/IIS Express support</span></span>

<span data-ttu-id="949fa-182">Windows Server 2012 veya üzeri ve Windows 8 veya üzeri ile IIS/IIS Express 8 veya üzeri, WebSocket protokolü için desteği vardır.</span><span class="sxs-lookup"><span data-stu-id="949fa-182">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="949fa-183">WebSockets, IIS Express kullanırken her zaman etkindir.</span><span class="sxs-lookup"><span data-stu-id="949fa-183">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="949fa-184">IIS WebSockets etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="949fa-184">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="949fa-185">Windows Server 2012 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="949fa-185">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="949fa-186">Bu adımları IIS Express kullanırken gerekli değildir</span><span class="sxs-lookup"><span data-stu-id="949fa-186">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="949fa-187">Kullanım **rol ve Özellik Ekle** Sihirbazı'ndan **Yönet** menüsü ya da bağlantı **Sunucu Yöneticisi**.</span><span class="sxs-lookup"><span data-stu-id="949fa-187">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="949fa-188">Seçin **rol tabanlı veya özellik tabanlı yükleme**.</span><span class="sxs-lookup"><span data-stu-id="949fa-188">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="949fa-189">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="949fa-189">Select **Next**.</span></span>
1. <span data-ttu-id="949fa-190">(Yerel sunucu varsayılan olarak seçilidir) uygun sunucuyu seçin.</span><span class="sxs-lookup"><span data-stu-id="949fa-190">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="949fa-191">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="949fa-191">Select **Next**.</span></span>
1. <span data-ttu-id="949fa-192">Genişletin **Web sunucusu (IIS)** içinde **rolleri** ağacında, genişletme **Web sunucusu**ve ardından **uygulama geliştirme**.</span><span class="sxs-lookup"><span data-stu-id="949fa-192">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="949fa-193">Seçin **WebSocket Protokolü**.</span><span class="sxs-lookup"><span data-stu-id="949fa-193">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="949fa-194">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="949fa-194">Select **Next**.</span></span>
1. <span data-ttu-id="949fa-195">Ek özelliklere ihtiyaç duyulmayan, seçin **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="949fa-195">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="949fa-196">**Yükle**'yi seçin.</span><span class="sxs-lookup"><span data-stu-id="949fa-196">Select **Install**.</span></span>
1. <span data-ttu-id="949fa-197">Yükleme tamamlandığında seçin **Kapat** sihirbazdan çıkmak için.</span><span class="sxs-lookup"><span data-stu-id="949fa-197">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="949fa-198">Windows 8 veya sonraki sürümlerde WebSocket Protokolü desteğini etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="949fa-198">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="949fa-199">Bu adımları IIS Express kullanırken gerekli değildir</span><span class="sxs-lookup"><span data-stu-id="949fa-199">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="949fa-200">Gidin **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özellikleri hakkında ya da kapalı** (ekranın sol).</span><span class="sxs-lookup"><span data-stu-id="949fa-200">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="949fa-201">Aşağıdaki düğümler açın: **Internet Information Services** > **World Wide Web Hizmetleri** > **uygulama geliştirme özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="949fa-201">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="949fa-202">Seçin **WebSocket Protokolü** özelliği.</span><span class="sxs-lookup"><span data-stu-id="949fa-202">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="949fa-203">**Tamam**’ı seçin.</span><span class="sxs-lookup"><span data-stu-id="949fa-203">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="949fa-204">Socket.io, Node.js dosyasını kullanırken WebSocket devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="949fa-204">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="949fa-205">WebSocket desteği kullanıyorsanız [socket.io](https://socket.io/) üzerinde [Node.js](https://nodejs.org/), varsayılan IIS WebSocket modülünü kullanarak devre dışı bırak `webSocket` öğesinde *web.config* veya *applicationHost.config*. Bu adım yapılamaz ve iletişim WebSocket yerine Node.js uygulaması işlemek IIS WebSocket modülü çalışır.</span><span class="sxs-lookup"><span data-stu-id="949fa-205">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="949fa-206">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="949fa-206">Next steps</span></span>

<span data-ttu-id="949fa-207">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) bu eşlik Yankı uygulama makaledir.</span><span class="sxs-lookup"><span data-stu-id="949fa-207">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="949fa-208">WebSocket bağlantılarını sağlayan bir web sayfasına sahip ve sunucu istemciye herhangi bir ileti aldıktan sonra yeniden gönderir.</span><span class="sxs-lookup"><span data-stu-id="949fa-208">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="949fa-209">(Bunu ayarlanmadı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden uygulamayı çalıştırın ve gidin http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="949fa-209">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="949fa-210">Web sayfasının sol üst bağlantı durumunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="949fa-210">The web page shows the connection status in the upper left:</span></span>

![Web sayfasının ilk durumu](websockets/_static/start.png)

<span data-ttu-id="949fa-212">Seçin **Connect** gösterilen URL'yi bir Web yuvası isteğini göndermek için.</span><span class="sxs-lookup"><span data-stu-id="949fa-212">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="949fa-213">Test iletisi girin ve seçin **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="949fa-213">Enter a test message and select **Send**.</span></span> <span data-ttu-id="949fa-214">İşiniz bittiğinde, seçin **Kapat yuva**.</span><span class="sxs-lookup"><span data-stu-id="949fa-214">When done, select **Close Socket**.</span></span> <span data-ttu-id="949fa-215">**İletişim günlük** bölüm açık, her gönderme bildirir ve kapatma eylemi,'olmuyor.</span><span class="sxs-lookup"><span data-stu-id="949fa-215">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web sayfasının ilk durumu](websockets/_static/end.png)
