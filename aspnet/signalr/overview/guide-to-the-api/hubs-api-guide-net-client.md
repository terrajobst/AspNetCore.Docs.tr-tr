---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: "ASP.NET SignalR hub'ları API Kılavuzu - .NET istemci (C#) | Microsoft Docs"
author: pfletcher
description: "Bu belge SignalR sürüm 2'in Windows Mağazası (WinRT), WPF, Silverlight ve simgeler gibi .NET istemcileri için hub API'sini kullanmaya tanıtılmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: c52a02291e18b1dd8a9d95b33fe466d17aae835f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="8eb38-103">ASP.NET SignalR hub'ları API Kılavuzu - .NET istemci (C#)</span><span class="sxs-lookup"><span data-stu-id="8eb38-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================
<span data-ttu-id="8eb38-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8eb38-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="8eb38-105">Bu belge SignalR sürüm 2'in Windows Mağazası (WinRT), WPF, Silverlight ve konsol uygulamaları gibi .NET istemcileri için hub API kullanarak giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="8eb38-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="8eb38-106">SignalR hub'ları API bir sunucuya bağlanan istemciler ve sunucu istemcilerine uzaktan yordam çağrılarını (RPC) yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="8eb38-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="8eb38-107">Sunucu kodu, istemciler tarafından çağrılabilir yöntemlerini tanımlama ve istemci üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="8eb38-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="8eb38-108">İstemci kodu sunucudan çağrılabilir yöntemlerini tanımlama ve sunucu üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="8eb38-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="8eb38-109">SignalR, istemci-sunucu tesisat tümünün ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="8eb38-110">SignalR kalıcı bağlantılar olarak adlandırılan bir alt düzey API de sunar.</span><span class="sxs-lookup"><span data-stu-id="8eb38-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="8eb38-111">Giriş SignalR, hub'lar ve kalıcı bağlantılar için ya da tam bir SignalR uygulamasının nasıl oluşturulacağını gösteren bir öğretici için bkz: [Başlarken SignalR -](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="8eb38-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="8eb38-112">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="8eb38-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8eb38-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="8eb38-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="8eb38-114">.NET 4.5</span></span>
> - <span data-ttu-id="8eb38-115">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="8eb38-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="8eb38-116">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="8eb38-117">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="8eb38-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="8eb38-118">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="8eb38-118">Questions and comments</span></span>
> 
> <span data-ttu-id="8eb38-119">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="8eb38-120">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="8eb38-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="8eb38-121">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="8eb38-121">Overview</span></span>

<span data-ttu-id="8eb38-122">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="8eb38-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="8eb38-123">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="8eb38-123">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="8eb38-124">Bir bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="8eb38-124">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="8eb38-125">Silverlight istemcilerden etki alanları arası bağlantıları</span><span class="sxs-lookup"><span data-stu-id="8eb38-125">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="8eb38-126">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8eb38-126">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="8eb38-127">WPF istemcileri en fazla eşzamanlı bağlantı sayısını ayarlama</span><span class="sxs-lookup"><span data-stu-id="8eb38-127">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="8eb38-128">Sorgu dizesi parametreleri belirtme</span><span class="sxs-lookup"><span data-stu-id="8eb38-128">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="8eb38-129">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="8eb38-129">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="8eb38-130">HTTP üst bilgilerini belirtme</span><span class="sxs-lookup"><span data-stu-id="8eb38-130">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="8eb38-131">İstemci sertifikalarını belirtme</span><span class="sxs-lookup"><span data-stu-id="8eb38-131">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="8eb38-132">Hub proxy oluşturma</span><span class="sxs-lookup"><span data-stu-id="8eb38-132">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="8eb38-133">Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="8eb38-133">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="8eb38-134">Parametresiz yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-134">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="8eb38-135">Parametre türleri belirtme parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-135">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="8eb38-136">Dinamik nesneler parametre belirterek parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-136">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="8eb38-137">Bir işleyici kaldırma</span><span class="sxs-lookup"><span data-stu-id="8eb38-137">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="8eb38-138">İstemciden sunucu yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="8eb38-138">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="8eb38-139">Bağlantı ömür olayları işleme</span><span class="sxs-lookup"><span data-stu-id="8eb38-139">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="8eb38-140">Hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="8eb38-140">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="8eb38-141">İstemci-tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="8eb38-141">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="8eb38-142">WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod</span><span class="sxs-lookup"><span data-stu-id="8eb38-142">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="8eb38-143">Örnek .NET istemci projeleri için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="8eb38-143">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="8eb38-144">[Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) Github.com'u (WinRT, Silverlight, konsol uygulama örnekler) üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8eb38-144">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="8eb38-145">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) Github.com'u (WPF örnek) üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8eb38-145">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="8eb38-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span><span class="sxs-lookup"><span data-stu-id="8eb38-146">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="8eb38-147">Sunucu veya JavaScript istemcilerinin program konusunda daha fazla belgeler için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="8eb38-147">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="8eb38-148">SignalR hub'ları API Kılavuzu - sunucu</span><span class="sxs-lookup"><span data-stu-id="8eb38-148">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="8eb38-149">SignalR hub'ları API Kılavuzu - JavaScript istemci</span><span class="sxs-lookup"><span data-stu-id="8eb38-149">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="8eb38-150">API başvuru konuları API'si .NET 4.5 sürümüne bağlantılardır.</span><span class="sxs-lookup"><span data-stu-id="8eb38-150">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="8eb38-151">.NET 4 kullanıyorsanız, bkz: [API konuları .NET 4 sürümü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="8eb38-151">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="8eb38-152">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="8eb38-152">Client setup</span></span>

<span data-ttu-id="8eb38-153">Yükleme [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet paketi (değil [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) paketi).</span><span class="sxs-lookup"><span data-stu-id="8eb38-153">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="8eb38-154">Bu paket, .NET 4 ve .NET 4.5 için WinRT, Silverlight, WPF, konsol uygulaması veya Windows Phone istemcileri destekler.</span><span class="sxs-lookup"><span data-stu-id="8eb38-154">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="8eb38-155">İstemcide sahip SignalR sürümü sunucuda yüklü sürümü farklıdır, SignalR genellikle fark uyum mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="8eb38-155">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="8eb38-156">Örneğin, SignalR sürüm 2 çalıştıran bir sunucuda yüklü 1.1.x sahip istemciler ve bunun yanı sıra sürüm 2'in yüklü olan istemcileri destekler.</span><span class="sxs-lookup"><span data-stu-id="8eb38-156">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="8eb38-157">Sunucusundaki sürümü ve istemcinin sürümü arasındaki farkı çok fazla olabilir veya istemci sunucunun yeni olan, SignalR döndürürse bir `InvalidOperationException` istemci bağlantısı kurmaya çalıştığında özel durum.</span><span class="sxs-lookup"><span data-stu-id="8eb38-157">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="8eb38-158">Hata iletisi "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="8eb38-158">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="8eb38-159">Bir bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="8eb38-159">How to establish a connection</span></span>

<span data-ttu-id="8eb38-160">Bir bağlantı kurmadan önce oluşturmak zorunda bir `HubConnection` nesne ve bir proxy oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8eb38-160">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="8eb38-161">Bağlantı kurmak için çağrı `Start` yöntemi `HubConnection` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-161">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="8eb38-162">JavaScript istemciler için en az bir olay işleyicisi çağırmadan önce kaydetmek zorunda `Start` bağlantı kurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="8eb38-162">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="8eb38-163">Bu .NET istemcileri için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-163">This is not necessary for .NET clients.</span></span> <span data-ttu-id="8eb38-164">JavaScript istemciler için oluşturulan proxy kodu otomatik olarak mevcut tüm hub'ları için proxy sunucusu üzerinde oluşturur ve bir işleyici kaydetme olduğundan hangi hub belirtmek nasıl kullanmak istemci amaçlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="8eb38-164">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="8eb38-165">Ancak, bir proxy oluşturmak hub'ı kullanacak SignalR varsayar böylece için bir .NET istemci Hub proxy'leri el ile oluşturduğunuz.</span><span class="sxs-lookup"><span data-stu-id="8eb38-165">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="8eb38-166">Varsayılan örnek kod kullanır "/ signalr" SignalR hizmete bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="8eb38-166">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="8eb38-167">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="8eb38-167">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="8eb38-168">`Start` Yöntemini zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="8eb38-168">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="8eb38-169">Bağlantı kurulduktan sonra kod sonraki satırların kadar yürütme yok emin olmak için `await` ASP.NET 4.5 zaman uyumsuz bir yöntem olarak veya `.Wait()` zaman uyumlu bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="8eb38-169">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="8eb38-170">Kullanmayan `.Wait()` WinRT istemcisinde.</span><span class="sxs-lookup"><span data-stu-id="8eb38-170">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="8eb38-171">Silverlight istemcilerden etki alanları arası bağlantıları</span><span class="sxs-lookup"><span data-stu-id="8eb38-171">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="8eb38-172">Silverlight istemcilerden etki alanları arası bağlantıları etkinleştirme hakkında daha fazla bilgi için bkz: [bir hizmet kullanılabilir etki alanı sınırlar boyunca yapma](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="8eb38-172">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="8eb38-173">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8eb38-173">How to configure the connection</span></span>

<span data-ttu-id="8eb38-174">Bir bağlantı kurmadan önce aşağıdaki seçeneklerden birini belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8eb38-174">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="8eb38-175">Eşzamanlı bağlantı sayısı sınırı.</span><span class="sxs-lookup"><span data-stu-id="8eb38-175">Concurrent connections limit.</span></span>
- <span data-ttu-id="8eb38-176">Sorgu dizesi parametreleri.</span><span class="sxs-lookup"><span data-stu-id="8eb38-176">Query string parameters.</span></span>
- <span data-ttu-id="8eb38-177">Taşıma yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-177">The transport method.</span></span>
- <span data-ttu-id="8eb38-178">HTTP üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="8eb38-178">HTTP headers.</span></span>
- <span data-ttu-id="8eb38-179">İstemci sertifikaları.</span><span class="sxs-lookup"><span data-stu-id="8eb38-179">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="8eb38-180">WPF istemcileri en fazla eşzamanlı bağlantı sayısını ayarlama</span><span class="sxs-lookup"><span data-stu-id="8eb38-180">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="8eb38-181">WPF istemcileri en fazla 2'in varsayılan değerini eşzamanlı bağlantı sayısını artırmak olabilir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-181">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="8eb38-182">Önerilen değer 10'dur.</span><span class="sxs-lookup"><span data-stu-id="8eb38-182">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="8eb38-183">Daha fazla bilgi için bkz: [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="8eb38-183">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="8eb38-184">Sorgu dizesi parametreleri belirtme</span><span class="sxs-lookup"><span data-stu-id="8eb38-184">How to specify query string parameters</span></span>

<span data-ttu-id="8eb38-185">İstemci bağlandığında sunucuya veri göndermek istiyorsanız, sorgu dizesi parametreleri bağlantı nesnesine ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8eb38-185">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="8eb38-186">Aşağıdaki örnek, bir sorgu dizesi parametresi istemci kodu ayarlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-186">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="8eb38-187">Aşağıdaki örnek, bir sorgu dizesi parametresi sunucu kodu okuma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-187">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="8eb38-188">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="8eb38-188">How to specify the transport method</span></span>

<span data-ttu-id="8eb38-189">Bağlama işleminin bir parçası olarak, bir SignalR istemci sunucuyla desteklenen en iyi aktarım belirlemek için sunucu ve istemci tarafından normalde görüşür.</span><span class="sxs-lookup"><span data-stu-id="8eb38-189">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="8eb38-190">Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, bu anlaşma işlemi devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-190">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="8eb38-191">Taşıma yöntemini belirtmek için bir taşıyıcı nesnesi başlangıç yönteme geçirin.</span><span class="sxs-lookup"><span data-stu-id="8eb38-191">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="8eb38-192">Aşağıdaki örnek, aktarım yöntemi istemci kodda belirtme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-192">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="8eb38-193">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) ad alanı taşımayı belirtmek için kullanabileceğiniz aşağıdaki sınıflar içerir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-193">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="8eb38-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="8eb38-194">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="8eb38-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="8eb38-195">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="8eb38-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (kullanılabilir. yalnızca sunucu ve istemci .NET 4.5 kullandığınızda)</span><span class="sxs-lookup"><span data-stu-id="8eb38-196">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="8eb38-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (istemci ve sunucu tarafından desteklenen en iyi aktarım otomatik olarak seçer.</span><span class="sxs-lookup"><span data-stu-id="8eb38-197">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="8eb38-198">Varsayılan taşımayı burasıdır.</span><span class="sxs-lookup"><span data-stu-id="8eb38-198">This is the default transport.</span></span> <span data-ttu-id="8eb38-199">Bu konuda geçirme `Start` yöntemi her şeyi geçirme değil aynı etkiye sahiptir.)</span><span class="sxs-lookup"><span data-stu-id="8eb38-199">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="8eb38-200">Yalnızca tarayıcılar tarafından kullanıldığından ForeverFrame aktarım bu listeye dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="8eb38-200">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="8eb38-201">Sunucu kodu aktarım yönteminde denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - bağlam özelliğinden istemcisi hakkında bilgi almak nasıl](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="8eb38-201">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="8eb38-202">Taşımalar ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarımları ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="8eb38-202">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="8eb38-203">HTTP üst bilgilerini belirtme</span><span class="sxs-lookup"><span data-stu-id="8eb38-203">How to specify HTTP headers</span></span>

<span data-ttu-id="8eb38-204">HTTP üstbilgileri ayarlamak için kullanın `Headers` özelliği değerinin bağlantı nesnesindeki.</span><span class="sxs-lookup"><span data-stu-id="8eb38-204">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="8eb38-205">Aşağıdaki örnek, bir HTTP üstbilgisi Ekle gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-205">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="8eb38-206">İstemci sertifikalarını belirtme</span><span class="sxs-lookup"><span data-stu-id="8eb38-206">How to specify client certificates</span></span>

<span data-ttu-id="8eb38-207">İstemci sertifikalarını eklemek için kullanın `AddClientCertificate` bağlantı nesnesi üzerinde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-207">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="8eb38-208">Hub proxy oluşturma</span><span class="sxs-lookup"><span data-stu-id="8eb38-208">How to create the Hub proxy</span></span>

<span data-ttu-id="8eb38-209">Bir Hub sunucudan çağırabilirsiniz istemci üzerinde yöntemleri tanımlamak için ve sunucudaki Hub yöntemlerini çağırma, Hub için bir proxy çağırarak oluşturun `CreateHubProxy` değerinin bağlantı nesnesindeki.</span><span class="sxs-lookup"><span data-stu-id="8eb38-209">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="8eb38-210">Dize için ilettiğiniz `CreateHubProxy` Hub sınıfın adını ya da tarafından belirtilen adını `HubName` bir sunucu üzerinde kullanıldıysa özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8eb38-210">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="8eb38-211">Ad eşleştirme büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="8eb38-211">Name matching is case-insensitive.</span></span>

<span data-ttu-id="8eb38-212">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="8eb38-212">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="8eb38-213">**İstemci proxy Hub sınıfı için oluşturma**</span><span class="sxs-lookup"><span data-stu-id="8eb38-213">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="8eb38-214">Hub sınıfıyla tasarlamanız varsa bir `HubName` özniteliği, bu adı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8eb38-214">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="8eb38-215">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="8eb38-215">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="8eb38-216">**İstemci proxy Hub sınıfı için oluşturma**</span><span class="sxs-lookup"><span data-stu-id="8eb38-216">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="8eb38-217">Çağırırsanız `HubConnection.CreateHubProxy` verilerle birden çok kez aynı `hubName`, aynı önbelleğe alma `IHubProxy` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-217">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="8eb38-218">Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="8eb38-218">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="8eb38-219">Sunucu çağırabilirsiniz bir yöntemi tanımlamak için proxy's kullanın `On` olay işleyicisi kaydetmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="8eb38-219">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="8eb38-220">Yöntemi ad eşleştirme büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="8eb38-220">Method name matching is case-insensitive.</span></span> <span data-ttu-id="8eb38-221">Örneğin, `Clients.All.UpdateStockPrice` sunucuda yürütecek `updateStockPrice`, `updatestockprice`, veya `UpdateStockPrice` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="8eb38-221">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="8eb38-222">Farklı istemci platformları UI güncelleştirmek için nasıl yöntemi kodu yazdığınız farklı gereksinimleri vardır.</span><span class="sxs-lookup"><span data-stu-id="8eb38-222">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="8eb38-223">Gösterilen örnek WinRT (Windows mağazası .NET) istemciler için verilebilir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-223">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="8eb38-224">WPF, Silverlight ve konsol uygulaması örnekleri verilmiştir [bu konunun ilerleyen bölümlerinde ayrı bir bölüm](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="8eb38-224">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="8eb38-225">Parametresiz yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-225">Methods without parameters</span></span>

<span data-ttu-id="8eb38-226">İşleme yöntemi parametrelerini yoksa genel olmayan kullanın `On` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8eb38-226">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="8eb38-227">**Sunucu kodu parametresiz istemci yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="8eb38-227">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="8eb38-228">**Yöntem için WinRT istemci kodu adlı parametresiz sunucusundan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="8eb38-228">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="8eb38-229">Parametre türleri belirtme parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-229">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="8eb38-230">İşleme yöntemi parametrelere sahipse, parametre türleri genel türleri belirtin `On` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-230">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="8eb38-231">Genel aşırı `On` yöntemi, 8 adete kadar parametreleri (Windows Phone 7 4) belirtmenize olanak verir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-231">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="8eb38-232">Aşağıdaki örnekte, bir parametre gönderilen `UpdateStockPrice` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-232">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="8eb38-233">**Sunucu kodu parametresi olan istemci yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="8eb38-233">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="8eb38-234">**Parametresi için kullanılan hisse senedi sınıfı**</span><span class="sxs-lookup"><span data-stu-id="8eb38-234">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="8eb38-235">**Bir yöntem için WinRT istemci kodu adlı bir parametre olan sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="8eb38-235">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="8eb38-236">Dinamik nesneler parametre belirterek parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-236">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="8eb38-237">Alternatif genel tür parametreleri belirtme olarak `On` yöntemi, dinamik nesneler olarak parametreleri belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8eb38-237">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="8eb38-238">**Sunucu kodu parametresi olan istemci yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="8eb38-238">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="8eb38-239">**Parametresi için kullanılan hisse senedi sınıfı**</span><span class="sxs-lookup"><span data-stu-id="8eb38-239">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="8eb38-240">**Bir yöntem için WinRT istemci kodu adlı bir dinamik Nesne parametresi için kullanarak, bir parametre olan sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="8eb38-240">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="8eb38-241">Bir işleyici kaldırma</span><span class="sxs-lookup"><span data-stu-id="8eb38-241">How to remove a handler</span></span>

<span data-ttu-id="8eb38-242">Bir işleyici kaldırmak için arama kendi `Dispose` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-242">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="8eb38-243">**Sunucudan adlı bir yöntem için istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="8eb38-243">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="8eb38-244">**İşleyici kaldırmak için istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="8eb38-244">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="8eb38-245">İstemciden sunucu yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="8eb38-245">How to call server methods from the client</span></span>

<span data-ttu-id="8eb38-246">Sunucuda bir yöntemi çağırmak için `Invoke` Hub proxy yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-246">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="8eb38-247">Sunucu yönteminin dönüş değeri yoksa, genel olmayan kullanın `Invoke` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-247">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="8eb38-248">**Bir dönüş değerine sahip bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="8eb38-248">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="8eb38-249">**İstemci kodu bir dönüş değerine sahip bir yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="8eb38-249">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="8eb38-250">Sunucu yönteminin dönüş değeri varsa, dönüş türü genel türü belirtin `Invoke` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-250">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="8eb38-251">**Dönüş değeri olan ve bir karmaşık tür parametresi alan bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="8eb38-251">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="8eb38-252">**Parametre ve dönüş değeri için kullanılan hisse senedi sınıfı**</span><span class="sxs-lookup"><span data-stu-id="8eb38-252">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="8eb38-253">**İstemci kodu dönüş değeri yok ve bir ASP.NET 4.5 async yöntemi bir karmaşık tür parametresi alan bir yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="8eb38-253">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="8eb38-254">**İstemci kodu dönüş değeri yok ve bir zaman uyumlu yöntemi bir karmaşık tür parametresi alan bir yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="8eb38-254">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="8eb38-255">`Invoke` Yöntemi zaman uyumsuz olarak yürütür ve döndürür bir `Task` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="8eb38-255">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="8eb38-256">Belirtmediyseniz `await` veya `.Wait()`, kodun sonraki satırında, çağırmayı yöntemi yürütülmesi tamamlandı önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="8eb38-256">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="8eb38-257">Bağlantı ömür olayları işleme</span><span class="sxs-lookup"><span data-stu-id="8eb38-257">How to handle connection lifetime events</span></span>

<span data-ttu-id="8eb38-258">SignalR aşağıdaki bağlantıyı işleyebilir ömür olayları sağlar:</span><span class="sxs-lookup"><span data-stu-id="8eb38-258">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="8eb38-259">`Received`: Herhangi bir veri bağlantısı alındığında oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="8eb38-259">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="8eb38-260">Alınan veriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="8eb38-260">Provides the received data.</span></span>
- <span data-ttu-id="8eb38-261">`ConnectionSlow`: İstemci yavaş veya sık bırakma bir bağlantı algıladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8eb38-261">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="8eb38-262">`Reconnecting`: Temel aktarımı yeniden bağlanmayı başladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8eb38-262">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="8eb38-263">`Reconnected`: Temel aktarımı bağlandı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-263">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="8eb38-264">`StateChanged`: Bağlantı durumu değiştiğinde oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="8eb38-264">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="8eb38-265">Eski durum ve yeni durum sağlar.</span><span class="sxs-lookup"><span data-stu-id="8eb38-265">Provides the old state and the new state.</span></span> <span data-ttu-id="8eb38-266">Durum değerleri bağlantısı hakkında bilgi için bkz [ConnectionState numaralandırma](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="8eb38-266">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="8eb38-267">`Closed`: Bağlantı kesildi tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-267">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="8eb38-268">Örneğin, önemli değildir ancak aralıklı bağlantısı sorunlarına neden hataları için uyarı iletileri görüntülemek istiyorsanız, gibi yavaşlığı veya sık bağlantı, bırakarak işlemek `ConnectionSlow` olay.</span><span class="sxs-lookup"><span data-stu-id="8eb38-268">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="8eb38-269">Daha fazla bilgi için bkz: [anlama ve SignalR bağlantısı ömrü olaylarını işleme](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="8eb38-269">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="8eb38-270">Hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="8eb38-270">How to handle errors</span></span>

<span data-ttu-id="8eb38-271">Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, SignalR sonra bir hata döndürür özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-271">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="8eb38-272">Örneğin, bir çağrı varsa `newContosoChatMessage` başarısız, hata nesnesindeki hata iletisini içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" üretim istemciler için ayrıntılı hata iletileri için ayrıntılı hata iletileri etkinleştirmek isteyip istemediğinizi ancak güvenlik nedeniyle önerilmez gönderme sorun giderme amacıyla, aşağıdaki kodu sunucuda kullanın.</span><span class="sxs-lookup"><span data-stu-id="8eb38-272">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="8eb38-273">SignalR başlatır hataları işlemek için bir işleyici ekleyebilirsiniz `Error` değerinin bağlantı nesnesindeki olay.</span><span class="sxs-lookup"><span data-stu-id="8eb38-273">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="8eb38-274">Yöntem çağrılarını hataları işlemek için bir try-catch bloğu içinde kodu alın.</span><span class="sxs-lookup"><span data-stu-id="8eb38-274">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="8eb38-275">İstemci-tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="8eb38-275">How to enable client-side logging</span></span>

<span data-ttu-id="8eb38-276">İstemci-tarafı günlük kaydını etkinleştirmek için ayarlanmış `TraceLevel` ve `TraceWriter` bağlantı nesne üzerindeki özellikleri.</span><span class="sxs-lookup"><span data-stu-id="8eb38-276">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="8eb38-277">WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod</span><span class="sxs-lookup"><span data-stu-id="8eb38-277">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="8eb38-278">Sunucu çağırabilirsiniz istemci yöntemleri tanımlamak için daha önce gösterilen kod örnekleri WinRT istemcileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-278">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="8eb38-279">Aşağıdaki örnekler, WPF, Silverlight ve konsol uygulaması istemciler için eşdeğer kodunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="8eb38-279">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="8eb38-280">Parametresiz yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-280">Methods without parameters</span></span>

<span data-ttu-id="8eb38-281">**WPF istemci kodunu parametresiz sunucusundan adlı yöntemi**</span><span class="sxs-lookup"><span data-stu-id="8eb38-281">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="8eb38-282">**Parametresiz sunucusundan adlı bir yöntem için Silverlight istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="8eb38-282">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="8eb38-283">**Parametresiz sunucusundan konsol uygulaması istemci kodu yöntemi için çağrılır**</span><span class="sxs-lookup"><span data-stu-id="8eb38-283">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="8eb38-284">Parametre türleri belirtme parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-284">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="8eb38-285">**WPF istemci kodu bir yöntem için parametre sunucusuyla çağrılır**</span><span class="sxs-lookup"><span data-stu-id="8eb38-285">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="8eb38-286">**Bir parametre olan sunucudan adlı bir yöntem için Silverlight istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="8eb38-286">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="8eb38-287">**Konsol uygulaması istemci kodu bir yöntem için parametre sunucusuyla çağrılır**</span><span class="sxs-lookup"><span data-stu-id="8eb38-287">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="8eb38-288">Dinamik nesneler parametre belirterek parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="8eb38-288">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="8eb38-289">**Dinamik Nesne parametresi için kullanarak, bir parametre olan sunucudan adlı bir yöntem için WPF istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="8eb38-289">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="8eb38-290">**Dinamik Nesne parametresi için kullanarak, bir parametre olan sunucudan adlı bir yöntem için Silverlight istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="8eb38-290">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="8eb38-291">**Sunucu parametresi için bir dinamik nesnesi kullanılarak bir parametre ile bir yöntem için konsol uygulaması istemci kodu çağrılır**</span><span class="sxs-lookup"><span data-stu-id="8eb38-291">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
