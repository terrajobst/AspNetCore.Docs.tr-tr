---
title: Günlüğe kaydetme ve ASP.NET Core signalr'da tanılama
author: anurse
description: ASP.NET Core SignalR uygulamanızdan tanılama toplama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 06/19/2019
uid: signalr/diagnostics
ms.openlocfilehash: 69dbd057b3dcadeb3ca5d94ede1234530fb447db
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313704"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a><span data-ttu-id="45005-103">Günlüğe kaydetme ve ASP.NET Core signalr'da tanılama</span><span class="sxs-lookup"><span data-stu-id="45005-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="45005-104">Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="45005-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="45005-105">Bu makalede tanılama sorunlarını gidermenize yardımcı olması için ASP.NET Core SignalR uygulamanızdan toplamak için yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="45005-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="45005-106">Sunucu tarafı günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="45005-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="45005-107">Sunucu tarafı günlüklerini uygulamanızdan hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="45005-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="45005-108">**Hiçbir zaman** GitHub gibi genel forumları için üretim uygulamalardan ham günlükleri gönderin.</span><span class="sxs-lookup"><span data-stu-id="45005-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="45005-109">SignalR ASP.NET Core parçası olduğundan, sistem günlüğü ASP.NET Core kullanır.</span><span class="sxs-lookup"><span data-stu-id="45005-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="45005-110">Varsayılan yapılandırmasında, çok az bilgi SignalR kaydeder, ancak bu yapılandırılmış.</span><span class="sxs-lookup"><span data-stu-id="45005-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="45005-111">İlgili belgelere bakın [ASP.NET Core günlüğü](xref:fundamentals/logging/index#configuration) ASP.NET Core günlüğü yapılandırma hakkında ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="45005-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="45005-112">SignalR iki Günlükçü kategoriler kullanır:</span><span class="sxs-lookup"><span data-stu-id="45005-112">SignalR uses two logger categories:</span></span>

* <span data-ttu-id="45005-113">`Microsoft.AspNetCore.SignalR` &ndash; Hub protokollerini ilgili günlükler için yöntemleri ve diğer Hub ilgili etkinlikleri çağırma, hub'ı etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="45005-113">`Microsoft.AspNetCore.SignalR` &ndash; for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="45005-114">`Microsoft.AspNetCore.Http.Connections` &ndash; WebSockets, uzun yoklama ve Server-Sent olayları ve alt düzey SignalR altyapı gibi taşımalar için günlükleri ilgili.</span><span class="sxs-lookup"><span data-stu-id="45005-114">`Microsoft.AspNetCore.Http.Connections` &ndash; for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="45005-115">SignalR öğesinden alınan ayrıntılı günlükleri etkinleştirmek için hem de önceki ön eklerin yapılandırmanız `Debug` düzeyde, *appsettings.json* dosyası aşağıdaki öğelere ekleyerek `LogLevel` alt konusundaki `Logging`:</span><span class="sxs-lookup"><span data-stu-id="45005-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="45005-116">Ayrıca bu kodunda yapılandırabilirsiniz, `CreateWebHostBuilder` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="45005-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="45005-117">JSON tabanlı yapılandırma kullanmıyorsanız, yapılandırma sisteminizde aşağıdaki yapılandırma değerleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="45005-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="45005-118">İç içe geçmiş yapılandırma değerlerini belirtmek nasıl belirlemek yapılandırma sistemi için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="45005-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="45005-119">Örneğin, ortam değişkenlerini kullanarak iki `_` yerine kullanılan karakterler `:` (örneğin, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span><span class="sxs-lookup"><span data-stu-id="45005-119">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="45005-120">Kullanmanızı öneririz `Debug` düzeyinde daha ayrıntılı tanılama için uygulamanızı toplanırken.</span><span class="sxs-lookup"><span data-stu-id="45005-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="45005-121">`Trace` Düzeyi çok düşük düzeyli tanılama üretir ve uygulamanızdaki sorunları tanılamak için nadiren gereklidir.</span><span class="sxs-lookup"><span data-stu-id="45005-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="45005-122">Sunucu tarafı günlüklerine erişme</span><span class="sxs-lookup"><span data-stu-id="45005-122">Access server-side logs</span></span>

<span data-ttu-id="45005-123">Sunucu tarafı günlüklerini nasıl erişmenizi, çalıştırmakta olduğunuz ortamınıza bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="45005-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="45005-124">Bir konsol uygulaması IIS dışında</span><span class="sxs-lookup"><span data-stu-id="45005-124">As a console app outside IIS</span></span>

<span data-ttu-id="45005-125">Bir konsol uygulamasında çalıştırıyorsanız [konsol günlüğe](xref:fundamentals/logging/index#console-provider) varsayılan olarak etkinleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="45005-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="45005-126">SignalR günlükleri konsolunda görünür.</span><span class="sxs-lookup"><span data-stu-id="45005-126">SignalR logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="45005-127">Visual Studio'dan IIS Express içinde</span><span class="sxs-lookup"><span data-stu-id="45005-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="45005-128">Visual Studio içinde günlük çıktısını görüntüler **çıkış** penceresi.</span><span class="sxs-lookup"><span data-stu-id="45005-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="45005-129">Seçin **ASP.NET Core Web sunucusu** seçeneği bırakın.</span><span class="sxs-lookup"><span data-stu-id="45005-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="45005-130">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="45005-130">Azure App Service</span></span>

<span data-ttu-id="45005-131">Etkinleştirme **uygulama günlüğü (dosya sistemi)** seçeneğini **tanılama günlükleri** Azure App Service portalının bölümüne ve yapılandırma **düzeyi** için `Verbose`.</span><span class="sxs-lookup"><span data-stu-id="45005-131">Enable the **Application Logging (Filesystem)** option in the **Diagnostics logs** section of the Azure App Service portal and configure the **Level** to `Verbose`.</span></span> <span data-ttu-id="45005-132">Günlükleri kullanılabilir **günlük akışını** hizmetini ve App Service dosya sistemindeki günlüklerde.</span><span class="sxs-lookup"><span data-stu-id="45005-132">Logs should be available from the **Log streaming** service and in logs on the file system of the App Service.</span></span> <span data-ttu-id="45005-133">Daha fazla bilgi için [Azure günlük akışını](xref:fundamentals/logging/index#azure-log-streaming).</span><span class="sxs-lookup"><span data-stu-id="45005-133">For more information, see [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="45005-134">Diğer ortamlarda</span><span class="sxs-lookup"><span data-stu-id="45005-134">Other environments</span></span>

<span data-ttu-id="45005-135">Başka bir ortama (örneğin, Docker, Kubernetes veya Windows hizmeti) uygulamanın dağıtıldığı olup <xref:fundamentals/logging/index> günlük sağlayıcıları ortam için uygun yapılandırma hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="45005-135">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="45005-136">JavaScript istemci günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="45005-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="45005-137">İstemci tarafı günlüklerini uygulamanızdan hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="45005-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="45005-138">**Hiçbir zaman** GitHub gibi genel forumları için üretim uygulamalardan ham günlükleri gönderin.</span><span class="sxs-lookup"><span data-stu-id="45005-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="45005-139">JavaScript istemcisi kullanılırken kullanarak günlüğe kaydetme seçeneklerini yapılandırabilirsiniz `configureLogging` metodunda `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="45005-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="45005-140">Tamamen günlüğünü devre dışı bırakmanız belirtin `signalR.LogLevel.None` içinde `configureLogging` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="45005-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="45005-141">Aşağıdaki tabloda kullanılabilir günlük düzeyleri için JavaScript istemci gösterir.</span><span class="sxs-lookup"><span data-stu-id="45005-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="45005-142">Günlük düzeyi şu değerlerden birini ayarlamak, günlüğe kaydetme düzeyi ve üzerindeki tüm düzeylerinde tabloda sağlar.</span><span class="sxs-lookup"><span data-stu-id="45005-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="45005-143">Düzey</span><span class="sxs-lookup"><span data-stu-id="45005-143">Level</span></span> | <span data-ttu-id="45005-144">Açıklama</span><span class="sxs-lookup"><span data-stu-id="45005-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="45005-145">Günlüğe ileti kaydedilmedi.</span><span class="sxs-lookup"><span data-stu-id="45005-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="45005-146">Bir uygulamanın tamamında hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="45005-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="45005-147">Geçerli işlem bir hata iletileri.</span><span class="sxs-lookup"><span data-stu-id="45005-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="45005-148">Önemli olmayan bir sorunu işaret eden iletileri.</span><span class="sxs-lookup"><span data-stu-id="45005-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="45005-149">Bilgilendirme iletileri.</span><span class="sxs-lookup"><span data-stu-id="45005-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="45005-150">Tanılama iletileri hata ayıklama için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="45005-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="45005-151">Belirli sorunları tanılamak için tasarlanan çok ayrıntılı tanılama iletileri.</span><span class="sxs-lookup"><span data-stu-id="45005-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="45005-152">Ayrıntı yapılandırdıktan sonra tarayıcı konsolunu (veya bir NodeJS uygulamasında standart çıktı) günlüklere yazılır.</span><span class="sxs-lookup"><span data-stu-id="45005-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="45005-153">Özel günlük sisteme günlükleri göndermek istiyorsanız, bir JavaScript nesnesi uygulayan sağlayabilir `ILogger` arabirimi.</span><span class="sxs-lookup"><span data-stu-id="45005-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="45005-154">Uygulanması gereken tek yöntemdir `log`, olay düzeyi alır ve ileti olay ile ilişkili.</span><span class="sxs-lookup"><span data-stu-id="45005-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="45005-155">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="45005-155">For example:</span></span>

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="45005-156">.NET istemci günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="45005-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="45005-157">İstemci tarafı günlüklerini uygulamanızdan hassas bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="45005-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="45005-158">**Hiçbir zaman** GitHub gibi genel forumları için üretim uygulamalardan ham günlükleri gönderin.</span><span class="sxs-lookup"><span data-stu-id="45005-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="45005-159">.NET İstemci'den günlükleri almak için kullanabileceğiniz `ConfigureLogging` metodunda `HubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="45005-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="45005-160">Bu aynı şekilde çalışır `ConfigureLogging` metodunda `WebHostBuilder` ve `HostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="45005-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="45005-161">ASP.NET Core içinde kullandığınız aynı günlük sağlayıcıları yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45005-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="45005-162">Ancak, el ile yükleyin ve tek tek günlük sağlayıcıları için NuGet paketlerini etkinleştirme gerekir.</span><span class="sxs-lookup"><span data-stu-id="45005-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="45005-163">Konsol günlüğü</span><span class="sxs-lookup"><span data-stu-id="45005-163">Console logging</span></span>

<span data-ttu-id="45005-164">Konsol günlüğü etkinleştirmek için ekleme [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) paket.</span><span class="sxs-lookup"><span data-stu-id="45005-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="45005-165">Ardından, `AddConsole` yöntemi konsol günlüğe yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="45005-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="45005-166">Hata ayıklama çıkış penceresinde günlüğü</span><span class="sxs-lookup"><span data-stu-id="45005-166">Debug output window logging</span></span>

<span data-ttu-id="45005-167">Gitmek için günlükleri de yapılandırabilirsiniz **çıkış** Visual Studio'daki.</span><span class="sxs-lookup"><span data-stu-id="45005-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="45005-168">Yükleme [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) kullanın ve paket `AddDebug` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="45005-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="45005-169">Diğer günlük sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="45005-169">Other logging providers</span></span>

<span data-ttu-id="45005-170">SignalR Serilog, Seq, NLog veya tümleşik şekilde çalışarak, herhangi bir günlük sisteminin gibi diğer günlük sağlayıcılarını destekler `Microsoft.Extensions.Logging`.</span><span class="sxs-lookup"><span data-stu-id="45005-170">SignalR supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="45005-171">Günlüğe kaydetme sisteminizi sağlıyorsa bir `ILoggerProvider`, ile kaydedebilirsiniz `AddProvider`:</span><span class="sxs-lookup"><span data-stu-id="45005-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="45005-172">Denetim ayrıntı düzeyi</span><span class="sxs-lookup"><span data-stu-id="45005-172">Control verbosity</span></span>

<span data-ttu-id="45005-173">Diğer yerlerden uygulamanızda oturum açıyorsanız, varsayılan düzeyini değiştirme `Debug` çok ayrıntılı olabilir.</span><span class="sxs-lookup"><span data-stu-id="45005-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="45005-174">SignalR günlükleri için günlüğe kaydetme düzeyini yapılandırmak için bir filtre kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45005-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="45005-175">Bu kodda, çok sunucuda aynı şekilde gerçekleştirebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="45005-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="45005-176">Ağ izleme</span><span class="sxs-lookup"><span data-stu-id="45005-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="45005-177">Ağ izleme, uygulamanız tarafından gönderilen her ileti tam içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="45005-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="45005-178">**Hiçbir zaman** ham ağ izlerini GitHub gibi genel forumları üretim uygulamalardan postalayabilir.</span><span class="sxs-lookup"><span data-stu-id="45005-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="45005-179">Bir sorunla karşılaşırsanız, ağ izleme bazen birçok yararlı bilgi sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="45005-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="45005-180">Bir sorun bizim sorun İzleyicisi'ni kullanarak dosyaya kullanacaksanız bu özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="45005-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="45005-181">Fiddler'ı (tercih edilen seçenek) ile bir ağ izleme Topla</span><span class="sxs-lookup"><span data-stu-id="45005-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="45005-182">Bu yöntem, tüm uygulamalar için çalışır.</span><span class="sxs-lookup"><span data-stu-id="45005-182">This method works for all apps.</span></span>

<span data-ttu-id="45005-183">Fiddler HTTP izlemeleri toplamak için çok güçlü bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="45005-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="45005-184">Buradan yükleyin [telerik.com/fiddler](https://www.telerik.com/fiddler), başlatın ve ardından uygulamanızı çalıştırın ve sorunu yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="45005-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="45005-185">Windows için fiddler kullanılabilir ve macOS ve Linux için beta sürümleri vardır.</span><span class="sxs-lookup"><span data-stu-id="45005-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="45005-186">HTTPS kullanarak bağlanırsa Fiddler HTTPS trafiği şifresini çözebilir emin olmak için bazı ek adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="45005-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="45005-187">Daha fazla ayrıntı için [Fiddler belgeleri](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span><span class="sxs-lookup"><span data-stu-id="45005-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="45005-188">İzleme derledik sonra seçerek izleme verebilirsiniz **dosya** > **Kaydet** > **tüm oturumları** menü çubuğundan.</span><span class="sxs-lookup"><span data-stu-id="45005-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions** from the menu bar.</span></span>

![Tüm oturumlar Fiddler ' dışarı aktarma](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="45005-190">Bir ağ izleme tcpdump (macOS ve yalnızca Linux) ile toplama</span><span class="sxs-lookup"><span data-stu-id="45005-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="45005-191">Bu yöntem, tüm uygulamalar için çalışır.</span><span class="sxs-lookup"><span data-stu-id="45005-191">This method works for all apps.</span></span>

<span data-ttu-id="45005-192">Bir komut kabuğu'ndan aşağıdaki komutu çalıştırarak tcpdump kullanarak ham TCP izlemeleri toplayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="45005-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="45005-193">Olması gerekebilir `root` veya komutu ile önek `sudo` izin hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="45005-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="45005-194">Değiştirin `[interface]` üzerinde yakalamak istediğiniz ağ arabirimine sahip.</span><span class="sxs-lookup"><span data-stu-id="45005-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="45005-195">Genellikle, bu gibi bir şeydir `/dev/eth0` (için standart, Ethernet arabirimi) ya da `/dev/lo0` (için localhost trafik).</span><span class="sxs-lookup"><span data-stu-id="45005-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="45005-196">Daha fazla bilgi için `tcpdump` ana bilgisayar sisteminizin man sayfasında.</span><span class="sxs-lookup"><span data-stu-id="45005-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="45005-197">Bir ağ izleme tarayıcıda Topla</span><span class="sxs-lookup"><span data-stu-id="45005-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="45005-198">Bu yöntem, yalnızca tarayıcı tabanlı uygulamalar için çalışır.</span><span class="sxs-lookup"><span data-stu-id="45005-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="45005-199">Çoğu tarayıcı geliştirici araçları, tarayıcı ve sunucu arasındaki ağ etkinliği yakalamanıza olanak tanıyan bir "Ağ" sekmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="45005-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="45005-200">Ancak, bu izlemelerin WebSocket ve Server-Sent olay iletileri dahil değildir.</span><span class="sxs-lookup"><span data-stu-id="45005-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="45005-201">Taşımaları kullanıyorsanız, Fiddler veya TcpDump (aşağıda açıklanmıştır) gibi bir araç kullanılması daha iyi bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="45005-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="45005-202">Microsoft Edge ve Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="45005-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="45005-203">(Yönergeleri için hem uç hem de Internet Explorer aynıdır)</span><span class="sxs-lookup"><span data-stu-id="45005-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="45005-204">Geliştirme Araçları'nı açmak için F12 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="45005-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="45005-205">Ağ sekmesine tıklayın</span><span class="sxs-lookup"><span data-stu-id="45005-205">Click the Network Tab</span></span>
3. <span data-ttu-id="45005-206">(Gerekirse) sayfayı yeniler ve sorunu yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="45005-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="45005-207">İzleme "HAR" dosyası olarak dışarı aktarmak için araç çubuğundaki Kaydet simgesine tıklayın:</span><span class="sxs-lookup"><span data-stu-id="45005-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![Kaydet simgesine Microsoft Edge geliştirici araçlarını Ağ sekmesi](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="45005-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="45005-209">Google Chrome</span></span>

1. <span data-ttu-id="45005-210">Geliştirme Araçları'nı açmak için F12 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="45005-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="45005-211">Ağ sekmesine tıklayın</span><span class="sxs-lookup"><span data-stu-id="45005-211">Click the Network Tab</span></span>
3. <span data-ttu-id="45005-212">(Gerekirse) sayfayı yeniler ve sorunu yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="45005-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="45005-213">Sağ istekleri listede herhangi bir yere tıklayın ve seçin "İçerikle HAR olarak kaydetme":</span><span class="sxs-lookup"><span data-stu-id="45005-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Google Chrome geliştirme araçları ağı sekmesini "HAR içeriğe sahip farklı kaydet" seçeneği](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="45005-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="45005-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="45005-216">Geliştirme Araçları'nı açmak için F12 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="45005-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="45005-217">Ağ sekmesine tıklayın</span><span class="sxs-lookup"><span data-stu-id="45005-217">Click the Network Tab</span></span>
3. <span data-ttu-id="45005-218">(Gerekirse) sayfayı yeniler ve sorunu yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="45005-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="45005-219">Sağ istekleri listede herhangi bir yere tıklayın ve "Kaydet tüm olarak HAR" seçin</span><span class="sxs-lookup"><span data-stu-id="45005-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Mozilla Firefox geliştirme araçları ağı sekmesini "Kaydetme tümünü HAR olarak" seçeneği](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="45005-221">GitHub sorunları tanılama dosyaları Ekle</span><span class="sxs-lookup"><span data-stu-id="45005-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="45005-222">GitHub sorunları için sahip oldukları için yeniden adlandırarak Tanılama dosyalarını ekleyebilirsiniz bir `.txt` uzantısı ve ardından sürükleyip bunları sorun açın.</span><span class="sxs-lookup"><span data-stu-id="45005-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="45005-223">Lütfen günlük dosyalarını veya ağ izlerini içeriğini bir GitHub sorunu yapıştırın yok.</span><span class="sxs-lookup"><span data-stu-id="45005-223">Please don't paste the content of log files or network traces into a GitHub issue.</span></span> <span data-ttu-id="45005-224">Bu günlükler ve izlemeler oldukça büyük olabilir ve GitHub genellikle bunları keser.</span><span class="sxs-lookup"><span data-stu-id="45005-224">These logs and traces can be quite large, and GitHub usually truncates them.</span></span>

![Bir GitHub sorunu açın günlük dosyaları sürükleme](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="45005-226">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="45005-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
