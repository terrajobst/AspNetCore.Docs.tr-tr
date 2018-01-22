---
title: "ASP.NET Core WebSockets desteği"
author: tdykstra
description: "ASP.NET Core WebSockets kullanmaya başlayacağınızı öğrenin."
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 46c1f42b925a43df470d7491a1e259ab51ea5f50
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="fa325-103">ASP.NET Core WebSockets giriş</span><span class="sxs-lookup"><span data-stu-id="fa325-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="fa325-104">Tarafından [zel Dykstra](https://github.com/tdykstra) ve [Barış Stanton-Nurse](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="fa325-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="fa325-105">Bu makalede, ASP.NET Core WebSockets kullanmaya başlama açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fa325-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="fa325-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) kalıcı iki yönlü iletişim kanalları üzerinden TCP bağlantıları sağlayan bir protokoldür.</span><span class="sxs-lookup"><span data-stu-id="fa325-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="fa325-107">Sohbet, yürütebilmektedir gibi uygulamalar için kullanılır, herhangi bir web uygulaması gerçek zamanlı işlevselliği istersiniz.</span><span class="sxs-lookup"><span data-stu-id="fa325-107">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="fa325-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fa325-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="fa325-109">Bkz: [sonraki adımlar](#next-steps) daha fazla bilgi için bölüm.</span><span class="sxs-lookup"><span data-stu-id="fa325-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="fa325-110">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="fa325-110">Prerequisites</span></span>

* <span data-ttu-id="fa325-111">ASP.NET Core 1.1 (1.0 çalışmaz)</span><span class="sxs-lookup"><span data-stu-id="fa325-111">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="fa325-112">ASP.NET Core üzerinde çalışan herhangi bir işletim sistemi:</span><span class="sxs-lookup"><span data-stu-id="fa325-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="fa325-113">Windows 7 / Windows Server 2008 ve sonraki sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa325-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="fa325-114">Linux</span><span class="sxs-lookup"><span data-stu-id="fa325-114">Linux</span></span>
  * <span data-ttu-id="fa325-115">macOS</span><span class="sxs-lookup"><span data-stu-id="fa325-115">macOS</span></span>

* <span data-ttu-id="fa325-116">**Özel durum**: uygulamanızı IIS ile Windows üzerinde çalışan veya WebListener ile kullanmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="fa325-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="fa325-117">Windows 8 / Windows Server 2012 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="fa325-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="fa325-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="fa325-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="fa325-119">WebSocket IIS'de etkinleştirilmelidir</span><span class="sxs-lookup"><span data-stu-id="fa325-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="fa325-120">Desteklenen tarayıcılar için http://caniuse.com/#feat=websockets bakın.</span><span class="sxs-lookup"><span data-stu-id="fa325-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="fa325-121">Ne zaman kullanılmalı</span><span class="sxs-lookup"><span data-stu-id="fa325-121">When to use it</span></span>

<span data-ttu-id="fa325-122">Bir yuva bağlantısı ile doğrudan çalışmak gerektiğinde WebSockets kullanın.</span><span class="sxs-lookup"><span data-stu-id="fa325-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="fa325-123">Örneğin, olası en iyi performansı için gerçek zamanlı oyun gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="fa325-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="fa325-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) daha zengin sağlayan uygulama modeli gerçek zamanlı işlevselliği, ancak yalnızca ASP.NET üzerinde ASP.NET Core çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="fa325-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="fa325-125">SignalR Core sürümü geliştirilme aşamasındadır; ilerleme durumunu izlemek için bkz: [SignalR Core için GitHub depo](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="fa325-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="fa325-126">SignalR Core için beklemek istemiyorsanız, WebSockets doğrudan artık kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa325-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="fa325-127">Ancak SignalR, gibi sağlayacak özellikleri geliştirmek gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="fa325-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="fa325-128">Geniş bir alternatif taşıma yöntemleri için otomatik geri dönüş kullanarak tarayıcı sürümleri için destek.</span><span class="sxs-lookup"><span data-stu-id="fa325-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="fa325-129">Bir bağlantı düştüğünde otomatik yeniden bağlanma.</span><span class="sxs-lookup"><span data-stu-id="fa325-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="fa325-130">Sunucuda veya yöntemleri çağırma istemciler için destek.</span><span class="sxs-lookup"><span data-stu-id="fa325-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="fa325-131">Birden çok sunucuya ölçeklendirme desteği.</span><span class="sxs-lookup"><span data-stu-id="fa325-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="fa325-132">Nasıl kullanılacağını</span><span class="sxs-lookup"><span data-stu-id="fa325-132">How to use it</span></span>

* <span data-ttu-id="fa325-133">Yükleme [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) paket.</span><span class="sxs-lookup"><span data-stu-id="fa325-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="fa325-134">Ara yazılımını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="fa325-134">Configure the middleware.</span></span>
* <span data-ttu-id="fa325-135">WebSocket istekleri kabul edin.</span><span class="sxs-lookup"><span data-stu-id="fa325-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="fa325-136">İleti gönderme ve alma.</span><span class="sxs-lookup"><span data-stu-id="fa325-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="fa325-137">Ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fa325-137">Configure the middleware</span></span>

<span data-ttu-id="fa325-138">WebSockets Ara yazılımında eklemek `Configure` yöntemi `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="fa325-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="fa325-139">Aşağıdaki ayarlar yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="fa325-139">The following settings can be configured:</span></span>

* <span data-ttu-id="fa325-140">`KeepAliveInterval`-Sık "ping" çerçeveler proxy'leri bağlantıyı açık tutmak emin olmak için istemciye göndermek nasıl.</span><span class="sxs-lookup"><span data-stu-id="fa325-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="fa325-141">`ReceiveBufferSize`-Veri almak için kullanılan arabellek boyutu.</span><span class="sxs-lookup"><span data-stu-id="fa325-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="fa325-142">Yalnızca ileri düzey kullanıcılar, kendi veri boyutuna göre bu, performans ayarlaması için değiştirmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa325-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="fa325-143">WebSocket istekleri kabul</span><span class="sxs-lookup"><span data-stu-id="fa325-143">Accept WebSocket requests</span></span>

<span data-ttu-id="fa325-144">İstek yaşam döngüsü devamındaki herhangi bir yerde (daha sonra `Configure` yöntemi veya bir MVC eylem, örneğin) bir Web yuvası isteği olup olmadığını denetleyin ve WebSocket isteğini kabul et.</span><span class="sxs-lookup"><span data-stu-id="fa325-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="fa325-145">Bu örnek daha sonra arasındadır `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fa325-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="fa325-146">Web yuvası isteğini üzerinde herhangi bir URL adresi gelebilir, ancak bu örnek kod yalnızca isteklerini kabul `/ws`.</span><span class="sxs-lookup"><span data-stu-id="fa325-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="fa325-147">İleti gönderme ve alma</span><span class="sxs-lookup"><span data-stu-id="fa325-147">Send and receive messages</span></span>

<span data-ttu-id="fa325-148">`AcceptWebSocketAsync` Yöntemi TCP bağlantısı WebSocket bağlantı yükseltir ve verir bir [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) nesnesi.</span><span class="sxs-lookup"><span data-stu-id="fa325-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="fa325-149">İleti gönderme ve alma için WebSocket nesnesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="fa325-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="fa325-150">Web yuvası isteğini kabul eden daha önce gösterilen kod iletir `WebSocket` nesnesine bir `Echo` yöntemi; burada'nın `Echo` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fa325-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="fa325-151">Kod bir ileti alır ve hemen aynı iletiyi gönderir.</span><span class="sxs-lookup"><span data-stu-id="fa325-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="fa325-152">İstemci bağlantısı kapatana kadar bunu bir döngüde kalır.</span><span class="sxs-lookup"><span data-stu-id="fa325-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="fa325-153">Bu döngü başlamadan önce WebSocket kabul ettiğinde, ara yazılım ardışık düzenini sona erer.</span><span class="sxs-lookup"><span data-stu-id="fa325-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="fa325-154">Yuva kapatma sırasında ardışık düzen unwinds.</span><span class="sxs-lookup"><span data-stu-id="fa325-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="fa325-155">Diğer bir deyişle, bir MVC eylem, örneğin isabet zaman olduğu gibi bir WebSocket kabul ardışık düzeninde ilerleyen isteği durdurur olacaktır.</span><span class="sxs-lookup"><span data-stu-id="fa325-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="fa325-156">Ancak bu döngü bitirip yuva kapatma isteği geri ardışık düzen işlemi devam eder.</span><span class="sxs-lookup"><span data-stu-id="fa325-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa325-157">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="fa325-157">Next steps</span></span>

<span data-ttu-id="fa325-158">[Örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) bu eşlik makaledir basit Yankı uygulama.</span><span class="sxs-lookup"><span data-stu-id="fa325-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="fa325-159">WebSocket bağlantılar sağlayan bir web sayfasına sahip ve sunucu, istemciye yalnızca aldığı iletileri yeniden gönderir.</span><span class="sxs-lookup"><span data-stu-id="fa325-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="fa325-160">Çalıştırın (bunu ayarlanmamış yukarı IIS Express ile Visual Studio'dan çalıştırmak için) bir komut isteminden ve http://localhost: 5000 için gidin.</span><span class="sxs-lookup"><span data-stu-id="fa325-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="fa325-161">Web sayfasının sol üst bağlantı durumunu gösterir:</span><span class="sxs-lookup"><span data-stu-id="fa325-161">The web page shows connection status at the upper left:</span></span>

![Web sayfasının ilk durumu](websockets/_static/start.png)

<span data-ttu-id="fa325-163">Seçin **Bağlan** gösterilen URL'yi bir Web yuvası isteğini göndermek için.</span><span class="sxs-lookup"><span data-stu-id="fa325-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="fa325-164">Sınama iletisi girin ve **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="fa325-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="fa325-165">İşiniz bittiğinde, seçin **Kapat yuva**.</span><span class="sxs-lookup"><span data-stu-id="fa325-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="fa325-166">**İletişim günlük** bölüm raporları her açık, gönderme ve kapatma eylemi olarak gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="fa325-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Web sayfasının ilk durumu](websockets/_static/end.png)
