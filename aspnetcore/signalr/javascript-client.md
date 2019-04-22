---
title: ASP.NET Core SignalR JavaScript istemcisi
author: bradygaster
description: ASP.NET Core SignalR JavaScript istemcisi genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: e58015221497a9f962edf9f9fdba7ea3025d7694
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705612"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="853bc-103">ASP.NET Core SignalR JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="853bc-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="853bc-104">Tarafından [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="853bc-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="853bc-105">ASP.NET Core SignalR JavaScript istemci kitaplığı, geliştiricilerin, sunucu taraflı hub kodu çağırmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="853bc-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="853bc-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="853bc-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="853bc-107">SignalR istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="853bc-107">Install the SignalR client package</span></span>

<span data-ttu-id="853bc-108">SignalR JavaScript istemci kitaplığı olarak teslim edilen bir [npm](https://www.npmjs.com/) paket.</span><span class="sxs-lookup"><span data-stu-id="853bc-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="853bc-109">Visual Studio kullanıyorsanız, çalıştırma `npm install` gelen **Paket Yöneticisi Konsolu** while kök klasöründe.</span><span class="sxs-lookup"><span data-stu-id="853bc-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="853bc-110">Visual Studio Code için komutu çalıştırın **tümleşik Terminalini**.</span><span class="sxs-lookup"><span data-stu-id="853bc-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="853bc-111">npm paket içeriğini yükler *node_modules\\@aspnet\signalr\dist\browser* klasör.</span><span class="sxs-lookup"><span data-stu-id="853bc-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="853bc-112">Adlı yeni bir klasör oluşturun *signalr* altında *wwwroot\\LIB* klasör.</span><span class="sxs-lookup"><span data-stu-id="853bc-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="853bc-113">Kopyalama *signalr.js* dosyasını *wwwroot\lib\signalr* klasör.</span><span class="sxs-lookup"><span data-stu-id="853bc-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="853bc-114">SignalR JavaScript istemcisi kullanma</span><span class="sxs-lookup"><span data-stu-id="853bc-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="853bc-115">SignalR JavaScript istemci başvuru `<script>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="853bc-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="853bc-116">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="853bc-116">Connect to a hub</span></span>

<span data-ttu-id="853bc-117">Aşağıdaki kod oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="853bc-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="853bc-118">Hub'ının adı büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="853bc-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="853bc-119">Çıkış noktaları arası bağlantılar</span><span class="sxs-lookup"><span data-stu-id="853bc-119">Cross-origin connections</span></span>

<span data-ttu-id="853bc-120">Genellikle, tarayıcılar istenen sayfa aynı etki alanında bağlantıları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="853bc-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="853bc-121">Ancak, başka bir etki alanı bağlantı gerekli olduğunda durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="853bc-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="853bc-122">Kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önlemek için [çıkış noktaları arası bağlantıları](xref:security/cors) varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="853bc-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="853bc-123">Çıkış noktaları arası istek izin vermek için bunu etkinleştirin `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="853bc-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="853bc-124">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="853bc-124">Call hub methods from client</span></span>

<span data-ttu-id="853bc-125">JavaScript istemcilerinin şirket hub'ları genel yöntemleri çağırmak [çağırma](/javascript/api/%40aspnet/signalr/hubconnection#invoke) yöntemi [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="853bc-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="853bc-126">`invoke` Yöntemi iki bağımsız değişkeni kabul eder:</span><span class="sxs-lookup"><span data-stu-id="853bc-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="853bc-127">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="853bc-127">The name of the hub method.</span></span> <span data-ttu-id="853bc-128">Aşağıdaki örnekte, yöntem hub'ında addır `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="853bc-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="853bc-129">Hub yönteminin içinde tanımlanan tüm bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="853bc-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="853bc-130">Aşağıdaki örnekte, bağımsız değişkeni addır `message`.</span><span class="sxs-lookup"><span data-stu-id="853bc-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="853bc-131">Kod örneği, Internet Explorer dışındaki tüm bilinen tarayıcılar'ın geçerli sürümlerinde desteklenen ok işlev sözdizimi kullanır.</span><span class="sxs-lookup"><span data-stu-id="853bc-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="853bc-132">Azure SignalR hizmeti kullanıyorsanız, *sunucusuz modu*, bir istemciden hub yöntemlerini çağıramazsınız.</span><span class="sxs-lookup"><span data-stu-id="853bc-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="853bc-133">Daha fazla bilgi için [SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="853bc-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="853bc-134">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="853bc-134">Call client methods from hub</span></span>

<span data-ttu-id="853bc-135">Hub'ından iletiler almak için bir yöntemi kullanarak tanımlarsınız [üzerinde](/javascript/api/%40aspnet/signalr/hubconnection#on) yöntemi `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="853bc-135">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="853bc-136">JavaScript istemci yöntemin adı.</span><span class="sxs-lookup"><span data-stu-id="853bc-136">The name of the JavaScript client method.</span></span> <span data-ttu-id="853bc-137">Aşağıdaki örnekte, yöntem adı olan `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="853bc-137">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="853bc-138">Bağımsız değişkenleri hub yöntemine geçirir.</span><span class="sxs-lookup"><span data-stu-id="853bc-138">Arguments the hub passes to the method.</span></span> <span data-ttu-id="853bc-139">Aşağıdaki örnekte, bağımsız değişken değeri olan `message`.</span><span class="sxs-lookup"><span data-stu-id="853bc-139">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="853bc-140">Önceki kodda `connection.on` sunucu tarafı kod kullanarak çağırdığında çalıştıran [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="853bc-140">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="853bc-141">SignalR yöntem adını eşleştirerek çağırmak için hangi istemci yöntemini belirler ve bağımsız değişkenler tanımlanan `SendAsync` ve `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="853bc-141">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="853bc-142">En iyi uygulama, çağrı [Başlat](/javascript/api/%40aspnet/signalr/hubconnection#start) metodunda `HubConnection` sonra `on`.</span><span class="sxs-lookup"><span data-stu-id="853bc-142">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="853bc-143">İletileri almadan önce İşleyicileriniz kayıtlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="853bc-143">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="853bc-144">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="853bc-144">Error handling and logging</span></span>

<span data-ttu-id="853bc-145">Zincir bir `catch` sonuna yöntemi `start` istemci tarafı hataları işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="853bc-145">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="853bc-146">Kullanım `console.error` tarayıcının konsola çıktı hataları.</span><span class="sxs-lookup"><span data-stu-id="853bc-146">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="853bc-147">Bağlantı kurulduğunda oturum için bir Günlükçü ve olay türünü geçirerek istemci tarafı günlük izleme ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="853bc-147">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="853bc-148">Belirtilen günlük düzeyi ve üzeri, iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="853bc-148">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="853bc-149">Kullanılabilir günlük düzeyleri aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="853bc-149">Available log levels are as follows:</span></span>

* <span data-ttu-id="853bc-150">`signalR.LogLevel.Error` &ndash; Hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="853bc-150">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="853bc-151">Günlükleri `Error` yalnızca iletileri.</span><span class="sxs-lookup"><span data-stu-id="853bc-151">Logs `Error` messages only.</span></span>
* <span data-ttu-id="853bc-152">`signalR.LogLevel.Warning` &ndash; Olası hatalar hakkında uyarı iletileri.</span><span class="sxs-lookup"><span data-stu-id="853bc-152">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="853bc-153">Günlükleri `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="853bc-153">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="853bc-154">`signalR.LogLevel.Information` &ndash; Hatasız durum iletileri.</span><span class="sxs-lookup"><span data-stu-id="853bc-154">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="853bc-155">Günlükleri `Information`, `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="853bc-155">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="853bc-156">`signalR.LogLevel.Trace` &ndash; İzleme iletileri.</span><span class="sxs-lookup"><span data-stu-id="853bc-156">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="853bc-157">Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="853bc-157">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="853bc-158">Kullanım [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metodunda [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) günlük düzeyi yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="853bc-158">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="853bc-159">İletileri tarayıcı konsoluna yazılır.</span><span class="sxs-lookup"><span data-stu-id="853bc-159">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="853bc-160">İstemcileri yeniden</span><span class="sxs-lookup"><span data-stu-id="853bc-160">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="853bc-161">Otomatik olarak yeniden</span><span class="sxs-lookup"><span data-stu-id="853bc-161">Automatically reconnect</span></span>

<span data-ttu-id="853bc-162">SignalR JavaScript istemcisi kullanarak otomatik olarak yeniden yapılandırılabilir `withAutomaticReconnect` metodunda [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="853bc-162">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="853bc-163">Varsayılan olarak otomatik olarak yeniden olmaz.</span><span class="sxs-lookup"><span data-stu-id="853bc-163">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="853bc-164">Herhangi bir parametre olmadan `withAutomaticReconnect()` dört girişimi başarısız olduktan sonra durduruluyor, her bir yeniden bağlanma denemesi denemeden önce sırasıyla 0, 2, 10 ve 30 saniye bekleyin üzere istemciyi yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="853bc-164">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="853bc-165">Yeniden bağlanma girişimleri başlatmadan önce `HubConnection` geçiş olur `HubConnectionState.Reconnecting` belirtin ve yangın kendi `onreconnecting` geçiş yerine geri çağırmaları `Disconnected` durumu ve tetikleme kendi `onclose` geri çağırmaları ister bir `HubConnection`otomatik yeniden olmadan yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="853bc-165">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="853bc-166">Bu, bağlantısı kesilmiş kullanıcıları uyarın ve UI öğelerini devre dışı bırakmak için bir fırsat sağlar.</span><span class="sxs-lookup"><span data-stu-id="853bc-166">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="853bc-167">İstemci ilk dört girişimlerinin içinde başarıyla bağlanırsa `HubConnection` geri geçeceğiyle `Connected` belirtin ve yangın kendi `onreconnected` geri çağırmaları.</span><span class="sxs-lookup"><span data-stu-id="853bc-167">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="853bc-168">Bu, bağlantı yeniden kurulmadan kullanıcılara bildirmek için bir fırsat sağlar.</span><span class="sxs-lookup"><span data-stu-id="853bc-168">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="853bc-169">Bağlantı tamamen yeni sunucuya baktığı yeni `connectionId` için sağlanan `onreconnected` geri çağırma.</span><span class="sxs-lookup"><span data-stu-id="853bc-169">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="853bc-170">`onreconnected` Geri çağırma'nın `connectionId` varsa parametresi tanımsız olur `HubConnection` için yapılandırılan [anlaşma atla](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="853bc-170">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="853bc-171">`withAutomaticReconnect()` Yapılandırma olmayacaktır `HubConnection` başlangıç hataları elle gerek ilk hataları yeniden denemek için:</span><span class="sxs-lookup"><span data-stu-id="853bc-171">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

```javascript
async function start() {
    try {
        await connection.start();
        console.assert(connection.state === signalR.HubConnectionState.Connected);
        console.log("connected");
    } catch (err) {
        console.assert(connection.state === signalR.HubConnectionState.Disconnected);
        console.log(err);
        setTimeout(() => start(), 5000);
    }
};
```

<span data-ttu-id="853bc-172">İstemci ilk dört girişimlerinin içinde başarıyla yeniden değil `HubConnection` geçiş olur `Disconnected` belirtin ve yangın kendi [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) geri çağırmalar.</span><span class="sxs-lookup"><span data-stu-id="853bc-172">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="853bc-173">Bu bağlantıyı kalıcı olarak kesildi ve sayfayı yenilemeyi önerilir kullanıcılara bildirmek için bir fırsat sağlar:</span><span class="sxs-lookup"><span data-stu-id="853bc-173">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

<span data-ttu-id="853bc-174">Özel bir bağlantıyı kesmeden önce yeniden bağlanma denemesi sayısını yapılandırma veya yeniden zamanlamayı değiştirmek için `withAutomaticReconnect` yeniden bağlanma girişimleri başlatmadan önce beklenecek milisaniye cinsinden gecikme değeri temsil eden sayı dizisi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="853bc-174">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="853bc-175">Yukarıdaki örnekte yapılandırır `HubConnection` hemen bağlantı kaybedildikten sonra yeniden bağlantılar denemesi başlatılacak.</span><span class="sxs-lookup"><span data-stu-id="853bc-175">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="853bc-176">Bu aynı zamanda varsayılan yapılandırması için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="853bc-176">This is also true for the default configuration.</span></span>

<span data-ttu-id="853bc-177">İlk yeniden deneme başarısız olursa, ikinci yeniden bağlanma denemesi de 2 Varsayılan yapılandırmada gibi saniye beklemek yerine başlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="853bc-177">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="853bc-178">İkinci yeniden bağlanma denemesi başarısız olursa, üçüncü yeniden denemede yeniden gibi varsayılan yapılandırması olan 10 saniye içinde başlar.</span><span class="sxs-lookup"><span data-stu-id="853bc-178">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="853bc-179">Bir çalışan yerine hata üçüncü yeniden bağlanma girişimi yapıldıktan sonra özel davranış ardından tekrar varsayılan davranış durdurarak kareninkinden daha girişimi 30 başka bir Varsayılan yapılandırmada gibi saniye içinde yeniden.</span><span class="sxs-lookup"><span data-stu-id="853bc-179">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="853bc-180">Zamanlama ve otomatik sayısı daha fazla denetime yeniden bağlanma girişimleri, isterseniz `withAutomaticReconnect` kabul eden bir nesneyi uygulama `IReconnectPolicy` adlı tek bir yöntem olan arabirimi `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="853bc-180">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IReconnectPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="853bc-181">`nextRetryDelayInMilliseconds` iki bağımsız değişkeni alır `previousRetryCount` ve `elapsedMilliseconds`, her iki numarayı olduğu.</span><span class="sxs-lookup"><span data-stu-id="853bc-181">`nextRetryDelayInMilliseconds` takes two arguments, `previousRetryCount` and `elapsedMilliseconds`, which are both numbers.</span></span> <span data-ttu-id="853bc-182">Yeniden bağlanma denemesi ilk önce her ikisini de `previousRetryCount` ve `elapsedMilliseconds` sıfır olur.</span><span class="sxs-lookup"><span data-stu-id="853bc-182">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero.</span></span> <span data-ttu-id="853bc-183">Başarısız tekrar deneme girişimleri sonra `previousRetryCount` birer birer artar ve `elapsedMilliseconds` şimdiye milisaniye cinsinden yeniden bağlanmayı harcanan süreyi yansıtacak şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="853bc-183">After each failed retry attempt, `previousRetryCount` will be incremented by one and `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds.</span></span>

<span data-ttu-id="853bc-184">`nextRetryDelayInMilliseconds` bir sonraki yeniden denemeden önce beklenecek milisaniye sayısını temsil eden bir sayı döndürmelidir veya `null` varsa `HubConnection` yeniden bağlanmayı durdurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="853bc-184">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: (previousRetryCount, elapsedMilliseconds) => {
          if (elapsedMilliseconds < 60000) {
            // If we've been reconnecting for less than 60 seconds so far,
            // wait between 0 and 10 seconds before the next reconnect attempt.
            return Math.random() * 10000;
          } else {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
          }
        })
    .build();
```

<span data-ttu-id="853bc-185">Alternatif olarak, istemci gösterildiği şekilde el ile yeniden bağlanacak kod yazabileceğiniz [el ile yeniden](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="853bc-185">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="853bc-186">El ile yeniden</span><span class="sxs-lookup"><span data-stu-id="853bc-186">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="853bc-187">Önce 3.0, SignalR için JavaScript istemci otomatik olarak yeniden değil.</span><span class="sxs-lookup"><span data-stu-id="853bc-187">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="853bc-188">İstemcinizi el ile yeniden kod yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="853bc-188">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="853bc-189">Aşağıdaki kod tipik el ile yeniden yaklaşımı gösterir:</span><span class="sxs-lookup"><span data-stu-id="853bc-189">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="853bc-190">Bir işlev (Bu durumda, `start` işlevi) bağlantıyı başlatmak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="853bc-190">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="853bc-191">Çağrı `start` bağlantının işlevinde `onclose` olay işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="853bc-191">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="853bc-192">Gerçek uygulama bir üstel geri alma kullanın veya ancak bir belirtilen sayıda vazgeçmeden önce yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="853bc-192">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="853bc-193">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="853bc-193">Additional resources</span></span>

* [<span data-ttu-id="853bc-194">JavaScript API'si başvurusu</span><span class="sxs-lookup"><span data-stu-id="853bc-194">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="853bc-195">JavaScript Öğreticisi</span><span class="sxs-lookup"><span data-stu-id="853bc-195">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="853bc-196">Web ve TypeScript Öğreticisi</span><span class="sxs-lookup"><span data-stu-id="853bc-196">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="853bc-197">Merkezler</span><span class="sxs-lookup"><span data-stu-id="853bc-197">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="853bc-198">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="853bc-198">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="853bc-199">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="853bc-199">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="853bc-200">Çıkış noktaları arası istekleri (CORS)</span><span class="sxs-lookup"><span data-stu-id="853bc-200">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="853bc-201">Sunucusuz Azure SignalR hizmeti belgeleri</span><span class="sxs-lookup"><span data-stu-id="853bc-201">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
