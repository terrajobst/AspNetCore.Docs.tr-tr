---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
title: ASP.NET SignalR Hubs API Kılavuzu - sunucu (SignalR 1.x) | Microsoft Docs
author: pfletcher
description: Bu belge, SignalR sürüm 1.1, kod örnekleri demonstratin ile ASP.NET SignalR hub'ları API sunucu tarafı programlama için bir giriş sağlar...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: 03e4b9f5-0fea-4d94-959f-014b2762a301
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: a51a2077e0b6cde80bc679e3a310c0c804d19d68
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288033"
---
<a name="aspnet-signalr-hubs-api-guide---server-signalr-1x"></a><span data-ttu-id="cf10c-103">ASP.NET SignalR Hubs API Kılavuzu - sunucu (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="cf10c-103">ASP.NET SignalR Hubs API Guide - Server (SignalR 1.x)</span></span>
====================
<span data-ttu-id="cf10c-104">tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="cf10c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="cf10c-105">Bu belge, sunucu tarafı ASP.NET SignalR hub'ları API sürüm 1.1, SignalR için genel seçenekleri gösteren kod örnekleri ile programlamaya giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-105">This document provides an introduction to programming the server side of the ASP.NET SignalR Hubs API for SignalR version 1.1, with code samples demonstrating common options.</span></span>
> 
> <span data-ttu-id="cf10c-106">SignalR hub'ları API, bir sunucuya bağlanan istemcilerin ve istemcilerin sunucuya uzaktan yordam çağrısı (RPC) oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="cf10c-107">Sunucu kodu, istemciler tarafından çağrılabilen yöntemleri tanımlamak ve bir istemcide çalışmasına yöntemler çağırır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="cf10c-108">İstemci kodu sunucudan çağıran yöntemleri tanımlamak ve sunucu üzerinde çalışan yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="cf10c-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="cf10c-109">SignalR tüm istemci-sunucu tesisat sizin için üstlenir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="cf10c-110">SignalR kalıcı bağlantı adlı bir alt düzey API'si de sunar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="cf10c-111">SignalR hub'ları ve kalıcı bağlantılar için giriş veya tam bir SignalR uygulamanın nasıl oluşturulacağını gösteren bir öğretici için bkz: [SignalR çalışmaya başlama -](index.md).</span><span class="sxs-lookup"><span data-stu-id="cf10c-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="cf10c-112">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="cf10c-112">Overview</span></span>

<span data-ttu-id="cf10c-113">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="cf10c-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="cf10c-114">SignalR yol kaydetmek ve SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cf10c-114">How to register the SignalR route and configure SignalR options</span></span>](#route)

    - [<span data-ttu-id="cf10c-115">/Signalr URL'si</span><span class="sxs-lookup"><span data-stu-id="cf10c-115">The /signalr URL</span></span>](#signalrurl)
    - [<span data-ttu-id="cf10c-116">SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cf10c-116">Configuring SignalR options</span></span>](#options)
- [<span data-ttu-id="cf10c-117">Oluşturma ve Hub sınıflarını kullanma</span><span class="sxs-lookup"><span data-stu-id="cf10c-117">How to create and use Hub classes</span></span>](#hubclass)

    - [<span data-ttu-id="cf10c-118">Hub nesne yaşam süresi</span><span class="sxs-lookup"><span data-stu-id="cf10c-118">Hub object lifetime</span></span>](#transience)
    - [<span data-ttu-id="cf10c-119">JavaScript istemcilerinin Hub adları camel casing</span><span class="sxs-lookup"><span data-stu-id="cf10c-119">Camel-casing of Hub names in JavaScript clients</span></span>](#hubnames)
    - [<span data-ttu-id="cf10c-120">Birden çok hub'ları</span><span class="sxs-lookup"><span data-stu-id="cf10c-120">Multiple Hubs</span></span>](#multiplehubs)
- [<span data-ttu-id="cf10c-121">İstemciler çağırabileceğiniz Hub sınıfı yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="cf10c-121">How to define methods in the Hub class that clients can call</span></span>](#hubmethods)

    - [<span data-ttu-id="cf10c-122">JavaScript istemcilerinin yöntemi adları camel casing</span><span class="sxs-lookup"><span data-stu-id="cf10c-122">Camel-casing of method names in JavaScript clients</span></span>](#methodnames)
    - [<span data-ttu-id="cf10c-123">Zaman zaman uyumsuz olarak çalıştırmak için</span><span class="sxs-lookup"><span data-stu-id="cf10c-123">When to execute asynchronously</span></span>](#asyncmethods)
    - [<span data-ttu-id="cf10c-124">Aşırı yüklemeler tanımlama</span><span class="sxs-lookup"><span data-stu-id="cf10c-124">Defining overloads</span></span>](#overloads)
- [<span data-ttu-id="cf10c-125">İstemci Hub sınıfı yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="cf10c-125">How to call client methods from the Hub class</span></span>](#callfromhub)

    - [<span data-ttu-id="cf10c-126">Hangi istemcilerin seçerek RPC alırsınız</span><span class="sxs-lookup"><span data-stu-id="cf10c-126">Selecting which clients will receive the RPC</span></span>](#selectingclients)
    - [<span data-ttu-id="cf10c-127">Yöntem adları için derleme zamanı doğrulama</span><span class="sxs-lookup"><span data-stu-id="cf10c-127">No compile-time validation for method names</span></span>](#dynamicmethodnames)
    - [<span data-ttu-id="cf10c-128">Ad eşleştirme büyük küçük harf duyarsız yöntemi</span><span class="sxs-lookup"><span data-stu-id="cf10c-128">Case-insensitive method name matching</span></span>](#caseinsensitive)
    - [<span data-ttu-id="cf10c-129">Zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="cf10c-129">Asynchronous execution</span></span>](#asyncclient)
- [<span data-ttu-id="cf10c-130">Hub sınıftan grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="cf10c-130">How to manage group membership from the Hub class</span></span>](#groupsfromhub)

    - [<span data-ttu-id="cf10c-131">Ekleme ve kaldırma yöntemlerinin, zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="cf10c-131">Asynchronous execution of Add and Remove methods</span></span>](#asyncgroupmethods)
    - [<span data-ttu-id="cf10c-132">Grup üyeliği kalıcılığı</span><span class="sxs-lookup"><span data-stu-id="cf10c-132">Group membership persistence</span></span>](#grouppersistence)
    - [<span data-ttu-id="cf10c-133">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="cf10c-133">Single-user groups</span></span>](#singleusergroups)
- [<span data-ttu-id="cf10c-134">Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="cf10c-134">How to handle connection lifetime events in the Hub class</span></span>](#connectionlifetime)

    - [<span data-ttu-id="cf10c-135">OnConnected OnDisconnected ve OnReconnected olduğunda çağırılır</span><span class="sxs-lookup"><span data-stu-id="cf10c-135">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>](#onreconnected)
    - [<span data-ttu-id="cf10c-136">Arayan durumu değil doldurulur</span><span class="sxs-lookup"><span data-stu-id="cf10c-136">Caller state not populated</span></span>](#nocallerstate)
- [<span data-ttu-id="cf10c-137">Bağlam özelliği istemci bilgilerini alma</span><span class="sxs-lookup"><span data-stu-id="cf10c-137">How to get information about the client from the Context property</span></span>](#contextproperty)
- [<span data-ttu-id="cf10c-138">Nasıl durumu Hub sınıfına ve istemciler arasında geçirme</span><span class="sxs-lookup"><span data-stu-id="cf10c-138">How to pass state between clients and the Hub class</span></span>](#passstate)
- [<span data-ttu-id="cf10c-139">Hub sınıfında hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="cf10c-139">How to handle errors in the Hub class</span></span>](#handleErrors)
- [<span data-ttu-id="cf10c-140">İstemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetme</span><span class="sxs-lookup"><span data-stu-id="cf10c-140">How to call client methods and manage groups from outside the Hub class</span></span>](#callfromoutsidehub)

    - [<span data-ttu-id="cf10c-141">İstemci yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="cf10c-141">Calling client methods</span></span>](#callingclientsoutsidehub)
    - [<span data-ttu-id="cf10c-142">Grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="cf10c-142">Managing group membership</span></span>](#managinggroupsoutsidehub)
- [<span data-ttu-id="cf10c-143">İzlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="cf10c-143">How to enable tracing</span></span>](#tracing)
- [<span data-ttu-id="cf10c-144">Hub ardışık düzeni özelleştirme</span><span class="sxs-lookup"><span data-stu-id="cf10c-144">How to customize the Hubs pipeline</span></span>](#hubpipeline)

<span data-ttu-id="cf10c-145">Program istemcilere hakkında daha fazla belge için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="cf10c-145">For documentation on how to program clients, see the following resources:</span></span>

- [<span data-ttu-id="cf10c-146">SignalR hub API Kılavuzu - JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="cf10c-146">SignalR Hubs API Guide - JavaScript Client</span></span>](index.md)
- [<span data-ttu-id="cf10c-147">SignalR hub API Kılavuzu - .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="cf10c-147">SignalR Hubs API Guide - .NET Client</span></span>](index.md)

<span data-ttu-id="cf10c-148">API başvuru konularına bağlar API .NET 4.5 sürümü var.</span><span class="sxs-lookup"><span data-stu-id="cf10c-148">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="cf10c-149">.NET 4 kullanıyorsanız, bkz. [API konuları .NET 4 sürümünü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="cf10c-149">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="route"></a>

## <a name="how-to-register-the-signalr-route-and-configure-signalr-options"></a><span data-ttu-id="cf10c-150">SignalR yol kaydetmek ve SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cf10c-150">How to register the SignalR route and configure SignalR options</span></span>

<span data-ttu-id="cf10c-151">İstemciler, Hub'ınıza bağlanmak için kullanacağı rota tanımlamak için çağrı [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) uygulama başlatıldığında yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cf10c-151">To define the route that clients will use to connect to your Hub, call the [MapHubs](https://msdn.microsoft.com/library/system.web.routing.signalrrouteextensions.maphubs(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="cf10c-152">`MapHubs` olan bir [genişletme yöntemi](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) için `System.Web.Routing.RouteCollection` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="cf10c-152">`MapHubs` is an [extension method](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) for the `System.Web.Routing.RouteCollection` class.</span></span> <span data-ttu-id="cf10c-153">Aşağıdaki örnek, bir SignalR hub'ları yolun tanımlamak gösterilmektedir *Global.asax* dosya.</span><span class="sxs-lookup"><span data-stu-id="cf10c-153">The following example shows how to define the SignalR Hubs route in the *Global.asax* file.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample1.cs)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample2.cs?highlight=5)]

<span data-ttu-id="cf10c-154">Bir ASP.NET MVC uygulaması için SignalR işlevselliği ekliyorsanız, SignalR rota diğer rotaların önce eklendiğinden emin olun.</span><span class="sxs-lookup"><span data-stu-id="cf10c-154">If you are adding SignalR functionality to an ASP.NET MVC application, make sure that the SignalR route is added before the other routes.</span></span> <span data-ttu-id="cf10c-155">Daha fazla bilgi için [Öğreticisi: SignalR ve MVC 4 ile çalışmaya başlama](index.md).</span><span class="sxs-lookup"><span data-stu-id="cf10c-155">For more information, see [Tutorial: Getting Started with SignalR and MVC 4](index.md).</span></span>

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a><span data-ttu-id="cf10c-156">/Signalr URL'si</span><span class="sxs-lookup"><span data-stu-id="cf10c-156">The /signalr URL</span></span>

<span data-ttu-id="cf10c-157">Varsayılan olarak, istemciler, Hub'ınıza bağlanmak için kullanacağı yönlendirme URL'si olan "/ signalr".</span><span class="sxs-lookup"><span data-stu-id="cf10c-157">By default, the route URL which clients will use to connect to your Hub is "/signalr".</span></span> <span data-ttu-id="cf10c-158">(Bu URL için otomatik olarak oluşturulan JavaScript dosyası "/ signalr/hubs" URL ile karıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="cf10c-158">(Don't confuse this URL with the "/signalr/hubs" URL, which is for the automatically generated JavaScript file.</span></span> <span data-ttu-id="cf10c-159">Oluşturulan proxy hakkında daha fazla bilgi için bkz: [SignalR Hubs API Kılavuzu - JavaScript istemcisi - oluşturulan proxy ve sizin için yaptığı](index.md).)</span><span class="sxs-lookup"><span data-stu-id="cf10c-159">For more information about the generated proxy, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).)</span></span>

<span data-ttu-id="cf10c-160">Bu temel URL için SignalR kullanılamaz hale olağandışı durumlar olabilir; Örneğin, projenizde adlı bir klasöre sahip *signalr* ve adını değiştirmek istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="cf10c-160">There might be extraordinary circumstances that make this base URL not usable for SignalR; for example, you have a folder in your project named *signalr* and you don't want to change the name.</span></span> <span data-ttu-id="cf10c-161">Bu durumda, aşağıdaki örneklerde gösterildiği gibi temel URL'sini değiştirebilirsiniz (Değiştir "/ signalr" örnek kodda, istenen URL ile).</span><span class="sxs-lookup"><span data-stu-id="cf10c-161">In that case, you can change the base URL, as shown in the following examples (replace "/signalr" in the sample code with your desired URL).</span></span>

<span data-ttu-id="cf10c-162">**URL'yi belirten sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-162">**Server code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample3.cs?highlight=1)]

<span data-ttu-id="cf10c-163">**JavaScript istemci URL'si (ile oluşturulan proxy) belirten bir kod**</span><span class="sxs-lookup"><span data-stu-id="cf10c-163">**JavaScript client code that specifies the URL (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample4.js?highlight=1)]

<span data-ttu-id="cf10c-164">**(Olmadan oluşturulan proxy) URL'sini belirtir, JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-164">**JavaScript client code that specifies the URL (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample5.js?highlight=1)]

<span data-ttu-id="cf10c-165">**URL'sini belirtir, .NET istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-165">**.NET client code that specifies the URL**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample6.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a><span data-ttu-id="cf10c-166">SignalR seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="cf10c-166">Configuring SignalR Options</span></span>

<span data-ttu-id="cf10c-167">Overloads biri `MapHubs` yöntemini etkinleştirmek, özel bir URL, bir özel bağımlılık çözümleyiciyi ve aşağıdaki seçenekleri belirtin:</span><span class="sxs-lookup"><span data-stu-id="cf10c-167">Overloads of the `MapHubs` method enable you to specify a custom URL, a custom dependency resolver, and the following options:</span></span>

- <span data-ttu-id="cf10c-168">Etki alanları arası çağrılar tarayıcı istemcilerinden gelen etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-168">Enable cross-domain calls from browser clients.</span></span>

    <span data-ttu-id="cf10c-169">Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantı `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-169">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="cf10c-170">Varsa sayfasından `http://contoso.com` için bir bağlantı kurar `http://fabrikam.com/signalr`, diğer bir deyişle etki alanları arası bağlantı.</span><span class="sxs-lookup"><span data-stu-id="cf10c-170">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="cf10c-171">Güvenlik nedenleriyle, etki alanları arası bağlantıları varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-171">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="cf10c-172">Daha fazla bilgi için [ASP.NET SignalR Hubs API Kılavuzu - JavaScript istemcisi - etki alanları arası bağlantı kurmak nasıl](index.md).</span><span class="sxs-lookup"><span data-stu-id="cf10c-172">For more information, see [ASP.NET SignalR Hubs API Guide - JavaScript Client - How to establish a cross-domain connection](index.md).</span></span>
- <span data-ttu-id="cf10c-173">Ayrıntılı hata iletilerini etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-173">Enable detailed error messages.</span></span>

    <span data-ttu-id="cf10c-174">Hatalar oluştuğunda, SignalR varsayılan davranışını istemciler için ne hakkında ayrıntılar olmadan bir bildirim iletisi göndermektir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-174">When errors occur, the default behavior of SignalR is to send to clients a notification message without details about what happened.</span></span> <span data-ttu-id="cf10c-175">Kötü amaçlı kullanıcıların uygulamanızı saldırıları bilgileri kullanmak mümkün olabilir çünkü istemciler için ayrıntılı hata bilgilerini göndermesini üretimde önerilmez.</span><span class="sxs-lookup"><span data-stu-id="cf10c-175">Sending detailed error information to clients is not recommended in production, because malicious users might be able to use the information in attacks against your application.</span></span> <span data-ttu-id="cf10c-176">Sorun giderme için geçici olarak daha bilgilendirici hata raporlamasını etkinleştirmek için bu seçeneği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-176">For troubleshooting, you can use this option to temporarily enable more informative error reporting.</span></span>
- <span data-ttu-id="cf10c-177">Otomatik olarak oluşturulan JavaScript proxy'si dosyaları devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="cf10c-177">Disable automatically generated JavaScript proxy files.</span></span>

    <span data-ttu-id="cf10c-178">Varsayılan olarak, yanıt URL'sine "/ signalr/hubs" Hub sınıflarınızı için proxy ile bir JavaScript dosyası oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-178">By default, a JavaScript file with proxies for your Hub classes is generated in response to the URL "/signalr/hubs".</span></span> <span data-ttu-id="cf10c-179">JavaScript proxy'leri kullanmak istemiyorsanız veya bu dosyayı el ile oluşturmanız ve istemcilerinizin fiziksel bir dosyaya başvurmak istiyorsanız, proxy oluşturma devre dışı bırakmak için bu seçeneği kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-179">If you don't want to use the JavaScript proxies, or if you want to generate this file manually and refer to a physical file in your clients, you can use this option to disable proxy generation.</span></span> <span data-ttu-id="cf10c-180">Daha fazla bilgi için [SignalR Hubs API Kılavuzu - JavaScript istemcisi - fiziksel dosya oluşturma SignalR için oluşturulan proxy](index.md).</span><span class="sxs-lookup"><span data-stu-id="cf10c-180">For more information, see [SignalR Hubs API Guide - JavaScript Client - How to create a physical file for the SignalR generated proxy](index.md).</span></span>

<span data-ttu-id="cf10c-181">Aşağıdaki örnek, bir çağrıda SignalR bağlantı URL'si ve bu seçenekleri belirtmek gösterilmektedir `MapHubs` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cf10c-181">The following example shows how to specify the SignalR connection URL and these options in a call to the `MapHubs` method.</span></span> <span data-ttu-id="cf10c-182">Özel bir URL belirtmek için Değiştir "/ signalr" örnekte URL'siyle kullanmak istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-182">To specify a custom URL, replace "/signalr" in the example with the URL that you want to use.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample7.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a><span data-ttu-id="cf10c-183">Oluşturma ve Hub sınıflarını kullanma</span><span class="sxs-lookup"><span data-stu-id="cf10c-183">How to create and use Hub classes</span></span>

<span data-ttu-id="cf10c-184">Bir Hub oluşturmak için türetilen bir sınıf oluşturma [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="cf10c-184">To create a Hub, create a class that derives from [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx).</span></span> <span data-ttu-id="cf10c-185">Aşağıdaki örnek, basit bir Hub sınıfı için bir sohbet uygulaması gösterir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-185">The following example shows a simple Hub class for a chat application.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample8.cs)]

<span data-ttu-id="cf10c-186">Bu örnekte, bir bağlı istemci çağırabilirsiniz `NewContosoChatMessage` yöntemi ve yaptığında, alınan verilerin bağlanan tüm istemciler için yayımladınız.</span><span class="sxs-lookup"><span data-stu-id="cf10c-186">In this example, a connected client can call the `NewContosoChatMessage` method, and when it does, the data received is broadcasted to all connected clients.</span></span>

<a id="transience"></a>

### <a name="hub-object-lifetime"></a><span data-ttu-id="cf10c-187">Hub nesne yaşam süresi</span><span class="sxs-lookup"><span data-stu-id="cf10c-187">Hub object lifetime</span></span>

<span data-ttu-id="cf10c-188">Hub sınıfının örneği yok ya da sunucu üzerindeki kendi koddan yöntemlerinin çağrılması; Tüm bunları sizin için SignalR hub'ları işlem hattı tarafından gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-188">You don't instantiate the Hub class or call its methods from your own code on the server; all that is done for you by the SignalR Hubs pipeline.</span></span> <span data-ttu-id="cf10c-189">SignalR hub'ı sınıfınıza yeni bir örneğini ne zaman bir istemci bağlanır, bağlantısı kesildiğinde veya sunucu için bir yöntem çağrısı yapar gibi bir Hub işlemin işlemek için gereken her zaman oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-189">SignalR creates a new instance of your Hub class each time it needs to handle a Hub operation such as when a client connects, disconnects, or makes a method call to the server.</span></span>

<span data-ttu-id="cf10c-190">Hub sınıfının örneklerini geçici olduğu için bir yöntem çağrısından sonraki durumunu korumak üzere kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="cf10c-190">Because instances of the Hub class are transient, you can't use them to maintain state from one method call to the next.</span></span> <span data-ttu-id="cf10c-191">Her sunucunun bir yöntem çağrısının bir istemciden Hub sınıfı işlemlerinizi yeni bir örneğini iletiyi alır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-191">Each time the server receives a method call from a client, a new instance of your Hub class processes the message.</span></span> <span data-ttu-id="cf10c-192">Birden fazla bağlantı ve yöntem çağrıları arasında durumu korumak için Hub sınıfına veya farklı bir sınıf türünden türemez bir veritabanı veya statik bir değişken gibi bazı başka bir yöntem kullanın `Hub`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-192">To maintain state through multiple connections and method calls, use some other method such as a database, or a static variable on the Hub class, or a different class that does not derive from `Hub`.</span></span> <span data-ttu-id="cf10c-193">Bellekteki verileri devam ediyorsa, uygulama etki alanı geri dönüştürüldüğünde Hub sınıfında statik bir değişken gibi bir yöntem kullanarak verileri kaybolur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-193">If you persist data in memory, using a method such as a static variable on the Hub class, the data will be lost when the app domain recycles.</span></span>

<span data-ttu-id="cf10c-194">Kendi koddan Hub sınıfı dışında çalışan istemciler için iletileri göndermek istiyorsanız bir Hub örneği oluşturarak bunu yapamazsınız, ancak Hub sınıfınız için bir başvuru SignalR bağlam nesnesi alarak yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-194">If you want to send messages to clients from your own code that runs outside the Hub class, you can't do it by instantiating a Hub class instance, but you can do it by getting a reference to the SignalR context object for your Hub class.</span></span> <span data-ttu-id="cf10c-195">Daha fazla bilgi için [istemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetmek nasıl](#callfromoutsidehub) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="cf10c-195">For more information, see [How to call client methods and manage groups from outside the Hub class](#callfromoutsidehub) later in this topic.</span></span>

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a><span data-ttu-id="cf10c-196">JavaScript istemcilerinin Hub adları camel casing</span><span class="sxs-lookup"><span data-stu-id="cf10c-196">Camel-casing of Hub names in JavaScript clients</span></span>

<span data-ttu-id="cf10c-197">Varsayılan olarak, JavaScript istemcilerinin, sınıf adı ortası büyük küçük harfleri sürümünü kullanarak hub'larına bakın.</span><span class="sxs-lookup"><span data-stu-id="cf10c-197">By default, JavaScript clients refer to Hubs by using a camel-cased version of the class name.</span></span> <span data-ttu-id="cf10c-198">Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-198">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span> <span data-ttu-id="cf10c-199">Önceki örneği olarak adlandırılır `contosoChatHub` JavaScript kod.</span><span class="sxs-lookup"><span data-stu-id="cf10c-199">The previous example would be referred to as `contosoChatHub` in JavaScript code.</span></span>

<span data-ttu-id="cf10c-200">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-200">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample9.cs?highlight=1)]

<span data-ttu-id="cf10c-201">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="cf10c-201">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample10.js?highlight=1)]

<span data-ttu-id="cf10c-202">İstemcilerin kullanın, eklemek farklı bir ad belirlemek istiyorsanız `HubName` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="cf10c-202">If you want to specify a different name for clients to use, add the `HubName` attribute.</span></span> <span data-ttu-id="cf10c-203">Kullandığınızda, bir `HubName` özniteliği, için JavaScript istemcilerde ortası büyük harf adı bir değişiklik yoktur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-203">When you use a `HubName` attribute, there is no name change to camel case on JavaScript clients.</span></span>

<span data-ttu-id="cf10c-204">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-204">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample11.cs?highlight=1)]

<span data-ttu-id="cf10c-205">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="cf10c-205">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample12.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a><span data-ttu-id="cf10c-206">Birden çok hub'ları</span><span class="sxs-lookup"><span data-stu-id="cf10c-206">Multiple Hubs</span></span>

<span data-ttu-id="cf10c-207">Bir uygulamada birden fazla Hub sınıfı tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-207">You can define multiple Hub classes in an application.</span></span> <span data-ttu-id="cf10c-208">Bunu yaptığınızda, paylaşılan bir bağlantısı ancak grupları ayrı:</span><span class="sxs-lookup"><span data-stu-id="cf10c-208">When you do that, the connection is shared but groups are separate:</span></span>

- <span data-ttu-id="cf10c-209">Tüm istemciler, hizmetiniz ile SignalR bağlantı kurmak için aynı URL'yi kullanır ("/ signalr" ya da bir belirttiyseniz, özel URL), hizmet tarafından tanımlanan ve bağlantı tüm hub'ları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-209">All clients will use the same URL to establish a SignalR connection with your service ("/signalr" or your custom URL if you specified one), and that connection is used for all Hubs defined by the service.</span></span>

    <span data-ttu-id="cf10c-210">Tek bir sınıfta tüm Hub işlevselliği tanımlamak için kıyasla çok sayıda hub için bir performans farkı yoktur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-210">There is no performance difference for multiple Hubs compared to defining all Hub functionality in a single class.</span></span>
- <span data-ttu-id="cf10c-211">Tüm hub'ları için aynı HTTP isteği bilgilerini edinin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-211">All Hubs get the same HTTP request information.</span></span>

    <span data-ttu-id="cf10c-212">Tüm hub'ları aynı bağlantıyı paylaşmak olduğundan, sunucunun aldığı yalnızca HTTP isteği ne SignalR bağlantı kurar özgün HTTP isteği gelen bilgilerdir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-212">Since all Hubs share the same connection, the only HTTP request information that the server gets is what comes in the original HTTP request that establishes the SignalR connection.</span></span> <span data-ttu-id="cf10c-213">Bir sorgu dizesi belirterek bilgi istemciden sunucuya geçirmek için bağlantı isteğini kullanırsanız, farklı sorgu dizeleri için farklı hub'lar sağlayamaz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-213">If you use the connection request to pass information from the client to the server by specifying a query string, you can't provide different query strings to different Hubs.</span></span> <span data-ttu-id="cf10c-214">Tüm hub'ları aynı bilgileri alır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-214">All Hubs will receive the same information.</span></span>
- <span data-ttu-id="cf10c-215">Tek bir dosyada, tüm hub'ları proxy'lerini oluşturulan JavaScript proxy'leri dosyasını içerir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-215">The generated JavaScript proxies file will contain proxies for all Hubs in one file.</span></span>

    <span data-ttu-id="cf10c-216">JavaScript proxy'si hakkında daha fazla bilgi için bkz. [SignalR Hubs API Kılavuzu - JavaScript istemcisi - oluşturulan proxy ve sizin için yaptığı](index.md).</span><span class="sxs-lookup"><span data-stu-id="cf10c-216">For information about JavaScript proxies, see [SignalR Hubs API Guide - JavaScript Client - The generated proxy and what it does for you](index.md).</span></span>
- <span data-ttu-id="cf10c-217">Hub'ları grupları tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-217">Groups are defined within Hubs.</span></span>

    <span data-ttu-id="cf10c-218">Adlandırılmış alt kümelerini bağlı istemciler için yayın gruplar SignalR öğesinde tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-218">In SignalR you can define named groups to broadcast to subsets of connected clients.</span></span> <span data-ttu-id="cf10c-219">Grupları, her Hub için ayrı olarak korunur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-219">Groups are maintained separately for each Hub.</span></span> <span data-ttu-id="cf10c-220">Örneğin, "Yöneticiler" adlı bir grup istemciler için bir dizi verilebilir, `ContosoChatHub` sınıfı ve aynı ada istemciler için farklı bir dizi bakın, `StockTickerHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="cf10c-220">For example, a group named "Administrators" would include one set of clients for your `ContosoChatHub` class, and the same group name would refer to a different set of clients for your `StockTickerHub` class.</span></span>

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a><span data-ttu-id="cf10c-221">İstemciler çağırabileceğiniz Hub sınıfı yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="cf10c-221">How to define methods in the Hub class that clients can call</span></span>

<span data-ttu-id="cf10c-222">İstemciden çağrılabilir olmasını istediğiniz Hub üzerindeki bir yöntem kullanıma sunmak için aşağıdaki örneklerde gösterildiği gibi genel bir yöntem bildirin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-222">To expose a method on the Hub that you want to be callable from the client, declare a public method, as shown in the following examples.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample14.cs?highlight=3)]

<span data-ttu-id="cf10c-223">Dönüş türü ve parametreleri, tüm C# yönteminde olduğu gibi karmaşık türler ve diziler de dahil olmak üzere belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-223">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="cf10c-224">Parametreleri almak veya çağırana döndürmesi herhangi bir veri istemci ve sunucu arasında JSON'ı kullanarak iletilir ve SignalR bağlama karmaşık nesnelerin ve nesne dizileri otomatik olarak işler.</span><span class="sxs-lookup"><span data-stu-id="cf10c-224">Any data that you receive in parameters or return to the caller is communicated between the client and the server by using JSON, and SignalR handles the binding of complex objects and arrays of objects automatically.</span></span>

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a><span data-ttu-id="cf10c-225">JavaScript istemcilerinin yöntemi adları camel casing</span><span class="sxs-lookup"><span data-stu-id="cf10c-225">Camel-casing of method names in JavaScript clients</span></span>

<span data-ttu-id="cf10c-226">Varsayılan olarak, JavaScript istemcilerinin, Hub yöntemleri için bir yöntem adı ortası büyük küçük harfleri sürümünü kullanarak bakın.</span><span class="sxs-lookup"><span data-stu-id="cf10c-226">By default, JavaScript clients refer to Hub methods by using a camel-cased version of the method name.</span></span> <span data-ttu-id="cf10c-227">Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-227">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="cf10c-228">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-228">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample15.cs?highlight=1)]

<span data-ttu-id="cf10c-229">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="cf10c-229">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample16.js?highlight=1)]

<span data-ttu-id="cf10c-230">İstemcilerin kullanın, eklemek farklı bir ad belirlemek istiyorsanız `HubMethodName` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="cf10c-230">If you want to specify a different name for clients to use, add the `HubMethodName` attribute.</span></span>

<span data-ttu-id="cf10c-231">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-231">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample17.cs?highlight=1)]

<span data-ttu-id="cf10c-232">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="cf10c-232">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a><span data-ttu-id="cf10c-233">Zaman zaman uyumsuz olarak çalıştırmak için</span><span class="sxs-lookup"><span data-stu-id="cf10c-233">When to execute asynchronously</span></span>

<span data-ttu-id="cf10c-234">Yöntemi uzun süre çalışan olması veya çalışmaya sahipse, Bekliyor, bir veritabanı araması veya bir web hizmeti çağrısı gibi ilgili, döndürerek Hub yönteminin zaman uyumsuz hale bir [görev](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (yerine `void` döndürür) veya [ Görev&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) nesne (yerine `T` dönüş türü).</span><span class="sxs-lookup"><span data-stu-id="cf10c-234">If the method will be long-running or has to do work that would involve waiting, such as a database lookup or a web service call, make the Hub method asynchronous by returning a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (in place of `void` return) or [Task&lt;T&gt;](https://msdn.microsoft.com/library/dd321424.aspx) object (in place of `T` return type).</span></span> <span data-ttu-id="cf10c-235">Döndüğünüzde bir `Task` yöntemi, SignalR nesneden bekler `Task` tamamlamak için ve istemci yöntem çağrısında nasıl kod içinde herhangi bir fark olması, sarmalanmış halden sonucu istemciye geri gönderir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-235">When you return a `Task` object from the method, SignalR waits for the `Task` to complete, and then it sends the unwrapped result back to the client, so there is no difference in how you code the method call in the client.</span></span>

<span data-ttu-id="cf10c-236">Bir Hub yöntemini olmasını zaman uyumsuz WebSocket taşıma kullandığında bağlantıya engel önler.</span><span class="sxs-lookup"><span data-stu-id="cf10c-236">Making a Hub method asynchronous avoids blocking the connection when it uses the WebSocket transport.</span></span> <span data-ttu-id="cf10c-237">Hub yönteminin tamamlanana kadar bir Hub yöntemini zaman uyumlu olarak yürütülür ve taşıma WebSocket olduğunda, aynı istemciden hub yöntemlerine yönelik sonraki çağrılarını engellenir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-237">When a Hub method executes synchronously and the transport is WebSocket, subsequent invocations of methods on the Hub from the same client are blocked until the Hub method completes.</span></span>

<span data-ttu-id="cf10c-238">Aynı yöntem eşzamanlı çalışacak biçimde kodlanmış veya zaman uyumsuz olarak çalışan iki sürümden çağırmak için JavaScript istemci kodu ardından aşağıdaki örnekte gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-238">The following example shows the same method coded to run synchronously or asynchronously, followed by JavaScript client code that works for calling either version.</span></span>

<span data-ttu-id="cf10c-239">**Zaman uyumlu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-239">**Synchronous**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample19.cs)]

<span data-ttu-id="cf10c-240">**Zaman uyumsuz - ASP.NET 4.5**</span><span class="sxs-lookup"><span data-stu-id="cf10c-240">**Asynchronous - ASP.NET 4.5**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

<span data-ttu-id="cf10c-241">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="cf10c-241">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample21.js)]

<span data-ttu-id="cf10c-242">ASP.NET 4.5 içinde zaman uyumsuz yöntemler kullanma hakkında daha fazla bilgi için bkz. [kullanarak ASP.NET MVC 4'te zaman uyumsuz yöntemleri](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="cf10c-242">For more information about how to use asynchronous methods in ASP.NET 4.5, see [Using Asynchronous Methods in ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span>

<a id="overloads"></a>

### <a name="defining-overloads"></a><span data-ttu-id="cf10c-243">Aşırı yüklemeler tanımlama</span><span class="sxs-lookup"><span data-stu-id="cf10c-243">Defining Overloads</span></span>

<span data-ttu-id="cf10c-244">Yöntemi için aşırı yüklemeleri tanımlamak istiyorsanız, her aşırı yükleme parametre sayısı farklı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-244">If you want to define overloads for a method, the number of parameters in each overload must be different.</span></span> <span data-ttu-id="cf10c-245">Farklı parametre türleri belirterek bir aşırı ayırt Hub sınıfınıza derleyeceği ancak SignalR hizmeti, çağrı aşırı istemciler çalıştığınızda çalışma zamanında bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-245">If you differentiate an overload just by specifying different parameter types, your Hub class will compile but the SignalR service will throw an exception at run time when clients try to call one of the overloads.</span></span>

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a><span data-ttu-id="cf10c-246">İstemci Hub sınıfı yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="cf10c-246">How to call client methods from the Hub class</span></span>

<span data-ttu-id="cf10c-247">İstemcisi, sunucudan yöntemleri çağırmak için kullanın `Clients` Hub sınıfınızın bir yöntemde bir özellik.</span><span class="sxs-lookup"><span data-stu-id="cf10c-247">To call client methods from the server, use the `Clients` property in a method in your Hub class.</span></span> <span data-ttu-id="cf10c-248">Aşağıdaki örnek, çağıran sunucu kodu gösterir `addNewMessageToPage` tüm bağlı istemcileri ve istemci kodu, bir JavaScript istemci yöntemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-248">The following example shows server code that calls `addNewMessageToPage` on all connected clients, and client code that defines the method in a JavaScript client.</span></span>

<span data-ttu-id="cf10c-249">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-249">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample22.cs?highlight=5)]

<span data-ttu-id="cf10c-250">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="cf10c-250">**JavaScript client using generated proxy**</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample23.html?highlight=1)]

<span data-ttu-id="cf10c-251">İstemci yöntemden dönüş değeri alınamıyor; söz dizimi gibi `int x = Clients.All.add(1,1)` çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="cf10c-251">You can't get a return value from a client method; syntax such as `int x = Clients.All.add(1,1)` does not work.</span></span>

<span data-ttu-id="cf10c-252">Karmaşık türler ve diziler için parametreleri belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-252">You can specify complex types and arrays for the parameters.</span></span> <span data-ttu-id="cf10c-253">Aşağıdaki örnek bir yöntem parametresi istemci bir karmaşık tür geçirir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-253">The following example passes a complex type to the client in a method parameter.</span></span>

<span data-ttu-id="cf10c-254">**Karmaşık bir nesne kullanarak bir istemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-254">**Server code that calls a client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample24.cs?highlight=3)]

<span data-ttu-id="cf10c-255">**Karmaşık bir nesne tanımlayan bir sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-255">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample25.cs?highlight=1)]

<span data-ttu-id="cf10c-256">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="cf10c-256">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample26.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a><span data-ttu-id="cf10c-257">Hangi istemcilerin seçerek RPC alırsınız</span><span class="sxs-lookup"><span data-stu-id="cf10c-257">Selecting which clients will receive the RPC</span></span>

<span data-ttu-id="cf10c-258">İstemciler özelliği döndürür bir [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) hangi istemcilerin RPC alırsınız belirtmek için çeşitli seçenekler sağlayan nesne:</span><span class="sxs-lookup"><span data-stu-id="cf10c-258">The Clients property returns a [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) object that provides several options for specifying which clients will receive the RPC:</span></span>

- <span data-ttu-id="cf10c-259">Bağlanan tüm istemciler.</span><span class="sxs-lookup"><span data-stu-id="cf10c-259">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample27.cs)]
- <span data-ttu-id="cf10c-260">Çağıran istemci yalnızca.</span><span class="sxs-lookup"><span data-stu-id="cf10c-260">Only the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample28.cs)]
- <span data-ttu-id="cf10c-261">Çağıran istemci dışındaki tüm istemcilerin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-261">All clients except the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample29.cs)]
- <span data-ttu-id="cf10c-262">Bağlantı kimliği ile tanımlanan belirli bir istemci</span><span class="sxs-lookup"><span data-stu-id="cf10c-262">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample30.css)]

    <span data-ttu-id="cf10c-263">Bu örnek `addContosoChatMessageToPage` çağıran istemci hakkında ve kullanmakla aynı etkiye sahip `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-263">This example calls `addContosoChatMessageToPage` on the calling client and has the same effect as using `Clients.Caller`.</span></span>
- <span data-ttu-id="cf10c-264">Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="cf10c-264">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample31.cs)]
- <span data-ttu-id="cf10c-265">Belirli bir grubun tüm bağlı istemcileri.</span><span class="sxs-lookup"><span data-stu-id="cf10c-265">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample32.css)]
- <span data-ttu-id="cf10c-266">Belirtilen istemcilerin bağlantı kimliği ile tanımlanan dışında belirtilen gruptaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="cf10c-266">All connected clients in a specified group except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample33.cs)]
- <span data-ttu-id="cf10c-267">Belirli bir grubun tüm bağlı istemcileri çağıran istemci dışındaki.</span><span class="sxs-lookup"><span data-stu-id="cf10c-267">All connected clients in a specified group except the calling client.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample34.css)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a><span data-ttu-id="cf10c-268">Yöntem adları için derleme zamanı doğrulama</span><span class="sxs-lookup"><span data-stu-id="cf10c-268">No compile-time validation for method names</span></span>

<span data-ttu-id="cf10c-269">Belirttiğiniz yöntem adı, IntelliSense veya derleme zamanı doğrulamasını yoktur anlamına gelen dinamik bir nesne olarak yorumlanır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-269">The method name that you specify is interpreted as a dynamic object, which means there is no IntelliSense or compile-time validation for it.</span></span> <span data-ttu-id="cf10c-270">İfade, çalışma zamanında değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-270">The expression is evaluated at run time.</span></span> <span data-ttu-id="cf10c-271">Yöntem çağrısının yürütüldüğünde, SignalR yöntem adı ve parametre değerlerini istemciye gönderir ve istemci bir yöntemi varsa, adla eşleşen, yöntemi çağrılır ve parametre değerlerini, kendisine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-271">When the method call executes, SignalR sends the method name and the parameter values to the client, and if the client has a method that matches the name, that method is called and the parameter values are passed to it.</span></span> <span data-ttu-id="cf10c-272">Eşleşen hiçbir yöntemi istemcide bulunursa, herhangi bir hata ortaya çıkar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-272">If no matching method is found on the client, no error is raised.</span></span> <span data-ttu-id="cf10c-273">Bir istemci yöntemi çağırdığınızda, arka planda istemciye SignalR ileten veri biçimi hakkında daha fazla bilgi için bkz [signalr'a giriş](index.md).</span><span class="sxs-lookup"><span data-stu-id="cf10c-273">For information about the format of the data that SignalR transmits to the client behind the scenes when you call a client method, see [Introduction to SignalR](index.md).</span></span>

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a><span data-ttu-id="cf10c-274">Ad eşleştirme büyük küçük harf duyarsız yöntemi</span><span class="sxs-lookup"><span data-stu-id="cf10c-274">Case-insensitive method name matching</span></span>

<span data-ttu-id="cf10c-275">Yöntem adı ile eşleşen büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-275">Method name matching is case-insensitive.</span></span> <span data-ttu-id="cf10c-276">Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütülür `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, veya `addContosoChatMessageToPage` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="cf10c-276">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, or `addContosoChatMessageToPage` on the client.</span></span>

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a><span data-ttu-id="cf10c-277">Zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="cf10c-277">Asynchronous execution</span></span>

<span data-ttu-id="cf10c-278">Çağıran, yöntemi zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="cf10c-278">The method that you call executes asynchronously.</span></span> <span data-ttu-id="cf10c-279">Bir istemci için bir yöntem çağrısının hemen sonraki kod satırlarını siz belirtmediğiniz sürece, istemciye veri aktarımı tamamlamak, SignalR için beklemenize gerek kalmadan yürütecek sonra gelen kodu yöntemi tamamlanmasını beklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-279">Any code that comes after a method call to a client will execute immediately without waiting for SignalR to finish transmitting data to clients unless you specify that the subsequent lines of code should wait for method completion.</span></span> <span data-ttu-id="cf10c-280">Aşağıdaki kod örnekleri iki istemci yöntemleri ardışık olarak yürütmek gösterilmektedir, kullanarak .NET 4.5 içinde çalışan kod, diğeri kullanarak .NET 4'te çalışan kod.</span><span class="sxs-lookup"><span data-stu-id="cf10c-280">The following code examples show how to execute two client methods sequentially, one using code that works in .NET 4.5, and one using code that works in .NET 4.</span></span>

<span data-ttu-id="cf10c-281">**.NET 4.5 örneği**</span><span class="sxs-lookup"><span data-stu-id="cf10c-281">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample35.cs?highlight=1,3)]

<span data-ttu-id="cf10c-282">**.NET 4 örneği**</span><span class="sxs-lookup"><span data-stu-id="cf10c-282">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample36.cs?highlight=3-4)]

<span data-ttu-id="cf10c-283">Kullanırsanız `await` veya `ContinueWith` sonraki satırlık bir kod yürütülmeden önce bir istemci yöntemi bitene kadar beklemek için gelmez sonraki satırlık bir kod yürütülmeden önce istemcileri aslında iletiyi alır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-283">If you use `await` or `ContinueWith` to wait until a client method finishes before the next line of code executes, that does not mean that clients will actually receive the message before the next line of code executes.</span></span> <span data-ttu-id="cf10c-284">Yalnızca bir istemci yöntem çağrısının "tamamlama" SignalR ileti göndermek için gereken her şey yapmış anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-284">"Completion" of a client method call only means that SignalR has done everything necessary to send the message.</span></span> <span data-ttu-id="cf10c-285">İstemcilerin ileti aldığı doğrulama gerekiyorsa, bu mekanizma kendiniz program gerekir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-285">If you need verification that clients received the message, you have to program that mechanism yourself.</span></span> <span data-ttu-id="cf10c-286">Örneğin, kod bir `MessageReceived` yöntemi Hub hem de `addContosoChatMessageToPage` , çağırın istemcide yöntemi `MessageReceived` , yaptıktan sonra iş istemcide gerçekleştirmek için ihtiyaç.</span><span class="sxs-lookup"><span data-stu-id="cf10c-286">For example, you could code a `MessageReceived` method on the Hub, and in the `addContosoChatMessageToPage` method on the client you could call `MessageReceived` after you do whatever work you need to do on the client.</span></span> <span data-ttu-id="cf10c-287">İçinde `MessageReceived` hub'ı gerçek istemci alma ve işleme özgün yöntem çağrısının hangi iş bağlıdır, bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-287">In `MessageReceived` in the Hub you can do whatever work depends on actual client reception and processing of the original method call.</span></span>

### <a name="how-to-use-a-string-variable-as-the-method-name"></a><span data-ttu-id="cf10c-288">Yöntem adı bir dize değişkeni kullanma</span><span class="sxs-lookup"><span data-stu-id="cf10c-288">How to use a string variable as the method name</span></span>

<span data-ttu-id="cf10c-289">Cast yöntemi adı bir dize değişkeni kullanarak bir istemci yöntem çağırmak istiyorsanız `Clients.All` (veya `Clients.Others`, `Clients.Caller`, vs.) için `IClientProxy` ve sonra çağrı [Invoke (methodName, args...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="cf10c-289">If you want to invoke a client method by using a string variable as the method name, cast `Clients.All` (or `Clients.Others`, `Clients.Caller`, etc.) to `IClientProxy` and then call [Invoke(methodName, args...)](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample37.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a><span data-ttu-id="cf10c-290">Hub sınıftan grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="cf10c-290">How to manage group membership from the Hub class</span></span>

<span data-ttu-id="cf10c-291">Signalr'da gruplarla, bağlı istemciler belirtilen alt kümelerine yayın iletileri için bir yöntem sağlar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-291">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="cf10c-292">Bir grupta herhangi bir sayıda istemciler olabilir ve istemci grupları herhangi bir sayıda üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-292">A group can have any number of clients, and a client can be a member of any number of groups.</span></span>

<span data-ttu-id="cf10c-293">Grup üyeliğini yönetmek için kullandığınız [Ekle](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ve [Kaldır](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) tarafından sağlanan yöntemleri `Groups` Hub sınıfın özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf10c-293">To manage group membership, use the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) and [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods provided by the `Groups` property of the Hub class.</span></span> <span data-ttu-id="cf10c-294">Aşağıdaki örnekte gösterildiği `Groups.Add` ve `Groups.Remove` istemci kodu tarafından çağrılan Hub yöntemlerinde kullanılan yöntemleri, onları çağıran JavaScript istemci kodu tarafından izlenen.</span><span class="sxs-lookup"><span data-stu-id="cf10c-294">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods that are called by client code, followed by JavaScript client code that calls them.</span></span>

<span data-ttu-id="cf10c-295">**Sunucu**</span><span class="sxs-lookup"><span data-stu-id="cf10c-295">**Server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample38.cs?highlight=5,10)]

<span data-ttu-id="cf10c-296">**Oluşturulan proxy kullanarak JavaScript istemcisi**</span><span class="sxs-lookup"><span data-stu-id="cf10c-296">**JavaScript client using generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample39.js)]

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample40.js)]

<span data-ttu-id="cf10c-297">Açıkça grupları oluşturmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="cf10c-297">You don't have to explicitly create groups.</span></span> <span data-ttu-id="cf10c-298">Etkin bir grup çağrıda adını belirttiğiniz ilk kez otomatik olarak oluşturulur `Groups.Add`, ve bu üyelik son bağlantı kaldırdığınızda silinir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-298">In effect a group is automatically created the first time you specify its name in a call to `Groups.Add`, and it is deleted when you remove the last connection from membership in it.</span></span>

<span data-ttu-id="cf10c-299">Bir grup üyeliği listesinin veya grupların listesini almak için hiçbir API yoktur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-299">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="cf10c-300">SignalR istemcileri ve gruplara göre iletiler gönderen bir [pub/sub modeli](http://en.wikipedia.org/wiki/Publish/subscribe), ve sunucu grupları veya grup üyeliklerinin listesi korumaz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-300">SignalR sends messages to clients and groups based on a [pub/sub model](http://en.wikipedia.org/wiki/Publish/subscribe), and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="cf10c-301">Bir web grubu için bir düğüm eklediğinizde, yeni bir düğüme dağıtılmasını SignalR tutar herhangi bir durum olduğundan bu ölçeklenebilirliği en üst düzeye yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-301">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a><span data-ttu-id="cf10c-302">Ekleme ve kaldırma yöntemlerinin, zaman uyumsuz yürütme</span><span class="sxs-lookup"><span data-stu-id="cf10c-302">Asynchronous execution of Add and Remove methods</span></span>

<span data-ttu-id="cf10c-303">`Groups.Add` Ve `Groups.Remove` zaman uyumsuz bir yöntem yürütülemez.</span><span class="sxs-lookup"><span data-stu-id="cf10c-303">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span> <span data-ttu-id="cf10c-304">Bir istemci bir gruba ekleyin ve hemen bir ileti grubunu kullanarak istemciye göndermek istiyorsanız, emin olmak sahip `Groups.Add` yöntemi önce tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-304">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the `Groups.Add` method finishes first.</span></span> <span data-ttu-id="cf10c-305">Aşağıdaki kod örnekleri, .NET 4.5 ve .NET 4'te çalışan kod kullanarak bir tane çalışan kod kullanarak bunun nasıl yapılacağını göster</span><span class="sxs-lookup"><span data-stu-id="cf10c-305">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4</span></span>

<span data-ttu-id="cf10c-306">**.NET 4.5 örneği**</span><span class="sxs-lookup"><span data-stu-id="cf10c-306">**.NET 4.5 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

<span data-ttu-id="cf10c-307">**.NET 4 örneği**</span><span class="sxs-lookup"><span data-stu-id="cf10c-307">**.NET 4 example**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample42.cs?highlight=3-4)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a><span data-ttu-id="cf10c-308">Grup üyeliği kalıcılığı</span><span class="sxs-lookup"><span data-stu-id="cf10c-308">Group membership persistence</span></span>

<span data-ttu-id="cf10c-309">SignalR bağlantıları izler, kullanıcıları değil, dolayısıyla bir kullanıcı kullanıcı her bağlandığında bağlantı aynı grupta olmasını istediğiniz, çağırmak zorunda `Groups.Add` her zaman kullanıcı yeni bir bağlantı kurar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-309">SignalR tracks connections, not users, so if you want a user to be in the same group every time the user establishes a connection, you have to call `Groups.Add` every time the user establishes a new connection.</span></span>

<span data-ttu-id="cf10c-310">Geçici bağlantı kaybı sonra bazen SignalR bağlantı otomatik olarak geri yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-310">After a temporary loss of connectivity, sometimes SignalR can restore the connection automatically.</span></span> <span data-ttu-id="cf10c-311">Bu durumda, yeni bir bağlantı kurmadan aynı bağlantıyı SignalR geri yüklüyor ve bu nedenle istemcinin Grup üyeliğini otomatik olarak geri.</span><span class="sxs-lookup"><span data-stu-id="cf10c-311">In that case, SignalR is restoring the same connection, not establishing a new connection, and so the client's group membership is automatically restored.</span></span> <span data-ttu-id="cf10c-312">Bağlantı durumu grup üyelikleri de dahil olmak üzere, her istemci için istemcinin gidiş dönüşlü geçici kesme sunucu yeniden başlatma veya hata sonucu olsa bile mümkün olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-312">This is possible even when the temporary break is the result of a server reboot or failure, because connection state for each client, including group memberships, is round-tripped to the client.</span></span> <span data-ttu-id="cf10c-313">Bir sunucu arıza ve bağlantı zaman aşımına uğramadan önce yeni bir sunucu tarafından değiştirilir, istemci otomatik olarak yeni sunucuya yeniden ve üyesi olduğu grupları'nı yeniden kaydolun.</span><span class="sxs-lookup"><span data-stu-id="cf10c-313">If a server goes down and is replaced by a new server before the connection times out, a client can reconnect automatically to the new server and re-enroll in groups it is a member of.</span></span>

<span data-ttu-id="cf10c-314">Bir bağlantı, bağlantı kaybı sonra otomatik olarak geri yüklenemez veya bağlantı zaman aşımına uğradığında veya (örneğin, bir tarayıcı için yeni bir sayfa gittiğinde) istemci kestiğinde, grup üyeliği kaybedilir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-314">When a connection can't be restored automatically after a loss of connectivity, or when the connection times out, or when the client disconnects (for example, when a browser navigates to a new page), group memberships are lost.</span></span> <span data-ttu-id="cf10c-315">Kullanıcı bir sonraki bağlanışında, yeni bir bağlantı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-315">The next time the user connects will be a new connection.</span></span> <span data-ttu-id="cf10c-316">Aynı kullanıcı yeni bir bağlantı kurduğunda grup üyeliklerini korumasına kullanıcılar ve gruplar ilişkileri izlemek ve grup üyelikleri kullanıcı yeni bir bağlantı kurar ve her zaman geri yüklemek uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-316">To maintain group memberships when the same user establishes a new connection, your application has to track the associations between users and groups, and restore group memberships each time a user establishes a new connection.</span></span>

<span data-ttu-id="cf10c-317">Bağlantılar ve tutarsızlıklara hakkında daha fazla bilgi için bkz. [Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl](#connectionlifetime) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="cf10c-317">For more information about connections and reconnections, see [How to handle connection lifetime events in the Hub class](#connectionlifetime) later in this topic.</span></span>

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a><span data-ttu-id="cf10c-318">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="cf10c-318">Single-user groups</span></span>

<span data-ttu-id="cf10c-319">SignalR genellikle kullanan uygulamalar, hangi kullanıcının bir ileti gönderdi ve hangi kullanıcının bir ileti almalıdır öğrenmek için kullanıcılar ve bağlantılar arasındaki ilişkileri izlemek zorunda.</span><span class="sxs-lookup"><span data-stu-id="cf10c-319">Applications that use SignalR typically have to keep track of the associations between users and connections in order to know which user has sent a message and which user(s) should be receiving a message.</span></span> <span data-ttu-id="cf10c-320">Grupları iki yaygın olarak kullanılan desenlerden birini yapmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-320">Groups are used in one of the two commonly used patterns for doing that.</span></span>

- <span data-ttu-id="cf10c-321">Tek kullanıcı grupları.</span><span class="sxs-lookup"><span data-stu-id="cf10c-321">Single-user groups.</span></span>

    <span data-ttu-id="cf10c-322">Kullanıcı adı grup adı belirtin ve kullanıcı bağlanır veya yeniden her zaman geçerli bağlantı kimliği gruba ekleyin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-322">You can specify the user name as the group name, and add the current connection ID to the group every time the user connects or reconnects.</span></span> <span data-ttu-id="cf10c-323">Kullanıcıya ileti göndermek için Grup gönderin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-323">To send messages to the user you send to the group.</span></span> <span data-ttu-id="cf10c-324">Bu yöntem bir dezavantajı, gruba kullanıcı çevrimiçi veya çevrimdışı olup olmadığını öğrenmek için bir yol sağlamaz olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-324">A disadvantage of this method is that the group doesn't provide you with a way to find out if the user is online or offline.</span></span>
- <span data-ttu-id="cf10c-325">Kullanıcı adları ve bağlantı kimlikleri arasındaki ilişkilendirmeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-325">Track associations between user names and connection IDs.</span></span>

    <span data-ttu-id="cf10c-326">Her bir kullanıcı adı ve bir veya daha fazla bağlantı kimlikleri arasında bir ilişki bir sözlük veya veritabanında depolamak ve depolanan veriler her zaman kullanıcı bağlandığında veya bağlantısı kesildiğinde güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-326">You can store an association between each user name and one or more connection IDs in a dictionary or database, and update the stored data each time the user connects or disconnects.</span></span> <span data-ttu-id="cf10c-327">Kullanıcıya ileti göndermek için bağlantı kimliği belirtin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-327">To send messages to the user you specify the connection IDs.</span></span> <span data-ttu-id="cf10c-328">Bu yöntem bir dezavantajı, daha fazla bellek alır olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-328">A disadvantage of this method is that it takes more memory.</span></span>

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a><span data-ttu-id="cf10c-329">Hub sınıfında bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="cf10c-329">How to handle connection lifetime events in the Hub class</span></span>

<span data-ttu-id="cf10c-330">Bağlantı ömrü olaylarını işlemek için normal bir kullanıcı veya bağlı olup olmadığını izler ve kullanıcı adları ve bağlantı kimlikleri arasındaki ilişkiyi izlemek için nedenleridir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-330">Typical reasons for handling connection lifetime events are to keep track of whether a user is connected or not, and to keep track of the association between user names and connection IDs.</span></span> <span data-ttu-id="cf10c-331">İstemciler bağlanın veya bağlantıyı kesin zaman kendi kodunuzu çalıştırmak için geçersiz kılma `OnConnected`, `OnDisconnected`, ve `OnReconnected` aşağıdaki örnekte gösterildiği gibi hub'ın sanal yöntemler sınıf.</span><span class="sxs-lookup"><span data-stu-id="cf10c-331">To run your own code when clients connect or disconnect, override the `OnConnected`, `OnDisconnected`, and `OnReconnected` virtual methods of the Hub class, as shown in the following example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample43.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a><span data-ttu-id="cf10c-332">OnConnected OnDisconnected ve OnReconnected olduğunda çağırılır</span><span class="sxs-lookup"><span data-stu-id="cf10c-332">When OnConnected, OnDisconnected, and OnReconnected are called</span></span>

<span data-ttu-id="cf10c-333">Bir tarayıcı yeni bir sayfaya gider her seferinde yeni bir bağlantı kurulması SignalR yürütülecek anlamına gelir sahip `OnDisconnected` yöntemi arkasından `OnConnected` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cf10c-333">Each time a browser navigates to a new page, a new connection has to be established, which means SignalR will execute the `OnDisconnected` method followed by the `OnConnected` method.</span></span> <span data-ttu-id="cf10c-334">Yeni bir bağlantı kurulduğunda SignalR her zaman yeni bir bağlantı kimliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cf10c-334">SignalR always creates a new connection ID when a new connection is established.</span></span>

<span data-ttu-id="cf10c-335">`OnReconnected` Olduğunda geçici kesme SignalR otomatik olarak, ne zaman kablo geçici olarak bağlantısı kesilir ve bağlantı zaman aşımına uğramadan önce yeniden bağlantı kuruldu gibi kurtarabileceğiniz bağlantılar yöntemi çağrılır. `OnDisconnected` İstemcinin bağlantısı kesildi ve SignalR olamaz otomatik olarak yeniden, ne zaman yeni bir sayfaya bir tarayıcı gider gibi yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-335">The `OnReconnected` method is called when there has been a temporary break in connectivity that SignalR can automatically recover from, such as when a cable is temporarily disconnected and reconnected before the connection times out. The `OnDisconnected` method is called when the client is disconnected and SignalR can't automatically reconnect, such as when a browser navigates to a new page.</span></span> <span data-ttu-id="cf10c-336">Bu nedenle, olası belirli bir istemcinin olayları dizisidir `OnConnected`, `OnReconnected`, `OnDisconnected`; veya `OnConnected`, `OnDisconnected`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-336">Therefore, a possible sequence of events for a given client is `OnConnected`, `OnReconnected`, `OnDisconnected`; or `OnConnected`, `OnDisconnected`.</span></span> <span data-ttu-id="cf10c-337">Sıra görmezsiniz `OnConnected`, `OnDisconnected`, `OnReconnected` belirli bir bağlantı için.</span><span class="sxs-lookup"><span data-stu-id="cf10c-337">You won't see the sequence `OnConnected`, `OnDisconnected`, `OnReconnected` for a given connection.</span></span>

<span data-ttu-id="cf10c-338">`OnDisconnected` Değil yönteminden ne zaman bir sunucu arıza gibi bazı senaryolarda veya uygulama etki alanı geri alır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-338">The `OnDisconnected` method doesn't get called in some scenarios, such as when a server goes down or the App Domain gets recycled.</span></span> <span data-ttu-id="cf10c-339">Başka bir sunucuya gelinceye veya uygulama etki alanı, geri dönüşüm tamamlandıktan, bazı istemciler bağlanın ve yangın mümkün olabilir `OnReconnected` olay.</span><span class="sxs-lookup"><span data-stu-id="cf10c-339">When another server comes on line or the App Domain completes its recycle, some clients may be able to reconnect and fire the `OnReconnected` event.</span></span>

<span data-ttu-id="cf10c-340">Daha fazla bilgi için [anlama ve signalr'da bağlantı ömrü olaylarını işleme](index.md).</span><span class="sxs-lookup"><span data-stu-id="cf10c-340">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a><span data-ttu-id="cf10c-341">Arayan durumu değil doldurulur</span><span class="sxs-lookup"><span data-stu-id="cf10c-341">Caller state not populated</span></span>

<span data-ttu-id="cf10c-342">Bağlantı ömrü olay işleyicisi yöntemleri içine girdiğiniz herhangi bir durumu anlamına sunucudan adlı `state` istemci üzerinde nesne değil doldurulacak içinde `Caller` sunucudaki özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf10c-342">The connection lifetime event handler methods are called from the server, which means that any state that you put in the `state` object on the client will not be populated in the `Caller` property on the server.</span></span> <span data-ttu-id="cf10c-343">Hakkında bilgi için `state` nesne ve `Caller` özelliği bkz [Hub sınıfına ve istemciler arasında durumunu nasıl](#passstate) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="cf10c-343">For information about the `state` object and the `Caller` property, see [How to pass state between clients and the Hub class](#passstate) later in this topic.</span></span>

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a><span data-ttu-id="cf10c-344">Bağlam özelliği istemci bilgilerini alma</span><span class="sxs-lookup"><span data-stu-id="cf10c-344">How to get information about the client from the Context property</span></span>

<span data-ttu-id="cf10c-345">İstemcisi hakkında bilgi almak için kullanın `Context` Hub sınıfın özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf10c-345">To get information about the client, use the `Context` property of the Hub class.</span></span> <span data-ttu-id="cf10c-346">`Context` Özelliği döndürür bir [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) aşağıdaki bilgilere erişim sağlayan nesne:</span><span class="sxs-lookup"><span data-stu-id="cf10c-346">The `Context` property returns a [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) object which provides access to the following information:</span></span>

- <span data-ttu-id="cf10c-347">Çağıran istemcinin bağlantı kimliği.</span><span class="sxs-lookup"><span data-stu-id="cf10c-347">The connection ID of the calling client.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample44.cs?highlight=1)]

    <span data-ttu-id="cf10c-348">Bağlantı kimliği (değeri kendi kodunuzda belirtemezsiniz) SignalR tarafından atanan bir GUID'dir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-348">The connection ID is a GUID that is assigned by SignalR (you can't specify the value in your own code).</span></span> <span data-ttu-id="cf10c-349">Her bağlantı ve uygulamanızda birden çok hub'a varsa tüm hub'ları tarafından kullanılan kimliği aynı bağlantı için bir bağlantı kimliği yok.</span><span class="sxs-lookup"><span data-stu-id="cf10c-349">There is one connection ID for each connection, and the same connection ID is used by all Hubs if you have multiple Hubs in your application.</span></span>
- <span data-ttu-id="cf10c-350">HTTP üst bilgisi verileri.</span><span class="sxs-lookup"><span data-stu-id="cf10c-350">HTTP header data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample45.cs?highlight=1)]

    <span data-ttu-id="cf10c-351">HTTP üst bilgiler de alabilirsiniz `Context.Headers`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-351">You can also get HTTP headers from `Context.Headers`.</span></span> <span data-ttu-id="cf10c-352">Aynı şeyi birden çok başvuru nedeni `Context.Headers` ilk olarak oluşturulduğu `Context.Request` özelliği daha sonra eklenen ve `Context.Headers` geriye dönük uyumluluk için tutulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-352">The reason for multiple references to the same thing is that `Context.Headers` was created first, the `Context.Request` property was added later, and `Context.Headers` was retained for backward compatibility.</span></span>
- <span data-ttu-id="cf10c-353">Dize verileri sorgulayın.</span><span class="sxs-lookup"><span data-stu-id="cf10c-353">Query string data.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample46.cs?highlight=1)]

    <span data-ttu-id="cf10c-354">Sorgu dizesi verileri da edinebilirsiniz `Context.QueryString`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-354">You can also get query string data from `Context.QueryString`.</span></span>

    <span data-ttu-id="cf10c-355">Bu özelliği alma sorgu dizesi SignalR bağlantısı HTTP isteği ile kullanılan paroladır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-355">The query string that you get in this property is the one that was used with the HTTP request that established the SignalR connection.</span></span> <span data-ttu-id="cf10c-356">İstemci istemci hakkındaki verileri istemciden sunucuya geçirmek için kullanışlı bir yoldur bağlantı yapılandırarak, sorgu dizesi parametreleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-356">You can add query string parameters in the client by configuring the connection, which is a convenient way to pass data about the client from the client to the server.</span></span> <span data-ttu-id="cf10c-357">Aşağıdaki örnek, oluşturulan proxy kullandığınızda, JavaScript istemci olarak bir sorgu dizesi eklemek için yollarından biri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-357">The following example shows one way to add a query string in a JavaScript client when you use the generated proxy.</span></span>

    [!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample47.js?highlight=1)]

    <span data-ttu-id="cf10c-358">Sorgu dizesi parametreleri ayarlama hakkında daha fazla bilgi için bkz. API kılavuzları için [JavaScript](index.md) ve [.NET](index.md) istemciler.</span><span class="sxs-lookup"><span data-stu-id="cf10c-358">For more information about setting query string parameters, see the API guides for the [JavaScript](index.md) and [.NET](index.md) clients.</span></span>

    <span data-ttu-id="cf10c-359">SignalR tarafından dahili olarak kullanılan bazı diğer değerler yanı sıra sorgu dize verileri, bağlantı için kullanılan aktarım yöntemi bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="cf10c-359">You can find the transport method used for the connection in the query string data, along with some other values used internally by SignalR:</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample48.cs)]

    <span data-ttu-id="cf10c-360">Değerini `transportMethod` "webSockets", "serverSentEvents", "foreverFrame" veya "longPolling" olacaktır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-360">The value of `transportMethod` will be "webSockets", "serverSentEvents", "foreverFrame" or "longPolling".</span></span> <span data-ttu-id="cf10c-361">Bu değer iade gerçekleştiriyorsanız `OnConnected` olay işleyicisi yönteminde, bazı senaryolarda ilk bağlantı için son anlaşılan taşıma yöntemini değil bir taşıma değeri alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="cf10c-361">Note that if you check this value in the `OnConnected` event handler method, in some scenarios you might initially get a transport value that is not the final negotiated transport method for the connection.</span></span> <span data-ttu-id="cf10c-362">Bu durumda yöntem bir özel durum oluşturur ve daha sonra tekrar son taşıma yöntemi oluşturulduğunda çağrılır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-362">In that case the method will throw an exception and will be called again later when the final transport method is established.</span></span>
- <span data-ttu-id="cf10c-363">Tanımlama bilgileri.</span><span class="sxs-lookup"><span data-stu-id="cf10c-363">Cookies.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    <span data-ttu-id="cf10c-364">Tanımlama bilgilerini de alabilirsiniz `Context.RequestCookies`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-364">You can also get cookies from `Context.RequestCookies`.</span></span>
- <span data-ttu-id="cf10c-365">Kullanıcı bilgileri.</span><span class="sxs-lookup"><span data-stu-id="cf10c-365">User information.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample50.cs?highlight=1)]
- <span data-ttu-id="cf10c-366">İstek için HttpContext nesnesi:</span><span class="sxs-lookup"><span data-stu-id="cf10c-366">The HttpContext object for the request :</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample51.cs?highlight=1)]

    <span data-ttu-id="cf10c-367">Almak yerine bu yöntemi kullanmak `HttpContext.Current` almak için `HttpContext` SignalR bağlantı nesnesi.</span><span class="sxs-lookup"><span data-stu-id="cf10c-367">Use this method instead of getting `HttpContext.Current` to get the `HttpContext` object for the SignalR connection.</span></span>

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a><span data-ttu-id="cf10c-368">Nasıl durumu Hub sınıfına ve istemciler arasında geçirme</span><span class="sxs-lookup"><span data-stu-id="cf10c-368">How to pass state between clients and the Hub class</span></span>

<span data-ttu-id="cf10c-369">İstemci proxy sağlayan bir `state` içinde depolayabileceğiniz her yöntem çağrısının ile sunucuya aktarılması istediğiniz veri nesnesi.</span><span class="sxs-lookup"><span data-stu-id="cf10c-369">The client proxy provides a `state` object in which you can store data that you want to be transmitted to the server with each method call.</span></span> <span data-ttu-id="cf10c-370">Sunucu üzerinde bu verilerine erişebilir `Clients.Caller` istemciler tarafından çağrılan Hub yöntemlerini bir özellik.</span><span class="sxs-lookup"><span data-stu-id="cf10c-370">On the server you can access this data in the `Clients.Caller` property in Hub methods that are called by clients.</span></span> <span data-ttu-id="cf10c-371">`Clients.Caller` Özelliği için bağlantı ömrü olay işleyicisi yöntemleri doldurulmamışsa `OnConnected`, `OnDisconnected`, ve `OnReconnected`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-371">The `Clients.Caller` property is not populated for the connection lifetime event handler methods `OnConnected`, `OnDisconnected`, and `OnReconnected`.</span></span>

<span data-ttu-id="cf10c-372">Oluşturma veya güncelleştirme verilerinde `state` nesne ve `Clients.Caller` özelliği, her iki yönde de çalışır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-372">Creating or updating data in the `state` object and the `Clients.Caller` property works in both directions.</span></span> <span data-ttu-id="cf10c-373">Değerleri sunucusunda güncelleştirebilirsiniz ve bunlar istemciye geçirilir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-373">You can update values in the server and they are passed back to the client.</span></span>

<span data-ttu-id="cf10c-374">Aşağıdaki örnek, iletilmesi için her yöntem çağrısının ile sunucu durumunu depolayan JavaScript istemci kodu gösterir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-374">The following example shows JavaScript client code that stores state for transmission to the server with every method call.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-server/samples/sample52.js?highlight=1-2)]

<span data-ttu-id="cf10c-375">Aşağıdaki örnek bir .NET istemci eşdeğer kod gösterir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-375">The following example shows the equivalent code in a .NET client.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample53.cs?highlight=1-2)]

<span data-ttu-id="cf10c-376">Hub sınıfınızda, bu verilerine erişebilir `Clients.Caller` özelliği.</span><span class="sxs-lookup"><span data-stu-id="cf10c-376">In your Hub class, you can access this data in the `Clients.Caller` property.</span></span> <span data-ttu-id="cf10c-377">Aşağıdaki örnek, önceki örnekte başvurulan durumunu alır. kod gösterir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-377">The following example shows code that retrieves the state referred to in the previous example.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample54.cs?highlight=3-4)]

> [!NOTE]
> <span data-ttu-id="cf10c-378">Kalıcı hale getirme durumu için bu düzenek itibaren her şeyi içine girdiğiniz büyük miktarlarda veri için tasarlanmamıştır `state` veya `Clients.Caller` özellik gidiş dönüşlü ile her yöntem çağırma.</span><span class="sxs-lookup"><span data-stu-id="cf10c-378">This mechanism for persisting state is not intended for large amounts of data, since everything you put in the `state` or `Clients.Caller` property is round-tripped with every method invocation.</span></span> <span data-ttu-id="cf10c-379">Kullanıcı adlarını veya sayaçları gibi küçük öğeler için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-379">It's useful for smaller items such as user names or counters.</span></span>


<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a><span data-ttu-id="cf10c-380">Hub sınıfında hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="cf10c-380">How to handle errors in the Hub class</span></span>

<span data-ttu-id="cf10c-381">Hub sınıfı yöntemlerinde meydana gelen hataları işlemek için aşağıdakilerden birini veya her ikisi de aşağıdaki yöntemlerden birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="cf10c-381">To handle errors that occur in your Hub class methods, use one or both of the following methods:</span></span>

- <span data-ttu-id="cf10c-382">Try-catch bloğu içinde yöntemi kodunuzu sarın ve özel durum nesnesi oturum açın.</span><span class="sxs-lookup"><span data-stu-id="cf10c-382">Wrap your method code in try-catch blocks and log the exception object.</span></span> <span data-ttu-id="cf10c-383">Hata ayıklama amacıyla istemciye bir özel durum gönderebilir, ancak güvenlik için üretim istemciler için ayrıntılı bilgi gönderme nedeniyle önerilmez.</span><span class="sxs-lookup"><span data-stu-id="cf10c-383">For debugging purposes you can send the exception to the client, but for security reasons sending detailed information to clients in production is not recommended.</span></span>
- <span data-ttu-id="cf10c-384">İşleme bir hub'ları işlem hattı modülünüzü oluşturmak [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="cf10c-384">Create a Hubs pipeline module that handles the [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) method.</span></span> <span data-ttu-id="cf10c-385">Aşağıdaki örnek, hatalar, modül hub ardışık düzene ekler Global.asax içindeki kod tarafından izlenen günlüklerini bir işlem hattı modül gösterir.</span><span class="sxs-lookup"><span data-stu-id="cf10c-385">The following example shows a pipeline module that logs errors, followed by code in Global.asax that injects the module into the Hubs pipeline.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample55.cs)]

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample56.cs?highlight=3)]

<span data-ttu-id="cf10c-386">Hub ardışık düzen modüllerine hakkında daha fazla bilgi için bkz: [hub ardışık düzeni özelleştirildiği nasıl](#hubpipeline) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="cf10c-386">For more information about Hub pipeline modules, see [How to customize the Hubs pipeline](#hubpipeline) later in this topic.</span></span>

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a><span data-ttu-id="cf10c-387">İzlemeyi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="cf10c-387">How to enable tracing</span></span>

<span data-ttu-id="cf10c-388">System.diagnostics öğesi sunucu-tarafı izlemeyi etkinleştirmek için Web.config dosyasına, bu örnekte gösterildiği gibi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="cf10c-388">To enable server-side tracing, add a system.diagnostics element to your Web.config file, as shown in this example:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-server/samples/sample57.html?highlight=17-72)]

<span data-ttu-id="cf10c-389">Visual Studio'da uygulamayı çalıştırdığınızda, günlükleri görüntüleyebilirsiniz **çıkış** penceresi.</span><span class="sxs-lookup"><span data-stu-id="cf10c-389">When you run the application in Visual Studio, you can view the logs in the **Output** window.</span></span>

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a><span data-ttu-id="cf10c-390">İstemci yöntemleri çağırmak ve Hub sınıfına dışındaki grupları yönetme</span><span class="sxs-lookup"><span data-stu-id="cf10c-390">How to call client methods and manage groups from outside the Hub class</span></span>

<span data-ttu-id="cf10c-391">İstemci Hub sınıfınıza daha farklı bir sınıftaki yöntemleri çağırmak için hub'ı için SignalR bağlam nesnesi bir başvuru almak ve, istemcide yöntemlerini çağıran veya grupları yönetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="cf10c-391">To call client methods from a different class than your Hub class, get a reference to the SignalR context object for the Hub and use that to call methods on the client or manage groups.</span></span>

<span data-ttu-id="cf10c-392">Aşağıdaki örnek `StockTicker` sınıfı bağlam nesnesini alır, sınıfının bir örneğini depolar, statik özellik bir sınıf örneğini depolar ve çağırmak için singleton sınıfı örneğinden bağlamı kullanır `updateStockPrice` istemciler yöntemi adlı bir Hub'ına bağlı `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-392">The following sample `StockTicker` class gets the context object, stores it in an instance of the class, stores the class instance in a static property, and uses the context from the singleton class instance to call the `updateStockPrice` method on clients that are connected to a Hub named `StockTickerHub`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample58.cs?highlight=8,24)]

<span data-ttu-id="cf10c-393">Uzun süreli bir nesne bağlamı birden çok kez kullanmanız gerekiyorsa, başvuru kez almak ve bunun yerine her zaman yeniden başlama kaydedin.</span><span class="sxs-lookup"><span data-stu-id="cf10c-393">If you need to use the context multiple-times in a long-lived object, get the reference once and save it rather than getting it again each time.</span></span> <span data-ttu-id="cf10c-394">Bir kez bağlamı alma SignalR istemcilere içinde ve Hub yöntemlerinizi istemci yöntem çağrıları yapmak aynı sıradaki iletiler gönderir sağlar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-394">Getting the context once ensures that SignalR sends messages to clients in the same sequence in which your Hub methods make client method invocations.</span></span> <span data-ttu-id="cf10c-395">SignalR bağlamı için bir hub'ı kullanmayı gösteren bir öğretici için bkz. [ASP.NET SignalR ile sunucu yayını](index.md).</span><span class="sxs-lookup"><span data-stu-id="cf10c-395">For a tutorial that shows how to use the SignalR context for a Hub, see [Server Broadcast with ASP.NET SignalR](index.md).</span></span>

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a><span data-ttu-id="cf10c-396">İstemci yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="cf10c-396">Calling client methods</span></span>

<span data-ttu-id="cf10c-397">Hangi istemcilerin RPC alırsınız belirtebilirsiniz, ancak bir Hub sınıftan çağrısı yaparken daha az seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-397">You can specify which clients will receive the RPC, but you have fewer options than when you call from a Hub class.</span></span> <span data-ttu-id="cf10c-398">Bunun nedeni herhangi bir yöntem gibi geçerli bir bağlantı kimliği bilgisi gerektiren şekilde bağlamı bir istemci belirli bir çağrıdan ilişkilendirilmiş olmasıdır `Clients.Others`, veya `Clients.Caller`, veya `Clients.OthersInGroup`, kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="cf10c-398">The reason for this is that the context is not associated with a particular call from a client, so any methods that require knowledge of the current connection ID, such as `Clients.Others`, or `Clients.Caller`, or `Clients.OthersInGroup`, are not available.</span></span> <span data-ttu-id="cf10c-399">Aşağıdaki seçenekler mevcuttur:</span><span class="sxs-lookup"><span data-stu-id="cf10c-399">The following options are available:</span></span>

- <span data-ttu-id="cf10c-400">Bağlanan tüm istemciler.</span><span class="sxs-lookup"><span data-stu-id="cf10c-400">All connected clients.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample59.cs)]
- <span data-ttu-id="cf10c-401">Bağlantı kimliği ile tanımlanan belirli bir istemci</span><span class="sxs-lookup"><span data-stu-id="cf10c-401">A specific client identified by connection ID.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample60.css)]
- <span data-ttu-id="cf10c-402">Bağlantı kimliği ile tanımlanan belirtilen istemcileri dışındaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="cf10c-402">All connected clients except the specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample61.cs)]
- <span data-ttu-id="cf10c-403">Belirli bir grubun tüm bağlı istemcileri.</span><span class="sxs-lookup"><span data-stu-id="cf10c-403">All connected clients in a specified group.</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample62.css)]
- <span data-ttu-id="cf10c-404">Bağlantı kimliği ile tanımlanan, belirtilen istemcilerin dışında belirtilen gruptaki tüm bağlı istemcileri</span><span class="sxs-lookup"><span data-stu-id="cf10c-404">All connected clients in a specified group except specified clients, identified by connection ID.</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample63.cs)]

<span data-ttu-id="cf10c-405">Hub sınıfınızda yöntemlerinden Hub olmayan sınıfınıza çağırıyorsanız, geçerli bağlantı kimliği geçirin ve ile kullanan `Clients.Client`, `Clients.AllExcept`, veya `Clients.Group` benzetimini yapmak için `Clients.Caller`, `Clients.Others`, veya `Clients.OthersInGroup`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-405">If you are calling into your non-Hub class from methods in your Hub class, you can pass in the current connection ID and use that with `Clients.Client`, `Clients.AllExcept`, or `Clients.Group` to simulate `Clients.Caller`, `Clients.Others`, or `Clients.OthersInGroup`.</span></span> <span data-ttu-id="cf10c-406">Aşağıdaki örnekte, `MoveShapeHub` sınıfı için bağlantı kimliği geçirir `Broadcaster` sınıfı böylece `Broadcaster` sınıfı benzetimini yapmak `Clients.Others`.</span><span class="sxs-lookup"><span data-stu-id="cf10c-406">In the following example, the `MoveShapeHub` class passes the connection ID to the `Broadcaster` class so that the `Broadcaster` class can simulate `Clients.Others`.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample64.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a><span data-ttu-id="cf10c-407">Grup üyeliğini yönetme</span><span class="sxs-lookup"><span data-stu-id="cf10c-407">Managing group membership</span></span>

<span data-ttu-id="cf10c-408">Bir Hub sınıfta olarak grupları yönetme için aynı seçeneklere sahip.</span><span class="sxs-lookup"><span data-stu-id="cf10c-408">For managing groups you have the same options as you do in a Hub class.</span></span>

- <span data-ttu-id="cf10c-409">Bir gruba istemci Ekle</span><span class="sxs-lookup"><span data-stu-id="cf10c-409">Add a client to a group</span></span>

    [!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample65.cs)]
- <span data-ttu-id="cf10c-410">Bir istemci bir gruptan kaldırma</span><span class="sxs-lookup"><span data-stu-id="cf10c-410">Remove a client from a group</span></span>

    [!code-css[Main](signalr-1x-hubs-api-guide-server/samples/sample66.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a><span data-ttu-id="cf10c-411">Hub ardışık düzeni özelleştirme</span><span class="sxs-lookup"><span data-stu-id="cf10c-411">How to customize the Hubs pipeline</span></span>

<span data-ttu-id="cf10c-412">SignalR Hub ardışık düzende kendi kod eklemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="cf10c-412">SignalR enables you to inject your own code into the Hub pipeline.</span></span> <span data-ttu-id="cf10c-413">Aşağıdaki örnek, istemci ve istemci üzerinde çağrılır giden yöntem çağrısının alınan gelen her yöntem çağrısının günlükleri özel bir Hub ardışık düzen modülü gösterir:</span><span class="sxs-lookup"><span data-stu-id="cf10c-413">The following example shows a custom Hub pipeline module that logs each incoming method call received from the client and outgoing method call invoked on the client:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample67.cs)]

<span data-ttu-id="cf10c-414">Aşağıdaki kod içinde *Global.asax* dosyayı, işlem hattında hub'ı çalıştırmak için modül kaydeder:</span><span class="sxs-lookup"><span data-stu-id="cf10c-414">The following code in the *Global.asax* file registers the module to run in the Hub pipeline:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-server/samples/sample68.cs?highlight=3)]

<span data-ttu-id="cf10c-415">Geçersiz kılabilirsiniz birçok farklı yöntem vardır.</span><span class="sxs-lookup"><span data-stu-id="cf10c-415">There are many different methods that you can override.</span></span> <span data-ttu-id="cf10c-416">Tam bir listesi için bkz. [HubPipelineModule yöntemleri](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="cf10c-416">For a complete list, see [HubPipelineModule Methods](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).</span></span>
