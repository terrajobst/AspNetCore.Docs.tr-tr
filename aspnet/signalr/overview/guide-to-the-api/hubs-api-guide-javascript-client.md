---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: ASP.NET SignalR Hubs API Kılavuzu - JavaScript istemcisi | Microsoft Docs
author: bradygaster
description: Bu belge için SignalR sürüm 2 JavaScript istemcilerinin, tarayıcılar ve Windows Store (WinJS) applicat gibi hub'ları API kullanarak bir giriş sağlar...
ms.author: bradyg
ms.date: 01/15/2019
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 7689fa5b6a3e81c9f767831423eb3efad72e1ab3
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836603"
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a><span data-ttu-id="19f71-103">ASP.NET SignalR Hubs API Kılavuzu - JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="19f71-103">ASP.NET SignalR Hubs API Guide - JavaScript Client</span></span>
====================

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="19f71-104">Bu belge için SignalR sürüm 2 JavaScript istemcilerinin, tarayıcıları ve Windows Store (WinJS) uygulamalar gibi hub'ları API kullanmaya giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="19f71-104">This document provides an introduction to using the Hubs API for SignalR version 2 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
>
> <span data-ttu-id="19f71-105">SignalR hub'ları API, bir sunucuya bağlanan istemcilerin ve istemcilerin sunucuya uzaktan yordam çağrısı (RPC) oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="19f71-105">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="19f71-106">Sunucu kodu, istemciler tarafından çağrılabilen yöntemleri tanımlamak ve bir istemcide çalışmasına yöntemler çağırır.</span><span class="sxs-lookup"><span data-stu-id="19f71-106">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="19f71-107">İstemci kodu sunucudan çağıran yöntemleri tanımlamak ve sunucu üzerinde çalışan yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="19f71-107">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="19f71-108">SignalR tüm istemci-sunucu tesisat sizin için üstlenir.</span><span class="sxs-lookup"><span data-stu-id="19f71-108">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
>
> <span data-ttu-id="19f71-109">SignalR kalıcı bağlantı adlı bir alt düzey API'si de sunar.</span><span class="sxs-lookup"><span data-stu-id="19f71-109">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="19f71-110">SignalR hub'ları ve kalıcı bağlantılar için bir giriş için bkz [signalr'a giriş](../getting-started/introduction-to-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="19f71-110">For an introduction to SignalR, Hubs, and Persistent Connections, see [Introduction to SignalR](../getting-started/introduction-to-signalr.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="19f71-111">Bu konu başlığında kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="19f71-111">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="19f71-112">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="19f71-112">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)
> - <span data-ttu-id="19f71-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="19f71-113">.NET 4.5</span></span>
> - <span data-ttu-id="19f71-114">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="19f71-114">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="19f71-115">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="19f71-115">Previous versions of this topic</span></span>
>
> <span data-ttu-id="19f71-116">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="19f71-116">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="19f71-117">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="19f71-117">Questions and comments</span></span>
>
> <span data-ttu-id="19f71-118">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="19f71-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="19f71-119">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="19f71-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="19f71-120">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="19f71-120">Overview</span></span>

<span data-ttu-id="19f71-121">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="19f71-121">This document contains the following sections:</span></span>

- [<span data-ttu-id="19f71-122">Oluşturulan proxy ve bunu sizin yerinize yapar</span><span class="sxs-lookup"><span data-stu-id="19f71-122">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="19f71-123">Oluşturulan proxy kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="19f71-123">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="19f71-124">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="19f71-124">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="19f71-125">Dinamik olarak oluşturulan proxy başvuru yapma</span><span class="sxs-lookup"><span data-stu-id="19f71-125">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="19f71-126">Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur</span><span class="sxs-lookup"><span data-stu-id="19f71-126">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="19f71-127">Nasıl bir bağlantı kurmak için</span><span class="sxs-lookup"><span data-stu-id="19f71-127">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="19f71-128">$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi</span><span class="sxs-lookup"><span data-stu-id="19f71-128">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="19f71-129">Zaman uyumsuz yürütme başlangıç yöntemi</span><span class="sxs-lookup"><span data-stu-id="19f71-129">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="19f71-130">Etki alanları arası bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="19f71-130">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="19f71-131">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="19f71-131">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="19f71-132">Sorgu dizesi parametrelerini belirtme</span><span class="sxs-lookup"><span data-stu-id="19f71-132">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="19f71-133">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="19f71-133">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="19f71-134">Bir Hub sınıfı için bir ara sunucu alma</span><span class="sxs-lookup"><span data-stu-id="19f71-134">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="19f71-135">Sunucu çağıran istemciye yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="19f71-135">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="19f71-136">İstemciden sunucu yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="19f71-136">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="19f71-137">Bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="19f71-137">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="19f71-138">Hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="19f71-138">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="19f71-139">İstemci tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="19f71-139">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="19f71-140">Sunucu veya .NET istemcileri program hakkında daha fazla belge için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="19f71-140">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="19f71-141">SignalR hub API Kılavuzu - sunucu</span><span class="sxs-lookup"><span data-stu-id="19f71-141">SignalR Hubs API Guide - Server</span></span>](hubs-api-guide-server.md)
- [<span data-ttu-id="19f71-142">SignalR hub API Kılavuzu - .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="19f71-142">SignalR Hubs API Guide - .NET Client</span></span>](hubs-api-guide-net-client.md)

<span data-ttu-id="19f71-143">SignalR 2 sunucu bileşeni (yoktur ancak bir .NET istemci SignalR 2 .NET 4.0 için) yalnızca .NET 4.5 üzerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="19f71-143">The SignalR 2 server component is only available on .NET 4.5 (though there is a .NET client for SignalR 2 on .NET 4.0).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="19f71-144">Oluşturulan proxy ve bunu sizin yerinize yapar</span><span class="sxs-lookup"><span data-stu-id="19f71-144">The generated proxy and what it does for you</span></span>

<span data-ttu-id="19f71-145">Bir SignalR hizmeti ile veya olmadan SignalR sizin için oluşturduğu bir ara sunucu ile iletişim kurmak için JavaScript istemci programlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19f71-145">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="19f71-146">Proxy sizin için ne yaptığını, sunucuya çağrıları, yazma yöntemleri bağlanın ve sunucu üzerinde yöntemleri çağırmak kod dizimini basitleştiren olur.</span><span class="sxs-lookup"><span data-stu-id="19f71-146">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="19f71-147">Sunucu yöntemlerini çağırmak için kod yazdığınızda, oluşturulan proxy, yerel bir işlev yürütülürken ancak gibi görünen sözdizimi kullanmanıza olanak sağlar: yazabileceğiniz `serverMethod(arg1, arg2)` yerine `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="19f71-147">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="19f71-148">Bir sunucu yöntem adı yanlış ise oluşturulan proxy söz dizimi anında ve anlaşılır bir istemci tarafı hata de sağlar.</span><span class="sxs-lookup"><span data-stu-id="19f71-148">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="19f71-149">Ve proxy'ler tanımlayan dosyayı el ile oluşturursanız, ayrıca IntelliSense desteği server yöntemlerini çağıran kod yazmak için alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19f71-149">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="19f71-150">Örneğin, sunucu üzerinde aşağıdaki Hub sınıfına olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="19f71-150">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="19f71-151">Aşağıdaki kod örnekleri ne JavaScript kodu çağırmak için nasıl göründüğüne Göster `NewContosoChatMessage` sunucu ve çağrılarını alma yöntemini `addContosoChatMessageToPage` sunucudan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19f71-151">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="19f71-152">**Oluşturulan proxy ile**</span><span class="sxs-lookup"><span data-stu-id="19f71-152">**With the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="19f71-153">**Üretilen Ara sunucu olmadan**</span><span class="sxs-lookup"><span data-stu-id="19f71-153">**Without the generated proxy**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="19f71-154">Oluşturulan proxy kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="19f71-154">When to use the generated proxy</span></span>

<span data-ttu-id="19f71-155">Sunucu çağıran bir istemci yöntemi için birden çok olay işleyicisi kaydetmek istiyorsanız, oluşturulan proxy kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="19f71-155">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="19f71-156">Aksi takdirde, oluşturulan proxy kullanmayı seçebilirsiniz veya kodlama tercihinize bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="19f71-156">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="19f71-157">Bunu kullanmayı seçerseniz, "signalr/hubs" URL'de başvuru yoksa bir `script` istemci kodunun öğesi.</span><span class="sxs-lookup"><span data-stu-id="19f71-157">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="19f71-158">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="19f71-158">Client setup</span></span>

<span data-ttu-id="19f71-159">JavaScript istemcisi, jQuery ve SignalR çekirdek JavaScript dosyası başvuruları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="19f71-159">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="19f71-160">JQuery sürümü 1.6.4 veya 1.7.2'ye, 1.8.2 veya 1.9.1 gibi önemli sonraki sürümler olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="19f71-160">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="19f71-161">Oluşturulan proxy kullanmaya karar verirseniz, ayrıca SignalR oluşturulan proxy JavaScript dosyasına bir başvuru gerekir.</span><span class="sxs-lookup"><span data-stu-id="19f71-161">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="19f71-162">Aşağıdaki örnek, başvuruları oluşturulan proxy kullanan bir HTML sayfasında nasıl görünebileceğini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="19f71-162">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="19f71-163">Bu sırada bu başvuruları eklenmelidir: jQuery ilk, son bundan sonra SignalR çekirdek ve SignalR proxy'leri.</span><span class="sxs-lookup"><span data-stu-id="19f71-163">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="19f71-164">Dinamik olarak oluşturulan proxy başvuru yapma</span><span class="sxs-lookup"><span data-stu-id="19f71-164">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="19f71-165">Önceki örnekte oluşturulan SignalR proxy fiziksel bir dosya için dinamik olarak oluşturulan JavaScript kodu başvurudur.</span><span class="sxs-lookup"><span data-stu-id="19f71-165">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="19f71-166">SignalR hareket halindeyken proxy için JavaScript kodunu oluşturur ve istemciye yanıt "/ signalr/hubs" URL olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="19f71-166">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="19f71-167">Sunucuda SignalR bağlantıları için farklı bir temel URL belirttiyseniz, `MapSignalR` yöntemi dinamik olarak oluşturulan proxy dosyanın URL'si olduğundan, özel bir URL ile "/ hubs" eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="19f71-167">If you specified a different base URL for SignalR connections on the server in your `MapSignalR` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="19f71-168">Windows 8 (Windows Store) JavaScript istemciler için dinamik olarak üretilen bir yerine fiziksel proxy dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="19f71-168">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="19f71-169">Daha fazla bilgi için [SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulan proxy](#manualproxy) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="19f71-169">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="19f71-170">Bir ASP.NET MVC 4 veya 5 Razor Görünümü'nde, proxy dosya Başvurusu'ndaki uygulama kökü başvurmak için tilde kullanın:</span><span class="sxs-lookup"><span data-stu-id="19f71-170">In an ASP.NET MVC 4 or 5 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="19f71-171">MVC 5'te SignalR kullanma hakkında daha fazla bilgi için bkz. [SignalR ve MVC 5 ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="19f71-171">For more information about using SignalR in MVC 5, see [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="19f71-172">Bir ASP.NET MVC 3 Razor Görünümü'nde kullanmak `Url.Content` proxy dosya başvuru amacıyla:</span><span class="sxs-lookup"><span data-stu-id="19f71-172">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="19f71-173">Bir ASP.NET Web formları uygulamasında `ResolveClientUrl` , proxy'leri dosya başvurusu veya (bir tilde işareti ile başlayan) bir uygulama kök göreli bir yol kullanarak ScriptManager aracılığıyla kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="19f71-173">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="19f71-174">Genel bir kural olarak, CSS ya da JavaScript dosyaları için kullanan "/ signalr/hubs" URL'yi belirtmek için aynı yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="19f71-174">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="19f71-175">Bir tilde kullanmadan bir URL belirtirseniz, bazı senaryolarda, IIS Express kullanarak Visual Studio'da test ancak tam IIS dağıttığınızda bir 404 hatası ile başarısız olur, uygulamanızın düzgün çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="19f71-175">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="19f71-176">Daha fazla bilgi için **kök düzeyinde kaynaklara başvurular çözümleniyor** içinde [ASP.NET Web projeleri için Visual Studio'daki Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx) MSDN sitesinden.</span><span class="sxs-lookup"><span data-stu-id="19f71-176">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="19f71-177">Bir web projesi, Visual Studio 2017'de hata ayıklama modunda çalıştırdığınızda ve Internet Explorer tarayıcı olarak kullanıyorsanız, proxy dosyasında görebilirsiniz **Çözüm Gezgini** altında **betikleri**.</span><span class="sxs-lookup"><span data-stu-id="19f71-177">When you run a web project in Visual Studio 2017 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Scripts**.</span></span>

<span data-ttu-id="19f71-178">Dosyanın içeriğini görmek için çift tıklatın **hubs**.</span><span class="sxs-lookup"><span data-stu-id="19f71-178">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="19f71-179">Visual Studio 2012 veya 2013 ve Internet Explorer kullanmıyorsanız veya hata ayıklama modunda değilse, dosyanın içeriğini "/ signalR/hubs" URL'sine göz atarak da edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19f71-179">If you are not using Visual Studio 2012 or 2013 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="19f71-180">Sitenizi en çalışıyorsa Örneğin, `http://localhost:56699`Git `http://localhost:56699/SignalR/hubs` tarayıcınızda.</span><span class="sxs-lookup"><span data-stu-id="19f71-180">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="19f71-181">Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur</span><span class="sxs-lookup"><span data-stu-id="19f71-181">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="19f71-182">Dinamik olarak oluşturulan proxy alternatif, proxy kodu fiziksel bir dosya oluşturun ve bu dosyaya başvuru.</span><span class="sxs-lookup"><span data-stu-id="19f71-182">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="19f71-183">Denetim önbelleğe alma veya davranışı'nı paketlemek için bunu veya sunucu yöntemlere yapılan çağrılar Kodladığınız IntelliSense almak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19f71-183">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="19f71-184">Proxy dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="19f71-184">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="19f71-185">Yükleme [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="19f71-185">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="19f71-186">Bir komut istemi açın ve *Araçları* SignalR.exe dosyasını içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="19f71-186">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="19f71-187">Araçlar klasöründe şu konumdadır:</span><span class="sxs-lookup"><span data-stu-id="19f71-187">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. <span data-ttu-id="19f71-188">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="19f71-188">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="19f71-189">Yolu, *.dll* genellikle *bin* proje klasörünüzde klasör.</span><span class="sxs-lookup"><span data-stu-id="19f71-189">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="19f71-190">Bu komut, adlı bir dosya oluşturur. *server.js* aynı klasörde *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="19f71-190">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="19f71-191">PUT *server.js* projenize uygun bir klasörde dosya, uygulamanız için uygun şekilde yeniden adlandırın ve "signalr/hubs" başvurusu yerine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="19f71-191">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="19f71-192">Nasıl bir bağlantı kurmak için</span><span class="sxs-lookup"><span data-stu-id="19f71-192">How to establish a connection</span></span>

<span data-ttu-id="19f71-193">Bir bağlantı kurabilmesi için önce bir bağlantı nesnesi oluşturun, bir proxy oluşturma ve sunucudan çağrılabilir yöntemleri için olay işleyicilerini kaydedin gerekir.</span><span class="sxs-lookup"><span data-stu-id="19f71-193">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="19f71-194">Proxy ve olay işleyicileri ayarlandığında çağırarak bağlantı kurmak `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19f71-194">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="19f71-195">Oluşturulan proxy kullanıyorsanız, oluşturulan proxy kodu bunu sizin yerinize yapar kendi kodunuzda bağlantı nesnesi oluşturmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="19f71-195">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="19f71-196">**(Oluşturulan proxy ile) bağlantı kurma**</span><span class="sxs-lookup"><span data-stu-id="19f71-196">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="19f71-197">**Bir bağlantı (olmadan oluşturulan proxy)**</span><span class="sxs-lookup"><span data-stu-id="19f71-197">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="19f71-198">Varsayılan örnek kodu kullanır "/ signalr" SignalR hizmetinize bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="19f71-198">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="19f71-199">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="19f71-199">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>

<span data-ttu-id="19f71-200">Varsayılan olarak, geçerli sunucu hub konumdur; farklı bir sunucuya bağlanıyorsanız çağırmadan önce URL'yi belirtin `start` aşağıdaki örnekte gösterildiği gibi yöntemi:</span><span class="sxs-lookup"><span data-stu-id="19f71-200">By default, the hub location is the current server; if you are connecting to a different server, specify the URL before calling the `start` method, as shown in the following example:</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> <span data-ttu-id="19f71-201">Normalde, olay işleyicileri çağırmadan önce kaydetmeniz `start` bağlantı kurmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19f71-201">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="19f71-202">Bağlantı kurulduktan sonra bazı olay işleyicileri kaydetmek istediğiniz, bunu yapabilirsiniz, ancak en az bir çağırmadan önce olay handler(s) kaydetmelisiniz `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19f71-202">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="19f71-203">Bunun bir nedeni olduğundan bir uygulamada birçok Hubs olabilir, ancak tetiklemesini istemezsiniz `OnConnected` yalnızca bunlardan birini kullanmak için kullanacaksanız, her hub'da olay.</span><span class="sxs-lookup"><span data-stu-id="19f71-203">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="19f71-204">Bağlantı kurulduğunda, bir istemci yöntemi bir Hub'ın proxy varlığını ne tetiklemek için SignalR söyler olan `OnConnected` olay.</span><span class="sxs-lookup"><span data-stu-id="19f71-204">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="19f71-205">Tüm olay işleyicileri çağırmadan önce kaydetmezseniz `start` yöntemi oluşturabileceksiniz Hub ancak Hub'ın üzerinde yöntem çağırmak `OnConnected` yöntemi olmaz çağrılabilir ve sunucudan hiçbir istemci yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="19f71-205">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="19f71-206">$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi</span><span class="sxs-lookup"><span data-stu-id="19f71-206">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="19f71-207">Oluşturulan proxy kullandığınızda örneklerden gördüğünüz gibi `$.connection.hub` bağlantı nesneye başvurur.</span><span class="sxs-lookup"><span data-stu-id="19f71-207">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="19f71-208">Çağırarak alma aynı nesne budur `$.hubConnection()` zaman kullanmadığınız oluşturulan proxy.</span><span class="sxs-lookup"><span data-stu-id="19f71-208">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="19f71-209">Oluşturulan proxy kodu, aşağıdaki deyimi yürüterek bağlantıyı sizin için oluşturur:</span><span class="sxs-lookup"><span data-stu-id="19f71-209">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Oluşturulan proxy dosyasında bağlantı oluşturma](hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="19f71-211">Oluşturulan proxy kullanırken ile her şeyi yapabilirsiniz `$.connection.hub` oluşturulan proxy kullanmadığınızda bir bağlantı nesnesi ile yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19f71-211">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="19f71-212">Zaman uyumsuz yürütme başlangıç yöntemi</span><span class="sxs-lookup"><span data-stu-id="19f71-212">Asynchronous execution of the start method</span></span>

<span data-ttu-id="19f71-213">`start` Yöntemi zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="19f71-213">The `start` method executes asynchronously.</span></span> <span data-ttu-id="19f71-214">Döndürür bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/), yani geri çağırma işlevleri gibi yöntemleri çağırarak ekleyebilirsiniz `pipe`, `done`, ve `fail`.</span><span class="sxs-lookup"><span data-stu-id="19f71-214">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="19f71-215">Bağlantı kurulduktan sonra yürütmek istediğiniz kodu varsa, sunucu yönteme bir çağrı gibi kod içindeki geri arama işlevi put veya bir geri çağırma işlevini çağırın.</span><span class="sxs-lookup"><span data-stu-id="19f71-215">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="19f71-216">`.done` Bağlantı kurulduktan sonra herhangi kod sonra sahip olduğunuz geri çağırma yöntemi yürütüldüğünde, `OnConnected` sunucuda olay işleyicisi yöntemi tamamlandıktan yürütülüyor.</span><span class="sxs-lookup"><span data-stu-id="19f71-216">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="19f71-217">Olarak kodun sonraki satırında, sonra "Şimdi bağlı" deyimi önceki örnekten koyarsanız `start` yöntem çağrısının (değil, bir `.done` geri çağırma), `console.log` satır çalıştırılacağı bağlantı kurulmadan önce aşağıda gösterildiği gibi Örnek:</span><span class="sxs-lookup"><span data-stu-id="19f71-217">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Bağlantı kurulduktan sonra çalışan kod yazmak için yanlış bir yol](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="19f71-219">Etki alanları arası bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="19f71-219">How to establish a cross-domain connection</span></span>

<span data-ttu-id="19f71-220">Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantı `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="19f71-220">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="19f71-221">Varsa sayfasından `http://contoso.com` için bir bağlantı kurar `http://fabrikam.com/signalr`, diğer bir deyişle etki alanları arası bağlantı.</span><span class="sxs-lookup"><span data-stu-id="19f71-221">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="19f71-222">Güvenlik nedenleriyle, etki alanları arası bağlantıları varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="19f71-222">For security reasons, cross-domain connections are disabled by default.</span></span>

<span data-ttu-id="19f71-223">SignalR 1.x, etki alanı istekleri çapraz tek bir EnableCrossDomain tarafından kontrol bayrağını.</span><span class="sxs-lookup"><span data-stu-id="19f71-223">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="19f71-224">Bu bayrak hem JSONP hem de CORS istekleri denetlenir.</span><span class="sxs-lookup"><span data-stu-id="19f71-224">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="19f71-225">Daha fazla esneklik için tüm CORS desteği SignalR sunucu bileşeninden kaldırıldı (JavaScript istemcilerinin kullanmaya devam CORS normalde tarayıcı onu desteklediğini algılanırsa), ve yeni OWIN ara yazılımı yapılan bu senaryoları desteklemek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="19f71-225">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript clients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="19f71-226">(Eski tarayıcılarda etki alanları arası istekleri desteklemek için) istemcide JSONP gerekiyorsa, ayarlayarak açıkça etkinleştirilmesi gerekecektir `EnableJSONP` üzerinde `HubConfiguration` nesnesini `true`, aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="19f71-226">If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="19f71-227">CORS az güvenli olsa da JSONP varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="19f71-227">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="19f71-228">**Microsoft.Owin.Cors projenize ekleme:** Bu kitaplığını yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="19f71-228">**Adding Microsoft.Owin.Cors to your project:** To install this library, run the following command in the Package Manager Console:</span></span>

`Install-Package Microsoft.Owin.Cors`

<span data-ttu-id="19f71-229">Bu komut 2.1.0 ekleyeceksiniz projenize paketin sürümü.</span><span class="sxs-lookup"><span data-stu-id="19f71-229">This command will add the 2.1.0 version of the package to your project.</span></span>

### <a name="calling-usecors"></a><span data-ttu-id="19f71-230">UseCors çağırma</span><span class="sxs-lookup"><span data-stu-id="19f71-230">Calling UseCors</span></span>

 <span data-ttu-id="19f71-231">Aşağıdaki kod parçacığı, etki alanları arası bağlantıları SignalR 2'de uygulama gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="19f71-231">The following code snippet demonstrates how to implement cross-domain connections in SignalR 2.</span></span>

<span data-ttu-id="19f71-232">**Uygulama etki alanları arası isteklerde SignalR 2**</span><span class="sxs-lookup"><span data-stu-id="19f71-232">**Implementing cross-domain requests in SignalR 2**</span></span>

<span data-ttu-id="19f71-233">Aşağıdaki kod bir SignalR 2 projesinde CORS veya JSONP etkinleştirme gösterir.</span><span class="sxs-lookup"><span data-stu-id="19f71-233">The following code demonstrates how to enable CORS or JSONP in a SignalR 2 project.</span></span> <span data-ttu-id="19f71-234">Bu kod örneği kullanan `Map` ve `RunSignalR` yerine `MapSignalR`, böylece CORS desteği gerektiren SignalR istekleri için CORS ara yazılımı çalıştırır (yerine tüm trafiği yolda belirtilen `MapSignalR`.) Harita, uygulamanın tamamı yerine belirli bir URL ön eki için çalıştırması gereken diğer tüm ara yazılımı için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="19f71-234">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) Map can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE]
>
> - <span data-ttu-id="19f71-235">Ayarlamamanız `jQuery.support.cors` kodunuzda true.</span><span class="sxs-lookup"><span data-stu-id="19f71-235">Don't set `jQuery.support.cors` to true in your code.</span></span>
>
>     ![JQuery.support.cors true olarak ayarlamanız gerekmez](hubs-api-guide-javascript-client/_static/image7.png)
>
>     <span data-ttu-id="19f71-237">SignalR CORS kullanımını işler.</span><span class="sxs-lookup"><span data-stu-id="19f71-237">SignalR handles the use of CORS.</span></span> <span data-ttu-id="19f71-238">Ayar `jQuery.support.cors` doğru devre dışı bırakır için JSONP tarayıcıyı destekleyen CORS varsaymak SignalR neden olduğundan.</span><span class="sxs-lookup"><span data-stu-id="19f71-238">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="19f71-239">Localhost URL bağlanırken sunucunun etki alanları arası bağlantılarda etkinleştirmediyseniz bile uygulama yerel olarak IE 10 ile çalışması için Internet Explorer 10, etki alanları arası bağlantı, göz önünde bulundurun olmaz.</span><span class="sxs-lookup"><span data-stu-id="19f71-239">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="19f71-240">Etki alanları arası bağlantıları Internet Explorer 9 ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="19f71-240">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="19f71-241">Etki alanları arası bağlantıları Chrome ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="19f71-241">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="19f71-242">Varsayılan örnek kodu kullanır "/ signalr" SignalR hizmetinize bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="19f71-242">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="19f71-243">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - /signalr URL](hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="19f71-243">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="19f71-244">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="19f71-244">How to configure the connection</span></span>

<span data-ttu-id="19f71-245">Bir bağlantı kurmadan önce sorgu dizesi parametreleri belirtin veya taşıma yöntemini belirtin.</span><span class="sxs-lookup"><span data-stu-id="19f71-245">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="19f71-246">Sorgu dizesi parametrelerini belirtme</span><span class="sxs-lookup"><span data-stu-id="19f71-246">How to specify query string parameters</span></span>

<span data-ttu-id="19f71-247">İstemci bağlandığında sunucuya veri göndermek istiyorsanız, bağlantı nesnesi için sorgu dizesi parametreleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19f71-247">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="19f71-248">Aşağıdaki örnekler bir sorgu dizesi parametresi istemci kodda ayarlama işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="19f71-248">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="19f71-249">**(İle oluşturulan proxy) başlatma yöntemi çağrılmadan önce bir sorgu dizesi değerini ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="19f71-249">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="19f71-250">**(Olmadan oluşturulan proxy) başlatma yöntemi çağrılmadan önce bir sorgu dizesi değerini ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="19f71-250">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

<span data-ttu-id="19f71-251">Aşağıdaki örnekte, sunucu kodu bir sorgu dizesi parametresi okunacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="19f71-251">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="19f71-252">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="19f71-252">How to specify the transport method</span></span>

<span data-ttu-id="19f71-253">Bağlama işleminin bir parçası, SignalR istemci ile sunucu hem sunucu hem de istemci tarafından desteklenen en iyi aktarım belirlemek için normalde görüşür.</span><span class="sxs-lookup"><span data-stu-id="19f71-253">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="19f71-254">Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, taşıma yöntemini çağırdığınızda belirterek bu anlaşma işlemi atlayabilirsiniz `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19f71-254">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="19f71-255">**Belirtir (ile oluşturulan proxy) aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-255">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

<span data-ttu-id="19f71-256">**Belirtir (olmadan oluşturulan proxy) aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-256">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

<span data-ttu-id="19f71-257">Alternatif olarak, bunları denemek için SignalR istediğiniz sırayla birden fazla taşıma yöntemleri belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="19f71-257">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="19f71-258">**İstemci kodu, bir özel taşıma geri dönüş şeması (ile oluşturulan proxy) belirtir**</span><span class="sxs-lookup"><span data-stu-id="19f71-258">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="19f71-259">**İstemci kodu, bir özel taşıma geri dönüş şeması (olmadan oluşturulan proxy) belirtir**</span><span class="sxs-lookup"><span data-stu-id="19f71-259">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="19f71-260">Taşıma yöntemi belirtmek için aşağıdaki değerleri kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="19f71-260">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="19f71-261">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="19f71-261">"webSockets"</span></span>
- <span data-ttu-id="19f71-262">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="19f71-262">"foreverFrame"</span></span>
- <span data-ttu-id="19f71-263">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="19f71-263">"serverSentEvents"</span></span>
- <span data-ttu-id="19f71-264">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="19f71-264">"longPolling"</span></span>

<span data-ttu-id="19f71-265">Aşağıdaki örnekler, hangi aktarım yöntemi bir bağlantı tarafından kullanıldığını nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="19f71-265">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="19f71-266">**Görüntüler (ile oluşturulan proxy) bir bağlantı tarafından kullanılan aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-266">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

<span data-ttu-id="19f71-267">**Görüntüler (olmadan oluşturulan proxy) bir bağlantı tarafından kullanılan aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-267">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

<span data-ttu-id="19f71-268">Sunucu kodu aktarım yöntemi denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - bağlam özelliği istemci hakkında bilgi almak nasıl](hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="19f71-268">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="19f71-269">Aktarım ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarım ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="19f71-269">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="19f71-270">Bir Hub sınıfı için bir ara sunucu alma</span><span class="sxs-lookup"><span data-stu-id="19f71-270">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="19f71-271">Oluşturduğunuz her bir bağlantı nesnesi bir veya daha fazla Hub sınıfları içeren bir SignalR service bağlantısı ile ilgili bilgileri yalıtır.</span><span class="sxs-lookup"><span data-stu-id="19f71-271">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="19f71-272">Bir Hub sınıfı ile iletişim kurmak için bir proxy nesnesi (oluşturulan proxy değil kullanıyorsanız), sizin oluşturduğunuz veya sizin için oluşturulduğu kullanın.</span><span class="sxs-lookup"><span data-stu-id="19f71-272">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="19f71-273">İstemcide Ara sunucu adını Hub sınıf adı ortası büyük küçük harfleri bir sürümü var.</span><span class="sxs-lookup"><span data-stu-id="19f71-273">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="19f71-274">Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.</span><span class="sxs-lookup"><span data-stu-id="19f71-274">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="19f71-275">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="19f71-275">**Hub class on server**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

<span data-ttu-id="19f71-276">**Hub için oluşturulan istemci proxy'si bir başvuru alma**</span><span class="sxs-lookup"><span data-stu-id="19f71-276">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

<span data-ttu-id="19f71-277">**Hub sınıfı (olmadan oluşturulan proxy) için istemci proxy oluşturma**</span><span class="sxs-lookup"><span data-stu-id="19f71-277">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="19f71-278">Hub sınıfınıza tasarlamanız, bir `HubName` özniteliği, servis talebi değiştirmeden tam adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="19f71-278">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="19f71-279">**HubName özniteliğine sahip sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="19f71-279">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

<span data-ttu-id="19f71-280">**Hub için oluşturulan istemci proxy'si bir başvuru alma**</span><span class="sxs-lookup"><span data-stu-id="19f71-280">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

<span data-ttu-id="19f71-281">**Hub sınıfı (olmadan oluşturulan proxy) için istemci proxy oluşturma**</span><span class="sxs-lookup"><span data-stu-id="19f71-281">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="19f71-282">Sunucu çağıran istemciye yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="19f71-282">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="19f71-283">Bir Hub'ından sunucu çağıran bir yöntemi tanımlamak için bir olay işleyicisi Hub proxy için kullanarak ekleme `client` oluşturulan proxy veya arama özelliğini `on` oluşturulan proxy kullanmıyorsanız yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19f71-283">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="19f71-284">Parametreleri karmaşık nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="19f71-284">The parameters can be complex objects.</span></span>

<span data-ttu-id="19f71-285">Çağırmadan önce olay işleyicisi ekleme `start` bağlantı kurmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19f71-285">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="19f71-286">(Çağırdıktan sonra olay işleyicileri eklemek istiyorsanız `start` yöntemi,'ündeki nota bakın [bağlantı kurmak nasıl](#establishconnection) daha önce açıklanan belge ve oluşturulan proxy kullanmadan bir yöntemi tanımlamak için sözdizimini kullanın.)</span><span class="sxs-lookup"><span data-stu-id="19f71-286">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="19f71-287">Yöntem adı ile eşleşen büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="19f71-287">Method name matching is case-insensitive.</span></span> <span data-ttu-id="19f71-288">Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütülür `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, veya `addcontosochatmessagetopage` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="19f71-288">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="19f71-289">**(İle oluşturulan proxy) istemcide yöntemi tanımlayın.**</span><span class="sxs-lookup"><span data-stu-id="19f71-289">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

<span data-ttu-id="19f71-290">**İstemci (ile oluşturulan proxy) yöntemi tanımlamak için alternatif bir yolu**</span><span class="sxs-lookup"><span data-stu-id="19f71-290">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

<span data-ttu-id="19f71-291">**İstemci (oluşturulan proxy olmadan veya başlangıç yöntemi çağrıldıktan sonra eklerken) yöntemi tanımlayın.**</span><span class="sxs-lookup"><span data-stu-id="19f71-291">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

<span data-ttu-id="19f71-292">**İstemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-292">**Server code that calls the client method**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

<span data-ttu-id="19f71-293">Aşağıdaki örnekler, karmaşık bir nesne bir yöntem parametresi olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="19f71-293">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="19f71-294">**Karmaşık bir nesnenin (ile oluşturulan proxy) alan istemci yöntemi tanımlayın.**</span><span class="sxs-lookup"><span data-stu-id="19f71-294">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

<span data-ttu-id="19f71-295">**Karmaşık bir nesnenin (olmadan oluşturulan proxy) alan istemci yöntemi tanımlayın.**</span><span class="sxs-lookup"><span data-stu-id="19f71-295">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

<span data-ttu-id="19f71-296">**Karmaşık bir nesne tanımlayan bir sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-296">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

<span data-ttu-id="19f71-297">**Karmaşık bir nesne kullanarak istemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-297">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="19f71-298">İstemciden sunucu yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="19f71-298">How to call server methods from the client</span></span>

<span data-ttu-id="19f71-299">İstemciden gelen sunucu yöntemini çağırmak için kullanın `server` oluşturulan proxy özelliğini veya `invoke` oluşturulan proxy kullanmıyorsanız Hub proxy yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19f71-299">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="19f71-300">Dönüş değeri veya parametreleri karmaşık nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="19f71-300">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="19f71-301">Yöntem adı ortası büyük sürümünde hub'da geçirin.</span><span class="sxs-lookup"><span data-stu-id="19f71-301">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="19f71-302">Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.</span><span class="sxs-lookup"><span data-stu-id="19f71-302">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="19f71-303">Aşağıdaki örnekler, dönüş değeri olmayan bir sunucu yöntemin nasıl çağrılacağını ve bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="19f71-303">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="19f71-304">**Sunucu yöntemiyle HubMethodName öznitelik yok**</span><span class="sxs-lookup"><span data-stu-id="19f71-304">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

<span data-ttu-id="19f71-305">**Karmaşık bir nesne tanımlayan bir sunucu kodu bir parametre geçirildi.**</span><span class="sxs-lookup"><span data-stu-id="19f71-305">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

<span data-ttu-id="19f71-306">**Çağıran Metoda (ile oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-306">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

<span data-ttu-id="19f71-307">**Çağıran Metoda (olmadan oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-307">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

<span data-ttu-id="19f71-308">Hub yönteminin ile donatılmış varsa bir `HubMethodName` özniteliği, servis talebi değiştirmeden bu adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="19f71-308">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="19f71-309">**Sunucu yöntemi** HubMethodName özniteliğine sahip</span><span class="sxs-lookup"><span data-stu-id="19f71-309">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

<span data-ttu-id="19f71-310">**Çağıran Metoda (ile oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-310">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="19f71-311">**Çağıran Metoda (olmadan oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-311">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

<span data-ttu-id="19f71-312">Önceki örneklerde, dönüş değeri olmayan bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="19f71-312">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="19f71-313">Aşağıdaki örnekler bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="19f71-313">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="19f71-314">**Bir dönüş değerine sahip bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-314">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

<span data-ttu-id="19f71-315">**Stok sınıfı için kullanılan** dönüş değeri</span><span class="sxs-lookup"><span data-stu-id="19f71-315">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

<span data-ttu-id="19f71-316">**Çağıran Metoda (ile oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-316">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

<span data-ttu-id="19f71-317">**Çağıran Metoda (olmadan oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="19f71-317">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="19f71-318">Bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="19f71-318">How to handle connection lifetime events</span></span>

<span data-ttu-id="19f71-319">SignalR işleyebilirsiniz ömür olayları aşağıdaki bağlantı sağlar:</span><span class="sxs-lookup"><span data-stu-id="19f71-319">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="19f71-320">`starting`: Herhangi bir veri bağlantısı üzerinden gönderilmeden önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="19f71-320">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="19f71-321">`received`: Herhangi bir veri bağlantısı alındığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="19f71-321">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="19f71-322">Alınan veriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="19f71-322">Provides the received data.</span></span>
- <span data-ttu-id="19f71-323">`connectionSlow`: İstemci yavaş veya sık bırakma bağlantı tespit ettiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="19f71-323">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="19f71-324">`reconnecting`: Temel alınan aktarımda yeniden bağlanmayı başladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="19f71-324">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="19f71-325">`reconnected`: Temel alınan aktarımda bağlandığınızda oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="19f71-325">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="19f71-326">`stateChanged`: Bağlantı durumu değiştiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="19f71-326">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="19f71-327">Eski durum ve yeni bir durum (bağlanma, bağlı, yeniden bağlanılıyor veya bağlantısı kesilmiş) sağlar.</span><span class="sxs-lookup"><span data-stu-id="19f71-327">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="19f71-328">`disconnected`: Bağlantı kesildiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="19f71-328">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="19f71-329">Örneğin, belirgin gecikmeler neden olabilecek bağlantı sorunları olduğunda uyarı iletileri görüntülemek istiyorsanız, tanıtıcı `connectionSlow` olay.</span><span class="sxs-lookup"><span data-stu-id="19f71-329">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="19f71-330">**(İle oluşturulan proxy) connectionSlow olayını işle**</span><span class="sxs-lookup"><span data-stu-id="19f71-330">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

<span data-ttu-id="19f71-331">**(Olmadan oluşturulan proxy) connectionSlow olayını işle**</span><span class="sxs-lookup"><span data-stu-id="19f71-331">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

<span data-ttu-id="19f71-332">Daha fazla bilgi için [anlama ve signalr'da bağlantı ömrü olaylarını işleme](handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="19f71-332">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](handling-connection-lifetime-events.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="19f71-333">Hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="19f71-333">How to handle errors</span></span>

<span data-ttu-id="19f71-334">SignalR JavaScript istemci sağlayan bir `error` olayı için bir işleyici ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19f71-334">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="19f71-335">Fail yöntemi, bir sunucu yöntem çağrısından kaynaklanan hatalar için bir işleyici eklemek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="19f71-335">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="19f71-336">Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, bir hatanın ardından SignalR döndüren özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir.</span><span class="sxs-lookup"><span data-stu-id="19f71-336">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="19f71-337">Örneğin, bir çağrı `newContosoChatMessage` başarısız, hata iletisi hata nesnesi içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" güvenlik nedeniyle, ayrıntılı hata iletileri için etkinleştirmek istiyorsanız ancak üretimde istemciler için ayrıntılı hata iletileri önerilmez gönderme sorun giderme amacıyla sunucu üzerinde aşağıdaki kodu kullanın.</span><span class="sxs-lookup"><span data-stu-id="19f71-337">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

<span data-ttu-id="19f71-338">Aşağıdaki örnek, bir hata olayı işleyicisi eklemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="19f71-338">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="19f71-339">**(İle oluşturulan proxy) bir hata işleyicisi ekleme**</span><span class="sxs-lookup"><span data-stu-id="19f71-339">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

<span data-ttu-id="19f71-340">**Bir hata işleyicisi (olmadan oluşturulan proxy) ekleme**</span><span class="sxs-lookup"><span data-stu-id="19f71-340">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

<span data-ttu-id="19f71-341">Aşağıdaki örnek, bir yöntem çağrısı bir hata nasıl ele alınacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="19f71-341">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="19f71-342">**Bir yöntem çağırma (ile oluşturulan proxy) bir hata işleme**</span><span class="sxs-lookup"><span data-stu-id="19f71-342">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

<span data-ttu-id="19f71-343">**Bir yöntem çağırma (olmadan oluşturulan proxy) bir hata işleme**</span><span class="sxs-lookup"><span data-stu-id="19f71-343">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="19f71-344">Bir yöntem çağrısı başarısız olursa, `error` olayı yükseltildiğinde de, bu nedenle kodunuzda `error` yöntemin işleyicisi ve `.fail` yöntemi geri çağırma yürütün.</span><span class="sxs-lookup"><span data-stu-id="19f71-344">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="19f71-345">İstemci tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="19f71-345">How to enable client-side logging</span></span>

<span data-ttu-id="19f71-346">Bir bağlantı üzerinde istemci tarafı günlüğe kaydetmeyi etkinleştirmek için ayarlanmış `logging` özelliği çağırmadan önce bağlantı nesnesindeki `start` bağlantı kurmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="19f71-346">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="19f71-347">**(İle oluşturulan proxy) günlük kaydını etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="19f71-347">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

<span data-ttu-id="19f71-348">**(Olmadan oluşturulan proxy) günlüğe yazmayı etkinleştir**</span><span class="sxs-lookup"><span data-stu-id="19f71-348">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="19f71-349">Günlükleri görmek için tarayıcınızın Geliştirici Araçları'nı açın ve konsol sekmesine gidin. Adım adım yönergeler ve ekran görüntüsü bunun nasıl yapılacağını gösteren gösteren bir öğretici için bkz. [ASP.NET Signalr - günlüğü etkinleştir ile sunucu yayını](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span><span class="sxs-lookup"><span data-stu-id="19f71-349">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](../getting-started/tutorial-server-broadcast-with-signalr.md#enable-logging).</span></span>
