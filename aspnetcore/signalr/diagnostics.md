---
title: ASP.NET Core SignalR günlüğe kaydetme ve tanılama
author: anurse
description: ASP.NET Core SignalR uygulamanızdan tanılamayı nasıl toplayacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/diagnostics
ms.openlocfilehash: c5bd2ac27f8ca486b0d75aed8439747f72448625
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963854"
---
# <a name="logging-and-diagnostics-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="5ac2c-103">ASP.NET Core SignalR günlüğe kaydetme ve tanılama</span><span class="sxs-lookup"><span data-stu-id="5ac2c-103">Logging and diagnostics in ASP.NET Core SignalR</span></span>

<span data-ttu-id="5ac2c-104">, [Andrew Stanton-nurte](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="5ac2c-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="5ac2c-105">Bu makalede, sorunları gidermeye yardımcı olmak için ASP.NET Core SignalR uygulamanızdan tanılama toplamaya yönelik rehberlik sunulmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-105">This article provides guidance for gathering diagnostics from your ASP.NET Core SignalR app to help troubleshoot issues.</span></span>

## <a name="server-side-logging"></a><span data-ttu-id="5ac2c-106">Sunucu tarafında günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="5ac2c-106">Server-side logging</span></span>

> [!WARNING]
> <span data-ttu-id="5ac2c-107">Sunucu tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="5ac2c-108">Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="5ac2c-109">SignalR ASP.NET Core bir parçası olduğundan, ASP.NET Core günlük sistemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-109">Since SignalR is part of ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="5ac2c-110">Varsayılan yapılandırmada SignalR çok az bilgi günlüğe kaydedilir, ancak bu yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-110">In the default configuration, SignalR logs very little information, but this can configured.</span></span> <span data-ttu-id="5ac2c-111">ASP.NET Core günlüğü yapılandırma hakkında ayrıntılar için [ASP.NET Core günlüğe kaydetme](xref:fundamentals/logging/index#configuration) hakkındaki belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

SignalR<span data-ttu-id="5ac2c-112"> iki günlükçü kategorisi kullanır:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-112"> uses two logger categories:</span></span>

* <span data-ttu-id="5ac2c-113">Merkez protokolleriyle ilgili Günlükler için `Microsoft.AspNetCore.SignalR`, hub 'Ları etkinleştirme, yöntemleri çağırma ve hub ile ilgili diğer etkinlikler için &ndash;.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-113">`Microsoft.AspNetCore.SignalR` &ndash; for logs related to Hub Protocols, activating Hubs, invoking methods, and other Hub-related activities.</span></span>
* <span data-ttu-id="5ac2c-114">WebSockets, uzun yoklama ve sunucu tarafından gönderilen olaylar ve alt düzey SignalR altyapısı gibi aktarımlarıyla ilgili Günlükler için `Microsoft.AspNetCore.Http.Connections` &ndash;.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-114">`Microsoft.AspNetCore.Http.Connections` &ndash; for logs related to transports such as WebSockets, Long Polling and Server-Sent Events and low-level SignalR infrastructure.</span></span>

<span data-ttu-id="5ac2c-115">SignalRayrıntılı günlükleri etkinleştirmek için, aşağıdaki öğeleri `Logging``LogLevel` alt bölümüne ekleyerek, yukarıdaki ön ekleri *appSettings. JSON* dosyanızdaki `Debug` düzeyine yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-115">To enable detailed logs from SignalR, configure both of the preceding prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

<span data-ttu-id="5ac2c-116">Ayrıca, `CreateWebHostBuilder` yöntemdeki kodda de yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-116">You can also configure this in code in your `CreateWebHostBuilder` method:</span></span>

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

<span data-ttu-id="5ac2c-117">JSON tabanlı yapılandırma kullanmıyorsanız, yapılandırma sisteminizde aşağıdaki yapılandırma değerlerini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-117">If you aren't using JSON-based configuration, set the following configuration values in your configuration system:</span></span>

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

<span data-ttu-id="5ac2c-118">İç içe yapılandırma değerlerinin nasıl belirleneceğini belirlemek için yapılandırma sisteminizin belgelerini denetleyin.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-118">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="5ac2c-119">Örneğin, ortam değişkenlerini kullanırken, `:` yerine iki `_` karakter kullanılır (örneğin, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span><span class="sxs-lookup"><span data-stu-id="5ac2c-119">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).</span></span>

<span data-ttu-id="5ac2c-120">Uygulamanız için daha ayrıntılı tanılama toplanırken `Debug` düzeyinin kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-120">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="5ac2c-121">`Trace` düzeyi çok düşük düzey Tanılamalar üretir ve uygulamanızdaki sorunları tanılamak için nadiren gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-121">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

## <a name="access-server-side-logs"></a><span data-ttu-id="5ac2c-122">Sunucu tarafı günlüklerine erişin</span><span class="sxs-lookup"><span data-stu-id="5ac2c-122">Access server-side logs</span></span>

<span data-ttu-id="5ac2c-123">Sunucu tarafı günlüklerine erişme, çalıştırdığınız ortama bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app-outside-iis"></a><span data-ttu-id="5ac2c-124">IIS dışında bir konsol uygulaması olarak</span><span class="sxs-lookup"><span data-stu-id="5ac2c-124">As a console app outside IIS</span></span>

<span data-ttu-id="5ac2c-125">Konsol uygulamasında çalıştırıyorsanız, [konsol günlükçüsü](xref:fundamentals/logging/index#console-provider) varsayılan olarak etkinleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> SignalR<span data-ttu-id="5ac2c-126"> Günlükler konsolunda görünür.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-126"> logs will appear in the console.</span></span>

### <a name="within-iis-express-from-visual-studio"></a><span data-ttu-id="5ac2c-127">Visual Studio 'dan IIS Express içinde</span><span class="sxs-lookup"><span data-stu-id="5ac2c-127">Within IIS Express from Visual Studio</span></span>

<span data-ttu-id="5ac2c-128">Visual Studio **çıktı** penceresinde günlük çıktısını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-128">Visual Studio displays the log output in the **Output** window.</span></span> <span data-ttu-id="5ac2c-129">**ASP.NET Core Web sunucusu** açılır seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-129">Select the **ASP.NET Core Web Server** drop down option.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="5ac2c-130">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="5ac2c-130">Azure App Service</span></span>

<span data-ttu-id="5ac2c-131">Azure App Service portalının **tanılama günlükleri** bölümünde **uygulama günlüğü (dosya sistemi)** seçeneğini etkinleştirin ve **düzeyi** `Verbose`olarak yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-131">Enable the **Application Logging (Filesystem)** option in the **Diagnostics logs** section of the Azure App Service portal and configure the **Level** to `Verbose`.</span></span> <span data-ttu-id="5ac2c-132">Günlükler **günlük akış** hizmetinden ve App Service dosya sistemindeki günlüklerde kullanılabilir olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-132">Logs should be available from the **Log streaming** service and in logs on the file system of the App Service.</span></span> <span data-ttu-id="5ac2c-133">Daha fazla bilgi için bkz. [Azure günlük akışı](xref:fundamentals/logging/index#azure-log-streaming).</span><span class="sxs-lookup"><span data-stu-id="5ac2c-133">For more information, see [Azure log streaming](xref:fundamentals/logging/index#azure-log-streaming).</span></span>

### <a name="other-environments"></a><span data-ttu-id="5ac2c-134">Diğer ortamlar</span><span class="sxs-lookup"><span data-stu-id="5ac2c-134">Other environments</span></span>

<span data-ttu-id="5ac2c-135">Uygulama başka bir ortama (örneğin, Docker, Kubernetes veya Windows hizmeti) dağıtılırsa, ortama uygun günlük sağlayıcılarının nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-135">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="javascript-client-logging"></a><span data-ttu-id="5ac2c-136">JavaScript istemci günlüğü</span><span class="sxs-lookup"><span data-stu-id="5ac2c-136">JavaScript client logging</span></span>

> [!WARNING]
> <span data-ttu-id="5ac2c-137">İstemci tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-137">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="5ac2c-138">Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-138">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="5ac2c-139">JavaScript istemcisini kullanırken, `HubConnectionBuilder``configureLogging` yöntemi kullanarak günlüğe kaydetme seçeneklerini yapılandırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-139">When using the JavaScript client, you can configure logging options using the `configureLogging` method on `HubConnectionBuilder`:</span></span>

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

<span data-ttu-id="5ac2c-140">Günlüğe kaydetmeyi tamamen devre dışı bırakmak için `configureLogging` yönteminde `signalR.LogLevel.None` belirtin.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-140">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="5ac2c-141">Aşağıdaki tabloda JavaScript istemcisi için kullanılabilir olan günlük düzeyleri gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-141">The following table shows log levels available to the JavaScript client.</span></span> <span data-ttu-id="5ac2c-142">Günlük düzeyinin bu değerlerden birine ayarlanması, bu düzeyde ve tabloda üzerindeki tüm düzeylerde günlüğe kaydetmeyi sağlar.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-142">Setting the log level to one of these values enables logging at that level and all levels above it in the table.</span></span>

| <span data-ttu-id="5ac2c-143">Düzey</span><span class="sxs-lookup"><span data-stu-id="5ac2c-143">Level</span></span> | <span data-ttu-id="5ac2c-144">Açıklama</span><span class="sxs-lookup"><span data-stu-id="5ac2c-144">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="5ac2c-145">Hiçbir ileti günlüğe kaydedilmez.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-145">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="5ac2c-146">Uygulamanın tamamında bir hata olduğunu gösteren mesajlar.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-146">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="5ac2c-147">Geçerli işlemdeki bir hatayı gösteren mesajlar.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-147">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="5ac2c-148">Önemli olmayan bir sorunu belirten mesajlar.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-148">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="5ac2c-149">Bilgi iletileri.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-149">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="5ac2c-150">Tanılama iletileri hata ayıklama için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-150">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="5ac2c-151">Belirli sorunları tanılamak için tasarlanan çok ayrıntılı tanılama iletileri.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-151">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

<span data-ttu-id="5ac2c-152">Ayrıntı düzeyini yapılandırdıktan sonra, Günlükler tarayıcı konsoluna yazılır (veya bir NodeJS uygulamasında standart çıkış).</span><span class="sxs-lookup"><span data-stu-id="5ac2c-152">Once you've configured the verbosity, the logs will be written to the Browser Console (or Standard Output in a NodeJS app).</span></span>

<span data-ttu-id="5ac2c-153">Günlükleri özel bir günlüğe kaydetme sistemine göndermek istiyorsanız, `ILogger` arabirimini uygulayan bir JavaScript nesnesi sağlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-153">If you want to send logs to a custom logging system, you can provide a JavaScript object implementing the `ILogger` interface.</span></span> <span data-ttu-id="5ac2c-154">Uygulanması gereken tek yöntem, olayın düzeyini ve olayla ilişkili iletiyi alan `log`.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-154">The only method that needs to be implemented is `log`, which takes the level of the event and the message associated with the event.</span></span> <span data-ttu-id="5ac2c-155">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-155">For example:</span></span>

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a><span data-ttu-id="5ac2c-156">.NET istemci günlüğü</span><span class="sxs-lookup"><span data-stu-id="5ac2c-156">.NET client logging</span></span>

> [!WARNING]
> <span data-ttu-id="5ac2c-157">İstemci tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-157">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="5ac2c-158">Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-158">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="5ac2c-159">.NET istemcisinden günlükleri almak için `HubConnectionBuilder``ConfigureLogging` yöntemi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-159">To get logs from the .NET client, you can use the `ConfigureLogging` method on `HubConnectionBuilder`.</span></span> <span data-ttu-id="5ac2c-160">Bu, `WebHostBuilder` ve `HostBuilder``ConfigureLogging` yöntemiyle aynı şekilde çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-160">This works the same way as the `ConfigureLogging` method on `WebHostBuilder` and `HostBuilder`.</span></span> <span data-ttu-id="5ac2c-161">ASP.NET Core ' de kullandığınız günlük sağlayıcılarını yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-161">You can configure the same logging providers you use in ASP.NET Core.</span></span> <span data-ttu-id="5ac2c-162">Ancak, bireysel günlük sağlayıcıları için NuGet paketlerini el ile yükleyip etkinleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-162">However, you have to manually install and enable the NuGet packages for the individual logging providers.</span></span>

### <a name="console-logging"></a><span data-ttu-id="5ac2c-163">Konsol günlüğü</span><span class="sxs-lookup"><span data-stu-id="5ac2c-163">Console logging</span></span>

<span data-ttu-id="5ac2c-164">Konsol günlüğünü etkinleştirmek için [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-164">In order to enable Console logging, add the [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) package.</span></span> <span data-ttu-id="5ac2c-165">Ardından, konsol günlükçüsü 'yi yapılandırmak için `AddConsole` yöntemini kullanın:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-165">Then, use the `AddConsole` method to configure the console logger:</span></span>

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a><span data-ttu-id="5ac2c-166">Çıkış penceresi günlüğüne hata ayıkla</span><span class="sxs-lookup"><span data-stu-id="5ac2c-166">Debug output window logging</span></span>

<span data-ttu-id="5ac2c-167">Günlükleri, Visual Studio 'daki **Çıkış** penceresine gitmek için de yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-167">You can also configure logs to go to the **Output** window in Visual Studio.</span></span> <span data-ttu-id="5ac2c-168">[Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) paketini yükleyip `AddDebug` yöntemi kullanın:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-168">Install the [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) package and use the `AddDebug` method:</span></span>

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a><span data-ttu-id="5ac2c-169">Diğer günlüğe kaydetme sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="5ac2c-169">Other logging providers</span></span>

SignalR<span data-ttu-id="5ac2c-170">, Serilog, seq, NLog veya `Microsoft.Extensions.Logging`ile tümleştirilen herhangi bir günlük sistemi gibi diğer günlük sağlayıcılarını destekler.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-170"> supports other logging providers such as Serilog, Seq, NLog, or any other logging system that integrates with `Microsoft.Extensions.Logging`.</span></span> <span data-ttu-id="5ac2c-171">Günlüğe kaydetme sisteminiz bir `ILoggerProvider`sağlıyorsa, bu dosyayı `AddProvider`kaydedebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-171">If your logging system provides an `ILoggerProvider`, you can register it with `AddProvider`:</span></span>

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a><span data-ttu-id="5ac2c-172">Denetim ayrıntı düzeyi</span><span class="sxs-lookup"><span data-stu-id="5ac2c-172">Control verbosity</span></span>

<span data-ttu-id="5ac2c-173">Uygulamanızdaki diğer yerlerden oturum açıyorsanız, varsayılan düzeyin `Debug` olarak değiştirilmesi çok ayrıntılı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-173">If you are logging from other places in your app, changing the default level to `Debug` may be too verbose.</span></span> <span data-ttu-id="5ac2c-174">Kayıt düzeyini SignalR Günlükler için yapılandırmak üzere bir filtre kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-174">You can use a Filter to configure the logging level for SignalR logs.</span></span> <span data-ttu-id="5ac2c-175">Bu, sunucuda olduğu şekilde kodda yapılabilir:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-175">This can be done in code, in much the same way as on the server:</span></span>

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a><span data-ttu-id="5ac2c-176">Ağ izlemeleri</span><span class="sxs-lookup"><span data-stu-id="5ac2c-176">Network traces</span></span>

> [!WARNING]
> <span data-ttu-id="5ac2c-177">Bir ağ izlemesi, uygulamanız tarafından gönderilen her iletinin tam içeriğini içerir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-177">A network trace contains the full contents of every message sent by your app.</span></span> <span data-ttu-id="5ac2c-178">Ham ağ izlemelerini, üretim uygulamalarından GitHub gibi genel forumlara **hiçbir** şekilde yayımlayamazsınız.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-178">**Never** post raw network traces from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="5ac2c-179">Bir sorunla karşılaşırsanız, ağ izleme bazen yararlı olabilecek çok sayıda bilgi sağlayabilir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-179">If you encounter an issue, a network trace can sometimes provide a lot of helpful information.</span></span> <span data-ttu-id="5ac2c-180">Sorun izleyicimizde bir sorun oluşturacaksanız bu özellikle yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-180">This is particularly useful if you're going to file an issue on our issue tracker.</span></span>

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a><span data-ttu-id="5ac2c-181">Fiddler ile ağ izleme toplama (tercih edilen seçenek)</span><span class="sxs-lookup"><span data-stu-id="5ac2c-181">Collect a network trace with Fiddler (preferred option)</span></span>

<span data-ttu-id="5ac2c-182">Bu yöntem tüm uygulamalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-182">This method works for all apps.</span></span>

<span data-ttu-id="5ac2c-183">Fiddler, HTTP izlemelerinin toplanması için çok güçlü bir araçtır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-183">Fiddler is a very powerful tool for collecting HTTP traces.</span></span> <span data-ttu-id="5ac2c-184">[Telerik.com/Fiddler](https://www.telerik.com/fiddler)adresinden yükleyip uygulamayı çalıştırın ve sorunu yeniden oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-184">Install it from [telerik.com/fiddler](https://www.telerik.com/fiddler), launch it, and then run your app and reproduce the issue.</span></span> <span data-ttu-id="5ac2c-185">Fiddler Windows için kullanılabilir ve macOS ve Linux için beta sürümleri mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-185">Fiddler is available for Windows, and there are beta versions for macOS and Linux.</span></span>

<span data-ttu-id="5ac2c-186">HTTPS kullanarak bağlanıyorsanız, Fiddler 'ın HTTPS trafiğinin şifresini çözebilmesini sağlamaya yönelik bazı ek adımlar vardır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-186">If you connect using HTTPS, there are some extra steps to ensure Fiddler can decrypt the HTTPS traffic.</span></span> <span data-ttu-id="5ac2c-187">Daha ayrıntılı bilgi için bkz. [Fiddler belgeleri](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span><span class="sxs-lookup"><span data-stu-id="5ac2c-187">For more details, see the [Fiddler documentation](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).</span></span>

<span data-ttu-id="5ac2c-188">İzlemeyi topladıktan sonra, **dosya** > seçerek izlemeyi dışarı aktarabilirsiniz. bu işlemi, menü çubuğundan **tüm oturumları** > **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-188">Once you've collected the trace, you can export the trace by choosing **File** > **Save** > **All Sessions** from the menu bar.</span></span>

![Fiddler 'tan tüm oturumlar dışarı aktarılıyor](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a><span data-ttu-id="5ac2c-190">Tcpdump ile bir ağ izlemesi toplayın (yalnızca macOS ve Linux)</span><span class="sxs-lookup"><span data-stu-id="5ac2c-190">Collect a network trace with tcpdump (macOS and Linux only)</span></span>

<span data-ttu-id="5ac2c-191">Bu yöntem tüm uygulamalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-191">This method works for all apps.</span></span>

<span data-ttu-id="5ac2c-192">Komut kabuğundan aşağıdaki komutu çalıştırarak, tcpdump kullanarak ham TCP izlemeleri toplayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-192">You can collect raw TCP traces using tcpdump by running the following command from a command shell.</span></span> <span data-ttu-id="5ac2c-193">Bir izin hatası alırsanız, `root` olması veya komutun `sudo` öneki olması gerekebilir:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-193">You may need to be `root` or prefix the command with `sudo` if you get a permissions error:</span></span>

```console
tcpdump -i [interface] -w trace.pcap
```

<span data-ttu-id="5ac2c-194">`[interface]`, yakalamak istediğiniz ağ arabirimiyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-194">Replace `[interface]` with the network interface you wish to capture on.</span></span> <span data-ttu-id="5ac2c-195">Genellikle bu, `/dev/eth0` (Standart Ethernet arabiriminiz için) veya `/dev/lo0` (localhost trafiği için) gibi bir şeydir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-195">Usually, this is something like `/dev/eth0` (for your standard Ethernet interface) or `/dev/lo0` (for localhost traffic).</span></span> <span data-ttu-id="5ac2c-196">Daha fazla bilgi için, ana bilgisayar sisteminizdeki `tcpdump` Man sayfasına bakın.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-196">For more information, see the `tcpdump` man page on your host system.</span></span>

## <a name="collect-a-network-trace-in-the-browser"></a><span data-ttu-id="5ac2c-197">Tarayıcıda bir ağ izlemesi toplayın</span><span class="sxs-lookup"><span data-stu-id="5ac2c-197">Collect a network trace in the browser</span></span>

<span data-ttu-id="5ac2c-198">Bu yöntem yalnızca tarayıcı tabanlı uygulamalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-198">This method only works for browser-based apps.</span></span>

<span data-ttu-id="5ac2c-199">Çoğu tarayıcı Geliştirici Araçları, tarayıcı ve sunucu arasında ağ etkinliğini yakalamanızı sağlayan bir "ağ" sekmesi vardır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-199">Most browser Developer Tools have a "Network" tab that allows you to capture network activity between the browser and the server.</span></span> <span data-ttu-id="5ac2c-200">Ancak, bu izlemeler WebSocket ve sunucu tarafından gönderilen olay iletilerini içermez.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-200">However, these traces don't include WebSocket and Server-Sent Event messages.</span></span> <span data-ttu-id="5ac2c-201">Bu taşımaları kullanıyorsanız, Fiddler veya TcpDump (aşağıda açıklanmıştır) gibi bir araç kullanmak daha iyi bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-201">If you are using those transports, using a tool like Fiddler or TcpDump (described below) is a better approach.</span></span>

### <a name="microsoft-edge-and-internet-explorer"></a><span data-ttu-id="5ac2c-202">Microsoft Edge ve Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="5ac2c-202">Microsoft Edge and Internet Explorer</span></span>

<span data-ttu-id="5ac2c-203">(Yönergeler hem Edge hem de Internet Explorer için aynıdır)</span><span class="sxs-lookup"><span data-stu-id="5ac2c-203">(The instructions are the same for both Edge and Internet Explorer)</span></span>

1. <span data-ttu-id="5ac2c-204">Geliştirici araçlarını açmak için F12 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="5ac2c-204">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="5ac2c-205">Ağ sekmesine tıklayın</span><span class="sxs-lookup"><span data-stu-id="5ac2c-205">Click the Network Tab</span></span>
3. <span data-ttu-id="5ac2c-206">Sayfayı (gerekirse) yenileyin ve sorunu yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="5ac2c-206">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="5ac2c-207">İzlemeyi "HAR" dosyası olarak dışarı aktarmak için araç çubuğundaki Kaydet simgesine tıklayın:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-207">Click the Save icon in the toolbar to export the trace as a "HAR" file:</span></span>

![Microsoft Edge geliştirme araçları Ağ sekmesinde Kaydet simgesi](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a><span data-ttu-id="5ac2c-209">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="5ac2c-209">Google Chrome</span></span>

1. <span data-ttu-id="5ac2c-210">Geliştirici araçlarını açmak için F12 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="5ac2c-210">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="5ac2c-211">Ağ sekmesine tıklayın</span><span class="sxs-lookup"><span data-stu-id="5ac2c-211">Click the Network Tab</span></span>
3. <span data-ttu-id="5ac2c-212">Sayfayı (gerekirse) yenileyin ve sorunu yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="5ac2c-212">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="5ac2c-213">İstek listesinde herhangi bir yere sağ tıklayın ve "İçerikle HAR olarak Kaydet" i seçin:</span><span class="sxs-lookup"><span data-stu-id="5ac2c-213">Right click anywhere in the list of requests and choose "Save as HAR with content":</span></span>

![Google Chrome geliştirme araçları Ağ sekmesinde "Içerik ile HAR olarak Kaydet" seçeneği](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a><span data-ttu-id="5ac2c-215">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="5ac2c-215">Mozilla Firefox</span></span>

1. <span data-ttu-id="5ac2c-216">Geliştirici araçlarını açmak için F12 tuşuna basın</span><span class="sxs-lookup"><span data-stu-id="5ac2c-216">Press F12 to open the Dev Tools</span></span>
2. <span data-ttu-id="5ac2c-217">Ağ sekmesine tıklayın</span><span class="sxs-lookup"><span data-stu-id="5ac2c-217">Click the Network Tab</span></span>
3. <span data-ttu-id="5ac2c-218">Sayfayı (gerekirse) yenileyin ve sorunu yeniden oluşturun</span><span class="sxs-lookup"><span data-stu-id="5ac2c-218">Refresh the page (if needed) and reproduce the problem</span></span>
4. <span data-ttu-id="5ac2c-219">İstek listesinde herhangi bir yere sağ tıklayın ve "tümünü HAR olarak Kaydet" i seçin</span><span class="sxs-lookup"><span data-stu-id="5ac2c-219">Right click anywhere in the list of requests and choose "Save All As HAR"</span></span>

![Mozilla Firefox geliştirme araçları Ağ sekmesinde "tümünü HAR olarak Kaydet" seçeneği](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a><span data-ttu-id="5ac2c-221">GitHub sorunlarına tanılama dosyaları iliştirme</span><span class="sxs-lookup"><span data-stu-id="5ac2c-221">Attach diagnostics files to GitHub issues</span></span>

<span data-ttu-id="5ac2c-222">Tanılama dosyalarını, `.txt` uzantısına sahip olacak şekilde yeniden adlandırarak ve sonra sorunu üzerine sürükleyip bırakarak GitHub sorunlarına iliştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-222">You can attach Diagnostics files to GitHub issues by renaming them so they have a `.txt` extension and then dragging and dropping them on to the issue.</span></span>

> [!NOTE]
> <span data-ttu-id="5ac2c-223">Lütfen günlük dosyalarının veya ağ izlemelerinin içeriğini bir GitHub sorununa yapıştırmayın.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-223">Please don't paste the content of log files or network traces into a GitHub issue.</span></span> <span data-ttu-id="5ac2c-224">Bu Günlükler ve izlemeler oldukça büyük olabilir ve GitHub genellikle bunları keser.</span><span class="sxs-lookup"><span data-stu-id="5ac2c-224">These logs and traces can be quite large, and GitHub usually truncates them.</span></span>

![Günlük dosyalarını bir GitHub sorununa sürükleme](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a><span data-ttu-id="5ac2c-226">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5ac2c-226">Additional resources</span></span>

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
