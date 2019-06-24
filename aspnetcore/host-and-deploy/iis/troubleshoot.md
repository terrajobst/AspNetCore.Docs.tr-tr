---
title: IIS üzerinde ASP.NET Core sorunlarını giderme
author: guardrex
description: Internet Information Services (IIS) ASP.NET Core uygulamaları dağıtımlarına sorunları tanılamayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2019
uid: host-and-deploy/iis/troubleshoot
ms.openlocfilehash: 4df370dd9b1a5a651bcf767b8b9ace4220bdc345
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313652"
---
# <a name="troubleshoot-aspnet-core-on-iis"></a><span data-ttu-id="5420f-103">IIS üzerinde ASP.NET Core sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="5420f-103">Troubleshoot ASP.NET Core on IIS</span></span>

<span data-ttu-id="5420f-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5420f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5420f-105">Bu makalede yönergeler bir ASP.NET Core tanılamak uygulama başlatma çıkış ile barındırırken sağlar [Internet Information Services (IIS)](/iis).</span><span class="sxs-lookup"><span data-stu-id="5420f-105">This article provides instructions on how to diagnose an ASP.NET Core app startup issue when hosting with [Internet Information Services (IIS)](/iis).</span></span> <span data-ttu-id="5420f-106">Bu makaledeki bilgiler, IIS'de Windows Server ve Windows Masaüstü barındırma için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="5420f-106">The information in this article applies to hosting in IIS on Windows Server and Windows Desktop.</span></span>

<span data-ttu-id="5420f-107">Ek sorun giderme konuları:</span><span class="sxs-lookup"><span data-stu-id="5420f-107">Additional troubleshooting topics:</span></span>

* <span data-ttu-id="5420f-108">Azure App Service ayrıca kullanır [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) ve IIS uygulama barındırın.</span><span class="sxs-lookup"><span data-stu-id="5420f-108">Azure App Service also uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and IIS to host apps.</span></span> <span data-ttu-id="5420f-109">Azure App Service'e özellikle ilgili öneriler sorun giderme için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="5420f-109">For troubleshooting advice that pertains specifically to Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>
* <span data-ttu-id="5420f-110"><xref:fundamentals/error-handling> yerel bir sistemde geliştirme sırasında ASP.NET Core uygulamaları hataları işlemek nasıl etkinleştireceğinizi de açıklar.</span><span class="sxs-lookup"><span data-stu-id="5420f-110"><xref:fundamentals/error-handling> covers how to handle errors in ASP.NET Core apps during development on a local system.</span></span>
* <span data-ttu-id="5420f-111">[Visual Studio kullanarak hata ayıklamayı öğrenin](/visualstudio/debugger/getting-started-with-the-debugger) Visual Studio hata ayıklayıcısını özelliklerini tanıtır.</span><span class="sxs-lookup"><span data-stu-id="5420f-111">[Learn to debug using Visual Studio](/visualstudio/debugger/getting-started-with-the-debugger) introduces the features of the Visual Studio debugger.</span></span>
* <span data-ttu-id="5420f-112">[Visual Studio kodu ile hata ayıklama](https://code.visualstudio.com/docs/editor/debugging) Visual Studio Code ile oluşturulmuş hata ayıklama desteğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="5420f-112">[Debugging with Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) describes the debugging support built into Visual Studio Code.</span></span>

[!INCLUDE[](~/includes/azure-iis-startup-errors.md)]

## <a name="troubleshoot-app-startup-errors"></a><span data-ttu-id="5420f-113">Uygulama başlatma hatalarını giderme</span><span class="sxs-lookup"><span data-stu-id="5420f-113">Troubleshoot app startup errors</span></span>

### <a name="enable-the-aspnet-core-module-debug-log"></a><span data-ttu-id="5420f-114">ASP.NET Core modülü hata ayıklama günlüğünü etkinleştir</span><span class="sxs-lookup"><span data-stu-id="5420f-114">Enable the ASP.NET Core Module debug log</span></span>

<span data-ttu-id="5420f-115">Uygulamanın aşağıdaki işleyicisi ayarları ekleyerek *web.config* ASP.NET Core modülü hata ayıklama günlükleri için dosyası:</span><span class="sxs-lookup"><span data-stu-id="5420f-115">Add the following handler settings to the app's *web.config* file to enable ASP.NET Core Module debug logs:</span></span>

```xml
<aspNetCore ...>
  <handlerSettings>
    <handlerSetting name="debugLevel" value="file" />
    <handlerSetting name="debugFile" value="c:\temp\ancm.log" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="5420f-116">Günlüğü için belirtilen yolun var olduğundan ve uygulama havuzu kimliğinin konumuna yazma izinlerine sahip olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5420f-116">Confirm that the path specified for the log exists and that the app pool's identity has write permissions to the location.</span></span>

### <a name="application-event-log"></a><span data-ttu-id="5420f-117">Uygulama olay günlüğü</span><span class="sxs-lookup"><span data-stu-id="5420f-117">Application Event Log</span></span>

<span data-ttu-id="5420f-118">Uygulama olay günlüğüne erişemedi:</span><span class="sxs-lookup"><span data-stu-id="5420f-118">Access the Application Event Log:</span></span>

1. <span data-ttu-id="5420f-119">Başlat menüsünü açın, arama **Olay Görüntüleyicisi'ni**ve ardından **Olay Görüntüleyicisi'ni** uygulama.</span><span class="sxs-lookup"><span data-stu-id="5420f-119">Open the Start menu, search for **Event Viewer**, and then select the **Event Viewer** app.</span></span>
1. <span data-ttu-id="5420f-120">İçinde **Olay Görüntüleyicisi'ni**açın **Windows Günlükleri** düğümü.</span><span class="sxs-lookup"><span data-stu-id="5420f-120">In **Event Viewer**, open the **Windows Logs** node.</span></span>
1. <span data-ttu-id="5420f-121">Seçin **uygulama** uygulama olay günlüğünü açın.</span><span class="sxs-lookup"><span data-stu-id="5420f-121">Select **Application** to open the Application Event Log.</span></span>
1. <span data-ttu-id="5420f-122">Başarısız olan uygulama ile ilişkili hataları arayın.</span><span class="sxs-lookup"><span data-stu-id="5420f-122">Search for errors associated with the failing app.</span></span> <span data-ttu-id="5420f-123">Hata içeren bir değeri *IIS AspNetCore Modülü* veya *IIS Express AspNetCore Modülü* içinde *kaynak* sütun.</span><span class="sxs-lookup"><span data-stu-id="5420f-123">Errors have a value of *IIS AspNetCore Module* or *IIS Express AspNetCore Module* in the *Source* column.</span></span>

### <a name="run-the-app-at-a-command-prompt"></a><span data-ttu-id="5420f-124">Uygulamayı bir komut isteminde aşağıdakini çalıştırın</span><span class="sxs-lookup"><span data-stu-id="5420f-124">Run the app at a command prompt</span></span>

<span data-ttu-id="5420f-125">Başlatma hataları birçok yararlı bilgiler uygulama olay günlüğü'ndeki üretmediği.</span><span class="sxs-lookup"><span data-stu-id="5420f-125">Many startup errors don't produce useful information in the Application Event Log.</span></span> <span data-ttu-id="5420f-126">Bazı hataların nedeni, barındıran sistemde bir komut isteminde uygulamayı çalıştırarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5420f-126">You can find the cause of some errors by running the app at a command prompt on the hosting system.</span></span>

#### <a name="framework-dependent-deployment"></a><span data-ttu-id="5420f-127">Framework bağımlı dağıtım</span><span class="sxs-lookup"><span data-stu-id="5420f-127">Framework-dependent deployment</span></span>

<span data-ttu-id="5420f-128">Uygulama ise bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span><span class="sxs-lookup"><span data-stu-id="5420f-128">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd):</span></span>

1. <span data-ttu-id="5420f-129">Bir komut isteminde, dağıtım klasörüne gidin ve uygulama ile uygulamanın derleme yürüterek çalıştırma *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="5420f-129">At a command prompt, navigate to the deployment folder and run the app by executing the app's assembly with *dotnet.exe*.</span></span> <span data-ttu-id="5420f-130">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `dotnet .\<assembly_name>.dll`.</span><span class="sxs-lookup"><span data-stu-id="5420f-130">In the following command, substitute the name of the app's assembly for \<assembly_name>: `dotnet .\<assembly_name>.dll`.</span></span>
1. <span data-ttu-id="5420f-131">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="5420f-131">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="5420f-132">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="5420f-132">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="5420f-133">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="5420f-133">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="5420f-134">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="5420f-134">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

#### <a name="self-contained-deployment"></a><span data-ttu-id="5420f-135">Kendi içinde dağıtım</span><span class="sxs-lookup"><span data-stu-id="5420f-135">Self-contained deployment</span></span>

<span data-ttu-id="5420f-136">Uygulama ise bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="5420f-136">If the app is a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd):</span></span>

1. <span data-ttu-id="5420f-137">Bir komut isteminde dağıtım klasörüne gidin ve uygulamanın yürütülebilir dosyayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5420f-137">At a command prompt, navigate to the deployment folder and run the app's executable.</span></span> <span data-ttu-id="5420f-138">Aşağıdaki komutta için uygulamanın derleme adı yerine \<assembly_name >: `<assembly_name>.exe`.</span><span class="sxs-lookup"><span data-stu-id="5420f-138">In the following command, substitute the name of the app's assembly for \<assembly_name>: `<assembly_name>.exe`.</span></span>
1. <span data-ttu-id="5420f-139">Konsol çıkışını herhangi bir hata gösteren uygulamadan konsol penceresine yazılır.</span><span class="sxs-lookup"><span data-stu-id="5420f-139">The console output from the app, showing any errors, is written to the console window.</span></span>
1. <span data-ttu-id="5420f-140">Uygulamaya bir istek yaparken, hataları meydana gelirse, burada Kestrel dinlediği bağlantı noktası ve ana bilgisayar için istekte bulunmak.</span><span class="sxs-lookup"><span data-stu-id="5420f-140">If the errors occur when making a request to the app, make a request to the host and port where Kestrel listens.</span></span> <span data-ttu-id="5420f-141">Post ve varsayılan ana bilgisayar kullanarak, istek yaptığınız `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="5420f-141">Using the default host and post, make a request to `http://localhost:5000/`.</span></span> <span data-ttu-id="5420f-142">Uygulamayı, normalde Kestrel uç nokta adresindeki yanıt verirse, sorun barındırma yapılandırmasında ve büyük olasılıkla daha az uygulama içinde ilgili daha yüksektir.</span><span class="sxs-lookup"><span data-stu-id="5420f-142">If the app responds normally at the Kestrel endpoint address, the problem is more likely related to the hosting configuration and less likely within the app.</span></span>

### <a name="aspnet-core-module-stdout-log"></a><span data-ttu-id="5420f-143">ASP.NET Core modülü stdout günlüğü</span><span class="sxs-lookup"><span data-stu-id="5420f-143">ASP.NET Core Module stdout log</span></span>

<span data-ttu-id="5420f-144">Stdout günlükleri görüntülemek ve etkinleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="5420f-144">To enable and view stdout logs:</span></span>

1. <span data-ttu-id="5420f-145">Barındıran sistemde sitenin dağıtım klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="5420f-145">Navigate to the site's deployment folder on the hosting system.</span></span>
1. <span data-ttu-id="5420f-146">Varsa *günlükleri* klasör mevcut değilse, bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5420f-146">If the *logs* folder isn't present, create the folder.</span></span> <span data-ttu-id="5420f-147">Oluşturmak MSBuild'ı etkinleştirme hakkında yönergeler için *günlükleri* otomatik olarak dağıtım klasörüne bakın [dizin yapısı](xref:host-and-deploy/directory-structure) konu.</span><span class="sxs-lookup"><span data-stu-id="5420f-147">For instructions on how to enable MSBuild to create the *logs* folder in the deployment automatically, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>
1. <span data-ttu-id="5420f-148">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="5420f-148">Edit the *web.config* file.</span></span> <span data-ttu-id="5420f-149">Ayarlama **stdoutLogEnabled** için `true` değiştirip **stdoutLogFile** yolu işaret edecek şekilde *günlükleri* klasör (örneğin, `.\logs\stdout`).</span><span class="sxs-lookup"><span data-stu-id="5420f-149">Set **stdoutLogEnabled** to `true` and change the **stdoutLogFile** path to point to the *logs* folder (for example, `.\logs\stdout`).</span></span> <span data-ttu-id="5420f-150">`stdout` Günlük dosyası adı ön eki içinde yoludur.</span><span class="sxs-lookup"><span data-stu-id="5420f-150">`stdout` in the path is the log file name prefix.</span></span> <span data-ttu-id="5420f-151">Oturum oluşturulduğunda bir zaman damgası, işlem kimliği ve dosya uzantısı otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="5420f-151">A timestamp, process id, and file extension are added automatically when the log is created.</span></span> <span data-ttu-id="5420f-152">Kullanarak `stdout` dosya adı ön eki genel günlük dosyası adında *stdout_20180205184032_5412.log*.</span><span class="sxs-lookup"><span data-stu-id="5420f-152">Using `stdout` as the file name prefix, a typical log file is named *stdout_20180205184032_5412.log*.</span></span>
1. <span data-ttu-id="5420f-153">Uygulama havuzunun kimliği için yazma izinlerine sahip olduğundan emin olun *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="5420f-153">Ensure your application pool's identity has write permissions to the *logs* folder.</span></span>
1. <span data-ttu-id="5420f-154">Güncelleştirilmiş Kaydet *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="5420f-154">Save the updated *web.config* file.</span></span>
1. <span data-ttu-id="5420f-155">Uygulamaya bir istek oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5420f-155">Make a request to the app.</span></span>
1. <span data-ttu-id="5420f-156">Gidin *günlükleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="5420f-156">Navigate to the *logs* folder.</span></span> <span data-ttu-id="5420f-157">Bulun ve en son stdout günlüğü'nü açın.</span><span class="sxs-lookup"><span data-stu-id="5420f-157">Find and open the most recent stdout log.</span></span>
1. <span data-ttu-id="5420f-158">Hatalar için günlüğü inceleyin.</span><span class="sxs-lookup"><span data-stu-id="5420f-158">Study the log for errors.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5420f-159">Sorun giderme işlemi tamamlandıktan sonra stdout günlüğü devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="5420f-159">Disable stdout logging when troubleshooting is complete.</span></span>

1. <span data-ttu-id="5420f-160">Düzen *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="5420f-160">Edit the *web.config* file.</span></span>
1. <span data-ttu-id="5420f-161">Ayarlama **stdoutLogEnabled** için `false`.</span><span class="sxs-lookup"><span data-stu-id="5420f-161">Set **stdoutLogEnabled** to `false`.</span></span>
1. <span data-ttu-id="5420f-162">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="5420f-162">Save the file.</span></span>

> [!WARNING]
> <span data-ttu-id="5420f-163">Uygulama veya sunucu başarısızlığı için hata stdout günlüğünü devre dışı bırakmak için yol açabilir.</span><span class="sxs-lookup"><span data-stu-id="5420f-163">Failure to disable the stdout log can lead to app or server failure.</span></span> <span data-ttu-id="5420f-164">Günlük dosyası boyutunu sınırlama yok veya oluşturulan günlük dosyası sayısı yoktur.</span><span class="sxs-lookup"><span data-stu-id="5420f-164">There's no limit on log file size or the number of log files created.</span></span>
>
> <span data-ttu-id="5420f-165">ASP.NET Core uygulamanızı rutin günlüğü için günlük dosyası boyutunu sınırlar ve günlükleri döndürür bir günlük kitaplığını kullanın.</span><span class="sxs-lookup"><span data-stu-id="5420f-165">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="5420f-166">Daha fazla bilgi için [üçüncü taraf günlük sağlayıcıları](xref:fundamentals/logging/index#third-party-logging-providers).</span><span class="sxs-lookup"><span data-stu-id="5420f-166">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

## <a name="enable-the-developer-exception-page"></a><span data-ttu-id="5420f-167">Geliştirici özel durumu sayfasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="5420f-167">Enable the Developer Exception Page</span></span>

<span data-ttu-id="5420f-168">`ASPNETCORE_ENVIRONMENT` [Ortam değişkeni web.config dosyasına eklenebilir](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) uygulama geliştirme ortamında çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="5420f-168">The `ASPNETCORE_ENVIRONMENT` [environment variable can be added to web.config](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) to run the app in the Development environment.</span></span> <span data-ttu-id="5420f-169">Ortama göre uygulama başlangıç kılmadığınız sürece `UseEnvironment` konak Oluşturucusu'ortam değişkenini ayarlayarak sağlar [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling) görünmesini zaman uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5420f-169">As long as the environment isn't overridden in app startup by `UseEnvironment` on the host builder, setting the environment variable allows the [Developer Exception Page](xref:fundamentals/error-handling) to appear when the app is run.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="InProcess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
  </environmentVariables>
</aspNetCore>
```

::: moniker-end

<span data-ttu-id="5420f-170">İçin ortam değişkenini ayarlayarak `ASPNETCORE_ENVIRONMENT` yalnızca Internet'e açık olmayan sunucuları test ve hazırlık kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="5420f-170">Setting the environment variable for `ASPNETCORE_ENVIRONMENT` is only recommended for use on staging and testing servers that aren't exposed to the Internet.</span></span> <span data-ttu-id="5420f-171">Ortam değişkeninden kaldırmak *web.config* sorun giderme sonra dosya.</span><span class="sxs-lookup"><span data-stu-id="5420f-171">Remove the environment variable from the *web.config* file after troubleshooting.</span></span> <span data-ttu-id="5420f-172">Ortam değişkenlerini ayarlama hakkında bilgi *web.config*, bkz: [aspNetCore environmentVariables alt öğesi](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span><span class="sxs-lookup"><span data-stu-id="5420f-172">For information on setting environment variables in *web.config*, see [environmentVariables child element of aspNetCore](xref:host-and-deploy/aspnet-core-module#setting-environment-variables).</span></span>

## <a name="common-startup-errors"></a><span data-ttu-id="5420f-173">Ortak başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="5420f-173">Common startup errors</span></span>

<span data-ttu-id="5420f-174">Bkz. <xref:host-and-deploy/azure-iis-errors-reference>.</span><span class="sxs-lookup"><span data-stu-id="5420f-174">See <xref:host-and-deploy/azure-iis-errors-reference>.</span></span> <span data-ttu-id="5420f-175">Uygulama başlatma önleyen yaygın sorunların çoğunu başvuru konusunda ele alınmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5420f-175">Most of the common problems that prevent app startup are covered in the reference topic.</span></span>

## <a name="obtain-data-from-an-app"></a><span data-ttu-id="5420f-176">Bir uygulamadan veri alın</span><span class="sxs-lookup"><span data-stu-id="5420f-176">Obtain data from an app</span></span>

<span data-ttu-id="5420f-177">Bir uygulama isteklerini yanıtlayabileceği ise, istek, bağlantı ve ek veri terminal satır içi ara yazılımın kullanılması uygulamayı edinin.</span><span class="sxs-lookup"><span data-stu-id="5420f-177">If an app is capable of responding to requests, obtain request, connection, and additional data from the app using terminal inline middleware.</span></span> <span data-ttu-id="5420f-178">Daha fazla bilgi ve örnek kod için bkz. <xref:test/troubleshoot#obtain-data-from-an-app>.</span><span class="sxs-lookup"><span data-stu-id="5420f-178">For more information and sample code, see <xref:test/troubleshoot#obtain-data-from-an-app>.</span></span>

## <a name="create-a-dump"></a><span data-ttu-id="5420f-179">Bir döküm oluşturma</span><span class="sxs-lookup"><span data-stu-id="5420f-179">Create a dump</span></span>

<span data-ttu-id="5420f-180">A *döküm* sistem belleğinin anlık görüntüsüdür ve uygulama kilitlenmesi, başlatma hatası ya da yavaş uygulama nedenini belirlemenize yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="5420f-180">A *dump* is a snapshot of the system's memory and can help determine the cause of an app crash, startup failure, or slow app.</span></span>

### <a name="app-crashes-or-encounters-an-exception"></a><span data-ttu-id="5420f-181">Uygulama kilitlendiğinde veya özel bir durum karşılaştığında</span><span class="sxs-lookup"><span data-stu-id="5420f-181">App crashes or encounters an exception</span></span>

<span data-ttu-id="5420f-182">Almak ve bir dökümü analiz [Windows hata bildirimi (WER)](/windows/desktop/wer/windows-error-reporting):</span><span class="sxs-lookup"><span data-stu-id="5420f-182">Obtain and analyze a dump from [Windows Error Reporting (WER)](/windows/desktop/wer/windows-error-reporting):</span></span>

1. <span data-ttu-id="5420f-183">Kilitlenme döküm dosyaları tutmak için bir klasör oluşturun `c:\dumps`.</span><span class="sxs-lookup"><span data-stu-id="5420f-183">Create a folder to hold crash dump files at `c:\dumps`.</span></span> <span data-ttu-id="5420f-184">Uygulama havuzu klasöre yazma erişimi olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5420f-184">The app pool must have write access to the folder.</span></span>
1. <span data-ttu-id="5420f-185">Çalıştırma [EnableDumps PowerShell Betiği](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="5420f-185">Run the [EnableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/EnableDumps.ps1):</span></span>
   * <span data-ttu-id="5420f-186">Uygulama kullanıyorsa [işlem içi barındırma modeli](xref:host-and-deploy/iis/index#in-process-hosting-model), komut dosyasını Çalıştır *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="5420f-186">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\EnableDumps w3wp.exe c:\dumps
     ```

   * <span data-ttu-id="5420f-187">Uygulama kullanıyorsa [işlem dışı barındırma modeli](xref:host-and-deploy/iis/index#out-of-process-hosting-model), komut dosyasını Çalıştır *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="5420f-187">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\EnableDumps dotnet.exe c:\dumps
     ```

1. <span data-ttu-id="5420f-188">Kilitlenme durumu oluşmasına neden olan koşulları altında uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="5420f-188">Run the app under the conditions that cause the crash to occur.</span></span>
1. <span data-ttu-id="5420f-189">Kilitlenme oluştuktan sonra Çalıştır [DisableDumps PowerShell Betiği](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span><span class="sxs-lookup"><span data-stu-id="5420f-189">After the crash has occurred, run the [DisableDumps PowerShell script](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/host-and-deploy/iis/troubleshoot/scripts/DisableDumps.ps1):</span></span>
   * <span data-ttu-id="5420f-190">Uygulama kullanıyorsa [işlem içi barındırma modeli](xref:host-and-deploy/iis/index#in-process-hosting-model), komut dosyasını Çalıştır *w3wp.exe*:</span><span class="sxs-lookup"><span data-stu-id="5420f-190">If the app uses the [in-process hosting model](xref:host-and-deploy/iis/index#in-process-hosting-model), run the script for *w3wp.exe*:</span></span>

     ```console
     .\DisableDumps w3wp.exe
     ```

   * <span data-ttu-id="5420f-191">Uygulama kullanıyorsa [işlem dışı barındırma modeli](xref:host-and-deploy/iis/index#out-of-process-hosting-model), komut dosyasını Çalıştır *dotnet.exe*:</span><span class="sxs-lookup"><span data-stu-id="5420f-191">If the app uses the [out-of-process hosting model](xref:host-and-deploy/iis/index#out-of-process-hosting-model), run the script for *dotnet.exe*:</span></span>

     ```console
     .\DisableDumps dotnet.exe
     ```

<span data-ttu-id="5420f-192">Bir uygulama kilitlenmelerini ve döküm toplama tamamlandıktan sonra uygulama normal şekilde sona izin verilmez.</span><span class="sxs-lookup"><span data-stu-id="5420f-192">After an app crashes and dump collection is complete, the app is allowed to terminate normally.</span></span> <span data-ttu-id="5420f-193">PowerShell Betiği, uygulama başına en fazla beş dökümleri toplanacak WER yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="5420f-193">The PowerShell script configures WER to collect up to five dumps per app.</span></span>

> [!WARNING]
> <span data-ttu-id="5420f-194">Kilitlenme bilgi dökümleri, çok miktarda disk alanı (en fazla birkaç gigabayt her)'kurmak alabilir.</span><span class="sxs-lookup"><span data-stu-id="5420f-194">Crash dumps might take up a large amount of disk space (up to several gigabytes each).</span></span>

### <a name="app-hangs-fails-during-startup-or-runs-normally"></a><span data-ttu-id="5420f-195">Uygulama yanıt vermemeye başlıyor, başlatma sırasında başarısız olursa veya normal bir şekilde çalışır</span><span class="sxs-lookup"><span data-stu-id="5420f-195">App hangs, fails during startup, or runs normally</span></span>

<span data-ttu-id="5420f-196">Bir uygulama *askıda* (yanıt vermiyor ancak kilitlenme değil), bkz. normalde, başlatma veya çalıştırma sırasında başarısız oluyor [kullanıcı modu dökümü dosyaları: En uygun aracı seçme](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) döküm oluşturmak için uygun bir aracı seçin.</span><span class="sxs-lookup"><span data-stu-id="5420f-196">When an app *hangs* (stops responding but doesn't crash), fails during startup, or runs normally, see [User-Mode Dump Files: Choosing the Best Tool](/windows-hardware/drivers/debugger/user-mode-dump-files#choosing-the-best-tool) to select an appropriate tool to produce the dump.</span></span>

### <a name="analyze-the-dump"></a><span data-ttu-id="5420f-197">Dökümü analiz edin</span><span class="sxs-lookup"><span data-stu-id="5420f-197">Analyze the dump</span></span>

<span data-ttu-id="5420f-198">Bir döküm çeşitli yaklaşımlar kullanılarak çözümlenebilir.</span><span class="sxs-lookup"><span data-stu-id="5420f-198">A dump can be analyzed using several approaches.</span></span> <span data-ttu-id="5420f-199">Daha fazla bilgi için [bir kullanıcı modu bilgi döküm dosyası çözümleme](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span><span class="sxs-lookup"><span data-stu-id="5420f-199">For more information, see [Analyzing a User-Mode Dump File](/windows-hardware/drivers/debugger/analyzing-a-user-mode-dump-file).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="5420f-200">Uzaktan hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="5420f-200">Remote debugging</span></span>

<span data-ttu-id="5420f-201">Bkz: [Visual Studio 2017'de uzak bir IIS bilgisayarda uzaktan hata ayıklama ASP.NET Core](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) Visual Studio belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="5420f-201">See [Remote Debug ASP.NET Core on a Remote IIS Computer in Visual Studio 2017](/visualstudio/debugger/remote-debugging-aspnet-on-a-remote-iis-computer) in the Visual Studio documentation.</span></span>

## <a name="application-insights"></a><span data-ttu-id="5420f-202">Application Insights</span><span class="sxs-lookup"><span data-stu-id="5420f-202">Application Insights</span></span>

<span data-ttu-id="5420f-203">[Application Insights](/azure/application-insights/) tan alınan telemetri verilerini günlüğe kaydetme ve raporlama özellikleri hata dahil olmak üzere, IIS tarafından barındırılan uygulamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="5420f-203">[Application Insights](/azure/application-insights/) provides telemetry from apps hosted by IIS, including error logging and reporting features.</span></span> <span data-ttu-id="5420f-204">Application Insights, yalnızca uygulamanın günlüğe kaydetme özelliklerini kullanılabilir olduğunda uygulama başladıktan sonra gerçekleşen hataları üzerinde rapor edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5420f-204">Application Insights can only report on errors that occur after the app starts when the app's logging features become available.</span></span> <span data-ttu-id="5420f-205">Daha fazla bilgi için [ASP.NET Core için Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="5420f-205">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="additional-advice"></a><span data-ttu-id="5420f-206">Ek öneriler</span><span class="sxs-lookup"><span data-stu-id="5420f-206">Additional advice</span></span>

<span data-ttu-id="5420f-207">Bazen ya da .NET Core SDK içinde uygulama geliştirme makine ya da paket sürümleri üzerinde hemen yükselttikten sonra çalışan bir uygulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="5420f-207">Sometimes a functioning app fails immediately after upgrading either the .NET Core SDK on the development machine or package versions within the app.</span></span> <span data-ttu-id="5420f-208">Bazı durumlarda, ana yükseltme yaparken, bir uygulama tutarsız paketleri kesilebilir.</span><span class="sxs-lookup"><span data-stu-id="5420f-208">In some cases, incoherent packages may break an app when performing major upgrades.</span></span> <span data-ttu-id="5420f-209">Bu sorunların çoğu, bu yönergeleri izleyerek düzeltilebilir:</span><span class="sxs-lookup"><span data-stu-id="5420f-209">Most of these issues can be fixed by following these instructions:</span></span>

1. <span data-ttu-id="5420f-210">Silme *bin* ve *obj* klasörleri.</span><span class="sxs-lookup"><span data-stu-id="5420f-210">Delete the *bin* and *obj* folders.</span></span>
1. <span data-ttu-id="5420f-211">Temizleyin, paketin önbelleğe *% USERPROFILE %\\.nuget\\paketleri* ve *% LocalAppData %\\Nuget\\v3 önbellek*.</span><span class="sxs-lookup"><span data-stu-id="5420f-211">Clear the package caches at *%UserProfile%\\.nuget\\packages* and *%LocalAppData%\\Nuget\\v3-cache*.</span></span>
1. <span data-ttu-id="5420f-212">Geri yükle ve projeyi yeniden derleyin.</span><span class="sxs-lookup"><span data-stu-id="5420f-212">Restore and rebuild the project.</span></span>
1. <span data-ttu-id="5420f-213">Önceki dağıtım sunucu üzerinde tamamen uygulama dağıtarak önce silindiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5420f-213">Confirm that the prior deployment on the server has been completely deleted prior to redeploying the app.</span></span>

> [!TIP]
> <span data-ttu-id="5420f-214">Yürütmek için paket önbellekleri temizlemek için kullanışlı bir yol olan `dotnet nuget locals all --clear` bir komut isteminden.</span><span class="sxs-lookup"><span data-stu-id="5420f-214">A convenient way to clear package caches is to execute `dotnet nuget locals all --clear` from a command prompt.</span></span>
>
> <span data-ttu-id="5420f-215">Paket önbelleklerini temizleyerek da elde edilebilir kullanarak [nuget.exe](https://www.nuget.org/downloads) araç ve komut yürütme `nuget locals all -clear`.</span><span class="sxs-lookup"><span data-stu-id="5420f-215">Clearing package caches can also be accomplished by using the [nuget.exe](https://www.nuget.org/downloads) tool and executing the command `nuget locals all -clear`.</span></span> <span data-ttu-id="5420f-216">*nuget.exe* Windows masaüstü işletim sistemi ile birlikte gelen bir yükleme değildir ve gelen ayrı olarak edinilmelidir [NuGet Web sitesi](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="5420f-216">*nuget.exe* isn't a bundled install with the Windows desktop operating system and must be obtained separately from the [NuGet website](https://www.nuget.org/downloads).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5420f-217">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5420f-217">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:fundamentals/error-handling>
* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/azure-apps/troubleshoot>
