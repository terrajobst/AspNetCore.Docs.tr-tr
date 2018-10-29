---
title: ASP.NET Core SignalR Java istemci
author: mikaelm12
description: ASP.NET Core SignalR Java istemcisi kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 646118c78d5d38b44b89d399cd06a5332a11d064
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207777"
---
# <a name="aspnet-core-signalr-java-client"></a><span data-ttu-id="14fe0-103">ASP.NET Core SignalR Java istemci</span><span class="sxs-lookup"><span data-stu-id="14fe0-103">ASP.NET Core SignalR Java client</span></span>

<span data-ttu-id="14fe0-104">Tarafından [Mikael Mengistu](https://twitter.com/MikaelM_12)</span><span class="sxs-lookup"><span data-stu-id="14fe0-104">By [Mikael Mengistu](https://twitter.com/MikaelM_12)</span></span>

<span data-ttu-id="14fe0-105">Android uygulamaları dahil olmak üzere Java koddan bir ASP.NET Core SignalR sunucusuna bağlanan Java istemci sağlar.</span><span class="sxs-lookup"><span data-stu-id="14fe0-105">The Java client enables connecting to an ASP.NET Core SignalR server from Java code, including Android apps.</span></span> <span data-ttu-id="14fe0-106">Gibi [JavaScript istemci](xref:signalr/javascript-client) ve [.NET istemci](xref:signalr/dotnet-client), almak ve gerçek zamanlı bir hub'ına ileti göndermek Java istemcisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="14fe0-106">Like the [JavaScript client](xref:signalr/javascript-client) and the [.NET client](xref:signalr/dotnet-client), the Java client enables you to receive and send messages to a hub in real time.</span></span> <span data-ttu-id="14fe0-107">Java istemci, ASP.NET Core 2.2 bulunan ve üzerinde desteklenir.</span><span class="sxs-lookup"><span data-stu-id="14fe0-107">The Java client is available in ASP.NET Core 2.2 and later.</span></span>

<span data-ttu-id="14fe0-108">Bu makalede bahsedilen örnek Java konsol uygulaması SignalR Java istemcisi kullanır.</span><span class="sxs-lookup"><span data-stu-id="14fe0-108">The sample Java console app referenced in this article uses the SignalR Java client.</span></span>

<span data-ttu-id="14fe0-109">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="14fe0-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-java-client-package"></a><span data-ttu-id="14fe0-110">SignalR Java istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="14fe0-110">Install the SignalR Java client package</span></span>

<span data-ttu-id="14fe0-111">*1.0.0 preview3 35501 signalr* JAR dosyasını istemcilerin SignalR hub'ları bağlanmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="14fe0-111">The *signalr-1.0.0-preview3-35501* JAR file allows clients to connect to SignalR hubs.</span></span> <span data-ttu-id="14fe0-112">JAR dosyası en son sürüm numarasını bulmak için bkz: [Maven arama sonuçları](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span><span class="sxs-lookup"><span data-stu-id="14fe0-112">To find the latest JAR file version number, see the [Maven search results](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).</span></span>

<span data-ttu-id="14fe0-113">Gradle kullanıyorsanız, aşağıdaki satırı ekleyin `dependencies` bölümünü, *build.gradle* dosyası:</span><span class="sxs-lookup"><span data-stu-id="14fe0-113">If using Gradle, add the following line to the `dependencies` section of your *build.gradle* file:</span></span>

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

<span data-ttu-id="14fe0-114">Maven kullanarak eklerseniz içinde aşağıdaki satırları `<dependencies>` öğesinin, *pom.xml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="14fe0-114">If using Maven, add the following lines inside the `<dependencies>` element of your *pom.xml* file:</span></span>

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a><span data-ttu-id="14fe0-115">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="14fe0-115">Connect to a hub</span></span>

<span data-ttu-id="14fe0-116">Kurmak için bir `HubConnection`, `HubConnectionBuilder` kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="14fe0-116">To establish a `HubConnection`, the `HubConnectionBuilder` should be used.</span></span> <span data-ttu-id="14fe0-117">Hub'ı URL'si ve günlük düzeyinde bir bağlantı oluşturulurken yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="14fe0-117">The hub URL and log level can be configured while building a connection.</span></span> <span data-ttu-id="14fe0-118">Gerekli tüm seçenekler herhangi birini çağırarak yapılandırma `HubConnectionBuilder` yöntemleri önce `build`.</span><span class="sxs-lookup"><span data-stu-id="14fe0-118">Configure any required options by calling any of the `HubConnectionBuilder` methods before `build`.</span></span> <span data-ttu-id="14fe0-119">Bağlantıyı başlatmak `start`.</span><span class="sxs-lookup"><span data-stu-id="14fe0-119">Start the connection with `start`.</span></span>

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="14fe0-120">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="14fe0-120">Call hub methods from client</span></span>

<span data-ttu-id="14fe0-121">Bir çağrı `send` bir hub yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="14fe0-121">A call to `send` invokes a hub method.</span></span> <span data-ttu-id="14fe0-122">Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `send`.</span><span class="sxs-lookup"><span data-stu-id="14fe0-122">Pass the hub method name and any arguments defined in the hub method to `send`.</span></span>

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="14fe0-123">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="14fe0-123">Call client methods from hub</span></span>

<span data-ttu-id="14fe0-124">Kullanım `hubConnection.on` hub çağıran istemciye yöntemleri tanımlamak için.</span><span class="sxs-lookup"><span data-stu-id="14fe0-124">Use `hubConnection.on` to define methods on the client that the hub can call.</span></span> <span data-ttu-id="14fe0-125">Yapı sonra ancak bağlantı başlatmadan önce yöntemleri tanımlar.</span><span class="sxs-lookup"><span data-stu-id="14fe0-125">Define the methods after building but before starting the connection.</span></span>

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a><span data-ttu-id="14fe0-126">Günlük kaydı ekleme</span><span class="sxs-lookup"><span data-stu-id="14fe0-126">Add logging</span></span>

<span data-ttu-id="14fe0-127">SignalR Java istemcinin kullandığı [SLF4J](https://www.slf4j.org/) günlüğe kaydetme için kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="14fe0-127">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="14fe0-128">Seçtiğiniz kendi özel günlük uygulama özel günlük bağımlılık olarak getirerek kullanıcıların Kitaplığı'nın izin veren bir üst düzey günlüğe kaydetme API'si var.</span><span class="sxs-lookup"><span data-stu-id="14fe0-128">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="14fe0-129">Aşağıdaki kod parçacığını nasıl kullanılacağını gösterir `java.util.logging` SignalR Java istemcisi ile.</span><span class="sxs-lookup"><span data-stu-id="14fe0-129">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="14fe0-130">Bağımlılıklarınızı içinde günlüğü yapılandırmazsanız, aşağıdaki uyarı iletisi ile bir varsayılan yok-işlem günlükçü SLF4J yükler:</span><span class="sxs-lookup"><span data-stu-id="14fe0-130">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="14fe0-131">Bu güvenle yoksayılabilir.</span><span class="sxs-lookup"><span data-stu-id="14fe0-131">This can safely be ignored.</span></span>

## <a name="known-limitations"></a><span data-ttu-id="14fe0-132">Bilinen sınırlamalar</span><span class="sxs-lookup"><span data-stu-id="14fe0-132">Known limitations</span></span>

<span data-ttu-id="14fe0-133">Bu bir Java istemci Önizleme sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="14fe0-133">This is a preview release of the Java client.</span></span> <span data-ttu-id="14fe0-134">Bazı özellikler desteklenmez:</span><span class="sxs-lookup"><span data-stu-id="14fe0-134">Some features aren't supported:</span></span>

* <span data-ttu-id="14fe0-135">Yalnızca JSON Protokolü desteklenir.</span><span class="sxs-lookup"><span data-stu-id="14fe0-135">Only the JSON protocol is supported.</span></span>
* <span data-ttu-id="14fe0-136">WebSockets taşıma desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="14fe0-136">Only the WebSockets transport is supported.</span></span>
* <span data-ttu-id="14fe0-137">Akış henüz desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="14fe0-137">Streaming isn't supported yet.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="14fe0-138">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="14fe0-138">Additional resources</span></span>

* [<span data-ttu-id="14fe0-139">Java API'si başvurusu</span><span class="sxs-lookup"><span data-stu-id="14fe0-139">Java API reference</span></span>](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
