---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: "ASP.NET SignalR hub'ları API Kılavuzu - sunucu (SignalR 1.x) | Microsoft Docs"
author: pfletcher
description: "Bu belge için SignalR sürüm 1.1, kod örnekleri demonstratin ile ASP.NET SignalR hub'ları API sunucu tarafı programlama için bir giriş sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: e594dd1ea4ae027cf0b82574fc5a3eb061b1f2e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="dc71b-103">ASP.NET SignalR hub'ları API Kılavuzu - sunucu (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="dc71b-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="dc71b-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="dc71b-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="dc71b-105">Bu belge, ASP.NET SignalR hub'ları API sunucu tarafı sürüm 1.1, SignalR için ortak seçeneklerini gösteren kod örnekleri ile programlama giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="dc71b-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="dc71b-106">SignalR hub'ları API bir sunucuya bağlanan istemciler ve sunucu istemcilerine uzaktan yordam çağrılarını (RPC) yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="dc71b-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="dc71b-107">Sunucu kodu, istemciler tarafından çağrılabilir yöntemlerini tanımlama ve istemci üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="dc71b-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="dc71b-108">İstemci kodu sunucudan çağrılabilir yöntemlerini tanımlama ve sunucu üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="dc71b-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="dc71b-109">SignalR, istemci-sunucu tesisat tümünün ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="dc71b-110">SignalR kalıcı bağlantılar olarak adlandırılan bir alt düzey API de sunar.</span><span class="sxs-lookup"><span data-stu-id="dc71b-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="dc71b-111">Giriş SignalR, hub'lar ve kalıcı bağlantılar için ya da tam bir SignalR uygulamasının nasıl oluşturulacağını gösteren bir öğretici için bkz: [Başlarken SignalR -](index.md).</span><span class="sxs-lookup"><span data-stu-id="dc71b-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="dc71b-112">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="dc71b-112">Overview</span></span>

<span data-ttu-id="dc71b-113">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="dc71b-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="dc71b-114">SignalR rota kaydetmek ve SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dc71b-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="dc71b-115">/Signalr URL'si</span><span class="sxs-lookup"><span data-stu-id="dc71b-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="dc71b-116">SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dc71b-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="dc71b-117">Oluşturma ve Hub sınıfları kullanma</span><span class="sxs-lookup"><span data-stu-id="dc71b-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="dc71b-118">Hub nesne ömrü</span><span class="sxs-lookup"><span data-stu-id="dc71b-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="dc71b-119">Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin Hub adları</span><span class="sxs-lookup"><span data-stu-id="dc71b-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="dc71b-120">Birden çok hub'ları</span><span class="sxs-lookup"><span data-stu-id="dc71b-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="dc71b-121">İstemcileri çağırabilirsiniz Hub sınıfında yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="dc71b-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="dc71b-122">Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin adlarında yöntemi</span><span class="sxs-lookup"><span data-stu-id="dc71b-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="dc71b-123">Zaman uyumsuz olarak yürütülecek ne zaman</span><span class="sxs-lookup"><span data-stu-id="dc71b-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="dc71b-124">Aşırı tanımlama</span><span class="sxs-lookup"><span data-stu-id="dc71b-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="dc71b-125">İstemci Hub sınıfından yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="dc71b-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="dc71b-126">Hangi istemcilerin seçerek RPC alırsınız</span><span class="sxs-lookup"><span data-stu-id="dc71b-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="dc71b-127">Yöntem adları için doğrulama olmaz derleme zamanı</span><span class="sxs-lookup"><span data-stu-id="dc71b-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="dc71b-128">Ad eşleştirme büyük küçük harf duyarsız yöntemini</span><span class="sxs-lookup"><span data-stu-id="dc71b-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="dc71b-129">Zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="dc71b-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="dc71b-130">Hub sınıfından grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="dc71b-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="dc71b-131">Add ve Remove yöntemlerini, zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="dc71b-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="dc71b-132">Grup üyeliği kalıcılığı</span><span class="sxs-lookup"><span data-stu-id="dc71b-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="dc71b-133">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="dc71b-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="dc71b-134">Bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını</span><span class="sxs-lookup"><span data-stu-id="dc71b-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="dc71b-135">OnConnected, OnDisconnected ve OnReconnected olduğunda çağrılır</span><span class="sxs-lookup"><span data-stu-id="dc71b-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="dc71b-136">Değil doldurulmuş arayan durumu</span><span class="sxs-lookup"><span data-stu-id="dc71b-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="dc71b-137">Bağlam özelliğinden istemcisi hakkında bilgi alma</span><span class="sxs-lookup"><span data-stu-id="dc71b-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="dc71b-138">Durum istemcileri ve Hub sınıfına arasında geçirmek nasıl</span><span class="sxs-lookup"><span data-stu-id="dc71b-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="dc71b-139">Hub sınıfında hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="dc71b-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="dc71b-140">İstemci yöntemlerini çağırın ve gruplardan Hub sınıfın dışından yönetme</span><span class="sxs-lookup"><span data-stu-id="dc71b-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="dc71b-141">İstemci yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="dc71b-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="dc71b-142">Grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="dc71b-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="dc71b-143">İzlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="dc71b-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="dc71b-144">Hub ardışık düzen özelleştirme</span><span class="sxs-lookup"><span data-stu-id="dc71b-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="dc71b-145">Program istemcilere nasıl belgeler için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="dc71b-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="dc71b-146">SignalR hub'ları API Kılavuzu - JavaScript istemci</span><span class="sxs-lookup"><span data-stu-id="dc71b-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="dc71b-147">SignalR hub'ları API Kılavuzu - .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="dc71b-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="dc71b-148">API başvuru konuları API'si .NET 4.5 sürümüne bağlantılardır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="dc71b-149">.NET 4 kullanıyorsanız, bkz: [API konuları .NET 4 sürümü](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="dc71b-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/en-us/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="dc71b-150">SignalR rota kaydetmek ve SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dc71b-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="dc71b-151">İstemcilerin Hub'ınıza bağlanmak için kullanacağı rota tanımlamak için arama [MapHubs](https://msdn.microsoft.com/en-us/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) uygulama başladığında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dc71b-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/en-us/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="dc71b-152">`MapHubs`olan bir [genişletme yöntemi](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) için `System.Web.Routing.RouteCollection` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="dc71b-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="dc71b-153">Aşağıdaki örnek SignalR hub'ları rotadaki tanımlamak nasıl gösterir *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="dc71b-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="dc71b-154">Bir ASP.NET MVC uygulaması için SignalR işlevselliği ekliyorsanız, SignalR rota diğer rotaların önce eklendiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="dc71b-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="dc71b-155">Daha fazla bilgi için bkz: [Öğreticisi: SignalR ve MVC 4 ile çalışmaya başlama](index.md).</span><span class="sxs-lookup"><span data-stu-id="dc71b-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="dc71b-156">/Signalr URL'si</span><span class="sxs-lookup"><span data-stu-id="dc71b-156">The /signalr URL</span></span>

<span data-ttu-id="dc71b-157">Varsayılan olarak, istemciler Hub'ınıza bağlanmak için kullanacağı rota URL'dir "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="dc71b-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="dc71b-158">(Bu URL için otomatik olarak oluşturulan JavaScript dosyası "/ signalr/hub" URL ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="dc71b-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="dc71b-159">Oluşturulan proxy hakkında daha fazla bilgi için bkz: [SignalR hub'ları API Kılavuzu - JavaScript istemci - oluşturulan proxy ve onu sizin için ne yaptığını](index.md).)</span><span class="sxs-lookup"><span data-stu-id="dc71b-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="dc71b-160">Bu temel URL SignalR için kullanılamaz hale olağanüstü durumlar olabilir; Örneğin, bir klasör adında projenizde var. *signalr* ve adını değiştirmek istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="dc71b-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="dc71b-161">Bu durumda, aşağıdaki örneklerde gösterildiği gibi temel URL değiştirebilirsiniz (Değiştir "/ signalr" örnek kodda, istenen URL ile).</span><span class="sxs-lookup"><span data-stu-id="dc71b-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="dc71b-162">**URL'yi belirtir sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="dc71b-163">**URL (ile oluşturulan proxy) belirtir JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="dc71b-164">**(Olmadan oluşturulan proxy) URL'yi belirtir JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="dc71b-165">**URL'yi belirtir .NET istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="dc71b-166">SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dc71b-166">Configuring SignalR Options</span></span>

<span data-ttu-id="dc71b-167">Overloads `MapHubs` yöntemini etkinleştirmek, özel bir URL, özel bağımlılık çözümleyici ve aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="dc71b-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="dc71b-168">Etki alanları arası çağrılar tarayıcı istemcilerinden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="dc71b-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="dc71b-169">Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantısı `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="dc71b-170">Varsa sayfasından `http://contoso.com` bir bağlantı kurar `http://fabrikam.com/signalr`, yani etki alanları arası bağlantı.</span><span class="sxs-lookup"><span data-stu-id="dc71b-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="dc71b-171">Güvenlik nedenleriyle, etki alanları arası bağlantılar, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="dc71b-172">Daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - JavaScript istemci - etki alanları arası bağlantı kurmak nasıl](index.md).</span><span class="sxs-lookup"><span data-stu-id="dc71b-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="dc71b-173">Ayrıntılı hata iletilerini etkinleştir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="dc71b-174">Hatalar oluştuğunda, SignalR varsayılan davranışını ne hakkında ayrıntılar olmadan bir bildirim iletisi istemcilere göndermektir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="dc71b-175">Kötü niyetli kullanıcılar, uygulamanızın saldırıları bilgileri kullanmak olabilir çünkü istemciler için ayrıntılı hata bilgileri gönderme üretimde önerilmez.</span><span class="sxs-lookup"><span data-stu-id="dc71b-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="dc71b-176">Sorun giderme için geçici olarak daha bilgilendirici hata raporlamayı etkinleştirmek için bu seçeneği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="dc71b-177">Otomatik olarak oluşturulan JavaScript proxy dosyaları devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="dc71b-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="dc71b-178">Varsayılan olarak, yanıt URL "/ signalr/hubs" olarak Hub sınıfları için proxy ile bir JavaScript dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="dc71b-179">JavaScript proxy'leri kullanmak istemiyorsanız veya bu dosyayı el ile oluşturmak ve fiziksel bir dosyaya istemcileriniz başvurmak istiyorsanız proxy oluşturması devre dışı bırakmak için bu seçeneği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="dc71b-180">Daha fazla bilgi için bkz: [SignalR hub'ları API Kılavuzu - JavaScript istemci - için SignalR fiziksel bir dosya oluşturmak nasıl oluşturulan proxy](index.md).</span><span class="sxs-lookup"><span data-stu-id="dc71b-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="dc71b-181">Aşağıdaki örnek, bir çağrıda SignalR bağlantı URL'si ve bu seçeneklerini belirtmek gösterilmektedir `MapHubs` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dc71b-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="dc71b-182">Özel bir URL belirtmek için Değiştir "/ signalr" kullanmak istediğiniz URL ile örnekte.</span><span class="sxs-lookup"><span data-stu-id="dc71b-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="dc71b-183">Oluşturma ve Hub sınıfları kullanma</span><span class="sxs-lookup"><span data-stu-id="dc71b-183">How to create and use Hub classes</span></span>

<span data-ttu-id="dc71b-184">Bir Hub oluşturmak için türeyen bir sınıf oluşturun [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="dc71b-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="dc71b-185">Aşağıdaki örnek, sohbet uygulaması için basit bir Hub sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="dc71b-186">Bu örnekte, bir bağlı istemci çağırabilirsiniz `NewContosoChatMessage` yöntemi ve yaptığında, alınan verileri bağlanan tüm istemciler için yayımladınız.</span><span class="sxs-lookup"><span data-stu-id="dc71b-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="dc71b-187">Hub nesne ömrü</span><span class="sxs-lookup"><span data-stu-id="dc71b-187">Hub object lifetime</span></span>

<span data-ttu-id="dc71b-188">Hub sınıfının örneği yok ya da kendi kodunuzu sunucuda yöntemlerinden çağırmanıza; Tüm sizin için SignalR hub'ları ardışık düzen tarafından yapılır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="dc71b-189">SignalR ne zaman bir istemci bağlanır, bağlantısını keser veya sunucuya bir yöntem çağrısı yapar gibi bir Hub işlemi işlemek için gereken her zaman, Hub sınıfının yeni bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="dc71b-190">Hub sınıfının örnekleri geçici olduğundan, bir yöntem çağrısı sonraki durumunu korumak için kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="dc71b-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="dc71b-191">Her zaman sunucu yöntemi çağrısı ileti bir istemciden Hub sınıfı işlemlerinizi yeni bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="dc71b-192">Birden çok bağlantıları ve yöntem çağrıları aracılığıyla durumunu korumak için Hub sınıfına veya türünden türemez farklı bir sınıf bir veritabanı veya statik değişkeni gibi bazı başka bir yöntem kullanın `Hub`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="dc71b-193">Bellek verileri devam ederse, uygulama etki alanı geri dönüştürüldüğünde Hub sınıfı üzerinde statik bir değişken gibi bir yöntem kullanarak verileri kaybolur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="dc71b-194">Hub sınıf dışında çalışan kendi kodundan istemcilere iletileri göndermek istiyorsanız, bir Hub sınıfı örneğini oluşturarak bunu yapamazsınız, ancak Hub sınıfınız için SignalR bağlamı nesneye bir başvurusu alarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="dc71b-195">Daha fazla bilgi için bkz: [istemci yöntemlerini çağırın ve Hub sınıfına dışında gruplarından yönetmek nasıl](#callfromoutsidehub) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="dc71b-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="dc71b-196">Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin Hub adları</span><span class="sxs-lookup"><span data-stu-id="dc71b-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="dc71b-197">Varsayılan olarak, JavaScript istemcilerinin sınıf adı başlamalıdır sürümünü kullanarak hub'lara bakın.</span><span class="sxs-lookup"><span data-stu-id="dc71b-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="dc71b-198">SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.</span><span class="sxs-lookup"><span data-stu-id="dc71b-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="dc71b-199">Önceki örneği olarak adlandırılan `contosoChatHub` JavaScript kodu.</span><span class="sxs-lookup"><span data-stu-id="dc71b-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="dc71b-200">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="dc71b-201">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="dc71b-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="dc71b-202">İstemcilerin kullanın, eklemek farklı bir ad belirtmek istiyorsanız `HubName` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dc71b-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="dc71b-203">Kullandığınızda, bir `HubName` özniteliği, JavaScript istemcilerde ortası büyük için ad değişiklik yoktur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="dc71b-204">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="dc71b-205">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="dc71b-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="dc71b-206">Birden çok hub'ları</span><span class="sxs-lookup"><span data-stu-id="dc71b-206">Multiple Hubs</span></span>

<span data-ttu-id="dc71b-207">Bir uygulamada birden çok Hub sınıfları tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="dc71b-208">Bunu yaptığınızda, bağlantı paylaşılan ancak grupları ayrı şunlardır:</span><span class="sxs-lookup"><span data-stu-id="dc71b-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="dc71b-209">Tüm istemciler aynı URL'ye hizmetinizle bir SignalR bağlantısı kurmak için kullanır ("/ signalr" veya bir belirtilmişse özel URL'nizi), hizmet tarafından tanımlanan ve bağlantı tüm hub'ları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="dc71b-210">Birden çok hub'ları tüm Hub işlevlerini tek bir sınıf tanımlama için karşılaştırılması için herhangi bir performans farkı yoktur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="dc71b-211">Tüm hub'ları aynı HTTP isteği bilgi alın.</span><span class="sxs-lookup"><span data-stu-id="dc71b-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="dc71b-212">Tüm hub'ı aynı bağlantıyı paylaştığında olduğundan, sunucunun alır yalnızca HTTP istek bilgileri ne SignalR bağlantı kurar özgün HTTP isteği gelen kalır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="dc71b-213">Bilgi bir sorgu dizesi belirterek istemciden sunucuya geçirmek için bağlantı isteğini kullanırsanız, farklı sorgu dizeleri için farklı hub sağlayamaz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="dc71b-214">Tüm hub'ları aynı bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="dc71b-215">Bir dosyadaki tüm hub'lara yönelik proxy'leri oluşturulan JavaScript proxy'leri dosyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="dc71b-216">JavaScript proxy'leri hakkında daha fazla bilgi için bkz: [SignalR hub'ları API Kılavuzu - JavaScript istemci - oluşturulan proxy ve onu sizin için ne yaptığını](index.md).</span><span class="sxs-lookup"><span data-stu-id="dc71b-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="dc71b-217">Grupları hub içinde tanımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="dc71b-218">Tanımlayabileceğiniz SignalR öğesinde bağlı istemciler alt kümeleri için yayın için Grup adı.</span><span class="sxs-lookup"><span data-stu-id="dc71b-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="dc71b-219">Grupları, her Hub için ayrı ayrı tutulur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="dc71b-220">Örneğin, "Yöneticiler" adlı bir grup istemciler için bir kümesini içerir, `ContosoChatHub` sınıfı ve aynı grubu adını istemciler için farklı bir dizi bakın, `StockTickerHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="dc71b-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="dc71b-221">İstemcileri çağırabilirsiniz Hub sınıfında yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="dc71b-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="dc71b-222">İstemciden aranabilir olmasını istediğiniz hub'ındaki bir yöntem kullanıma sunmak için aşağıdaki örneklerde gösterildiği gibi genel bir yöntem bildirin.</span><span class="sxs-lookup"><span data-stu-id="dc71b-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="dc71b-223">Dönüş türü ve tüm C# yönteminde olduğu gibi karmaşık türler ve diziler dahil olmak üzere parametreleri de belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="dc71b-224">Alırsınız parametrelerde veya çağırana döndüren herhangi bir veri istemci ve sunucu arasında JSON kullanarak bildirilir ve karmaşık nesne bağlama ve nesne dizileri SignalR otomatik olarak yönetir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="dc71b-225">Ortası büyük-büyük küçük harf kullanımını JavaScript istemcilerinin adlarında yöntemi</span><span class="sxs-lookup"><span data-stu-id="dc71b-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="dc71b-226">Varsayılan olarak, JavaScript istemcilerinin yöntem adı başlamalıdır sürümünü kullanarak Hub yöntemlerine bakın.</span><span class="sxs-lookup"><span data-stu-id="dc71b-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="dc71b-227">SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.</span><span class="sxs-lookup"><span data-stu-id="dc71b-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="dc71b-228">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="dc71b-229">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="dc71b-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="dc71b-230">İstemcilerin kullanın, eklemek farklı bir ad belirtmek istiyorsanız `HubMethodName` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="dc71b-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="dc71b-231">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="dc71b-232">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="dc71b-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="dc71b-233">Zaman uyumsuz olarak yürütülecek ne zaman</span><span class="sxs-lookup"><span data-stu-id="dc71b-233">When to execute asynchronously</span></span>

<span data-ttu-id="dc71b-234">Yöntemi uzun süre çalışan olması veya çalışmak olup olmadığını, veritabanı arama veya bir web hizmeti çağrısı gibi bekleme içeren, döndürerek Hub yöntemini zaman uyumsuz hale bir [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (yerine `void` dönüş) veya [ Görev&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/dd321424.aspx) nesne (yerine `T` dönüş türü).</span><span class="sxs-lookup"><span data-stu-id="dc71b-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/en-us/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="dc71b-235">Döndüğünüzde bir `Task` SignalR yöntemi nesnesinden bekler `Task` tamamlamak için ve bu yüzden yöntem çağrısı istemci kodu nasıl içinde herhangi bir fark ardından sarmalanmamış sonuç istemciye geri gönderir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="dc71b-236">Bir Hub yöntemini olmasını zaman uyumsuz WebSocket taşıma kullandığında bağlantıyı engelliyor önler.</span><span class="sxs-lookup"><span data-stu-id="dc71b-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="dc71b-237">Hub yönteminin tamamlayana kadar bir Hub yöntemini zaman uyumlu olarak yürütür ve WebSocket taşıma olduğunda, aynı istemciden hub yöntemlerine yönelik sonraki çağrılarını engellenir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="dc71b-238">Aynı yöntem eşzamanlı çalışacak biçimde kodlanmış veya zaman uyumsuz olarak, her iki sürümü çağırmak için çalışır JavaScript istemci kodu ve ardından aşağıdaki örnekte gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="dc71b-239">**Zaman uyumlu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="dc71b-240">**Zaman uyumsuz - ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="dc71b-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="dc71b-241">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="dc71b-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="dc71b-242">ASP.NET 4.5 içinde zaman uyumsuz yöntemleri kullanma hakkında daha fazla bilgi için bkz: [kullanarak ASP.NET MVC 4'te zaman uyumsuz yöntemleri](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="dc71b-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="dc71b-243">Aşırı tanımlama</span><span class="sxs-lookup"><span data-stu-id="dc71b-243">Defining Overloads</span></span>

<span data-ttu-id="dc71b-244">Bir yöntemi için aşırı tanımlamak istiyorsanız, her aşırı parametre sayısı farklı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="dc71b-245">Farklı parametre türleri belirterek bir aşırı ayırt Hub sınıfınız derlenir ancak SignalR hizmet çağrısı aşırı birini istemcileri çalıştığınızda çalışma zamanında bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="dc71b-246">İstemci Hub sınıfından yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="dc71b-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="dc71b-247">İstemci sunucudan yöntemleri çağırmak için kullanın `Clients` Hub sınıfınız yönteminde bir özellik.</span><span class="sxs-lookup"><span data-stu-id="dc71b-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="dc71b-248">Aşağıdaki örnek, çağıran sunucu kodu gösterir `addNewMessageToPage` tüm bağlı istemcileri ve bir JavaScript istemci yöntemi tanımlar istemci kodu.</span><span class="sxs-lookup"><span data-stu-id="dc71b-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="dc71b-249">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="dc71b-250">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="dc71b-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="dc71b-251">Bir istemci yöntemden dönüş değeri alınamıyor; sözdizimi gibi `int x = Clients.All.add(1,1)` çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="dc71b-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="dc71b-252">Karmaşık türler ve diziler parametreleri için belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="dc71b-253">Aşağıdaki örnek karmaşık bir tür bir yöntem parametresi istemcisinde geçirir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="dc71b-254">**Karmaşık bir nesne kullanarak bir istemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="dc71b-255">**Karmaşık nesne tanımlar sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="dc71b-256">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="dc71b-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="dc71b-257">Hangi istemcilerin seçerek RPC alırsınız</span><span class="sxs-lookup"><span data-stu-id="dc71b-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="dc71b-258">İstemcileri özelliği döndürür bir [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) hangi istemcilerin RPC alacak belirtmek için çeşitli seçenekler sağlayan nesne:</span><span class="sxs-lookup"><span data-stu-id="dc71b-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="dc71b-259">Bağlanan tüm istemciler.</span><span class="sxs-lookup"><span data-stu-id="dc71b-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="dc71b-260">Yalnızca çağıran istemci.</span><span class="sxs-lookup"><span data-stu-id="dc71b-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="dc71b-261">Çağıran istemci dışındaki tüm istemcilerin.</span><span class="sxs-lookup"><span data-stu-id="dc71b-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="dc71b-262">Bağlantı kimliği ile tanımlanan belirli bir istemci</span><span class="sxs-lookup"><span data-stu-id="dc71b-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="dc71b-263">Bu örnek çağırır `addContosoChatMessageToPage` çağıran istemci hakkında ve kullanarak aynı etkiye sahip `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="dc71b-264">Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="dc71b-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="dc71b-265">Belirli bir grubun tüm bağlı istemcileri.</span><span class="sxs-lookup"><span data-stu-id="dc71b-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="dc71b-266">Bağlantı kimliği ile tanımlanan belirtilen istemciler dışında belirtilen gruptaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="dc71b-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="dc71b-267">Belirtilen bir grubundaki tüm bağlı istemcileri çağıran istemci dışındaki.</span><span class="sxs-lookup"><span data-stu-id="dc71b-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="dc71b-268">Yöntem adları için doğrulama olmaz derleme zamanı</span><span class="sxs-lookup"><span data-stu-id="dc71b-268">No compile-time validation for method names</span></span>

<span data-ttu-id="dc71b-269">Yöntem adı IntelliSense veya derleme zamanı doğrulamasını yoktur anlamına gelir dinamik bir nesne olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="dc71b-270">İfade, çalışma zamanında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="dc71b-271">Yöntem çağrısının yürütüldüğünde, kendisine SignalR yöntem adı ve parametre değerlerini istemciye gönderir ve istemci bir yöntemi varsa adı ile eşleşen yöntem çağrılır ve parametre değerlerini geçirildi.</span><span class="sxs-lookup"><span data-stu-id="dc71b-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="dc71b-272">Eşleşen bir yöntem istemcide bulunursa, herhangi bir hata oluştu.</span><span class="sxs-lookup"><span data-stu-id="dc71b-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="dc71b-273">Bir istemci yöntemini çağırdığınızda, arka planda istemcisi için SignalR ileten veri biçimi hakkında daha fazla bilgi için bkz: [SignalR giriş](index.md).</span><span class="sxs-lookup"><span data-stu-id="dc71b-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="dc71b-274">Ad eşleştirme büyük küçük harf duyarsız yöntemini</span><span class="sxs-lookup"><span data-stu-id="dc71b-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="dc71b-275">Yöntemi ad eşleştirme büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="dc71b-276">Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütecek `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, veya `addContosoChatMessageToPage` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="dc71b-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="dc71b-277">Zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="dc71b-277">Asynchronous execution</span></span>

<span data-ttu-id="dc71b-278">Çağrı yöntemini zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="dc71b-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="dc71b-279">Bir istemci için bir yöntem çağrısı, belirtmediğiniz sürece istemciler veri aktarırken kod sonraki satırların tamamlamak SignalR için beklenmeden hemen yürütülmez sonra gelen herhangi bir kod yöntemi tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="dc71b-280">İki istemci yöntemleri sırayla yürütmek nasıl Göster aşağıdaki kod örnekleri, kullanarak .NET 4.5, çalışır kod, diğeri kullanarak .NET 4'te bu works kod.</span><span class="sxs-lookup"><span data-stu-id="dc71b-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="dc71b-281">**.NET 4.5 örneği**</span><span class="sxs-lookup"><span data-stu-id="dc71b-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="dc71b-282">**.NET 4 örnek**</span><span class="sxs-lookup"><span data-stu-id="dc71b-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="dc71b-283">Kullanırsanız `await` veya `ContinueWith` kodun sonraki satırında, yürütülmeden önce bir istemci yöntemi sonlanana kadar beklemeniz için gelmez kodun sonraki satırında, yürütülmeden önce istemcileri gerçekte iletiyi alır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="dc71b-284">Bir istemci yöntem çağrısının "tamamlama" yalnızca SignalR ileti göndermek için gereken her şeyi yaptığına anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="dc71b-285">İstemcileri iletisini aldığınızı doğrulama gerekiyorsa, bu mekanizma kendiniz program sahip.</span><span class="sxs-lookup"><span data-stu-id="dc71b-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="dc71b-286">Örneğin, aşağıdaki kodu yazabilirsiniz bir `MessageReceived` yöntemi hub'ı hem de `addContosoChatMessageToPage` çağrı istemcide yöntemi `MessageReceived` , yaptıktan sonra iş yapmanız gereken istemcide.</span><span class="sxs-lookup"><span data-stu-id="dc71b-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="dc71b-287">İçinde `MessageReceived` hub'ı gerçek istemci alımı ve özgün yöntem çağrısı işlenmesini hangi iş bağlıdır yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="dc71b-288">Yöntem adı bir dize değişkeni kullanma</span><span class="sxs-lookup"><span data-stu-id="dc71b-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="dc71b-289">Cast yöntemi adı olarak bir dize değişkeni kullanarak bir istemci yöntemi çağırma istiyorsanız `Clients.All` (veya `Clients.Others`, `Clients.Caller`, vs.) için `IClientProxy` ve ardından arama [Invoke (methodName, bağımsız değişken...) ](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="dc71b-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="dc71b-290">Hub sınıfından grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="dc71b-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="dc71b-291">SignalR gruplarında yayın iletileri belirtilen kümelerine bağlı istemciler için bir yöntem sağlar.</span><span class="sxs-lookup"><span data-stu-id="dc71b-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="dc71b-292">Bir grup herhangi bir sayıda istemcileri içerebilir ve bir istemci grupları herhangi bir sayıda üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="dc71b-293">Grup üyeliğini yönetmek için [Ekle](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ve [kaldırmak](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) tarafından sağlanan yöntemleri `Groups` Hub sınıfın özelliği.</span><span class="sxs-lookup"><span data-stu-id="dc71b-293">To manage group membership, use the [Add](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="dc71b-294">Aşağıdaki örnekte gösterildiği `Groups.Add` ve `Groups.Remove` istemci kodu tarafından çağrılan Hub yöntemlerini kullanılan yöntemleri ve ardından onları çağıran tarafından JavaScript istemci kodu.</span><span class="sxs-lookup"><span data-stu-id="dc71b-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="dc71b-295">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="dc71b-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="dc71b-296">**Oluşturulan proxy kullanarak JavaScript istemci**</span><span class="sxs-lookup"><span data-stu-id="dc71b-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="dc71b-297">Açıkça grupları oluşturmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="dc71b-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="dc71b-298">Etkin bir grup çağrıda adını belirttiğiniz ilk kez otomatik olarak oluşturulur `Groups.Add`, ve bu üyelik son bağlantı kaldırdığınızda silinir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="dc71b-299">Bir grup üyeliği listesinin veya gruplarının bir listesini almak için hiçbir API yoktur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="dc71b-300">SignalR istemcileri ve gruplara göre iletileri gönderen bir [pub/alt model](http://en.wikipedia.org/wiki/Publish/subscribe), ve sunucu grupları ve grup üyeliklerine listelerini içermez.</span><span class="sxs-lookup"><span data-stu-id="dc71b-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="dc71b-301">Bir web grubu için bir düğüm eklediğinizde, yeni bir düğüme yayılması SignalR tutar herhangi bir durum olduğundan bu ölçeklenebilirlik, en üst düzeye çıkarmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="dc71b-302">Add ve Remove yöntemlerini, zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="dc71b-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="dc71b-303">`Groups.Add` Ve `Groups.Remove` zaman uyumsuz bir yöntem yürütülemez.</span><span class="sxs-lookup"><span data-stu-id="dc71b-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="dc71b-304">Bir istemci bir gruba eklemek ve hemen Grup seçeneğini kullanarak bir ileti istemciye göndermek istediğinizden emin olmak varsa `Groups.Add` yöntemi bitmeden önce.</span><span class="sxs-lookup"><span data-stu-id="dc71b-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="dc71b-305">Aşağıdaki kod örnekleri, .NET 4.5 ve .NET 4'te çalışan kod kullanarak bir tane çalışan kod kullanarak bir tane nasıl Göster</span><span class="sxs-lookup"><span data-stu-id="dc71b-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="dc71b-306">**.NET 4.5 örneği**</span><span class="sxs-lookup"><span data-stu-id="dc71b-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="dc71b-307">**.NET 4 örnek**</span><span class="sxs-lookup"><span data-stu-id="dc71b-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="dc71b-308">Grup üyeliği kalıcılığı</span><span class="sxs-lookup"><span data-stu-id="dc71b-308">Group membership persistence</span></span>

<span data-ttu-id="dc71b-309">SignalR bağlantıları izler, kullanıcıları değil, dolayısıyla bir kullanıcının kullanıcı her bağlandığında, bir bağlantı aynı grupta olmasını istediğiniz, çağrı zorunda `Groups.Add` edildiğinde kullanıcı yeni bir bağlantı kurar.</span><span class="sxs-lookup"><span data-stu-id="dc71b-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="dc71b-310">Geçici bir bağlantı kaybı sonra bazen SignalR bağlantısı otomatik olarak geri yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="dc71b-311">Bu durumda, yeni bir bağlantı kurmadan aynı bağlantı SignalR geri yüklüyor ve bu nedenle istemcinin Grup üyeliğini otomatik olarak geri.</span><span class="sxs-lookup"><span data-stu-id="dc71b-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="dc71b-312">Bağlantı durumu grup üyelikleri de dahil olmak üzere her istemci için istemcinin gidiş-dönüş olduğundan bu geçici sonu nedeni sunucu yeniden veya hata olduğunda bile mümkündür.</span><span class="sxs-lookup"><span data-stu-id="dc71b-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="dc71b-313">Bir sunucu bağlantısı kesilse ve bağlantı zaman aşımına uğramadan önce yeni bir sunucu tarafından değiştirilirse, istemci otomatik olarak yeni sunucuya yeniden bağlanma ve bir üyesidir gruplarında yeniden kaydolun.</span><span class="sxs-lookup"><span data-stu-id="dc71b-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="dc71b-314">Bağlantı otomatik olarak bağlantı kaybı sonra geri yüklenemiyor veya bağlantı zaman aşımına uğradığında veya (örneğin, bir tarayıcı için yeni bir sayfa gittiğinde) istemci kestiğinde, grup üyelikleri kaybolur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="dc71b-315">Kullanıcı bir sonraki bağlanışında yeni bir bağlantı olur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="dc71b-316">Aynı kullanıcı yeni bir bağlantı kurduğunda grup üyeliklerini korumasına kullanıcılar ve gruplar ilişkilendirmeleri izlemek ve grup üyeliklerini bir kullanıcı yeni bir bağlantı kurar her zaman geri yüklemek, uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="dc71b-317">Bağlantıları ve tutarsızlıklara hakkında daha fazla bilgi için bkz: [bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını](#connectionlifetime) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="dc71b-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="dc71b-318">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="dc71b-318">Single-user groups</span></span>

<span data-ttu-id="dc71b-319">Hangi kullanıcı bir ileti gönderdi ve hangi kullanıcıları bir ileti almalıdır bilmek için kullanıcıları ve bağlantıları arasındaki ilişkilendirmeleri izlemek SignalR genellikle kullanan uygulamalar vardır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="dc71b-320">Gruplar iki yaygın olarak kullanılan desenleri birini yapmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="dc71b-321">Tek kullanıcı grupları.</span><span class="sxs-lookup"><span data-stu-id="dc71b-321">Single-user groups.</span></span>

    <span data-ttu-id="dc71b-322">Grup adı olarak kullanıcı adı belirtin ve kullanıcı bağlandığında veya yeniden bağlandığında her zaman geçerli bağlantı kimliği grubuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="dc71b-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="dc71b-323">Kullanıcıya iletileri göndermek için Grup gönderin.</span><span class="sxs-lookup"><span data-stu-id="dc71b-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="dc71b-324">Bu yöntem bir dezavantajı, grup kullanıcının çevrimiçi veya çevrimdışı olup olmadığını öğrenmek için bir yol sağlamaz ' dir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="dc71b-325">Kullanıcı adı ve bağlantı kimlikleri ilişkilendirmeleri izler.</span><span class="sxs-lookup"><span data-stu-id="dc71b-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="dc71b-326">Her bir kullanıcı adı ve bir veya daha fazla bağlantı kimlikleri arasında bir ilişki bir sözlük veya veritabanında depolamak ve kullanıcı bağlandığında veya bağlantısı kesildiğinde her zaman depolanan verileri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="dc71b-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="dc71b-327">Kullanıcıya iletileri göndermek için Bağlantı kimliklerinin belirtin.</span><span class="sxs-lookup"><span data-stu-id="dc71b-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="dc71b-328">Bu yöntem bir dezavantajı, daha fazla bellek sürdüğünü ' dir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="dc71b-329">Bağlantı ömür olayları Hub sınıfında nasıl ele alınacağını</span><span class="sxs-lookup"><span data-stu-id="dc71b-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="dc71b-330">Bir kullanıcı veya bağlı olup olmadığını izler ve kullanıcı adı ve bağlantı kimlikleri arasındaki ilişkiyi izlemek için bağlantı ömür olayları işlemek için tipik nedenleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="dc71b-331">İstemcileri bağlanın veya bağlantıyı kesin zaman kendi kodunuzu çalıştırmak için geçersiz kılma `OnConnected`, `OnDisconnected`, ve `OnReconnected` Hub'ın sanal yöntemler sınıfı, aşağıdaki örnekte gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="dc71b-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="dc71b-332">OnConnected, OnDisconnected ve OnReconnected olduğunda çağrılır</span><span class="sxs-lookup"><span data-stu-id="dc71b-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="dc71b-333">Bir tarayıcı yeni bir sayfaya gider her zaman yeni bir bağlantı kurulması SignalR yürütülecek yani sahip `OnDisconnected` yöntemi arkasından `OnConnected` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dc71b-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="dc71b-334">Yeni bir bağlantı kurulduğunda SignalR her zaman yeni bir bağlantı kimliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="dc71b-335">`OnReconnected` Yaşandığında geçici sonu SignalR otomatik olarak, ne zaman bir kablo geçici olarak bağlantısı kesilir ve bağlantısı zaman aşımına uğramadan önce bağlantısı yeniden kurulmuş gibi kurtarabilirsiniz bağlantılar yöntemi çağrılır. `OnDisconnected` Yöntemi, istemci bağlantısı ve SignalR olamaz otomatik olarak yeniden, bir tarayıcı için yeni bir sayfa zaman gider gibi olduğunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="dc71b-336">Bu nedenle, belirli bir istemcinin olayları olası dizisidir `OnConnected`, `OnReconnected`, `OnDisconnected`; veya `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="dc71b-337">Sıra görmezsiniz `OnConnected`, `OnDisconnected`, `OnReconnected` belirli bir bağlantı için.</span><span class="sxs-lookup"><span data-stu-id="dc71b-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="dc71b-338">`OnDisconnected` Yöntemi olmayan adlı bir sunucu zaman arıza gibi bazı senaryolarda veya uygulama etki alanı geri alır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="dc71b-339">Başka bir sunucuya gelinceye veya uygulama etki alanı kendi Geri Dönüşüm tamamlandığında, bazı istemciler yeniden bağlanın ve yangın mümkün olabilir `OnReconnected` olay.</span><span class="sxs-lookup"><span data-stu-id="dc71b-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="dc71b-340">Daha fazla bilgi için bkz: [anlama ve SignalR bağlantısı ömrü olaylarını işleme](index.md).</span><span class="sxs-lookup"><span data-stu-id="dc71b-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="dc71b-341">Değil doldurulmuş arayan durumu</span><span class="sxs-lookup"><span data-stu-id="dc71b-341">Caller state not populated</span></span>

<span data-ttu-id="dc71b-342">Bağlantı ömrü olay işleyicisi yöntemleri içine herhangi bir durum anlamına sunucusundan denir `state` istemcideki nesne içinde doldurulmayacak `Caller` sunucudaki özelliği.</span><span class="sxs-lookup"><span data-stu-id="dc71b-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="dc71b-343">Hakkında bilgi için `state` nesne ve `Caller` özelliği, bkz: [durumu istemcileri ve Hub sınıfına arasında geçirmek nasıl](#passstate) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="dc71b-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="dc71b-344">Bağlam özelliğinden istemcisi hakkında bilgi alma</span><span class="sxs-lookup"><span data-stu-id="dc71b-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="dc71b-345">İstemcisi hakkında bilgi almak için `Context` Hub sınıfın özelliği.</span><span class="sxs-lookup"><span data-stu-id="dc71b-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="dc71b-346">`Context` Özelliği döndürür bir [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) aşağıdaki bilgilere erişim sağlayan nesnesi:</span><span class="sxs-lookup"><span data-stu-id="dc71b-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/en-us/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="dc71b-347">Çağıran istemcinin bağlantı kimliği.</span><span class="sxs-lookup"><span data-stu-id="dc71b-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="dc71b-348">Bağlantı kimliği (kendi kodunuzu değeri belirtemezsiniz) SignalR tarafından atanan bir GUID değeridir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="dc71b-349">Her bağlantı ve uygulamanızda birden çok hub'lar varsa tüm hub tarafından kullanılan kimliği aynı bağlantı için bir bağlantı kimliği yok.</span><span class="sxs-lookup"><span data-stu-id="dc71b-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="dc71b-350">HTTP üstbilgisi verileri.</span><span class="sxs-lookup"><span data-stu-id="dc71b-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="dc71b-351">HTTP üstbilgileri elde edebilirsiniz `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="dc71b-352">Aynı şey birden fazla başvuru nedeni `Context.Headers` ilk, oluşturulan `Context.Request` özelliği daha sonra eklenen ve `Context.Headers` geriye dönük uyumluluk için korunur.</span><span class="sxs-lookup"><span data-stu-id="dc71b-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="dc71b-353">Dize verilerini sorgu.</span><span class="sxs-lookup"><span data-stu-id="dc71b-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="dc71b-354">Sorgu dizesi verileri elde edebilirsiniz `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="dc71b-355">Bu özellik alma sorgu dizesi bir SignalR bağlantısı oluşturulmuş olan HTTP isteği kullanılan adrestir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="dc71b-356">İstemci hakkındaki verileri istemciden sunucuya geçirmek için kullanışlı bir yoldur bağlantı yapılandırarak, sorgu dizesi parametreleri istemcisinde ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="dc71b-357">Aşağıdaki örnek, oluşturulan proxy kullandığınızda bir JavaScript istemci bir sorgu dizesi eklemek için bir yol gösterir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="dc71b-358">Sorgu dizesi parametreleri ayarlama hakkında daha fazla bilgi için bkz. API kılavuzları için [JavaScript](index.md) ve [.NET](index.md) istemciler.</span><span class="sxs-lookup"><span data-stu-id="dc71b-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="dc71b-359">Sorgu dizesi verileri SignalR tarafından dahili olarak kullanılan bazı değerler birlikte bağlantı için kullanılan aktarım yöntemi bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="dc71b-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="dc71b-360">Değeri `transportMethod` "webSockets", "serverSentEvents", "foreverFrame" veya "longPolling" olacaktır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="dc71b-361">Bu değer işaretlerseniz unutmayın `OnConnected` olay işleyicisi yöntemi, bazı senaryolarda bağlantı için son anlaşılan taşıma yöntemi olmayan bir taşıma değeri başlangıçta alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="dc71b-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="dc71b-362">Bu durumda yöntemi bir özel durum oluşturur ve daha sonra yeniden son taşıma yöntemi kurulduğunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="dc71b-363">Tanımlama bilgileri.</span><span class="sxs-lookup"><span data-stu-id="dc71b-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="dc71b-364">Tanımlama bilgilerini de alabilirsiniz `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="dc71b-365">Kullanıcı bilgileri.</span><span class="sxs-lookup"><span data-stu-id="dc71b-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="dc71b-366">İsteğin HttpContext nesnesi:</span><span class="sxs-lookup"><span data-stu-id="dc71b-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="dc71b-367">Alma yerine bu yöntemi kullanmak `HttpContext.Current` almak için `HttpContext` SignalR bağlantı için nesnesi.</span><span class="sxs-lookup"><span data-stu-id="dc71b-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="dc71b-368">Durum istemcileri ve Hub sınıfına arasında geçirmek nasıl</span><span class="sxs-lookup"><span data-stu-id="dc71b-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="dc71b-369">İstemci proxy sağlayan bir `state` içinde depolayabilirsiniz sunucusuna her yöntem çağrısı ile iletilmesi istediğiniz veri nesnesi.</span><span class="sxs-lookup"><span data-stu-id="dc71b-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="dc71b-370">Sunucuda bu verilerine erişebilir `Clients.Caller` istemciler tarafından çağrılan Hub yöntemlerini özelliği.</span><span class="sxs-lookup"><span data-stu-id="dc71b-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="dc71b-371">`Clients.Caller` Özelliği için bağlantı ömrü olay işleyicisi yöntemleri değil doldurulmuş `OnConnected`, `OnDisconnected`, ve `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="dc71b-372">Oluşturma veya güncelleştirme verilerde `state` nesne ve `Clients.Caller` özelliği her iki yönde de çalışır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="dc71b-373">Sunucu değerlerde güncelleştirebilir ve istemciye geçirilir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="dc71b-374">Aşağıdaki örnek durumu iletilmesi için her yöntem çağrısı sunucusuyla depolar JavaScript istemci kodu gösterir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="dc71b-375">Aşağıdaki örnek, bir .NET istemci eşdeğeri olan kodu gösterir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="dc71b-376">Hub sınıfınızda bu verilerine erişebilir `Clients.Caller` özelliği.</span><span class="sxs-lookup"><span data-stu-id="dc71b-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="dc71b-377">Aşağıdaki örnek, önceki örnekte başvurulan durumu alır kodu gösterir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="dc71b-378">Bu mekanizma kalıcı durumu için her şeyi içine itibaren büyük miktarlarda verilerin amaçlanmamıştır `state` veya `Clients.Caller` özelliktir gidiş-dönüş ile her yöntem çağırma.</span><span class="sxs-lookup"><span data-stu-id="dc71b-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="dc71b-379">Kullanıcı adlarını veya sayaçları gibi küçük öğeler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="dc71b-380">Hub sınıfında hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="dc71b-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="dc71b-381">Hub sınıfı yöntemlerinizi oluşan hataları işlemek için aşağıdakilerden birini veya her ikisi de aşağıdaki yöntemlerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="dc71b-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="dc71b-382">Try-catch bloklarını yöntemi kodunuzu kaydırma ve özel durum nesnesi oturum açın.</span><span class="sxs-lookup"><span data-stu-id="dc71b-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="dc71b-383">Hata ayıklama amacıyla istemciye özel gönderebilir, ancak güvenlik için üretim istemciler için ayrıntılı bilgi gönderme nedeniyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="dc71b-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="dc71b-384">İşleme bir hub ardışık düzen modül oluşturma [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dc71b-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="dc71b-385">Aşağıdaki örnek hub ardışık düzenine Modülü yerleştirir Global.asax kodda ve ardından hatalarını günlüğe bir ardışık düzen modülü gösterir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="dc71b-386">Hub ardışık düzen modüllerine hakkında daha fazla bilgi için bkz: [hub ardışık düzen özelleştirmek nasıl](#hubpipeline) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="dc71b-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="dc71b-387">İzlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="dc71b-387">How to enable tracing</span></span>

<span data-ttu-id="dc71b-388">System.diagnostics öğesi sunucu-tarafı izlemeyi etkinleştirmek için Web.config dosyanıza Bu örnekte gösterildiği gibi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="dc71b-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="dc71b-389">Visual Studio'da uygulamayı çalıştırdığınızda, günlükleri görüntüleyebilirsiniz **çıkış** penceresi.</span><span class="sxs-lookup"><span data-stu-id="dc71b-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="dc71b-390">İstemci yöntemlerini çağırın ve gruplardan Hub sınıfın dışından yönetme</span><span class="sxs-lookup"><span data-stu-id="dc71b-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="dc71b-391">İstemci Hub sınıfınızın daha farklı bir sınıftan yöntemleri çağırmak için Hub için SignalR bağlamı nesneye bir başvurusu alın ve, istemcide yöntemlerini çağıran veya grupları yönetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="dc71b-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="dc71b-392">Aşağıdaki örnek `StockTicker` sınıfı context nesnesi alır, sınıfının bir örneğini depolar, bir statik özellik sınıf örneği depolar ve çağırmak için singleton sınıfı örneği bağlamından kullanır `updateStockPrice` istemciler üzerinde yöntemi adlı bir Hub'ına bağlı `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="dc71b-393">Uzun süreli bir nesne bağlamı birden çok kez kullanmanız gerekiyorsa, bir kez başvurusu alın ve yerine her zaman tekrar alma kaydedin.</span><span class="sxs-lookup"><span data-stu-id="dc71b-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="dc71b-394">Bağlamı bir kez alma SignalR içinde ve Hub yöntemlerine istemci yöntem çağrılarına olun aynı sırayla istemcilere iletileri gönderir sağlar.</span><span class="sxs-lookup"><span data-stu-id="dc71b-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="dc71b-395">SignalR bağlamı için bir hub'ı kullanmak nasıl oluşturulduğunu gösteren bir öğretici için bkz [Server yayını ASP.NET SignalR ile](index.md).</span><span class="sxs-lookup"><span data-stu-id="dc71b-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="dc71b-396">İstemci yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="dc71b-396">Calling client methods</span></span>

<span data-ttu-id="dc71b-397">Hangi istemcilerin RPC alacak belirtebilirsiniz, ancak bir Hub sınıftan çağırdığınızda değerinden daha az seçeneğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="dc71b-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="dc71b-398">Bunun nedeni herhangi bir yöntem gibi geçerli bağlantı kimliği bilgisi gerektiren bağlamı bir istemciden belirli çağrısıyla ilişkili olmadığından emin olup `Clients.Others`, veya `Clients.Caller`, veya `Clients.OthersInGroup`, kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="dc71b-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="dc71b-399">Aşağıdaki seçenekler mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="dc71b-399">The following options are available:</span></span>

- <span data-ttu-id="dc71b-400">Bağlanan tüm istemciler.</span><span class="sxs-lookup"><span data-stu-id="dc71b-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="dc71b-401">Bağlantı kimliği ile tanımlanan belirli bir istemci</span><span class="sxs-lookup"><span data-stu-id="dc71b-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="dc71b-402">Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="dc71b-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="dc71b-403">Belirli bir grubun tüm bağlı istemcileri.</span><span class="sxs-lookup"><span data-stu-id="dc71b-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="dc71b-404">Bağlantı kimliği ile tanımlanan belirtilen istemciler dışında belirtilen gruptaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="dc71b-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="dc71b-405">Hub sınıfınızda yöntemleri Hub bileşen sınıfına arıyorsanız, geçerli bağlantı kimliği geçirmek ve ile kullanan `Clients.Client`, `Clients.AllExcept`, veya `Clients.Group` benzetimini yapmak için `Clients.Caller`, `Clients.Others`, veya `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="dc71b-406">Aşağıdaki örnekte, `MoveShapeHub` sınıfı için bağlantı kimliği geçirir `Broadcaster` sınıfı böylece `Broadcaster` sınıfı benzetimini `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="dc71b-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="dc71b-407">Grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="dc71b-407">Managing group membership</span></span>

<span data-ttu-id="dc71b-408">Bir Hub sınıfta yaptığınız gibi grupları yönetmek için aynı seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="dc71b-409">Bir gruba istemci Ekle</span><span class="sxs-lookup"><span data-stu-id="dc71b-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="dc71b-410">Bir istemci bir gruptan kaldırma</span><span class="sxs-lookup"><span data-stu-id="dc71b-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="dc71b-411">Hub ardışık düzen özelleştirme</span><span class="sxs-lookup"><span data-stu-id="dc71b-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="dc71b-412">SignalR Hub ardışık düzenine kendi kodunuzu eklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="dc71b-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="dc71b-413">Aşağıdaki örnek, istemci ve istemci üzerinde çağrılan giden yöntem çağrısı alınan gelen her yöntem çağrısı günlüklerini özel bir Hub ardışık düzen modülü gösterir:</span><span class="sxs-lookup"><span data-stu-id="dc71b-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="dc71b-414">Aşağıdaki kod *Global.asax* dosyayı Hub ardışık düzeninde çalıştırmak için modülü kaydeder:</span><span class="sxs-lookup"><span data-stu-id="dc71b-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="dc71b-415">Geçersiz kılabilirsiniz birçok farklı yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="dc71b-415">There are many different methods that you can override.</span></span> <span data-ttu-id="dc71b-416">Tam bir listesi için bkz: [HubPipelineModule yöntemleri](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="dc71b-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/en-us/library/jj918633(v=vs.111).aspx).</span></span>
