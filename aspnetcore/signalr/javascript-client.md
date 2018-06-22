---
title: ASP.NET Core SignalR JavaScript istemci
author: rachelappel
description: ASP.NET Core SignalR JavaScript istemci genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: 9ea12dc21abfef6d2e6f3fcc455d8aa58ecaa011
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271950"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="dce5b-103">ASP.NET Core SignalR JavaScript istemci</span><span class="sxs-lookup"><span data-stu-id="dce5b-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="dce5b-104">Tarafından [Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="dce5b-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="dce5b-105">ASP.NET Core SignalR JavaScript istemci kitaplığını, sunucu taraflı hub kod geliştiricilerin sağlar.</span><span class="sxs-lookup"><span data-stu-id="dce5b-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="dce5b-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="dce5b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="dce5b-107">SignalR istemci paketini yükle</span><span class="sxs-lookup"><span data-stu-id="dce5b-107">Install the SignalR client package</span></span>

<span data-ttu-id="dce5b-108">SignalR JavaScript istemci kitaplığını olarak teslim edilen bir [npm](https://www.npmjs.com/) paket.</span><span class="sxs-lookup"><span data-stu-id="dce5b-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="dce5b-109">Visual Studio kullanıyorsanız, çalıştırmak `npm install` gelen **Paket Yöneticisi Konsolu** while kök klasöründe.</span><span class="sxs-lookup"><span data-stu-id="dce5b-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="dce5b-110">Visual Studio Code komutu Çalıştır **tümleşik Terminal**.</span><span class="sxs-lookup"><span data-stu-id="dce5b-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="dce5b-111">Npm yükler ve paket içeriklerinin *node_modules\\ @aspnet\signalr\dist\browser*  klasör.</span><span class="sxs-lookup"><span data-stu-id="dce5b-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="dce5b-112">Adlı yeni bir klasör oluşturun *signalr* altında *wwwroot\\lib* klasör.</span><span class="sxs-lookup"><span data-stu-id="dce5b-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="dce5b-113">Kopya *signalr.js* dosya *wwwroot\lib\signalr* klasör.</span><span class="sxs-lookup"><span data-stu-id="dce5b-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="dce5b-114">SignalR JavaScript istemci kullanın</span><span class="sxs-lookup"><span data-stu-id="dce5b-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="dce5b-115">SignalR JavaScript istemci başvuru `<script>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="dce5b-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="dce5b-116">Bir hub'a bağlanması</span><span class="sxs-lookup"><span data-stu-id="dce5b-116">Connect to a hub</span></span>

<span data-ttu-id="dce5b-117">Aşağıdaki kod oluşturur ve bir bağlantı başlatır.</span><span class="sxs-lookup"><span data-stu-id="dce5b-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="dce5b-118">Hub'ın adı büyük küçük harfe duyarlı değil.</span><span class="sxs-lookup"><span data-stu-id="dce5b-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="dce5b-119">Çıkış noktaları arası bağlantılar</span><span class="sxs-lookup"><span data-stu-id="dce5b-119">Cross-origin connections</span></span>

<span data-ttu-id="dce5b-120">Genellikle, tarayıcılar, istenen sayfa aynı etki alanından bağlantıları yükler.</span><span class="sxs-lookup"><span data-stu-id="dce5b-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="dce5b-121">Ancak, başka bir etki alanıyla bağlantı gerekli olduğunda durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="dce5b-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="dce5b-122">Kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önlemek için [çıkış noktaları arası bağlantılar](xref:security/cors) varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="dce5b-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="dce5b-123">Çıkış noktaları arası istek izin vermek için içinde etkinleştirmek `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="dce5b-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="dce5b-124">İstemciden hub yöntemlerini çağırın</span><span class="sxs-lookup"><span data-stu-id="dce5b-124">Call hub methods from client</span></span>

<span data-ttu-id="dce5b-125">JavaScript istemcileri genel yöntemler hub tarafından üzerinde kullanarak Çağır `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="dce5b-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="dce5b-126">`invoke` Yöntemi iki bağımsız değişken kabul eder:</span><span class="sxs-lookup"><span data-stu-id="dce5b-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="dce5b-127">Hub yönteminin adı.</span><span class="sxs-lookup"><span data-stu-id="dce5b-127">The name of the hub method.</span></span> <span data-ttu-id="dce5b-128">Aşağıdaki örnekte, hub addır `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="dce5b-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="dce5b-129">Hub yönteminin tanımlanan herhangi bir bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="dce5b-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="dce5b-130">Aşağıdaki örnekte, bağımsız değişkeni addır `message`.</span><span class="sxs-lookup"><span data-stu-id="dce5b-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="dce5b-131">İstemci hub'ından yöntemlerini çağırın</span><span class="sxs-lookup"><span data-stu-id="dce5b-131">Call client methods from hub</span></span>

<span data-ttu-id="dce5b-132">Hub'dan iletileri almak için bir yöntemi kullanarak tanımlarsınız `connection.on` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dce5b-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="dce5b-133">JavaScript istemci yöntemin adı.</span><span class="sxs-lookup"><span data-stu-id="dce5b-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="dce5b-134">Aşağıdaki örnekte, yöntem addır `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="dce5b-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="dce5b-135">Bağımsız değişkenleri hub için yöntem geçirir.</span><span class="sxs-lookup"><span data-stu-id="dce5b-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="dce5b-136">Aşağıdaki örnekte, bağımsız değişken değeri olan `message`.</span><span class="sxs-lookup"><span data-stu-id="dce5b-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="dce5b-137">Önceki kod `connection.on` sunucu tarafı kodu kullanarak çağırdığında çalışır `SendAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="dce5b-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="dce5b-138">SignalR belirler yöntem adı eşleştirerek çağırmak için hangi istemci yöntemi ve bağımsız değişkenler tanımlanan `SendAsync` ve `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="dce5b-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="dce5b-139">En iyi uygulama, arama `connection.start` sonra `connection.on` herhangi bir ileti almadan önce işleyicileri kaydedilir şekilde.</span><span class="sxs-lookup"><span data-stu-id="dce5b-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="dce5b-140">Hata işleme ve günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="dce5b-140">Error handling and logging</span></span>

<span data-ttu-id="dce5b-141">Zincir bir `catch` yöntemi sonuna `connection.start` istemci-tarafı hataları işlemek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="dce5b-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="dce5b-142">Kullanım `console.error` tarayıcının konsola çıktı hataları.</span><span class="sxs-lookup"><span data-stu-id="dce5b-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="dce5b-143">İstemci-tarafı günlük izleme Günlükçü ve olay türü bağlantı yapıldığında oturum geçirerek ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="dce5b-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="dce5b-144">Belirtilen günlük düzeyi ve daha yüksek iletileri günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="dce5b-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="dce5b-145">Kullanılabilir günlük düzeyleri aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="dce5b-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="dce5b-146">`signalR.LogLevel.Error` : Hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="dce5b-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="dce5b-147">Günlükleri `Error` yalnızca iletiler.</span><span class="sxs-lookup"><span data-stu-id="dce5b-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="dce5b-148">`signalR.LogLevel.Warning` : Olası hatalar hakkında uyarı iletileri.</span><span class="sxs-lookup"><span data-stu-id="dce5b-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="dce5b-149">Günlükleri `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="dce5b-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="dce5b-150">`signalR.LogLevel.Information` : Hatasız durum iletileri.</span><span class="sxs-lookup"><span data-stu-id="dce5b-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="dce5b-151">Günlükleri `Information`, `Warning`, ve `Error` iletileri.</span><span class="sxs-lookup"><span data-stu-id="dce5b-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="dce5b-152">`signalR.LogLevel.Trace` : İletileri izleme.</span><span class="sxs-lookup"><span data-stu-id="dce5b-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="dce5b-153">Hub ve istemci arasında taşınan veriler dahil her şeyi günlüğe kaydeder.</span><span class="sxs-lookup"><span data-stu-id="dce5b-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="dce5b-154">Kullanım `configureLogging` yöntemi `HubConnectionBuilder` günlük düzeyini yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="dce5b-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="dce5b-155">İletileri tarayıcı konsoluna yazılır.</span><span class="sxs-lookup"><span data-stu-id="dce5b-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="dce5b-156">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="dce5b-156">Related resources</span></span>

* [<span data-ttu-id="dce5b-157">Merkezler</span><span class="sxs-lookup"><span data-stu-id="dce5b-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="dce5b-158">.NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="dce5b-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="dce5b-159">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="dce5b-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="dce5b-160">ASP.NET Core çıkış noktaları arası istekleri (CORS) etkinleştir</span><span class="sxs-lookup"><span data-stu-id="dce5b-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)