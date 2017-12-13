---
uid: signalr/overview/older-versions/signalr-performance
title: SignalR performans (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: SignalR performans
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 52052ad202958eb5d648ceb64d9f06fb86ef3777
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="signalr-performance-signalr-1x"></a><span data-ttu-id="4b468-103">SignalR performans (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="4b468-103">SignalR Performance (SignalR 1.x)</span></span>
====================
<span data-ttu-id="4b468-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="4b468-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="4b468-105">Bu konuda, bir SignalR uygulama performansını artırmak için tasarım ve ölçmek açıklar.</span><span class="sxs-lookup"><span data-stu-id="4b468-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>


<span data-ttu-id="4b468-106">SignalR performans ve ölçeklendirme son sunumu için bkz: [ASP.NET SignalR ile gerçek zamanlı Web ölçeklendirme](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="4b468-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="4b468-107">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="4b468-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="4b468-108">Tasarım konuları</span><span class="sxs-lookup"><span data-stu-id="4b468-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="4b468-109">SignalR sunucunuz için performans ayarlama</span><span class="sxs-lookup"><span data-stu-id="4b468-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="4b468-110">Performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="4b468-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="4b468-111">SignalR performans sayaçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="4b468-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="4b468-112">Diğer performans sayaçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="4b468-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="4b468-113">Diğer kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4b468-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="4b468-114">Tasarım konuları</span><span class="sxs-lookup"><span data-stu-id="4b468-114">Design considerations</span></span>

<span data-ttu-id="4b468-115">Bu bölüm performans gereksiz ağ trafiğini oluşturarak engelliyordu değil emin olmak için bir SignalR uygulamasının Tasarım sırasında uygulanan desenleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="4b468-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="4b468-116">İleti sıklığı azaltma</span><span class="sxs-lookup"><span data-stu-id="4b468-116">Throttling message frequency</span></span>

<span data-ttu-id="4b468-117">Yüksek yoğunlukta (örneğin, gerçek zamanlı oyun uygulaması) iletileri gönderir bile bir uygulamada, ikinci bir birkaç iletileri göndermek uygulamaların çoğu gerekmez.</span><span class="sxs-lookup"><span data-stu-id="4b468-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="4b468-118">Her istemci ürettiği trafik miktarını azaltmak için bir ileti döngüsü kuyruklar ve göndereceğini en sık sabit oranı çok iletileri uygulanabilir (Bu süre içinde iletileri ise diğer bir deyişle, belirli sayıda iletileri kadar her saniye gönderilir terval) gönderilmek üzere kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4b468-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="4b468-119">İletiler (hem istemci hem de sunucusundan) için belirli bir hızı kısıtlar örnek bir uygulama için bkz: [SignalR ile yüksek sıklıkta gerçek zamanlı](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="4b468-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="4b468-120">İleti boyutunu azaltma</span><span class="sxs-lookup"><span data-stu-id="4b468-120">Reducing message size</span></span>

<span data-ttu-id="4b468-121">Serileştirilmiş nesneler boyutunu azaltarak bir SignalR ileti boyutunu azaltabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b468-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="4b468-122">Aktarılması gerekmez özellikleri içeren bir nesne gönderiyorsanız, sunucu kodunda bu özellikleri kullanarak serileştirilen gelen engelliyor `JsonIgnore` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b468-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="4b468-123">Özellik adlarını da iletide depolanır; Özellik adlarını kullanarak kısaltılmış `JsonProperty` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="4b468-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="4b468-124">Aşağıdaki kod örneği, istemciye gönderilen bir özelliği dışarıda tutması etme ve özellik adları kısaltın gösterir:</span><span class="sxs-lookup"><span data-stu-id="4b468-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="4b468-125">**İstemciye gönderilen veri dışlanacak JsonIgnore özniteliği ve ileti boyutunu azaltmak için Item özniteliğini gösterir .NET sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="4b468-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="4b468-126">Okunabilirliğini korumak amacıyla / Bakımı istemci kodu kısaltılmış özellik adları olabilir açmayla İnsan kolay için adları iletiyi aldıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="4b468-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="4b468-127">Aşağıdaki kod örneği, ileti sözleşmesi (eşleme) tanımlayarak uzun olanlara kısaltılmış adlarını yeniden eşleme, olası bir yol gösterir ve kullanarak `reMap` işlevi için en iyi duruma getirilmiş ileti sınıfı sözleşme uygulamak için:</span><span class="sxs-lookup"><span data-stu-id="4b468-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="4b468-128">**Yeniden harita oluşturma istemci tarafı JavaScript kodu okunabilir adlarına özellik adları kısaltılmış**</span><span class="sxs-lookup"><span data-stu-id="4b468-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="4b468-129">Adları aynı zamanda sunucu için istemci iletilerindeki yöntemin aynısı kullanılarak kısaltılması.</span><span class="sxs-lookup"><span data-stu-id="4b468-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="4b468-130">(Diğer bir deyişle, ileti için kullanılan bellek miktarı) bellek kullanım alanını azaltmaya ileti nesne da performansı geliştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b468-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="4b468-131">Örneğin, tam aralığını bir `int` gerekli değildir, bir `short` veya `byte` yerine kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4b468-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="4b468-132">Ayrıca, sunucu belleği, ileti boyutunu azaltma ileti veri yolunda iletileri depolandığından sunucu bellek sorunları gidermek erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4b468-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="4b468-133">SignalR sunucunuz için performans ayarlama</span><span class="sxs-lookup"><span data-stu-id="4b468-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="4b468-134">Aşağıdaki yapılandırma ayarları, bir SignalR uygulamasında daha iyi performans için sunucunuzu ayarlamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="4b468-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="4b468-135">Bir ASP.NET uygulamasında performansı hakkında genel bilgi için bkz: [ASP.NET performans geliştirme](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b468-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/en-us/library/ff647787.aspx).</span></span>

<span data-ttu-id="4b468-136">**SignalR yapılandırma ayarları**</span><span class="sxs-lookup"><span data-stu-id="4b468-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="4b468-137">**DefaultMessageBufferSize**: varsayılan olarak, SignalR hub bağlantı başına başına bellek 1000 iletilerinde korur.</span><span class="sxs-lookup"><span data-stu-id="4b468-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="4b468-138">Büyük iletileri kullanılıyorsa, bu da bu değeri azaltarak alleviated bellek sorunlar oluşturabilir.</span><span class="sxs-lookup"><span data-stu-id="4b468-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="4b468-139">Bu ayar ayarlanabilir `Application_Start` olay işleyicisi bir ASP.NET uygulamasında veya içinde `Configuration` kendini barındıran bir uygulamada OWIN başlangıç sınıfı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="4b468-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="4b468-140">Aşağıdaki örnek, kullanılan sunucu bellek miktarını azaltmak için uygulamanızın bellek ayak izini azaltmak için bu değeri azaltmak gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="4b468-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="4b468-141">**Varsayılan ileti arabellek boyutunu azaltmak için .NET sunucu kodunda Global.asax**</span><span class="sxs-lookup"><span data-stu-id="4b468-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="4b468-142">**IIS yapılandırma ayarları**</span><span class="sxs-lookup"><span data-stu-id="4b468-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="4b468-143">**Uygulama başına en fazla eşzamanlı istek**: eşzamanlı IIS sayısını artırmayı istekleri isteklere hizmet vermek için kullanılabilir sunucu kaynakları artacaktır.</span><span class="sxs-lookup"><span data-stu-id="4b468-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="4b468-144">Varsayılan değer 5000'dir; Bu ayar artırmak için yükseltilmiş bir komut isteminde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4b468-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="4b468-145">**ASP.NET yapılandırma ayarları**</span><span class="sxs-lookup"><span data-stu-id="4b468-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="4b468-146">Bu bölümde ayarlanabilir yapılandırma ayarlarını içeren `aspnet.config` dosya.</span><span class="sxs-lookup"><span data-stu-id="4b468-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="4b468-147">Bu dosya, platforma bağlı olarak iki konumlardan birinde bulunur:</span><span class="sxs-lookup"><span data-stu-id="4b468-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="4b468-148">SignalR performansını artırabilir ASP.NET ayarları aşağıdakileri içerir:</span><span class="sxs-lookup"><span data-stu-id="4b468-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="4b468-149">**CPU başına en fazla eşzamanlı istek**: Bu ayar artan performans sorunları hafifletmek.</span><span class="sxs-lookup"><span data-stu-id="4b468-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="4b468-150">Bu ayar artırmak için şu yapılandırma ayarı ekleme `aspnet.config` dosyası:</span><span class="sxs-lookup"><span data-stu-id="4b468-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="4b468-151">**İstek sırası sınırı**: toplam bağlantı sayısını aştığında `maxConcurrentRequestsPerCPU` ayarlama, ASP.NET bir sıra kullanan isteklerinin azaltma başlar.</span><span class="sxs-lookup"><span data-stu-id="4b468-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="4b468-152">Sıranın boyutunu artırmak için artırabilirsiniz `requestQueueLimit` ayarı.</span><span class="sxs-lookup"><span data-stu-id="4b468-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="4b468-153">Bunu yapmak için şu yapılandırma ayarı ekleyin `processModel` düğümünde `config/machine.config` (yerine `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="4b468-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="4b468-154">Performans sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="4b468-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="4b468-155">Bu bölümde, uygulamanızda performans sorunları bulma yolları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4b468-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="4b468-156">WebSocket kullanılmakta olduğunu doğrulama</span><span class="sxs-lookup"><span data-stu-id="4b468-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="4b468-157">SignalR istemci ve sunucu arasında iletişim için aktarımları çeşitli kullanabilirsiniz, ancak WebSocket önemli performans avantajı sunar ve istemci ve sunucu destekliyorsa kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4b468-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="4b468-158">İstemci ve sunucu WebSocket gereksinimlerini karşılayıp karşılamadığını belirlemek için bkz: [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="4b468-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="4b468-159">Hangi aktarım uygulamanızda kullanıldığını belirlemek için tarayıcının Geliştirici Araçları'nı kullanın ve hangi aktarım bağlantı için kullanıldığını görmek için günlüklerini inceleyin.</span><span class="sxs-lookup"><span data-stu-id="4b468-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="4b468-160">Internet Explorer ve Chrome tarayıcı geliştirme araçları kullanma hakkında daha fazla bilgi için bkz: [aktarımları ve geri dönüşler](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="4b468-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="4b468-161">SignalR performans sayaçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="4b468-161">Using SignalR performance counters</span></span>

<span data-ttu-id="4b468-162">Bu bölümde SignalR performans sayaçlarını, etkinleştirme ve nasıl kullanılacağını açıklar bulunan `Microsoft.AspNet.SignalR.Utils` paket.</span><span class="sxs-lookup"><span data-stu-id="4b468-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="4b468-163">Signalr.exe yükleme</span><span class="sxs-lookup"><span data-stu-id="4b468-163">Installing signalr.exe</span></span>

<span data-ttu-id="4b468-164">Performans sayaçları SignalR.exe adlı bir programı kullanarak sunucuya eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="4b468-164">Peformance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="4b468-165">Bu yardımcı programı yüklemek için aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="4b468-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="4b468-166">Visual Studio uygulamanızı seçin **Araçları**, **kitaplık Paket Yöneticisi**, **çözüm için NuGet paketlerini Yönet...**</span><span class="sxs-lookup"><span data-stu-id="4b468-166">In your Visual Studio application, select **Tools**, **Library Package Manager**, **Manage NuGet Packages for Solution...**</span></span>
2. <span data-ttu-id="4b468-167">Arama **signalr.utils**ve yükleme seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="4b468-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="4b468-168">Paketi yüklemek için lisans sözleşmesini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="4b468-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="4b468-169">SignalR.exe için yüklenecek `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="4b468-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="4b468-170">Performans sayaçları ile SignalR.exe yükleniyor</span><span class="sxs-lookup"><span data-stu-id="4b468-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="4b468-171">SignalR performans sayaçlarını yüklemek için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4b468-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="4b468-172">SignalR performans sayaçlarını kaldırmak için yükseltilmiş bir komut istemi aşağıdaki parametresiyle SignalR.exe çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="4b468-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="4b468-173">SignalR performans sayaçları</span><span class="sxs-lookup"><span data-stu-id="4b468-173">SignalR Performance counters</span></span>

<span data-ttu-id="4b468-174">Utilites paketi aşağıdaki performans sayaçlarını yükler.</span><span class="sxs-lookup"><span data-stu-id="4b468-174">The utilites package installs the following performance counters.</span></span> <span data-ttu-id="4b468-175">Son uygulama havuzu veya sunucu yeniden başlatıldığından beri "Toplam" sayaçları olayların sayısını ölçer.</span><span class="sxs-lookup"><span data-stu-id="4b468-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="4b468-176">**Bağlantı ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="4b468-176">**Connection metrics**</span></span>

<span data-ttu-id="4b468-177">Aşağıdaki ölçümleri ortaya bağlantı ömür olayları ölçün.</span><span class="sxs-lookup"><span data-stu-id="4b468-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="4b468-178">Daha fazla bilgi için bkz: [anlama ve bağlantı ömür olayları işleme](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="4b468-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="4b468-179">**Bağlı bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="4b468-179">**Connections Connected**</span></span>
- <span data-ttu-id="4b468-180">**Bağlantısı yeniden kurulmuş bağlantıları**</span><span class="sxs-lookup"><span data-stu-id="4b468-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="4b468-181">**Bağlantıları Disonnected**</span><span class="sxs-lookup"><span data-stu-id="4b468-181">**Connections Disonnected**</span></span>
- <span data-ttu-id="4b468-182">**Bağlantılar-geçerli**</span><span class="sxs-lookup"><span data-stu-id="4b468-182">**Connections Current**</span></span>

<span data-ttu-id="4b468-183">**İleti ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="4b468-183">**Message metrics**</span></span>

<span data-ttu-id="4b468-184">SignalR tarafından oluşturulan ileti trafiği ölçümleri.</span><span class="sxs-lookup"><span data-stu-id="4b468-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="4b468-185">**Toplam alınan bağlantı iletisi**</span><span class="sxs-lookup"><span data-stu-id="4b468-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="4b468-186">**Toplam gönderilen bağlantısı iletileri**</span><span class="sxs-lookup"><span data-stu-id="4b468-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="4b468-187">**Bağlantı alınan ileti sayısı/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="4b468-188">**Bağlantı gönderilen ileti sayısı/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="4b468-189">**İleti veri yolu ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="4b468-189">**Message bus metrics**</span></span>

<span data-ttu-id="4b468-190">İç SignalR ileti veri yolu, tüm gelen ve giden SignalR iletilerinin sıraya alınma sırası trafiğinin ölçümleri.</span><span class="sxs-lookup"><span data-stu-id="4b468-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="4b468-191">Bir ileti **yayımlanan** zaman, gönderilen veya yayın.</span><span class="sxs-lookup"><span data-stu-id="4b468-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="4b468-192">A **abone** bu bağlamda bir ileti veri yoluna abonelikte; bu istemciler ve sunucu sayısına eşit.</span><span class="sxs-lookup"><span data-stu-id="4b468-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="4b468-193">Bir **ayrılmış çalışan** , etkin bağlantıları için; veri gönderen bir bileşen olan bir **meşgul çalışan** etkin olarak ileti gönderme biridir.</span><span class="sxs-lookup"><span data-stu-id="4b468-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="4b468-194">**Alınan toplam ileti veri yoluna iletileri**</span><span class="sxs-lookup"><span data-stu-id="4b468-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="4b468-195">**İleti veri yoluna iletileri alınan/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="4b468-196">**Toplam ileti veri yoluna yayımlanan**</span><span class="sxs-lookup"><span data-stu-id="4b468-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="4b468-197">**İleti veri yoluna iletileri yayımlanan/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="4b468-198">**İleti veri yolu aboneleri geçerli**</span><span class="sxs-lookup"><span data-stu-id="4b468-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="4b468-199">**İleti veri yolu aboneleri toplam**</span><span class="sxs-lookup"><span data-stu-id="4b468-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="4b468-200">**İleti veri yolu aboneleri/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="4b468-201">**Çalışanlar ayrılmış ileti veri yolu**</span><span class="sxs-lookup"><span data-stu-id="4b468-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="4b468-202">**İleti veri yolu meşgul çalışanların**</span><span class="sxs-lookup"><span data-stu-id="4b468-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="4b468-203">**İleti veri yolu konuları geçerli**</span><span class="sxs-lookup"><span data-stu-id="4b468-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="4b468-204">**Hata ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="4b468-204">**Error metrics**</span></span>

<span data-ttu-id="4b468-205">SignalR ileti trafiği tarafından oluşturulan hatalar ölçümleri.</span><span class="sxs-lookup"><span data-stu-id="4b468-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="4b468-206">**Hub çözümleme** hataları hub veya hub yönteminin çözümlenemiyor olduğunda oluşur.</span><span class="sxs-lookup"><span data-stu-id="4b468-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="4b468-207">**Hub çağrısının** hataları olan bir hub yöntemini çağırma sırasında oluşturulan özel durumları.</span><span class="sxs-lookup"><span data-stu-id="4b468-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="4b468-208">**Aktarım** bir HTTP isteği veya yanıtı sırasında oluşturulan bağlantı hatalarını hatalardır.</span><span class="sxs-lookup"><span data-stu-id="4b468-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="4b468-209">**Hatalar: Tüm toplam**</span><span class="sxs-lookup"><span data-stu-id="4b468-209">**Errors: All Total**</span></span>
- <span data-ttu-id="4b468-210">**Hata: Tüm/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="4b468-211">**Hata: Hub çözümleme toplam**</span><span class="sxs-lookup"><span data-stu-id="4b468-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="4b468-212">**Hata: Hub çözümleme/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="4b468-213">**Hatalar: Hub çağırma toplam**</span><span class="sxs-lookup"><span data-stu-id="4b468-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="4b468-214">**Hata: Hub çağırma/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="4b468-215">**Hata: Aktarım toplam**</span><span class="sxs-lookup"><span data-stu-id="4b468-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="4b468-216">**Hata: Aktarım/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="4b468-217">**Genişletme ölçümleri**</span><span class="sxs-lookup"><span data-stu-id="4b468-217">**Scaleout metrics**</span></span>

<span data-ttu-id="4b468-218">Aşağıdaki trafik ve genişletme sağlayıcı tarafından oluşturulan hatalar ölçümleri.</span><span class="sxs-lookup"><span data-stu-id="4b468-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="4b468-219">A **akış** bu bağlamda bir ölçek birimi genişletme sağlayıcı tarafından kullanılan; SQL Server kullanılıyorsa bir tablo, Service Bus kullanılırsa, bir konu ve abonelik Redis kullandıysanız budur.</span><span class="sxs-lookup"><span data-stu-id="4b468-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="4b468-220">Varsayılan olarak, yalnızca bir akış kullanılır, ancak bu SQL Server ve hizmet veri yolu yapılandırması aracılığıyla artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="4b468-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="4b468-221">A **arabelleği** akış, hatalı bir duruma geçtiğini; akış hatalı durumda olduğunda hemen akış artık hatalı kadar devre kartına gönderilen tüm iletiler başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4b468-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="4b468-222">**Gönderme sırası uzunluğu** gönderilen, ancak henüz gönderilen ileti sayısıdır.</span><span class="sxs-lookup"><span data-stu-id="4b468-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="4b468-223">**Saniye başına alınan genişletme ileti veri yoluna iletileri**</span><span class="sxs-lookup"><span data-stu-id="4b468-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="4b468-224">**Genişletme akışları toplam**</span><span class="sxs-lookup"><span data-stu-id="4b468-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="4b468-225">**Açık genişletme akışları**</span><span class="sxs-lookup"><span data-stu-id="4b468-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="4b468-226">**Arabelleğe alma genişletme akışlar**</span><span class="sxs-lookup"><span data-stu-id="4b468-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="4b468-227">**Genişletme hataları toplam**</span><span class="sxs-lookup"><span data-stu-id="4b468-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="4b468-228">**Genişletme Hatası/sn**</span><span class="sxs-lookup"><span data-stu-id="4b468-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="4b468-229">**Genişletme gönderme sırası uzunluğu**</span><span class="sxs-lookup"><span data-stu-id="4b468-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="4b468-230">Bu sayaçlar ölçme daha fazla bilgi için bkz: [SignalR genişletme Azure Service Bus ile](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="4b468-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="4b468-231">Diğer performans sayaçlarını kullanma</span><span class="sxs-lookup"><span data-stu-id="4b468-231">Using other performance counters</span></span>

<span data-ttu-id="4b468-232">Aşağıdaki performans sayaçları da uygulamanızın performansını izleme yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4b468-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="4b468-233">**Bellek**</span><span class="sxs-lookup"><span data-stu-id="4b468-233">**Memory**</span></span>

- <span data-ttu-id="4b468-234">.NET CLR bellek # bayt tüm yığınlardaki (w3wp)</span><span class="sxs-lookup"><span data-stu-id="4b468-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="4b468-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="4b468-235">**ASP.NET**</span></span>

- <span data-ttu-id="4b468-236">ASP.NET\Requests geçerli</span><span class="sxs-lookup"><span data-stu-id="4b468-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="4b468-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="4b468-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="4b468-238">ASP.NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="4b468-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="4b468-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="4b468-239">**CPU**</span></span>

- <span data-ttu-id="4b468-240">İşlemci Information\Processor zamanı</span><span class="sxs-lookup"><span data-stu-id="4b468-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="4b468-241">**TCP/IP'Yİ**</span><span class="sxs-lookup"><span data-stu-id="4b468-241">**TCP/IP**</span></span>

- <span data-ttu-id="4b468-242">TCPv6/kurulan bağlantılar</span><span class="sxs-lookup"><span data-stu-id="4b468-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="4b468-243">TCPv4/kurulan bağlantılar</span><span class="sxs-lookup"><span data-stu-id="4b468-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="4b468-244">**Web hizmeti**</span><span class="sxs-lookup"><span data-stu-id="4b468-244">**Web Service**</span></span>

- <span data-ttu-id="4b468-245">Web Service\Current bağlantıları</span><span class="sxs-lookup"><span data-stu-id="4b468-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="4b468-246">Web Service\Maximum bağlantıları</span><span class="sxs-lookup"><span data-stu-id="4b468-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="4b468-247">**İş parçacığı oluşturma**</span><span class="sxs-lookup"><span data-stu-id="4b468-247">**Threading**</span></span>

- <span data-ttu-id="4b468-248">.NET CLR LocksAndThreads\# geçerli mantıksal iş parçacığı sayısı</span><span class="sxs-lookup"><span data-stu-id="4b468-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="4b468-249">.NET CLR LocksAnd iş parçacığı\# geçerli fiziksel iş parçacığı sayısı</span><span class="sxs-lookup"><span data-stu-id="4b468-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="4b468-250">Diğer Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4b468-250">Other Resources</span></span>

<span data-ttu-id="4b468-251">ASP.NET Performans izleme ve ayarlama hakkında daha fazla bilgi için aşağıdaki konulara bakın:</span><span class="sxs-lookup"><span data-stu-id="4b468-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="4b468-252">[ASP.NET Performans genel bakış](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="4b468-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/en-us/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="4b468-253">IIS 7.5, IIS 7.0 ve IIS 6.0 ASP.NET iş parçacığı kullanımı</span><span class="sxs-lookup"><span data-stu-id="4b468-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="4b468-254">&lt;applicationPool&gt; öğesi (Web Ayarları)</span><span class="sxs-lookup"><span data-stu-id="4b468-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/en-us/library/dd560842.aspx)
