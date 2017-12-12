---
uid: signalr/overview/performance/signalr-performance
title: SignalR performans | Microsoft Docs
author: pfletcher
description: SignalR performans
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 3751f5e7-59db-4be0-a290-50abc24e5c84
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: dec2602e47fbcb838643a506a7e3feebda9d9c81
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance"></a><span data-ttu-id="601f4-103">SignalR performans</span><span class="sxs-lookup"><span data-stu-id="601f4-103">SignalR Performance</span></span>
====================
<span data-ttu-id="601f4-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="601f4-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="601f4-105">Bu konuda, bir SignalR uygulama performansını artırmak için tasarım ve ölçmek açıklar.</span><span class="sxs-lookup"><span data-stu-id="601f4-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="601f4-106">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="601f4-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="601f4-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="601f4-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="601f4-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="601f4-108">.NET 4.5</span></span>
> - <span data-ttu-id="601f4-109">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="601f4-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="601f4-110">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="601f4-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="601f4-111">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="601f4-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="601f4-112">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="601f4-112">Questions and comments</span></span>
> 
> <span data-ttu-id="601f4-113">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="601f4-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="601f4-114">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="601f4-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="601f4-115">SignalR performans ve ölçeklendirme son sunumu için bkz: [ASP.NET SignalR ile gerçek zamanlı Web ölçeklendirme](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="601f4-115">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="601f4-116">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="601f4-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="601f4-117">Tasarım konuları</span><span class="sxs-lookup"><span data-stu-id="601f4-117">Design considerations</span></span>](#design)
- [<span data-ttu-id="601f4-118">SignalR sunucunuz için performans ayarlama</span><span class="sxs-lookup"><span data-stu-id="601f4-118">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="601f4-119">Performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="601f4-119">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="601f4-120">SignalR performans sayaçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="601f4-120">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="601f4-121">Diğer performans sayaçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="601f4-121">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="601f4-122">Diğer kaynaklar</span><span class="sxs-lookup"><span data-stu-id="601f4-122">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="601f4-123">Tasarım konuları</span><span class="sxs-lookup"><span data-stu-id="601f4-123">Design considerations</span></span>

<span data-ttu-id="601f4-124">Bu bölüm performans gereksiz ağ trafiğini oluşturarak engelliyordu değil emin olmak için bir SignalR uygulamasının Tasarım sırasında uygulanan desenleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="601f4-124">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="601f4-125">İleti sıklığı azaltma</span><span class="sxs-lookup"><span data-stu-id="601f4-125">Throttling message frequency</span></span>

<span data-ttu-id="601f4-126">Yüksek yoğunlukta (örneğin, gerçek zamanlı oyun uygulaması) iletileri gönderir bile bir uygulamada, ikinci bir birkaç iletileri göndermek uygulamaların çoğu gerekmez.</span><span class="sxs-lookup"><span data-stu-id="601f4-126">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="601f4-127">Her istemci ürettiği trafik miktarını azaltmak için bir ileti döngüsü kuyruklar ve göndereceğini en sık sabit oranı çok iletileri uygulanabilir (Bu süre içinde iletileri ise diğer bir deyişle, belirli sayıda iletileri kadar her saniye gönderilir terval) gönderilmek üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="601f4-127">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="601f4-128">İletiler (hem istemci hem de sunucusundan) için belirli bir hızı kısıtlar örnek bir uygulama için bkz: [SignalR ile yüksek sıklıkta gerçek zamanlı](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="601f4-128">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="601f4-129">İleti boyutunu azaltma</span><span class="sxs-lookup"><span data-stu-id="601f4-129">Reducing message size</span></span>

<span data-ttu-id="601f4-130">Serileştirilmiş nesneler boyutunu azaltarak bir SignalR ileti boyutunu azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="601f4-130">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="601f4-131">Aktarılması gerekmez özellikleri içeren bir nesne gönderiyorsanız, sunucu kodunda bu özellikleri kullanarak serileştirilen gelen engelliyor `JsonIgnore` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="601f4-131">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="601f4-132">Özellik adlarını da iletide depolanır; Özellik adlarını kullanarak kısaltılmış `JsonProperty` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="601f4-132">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="601f4-133">Aşağıdaki kod örneği, istemciye gönderilen bir özelliği dışarıda tutması etme ve özellik adları kısaltın gösterir:</span><span class="sxs-lookup"><span data-stu-id="601f4-133">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="601f4-134">**İstemciye gönderilen veri dışlanacak JsonIgnore özniteliği ve ileti boyutunu azaltmak için Item özniteliğini gösterir .NET sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="601f4-134">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="601f4-135">Okunabilirliğini korumak amacıyla / Bakımı istemci kodu kısaltılmış özellik adları olabilir açmayla İnsan kolay için adları iletiyi aldıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="601f4-135">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="601f4-136">Aşağıdaki kod örneği, ileti sözleşmesi (eşleme) tanımlayarak uzun olanlara kısaltılmış adlarını yeniden eşleme, olası bir yol gösterir ve kullanarak `reMap` işlevi için en iyi duruma getirilmiş ileti sınıfı sözleşme uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="601f4-136">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="601f4-137">**Yeniden harita oluşturma istemci tarafı JavaScript kodu okunabilir adlarına özellik adları kısaltılmış**</span><span class="sxs-lookup"><span data-stu-id="601f4-137">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="601f4-138">Adları aynı zamanda sunucu için istemci iletilerindeki yöntemin aynısı kullanılarak kısaltılması.</span><span class="sxs-lookup"><span data-stu-id="601f4-138">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="601f4-139">(Diğer bir deyişle, ileti için kullanılan bellek miktarı) bellek kullanım alanını azaltmaya ileti nesne da performansı geliştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="601f4-139">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="601f4-140">Örneğin, tam aralığını bir `int` gerekli değildir, bir `short` veya `byte` yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="601f4-140">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="601f4-141">Ayrıca, sunucu belleği, ileti boyutunu azaltma ileti veri yolunda iletileri depolandığından sunucu bellek sorunları gidermek erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="601f4-141">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="601f4-142">SignalR sunucunuz için performans ayarlama</span><span class="sxs-lookup"><span data-stu-id="601f4-142">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="601f4-143">Aşağıdaki yapılandırma ayarları, bir SignalR uygulamasında daha iyi performans için sunucunuzu ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="601f4-143">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="601f4-144">Bir ASP.NET uygulamasında performansı hakkında genel bilgi için bkz: [ASP.NET performans geliştirme](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="601f4-144">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span></span>

<span data-ttu-id="601f4-145">**SignalR yapılandırma ayarları**</span><span class="sxs-lookup"><span data-stu-id="601f4-145">**SignalR configuration settings**</span></span>

- <span data-ttu-id="601f4-146">**DefaultMessageBufferSize**: varsayılan olarak, SignalR hub bağlantı başına başına bellek 1000 iletilerinde korur.</span><span class="sxs-lookup"><span data-stu-id="601f4-146">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="601f4-147">Büyük iletileri kullanılıyorsa, bu da bu değeri azaltarak alleviated bellek sorunlar oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="601f4-147">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="601f4-148">Bu ayar ayarlanabilir `Application_Start` olay işleyicisi bir ASP.NET uygulamasında veya içinde `Configuration` kendini barındıran bir uygulamada OWIN başlangıç sınıfı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="601f4-148">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="601f4-149">Aşağıdaki örnek, kullanılan sunucu bellek miktarını azaltmak için uygulamanızın bellek ayak izini azaltmak için bu değeri azaltmak gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="601f4-149">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="601f4-150">**Varsayılan ileti arabellek boyutunu azaltmak için haline .NET sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="601f4-150">**.NET server code in Startup.cs for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="601f4-151">**IIS yapılandırma ayarları**</span><span class="sxs-lookup"><span data-stu-id="601f4-151">**IIS configuration settings**</span></span>

- <span data-ttu-id="601f4-152">**Uygulama başına en fazla eşzamanlı istek**: eşzamanlı IIS sayısını artırmayı istekleri isteklere hizmet vermek için kullanılabilir sunucu kaynakları artacaktır.</span><span class="sxs-lookup"><span data-stu-id="601f4-152">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="601f4-153">Varsayılan değer 5000'dir; Bu ayar artırmak için yükseltilmiş bir komut isteminde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="601f4-153">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]
- <span data-ttu-id="601f4-154">**ApplicationPool QueueLength**: Bu maksimum HTTP.sys'nin uygulama havuzu için sıraya koyar isteklerinin sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="601f4-154">**ApplicationPool QueueLength**: This is the maximum number of requests that Http.sys queues for the application pool.</span></span> <span data-ttu-id="601f4-155">Sıra dolduğunda yeni istekler 503 "Hizmet kullanılamıyor" yanıtı alırsınız.</span><span class="sxs-lookup"><span data-stu-id="601f4-155">When the queue is full, new requests receive a 503 "Service Unavailable" response.</span></span> <span data-ttu-id="601f4-156">Varsayılan değer 1000'dir.</span><span class="sxs-lookup"><span data-stu-id="601f4-156">The default value is 1000.</span></span>

    <span data-ttu-id="601f4-157">Uygulamanızı barındıran uygulama havuzundaki çalışan işlem sırası uzunluğu kısaltmayı, bellek kaynaklarının tasarrufu.</span><span class="sxs-lookup"><span data-stu-id="601f4-157">Shortening the queue length for the worker process in the application pool hosting your application will conserve memory resources.</span></span> <span data-ttu-id="601f4-158">Daha fazla bilgi için bkz: [yönetme, ayarlama ve uygulama havuzlarını yapılandırma](https://technet.microsoft.com/en-us/library/cc745955.aspx).</span><span class="sxs-lookup"><span data-stu-id="601f4-158">For more information, see [Managing, Tuning, and Configuring Application Pools](https://technet.microsoft.com/en-us/library/cc745955.aspx).</span></span>

<span data-ttu-id="601f4-159">**ASP.NET yapılandırma ayarları**</span><span class="sxs-lookup"><span data-stu-id="601f4-159">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="601f4-160">Bu bölümde ayarlanabilir yapılandırma ayarlarını içeren `aspnet.config` dosya.</span><span class="sxs-lookup"><span data-stu-id="601f4-160">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="601f4-161">Bu dosya, platforma bağlı olarak iki konumlardan birinde bulunur:</span><span class="sxs-lookup"><span data-stu-id="601f4-161">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="601f4-162">SignalR performansını artırabilir ASP.NET ayarları aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="601f4-162">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="601f4-163">**CPU başına en fazla eşzamanlı istek**: Bu ayar artan performans sorunları hafifletmek.</span><span class="sxs-lookup"><span data-stu-id="601f4-163">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="601f4-164">Bu ayar artırmak için şu yapılandırma ayarı ekleme `aspnet.config` dosyası:</span><span class="sxs-lookup"><span data-stu-id="601f4-164">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="601f4-165">**İstek sırası sınırı**: toplam bağlantı sayısını aştığında `maxConcurrentRequestsPerCPU` ayarlama, ASP.NET bir sıra kullanan isteklerinin azaltma başlar.</span><span class="sxs-lookup"><span data-stu-id="601f4-165">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="601f4-166">Sıranın boyutunu artırmak için artırabilirsiniz `requestQueueLimit` ayarı.</span><span class="sxs-lookup"><span data-stu-id="601f4-166">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="601f4-167">Bunu yapmak için şu yapılandırma ayarı ekleyin `processModel` düğümünde `config/machine.config` (yerine `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="601f4-167">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="601f4-168">Performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="601f4-168">Troubleshooting performance issues</span></span>

<span data-ttu-id="601f4-169">Bu bölümde, uygulamanızda performans sorunları bulma yolları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="601f4-169">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="601f4-170">WebSocket kullanılmakta olduğunu doğrulama</span><span class="sxs-lookup"><span data-stu-id="601f4-170">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="601f4-171">SignalR istemci ve sunucu arasında iletişim için aktarımları çeşitli kullanabilirsiniz, ancak WebSocket önemli performans avantajı sunar ve istemci ve sunucu destekliyorsa kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="601f4-171">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="601f4-172">İstemci ve sunucu WebSocket gereksinimlerini karşılayıp karşılamadığını belirlemek için bkz: [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="601f4-172">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="601f4-173">Hangi aktarım uygulamanızda kullanıldığını belirlemek için tarayıcının Geliştirici Araçları'nı kullanın ve hangi aktarım bağlantı için kullanıldığını görmek için günlüklerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="601f4-173">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="601f4-174">Internet Explorer ve Chrome tarayıcı geliştirme araçları kullanma hakkında daha fazla bilgi için bkz: [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="601f4-174">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="601f4-175">SignalR performans sayaçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="601f4-175">Using SignalR performance counters</span></span>

<span data-ttu-id="601f4-176">Bu bölümde SignalR performans sayaçlarını, etkinleştirme ve nasıl kullanılacağını açıklar bulunan `Microsoft.AspNet.SignalR.Utils` paket.</span><span class="sxs-lookup"><span data-stu-id="601f4-176">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="601f4-177">Signalr.exe yükleme</span><span class="sxs-lookup"><span data-stu-id="601f4-177">Installing signalr.exe</span></span>

<span data-ttu-id="601f4-178">Performans sayaçları SignalR.exe adlı bir programı kullanarak sunucuya eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="601f4-178">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="601f4-179">Bu yardımcı programı yüklemek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="601f4-179">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="601f4-180">Visual Studio uygulamanızı seçin **Araçları**, **kitaplık Paket Yöneticisi**, **çözüm için NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="601f4-180">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="601f4-181">Arama **signalr.utils**ve yükleme seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="601f4-181">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="601f4-182">Paketi yüklemek için lisans sözleşmesini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="601f4-182">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="601f4-183">SignalR.exe için yüklenecek `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="601f4-183">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="601f4-184">Performans sayaçları ile SignalR.exe yükleniyor</span><span class="sxs-lookup"><span data-stu-id="601f4-184">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="601f4-185">SignalR performans sayaçlarını yüklemek için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="601f4-185">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="601f4-186">SignalR performans sayaçlarını kaldırmak için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="601f4-186">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="601f4-187">SignalR performans sayaçları</span><span class="sxs-lookup"><span data-stu-id="601f4-187">SignalR Performance counters</span></span>

<span data-ttu-id="601f4-188">Yardımcı programlar paketi aşağıdaki performans sayaçlarını yükler.</span><span class="sxs-lookup"><span data-stu-id="601f4-188">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="601f4-189">Son uygulama havuzu veya sunucu yeniden başlatıldığından beri "Toplam" sayaçları olayların sayısını ölçer.</span><span class="sxs-lookup"><span data-stu-id="601f4-189">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="601f4-190">**Bağlantı ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="601f4-190">**Connection metrics**</span></span>

<span data-ttu-id="601f4-191">Aşağıdaki ölçümleri ortaya bağlantı ömür olayları ölçün.</span><span class="sxs-lookup"><span data-stu-id="601f4-191">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="601f4-192">Daha fazla bilgi için bkz: [anlama ve bağlantı ömür olayları işleme](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="601f4-192">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="601f4-193">**Bağlı bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="601f4-193">**Connections Connected**</span></span>
- <span data-ttu-id="601f4-194">**Bağlantısı yeniden kurulmuş bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="601f4-194">**Connections Reconnected**</span></span>
- <span data-ttu-id="601f4-195">**Bağlantısı kesilmiş bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="601f4-195">**Connections Disconnected**</span></span>
- <span data-ttu-id="601f4-196">**Bağlantılar-geçerli**</span><span class="sxs-lookup"><span data-stu-id="601f4-196">**Connections Current**</span></span>

<span data-ttu-id="601f4-197">**İleti ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="601f4-197">**Message metrics**</span></span>

<span data-ttu-id="601f4-198">SignalR tarafından oluşturulan ileti trafiği ölçümleri.</span><span class="sxs-lookup"><span data-stu-id="601f4-198">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="601f4-199">**Toplam alınan bağlantı iletisi**</span><span class="sxs-lookup"><span data-stu-id="601f4-199">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="601f4-200">**Toplam gönderilen bağlantısı iletileri**</span><span class="sxs-lookup"><span data-stu-id="601f4-200">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="601f4-201">**Bağlantı alınan ileti sayısı/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-201">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="601f4-202">**Bağlantı gönderilen ileti sayısı/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-202">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="601f4-203">**İleti veri yolu ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="601f4-203">**Message bus metrics**</span></span>

<span data-ttu-id="601f4-204">İç SignalR ileti veri yolu, tüm gelen ve giden SignalR iletilerinin sıraya alınma sırası trafiğinin ölçümleri.</span><span class="sxs-lookup"><span data-stu-id="601f4-204">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="601f4-205">Bir ileti **yayımlanan** zaman, gönderilen veya yayın.</span><span class="sxs-lookup"><span data-stu-id="601f4-205">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="601f4-206">A **abone** bu bağlamda bir ileti veri yoluna abonelikte; bu istemciler ve sunucu sayısına eşit.</span><span class="sxs-lookup"><span data-stu-id="601f4-206">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="601f4-207">Bir **ayrılmış çalışan** , etkin bağlantıları için; veri gönderen bir bileşen olan bir **meşgul çalışan** etkin olarak ileti gönderme biridir.</span><span class="sxs-lookup"><span data-stu-id="601f4-207">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="601f4-208">**Alınan toplam ileti veri yoluna iletileri**</span><span class="sxs-lookup"><span data-stu-id="601f4-208">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="601f4-209">**İleti veri yoluna iletileri alınan/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-209">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="601f4-210">**Toplam ileti veri yoluna yayımlanan**</span><span class="sxs-lookup"><span data-stu-id="601f4-210">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="601f4-211">**İleti veri yoluna iletileri yayımlanan/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-211">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="601f4-212">**İleti veri yolu aboneleri geçerli**</span><span class="sxs-lookup"><span data-stu-id="601f4-212">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="601f4-213">**İleti veri yolu aboneleri toplam**</span><span class="sxs-lookup"><span data-stu-id="601f4-213">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="601f4-214">**İleti veri yolu aboneleri/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-214">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="601f4-215">**Çalışanlar ayrılmış ileti veri yolu**</span><span class="sxs-lookup"><span data-stu-id="601f4-215">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="601f4-216">**İleti veri yolu meşgul çalışanların**</span><span class="sxs-lookup"><span data-stu-id="601f4-216">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="601f4-217">**İleti veri yolu konuları geçerli**</span><span class="sxs-lookup"><span data-stu-id="601f4-217">**Message Bus Topics Current**</span></span>

<span data-ttu-id="601f4-218">**Hata ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="601f4-218">**Error metrics**</span></span>

<span data-ttu-id="601f4-219">SignalR ileti trafiği tarafından oluşturulan hatalar ölçümleri.</span><span class="sxs-lookup"><span data-stu-id="601f4-219">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="601f4-220">**Hub çözümleme** hataları hub veya hub yönteminin çözümlenemiyor olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="601f4-220">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="601f4-221">**Hub çağrısının** hataları olan bir hub yöntemini çağırma sırasında oluşturulan özel durumları.</span><span class="sxs-lookup"><span data-stu-id="601f4-221">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="601f4-222">**Aktarım** bir HTTP isteği veya yanıtı sırasında oluşturulan bağlantı hatalarını hatalardır.</span><span class="sxs-lookup"><span data-stu-id="601f4-222">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="601f4-223">**Hatalar: Tüm toplam**</span><span class="sxs-lookup"><span data-stu-id="601f4-223">**Errors: All Total**</span></span>
- <span data-ttu-id="601f4-224">**Hata: Tüm/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-224">**Errors: All/Sec**</span></span>
- <span data-ttu-id="601f4-225">**Hata: Hub çözümleme toplam**</span><span class="sxs-lookup"><span data-stu-id="601f4-225">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="601f4-226">**Hata: Hub çözümleme/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-226">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="601f4-227">**Hatalar: Hub çağırma toplam**</span><span class="sxs-lookup"><span data-stu-id="601f4-227">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="601f4-228">**Hata: Hub çağırma/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-228">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="601f4-229">**Hata: Aktarım toplam**</span><span class="sxs-lookup"><span data-stu-id="601f4-229">**Errors: Transport Total**</span></span>
- <span data-ttu-id="601f4-230">**Hata: Aktarım/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-230">**Errors: Transport/Sec**</span></span>

<a id="scaleout_metrics"></a>

<span data-ttu-id="601f4-231">**Genişletme ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="601f4-231">**Scaleout metrics**</span></span>

<span data-ttu-id="601f4-232">Aşağıdaki trafik ve genişletme sağlayıcı tarafından oluşturulan hatalar ölçümleri.</span><span class="sxs-lookup"><span data-stu-id="601f4-232">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="601f4-233">A **akış** bu bağlamda bir ölçek birimi genişletme sağlayıcı tarafından kullanılan; SQL Server kullanılıyorsa bir tablo, Service Bus kullanılırsa, bir konu ve abonelik Redis kullandıysanız budur.</span><span class="sxs-lookup"><span data-stu-id="601f4-233">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="601f4-234">Her akış sıralı okuma ve yazma işlemleri sağlar; tek bir akış olası bir ölçek performans sorunu olduğundan, bu performans sorunu azaltmaya yardımcı olmak için akış sayısı artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="601f4-234">Each stream ensures ordered read and write operations; a single stream is a potential scale bottleneck, so the number of streams can be increased to help reduce that bottleneck.</span></span> <span data-ttu-id="601f4-235">Birden çok akış kullandıysanız, SignalR (parça) iletileri sırayla verilen tüm bağlantısından gönderilen iletileri garantiler şekilde bu akışları arasında otomatik olarak dağıtır.</span><span class="sxs-lookup"><span data-stu-id="601f4-235">If multiple streams are used, SignalR will automatically distribute (shard) messages across these streams in a way that ensures messages sent from any given connection are in order.</span></span>

<span data-ttu-id="601f4-236">[MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) SignalR tarafından tutulan genişletme gönderme sırası uzunluğu ayarı denetler.</span><span class="sxs-lookup"><span data-stu-id="601f4-236">The [MaxQueueLength](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx) setting controls the length of the scaleout send queue maintained by SignalR.</span></span> <span data-ttu-id="601f4-237">Bir değer için 0 birer birer için yapılandırılmış ileti devre kartı gönderilmek üzere bir gönderme sıradaki tüm iletileri yerleştirir daha büyük olarak ayarlama.</span><span class="sxs-lookup"><span data-stu-id="601f4-237">Setting it to a value greater than 0 will place all messages in a send queue to be sent one at a time to the configured messaging backplane.</span></span> <span data-ttu-id="601f4-238">Sıranın boyutunu yapılandırılan uzunluktan kalırsa, göndermek için sonraki çağrılar hemen başarısız bir [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) sırasındaki ileti sayısını ayarından küçük olana kadar yeniden.</span><span class="sxs-lookup"><span data-stu-id="601f4-238">If the size of the queue goes above the configured length, subsequent calls to send will fail immediately with an [InvalidOperationException](https://msdn.microsoft.com/en-us/library/system.invalidoperationexception(v=vs.118).aspx) until the number of messages in the queue is less than the setting again.</span></span> <span data-ttu-id="601f4-239">Uygulanan backplanes genellikle kendi queuing veya akış denetimi yerinde olduğundan çağrısı varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="601f4-239">Queueing is disabled by default because the implemented backplanes generally have their own queuing or flow-control in place.</span></span> <span data-ttu-id="601f4-240">SQL Server söz konusu olduğunda, bağlantı havuzu herhangi bir zamanda geçmeden gönderir sayısını etkili bir şekilde sınırlar.</span><span class="sxs-lookup"><span data-stu-id="601f4-240">In the case of SQL Server, connection pooling effectively limits the number of sends going on at any one time.</span></span>

<span data-ttu-id="601f4-241">Varsayılan olarak, yalnızca bir akış SQL Server ve Redis için kullanılan, beş akışlar, hizmet veri yolu için kullanılır ve çağrısı devre dışıdır, ancak bu ayarları, SQL Server ve hizmet veri yolu üzerinde yapılandırma yoluyla değiştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="601f4-241">By default, only one stream is used for SQL Server and Redis, five streams are used for Service Bus, and queueing is disabled, but these settings can be changed through configuration on SQL Server and Service Bus:</span></span>

<span data-ttu-id="601f4-242">**SQL Server devre kartı için tablo sayısı ve kuyruk uzunluğu yapılandırmak için .NET sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="601f4-242">**.NET Server Code for configuring table count and queue length for SQL Server backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample9.cs)]

<span data-ttu-id="601f4-243">**Hizmet veri yolu devre kartı için konu sayısı ve kuyruk uzunluğu yapılandırmak için .NET sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="601f4-243">**.NET Server Code for configuring topic count and queue length for Service Bus backplane**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample10.cs)]

<span data-ttu-id="601f4-244">A **arabelleği** akış, hatalı bir duruma geçtiğini; akış hatalı durumda olduğunda hemen akış artık hatalı kadar devre kartına gönderilen tüm iletiler başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="601f4-244">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="601f4-245">**Gönderme sırası uzunluğu** gönderilen, ancak henüz gönderilen ileti sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="601f4-245">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="601f4-246">**Saniye başına alınan genişletme ileti veri yoluna iletileri**</span><span class="sxs-lookup"><span data-stu-id="601f4-246">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="601f4-247">**Genişletme akışları toplam**</span><span class="sxs-lookup"><span data-stu-id="601f4-247">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="601f4-248">**Açık genişletme akışları**</span><span class="sxs-lookup"><span data-stu-id="601f4-248">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="601f4-249">**Arabelleğe alma genişletme akışlar**</span><span class="sxs-lookup"><span data-stu-id="601f4-249">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="601f4-250">**Genişletme hataları toplam**</span><span class="sxs-lookup"><span data-stu-id="601f4-250">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="601f4-251">**Genişletme Hatası/sn**</span><span class="sxs-lookup"><span data-stu-id="601f4-251">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="601f4-252">**Genişletme gönderme sırası uzunluğu**</span><span class="sxs-lookup"><span data-stu-id="601f4-252">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="601f4-253">Bu sayaçlar ölçme daha fazla bilgi için bkz: [SignalR genişletme Azure Service Bus ile](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="601f4-253">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="601f4-254">Diğer performans sayaçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="601f4-254">Using other performance counters</span></span>

<span data-ttu-id="601f4-255">Aşağıdaki performans sayaçları da uygulamanızın performansını izleme yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="601f4-255">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="601f4-256">**Bellek**</span><span class="sxs-lookup"><span data-stu-id="601f4-256">**Memory**</span></span>

- <span data-ttu-id="601f4-257">.NET CLR bellek\\# bayt tüm yığınlardaki (w3wp)</span><span class="sxs-lookup"><span data-stu-id="601f4-257">.NET CLR Memory\\# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="601f4-258">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="601f4-258">**ASP.NET**</span></span>

- <span data-ttu-id="601f4-259">ASP.NET\Requests geçerli</span><span class="sxs-lookup"><span data-stu-id="601f4-259">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="601f4-260">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="601f4-260">ASP.NET\Queued</span></span>
- <span data-ttu-id="601f4-261">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="601f4-261">ASP.NET\Rejected</span></span>

<span data-ttu-id="601f4-262">**CPU**</span><span class="sxs-lookup"><span data-stu-id="601f4-262">**CPU**</span></span>

- <span data-ttu-id="601f4-263">İşlemci Information\Processor zamanı</span><span class="sxs-lookup"><span data-stu-id="601f4-263">Processor Information\Processor Time</span></span>

<span data-ttu-id="601f4-264">**TCP/IP'Yİ**</span><span class="sxs-lookup"><span data-stu-id="601f4-264">**TCP/IP**</span></span>

- <span data-ttu-id="601f4-265">TCPv6/kurulan bağlantılar</span><span class="sxs-lookup"><span data-stu-id="601f4-265">TCPv6/Connections Established</span></span>
- <span data-ttu-id="601f4-266">TCPv4/kurulan bağlantılar</span><span class="sxs-lookup"><span data-stu-id="601f4-266">TCPv4/Connections Established</span></span>

<span data-ttu-id="601f4-267">**Web hizmeti**</span><span class="sxs-lookup"><span data-stu-id="601f4-267">**Web Service**</span></span>

- <span data-ttu-id="601f4-268">Web Service\Current bağlantıları</span><span class="sxs-lookup"><span data-stu-id="601f4-268">Web Service\Current Connections</span></span>
- <span data-ttu-id="601f4-269">Web Service\Maximum bağlantıları</span><span class="sxs-lookup"><span data-stu-id="601f4-269">Web Service\Maximum Connections</span></span>

<span data-ttu-id="601f4-270">**İş parçacığı oluşturma**</span><span class="sxs-lookup"><span data-stu-id="601f4-270">**Threading**</span></span>

- <span data-ttu-id="601f4-271">.NET CLR kilitler ve iş parçacığı\\geçerli mantıksal iş parçacığı sayısı</span><span class="sxs-lookup"><span data-stu-id="601f4-271">.NET CLR Locks And Threads\\# of current logical Threads</span></span>
- <span data-ttu-id="601f4-272">.NET CLR kilitler ve iş parçacığı\\geçerli fiziksel iş parçacığı sayısı</span><span class="sxs-lookup"><span data-stu-id="601f4-272">.NET CLR Locks And Threads\\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="601f4-273">Diğer Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="601f4-273">Other Resources</span></span>

<span data-ttu-id="601f4-274">ASP.NET Performans izleme ve ayarlama hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="601f4-274">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="601f4-275">[ASP.NET Performans genel bakış](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="601f4-275">[ASP.NET Performance Overview](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="601f4-276">IIS 7.5, IIS 7.0 ve IIS 6.0 ASP.NET iş parçacığı kullanımı</span><span class="sxs-lookup"><span data-stu-id="601f4-276">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="601f4-277">&lt;applicationPool&gt; öğesi (Web Ayarları)</span><span class="sxs-lookup"><span data-stu-id="601f4-277">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
