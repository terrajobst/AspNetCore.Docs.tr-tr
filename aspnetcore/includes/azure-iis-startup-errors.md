## <a name="app-startup-errors"></a><span data-ttu-id="85527-101">Uygulama başlatma hataları</span><span class="sxs-lookup"><span data-stu-id="85527-101">App startup errors</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="85527-102">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="85527-102">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="85527-103">A *502.5 - işlem hatası* veya *500.30 - başlangıç hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="85527-103">A *502.5 - Process Failure* or a *500.30 - Start Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="85527-104">Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma.</span><span class="sxs-lookup"><span data-stu-id="85527-104">In Visual Studio, an ASP.NET Core project defaults to [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hosting during debugging.</span></span> <span data-ttu-id="85527-105">A *502.5 işlem hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="85527-105">A *502.5 Process Failure* that occurs when debugging locally can be troubleshooted using the advice in this topic.</span></span>

::: moniker-end

### <a name="500-internal-server-error"></a><span data-ttu-id="85527-106">500 İç sunucu hatası</span><span class="sxs-lookup"><span data-stu-id="85527-106">500 Internal Server Error</span></span>

<span data-ttu-id="85527-107">Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.</span><span class="sxs-lookup"><span data-stu-id="85527-107">The app starts, but an error prevents the server from fulfilling the request.</span></span>

<span data-ttu-id="85527-108">Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur.</span><span class="sxs-lookup"><span data-stu-id="85527-108">This error occurs within the app's code during startup or while creating a response.</span></span> <span data-ttu-id="85527-109">Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="85527-109">The response may contain no content, or the response may appear as a *500 Internal Server Error* in the browser.</span></span> <span data-ttu-id="85527-110">Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir.</span><span class="sxs-lookup"><span data-stu-id="85527-110">The Application Event Log usually states that the app started normally.</span></span> <span data-ttu-id="85527-111">Sunucunun açısından bakıldığında, doğru olmasıdır.</span><span class="sxs-lookup"><span data-stu-id="85527-111">From the server's perspective, that's correct.</span></span> <span data-ttu-id="85527-112">Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor.</span><span class="sxs-lookup"><span data-stu-id="85527-112">The app did start, but it can't generate a valid response.</span></span> <span data-ttu-id="85527-113">Uygulama sunucuda bir komut isteminde çalıştırın veya [ASP.NET Core modülü stdout günlüğünü etkinleştir](#aspnet-core-module-stdout-log) sorunu gidermek için.</span><span class="sxs-lookup"><span data-stu-id="85527-113">Run the app at a command prompt on the server or [enable the ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log) to troubleshoot the problem.</span></span>

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="85527-114">500.0 işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="85527-114">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="85527-115">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="85527-115">The worker process fails.</span></span> <span data-ttu-id="85527-116">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-116">The app doesn't start.</span></span>

<span data-ttu-id="85527-117">[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) .NET Core CLR bulup işlemde istek işleyicisi bulma başarısız oluyor (*aspnetcorev2_inprocess.dll*).</span><span class="sxs-lookup"><span data-stu-id="85527-117">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the .NET Core CLR and find the in-process request handler (*aspnetcorev2_inprocess.dll*).</span></span> <span data-ttu-id="85527-118">Kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="85527-118">Check that:</span></span>

* <span data-ttu-id="85527-119">Uygulamayı ya da hedeflediğinden [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet paketini veya [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="85527-119">The app targets either the [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet package or the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
* <span data-ttu-id="85527-120">ASP.NET Core paylaşılan framework'ün hedefliyorsa hedef makinede yüklü sürümü.</span><span class="sxs-lookup"><span data-stu-id="85527-120">The version of the ASP.NET Core shared framework that the app targets is installed on the target machine.</span></span>

### <a name="5000-out-of-process-handler-load-failure"></a><span data-ttu-id="85527-121">500.0 giden işlem işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="85527-121">500.0 Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="85527-122">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="85527-122">The worker process fails.</span></span> <span data-ttu-id="85527-123">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-123">The app doesn't start.</span></span>

<span data-ttu-id="85527-124">[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) işlem dışı barındırma istek işleyicisi bulmak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="85527-124">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) fails to find the out-of-process hosting request handler.</span></span> <span data-ttu-id="85527-125">Emin *aspnetcorev2_outofprocess.dll* yanında bir alt klasöründe yoksa *aspnetcorev2.dll*.</span><span class="sxs-lookup"><span data-stu-id="85527-125">Make sure the *aspnetcorev2_outofprocess.dll* is present in a subfolder next to *aspnetcorev2.dll*.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a><span data-ttu-id="85527-126">500.0 işlem içi işleyici yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="85527-126">500.0 In-Process Handler Load Failure</span></span>

<span data-ttu-id="85527-127">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="85527-127">The worker process fails.</span></span> <span data-ttu-id="85527-128">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-128">The app doesn't start.</span></span>

<span data-ttu-id="85527-129">Yüklenirken bilinmeyen bir hata oluştu [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) bileşenleri.</span><span class="sxs-lookup"><span data-stu-id="85527-129">An unknown error occurred loading [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) components.</span></span> <span data-ttu-id="85527-130">Aşağıdaki eylemlerden birini gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="85527-130">Take one of the following actions:</span></span>

* <span data-ttu-id="85527-131">İlgili kişi [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (seçin **Geliştirici Araçları** ardından **ASP.NET Core**).</span><span class="sxs-lookup"><span data-stu-id="85527-131">Contact [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (select **Developer Tools** then **ASP.NET Core**).</span></span>
* <span data-ttu-id="85527-132">Stack Overflow sitesinde bir soru sorun.</span><span class="sxs-lookup"><span data-stu-id="85527-132">Ask a question on Stack Overflow.</span></span>
* <span data-ttu-id="85527-133">Bir sorun üzerinde dosya bizim [GitHub deposu](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="85527-133">File an issue on our [GitHub repository](https://github.com/aspnet/AspNetCore).</span></span>

### <a name="50030-in-process-startup-failure"></a><span data-ttu-id="85527-134">500.30 işlemdeki başlatma hatası</span><span class="sxs-lookup"><span data-stu-id="85527-134">500.30 In-Process Startup Failure</span></span>

<span data-ttu-id="85527-135">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="85527-135">The worker process fails.</span></span> <span data-ttu-id="85527-136">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-136">The app doesn't start.</span></span>

<span data-ttu-id="85527-137">[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) işlemdeki .NET Core CLR, ancak başlatma girişimleri başarısız başlatmak.</span><span class="sxs-lookup"><span data-stu-id="85527-137">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core CLR in-process, but it fails to start.</span></span> <span data-ttu-id="85527-138">Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="85527-138">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="85527-139">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="85527-139">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="85527-140">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="85527-140">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span>

### <a name="50031-ancm-failed-to-find-native-dependencies"></a><span data-ttu-id="85527-141">500.31 yerel bağımlılıkları bulmak ANCM başarısız oldu</span><span class="sxs-lookup"><span data-stu-id="85527-141">500.31 ANCM Failed to Find Native Dependencies</span></span>

<span data-ttu-id="85527-142">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="85527-142">The worker process fails.</span></span> <span data-ttu-id="85527-143">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-143">The app doesn't start.</span></span>

<span data-ttu-id="85527-144">[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) başlatmak .NET Core çalışma zamanı işlemdeki, ancak başlatma girişimleri başarısız.</span><span class="sxs-lookup"><span data-stu-id="85527-144">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the .NET Core runtime in-process, but it fails to start.</span></span> <span data-ttu-id="85527-145">En yaygın nedeni bu başlatma başarısız olduğunda `Microsoft.NETCore.App` veya `Microsoft.AspNetCore.App` çalışma zamanı yüklü değil.</span><span class="sxs-lookup"><span data-stu-id="85527-145">The most common cause of this startup failure is when the `Microsoft.NETCore.App` or `Microsoft.AspNetCore.App` runtime isn't installed.</span></span> <span data-ttu-id="85527-146">ASP.NET Core 3.0 hedefine uygulamanın dağıtıldığı ve bu sürüm makinede mevcut değil, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="85527-146">If the app is deployed to target ASP.NET Core 3.0 and that version doesn't exist on the machine, this error occurs.</span></span> <span data-ttu-id="85527-147">Bir örnek hata iletisi aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="85527-147">An example error message follows:</span></span>

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

<span data-ttu-id="85527-148">Hata iletisi, tüm yüklü .NET Core sürümleri ve uygulama tarafından istenen sürümü listelenir.</span><span class="sxs-lookup"><span data-stu-id="85527-148">The error message lists all the installed .NET Core versions and the version requested by the app.</span></span> <span data-ttu-id="85527-149">Ya da bu hatayı düzeltmek için:</span><span class="sxs-lookup"><span data-stu-id="85527-149">To fix this error, either:</span></span>

* <span data-ttu-id="85527-150">Makine üzerinde .NET Core uygun sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="85527-150">Install the appropriate version of .NET Core on the machine.</span></span>
* <span data-ttu-id="85527-151">Uygulama makinede mevcut olan .NET Core sürümünü hedefleyecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="85527-151">Change the app to target a version of .NET Core that's present on the machine.</span></span>
* <span data-ttu-id="85527-152">Uygulama olarak Yayımla bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd).</span><span class="sxs-lookup"><span data-stu-id="85527-152">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

<span data-ttu-id="85527-153">Geliştirme çalıştırırken ( `ASPNETCORE_ENVIRONMENT` ortam değişkeni ayarlandığında `Development`), HTTP yanıtı belirli bir hata yazılır.</span><span class="sxs-lookup"><span data-stu-id="85527-153">When running in development (the `ASPNETCORE_ENVIRONMENT` environment variable is set to `Development`), the specific error is written to the HTTP response.</span></span> <span data-ttu-id="85527-154">Bir işlem başlatma hatanın nedenini ayrıca şurada bulunur [uygulama olay günlüğüne](#application-event-log).</span><span class="sxs-lookup"><span data-stu-id="85527-154">The cause of a process startup failure is also found in the [Application Event Log](#application-event-log).</span></span>

### <a name="50032-ancm-failed-to-load-dll"></a><span data-ttu-id="85527-155">500.32 yük dll ANCM başarısız oldu</span><span class="sxs-lookup"><span data-stu-id="85527-155">500.32 ANCM Failed to Load dll</span></span>

<span data-ttu-id="85527-156">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="85527-156">The worker process fails.</span></span> <span data-ttu-id="85527-157">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-157">The app doesn't start.</span></span>

<span data-ttu-id="85527-158">Bu hatanın en yaygın nedeni, uygulama için bir uyumsuz işlemciye mimari yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="85527-158">The most common cause for this error is that the app is published for an incompatible processor architecture.</span></span> <span data-ttu-id="85527-159">Çalışan işlemin bir 32 bit uygulama olarak çalıştığı ve uygulama 64-bit hedefine yayımlandı, bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="85527-159">If the worker process is running as a 32-bit app and the app was published to target 64-bit, this error occurs.</span></span>

<span data-ttu-id="85527-160">Ya da bu hatayı düzeltmek için:</span><span class="sxs-lookup"><span data-stu-id="85527-160">To fix this error, either:</span></span>

* <span data-ttu-id="85527-161">Çalışan işlemi olarak aynı işlemci mimarisi için uygulamayı yeniden yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="85527-161">Republish the app for the same processor architecture as the worker process.</span></span>
* <span data-ttu-id="85527-162">Uygulama olarak Yayımla bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-executables-fde).</span><span class="sxs-lookup"><span data-stu-id="85527-162">Publish the app as a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-executables-fde).</span></span>

### <a name="50033-ancm-request-handler-load-failure"></a><span data-ttu-id="85527-163">500.33 ANCM istek işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="85527-163">500.33 ANCM Request Handler Load Failure</span></span>

<span data-ttu-id="85527-164">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="85527-164">The worker process fails.</span></span> <span data-ttu-id="85527-165">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-165">The app doesn't start.</span></span>

<span data-ttu-id="85527-166">Uygulamaya başvurmak yaramadı `Microsoft.AspNetCore.App` framework.</span><span class="sxs-lookup"><span data-stu-id="85527-166">The app didn't reference the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="85527-167">Yalnızca hedefleyen uygulamaları `Microsoft.AspNetCore.App` framework tarafından barındırılabilir [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="85527-167">Only apps targeting the `Microsoft.AspNetCore.App` framework can be hosted by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="85527-168">Bu hatayı düzeltmek için uygulamayı hedeflediği onaylayın `Microsoft.AspNetCore.App` framework.</span><span class="sxs-lookup"><span data-stu-id="85527-168">To fix this error, confirm that the app is targeting the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="85527-169">Denetleme `.runtimeconfig.json` uygulama tarafından hedeflenen framework doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="85527-169">Check the `.runtimeconfig.json` to verify the framework targeted by the app.</span></span>

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a><span data-ttu-id="85527-170">500.34 desteklenmeyen barındırma modelleri ANCM karma</span><span class="sxs-lookup"><span data-stu-id="85527-170">500.34 ANCM Mixed Hosting Models Not Supported</span></span>

<span data-ttu-id="85527-171">Çalışan işlemi, hem bir işlem içi uygulamayı hem de işlem dışı uygulama aynı işlem içinde çalıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-171">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="85527-172">Bu hatayı düzeltmek için ayrı IIS uygulama havuzları, uygulamaları çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="85527-172">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a><span data-ttu-id="85527-173">500.35 aynı işlemde birden çok işlem içi uygulama ANCM</span><span class="sxs-lookup"><span data-stu-id="85527-173">500.35 ANCM Multiple In-Process Applications in same Process</span></span>

<span data-ttu-id="85527-174">Çalışan işlemi, hem bir işlem içi uygulamayı hem de işlem dışı uygulama aynı işlem içinde çalıştırılamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-174">The worker process can't run both an in-process app and an out-of-process app in the same process.</span></span>

<span data-ttu-id="85527-175">Bu hatayı düzeltmek için ayrı IIS uygulama havuzları, uygulamaları çalıştırma.</span><span class="sxs-lookup"><span data-stu-id="85527-175">To fix this error, run apps in separate IIS application pools.</span></span>

### <a name="50036-ancm-out-of-process-handler-load-failure"></a><span data-ttu-id="85527-176">500.36 ANCM giden işlem işleyicisi yükleme hatası</span><span class="sxs-lookup"><span data-stu-id="85527-176">500.36 ANCM Out-Of-Process Handler Load Failure</span></span>

<span data-ttu-id="85527-177">İşlem dışı istek işleyicisi *aspnetcorev2_outofprocess.dll*, yanındaki değil *aspnetcorev2.dll* dosya.</span><span class="sxs-lookup"><span data-stu-id="85527-177">The out-of-process request handler, *aspnetcorev2_outofprocess.dll*, isn't next to the *aspnetcorev2.dll* file.</span></span> <span data-ttu-id="85527-178">Bu, bozuk bir yüklemeyi gösterir [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="85527-178">This indicates a corrupted installation of the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="85527-179">Bu hatayı düzeltmek için yüklemesini onarmak [.NET Core barındırma paket](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (IIS için) ya da Visual Studio (için IIS Express).</span><span class="sxs-lookup"><span data-stu-id="85527-179">To fix this error, repair the installation of the [.NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (for IIS) or Visual Studio (for IIS Express).</span></span>

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a><span data-ttu-id="85527-180">500.37 ANCM başlangıç süre sınırı içinde başlatılamadı</span><span class="sxs-lookup"><span data-stu-id="85527-180">500.37 ANCM Failed to Start Within Startup Time Limit</span></span>

<span data-ttu-id="85527-181">ANCM bulunduğunda başlangıç süre sınırı içinde başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="85527-181">ANCM failed to start within the provied startup time limit.</span></span> <span data-ttu-id="85527-182">Varsayılan olarak, zaman aşımı süresi, 120 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="85527-182">By default, the timeout is 120 seconds.</span></span>

<span data-ttu-id="85527-183">Çok sayıda uygulamalar aynı makinede başlatırken bu hata oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="85527-183">This error can occur when starting a large number of apps on the same machine.</span></span> <span data-ttu-id="85527-184">Sunucuda CPU/bellek ani başlatma sırasında kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="85527-184">Check for CPU/Memory usage spikes on the server during startup.</span></span> <span data-ttu-id="85527-185">Birden fazla uygulama başlatma işlemi basamaklandırmak gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="85527-185">You may need to stagger the startup process of multiple apps.</span></span>

::: moniker-end

### <a name="5025-process-failure"></a><span data-ttu-id="85527-186">502.5 işlem hatası</span><span class="sxs-lookup"><span data-stu-id="85527-186">502.5 Process Failure</span></span>

<span data-ttu-id="85527-187">Çalışan işlemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="85527-187">The worker process fails.</span></span> <span data-ttu-id="85527-188">Uygulama başlamaz.</span><span class="sxs-lookup"><span data-stu-id="85527-188">The app doesn't start.</span></span>

<span data-ttu-id="85527-189">[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) başlatmak çalışan işlemi ancak başlatma girişimleri başarısız.</span><span class="sxs-lookup"><span data-stu-id="85527-189">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) attempts to start the worker process but it fails to start.</span></span> <span data-ttu-id="85527-190">Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).</span><span class="sxs-lookup"><span data-stu-id="85527-190">The cause of a process startup failure can usually be determined from entries in the [Application Event Log](#application-event-log) and the [ASP.NET Core Module stdout log](#aspnet-core-module-stdout-log).</span></span>

<span data-ttu-id="85527-191">Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir.</span><span class="sxs-lookup"><span data-stu-id="85527-191">A common failure condition is the app is misconfigured due to targeting a version of the ASP.NET Core shared framework that isn't present.</span></span> <span data-ttu-id="85527-192">Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.</span><span class="sxs-lookup"><span data-stu-id="85527-192">Check which versions of the ASP.NET Core shared framework are installed on the target machine.</span></span> <span data-ttu-id="85527-193">*Paylaşılan çerçeve* derlemeleri kümesidir ( *.dll* dosyaları) bu makinede yüklü ve gibi bir metapackage tarafından başvurulan `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="85527-193">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and referenced by a metapackage such as `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="85527-194">Gerekli en düşük sürüm metapackage başvuru belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="85527-194">The metapackage reference can specify a minimum required version.</span></span> <span data-ttu-id="85527-195">Daha fazla bilgi için [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="85527-195">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="85527-196">*502.5 işlem hatası* hata sayfası, barındırma veya uygulama yanlış yapılandırma çalışan işlemin başarısız olmasına neden olduğunda döndürülür:</span><span class="sxs-lookup"><span data-stu-id="85527-196">The *502.5 Process Failure* error page is returned when a hosting or app misconfiguration causes the worker process to fail:</span></span>

### <a name="failed-to-start-application-errorcode-0x800700c1"></a><span data-ttu-id="85527-197">Uygulama (hata kodu: '0x800700c1') başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="85527-197">Failed to start application (ErrorCode '0x800700c1')</span></span>

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

<span data-ttu-id="85527-198">Uygulama başlatılamadı uygulamanın derleme ( *.dll*) yüklenmesi tamamlanamadı.</span><span class="sxs-lookup"><span data-stu-id="85527-198">The app failed to start because the app's assembly (*.dll*) couldn't be loaded.</span></span>

<span data-ttu-id="85527-199">W3wp/ıısexpress işlemi ile yayımlanan uygulama arasındaki bir bit genişliği uyuşmazlığı olduğunda bu hata oluşur.</span><span class="sxs-lookup"><span data-stu-id="85527-199">This error occurs when there's a bitness mismatch between the published app and the w3wp/iisexpress process.</span></span>

<span data-ttu-id="85527-200">Uygulama havuzunun 32-bit ayarının doğru olduğundan emin olun:</span><span class="sxs-lookup"><span data-stu-id="85527-200">Confirm that the app pool's 32-bit setting is correct:</span></span>

1. <span data-ttu-id="85527-201">IIS Yöneticisi'nin uygulama havuzunu seçin **uygulama havuzları**.</span><span class="sxs-lookup"><span data-stu-id="85527-201">Select the app pool in IIS Manager's **Application Pools**.</span></span>
1. <span data-ttu-id="85527-202">Seçin **Gelişmiş ayarlar** altında **uygulama havuzunu Düzenle** içinde **eylemleri** paneli.</span><span class="sxs-lookup"><span data-stu-id="85527-202">Select **Advanced Settings** under **Edit Application Pool** in the **Actions** panel.</span></span>
1. <span data-ttu-id="85527-203">Ayarlama **32-Bit uygulamaları etkinleştir**:</span><span class="sxs-lookup"><span data-stu-id="85527-203">Set **Enable 32-Bit Applications**:</span></span>
   * <span data-ttu-id="85527-204">Bir 32-bit (x86) dağıtma, uygulama ayarlarsanız değer `True`.</span><span class="sxs-lookup"><span data-stu-id="85527-204">If deploying a 32-bit (x86) app, set the value to `True`.</span></span>
   * <span data-ttu-id="85527-205">Bir 64-bit (x64) dağıtma, uygulama ayarlarsanız değer `False`.</span><span class="sxs-lookup"><span data-stu-id="85527-205">If deploying a 64-bit (x64) app, set the value to `False`.</span></span>

### <a name="connection-reset"></a><span data-ttu-id="85527-206">Bağlantı sıfırlama</span><span class="sxs-lookup"><span data-stu-id="85527-206">Connection reset</span></span>

<span data-ttu-id="85527-207">Üst bilgileri gönderildiğinde sonra bir hata oluşursa, sunucunun göndermek çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda.</span><span class="sxs-lookup"><span data-stu-id="85527-207">If an error occurs after the headers are sent, it's too late for the server to send a **500 Internal Server Error** when an error occurs.</span></span> <span data-ttu-id="85527-208">Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="85527-208">This often happens when an error occurs during the serialization of complex objects for a response.</span></span> <span data-ttu-id="85527-209">Bu tür olarak görünür bir *bağlantı sıfırlama* istemci üzerinde hata.</span><span class="sxs-lookup"><span data-stu-id="85527-209">This type of error appears as a *connection reset* error on the client.</span></span> <span data-ttu-id="85527-210">[Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataları gidermeye yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="85527-210">[Application logging](xref:fundamentals/logging/index) can help troubleshoot these types of errors.</span></span>

## <a name="default-startup-limits"></a><span data-ttu-id="85527-211">Varsayılan başlangıç sınırları</span><span class="sxs-lookup"><span data-stu-id="85527-211">Default startup limits</span></span>

<span data-ttu-id="85527-212">[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) varsayılan ile yapılandırılmış *startupTimeLimit* 120 saniye.</span><span class="sxs-lookup"><span data-stu-id="85527-212">The [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) is configured with a default *startupTimeLimit* of 120 seconds.</span></span> <span data-ttu-id="85527-213">Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="85527-213">When left at the default value, an app may take up to two minutes to start before the module logs a process failure.</span></span> <span data-ttu-id="85527-214">Modül yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="85527-214">For information on configuring the module, see [Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>
