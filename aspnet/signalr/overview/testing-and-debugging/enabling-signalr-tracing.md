---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: "SignalR izlemeyi etkinleştirme | Microsoft Docs"
author: tfitzmac
description: "Bu belge, etkinleştirmek ve SignalR sunucular ve istemciler için izlemeyi yapılandırmak açıklar. İzleme olayları hakkında tanılama bilgilerini görüntülemek sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/08/2014
ms.topic: article
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 2f01ab5d66e44cd82634f1b3df1ca6c78b7fd9d5
ms.sourcegitcommit: c07fb5cb5df0a12f9fe6735fcbc90964608fa687
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
<a name="enabling-signalr-tracing"></a><span data-ttu-id="f4867-104">SignalR izlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f4867-104">Enabling SignalR Tracing</span></span>
====================
<span data-ttu-id="f4867-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f4867-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f4867-106">Bu belge, etkinleştirmek ve SignalR sunucular ve istemciler için izlemeyi yapılandırmak açıklar.</span><span class="sxs-lookup"><span data-stu-id="f4867-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="f4867-107">İzleme olayları hakkında tanılama bilgisi SignalR uygulamanızda görüntülemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="f4867-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
> 
> <span data-ttu-id="f4867-108">Bu konuda, ilk olarak CAN Fletcher'dan tarafından yazıldı.</span><span class="sxs-lookup"><span data-stu-id="f4867-108">This topic was originally written by Patrick Fletcher.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f4867-109">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="f4867-109">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="f4867-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f4867-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="f4867-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="f4867-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="f4867-112">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="f4867-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="f4867-113">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="f4867-113">Questions and comments</span></span>
> 
> <span data-ttu-id="f4867-114">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="f4867-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="f4867-115">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="f4867-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="f4867-116">İzleme etkinleştirildiğinde SignalR uygulama olayları için günlük girişleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="f4867-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="f4867-117">Hem istemci hem de sunucu olayları oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="f4867-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="f4867-118">Sunucu günlüklerini bağlantısı, genişletme sağlayıcısındaki ve message bus olayları izleme.</span><span class="sxs-lookup"><span data-stu-id="f4867-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="f4867-119">İstemci günlükleri bağlantı olayları izleme.</span><span class="sxs-lookup"><span data-stu-id="f4867-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="f4867-120">SignalR 2.1 ve daha sonra istemci izleme hub çağırma iletileri tam içeriğini günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f4867-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="f4867-121">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="f4867-121">Contents</span></span>

- [<span data-ttu-id="f4867-122">Sunucunun izlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f4867-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="f4867-123">Metin dosyaları için sunucu olayları günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="f4867-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="f4867-124">Sunucu olayları olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="f4867-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="f4867-125">.NET istemci (Windows Masaüstü uygulamaları) izlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f4867-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="f4867-126">Konsola masaüstü istemcisi olayları günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="f4867-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="f4867-127">Bir metin dosyasına masaüstü istemcisi olayları günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="f4867-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="f4867-128">Windows Phone 8 istemcilerinde izlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f4867-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="f4867-129">Windows Phone istemci olayları günlüğe kaydetmeyi için kullanıcı Arabirimi</span><span class="sxs-lookup"><span data-stu-id="f4867-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="f4867-130">Hata ayıklama konsoluna Windows Phone istemci olayları günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="f4867-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="f4867-131">JavaScript istemci izlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f4867-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="f4867-132">Sunucunun izlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f4867-132">Enabling tracing on the server</span></span>

<span data-ttu-id="f4867-133">Sunucu uygulamanın yapılandırma dosyasına (App.config veya Web.config proje türüne bağlı olarak.) içinde izlemeyi etkinleştir Kategorilerini olayların günlüğe kaydetmek istediğiniz belirtin.</span><span class="sxs-lookup"><span data-stu-id="f4867-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="f4867-134">Yapılandırma dosyasında da mi bir metin dosyasına, Windows olay günlüğü veya uygulaması kullanarak özel bir günlük olaylarını günlüğe kaydedecek şekilde belirtmeniz [TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="f4867-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="f4867-135">Sunucu olay kategorileri iletilerinin aşağıdaki sıralar içerir:</span><span class="sxs-lookup"><span data-stu-id="f4867-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="f4867-136">Kaynak</span><span class="sxs-lookup"><span data-stu-id="f4867-136">Source</span></span> | <span data-ttu-id="f4867-137">İletiler</span><span class="sxs-lookup"><span data-stu-id="f4867-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="f4867-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="f4867-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="f4867-139">SQL Message Bus genişletme sağlayıcı Kurulum, veritabanı işlemi, hata ve zaman aşımı olayları</span><span class="sxs-lookup"><span data-stu-id="f4867-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="f4867-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="f4867-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="f4867-141">Hizmet veri yolu genişletme sağlayıcısı konu oluşturma ve aboneliği, hata ve mesajlaşma olayları</span><span class="sxs-lookup"><span data-stu-id="f4867-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="f4867-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="f4867-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="f4867-143">Redis genişletme sağlayıcı bağlantı, bağlantıyı kesme ve hata olayları</span><span class="sxs-lookup"><span data-stu-id="f4867-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="f4867-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="f4867-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="f4867-145">Genişletme ileti olayları</span><span class="sxs-lookup"><span data-stu-id="f4867-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="f4867-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="f4867-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="f4867-147">WebSocket taşıma bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları</span><span class="sxs-lookup"><span data-stu-id="f4867-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f4867-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="f4867-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="f4867-149">Bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları ServerSentEvents taşıma</span><span class="sxs-lookup"><span data-stu-id="f4867-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f4867-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="f4867-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="f4867-151">ForeverFrame aktarım bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları</span><span class="sxs-lookup"><span data-stu-id="f4867-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f4867-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="f4867-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="f4867-153">LongPolling aktarım bağlantı, bağlantıyı kesme, Mesajlaşma ve hata olayları</span><span class="sxs-lookup"><span data-stu-id="f4867-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="f4867-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="f4867-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="f4867-155">Bağlantı, bağlantıyı kesme ve keepalive olayları taşıma</span><span class="sxs-lookup"><span data-stu-id="f4867-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="f4867-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="f4867-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="f4867-157">Hub bulma olayları</span><span class="sxs-lookup"><span data-stu-id="f4867-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="f4867-158">Metin dosyaları için sunucu olayları günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="f4867-158">Logging server events to text files</span></span>

<span data-ttu-id="f4867-159">Aşağıdaki kod, her olay kategorisi izlemeyi etkinleştirmek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="f4867-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="f4867-160">Bu örnek uygulama metin dosyalarına olaylarını günlüğe kaydedecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="f4867-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="f4867-161">**İzlemeyi etkinleştirmek için XML sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="f4867-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="f4867-162">Yukarıdaki kod `SignalRSwitch` girişi belirtir [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx) belirtilen günlük gönderilen olaylar için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f4867-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="f4867-163">Bu durumda, kümesine `Verbose` yani tüm hata ayıklama ve izleme iletilerini günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="f4867-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="f4867-164">Aşağıdaki çıkış girişlerinden gösterir `transports.log.txt` Yukarıdaki yapılandırma dosyası kullanarak bir uygulamanın dosyası.</span><span class="sxs-lookup"><span data-stu-id="f4867-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="f4867-165">Yeni bir gösterir bağlantı, bağlantı kaldırıldı ve aktarım sinyal olaylar.</span><span class="sxs-lookup"><span data-stu-id="f4867-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="f4867-166">Sunucu olayları olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="f4867-166">Logging server events to the event log</span></span>

<span data-ttu-id="f4867-167">Bir metin dosyası yerine olay günlüğü olaylarını günlüğe kaydedecek şekilde girdileri değerlerini değiştirmek `sharedListeners` düğümü.</span><span class="sxs-lookup"><span data-stu-id="f4867-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="f4867-168">Aşağıdaki kod, sunucu olayları olay günlüğüne gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="f4867-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="f4867-169">**Olayları olay günlüğüne XML sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="f4867-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="f4867-170">Olayların uygulama günlüğüne kaydedilir ve aşağıda gösterildiği gibi Olay Görüntüleyicisi yoluyla kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="f4867-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![SignalR günlüklerini gösteren Olay Görüntüleyicisi](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="f4867-172">Olay günlüğü kullanırken ayarlayın **TraceLevel** için **hata** ileti sayısını yönetilebilir tutmak için.</span><span class="sxs-lookup"><span data-stu-id="f4867-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="f4867-173">.NET istemci (Windows Masaüstü uygulamaları) izlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f4867-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="f4867-174">.NET istemci olayları konsolu, bir metin dosyası veya uygulaması kullanarak özel bir günlük oturum [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="f4867-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="f4867-175">.NET istemci günlüğe kaydetmeyi etkinleştirmek için bağlantının ayarlayın `TraceLevel` özelliğine bir [TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) değeri ve `TraceWriter` özelliğine geçerli bir [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx) örneği.</span><span class="sxs-lookup"><span data-stu-id="f4867-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="f4867-176">Konsola masaüstü istemcisi olayları günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="f4867-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="f4867-177">Aşağıdaki C# kodu konsoluna .NET İstemcisi'nde olayları günlüğe gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="f4867-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="f4867-178">Bir metin dosyasına masaüstü istemcisi olayları günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="f4867-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="f4867-179">Aşağıdaki C# kodu, bir metin dosyasına .NET İstemcisi'nde olayları günlüğe gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="f4867-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="f4867-180">Aşağıdaki çıkış girişlerinden gösterir `ClientLog.txt` Yukarıdaki yapılandırma dosyası kullanarak bir uygulamanın dosyası.</span><span class="sxs-lookup"><span data-stu-id="f4867-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="f4867-181">Sunucuya bağlanırken istemci gösterir ve bir istemci yöntemi çağırma hub adı verilen `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="f4867-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="f4867-182">Windows Phone 8 istemcilerinde izlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f4867-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="f4867-183">SignalR uygulamalarını Windows Phone uygulamaları için aynı .NET istemcisi Masaüstü uygulamaları kullan ancak [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx) ve bir dosyaya yazma [StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx) kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="f4867-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/en-us/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/en-us/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="f4867-184">Bunun yerine, özel bir uygulamasını oluşturmak için gereken [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) izleme.</span><span class="sxs-lookup"><span data-stu-id="f4867-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span> 

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="f4867-185">Windows Phone istemci olayları günlüğe kaydetmeyi için kullanıcı Arabirimi</span><span class="sxs-lookup"><span data-stu-id="f4867-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="f4867-186">[SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) İzleme çıktısı Yazar bir Windows Phone örneği içeren bir [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) özel kullanarak [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) adlı uygulama `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="f4867-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="f4867-187">Bu sınıf bulunabilir **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** projesi.</span><span class="sxs-lookup"><span data-stu-id="f4867-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="f4867-188">Bir örneğini oluştururken `TextBlockWriter`, geçerli geçirmek [SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx)ve bir [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) , burada oluşturacak bir [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) izlemesi kullanmak için Çıktı:</span><span class="sxs-lookup"><span data-stu-id="f4867-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/en-us/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="f4867-189">İzleme çıkışının sonra yeni bir yazılır [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) oluşturulan [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) içinde geçirilen:</span><span class="sxs-lookup"><span data-stu-id="f4867-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="f4867-190">Hata ayıklama konsoluna Windows Phone istemci olayları günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="f4867-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="f4867-191">Kullanıcı Arabirimi yerine hata ayıklama konsoluna çıkış göndermek için uygulaması oluşturma [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) , hata ayıklama penceresine yazar ve, bağlantının atamak [TraceWriter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) özelliği:</span><span class="sxs-lookup"><span data-stu-id="f4867-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/en-us/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="f4867-192">İzleme bilgilerini sonra Visual Studio hata ayıklama penceresinde yazılır:</span><span class="sxs-lookup"><span data-stu-id="f4867-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="f4867-193">JavaScript istemci izlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="f4867-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="f4867-194">İstemci tarafı bir bağlantıda günlüğe kaydetmeyi etkinleştirmek üzere ayarlamak `logging` özelliği çağırmadan önce bağlantıyı nesne `start` bağlantı kurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="f4867-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="f4867-195">**Tarayıcı konsoluna (ile oluşturulan proxy) izlemeyi etkinleştirmek için istemci JavaScript kodu**</span><span class="sxs-lookup"><span data-stu-id="f4867-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="f4867-196">**Tarayıcı konsoluna (olmadan oluşturulan proxy) izlemeyi etkinleştirmek için istemci JavaScript kodu**</span><span class="sxs-lookup"><span data-stu-id="f4867-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="f4867-197">İzleme etkinleştirildiğinde, JavaScript istemci tarayıcı konsoluna olayları kaydeder.</span><span class="sxs-lookup"><span data-stu-id="f4867-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="f4867-198">Tarayıcı konsoluna erişmek için bkz: [izleme taşımaları](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="f4867-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="f4867-199">Aşağıdaki ekran görüntüsünde izlemenin etkin bir SignalR JavaScript istemci gösterir.</span><span class="sxs-lookup"><span data-stu-id="f4867-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="f4867-200">Bağlantı ve hub çağırma olayları tarayıcı konsolunda gösterir:</span><span class="sxs-lookup"><span data-stu-id="f4867-200">It shows connection and hub invocation events in the browser console:</span></span>

![Tarayıcı konsoluna SignalR izleme olayları](enabling-signalr-tracing/_static/image4.png)
