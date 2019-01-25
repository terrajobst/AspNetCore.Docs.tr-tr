---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-net-client
title: ASP.NET SignalR Hubs API Kılavuzu - .NET istemcisi (C#) | Microsoft Docs
author: bradygaster
description: Bu belge, SignalR sürüm 2 (WinRT) Windows Store, WPF, Silverlight ve simgeler gibi .NET istemcileri için hub'ları API kullanarak bir giriş sağlar...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: 6d02d9f7-94e5-4140-9f51-5a6040f274f6
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-net-client
msc.type: authoredcontent
ms.openlocfilehash: df12193b6ba3cc8b080047276ed7174583e7ff8a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837604"
---
<a name="aspnet-signalr-hubs-api-guide---net-client-c"></a><span data-ttu-id="62a0c-103">ASP.NET SignalR Hubs API Kılavuzu - .NET istemcisi (C#)</span><span class="sxs-lookup"><span data-stu-id="62a0c-103">ASP.NET SignalR Hubs API Guide - .NET Client (C#)</span></span>
====================

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="62a0c-104">Bu belge, SignalR sürüm 2 (WinRT) Windows Store, WPF, Silverlight ve konsol uygulamaları gibi .NET istemcileri için hub'ları API kullanmaya giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="62a0c-104">This document provides an introduction to using the Hubs API for SignalR version 2 in .NET clients, such as Windows Store (WinRT), WPF, Silverlight, and console applications.</span></span>
>
> <span data-ttu-id="62a0c-105">SignalR hub'ları API, bir sunucuya bağlanan istemcilerin ve istemcilerin sunucuya uzaktan yordam çağrısı (RPC) oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="62a0c-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="62a0c-106">Sunucu kodu, istemciler tarafından çağrılabilen yöntemleri tanımlamak ve bir istemcide çalışmasına yöntemler çağırır.</span><span class="sxs-lookup"><span data-stu-id="62a0c-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="62a0c-107">İstemci kodu sunucudan çağıran yöntemleri tanımlamak ve sunucu üzerinde çalışan yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="62a0c-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="62a0c-108">SignalR tüm istemci-sunucu tesisat sizin için üstlenir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="62a0c-109">SignalR kalıcı bağlantı adlı bir alt düzey API'si de sunar.</span><span class="sxs-lookup"><span data-stu-id="62a0c-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="62a0c-110">SignalR hub'ları ve kalıcı bağlantılar için giriş veya tam bir SignalR uygulamanın nasıl oluşturulacağını gösteren bir öğretici için bkz: [SignalR çalışmaya başlama -](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="62a0c-110">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="62a0c-111">Bu konu başlığında kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="62a0c-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="62a0c-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="62a0c-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="62a0c-113">.NET 4.5</span></span>
> - <span data-ttu-id="62a0c-114">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="62a0c-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="62a0c-115">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="62a0c-116">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="62a0c-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="62a0c-117">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="62a0c-117">Questions and comments</span></span>
>
> <span data-ttu-id="62a0c-118">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="62a0c-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="62a0c-119">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="62a0c-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="62a0c-120">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="62a0c-120">Overview</span></span>

<span data-ttu-id="62a0c-121">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="62a0c-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="62a0c-122">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="62a0c-122">Client Setup</span></span>](#clientsetup)
- [<span data-ttu-id="62a0c-123">Nasıl bir bağlantı kurmak için</span><span class="sxs-lookup"><span data-stu-id="62a0c-123">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="62a0c-124">Silverlight istemcileri etki alanları arası bağlantılar</span><span class="sxs-lookup"><span data-stu-id="62a0c-124">Cross-domain connections from Silverlight clients</span></span>](#slcrossdomain)
- [<span data-ttu-id="62a0c-125">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="62a0c-125">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="62a0c-126">WPF istemcileri en fazla eş zamanlı bağlantı sayısını ayarlama</span><span class="sxs-lookup"><span data-stu-id="62a0c-126">How to set the maximum number of concurrent connections in WPF clients</span></span>](#maxconnections)
    - [<span data-ttu-id="62a0c-127">Sorgu dizesi parametrelerini belirtme</span><span class="sxs-lookup"><span data-stu-id="62a0c-127">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="62a0c-128">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="62a0c-128">How to specify the transport method</span></span>](#transport)
    - [<span data-ttu-id="62a0c-129">HTTP üst bilgilerini belirtme</span><span class="sxs-lookup"><span data-stu-id="62a0c-129">How to specify HTTP headers</span></span>](#httpheaders)
    - [<span data-ttu-id="62a0c-130">İstemci sertifikalarını belirtmek nasıl</span><span class="sxs-lookup"><span data-stu-id="62a0c-130">How to specify client certificates</span></span>](#clientcertificate)
- [<span data-ttu-id="62a0c-131">Hub proxy oluşturma</span><span class="sxs-lookup"><span data-stu-id="62a0c-131">How to create the Hub proxy</span></span>](#proxy)
- [<span data-ttu-id="62a0c-132">Sunucu çağıran istemciye yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="62a0c-132">How to define methods on the client that the server can call</span></span>](#callclient)

    - [<span data-ttu-id="62a0c-133">Parametresiz yöntemleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-133">Methods without parameters</span></span>](#clientmethodswithoutparms)
    - [<span data-ttu-id="62a0c-134">Parametre türleri belirtme parametrelere sahip yöntemleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-134">Methods with parameters, specifying parameter types</span></span>](#clientmethodswithparmtypes)
    - [<span data-ttu-id="62a0c-135">Parametreler için dinamik nesneleri belirterek parametrelere sahip yöntemleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-135">Methods with parameters, specifying dynamic objects for the parameters</span></span>](#clientmethodswithdynamparms)
    - [<span data-ttu-id="62a0c-136">Bir işleyici kaldırma</span><span class="sxs-lookup"><span data-stu-id="62a0c-136">How to remove a handler</span></span>](#removehandler)
- [<span data-ttu-id="62a0c-137">İstemciden sunucu yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="62a0c-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="62a0c-138">Bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="62a0c-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="62a0c-139">Hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="62a0c-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="62a0c-140">İstemci tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="62a0c-140">How to enable client-side logging</span></span>](#logging)
- [<span data-ttu-id="62a0c-141">WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod</span><span class="sxs-lookup"><span data-stu-id="62a0c-141">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>](#wpfsl)

<span data-ttu-id="62a0c-142">Örnek .NET istemci projeleri için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="62a0c-142">For a sample .NET client projects, see the following resources:</span></span>

- <span data-ttu-id="62a0c-143">[Gustavo armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) GitHub.com (WinRT, Silverlight, konsol uygulaması örnekleri) üzerinde.</span><span class="sxs-lookup"><span data-stu-id="62a0c-143">[gustavo-armenta / SignalR-Samples](https://github.com/gustavo-armenta/SignalR-Samples) on GitHub.com (WinRT, Silverlight, console app examples).</span></span>
- <span data-ttu-id="62a0c-144">[DamianEdwards / SignalR MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) üzerinde GitHub.com (WPF örnek).</span><span class="sxs-lookup"><span data-stu-id="62a0c-144">[DamianEdwards / SignalR-MoveShapeDemo / MoveShape.Desktop](https://github.com/DamianEdwards/SignalR-MoveShapeDemo/tree/master/MoveShape/MoveShape.Desktop) on GitHub.com (WPF example).</span></span>
- <span data-ttu-id="62a0c-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) üzerinde GitHub.com (konsol uygulaması örnek).</span><span class="sxs-lookup"><span data-stu-id="62a0c-145">[SignalR / Microsoft.AspNet.SignalR.Client.Samples](https://github.com/SignalR/SignalR/tree/master/samples/Microsoft.AspNet.SignalR.Client.Samples) on GitHub.com (Console app example).</span></span>

<span data-ttu-id="62a0c-146">Sunucu veya JavaScript istemcilerinin program hakkında daha fazla belge için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="62a0c-146">For documentation on how to program the server or JavaScript clients, see the following resources:</span></span>

- [<span data-ttu-id="62a0c-147">SignalR hub API Kılavuzu - sunucu</span><span class="sxs-lookup"><span data-stu-id="62a0c-147">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="62a0c-148">SignalR hub API Kılavuzu - JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="62a0c-148">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)

<span data-ttu-id="62a0c-149">API başvuru konularına bağlar API .NET 4.5 sürümü var.</span><span class="sxs-lookup"><span data-stu-id="62a0c-149">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="62a0c-150">.NET 4 kullanıyorsanız, bkz. [API konuları .NET 4 sürümünü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="62a0c-150">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="62a0c-151">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="62a0c-151">Client setup</span></span>

<span data-ttu-id="62a0c-152">Yükleme [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet paketini (değil [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) paketi).</span><span class="sxs-lookup"><span data-stu-id="62a0c-152">Install the [Microsoft.AspNet.SignalR.Client](http://nuget.org/packages/Microsoft.AspNet.SignalR.Client) NuGet package (not the [Microsoft.AspNet.SignalR](http://nuget.org/packages/microsoft.aspnet.signalr) package).</span></span> <span data-ttu-id="62a0c-153">Bu paket, .NET 4 ve .NET 4.5 için WinRT, Silverlight, WPF, konsol uygulaması ve Windows Phone istemcileri destekler.</span><span class="sxs-lookup"><span data-stu-id="62a0c-153">This package supports WinRT, Silverlight, WPF, console application, and Windows Phone clients, for both .NET 4 and .NET 4.5.</span></span>

<span data-ttu-id="62a0c-154">İstemcide sahip SignalR sürümü sunucuda yüklü sürümden farklı ise, SignalR genellikle farkı uyum sağlamak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="62a0c-154">If the version of SignalR that you have on the client is different from the version that you have on the server, SignalR is often able to adapt to the difference.</span></span> <span data-ttu-id="62a0c-155">Örneğin, SignalR sürüm 2 çalıştıran bir sunucuya 1.1.x yüklü olan istemciler ve bunun yanı sıra sürüm 2'in yüklü olan istemcileri destekler.</span><span class="sxs-lookup"><span data-stu-id="62a0c-155">For example, a server running SignalR version 2 will support clients that have 1.1.x installed as well as clients that have version 2 installed.</span></span> <span data-ttu-id="62a0c-156">Sunucu sürümünde ve istemci sürümü arasındaki fark çok fazla olabilir veya istemcinin sunucudan daha yeniyse, SignalR oluşturur bir `InvalidOperationException` istemci bağlantısı kurmaya çalıştığında bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="62a0c-156">If the difference between the version on the server and the version on the client is too great, or if the client is newer than the server, SignalR throws an `InvalidOperationException` exception when the client tries to establish a connection.</span></span> <span data-ttu-id="62a0c-157">Hata iletisi "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span><span class="sxs-lookup"><span data-stu-id="62a0c-157">The error message is "`You are using a version of the client that isn't compatible with the server. Client version X.X, server version X.X`".</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="62a0c-158">Nasıl bir bağlantı kurmak için</span><span class="sxs-lookup"><span data-stu-id="62a0c-158">How to establish a connection</span></span>

<span data-ttu-id="62a0c-159">Bir bağlantı kurabilmesi için önce oluşturmak sahip bir `HubConnection` nesnesi ve bir ara sunucu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="62a0c-159">Before you can establish a connection, you have to create a `HubConnection` object and create a proxy.</span></span> <span data-ttu-id="62a0c-160">Bağlantı kurmak için çağrı `Start` metodunda `HubConnection` nesne.</span><span class="sxs-lookup"><span data-stu-id="62a0c-160">To establish the connection, call the `Start` method on the `HubConnection` object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample1.cs?highlight=1,4)]

> [!NOTE]
> <span data-ttu-id="62a0c-161">JavaScript istemciler için çağırmadan önce en az bir olay işleyicisi kaydetmek zorunda `Start` bağlantı kurmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-161">For JavaScript clients you have to register at least one event handler before calling the `Start` method to establish the connection.</span></span> <span data-ttu-id="62a0c-162">Bu, .NET istemcileri için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-162">This is not necessary for .NET clients.</span></span> <span data-ttu-id="62a0c-163">JavaScript istemciler için oluşturulan proxy kodu otomatik olarak mevcut tüm hub'ları proxy'lerini sunucuda oluşturur ve bir işleyici kaydetmektir hangi hub'ları nasıl belirttiğiniz istemcinizi kullanmayı düşünüyor.</span><span class="sxs-lookup"><span data-stu-id="62a0c-163">For JavaScript clients, the generated proxy code automatically creates proxies for all Hubs that exist on the server, and registering a handler is how you indicate which Hubs your client intends to use.</span></span> <span data-ttu-id="62a0c-164">Ancak SignalR kabul eder, proxy için oluşturduğunuz hub'ı kullanacak şekilde için .NET istemci Hub proxy el ile oluşturduğunuz.</span><span class="sxs-lookup"><span data-stu-id="62a0c-164">But for a .NET client you create Hub proxies manually, so SignalR assumes that you will be using any Hub that you create a proxy for.</span></span>


<span data-ttu-id="62a0c-165">Varsayılan örnek kodu kullanır "/ signalr" SignalR hizmetinize bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="62a0c-165">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="62a0c-166">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="62a0c-166">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="62a0c-167">`Start` Yöntemi zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="62a0c-167">The `Start` method executes asynchronously.</span></span> <span data-ttu-id="62a0c-168">Bağlantı kurulduktan sonra sonraki kod satırlarını kadar yürütmek yoksa emin olmak için `await` ASP.NET 4.5 zaman uyumsuz yönteminde veya `.Wait()` zaman uyumlu bir yöntem içinde.</span><span class="sxs-lookup"><span data-stu-id="62a0c-168">To make sure that subsequent lines of code don't execute until after the connection is established, use `await` in an ASP.NET 4.5 asynchronous method or `.Wait()` in a synchronous method.</span></span> <span data-ttu-id="62a0c-169">Kullanmayın `.Wait()` bir WinRT istemcisinde.</span><span class="sxs-lookup"><span data-stu-id="62a0c-169">Don't use `.Wait()` in a WinRT client.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample2.cs?highlight=1)]

[!code-css[Main](hubs-api-guide-net-client/samples/sample3.css?highlight=1)]

<a id="slcrossdomain"></a>

### <a name="cross-domain-connections-from-silverlight-clients"></a><span data-ttu-id="62a0c-170">Silverlight istemcileri etki alanları arası bağlantılar</span><span class="sxs-lookup"><span data-stu-id="62a0c-170">Cross-domain connections from Silverlight clients</span></span>

<span data-ttu-id="62a0c-171">Silverlight istemcilerden etki alanları arası bağlantıları etkinleştirme hakkında daha fazla bilgi için bkz: [bir hizmet üzerinden etki alanı sınırlarında kullanılabilir hale getirme](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span><span class="sxs-lookup"><span data-stu-id="62a0c-171">For information about how to enable cross-domain connections from Silverlight clients, see [Making a Service Available Across Domain Boundaries](https://msdn.microsoft.com/library/cc197955(v=vs.95).aspx).</span></span>

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="62a0c-172">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="62a0c-172">How to configure the connection</span></span>

<span data-ttu-id="62a0c-173">Bir bağlantı kurmadan önce aşağıdaki seçeneklerden birini belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="62a0c-173">Before you establish a connection, you can specify any of the following options:</span></span>

- <span data-ttu-id="62a0c-174">Eş zamanlı bağlantı sayısı sınırı.</span><span class="sxs-lookup"><span data-stu-id="62a0c-174">Concurrent connections limit.</span></span>
- <span data-ttu-id="62a0c-175">Dize parametreleri sorgulayın.</span><span class="sxs-lookup"><span data-stu-id="62a0c-175">Query string parameters.</span></span>
- <span data-ttu-id="62a0c-176">Taşıma yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-176">The transport method.</span></span>
- <span data-ttu-id="62a0c-177">HTTP üstbilgileri.</span><span class="sxs-lookup"><span data-stu-id="62a0c-177">HTTP headers.</span></span>
- <span data-ttu-id="62a0c-178">İstemci sertifikaları.</span><span class="sxs-lookup"><span data-stu-id="62a0c-178">Client certificates.</span></span>

<a id="maxconnections"></a>

### <a name="how-to-set-the-maximum-number-of-concurrent-connections-in-wpf-clients"></a><span data-ttu-id="62a0c-179">WPF istemcileri en fazla eş zamanlı bağlantı sayısını ayarlama</span><span class="sxs-lookup"><span data-stu-id="62a0c-179">How to set the maximum number of concurrent connections in WPF clients</span></span>

<span data-ttu-id="62a0c-180">WPF istemcileri 2 varsayılan değerini eşzamanlı bağlantıların maksimum sayısını artırmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-180">In WPF clients, you might have to increase the maximum number of concurrent connections from its default value of 2.</span></span> <span data-ttu-id="62a0c-181">Önerilen değer 10'dur.</span><span class="sxs-lookup"><span data-stu-id="62a0c-181">The recommended value is 10.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample4.cs?highlight=4)]

<span data-ttu-id="62a0c-182">Daha fazla bilgi için [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span><span class="sxs-lookup"><span data-stu-id="62a0c-182">For more information, see [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx).</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="62a0c-183">Sorgu dizesi parametrelerini belirtme</span><span class="sxs-lookup"><span data-stu-id="62a0c-183">How to specify query string parameters</span></span>

<span data-ttu-id="62a0c-184">İstemci bağlandığında sunucuya veri göndermek istiyorsanız, bağlantı nesnesi için sorgu dizesi parametreleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="62a0c-184">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="62a0c-185">Aşağıdaki örnek bir sorgu dizesi parametresi istemci kodu ayarlama işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-185">The following example shows how to set a query string parameter in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample5.cs)]

<span data-ttu-id="62a0c-186">Aşağıdaki örnekte, sunucu kodu bir sorgu dizesi parametresi okunacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-186">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample6.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="62a0c-187">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="62a0c-187">How to specify the transport method</span></span>

<span data-ttu-id="62a0c-188">Bağlama işleminin bir parçası, SignalR istemci ile sunucu hem sunucu hem de istemci tarafından desteklenen en iyi aktarım belirlemek için normalde görüşür.</span><span class="sxs-lookup"><span data-stu-id="62a0c-188">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="62a0c-189">Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, bu anlaşma işlemi devre dışı bırakabilir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-189">If you already know which transport you want to use, you can bypass this negotiation process.</span></span> <span data-ttu-id="62a0c-190">Aktarım yöntemi belirtmek için başlangıç yöntemine bir transport nesnesi içinde geçirin.</span><span class="sxs-lookup"><span data-stu-id="62a0c-190">To specify the transport method, pass in a transport object to the Start method.</span></span> <span data-ttu-id="62a0c-191">Aşağıdaki örnek, aktarım yöntemi istemci kodu belirtmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-191">The following example shows how to specify the transport method in client code.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample7.cs?highlight=4)]

<span data-ttu-id="62a0c-192">[Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) ad alanı taşıma belirtmek için kullanabileceğiniz aşağıdaki sınıfları içerir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-192">The [Microsoft.AspNet.SignalR.Client.Transports](https://msdn.microsoft.com/library/jj918090(v=vs.111).aspx) namespace includes the following classes that you can use to specify the transport.</span></span>

- <span data-ttu-id="62a0c-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="62a0c-193">[LongPollingTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.longpollingtransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="62a0c-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span><span class="sxs-lookup"><span data-stu-id="62a0c-194">[ServerSentEventsTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.serversenteventstransport(v=vs.111).aspx)</span></span>
- <span data-ttu-id="62a0c-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (kullanılabilir. yalnızca, .NET 4.5 hem sunucu hem de istemci kullandığınızda)</span><span class="sxs-lookup"><span data-stu-id="62a0c-195">[WebSocketTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.websockettransport(v=vs.111).aspx) (Available only when both server and client use .NET 4.5.)</span></span>
- <span data-ttu-id="62a0c-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (istemci ve sunucu tarafından desteklenen en iyi aktarım otomatik olarak seçer.</span><span class="sxs-lookup"><span data-stu-id="62a0c-196">[AutoTransport](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.transports.autotransport(v=vs.111).aspx) (Automatically chooses the best transport that is supported by both the client and the server.</span></span> <span data-ttu-id="62a0c-197">Varsayılan aktarım budur.</span><span class="sxs-lookup"><span data-stu-id="62a0c-197">This is the default transport.</span></span> <span data-ttu-id="62a0c-198">Bu konuda geçirme `Start` yöntemi her şeyi geçmiyor aynı etkiye sahiptir.)</span><span class="sxs-lookup"><span data-stu-id="62a0c-198">Passing this in to the `Start` method has the same effect as not passing in anything.)</span></span>

<span data-ttu-id="62a0c-199">Yalnızca tarayıcı tarafından kullanıldığından ForeverFrame taşıma bu listede dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="62a0c-199">The ForeverFrame transport is not included in this list because it is used only by browsers.</span></span>

<span data-ttu-id="62a0c-200">Sunucu kodu aktarım yöntemi denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - bağlam özelliği istemci hakkında bilgi almak nasıl](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="62a0c-200">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="62a0c-201">Aktarım ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarım ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="62a0c-201">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="httpheaders"></a>

### <a name="how-to-specify-http-headers"></a><span data-ttu-id="62a0c-202">HTTP üst bilgilerini belirtme</span><span class="sxs-lookup"><span data-stu-id="62a0c-202">How to specify HTTP headers</span></span>

<span data-ttu-id="62a0c-203">HTTP üst bilgilerini ayarlayacak şekilde kullanmak `Headers` bağlantı nesnesindeki özelliği.</span><span class="sxs-lookup"><span data-stu-id="62a0c-203">To set HTTP headers, use the `Headers` property on the connection object.</span></span> <span data-ttu-id="62a0c-204">Aşağıdaki örnek, bir HTTP üstbilgisi Ekle gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-204">The following example shows how to add an HTTP header.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample8.cs?highlight=2)]

<a id="clientcertificate"></a>

### <a name="how-to-specify-client-certificates"></a><span data-ttu-id="62a0c-205">İstemci sertifikalarını belirtmek nasıl</span><span class="sxs-lookup"><span data-stu-id="62a0c-205">How to specify client certificates</span></span>

<span data-ttu-id="62a0c-206">İstemci sertifikaları eklemek için `AddClientCertificate` bağlantı nesnesi üzerinde yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-206">To add client certificates, use the `AddClientCertificate` method on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample9.cs?highlight=2)]

<a id="proxy"></a>

## <a name="how-to-create-the-hub-proxy"></a><span data-ttu-id="62a0c-207">Hub proxy oluşturma</span><span class="sxs-lookup"><span data-stu-id="62a0c-207">How to create the Hub proxy</span></span>

<span data-ttu-id="62a0c-208">Sunucudan bir Hub çağıran istemci üzerinde yöntemleri tanımlamak için ve sunucudaki Hub yöntemlerini çağırma, Hub için bir proxy çağırarak oluşturma `CreateHubProxy` bağlantı nesnesindeki.</span><span class="sxs-lookup"><span data-stu-id="62a0c-208">In order to define methods on the client that a Hub can call from the server, and to invoke methods on a Hub at the server, create a proxy for the Hub by calling `CreateHubProxy` on the connection object.</span></span> <span data-ttu-id="62a0c-209">Dize, için geçirdiğiniz `CreateHubProxy` Hub sınıfınıza adını veya tarafından belirtilen adı `HubName` sunucuda kullanılan bir özniteliği.</span><span class="sxs-lookup"><span data-stu-id="62a0c-209">The string you pass in to `CreateHubProxy` is the name of your Hub class, or the name specified by the `HubName` attribute if one was used on the server.</span></span> <span data-ttu-id="62a0c-210">Ad eşleştirme büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="62a0c-210">Name matching is case-insensitive.</span></span>

<span data-ttu-id="62a0c-211">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="62a0c-211">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample10.cs?highlight=1)]

<span data-ttu-id="62a0c-212">**Hub sınıfı için istemci proxy oluşturma**</span><span class="sxs-lookup"><span data-stu-id="62a0c-212">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample11.cs?highlight=2)]

<span data-ttu-id="62a0c-213">Hub sınıfınıza tasarlamanız, bir `HubName` özniteliği, bu adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="62a0c-213">If you decorate your Hub class with a `HubName` attribute, use that name.</span></span>

<span data-ttu-id="62a0c-214">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="62a0c-214">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample12.cs)]

<span data-ttu-id="62a0c-215">**Hub sınıfı için istemci proxy oluşturma**</span><span class="sxs-lookup"><span data-stu-id="62a0c-215">**Create client proxy for the Hub class**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample13.cs?highlight=2)]

<span data-ttu-id="62a0c-216">Eğer `HubConnection.CreateHubProxy` birden çok kez ile aynı `hubName`, aynı önbelleğe alma `IHubProxy` nesne.</span><span class="sxs-lookup"><span data-stu-id="62a0c-216">If you call `HubConnection.CreateHubProxy` multiple times with the same `hubName`, you get the same cached `IHubProxy` object.</span></span>

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="62a0c-217">Sunucu çağıran istemciye yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="62a0c-217">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="62a0c-218">Proxy sunucu çağırabilen bir yöntemi tanımlamak için kullanmak `On` bir olay işleyicisi kaydetmek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-218">To define a method that the server can call, use the proxy's `On` method to register an event handler.</span></span>

<span data-ttu-id="62a0c-219">Yöntem adı ile eşleşen büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="62a0c-219">Method name matching is case-insensitive.</span></span> <span data-ttu-id="62a0c-220">Örneğin, `Clients.All.UpdateStockPrice` sunucuda yürütülür `updateStockPrice`, `updatestockprice`, veya `UpdateStockPrice` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="62a0c-220">For example, `Clients.All.UpdateStockPrice` on the server will execute `updateStockPrice`, `updatestockprice`, or `UpdateStockPrice` on the client.</span></span>

<span data-ttu-id="62a0c-221">Farklı istemci platformları UI'yi güncellemeye yöntemine kodu yazarsınız nasıl farklı gereksinimlere sahip.</span><span class="sxs-lookup"><span data-stu-id="62a0c-221">Different client platforms have different requirements for how you write method code to update the UI.</span></span> <span data-ttu-id="62a0c-222">Gösterilen WinRT (Windows Store .NET) istemciler için verilebilir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-222">The examples shown are for WinRT (Windows Store .NET) clients.</span></span> <span data-ttu-id="62a0c-223">İçinde WPF, Silverlight ve konsol uygulaması örnekleri verilmiştir [ayrı bir bölümde bu konunun ilerleyen bölümlerinde](#wpfsl).</span><span class="sxs-lookup"><span data-stu-id="62a0c-223">WPF, Silverlight, and console application examples are provided in [a separate section later in this topic](#wpfsl).</span></span>

<a id="clientmethodswithoutparms"></a>

### <a name="methods-without-parameters"></a><span data-ttu-id="62a0c-224">Parametresiz yöntemleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-224">Methods without parameters</span></span>

<span data-ttu-id="62a0c-225">İşleme yöntemi parametrelerine sahip değildir, genel olmayan aşırı yüklemesini kullanın. `On` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="62a0c-225">If the method you're handling does not have parameters, use the non-generic overload of the `On` method:</span></span>

<span data-ttu-id="62a0c-226">**Sunucu kodu parametresiz istemci yöntemi çağırma**</span><span class="sxs-lookup"><span data-stu-id="62a0c-226">**Server code calling client method without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample14.cs?highlight=5)]

<span data-ttu-id="62a0c-227">**WinRT istemci kodu yöntemi için çağrılır parametresiz sunucusundan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="62a0c-227">**WinRT Client code for method called from server without parameters ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample15.cs)]

<a id="clientmethodswithparmtypes"></a>

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="62a0c-228">Parametre türleri belirtme parametrelere sahip yöntemleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-228">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="62a0c-229">İşleme yöntemi parametrelere sahipse, parametre türleri genel türlerini belirtmek `On` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-229">If the method you're handling has parameters, specify the types of the parameters as the generic types of the `On` method.</span></span> <span data-ttu-id="62a0c-230">Genel aşırı yükleme `On` yöntemi en fazla 8 parametreleri (Windows Phone 7 4) belirtmenize olanak verir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-230">There are generic overloads of the `On` method to enable you to specify up to 8 parameters (4 on Windows Phone 7).</span></span> <span data-ttu-id="62a0c-231">Aşağıdaki örnekte, bir parametre için gönderilen `UpdateStockPrice` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-231">In the following example, one parameter is sent to the `UpdateStockPrice` method.</span></span>

<span data-ttu-id="62a0c-232">**Sunucu kodu istemci yöntemi parametresi ile çağırılıyor**</span><span class="sxs-lookup"><span data-stu-id="62a0c-232">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample16.cs?highlight=3)]

<span data-ttu-id="62a0c-233">**Parametresi için kullanılan stok sınıfı**</span><span class="sxs-lookup"><span data-stu-id="62a0c-233">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample17.cs)]

<span data-ttu-id="62a0c-234">**WinRT istemci kodu için bir yöntem olarak adlandırılan bir parametresi olan bir sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="62a0c-234">**WinRT Client code for a method called from server with a parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample18.cs?highlight=1,5)]

<a id="clientmethodswithdynamparms"></a>

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="62a0c-235">Parametreler için dinamik nesneleri belirterek parametrelere sahip yöntemleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-235">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="62a0c-236">Alternatif genel tür parametrelerini belirtme olarak `On` yöntemi parametrelerini dinamik nesneler olarak belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="62a0c-236">As an alternative to specifying parameters as generic types of the `On` method, you can specify parameters as dynamic objects:</span></span>

<span data-ttu-id="62a0c-237">**Sunucu kodu istemci yöntemi parametresi ile çağırılıyor**</span><span class="sxs-lookup"><span data-stu-id="62a0c-237">**Server code calling client method with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample19.cs?highlight=3)]

<span data-ttu-id="62a0c-238">**Parametresi için kullanılan stok sınıfı**</span><span class="sxs-lookup"><span data-stu-id="62a0c-238">**The Stock class used for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample20.cs)]

<span data-ttu-id="62a0c-239">**Bir yöntem için WinRT istemci kodu adlı bir parametresiyle dinamik Nesne parametresi için kullanarak sunucudan ([WPF ve Silverlight Örnekleri bu konunun ilerleyen bölümlerinde bkz](#wpfsl))**</span><span class="sxs-lookup"><span data-stu-id="62a0c-239">**WinRT Client code for a method called from server with a parameter, using a dynamic object for the parameter ([see WPF and Silverlight examples later in this topic](#wpfsl))**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample21.cs?highlight=1,5)]

<a id="removehandler"></a>

### <a name="how-to-remove-a-handler"></a><span data-ttu-id="62a0c-240">Bir işleyici kaldırma</span><span class="sxs-lookup"><span data-stu-id="62a0c-240">How to remove a handler</span></span>

<span data-ttu-id="62a0c-241">Bir işleyici kaldırmak için arama, `Dispose` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-241">To remove a handler, call its `Dispose` method.</span></span>

<span data-ttu-id="62a0c-242">**Sunucudan adlı bir yöntem için istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="62a0c-242">**Client code for a method called from server**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample22.cs?highlight=1)]

<span data-ttu-id="62a0c-243">**İstemci kodu, işleyici kaldırmak için**</span><span class="sxs-lookup"><span data-stu-id="62a0c-243">**Client code to remove the handler**</span></span>

[!code-css[Main](hubs-api-guide-net-client/samples/sample23.css?highlight=1)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="62a0c-244">İstemciden sunucu yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="62a0c-244">How to call server methods from the client</span></span>

<span data-ttu-id="62a0c-245">Sunucu üzerinde bir yöntemi çağırmak için `Invoke` Hub proxy yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-245">To call a method on the server, use the `Invoke` method on the Hub proxy.</span></span>

<span data-ttu-id="62a0c-246">Sunucu yönteminin dönüş değeri varsa, genel olmayan aşırı yüklemesini kullanın `Invoke` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-246">If the server method has no return value, use the non-generic overload of the `Invoke` method.</span></span>

<span data-ttu-id="62a0c-247">**Dönüş değeri içeren bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="62a0c-247">**Server code for a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample24.cs?highlight=3)]

<span data-ttu-id="62a0c-248">**İstemci kodu, dönüş değeri olmayan bir yöntem çağırma**</span><span class="sxs-lookup"><span data-stu-id="62a0c-248">**Client code calling a method that has no return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="62a0c-249">Sunucu yönteminin dönüş değeri varsa, dönüş türü genel türü olarak belirtin. `Invoke` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="62a0c-249">If the server method has a return value, specify the return type as the generic type of the `Invoke` method.</span></span>

<span data-ttu-id="62a0c-250">**Bir değer döndürmez ve karmaşık tür parametre alan bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="62a0c-250">**Server code for a method that has a return value and takes a complex type parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="62a0c-251">**Stok sınıfı parametresi için kullanılan ve dönüş değeri**</span><span class="sxs-lookup"><span data-stu-id="62a0c-251">**The Stock class used for the parameter and return value**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample27.cs)]

<span data-ttu-id="62a0c-252">**İstemci kodu bir değer döndürmez ve bir ASP.NET 4.5 zaman uyumsuz yöntemde bir karmaşık tür parametre alan bir yöntem çağırma**</span><span class="sxs-lookup"><span data-stu-id="62a0c-252">**Client code calling a method that has a return value and takes a complex type parameter, in an ASP.NET 4.5 async method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample28.cs?highlight=1-2)]

<span data-ttu-id="62a0c-253">**İstemci kodu bir değer döndürmez ve zaman uyumlu bir yöntemde bir karmaşık tür parametre alan bir yöntem çağırma**</span><span class="sxs-lookup"><span data-stu-id="62a0c-253">**Client code calling a method that has a return value and takes a complex type parameter, in a synchronous method**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample29.cs?highlight=1-2)]

<span data-ttu-id="62a0c-254">`Invoke` Yöntemi zaman uyumsuz olarak yürütür ve döndürür bir `Task` nesne.</span><span class="sxs-lookup"><span data-stu-id="62a0c-254">The `Invoke` method executes asynchronously and returns a `Task` object.</span></span> <span data-ttu-id="62a0c-255">Belirtmezseniz `await` veya `.Wait()`, sonraki kod satırına, çağırma yöntemi yürütmeyi bitirmeden önce yürütülür.</span><span class="sxs-lookup"><span data-stu-id="62a0c-255">If you don't specify `await` or `.Wait()`, the next line of code will execute before the method that you invoke has finished executing.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="62a0c-256">Bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="62a0c-256">How to handle connection lifetime events</span></span>

<span data-ttu-id="62a0c-257">SignalR işleyebilirsiniz ömür olayları aşağıdaki bağlantı sağlar:</span><span class="sxs-lookup"><span data-stu-id="62a0c-257">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="62a0c-258">`Received`: Herhangi bir veri bağlantısı alındığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="62a0c-258">`Received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="62a0c-259">Alınan veriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="62a0c-259">Provides the received data.</span></span>
- <span data-ttu-id="62a0c-260">`ConnectionSlow`: İstemci yavaş veya sık bırakma bağlantı tespit ettiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="62a0c-260">`ConnectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="62a0c-261">`Reconnecting`: Temel alınan aktarımda yeniden bağlanmayı başladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="62a0c-261">`Reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="62a0c-262">`Reconnected`: Temel alınan aktarımda bağlandığınızda oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="62a0c-262">`Reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="62a0c-263">`StateChanged`: Bağlantı durumu değiştiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="62a0c-263">`StateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="62a0c-264">Eski durum ve yeni durum sağlar.</span><span class="sxs-lookup"><span data-stu-id="62a0c-264">Provides the old state and the new state.</span></span> <span data-ttu-id="62a0c-265">Durum değerleri bağlantısı hakkında bilgi için bkz [ConnectionState numaralandırma](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="62a0c-265">For information about connection state values see [ConnectionState Enumeration](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connectionstate(v=vs.111).aspx).</span></span>
- <span data-ttu-id="62a0c-266">`Closed`: Bağlantı kesildiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="62a0c-266">`Closed`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="62a0c-267">Örneğin, önemli değildir ancak aralıklı bağlantı sorunlarına neden bir hata için uyarı iletileri görüntülemek istiyorsanız, gibi yavaşlık ya da sık sık bağlantısı, bırakarak işlemek `ConnectionSlow` olay.</span><span class="sxs-lookup"><span data-stu-id="62a0c-267">For example, if you want to display warning messages for errors that are not fatal but cause intermittent connection problems, such as slowness or frequent dropping of the connection, handle the `ConnectionSlow` event.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample30.cs)]

<span data-ttu-id="62a0c-268">Daha fazla bilgi için [anlama ve signalr'da bağlantı ömrü olaylarını işleme](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="62a0c-268">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="62a0c-269">Hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="62a0c-269">How to handle errors</span></span>

<span data-ttu-id="62a0c-270">Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, bir hatanın ardından SignalR döndüren özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-270">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="62a0c-271">Örneğin, bir çağrı `newContosoChatMessage` başarısız, hata iletisi hata nesnesi içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" güvenlik nedeniyle, ayrıntılı hata iletileri için etkinleştirmek istiyorsanız ancak üretimde istemciler için ayrıntılı hata iletileri önerilmez gönderme sorun giderme amacıyla sunucu üzerinde aşağıdaki kodu kullanın.</span><span class="sxs-lookup"><span data-stu-id="62a0c-271">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample31.cs?highlight=2)]

<a id="handleerrors"></a>

<span data-ttu-id="62a0c-272">SignalR oluşturan hataları işlemek için bir işleyici ekleyebilirsiniz `Error` bağlantı nesnesindeki olay.</span><span class="sxs-lookup"><span data-stu-id="62a0c-272">To handle errors that SignalR raises, you can add a handler for the `Error` event on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample32.cs)]

<span data-ttu-id="62a0c-273">Yöntem çağrıları hatalarını işlemek için bir try-catch bloğu içinde kod alın.</span><span class="sxs-lookup"><span data-stu-id="62a0c-273">To handle errors from method invocations, wrap the code in a try-catch block.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample33.cs)]

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="62a0c-274">İstemci tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="62a0c-274">How to enable client-side logging</span></span>

<span data-ttu-id="62a0c-275">İstemci tarafı günlük kaydını etkinleştirmek için ayarlanmış `TraceLevel` ve `TraceWriter` bağlantı nesnesindeki özellikleri.</span><span class="sxs-lookup"><span data-stu-id="62a0c-275">To enable client-side logging, set the `TraceLevel` and `TraceWriter` properties on the connection object.</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample34.cs?highlight=2-3)]

<a id="wpfsl"></a>

## <a name="wpf-silverlight-and-console-application-code-samples-for-client-methods-that-the-server-can-call"></a><span data-ttu-id="62a0c-276">WPF, Silverlight ve konsol uygulaması sunucu çağırabilirsiniz istemci yöntemleri için örnek kod</span><span class="sxs-lookup"><span data-stu-id="62a0c-276">WPF, Silverlight, and console application code samples for client methods that the server can call</span></span>

<span data-ttu-id="62a0c-277">Sunucu çağırabilirsiniz istemci yöntemleri tanımlamak için daha önce gösterilen kod örnekleri, WinRT istemcileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-277">The code samples shown earlier for defining client methods that the server can call apply to WinRT clients.</span></span> <span data-ttu-id="62a0c-278">Aşağıdaki örnekler, WPF, Silverlight ve konsol uygulaması istemciler için eşdeğer kod gösterir.</span><span class="sxs-lookup"><span data-stu-id="62a0c-278">The following samples show the equivalent code for WPF, Silverlight, and console application clients.</span></span>

### <a name="methods-without-parameters"></a><span data-ttu-id="62a0c-279">Parametresiz yöntemleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-279">Methods without parameters</span></span>

<span data-ttu-id="62a0c-280">**Parametre olmadan sunucu WPF istemci kodu yöntemi için çağrılır**</span><span class="sxs-lookup"><span data-stu-id="62a0c-280">**WPF client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="62a0c-281">**Parametresiz sunucusundan Silverlight istemci kodu yöntemi için çağrılır**</span><span class="sxs-lookup"><span data-stu-id="62a0c-281">**Silverlight client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample36.cs?highlight=1)]

<span data-ttu-id="62a0c-282">**Parametre olmadan sunucu konsol uygulaması istemci kodu yöntemi için çağrılır**</span><span class="sxs-lookup"><span data-stu-id="62a0c-282">**Console application client code for method called from server without parameters**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample37.cs?highlight=1)]

### <a name="methods-with-parameters-specifying-the-parameter-types"></a><span data-ttu-id="62a0c-283">Parametre türleri belirtme parametrelere sahip yöntemleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-283">Methods with parameters, specifying the parameter types</span></span>

<span data-ttu-id="62a0c-284">**WPF istemci kodu bir yöntem için parametre sunucusuyla çağrılır**</span><span class="sxs-lookup"><span data-stu-id="62a0c-284">**WPF client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample38.cs?highlight=1,4)]

<span data-ttu-id="62a0c-285">**Sunucu parametresi olan bir yöntem için Silverlight istemci kodu çağrılır**</span><span class="sxs-lookup"><span data-stu-id="62a0c-285">**Silverlight client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample39.cs?highlight=1,5)]

<span data-ttu-id="62a0c-286">**Konsol uygulaması istemci kodu bir yöntem için parametre sunucusuyla çağrılır**</span><span class="sxs-lookup"><span data-stu-id="62a0c-286">**Console application client code for a method called from server with a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample40.cs?highlight=1-2)]

### <a name="methods-with-parameters-specifying-dynamic-objects-for-the-parameters"></a><span data-ttu-id="62a0c-287">Parametreler için dinamik nesneleri belirterek parametrelere sahip yöntemleri</span><span class="sxs-lookup"><span data-stu-id="62a0c-287">Methods with parameters, specifying dynamic objects for the parameters</span></span>

<span data-ttu-id="62a0c-288">**WPF sunucusundan dinamik Nesne parametresi için kullanarak, bir parametre olarak adlandırılan bir yöntem için istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="62a0c-288">**WPF client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample41.cs?highlight=1,4)]

<span data-ttu-id="62a0c-289">**Dinamik Nesne parametresi için kullanarak, bir parametre ile sunucusundan adlı bir yöntem için Silverlight istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="62a0c-289">**Silverlight client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample42.cs?highlight=1,5)]

<span data-ttu-id="62a0c-290">**Konsol uygulaması istemci kodu için bir yöntem parametresi için bir dinamik Nesne kullanarak, bir parametre sunucusuyla çağrılır**</span><span class="sxs-lookup"><span data-stu-id="62a0c-290">**Console application client code for a method called from server with a parameter, using a dynamic object for the parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-net-client/samples/sample43.cs?highlight=1-2)]
