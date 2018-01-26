---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
title: "ASP.NET SignalR hub'ları API Kılavuzu - .NET istemci (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Bu belge SignalR sürüm 2'in Windows Mağazası (WinRT), WPF, Silverlight ve simgeler gibi .NET istemcileri için hub API'sini kullanmaya tanıtılmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: c334adc3-d6dc-44f3-9f06-f7634475aad3
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: 9a61bd255a217876aa2fdbeb6389539483b9f013
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---net-client-signalr-1x"></a><span data-ttu-id="e044d-103">ASP.NET SignalR hub'ları API Kılavuzu - .NET istemci (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="e044d-103">ASP.NET SignalR Hubs API Guide - .NET Client (SignalR 1.x)</span></span>
====================
<span data-ttu-id="e044d-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e044d-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="e044d-105">Bu belge SignalR sürüm 2'in Windows Mağazası (WinRT), WPF, Silverlight ve konsol uygulamaları gibi .NET istemcileri için hub API kullanarak giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="e044d-105">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
> 
> <span data-ttu-id="e044d-106">SignalR hub'ları API bir sunucuya bağlanan istemciler ve sunucu istemcilerine uzaktan yordam çağrılarını (RPC) yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="e044d-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="e044d-107">Sunucu kodu, istemciler tarafından çağrılabilir yöntemlerini tanımlama ve istemci üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="e044d-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="e044d-108">İstemci kodu sunucudan çağrılabilir yöntemlerini tanımlama ve sunucu üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="e044d-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="e044d-109">SignalR, istemci-sunucu tesisat tümünün ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="e044d-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="e044d-110">SignalR kalıcı bağlantılar olarak adlandırılan bir alt düzey API de sunar.</span><span class="sxs-lookup"><span data-stu-id="e044d-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="e044d-111">Giriş SignalR, hub'lar ve kalıcı bağlantılar için ya da tam bir SignalR uygulamasının nasıl oluşturulacağını gösteren bir öğretici için bkz: [Başlarken SignalR -](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="e044d-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="e044d-112">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="e044d-112">Overview</span></span>

<span data-ttu-id="e044d-113">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="e044d-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="e044d-114">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="e044d-114">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="e044d-115">Bir bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="e044d-115">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="e044d-116">Silverlight istemcilerden etki alanları arası bağlantıları</span><span class="sxs-lookup"><span data-stu-id="e044d-116">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="e044d-117">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e044d-117">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="e044d-118">WPF istemcileri en fazla eşzamanlı bağlantı sayısını ayarlama</span><span class="sxs-lookup"><span data-stu-id="e044d-118">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="e044d-119">Sorgu dizesi parametreleri belirtme</span><span class="sxs-lookup"><span data-stu-id="e044d-119">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="e044d-120">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="e044d-120">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="e044d-121">HTTP üst bilgilerini belirtme</span><span class="sxs-lookup"><span data-stu-id="e044d-121">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="e044d-122">İstemci sertifikalarını belirtme</span><span class="sxs-lookup"><span data-stu-id="e044d-122">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="e044d-123">Hub proxy oluşturma</span><span class="sxs-lookup"><span data-stu-id="e044d-123">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="e044d-124">Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="e044d-124">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="e044d-125">Parametresiz yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e044d-125">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="e044d-126">Parametre türleri belirtme parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e044d-126">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="e044d-127">Dinamik nesneler parametre belirterek parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e044d-127">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="e044d-128">Bir işleyici kaldırma</span><span class="sxs-lookup"><span data-stu-id="e044d-128">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="e044d-129">İstemciden sunucu yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="e044d-129">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="e044d-130">Bağlantı ömür olayları işleme</span><span class="sxs-lookup"><span data-stu-id="e044d-130">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="e044d-131">Hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="e044d-131">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="e044d-132">İstemci-tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="e044d-132">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="e044d-133">WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod</span><span class="sxs-lookup"><span data-stu-id="e044d-133">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="e044d-134">Örnek .NET istemci projeleri için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="e044d-134">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="e044d-135">[Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) Github.com'u (WinRT, Silverlight, konsol uygulama örnekler) üzerinde.</span><span class="sxs-lookup"><span data-stu-id="e044d-135">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="e044d-136">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) Github.com'u (WPF örnek) üzerinde.</span><span class="sxs-lookup"><span data-stu-id="e044d-136">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="e044d-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span><span class="sxs-lookup"><span data-stu-id="e044d-137">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="e044d-138">Sunucu veya JavaScript istemcilerinin program konusunda daha fazla belgeler için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="e044d-138">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="e044d-139">SignalR hub'ları API Kılavuzu - sunucu</span><span class="sxs-lookup"><span data-stu-id="e044d-139">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="e044d-140">SignalR hub'ları API Kılavuzu - JavaScript istemci</span><span class="sxs-lookup"><span data-stu-id="e044d-140">SignalR Hubs API Guide - JavaScript Client</span></span>](../guide-to-the-api/hubs-api-guide-javascript-client.md)

<span data-ttu-id="e044d-141">API başvuru konuları API'si .NET 4.5 sürümüne bağlantılardır.</span><span class="sxs-lookup"><span data-stu-id="e044d-141">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="e044d-142">.NET 4 kullanıyorsanız, bkz: [API konuları .NET 4 sürümü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="e044d-142">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="e044d-143">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="e044d-143">Client setup</span></span>

<span data-ttu-id="e044d-144">Yükleme [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet paketi (değil [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) paketi).</span><span class="sxs-lookup"><span data-stu-id="e044d-144">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="e044d-145">Bu paket, .NET 4 ve .NET 4.5 için WinRT, Silverlight, WPF, konsol uygulaması veya Windows Phone istemcileri destekler.</span><span class="sxs-lookup"><span data-stu-id="e044d-145">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="e044d-146">İstemcide sahip SignalR sürümü sunucuda yüklü sürümü farklıdır, SignalR genellikle fark uyum mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="e044d-146">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="e044d-147">Örneğin, SignalR sürüm 2.0 yayımlanır ve bu sunucuda yüklediğinizde sunucu 1.1.x 2.0 yüklü istemcileri yanı sıra yüklü olan istemcileri destekler.</span><span class="sxs-lookup"><span data-stu-id="e044d-147">For example, when SignalR version 2.0 is released and you install that on the server, the server will support clients that have 1.1.x installed as well as clients that have 2.0 installed.</span></span> <span data-ttu-id="e044d-148">Sunucusundaki sürümü ve istemcinin sürümü arasındaki farkı çok fazlaysa, SignalR oluşturur bir `InvalidOperationException` istemci bağlantısı kurmaya çalıştığında özel durum.</span><span class="sxs-lookup"><span data-stu-id="e044d-148">If the difference between the version on the server and the version on the client is too great, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="e044d-149">Hata iletisi "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="e044d-149">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="e044d-150">Bir bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="e044d-150">How to establish a connection</span></span>

<span data-ttu-id="e044d-151">Bir bağlantı kurmadan önce oluşturmak zorunda bir `HubConnection` nesne ve bir proxy oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e044d-151">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="e044d-152">Bağlantı kurmak için çağrı `Start` yöntemi `HubConnection` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="e044d-152">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="e044d-153">JavaScript istemciler için en az bir olay işleyicisi çağırmadan önce kaydetmek zorunda `Start` bağlantı kurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="e044d-153">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="e044d-154">Bu .NET istemcileri için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="e044d-154">This is not necessary for .NET clients.</span></span> <span data-ttu-id="e044d-155">JavaScript istemciler için oluşturulan proxy kodu otomatik olarak mevcut tüm hub'ları için proxy sunucusu üzerinde oluşturur ve bir işleyici kaydetme olduğundan hangi hub belirtmek nasıl kullanmak istemci amaçlamaktadır.</span><span class="sxs-lookup"><span data-stu-id="e044d-155">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="e044d-156">Ancak, bir proxy oluşturmak hub'ı kullanacak SignalR varsayar böylece için bir .NET istemci Hub proxy'leri el ile oluşturduğunuz.</span><span class="sxs-lookup"><span data-stu-id="e044d-156">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="e044d-157">Varsayılan örnek kod kullanır "/ signalr" SignalR hizmete bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="e044d-157">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="e044d-158">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="e044d-158">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="e044d-159">`Start` Yöntemini zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="e044d-159">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="e044d-160">Bağlantı kurulduktan sonra kod sonraki satırların kadar yürütme yok emin olmak için `await` ASP.NET 4.5 zaman uyumsuz bir yöntem olarak veya `.Wait()` zaman uyumlu bir yöntem.</span><span class="sxs-lookup"><span data-stu-id="e044d-160">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="e044d-161">Kullanmayan `.Wait()` WinRT istemcisinde.</span><span class="sxs-lookup"><span data-stu-id="e044d-161">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<span data-ttu-id="e044d-162">`HubConnection` İş parçacığı bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e044d-162">The `HubConnection` class is thread-safe.</span></span>

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="e044d-163">Silverlight istemcilerden etki alanları arası bağlantıları</span><span class="sxs-lookup"><span data-stu-id="e044d-163">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="e044d-164">Silverlight istemcilerden etki alanları arası bağlantıları etkinleştirme hakkında daha fazla bilgi için bkz: [bir hizmet kullanılabilir etki alanı sınırlar boyunca yapma](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="e044d-164">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="e044d-165">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e044d-165">How to configure the connection</span></span>

<span data-ttu-id="e044d-166">Bir bağlantı kurmadan önce aşağıdaki seçeneklerden birini belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e044d-166">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="e044d-167">Eşzamanlı bağlantı sayısı sınırı.</span><span class="sxs-lookup"><span data-stu-id="e044d-167">Concurrent connections limit.</span></span>
- <span data-ttu-id="e044d-168">Sorgu dizesi parametreleri.</span><span class="sxs-lookup"><span data-stu-id="e044d-168">Query string parameters.</span></span>
- <span data-ttu-id="e044d-169">Taşıma yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e044d-169">The transport method.</span></span>
- <span data-ttu-id="e044d-170">HTTP üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="e044d-170">HTTP headers.</span></span>
- <span data-ttu-id="e044d-171">İstemci sertifikaları.</span><span class="sxs-lookup"><span data-stu-id="e044d-171">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="e044d-172">WPF istemcileri en fazla eşzamanlı bağlantı sayısını ayarlama</span><span class="sxs-lookup"><span data-stu-id="e044d-172">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="e044d-173">WPF istemcileri en fazla 2'in varsayılan değerini eşzamanlı bağlantı sayısını artırmak olabilir.</span><span class="sxs-lookup"><span data-stu-id="e044d-173">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="e044d-174">Önerilen değer 10'dur.</span><span class="sxs-lookup"><span data-stu-id="e044d-174">The recommended value is 10.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="e044d-175">Daha fazla bilgi için bkz: [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="e044d-175">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="e044d-176">Sorgu dizesi parametreleri belirtme</span><span class="sxs-lookup"><span data-stu-id="e044d-176">How to specify query string parameters</span></span>

<span data-ttu-id="e044d-177">İstemci bağlandığında sunucuya veri göndermek istiyorsanız, sorgu dizesi parametreleri bağlantı nesnesine ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e044d-177">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="e044d-178">Aşağıdaki örnek, bir sorgu dizesi parametresi istemci kodu ayarlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e044d-178">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="e044d-179">Aşağıdaki örnek, bir sorgu dizesi parametresi sunucu kodu okuma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e044d-179">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="e044d-180">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="e044d-180">How to specify the transport method</span></span>

<span data-ttu-id="e044d-181">Bağlama işleminin bir parçası olarak, bir SignalR istemci sunucuyla desteklenen en iyi aktarım belirlemek için sunucu ve istemci tarafından normalde görüşür.</span><span class="sxs-lookup"><span data-stu-id="e044d-181">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="e044d-182">Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, bu anlaşma işlemi devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="e044d-182">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="e044d-183">Taşıma yöntemini belirtmek için bir taşıyıcı nesnesi başlangıç yönteme geçirin.</span><span class="sxs-lookup"><span data-stu-id="e044d-183">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="e044d-184">Aşağıdaki örnek, aktarım yöntemi istemci kodda belirtme gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e044d-184">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="e044d-185">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) ad alanı taşımayı belirtmek için kullanabileceğiniz aşağıdaki sınıflar içerir.</span><span class="sxs-lookup"><span data-stu-id="e044d-185">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="e044d-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="e044d-186">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="e044d-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="e044d-187">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="e044d-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (kullanılabilir. yalnızca sunucu ve istemci .NET 4.5 kullandığınızda)</span><span class="sxs-lookup"><span data-stu-id="e044d-188">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="e044d-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (istemci ve sunucu tarafından desteklenen en iyi aktarım otomatik olarak seçer.</span><span class="sxs-lookup"><span data-stu-id="e044d-189">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="e044d-190">Varsayılan taşımayı burasıdır.</span><span class="sxs-lookup"><span data-stu-id="e044d-190">This is the default transport.</span></span> <span data-ttu-id="e044d-191">Bu konuda geçirme `Start` yöntemi her şeyi geçirme değil aynı etkiye sahiptir.)</span><span class="sxs-lookup"><span data-stu-id="e044d-191">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="e044d-192">Yalnızca tarayıcılar tarafından kullanıldığından ForeverFrame aktarım bu listeye dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="e044d-192">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="e044d-193">Sunucu kodu aktarım yönteminde denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - bağlam özelliğinden istemcisi hakkında bilgi almak nasıl](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="e044d-193">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="e044d-194">Taşımalar ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarımları ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="e044d-194">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="e044d-195">HTTP üst bilgilerini belirtme</span><span class="sxs-lookup"><span data-stu-id="e044d-195">How to specify HTTP headers</span></span>

<span data-ttu-id="e044d-196">HTTP üstbilgileri ayarlamak için kullanın `Headers` özelliği değerinin bağlantı nesnesindeki.</span><span class="sxs-lookup"><span data-stu-id="e044d-196">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="e044d-197">Aşağıdaki örnek, bir HTTP üstbilgisi Ekle gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e044d-197">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="e044d-198">İstemci sertifikalarını belirtme</span><span class="sxs-lookup"><span data-stu-id="e044d-198">How to specify client certificates</span></span>

<span data-ttu-id="e044d-199">İstemci sertifikalarını eklemek için kullanın `AddClientCertificate` bağlantı nesnesi üzerinde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e044d-199">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="e044d-200">Hub proxy oluşturma</span><span class="sxs-lookup"><span data-stu-id="e044d-200">How to create the Hub proxy</span></span>

<span data-ttu-id="e044d-201">Bir Hub sunucudan çağırabilirsiniz istemci üzerinde yöntemleri tanımlamak için ve sunucudaki Hub yöntemlerini çağırma, Hub için bir proxy çağırarak oluşturun `CreateHubProxy` değerinin bağlantı nesnesindeki.</span><span class="sxs-lookup"><span data-stu-id="e044d-201">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="e044d-202">Dize için ilettiğiniz `CreateHubProxy` Hub sınıfın adını ya da tarafından belirtilen adını `HubName` bir sunucu üzerinde kullanıldıysa özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e044d-202">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="e044d-203">Ad eşleştirme büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="e044d-203">Name matching is case-insensitive.</span></span>

<span data-ttu-id="e044d-204">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="e044d-204">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="e044d-205">**İstemci proxy Hub sınıfı için oluşturma**</span><span class="sxs-lookup"><span data-stu-id="e044d-205">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="e044d-206">Hub sınıfıyla tasarlamanız varsa bir `HubName` özniteliği, bu adı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e044d-206">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="e044d-207">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="e044d-207">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="e044d-208">**İstemci proxy Hub sınıfı için oluşturma**</span><span class="sxs-lookup"><span data-stu-id="e044d-208">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="e044d-209">Proxy nesnesi iş parçacığı güvenlidir.</span><span class="sxs-lookup"><span data-stu-id="e044d-209">The proxy object is thread-safe.</span></span> <span data-ttu-id="e044d-210">Aslında, çağırırsanız `HubConnection.CreateHubProxy` verilerle birden çok kez aynı `hubName`, aynı önbelleğe alma `IHubProxy` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="e044d-210">In fact, if you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="e044d-211">Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="e044d-211">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="e044d-212">Sunucu çağırabilirsiniz bir yöntemi tanımlamak için proxy's kullanın `On` olay işleyicisi kaydetmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="e044d-212">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="e044d-213">Yöntemi ad eşleştirme büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="e044d-213">Method name matching is case-insensitive.</span></span> <span data-ttu-id="e044d-214">Örneğin, `Clients.All.UpdateStockPrice` sunucuda yürütecek `updateStockPrice`, `updatestockprice`, veya `UpdateStockPrice` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="e044d-214">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="e044d-215">Farklı istemci platformları UI güncelleştirmek için nasıl yöntemi kodu yazdığınız farklı gereksinimleri vardır.</span><span class="sxs-lookup"><span data-stu-id="e044d-215">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="e044d-216">Gösterilen örnek WinRT (Windows mağazası .NET) istemciler için verilebilir.</span><span class="sxs-lookup"><span data-stu-id="e044d-216">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="e044d-217">WPF, Silverlight ve konsol uygulaması örnekleri verilmiştir [bu konunun ilerleyen bölümlerinde ayrı bir bölüm](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="e044d-217">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="e044d-218">Parametresiz yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e044d-218">Methods without parameters</span></span>

<span data-ttu-id="e044d-219">İşleme yöntemi parametrelerini yoksa genel olmayan kullanın `On` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e044d-219">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="e044d-220">**Sunucu kodu parametresiz istemci yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="e044d-220">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="e044d-221">**Yöntem için WinRT istemci kodu adlı parametresiz sunucusundan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="e044d-221">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="e044d-222">Parametre türleri belirtme parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e044d-222">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="e044d-223">İşleme yöntemi parametrelere sahipse, parametre türleri genel türleri belirtin `On` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e044d-223">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="e044d-224">Genel aşırı `On` yöntemi, 8 adete kadar parametreleri (Windows Phone 7 4) belirtmenize olanak verir.</span><span class="sxs-lookup"><span data-stu-id="e044d-224">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="e044d-225">Aşağıdaki örnekte, bir parametre gönderilen `UpdateStockPrice` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e044d-225">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="e044d-226">**Sunucu kodu parametresi olan istemci yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="e044d-226">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="e044d-227">**Parametresi için kullanılan hisse senedi sınıfı**</span><span class="sxs-lookup"><span data-stu-id="e044d-227">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="e044d-228">**Bir yöntem için WinRT istemci kodu adlı bir parametre olan sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="e044d-228">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="e044d-229">Dinamik nesneler parametre belirterek parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e044d-229">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="e044d-230">Alternatif genel tür parametreleri belirtme olarak `On` yöntemi, dinamik nesneler olarak parametreleri belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e044d-230">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="e044d-231">**Sunucu kodu parametresi olan istemci yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="e044d-231">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="e044d-232">**Parametresi için kullanılan hisse senedi sınıfı**</span><span class="sxs-lookup"><span data-stu-id="e044d-232">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="e044d-233">**Bir yöntem için WinRT istemci kodu adlı bir dinamik Nesne parametresi için kullanarak, bir parametre olan sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="e044d-233">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="e044d-234">Bir işleyici kaldırma</span><span class="sxs-lookup"><span data-stu-id="e044d-234">How to remove a handler</span></span>

<span data-ttu-id="e044d-235">Bir işleyici kaldırmak için arama kendi `Dispose` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e044d-235">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="e044d-236">**Sunucudan adlı bir yöntem için istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="e044d-236">**Client code for a method called from server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="e044d-237">**İşleyici kaldırmak için istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="e044d-237">**Client code to remove the handler**</span></span>

[!code-css[Main](signalr-1x-hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="e044d-238">İstemciden sunucu yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="e044d-238">How to call server methods from the client</span></span>

<span data-ttu-id="e044d-239">Sunucuda bir yöntemi çağırmak için `Invoke` Hub proxy yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e044d-239">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="e044d-240">Sunucu yönteminin dönüş değeri yoksa, genel olmayan kullanın `Invoke` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e044d-240">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="e044d-241">**Bir dönüş değerine sahip bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="e044d-241">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="e044d-242">**İstemci kodu bir dönüş değerine sahip bir yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="e044d-242">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="e044d-243">Sunucu yönteminin dönüş değeri varsa, dönüş türü genel türü belirtin `Invoke` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e044d-243">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="e044d-244">**Dönüş değeri olan ve bir karmaşık tür parametresi alan bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="e044d-244">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="e044d-245">**Parametre ve dönüş değeri için kullanılan hisse senedi sınıfı**</span><span class="sxs-lookup"><span data-stu-id="e044d-245">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="e044d-246">**İstemci kodu dönüş değeri yok ve bir ASP.NET 4.5 async yöntemi bir karmaşık tür parametresi alan bir yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="e044d-246">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="e044d-247">**İstemci kodu dönüş değeri yok ve bir zaman uyumlu yöntemi bir karmaşık tür parametresi alan bir yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="e044d-247">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="e044d-248">`Invoke` Yöntemi zaman uyumsuz olarak yürütür ve döndürür bir `Task` nesnesi.</span><span class="sxs-lookup"><span data-stu-id="e044d-248">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="e044d-249">Belirtmediyseniz `await` veya `.Wait()`, kodun sonraki satırında, çağırmayı yöntemi yürütülmesi tamamlandı önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="e044d-249">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="e044d-250">Bağlantı ömür olayları işleme</span><span class="sxs-lookup"><span data-stu-id="e044d-250">How to handle connection lifetime events</span></span>

<span data-ttu-id="e044d-251">SignalR aşağıdaki bağlantıyı işleyebilir ömür olayları sağlar:</span><span class="sxs-lookup"><span data-stu-id="e044d-251">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="e044d-252">`Received`: Herhangi bir veri bağlantısı alındığında oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="e044d-252">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="e044d-253">Alınan veriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="e044d-253">Provides the received data.</span></span>
- <span data-ttu-id="e044d-254">`ConnectionSlow`: İstemci yavaş veya sık bırakma bir bağlantı algıladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e044d-254">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="e044d-255">`Reconnecting`: Temel aktarımı yeniden bağlanmayı başladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e044d-255">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="e044d-256">`Reconnected`: Temel aktarımı bağlandı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="e044d-256">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="e044d-257">`StateChanged`: Bağlantı durumu değiştiğinde oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="e044d-257">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="e044d-258">Eski durum ve yeni durum sağlar.</span><span class="sxs-lookup"><span data-stu-id="e044d-258">Provides the old state and the new state.</span></span> <span data-ttu-id="e044d-259">Durum değerleri bağlantısı hakkında bilgi için bkz [ConnectionState numaralandırma](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="e044d-259">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="e044d-260">`Closed`: Bağlantı kesildi tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="e044d-260">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="e044d-261">Örneğin, önemli değildir ancak aralıklı bağlantısı sorunlarına neden hataları için uyarı iletileri görüntülemek istiyorsanız, gibi yavaşlığı veya sık bağlantı, bırakarak işlemek `ConnectionSlow` olay.</span><span class="sxs-lookup"><span data-stu-id="e044d-261">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="e044d-262">Daha fazla bilgi için bkz: [anlama ve SignalR bağlantısı ömrü olaylarını işleme](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="e044d-262">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="e044d-263">Hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="e044d-263">How to handle errors</span></span>

<span data-ttu-id="e044d-264">Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, SignalR sonra bir hata döndürür özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir.</span><span class="sxs-lookup"><span data-stu-id="e044d-264">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="e044d-265">Örneğin, bir çağrı varsa `newContosoChatMessage` başarısız, hata nesnesindeki hata iletisini içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" üretim istemciler için ayrıntılı hata iletileri için ayrıntılı hata iletileri etkinleştirmek isteyip istemediğinizi ancak güvenlik nedeniyle önerilmez gönderme sorun giderme amacıyla, aşağıdaki kodu sunucuda kullanın.</span><span class="sxs-lookup"><span data-stu-id="e044d-265">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="e044d-266">SignalR başlatır hataları işlemek için bir işleyici ekleyebilirsiniz `Error` değerinin bağlantı nesnesindeki olay.</span><span class="sxs-lookup"><span data-stu-id="e044d-266">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="e044d-267">Yöntem çağrılarını hataları işlemek için bir try-catch bloğu içinde kodu alın.</span><span class="sxs-lookup"><span data-stu-id="e044d-267">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="e044d-268">İstemci-tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="e044d-268">How to enable client-side logging</span></span>

<span data-ttu-id="e044d-269">İstemci-tarafı günlük kaydını etkinleştirmek için ayarlanmış `TraceLevel` ve `TraceWriter` bağlantı nesne üzerindeki özellikleri.</span><span class="sxs-lookup"><span data-stu-id="e044d-269">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="e044d-270">WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod</span><span class="sxs-lookup"><span data-stu-id="e044d-270">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="e044d-271">Sunucu çağırabilirsiniz istemci yöntemleri tanımlamak için daha önce gösterilen kod örnekleri WinRT istemcileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e044d-271">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="e044d-272">Aşağıdaki örnekler, WPF, Silverlight ve konsol uygulaması istemciler için eşdeğer kodunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="e044d-272">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="e044d-273">Parametresiz yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e044d-273">Methods without parameters</span></span>

<span data-ttu-id="e044d-274">**WPF istemci kodunu parametresiz sunucusundan adlı yöntemi**</span><span class="sxs-lookup"><span data-stu-id="e044d-274">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="e044d-275">**Parametresiz sunucusundan adlı bir yöntem için Silverlight istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="e044d-275">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="e044d-276">**Parametresiz sunucusundan konsol uygulaması istemci kodu yöntemi için çağrılır**</span><span class="sxs-lookup"><span data-stu-id="e044d-276">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="e044d-277">Parametre türleri belirtme parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e044d-277">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="e044d-278">**WPF istemci kodu bir yöntem için parametre sunucusuyla çağrılır**</span><span class="sxs-lookup"><span data-stu-id="e044d-278">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="e044d-279">**Bir parametre olan sunucudan adlı bir yöntem için Silverlight istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="e044d-279">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="e044d-280">**Konsol uygulaması istemci kodu bir yöntem için parametre sunucusuyla çağrılır**</span><span class="sxs-lookup"><span data-stu-id="e044d-280">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="e044d-281">Dinamik nesneler parametre belirterek parametrelerle yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e044d-281">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="e044d-282">**Dinamik Nesne parametresi için kullanarak, bir parametre olan sunucudan adlı bir yöntem için WPF istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="e044d-282">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="e044d-283">**Dinamik Nesne parametresi için kullanarak, bir parametre olan sunucudan adlı bir yöntem için Silverlight istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="e044d-283">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="e044d-284">**Sunucu parametresi için bir dinamik nesnesi kullanılarak bir parametre ile bir yöntem için konsol uygulaması istemci kodu çağrılır**</span><span class="sxs-lookup"><span data-stu-id="e044d-284">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
