---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: ASP.NET SignalR hub'ları API Kılavuzu - JavaScript istemci | Microsoft Docs
author: pfletcher
description: Bu belge için SignalR sürüm 2'in JavaScript istemciler, tarayıcıları ve Windows Mağazası (WinJS) applicat gibi hub API'sini kullanmaya tanıtılmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 794ab576d3c6600911f331bab7c335476e45a0c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28035341"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="50fce-103">ASP.NET SignalR hub'ları API Kılavuzu - JavaScript istemci</span><span class="sxs-lookup"><span data-stu-id="50fce-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="50fce-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="50fce-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="50fce-105">Bu belge SignalR sürüm 2'in JavaScript istemciler, tarayıcıları ve Windows Mağazası (WinJS) uygulamaları gibi için hub API kullanarak giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="50fce-105">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="50fce-106">SignalR hub'ları API bir sunucuya bağlanan istemciler ve sunucu istemcilerine uzaktan yordam çağrılarını (RPC) yapmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="50fce-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="50fce-107">Sunucu kodu, istemciler tarafından çağrılabilir yöntemlerini tanımlama ve istemci üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="50fce-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="50fce-108">İstemci kodu sunucudan çağrılabilir yöntemlerini tanımlama ve sunucu üzerinde çalışan yöntemlerini çağırın.</span><span class="sxs-lookup"><span data-stu-id="50fce-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="50fce-109">SignalR, istemci-sunucu tesisat tümünün ilgilenir.</span><span class="sxs-lookup"><span data-stu-id="50fce-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="50fce-110">SignalR kalıcı bağlantılar olarak adlandırılan bir alt düzey API de sunar.</span><span class="sxs-lookup"><span data-stu-id="50fce-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="50fce-111">SignalR, hub'lara ve kalıcı bağlantıların giriş için bkz: [SignalR giriş](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="50fce-111">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="50fce-112">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="50fce-112">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="50fce-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="50fce-113">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="50fce-114">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="50fce-114">.NET 4.5</span></span>
> - <span data-ttu-id="50fce-115">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="50fce-115">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="50fce-116">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="50fce-116">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="50fce-117">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="50fce-117">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="50fce-118">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="50fce-118">Questions and comments</span></span>
> 
> <span data-ttu-id="50fce-119">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="50fce-119">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="50fce-120">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="50fce-120">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="50fce-121">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="50fce-121">Overview</span></span>

<span data-ttu-id="50fce-122">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="50fce-122">This document contains the following sections:</span></span>

- [<span data-ttu-id="50fce-123">Oluşturulan proxy ve onu sizin için ne yaptığını</span><span class="sxs-lookup"><span data-stu-id="50fce-123">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="50fce-124">Oluşturulan proxy kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="50fce-124">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="50fce-125">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="50fce-125">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="50fce-126">Dinamik olarak üretilen proxy başvuru yapma</span><span class="sxs-lookup"><span data-stu-id="50fce-126">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="50fce-127">Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur</span><span class="sxs-lookup"><span data-stu-id="50fce-127">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="50fce-128">Bir bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="50fce-128">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="50fce-129">$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi</span><span class="sxs-lookup"><span data-stu-id="50fce-129">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="50fce-130">Zaman uyumsuz yürütme başlangıç yöntemi</span><span class="sxs-lookup"><span data-stu-id="50fce-130">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="50fce-131">Etki alanları arası bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="50fce-131">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="50fce-132">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="50fce-132">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="50fce-133">Sorgu dizesi parametreleri belirtme</span><span class="sxs-lookup"><span data-stu-id="50fce-133">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="50fce-134">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="50fce-134">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="50fce-135">Bir Hub sınıf için bir proxy alma</span><span class="sxs-lookup"><span data-stu-id="50fce-135">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="50fce-136">Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="50fce-136">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="50fce-137">İstemciden sunucu yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="50fce-137">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="50fce-138">Bağlantı ömür olayları işleme</span><span class="sxs-lookup"><span data-stu-id="50fce-138">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="50fce-139">Hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="50fce-139">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="50fce-140">İstemci-tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="50fce-140">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="50fce-141">Sunucu veya .NET istemcileri program konusunda daha fazla belgeler için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="50fce-141">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="50fce-142">SignalR hub'ları API Kılavuzu - sunucu</span><span class="sxs-lookup"><span data-stu-id="50fce-142">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="50fce-143">SignalR hub'ları API Kılavuzu - .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="50fce-143">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="50fce-144">SignalR 2 sunucu bileşeni (rağmen bir .NET istemci SignalR 2, .NET 4.0 için) yalnızca .NET 4.5 üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50fce-144">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="50fce-145">Oluşturulan proxy ve onu sizin için ne yaptığını</span><span class="sxs-lookup"><span data-stu-id="50fce-145">The generated proxy and what it does for you</span></span>

<span data-ttu-id="50fce-146">SignalR hizmet ile veya SignalR sizin için oluşturduğu ara sunucu olmadan ile iletişim kurmak için JavaScript istemci programlama yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50fce-146">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="50fce-147">Proxy sizin için ne yaptığını, sunucuya çağrıları, yazma yöntemleri bağlanmanız ve sunucuda yöntemlerini çağıran kodu söz dizimini basitleştirmek olur.</span><span class="sxs-lookup"><span data-stu-id="50fce-147">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="50fce-148">Sunucu yöntemleri çağırmak için kodu yazarken oluşturulan proxy, yerel bir işlev yürütülmekte ancak gibi görünen sözdizimi kullanmanıza olanak tanır: yazabileceğiniz `serverMethod(arg1, arg2)` yerine `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="50fce-148">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="50fce-149">Bir sunucu yöntem adı yanlış yazdınız oluşturulan proxy sözdizimi ayrıca istemci tarafı hata hemen ve intelligible sağlar.</span><span class="sxs-lookup"><span data-stu-id="50fce-149">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="50fce-150">Ve proxy'leri tanımlayan dosyasını elle oluşturursanız, ayrıca IntelliSense desteği server yöntemlerini çağıran kodu yazmak için alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50fce-150">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="50fce-151">Örneğin, sunucu üzerinde aşağıdaki Hub sınıf olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="50fce-151">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="50fce-152">Aşağıdaki kod ne JavaScript kodu çağırmaktan gibi görünüyor örnekler `NewContosoChatMessage` sunucu ve çağırmaları alma yöntemini `addContosoChatMessageToPage` sunucudan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="50fce-152">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="50fce-153">**Oluşturulan proxy ile**</span><span class="sxs-lookup"><span data-stu-id="50fce-153">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="50fce-154">**Oluşturulan proxy**</span><span class="sxs-lookup"><span data-stu-id="50fce-154">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="50fce-155">Oluşturulan proxy kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="50fce-155">When to use the generated proxy</span></span>

<span data-ttu-id="50fce-156">Sunucu çağıran bir istemci yöntemi için birden çok olay işleyicilerini kaydetmek istiyorsanız, oluşturulan proxy kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="50fce-156">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="50fce-157">Aksi takdirde, oluşturulan proxy kullanmayı seçebilirsiniz veya kodlama tercihinize bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="50fce-157">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="50fce-158">Bunu kullanmayı seçerseniz, "signalr/hub" URL'de başvuru yoksa bir `script` istemci kodunuzda öğesi.</span><span class="sxs-lookup"><span data-stu-id="50fce-158">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="50fce-159">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="50fce-159">Client setup</span></span>

<span data-ttu-id="50fce-160">JavaScript istemci jQuery ve SignalR core JavaScript dosyası başvuruları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="50fce-160">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="50fce-161">JQuery sürümü 1.6.4 veya 1.7.2, 1.8.2 veya 1.9.1 gibi önemli sonraki sürümleri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="50fce-161">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="50fce-162">Oluşturulan proxy kullanmaya karar verirseniz ayrıca SignalR oluşturulan proxy JavaScript dosyası başvuru gerekir.</span><span class="sxs-lookup"><span data-stu-id="50fce-162">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="50fce-163">Aşağıdaki örnek, başvuru oluşturulan proxy kullanan bir HTML sayfasında nasıl görünebileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="50fce-163">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="50fce-164">Bu başvuruları bu sırada yer almalıdır: jQuery ilk, son SignalR core bundan sonra ve SignalR proxy'leri.</span><span class="sxs-lookup"><span data-stu-id="50fce-164">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="50fce-165">Dinamik olarak üretilen proxy başvuru yapma</span><span class="sxs-lookup"><span data-stu-id="50fce-165">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="50fce-166">Önceki örnekte oluşturulan SignalR proxy fiziksel bir dosya için dinamik olarak üretilen JavaScript kodu başvurudur.</span><span class="sxs-lookup"><span data-stu-id="50fce-166">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="50fce-167">SignalR anında proxy'si için JavaScript kodunu oluşturur ve istemci yanıt "/ signalr/hub" URL olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="50fce-167">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="50fce-168">Sunucuda SignalR bağlantıları için farklı bir temel URL belirttiyseniz, `MapSignalR` dinamik olarak üretilen proxy dosyanın URL'sini yöntemidir, özel bir URL ile "/ hub" eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="50fce-168">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="50fce-169">Windows 8 (Windows mağazası) JavaScript istemciler için dinamik olarak üretilen bir yerine fiziksel proxy dosyasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="50fce-169">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="50fce-170">Daha fazla bilgi için bkz: [SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulan proxy](#manualproxy) bu konuda daha sonra.</span><span class="sxs-lookup"><span data-stu-id="50fce-170">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="50fce-171">Bir ASP.NET MVC 4 veya 5 Razor görünümünde tilde proxy dosya Başvurusu'ndaki uygulama kökü başvurmak için kullanın:</span><span class="sxs-lookup"><span data-stu-id="50fce-171">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="50fce-172">MVC 5'te SignalR kullanma hakkında daha fazla bilgi için bkz: [SignalR ve MVC 5 ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="50fce-172">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="50fce-173">Bir ASP.NET MVC 3 Razor görünümünde kullanın `Url.Content` proxy dosya başvurusu için:</span><span class="sxs-lookup"><span data-stu-id="50fce-173">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="50fce-174">Bir ASP.NET Web Forms uygulamada kullanan `ResolveClientUrl` , proxy'leri dosya başvuru veya (bir tilde başlayarak) bir uygulama kök göreli bir yol kullanarak ScriptManager aracılığıyla kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="50fce-174">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="50fce-175">Genel kural olarak, aynı yöntemi CSS veya JavaScript dosyaları için kullandığınız "/ signalr/hub" URL'yi belirtmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="50fce-175">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="50fce-176">Bir tilde kullanmadan bir URL belirtin, IIS Express kullanarak Visual Studio'da test ancak tam IIS dağıttığınızda 404 hatası ile başarısız olur bazı senaryolarda, uygulamanızın doğru şekilde çalışmayacak.</span><span class="sxs-lookup"><span data-stu-id="50fce-176">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="50fce-177">Daha fazla bilgi için bkz: **kök düzeyinde kaynaklara başvurular çözme** içinde [ASP.NET Web projeleri için Visual Studio'da Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx) MSDN sitesinden.</span><span class="sxs-lookup"><span data-stu-id="50fce-177">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="50fce-178">Bir web projesi, Visual Studio 2013'te hata ayıklama modunda çalıştırdığınızda ve tarayıcı olarak Internet Explorer kullanıyorsanız proxy dosyasında görebilirsiniz **Çözüm Gezgini** altında **betik belgelerini**gösterildiği gibi Aşağıdaki çizimde.</span><span class="sxs-lookup"><span data-stu-id="50fce-178">When you run a web project in Visual Studio 2013 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Çözüm Gezgini'nde JavaScript oluşturulan proxy dosyası](hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="50fce-180">Dosya içeriğini görüntülemek için çift **hub**.</span><span class="sxs-lookup"><span data-stu-id="50fce-180">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="50fce-181">"/ SignalR/hub" URL'sine göz atarak, Visual Studio 2012 veya 2013 ve Internet Explorer kullanmıyorsanız ya da hata ayıklama modunda değilse, dosyanın içeriğini alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50fce-181">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="50fce-182">Örneğin, siteniz en çalışıyorsa, `http://localhost:56699`gidin `http://localhost:56699/SignalR/hubs` tarayıcınızda.</span><span class="sxs-lookup"><span data-stu-id="50fce-182">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="50fce-183">Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur</span><span class="sxs-lookup"><span data-stu-id="50fce-183">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="50fce-184">Dinamik olarak üretilen proxy alternatif olarak, proxy koduna sahip fiziksel bir dosya oluşturun ve bu dosyayı ilişkilendirin.</span><span class="sxs-lookup"><span data-stu-id="50fce-184">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="50fce-185">Önbelleğe alma veya davranışı paketleme üzerinde denetim için bunu veya sunucu yöntem çağrıları kodlama IntelliSense edilirken isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50fce-185">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="50fce-186">Bir proxy dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="50fce-186">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="50fce-187">Yükleme [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="50fce-187">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="50fce-188">Bir komut istemi açın ve *Araçları* SignalR.exe dosyasını içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="50fce-188">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="50fce-189">Araçlar klasörünü şu konumdadır:</span><span class="sxs-lookup"><span data-stu-id="50fce-189">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="50fce-190">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="50fce-190">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="50fce-191">Yolu, *.dll* genellikle *bin* proje klasörünüzdeki klasörüne.</span><span class="sxs-lookup"><span data-stu-id="50fce-191">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="50fce-192">Bu komut adlı bir dosya oluşturur *server.js* aynı klasörde *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="50fce-192">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="50fce-193">PUT *server.js* dosya projenizdeki uygun bir klasör içinde uygulamanız için uygun şekilde yeniden adlandırın ve "signalr/hub" başvuru yerine, bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="50fce-193">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="50fce-194">Bir bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="50fce-194">How to establish a connection</span></span>

<span data-ttu-id="50fce-195">Bir bağlantı oluşturabilir önce bir bağlantı nesnesi oluşturmak, bir proxy oluşturmak ve sunucudan çağrılabilir yöntemleri için olay işleyicileri kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="50fce-195">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="50fce-196">Proxy ve olay işleyicileri ayarlandığında çağırarak bağlantı `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="50fce-196">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="50fce-197">Oluşturulan proxy kullanıyorsanız, oluşturulan proxy kod bunu sizin için yapar kendi kodunuzu bağlantı nesnesi oluşturmak yok.</span><span class="sxs-lookup"><span data-stu-id="50fce-197">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="50fce-198">**(Oluşturulan proxy ile) bağlantı kurun**</span><span class="sxs-lookup"><span data-stu-id="50fce-198">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="50fce-199">**Bir bağlantı (olmadan oluşturulan proxy)**</span><span class="sxs-lookup"><span data-stu-id="50fce-199">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="50fce-200">Varsayılan örnek kod kullanır "/ signalr" SignalR hizmete bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="50fce-200">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="50fce-201">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="50fce-201">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="50fce-202">Varsayılan olarak, geçerli sunucu hub konumdur; farklı bir sunucuya bağlanıyorsanız çağırmadan önce URL'yi belirtin `start` yöntemi, aşağıdaki örnekte gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="50fce-202">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="50fce-203">Normalde, olay işleyicileri çağırmadan önce kaydetmeniz `start` bağlantı kurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="50fce-203">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="50fce-204">Bağlantı kurduktan sonra bazı olay işleyicilerini kaydetmek istiyorsanız, bunu yapabilirsiniz ancak, olay handler(s) çağırmadan önce en az biri kaydetmeniz gerekir `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="50fce-204">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="50fce-205">Bunun bir nedeni olan bir uygulamada birçok hub olabilir, ancak tetiklemek istemezsiniz `OnConnected` yalnızca bunlardan birini kullanmak için kullanacaksanız her Hub olayı.</span><span class="sxs-lookup"><span data-stu-id="50fce-205">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="50fce-206">Bağlantı kurulduğunda istemci yöntemi bir Hub'ın proxy varlığını ne tetiklemek için SignalR söyler olduğu `OnConnected` olay.</span><span class="sxs-lookup"><span data-stu-id="50fce-206">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="50fce-207">Tüm olay işleyicileri çağırmadan önce kaydetmezseniz `start` yöntemi, görebilirsiniz Hub ancak Hub'ın yöntemleri çağırmak `OnConnected` yöntemi olmaz çağrılabilir ve sunucudan hiçbir istemci yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="50fce-207">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="50fce-208">$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi</span><span class="sxs-lookup"><span data-stu-id="50fce-208">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="50fce-209">Oluşturulan proxy kullandığınızda örneklerinden gördüğünüz `$.connection.hub` bağlantı nesnesine başvuruyor.</span><span class="sxs-lookup"><span data-stu-id="50fce-209">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="50fce-210">Bu çağırarak aldığınız, aynı nesnesidir `$.hubConnection()` zaman kullanmadığınız oluşturulan proxy.</span><span class="sxs-lookup"><span data-stu-id="50fce-210">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="50fce-211">Oluşturulan proxy kod şu deyimi yürüterek bağlantı sizin için oluşturur:</span><span class="sxs-lookup"><span data-stu-id="50fce-211">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Oluşturulan proxy dosyasında bir bağlantı oluşturma](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="50fce-213">Oluşturulan proxy kullanırken ile herhangi bir şey yapabilirsiniz `$.connection.hub` oluşturulan proxy kullanmadığınızda bir bağlantı nesnesi ile yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50fce-213">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="50fce-214">Zaman uyumsuz yürütme başlangıç yöntemi</span><span class="sxs-lookup"><span data-stu-id="50fce-214">Asynchronous execution of the start method</span></span>

<span data-ttu-id="50fce-215">`start` Yöntemini zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="50fce-215">The `start` method executes asynchronously.</span></span> <span data-ttu-id="50fce-216">Döndürdüğü bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/), yani geri arama işlevleri yöntemleri gibi çağırarak ekleyebilirsiniz `pipe`, `done`, ve `fail`.</span><span class="sxs-lookup"><span data-stu-id="50fce-216">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="50fce-217">Bağlantı kurulduktan sonra yürütmek istediğiniz kodu varsa, sunucu yöntemi çağrısı gibi bu kodu bir geri çağırma işlevini put veya bir geri çağırma işlevini çağırın.</span><span class="sxs-lookup"><span data-stu-id="50fce-217">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="50fce-218">`.done` Bağlantı kurulduktan sonra herhangi bir kod sonra elinizde geri çağırma yöntemi yürütüldüğünde, `OnConnected` sunucuda olay işleyicisi yöntemi tamamlandıktan yürütülüyor.</span><span class="sxs-lookup"><span data-stu-id="50fce-218">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="50fce-219">Kodun sonraki satırında, sonra olarak "Şimdi bağlı" deyimi önceki örnekten yerleştirirseniz `start` yöntem çağrısı (değil, bir `.done` geri çağırma), `console.log` satır yürütülecek bağlantı kurulmadan önce aşağıda gösterildiği gibi Örnek:</span><span class="sxs-lookup"><span data-stu-id="50fce-219">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Bağlantı kurulduktan sonra çalışan bir kod yazmak için yanlış biçimde](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="50fce-221">Etki alanları arası bağlantı kurmak nasıl</span><span class="sxs-lookup"><span data-stu-id="50fce-221">How to establish a cross-domain connection</span></span>

<span data-ttu-id="50fce-222">Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantısı `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="50fce-222">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="50fce-223">Varsa sayfasından `http://contoso.com` bir bağlantı kurar `http://fabrikam.com/signalr`, yani etki alanları arası bağlantı.</span><span class="sxs-lookup"><span data-stu-id="50fce-223">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="50fce-224">Güvenlik nedenleriyle, etki alanları arası bağlantılar, varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="50fce-224">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="50fce-225">Etki alanı istekleri arası 1.x tek EnableCrossDomain tarafından kontrol SignalR bayrak.</span><span class="sxs-lookup"><span data-stu-id="50fce-225">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="50fce-226">Bu bayrak JSONP ve CORS isteklerini denetlenir.</span><span class="sxs-lookup"><span data-stu-id="50fce-226">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="50fce-227">Daha fazla esneklik için tüm CORS desteği SignalR sunucu bileşeninden kaldırıldı (JavaScript istemcilerinin kullanmaya devam CORS normalde tarayıcı onu destekleyen algılanırsa), ve yeni OWIN ara yazılımı sağlandıktan bu senaryoları desteklemek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50fce-227">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="50fce-228">(Etki alanları arası istekleri eski tarayıcılarda desteklemek için) istemcide JSONP gerekirse, açıkça ayarlayarak etkinleştirilmesi gerekir `EnableJSONP` üzerinde `HubConfiguration` nesnesine `true`, aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="50fce-228">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="50fce-229">CORS az güvenlidir JSONP varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="50fce-229">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="50fce-230">**Microsoft.Owin.Cors projenize ekleme:** bu Kitaplığı'nı yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="50fce-230">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="50fce-231">Bu komut 2.1.0 ekleyeceksiniz projenize paketin sürümü.</span><span class="sxs-lookup"><span data-stu-id="50fce-231">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="50fce-232">UseCors çağırma</span><span class="sxs-lookup"><span data-stu-id="50fce-232">Calling UseCors</span></span>

 <span data-ttu-id="50fce-233">Aşağıdaki kod parçacığını SignalR 2'de etki alanları arası bağlantılarını uygulamadan gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="50fce-233">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span> 

<span data-ttu-id="50fce-234">**SignalR 2'deki uygulama etki alanları arası istekleri**</span><span class="sxs-lookup"><span data-stu-id="50fce-234">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="50fce-235">Aşağıdaki kod, CORS veya JSONP SignalR 2 projede etkinleştirme gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="50fce-235">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="50fce-236">Bu kod örneği kullanan `Map` ve `RunSignalR` yerine `MapSignalR`, CORS desteği gerektiren SignalR istekleri için CORS Ara çalışır (yerine belirtilen yoldaki tüm trafik için `MapSignalR`.) Harita, belirli bir URL öneki için yerine tüm uygulama için çalıştırmak için gereken diğer tüm ara yazılımı için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="50fce-236">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - <span data-ttu-id="50fce-237">Ayarlamazsanız `jQuery.support.cors` kodunuzda true.</span><span class="sxs-lookup"><span data-stu-id="50fce-237">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![JQuery.support.cors true olarak ayarlamanız gerekmez](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="50fce-239">SignalR CORS kullanımını işler.</span><span class="sxs-lookup"><span data-stu-id="50fce-239">SignalR handles the use of CORS.</span></span> <span data-ttu-id="50fce-240">Ayarı `jQuery.support.cors` true devre dışı bırakır için JSONP tarayıcının destekleyip CORS varsaymak SignalR neden olduğu.</span><span class="sxs-lookup"><span data-stu-id="50fce-240">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="50fce-241">Localhost URL'sini bağlandığınız, sunucuda etki alanları arası bağlantılar etkinleştirmediniz olsa bile uygulama yerel olarak IE 10 ile çalışması için Internet Explorer 10, etki alanları arası bağlantı göz önünde olmaz.</span><span class="sxs-lookup"><span data-stu-id="50fce-241">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="50fce-242">Etki alanları arası bağlantılar Internet Explorer 9 ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="50fce-242">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="50fce-243">Etki alanları arası bağlantılar Chrome ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="50fce-243">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="50fce-244">Varsayılan örnek kod kullanır "/ signalr" SignalR hizmete bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="50fce-244">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="50fce-245">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="50fce-245">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="50fce-246">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="50fce-246">How to configure the connection</span></span>

<span data-ttu-id="50fce-247">Bir bağlantısı kurmadan önce sorgu dizesi parametreleri belirtin veya taşıma yöntemini belirtin.</span><span class="sxs-lookup"><span data-stu-id="50fce-247">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="50fce-248">Sorgu dizesi parametreleri belirtme</span><span class="sxs-lookup"><span data-stu-id="50fce-248">How to specify query string parameters</span></span>

<span data-ttu-id="50fce-249">İstemci bağlandığında sunucuya veri göndermek istiyorsanız, sorgu dizesi parametreleri bağlantı nesnesine ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50fce-249">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="50fce-250">Aşağıdaki örnekler bir sorgu dizesi parametresi istemci kodu ayarlamak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="50fce-250">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="50fce-251">**Bir sorgu dizesi değeri (ile oluşturulan proxy) başlatma yöntemi çağırmadan önce ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="50fce-251">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="50fce-252">**Bir sorgu dizesi değeri (olmadan oluşturulan proxy) başlatma yöntemi çağırmadan önce ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="50fce-252">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="50fce-253">Aşağıdaki örnek, bir sorgu dizesi parametresi sunucu kodu okuma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="50fce-253">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="50fce-254">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="50fce-254">How to specify the transport method</span></span>

<span data-ttu-id="50fce-255">Bağlama işleminin bir parçası olarak, bir SignalR istemci sunucuyla desteklenen en iyi aktarım belirlemek için sunucu ve istemci tarafından normalde görüşür.</span><span class="sxs-lookup"><span data-stu-id="50fce-255">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="50fce-256">Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, taşıma yöntemini çağırdığınızda belirterek bu anlaşma işlemi atlayabilirsiniz `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="50fce-256">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="50fce-257">**Belirtir (ile oluşturulan proxy) aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-257">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="50fce-258">**Belirtir (olmadan oluşturulan proxy) aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-258">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="50fce-259">Alternatif olarak, bunları denemek için SignalR istediğiniz sırayla birden çok taşıma yöntemleri belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="50fce-259">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="50fce-260">**(İle oluşturulan proxy) bir özel taşıma geri dönüş düzeni belirtir istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-260">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="50fce-261">**(Olmadan oluşturulan proxy) bir özel taşıma geri dönüş düzeni belirtir istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-261">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="50fce-262">Taşıma yöntemini belirtmek için aşağıdaki değerleri kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="50fce-262">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="50fce-263">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="50fce-263">"webSockets"</span></span>
- <span data-ttu-id="50fce-264">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="50fce-264">"foreverFrame"</span></span>
- <span data-ttu-id="50fce-265">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="50fce-265">"serverSentEvents"</span></span>
- <span data-ttu-id="50fce-266">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="50fce-266">"longPolling"</span></span>

<span data-ttu-id="50fce-267">Aşağıdaki örnekler, hangi taşıma yöntemi bir bağlantı tarafından kullanılan çıkış bulmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="50fce-267">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="50fce-268">**Bir bağlantıyla (oluşturulan proxy) tarafından kullanılan taşıma yöntemini görüntüler istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-268">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="50fce-269">**Bir bağlantı (olmadan oluşturulan proxy) tarafından kullanılan taşıma yöntemini görüntüler istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-269">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="50fce-270">Sunucu kodu aktarım yönteminde denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR hub'ları API Kılavuzu - Server - bağlam özelliğinden istemcisi hakkında bilgi almak nasıl](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="50fce-270">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="50fce-271">Taşımalar ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarımları ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="50fce-271">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="50fce-272">Bir Hub sınıf için bir proxy alma</span><span class="sxs-lookup"><span data-stu-id="50fce-272">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="50fce-273">Oluşturduğunuz her bağlantı nesnesi bir veya daha fazla Hub sınıfları içeren bir SignalR hizmet bağlantısıyla ilgili bilgileri yalıtır.</span><span class="sxs-lookup"><span data-stu-id="50fce-273">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="50fce-274">Bir Hub sınıf ile iletişim kurmak için bir proxy nesnesi (oluşturulan proxy değil kullanıyorsanız), sizin oluşturduğunuz veya sizin yerinize oluşturulduğu kullanın.</span><span class="sxs-lookup"><span data-stu-id="50fce-274">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="50fce-275">İstemcide Hub sınıf adı başlamalıdır sürümü proxy adıdır.</span><span class="sxs-lookup"><span data-stu-id="50fce-275">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="50fce-276">SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.</span><span class="sxs-lookup"><span data-stu-id="50fce-276">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="50fce-277">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="50fce-277">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="50fce-278">**Hub için oluşturulan istemci proxy başvuru al**</span><span class="sxs-lookup"><span data-stu-id="50fce-278">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="50fce-279">**İstemci proxy (olmadan oluşturulan proxy) Hub sınıfı için oluşturma**</span><span class="sxs-lookup"><span data-stu-id="50fce-279">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="50fce-280">Hub sınıfıyla tasarlamanız varsa bir `HubName` özniteliği, durum değiştirmeden tam ad kullanın.</span><span class="sxs-lookup"><span data-stu-id="50fce-280">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="50fce-281">**HubName özniteliğiyle sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="50fce-281">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="50fce-282">**Hub için oluşturulan istemci proxy başvuru al**</span><span class="sxs-lookup"><span data-stu-id="50fce-282">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="50fce-283">**İstemci proxy (olmadan oluşturulan proxy) Hub sınıfı için oluşturma**</span><span class="sxs-lookup"><span data-stu-id="50fce-283">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="50fce-284">Sunucu çağırabilirsiniz istemcide yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="50fce-284">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="50fce-285">Sunucu Hub'ından çağırabilirsiniz bir yöntemi tanımlamak için bir olay işleyicisi Hub proxy için kullanarak eklemek `client` oluşturulan proxy veya arama özelliğini `on` oluşturulan proxy kullanmıyorsanız yöntemi.</span><span class="sxs-lookup"><span data-stu-id="50fce-285">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="50fce-286">Parametreleri karmaşık nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="50fce-286">The parameters can be complex objects.</span></span>

<span data-ttu-id="50fce-287">Arama yapmadan önce olay işleyicisi ekleme `start` bağlantı kurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="50fce-287">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="50fce-288">(Çağrıldıktan sonra olay işleyicileri eklemek istiyorsanız `start` yöntemi, bkz. Not [bağlantı kurmak nasıl](#establishconnection) daha önce bu belge ve oluşturulan proxy kullanmadan bir yöntemi tanımlamak için sözdizimini kullanın.)</span><span class="sxs-lookup"><span data-stu-id="50fce-288">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="50fce-289">Yöntemi ad eşleştirme büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="50fce-289">Method name matching is case-insensitive.</span></span> <span data-ttu-id="50fce-290">Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütecek `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, veya `addcontosochatmessagetopage` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="50fce-290">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="50fce-291">**(İle oluşturulan proxy) istemcide yöntemi tanımlayın**</span><span class="sxs-lookup"><span data-stu-id="50fce-291">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="50fce-292">**İstemci (ile oluşturulan proxy) yöntemi tanımlamak için alternatif bir yolu**</span><span class="sxs-lookup"><span data-stu-id="50fce-292">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="50fce-293">**İstemci (oluşturulan proxy olmadan veya start yöntemi çağrıldıktan sonra eklerken) yöntemi tanımlayın**</span><span class="sxs-lookup"><span data-stu-id="50fce-293">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="50fce-294">**İstemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-294">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="50fce-295">Aşağıdaki örnekler yöntemi parametre olarak karmaşık bir nesne içerir.</span><span class="sxs-lookup"><span data-stu-id="50fce-295">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="50fce-296">**Karmaşık bir nesne (ile oluşturulan proxy) alır istemcide yöntemi tanımlayın**</span><span class="sxs-lookup"><span data-stu-id="50fce-296">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="50fce-297">**Karmaşık bir nesne (olmadan oluşturulan proxy) alır istemcide yöntemi tanımlayın**</span><span class="sxs-lookup"><span data-stu-id="50fce-297">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="50fce-298">**Karmaşık nesne tanımlar sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-298">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="50fce-299">**Karmaşık bir nesne kullanarak istemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-299">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="50fce-300">İstemciden sunucu yöntemleri çağırmak nasıl</span><span class="sxs-lookup"><span data-stu-id="50fce-300">How to call server methods from the client</span></span>

<span data-ttu-id="50fce-301">İstemciden bir sunucu yöntemi çağırmak için `server` oluşturulan proxy özelliği veya `invoke` yöntemi oluşturulan proxy kullanmıyorsanız Hub proxy.</span><span class="sxs-lookup"><span data-stu-id="50fce-301">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="50fce-302">Dönüş değeri veya parametreleri karmaşık nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="50fce-302">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="50fce-303">Yöntem adı ortası büyük harf sürümünde hub'da geçirin.</span><span class="sxs-lookup"><span data-stu-id="50fce-303">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="50fce-304">SignalR otomatik olarak bu değişiklik yapar ve bu böylece JavaScript kodu JavaScript kurallarına uygun.</span><span class="sxs-lookup"><span data-stu-id="50fce-304">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="50fce-305">Aşağıdaki örnekler, dönüş değeri sahip olmayan bir sunucu yöntemin nasıl çağrılacağını ve bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="50fce-305">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="50fce-306">**Sunucu yöntemiyle HubMethodName özniteliği yok**</span><span class="sxs-lookup"><span data-stu-id="50fce-306">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="50fce-307">**Karmaşık nesne tanımlar sunucu kodu içindeki bir parametre geçirildi**</span><span class="sxs-lookup"><span data-stu-id="50fce-307">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="50fce-308">**(İle oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-308">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="50fce-309">**(Olmadan oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-309">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="50fce-310">Hub yöntemi ile donatılmış varsa bir `HubMethodName` özniteliği, durum değiştirmeden bu adı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50fce-310">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="50fce-311">**Sunucu yöntemini** HubMethodName özniteliğine sahip</span><span class="sxs-lookup"><span data-stu-id="50fce-311">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="50fce-312">**(İle oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-312">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="50fce-313">**(Olmadan oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-313">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="50fce-314">Önceki örneklerde bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="50fce-314">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="50fce-315">Aşağıdaki örnekler bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="50fce-315">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="50fce-316">**Dönüş değeri olan bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-316">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="50fce-317">**İçin kullanılan hisse senedi sınıfı** dönüş değeri</span><span class="sxs-lookup"><span data-stu-id="50fce-317">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="50fce-318">**(İle oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-318">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="50fce-319">**(Olmadan oluşturulan proxy) sunucu yöntemini çağıran istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="50fce-319">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="50fce-320">Bağlantı ömür olayları işleme</span><span class="sxs-lookup"><span data-stu-id="50fce-320">How to handle connection lifetime events</span></span>

<span data-ttu-id="50fce-321">SignalR aşağıdaki bağlantıyı işleyebilir ömür olayları sağlar:</span><span class="sxs-lookup"><span data-stu-id="50fce-321">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="50fce-322">`starting`: Herhangi bir veri bağlantısı üzerinden gönderilmeden önce oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="50fce-322">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="50fce-323">`received`: Herhangi bir veri bağlantısı alındığında oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="50fce-323">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="50fce-324">Alınan veriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="50fce-324">Provides the received data.</span></span>
- <span data-ttu-id="50fce-325">`connectionSlow`: İstemci yavaş veya sık bırakma bir bağlantı algıladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50fce-325">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="50fce-326">`reconnecting`: Temel aktarımı yeniden bağlanmayı başladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50fce-326">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="50fce-327">`reconnected`: Temel aktarımı bağlandı tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="50fce-327">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="50fce-328">`stateChanged`: Bağlantı durumu değiştiğinde oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="50fce-328">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="50fce-329">Eski durum ve yeni bir durum (bağlanma, bağlı, Reconnecting veya bağlantı kesildi) sağlar.</span><span class="sxs-lookup"><span data-stu-id="50fce-329">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="50fce-330">`disconnected`: Bağlantı kesildi tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="50fce-330">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="50fce-331">Örneğin, fark gecikmeye neden olabilecek bağlantı sorunları olduğunda uyarı iletileri görüntülemek istiyorsanız, işleme `connectionSlow` olay.</span><span class="sxs-lookup"><span data-stu-id="50fce-331">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="50fce-332">**(İle oluşturulan proxy) connectionSlow olayını işle**</span><span class="sxs-lookup"><span data-stu-id="50fce-332">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="50fce-333">**(Olmadan oluşturulan proxy) connectionSlow olayını işle**</span><span class="sxs-lookup"><span data-stu-id="50fce-333">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="50fce-334">Daha fazla bilgi için bkz: [anlama ve SignalR bağlantısı ömrü olaylarını işleme](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="50fce-334">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="50fce-335">Hataların nasıl işleneceğini</span><span class="sxs-lookup"><span data-stu-id="50fce-335">How to handle errors</span></span>

<span data-ttu-id="50fce-336">SignalR JavaScript istemci sağlayan bir `error` olay işleyicisi ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50fce-336">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="50fce-337">Başarısız yöntemi, bir sunucu yöntemi çağrısından kaynaklanan hatalar için bir işleyici eklemek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50fce-337">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="50fce-338">Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, SignalR sonra bir hata döndürür özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir.</span><span class="sxs-lookup"><span data-stu-id="50fce-338">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="50fce-339">Örneğin, bir çağrı varsa `newContosoChatMessage` başarısız, hata nesnesindeki hata iletisini içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" üretim istemciler için ayrıntılı hata iletileri için ayrıntılı hata iletileri etkinleştirmek isteyip istemediğinizi ancak güvenlik nedeniyle önerilmez gönderme sorun giderme amacıyla, aşağıdaki kodu sunucuda kullanın.</span><span class="sxs-lookup"><span data-stu-id="50fce-339">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="50fce-340">Aşağıdaki örnek, hata olayı için bir işleyici ekleyin gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="50fce-340">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="50fce-341">**Bir hata işleyicisi (ile oluşturulan proxy) ekleyin**</span><span class="sxs-lookup"><span data-stu-id="50fce-341">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="50fce-342">**Bir hata işleyicisi (olmadan oluşturulan proxy) ekleyin**</span><span class="sxs-lookup"><span data-stu-id="50fce-342">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="50fce-343">Aşağıdaki örnek, bir yöntem çağrısının bir hatadan nasıl ele alınacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="50fce-343">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="50fce-344">**Bir yöntemi çağırma (ile oluşturulan proxy) bir hata işleme**</span><span class="sxs-lookup"><span data-stu-id="50fce-344">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="50fce-345">**Bir yöntemi çağırma (olmadan oluşturulan proxy) bir hata işleme**</span><span class="sxs-lookup"><span data-stu-id="50fce-345">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="50fce-346">Bir yöntem çağrısının başarısız olursa, `error` olayı de oluşturulur, bu nedenle, kodunuzda `error` yöntemi işleyici ve `.fail` yöntemi geri çağırma yürütün.</span><span class="sxs-lookup"><span data-stu-id="50fce-346">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="50fce-347">İstemci-tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="50fce-347">How to enable client-side logging</span></span>

<span data-ttu-id="50fce-348">İstemci tarafı bir bağlantıda günlüğe kaydetmeyi etkinleştirmek üzere ayarlamak `logging` özelliği çağırmadan önce bağlantıyı nesne `start` bağlantı kurmak için yöntem.</span><span class="sxs-lookup"><span data-stu-id="50fce-348">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="50fce-349">**(İle oluşturulan proxy) günlük kaydını etkinleştir**</span><span class="sxs-lookup"><span data-stu-id="50fce-349">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="50fce-350">**(Olmadan oluşturulan proxy) günlük kaydını etkinleştir**</span><span class="sxs-lookup"><span data-stu-id="50fce-350">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="50fce-351">Günlükleri görmek için tarayıcınızın geliştirici araçları açın ve konsol sekmesine gidin. Adım adım yönergeler ve ekran bunu nasıl yapacağınızı görüntülerini gösteren bir öğretici için bkz [Server yayını ASP.NET Signalr - Enable Logging ile](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span><span class="sxs-lookup"><span data-stu-id="50fce-351">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging).</span></span>
