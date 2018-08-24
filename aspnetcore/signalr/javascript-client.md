---
title: ASP.NET Core SignalR JavaScript istemcisi
author: tdykstra
description: ASP.NET Core SignalR JavaScript istemcisi genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 639c30f1d145a3da5e4f5857f32c1b573c1bfce2
ms.sourcegitcommit: 2c158fcfd325cad97ead608a816e525fe3dcf757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2018
ms.locfileid: "41753545"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="5f36c-103">ASP.NET Core SignalR JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="5f36c-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="5f36c-104">Tarafından [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="5f36c-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="5f36c-105">ASP.NET Core SignalR JavaScript istemci kitaplığı, geliştiricilerin, sunucu taraflı hub kodu çağırmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f36c-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="5f36c-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f36c-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="5f36c-107">SignalR istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="5f36c-107">Install the SignalR client package</span></span>

<span data-ttu-id="5f36c-108">SignalR JavaScript istemci kitaplığı olarak teslim edilen bir [npm](https://www.npmjs.com/) paket.</span><span class="sxs-lookup"><span data-stu-id="5f36c-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="5f36c-109">Visual Studio kullanıyorsanız, çalıştırma `npm install` gelen **Paket Yöneticisi Konsolu** while kök klasöründe.</span><span class="sxs-lookup"><span data-stu-id="5f36c-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="5f36c-110">Visual Studio Code için komutu çalıştırın **tümleşik Terminalini**.</span><span class="sxs-lookup"><span data-stu-id="5f36c-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="5f36c-111">npm paket içeriğini yükler *node_modules\\ @aspnet\signalr\dist\browser* klasör.</span><span class="sxs-lookup"><span data-stu-id="5f36c-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="5f36c-112">Adlı yeni bir klasör oluşturun *signalr* altında *wwwroot\\LIB* klasör.</span><span class="sxs-lookup"><span data-stu-id="5f36c-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="5f36c-113">Kopyalama *signalr.js* dosyasını *wwwroot\lib\signalr* klasör.</span><span class="sxs-lookup"><span data-stu-id="5f36c-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="5f36c-114">SignalR JavaScript istemcisi kullanma</span><span class="sxs-lookup"><span data-stu-id="5f36c-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="5f36c-115">SignalR JavaScript istemci başvuru `<script>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5f36c-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="5f36c-116">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="5f36c-116">Connect to a hub</span></span>

<span data-ttu-id="5f36c-117">Aşağıdaki kod oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="5f36c-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="5f36c-118">Hub'ının adı büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="5f36c-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="5f36c-119">Çıkış noktaları arası bağlantılar</span><span class="sxs-lookup"><span data-stu-id="5f36c-119">Cross-origin connections</span></span>

<span data-ttu-id="5f36c-120">Genellikle, tarayıcılar istenen sayfa aynı etki alanında bağlantıları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="5f36c-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="5f36c-121">Ancak, başka bir etki alanı bağlantı gerekli olduğunda durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="5f36c-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="5f36c-122">Kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önlemek için [çıkış noktaları arası bağlantıları](xref:security/cors) varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="5f36c-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="5f36c-123">Çıkış noktaları arası istek izin vermek için bunu etkinleştirin `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5f36c-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="5f36c-124">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="5f36c-124">Call hub methods from client</span></span>

<span data-ttu-id="5f36c-125">JavaScript istemcilerinin şirket hub'ları genel yöntemleri çağırmak [çağırma](/javascript/api/%40aspnet/signalr/hubconnection#invoke) yöntemi [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="5f36c-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="5f36c-126">`invoke` Yöntemi iki bağımsız değişkeni kabul eder:</span><span class="sxs-lookup"><span data-stu-id="5f36c-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="5f36c-127">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="5f36c-127">The name of the hub method.</span></span> <span data-ttu-id="5f36c-128">Aşağıdaki örnekte, yöntem hub'ında addır `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="5f36c-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="5f36c-129">Hub yönteminin içinde tanımlanan tüm bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="5f36c-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="5f36c-130">Aşağıdaki örnekte, bağımsız değişkeni addır `message`.</span><span class="sxs-lookup"><span data-stu-id="5f36c-130">In the following example, the argument name is `message`.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="5f36c-131">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="5f36c-131">Call client methods from hub</span></span>

<span data-ttu-id="5f36c-132">Hub'ından iletiler almak için bir yöntemi kullanarak tanımlarsınız [üzerinde](/javascript/api/%40aspnet/signalr/hubconnection#on) yöntemi `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="5f36c-132">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="5f36c-133">JavaScript istemci yöntemin adı.</span><span class="sxs-lookup"><span data-stu-id="5f36c-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="5f36c-134">Aşağıdaki örnekte, yöntem adı olan `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="5f36c-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="5f36c-135">Bağımsız değişkenleri hub yöntemine geçirir.</span><span class="sxs-lookup"><span data-stu-id="5f36c-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="5f36c-136">Aşağıdaki örnekte, bağımsız değişken değeri olan `message`.</span><span class="sxs-lookup"><span data-stu-id="5f36c-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="5f36c-137">Önceki kodda `connection.on` sunucu tarafı kod kullanarak çağırdığında çalıştıran [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5f36c-137">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="5f36c-138">SignalR yöntem adını eşleştirerek çağırmak için hangi istemci yöntemini belirler ve bağımsız değişkenler tanımlanan `SendAsync` ve `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="5f36c-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="5f36c-139">En iyi uygulama, çağrı [Başlat](/javascript/api/%40aspnet/signalr/hubconnection#start) metodunda `HubConnection` sonra `on`.</span><span class="sxs-lookup"><span data-stu-id="5f36c-139">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="5f36c-140">İletileri almadan önce İşleyicileriniz kayıtlı sağlar.</span><span class="sxs-lookup"><span data-stu-id="5f36c-140">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="5f36c-141">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="5f36c-141">Error handling and logging</span></span>

<span data-ttu-id="5f36c-142">Zincir bir `catch` sonuna yöntemi `start` istemci tarafı hataları işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5f36c-142">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="5f36c-143">Kullanım `console.error` tarayıcının konsola çıktı hataları.</span><span class="sxs-lookup"><span data-stu-id="5f36c-143">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="5f36c-144">Bağlantı kurulduğunda oturum için bir Günlükçü ve olay türünü geçirerek istemci tarafı günlük izleme ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="5f36c-144">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="5f36c-145">Belirtilen günlük düzeyi ve üzeri, iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5f36c-145">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="5f36c-146">Kullanılabilir günlük düzeyleri aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="5f36c-146">Available log levels are as follows:</span></span>

* <span data-ttu-id="5f36c-147">`signalR.LogLevel.Error` &ndash; Hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="5f36c-147">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="5f36c-148">Günlükleri `Error` yalnızca iletileri.</span><span class="sxs-lookup"><span data-stu-id="5f36c-148">Logs `Error` messages only.</span></span>
* <span data-ttu-id="5f36c-149">`signalR.LogLevel.Warning` &ndash; Olası hatalar hakkında uyarı iletileri.</span><span class="sxs-lookup"><span data-stu-id="5f36c-149">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="5f36c-150">Günlükleri `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="5f36c-150">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="5f36c-151">`signalR.LogLevel.Information` &ndash; Hatasız durum iletileri.</span><span class="sxs-lookup"><span data-stu-id="5f36c-151">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="5f36c-152">Günlükleri `Information`, `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="5f36c-152">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="5f36c-153">`signalR.LogLevel.Trace` &ndash; İzleme iletileri.</span><span class="sxs-lookup"><span data-stu-id="5f36c-153">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="5f36c-154">Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="5f36c-154">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="5f36c-155">Kullanım [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metodunda [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) günlük düzeyi yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="5f36c-155">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="5f36c-156">İletileri tarayıcı konsoluna yazılır.</span><span class="sxs-lookup"><span data-stu-id="5f36c-156">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="5f36c-157">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5f36c-157">Related resources</span></span>

* [<span data-ttu-id="5f36c-158">JavaScript API Başvurusu</span><span class="sxs-lookup"><span data-stu-id="5f36c-158">JavaScript API reference</span></span>](/javascript/api/)
* [<span data-ttu-id="5f36c-159">Merkezler</span><span class="sxs-lookup"><span data-stu-id="5f36c-159">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="5f36c-160">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="5f36c-160">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="5f36c-161">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="5f36c-161">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="5f36c-162">ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="5f36c-162">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
