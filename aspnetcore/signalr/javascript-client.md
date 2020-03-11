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
ms.openlocfilehash: 3086b4aa532dfe992e19c193ef76f216f7835164
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657859"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a><span data-ttu-id="a2e70-103">ASP.NET Core SignalR JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="a2e70-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="a2e70-104">, [Oychel Appel](https://twitter.com/rachelappel) tarafından</span><span class="sxs-lookup"><span data-stu-id="a2e70-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="a2e70-105">ASP.NET Core SignalR JavaScript istemci kitaplığı, geliştiricilerin sunucu tarafı hub kodunu çağırmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2e70-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="a2e70-106">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a2e70-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-client-package"></a><span data-ttu-id="a2e70-107">SignalR istemci paketini yükler</span><span class="sxs-lookup"><span data-stu-id="a2e70-107">Install the SignalR client package</span></span>

<span data-ttu-id="a2e70-108">SignalR JavaScript istemci kitaplığı [NPM](https://www.npmjs.com/) paketi olarak dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="a2e70-109">Visual Studio kullanıyorsanız, kök klasörde, **Paket Yöneticisi konsolundan** `npm install` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="a2e70-110">Visual Studio Code için, **Tümleşik terminalden**komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

<span data-ttu-id="a2e70-111">NPM *node_modules\\@microsoft\signalr\dist\browser* klasöre paket içeriğini yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="a2e70-111">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="a2e70-112">*Wwwroot\\lib* klasörü altında *SignalR* adlı yeni bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a2e70-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="a2e70-113">*SignalR. js* dosyasını *wwwroot\lib\signalr* klasörüne kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="a2e70-114">NPM *node_modules\\@aspnet\signalr\dist\browser* klasöre paket içeriğini yüklüyor.</span><span class="sxs-lookup"><span data-stu-id="a2e70-114">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="a2e70-115">*Wwwroot\\lib* klasörü altında *SignalR* adlı yeni bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a2e70-115">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="a2e70-116">*SignalR. js* dosyasını *wwwroot\lib\signalr* klasörüne kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-116">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a><span data-ttu-id="a2e70-117">SignalR JavaScript istemcisini kullanma</span><span class="sxs-lookup"><span data-stu-id="a2e70-117">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="a2e70-118">`<script>` öğesinde JavaScript istemcisine SignalR başvurun.</span><span class="sxs-lookup"><span data-stu-id="a2e70-118">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="a2e70-119">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="a2e70-119">Connect to a hub</span></span>

<span data-ttu-id="a2e70-120">Aşağıdaki kod oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-120">The following code creates and starts a connection.</span></span> <span data-ttu-id="a2e70-121">Hub'ının adı büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-121">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="a2e70-122">Çıkış noktaları arası bağlantılar</span><span class="sxs-lookup"><span data-stu-id="a2e70-122">Cross-origin connections</span></span>

<span data-ttu-id="a2e70-123">Genellikle, tarayıcılar istenen sayfa aynı etki alanında bağlantıları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="a2e70-123">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="a2e70-124">Ancak, başka bir etki alanı bağlantı gerekli olduğunda durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-124">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="a2e70-125">Kötü amaçlı bir sitenin başka bir siteden hassas verileri okumasını engellemek için, [Çıkış](xref:security/cors) noktaları varsayılan olarak devre dışı bırakılır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-125">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="a2e70-126">Bir çapraz kaynak isteğine izin vermek için `Startup` sınıfında etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="a2e70-126">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="a2e70-127">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="a2e70-127">Call hub methods from client</span></span>

<span data-ttu-id="a2e70-128">JavaScript istemcileri, [Hubconnection](/javascript/api/%40aspnet/signalr/hubconnection)'un [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) yöntemi aracılığıyla hub 'larda ortak yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-128">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="a2e70-129">`invoke` yöntemi iki bağımsız değişkeni kabul eder:</span><span class="sxs-lookup"><span data-stu-id="a2e70-129">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="a2e70-130">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="a2e70-130">The name of the hub method.</span></span> <span data-ttu-id="a2e70-131">Aşağıdaki örnekte, hub 'daki Yöntem adı `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-131">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="a2e70-132">Hub yönteminin içinde tanımlanan tüm bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="a2e70-132">Any arguments defined in the hub method.</span></span> <span data-ttu-id="a2e70-133">Aşağıdaki örnekte, bağımsız değişken adı `message`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-133">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="a2e70-134">Örnek kod, Internet Explorer hariç tüm büyük tarayıcıların güncel sürümlerinde desteklenen ok işlev sözdizimini kullanır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-134">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="a2e70-135">Azure SignalR hizmetini *sunucusuz modda*kullanıyorsanız, bir istemciden hub yöntemlerini çağıramezsiniz.</span><span class="sxs-lookup"><span data-stu-id="a2e70-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="a2e70-136">Daha fazla bilgi için [SignalR hizmeti belgelerine](/azure/azure-signalr/signalr-concept-serverless-development-config)bakın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="a2e70-137">`invoke` yöntemi bir JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)döndürür.</span><span class="sxs-lookup"><span data-stu-id="a2e70-137">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="a2e70-138">`Promise`, sunucudaki yöntemi döndürdüğünde döndürülen değer (varsa) ile çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-138">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="a2e70-139">Sunucu üzerindeki yöntem bir hata oluşturursa `Promise` hata iletisiyle reddedilir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-139">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="a2e70-140">Bu durumları (veya `await` sözdizimini) işlemek için `Promise` üzerinde `then` ve `catch` yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-140">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="a2e70-141">`send` yöntemi bir JavaScript `Promise`döndürür.</span><span class="sxs-lookup"><span data-stu-id="a2e70-141">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="a2e70-142">İleti sunucuya gönderildiğinde `Promise` çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-142">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="a2e70-143">İleti gönderilirken bir hata oluşursa `Promise` hata iletisiyle reddedilir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-143">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="a2e70-144">Bu durumları (veya `await` sözdizimini) işlemek için `Promise` üzerinde `then` ve `catch` yöntemlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-144">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="a2e70-145">`send` kullanmak sunucu iletiyi almadan önce beklemez.</span><span class="sxs-lookup"><span data-stu-id="a2e70-145">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="a2e70-146">Sonuç olarak, sunucudan veri veya hata döndürülmesi mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-146">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="a2e70-147">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="a2e70-147">Call client methods from hub</span></span>

<span data-ttu-id="a2e70-148">Hub 'dan ileti almak için `HubConnection`[on](/javascript/api/%40aspnet/signalr/hubconnection#on) metodunu kullanarak bir yöntemi tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-148">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="a2e70-149">JavaScript istemci yöntemin adı.</span><span class="sxs-lookup"><span data-stu-id="a2e70-149">The name of the JavaScript client method.</span></span> <span data-ttu-id="a2e70-150">Aşağıdaki örnekte, yöntem adı `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-150">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="a2e70-151">Bağımsız değişkenleri hub yöntemine geçirir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-151">Arguments the hub passes to the method.</span></span> <span data-ttu-id="a2e70-152">Aşağıdaki örnekte, bağımsız değişken değeri `message`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-152">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="a2e70-153">`connection.on` önceki kod, sunucu tarafı kodu [Sendadsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemi kullanılarak çağırdığında çalışır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-153">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR<span data-ttu-id="a2e70-154">, `SendAsync` ve `connection.on`tanımlanan yöntem adı ve bağımsız değişkenlerle eşleştirerek hangi istemci yönteminin çağrılacağını belirler.</span><span class="sxs-lookup"><span data-stu-id="a2e70-154"> determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="a2e70-155">En iyi uygulama olarak, `on`sonra `HubConnection` [Başlangıç](/javascript/api/%40aspnet/signalr/hubconnection#start) yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-155">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="a2e70-156">İletileri almadan önce İşleyicileriniz kayıtlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2e70-156">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="a2e70-157">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="a2e70-157">Error handling and logging</span></span>

<span data-ttu-id="a2e70-158">İstemci tarafı hatalarını işlemek için `start` yönteminin sonuna bir `catch` yöntemi zincirleyin.</span><span class="sxs-lookup"><span data-stu-id="a2e70-158">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="a2e70-159">Hataları tarayıcı konsoluna çıkarmak için `console.error` kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-159">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="a2e70-160">Bağlantı kurulduğunda oturum için bir Günlükçü ve olay türünü geçirerek istemci tarafı günlük izleme ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-160">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="a2e70-161">Belirtilen günlük düzeyi ve üzeri, iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-161">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="a2e70-162">Kullanılabilir günlük düzeyleri aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="a2e70-162">Available log levels are as follows:</span></span>

* <span data-ttu-id="a2e70-163">&ndash; hata iletileri `signalR.LogLevel.Error`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-163">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="a2e70-164">Yalnızca `Error` iletileri günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a2e70-164">Logs `Error` messages only.</span></span>
* <span data-ttu-id="a2e70-165">olası hatalar hakkında &ndash; uyarı iletileri `signalR.LogLevel.Warning`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-165">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="a2e70-166">`Warning`ve `Error` iletileri günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a2e70-166">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="a2e70-167">hata olmadan &ndash; durum iletileri `signalR.LogLevel.Information`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-167">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="a2e70-168">`Information`, `Warning`ve `Error` iletileri günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a2e70-168">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="a2e70-169">Izleme iletilerini &ndash; `signalR.LogLevel.Trace`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-169">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="a2e70-170">Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a2e70-170">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="a2e70-171">Günlük düzeyini yapılandırmak için [Hubconnectionbuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) üzerinde [configurelogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) yöntemini kullanın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-171">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="a2e70-172">İletileri tarayıcı konsoluna yazılır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-172">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="a2e70-173">İstemcileri yeniden</span><span class="sxs-lookup"><span data-stu-id="a2e70-173">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="a2e70-174">Otomatik olarak yeniden bağlan</span><span class="sxs-lookup"><span data-stu-id="a2e70-174">Automatically reconnect</span></span>

<span data-ttu-id="a2e70-175">SignalR için JavaScript istemcisi, [Hubconnectionbuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)'daki `withAutomaticReconnect` yöntemi kullanılarak otomatik olarak yeniden bağlanacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-175">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="a2e70-176">Varsayılan olarak otomatik olarak yeniden bağlanmaz.</span><span class="sxs-lookup"><span data-stu-id="a2e70-176">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="a2e70-177">Hiçbir parametre olmadan, `withAutomaticReconnect()` her bir yeniden bağlanma denemesini denemeden önce, dört başarısız denemeden sonra durdurmadan, istemciyi 0, 2, 10 ve 30 saniye bekleyecek şekilde yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-177">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="a2e70-178">Yeniden bağlanma girişimlerini başlatmadan önce, `HubConnection` `HubConnectionState.Reconnecting` durumuna geçer ve `onclose` geri çağırmaları `Disconnected`, otomatik yeniden bağlanma yapılandırması olmadan `HubConnection` gibi tetikleyerek `onreconnecting` geri çağırmaları tetikler.</span><span class="sxs-lookup"><span data-stu-id="a2e70-178">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="a2e70-179">Bu, kullanıcıların bağlantının kaybedildiği ve Kullanıcı arabirimi öğelerini devre dışı bırakan kullanıcıları uyarma fırsatı sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2e70-179">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="a2e70-180">İstemci ilk dört denemeden sonra başarıyla yeniden bağlanırsa, `HubConnection` `Connected` duruma geri dönerek `onreconnected` geri çağırmaları harekete geçer.</span><span class="sxs-lookup"><span data-stu-id="a2e70-180">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="a2e70-181">Bu, kullanıcılara bağlantı yeniden kurulmadığını bildirmek için bir fırsat sağlar.</span><span class="sxs-lookup"><span data-stu-id="a2e70-181">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="a2e70-182">Bağlantı sunucuya tamamen yeni göründüğünden, `onreconnected` geri çağırması için yeni bir `connectionId` sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-182">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="a2e70-183">`HubConnection` [anlaşmayı atlayacak](xref:signalr/configuration#configure-client-options)şekilde yapılandırıldıysa, `onreconnected` geri aramanın `connectionId` parametresi tanımsız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-183">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="a2e70-184">`withAutomaticReconnect()`, ilk başlatma başarısızlıklarını yeniden denemek için `HubConnection` yapılandırmaz, bu nedenle başlatma hatalarının el ile işlenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="a2e70-184">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="a2e70-185">İstemci ilk dört denemeden sonra başarıyla yeniden bağlanmazsa, `HubConnection` `Disconnected` durumuna geçer ve [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) geri aramalarını harekete geçirebilir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-185">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="a2e70-186">Bu, kullanıcılara bağlantının kalıcı olarak kaybedilmediğini bildirme ve sayfayı yenilemeyi önerme olanağı sağlar:</span><span class="sxs-lookup"><span data-stu-id="a2e70-186">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="a2e70-187">Bağlantıyı kesmeden veya yeniden bağlanma zamanlamasını değiştirmeden önce özel sayıda yeniden bağlantı girişimi yapılandırmak için `withAutomaticReconnect`, her bir yeniden bağlanma girişimine başlamadan önce beklenecek gecikme süresi temsil eden bir sayı dizisini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="a2e70-187">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="a2e70-188">Yukarıdaki örnek, bağlantı kaybolduktan hemen sonra yeniden bağlanmaya başlamak için `HubConnection` yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-188">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="a2e70-189">Bu, varsayılan yapılandırma için de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-189">This is also true for the default configuration.</span></span>

<span data-ttu-id="a2e70-190">İlk yeniden bağlantı girişimi başarısız olursa, ikinci yeniden bağlanma denemesi de varsayılan yapılandırmada olduğu gibi 2 saniye beklemek yerine hemen başlatılır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-190">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="a2e70-191">İkinci yeniden bağlantı girişimi başarısız olursa, üçüncü yeniden bağlanma denemesi varsayılan yapılandırma gibi 10 saniye içinde başlar.</span><span class="sxs-lookup"><span data-stu-id="a2e70-191">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="a2e70-192">Özel davranış daha sonra varsayılan şekilde yeniden bağlantı kurmayı denemek yerine, üçüncü yeniden bağlantı girişimi başarısızlığından sonra varsayılan davranıştan sonra yeniden bir kez daha fazla yeniden bağlantı kurmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-192">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="a2e70-193">Otomatik yeniden bağlanma denemelerinin zamanlaması ve sayısı üzerinde daha fazla denetime sahip olmak istiyorsanız `withAutomaticReconnect`, `nextRetryDelayInMilliseconds`adlı tek bir yönteme sahip `IRetryPolicy` arabirimini uygulayan bir nesneyi kabul eder.</span><span class="sxs-lookup"><span data-stu-id="a2e70-193">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="a2e70-194">`nextRetryDelayInMilliseconds` `RetryContext`türünde tek bir bağımsız değişken alır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-194">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="a2e70-195">`RetryContext` üç özelliğe sahiptir: `previousRetryCount`, `elapsedMilliseconds` ve `retryReason` `number`, `number` ve `Error`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-195">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="a2e70-196">İlk yeniden bağlanma denemesinden önce, hem `previousRetryCount` hem de `elapsedMilliseconds` sıfır olur ve `retryReason` bağlantının kaybolmasına neden olan hata olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a2e70-196">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="a2e70-197">Her başarısız yeniden deneme denemesinden `previousRetryCount`, `elapsedMilliseconds` bir kez artırılır, bu, şimdiye kadar geçen süre (milisaniye olarak) ve `retryReason` son yeniden bağlanma denemesinin başarısız olmasına neden olacak hata miktarını yansıtacak şekilde güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-197">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="a2e70-198">`nextRetryDelayInMilliseconds`, sonraki yeniden bağlanma girişiminden önce beklenecek milisaniye sayısını temsil eden bir sayı döndürmelidir veya `HubConnection` yeniden bağlamayı durdurması gerekiyorsa `null`.</span><span class="sxs-lookup"><span data-stu-id="a2e70-198">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="a2e70-199">Alternatif olarak, [el ile yeniden bağlanma](#manually-reconnect)bölümünde gösterildiği gibi istemcinizi el ile yeniden bağlayacaksınız.</span><span class="sxs-lookup"><span data-stu-id="a2e70-199">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="a2e70-200">El ile yeniden bağlan</span><span class="sxs-lookup"><span data-stu-id="a2e70-200">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="a2e70-201">3,0 ' den önce SignalR JavaScript istemcisi otomatik olarak yeniden bağlanmaz.</span><span class="sxs-lookup"><span data-stu-id="a2e70-201">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="a2e70-202">İstemcinizi el ile yeniden kod yazmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a2e70-202">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="a2e70-203">Aşağıdaki kod, genel bir el ile yeniden bağlantı yaklaşımını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="a2e70-203">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="a2e70-204">Bir işlev (Bu durumda `start` işlevi) bağlantıyı başlatmak için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a2e70-204">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="a2e70-205">Bağlantının `onclose` olay işleyicisinde `start` işlevini çağırın.</span><span class="sxs-lookup"><span data-stu-id="a2e70-205">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="a2e70-206">Gerçek uygulama bir üstel geri alma kullanın veya ancak bir belirtilen sayıda vazgeçmeden önce yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="a2e70-206">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2e70-207">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a2e70-207">Additional resources</span></span>

* [<span data-ttu-id="a2e70-208">JavaScript API'si başvurusu</span><span class="sxs-lookup"><span data-stu-id="a2e70-208">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="a2e70-209">JavaScript öğreticisi</span><span class="sxs-lookup"><span data-stu-id="a2e70-209">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="a2e70-210">WebPack ve TypeScript öğreticisi</span><span class="sxs-lookup"><span data-stu-id="a2e70-210">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="a2e70-211">Merkezler</span><span class="sxs-lookup"><span data-stu-id="a2e70-211">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="a2e70-212">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="a2e70-212">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="a2e70-213">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="a2e70-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="a2e70-214">Çıkış noktaları arası Istekler (CORS)</span><span class="sxs-lookup"><span data-stu-id="a2e70-214">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* <span data-ttu-id="a2e70-215">[Azure SignalR hizmeti sunucusuz belgeler](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="a2e70-215">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
