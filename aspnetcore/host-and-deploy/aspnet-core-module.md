---
title: "ASP.NET çekirdeği modülü yapılandırma başvurusu"
author: guardrex
description: "ASP.NET Core uygulamaları barındırmak için ASP.NET Core modülü yapılandırmayı öğrenin."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: c01abed767a226eae68725c1c53d922eac2f705e
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2018
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="a9f61-103">ASP.NET çekirdeği modülü yapılandırma başvurusu</span><span class="sxs-lookup"><span data-stu-id="a9f61-103">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="a9f61-104">Tarafından [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="a9f61-104">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="a9f61-105">Bu belge, ASP.NET Core uygulamaları barındırmak için ASP.NET Core Modülü'nü yapılandırma hakkında yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9f61-105">This document provides instructions on how to configure the ASP.NET Core Module for hosting ASP.NET Core apps.</span></span> <span data-ttu-id="a9f61-106">Yükleme yönergeleri ve ASP.NET Core modülü için giriş için bkz: [ASP.NET Core Modülü'ne genel bakış](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a9f61-106">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="a9f61-107">Web.config ile yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a9f61-107">Configuration with web.config</span></span>

<span data-ttu-id="a9f61-108">ASP.NET çekirdeği modülü ile yapılandırılmış `aspNetCore` bölümünü `system.webServer` sitenin düğümünde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="a9f61-108">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="a9f61-109">Aşağıdaki *web.config* dosyası için yayınlanmış bir [framework bağımlı dağıtım](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) ve site isteklerini işlemek için ASP.NET Core Modülü'nü yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="a9f61-109">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet" 
                arguments=".\MyApp.dll" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="a9f61-110">Aşağıdaki *web.config* için yayımlanan bir [müstakil dağıtım](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="a9f61-110">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
                stdoutLogEnabled="false" 
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="a9f61-111">İçin bir uygulama dağıtıldığında [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` yolu ayarlanmış `\\?\%home%\LogFiles\stdout`.</span><span class="sxs-lookup"><span data-stu-id="a9f61-111">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="a9f61-112">Yolun stdout günlüklerine kaydeder *LogFiles* otomatik olarak konumdur klasörü hizmeti tarafından oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="a9f61-112">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="a9f61-113">Bkz: [alt uygulama yapılandırma](xref:host-and-deploy/iis/index#sub-application-configuration) yapılandırmasındaki ilgili önemli bir not için *web.config* alt uygulamaları dosyalarında.</span><span class="sxs-lookup"><span data-stu-id="a9f61-113">See [Sub-application configuration](xref:host-and-deploy/iis/index#sub-application-configuration) for an important note pertaining to the configuration of *web.config* files in sub-apps.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="a9f61-114">AspNetCore öğenin öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="a9f61-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="a9f61-115">Öznitelik</span><span class="sxs-lookup"><span data-stu-id="a9f61-115">Attribute</span></span> | <span data-ttu-id="a9f61-116">Açıklama</span><span class="sxs-lookup"><span data-stu-id="a9f61-116">Description</span></span> | <span data-ttu-id="a9f61-117">Varsayılan</span><span class="sxs-lookup"><span data-stu-id="a9f61-117">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="a9f61-118">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9f61-118">Optional string attribute.</span></span></p><p><span data-ttu-id="a9f61-119">Belirtilen yürütülebilir dosya bağımsız değişkenleri **processPath**.</span><span class="sxs-lookup"><span data-stu-id="a9f61-119">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <span data-ttu-id="a9f61-120">doğru veya yanlış.</span><span class="sxs-lookup"><span data-stu-id="a9f61-120">true or false.</span></span></p><p><span data-ttu-id="a9f61-121">TRUE ise, **502.5 - işlem hatası** sayfa geçersiz kılınır ve 502 durum kod sayfası yapılandırılan *web.config* önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-121">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <span data-ttu-id="a9f61-122">doğru veya yanlış.</span><span class="sxs-lookup"><span data-stu-id="a9f61-122">true or false.</span></span></p><p><span data-ttu-id="a9f61-123">TRUE ise, belirteç istek başına bir üstbilgi 'MS-ASPNETCORE-WINAUTHTOKEN' % ASPNETCORE_PORT % üzerinde dinleme alt işlem iletilir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-123">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="a9f61-124">Bu belirteç istek başına CloseHandle çağırmak için işlem sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-124">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processPath` | <p><span data-ttu-id="a9f61-125">Gerekli dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9f61-125">Required string attribute.</span></span></p><p><span data-ttu-id="a9f61-126">HTTP isteklerini dinlemeye bir işlem başlatır yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="a9f61-126">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="a9f61-127">Göreli yollar desteklenir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-127">Relative paths are supported.</span></span> <span data-ttu-id="a9f61-128">Yol ile başlıyorsa `.`, yolun site köküne göre değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-128">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="a9f61-129">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9f61-129">Optional integer attribute.</span></span></p><p><span data-ttu-id="a9f61-130">Belirtilen işlem sayısını belirtir **processPath** dakikada kilitlenmesine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-130">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="a9f61-131">Bu sınır aşılırsa dakikanın geri kalan işlem başlatma modül durdurur.</span><span class="sxs-lookup"><span data-stu-id="a9f61-131">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | `10` |
| `requestTimeout` | <p><span data-ttu-id="a9f61-132">İsteğe bağlı timespan özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9f61-132">Optional timespan attribute.</span></span></p><p><span data-ttu-id="a9f61-133">ASP.NET çekirdeği Modülü ' % ASPNETCORE_PORT % üzerinde dinleme işleminden yanıt bekleyeceği süreyi belirtir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-133">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="a9f61-134">`requestTimeout` Yalnızca tam dakikalar içinde aksi 2 dakika olarak varsayılan olarak belirtilmelidir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-134">The `requestTimeout` must be specified in whole minutes only, otherwise it defaults to 2 minutes.</span></span></p> | `00:02:00` |
| `shutdownTimeLimit` | <p><span data-ttu-id="a9f61-135">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9f61-135">Optional integer attribute.</span></span></p><p><span data-ttu-id="a9f61-136">Modül düzgün biçimde kapatmak için yürütülebilir dosyası için bekleyeceği saniye cinsinden süre olduğunda *app_offline.htm* dosya algılandı.</span><span class="sxs-lookup"><span data-stu-id="a9f61-136">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | `10` |
| `startupTimeLimit` | <p><span data-ttu-id="a9f61-137">İsteğe bağlı tamsayı özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9f61-137">Optional integer attribute.</span></span></p><p><span data-ttu-id="a9f61-138">Modül yürütülebilir dosyanın bağlantı noktasını dinleyen bir işlemi başlatmak için bekleyeceği saniye cinsinden süre.</span><span class="sxs-lookup"><span data-stu-id="a9f61-138">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="a9f61-139">Bu süre aşılırsa, modül işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-139">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="a9f61-140">Modül yeni bir istek alırsa ve uygulamayı başlatmak gelmedikçe gelen istekler işlemi yeniden başlatmaya devam işlemini yeniden dener **rapidFailsPerMinute** son kez sayısı dakika alınıyor.</span><span class="sxs-lookup"><span data-stu-id="a9f61-140">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p> | `120` |
| `stdoutLogEnabled` | <p><span data-ttu-id="a9f61-141">İsteğe bağlı Boole öznitelik.</span><span class="sxs-lookup"><span data-stu-id="a9f61-141">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="a9f61-142">TRUE ise, **stdout** ve **stderr** belirtilen işlem için **processPath** belirtilen dosyasına yeniden yönlendiriliyor **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="a9f61-142">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="a9f61-143">İsteğe bağlı dize özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9f61-143">Optional string attribute.</span></span></p><p><span data-ttu-id="a9f61-144">Kendisi için göreli veya mutlak dosya yolunu belirtir **stdout** ve **stderr** belirtilen işleminden **processPath** kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-144">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="a9f61-145">Site köküne göre göreli yollardır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-145">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="a9f61-146">İle başlayan herhangi bir yol `.` olan göre sitenin kök ve diğer tüm yolları mutlak yollar olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-146">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="a9f61-147">Yolu sağlanan herhangi bir klasörde günlük dosyası oluşturmak modülün mevcut olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-147">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="a9f61-148">Alt çizgi sınırlayıcıları, zaman damgası, işlem kimliği ve dosya uzantısını kullanarak (*.log*) kesimini son eklenen **stdoutLogFile** yolu.</span><span class="sxs-lookup"><span data-stu-id="a9f61-148">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="a9f61-149">Varsa `.\logs\stdout` sağlanan bir değer olarak, bir örnek stdout günlük olarak kaydedilir *stdout_20180205194132_1934.log* içinde *günlükleri* 2/5/2018 19:41:32 1934 bir işlem kimlikli adresindeki kaydedildiğinde klasör.</span><span class="sxs-lookup"><span data-stu-id="a9f61-149">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="a9f61-150">Ortam değişkenlerini ayarlama</span><span class="sxs-lookup"><span data-stu-id="a9f61-150">Setting environment variables</span></span>

<span data-ttu-id="a9f61-151">Ortam değişkenleri, işlem için belirtilebilir `processPath` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9f61-151">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="a9f61-152">Bir ortam değişkeni ile belirtin `environmentVariable` alt öğesi olan bir `environmentVariables` koleksiyon öğesi.</span><span class="sxs-lookup"><span data-stu-id="a9f61-152">Specify an environment variable with the `environmentVariable` child element of an `environmentVariables` collection element.</span></span> <span data-ttu-id="a9f61-153">Bu bölümde ayarlamak ortam değişkenleri ortam değişkenleri sistem önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-153">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="a9f61-154">Aşağıdaki örnekte, iki ortam değişkenlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="a9f61-154">The following example sets two environment variables.</span></span> <span data-ttu-id="a9f61-155">`ASPNETCORE_ENVIRONMENT` uygulamanın ortamına yapılandırır `Development`.</span><span class="sxs-lookup"><span data-stu-id="a9f61-155">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="a9f61-156">Bir geliştirici geçici olarak bu değer kümesinde *web.config* zorlamak için dosya [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) bir uygulama özel durum hata ayıklama sırasında yüklenecek.</span><span class="sxs-lookup"><span data-stu-id="a9f61-156">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="a9f61-157">`CONFIG_DIR` bir kullanıcı tanımlı ortam değişkeni Geliştirici uygulamanın yapılandırma dosyası yükleme için bir yol oluşturmak için başlangıç değeri okuyan kodu yazıldığı örneğidir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-157">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> <span data-ttu-id="a9f61-158">Yalnızca kümesi `ASPNETCORE_ENVIRONMENT` envirnonment değişkenine `Development` hazırlama ve Internet gibi güvenilmeyen ağlara erişilemez sunucuları test etme.</span><span class="sxs-lookup"><span data-stu-id="a9f61-158">Only set the `ASPNETCORE_ENVIRONMENT` envirnonment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="appofflinehtm"></a><span data-ttu-id="a9f61-159">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="a9f61-159">app_offline.htm</span></span>

<span data-ttu-id="a9f61-160">Bir dosya varsa adıyla *app_offline.htm* algılandığında, bir uygulamanın kök dizininde ASP.NET Core modülü gelen istekleri işlemeyi durdur ve uygulama düzgün biçimde kapatılması çalışır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-160">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="a9f61-161">Uygulama tanımlı saniye sonra hala çalışıyorsa `shutdownTimeLimit`, ASP.NET Core modülü çalışan işlemi sonlandırır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-161">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="a9f61-162">Sırada *app_offline.htm* dosya varsa, ASP.NET Core modülü isteklerine içeriğini göndererek yanıt *app_offline.htm* dosya.</span><span class="sxs-lookup"><span data-stu-id="a9f61-162">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="a9f61-163">Zaman *app_offline.htm* dosya kaldırılır, uygulamayı bir sonraki istekte başlatır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-163">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="a9f61-164">Başlatma hata sayfası</span><span class="sxs-lookup"><span data-stu-id="a9f61-164">Start-up error page</span></span>

<span data-ttu-id="a9f61-165">Arka uç işlem veya arka uç işlemi başlar ancak yapılandırılmış bağlantı noktasında dinleyecek şekilde başarısız başlatmak ASP.NET Core modülü başarısız olursa bir *502.5 işlem hatası* durum kod sayfası görünür.</span><span class="sxs-lookup"><span data-stu-id="a9f61-165">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 Process Failure* status code page appears.</span></span> <span data-ttu-id="a9f61-166">Bu sayfayı bastırmak ve varsayılan IIS 502 durum kod sayfasına dönmek için kullanmak `disableStartUpErrorPage` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="a9f61-166">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="a9f61-167">Özel hata iletileri yapılandırma hakkında daha fazla bilgi için bkz: [HTTP hataları `<httpErrors>` ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="a9f61-167">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

![502.5 işlem hatası durumu kod sayfası](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="a9f61-169">Günlük oluşturma ve yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="a9f61-169">Log creation and redirection</span></span>

<span data-ttu-id="a9f61-170">ASP.NET çekirdeği modülü yönlendiren `stdout` ve `stderr` diske günlükleri `stdoutLogEnabled` ve `stdoutLogFile` özniteliklerini `aspNetCore` öğesi ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-170">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="a9f61-171">Herhangi bir klasörde `stdoutLogFile` yolu günlük dosyası oluşturmak modülün var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-171">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="a9f61-172">Bir zaman damgası ve dosya uzantısı günlük dosyası oluşturulduğunda otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-172">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="a9f61-173">İşlem geri dönüştürme/yeniden başlatma oluşmadığı sürece günlükleri Döndürülmüş değil.</span><span class="sxs-lookup"><span data-stu-id="a9f61-173">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="a9f61-174">Günlükleri tüketen disk alanını sınırla barındırma sorumluluğundadır.</span><span class="sxs-lookup"><span data-stu-id="a9f61-174">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="a9f61-175">Kullanarak `stdout` günlük, uygulama başlatma sorunlarını gidermek için yalnızca önerilir.</span><span class="sxs-lookup"><span data-stu-id="a9f61-175">Using the `stdout` log is only recommended for troubleshooting app startup issues.</span></span> <span data-ttu-id="a9f61-176">Stdout günlük genel uygulama günlüğü amaçlar için kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="a9f61-176">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="a9f61-177">Bir ASP.NET Core uygulamada rutin günlüğü için günlük dosyası boyutu sınırlar ve günlükleri döndüğü bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="a9f61-177">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="a9f61-178">Daha fazla bilgi için bkz: [üçüncü taraf günlüğü sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="a9f61-178">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="a9f61-179">Günlük dosyası adı zaman damgası, işlem kimliği ve dosya uzantısı ekleyerek oluşur (*.log*) son segmenti için `stdoutLogFile` yol (genellikle *stdout*) alt çizgi tarafından ayrılmış.</span><span class="sxs-lookup"><span data-stu-id="a9f61-179">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="a9f61-180">Varsa `stdoutLogFile` yolu bitiyor ile *stdout*, 2/5/2018 19:42: 32'de oluşturulan 1934 PID ile bir uygulama için bir günlük dosyası adına sahip *stdout_20180205194132_1934.log*.</span><span class="sxs-lookup"><span data-stu-id="a9f61-180">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="a9f61-181">Aşağıdaki örnek `aspNetCore` öğesi yapılandırır `stdout` Azure App Service içinde barındırılan bir uygulama için günlüğe kaydetme.</span><span class="sxs-lookup"><span data-stu-id="a9f61-181">The following sample `aspNetCore` element configures `stdout` logging for an app hosted in Azure App Service.</span></span> <span data-ttu-id="a9f61-182">Bir yerel yol veya ağ paylaşım yolu yerel günlüğe kaydetme için kabul edilebilir değil.</span><span class="sxs-lookup"><span data-stu-id="a9f61-182">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="a9f61-183">Uygulama havuzu kullanıcı kimliği sağlanan yolun yazma izni olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a9f61-183">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

<span data-ttu-id="a9f61-184">Bkz: [web.config yapılandırmayla](#configuration-with-webconfig) ilişkin bir örnek `aspNetCore` öğesinde *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="a9f61-184">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the *web.config* file.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="a9f61-185">ASP.NET çekirdeği modülü ile bir IIS paylaşılan yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a9f61-185">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="a9f61-186">ASP.NET çekirdeği Modülü Yükleyicisi ayrıcalıklarıyla çalıştırır **sistem** hesabı.</span><span class="sxs-lookup"><span data-stu-id="a9f61-186">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="a9f61-187">IIS paylaşılan yapılandırması tarafından kullanılan paylaşım yolu için izin sahip yerel sistem hesabı olmayan değiştirmek için bir erişim reddedildi hatası modülü ayarlarını yapılandırmak çalışırken yükleyici isabetler *applicationHost.config* paylaşımda.</span><span class="sxs-lookup"><span data-stu-id="a9f61-187">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer hits an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span> <span data-ttu-id="a9f61-188">IIS paylaşılan yapılandırması kullanırken, aşağıdaki adımları izleyin:</span><span class="sxs-lookup"><span data-stu-id="a9f61-188">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="a9f61-189">IIS paylaşılan yapılandırması devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="a9f61-189">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="a9f61-190">Yükleyiciyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a9f61-190">Run the installer.</span></span>
1. <span data-ttu-id="a9f61-191">Güncelleştirilmiş verme *applicationHost.config* dosya paylaşımına.</span><span class="sxs-lookup"><span data-stu-id="a9f61-191">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="a9f61-192">IIS paylaşılan yapılandırma yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="a9f61-192">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="a9f61-193">Modül sürümü ve Paket Yükleyici günlüklerini barındırma</span><span class="sxs-lookup"><span data-stu-id="a9f61-193">Module version and hosting bundle installer logs</span></span>

<span data-ttu-id="a9f61-194">Yüklü ASP.NET Core modülünün sürümünü belirlemek için:</span><span class="sxs-lookup"><span data-stu-id="a9f61-194">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="a9f61-195">Barındıran sistemde gidin *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="a9f61-195">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="a9f61-196">Bulun *aspnetcore.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="a9f61-196">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="a9f61-197">Dosyaya sağ tıklayın ve seçin **özellikleri** bağlam menüsünde.</span><span class="sxs-lookup"><span data-stu-id="a9f61-197">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="a9f61-198">Seçin **ayrıntıları** sekmesi. **Dosya sürümü** ve **ürün sürümü** modülünün yüklü sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="a9f61-198">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="a9f61-199">Modül için Windows Server'ı barındıran Paket Yükleyici günlüklerini konumunda bulunan *C:\\kullanıcılar\\% UserName %\\AppData\\yerel\\Temp*. Dosya adında *dd_DotNetCoreWinSvrHosting__\<zaman damgası > _000_AspNetCoreModule_x64.log*.</span><span class="sxs-lookup"><span data-stu-id="a9f61-199">The Windows Server Hosting bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="a9f61-200">Modül, şema ve yapılandırmasını dosya konumları</span><span class="sxs-lookup"><span data-stu-id="a9f61-200">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="a9f61-201">Modül</span><span class="sxs-lookup"><span data-stu-id="a9f61-201">Module</span></span>

<span data-ttu-id="a9f61-202">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a9f61-202">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="a9f61-203">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a9f61-203">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="a9f61-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a9f61-204">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="a9f61-205">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="a9f61-205">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="a9f61-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a9f61-206">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="a9f61-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="a9f61-207">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="a9f61-208">Şema</span><span class="sxs-lookup"><span data-stu-id="a9f61-208">Schema</span></span>

<span data-ttu-id="a9f61-209">**IIS**</span><span class="sxs-lookup"><span data-stu-id="a9f61-209">**IIS**</span></span>

   * <span data-ttu-id="a9f61-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="a9f61-210">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="a9f61-211">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a9f61-211">**IIS Express**</span></span>

   * <span data-ttu-id="a9f61-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="a9f61-212">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="a9f61-213">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a9f61-213">Configuration</span></span>

<span data-ttu-id="a9f61-214">**IIS**</span><span class="sxs-lookup"><span data-stu-id="a9f61-214">**IIS**</span></span>

   * <span data-ttu-id="a9f61-215">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a9f61-215">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="a9f61-216">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="a9f61-216">**IIS Express**</span></span>

   * <span data-ttu-id="a9f61-217">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="a9f61-217">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="a9f61-218">Dosyalar için arama yaparak bulunabilir *aspnetcore.dll* içinde *applicationHost.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="a9f61-218">The files can be found by searching for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="a9f61-219">IIS Express, *applicationHost.config* dosyası varsayılan olarak mevcut olmaz.</span><span class="sxs-lookup"><span data-stu-id="a9f61-219">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="a9f61-220">Dosyanın oluşturulma  *\<application_root >\\.vs\\config* Visual Studio çözümünde hiçbir web uygulaması projesi başlatırken.</span><span class="sxs-lookup"><span data-stu-id="a9f61-220">The file is created at *\<application_root>\\.vs\\config* when starting any web app project in the Visual Studio solution.</span></span>
