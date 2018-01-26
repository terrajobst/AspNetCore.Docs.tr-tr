---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: "SignalR 1.x hub API Kılavuzu - JavaScript istemci | Microsoft Docs"
author: pfletcher
description: "Bu belge SignalR sürüm 1.1 tarayıcılar ve Windows Mağazası (WinJS) applic gibi JavaScript istemcileri için hub API'sini kullanmaya tanıtılmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/17/2013
ms.topic: article
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: f92470b2022f343cfd6d822abb255dc19947b4d1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="867d5-103">SignalR 1.x hub API Kılavuzu - JavaScript istemci</span><span class="sxs-lookup"><span data-stu-id="867d5-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="867d5-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="867d5-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="867d5-105">Bu belge SignalR sürüm 1.1 tarayıcılar ve Windows Mağazası (WinJS) uygulamaları gibi JavaScript istemcileri için hub API kullanarak giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="867d5-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="867d5-106">SignalR hub'ları API bir sunucuya bağlanan istemciler ve sunucu istemcilerine uzaktan yordam çağrılarını (RPC) yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="867d5-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="867d5-107">Sunucu kodu, istemciler tarafından çağrılabilir yöntemlerini tanımlama ve istemci üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="867d5-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="867d5-108">İstemci kodu sunucudan çağrılabilir yöntemlerini tanımlama ve sunucu üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="867d5-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="867d5-109">SignalR, istemci-sunucu tesisat tümünün ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="867d5-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="867d5-110">SignalR kalıcı bağlantılar olarak adlandırılan bir alt düzey API de sunar.</span><span class="sxs-lookup"><span data-stu-id="867d5-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="867d5-111">Giriş SignalR, hub'lar ve kalıcı bağlantılar için ya da tam bir SignalR uygulamasının nasıl oluşturulacağını gösteren bir öğretici için bkz: [Başlarken SignalR -](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="867d5-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="867d5-112">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="867d5-112">Overview</span></span>

<span data-ttu-id="867d5-113">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="867d5-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="867d5-114">Oluşturulan proxy ve onu sizin için ne yaptığını</span><span class="sxs-lookup"><span data-stu-id="867d5-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="867d5-115">Oluşturulan proxy kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="867d5-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="867d5-116">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="867d5-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="867d5-117">Dinamik olarak üretilen proxy başvuru yapma</span><span class="sxs-lookup"><span data-stu-id="867d5-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="867d5-118">Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur</span><span class="sxs-lookup"><span data-stu-id="867d5-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="867d5-119">Bir bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="867d5-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="867d5-120">$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi</span><span class="sxs-lookup"><span data-stu-id="867d5-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="867d5-121">Zaman uyumsuz yürütme başlangıç yöntemi</span><span class="sxs-lookup"><span data-stu-id="867d5-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="867d5-122">Etki alanları arası bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="867d5-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="867d5-123">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="867d5-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="867d5-124">Sorgu dizesi parametreleri belirtme</span><span class="sxs-lookup"><span data-stu-id="867d5-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="867d5-125">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="867d5-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="867d5-126">Bir Hub sınıf için bir proxy alma</span><span class="sxs-lookup"><span data-stu-id="867d5-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="867d5-127">Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="867d5-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="867d5-128">İstemciden sunucu yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="867d5-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="867d5-129">Bağlantı ömür olayları işleme</span><span class="sxs-lookup"><span data-stu-id="867d5-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="867d5-130">Hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="867d5-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="867d5-131">İstemci-tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="867d5-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="867d5-132">Sunucu veya .NET istemcileri program konusunda daha fazla belgeler için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="867d5-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="867d5-133">SignalR hub'ları API Kılavuzu - sunucu</span><span class="sxs-lookup"><span data-stu-id="867d5-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="867d5-134">SignalR hub'ları API Kılavuzu - .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="867d5-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="867d5-135">API başvuru konuları API'si .NET 4.5 sürümüne bağlantılardır.</span><span class="sxs-lookup"><span data-stu-id="867d5-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="867d5-136">.NET 4 kullanıyorsanız, bkz: [API konuları .NET 4 sürümü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="867d5-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="867d5-137">Oluşturulan proxy ve onu sizin için ne yaptığını</span><span class="sxs-lookup"><span data-stu-id="867d5-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="867d5-138">SignalR hizmet ile veya SignalR sizin için oluşturduğu ara sunucu olmadan ile iletişim kurmak için JavaScript istemci programlama yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="867d5-139">Proxy sizin için ne yaptığını, sunucuya çağrıları, yazma yöntemleri bağlanmanız ve sunucuda yöntemlerini çağıran kodu söz dizimini basitleştirmek olur.</span><span class="sxs-lookup"><span data-stu-id="867d5-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="867d5-140">Sunucu yöntemleri çağırmak için kodu yazarken oluşturulan proxy, yerel bir işlev yürütülmekte ancak gibi görünen sözdizimi kullanmanıza olanak tanır: yazabileceğiniz `serverMethod(arg1, arg2)` yerine `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="867d5-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="867d5-141">Bir sunucu yöntem adı yanlış yazdınız oluşturulan proxy sözdizimi ayrıca istemci tarafı hata hemen ve intelligible sağlar.</span><span class="sxs-lookup"><span data-stu-id="867d5-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="867d5-142">Ve proxy'leri tanımlayan dosyasını elle oluşturursanız, ayrıca IntelliSense desteği server yöntemlerini çağıran kodu yazmak için alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="867d5-143">Örneğin, sunucu üzerinde aşağıdaki Hub sınıf olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="867d5-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="867d5-144">Aşağıdaki kod ne JavaScript kodu çağırmaktan gibi görünüyor örnekler `NewContosoChatMessage` sunucu ve çağırmaları alma yöntemini `addContosoChatMessageToPage` sunucudan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="867d5-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="867d5-145">**Oluşturulan proxy ile**</span><span class="sxs-lookup"><span data-stu-id="867d5-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="867d5-146">**Oluşturulan proxy**</span><span class="sxs-lookup"><span data-stu-id="867d5-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="867d5-147">Oluşturulan proxy kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="867d5-147">When to use the generated proxy</span></span>

<span data-ttu-id="867d5-148">Sunucu çağıran bir istemci yöntemi için birden çok olay işleyicilerini kaydetmek istiyorsanız, oluşturulan proxy kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="867d5-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="867d5-149">Aksi takdirde, oluşturulan proxy kullanmayı seçebilirsiniz veya kodlama tercihinize bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="867d5-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="867d5-150">Bunu kullanmayı seçerseniz, "signalr/hub" URL'de başvuru yoksa bir `script` istemci kodunuzda öğesi.</span><span class="sxs-lookup"><span data-stu-id="867d5-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="867d5-151">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="867d5-151">Client setup</span></span>

<span data-ttu-id="867d5-152">JavaScript istemci jQuery ve SignalR core JavaScript dosyası başvuruları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="867d5-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="867d5-153">JQuery sürümü 1.6.4 veya 1.7.2, 1.8.2 veya 1.9.1 gibi önemli sonraki sürümleri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="867d5-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="867d5-154">Oluşturulan proxy kullanmaya karar verirseniz ayrıca SignalR oluşturulan proxy JavaScript dosyası başvuru gerekir.</span><span class="sxs-lookup"><span data-stu-id="867d5-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="867d5-155">Aşağıdaki örnek, başvuru oluşturulan proxy kullanan bir HTML sayfasında nasıl görünebileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="867d5-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="867d5-156">Bu başvuruları bu sırada yer almalıdır: jQuery ilk, son SignalR core bundan sonra ve SignalR proxy'leri.</span><span class="sxs-lookup"><span data-stu-id="867d5-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="867d5-157">Dinamik olarak üretilen proxy başvuru yapma</span><span class="sxs-lookup"><span data-stu-id="867d5-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="867d5-158">Önceki örnekte oluşturulan SignalR proxy fiziksel bir dosya için dinamik olarak üretilen JavaScript kodu başvurudur.</span><span class="sxs-lookup"><span data-stu-id="867d5-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="867d5-159">SignalR anında proxy'si için JavaScript kodunu oluşturur ve istemci yanıt "/ signalr/hub" URL olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="867d5-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="867d5-160">Sunucuda SignalR bağlantıları için farklı bir temel URL belirttiyseniz, `MapHubs` dinamik olarak üretilen proxy dosyanın URL'sini yöntemidir, özel bir URL ile "/ hub" eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="867d5-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="867d5-161">Windows 8 (Windows mağazası) JavaScript istemciler için dinamik olarak üretilen bir yerine fiziksel proxy dosyasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="867d5-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="867d5-162">Daha fazla bilgi için bkz: [SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulan proxy](#manualproxy) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="867d5-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="867d5-163">Bir ASP.NET MVC 4 Razor görünümünde tilde proxy dosya Başvurusu'ndaki uygulama kökü başvurmak için kullanın:</span><span class="sxs-lookup"><span data-stu-id="867d5-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="867d5-164">MVC 4'te SignalR kullanma hakkında daha fazla bilgi için bkz: [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="867d5-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="867d5-165">Bir ASP.NET MVC 3 Razor görünümünde kullanın `Url.Content` proxy dosya başvurusu için:</span><span class="sxs-lookup"><span data-stu-id="867d5-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="867d5-166">Bir ASP.NET Web Forms uygulamada kullanan `ResolveClientUrl` , proxy'leri dosya başvuru veya (bir tilde başlayarak) bir uygulama kök göreli bir yol kullanarak ScriptManager aracılığıyla kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="867d5-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="867d5-167">Genel kural olarak, aynı yöntemi CSS veya JavaScript dosyaları için kullandığınız "/ signalr/hub" URL'yi belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="867d5-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="867d5-168">Bir tilde kullanmadan bir URL belirtin, IIS Express kullanarak Visual Studio'da test ancak tam IIS dağıttığınızda 404 hatası ile başarısız olur bazı senaryolarda, uygulamanızın doğru şekilde çalışmayacak.</span><span class="sxs-lookup"><span data-stu-id="867d5-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="867d5-169">Daha fazla bilgi için bkz: **kök düzeyinde kaynaklara başvurular çözme** içinde [ASP.NET Web projeleri için Visual Studio'da Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx) MSDN sitesinden.</span><span class="sxs-lookup"><span data-stu-id="867d5-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="867d5-170">Visual Studio 2012'de bir web projesi hata ayıklama modunda çalıştırdığınızda ve tarayıcı olarak Internet Explorer kullanıyorsanız proxy dosyasında görebilirsiniz **Çözüm Gezgini** altında **betik belgelerini**gösterildiği gibi Aşağıdaki çizimde.</span><span class="sxs-lookup"><span data-stu-id="867d5-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Çözüm Gezgini'nde JavaScript oluşturulan proxy dosyası](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="867d5-172">Dosya içeriğini görüntülemek için çift **hub**.</span><span class="sxs-lookup"><span data-stu-id="867d5-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="867d5-173">"/ SignalR/hub" URL'sine göz atarak, Visual Studio 2012 ve Internet Explorer kullanmıyorsanız ya da hata ayıklama modunda değilse, dosyanın içeriğini alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="867d5-174">Örneğin, siteniz en çalışıyorsa, `http://localhost:56699`gidin `http://localhost:56699/SignalR/hubs` tarayıcınızda.</span><span class="sxs-lookup"><span data-stu-id="867d5-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="867d5-175">Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur</span><span class="sxs-lookup"><span data-stu-id="867d5-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="867d5-176">Dinamik olarak üretilen proxy alternatif olarak, proxy koduna sahip fiziksel bir dosya oluşturun ve bu dosyayı ilişkilendirin.</span><span class="sxs-lookup"><span data-stu-id="867d5-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="867d5-177">Önbelleğe alma veya davranışı paketleme üzerinde denetim için bunu veya sunucu yöntem çağrıları kodlama IntelliSense edilirken isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="867d5-178">Bir proxy dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="867d5-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="867d5-179">Yükleme [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="867d5-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="867d5-180">Bir komut istemi açın ve *Araçları* SignalR.exe dosyasını içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="867d5-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="867d5-181">Araçlar klasörünü şu konumdadır:</span><span class="sxs-lookup"><span data-stu-id="867d5-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="867d5-182">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="867d5-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="867d5-183">Yolu, *.dll* genellikle *bin* proje klasörünüzdeki klasörüne.</span><span class="sxs-lookup"><span data-stu-id="867d5-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="867d5-184">Bu komut adlı bir dosya oluşturur *server.js* aynı klasörde *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="867d5-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="867d5-185">PUT *server.js* dosya projenizdeki uygun bir klasör içinde uygulamanız için uygun şekilde yeniden adlandırın ve "signalr/hub" başvuru yerine, bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="867d5-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="867d5-186">Bir bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="867d5-186">How to establish a connection</span></span>

<span data-ttu-id="867d5-187">Bir bağlantı oluşturabilir önce bir bağlantı nesnesi oluşturmak, bir proxy oluşturmak ve sunucudan çağrılabilir yöntemleri için olay işleyicileri kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="867d5-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="867d5-188">Proxy ve olay işleyicileri ayarlandığında çağırarak bağlantı `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="867d5-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="867d5-189">Oluşturulan proxy kullanıyorsanız, oluşturulan proxy kod bunu sizin için yapar kendi kodunuzu bağlantı nesnesi oluşturmak yok.</span><span class="sxs-lookup"><span data-stu-id="867d5-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="867d5-190">**(Oluşturulan proxy ile) bağlantı kurun**</span><span class="sxs-lookup"><span data-stu-id="867d5-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="867d5-191">**Bir bağlantı (olmadan oluşturulan proxy)**</span><span class="sxs-lookup"><span data-stu-id="867d5-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="867d5-192">Varsayılan örnek kod kullanır "/ signalr" SignalR hizmete bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="867d5-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="867d5-193">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="867d5-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="867d5-194">Normalde, olay işleyicileri çağırmadan önce kaydetmeniz `start` bağlantı kurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="867d5-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="867d5-195">Bağlantı kurduktan sonra bazı olay işleyicilerini kaydetmek istiyorsanız, bunu yapabilirsiniz ancak, olay handler(s) çağırmadan önce en az biri kaydetmeniz gerekir `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="867d5-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="867d5-196">Bunun bir nedeni olan bir uygulamada birçok hub olabilir, ancak tetiklemek istemezsiniz `OnConnected` yalnızca bunlardan birini kullanmak için kullanacaksanız her Hub olayı.</span><span class="sxs-lookup"><span data-stu-id="867d5-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="867d5-197">Bağlantı kurulduğunda istemci yöntemi bir Hub'ın proxy varlığını ne tetiklemek için SignalR söyler olduğu `OnConnected` olay.</span><span class="sxs-lookup"><span data-stu-id="867d5-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="867d5-198">Tüm olay işleyicileri çağırmadan önce kaydetmezseniz `start` yöntemi, görebilirsiniz Hub ancak Hub'ın yöntemleri çağırmak `OnConnected` yöntemi olmaz çağrılabilir ve sunucudan hiçbir istemci yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="867d5-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="867d5-199">$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi</span><span class="sxs-lookup"><span data-stu-id="867d5-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="867d5-200">Oluşturulan proxy kullandığınızda örneklerinden gördüğünüz `$.connection.hub` bağlantı nesnesine başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="867d5-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="867d5-201">Bu çağırarak aldığınız, aynı nesnesidir `$.hubConnection()` zaman kullanmadığınız oluşturulan proxy.</span><span class="sxs-lookup"><span data-stu-id="867d5-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="867d5-202">Oluşturulan proxy kod şu deyimi yürüterek bağlantı sizin için oluşturur:</span><span class="sxs-lookup"><span data-stu-id="867d5-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Oluşturulan proxy dosyasında bir bağlantı oluşturma](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="867d5-204">Oluşturulan proxy kullanırken ile herhangi bir şey yapabilirsiniz `$.connection.hub` oluşturulan proxy kullanmadığınızda bir bağlantı nesnesi ile yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="867d5-205">Zaman uyumsuz yürütme başlangıç yöntemi</span><span class="sxs-lookup"><span data-stu-id="867d5-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="867d5-206">`start` Yöntemini zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="867d5-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="867d5-207">Döndürdüğü bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/), yani geri arama işlevleri yöntemleri gibi çağırarak ekleyebilirsiniz `pipe`, `done`, ve `fail`.</span><span class="sxs-lookup"><span data-stu-id="867d5-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="867d5-208">Bağlantı kurulduktan sonra yürütmek istediğiniz kodu varsa, sunucu yöntemi çağrısı gibi bu kodu bir geri çağırma işlevini put veya bir geri çağırma işlevini çağırın.</span><span class="sxs-lookup"><span data-stu-id="867d5-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="867d5-209">`.done` Bağlantı kurulduktan sonra herhangi bir kod sonra elinizde geri çağırma yöntemi yürütüldüğünde, `OnConnected` sunucuda olay işleyicisi yöntemi tamamlandıktan yürütülüyor.</span><span class="sxs-lookup"><span data-stu-id="867d5-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="867d5-210">Kodun sonraki satırında, sonra olarak "Şimdi bağlı" deyimi önceki örnekten yerleştirirseniz `start` yöntem çağrısı (değil, bir `.done` geri çağırma), `console.log` satır yürütülecek bağlantı kurulmadan önce aşağıda gösterildiği gibi Örnek:</span><span class="sxs-lookup"><span data-stu-id="867d5-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Bağlantı kurulduktan sonra çalışan bir kod yazmak için yanlış biçimde](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="867d5-212">Etki alanları arası bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="867d5-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="867d5-213">Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantısı `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="867d5-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="867d5-214">Varsa sayfasından `http://contoso.com` bir bağlantı kurar `http://fabrikam.com/signalr`, yani etki alanları arası bağlantı.</span><span class="sxs-lookup"><span data-stu-id="867d5-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="867d5-215">Güvenlik nedenleriyle, etki alanları arası bağlantılar, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="867d5-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="867d5-216">Etki alanları arası bağlantı kurmak için etki alanları arası bağlantılar sunucu üzerinde etkinleştirildiğinden emin olun ve bağlantı nesnesi oluşturduğunuzda bağlantı URL'si belirtin.</span><span class="sxs-lookup"><span data-stu-id="867d5-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="867d5-217">SignalR kullanacağınız uygun teknoloji etki alanları arası bağlantılar için aşağıdaki gibi [JSONP](http://en.wikipedia.org/wiki/JSONP) veya [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="867d5-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="867d5-218">Sunucu üzerinde çağırdığınızda, bu seçeneği belirleyerek etki alanları arası bağlantıları etkinleştirmek `MapHubs` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="867d5-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="867d5-219">İstemcide (olmadan oluşturulan proxy) bağlantı nesnesi oluşturduğunuzda ya da (ile oluşturulan proxy) başlatma yöntemini çağırmadan önce URL'yi belirtin.</span><span class="sxs-lookup"><span data-stu-id="867d5-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="867d5-220">**(İle oluşturulan proxy) etki alanları arası bağlantı belirtir istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="867d5-221">**Etki alanları arası bağlantı (olmadan oluşturulan proxy) belirten istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="867d5-222">Kullandığınızda `$.hubConnection` oluşturucusunu eklemek gerekmez `signalr` URL'deki otomatik olarak eklendiğinden (belirtmediğiniz sürece `useDefaultUrl` olarak `false`).</span><span class="sxs-lookup"><span data-stu-id="867d5-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="867d5-223">Farklı uç noktalar birden çok bağlantı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="867d5-224">Ayarlamazsanız `jQuery.support.cors` kodunuzda true.</span><span class="sxs-lookup"><span data-stu-id="867d5-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![JQuery.support.cors true olarak ayarlamanız gerekmez](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="867d5-226">SignalR JSONP veya CORS kullanımını işler.</span><span class="sxs-lookup"><span data-stu-id="867d5-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="867d5-227">Ayarı `jQuery.support.cors` true devre dışı bırakır için JSONP tarayıcının destekleyip CORS varsaymak SignalR neden olduğu.</span><span class="sxs-lookup"><span data-stu-id="867d5-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="867d5-228">Localhost URL'sini bağlandığınız, sunucuda etki alanları arası bağlantılar etkinleştirmediniz olsa bile uygulama yerel olarak IE 10 ile çalışması için Internet Explorer 10, etki alanları arası bağlantı göz önünde olmaz.</span><span class="sxs-lookup"><span data-stu-id="867d5-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="867d5-229">Etki alanları arası bağlantılar Internet Explorer 9 ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="867d5-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="867d5-230">Etki alanları arası bağlantılar Chrome ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="867d5-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="867d5-231">Varsayılan örnek kod kullanır "/ signalr" SignalR hizmete bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="867d5-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="867d5-232">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="867d5-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="867d5-233">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="867d5-233">How to configure the connection</span></span>

<span data-ttu-id="867d5-234">Bir bağlantısı kurmadan önce sorgu dizesi parametreleri belirtin veya taşıma yöntemini belirtin.</span><span class="sxs-lookup"><span data-stu-id="867d5-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="867d5-235">Sorgu dizesi parametreleri belirtme</span><span class="sxs-lookup"><span data-stu-id="867d5-235">How to specify query string parameters</span></span>

<span data-ttu-id="867d5-236">İstemci bağlandığında sunucuya veri göndermek istiyorsanız, sorgu dizesi parametreleri bağlantı nesnesine ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="867d5-237">Aşağıdaki örnekler bir sorgu dizesi parametresi istemci kodu ayarlamak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="867d5-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="867d5-238">**Bir sorgu dizesi değeri (ile oluşturulan proxy) başlatma yöntemi çağırmadan önce ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="867d5-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="867d5-239">**Bir sorgu dizesi değeri (olmadan oluşturulan proxy) başlatma yöntemi çağırmadan önce ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="867d5-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="867d5-240">Aşağıdaki örnek, bir sorgu dizesi parametresi sunucu kodu okuma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="867d5-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="867d5-241">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="867d5-241">How to specify the transport method</span></span>

<span data-ttu-id="867d5-242">Bağlama işleminin bir parçası olarak, bir SignalR istemci sunucuyla desteklenen en iyi aktarım belirlemek için sunucu ve istemci tarafından normalde görüşür.</span><span class="sxs-lookup"><span data-stu-id="867d5-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="867d5-243">Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, taşıma yöntemini çağırdığınızda belirterek bu anlaşma işlemi atlayabilirsiniz `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="867d5-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="867d5-244">**Belirtir (ile oluşturulan proxy) aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="867d5-245">**Belirtir (olmadan oluşturulan proxy) aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="867d5-246">Alternatif olarak, bunları denemek için SignalR istediğiniz sırayla birden çok taşıma yöntemleri belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="867d5-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="867d5-247">**(İle oluşturulan proxy) bir özel taşıma geri dönüş düzeni belirtir istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="867d5-248">**(Olmadan oluşturulan proxy) bir özel taşıma geri dönüş düzeni belirtir istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="867d5-249">Taşıma yöntemini belirtmek için aşağıdaki değerleri kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="867d5-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="867d5-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="867d5-250">"webSockets"</span></span>
- <span data-ttu-id="867d5-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="867d5-251">"foreverFrame"</span></span>
- <span data-ttu-id="867d5-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="867d5-252">"serverSentEvents"</span></span>
- <span data-ttu-id="867d5-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="867d5-253">"longPolling"</span></span>

<span data-ttu-id="867d5-254">Aşağıdaki örnekler, hangi taşıma yöntemi bir bağlantı tarafından kullanılan çıkış bulmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="867d5-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="867d5-255">**Bir bağlantıyla (oluşturulan proxy) tarafından kullanılan taşıma yöntemini görüntüler istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="867d5-256">**Bir bağlantı (olmadan oluşturulan proxy) tarafından kullanılan taşıma yöntemini görüntüler istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="867d5-257">Sunucu kodu aktarım yönteminde denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - bağlam özelliğinden istemcisi hakkında bilgi almak nasıl](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="867d5-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="867d5-258">Taşımalar ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarımları ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="867d5-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="867d5-259">Bir Hub sınıf için bir proxy alma</span><span class="sxs-lookup"><span data-stu-id="867d5-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="867d5-260">Oluşturduğunuz her bağlantı nesnesi bir veya daha fazla Hub sınıfları içeren bir SignalR hizmet bağlantısıyla ilgili bilgileri yalıtır.</span><span class="sxs-lookup"><span data-stu-id="867d5-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="867d5-261">Bir Hub sınıf ile iletişim kurmak için bir proxy nesnesi (oluşturulan proxy değil kullanıyorsanız), sizin oluşturduğunuz veya sizin yerinize oluşturulduğu kullanın.</span><span class="sxs-lookup"><span data-stu-id="867d5-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="867d5-262">İstemcide Hub sınıf adı başlamalıdır sürümü proxy adıdır.</span><span class="sxs-lookup"><span data-stu-id="867d5-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="867d5-263">SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.</span><span class="sxs-lookup"><span data-stu-id="867d5-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="867d5-264">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="867d5-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="867d5-265">**Hub için oluşturulan istemci proxy başvuru al**</span><span class="sxs-lookup"><span data-stu-id="867d5-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="867d5-266">**İstemci proxy (olmadan oluşturulan proxy) Hub sınıfı için oluşturma**</span><span class="sxs-lookup"><span data-stu-id="867d5-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="867d5-267">Hub sınıfıyla tasarlamanız varsa bir `HubName` özniteliği, durum değiştirmeden tam ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="867d5-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="867d5-268">**HubName özniteliğiyle sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="867d5-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="867d5-269">**Hub için oluşturulan istemci proxy başvuru al**</span><span class="sxs-lookup"><span data-stu-id="867d5-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="867d5-270">**İstemci proxy (olmadan oluşturulan proxy) Hub sınıfı için oluşturma**</span><span class="sxs-lookup"><span data-stu-id="867d5-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="867d5-271">Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="867d5-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="867d5-272">Sunucu Hub'ından çağırabilirsiniz bir yöntemi tanımlamak için bir olay işleyicisi Hub proxy için kullanarak eklemek `client` oluşturulan proxy veya arama özelliğini `on` oluşturulan proxy kullanmıyorsanız yöntemi.</span><span class="sxs-lookup"><span data-stu-id="867d5-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="867d5-273">Parametreleri karmaşık nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="867d5-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="867d5-274">Arama yapmadan önce olay işleyicisi ekleme `start` bağlantı kurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="867d5-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="867d5-275">(Çağrıldıktan sonra olay işleyicileri eklemek istiyorsanız `start` yöntemi, bkz. Not [bağlantı kurmak nasıl](#establishconnection) daha önce bu belge ve oluşturulan proxy kullanmadan bir yöntemi tanımlamak için sözdizimini kullanın.)</span><span class="sxs-lookup"><span data-stu-id="867d5-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="867d5-276">Yöntemi ad eşleştirme büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="867d5-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="867d5-277">Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütecek `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, veya `addcontosochatmessagetopage` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="867d5-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="867d5-278">**(İle oluşturulan proxy) istemcide yöntemi tanımlayın**</span><span class="sxs-lookup"><span data-stu-id="867d5-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="867d5-279">**İstemci (ile oluşturulan proxy) yöntemi tanımlamak için alternatif bir yolu**</span><span class="sxs-lookup"><span data-stu-id="867d5-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="867d5-280">**İstemci (oluşturulan proxy olmadan veya start yöntemi çağrıldıktan sonra eklerken) yöntemi tanımlayın**</span><span class="sxs-lookup"><span data-stu-id="867d5-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="867d5-281">**İstemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="867d5-282">Aşağıdaki örnekler yöntemi parametre olarak karmaşık bir nesne içerir.</span><span class="sxs-lookup"><span data-stu-id="867d5-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="867d5-283">**Karmaşık bir nesne (ile oluşturulan proxy) alır istemcide yöntemi tanımlayın**</span><span class="sxs-lookup"><span data-stu-id="867d5-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="867d5-284">**Karmaşık bir nesne (olmadan oluşturulan proxy) alır istemcide yöntemi tanımlayın**</span><span class="sxs-lookup"><span data-stu-id="867d5-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="867d5-285">**Karmaşık nesne tanımlar sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="867d5-286">**Karmaşık bir nesne kullanarak istemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="867d5-287">İstemciden sunucu yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="867d5-287">How to call server methods from the client</span></span>

<span data-ttu-id="867d5-288">İstemciden bir sunucu yöntemi çağırmak için `server` oluşturulan proxy özelliği veya `invoke` yöntemi oluşturulan proxy kullanmıyorsanız Hub proxy.</span><span class="sxs-lookup"><span data-stu-id="867d5-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="867d5-289">Dönüş değeri veya parametreleri karmaşık nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="867d5-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="867d5-290">Yöntem adı ortası büyük harf sürümünde hub'da geçirin.</span><span class="sxs-lookup"><span data-stu-id="867d5-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="867d5-291">SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.</span><span class="sxs-lookup"><span data-stu-id="867d5-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="867d5-292">Aşağıdaki örnekler, dönüş değeri sahip olmayan bir sunucu yöntemin nasıl çağrılacağını ve bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="867d5-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="867d5-293">**Sunucu yöntemiyle HubMethodName özniteliği yok**</span><span class="sxs-lookup"><span data-stu-id="867d5-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="867d5-294">**Karmaşık nesne tanımlar sunucu kodu içindeki bir parametre geçirildi**</span><span class="sxs-lookup"><span data-stu-id="867d5-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="867d5-295">**(İle oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="867d5-296">**(Olmadan oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="867d5-297">Hub yöntemi ile donatılmış varsa bir `HubMethodName` özniteliği, durum değiştirmeden bu adı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="867d5-298">**Sunucu yöntemini** HubMethodName özniteliğine sahip</span><span class="sxs-lookup"><span data-stu-id="867d5-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="867d5-299">**(İle oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="867d5-300">**(Olmadan oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="867d5-301">Önceki örneklerde bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="867d5-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="867d5-302">Aşağıdaki örnekler bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="867d5-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="867d5-303">**Dönüş değeri olan bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="867d5-304">**İçin kullanılan hisse senedi sınıfı** dönüş değeri</span><span class="sxs-lookup"><span data-stu-id="867d5-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="867d5-305">**(İle oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="867d5-306">**(Olmadan oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="867d5-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="867d5-307">Bağlantı ömür olayları işleme</span><span class="sxs-lookup"><span data-stu-id="867d5-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="867d5-308">SignalR aşağıdaki bağlantıyı işleyebilir ömür olayları sağlar:</span><span class="sxs-lookup"><span data-stu-id="867d5-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="867d5-309">`starting`: Herhangi bir veri bağlantısı üzerinden gönderilmeden önce oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="867d5-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="867d5-310">`received`: Herhangi bir veri bağlantısı alındığında oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="867d5-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="867d5-311">Alınan veriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="867d5-311">Provides the received data.</span></span>
- <span data-ttu-id="867d5-312">`connectionSlow`: İstemci yavaş veya sık bırakma bir bağlantı algıladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="867d5-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="867d5-313">`reconnecting`: Temel aktarımı yeniden bağlanmayı başladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="867d5-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="867d5-314">`reconnected`: Temel aktarımı bağlandı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="867d5-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="867d5-315">`stateChanged`: Bağlantı durumu değiştiğinde oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="867d5-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="867d5-316">Eski durum ve yeni bir durum (bağlanma, bağlı, Reconnecting veya bağlantı kesildi) sağlar.</span><span class="sxs-lookup"><span data-stu-id="867d5-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="867d5-317">`disconnected`: Bağlantı kesildi tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="867d5-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="867d5-318">Örneğin, fark gecikmeye neden olabilecek bağlantı sorunları olduğunda uyarı iletileri görüntülemek istiyorsanız, işleme `connectionSlow` olay.</span><span class="sxs-lookup"><span data-stu-id="867d5-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="867d5-319">**(İle oluşturulan proxy) connectionSlow olayını işle**</span><span class="sxs-lookup"><span data-stu-id="867d5-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="867d5-320">**(Olmadan oluşturulan proxy) connectionSlow olayını işle**</span><span class="sxs-lookup"><span data-stu-id="867d5-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="867d5-321">Daha fazla bilgi için bkz: [anlama ve SignalR bağlantısı ömrü olaylarını işleme](index.md).</span><span class="sxs-lookup"><span data-stu-id="867d5-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="867d5-322">Hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="867d5-322">How to handle errors</span></span>

<span data-ttu-id="867d5-323">SignalR JavaScript istemci sağlayan bir `error` olay işleyicisi ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="867d5-324">Başarısız yöntemi, bir sunucu yöntemi çağrısından kaynaklanan hatalar için bir işleyici eklemek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="867d5-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="867d5-325">Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, SignalR sonra bir hata döndürür özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir.</span><span class="sxs-lookup"><span data-stu-id="867d5-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="867d5-326">Örneğin, bir çağrı varsa `newContosoChatMessage` başarısız, hata nesnesindeki hata iletisini içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" üretim istemciler için ayrıntılı hata iletileri için ayrıntılı hata iletileri etkinleştirmek isteyip istemediğinizi ancak güvenlik nedeniyle önerilmez gönderme sorun giderme amacıyla, aşağıdaki kodu sunucuda kullanın.</span><span class="sxs-lookup"><span data-stu-id="867d5-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="867d5-327">Aşağıdaki örnek, hata olayı için bir işleyici ekleyin gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="867d5-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="867d5-328">**Bir hata işleyicisi (ile oluşturulan proxy) ekleyin**</span><span class="sxs-lookup"><span data-stu-id="867d5-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="867d5-329">**Bir hata işleyicisi (olmadan oluşturulan proxy) ekleyin**</span><span class="sxs-lookup"><span data-stu-id="867d5-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="867d5-330">Aşağıdaki örnek, bir yöntem çağrısının bir hatadan nasıl ele alınacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="867d5-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="867d5-331">**Bir yöntemi çağırma (ile oluşturulan proxy) bir hata işleme**</span><span class="sxs-lookup"><span data-stu-id="867d5-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="867d5-332">**Bir yöntemi çağırma (olmadan oluşturulan proxy) bir hata işleme**</span><span class="sxs-lookup"><span data-stu-id="867d5-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="867d5-333">Bir yöntem çağrısının başarısız olursa, `error` olayı de oluşturulur, bu nedenle, kodunuzda `error` yöntemi işleyici ve `.fail` yöntemi geri çağırma yürütün.</span><span class="sxs-lookup"><span data-stu-id="867d5-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="867d5-334">İstemci-tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="867d5-334">How to enable client-side logging</span></span>

<span data-ttu-id="867d5-335">İstemci tarafı bir bağlantıda günlüğe kaydetmeyi etkinleştirmek üzere ayarlamak `logging` özelliği çağırmadan önce bağlantıyı nesne `start` bağlantı kurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="867d5-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="867d5-336">**(İle oluşturulan proxy) günlük kaydını etkinleştir**</span><span class="sxs-lookup"><span data-stu-id="867d5-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="867d5-337">**(Olmadan oluşturulan proxy) günlük kaydını etkinleştir**</span><span class="sxs-lookup"><span data-stu-id="867d5-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="867d5-338">Günlükleri görmek için tarayıcınızın geliştirici araçları açın ve konsol sekmesine gidin. Adım adım yönergeler ve ekran bunu nasıl yapacağınızı görüntülerini gösteren bir öğretici için bkz [Server yayını ASP.NET Signalr - Enable Logging ile](index.md).</span><span class="sxs-lookup"><span data-stu-id="867d5-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
