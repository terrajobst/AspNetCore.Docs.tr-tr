---
title: ASP.NET Core SignalR JavaScript istemcisi
author: bradygaster
description: ASP.NET Core SignalR JavaScript istemcisine genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/javascript-client
ms.openlocfilehash: eaf737642cdbd7ab2b1b5c16538b47a70cddd332
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354694"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a><span data-ttu-id="7cbd3-103">ASP.NET Core SignalR JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="7cbd3-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="7cbd3-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="7cbd3-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="7cbd3-105">ASP.NET Core SignalR JavaScript istemci kitaplığı, geliştiricilerin sunucu tarafı hub kodunu çağırmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="7cbd3-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7cbd3-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-client-package"></a><span data-ttu-id="7cbd3-107">SignalR istemci paketini yükler</span><span class="sxs-lookup"><span data-stu-id="7cbd3-107">Install the SignalR client package</span></span>

<span data-ttu-id="7cbd3-108">SignalR JavaScript istemci kitaplığı [NPM](https://www.npmjs.com/) paketi olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="7cbd3-109">Visual Studio kullanıyorsanız, çalıştırma `npm install` gelen **Paket Yöneticisi Konsolu** while kök klasöründe.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="7cbd3-110">Visual Studio Code için komutu çalıştırın **tümleşik Terminalini**.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

<span data-ttu-id="7cbd3-111">npm paket içeriğini yükler *node_modules\\@microsoft\signalr\dist\browser* klasör.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-111">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="7cbd3-112">Adlı yeni bir klasör oluşturun *signalr* altında *wwwroot\\LIB* klasör.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="7cbd3-113">Kopyalama *signalr.js* dosyasını *wwwroot\lib\signalr* klasör.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="7cbd3-114">npm paket içeriğini yükler *node_modules\\@aspnet\signalr\dist\browser* klasör.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-114">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="7cbd3-115">Adlı yeni bir klasör oluşturun *signalr* altında *wwwroot\\LIB* klasör.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-115">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="7cbd3-116">Kopyalama *signalr.js* dosyasını *wwwroot\lib\signalr* klasör.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-116">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a><span data-ttu-id="7cbd3-117">SignalR JavaScript istemcisini kullanma</span><span class="sxs-lookup"><span data-stu-id="7cbd3-117">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="7cbd3-118">`<script>` öğesinde JavaScript istemcisine SignalR başvurun.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-118">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="7cbd3-119">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="7cbd3-119">Connect to a hub</span></span>

<span data-ttu-id="7cbd3-120">Aşağıdaki kod oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-120">The following code creates and starts a connection.</span></span> <span data-ttu-id="7cbd3-121">Hub'ının adı büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-121">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="7cbd3-122">Çıkış noktaları arası bağlantılar</span><span class="sxs-lookup"><span data-stu-id="7cbd3-122">Cross-origin connections</span></span>

<span data-ttu-id="7cbd3-123">Genellikle, tarayıcılar istenen sayfa aynı etki alanında bağlantıları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-123">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="7cbd3-124">Ancak, başka bir etki alanı bağlantı gerekli olduğunda durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-124">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="7cbd3-125">Kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önlemek için [çıkış noktaları arası bağlantıları](xref:security/cors) varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-125">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="7cbd3-126">Çıkış noktaları arası istek izin vermek için bunu etkinleştirin `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-126">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="7cbd3-127">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="7cbd3-127">Call hub methods from client</span></span>

<span data-ttu-id="7cbd3-128">JavaScript istemcilerinin şirket hub'ları genel yöntemleri çağırmak [çağırma](/javascript/api/%40aspnet/signalr/hubconnection#invoke) yöntemi [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="7cbd3-128">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="7cbd3-129">`invoke` Yöntemi iki bağımsız değişkeni kabul eder:</span><span class="sxs-lookup"><span data-stu-id="7cbd3-129">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="7cbd3-130">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-130">The name of the hub method.</span></span> <span data-ttu-id="7cbd3-131">Aşağıdaki örnekte, yöntem hub'ında addır `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-131">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="7cbd3-132">Hub yönteminin içinde tanımlanan tüm bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-132">Any arguments defined in the hub method.</span></span> <span data-ttu-id="7cbd3-133">Aşağıdaki örnekte, bağımsız değişkeni addır `message`.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-133">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="7cbd3-134">Örnek kod, Internet Explorer hariç tüm büyük tarayıcıların güncel sürümlerinde desteklenen ok işlev sözdizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-134">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="7cbd3-135">Azure SignalR hizmetini *sunucusuz modda*kullanıyorsanız, bir istemciden hub yöntemlerini çağıramezsiniz.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="7cbd3-136">Daha fazla bilgi için [SignalR hizmeti belgelerine](/azure/azure-signalr/signalr-concept-serverless-development-config)bakın.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="7cbd3-137">`invoke` yöntemi bir JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)döndürür.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-137">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="7cbd3-138">`Promise`, sunucudaki yöntemi döndürdüğünde döndürülen değer (varsa) ile çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-138">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="7cbd3-139">Sunucu üzerindeki yöntem bir hata oluşturursa `Promise` hata iletisiyle reddedilir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-139">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="7cbd3-140">Bu durumları (veya `await` sözdizimini) işlemek için `Promise` üzerinde `then` ve `catch` yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-140">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="7cbd3-141">`send` yöntemi bir JavaScript `Promise`döndürür.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-141">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="7cbd3-142">İleti sunucuya gönderildiğinde `Promise` çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-142">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="7cbd3-143">İleti gönderilirken bir hata oluşursa `Promise` hata iletisiyle reddedilir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-143">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="7cbd3-144">Bu durumları (veya `await` sözdizimini) işlemek için `Promise` üzerinde `then` ve `catch` yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-144">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="7cbd3-145">`send` kullanmak sunucu iletiyi almadan önce beklemez.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-145">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="7cbd3-146">Sonuç olarak, sunucudan veri veya hata döndürülmesi mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-146">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="7cbd3-147">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="7cbd3-147">Call client methods from hub</span></span>

<span data-ttu-id="7cbd3-148">Hub'ından iletiler almak için bir yöntemi kullanarak tanımlarsınız [üzerinde](/javascript/api/%40aspnet/signalr/hubconnection#on) yöntemi `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-148">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="7cbd3-149">JavaScript istemci yöntemin adı.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-149">The name of the JavaScript client method.</span></span> <span data-ttu-id="7cbd3-150">Aşağıdaki örnekte, yöntem adı olan `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-150">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="7cbd3-151">Bağımsız değişkenleri hub yöntemine geçirir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-151">Arguments the hub passes to the method.</span></span> <span data-ttu-id="7cbd3-152">Aşağıdaki örnekte, bağımsız değişken değeri olan `message`.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-152">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="7cbd3-153">Önceki kodda `connection.on` sunucu tarafı kod kullanarak çağırdığında çalıştıran [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-153">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR<span data-ttu-id="7cbd3-154">, `SendAsync` ve `connection.on`tanımlanan yöntem adı ve bağımsız değişkenlerle eşleştirerek hangi istemci yönteminin çağrılacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-154"> determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="7cbd3-155">En iyi uygulama, çağrı [Başlat](/javascript/api/%40aspnet/signalr/hubconnection#start) metodunda `HubConnection` sonra `on`.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-155">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="7cbd3-156">İletileri almadan önce İşleyicileriniz kayıtlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-156">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="7cbd3-157">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="7cbd3-157">Error handling and logging</span></span>

<span data-ttu-id="7cbd3-158">Zincir bir `catch` sonuna yöntemi `start` istemci tarafı hataları işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-158">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="7cbd3-159">Kullanım `console.error` tarayıcının konsola çıktı hataları.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-159">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="7cbd3-160">Bağlantı kurulduğunda oturum için bir Günlükçü ve olay türünü geçirerek istemci tarafı günlük izleme ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-160">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="7cbd3-161">Belirtilen günlük düzeyi ve üzeri, iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-161">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="7cbd3-162">Kullanılabilir günlük düzeyleri aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="7cbd3-162">Available log levels are as follows:</span></span>

* <span data-ttu-id="7cbd3-163">`signalR.LogLevel.Error` &ndash; Hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-163">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="7cbd3-164">Günlükleri `Error` yalnızca iletileri.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-164">Logs `Error` messages only.</span></span>
* <span data-ttu-id="7cbd3-165">`signalR.LogLevel.Warning` &ndash; Olası hatalar hakkında uyarı iletileri.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-165">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="7cbd3-166">Günlükleri `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-166">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="7cbd3-167">`signalR.LogLevel.Information` &ndash; Hatasız durum iletileri.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-167">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="7cbd3-168">Günlükleri `Information`, `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-168">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="7cbd3-169">`signalR.LogLevel.Trace` &ndash; İzleme iletileri.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-169">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="7cbd3-170">Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-170">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="7cbd3-171">Kullanım [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metodunda [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) günlük düzeyi yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-171">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="7cbd3-172">İletileri tarayıcı konsoluna yazılır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-172">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="7cbd3-173">İstemcileri yeniden</span><span class="sxs-lookup"><span data-stu-id="7cbd3-173">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="7cbd3-174">Otomatik olarak yeniden bağlan</span><span class="sxs-lookup"><span data-stu-id="7cbd3-174">Automatically reconnect</span></span>

<span data-ttu-id="7cbd3-175">SignalR için JavaScript istemcisi, [Hubconnectionbuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)'daki `withAutomaticReconnect` yöntemi kullanılarak otomatik olarak yeniden bağlanacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-175">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="7cbd3-176">Varsayılan olarak otomatik olarak yeniden bağlanmaz.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-176">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="7cbd3-177">Hiçbir parametre olmadan, `withAutomaticReconnect()` her bir yeniden bağlanma denemesini denemeden önce, dört başarısız denemeden sonra durdurmadan, istemciyi 0, 2, 10 ve 30 saniye bekleyecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-177">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="7cbd3-178">Yeniden bağlanma girişimlerini başlatmadan önce, `HubConnection` `HubConnectionState.Reconnecting` durumuna geçer ve `onclose` geri çağırmaları `Disconnected`, otomatik yeniden bağlanma yapılandırması olmadan `HubConnection` gibi tetikleyerek `onreconnecting` geri çağırmaları tetikler.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-178">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="7cbd3-179">Bu, kullanıcıların bağlantının kaybedildiği ve Kullanıcı arabirimi öğelerini devre dışı bırakan kullanıcıları uyarma fırsatı sağlar.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-179">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="7cbd3-180">İstemci ilk dört denemeden sonra başarıyla yeniden bağlanırsa, `HubConnection` `Connected` duruma geri dönerek `onreconnected` geri çağırmaları harekete geçer.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-180">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="7cbd3-181">Bu, kullanıcılara bağlantı yeniden kurulmadığını bildirmek için bir fırsat sağlar.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-181">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="7cbd3-182">Bağlantı sunucuya tamamen yeni göründüğünden, `onreconnected` geri çağırması için yeni bir `connectionId` sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-182">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="7cbd3-183">`HubConnection` [anlaşmayı atlayacak](xref:signalr/configuration#configure-client-options)şekilde yapılandırıldıysa, `onreconnected` geri aramanın `connectionId` parametresi tanımsız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-183">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="7cbd3-184">`withAutomaticReconnect()`, ilk başlatma başarısızlıklarını yeniden denemek için `HubConnection` yapılandırmaz, bu nedenle başlatma hatalarının el ile işlenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="7cbd3-184">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="7cbd3-185">İstemci ilk dört denemeden sonra başarıyla yeniden bağlanmazsa, `HubConnection` `Disconnected` durumuna geçer ve [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) geri aramalarını harekete geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-185">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="7cbd3-186">Bu, kullanıcılara bağlantının kalıcı olarak kaybedilmediğini bildirme ve sayfayı yenilemeyi önerme olanağı sağlar:</span><span class="sxs-lookup"><span data-stu-id="7cbd3-186">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="7cbd3-187">Bağlantıyı kesmeden veya yeniden bağlanma zamanlamasını değiştirmeden önce özel sayıda yeniden bağlantı girişimi yapılandırmak için `withAutomaticReconnect`, her bir yeniden bağlanma girişimine başlamadan önce beklenecek gecikme süresi temsil eden bir sayı dizisini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-187">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="7cbd3-188">Yukarıdaki örnek, bağlantı kaybolduktan hemen sonra yeniden bağlanmaya başlamak için `HubConnection` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-188">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="7cbd3-189">Bu, varsayılan yapılandırma için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-189">This is also true for the default configuration.</span></span>

<span data-ttu-id="7cbd3-190">İlk yeniden bağlantı girişimi başarısız olursa, ikinci yeniden bağlanma denemesi de varsayılan yapılandırmada olduğu gibi 2 saniye beklemek yerine hemen başlatılır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-190">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="7cbd3-191">İkinci yeniden bağlantı girişimi başarısız olursa, üçüncü yeniden bağlanma denemesi varsayılan yapılandırma gibi 10 saniye içinde başlar.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-191">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="7cbd3-192">Özel davranış daha sonra varsayılan şekilde yeniden bağlantı kurmayı denemek yerine, üçüncü yeniden bağlantı girişimi başarısızlığından sonra varsayılan davranıştan sonra yeniden bir kez daha fazla yeniden bağlantı kurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-192">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="7cbd3-193">Otomatik yeniden bağlanma denemelerinin zamanlaması ve sayısı üzerinde daha fazla denetime sahip olmak istiyorsanız `withAutomaticReconnect`, `nextRetryDelayInMilliseconds`adlı tek bir yönteme sahip `IRetryPolicy` arabirimini uygulayan bir nesneyi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-193">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="7cbd3-194">`nextRetryDelayInMilliseconds` `RetryContext`türünde tek bir bağımsız değişken alır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-194">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="7cbd3-195">`RetryContext` üç özelliğe sahiptir: `previousRetryCount`, `elapsedMilliseconds` ve `retryReason` `number`, `number` ve `Error`.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-195">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="7cbd3-196">İlk yeniden bağlanma denemesinden önce, hem `previousRetryCount` hem de `elapsedMilliseconds` sıfır olur ve `retryReason` bağlantının kaybolmasına neden olan hata olacaktır.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-196">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="7cbd3-197">Her başarısız yeniden deneme denemesinden `previousRetryCount`, `elapsedMilliseconds` bir kez artırılır, bu, şimdiye kadar geçen süre (milisaniye olarak) ve `retryReason` son yeniden bağlanma denemesinin başarısız olmasına neden olacak hata miktarını yansıtacak şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-197">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="7cbd3-198">`nextRetryDelayInMilliseconds`, sonraki yeniden bağlanma girişiminden önce beklenecek milisaniye sayısını temsil eden bir sayı döndürmelidir veya `HubConnection` yeniden bağlamayı durdurması gerekiyorsa `null`.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-198">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: retryContext => {
            if (retryContext.elapsedMilliseconds < 60000) {
                // If we've been reconnecting for less than 60 seconds so far,
                // wait between 0 and 10 seconds before the next reconnect attempt.
                return Math.random() * 10000;
            } else {
                // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
                return null;
            }
        }
    })
    .build();
```

<span data-ttu-id="7cbd3-199">Alternatif olarak, [el ile yeniden bağlanma](#manually-reconnect)bölümünde gösterildiği gibi istemcinizi el ile yeniden bağlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-199">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="7cbd3-200">El ile yeniden bağlan</span><span class="sxs-lookup"><span data-stu-id="7cbd3-200">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="7cbd3-201">3,0 ' den önce SignalR JavaScript istemcisi otomatik olarak yeniden bağlanmaz.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-201">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="7cbd3-202">İstemcinizi el ile yeniden kod yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-202">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="7cbd3-203">Aşağıdaki kod, genel bir el ile yeniden bağlantı yaklaşımını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="7cbd3-203">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="7cbd3-204">Bir işlev (Bu durumda, `start` işlevi) bağlantıyı başlatmak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-204">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="7cbd3-205">Çağrı `start` bağlantının işlevinde `onclose` olay işleyicisi.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-205">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="7cbd3-206">Gerçek uygulama bir üstel geri alma kullanın veya ancak bir belirtilen sayıda vazgeçmeden önce yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="7cbd3-206">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7cbd3-207">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7cbd3-207">Additional resources</span></span>

* [<span data-ttu-id="7cbd3-208">JavaScript API'si başvurusu</span><span class="sxs-lookup"><span data-stu-id="7cbd3-208">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="7cbd3-209">JavaScript Öğreticisi</span><span class="sxs-lookup"><span data-stu-id="7cbd3-209">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="7cbd3-210">Web ve TypeScript Öğreticisi</span><span class="sxs-lookup"><span data-stu-id="7cbd3-210">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="7cbd3-211">Merkezler</span><span class="sxs-lookup"><span data-stu-id="7cbd3-211">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="7cbd3-212">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="7cbd3-212">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="7cbd3-213">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="7cbd3-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="7cbd3-214">Çıkış noktaları arası istekleri (CORS)</span><span class="sxs-lookup"><span data-stu-id="7cbd3-214">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* <span data-ttu-id="7cbd3-215">[Azure SignalR hizmeti sunucusuz belgeler](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="7cbd3-215">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
