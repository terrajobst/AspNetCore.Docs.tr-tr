---
title: ASP.NET Core SignalR JavaScript istemcisi
author: tdykstra
description: ASP.NET Core SignalR JavaScript istemcisi genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: c13c41b0344b0c880e842f2799d6ee97bd7fff7e
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095430"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="b6eff-103">ASP.NET Core SignalR JavaScript istemcisi</span><span class="sxs-lookup"><span data-stu-id="b6eff-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="b6eff-104">Tarafından [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="b6eff-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="b6eff-105">ASP.NET Core SignalR JavaScript istemci kitaplığı, geliştiricilerin, sunucu taraflı hub kodu çağırmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="b6eff-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="b6eff-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b6eff-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="b6eff-107">SignalR istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="b6eff-107">Install the SignalR client package</span></span>

<span data-ttu-id="b6eff-108">SignalR JavaScript istemci kitaplığı olarak teslim edilen bir [npm](https://www.npmjs.com/) paket.</span><span class="sxs-lookup"><span data-stu-id="b6eff-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="b6eff-109">Visual Studio kullanıyorsanız, çalıştırma `npm install` gelen **Paket Yöneticisi Konsolu** while kök klasöründe.</span><span class="sxs-lookup"><span data-stu-id="b6eff-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="b6eff-110">Visual Studio Code için komutu çalıştırın **tümleşik Terminalini**.</span><span class="sxs-lookup"><span data-stu-id="b6eff-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="b6eff-111">Npm paket içeriğini yükler *node_modules\\ @aspnet\signalr\dist\browser*  klasör.</span><span class="sxs-lookup"><span data-stu-id="b6eff-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="b6eff-112">Adlı yeni bir klasör oluşturun *signalr* altında *wwwroot\\LIB* klasör.</span><span class="sxs-lookup"><span data-stu-id="b6eff-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="b6eff-113">Kopyalama *signalr.js* dosyasını *wwwroot\lib\signalr* klasör.</span><span class="sxs-lookup"><span data-stu-id="b6eff-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="b6eff-114">SignalR JavaScript istemcisi kullanma</span><span class="sxs-lookup"><span data-stu-id="b6eff-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="b6eff-115">SignalR JavaScript istemci başvuru `<script>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="b6eff-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="b6eff-116">Bir hub'ına bağlama</span><span class="sxs-lookup"><span data-stu-id="b6eff-116">Connect to a hub</span></span>

<span data-ttu-id="b6eff-117">Aşağıdaki kod oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="b6eff-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="b6eff-118">Hub'ının adı büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="b6eff-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="b6eff-119">Çıkış noktaları arası bağlantılar</span><span class="sxs-lookup"><span data-stu-id="b6eff-119">Cross-origin connections</span></span>

<span data-ttu-id="b6eff-120">Genellikle, tarayıcılar istenen sayfa aynı etki alanında bağlantıları yükleyin.</span><span class="sxs-lookup"><span data-stu-id="b6eff-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="b6eff-121">Ancak, başka bir etki alanı bağlantı gerekli olduğunda durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="b6eff-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="b6eff-122">Kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önlemek için [çıkış noktaları arası bağlantıları](xref:security/cors) varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="b6eff-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="b6eff-123">Çıkış noktaları arası istek izin vermek için bunu etkinleştirin `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b6eff-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="b6eff-124">İstemciden hub yöntemlerini çağırma</span><span class="sxs-lookup"><span data-stu-id="b6eff-124">Call hub methods from client</span></span>

<span data-ttu-id="b6eff-125">JavaScript istemcileri aramak ortak yöntemler şirket hub'ları tarafından kullanarak `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="b6eff-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="b6eff-126">`invoke` Yöntemi iki bağımsız değişkeni kabul eder:</span><span class="sxs-lookup"><span data-stu-id="b6eff-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="b6eff-127">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="b6eff-127">The name of the hub method.</span></span> <span data-ttu-id="b6eff-128">Aşağıdaki örnekte, hub addır `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="b6eff-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="b6eff-129">Hub yönteminin içinde tanımlanan tüm bağımsız değişkenler.</span><span class="sxs-lookup"><span data-stu-id="b6eff-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="b6eff-130">Aşağıdaki örnekte, bağımsız değişkeni addır `message`.</span><span class="sxs-lookup"><span data-stu-id="b6eff-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="b6eff-131">İstemci hub'ından yöntemleri çağırma</span><span class="sxs-lookup"><span data-stu-id="b6eff-131">Call client methods from hub</span></span>

<span data-ttu-id="b6eff-132">Hub'ından iletiler almak için bir yöntemi kullanarak tanımlarsınız `connection.on` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b6eff-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="b6eff-133">JavaScript istemci yöntemin adı.</span><span class="sxs-lookup"><span data-stu-id="b6eff-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="b6eff-134">Aşağıdaki örnekte, yöntem adı olan `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="b6eff-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="b6eff-135">Bağımsız değişkenleri hub yöntemine geçirir.</span><span class="sxs-lookup"><span data-stu-id="b6eff-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="b6eff-136">Aşağıdaki örnekte, bağımsız değişken değeri olan `message`.</span><span class="sxs-lookup"><span data-stu-id="b6eff-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="b6eff-137">Önceki kodda `connection.on` sunucu tarafı kod kullanarak çağırdığında çalıştıran `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b6eff-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="b6eff-138">SignalR yöntem adını eşleştirerek çağırmak için hangi istemci yöntemini belirler ve bağımsız değişkenler tanımlanan `SendAsync` ve `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="b6eff-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="b6eff-139">En iyi uygulama, çağrı `connection.start` sonra `connection.on` işleyicilerinizden herhangi iletileri almadan önce kayıtlı olan şekilde.</span><span class="sxs-lookup"><span data-stu-id="b6eff-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="b6eff-140">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="b6eff-140">Error handling and logging</span></span>

<span data-ttu-id="b6eff-141">Zincir bir `catch` sonuna yöntemi `connection.start` istemci tarafı hataları işlemek için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b6eff-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="b6eff-142">Kullanım `console.error` tarayıcının konsola çıktı hataları.</span><span class="sxs-lookup"><span data-stu-id="b6eff-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="b6eff-143">Bağlantı kurulduğunda oturum için bir Günlükçü ve olay türünü geçirerek istemci tarafı günlük izleme ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="b6eff-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="b6eff-144">Belirtilen günlük düzeyi ve üzeri, iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="b6eff-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="b6eff-145">Kullanılabilir günlük düzeyleri aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="b6eff-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="b6eff-146">`signalR.LogLevel.Error` : Hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="b6eff-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="b6eff-147">Günlükleri `Error` yalnızca iletileri.</span><span class="sxs-lookup"><span data-stu-id="b6eff-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="b6eff-148">`signalR.LogLevel.Warning` : Olası hatalar hakkında uyarı iletileri.</span><span class="sxs-lookup"><span data-stu-id="b6eff-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="b6eff-149">Günlükleri `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="b6eff-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="b6eff-150">`signalR.LogLevel.Information` : Hatasız durum iletileri.</span><span class="sxs-lookup"><span data-stu-id="b6eff-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="b6eff-151">Günlükleri `Information`, `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="b6eff-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="b6eff-152">`signalR.LogLevel.Trace` : İzleme iletileri.</span><span class="sxs-lookup"><span data-stu-id="b6eff-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="b6eff-153">Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="b6eff-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="b6eff-154">Kullanım `configureLogging` metodunda `HubConnectionBuilder` günlük düzeyi yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="b6eff-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="b6eff-155">İletileri tarayıcı konsoluna yazılır.</span><span class="sxs-lookup"><span data-stu-id="b6eff-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="b6eff-156">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b6eff-156">Related resources</span></span>

* [<span data-ttu-id="b6eff-157">Merkezler</span><span class="sxs-lookup"><span data-stu-id="b6eff-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="b6eff-158">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="b6eff-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="b6eff-159">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="b6eff-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="b6eff-160">ASP.NET core'da çıkış noktaları arası istekleri (CORS) etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="b6eff-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
