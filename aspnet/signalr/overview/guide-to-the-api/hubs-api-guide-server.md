---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: "ASP.NET SignalR hub'ları API Kılavuzu - sunucu (C#) | Microsoft Docs"
author: pfletcher
description: "Bu belge gösteren kod örnekleri ile sürüm 2, SignalR için ASP.NET SignalR hub'ları API sunucu tarafı programlama için bir giriş sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: c2567d4d39a494daf77a23db5dff83c8fae4925d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a><span data-ttu-id="85baf-103">ASP.NET SignalR hub'ları API Kılavuzu - sunucu (C#)</span><span class="sxs-lookup"><span data-stu-id="85baf-103">ASP.NET SignalR Hubs API Guide - Server (C#)</span></span>
====================
<span data-ttu-id="85baf-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="85baf-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="85baf-105">Bu belge, ASP.NET SignalR hub'ları API sunucu tarafı sürüm 2, SignalR için ortak seçeneklerini gösteren kod örnekleri ile programlama giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="85baf-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 2, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="85baf-106">SignalR hub'ları API bir sunucuya bağlanan istemciler ve sunucu istemcilerine uzaktan yordam çağrılarını (RPC) yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="85baf-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="85baf-107">Sunucu kodu, istemciler tarafından çağrılabilir yöntemlerini tanımlama ve istemci üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="85baf-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="85baf-108">İstemci kodu sunucudan çağrılabilir yöntemlerini tanımlama ve sunucu üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="85baf-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="85baf-109">SignalR, istemci-sunucu tesisat tümünün ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="85baf-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="85baf-110">SignalR kalıcı bağlantılar olarak adlandırılan bir alt düzey API de sunar.</span><span class="sxs-lookup"><span data-stu-id="85baf-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="85baf-111">SignalR, hub'lara ve kalıcı bağlantıların giriş için bkz: [SignalR 2 giriş](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="85baf-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR 2](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="85baf-112">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="85baf-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="85baf-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="85baf-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="85baf-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="85baf-114">.NET 4.5</span></span>
> - <span data-ttu-id="85baf-115">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="85baf-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="topic-versions"></a><span data-ttu-id="85baf-116">Konu sürümleri</span><span class="sxs-lookup"><span data-stu-id="85baf-116">Topic versions</span></span>
> 
> <span data-ttu-id="85baf-117">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="85baf-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="85baf-118">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="85baf-118">Questions and comments</span></span>
> 
> <span data-ttu-id="85baf-119">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="85baf-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="85baf-120">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="85baf-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="85baf-121">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="85baf-121">Overview</span></span>

<span data-ttu-id="85baf-122">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="85baf-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="85baf-123">SignalR Ara kaydetme</span><span class="sxs-lookup"><span data-stu-id="85baf-123">How to register SignalR middleware</span></span>](#route)

    - [<span data-ttu-id="85baf-124">/Signalr URL'si</span><span class="sxs-lookup"><span data-stu-id="85baf-124">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="85baf-125">SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="85baf-125">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="85baf-126">Oluşturma ve Hub sınıfları kullanma</span><span class="sxs-lookup"><span data-stu-id="85baf-126">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="85baf-127">Hub nesne ömrü</span><span class="sxs-lookup"><span data-stu-id="85baf-127">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="85baf-128">Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin Hub adları</span><span class="sxs-lookup"><span data-stu-id="85baf-128">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="85baf-129">Birden çok hub'ları</span><span class="sxs-lookup"><span data-stu-id="85baf-129">Multiple Hubs</span></span>](#multiplehubs)
    - [<span data-ttu-id="85baf-130">Kesin türü belirtilmiş hub'ları</span><span class="sxs-lookup"><span data-stu-id="85baf-130">Strongly-Typed Hubs</span></span>](#stronglytypedhubs)
- [<span data-ttu-id="85baf-131">İstemcileri çağırabilirsiniz Hub sınıfında yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="85baf-131">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="85baf-132">Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin adlarında yöntemi</span><span class="sxs-lookup"><span data-stu-id="85baf-132">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="85baf-133">Zaman uyumsuz olarak yürütülecek ne zaman</span><span class="sxs-lookup"><span data-stu-id="85baf-133">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="85baf-134">Aşırı tanımlama</span><span class="sxs-lookup"><span data-stu-id="85baf-134">Defining overloads</span></span>](#overloads)
    - [<span data-ttu-id="85baf-135">Hub yöntemi çağrılarına ilerlemesini raporlama</span><span class="sxs-lookup"><span data-stu-id="85baf-135">Reporting progress from hub method invocations</span></span>](#progress)
- [<span data-ttu-id="85baf-136">İstemci Hub sınıfından yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="85baf-136">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="85baf-137">Hangi istemcilerin seçerek RPC alırsınız</span><span class="sxs-lookup"><span data-stu-id="85baf-137">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="85baf-138">Yöntem adları için doğrulama olmaz derleme zamanı</span><span class="sxs-lookup"><span data-stu-id="85baf-138">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="85baf-139">Ad eşleştirme büyük küçük harf duyarsız yöntemini</span><span class="sxs-lookup"><span data-stu-id="85baf-139">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="85baf-140">Zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="85baf-140">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="85baf-141">Hub sınıfından grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="85baf-141">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="85baf-142">Add ve Remove yöntemlerini, zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="85baf-142">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="85baf-143">Grup üyeliği kalıcılığı</span><span class="sxs-lookup"><span data-stu-id="85baf-143">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="85baf-144">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="85baf-144">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="85baf-145">Bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını</span><span class="sxs-lookup"><span data-stu-id="85baf-145">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="85baf-146">OnConnected, OnDisconnected ve OnReconnected olduğunda çağrılır</span><span class="sxs-lookup"><span data-stu-id="85baf-146">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="85baf-147">Değil doldurulmuş arayan durumu</span><span class="sxs-lookup"><span data-stu-id="85baf-147">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="85baf-148">Bağlam özelliğinden istemcisi hakkında bilgi alma</span><span class="sxs-lookup"><span data-stu-id="85baf-148">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="85baf-149">Durum istemcileri ve Hub sınıfına arasında geçirmek nasıl</span><span class="sxs-lookup"><span data-stu-id="85baf-149">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="85baf-150">Hub sınıfında hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="85baf-150">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="85baf-151">İstemci yöntemlerini çağırın ve gruplardan Hub sınıfın dışından yönetme</span><span class="sxs-lookup"><span data-stu-id="85baf-151">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="85baf-152">İstemci yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="85baf-152">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="85baf-153">Grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="85baf-153">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="85baf-154">İzlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="85baf-154">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="85baf-155">Hub ardışık düzen özelleştirme</span><span class="sxs-lookup"><span data-stu-id="85baf-155">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="85baf-156">Program istemcilere nasıl belgeler için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="85baf-156">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="85baf-157">SignalR hub'ları API Kılavuzu - JavaScript istemci</span><span class="sxs-lookup"><span data-stu-id="85baf-157">SignalR Hubs API Guide - JavaScript Client</span></span>](hubs-api-guide-javascript-client.md)
- [<span data-ttu-id="85baf-158">SignalR hub'ları API Kılavuzu - .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="85baf-158">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="85baf-159">SignalR 2 için sunucu bileşenlerini, yalnızca .NET 4. 5 ' kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="85baf-159">The server components for SignalR 2 are only available in .NET 4.5.</span></span> <span data-ttu-id="85baf-160">.NET 4.0 çalıştıran sunucular SignalR v1.x kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="85baf-160">Servers running .NET 4.0 must use SignalR v1.x.</span></span>

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a><span data-ttu-id="85baf-161">SignalR Ara kaydetme</span><span class="sxs-lookup"><span data-stu-id="85baf-161">How to register SignalR middleware</span></span>

<span data-ttu-id="85baf-162">İstemcilerin Hub'ınıza bağlanmak için kullanacağı rota tanımlamak için arama `MapSignalR` uygulama başladığında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="85baf-162">To define the route that clients will use to connect to your Hub, call the `MapSignalR` method when the application starts.</span></span> <span data-ttu-id="85baf-163">`MapSignalR`olan bir [genişletme yöntemi](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) için `OwinExtensions` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="85baf-163">`MapSignalR` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `OwinExtensions` class.</span></span> <span data-ttu-id="85baf-164">Aşağıdaki örnek, OWIN başlangıç sınıfı kullanarak SignalR hub'ları yol tanımlamak gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="85baf-164">The following example shows how to define the SignalR Hubs route using an OWIN startup class.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

<span data-ttu-id="85baf-165">Bir ASP.NET MVC uygulaması için SignalR işlevselliği ekliyorsanız, SignalR rota diğer rotaların önce eklendiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="85baf-165">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="85baf-166">Daha fazla bilgi için bkz: [Öğreticisi: SignalR 2 ve MVC 5 ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="85baf-166">For more information, see [Tutorial: Getting Started with SignalR 2 and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="85baf-167">/Signalr URL'si</span><span class="sxs-lookup"><span data-stu-id="85baf-167">The /signalr URL</span></span>

<span data-ttu-id="85baf-168">Varsayılan olarak, istemciler Hub'ınıza bağlanmak için kullanacağı rota URL'dir "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="85baf-168">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="85baf-169">(Bu URL için otomatik olarak oluşturulan JavaScript dosyası "/ signalr/hub" URL ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="85baf-169">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="85baf-170">Oluşturulan proxy hakkında daha fazla bilgi için bkz: [SignalR hub'ları API Kılavuzu - JavaScript istemci - oluşturulan proxy ve onu sizin için ne yaptığını](hubs-api-guide-javascript-client.md#genproxy).)</span><span class="sxs-lookup"><span data-stu-id="85baf-170">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).)</span></span>

<span data-ttu-id="85baf-171">Bu temel URL SignalR için kullanılamaz hale olağanüstü durumlar olabilir; Örneğin, bir klasör adında projenizde var. *signalr* ve adını değiştirmek istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="85baf-171">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="85baf-172">Bu durumda, aşağıdaki örneklerde gösterildiği gibi temel URL değiştirebilirsiniz (Değiştir "/ signalr" örnek kodda, istenen URL ile).</span><span class="sxs-lookup"><span data-stu-id="85baf-172">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="85baf-173">**URL'yi belirtir sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="85baf-173">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

<span data-ttu-id="85baf-174">**URL (ile oluşturulan proxy) belirtir JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="85baf-174">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

<span data-ttu-id="85baf-175">**(Olmadan oluşturulan proxy) URL'yi belirtir JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="85baf-175">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="85baf-176">**URL'yi belirtir .NET istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="85baf-176">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="85baf-177">SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="85baf-177">Configuring SignalR Options</span></span>

<span data-ttu-id="85baf-178">Overloads `MapSignalR` yöntemini etkinleştirmek, özel bir URL, özel bağımlılık çözümleyici ve aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="85baf-178">Overloads of the `MapSignalR` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="85baf-179">Etki alanları arası çağrılar CORS veya JSONP tarayıcı istemcilerinden kullanarak etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="85baf-179">Enable cross-domain calls using CORS or JSONP from browser clients.</span></span>

    <span data-ttu-id="85baf-180">Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantısı `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="85baf-180">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="85baf-181">Varsa sayfasından `http://contoso.com` bir bağlantı kurar `http://fabrikam.com/signalr`, yani etki alanları arası bağlantı.</span><span class="sxs-lookup"><span data-stu-id="85baf-181">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="85baf-182">Güvenlik nedenleriyle, etki alanları arası bağlantılar, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="85baf-182">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="85baf-183">Daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - JavaScript istemci - etki alanları arası bağlantı kurmak nasıl](hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="85baf-183">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](hubs-api-guide-javascript-client.md#crossdomain).</span></span>
- <span data-ttu-id="85baf-184">Ayrıntılı hata iletilerini etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="85baf-184">Enable detailed error messages.</span></span>

    <span data-ttu-id="85baf-185">Hatalar oluştuğunda, SignalR varsayılan davranışını ne hakkında ayrıntılar olmadan bir bildirim iletisi istemcilere göndermektir.</span><span class="sxs-lookup"><span data-stu-id="85baf-185">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="85baf-186">Kötü niyetli kullanıcılar, uygulamanızın saldırıları bilgileri kullanmak olabilir çünkü istemciler için ayrıntılı hata bilgileri gönderme üretimde önerilmez.</span><span class="sxs-lookup"><span data-stu-id="85baf-186">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="85baf-187">Sorun giderme için geçici olarak daha bilgilendirici hata raporlamayı etkinleştirmek için bu seçeneği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-187">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="85baf-188">Otomatik olarak oluşturulan JavaScript proxy dosyaları devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="85baf-188">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="85baf-189">Varsayılan olarak, yanıt URL "/ signalr/hubs" olarak Hub sınıfları için proxy ile bir JavaScript dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="85baf-189">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="85baf-190">JavaScript proxy'leri kullanmak istemiyorsanız veya bu dosyayı el ile oluşturmak ve fiziksel bir dosyaya istemcileriniz başvurmak istiyorsanız proxy oluşturması devre dışı bırakmak için bu seçeneği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-190">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="85baf-191">Daha fazla bilgi için bkz: [SignalR hub'ları API Kılavuzu - JavaScript istemci - için SignalR fiziksel bir dosya oluşturmak nasıl oluşturulan proxy](hubs-api-guide-javascript-client.md#manualproxy).</span><span class="sxs-lookup"><span data-stu-id="85baf-191">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](hubs-api-guide-javascript-client.md#manualproxy).</span></span>

<span data-ttu-id="85baf-192">Aşağıdaki örnek, bir çağrıda SignalR bağlantı URL'si ve bu seçeneklerini belirtmek gösterilmektedir `MapSignalR` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="85baf-192">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapSignalR` method.</span></span> <span data-ttu-id="85baf-193">Özel bir URL belirtmek için Değiştir "/ signalr" kullanmak istediğiniz URL ile örnekte.</span><span class="sxs-lookup"><span data-stu-id="85baf-193">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="85baf-194">Oluşturma ve Hub sınıfları kullanma</span><span class="sxs-lookup"><span data-stu-id="85baf-194">How to create and use Hub classes</span></span>

<span data-ttu-id="85baf-195">Bir Hub oluşturmak için türeyen bir sınıf oluşturun [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="85baf-195">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="85baf-196">Aşağıdaki örnek, sohbet uygulaması için basit bir Hub sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="85baf-196">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

<span data-ttu-id="85baf-197">Bu örnekte, bir bağlı istemci çağırabilirsiniz `NewContosoChatMessage` yöntemi ve yaptığında, alınan verileri bağlanan tüm istemciler için yayımladınız.</span><span class="sxs-lookup"><span data-stu-id="85baf-197">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="85baf-198">Hub nesne ömrü</span><span class="sxs-lookup"><span data-stu-id="85baf-198">Hub object lifetime</span></span>

<span data-ttu-id="85baf-199">Hub sınıfının örneği yok ya da kendi kodunuzu sunucuda yöntemlerinden çağırmanıza; Tüm sizin için SignalR hub'ları ardışık düzen tarafından yapılır.</span><span class="sxs-lookup"><span data-stu-id="85baf-199">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="85baf-200">SignalR ne zaman bir istemci bağlanır, bağlantısını keser veya sunucuya bir yöntem çağrısı yapar gibi bir Hub işlemi işlemek için gereken her zaman, Hub sınıfının yeni bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="85baf-200">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="85baf-201">Hub sınıfının örnekleri geçici olduğundan, bir yöntem çağrısı sonraki durumunu korumak için kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="85baf-201">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="85baf-202">Her zaman sunucu yöntemi çağrısı ileti bir istemciden Hub sınıfı işlemlerinizi yeni bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="85baf-202">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="85baf-203">Birden çok bağlantıları ve yöntem çağrıları aracılığıyla durumunu korumak için Hub sınıfına veya türünden türemez farklı bir sınıf bir veritabanı veya statik değişkeni gibi bazı başka bir yöntem kullanın `Hub`.</span><span class="sxs-lookup"><span data-stu-id="85baf-203">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="85baf-204">Bellek verileri devam ederse, uygulama etki alanı geri dönüştürüldüğünde Hub sınıfı üzerinde statik bir değişken gibi bir yöntem kullanarak verileri kaybolur.</span><span class="sxs-lookup"><span data-stu-id="85baf-204">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="85baf-205">Hub sınıf dışında çalışan kendi kodundan istemcilere iletileri göndermek istiyorsanız, bir Hub sınıfı örneğini oluşturarak bunu yapamazsınız, ancak Hub sınıfınız için SignalR bağlamı nesneye bir başvurusu alarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-205">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="85baf-206">Daha fazla bilgi için bkz: [istemci yöntemlerini çağırın ve Hub sınıfına dışında gruplarından yönetmek nasıl](#callfromoutsidehub) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="85baf-206">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="85baf-207">Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin Hub adları</span><span class="sxs-lookup"><span data-stu-id="85baf-207">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="85baf-208">Varsayılan olarak, JavaScript istemcilerinin sınıf adı başlamalıdır sürümünü kullanarak hub'lara bakın.</span><span class="sxs-lookup"><span data-stu-id="85baf-208">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="85baf-209">SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.</span><span class="sxs-lookup"><span data-stu-id="85baf-209">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="85baf-210">Önceki örneği olarak adlandırılan `contosoChatHub` JavaScript kodu.</span><span class="sxs-lookup"><span data-stu-id="85baf-210">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="85baf-211">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="85baf-211">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

<span data-ttu-id="85baf-212">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="85baf-212">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

<span data-ttu-id="85baf-213">İstemcilerin kullanın, eklemek farklı bir ad belirtmek istiyorsanız `HubName` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="85baf-213">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="85baf-214">Kullandığınızda, bir `HubName` özniteliği, JavaScript istemcilerde ortası büyük için ad değişiklik yoktur.</span><span class="sxs-lookup"><span data-stu-id="85baf-214">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="85baf-215">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="85baf-215">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

<span data-ttu-id="85baf-216">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="85baf-216">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="85baf-217">Birden çok hub'ları</span><span class="sxs-lookup"><span data-stu-id="85baf-217">Multiple Hubs</span></span>

<span data-ttu-id="85baf-218">Bir uygulamada birden çok Hub sınıfları tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-218">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="85baf-219">Bunu yaptığınızda, bağlantı paylaşılan ancak grupları ayrı şunlardır:</span><span class="sxs-lookup"><span data-stu-id="85baf-219">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="85baf-220">Tüm istemciler aynı URL'ye hizmetinizle bir SignalR bağlantısı kurmak için kullanır ("/ signalr" veya bir belirtilmişse özel URL'nizi), hizmet tarafından tanımlanan ve bağlantı tüm hub'ları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="85baf-220">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="85baf-221">Birden çok hub'ları tüm Hub işlevlerini tek bir sınıf tanımlama için karşılaştırılması için herhangi bir performans farkı yoktur.</span><span class="sxs-lookup"><span data-stu-id="85baf-221">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="85baf-222">Tüm hub'ları aynı HTTP isteği bilgi alın.</span><span class="sxs-lookup"><span data-stu-id="85baf-222">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="85baf-223">Tüm hub'ı aynı bağlantıyı paylaştığında olduğundan, sunucunun alır yalnızca HTTP istek bilgileri ne SignalR bağlantı kurar özgün HTTP isteği gelen kalır.</span><span class="sxs-lookup"><span data-stu-id="85baf-223">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="85baf-224">Bilgi bir sorgu dizesi belirterek istemciden sunucuya geçirmek için bağlantı isteğini kullanırsanız, farklı sorgu dizeleri için farklı hub sağlayamaz.</span><span class="sxs-lookup"><span data-stu-id="85baf-224">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="85baf-225">Tüm hub'ları aynı bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="85baf-225">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="85baf-226">Bir dosyadaki tüm hub'lara yönelik proxy'leri oluşturulan JavaScript proxy'leri dosyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="85baf-226">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="85baf-227">JavaScript proxy'leri hakkında daha fazla bilgi için bkz: [SignalR hub'ları API Kılavuzu - JavaScript istemci - oluşturulan proxy ve onu sizin için ne yaptığını](hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="85baf-227">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](hubs-api-guide-javascript-client.md#genproxy).</span></span>
- <span data-ttu-id="85baf-228">Grupları hub içinde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="85baf-228">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="85baf-229">Tanımlayabileceğiniz SignalR öğesinde bağlı istemciler alt kümeleri için yayın için Grup adı.</span><span class="sxs-lookup"><span data-stu-id="85baf-229">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="85baf-230">Grupları, her Hub için ayrı ayrı tutulur.</span><span class="sxs-lookup"><span data-stu-id="85baf-230">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="85baf-231">Örneğin, "Yöneticiler" adlı bir grup istemciler için bir kümesini içerir, `ContosoChatHub` sınıfı ve aynı grubu adını istemciler için farklı bir dizi bakın, `StockTickerHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="85baf-231">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a><span data-ttu-id="85baf-232">Kesin türü belirtilmiş hub'ları</span><span class="sxs-lookup"><span data-stu-id="85baf-232">Strongly-Typed Hubs</span></span>

<span data-ttu-id="85baf-233">İstemciniz olabilir, hub yöntemleri için bir arabirimi tanımlamak için başvuru (ve hub yöntemlerine IntelliSense etkinleştirin) türetilen hub'ından `Hub<T>` (SignalR 2.1 içinde sunulan) yerine `Hub`:</span><span class="sxs-lookup"><span data-stu-id="85baf-233">To define an interface for your hub methods that your client can reference (and enable Intellisense on your hub methods), derive your hub from `Hub<T>` (introduced in SignalR 2.1) rather than `Hub`:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="85baf-234">İstemcileri çağırabilirsiniz Hub sınıfında yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="85baf-234">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="85baf-235">İstemciden aranabilir olmasını istediğiniz hub'ındaki bir yöntem kullanıma sunmak için aşağıdaki örneklerde gösterildiği gibi genel bir yöntem bildirin.</span><span class="sxs-lookup"><span data-stu-id="85baf-235">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="85baf-236">Dönüş türü ve tüm C# yönteminde olduğu gibi karmaşık türler ve diziler dahil olmak üzere parametreleri de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-236">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="85baf-237">Alırsınız parametrelerde veya çağırana döndüren herhangi bir veri istemci ve sunucu arasında JSON kullanarak bildirilir ve karmaşık nesne bağlama ve nesne dizileri SignalR otomatik olarak yönetir.</span><span class="sxs-lookup"><span data-stu-id="85baf-237">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="85baf-238">Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin adlarında yöntemi</span><span class="sxs-lookup"><span data-stu-id="85baf-238">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="85baf-239">Varsayılan olarak, JavaScript istemcilerinin yöntem adı başlamalıdır sürümünü kullanarak Hub yöntemlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="85baf-239">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="85baf-240">SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.</span><span class="sxs-lookup"><span data-stu-id="85baf-240">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="85baf-241">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="85baf-241">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="85baf-242">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="85baf-242">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="85baf-243">İstemcilerin kullanın, eklemek farklı bir ad belirtmek istiyorsanız `HubMethodName` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="85baf-243">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="85baf-244">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="85baf-244">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="85baf-245">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="85baf-245">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="85baf-246">Zaman uyumsuz olarak yürütülecek ne zaman</span><span class="sxs-lookup"><span data-stu-id="85baf-246">When to execute asynchronously</span></span>

<span data-ttu-id="85baf-247">Yöntemi uzun süre çalışan olması veya çalışmak olup olmadığını, veritabanı arama veya bir web hizmeti çağrısı gibi bekleme içeren, döndürerek Hub yöntemini zaman uyumsuz hale bir [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (yerine `void` dönüş) veya [ Görev&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) nesne (yerine `T` dönüş türü).</span><span class="sxs-lookup"><span data-stu-id="85baf-247">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="85baf-248">Döndüğünüzde bir `Task` SignalR yöntemi nesnesinden bekler `Task` tamamlamak için ve bu yüzden yöntem çağrısı istemci kodu nasıl içinde herhangi bir fark ardından sarmalanmamış sonuç istemciye geri gönderir.</span><span class="sxs-lookup"><span data-stu-id="85baf-248">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="85baf-249">Bir Hub yöntemini olmasını zaman uyumsuz WebSocket taşıma kullandığında bağlantıyı engelliyor önler.</span><span class="sxs-lookup"><span data-stu-id="85baf-249">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="85baf-250">Hub yönteminin tamamlayana kadar bir Hub yöntemini zaman uyumlu olarak yürütür ve WebSocket taşıma olduğunda, aynı istemciden hub yöntemlerine yönelik sonraki çağrılarını engellenir.</span><span class="sxs-lookup"><span data-stu-id="85baf-250">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="85baf-251">Aynı yöntem eşzamanlı çalışacak biçimde kodlanmış veya zaman uyumsuz olarak, her iki sürümü çağırmak için çalışır JavaScript istemci kodu ve ardından aşağıdaki örnekte gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="85baf-251">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="85baf-252">**Zaman uyumlu**</span><span class="sxs-lookup"><span data-stu-id="85baf-252">**Synchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="85baf-253">**Zaman uyumsuz**</span><span class="sxs-lookup"><span data-stu-id="85baf-253">**Asynchronous**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="85baf-254">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="85baf-254">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="85baf-255">ASP.NET 4.5 içinde zaman uyumsuz yöntemleri kullanma hakkında daha fazla bilgi için bkz: [kullanarak ASP.NET MVC 4'te zaman uyumsuz yöntemleri](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="85baf-255">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="85baf-256">Aşırı tanımlama</span><span class="sxs-lookup"><span data-stu-id="85baf-256">Defining Overloads</span></span>

<span data-ttu-id="85baf-257">Bir yöntemi için aşırı tanımlamak istiyorsanız, her aşırı parametre sayısı farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="85baf-257">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="85baf-258">Farklı parametre türleri belirterek bir aşırı ayırt Hub sınıfınız derlenir ancak SignalR hizmet çağrısı aşırı birini istemcileri çalıştığınızda çalışma zamanında bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="85baf-258">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a><span data-ttu-id="85baf-259">Hub yöntemi çağrılarına ilerlemesini raporlama</span><span class="sxs-lookup"><span data-stu-id="85baf-259">Reporting progress from hub method invocations</span></span>

<span data-ttu-id="85baf-260">SignalR 2.1 için destek ekler [düzeni raporlama ilerleme](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) .NET 4. 5 ' sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="85baf-260">SignalR 2.1 adds support for the [progress reporting pattern](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) introduced in .NET 4.5.</span></span> <span data-ttu-id="85baf-261">İlerleme durumu raporlama uygulamak için tanımlama bir `IProgress<T>` istemciniz erişebilir, hub yöntem için parametre:</span><span class="sxs-lookup"><span data-stu-id="85baf-261">To implement progress reporting, define an `IProgress<T>` parameter for your hub method that your client can access:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

<span data-ttu-id="85baf-262">Uzun süre çalışan sunucu yöntemini yazılırken zaman uyumsuz gibi bir zaman uyumsuz programlama desen kullanılması daha önemlidir / beklemek yerine hub iş parçacığı engelleme.</span><span class="sxs-lookup"><span data-stu-id="85baf-262">When writing a long-running server method, it is important to use an asynchronous programming pattern like Async/ Await rather than blocking the hub thread.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="85baf-263">İstemci Hub sınıfından yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="85baf-263">How to call client methods from the Hub class</span></span>

<span data-ttu-id="85baf-264">İstemci sunucudan yöntemleri çağırmak için kullanın `Clients` Hub sınıfınız yönteminde bir özellik.</span><span class="sxs-lookup"><span data-stu-id="85baf-264">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="85baf-265">Aşağıdaki örnek, çağıran sunucu kodu gösterir `addNewMessageToPage` tüm bağlı istemcileri ve bir JavaScript istemci yöntemi tanımlar istemci kodu.</span><span class="sxs-lookup"><span data-stu-id="85baf-265">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="85baf-266">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="85baf-266">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

<span data-ttu-id="85baf-267">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="85baf-267">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

<span data-ttu-id="85baf-268">Bir istemci yöntemden dönüş değeri alınamıyor; sözdizimi gibi `int x = Clients.All.add(1,1)` çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="85baf-268">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="85baf-269">Karmaşık türler ve diziler parametreleri için belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-269">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="85baf-270">Aşağıdaki örnek karmaşık bir tür bir yöntem parametresi istemcisinde geçirir.</span><span class="sxs-lookup"><span data-stu-id="85baf-270">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="85baf-271">**Karmaşık bir nesne kullanarak bir istemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="85baf-271">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

<span data-ttu-id="85baf-272">**Karmaşık nesne tanımlar sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="85baf-272">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

<span data-ttu-id="85baf-273">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="85baf-273">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="85baf-274">Hangi istemcilerin seçerek RPC alırsınız</span><span class="sxs-lookup"><span data-stu-id="85baf-274">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="85baf-275">İstemcileri özelliği döndürür bir [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) hangi istemcilerin RPC alacak belirtmek için çeşitli seçenekler sağlayan nesne:</span><span class="sxs-lookup"><span data-stu-id="85baf-275">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="85baf-276">Bağlanan tüm istemciler.</span><span class="sxs-lookup"><span data-stu-id="85baf-276">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="85baf-277">Yalnızca çağıran istemci.</span><span class="sxs-lookup"><span data-stu-id="85baf-277">Only the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="85baf-278">Çağıran istemci dışındaki tüm istemcilerin.</span><span class="sxs-lookup"><span data-stu-id="85baf-278">All clients except the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- <span data-ttu-id="85baf-279">Bağlantı kimliği ile tanımlanan belirli bir istemci</span><span class="sxs-lookup"><span data-stu-id="85baf-279">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    <span data-ttu-id="85baf-280">Bu örnek çağırır `addContosoChatMessageToPage` çağıran istemci hakkında ve kullanarak aynı etkiye sahip `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="85baf-280">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="85baf-281">Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="85baf-281">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- <span data-ttu-id="85baf-282">Belirli bir grubun tüm bağlı istemcileri.</span><span class="sxs-lookup"><span data-stu-id="85baf-282">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- <span data-ttu-id="85baf-283">Bağlantı kimliği ile tanımlanan belirtilen istemciler dışında belirtilen gruptaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="85baf-283">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- <span data-ttu-id="85baf-284">Belirtilen bir grubundaki tüm bağlı istemcileri çağıran istemci dışındaki.</span><span class="sxs-lookup"><span data-stu-id="85baf-284">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- <span data-ttu-id="85baf-285">UserId tarafından tanımlanan belirli bir kullanıcı.</span><span class="sxs-lookup"><span data-stu-id="85baf-285">A specific user, identified by userId.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    <span data-ttu-id="85baf-286">Varsayılan olarak, `IPrincipal.Identity.Name`, ancak bu tarafından değiştirilebilir [IUserIdProvider uygulaması genel ana bilgisayar kaydetme](mapping-users-to-connections.md#IUserIdProvider).</span><span class="sxs-lookup"><span data-stu-id="85baf-286">By default, this is `IPrincipal.Identity.Name`, but this can be changed by [registering an implementation of IUserIdProvider with the global host](mapping-users-to-connections.md#IUserIdProvider).</span></span>
- <span data-ttu-id="85baf-287">Tüm istemci ve bağlantı kimlikleri listesindeki grupları.</span><span class="sxs-lookup"><span data-stu-id="85baf-287">All clients and groups in a list of connection IDs.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- <span data-ttu-id="85baf-288">Gruplarının listesi.</span><span class="sxs-lookup"><span data-stu-id="85baf-288">A list of groups.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- <span data-ttu-id="85baf-289">Bir kullanıcı adına göre.</span><span class="sxs-lookup"><span data-stu-id="85baf-289">A user by name.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- <span data-ttu-id="85baf-290">Kullanıcı adları (SignalR 2.1 içinde sunulan) listesi.</span><span class="sxs-lookup"><span data-stu-id="85baf-290">A list of user names (introduced in SignalR 2.1).</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="85baf-291">Yöntem adları için doğrulama olmaz derleme zamanı</span><span class="sxs-lookup"><span data-stu-id="85baf-291">No compile-time validation for method names</span></span>

<span data-ttu-id="85baf-292">Yöntem adı IntelliSense veya derleme zamanı doğrulamasını yoktur anlamına gelir dinamik bir nesne olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="85baf-292">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="85baf-293">İfade, çalışma zamanında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="85baf-293">The expression is evaluated at run time.</span></span> <span data-ttu-id="85baf-294">Yöntem çağrısının yürütüldüğünde, kendisine SignalR yöntem adı ve parametre değerlerini istemciye gönderir ve istemci bir yöntemi varsa adı ile eşleşen yöntem çağrılır ve parametre değerlerini geçirildi.</span><span class="sxs-lookup"><span data-stu-id="85baf-294">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="85baf-295">Eşleşen bir yöntem istemcide bulunursa, herhangi bir hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="85baf-295">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="85baf-296">Bir istemci yöntemini çağırdığınızda, arka planda istemcisi için SignalR ileten veri biçimi hakkında daha fazla bilgi için bkz: [SignalR giriş](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="85baf-296">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="85baf-297">Ad eşleştirme büyük küçük harf duyarsız yöntemini</span><span class="sxs-lookup"><span data-stu-id="85baf-297">Case-insensitive method name matching</span></span>

<span data-ttu-id="85baf-298">Yöntemi ad eşleştirme büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="85baf-298">Method name matching is case-insensitive.</span></span> <span data-ttu-id="85baf-299">Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütecek `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, veya `addContosoChatMessageToPage` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="85baf-299">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="85baf-300">Zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="85baf-300">Asynchronous execution</span></span>

<span data-ttu-id="85baf-301">Çağrı yöntemini zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="85baf-301">The method that you call executes asynchronously.</span></span> <span data-ttu-id="85baf-302">Bir istemci için bir yöntem çağrısı, belirtmediğiniz sürece istemciler veri aktarırken kod sonraki satırların tamamlamak SignalR için beklenmeden hemen yürütülmez sonra gelen herhangi bir kod yöntemi tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="85baf-302">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="85baf-303">Aşağıdaki kod örneği, iki istemci yöntemleri sırayla yürütmek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="85baf-303">The following code example shows how to execute two client methods sequentially.</span></span>

<span data-ttu-id="85baf-304">**Await (.NET 4.5) kullanma**</span><span class="sxs-lookup"><span data-stu-id="85baf-304">**Using Await (.NET 4.5)**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="85baf-305">Kullanırsanız `await` kodun sonraki satırında, yürütülmeden önce bir istemci yöntemi sonlanana kadar beklemeniz için gelmez kodun sonraki satırında, yürütülmeden önce istemcileri gerçekte iletiyi alır.</span><span class="sxs-lookup"><span data-stu-id="85baf-305">If you use `await` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="85baf-306">Bir istemci yöntem çağrısının "tamamlama" yalnızca SignalR ileti göndermek için gereken her şeyi yaptığına anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="85baf-306">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="85baf-307">İstemcileri iletisini aldığınızı doğrulama gerekiyorsa, bu mekanizma kendiniz program sahip.</span><span class="sxs-lookup"><span data-stu-id="85baf-307">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="85baf-308">Örneğin, aşağıdaki kodu yazabilirsiniz bir `MessageReceived` yöntemi hub'ı hem de `addContosoChatMessageToPage` çağrı istemcide yöntemi `MessageReceived` , yaptıktan sonra iş yapmanız gereken istemcide.</span><span class="sxs-lookup"><span data-stu-id="85baf-308">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="85baf-309">İçinde `MessageReceived` hub'ı gerçek istemci alımı ve özgün yöntem çağrısı işlenmesini hangi iş bağlıdır yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-309">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="85baf-310">Yöntem adı bir dize değişkeni kullanma</span><span class="sxs-lookup"><span data-stu-id="85baf-310">How to use a string variable as the method name</span></span>

<span data-ttu-id="85baf-311">Cast yöntemi adı olarak bir dize değişkeni kullanarak bir istemci yöntemi çağırma istiyorsanız `Clients.All` (veya `Clients.Others`, `Clients.Caller`, vs.) için `IClientProxy` ve ardından arama [Invoke (methodName, bağımsız değişken...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="85baf-311">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="85baf-312">Hub sınıfından grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="85baf-312">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="85baf-313">SignalR gruplarında yayın iletileri belirtilen kümelerine bağlı istemciler için bir yöntem sağlar.</span><span class="sxs-lookup"><span data-stu-id="85baf-313">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="85baf-314">Bir grup herhangi bir sayıda istemcileri içerebilir ve bir istemci grupları herhangi bir sayıda üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="85baf-314">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="85baf-315">Grup üyeliğini yönetmek için [Ekle](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ve [kaldırmak](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) tarafından sağlanan yöntemleri `Groups` Hub sınıfın özelliği.</span><span class="sxs-lookup"><span data-stu-id="85baf-315">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="85baf-316">Aşağıdaki örnekte gösterildiği `Groups.Add` ve `Groups.Remove` istemci kodu tarafından çağrılan Hub yöntemlerini kullanılan yöntemleri ve ardından onları çağıran tarafından JavaScript istemci kodu.</span><span class="sxs-lookup"><span data-stu-id="85baf-316">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="85baf-317">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="85baf-317">**Server**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

<span data-ttu-id="85baf-318">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="85baf-318">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

<span data-ttu-id="85baf-319">Açıkça grupları oluşturmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="85baf-319">You don't have to explicitly create groups.</span></span> <span data-ttu-id="85baf-320">Etkin bir grup çağrıda adını belirttiğiniz ilk kez otomatik olarak oluşturulur `Groups.Add`, ve bu üyelik son bağlantı kaldırdığınızda silinir.</span><span class="sxs-lookup"><span data-stu-id="85baf-320">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="85baf-321">Bir grup üyeliği listesinin veya gruplarının bir listesini almak için hiçbir API yoktur.</span><span class="sxs-lookup"><span data-stu-id="85baf-321">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="85baf-322">SignalR istemcileri ve gruplara göre iletileri gönderen bir [pub/alt model](http://en.wikipedia.org/wiki/Publish/subscribe), ve sunucu grupları ve grup üyeliklerine listelerini içermez.</span><span class="sxs-lookup"><span data-stu-id="85baf-322">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="85baf-323">Bir web grubu için bir düğüm eklediğinizde, yeni bir düğüme yayılması SignalR tutar herhangi bir durum olduğundan bu ölçeklenebilirlik, en üst düzeye çıkarmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="85baf-323">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="85baf-324">Add ve Remove yöntemlerini, zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="85baf-324">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="85baf-325">`Groups.Add` Ve `Groups.Remove` zaman uyumsuz bir yöntem yürütülemez.</span><span class="sxs-lookup"><span data-stu-id="85baf-325">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="85baf-326">Bir istemci bir gruba eklemek ve hemen Grup seçeneğini kullanarak bir ileti istemciye göndermek istediğinizden emin olmak varsa `Groups.Add` yöntemi bitmeden önce.</span><span class="sxs-lookup"><span data-stu-id="85baf-326">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="85baf-327">Aşağıdaki kod örneğinde bunun nasıl yapılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="85baf-327">The following code example shows how to do that.</span></span>

<span data-ttu-id="85baf-328">**Bir istemci bir gruba eklemek ve bu istemci Mesajlaşma**</span><span class="sxs-lookup"><span data-stu-id="85baf-328">**Adding a client to a group and then messaging that client**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="85baf-329">Grup üyeliği kalıcılığı</span><span class="sxs-lookup"><span data-stu-id="85baf-329">Group membership persistence</span></span>

<span data-ttu-id="85baf-330">SignalR bağlantıları izler, kullanıcıları değil, dolayısıyla bir kullanıcının kullanıcı her bağlandığında, bir bağlantı aynı grupta olmasını istediğiniz, çağrı zorunda `Groups.Add` edildiğinde kullanıcı yeni bir bağlantı kurar.</span><span class="sxs-lookup"><span data-stu-id="85baf-330">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="85baf-331">Geçici bir bağlantı kaybı sonra bazen SignalR bağlantısı otomatik olarak geri yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-331">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="85baf-332">Bu durumda, yeni bir bağlantı kurmadan aynı bağlantı SignalR geri yüklüyor ve bu nedenle istemcinin Grup üyeliğini otomatik olarak geri.</span><span class="sxs-lookup"><span data-stu-id="85baf-332">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="85baf-333">Bağlantı durumu grup üyelikleri de dahil olmak üzere her istemci için istemcinin gidiş-dönüş olduğundan bu geçici sonu nedeni sunucu yeniden veya hata olduğunda bile mümkündür.</span><span class="sxs-lookup"><span data-stu-id="85baf-333">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="85baf-334">Bir sunucu bağlantısı kesilse ve bağlantı zaman aşımına uğramadan önce yeni bir sunucu tarafından değiştirilirse, istemci otomatik olarak yeni sunucuya yeniden bağlanma ve bir üyesidir gruplarında yeniden kaydolun.</span><span class="sxs-lookup"><span data-stu-id="85baf-334">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="85baf-335">Bağlantı otomatik olarak bağlantı kaybı sonra geri yüklenemiyor veya bağlantı zaman aşımına uğradığında veya (örneğin, bir tarayıcı için yeni bir sayfa gittiğinde) istemci kestiğinde, grup üyelikleri kaybolur.</span><span class="sxs-lookup"><span data-stu-id="85baf-335">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="85baf-336">Kullanıcı bir sonraki bağlanışında yeni bir bağlantı olur.</span><span class="sxs-lookup"><span data-stu-id="85baf-336">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="85baf-337">Aynı kullanıcı yeni bir bağlantı kurduğunda grup üyeliklerini korumasına kullanıcılar ve gruplar ilişkilendirmeleri izlemek ve grup üyeliklerini bir kullanıcı yeni bir bağlantı kurar her zaman geri yüklemek, uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="85baf-337">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="85baf-338">Bağlantıları ve tutarsızlıklara hakkında daha fazla bilgi için bkz: [bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını](#connectionlifetime) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="85baf-338">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="85baf-339">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="85baf-339">Single-user groups</span></span>

<span data-ttu-id="85baf-340">Hangi kullanıcı bir ileti gönderdi ve hangi kullanıcıları bir ileti almalıdır bilmek için kullanıcıları ve bağlantıları arasındaki ilişkilendirmeleri izlemek SignalR genellikle kullanan uygulamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="85baf-340">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="85baf-341">Gruplar iki yaygın olarak kullanılan desenleri birini yapmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="85baf-341">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="85baf-342">Tek kullanıcı grupları.</span><span class="sxs-lookup"><span data-stu-id="85baf-342">Single-user groups.</span></span>

    <span data-ttu-id="85baf-343">Grup adı olarak kullanıcı adı belirtin ve kullanıcı bağlandığında veya yeniden bağlandığında her zaman geçerli bağlantı kimliği grubuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="85baf-343">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="85baf-344">Kullanıcıya iletileri göndermek için Grup gönderin.</span><span class="sxs-lookup"><span data-stu-id="85baf-344">To send messages to the user you send to the group.</span></span> <span data-ttu-id="85baf-345">Bu yöntem bir dezavantajı, grup kullanıcının çevrimiçi veya çevrimdışı olup olmadığını öğrenmek için bir yol sağlamaz ' dir.</span><span class="sxs-lookup"><span data-stu-id="85baf-345">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="85baf-346">Kullanıcı adı ve bağlantı kimlikleri ilişkilendirmeleri izler.</span><span class="sxs-lookup"><span data-stu-id="85baf-346">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="85baf-347">Her bir kullanıcı adı ve bir veya daha fazla bağlantı kimlikleri arasında bir ilişki bir sözlük veya veritabanında depolamak ve kullanıcı bağlandığında veya bağlantısı kesildiğinde her zaman depolanan verileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="85baf-347">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="85baf-348">Kullanıcıya iletileri göndermek için Bağlantı kimliklerinin belirtin.</span><span class="sxs-lookup"><span data-stu-id="85baf-348">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="85baf-349">Bu yöntem bir dezavantajı, daha fazla bellek sürdüğünü ' dir.</span><span class="sxs-lookup"><span data-stu-id="85baf-349">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="85baf-350">Bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını</span><span class="sxs-lookup"><span data-stu-id="85baf-350">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="85baf-351">Bir kullanıcı veya bağlı olup olmadığını izler ve kullanıcı adı ve bağlantı kimlikleri arasındaki ilişkiyi izlemek için bağlantı ömür olayları işlemek için tipik nedenleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="85baf-351">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="85baf-352">İstemcileri bağlanın veya bağlantıyı kesin zaman kendi kodunuzu çalıştırmak için geçersiz kılma `OnConnected`, `OnDisconnected`, ve `OnReconnected` Hub'ın sanal yöntemler sınıfı, aşağıdaki örnekte gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="85baf-352">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="85baf-353">OnConnected, OnDisconnected ve OnReconnected olduğunda çağrılır</span><span class="sxs-lookup"><span data-stu-id="85baf-353">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="85baf-354">Bir tarayıcı yeni bir sayfaya gider her zaman yeni bir bağlantı kurulması SignalR yürütülecek yani sahip `OnDisconnected` yöntemi arkasından `OnConnected` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="85baf-354">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="85baf-355">Yeni bir bağlantı kurulduğunda SignalR her zaman yeni bir bağlantı kimliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="85baf-355">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="85baf-356">`OnReconnected` Yaşandığında geçici sonu SignalR otomatik olarak, ne zaman bir kablo geçici olarak bağlantısı kesilir ve bağlantısı zaman aşımına uğramadan önce bağlantısı yeniden kurulmuş gibi kurtarabilirsiniz bağlantılar yöntemi çağrılır. `OnDisconnected` Yöntemi, istemci bağlantısı ve SignalR olamaz otomatik olarak yeniden, bir tarayıcı için yeni bir sayfa zaman gider gibi olduğunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="85baf-356">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="85baf-357">Bu nedenle, belirli bir istemcinin olayları olası dizisidir `OnConnected`, `OnReconnected`, `OnDisconnected`; veya `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="85baf-357">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="85baf-358">Sıra görmezsiniz `OnConnected`, `OnDisconnected`, `OnReconnected` belirli bir bağlantı için.</span><span class="sxs-lookup"><span data-stu-id="85baf-358">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="85baf-359">`OnDisconnected` Yöntemi olmayan adlı bir sunucu zaman arıza gibi bazı senaryolarda veya uygulama etki alanı geri alır.</span><span class="sxs-lookup"><span data-stu-id="85baf-359">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="85baf-360">Başka bir sunucuya gelinceye veya uygulama etki alanı kendi Geri Dönüşüm tamamlandığında, bazı istemciler yeniden bağlanın ve yangın mümkün olabilir `OnReconnected` olay.</span><span class="sxs-lookup"><span data-stu-id="85baf-360">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="85baf-361">Daha fazla bilgi için bkz: [anlama ve SignalR bağlantısı ömrü olaylarını işleme](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="85baf-361">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="85baf-362">Değil doldurulmuş arayan durumu</span><span class="sxs-lookup"><span data-stu-id="85baf-362">Caller state not populated</span></span>

<span data-ttu-id="85baf-363">Bağlantı ömrü olay işleyicisi yöntemleri içine herhangi bir durum anlamına sunucusundan denir `state` istemcideki nesne içinde doldurulmayacak `Caller` sunucudaki özelliği.</span><span class="sxs-lookup"><span data-stu-id="85baf-363">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="85baf-364">Hakkında bilgi için `state` nesne ve `Caller` özelliği, bkz: [durumu istemcileri ve Hub sınıfına arasında geçirmek nasıl](#passstate) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="85baf-364">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="85baf-365">Bağlam özelliğinden istemcisi hakkında bilgi alma</span><span class="sxs-lookup"><span data-stu-id="85baf-365">How to get information about the client from the Context property</span></span>

<span data-ttu-id="85baf-366">İstemcisi hakkında bilgi almak için `Context` Hub sınıfın özelliği.</span><span class="sxs-lookup"><span data-stu-id="85baf-366">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="85baf-367">`Context` Özelliği döndürür bir [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) aşağıdaki bilgilere erişim sağlayan nesnesi:</span><span class="sxs-lookup"><span data-stu-id="85baf-367">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="85baf-368">Çağıran istemcinin bağlantı kimliği.</span><span class="sxs-lookup"><span data-stu-id="85baf-368">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    <span data-ttu-id="85baf-369">Bağlantı kimliği (kendi kodunuzu değeri belirtemezsiniz) SignalR tarafından atanan bir GUID değeridir.</span><span class="sxs-lookup"><span data-stu-id="85baf-369">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="85baf-370">Her bağlantı ve uygulamanızda birden çok hub'lar varsa tüm hub tarafından kullanılan kimliği aynı bağlantı için bir bağlantı kimliği yok.</span><span class="sxs-lookup"><span data-stu-id="85baf-370">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="85baf-371">HTTP üstbilgisi verileri.</span><span class="sxs-lookup"><span data-stu-id="85baf-371">HTTP header data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="85baf-372">HTTP üstbilgileri elde edebilirsiniz `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="85baf-372">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="85baf-373">Aynı şey birden fazla başvuru nedeni `Context.Headers` ilk, oluşturulan `Context.Request` özelliği daha sonra eklenen ve `Context.Headers` geriye dönük uyumluluk için korunur.</span><span class="sxs-lookup"><span data-stu-id="85baf-373">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="85baf-374">Dize verilerini sorgu.</span><span class="sxs-lookup"><span data-stu-id="85baf-374">Query string data.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    <span data-ttu-id="85baf-375">Sorgu dizesi verileri elde edebilirsiniz `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="85baf-375">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="85baf-376">Bu özellik alma sorgu dizesi bir SignalR bağlantısı oluşturulmuş olan HTTP isteği kullanılan adrestir.</span><span class="sxs-lookup"><span data-stu-id="85baf-376">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="85baf-377">İstemci hakkındaki verileri istemciden sunucuya geçirmek için kullanışlı bir yoldur bağlantı yapılandırarak, sorgu dizesi parametreleri istemcisinde ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-377">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="85baf-378">Aşağıdaki örnek, oluşturulan proxy kullandığınızda bir JavaScript istemci bir sorgu dizesi eklemek için bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="85baf-378">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    <span data-ttu-id="85baf-379">Sorgu dizesi parametreleri ayarlama hakkında daha fazla bilgi için bkz. API kılavuzları için [JavaScript](hubs-api-guide-javascript-client.md) ve [.NET](hubs-api-guide-net-client.md) istemciler.</span><span class="sxs-lookup"><span data-stu-id="85baf-379">For more information about setting query string parameters, see the API guides for the [JavaScript](hubs-api-guide-javascript-client.md) and [.NET](hubs-api-guide-net-client.md) clients.</span></span>

    <span data-ttu-id="85baf-380">Sorgu dizesi verileri SignalR tarafından dahili olarak kullanılan bazı değerler birlikte bağlantı için kullanılan aktarım yöntemi bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="85baf-380">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    <span data-ttu-id="85baf-381">Değeri `transportMethod` "webSockets", "serverSentEvents", "foreverFrame" veya "longPolling" olacaktır.</span><span class="sxs-lookup"><span data-stu-id="85baf-381">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="85baf-382">Bu değer işaretlerseniz unutmayın `OnConnected` olay işleyicisi yöntemi, bazı senaryolarda bağlantı için son anlaşılan taşıma yöntemi olmayan bir taşıma değeri başlangıçta alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85baf-382">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="85baf-383">Bu durumda yöntemi bir özel durum oluşturur ve daha sonra yeniden son taşıma yöntemi kurulduğunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="85baf-383">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="85baf-384">Tanımlama bilgileri.</span><span class="sxs-lookup"><span data-stu-id="85baf-384">Cookies.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    <span data-ttu-id="85baf-385">Tanımlama bilgilerini de alabilirsiniz `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="85baf-385">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="85baf-386">Kullanıcı bilgileri.</span><span class="sxs-lookup"><span data-stu-id="85baf-386">User information.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- <span data-ttu-id="85baf-387">İsteğin HttpContext nesnesi:</span><span class="sxs-lookup"><span data-stu-id="85baf-387">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    <span data-ttu-id="85baf-388">Alma yerine bu yöntemi kullanmak `HttpContext.Current` almak için `HttpContext` SignalR bağlantı için nesnesi.</span><span class="sxs-lookup"><span data-stu-id="85baf-388">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="85baf-389">Durum istemcileri ve Hub sınıfına arasında geçirmek nasıl</span><span class="sxs-lookup"><span data-stu-id="85baf-389">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="85baf-390">İstemci proxy sağlayan bir `state` içinde depolayabilirsiniz sunucusuna her yöntem çağrısı ile iletilmesi istediğiniz veri nesnesi.</span><span class="sxs-lookup"><span data-stu-id="85baf-390">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="85baf-391">Sunucuda bu verilerine erişebilir `Clients.Caller` istemciler tarafından çağrılan Hub yöntemlerini özelliği.</span><span class="sxs-lookup"><span data-stu-id="85baf-391">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="85baf-392">`Clients.Caller` Özelliği için bağlantı ömrü olay işleyicisi yöntemleri değil doldurulmuş `OnConnected`, `OnDisconnected`, ve `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="85baf-392">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="85baf-393">Oluşturma veya güncelleştirme verilerde `state` nesne ve `Clients.Caller` özelliği her iki yönde de çalışır.</span><span class="sxs-lookup"><span data-stu-id="85baf-393">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="85baf-394">Sunucu değerlerde güncelleştirebilir ve istemciye geçirilir.</span><span class="sxs-lookup"><span data-stu-id="85baf-394">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="85baf-395">Aşağıdaki örnek durumu iletilmesi için her yöntem çağrısı sunucusuyla depolar JavaScript istemci kodu gösterir.</span><span class="sxs-lookup"><span data-stu-id="85baf-395">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

<span data-ttu-id="85baf-396">Aşağıdaki örnek, bir .NET istemci eşdeğeri olan kodu gösterir.</span><span class="sxs-lookup"><span data-stu-id="85baf-396">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

<span data-ttu-id="85baf-397">Hub sınıfınızda bu verilerine erişebilir `Clients.Caller` özelliği.</span><span class="sxs-lookup"><span data-stu-id="85baf-397">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="85baf-398">Aşağıdaki örnek, önceki örnekte başvurulan durumu alır kodu gösterir.</span><span class="sxs-lookup"><span data-stu-id="85baf-398">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="85baf-399">Bu mekanizma kalıcı durumu için her şeyi içine itibaren büyük miktarlarda verilerin amaçlanmamıştır `state` veya `Clients.Caller` özelliktir gidiş-dönüş ile her yöntem çağırma.</span><span class="sxs-lookup"><span data-stu-id="85baf-399">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="85baf-400">Kullanıcı adlarını veya sayaçları gibi küçük öğeler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="85baf-400">It's useful for smaller items such as user names or counters.</span></span>


<span data-ttu-id="85baf-401">VB.NET veya kesin türü belirtilmiş bir hub'de, çağıran durum nesnesi aracılığıyla erişilemez durumda `Clients.Caller`; bunun yerine, kullanın `Clients.CallerState` (SignalR 2.1 içinde sunulan):</span><span class="sxs-lookup"><span data-stu-id="85baf-401">In VB.NET or in a strongly-typed hub, the caller state object can't be accessed through `Clients.Caller`; instead, use `Clients.CallerState` (introduced in SignalR 2.1):</span></span>

<span data-ttu-id="85baf-402">**CallerState C# kullanarak**</span><span class="sxs-lookup"><span data-stu-id="85baf-402">**Using CallerState in C#**</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

<span data-ttu-id="85baf-403">**Visual Basic'te CallerState kullanma**</span><span class="sxs-lookup"><span data-stu-id="85baf-403">**Using CallerState in Visual Basic**</span></span>

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="85baf-404">Hub sınıfında hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="85baf-404">How to handle errors in the Hub class</span></span>

<span data-ttu-id="85baf-405">Hub sınıfı yöntemlerinizi oluşan hataları işlemek için bir veya daha fazla aşağıdaki yöntemlerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="85baf-405">To handle errors that occur in your Hub class methods, use one or more of the following methods:</span></span>

- <span data-ttu-id="85baf-406">Try-catch bloklarını yöntemi kodunuzu kaydırma ve özel durum nesnesi oturum açın.</span><span class="sxs-lookup"><span data-stu-id="85baf-406">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="85baf-407">Hata ayıklama amacıyla istemciye özel gönderebilir, ancak güvenlik için üretim istemciler için ayrıntılı bilgi gönderme nedeniyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="85baf-407">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="85baf-408">İşleme bir hub ardışık düzen modül oluşturma [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="85baf-408">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="85baf-409">Aşağıdaki örnek kod hub ardışık düzenine Modülü yerleştirir haline ve ardından hatalarını günlüğe bir ardışık düzen modülü gösterir.</span><span class="sxs-lookup"><span data-stu-id="85baf-409">The following example shows a pipeline module that logs errors, followed by code in Startup.cs that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- <span data-ttu-id="85baf-410">Kullanım `HubException` (SignalR 2'de sunulmuştur) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="85baf-410">Use the `HubException` class (introduced in SignalR 2).</span></span> <span data-ttu-id="85baf-411">Bu hata, tüm hub çağrısından durum.</span><span class="sxs-lookup"><span data-stu-id="85baf-411">This error can be thrown from any hub invocation.</span></span> <span data-ttu-id="85baf-412">`HubError` Oluşturucusu dize ileti ve ek hata verileri depolamak için bir nesne alır.</span><span class="sxs-lookup"><span data-stu-id="85baf-412">The `HubError` constructor takes a string message, and an object for storing extra error data.</span></span> <span data-ttu-id="85baf-413">SignalR otomatik-özel durum seri hale getirmek ve burada, reddetme veya hub yöntemi çağrısının başarısız kullanılacak istemciye göndermek.</span><span class="sxs-lookup"><span data-stu-id="85baf-413">SignalR will auto-serialize the exception and send it to the client, where it will be used to reject or fail the hub method invocation.</span></span>

    <span data-ttu-id="85baf-414">Aşağıdaki kod örnekleri throw göstermektedir bir `HubException` bir Hub çağrısının ve JavaScript ve .NET istemcilerde özel durum işleme sırasında.</span><span class="sxs-lookup"><span data-stu-id="85baf-414">The following code samples demonstrate how to throw a `HubException` during a Hub invocation, and how to handle the exception on JavaScript and .NET clients.</span></span>

    <span data-ttu-id="85baf-415">**HubException sınıfı gösteren sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="85baf-415">**Server code demonstrating the HubException class**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    <span data-ttu-id="85baf-416">**Bir hub bir HubException atma yanıt gösteren JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="85baf-416">**JavaScript client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    <span data-ttu-id="85baf-417">**Bir hub bir HubException atma yanıt gösteren .NET istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="85baf-417">**.NET client code demonstrating response to throwing a HubException in a hub**</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

<span data-ttu-id="85baf-418">Hub ardışık düzen modüllerine hakkında daha fazla bilgi için bkz: [hub ardışık düzen özelleştirmek nasıl](#hubpipeline) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="85baf-418">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="85baf-419">İzlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="85baf-419">How to enable tracing</span></span>

<span data-ttu-id="85baf-420">System.diagnostics öğesi sunucu-tarafı izlemeyi etkinleştirmek için Web.config dosyanıza Bu örnekte gösterildiği gibi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="85baf-420">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

<span data-ttu-id="85baf-421">Visual Studio'da uygulamayı çalıştırdığınızda, günlükleri görüntüleyebilirsiniz **çıkış** penceresi.</span><span class="sxs-lookup"><span data-stu-id="85baf-421">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="85baf-422">İstemci yöntemlerini çağırın ve gruplardan Hub sınıfın dışından yönetme</span><span class="sxs-lookup"><span data-stu-id="85baf-422">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="85baf-423">İstemci Hub sınıfınızın daha farklı bir sınıftan yöntemleri çağırmak için Hub için SignalR bağlamı nesneye bir başvurusu alın ve, istemcide yöntemlerini çağıran veya grupları yönetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="85baf-423">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="85baf-424">Aşağıdaki örnek `StockTicker` sınıfı context nesnesi alır, sınıfının bir örneğini depolar, bir statik özellik sınıf örneği depolar ve çağırmak için singleton sınıfı örneği bağlamından kullanır `updateStockPrice` istemciler üzerinde yöntemi adlı bir Hub'ına bağlı `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="85baf-424">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

<span data-ttu-id="85baf-425">Uzun süreli bir nesne bağlamı birden çok kez kullanmanız gerekiyorsa, bir kez başvurusu alın ve yerine her zaman tekrar alma kaydedin.</span><span class="sxs-lookup"><span data-stu-id="85baf-425">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="85baf-426">Bağlamı bir kez alma SignalR içinde ve Hub yöntemlerine istemci yöntem çağrılarına olun aynı sırayla istemcilere iletileri gönderir sağlar.</span><span class="sxs-lookup"><span data-stu-id="85baf-426">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="85baf-427">SignalR bağlamı için bir hub'ı kullanmak nasıl oluşturulduğunu gösteren bir öğretici için bkz [Server yayını ASP.NET SignalR ile](../getting-started/tutorial-server-broadcast-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="85baf-427">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="85baf-428">İstemci yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="85baf-428">Calling client methods</span></span>

<span data-ttu-id="85baf-429">Hangi istemcilerin RPC alacak belirtebilirsiniz, ancak bir Hub sınıftan çağırdığınızda değerinden daha az seçeneğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="85baf-429">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="85baf-430">Bunun nedeni herhangi bir yöntem gibi geçerli bağlantı kimliği bilgisi gerektiren bağlamı bir istemciden belirli çağrısıyla ilişkili olmadığından emin olup `Clients.Others`, veya `Clients.Caller`, veya `Clients.OthersInGroup`, kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="85baf-430">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="85baf-431">Aşağıdaki seçenekler mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="85baf-431">The following options are available:</span></span>

- <span data-ttu-id="85baf-432">Bağlanan tüm istemciler.</span><span class="sxs-lookup"><span data-stu-id="85baf-432">All connected clients.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- <span data-ttu-id="85baf-433">Bağlantı kimliği ile tanımlanan belirli bir istemci</span><span class="sxs-lookup"><span data-stu-id="85baf-433">A specific client identified by connection ID.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- <span data-ttu-id="85baf-434">Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="85baf-434">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- <span data-ttu-id="85baf-435">Belirli bir grubun tüm bağlı istemcileri.</span><span class="sxs-lookup"><span data-stu-id="85baf-435">All connected clients in a specified group.</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- <span data-ttu-id="85baf-436">Bağlantı kimliği ile tanımlanan belirtilen istemciler dışında belirtilen gruptaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="85baf-436">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

<span data-ttu-id="85baf-437">Hub sınıfınızda yöntemleri Hub bileşen sınıfına arıyorsanız, geçerli bağlantı kimliği geçirmek ve ile kullanan `Clients.Client`, `Clients.AllExcept`, veya `Clients.Group` benzetimini yapmak için `Clients.Caller`, `Clients.Others`, veya `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="85baf-437">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="85baf-438">Aşağıdaki örnekte, `MoveShapeHub` sınıfı için bağlantı kimliği geçirir `Broadcaster` sınıfı böylece `Broadcaster` sınıfı benzetimini `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="85baf-438">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="85baf-439">Grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="85baf-439">Managing group membership</span></span>

<span data-ttu-id="85baf-440">Bir Hub sınıfta yaptığınız gibi grupları yönetmek için aynı seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="85baf-440">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="85baf-441">Bir gruba istemci Ekle</span><span class="sxs-lookup"><span data-stu-id="85baf-441">Add a client to a group</span></span>

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- <span data-ttu-id="85baf-442">Bir istemci bir gruptan kaldırma</span><span class="sxs-lookup"><span data-stu-id="85baf-442">Remove a client from a group</span></span>

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="85baf-443">Hub ardışık düzen özelleştirme</span><span class="sxs-lookup"><span data-stu-id="85baf-443">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="85baf-444">SignalR Hub ardışık düzenine kendi kodunuzu eklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="85baf-444">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="85baf-445">Aşağıdaki örnek, istemci ve istemci üzerinde çağrılan giden yöntem çağrısı alınan gelen her yöntem çağrısı günlüklerini özel bir Hub ardışık düzen modülü gösterir:</span><span class="sxs-lookup"><span data-stu-id="85baf-445">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

<span data-ttu-id="85baf-446">Aşağıdaki kod *haline* dosyayı Hub ardışık düzeninde çalıştırmak için modülü kaydeder:</span><span class="sxs-lookup"><span data-stu-id="85baf-446">The following code in the *Startup.cs* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

<span data-ttu-id="85baf-447">Geçersiz kılabilirsiniz birçok farklı yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="85baf-447">There are many different methods that you can override.</span></span> <span data-ttu-id="85baf-448">Tam bir listesi için bkz: [HubPipelineModule yöntemleri](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="85baf-448">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
