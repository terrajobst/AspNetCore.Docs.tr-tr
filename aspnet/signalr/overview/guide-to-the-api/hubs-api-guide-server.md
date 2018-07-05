---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: ASP.NET SignalR Hubs API Kılavuzu - sunucu (C#) | Microsoft Docs
author: pfletcher
description: Bu belge, sunucu tarafı ASP.NET SignalR hub'ları API sürüm 2, SignalR için gösteren kod örnekleri ile programlamaya giriş sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c8814236495c3680ad648234f2d2507730f4f775
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382101"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="68298-103">ASP.NET SignalR Hubs API Kılavuzu - sunucu (C#)</span><span class="sxs-lookup"><span data-stu-id="68298-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>
====================
<span data-ttu-id="68298-104">tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="68298-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="68298-105">Bu belge, sunucu tarafı ASP.NET SignalR hub'ları API sürüm 2, SignalR için genel seçenekleri gösteren kod örnekleri ile programlamaya giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="68298-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="68298-106">SignalR hub'ları API, bir sunucuya bağlanan istemcilerin ve istemcilerin sunucuya uzaktan yordam çağrısı (RPC) oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="68298-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="68298-107">Sunucu kodu, istemciler tarafından çağrılabilen yöntemleri tanımlamak ve bir istemcide çalışmasına yöntemler çağırır.</span><span class="sxs-lookup"><span data-stu-id="68298-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="68298-108">İstemci kodu sunucudan çağıran yöntemleri tanımlamak ve sunucu üzerinde çalışan yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="68298-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="68298-109">SignalR tüm istemci-sunucu tesisat sizin için üstlenir.</span><span class="sxs-lookup"><span data-stu-id="68298-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="68298-110">SignalR kalıcı bağlantı adlı bir alt düzey API'si de sunar.</span><span class="sxs-lookup"><span data-stu-id="68298-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="68298-111">SignalR hub'ları ve kalıcı bağlantılar için bir giriş için bkz [SignalR 2 giriş](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="68298-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="68298-112">Bu konu başlığında kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="68298-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="68298-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="68298-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="68298-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="68298-114">.NET 4.5</span></span>
> - <span data-ttu-id="68298-115">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="68298-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="68298-116">Konu sürümleri</span><span class="sxs-lookup"><span data-stu-id="68298-116">Topic versions</span></span>
> 
> <span data-ttu-id="68298-117">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="68298-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="68298-118">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="68298-118">Questions and comments</span></span>
> 
> <span data-ttu-id="68298-119">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="68298-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="68298-120">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="68298-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="68298-121">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="68298-121">Overview</span></span>

<span data-ttu-id="68298-122">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="68298-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="68298-123">SignalR ara yazılım kaydetme</span><span class="sxs-lookup"><span data-stu-id="68298-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="68298-124">/Signalr URL'si</span><span class="sxs-lookup"><span data-stu-id="68298-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="68298-125">SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="68298-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="68298-126">Oluşturma ve Hub sınıflarını kullanma</span><span class="sxs-lookup"><span data-stu-id="68298-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="68298-127">Hub nesne yaşam süresi</span><span class="sxs-lookup"><span data-stu-id="68298-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="68298-128">JavaScript istemcilerinin Hub adları camel casing</span><span class="sxs-lookup"><span data-stu-id="68298-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="68298-129">Birden çok hub'ları</span><span class="sxs-lookup"><span data-stu-id="68298-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="68298-130">Kesin tür belirtilmiş hub'ları</span><span class="sxs-lookup"><span data-stu-id="68298-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="68298-131">İstemciler çağırabileceğiniz Hub sınıfı yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="68298-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="68298-132">JavaScript istemcilerinin yöntemi adları camel casing</span><span class="sxs-lookup"><span data-stu-id="68298-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="68298-133">Zaman zaman uyumsuz olarak çalıştırmak için</span><span class="sxs-lookup"><span data-stu-id="68298-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="68298-134">Aşırı yüklemeler tanımlama</span><span class="sxs-lookup"><span data-stu-id="68298-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="68298-135">Gelen hub yöntemi çağrılarına ilerleme durumunu bildirme</span><span class="sxs-lookup"><span data-stu-id="68298-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="68298-136">İstemci Hub sınıfı yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="68298-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="68298-137">Hangi istemcilerin seçerek RPC alırsınız</span><span class="sxs-lookup"><span data-stu-id="68298-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="68298-138">Yöntem adları için derleme zamanı doğrulama</span><span class="sxs-lookup"><span data-stu-id="68298-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="68298-139">Ad eşleştirme büyük küçük harf duyarsız yöntemi</span><span class="sxs-lookup"><span data-stu-id="68298-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="68298-140">Zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="68298-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="68298-141">Hub sınıftan grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="68298-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="68298-142">Ekleme ve kaldırma yöntemlerinin, zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="68298-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="68298-143">Grup üyeliği kalıcılığı</span><span class="sxs-lookup"><span data-stu-id="68298-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="68298-144">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="68298-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="68298-145">Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="68298-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="68298-146">OnConnected OnDisconnected ve OnReconnected olduğunda çağırılır</span><span class="sxs-lookup"><span data-stu-id="68298-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="68298-147">Arayan durumu değil doldurulur</span><span class="sxs-lookup"><span data-stu-id="68298-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="68298-148">Bağlam özelliği istemci bilgilerini alma</span><span class="sxs-lookup"><span data-stu-id="68298-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="68298-149">Nasıl durumu Hub sınıfına ve istemciler arasında geçirme</span><span class="sxs-lookup"><span data-stu-id="68298-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="68298-150">Hub sınıfında hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="68298-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="68298-151">İstemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetme</span><span class="sxs-lookup"><span data-stu-id="68298-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="68298-152">İstemci yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="68298-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="68298-153">Grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="68298-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="68298-154">İzlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="68298-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="68298-155">Hub ardışık düzeni özelleştirme</span><span class="sxs-lookup"><span data-stu-id="68298-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="68298-156">Program istemcilere hakkında daha fazla belge için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="68298-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="68298-157">SignalR hub API Kılavuzu - JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="68298-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="68298-158">SignalR hub API Kılavuzu - .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="68298-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="68298-159">SignalR 2 için sunucu bileşenlerini, yalnızca .NET 4.5 içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="68298-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="68298-160">.NET 4.0 çalıştıran sunucular, SignalR v1.x kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="68298-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="68298-161">SignalR ara yazılım kaydetme</span><span class="sxs-lookup"><span data-stu-id="68298-161">How to register SignalR middleware</span></span>

<span data-ttu-id="68298-162">İstemciler, Hub'ınıza bağlanmak için kullanacağı rota tanımlamak için çağrı `MapSignalR` uygulama başlatıldığında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="68298-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="68298-163">`MapSignalR` olan bir [genişletme yöntemi](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) için `OwinExtensions` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="68298-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="68298-164">Aşağıdaki örnek, bir OWIN başlangıç sınıfı kullanarak SignalR hub'ları rotaya tanımlamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="68298-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="68298-165">Bir ASP.NET MVC uygulaması için SignalR işlevselliği ekliyorsanız, SignalR rota diğer rotaların önce eklendiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="68298-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="68298-166">Daha fazla bilgi için [öğretici: SignalR 2 ve MVC 5 ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="68298-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="68298-167">/Signalr URL'si</span><span class="sxs-lookup"><span data-stu-id="68298-167">The /signalr URL</span></span>

<span data-ttu-id="68298-168">Varsayılan olarak, istemciler, Hub'ınıza bağlanmak için kullanacağı yönlendirme URL'si olan "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="68298-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="68298-169">(Bu URL için otomatik olarak oluşturulan JavaScript dosyası "/ signalr/hubs" URL ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="68298-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="68298-170">Oluşturulan proxy hakkında daha fazla bilgi için bkz: [SignalR Hubs API Kılavuzu - JavaScript istemcisi - oluşturulan proxy ve sizin için yaptığı](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="68298-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="68298-171">Bu temel URL için SignalR kullanılamaz hale olağandışı durumlar olabilir; Örneğin, projenizde adlı bir klasöre sahip *signalr* ve adını değiştirmek istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="68298-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="68298-172">Bu durumda, aşağıdaki örneklerde gösterildiği gibi temel URL'sini değiştirebilirsiniz (Değiştir "/ signalr" örnek kodda, istenen URL ile).</span><span class="sxs-lookup"><span data-stu-id="68298-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="68298-173">**URL'yi belirten sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="68298-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="68298-174">**JavaScript istemci URL'si (ile oluşturulan proxy) belirten bir kod**</span><span class="sxs-lookup"><span data-stu-id="68298-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="68298-175">**(Olmadan oluşturulan proxy) URL'sini belirtir, JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="68298-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="68298-176">**URL'sini belirtir, .NET istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="68298-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="68298-177">SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="68298-177">Configuring SignalR Options</span></span>

<span data-ttu-id="68298-178">Overloads biri `MapSignalR` yöntemini etkinleştirmek, özel bir URL, bir özel bağımlılık çözümleyiciyi ve aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="68298-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="68298-179">CORS veya JSONP tarayıcı istemcilerinden kullanan etki alanları arası çağrılar etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="68298-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="68298-180">Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantı `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="68298-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="68298-181">Varsa sayfasından `http://contoso.com` için bir bağlantı kurar `http://fabrikam.com/signalr`, diğer bir deyişle etki alanları arası bağlantı.</span><span class="sxs-lookup"><span data-stu-id="68298-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="68298-182">Güvenlik nedenleriyle, etki alanları arası bağlantıları varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="68298-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="68298-183">Daha fazla bilgi için [ASP.NET SignalR Hubs API Kılavuzu - JavaScript istemcisi - etki alanları arası bağlantı kurmak nasıl](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="68298-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="68298-184">Ayrıntılı hata iletilerini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="68298-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="68298-185">Hatalar oluştuğunda, SignalR varsayılan davranışını istemciler için ne hakkında ayrıntılar olmadan bir bildirim iletisi göndermektir.</span><span class="sxs-lookup"><span data-stu-id="68298-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="68298-186">Kötü amaçlı kullanıcıların uygulamanızı saldırıları bilgileri kullanmak mümkün olabilir çünkü istemciler için ayrıntılı hata bilgilerini göndermesini üretimde önerilmez.</span><span class="sxs-lookup"><span data-stu-id="68298-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="68298-187">Sorun giderme için geçici olarak daha bilgilendirici hata raporlamasını etkinleştirmek için bu seçeneği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="68298-188">Otomatik olarak oluşturulan JavaScript proxy'si dosyaları devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="68298-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="68298-189">Varsayılan olarak, yanıt URL'sine "/ signalr/hubs" Hub sınıflarınızı için proxy ile bir JavaScript dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="68298-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="68298-190">JavaScript proxy'leri kullanmak istemiyorsanız veya bu dosyayı el ile oluşturmanız ve istemcilerinizin fiziksel bir dosyaya başvurmak istiyorsanız, proxy oluşturma devre dışı bırakmak için bu seçeneği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="68298-191">Daha fazla bilgi için [SignalR Hubs API Kılavuzu - JavaScript istemcisi - fiziksel dosya oluşturma SignalR için oluşturulan proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="68298-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="68298-192">Aşağıdaki örnek, bir çağrıda SignalR bağlantı URL'si ve bu seçenekleri belirtmek gösterilmektedir `MapSignalR` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="68298-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="68298-193">Özel bir URL belirtmek için Değiştir "/ signalr" örnekte URL'siyle kullanmak istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="68298-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="68298-194">Oluşturma ve Hub sınıflarını kullanma</span><span class="sxs-lookup"><span data-stu-id="68298-194">How to create and use Hub classes</span></span>

<span data-ttu-id="68298-195">Bir Hub oluşturmak için türetilen bir sınıf oluşturma [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="68298-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="68298-196">Aşağıdaki örnek, basit bir Hub sınıfı için bir sohbet uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="68298-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="68298-197">Bu örnekte, bir bağlı istemci çağırabilirsiniz `NewContosoChatMessage` yöntemi ve yaptığında, alınan verilerin bağlanan tüm istemciler için yayımladınız.</span><span class="sxs-lookup"><span data-stu-id="68298-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="68298-198">Hub nesne yaşam süresi</span><span class="sxs-lookup"><span data-stu-id="68298-198">Hub object lifetime</span></span>

<span data-ttu-id="68298-199">Hub sınıfının örneği yok ya da sunucu üzerindeki kendi koddan yöntemlerinin çağrılması; Tüm bunları sizin için SignalR hub'ları işlem hattı tarafından gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="68298-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="68298-200">SignalR hub'ı sınıfınıza yeni bir örneğini ne zaman bir istemci bağlanır, bağlantısı kesildiğinde veya sunucu için bir yöntem çağrısı yapar gibi bir Hub işlemin işlemek için gereken her zaman oluşturur.</span><span class="sxs-lookup"><span data-stu-id="68298-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="68298-201">Hub sınıfının örneklerini geçici olduğu için bir yöntem çağrısından sonraki durumunu korumak üzere kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="68298-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="68298-202">Her sunucunun bir yöntem çağrısının bir istemciden Hub sınıfı işlemlerinizi yeni bir örneğini iletiyi alır.</span><span class="sxs-lookup"><span data-stu-id="68298-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="68298-203">Birden fazla bağlantı ve yöntem çağrıları arasında durumu korumak için Hub sınıfına veya farklı bir sınıf türünden türemez bir veritabanı veya statik bir değişken gibi bazı başka bir yöntem kullanın `Hub`.</span><span class="sxs-lookup"><span data-stu-id="68298-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="68298-204">Bellekteki verileri devam ediyorsa, uygulama etki alanı geri dönüştürüldüğünde Hub sınıfında statik bir değişken gibi bir yöntem kullanarak verileri kaybolur.</span><span class="sxs-lookup"><span data-stu-id="68298-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="68298-205">Kendi koddan Hub sınıfı dışında çalışan istemciler için iletileri göndermek istiyorsanız bir Hub örneği oluşturarak bunu yapamazsınız, ancak Hub sınıfınız için bir başvuru SignalR bağlam nesnesi alarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="68298-206">Daha fazla bilgi için [istemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetmek nasıl](#callfromoutsidehub) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="68298-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="68298-207">JavaScript istemcilerinin Hub adları camel casing</span><span class="sxs-lookup"><span data-stu-id="68298-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="68298-208">Varsayılan olarak, JavaScript istemcilerinin, sınıf adı ortası büyük küçük harfleri sürümünü kullanarak hub'larına bakın.</span><span class="sxs-lookup"><span data-stu-id="68298-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="68298-209">Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.</span><span class="sxs-lookup"><span data-stu-id="68298-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="68298-210">Önceki örneği olarak adlandırılır `contosoChatHub` JavaScript kod.</span><span class="sxs-lookup"><span data-stu-id="68298-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="68298-211">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="68298-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="68298-212">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="68298-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="68298-213">İstemcilerin kullanın, eklemek farklı bir ad belirlemek istiyorsanız `HubName` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="68298-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="68298-214">Kullandığınızda, bir `HubName` özniteliği, için JavaScript istemcilerde ortası büyük harf adı bir değişiklik yoktur.</span><span class="sxs-lookup"><span data-stu-id="68298-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="68298-215">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="68298-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="68298-216">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="68298-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="68298-217">Birden çok hub'ları</span><span class="sxs-lookup"><span data-stu-id="68298-217">Multiple Hubs</span></span>

<span data-ttu-id="68298-218">Bir uygulamada birden fazla Hub sınıfı tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="68298-219">Bunu yaptığınızda, paylaşılan bir bağlantısı ancak grupları ayrı:</span><span class="sxs-lookup"><span data-stu-id="68298-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="68298-220">Tüm istemciler, hizmetiniz ile SignalR bağlantı kurmak için aynı URL'yi kullanır ("/ signalr" ya da bir belirttiyseniz, özel URL), hizmet tarafından tanımlanan ve bağlantı tüm hub'ları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="68298-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="68298-221">Tek bir sınıfta tüm Hub işlevselliği tanımlamak için kıyasla çok sayıda hub için bir performans farkı yoktur.</span><span class="sxs-lookup"><span data-stu-id="68298-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="68298-222">Tüm hub'ları için aynı HTTP isteği bilgilerini edinin.</span><span class="sxs-lookup"><span data-stu-id="68298-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="68298-223">Tüm hub'ları aynı bağlantıyı paylaşmak olduğundan, sunucunun aldığı yalnızca HTTP isteği ne SignalR bağlantı kurar özgün HTTP isteği gelen bilgilerdir.</span><span class="sxs-lookup"><span data-stu-id="68298-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="68298-224">Bir sorgu dizesi belirterek bilgi istemciden sunucuya geçirmek için bağlantı isteğini kullanırsanız, farklı sorgu dizeleri için farklı hub'lar sağlayamaz.</span><span class="sxs-lookup"><span data-stu-id="68298-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="68298-225">Tüm hub'ları aynı bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="68298-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="68298-226">Tek bir dosyada, tüm hub'ları proxy'lerini oluşturulan JavaScript proxy'leri dosyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="68298-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="68298-227">JavaScript proxy'si hakkında daha fazla bilgi için bkz. [SignalR Hubs API Kılavuzu - JavaScript istemcisi - oluşturulan proxy ve sizin için yaptığı](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="68298-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="68298-228">Hub'ları grupları tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="68298-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="68298-229">Adlandırılmış alt kümelerini bağlı istemciler için yayın gruplar SignalR öğesinde tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="68298-230">Grupları, her Hub için ayrı olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="68298-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="68298-231">Örneğin, "Yöneticiler" adlı bir grup istemciler için bir dizi verilebilir, `ContosoChatHub` sınıfı ve aynı ada istemciler için farklı bir dizi bakın, `StockTickerHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="68298-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="68298-232">Kesin tür belirtilmiş hub'ları</span><span class="sxs-lookup"><span data-stu-id="68298-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="68298-233">İstemciniz için hub yöntemleri için bir arabirim tanımlamak için başvuru (ve hub yöntemlerinizi IntelliSense etkinleştirin) türetilen hub'ınızdan `Hub<T>` (SignalR 2.1 içinde sunulmuştur) yerine `Hub`:</span><span class="sxs-lookup"><span data-stu-id="68298-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="68298-234">İstemciler çağırabileceğiniz Hub sınıfı yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="68298-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="68298-235">İstemciden çağrılabilir olmasını istediğiniz Hub üzerindeki bir yöntem kullanıma sunmak için aşağıdaki örneklerde gösterildiği gibi genel bir yöntem bildirin.</span><span class="sxs-lookup"><span data-stu-id="68298-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="68298-236">Dönüş türü ve parametreleri, tüm C# yönteminde olduğu gibi karmaşık türler ve diziler de dahil olmak üzere belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="68298-237">Parametreleri almak veya çağırana döndürmesi herhangi bir veri istemci ve sunucu arasında JSON'ı kullanarak iletilir ve SignalR bağlama karmaşık nesnelerin ve nesne dizileri otomatik olarak işler.</span><span class="sxs-lookup"><span data-stu-id="68298-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="68298-238">JavaScript istemcilerinin yöntemi adları camel casing</span><span class="sxs-lookup"><span data-stu-id="68298-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="68298-239">Varsayılan olarak, JavaScript istemcilerinin, Hub yöntemleri için bir yöntem adı ortası büyük küçük harfleri sürümünü kullanarak bakın.</span><span class="sxs-lookup"><span data-stu-id="68298-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="68298-240">Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.</span><span class="sxs-lookup"><span data-stu-id="68298-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="68298-241">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="68298-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="68298-242">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="68298-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="68298-243">İstemcilerin kullanın, eklemek farklı bir ad belirlemek istiyorsanız `HubMethodName` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="68298-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="68298-244">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="68298-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="68298-245">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="68298-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="68298-246">Zaman zaman uyumsuz olarak çalıştırmak için</span><span class="sxs-lookup"><span data-stu-id="68298-246">When to execute asynchronously</span></span>

<span data-ttu-id="68298-247">Yöntemi uzun süre çalışan olması veya çalışmaya sahipse, Bekliyor, bir veritabanı araması veya bir web hizmeti çağrısı gibi ilgili, döndürerek Hub yönteminin zaman uyumsuz hale bir [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (yerine `void` döndürür) veya [ Görev&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) nesne (yerine `T` dönüş türü).</span><span class="sxs-lookup"><span data-stu-id="68298-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="68298-248">Döndüğünüzde bir `Task` yöntemi, SignalR nesneden bekler `Task` tamamlamak için ve istemci yöntem çağrısında nasıl kod içinde herhangi bir fark olması, sarmalanmış halden sonucu istemciye geri gönderir.</span><span class="sxs-lookup"><span data-stu-id="68298-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="68298-249">Bir Hub yöntemini olmasını zaman uyumsuz WebSocket taşıma kullandığında bağlantıya engel önler.</span><span class="sxs-lookup"><span data-stu-id="68298-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="68298-250">Hub yönteminin tamamlanana kadar bir Hub yöntemini zaman uyumlu olarak yürütülür ve taşıma WebSocket olduğunda, aynı istemciden hub yöntemlerine yönelik sonraki çağrılarını engellenir.</span><span class="sxs-lookup"><span data-stu-id="68298-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="68298-251">Aynı yöntem eşzamanlı çalışacak biçimde kodlanmış veya zaman uyumsuz olarak çalışan iki sürümden çağırmak için JavaScript istemci kodu ardından aşağıdaki örnekte gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="68298-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="68298-252">**Zaman uyumlu**</span><span class="sxs-lookup"><span data-stu-id="68298-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="68298-253">**Zaman uyumsuz**</span><span class="sxs-lookup"><span data-stu-id="68298-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="68298-254">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="68298-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="68298-255">ASP.NET 4.5 içinde zaman uyumsuz yöntemler kullanma hakkında daha fazla bilgi için bkz. [kullanarak ASP.NET MVC 4'te zaman uyumsuz yöntemleri](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="68298-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="68298-256">Aşırı yüklemeler tanımlama</span><span class="sxs-lookup"><span data-stu-id="68298-256">Defining Overloads</span></span>

<span data-ttu-id="68298-257">Yöntemi için aşırı yüklemeleri tanımlamak istiyorsanız, her aşırı yükleme parametre sayısı farklı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="68298-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="68298-258">Farklı parametre türleri belirterek bir aşırı ayırt Hub sınıfınıza derleyeceği ancak SignalR hizmeti, çağrı aşırı istemciler çalıştığınızda çalışma zamanında bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="68298-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="68298-259">Gelen hub yöntemi çağrılarına ilerleme durumunu bildirme</span><span class="sxs-lookup"><span data-stu-id="68298-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="68298-260">SignalR 2.1 için destek ekler [deseni raporlama ilerleme](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) .NET 4. 5 ' tanıtılan.</span><span class="sxs-lookup"><span data-stu-id="68298-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="68298-261">İlerleme durumunu bildirme uygulamak için tanımladığınız bir `IProgress<T>` istemcinizi erişebilir, hub yöntemi parametresi:</span><span class="sxs-lookup"><span data-stu-id="68298-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="68298-262">Uzun süre çalışan sunucusu yönteminin yazma sırasında gibi zaman uyumsuz bir zaman uyumsuz programlama deseni kullanılacak önemli olduğu / Await yerine hub iş parçacığını engelleme.</span><span class="sxs-lookup"><span data-stu-id="68298-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="68298-263">İstemci Hub sınıfı yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="68298-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="68298-264">İstemcisi, sunucudan yöntemleri çağırmak için kullanın `Clients` Hub sınıfınızın bir yöntemde bir özellik.</span><span class="sxs-lookup"><span data-stu-id="68298-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="68298-265">Aşağıdaki örnek, çağıran sunucu kodu gösterir `addNewMessageToPage` tüm bağlı istemcileri ve istemci kodu, bir JavaScript istemci yöntemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="68298-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="68298-266">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="68298-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="68298-267">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="68298-267">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="68298-268">İstemci yöntemden dönüş değeri alınamıyor; söz dizimi gibi `int x = Clients.All.add(1,1)` çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="68298-268">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="68298-269">Karmaşık türler ve diziler için parametreleri belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-269">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="68298-270">Aşağıdaki örnek bir yöntem parametresi istemci bir karmaşık tür geçirir.</span><span class="sxs-lookup"><span data-stu-id="68298-270">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="68298-271">**Karmaşık bir nesne kullanarak bir istemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="68298-271">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="68298-272">**Karmaşık bir nesne tanımlayan bir sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="68298-272">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="68298-273">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="68298-273">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="68298-274">Hangi istemcilerin seçerek RPC alırsınız</span><span class="sxs-lookup"><span data-stu-id="68298-274">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="68298-275">İstemciler özelliği döndürür bir [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) hangi istemcilerin RPC alırsınız belirtmek için çeşitli seçenekler sağlayan nesne:</span><span class="sxs-lookup"><span data-stu-id="68298-275">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="68298-276">Bağlanan tüm istemciler.</span><span class="sxs-lookup"><span data-stu-id="68298-276">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="68298-277">Çağıran istemci yalnızca.</span><span class="sxs-lookup"><span data-stu-id="68298-277">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="68298-278">Çağıran istemci dışındaki tüm istemcilerin.</span><span class="sxs-lookup"><span data-stu-id="68298-278">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="68298-279">Bağlantı kimliği ile tanımlanan belirli bir istemci</span><span class="sxs-lookup"><span data-stu-id="68298-279">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="68298-280">Bu örnek `addContosoChatMessageToPage` çağıran istemci hakkında ve kullanmakla aynı etkiye sahip `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="68298-280">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="68298-281">Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="68298-281">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="68298-282">Belirli bir grubun tüm bağlı istemcileri.</span><span class="sxs-lookup"><span data-stu-id="68298-282">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="68298-283">Belirtilen istemcilerin bağlantı kimliği ile tanımlanan dışında belirtilen gruptaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="68298-283">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="68298-284">Belirli bir grubun tüm bağlı istemcileri çağıran istemci dışındaki.</span><span class="sxs-lookup"><span data-stu-id="68298-284">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="68298-285">Kullanıcı tarafından tanımlanan belirli bir kullanıcı.</span><span class="sxs-lookup"><span data-stu-id="68298-285">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="68298-286">Varsayılan olarak, `IPrincipal.Identity.Name`, ancak bu tarafından değiştirilebilir [IUserIdProvider uygulaması genel ana bilgisayar kaydetme](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="68298-286">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="68298-287">Tüm istemciler ve grupları listesinde bağlantı kimlikleri.</span><span class="sxs-lookup"><span data-stu-id="68298-287">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="68298-288">Gruplarının listesi.</span><span class="sxs-lookup"><span data-stu-id="68298-288">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="68298-289">Bir kullanıcı adı.</span><span class="sxs-lookup"><span data-stu-id="68298-289">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="68298-290">(SignalR 2.1 içinde sunulmuştur) kullanıcı adlarının listesi.</span><span class="sxs-lookup"><span data-stu-id="68298-290">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="68298-291">Yöntem adları için derleme zamanı doğrulama</span><span class="sxs-lookup"><span data-stu-id="68298-291">No compile-time validation for method names</span></span>

<span data-ttu-id="68298-292">Belirttiğiniz yöntem adı, IntelliSense veya derleme zamanı doğrulamasını yoktur anlamına gelen dinamik bir nesne olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="68298-292">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="68298-293">İfade, çalışma zamanında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="68298-293">The expression is evaluated at run time.</span></span> <span data-ttu-id="68298-294">Yöntem çağrısının yürütüldüğünde, SignalR yöntem adı ve parametre değerlerini istemciye gönderir ve istemci bir yöntemi varsa, adla eşleşen, yöntemi çağrılır ve parametre değerlerini, kendisine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="68298-294">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="68298-295">Eşleşen hiçbir yöntemi istemcide bulunursa, herhangi bir hata ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="68298-295">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="68298-296">Bir istemci yöntemi çağırdığınızda, arka planda istemciye SignalR ileten veri biçimi hakkında daha fazla bilgi için bkz [signalr'a giriş](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="68298-296">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="68298-297">Ad eşleştirme büyük küçük harf duyarsız yöntemi</span><span class="sxs-lookup"><span data-stu-id="68298-297">Case-insensitive method name matching</span></span>

<span data-ttu-id="68298-298">Yöntem adı ile eşleşen büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="68298-298">Method name matching is case-insensitive.</span></span> <span data-ttu-id="68298-299">Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütülür `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, veya `addContosoChatMessageToPage` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="68298-299">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="68298-300">Zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="68298-300">Asynchronous execution</span></span>

<span data-ttu-id="68298-301">Çağıran, yöntemi zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="68298-301">The method that you call executes asynchronously.</span></span> <span data-ttu-id="68298-302">Bir istemci için bir yöntem çağrısının hemen sonraki kod satırlarını siz belirtmediğiniz sürece, istemciye veri aktarımı tamamlamak, SignalR için beklemenize gerek kalmadan yürütecek sonra gelen kodu yöntemi tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="68298-302">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="68298-303">Aşağıdaki kod örneği, iki istemci yöntemleri ardışık olarak yürütmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="68298-303">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="68298-304">**Await (.NET 4.5) kullanma**</span><span class="sxs-lookup"><span data-stu-id="68298-304">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="68298-305">Kullanırsanız `await` sonraki satırlık bir kod yürütülmeden önce bir istemci yöntemi bitene kadar beklemek için gelmez sonraki satırlık bir kod yürütülmeden önce istemcileri aslında iletiyi alır.</span><span class="sxs-lookup"><span data-stu-id="68298-305">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="68298-306">Yalnızca bir istemci yöntem çağrısının "tamamlama" SignalR ileti göndermek için gereken her şey yapmış anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="68298-306">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="68298-307">İstemcilerin ileti aldığı doğrulama gerekiyorsa, bu mekanizma kendiniz program gerekir.</span><span class="sxs-lookup"><span data-stu-id="68298-307">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="68298-308">Örneğin, kod bir `MessageReceived` yöntemi Hub hem de `addContosoChatMessageToPage` , çağırın istemcide yöntemi `MessageReceived` , yaptıktan sonra iş istemcide gerçekleştirmek için ihtiyaç.</span><span class="sxs-lookup"><span data-stu-id="68298-308">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="68298-309">İçinde `MessageReceived` hub'ı gerçek istemci alma ve işleme özgün yöntem çağrısının hangi iş bağlıdır, bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-309">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="68298-310">Yöntem adı bir dize değişkeni kullanma</span><span class="sxs-lookup"><span data-stu-id="68298-310">How to use a string variable as the method name</span></span>

<span data-ttu-id="68298-311">Cast yöntemi adı bir dize değişkeni kullanarak bir istemci yöntem çağırmak istiyorsanız `Clients.All` (veya `Clients.Others`, `Clients.Caller`, vs.) için `IClientProxy` ve sonra çağrı [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="68298-311">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="68298-312">Hub sınıftan grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="68298-312">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="68298-313">Signalr'da gruplarla, bağlı istemciler belirtilen alt kümelerine yayın iletileri için bir yöntem sağlar.</span><span class="sxs-lookup"><span data-stu-id="68298-313">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="68298-314">Bir grupta herhangi bir sayıda istemciler olabilir ve istemci grupları herhangi bir sayıda üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="68298-314">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="68298-315">Grup üyeliğini yönetmek için kullandığınız [Ekle](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ve [Kaldır](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) tarafından sağlanan yöntemleri `Groups` Hub sınıfın özelliği.</span><span class="sxs-lookup"><span data-stu-id="68298-315">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="68298-316">Aşağıdaki örnekte gösterildiği `Groups.Add` ve `Groups.Remove` istemci kodu tarafından çağrılan Hub yöntemlerinde kullanılan yöntemleri, onları çağıran JavaScript istemci kodu tarafından izlenen.</span><span class="sxs-lookup"><span data-stu-id="68298-316">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="68298-317">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="68298-317">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="68298-318">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="68298-318">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="68298-319">Açıkça grupları oluşturmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="68298-319">You don't have to explicitly create groups.</span></span> <span data-ttu-id="68298-320">Etkin bir grup çağrıda adını belirttiğiniz ilk kez otomatik olarak oluşturulur `Groups.Add`, ve bu üyelik son bağlantı kaldırdığınızda silinir.</span><span class="sxs-lookup"><span data-stu-id="68298-320">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="68298-321">Bir grup üyeliği listesinin veya grupların listesini almak için hiçbir API yoktur.</span><span class="sxs-lookup"><span data-stu-id="68298-321">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="68298-322">SignalR istemcileri ve gruplara göre iletiler gönderen bir [pub/sub modeli](http://en.wikipedia.org/wiki/Publish/subscribe), ve sunucu grupları veya grup üyeliklerinin listesi korumaz.</span><span class="sxs-lookup"><span data-stu-id="68298-322">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="68298-323">Bir web grubu için bir düğüm eklediğinizde, yeni bir düğüme dağıtılmasını SignalR tutar herhangi bir durum olduğundan bu ölçeklenebilirliği en üst düzeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="68298-323">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="68298-324">Ekleme ve kaldırma yöntemlerinin, zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="68298-324">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="68298-325">`Groups.Add` Ve `Groups.Remove` zaman uyumsuz bir yöntem yürütülemez.</span><span class="sxs-lookup"><span data-stu-id="68298-325">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="68298-326">Bir istemci bir gruba ekleyin ve hemen bir ileti grubunu kullanarak istemciye göndermek istiyorsanız, emin olmak sahip `Groups.Add` yöntemi önce tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="68298-326">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="68298-327">Aşağıdaki kod örneği bunu nasıl yapacağınız gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="68298-327">The following code example shows how to do that.</span></span>

<span data-ttu-id="68298-328">**Bir istemci bir gruba eklemek ve ardından istemci Mesajlaşma**</span><span class="sxs-lookup"><span data-stu-id="68298-328">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="68298-329">Grup üyeliği kalıcılığı</span><span class="sxs-lookup"><span data-stu-id="68298-329">Group membership persistence</span></span>

<span data-ttu-id="68298-330">SignalR bağlantıları izler, kullanıcıları değil, dolayısıyla bir kullanıcı kullanıcı her bağlandığında bağlantı aynı grupta olmasını istediğiniz, çağırmak zorunda `Groups.Add` her zaman kullanıcı yeni bir bağlantı kurar.</span><span class="sxs-lookup"><span data-stu-id="68298-330">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="68298-331">Geçici bağlantı kaybı sonra bazen SignalR bağlantı otomatik olarak geri yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-331">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="68298-332">Bu durumda, yeni bir bağlantı kurmadan aynı bağlantıyı SignalR geri yüklüyor ve bu nedenle istemcinin Grup üyeliğini otomatik olarak geri.</span><span class="sxs-lookup"><span data-stu-id="68298-332">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="68298-333">Bağlantı durumu grup üyelikleri de dahil olmak üzere, her istemci için istemcinin gidiş dönüşlü geçici kesme sunucu yeniden başlatma veya hata sonucu olsa bile mümkün olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="68298-333">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="68298-334">Bir sunucu arıza ve bağlantı zaman aşımına uğramadan önce yeni bir sunucu tarafından değiştirilir, istemci otomatik olarak yeni sunucuya yeniden ve üyesi olduğu grupları'nı yeniden kaydolun.</span><span class="sxs-lookup"><span data-stu-id="68298-334">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="68298-335">Bir bağlantı, bağlantı kaybı sonra otomatik olarak geri yüklenemez veya bağlantı zaman aşımına uğradığında veya (örneğin, bir tarayıcı için yeni bir sayfa gittiğinde) istemci kestiğinde, grup üyeliği kaybedilir.</span><span class="sxs-lookup"><span data-stu-id="68298-335">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="68298-336">Kullanıcı bir sonraki bağlanışında, yeni bir bağlantı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="68298-336">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="68298-337">Aynı kullanıcı yeni bir bağlantı kurduğunda grup üyeliklerini korumasına kullanıcılar ve gruplar ilişkileri izlemek ve grup üyelikleri kullanıcı yeni bir bağlantı kurar ve her zaman geri yüklemek uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="68298-337">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="68298-338">Bağlantılar ve tutarsızlıklara hakkında daha fazla bilgi için bkz. [Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl](#connectionlifetime) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="68298-338">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="68298-339">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="68298-339">Single-user groups</span></span>

<span data-ttu-id="68298-340">SignalR genellikle kullanan uygulamalar, hangi kullanıcının bir ileti gönderdi ve hangi kullanıcının bir ileti almalıdır öğrenmek için kullanıcılar ve bağlantılar arasındaki ilişkileri izlemek zorunda.</span><span class="sxs-lookup"><span data-stu-id="68298-340">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="68298-341">Grupları iki yaygın olarak kullanılan desenlerden birini yapmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="68298-341">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="68298-342">Tek kullanıcı grupları.</span><span class="sxs-lookup"><span data-stu-id="68298-342">Single-user groups.</span></span>

    <span data-ttu-id="68298-343">Kullanıcı adı grup adı belirtin ve kullanıcı bağlanır veya yeniden her zaman geçerli bağlantı kimliği gruba ekleyin.</span><span class="sxs-lookup"><span data-stu-id="68298-343">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="68298-344">Kullanıcıya ileti göndermek için Grup gönderin.</span><span class="sxs-lookup"><span data-stu-id="68298-344">To send messages to the user you send to the group.</span></span> <span data-ttu-id="68298-345">Bu yöntem bir dezavantajı, gruba kullanıcı çevrimiçi veya çevrimdışı olup olmadığını öğrenmek için bir yol sağlamaz olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="68298-345">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="68298-346">Kullanıcı adları ve bağlantı kimlikleri arasındaki ilişkilendirmeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="68298-346">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="68298-347">Her bir kullanıcı adı ve bir veya daha fazla bağlantı kimlikleri arasında bir ilişki bir sözlük veya veritabanında depolamak ve depolanan veriler her zaman kullanıcı bağlandığında veya bağlantısı kesildiğinde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="68298-347">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="68298-348">Kullanıcıya ileti göndermek için bağlantı kimliği belirtin.</span><span class="sxs-lookup"><span data-stu-id="68298-348">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="68298-349">Bu yöntem bir dezavantajı, daha fazla bellek alır olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="68298-349">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="68298-350">Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="68298-350">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="68298-351">Bağlantı ömrü olaylarını işlemek için normal bir kullanıcı veya bağlı olup olmadığını izler ve kullanıcı adları ve bağlantı kimlikleri arasındaki ilişkiyi izlemek için nedenleridir.</span><span class="sxs-lookup"><span data-stu-id="68298-351">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="68298-352">İstemciler bağlanın veya bağlantıyı kesin zaman kendi kodunuzu çalıştırmak için geçersiz kılma `OnConnected`, `OnDisconnected`, ve `OnReconnected` aşağıdaki örnekte gösterildiği gibi hub'ın sanal yöntemler sınıf.</span><span class="sxs-lookup"><span data-stu-id="68298-352">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="68298-353">OnConnected OnDisconnected ve OnReconnected olduğunda çağırılır</span><span class="sxs-lookup"><span data-stu-id="68298-353">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="68298-354">Bir tarayıcı yeni bir sayfaya gider her seferinde yeni bir bağlantı kurulması SignalR yürütülecek anlamına gelir sahip `OnDisconnected` yöntemi arkasından `OnConnected` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="68298-354">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="68298-355">Yeni bir bağlantı kurulduğunda SignalR her zaman yeni bir bağlantı kimliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="68298-355">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="68298-356">`OnReconnected` Olduğunda geçici kesme SignalR otomatik olarak, ne zaman kablo geçici olarak bağlantısı kesilir ve bağlantı zaman aşımına uğramadan önce yeniden bağlantı kuruldu gibi kurtarabileceğiniz bağlantılar yöntemi çağrılır. `OnDisconnected` İstemcinin bağlantısı kesildi ve SignalR olamaz otomatik olarak yeniden, ne zaman yeni bir sayfaya bir tarayıcı gider gibi yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="68298-356">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="68298-357">Bu nedenle, olası belirli bir istemcinin olayları dizisidir `OnConnected`, `OnReconnected`, `OnDisconnected`; veya `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="68298-357">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="68298-358">Sıra görmezsiniz `OnConnected`, `OnDisconnected`, `OnReconnected` belirli bir bağlantı için.</span><span class="sxs-lookup"><span data-stu-id="68298-358">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="68298-359">`OnDisconnected` Değil yönteminden ne zaman bir sunucu arıza gibi bazı senaryolarda veya uygulama etki alanı geri alır.</span><span class="sxs-lookup"><span data-stu-id="68298-359">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="68298-360">Başka bir sunucuya gelinceye veya uygulama etki alanı, geri dönüşüm tamamlandıktan, bazı istemciler bağlanın ve yangın mümkün olabilir `OnReconnected` olay.</span><span class="sxs-lookup"><span data-stu-id="68298-360">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="68298-361">Daha fazla bilgi için [anlama ve signalr'da bağlantı ömrü olaylarını işleme](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="68298-361">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="68298-362">Arayan durumu değil doldurulur</span><span class="sxs-lookup"><span data-stu-id="68298-362">Caller state not populated</span></span>

<span data-ttu-id="68298-363">Bağlantı ömrü olay işleyicisi yöntemleri içine girdiğiniz herhangi bir durumu anlamına sunucudan adlı `state` istemci üzerinde nesne değil doldurulacak içinde `Caller` sunucudaki özelliği.</span><span class="sxs-lookup"><span data-stu-id="68298-363">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="68298-364">Hakkında bilgi için `state` nesne ve `Caller` özelliği bkz [Hub sınıfına ve istemciler arasında durumunu nasıl](#passstate) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="68298-364">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="68298-365">Bağlam özelliği istemci bilgilerini alma</span><span class="sxs-lookup"><span data-stu-id="68298-365">How to get information about the client from the Context property</span></span>

<span data-ttu-id="68298-366">İstemcisi hakkında bilgi almak için kullanın `Context` Hub sınıfın özelliği.</span><span class="sxs-lookup"><span data-stu-id="68298-366">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="68298-367">`Context` Özelliği döndürür bir [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) aşağıdaki bilgilere erişim sağlayan nesne:</span><span class="sxs-lookup"><span data-stu-id="68298-367">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="68298-368">Çağıran istemcinin bağlantı kimliği.</span><span class="sxs-lookup"><span data-stu-id="68298-368">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="68298-369">Bağlantı kimliği (değeri kendi kodunuzda belirtemezsiniz) SignalR tarafından atanan bir GUID'dir.</span><span class="sxs-lookup"><span data-stu-id="68298-369">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="68298-370">Her bağlantı ve uygulamanızda birden çok hub'a varsa tüm hub'ları tarafından kullanılan kimliği aynı bağlantı için bir bağlantı kimliği yok.</span><span class="sxs-lookup"><span data-stu-id="68298-370">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="68298-371">HTTP üst bilgisi verileri.</span><span class="sxs-lookup"><span data-stu-id="68298-371">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="68298-372">HTTP üst bilgiler de alabilirsiniz `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="68298-372">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="68298-373">Aynı şeyi birden çok başvuru nedeni `Context.Headers` ilk olarak oluşturulduğu `Context.Request` özelliği daha sonra eklenen ve `Context.Headers` geriye dönük uyumluluk için tutulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="68298-373">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="68298-374">Dize verileri sorgulayın.</span><span class="sxs-lookup"><span data-stu-id="68298-374">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="68298-375">Sorgu dizesi verileri da edinebilirsiniz `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="68298-375">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="68298-376">Bu özelliği alma sorgu dizesi SignalR bağlantısı HTTP isteği ile kullanılan paroladır.</span><span class="sxs-lookup"><span data-stu-id="68298-376">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="68298-377">İstemci istemci hakkındaki verileri istemciden sunucuya geçirmek için kullanışlı bir yoldur bağlantı yapılandırarak, sorgu dizesi parametreleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-377">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="68298-378">Aşağıdaki örnek, oluşturulan proxy kullandığınızda, JavaScript istemci olarak bir sorgu dizesi eklemek için yollarından biri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="68298-378">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="68298-379">Sorgu dizesi parametreleri ayarlama hakkında daha fazla bilgi için bkz. API kılavuzları için [JavaScript](hubs-api-guide-javascript-client.md) ve [.NET](hubs-api-guide-net-client.md) istemciler.</span><span class="sxs-lookup"><span data-stu-id="68298-379">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="68298-380">SignalR tarafından dahili olarak kullanılan bazı diğer değerler yanı sıra sorgu dize verileri, bağlantı için kullanılan aktarım yöntemi bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="68298-380">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="68298-381">Değerini `transportMethod` "webSockets", "serverSentEvents", "foreverFrame" veya "longPolling" olacaktır.</span><span class="sxs-lookup"><span data-stu-id="68298-381">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="68298-382">Bu değer iade gerçekleştiriyorsanız `OnConnected` olay işleyicisi yönteminde, bazı senaryolarda ilk bağlantı için son anlaşılan taşıma yöntemini değil bir taşıma değeri alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="68298-382">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="68298-383">Bu durumda yöntem bir özel durum oluşturur ve daha sonra tekrar son taşıma yöntemi oluşturulduğunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="68298-383">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="68298-384">Tanımlama bilgileri.</span><span class="sxs-lookup"><span data-stu-id="68298-384">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="68298-385">Tanımlama bilgilerini de alabilirsiniz `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="68298-385">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="68298-386">Kullanıcı bilgileri.</span><span class="sxs-lookup"><span data-stu-id="68298-386">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="68298-387">İstek için HttpContext nesnesi:</span><span class="sxs-lookup"><span data-stu-id="68298-387">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="68298-388">Almak yerine bu yöntemi kullanmak `HttpContext.Current` almak için `HttpContext` SignalR bağlantı nesnesi.</span><span class="sxs-lookup"><span data-stu-id="68298-388">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="68298-389">Nasıl durumu Hub sınıfına ve istemciler arasında geçirme</span><span class="sxs-lookup"><span data-stu-id="68298-389">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="68298-390">İstemci proxy sağlayan bir `state` içinde depolayabileceğiniz her yöntem çağrısının ile sunucuya aktarılması istediğiniz veri nesnesi.</span><span class="sxs-lookup"><span data-stu-id="68298-390">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="68298-391">Sunucu üzerinde bu verilerine erişebilir `Clients.Caller` istemciler tarafından çağrılan Hub yöntemlerini bir özellik.</span><span class="sxs-lookup"><span data-stu-id="68298-391">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="68298-392">`Clients.Caller` Özelliği için bağlantı ömrü olay işleyicisi yöntemleri doldurulmamışsa `OnConnected`, `OnDisconnected`, ve `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="68298-392">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="68298-393">Oluşturma veya güncelleştirme verilerinde `state` nesne ve `Clients.Caller` özelliği, her iki yönde de çalışır.</span><span class="sxs-lookup"><span data-stu-id="68298-393">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="68298-394">Değerleri sunucusunda güncelleştirebilirsiniz ve bunlar istemciye geçirilir.</span><span class="sxs-lookup"><span data-stu-id="68298-394">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="68298-395">Aşağıdaki örnek, iletilmesi için her yöntem çağrısının ile sunucu durumunu depolayan JavaScript istemci kodu gösterir.</span><span class="sxs-lookup"><span data-stu-id="68298-395">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="68298-396">Aşağıdaki örnek bir .NET istemci eşdeğer kod gösterir.</span><span class="sxs-lookup"><span data-stu-id="68298-396">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="68298-397">Hub sınıfınızda, bu verilerine erişebilir `Clients.Caller` özelliği.</span><span class="sxs-lookup"><span data-stu-id="68298-397">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="68298-398">Aşağıdaki örnek, önceki örnekte başvurulan durumunu alır. kod gösterir.</span><span class="sxs-lookup"><span data-stu-id="68298-398">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="68298-399">Kalıcı hale getirme durumu için bu düzenek itibaren her şeyi içine girdiğiniz büyük miktarlarda veri için tasarlanmamıştır `state` veya `Clients.Caller` özellik gidiş dönüşlü ile her yöntem çağırma.</span><span class="sxs-lookup"><span data-stu-id="68298-399">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="68298-400">Kullanıcı adlarını veya sayaçları gibi küçük öğeler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="68298-400">It's useful for smaller items such as user names or counters.</span></span>


<span data-ttu-id="68298-401">Aracılığıyla VB.NET veya türü kesin belirlenmiş bir hub'ı, arayanın durum nesnesi erişilemez `Clients.Caller`; bunun yerine, kullanın `Clients.CallerState` (SignalR 2.1 içinde sunulmuştur):</span><span class="sxs-lookup"><span data-stu-id="68298-401">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="68298-402">**CallerState C# kullanma**</span><span class="sxs-lookup"><span data-stu-id="68298-402">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="68298-403">**Visual Basic'te CallerState kullanma**</span><span class="sxs-lookup"><span data-stu-id="68298-403">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="68298-404">Hub sınıfında hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="68298-404">How to handle errors in the Hub class</span></span>

<span data-ttu-id="68298-405">Hub sınıfı yöntemlerinde meydana gelen hataları işlemek için bir veya daha fazla aşağıdaki yöntemlerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="68298-405">To handle errors that occur in your Hub class methods, use one or more of the following methods:</span></span>

- <span data-ttu-id="68298-406">Try-catch bloğu içinde yöntemi kodunuzu sarın ve özel durum nesnesi oturum açın.</span><span class="sxs-lookup"><span data-stu-id="68298-406">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="68298-407">Hata ayıklama amacıyla istemciye bir özel durum gönderebilir, ancak güvenlik için üretim istemciler için ayrıntılı bilgi gönderme nedeniyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="68298-407">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="68298-408">İşleme bir hub'ları işlem hattı modülünüzü oluşturmak [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="68298-408">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="68298-409">Aşağıdaki örnek, hatalar, modül hub ardışık düzene ekler. Startup.cs içindeki kod tarafından izlenen günlüklerini bir işlem hattı modül gösterir.</span><span class="sxs-lookup"><span data-stu-id="68298-409">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="68298-410">Kullanım `HubException` sınıfı (SignalR 2'de sunulmuştur).</span><span class="sxs-lookup"><span data-stu-id="68298-410">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="68298-411">Bu hata, herhangi bir hub çağrısından atılabilir.</span><span class="sxs-lookup"><span data-stu-id="68298-411">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="68298-412">`HubError` Oluşturucusu bir dize iletisi ve ek hata verileri depolamak için bir nesne alır.</span><span class="sxs-lookup"><span data-stu-id="68298-412">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="68298-413">SignalR, özel durumu otomatik olarak serileştirmek ve burada, reddetme veya hub yöntemi çağrısının başarısız için kullanılacak istemciye gönderir.</span><span class="sxs-lookup"><span data-stu-id="68298-413">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="68298-414">Aşağıdaki kod örnekleri nasıl throw gösteren bir `HubException` bir Hub çağrısının yanı sıra, JavaScript ve .NET istemcilerde özel durumu işlemek nasıl sırasında.</span><span class="sxs-lookup"><span data-stu-id="68298-414">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="68298-415">**Sunucu kodu gösteren HubException sınıfı**</span><span class="sxs-lookup"><span data-stu-id="68298-415">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="68298-416">**Bir hub'ı bir HubException atma yanıt gösteren JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="68298-416">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="68298-417">**.NET istemci kodunu gösteren bir hub'ı bir HubException atma yanıt**</span><span class="sxs-lookup"><span data-stu-id="68298-417">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="68298-418">Hub ardışık düzen modüllerine hakkında daha fazla bilgi için bkz: [hub ardışık düzeni özelleştirildiği nasıl](#hubpipeline) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="68298-418">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="68298-419">İzlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="68298-419">How to enable tracing</span></span>

<span data-ttu-id="68298-420">System.diagnostics öğesi sunucu-tarafı izlemeyi etkinleştirmek için Web.config dosyasına, bu örnekte gösterildiği gibi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="68298-420">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="68298-421">Visual Studio'da uygulamayı çalıştırdığınızda, günlükleri görüntüleyebilirsiniz **çıkış** penceresi.</span><span class="sxs-lookup"><span data-stu-id="68298-421">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="68298-422">İstemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetme</span><span class="sxs-lookup"><span data-stu-id="68298-422">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="68298-423">İstemci Hub sınıfınıza daha farklı bir sınıftaki yöntemleri çağırmak için hub'ı için SignalR bağlam nesnesi bir başvuru almak ve, istemcide yöntemlerini çağıran veya grupları yönetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="68298-423">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="68298-424">Aşağıdaki örnek `StockTicker` sınıfı bağlam nesnesini alır, sınıfının bir örneğini depolar, statik özellik bir sınıf örneğini depolar ve çağırmak için singleton sınıfı örneğinden bağlamı kullanır `updateStockPrice` istemciler yöntemi adlı bir Hub'ına bağlı `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="68298-424">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="68298-425">Uzun süreli bir nesne bağlamı birden çok kez kullanmanız gerekiyorsa, başvuru kez almak ve bunun yerine her zaman yeniden başlama kaydedin.</span><span class="sxs-lookup"><span data-stu-id="68298-425">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="68298-426">Bir kez bağlamı alma SignalR istemcilere içinde ve Hub yöntemlerinizi istemci yöntem çağrıları yapmak aynı sıradaki iletiler gönderir sağlar.</span><span class="sxs-lookup"><span data-stu-id="68298-426">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="68298-427">SignalR bağlamı için bir hub'ı kullanmayı gösteren bir öğretici için bkz. [ASP.NET SignalR ile sunucu yayını](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="68298-427">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="68298-428">İstemci yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="68298-428">Calling client methods</span></span>

<span data-ttu-id="68298-429">Hangi istemcilerin RPC alırsınız belirtebilirsiniz, ancak bir Hub sınıftan çağrısı yaparken daha az seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="68298-429">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="68298-430">Bunun nedeni herhangi bir yöntem gibi geçerli bir bağlantı kimliği bilgisi gerektiren şekilde bağlamı bir istemci belirli bir çağrıdan ilişkilendirilmiş olmasıdır `Clients.Others`, veya `Clients.Caller`, veya `Clients.OthersInGroup`, kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="68298-430">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="68298-431">Aşağıdaki seçenekler mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="68298-431">The following options are available:</span></span>

- <span data-ttu-id="68298-432">Bağlanan tüm istemciler.</span><span class="sxs-lookup"><span data-stu-id="68298-432">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="68298-433">Bağlantı kimliği ile tanımlanan belirli bir istemci</span><span class="sxs-lookup"><span data-stu-id="68298-433">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="68298-434">Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="68298-434">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="68298-435">Belirli bir grubun tüm bağlı istemcileri.</span><span class="sxs-lookup"><span data-stu-id="68298-435">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="68298-436">Bağlantı kimliği ile tanımlanan, belirtilen istemcilerin dışında belirtilen gruptaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="68298-436">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="68298-437">Hub sınıfınızda yöntemlerinden Hub olmayan sınıfınıza çağırıyorsanız, geçerli bağlantı kimliği geçirin ve ile kullanan `Clients.Client`, `Clients.AllExcept`, veya `Clients.Group` benzetimini yapmak için `Clients.Caller`, `Clients.Others`, veya `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="68298-437">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="68298-438">Aşağıdaki örnekte, `MoveShapeHub` sınıfı için bağlantı kimliği geçirir `Broadcaster` sınıfı böylece `Broadcaster` sınıfı benzetimini yapmak `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="68298-438">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="68298-439">Grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="68298-439">Managing group membership</span></span>

<span data-ttu-id="68298-440">Bir Hub sınıfta olarak grupları yönetme için aynı seçeneklere sahip.</span><span class="sxs-lookup"><span data-stu-id="68298-440">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="68298-441">Bir gruba istemci Ekle</span><span class="sxs-lookup"><span data-stu-id="68298-441">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="68298-442">Bir istemci bir gruptan kaldırma</span><span class="sxs-lookup"><span data-stu-id="68298-442">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="68298-443">Hub ardışık düzeni özelleştirme</span><span class="sxs-lookup"><span data-stu-id="68298-443">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="68298-444">SignalR Hub ardışık düzende kendi kod eklemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="68298-444">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="68298-445">Aşağıdaki örnek, istemci ve istemci üzerinde çağrılır giden yöntem çağrısının alınan gelen her yöntem çağrısının günlükleri özel bir Hub ardışık düzen modülü gösterir:</span><span class="sxs-lookup"><span data-stu-id="68298-445">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="68298-446">Aşağıdaki kod içinde *Startup.cs* dosyayı, işlem hattında hub'ı çalıştırmak için modül kaydeder:</span><span class="sxs-lookup"><span data-stu-id="68298-446">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="68298-447">Geçersiz kılabilirsiniz birçok farklı yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="68298-447">There are many different methods that you can override.</span></span> <span data-ttu-id="68298-448">Tam bir listesi için bkz. [HubPipelineModule yöntemleri](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="68298-448">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
