---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x Hubs API Kılavuzu - JavaScript istemcisi | Microsoft Docs
author: pfletcher
description: Bu belge, SignalR sürüm 1.1 tarayıcılar ve Windows Store (WinJS) applic gibi JavaScript istemcileri için hub'ları API kullanarak bir giriş sağlar...
ms.author: riande
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 2d50a92cff96be5c5c60105bba6682d38f9666b6
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53288098"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a><span data-ttu-id="fe532-103">SignalR 1.x Hubs API Kılavuzu - JavaScript İstemcisi</span><span class="sxs-lookup"><span data-stu-id="fe532-103">SignalR 1.x Hubs API Guide - JavaScript Client</span></span>
====================
<span data-ttu-id="fe532-104">tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fe532-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="fe532-105">Bu belge, SignalR sürüm 1.1 tarayıcıları ve Windows Store (WinJS) uygulamalar gibi JavaScript istemcileri için hub'ları API kullanmaya giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe532-105">This document provides an introduction to using the Hubs API for SignalR version 1.1 in JavaScript clients, such as browsers and Windows Store (WinJS) applications.</span></span>
> 
> <span data-ttu-id="fe532-106">SignalR hub'ları API, bir sunucuya bağlanan istemcilerin ve istemcilerin sunucuya uzaktan yordam çağrısı (RPC) oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe532-106">The SignalR Hubs API enables you to make remote procedure calls (RPCs) from a server to connected clients and from clients to the server.</span></span> <span data-ttu-id="fe532-107">Sunucu kodu, istemciler tarafından çağrılabilen yöntemleri tanımlamak ve bir istemcide çalışmasına yöntemler çağırır.</span><span class="sxs-lookup"><span data-stu-id="fe532-107">In server code, you define methods that can be called by clients, and you call methods that run on the client.</span></span> <span data-ttu-id="fe532-108">İstemci kodu sunucudan çağıran yöntemleri tanımlamak ve sunucu üzerinde çalışan yöntemleri çağırın.</span><span class="sxs-lookup"><span data-stu-id="fe532-108">In client code, you define methods that can be called from the server, and you call methods that run on the server.</span></span> <span data-ttu-id="fe532-109">SignalR tüm istemci-sunucu tesisat sizin için üstlenir.</span><span class="sxs-lookup"><span data-stu-id="fe532-109">SignalR takes care of all of the client-to-server plumbing for you.</span></span>
> 
> <span data-ttu-id="fe532-110">SignalR kalıcı bağlantı adlı bir alt düzey API'si de sunar.</span><span class="sxs-lookup"><span data-stu-id="fe532-110">SignalR also offers a lower-level API called Persistent Connections.</span></span> <span data-ttu-id="fe532-111">SignalR hub'ları ve kalıcı bağlantılar için giriş veya tam bir SignalR uygulamanın nasıl oluşturulacağını gösteren bir öğretici için bkz: [SignalR çalışmaya başlama -](../getting-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="fe532-111">For an introduction to SignalR, Hubs, and Persistent Connections, or for a tutorial that shows how to build a complete SignalR application, see [SignalR - Getting Started](../getting-started/index.md).</span></span>


## <a name="overview"></a><span data-ttu-id="fe532-112">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="fe532-112">Overview</span></span>

<span data-ttu-id="fe532-113">Bu belgede aşağıdaki bölümler yer alır:</span><span class="sxs-lookup"><span data-stu-id="fe532-113">This document contains the following sections:</span></span>

- [<span data-ttu-id="fe532-114">Oluşturulan proxy ve bunu sizin yerinize yapar</span><span class="sxs-lookup"><span data-stu-id="fe532-114">The generated proxy and what it does for you</span></span>](#genproxy)

    - [<span data-ttu-id="fe532-115">Oluşturulan proxy kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="fe532-115">When to use the generated proxy</span></span>](#cantusegenproxy)
- [<span data-ttu-id="fe532-116">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="fe532-116">Client Setup</span></span>](#clientsetup)

    - [<span data-ttu-id="fe532-117">Dinamik olarak oluşturulan proxy başvuru yapma</span><span class="sxs-lookup"><span data-stu-id="fe532-117">How to reference the dynamically generated proxy</span></span>](#dynamicproxy)
    - [<span data-ttu-id="fe532-118">Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur</span><span class="sxs-lookup"><span data-stu-id="fe532-118">How to create a physical file for the SignalR generated proxy</span></span>](#manualproxy)
- [<span data-ttu-id="fe532-119">Nasıl bir bağlantı kurmak için</span><span class="sxs-lookup"><span data-stu-id="fe532-119">How to establish a connection</span></span>](#establishconnection)

    - [<span data-ttu-id="fe532-120">$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi</span><span class="sxs-lookup"><span data-stu-id="fe532-120">$.connection.hub is the same object that $.hubConnection() creates</span></span>](#connequivalence)
    - [<span data-ttu-id="fe532-121">Zaman uyumsuz yürütme başlangıç yöntemi</span><span class="sxs-lookup"><span data-stu-id="fe532-121">Asynchronous execution of the start method</span></span>](#asyncstart)
- [<span data-ttu-id="fe532-122">Etki alanları arası bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="fe532-122">How to establish a cross-domain connection</span></span>](#crossdomain)
- [<span data-ttu-id="fe532-123">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fe532-123">How to configure the connection</span></span>](#configureconnection)

    - [<span data-ttu-id="fe532-124">Sorgu dizesi parametrelerini belirtme</span><span class="sxs-lookup"><span data-stu-id="fe532-124">How to specify query string parameters</span></span>](#querystring)
    - [<span data-ttu-id="fe532-125">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="fe532-125">How to specify the transport method</span></span>](#transport)
- [<span data-ttu-id="fe532-126">Bir Hub sınıfı için bir ara sunucu alma</span><span class="sxs-lookup"><span data-stu-id="fe532-126">How to get a proxy for a Hub class</span></span>](#getproxy)
- [<span data-ttu-id="fe532-127">Sunucu çağıran istemciye yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="fe532-127">How to define methods on the client that the server can call</span></span>](#callclient)
- [<span data-ttu-id="fe532-128">İstemciden sunucu yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="fe532-128">How to call server methods from the client</span></span>](#callserver)
- [<span data-ttu-id="fe532-129">Bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="fe532-129">How to handle connection lifetime events</span></span>](#connectionlifetime)
- [<span data-ttu-id="fe532-130">Hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="fe532-130">How to handle errors</span></span>](#handleerrors)
- [<span data-ttu-id="fe532-131">İstemci tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="fe532-131">How to enable client-side logging</span></span>](#logging)

<span data-ttu-id="fe532-132">Sunucu veya .NET istemcileri program hakkında daha fazla belge için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="fe532-132">For documentation on how to program the server or .NET clients, see the following resources:</span></span>

- [<span data-ttu-id="fe532-133">SignalR hub API Kılavuzu - sunucu</span><span class="sxs-lookup"><span data-stu-id="fe532-133">SignalR Hubs API Guide - Server</span></span>](../guide-to-the-api/hubs-api-guide-server.md)
- [<span data-ttu-id="fe532-134">SignalR hub API Kılavuzu - .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="fe532-134">SignalR Hubs API Guide - .NET Client</span></span>](../guide-to-the-api/hubs-api-guide-net-client.md)

<span data-ttu-id="fe532-135">API başvuru konularına bağlar API .NET 4.5 sürümü var.</span><span class="sxs-lookup"><span data-stu-id="fe532-135">Links to API Reference topics are to the .NET 4.5 version of the API.</span></span> <span data-ttu-id="fe532-136">.NET 4 kullanıyorsanız, bkz. [API konuları .NET 4 sürümünü](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="fe532-136">If you're using .NET 4, see [the .NET 4 version of the API topics](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).</span></span>

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a><span data-ttu-id="fe532-137">Oluşturulan proxy ve bunu sizin yerinize yapar</span><span class="sxs-lookup"><span data-stu-id="fe532-137">The generated proxy and what it does for you</span></span>

<span data-ttu-id="fe532-138">Bir SignalR hizmeti ile veya olmadan SignalR sizin için oluşturduğu bir ara sunucu ile iletişim kurmak için JavaScript istemci programlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe532-138">You can program a JavaScript client to communicate with a SignalR service with or without a proxy that SignalR generates for you.</span></span> <span data-ttu-id="fe532-139">Proxy sizin için ne yaptığını, sunucuya çağrıları, yazma yöntemleri bağlanın ve sunucu üzerinde yöntemleri çağırmak kod dizimini basitleştiren olur.</span><span class="sxs-lookup"><span data-stu-id="fe532-139">What the proxy does for you is simplify the syntax of the code you use to connect, write methods that the server calls, and call methods on the server.</span></span>

<span data-ttu-id="fe532-140">Sunucu yöntemlerini çağırmak için kod yazdığınızda, oluşturulan proxy, yerel bir işlev yürütülürken ancak gibi görünen sözdizimi kullanmanıza olanak sağlar: yazabileceğiniz `serverMethod(arg1, arg2)` yerine `invoke('serverMethod', arg1, arg2)`.</span><span class="sxs-lookup"><span data-stu-id="fe532-140">When you write code to call server methods, the generated proxy enables you to use syntax that looks as though you were executing a local function: you can write `serverMethod(arg1, arg2)` instead of `invoke('serverMethod', arg1, arg2)`.</span></span> <span data-ttu-id="fe532-141">Bir sunucu yöntem adı yanlış ise oluşturulan proxy söz dizimi anında ve anlaşılır bir istemci tarafı hata de sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe532-141">The generated proxy syntax also enables an immediate and intelligible client-side error if you mistype a server method name.</span></span> <span data-ttu-id="fe532-142">Ve proxy'ler tanımlayan dosyayı el ile oluşturursanız, ayrıca IntelliSense desteği server yöntemlerini çağıran kod yazmak için alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe532-142">And if you manually create the file that defines the proxies, you can also get IntelliSense support for writing code that calls server methods.</span></span>

<span data-ttu-id="fe532-143">Örneğin, sunucu üzerinde aşağıdaki Hub sınıfına olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="fe532-143">For example, suppose you have the following Hub class on the server:</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

<span data-ttu-id="fe532-144">Aşağıdaki kod örnekleri ne JavaScript kodu çağırmak için nasıl göründüğüne Göster `NewContosoChatMessage` sunucu ve çağrılarını alma yöntemini `addContosoChatMessageToPage` sunucudan yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-144">The following code examples show what JavaScript code looks like for invoking the `NewContosoChatMessage` method on the server and receiving invocations of the `addContosoChatMessageToPage` method from the server.</span></span>

<span data-ttu-id="fe532-145">**Oluşturulan proxy ile**</span><span class="sxs-lookup"><span data-stu-id="fe532-145">**With the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

<span data-ttu-id="fe532-146">**Üretilen Ara sunucu olmadan**</span><span class="sxs-lookup"><span data-stu-id="fe532-146">**Without the generated proxy**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a><span data-ttu-id="fe532-147">Oluşturulan proxy kullanma zamanı</span><span class="sxs-lookup"><span data-stu-id="fe532-147">When to use the generated proxy</span></span>

<span data-ttu-id="fe532-148">Sunucu çağıran bir istemci yöntemi için birden çok olay işleyicisi kaydetmek istiyorsanız, oluşturulan proxy kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="fe532-148">If you want to register multiple event handlers for a client method that the server calls, you can't use the generated proxy.</span></span> <span data-ttu-id="fe532-149">Aksi takdirde, oluşturulan proxy kullanmayı seçebilirsiniz veya kodlama tercihinize bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="fe532-149">Otherwise, you can choose to use the generated proxy or not based on your coding preference.</span></span> <span data-ttu-id="fe532-150">Bunu kullanmayı seçerseniz, "signalr/hubs" URL'de başvuru yoksa bir `script` istemci kodunun öğesi.</span><span class="sxs-lookup"><span data-stu-id="fe532-150">If you choose not to use it, you don't have to reference the "signalr/hubs" URL in a `script` element in your client code.</span></span>

<a id="clientsetup"></a>

## <a name="client-setup"></a><span data-ttu-id="fe532-151">İstemci Kurulumu</span><span class="sxs-lookup"><span data-stu-id="fe532-151">Client setup</span></span>

<span data-ttu-id="fe532-152">JavaScript istemcisi, jQuery ve SignalR çekirdek JavaScript dosyası başvuruları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fe532-152">A JavaScript client requires references to jQuery and the SignalR core JavaScript file.</span></span> <span data-ttu-id="fe532-153">JQuery sürümü 1.6.4 veya 1.7.2'ye, 1.8.2 veya 1.9.1 gibi önemli sonraki sürümler olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fe532-153">The jQuery version must be 1.6.4 or major later versions, such as 1.7.2, 1.8.2, or 1.9.1.</span></span> <span data-ttu-id="fe532-154">Oluşturulan proxy kullanmaya karar verirseniz, ayrıca SignalR oluşturulan proxy JavaScript dosyasına bir başvuru gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe532-154">If you decide to use the generated proxy, you also need a reference to the SignalR generated proxy JavaScript file.</span></span> <span data-ttu-id="fe532-155">Aşağıdaki örnek, başvuruları oluşturulan proxy kullanan bir HTML sayfasında nasıl görünebileceğini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="fe532-155">The following example shows what the references might look like in an HTML page that uses the generated proxy.</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

<span data-ttu-id="fe532-156">Bu sırada bu başvuruları eklenmelidir: jQuery ilk, son bundan sonra SignalR çekirdek ve SignalR proxy'leri.</span><span class="sxs-lookup"><span data-stu-id="fe532-156">These references must be included in this order: jQuery first, SignalR core after that, and SignalR proxies last.</span></span>

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a><span data-ttu-id="fe532-157">Dinamik olarak oluşturulan proxy başvuru yapma</span><span class="sxs-lookup"><span data-stu-id="fe532-157">How to reference the dynamically generated proxy</span></span>

<span data-ttu-id="fe532-158">Önceki örnekte oluşturulan SignalR proxy fiziksel bir dosya için dinamik olarak oluşturulan JavaScript kodu başvurudur.</span><span class="sxs-lookup"><span data-stu-id="fe532-158">In the preceding example, the reference to the SignalR generated proxy is to dynamically generated JavaScript code, not to a physical file.</span></span> <span data-ttu-id="fe532-159">SignalR hareket halindeyken proxy için JavaScript kodunu oluşturur ve istemciye yanıt "/ signalr/hubs" URL olarak görev yapar.</span><span class="sxs-lookup"><span data-stu-id="fe532-159">SignalR creates the JavaScript code for the proxy on the fly and serves it to the client in response to the "/signalr/hubs" URL.</span></span> <span data-ttu-id="fe532-160">Sunucuda SignalR bağlantıları için farklı bir temel URL belirttiyseniz, `MapHubs` yöntemi dinamik olarak oluşturulan proxy dosyanın URL'si olduğundan, özel bir URL ile "/ hubs" eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="fe532-160">If you specified a different base URL for SignalR connections on the server in your `MapHubs` method, the URL for the dynamically generated proxy file is your custom URL with "/hubs" appended to it.</span></span>

> [!NOTE]
> <span data-ttu-id="fe532-161">Windows 8 (Windows Store) JavaScript istemciler için dinamik olarak üretilen bir yerine fiziksel proxy dosyası kullanın.</span><span class="sxs-lookup"><span data-stu-id="fe532-161">For Windows 8 (Windows Store) JavaScript clients, use the physical proxy file instead of the dynamically generated one.</span></span> <span data-ttu-id="fe532-162">Daha fazla bilgi için [SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulan proxy](#manualproxy) bu konuda.</span><span class="sxs-lookup"><span data-stu-id="fe532-162">For more information, see [How to create a physical file for the SignalR generated proxy](#manualproxy) later in this topic.</span></span>


<span data-ttu-id="fe532-163">Bir ASP.NET MVC 4 Razor Görünümü'nde, proxy dosya Başvurusu'ndaki uygulama kökü başvurmak için tilde kullanın:</span><span class="sxs-lookup"><span data-stu-id="fe532-163">In an ASP.NET MVC 4 Razor view, use the tilde to refer to the application root in your proxy file reference:</span></span>

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

<span data-ttu-id="fe532-164">MVC 4'te SignalR kullanma hakkında daha fazla bilgi için bkz. [SignalR ve MVC 4 ile çalışmaya başlama](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="fe532-164">For more information about using SignalR in MVC 4, see [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="fe532-165">Bir ASP.NET MVC 3 Razor Görünümü'nde kullanmak `Url.Content` proxy dosya başvuru amacıyla:</span><span class="sxs-lookup"><span data-stu-id="fe532-165">In an ASP.NET MVC 3 Razor view, use `Url.Content` for your proxy file reference:</span></span>

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

<span data-ttu-id="fe532-166">Bir ASP.NET Web formları uygulamasında `ResolveClientUrl` , proxy'leri dosya başvurusu veya (bir tilde işareti ile başlayan) bir uygulama kök göreli bir yol kullanarak ScriptManager aracılığıyla kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="fe532-166">In an ASP.NET Web Forms application, use `ResolveClientUrl` for your proxies file reference or register it via the ScriptManager using an app root relative path (starting with a tilde):</span></span>

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

<span data-ttu-id="fe532-167">Genel bir kural olarak, CSS ya da JavaScript dosyaları için kullanan "/ signalr/hubs" URL'yi belirtmek için aynı yöntemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="fe532-167">As a general rule, use the same method for specifying the "/signalr/hubs" URL that you use for CSS or JavaScript files.</span></span> <span data-ttu-id="fe532-168">Bir tilde kullanmadan bir URL belirtirseniz, bazı senaryolarda, IIS Express kullanarak Visual Studio'da test ancak tam IIS dağıttığınızda bir 404 hatası ile başarısız olur, uygulamanızın düzgün çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="fe532-168">If you specify a URL without using a tilde, in some scenarios your application will work correctly when you test in Visual Studio using IIS Express but will fail with a 404 error when you deploy to full IIS.</span></span> <span data-ttu-id="fe532-169">Daha fazla bilgi için **kök düzeyinde kaynaklara başvurular çözümleniyor** içinde [ASP.NET Web projeleri için Visual Studio'daki Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx) MSDN sitesinden.</span><span class="sxs-lookup"><span data-stu-id="fe532-169">For more information, see **Resolving References to Root-Level Resources** in [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx) on the MSDN site.</span></span>

<span data-ttu-id="fe532-170">Bir web projesi, Visual Studio 2012'de hata ayıklama modunda çalıştırdığınızda ve Internet Explorer tarayıcı olarak kullanıyorsanız, proxy dosyasında görebilirsiniz **Çözüm Gezgini** altında **betik belgelerini**gösterildiği Aşağıdaki çizim.</span><span class="sxs-lookup"><span data-stu-id="fe532-170">When you run a web project in Visual Studio 2012 in debug mode, and if you use Internet Explorer as your browser, you can see the proxy file in **Solution Explorer** under **Script Documents**, as shown in the following illustration.</span></span>

![Çözüm Gezgini'nde bir JavaScript oluşturulan proxy dosyası](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

<span data-ttu-id="fe532-172">Dosyanın içeriğini görmek için çift tıklatın **hubs**.</span><span class="sxs-lookup"><span data-stu-id="fe532-172">To see the contents of the file, double-click **hubs**.</span></span> <span data-ttu-id="fe532-173">Visual Studio 2012 ve Internet Explorer kullanmıyorsanız veya hata ayıklama modunda değilse, dosyanın içeriğini "/ signalR/hubs" URL'sine göz atarak da edinebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe532-173">If you are not using Visual Studio 2012 and Internet Explorer, or if you are not in debug mode, you can also get the contents of the file by browsing to the "/signalR/hubs" URL.</span></span> <span data-ttu-id="fe532-174">Sitenizi en çalışıyorsa Örneğin, `http://localhost:56699`Git `http://localhost:56699/SignalR/hubs` tarayıcınızda.</span><span class="sxs-lookup"><span data-stu-id="fe532-174">For example, if your site is running at `http://localhost:56699`, go to `http://localhost:56699/SignalR/hubs` in your browser.</span></span>

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a><span data-ttu-id="fe532-175">Proxy SignalR için fiziksel bir dosya oluşturmak nasıl oluşturulur</span><span class="sxs-lookup"><span data-stu-id="fe532-175">How to create a physical file for the SignalR generated proxy</span></span>

<span data-ttu-id="fe532-176">Dinamik olarak oluşturulan proxy alternatif, proxy kodu fiziksel bir dosya oluşturun ve bu dosyaya başvuru.</span><span class="sxs-lookup"><span data-stu-id="fe532-176">As an alternative to the dynamically generated proxy, you can create a physical file that has the proxy code and reference that file.</span></span> <span data-ttu-id="fe532-177">Denetim önbelleğe alma veya davranışı'nı paketlemek için bunu veya sunucu yöntemlere yapılan çağrılar Kodladığınız IntelliSense almak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe532-177">You might want to do that for control over caching or bundling behavior, or to get IntelliSense when you are coding calls to server methods.</span></span>

<span data-ttu-id="fe532-178">Proxy dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="fe532-178">To create a proxy file, perform the following steps:</span></span>

1. <span data-ttu-id="fe532-179">Yükleme [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="fe532-179">Install the [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet package.</span></span>
2. <span data-ttu-id="fe532-180">Bir komut istemi açın ve *Araçları* SignalR.exe dosyasını içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="fe532-180">Open a command prompt and browse to the *tools* folder that contains the SignalR.exe file.</span></span> <span data-ttu-id="fe532-181">Araçlar klasöründe şu konumdadır:</span><span class="sxs-lookup"><span data-stu-id="fe532-181">The tools folder is at the following location:</span></span>

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. <span data-ttu-id="fe532-182">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="fe532-182">Enter the following command:</span></span>

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    <span data-ttu-id="fe532-183">Yolu, *.dll* genellikle *bin* proje klasörünüzde klasör.</span><span class="sxs-lookup"><span data-stu-id="fe532-183">The path to your *.dll* is typically the *bin* folder in your project folder.</span></span>

    <span data-ttu-id="fe532-184">Bu komut, adlı bir dosya oluşturur. *server.js* aynı klasörde *signalr.exe*.</span><span class="sxs-lookup"><span data-stu-id="fe532-184">This command creates a file named *server.js* in the same folder as *signalr.exe*.</span></span>
4. <span data-ttu-id="fe532-185">PUT *server.js* projenize uygun bir klasörde dosya, uygulamanız için uygun şekilde yeniden adlandırın ve "signalr/hubs" başvurusu yerine bir başvuru ekleyin.</span><span class="sxs-lookup"><span data-stu-id="fe532-185">Put the *server.js* file in an appropriate folder in your project, rename it as appropriate for your application, and add a reference to it in place of the "signalr/hubs" reference.</span></span>

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a><span data-ttu-id="fe532-186">Nasıl bir bağlantı kurmak için</span><span class="sxs-lookup"><span data-stu-id="fe532-186">How to establish a connection</span></span>

<span data-ttu-id="fe532-187">Bir bağlantı kurabilmesi için önce bir bağlantı nesnesi oluşturun, bir proxy oluşturma ve sunucudan çağrılabilir yöntemleri için olay işleyicilerini kaydedin gerekir.</span><span class="sxs-lookup"><span data-stu-id="fe532-187">Before you can establish a connection, you have to create a connection object, create a proxy, and register event handlers for methods that can be called from the server.</span></span> <span data-ttu-id="fe532-188">Proxy ve olay işleyicileri ayarlandığında çağırarak bağlantı kurmak `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-188">When the proxy and event handlers are set up, establish the connection by calling the `start` method.</span></span>

<span data-ttu-id="fe532-189">Oluşturulan proxy kullanıyorsanız, oluşturulan proxy kodu bunu sizin yerinize yapar kendi kodunuzda bağlantı nesnesi oluşturmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fe532-189">If you are using the generated proxy, you don't have to create the connection object in your own code because the generated proxy code does it for you.</span></span>

<a id="nogenconnection"></a>

<span data-ttu-id="fe532-190">**(Oluşturulan proxy ile) bağlantı kurma**</span><span class="sxs-lookup"><span data-stu-id="fe532-190">**Establish a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

<span data-ttu-id="fe532-191">**Bir bağlantı (olmadan oluşturulan proxy)**</span><span class="sxs-lookup"><span data-stu-id="fe532-191">**Establish a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

<span data-ttu-id="fe532-192">Varsayılan örnek kodu kullanır "/ signalr" SignalR hizmetinize bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="fe532-192">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="fe532-193">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="fe532-193">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>

> [!NOTE]
> <span data-ttu-id="fe532-194">Normalde, olay işleyicileri çağırmadan önce kaydetmeniz `start` bağlantı kurmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-194">Normally you register event handlers before calling the `start` method to establish the connection.</span></span> <span data-ttu-id="fe532-195">Bağlantı kurulduktan sonra bazı olay işleyicileri kaydetmek istediğiniz, bunu yapabilirsiniz, ancak en az bir çağırmadan önce olay handler(s) kaydetmelisiniz `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-195">If you want to register some event handlers after establishing the connection, you can do that, but you must register at least one of your event handler(s) before calling the `start` method.</span></span> <span data-ttu-id="fe532-196">Bunun bir nedeni olduğundan bir uygulamada birçok Hubs olabilir, ancak tetiklemesini istemezsiniz `OnConnected` yalnızca bunlardan birini kullanmak için kullanacaksanız, her hub'da olay.</span><span class="sxs-lookup"><span data-stu-id="fe532-196">One reason for this is that there can be many Hubs in an application, but you wouldn't want to trigger the `OnConnected` event on every Hub if you are only going to use to one of them.</span></span> <span data-ttu-id="fe532-197">Bağlantı kurulduğunda, bir istemci yöntemi bir Hub'ın proxy varlığını ne tetiklemek için SignalR söyler olan `OnConnected` olay.</span><span class="sxs-lookup"><span data-stu-id="fe532-197">When the connection is established, the presence of a client method on a Hub's proxy is what tells SignalR to trigger the `OnConnected` event.</span></span> <span data-ttu-id="fe532-198">Tüm olay işleyicileri çağırmadan önce kaydetmezseniz `start` yöntemi oluşturabileceksiniz Hub ancak Hub'ın üzerinde yöntem çağırmak `OnConnected` yöntemi olmaz çağrılabilir ve sunucudan hiçbir istemci yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fe532-198">If you don't register any event handlers before calling the `start` method, you will be able to invoke methods on the Hub, but the Hub's `OnConnected` method won't be called and no client methods will be invoked from the server.</span></span>


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a><span data-ttu-id="fe532-199">$. connection.hub aynıdır, $.hubConnection() oluşturur nesnesi</span><span class="sxs-lookup"><span data-stu-id="fe532-199">$.connection.hub is the same object that $.hubConnection() creates</span></span>

<span data-ttu-id="fe532-200">Oluşturulan proxy kullandığınızda örneklerden gördüğünüz gibi `$.connection.hub` bağlantı nesneye başvurur.</span><span class="sxs-lookup"><span data-stu-id="fe532-200">As you can see from the examples, when you use the generated proxy, `$.connection.hub` refers to the connection object.</span></span> <span data-ttu-id="fe532-201">Çağırarak alma aynı nesne budur `$.hubConnection()` zaman kullanmadığınız oluşturulan proxy.</span><span class="sxs-lookup"><span data-stu-id="fe532-201">This is the same object that you get by calling `$.hubConnection()` when you aren't using the generated proxy.</span></span> <span data-ttu-id="fe532-202">Oluşturulan proxy kodu, aşağıdaki deyimi yürüterek bağlantıyı sizin için oluşturur:</span><span class="sxs-lookup"><span data-stu-id="fe532-202">The generated proxy code creates the connection for you by executing the following statement:</span></span>

![Oluşturulan proxy dosyasında bağlantı oluşturma](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

<span data-ttu-id="fe532-204">Oluşturulan proxy kullanırken ile her şeyi yapabilirsiniz `$.connection.hub` oluşturulan proxy kullanmadığınızda bir bağlantı nesnesi ile yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe532-204">When you're using the generated proxy, you can do anything with `$.connection.hub` that you can do with a connection object when you're not using the generated proxy.</span></span>

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a><span data-ttu-id="fe532-205">Zaman uyumsuz yürütme başlangıç yöntemi</span><span class="sxs-lookup"><span data-stu-id="fe532-205">Asynchronous execution of the start method</span></span>

<span data-ttu-id="fe532-206">`start` Yöntemi zaman uyumsuz olarak yürütür.</span><span class="sxs-lookup"><span data-stu-id="fe532-206">The `start` method executes asynchronously.</span></span> <span data-ttu-id="fe532-207">Döndürür bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/), yani geri çağırma işlevleri gibi yöntemleri çağırarak ekleyebilirsiniz `pipe`, `done`, ve `fail`.</span><span class="sxs-lookup"><span data-stu-id="fe532-207">It returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/), which means that you can add callback functions by calling methods such as `pipe`, `done`, and `fail`.</span></span> <span data-ttu-id="fe532-208">Bağlantı kurulduktan sonra yürütmek istediğiniz kodu varsa, sunucu yönteme bir çağrı gibi kod içindeki geri arama işlevi put veya bir geri çağırma işlevini çağırın.</span><span class="sxs-lookup"><span data-stu-id="fe532-208">If you have code that you want to execute after the connection is established, such as a call to a server method, put that code in a callback function or call it from a callback function.</span></span> <span data-ttu-id="fe532-209">`.done` Bağlantı kurulduktan sonra herhangi kod sonra sahip olduğunuz geri çağırma yöntemi yürütüldüğünde, `OnConnected` sunucuda olay işleyicisi yöntemi tamamlandıktan yürütülüyor.</span><span class="sxs-lookup"><span data-stu-id="fe532-209">The `.done` callback method is executed after the connection has been established, and after any code that you have in your `OnConnected` event handler method on the server finishes executing.</span></span>

<span data-ttu-id="fe532-210">Olarak kodun sonraki satırında, sonra "Şimdi bağlı" deyimi önceki örnekten koyarsanız `start` yöntem çağrısının (değil, bir `.done` geri çağırma), `console.log` satır çalıştırılacağı bağlantı kurulmadan önce aşağıda gösterildiği gibi Örnek:</span><span class="sxs-lookup"><span data-stu-id="fe532-210">If you put the "Now connected" statement from the preceding example as the next line of code after the `start` method call (not in a `.done` callback), the `console.log` line will execute before the connection is established, as shown in the following example:</span></span>

![Bağlantı kurulduktan sonra çalışan kod yazmak için yanlış bir yol](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a><span data-ttu-id="fe532-212">Etki alanları arası bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="fe532-212">How to establish a cross-domain connection</span></span>

<span data-ttu-id="fe532-213">Genellikle bir sayfadan tarayıcı yüklerse `http://contoso.com`, aynı etki alanında altındadır SignalR bağlantı `http://contoso.com/signalr`.</span><span class="sxs-lookup"><span data-stu-id="fe532-213">Typically if the browser loads a page from `http://contoso.com`, the SignalR connection is in the same domain, at `http://contoso.com/signalr`.</span></span> <span data-ttu-id="fe532-214">Varsa sayfasından `http://contoso.com` için bir bağlantı kurar `http://fabrikam.com/signalr`, diğer bir deyişle etki alanları arası bağlantı.</span><span class="sxs-lookup"><span data-stu-id="fe532-214">If the page from `http://contoso.com` makes a connection to `http://fabrikam.com/signalr`, that is a cross-domain connection.</span></span> <span data-ttu-id="fe532-215">Güvenlik nedenleriyle, etki alanları arası bağlantıları varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="fe532-215">For security reasons, cross-domain connections are disabled by default.</span></span> <span data-ttu-id="fe532-216">Etki alanları arası bağlantı kurmak için etki alanları arası bağlantıları sunucu üzerinde etkin olduğundan emin olun ve bağlantı nesnesi oluşturduğunuzda, bağlantı URL'si belirtin.</span><span class="sxs-lookup"><span data-stu-id="fe532-216">To establish a cross-domain connection, make sure that cross-domain connections are enabled on the server, and specify the connection URL when you create the connection object.</span></span> <span data-ttu-id="fe532-217">SignalR kullanacağı en uygun teknolojiyi etki alanları arası bağlantılar için gibi [JSONP](http://en.wikipedia.org/wiki/JSONP) veya [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span><span class="sxs-lookup"><span data-stu-id="fe532-217">SignalR will use the appropriate technology for cross-domain connections, such as [JSONP](http://en.wikipedia.org/wiki/JSONP) or [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).</span></span>

<span data-ttu-id="fe532-218">Sunucuda çağırdığınızda, bu seçeneği belirleyerek etki alanları arası bağlantıları etkinleştirme `MapHubs` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-218">On the server, enable cross-domain connections by selecting that option when you call the `MapHubs` method.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

<span data-ttu-id="fe532-219">İstemcide (olmadan oluşturulan proxy) bağlantı nesnesi oluşturduğunuzda ya da (ile oluşturulan proxy) başlatma yöntemi çağırmadan önce URL'yi belirtin.</span><span class="sxs-lookup"><span data-stu-id="fe532-219">On the client, specify the URL when you create the connection object (without the generated proxy) or before you call the start method (with the generated proxy).</span></span>

<span data-ttu-id="fe532-220">**(İle oluşturulan proxy) etki alanları arası bağlantı belirten istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-220">**Client code that specifies a cross-domain connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

<span data-ttu-id="fe532-221">**Etki alanları arası bağlantı (olmadan oluşturulan proxy) belirten bir istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-221">**Client code that specifies a cross-domain connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

<span data-ttu-id="fe532-222">Kullanırken `$.hubConnection` oluşturucusu yok içerecek şekilde `signalr` URL'sindeki otomatik olarak eklendiğinden (siz belirtmediğiniz sürece `useDefaultUrl` olarak `false`).</span><span class="sxs-lookup"><span data-stu-id="fe532-222">When you use the `$.hubConnection` constructor, you don't have to include `signalr` in the URL because it is added automatically (unless you specify `useDefaultUrl` as `false`).</span></span>

<span data-ttu-id="fe532-223">Farklı uç noktalar birden fazla bağlantı oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe532-223">You can create multiple connections to different endpoints.</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - <span data-ttu-id="fe532-224">Ayarlamamanız `jQuery.support.cors` kodunuzda true.</span><span class="sxs-lookup"><span data-stu-id="fe532-224">Don't set `jQuery.support.cors` to true in your code.</span></span>
> 
>     ![JQuery.support.cors true olarak ayarlamanız gerekmez](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     <span data-ttu-id="fe532-226">SignalR CORS ve JSONP kullanımı işler.</span><span class="sxs-lookup"><span data-stu-id="fe532-226">SignalR handles the use of JSONP or CORS.</span></span> <span data-ttu-id="fe532-227">Ayar `jQuery.support.cors` doğru devre dışı bırakır için JSONP tarayıcıyı destekleyen CORS varsaymak SignalR neden olduğundan.</span><span class="sxs-lookup"><span data-stu-id="fe532-227">Setting `jQuery.support.cors` to true disables JSONP because it causes SignalR to assume the browser supports CORS.</span></span>
> - <span data-ttu-id="fe532-228">Localhost URL bağlanırken sunucunun etki alanları arası bağlantılarda etkinleştirmediyseniz bile uygulama yerel olarak IE 10 ile çalışması için Internet Explorer 10, etki alanları arası bağlantı, göz önünde bulundurun olmaz.</span><span class="sxs-lookup"><span data-stu-id="fe532-228">When you're connecting to a localhost URL, Internet Explorer 10 won't consider it a cross-domain connection, so the application will work locally with IE 10 even if you haven't enabled cross-domain connections on the server.</span></span>
> - <span data-ttu-id="fe532-229">Etki alanları arası bağlantıları Internet Explorer 9 ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span><span class="sxs-lookup"><span data-stu-id="fe532-229">For information about using cross-domain connections with Internet Explorer 9, see [this StackOverflow thread](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).</span></span>
> - <span data-ttu-id="fe532-230">Etki alanları arası bağlantıları Chrome ile kullanma hakkında daha fazla bilgi için bkz: [bu StackOverflow iş parçacığı](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span><span class="sxs-lookup"><span data-stu-id="fe532-230">For information about using cross-domain connections with Chrome, see [this StackOverflow thread](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).</span></span>
> - <span data-ttu-id="fe532-231">Varsayılan örnek kodu kullanır "/ signalr" SignalR hizmetinize bağlanmak için URL.</span><span class="sxs-lookup"><span data-stu-id="fe532-231">The sample code uses the default "/signalr" URL to connect to your SignalR service.</span></span> <span data-ttu-id="fe532-232">Farklı bir temel URL'si belirtme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span><span class="sxs-lookup"><span data-stu-id="fe532-232">For information about how to specify a different base URL, see [ASP.NET SignalR Hubs API Guide - Server - The /signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).</span></span>


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a><span data-ttu-id="fe532-233">Bağlantı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="fe532-233">How to configure the connection</span></span>

<span data-ttu-id="fe532-234">Bir bağlantı kurmadan önce sorgu dizesi parametreleri belirtin veya taşıma yöntemini belirtin.</span><span class="sxs-lookup"><span data-stu-id="fe532-234">Before you establish a connection, you can specify query string parameters or specify the transport method.</span></span>

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a><span data-ttu-id="fe532-235">Sorgu dizesi parametrelerini belirtme</span><span class="sxs-lookup"><span data-stu-id="fe532-235">How to specify query string parameters</span></span>

<span data-ttu-id="fe532-236">İstemci bağlandığında sunucuya veri göndermek istiyorsanız, bağlantı nesnesi için sorgu dizesi parametreleri ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe532-236">If you want to send data to the server when the client connects, you can add query string parameters to the connection object.</span></span> <span data-ttu-id="fe532-237">Aşağıdaki örnekler bir sorgu dizesi parametresi istemci kodda ayarlama işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="fe532-237">The following examples show how to set a query string parameter in client code.</span></span>

<span data-ttu-id="fe532-238">**(İle oluşturulan proxy) başlatma yöntemi çağrılmadan önce bir sorgu dizesi değerini ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="fe532-238">**Set a query string value before calling the start method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

<span data-ttu-id="fe532-239">**(Olmadan oluşturulan proxy) başlatma yöntemi çağrılmadan önce bir sorgu dizesi değerini ayarlayın**</span><span class="sxs-lookup"><span data-stu-id="fe532-239">**Set a query string value before calling the start method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

<span data-ttu-id="fe532-240">Aşağıdaki örnekte, sunucu kodu bir sorgu dizesi parametresi okunacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fe532-240">The following example shows how to read a query string parameter in server code.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a><span data-ttu-id="fe532-241">Taşıma yöntemini belirleme</span><span class="sxs-lookup"><span data-stu-id="fe532-241">How to specify the transport method</span></span>

<span data-ttu-id="fe532-242">Bağlama işleminin bir parçası, SignalR istemci ile sunucu hem sunucu hem de istemci tarafından desteklenen en iyi aktarım belirlemek için normalde görüşür.</span><span class="sxs-lookup"><span data-stu-id="fe532-242">As part of the process of connecting, a SignalR client normally negotiates with the server to determine the best transport that is supported by both server and client.</span></span> <span data-ttu-id="fe532-243">Kullanmak istediğiniz hangi aktarım zaten biliyorsanız, taşıma yöntemini çağırdığınızda belirterek bu anlaşma işlemi atlayabilirsiniz `start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-243">If you already know which transport you want to use, you can bypass this negotiation process by specifying the transport method when you call the `start` method.</span></span>

<span data-ttu-id="fe532-244">**Belirtir (ile oluşturulan proxy) aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-244">**Client code that specifies the transport method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

<span data-ttu-id="fe532-245">**Belirtir (olmadan oluşturulan proxy) aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-245">**Client code that specifies the transport method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

<span data-ttu-id="fe532-246">Alternatif olarak, bunları denemek için SignalR istediğiniz sırayla birden fazla taşıma yöntemleri belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fe532-246">As an alternative, you can specify multiple transport methods in the order in which you want SignalR to try them:</span></span>

<span data-ttu-id="fe532-247">**İstemci kodu, bir özel taşıma geri dönüş şeması (ile oluşturulan proxy) belirtir**</span><span class="sxs-lookup"><span data-stu-id="fe532-247">**Client code that specifies a custom transport fallback scheme (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

<span data-ttu-id="fe532-248">**İstemci kodu, bir özel taşıma geri dönüş şeması (olmadan oluşturulan proxy) belirtir**</span><span class="sxs-lookup"><span data-stu-id="fe532-248">**Client code that specifies a custom transport fallback scheme (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

<span data-ttu-id="fe532-249">Taşıma yöntemi belirtmek için aşağıdaki değerleri kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fe532-249">You can use the following values for specifying the transport method:</span></span>

- <span data-ttu-id="fe532-250">"webSockets"</span><span class="sxs-lookup"><span data-stu-id="fe532-250">"webSockets"</span></span>
- <span data-ttu-id="fe532-251">"foreverFrame"</span><span class="sxs-lookup"><span data-stu-id="fe532-251">"foreverFrame"</span></span>
- <span data-ttu-id="fe532-252">"serverSentEvents"</span><span class="sxs-lookup"><span data-stu-id="fe532-252">"serverSentEvents"</span></span>
- <span data-ttu-id="fe532-253">"longPolling"</span><span class="sxs-lookup"><span data-stu-id="fe532-253">"longPolling"</span></span>

<span data-ttu-id="fe532-254">Aşağıdaki örnekler, hangi aktarım yöntemi bir bağlantı tarafından kullanıldığını nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="fe532-254">The following examples show how to find out which transport method is being used by a connection.</span></span>

<span data-ttu-id="fe532-255">**Görüntüler (ile oluşturulan proxy) bir bağlantı tarafından kullanılan aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-255">**Client code that displays the transport method used by a connection (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

<span data-ttu-id="fe532-256">**Görüntüler (olmadan oluşturulan proxy) bir bağlantı tarafından kullanılan aktarım yöntemi istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-256">**Client code that displays the transport method used by a connection (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

<span data-ttu-id="fe532-257">Sunucu kodu aktarım yöntemi denetleme hakkında daha fazla bilgi için bkz: [ASP.NET SignalR Hubs API Kılavuzu - sunucu - bağlam özelliği istemci hakkında bilgi almak nasıl](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span><span class="sxs-lookup"><span data-stu-id="fe532-257">For information about how to check the transport method in server code, see [ASP.NET SignalR Hubs API Guide - Server - How to get information about the client from the Context property](../guide-to-the-api/hubs-api-guide-server.md#contextproperty).</span></span> <span data-ttu-id="fe532-258">Aktarım ve geri dönüşler hakkında daha fazla bilgi için bkz: [SignalR - aktarım ve geri dönüşler giriş](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="fe532-258">For more information about transports and fallbacks, see [Introduction to SignalR - Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a><span data-ttu-id="fe532-259">Bir Hub sınıfı için bir ara sunucu alma</span><span class="sxs-lookup"><span data-stu-id="fe532-259">How to get a proxy for a Hub class</span></span>

<span data-ttu-id="fe532-260">Oluşturduğunuz her bir bağlantı nesnesi bir veya daha fazla Hub sınıfları içeren bir SignalR service bağlantısı ile ilgili bilgileri yalıtır.</span><span class="sxs-lookup"><span data-stu-id="fe532-260">Each connection object that you create encapsulates information about a connection to a SignalR service that contains one or more Hub classes.</span></span> <span data-ttu-id="fe532-261">Bir Hub sınıfı ile iletişim kurmak için bir proxy nesnesi (oluşturulan proxy değil kullanıyorsanız), sizin oluşturduğunuz veya sizin için oluşturulduğu kullanın.</span><span class="sxs-lookup"><span data-stu-id="fe532-261">To communicate with a Hub class, you use a proxy object which you create yourself (if you're not using the generated proxy) or which is generated for you.</span></span>

<span data-ttu-id="fe532-262">İstemcide Ara sunucu adını Hub sınıf adı ortası büyük küçük harfleri bir sürümü var.</span><span class="sxs-lookup"><span data-stu-id="fe532-262">On the client the proxy name is a camel-cased version of the Hub class name.</span></span> <span data-ttu-id="fe532-263">Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.</span><span class="sxs-lookup"><span data-stu-id="fe532-263">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="fe532-264">**Sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="fe532-264">**Hub class on server**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

<span data-ttu-id="fe532-265">**Hub için oluşturulan istemci proxy'si bir başvuru alma**</span><span class="sxs-lookup"><span data-stu-id="fe532-265">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

<span data-ttu-id="fe532-266">**Hub sınıfı (olmadan oluşturulan proxy) için istemci proxy oluşturma**</span><span class="sxs-lookup"><span data-stu-id="fe532-266">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

<span data-ttu-id="fe532-267">Hub sınıfınıza tasarlamanız, bir `HubName` özniteliği, servis talebi değiştirmeden tam adını kullanın.</span><span class="sxs-lookup"><span data-stu-id="fe532-267">If you decorate your Hub class with a `HubName` attribute, use the exact name without changing case.</span></span>

<span data-ttu-id="fe532-268">**HubName özniteliğine sahip sunucudaki hub sınıfı**</span><span class="sxs-lookup"><span data-stu-id="fe532-268">**Hub class on server with HubName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<span data-ttu-id="fe532-269">**Hub için oluşturulan istemci proxy'si bir başvuru alma**</span><span class="sxs-lookup"><span data-stu-id="fe532-269">**Get a reference to the generated client proxy for the Hub**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

<span data-ttu-id="fe532-270">**Hub sınıfı (olmadan oluşturulan proxy) için istemci proxy oluşturma**</span><span class="sxs-lookup"><span data-stu-id="fe532-270">**Create client proxy for the Hub class (without generated proxy)**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a><span data-ttu-id="fe532-271">Sunucu çağıran istemciye yöntemleri tanımlama</span><span class="sxs-lookup"><span data-stu-id="fe532-271">How to define methods on the client that the server can call</span></span>

<span data-ttu-id="fe532-272">Bir Hub'ından sunucu çağıran bir yöntemi tanımlamak için bir olay işleyicisi Hub proxy için kullanarak ekleme `client` oluşturulan proxy veya arama özelliğini `on` oluşturulan proxy kullanmıyorsanız yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-272">To define a method that the server can call from a Hub, add an event handler to the Hub proxy by using the `client` property of the generated proxy, or call the `on` method if you aren't using the generated proxy.</span></span> <span data-ttu-id="fe532-273">Parametreleri karmaşık nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="fe532-273">The parameters can be complex objects.</span></span>

<span data-ttu-id="fe532-274">Çağırmadan önce olay işleyicisi ekleme `start` bağlantı kurmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-274">Add the event handler before you call the `start` method to establish the connection.</span></span> <span data-ttu-id="fe532-275">(Çağırdıktan sonra olay işleyicileri eklemek istiyorsanız `start` yöntemi,'ündeki nota bakın [bağlantı kurmak nasıl](#establishconnection) daha önce açıklanan belge ve oluşturulan proxy kullanmadan bir yöntemi tanımlamak için sözdizimini kullanın.)</span><span class="sxs-lookup"><span data-stu-id="fe532-275">(If you want to add event handlers after calling the `start` method, see the note in [How to establish a connection](#establishconnection) earlier in this document, and use the syntax shown for defining a method without using the generated proxy.)</span></span>

<span data-ttu-id="fe532-276">Yöntem adı ile eşleşen büyük küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="fe532-276">Method name matching is case-insensitive.</span></span> <span data-ttu-id="fe532-277">Örneğin, `Clients.All.addContosoChatMessageToPage` sunucuda yürütülür `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, veya `addcontosochatmessagetopage` istemci üzerinde.</span><span class="sxs-lookup"><span data-stu-id="fe532-277">For example, `Clients.All.addContosoChatMessageToPage` on the server will execute `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`, or `addcontosochatmessagetopage` on the client.</span></span>

<span data-ttu-id="fe532-278">**(İle oluşturulan proxy) istemcide yöntemi tanımlayın.**</span><span class="sxs-lookup"><span data-stu-id="fe532-278">**Define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

<span data-ttu-id="fe532-279">**İstemci (ile oluşturulan proxy) yöntemi tanımlamak için alternatif bir yolu**</span><span class="sxs-lookup"><span data-stu-id="fe532-279">**Alternate way to define method on client (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

<span data-ttu-id="fe532-280">**İstemci (oluşturulan proxy olmadan veya başlangıç yöntemi çağrıldıktan sonra eklerken) yöntemi tanımlayın.**</span><span class="sxs-lookup"><span data-stu-id="fe532-280">**Define method on client (without the generated proxy, or when adding after calling the start method)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

<span data-ttu-id="fe532-281">**İstemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-281">**Server code that calls the client method**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

<span data-ttu-id="fe532-282">Aşağıdaki örnekler, karmaşık bir nesne bir yöntem parametresi olarak içerir.</span><span class="sxs-lookup"><span data-stu-id="fe532-282">The following examples include a complex object as a method parameter.</span></span>

<span data-ttu-id="fe532-283">**Karmaşık bir nesnenin (ile oluşturulan proxy) alan istemci yöntemi tanımlayın.**</span><span class="sxs-lookup"><span data-stu-id="fe532-283">**Define method on client that takes a complex object (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

<span data-ttu-id="fe532-284">**Karmaşık bir nesnenin (olmadan oluşturulan proxy) alan istemci yöntemi tanımlayın.**</span><span class="sxs-lookup"><span data-stu-id="fe532-284">**Define method on client that takes a complex object (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

<span data-ttu-id="fe532-285">**Karmaşık bir nesne tanımlayan bir sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-285">**Server code that defines the complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

<span data-ttu-id="fe532-286">**Karmaşık bir nesne kullanarak istemci yöntemini çağıran sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-286">**Server code that calls the client method using a complex object**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a><span data-ttu-id="fe532-287">İstemciden sunucu yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="fe532-287">How to call server methods from the client</span></span>

<span data-ttu-id="fe532-288">İstemciden gelen sunucu yöntemini çağırmak için kullanın `server` oluşturulan proxy özelliğini veya `invoke` oluşturulan proxy kullanmıyorsanız Hub proxy yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-288">To call a server method from the client, use the `server` property of the generated proxy or the `invoke` method on the Hub proxy if you aren't using the generated proxy.</span></span> <span data-ttu-id="fe532-289">Dönüş değeri veya parametreleri karmaşık nesneler olabilir.</span><span class="sxs-lookup"><span data-stu-id="fe532-289">The return value or parameters can be complex objects.</span></span>

<span data-ttu-id="fe532-290">Yöntem adı ortası büyük sürümünde hub'da geçirin.</span><span class="sxs-lookup"><span data-stu-id="fe532-290">Pass in a camel-case version of the method name on the Hub.</span></span> <span data-ttu-id="fe532-291">Böylece JavaScript kodu JavaScript kurallarına uymak SignalR bu değişikliği otomatik olarak yapar.</span><span class="sxs-lookup"><span data-stu-id="fe532-291">SignalR automatically makes this change so that JavaScript code can conform to JavaScript conventions.</span></span>

<span data-ttu-id="fe532-292">Aşağıdaki örnekler, dönüş değeri olmayan bir sunucu yöntemin nasıl çağrılacağını ve bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fe532-292">The following examples show how to call a server method that doesn't have a return value and how to call a server method that does have a return value.</span></span>

<span data-ttu-id="fe532-293">**Sunucu yöntemiyle HubMethodName öznitelik yok**</span><span class="sxs-lookup"><span data-stu-id="fe532-293">**Server method with no HubMethodName attribute**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

<span data-ttu-id="fe532-294">**Karmaşık bir nesne tanımlayan bir sunucu kodu bir parametre geçirildi.**</span><span class="sxs-lookup"><span data-stu-id="fe532-294">**Server code that defines the complex object passed in a parameter**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

<span data-ttu-id="fe532-295">**Çağıran Metoda (ile oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-295">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

<span data-ttu-id="fe532-296">**Çağıran Metoda (olmadan oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-296">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

<span data-ttu-id="fe532-297">Hub yönteminin ile donatılmış varsa bir `HubMethodName` özniteliği, servis talebi değiştirmeden bu adı kullanın.</span><span class="sxs-lookup"><span data-stu-id="fe532-297">If you decorated the Hub method with a `HubMethodName` attribute, use that name without changing case.</span></span>

<span data-ttu-id="fe532-298">**Sunucu yöntemi** HubMethodName özniteliğine sahip</span><span class="sxs-lookup"><span data-stu-id="fe532-298">**Server method** with a HubMethodName attribute</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

<span data-ttu-id="fe532-299">**Çağıran Metoda (ile oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-299">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

<span data-ttu-id="fe532-300">**Çağıran Metoda (olmadan oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-300">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

<span data-ttu-id="fe532-301">Önceki örneklerde, dönüş değeri olmayan bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fe532-301">The preceding examples show how to call a server method that has no return value.</span></span> <span data-ttu-id="fe532-302">Aşağıdaki örnekler bir dönüş değerine sahip bir sunucu yöntemin nasıl çağrılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fe532-302">The following examples show how to call a server method that has a return value.</span></span>

<span data-ttu-id="fe532-303">**Bir dönüş değerine sahip bir yöntem için sunucu kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-303">**Server code for a method that has a return value**</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

<span data-ttu-id="fe532-304">**Stok sınıfı için kullanılan** dönüş değeri</span><span class="sxs-lookup"><span data-stu-id="fe532-304">**The Stock class used for the** return value</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

<span data-ttu-id="fe532-305">**Çağıran Metoda (ile oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-305">**Client code that invokes the server method (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

<span data-ttu-id="fe532-306">**Çağıran Metoda (olmadan oluşturulan proxy) istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="fe532-306">**Client code that invokes the server method (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a><span data-ttu-id="fe532-307">Bağlantı ömrü olaylarını işlemek nasıl</span><span class="sxs-lookup"><span data-stu-id="fe532-307">How to handle connection lifetime events</span></span>

<span data-ttu-id="fe532-308">SignalR işleyebilirsiniz ömür olayları aşağıdaki bağlantı sağlar:</span><span class="sxs-lookup"><span data-stu-id="fe532-308">SignalR provides the following connection lifetime events that you can handle:</span></span>

- <span data-ttu-id="fe532-309">`starting`: Herhangi bir veri bağlantısı üzerinden gönderilmeden önce oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fe532-309">`starting`: Raised before any data is sent over the connection.</span></span>
- <span data-ttu-id="fe532-310">`received`: Herhangi bir veri bağlantısı alındığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fe532-310">`received`: Raised when any data is received on the connection.</span></span> <span data-ttu-id="fe532-311">Alınan veriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe532-311">Provides the received data.</span></span>
- <span data-ttu-id="fe532-312">`connectionSlow`: İstemci yavaş veya sık bırakma bağlantı tespit ettiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fe532-312">`connectionSlow`: Raised when the client detects a slow or frequently dropping connection.</span></span>
- <span data-ttu-id="fe532-313">`reconnecting`: Temel alınan aktarımda yeniden bağlanmayı başladığında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fe532-313">`reconnecting`: Raised when the underlying transport begins reconnecting.</span></span>
- <span data-ttu-id="fe532-314">`reconnected`: Temel alınan aktarımda bağlandığınızda oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fe532-314">`reconnected`: Raised when the underlying transport has reconnected.</span></span>
- <span data-ttu-id="fe532-315">`stateChanged`: Bağlantı durumu değiştiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fe532-315">`stateChanged`: Raised when the connection state changes.</span></span> <span data-ttu-id="fe532-316">Eski durum ve yeni bir durum (bağlanma, bağlı, yeniden bağlanılıyor veya bağlantısı kesilmiş) sağlar.</span><span class="sxs-lookup"><span data-stu-id="fe532-316">Provides the old state and the new state (Connecting, Connected, Reconnecting, or Disconnected).</span></span>
- <span data-ttu-id="fe532-317">`disconnected`: Bağlantı kesildiğinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fe532-317">`disconnected`: Raised when the connection has disconnected.</span></span>

<span data-ttu-id="fe532-318">Örneğin, belirgin gecikmeler neden olabilecek bağlantı sorunları olduğunda uyarı iletileri görüntülemek istiyorsanız, tanıtıcı `connectionSlow` olay.</span><span class="sxs-lookup"><span data-stu-id="fe532-318">For example, if you want to display warning messages when there are connection problems that might cause noticeable delays, handle the `connectionSlow` event.</span></span>

<span data-ttu-id="fe532-319">**(İle oluşturulan proxy) connectionSlow olayını işle**</span><span class="sxs-lookup"><span data-stu-id="fe532-319">**Handle the connectionSlow event (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

<span data-ttu-id="fe532-320">**(Olmadan oluşturulan proxy) connectionSlow olayını işle**</span><span class="sxs-lookup"><span data-stu-id="fe532-320">**Handle the connectionSlow event (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

<span data-ttu-id="fe532-321">Daha fazla bilgi için [anlama ve signalr'da bağlantı ömrü olaylarını işleme](index.md).</span><span class="sxs-lookup"><span data-stu-id="fe532-321">For more information, see [Understanding and Handling Connection Lifetime Events in SignalR](index.md).</span></span>

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a><span data-ttu-id="fe532-322">Hatalarını işleme</span><span class="sxs-lookup"><span data-stu-id="fe532-322">How to handle errors</span></span>

<span data-ttu-id="fe532-323">SignalR JavaScript istemci sağlayan bir `error` olayı için bir işleyici ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe532-323">The SignalR JavaScript client provides an `error` event that you can add a handler for.</span></span> <span data-ttu-id="fe532-324">Fail yöntemi, bir sunucu yöntem çağrısından kaynaklanan hatalar için bir işleyici eklemek için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fe532-324">You can also use the fail method to add a handler for errors that result from a server method invocation.</span></span>

<span data-ttu-id="fe532-325">Ayrıntılı hata iletileri sunucuda açıkça etkinleştirmezseniz, bir hatanın ardından SignalR döndüren özel durum nesnesi hata ile ilgili en düşük miktarda bilgiyi içerir.</span><span class="sxs-lookup"><span data-stu-id="fe532-325">If you don't explicitly enable detailed error messages on the server, the exception object that SignalR returns after an error contains minimal information about the error.</span></span> <span data-ttu-id="fe532-326">Örneğin, bir çağrı `newContosoChatMessage` başarısız, hata iletisi hata nesnesi içeren "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" güvenlik nedeniyle, ayrıntılı hata iletileri için etkinleştirmek istiyorsanız ancak üretimde istemciler için ayrıntılı hata iletileri önerilmez gönderme sorun giderme amacıyla sunucu üzerinde aşağıdaki kodu kullanın.</span><span class="sxs-lookup"><span data-stu-id="fe532-326">For example, if a call to `newContosoChatMessage` fails, the error message in the error object contains "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" Sending detailed error messages to clients in production is not recommended for security reasons, but if you want to enable detailed error messages for troubleshooting purposes, use the following code on the server.</span></span>

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

<span data-ttu-id="fe532-327">Aşağıdaki örnek, bir hata olayı işleyicisi eklemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fe532-327">The following example shows how to add a handler for the error event.</span></span>

<span data-ttu-id="fe532-328">**(İle oluşturulan proxy) bir hata işleyicisi ekleme**</span><span class="sxs-lookup"><span data-stu-id="fe532-328">**Add an error handler (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

<span data-ttu-id="fe532-329">**Bir hata işleyicisi (olmadan oluşturulan proxy) ekleme**</span><span class="sxs-lookup"><span data-stu-id="fe532-329">**Add an error handler (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

<span data-ttu-id="fe532-330">Aşağıdaki örnek, bir yöntem çağrısı bir hata nasıl ele alınacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fe532-330">The following example shows how to handle an error from a method invocation.</span></span>

<span data-ttu-id="fe532-331">**Bir yöntem çağırma (ile oluşturulan proxy) bir hata işleme**</span><span class="sxs-lookup"><span data-stu-id="fe532-331">**Handle an error from a method invocation (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

<span data-ttu-id="fe532-332">**Bir yöntem çağırma (olmadan oluşturulan proxy) bir hata işleme**</span><span class="sxs-lookup"><span data-stu-id="fe532-332">**Handle an error from a method invocation (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

<span data-ttu-id="fe532-333">Bir yöntem çağrısı başarısız olursa, `error` olayı yükseltildiğinde de, bu nedenle kodunuzda `error` yöntemin işleyicisi ve `.fail` yöntemi geri çağırma yürütün.</span><span class="sxs-lookup"><span data-stu-id="fe532-333">If a method invocation fails, the `error` event is also raised, so your code in the `error` method handler and in the `.fail` method callback would execute.</span></span>

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a><span data-ttu-id="fe532-334">İstemci tarafı günlük kaydını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="fe532-334">How to enable client-side logging</span></span>

<span data-ttu-id="fe532-335">Bir bağlantı üzerinde istemci tarafı günlüğe kaydetmeyi etkinleştirmek için ayarlanmış `logging` özelliği çağırmadan önce bağlantı nesnesindeki `start` bağlantı kurmak için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="fe532-335">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="fe532-336">**(İle oluşturulan proxy) günlük kaydını etkinleştirme**</span><span class="sxs-lookup"><span data-stu-id="fe532-336">**Enable logging (with the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

<span data-ttu-id="fe532-337">**(Olmadan oluşturulan proxy) günlüğe yazmayı etkinleştir**</span><span class="sxs-lookup"><span data-stu-id="fe532-337">**Enable logging (without the generated proxy)**</span></span>

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

<span data-ttu-id="fe532-338">Günlükleri görmek için tarayıcınızın Geliştirici Araçları'nı açın ve konsol sekmesine gidin. Adım adım yönergeler ve ekran görüntüsü bunun nasıl yapılacağını gösteren gösteren bir öğretici için bkz. [ASP.NET Signalr - günlüğü etkinleştir ile sunucu yayını](index.md).</span><span class="sxs-lookup"><span data-stu-id="fe532-338">To see the logs, open your browser's developer tools and go to the Console tab. For a tutorial that shows step-by-step instructions and screen shots that show how to do this, see [Server Broadcast with ASP.NET Signalr - Enable Logging](index.md).</span></span>
