---
uid: signalr/overview/performance/signalr-performance
title: SignalR performansı | Microsoft Docs
author: pfletcher
description: SignalR performansı
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bd44d77ae6166c58addbd5817977fdc26d44c024
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839521"
---
<a name="signalr-performance"></a><span data-ttu-id="5f664-103">SignalR performansı</span><span class="sxs-lookup"><span data-stu-id="5f664-103">SignalR Performance</span></span>
====================
<span data-ttu-id="5f664-104">tarafından [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="5f664-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="5f664-105">Bu konuda, SignalR uygulama performansını artırmak için tasarlayın ve ölçmek açıklar.</span><span class="sxs-lookup"><span data-stu-id="5f664-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="5f664-106">Bu konu başlığında kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="5f664-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="5f664-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5f664-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="5f664-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5f664-108">.NET 4.5</span></span>
> - <span data-ttu-id="5f664-109">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="5f664-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="5f664-110">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="5f664-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="5f664-111">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="5f664-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="5f664-112">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="5f664-112">Questions and comments</span></span>
> 
> <span data-ttu-id="5f664-113">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="5f664-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="5f664-114">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="5f664-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="5f664-115">SignalR performans ve ölçeklendirme hakkında son sunumu için bkz [ASP.NET SignalR ile gerçek zamanlı Web ölçeklendirme](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="5f664-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="5f664-116">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="5f664-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="5f664-117">Tasarım konuları</span><span class="sxs-lookup"><span data-stu-id="5f664-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="5f664-118">SignalR sunucunuz için performans ayarlama</span><span class="sxs-lookup"><span data-stu-id="5f664-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="5f664-119">Performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="5f664-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="5f664-120">SignalR performans sayaçları kullanma</span><span class="sxs-lookup"><span data-stu-id="5f664-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="5f664-121">Diğer performans sayaçları kullanma</span><span class="sxs-lookup"><span data-stu-id="5f664-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="5f664-122">Diğer kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5f664-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="5f664-123">Tasarım konuları</span><span class="sxs-lookup"><span data-stu-id="5f664-123">Design considerations</span></span>

<span data-ttu-id="5f664-124">Bu bölümde performans gereksiz ağ trafiğini oluşturarak engelliyordu değil emin olmak için bir SignalR uygulamasının Tasarım sırasında uygulanan desenler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5f664-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="5f664-125">İleti sıklığı azaltma</span><span class="sxs-lookup"><span data-stu-id="5f664-125">Throttling message frequency</span></span>

<span data-ttu-id="5f664-126">(Örneğin, gerçek zamanlı oyun uygulaması) bir yüksek sıklıkta ileti gönderen bile bir uygulamada, çoğu uygulama, ikinci bir fazla sayıda ileti göndermek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="5f664-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="5f664-127">Her istemci ürettiği trafik miktarını azaltmak için bir ileti döngüsü kuyruklar ve göndereceğini sık en fazla sabit bir ücrete iletileri uygulanabilir (süre içerisinde iletileri ise diğer bir deyişle, belirli sayıda ileti kadar her saniye gönderilir gönderilecek oluşturma aralığı).</span><span class="sxs-lookup"><span data-stu-id="5f664-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="5f664-128">İletiler (hem istemci ve sunucu) için belirli bir fiyatı kısıtlar örnek bir uygulama için bkz. [SignalR ile yüksek sıklıkta gerçek zamanlı](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="5f664-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="5f664-129">İleti boyutu azaltma</span><span class="sxs-lookup"><span data-stu-id="5f664-129">Reducing message size</span></span>

<span data-ttu-id="5f664-130">SignalR ileti boyutu, serileştirilmiş nesneler boyutunu azaltarak azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="5f664-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="5f664-131">Aktarılması gerekmez özellikleri içeren bir nesne gönderiyorsanız, sunucu kodunda bu özellikleri kullanarak serileştirilen öğesinden önlemek `JsonIgnore` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5f664-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="5f664-132">Özellik adlarını da iletide depolanır; Özellik adlarını kullanarak kısalttık `JsonProperty` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5f664-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="5f664-133">Aşağıdaki kod örneği, nasıl istemciye gönderilen bir özelliği dışarıda ve özellik adlarını kısaltmak nasıl gösterir:</span><span class="sxs-lookup"><span data-stu-id="5f664-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="5f664-134">**İstemciye gönderilen veri dışlanacak JsonIgnore öznitelik ve ileti boyutunu küçültmek için Item özniteliğini gösterir .NET sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="5f664-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="5f664-135">Okunabilirliğini korumak için / bakım istemci kodu içinde kısaltılmış özellik adları olabilir İnsan dostu uri'lerini adları ileti alındıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="5f664-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="5f664-136">Aşağıdaki kod örneği bir ileti anlaşması (eşleme) tanımlayarak uzun olanlara, kısaltılmış adı yeniden eşleme, olası bir yol gösterir ve kullanarak `reMap` işlevi en iyi duruma getirilmiş ileti sınıfı için sözleşme uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="5f664-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="5f664-137">**Özellik adlarının okunabilir adlarına yeniden eşleyen bir istemci tarafı JavaScript kodu kısalttık**</span><span class="sxs-lookup"><span data-stu-id="5f664-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="5f664-138">Adları iletilerinde istemci aynı zamanda sunucu için aynı yöntemi kullanarak kısaltılması.</span><span class="sxs-lookup"><span data-stu-id="5f664-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="5f664-139">(Diğer bir deyişle, ileti için kullanılan bellek miktarı) bellek kullanım alanını azaltmaya ileti nesne performansını iyileştirebilir.</span><span class="sxs-lookup"><span data-stu-id="5f664-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="5f664-140">Örneğin, tam aralığının bir `int` gerekli değildir, bir `short` veya `byte` bunun yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f664-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="5f664-141">İletileri ileti veri yolunda iletileri boyutunu azaltma, sunucu belleği depolandığından sunucu bellek sorunlarını gidermek de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f664-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="5f664-142">SignalR sunucunuz için performans ayarlama</span><span class="sxs-lookup"><span data-stu-id="5f664-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="5f664-143">Aşağıdaki yapılandırma ayarları, SignalR uygulamada daha iyi performans için sunucunuzu ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f664-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="5f664-144">Bir ASP.NET uygulamasında performansını artırma hakkında genel bilgi için bkz. [ASP.NET performans geliştirme](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f664-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="5f664-145">**SignalR yapılandırma ayarları**</span><span class="sxs-lookup"><span data-stu-id="5f664-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="5f664-146">**DefaultMessageBufferSize**: varsayılan olarak, SignalR hub'ı bağlantı başına başına bellek 1000 iletilerinde korur.</span><span class="sxs-lookup"><span data-stu-id="5f664-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="5f664-147">Büyük iletileri kullanılıyorsa, bu, bu değer azaltarak alleviated bellek sorunlarını oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="5f664-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="5f664-148">Bu ayar ayarlanabilir `Application_Start` olay işleyicisi bir ASP.NET uygulaması ya da `Configuration` yöntemi OWIN başlangıç sınıfı şirket içinde barındırılan bir uygulamada.</span><span class="sxs-lookup"><span data-stu-id="5f664-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="5f664-149">Aşağıdaki örnek, kullanılan sunucu bellek miktarını azaltmak için uygulamanızın bellek ayak izini azaltmak için bu değeri azaltmanız gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="5f664-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="5f664-150">**Varsayılan ileti arabellek boyutunu azaltmak için .NET sunucu kodu Startup.cs dosyasındaki**</span><span class="sxs-lookup"><span data-stu-id="5f664-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="5f664-151">**IIS yapılandırma ayarları**</span><span class="sxs-lookup"><span data-stu-id="5f664-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="5f664-152">**Uygulama başına en fazla eş zamanlı istek**: eş zamanlı IIS sayısının artırılması istekleri isteklerine hizmet için kullanılabilir sunucu kaynaklarına artacaktır.</span><span class="sxs-lookup"><span data-stu-id="5f664-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="5f664-153">Varsayılan değer 5000'dir; Bu ayar artırmak için yükseltilmiş bir komut isteminde aşağıdaki komutları yürütün:</span><span class="sxs-lookup"><span data-stu-id="5f664-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="5f664-154">**ApplicationPool QueueLength**: http.sys'nin uygulama havuzu için sıraya istekleri sayısının budur.</span><span class="sxs-lookup"><span data-stu-id="5f664-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="5f664-155">Kuyruk dolduğunda yeni istekler 503 "Hizmet kullanılamıyor" yanıtı alırsınız.</span><span class="sxs-lookup"><span data-stu-id="5f664-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="5f664-156">Varsayılan değer 1000'dir.</span><span class="sxs-lookup"><span data-stu-id="5f664-156">The default value is 1000.</span></span>

    <span data-ttu-id="5f664-157">Kuyruk uzunluğu uygulamanızı barındıran uygulama havuzunda çalışan işleminin kısaltmayı, bellek kaynaklarının tasarrufu.</span><span class="sxs-lookup"><span data-stu-id="5f664-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="5f664-158">Daha fazla bilgi için [yönetme, ayarlama ve uygulama havuzlarını yapılandırma](https://technet.microsoft.com/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f664-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/library/cc745955.aspx).</span></span>

<span data-ttu-id="5f664-159">**ASP.NET yapılandırma ayarları**</span><span class="sxs-lookup"><span data-stu-id="5f664-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="5f664-160">Bu bölümü, ayarlanabilen yapılandırma ayarlarını içerir `aspnet.config` dosya.</span><span class="sxs-lookup"><span data-stu-id="5f664-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="5f664-161">Bu dosya, platforma bağlı olarak iki konumlardan birinde bulunur:</span><span class="sxs-lookup"><span data-stu-id="5f664-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="5f664-162">SignalR performansı iyileştirebilir ASP.NET ayarları aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="5f664-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="5f664-163">**CPU başına en fazla eş zamanlı istek**: Bu ayar artan performans sorunları hafifletmek.</span><span class="sxs-lookup"><span data-stu-id="5f664-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="5f664-164">Bu ayar artırmak için aşağıdaki yapılandırma ayarı ekleme `aspnet.config` dosyası:</span><span class="sxs-lookup"><span data-stu-id="5f664-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="5f664-165">**Sıra sınırı iste**: toplam bağlantı sayısını aştığında `maxConcurrentRequestsPerCPU` ayarlama, ASP.NET bir kuyruk kullanma istekleri azaltma başlar.</span><span class="sxs-lookup"><span data-stu-id="5f664-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="5f664-166">Sıranın boyutunu artırmak için artırabilirsiniz `requestQueueLimit` ayarı.</span><span class="sxs-lookup"><span data-stu-id="5f664-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="5f664-167">Bunu yapmak için aşağıdaki yapılandırma ayarı ekleme `processModel` düğümünde `config/machine.config` (yerine `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="5f664-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="5f664-168">Performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="5f664-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="5f664-169">Bu bölümde, uygulamanızda performans sorunlarını bulmak için yolları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5f664-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="5f664-170">WebSocket kullanılmakta olduğunu doğrulama</span><span class="sxs-lookup"><span data-stu-id="5f664-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="5f664-171">SignalR istemci ve sunucu arasındaki iletişim için taşımalar çeşitli kullanabilirsiniz, ancak WebSocket önemli performans avantajı sunar ve istemci ve sunucu destekliyorsa kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5f664-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="5f664-172">İstemci ve sunucu WebSocket gereksinimlerini karşılayıp karşılamadığını belirlemek için bkz: [aktarım ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5f664-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="5f664-173">Hangi aktarım uygulamanızda kullanıldığını belirlemek için tarayıcı geliştirici araçlarını kullanın ve aktarım bağlantı için kullanıldığını görmek için günlüklerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="5f664-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="5f664-174">Internet Explorer ve Chrome tarayıcı geliştirme araçları kullanma hakkında daha fazla bilgi için bkz. [aktarım ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="5f664-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="5f664-175">SignalR performans sayaçları kullanma</span><span class="sxs-lookup"><span data-stu-id="5f664-175">Using SignalR performance counters</span></span>

<span data-ttu-id="5f664-176">Bu bölümde, etkinleştirmek ve kullanmak SignalR performans sayaçları açıklanmaktadır bulunan `Microsoft.AspNet.SignalR.Utils` paket.</span><span class="sxs-lookup"><span data-stu-id="5f664-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="5f664-177">Signalr.exe yükleme</span><span class="sxs-lookup"><span data-stu-id="5f664-177">Installing signalr.exe</span></span>

<span data-ttu-id="5f664-178">Performans sayaçları SignalR.exe adlı bir yardımcı program kullanarak sunucuya eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="5f664-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="5f664-179">Bu yardımcı programını yüklemek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="5f664-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="5f664-180">Visual Studio uygulamanızı seçin **Araçları**, **kitaplık Paket Yöneticisi**, **çözüm için NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="5f664-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="5f664-181">Arama **signalr.utils**ve yükleme seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="5f664-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="5f664-182">Paketi yüklemek için lisans sözleşmesini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="5f664-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="5f664-183">SignalR.exe için yüklenecek `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="5f664-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="5f664-184">Performans sayaçları ile SignalR.exe yükleniyor</span><span class="sxs-lookup"><span data-stu-id="5f664-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="5f664-185">SignalR performans sayaçlarını yüklemek için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="5f664-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="5f664-186">SignalR performans sayaçları kaldırmak için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="5f664-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="5f664-187">SignalR performans sayaçları</span><span class="sxs-lookup"><span data-stu-id="5f664-187">SignalR Performance counters</span></span>

<span data-ttu-id="5f664-188">Aşağıdaki performans sayaçları yardımcı programları paketi yükler.</span><span class="sxs-lookup"><span data-stu-id="5f664-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="5f664-189">Son uygulama havuzunu veya sunucu yeniden başlatıldığından beri "Toplam" sayaçları, olay sayısını ölçer.</span><span class="sxs-lookup"><span data-stu-id="5f664-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="5f664-190">**Bağlantı ölçümü**</span><span class="sxs-lookup"><span data-stu-id="5f664-190">**Connection metrics**</span></span>

<span data-ttu-id="5f664-191">Aşağıdaki ölçümler gerçekleşen bağlantı ömrü olaylarını ölçün.</span><span class="sxs-lookup"><span data-stu-id="5f664-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="5f664-192">Daha fazla bilgi için [anlama ve işleme bağlantı ömrü olaylarını](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="5f664-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="5f664-193">**Bağlı bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="5f664-193">**Connections Connected**</span></span>
- <span data-ttu-id="5f664-194">**Bağlantıları yeniden bağlantı kuruldu**</span><span class="sxs-lookup"><span data-stu-id="5f664-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="5f664-195">**Bağlantısı kesilen bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="5f664-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="5f664-196">**Bağlantılar-geçerli**</span><span class="sxs-lookup"><span data-stu-id="5f664-196">**Connections Current**</span></span>

<span data-ttu-id="5f664-197">**İleti ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="5f664-197">**Message metrics**</span></span>

<span data-ttu-id="5f664-198">Aşağıdaki ölçümler SignalR tarafından oluşturulan ileti trafiği ölçer.</span><span class="sxs-lookup"><span data-stu-id="5f664-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="5f664-199">**Alınan toplam bağlantı iletisi**</span><span class="sxs-lookup"><span data-stu-id="5f664-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="5f664-200">**Toplam gönderilen bağlantısı iletileri**</span><span class="sxs-lookup"><span data-stu-id="5f664-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="5f664-201">**Bağlantı alınan ileti sayısı/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="5f664-202">**Bağlantı gönderilen ileti sayısı/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="5f664-203">**Message bus ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="5f664-203">**Message bus metrics**</span></span>

<span data-ttu-id="5f664-204">Aşağıdaki ölçümler, iç SignalR ileti veri, tüm gelen ve giden SignalR iletileri yerleştirilir kuyruk üzerinden geçen trafik ölçün.</span><span class="sxs-lookup"><span data-stu-id="5f664-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="5f664-205">Bir ileti **yayımlanan** zaman bunu gönderilir ya da yayın.</span><span class="sxs-lookup"><span data-stu-id="5f664-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="5f664-206">A **abone** bu bağlamda bir ileti veri yoluna abonelikte; bu istemciler ve sunucu sayısına eşit.</span><span class="sxs-lookup"><span data-stu-id="5f664-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="5f664-207">Bir **ayrılmış çalışan** veri gönderen etkin bağlantılar için; bir bileşeni bir **meşgul çalışan** , etkin bir ileti gönderen bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="5f664-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="5f664-208">**Alınan toplam ileti veri yoluna iletileri**</span><span class="sxs-lookup"><span data-stu-id="5f664-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="5f664-209">**İleti veri yoluna iletileri/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5f664-210">**Toplam ileti veri yoluna yayımlanan**</span><span class="sxs-lookup"><span data-stu-id="5f664-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="5f664-211">**İleti veri yoluna iletileri yayımlanan/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="5f664-212">**İleti veri yoluna abone geçerli**</span><span class="sxs-lookup"><span data-stu-id="5f664-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="5f664-213">**İleti veri yoluna abone toplam**</span><span class="sxs-lookup"><span data-stu-id="5f664-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="5f664-214">**İleti veri yoluna abone/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="5f664-215">**İleti veri yoluna ayrılan çalışanları**</span><span class="sxs-lookup"><span data-stu-id="5f664-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="5f664-216">**İleti veri yolu meşgul çalışanların**</span><span class="sxs-lookup"><span data-stu-id="5f664-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="5f664-217">**İleti veri yolu konuları geçerli**</span><span class="sxs-lookup"><span data-stu-id="5f664-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="5f664-218">**Hata ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="5f664-218">**Error metrics**</span></span>

<span data-ttu-id="5f664-219">SignalR ileti trafiği tarafından oluşturulan hatalar aşağıdaki ölçümleri ölçün.</span><span class="sxs-lookup"><span data-stu-id="5f664-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="5f664-220">**Hub çözümleme** hataları bir hub veya hub'yöntemini çözümlenemiyor olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="5f664-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="5f664-221">**Hub çağırma** hataları olan bir hub yöntemi çağrılırken oluşan özel durum.</span><span class="sxs-lookup"><span data-stu-id="5f664-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="5f664-222">**Aktarım** bir HTTP isteği veya yanıtı sırasında oluşturulan bağlantı hataları hatalardır.</span><span class="sxs-lookup"><span data-stu-id="5f664-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="5f664-223">**Hataları: Tüm toplam**</span><span class="sxs-lookup"><span data-stu-id="5f664-223">**Errors: All Total**</span></span>
- <span data-ttu-id="5f664-224">**Hata: All/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="5f664-225">**Hataları: Hub çözümleme toplam**</span><span class="sxs-lookup"><span data-stu-id="5f664-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="5f664-226">**Hata: Hub çözümleme/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="5f664-227">**Hataları: Hub çağırma toplam**</span><span class="sxs-lookup"><span data-stu-id="5f664-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="5f664-228">**Hata: Hub çağırma/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="5f664-229">**Hata: Aktarım toplam**</span><span class="sxs-lookup"><span data-stu-id="5f664-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="5f664-230">**Hata: Aktarım/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="5f664-231">**Genişletme ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="5f664-231">**Scaleout metrics**</span></span>

<span data-ttu-id="5f664-232">Aşağıdaki ölçümler, trafik ve genişletme sağlayıcı tarafından oluşturulan hatalar ölçün.</span><span class="sxs-lookup"><span data-stu-id="5f664-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="5f664-233">A **Stream** bu bağlamda bir ölçek birimi genişletme sağlayıcı tarafından kullanılan; SQL Server kullandıysanız bir tablo, Service Bus kullanılırsa, bir konu ve abonelik Redis kullandıysanız budur.</span><span class="sxs-lookup"><span data-stu-id="5f664-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="5f664-234">Her akış, sıralı okuma ve yazma işlemleri sağlar; tek bir akışın olası bir ölçek sorunu olduğundan, bu sorunu azaltmak için akış sayısı artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="5f664-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="5f664-235">Birden çok akış kullandıysanız, SignalR (parça) iletileri herhangi belirli bağlantısından gönderilen iletiler sırada olmalarını şekilde bu akışları arasında otomatik olarak dağıtır.</span><span class="sxs-lookup"><span data-stu-id="5f664-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="5f664-236">[MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) ayarı SignalR tarafından tutulan genişletme gönderme sırası uzunluğunu denetler.</span><span class="sxs-lookup"><span data-stu-id="5f664-236">The [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="5f664-237">Bir değer için 0 teker teker yapılandırılmış Mesajlaşma devre kartına gönderilen için bir gönderme sırasındaki tüm iletileri yerleştireceğiniz daha büyük olarak ayarlanıyor.</span><span class="sxs-lookup"><span data-stu-id="5f664-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="5f664-238">Kuyruk boyutu yapılandırılan uzunluğu aşması durumunda, göndermek için sonraki çağrılar hemen başarısız bir [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) sırasındaki ileti sayısını ayarından küçük olana kadar yeniden.</span><span class="sxs-lookup"><span data-stu-id="5f664-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="5f664-239">Uygulanan backplanes genellikle kendi queuing veya akış denetimi yerinde olduğundan çağrısı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="5f664-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="5f664-240">SQL Server'ın söz konusu olduğunda bağlantı havuzu gönderen herhangi bir zamanda geçmeden sayısını etkili bir şekilde sınırlar.</span><span class="sxs-lookup"><span data-stu-id="5f664-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="5f664-241">SQL Server ve Redis için varsayılan olarak, yalnızca bir akış kullanılır, beş akışları, Service Bus için kullanılır ve kuyruğa alma devre dışı bırakıldı, ancak bu ayarlar, SQL Server ve Service Bus üzerinde yapılandırma yoluyla değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="5f664-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="5f664-242">**SQL Server devre kartı için tablo sayısı ve kuyruk uzunluğu yapılandırmak için .NET sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="5f664-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="5f664-243">**Service Bus devre kartı için konu sayısı ve kuyruk uzunluğu yapılandırmak için .NET sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="5f664-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="5f664-244">A **arabelleği** stream, hatalı bir duruma geçtiğini; akış hatalı bir durumda olduğunda hemen akış artık hatalı kadar devre kartına gönderilen tüm iletilerin başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="5f664-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="5f664-245">**Gönderme sırası uzunluğu** gönderilen, ancak henüz gönderilen iletilerin sayısı.</span><span class="sxs-lookup"><span data-stu-id="5f664-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="5f664-246">**Genişletme ileti veri yoluna iletileri/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="5f664-247">**Genişletme akışları toplam**</span><span class="sxs-lookup"><span data-stu-id="5f664-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="5f664-248">**Genişletme açık akışları**</span><span class="sxs-lookup"><span data-stu-id="5f664-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="5f664-249">**Genişletme akışları arabelleğe alma**</span><span class="sxs-lookup"><span data-stu-id="5f664-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="5f664-250">**Genişletme hataları toplamı**</span><span class="sxs-lookup"><span data-stu-id="5f664-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="5f664-251">**Genişletme Hatası/sn**</span><span class="sxs-lookup"><span data-stu-id="5f664-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="5f664-252">**Genişletme gönderme sırası uzunluğu**</span><span class="sxs-lookup"><span data-stu-id="5f664-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="5f664-253">Ne Bu sayaçlar ölçme ile ilgili daha fazla bilgi için bkz: [Azure Service Bus ile SignalR ölçeğini genişletme](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="5f664-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="5f664-254">Diğer performans sayaçları kullanma</span><span class="sxs-lookup"><span data-stu-id="5f664-254">Using other performance counters</span></span>

<span data-ttu-id="5f664-255">Aşağıdaki performans sayaçları, uygulamanızın performansını izlemek için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5f664-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="5f664-256">**Bellek**</span><span class="sxs-lookup"><span data-stu-id="5f664-256">**Memory**</span></span>

- <span data-ttu-id="5f664-257">.NET CLR bellek\\bayt miktarı tüm yığınlardaki (w3wp)</span><span class="sxs-lookup"><span data-stu-id="5f664-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="5f664-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="5f664-258">**ASP.NET**</span></span>

- <span data-ttu-id="5f664-259">ASP.NET\Requests geçerli</span><span class="sxs-lookup"><span data-stu-id="5f664-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="5f664-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="5f664-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="5f664-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="5f664-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="5f664-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="5f664-262">**CPU**</span></span>

- <span data-ttu-id="5f664-263">İşlemci Information\Processor zamanı</span><span class="sxs-lookup"><span data-stu-id="5f664-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="5f664-264">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="5f664-264">**TCP/IP**</span></span>

- <span data-ttu-id="5f664-265">TCPv6/kurulan bağlantılar</span><span class="sxs-lookup"><span data-stu-id="5f664-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="5f664-266">TCPv4/kurulan bağlantılar</span><span class="sxs-lookup"><span data-stu-id="5f664-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="5f664-267">**Web hizmeti**</span><span class="sxs-lookup"><span data-stu-id="5f664-267">**Web Service**</span></span>

- <span data-ttu-id="5f664-268">Web Service\Current bağlantıları</span><span class="sxs-lookup"><span data-stu-id="5f664-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="5f664-269">Web Service\Maximum bağlantıları</span><span class="sxs-lookup"><span data-stu-id="5f664-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="5f664-270">**İş parçacığı oluşturma**</span><span class="sxs-lookup"><span data-stu-id="5f664-270">**Threading**</span></span>

- <span data-ttu-id="5f664-271">.NET CLR kilitler ve iş parçacığı\\geçerli mantıksal iş parçacığı sayısı</span><span class="sxs-lookup"><span data-stu-id="5f664-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="5f664-272">.NET CLR kilitler ve iş parçacığı\\geçerli fiziksel iş parçacığı sayısı</span><span class="sxs-lookup"><span data-stu-id="5f664-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="5f664-273">Diğer Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5f664-273">Other Resources</span></span>

<span data-ttu-id="5f664-274">ASP.NET Performans izleme ve ayarlama hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="5f664-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="5f664-275">[ASP.NET performansa genel bakış](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="5f664-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="5f664-276">IIS 6.0 IIS 7.5 ve IIS 7.0 üzerinde ASP.NET iş parçacığı kullanımı</span><span class="sxs-lookup"><span data-stu-id="5f664-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="5f664-277">&lt;applicationPool&gt; öğesi (Web Ayarları)</span><span class="sxs-lookup"><span data-stu-id="5f664-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
