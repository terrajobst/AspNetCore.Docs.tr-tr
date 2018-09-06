---
title: ASP.NET Core SignalR Java istemci
author: mikaelm12
description: ASP.NET Core SignalR Java istemcisi kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/java-client
ms.openlocfilehash: f110f5391ac34f5cb4a72f64c16d86c8a37369a2
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43995430"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="c1016-103">ASP.NET Core SignalR Java istemci</span><span class="sxs-lookup"><span data-stu-id="c1016-103">ASP.NET Core SignalR Java Client</span></span>

<span data-ttu-id="c1016-104">Tarafından [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="c1016-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="c1016-105">Android uygulamaları dahil olmak üzere Java koddan bir ASP.NET Core SignalR sunucusuna bağlanan Java istemci sağlar.</span><span class="sxs-lookup"><span data-stu-id="c1016-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="c1016-106">Gibi [JavaScript istemci](xref:signalr/javascript-client) ve [.NET istemci](xref:signalr/dotnet-client), almak ve gerçek zamanlı bir hub'ına ileti göndermek Java istemcisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="c1016-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="c1016-107">Java istemci, ASP.NET Core 2.2 bulunan ve üzerinde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="c1016-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="c1016-108">Bu makalede bahsedilen örnek Java konsol uygulaması SignalR Java istemcisi kullanır.</span><span class="sxs-lookup"><span data-stu-id="c1016-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="c1016-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c1016-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="c1016-110">SignalR Java istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="c1016-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="c1016-111">*0.1.0 preview1 35029 signalr* JAR dosyasını istemcilerin SignalR hub'ları bağlanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="c1016-111">The *signalr-0.1.0-preview1-35029* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="c1016-112">JAR dosyası en son sürüm numarasını bulmak için bkz: [Maven arama sonuçları](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span><span class="sxs-lookup"><span data-stu-id="c1016-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.aspnet%20AND%20a:signalr&core=gav).</span></span>

<span data-ttu-id="c1016-113">Gradle kullanıyorsanız, aşağıdaki satırı ekleyin `dependencies` bölümünü, *build.gradle* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c1016-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.aspnet:signalr:0.1.0-preview1-35029'
```

<span data-ttu-id="c1016-114">Maven kullanarak eklerseniz içinde aşağıdaki satırları `<dependencies>` öğesinin, *pom.xml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c1016-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="c1016-115">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="c1016-115">Connect to a hub</span></span>

<span data-ttu-id="c1016-116">Kurmak için bir `HubConnection`, `HubConnectionBuilder` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="c1016-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="c1016-117">Hub'ı URL'si ve günlük düzeyinde bir bağlantı oluşturulurken yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="c1016-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="c1016-118">Gerekli tüm seçenekler herhangi birini çağırarak yapılandırma `HubConnectionBuilder` yöntemleri önce `build`.</span><span class="sxs-lookup"><span data-stu-id="c1016-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="c1016-119">Bağlantıyı başlatmak `start`.</span><span class="sxs-lookup"><span data-stu-id="c1016-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=17-20)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c1016-120">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="c1016-120">Call hub methods from client</span></span>

<span data-ttu-id="c1016-121">Bir çağrı `send` bir hub yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="c1016-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="c1016-122">Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `send`.</span><span class="sxs-lookup"><span data-stu-id="c1016-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=31)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c1016-123">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="c1016-123">Call client methods from hub</span></span>

<span data-ttu-id="c1016-124">Kullanım `hubConnection.on` hub çağıran istemciye yöntemleri tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="c1016-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="c1016-125">Yapı sonra ancak bağlantı başlatmadan önce yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c1016-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=22-24)]

## <a name="known-limitations"></a><span data-ttu-id="c1016-126">Bilinen sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="c1016-126">Known limitations</span></span>

<span data-ttu-id="c1016-127">Bu bir Java istemci erken Önizleme sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="c1016-127">This is an early preview release of the Java client.</span></span> <span data-ttu-id="c1016-128">Henüz desteklenmeyen çok özellikleri vardır.</span><span class="sxs-lookup"><span data-stu-id="c1016-128">There are many features that aren't supported yet.</span></span> <span data-ttu-id="c1016-129">Aşağıdaki boşlukları gelecek sürümleri için üzerinde çalışılan:</span><span class="sxs-lookup"><span data-stu-id="c1016-129">The following gaps are being worked on for future releases:</span></span>

* <span data-ttu-id="c1016-130">Yalnızca ilkel türler, parametre olarak kabul edilebilir ve dönüş türleri.</span><span class="sxs-lookup"><span data-stu-id="c1016-130">Only primitive types can be accepted as parameters and return types.</span></span>
* <span data-ttu-id="c1016-131">Zaman uyumlu apı'lerdir.</span><span class="sxs-lookup"><span data-stu-id="c1016-131">The APIs are synchronous.</span></span>
* <span data-ttu-id="c1016-132">Yalnızca "Gönder" çağrısı türü şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c1016-132">Only the "Send" call type is supported at this time.</span></span> <span data-ttu-id="c1016-133">"Çağır" ve dönüş değerleri akış desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="c1016-133">"Invoke" and streaming of return values aren't supported.</span></span>
* <span data-ttu-id="c1016-134">İstemci şu anda desteklemiyor [Azure SignalR hizmeti](/azure/azure-signalr/).</span><span class="sxs-lookup"><span data-stu-id="c1016-134">The client doesn't currently support the [Azure SignalR Service](/azure/azure-signalr/).</span></span>
* <span data-ttu-id="c1016-135">Yalnızca JSON Protokolü desteklenir.</span><span class="sxs-lookup"><span data-stu-id="c1016-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="c1016-136">WebSockets taşıma desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c1016-136">Only the WebSockets transport is supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1016-137">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c1016-137">Additional resources</span></span>

* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
