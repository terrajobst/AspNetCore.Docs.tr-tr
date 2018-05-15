---
title: ASP.NET Core kullanan birden çok ortamlar
author: rick-anderson
description: Birden çok ortamlar genelinde uygulamanızın davranışını denetlemek için ASP.NET Core destek nasıl sağladığını öğrenin.
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: 2c8441db527203aeea516073dae3bc335c335565
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="ae48b-103">ASP.NET Core kullanan birden çok ortamlar</span><span class="sxs-lookup"><span data-stu-id="ae48b-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="ae48b-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ae48b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ae48b-105">ASP.NET Core, ortam değişkenleri ile çalışma zamanında uygulama davranışını ayarlamak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="ae48b-105">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="ae48b-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ae48b-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="ae48b-107">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="ae48b-107">Environments</span></span>

<span data-ttu-id="ae48b-108">ASP.NET Core okur ortam değişkeni `ASPNETCORE_ENVIRONMENT` uygulama başlangıçta ve değer depoları [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="ae48b-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="ae48b-109">`ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir ancak [üç değerden](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) framework tarafından desteklenir: [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [hazırlama](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), ve [üretim](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="ae48b-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="ae48b-110">Varsa `ASPNETCORE_ENVIRONMENT` , varsayılan için ayarlı değil `Production`.</span><span class="sxs-lookup"><span data-stu-id="ae48b-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it will default to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="ae48b-111">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="ae48b-111">The preceding code:</span></span>

* <span data-ttu-id="ae48b-112">Çağrıları [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) zaman `ASPNETCORE_ENVIRONMENT` ayarlanır `Development`.</span><span class="sxs-lookup"><span data-stu-id="ae48b-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="ae48b-113">Çağrıları [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) zaman değerini `ASPNETCORE_ENVIRONMENT` aşağıdakilerden birini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="ae48b-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="ae48b-114">[Ortam etiketi yardımcı ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) değerini kullanır `IHostingEnvironment.EnvironmentName` dahil etmek veya hariç biçimlendirme öğesi içinde:</span><span class="sxs-lookup"><span data-stu-id="ae48b-114">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="ae48b-115">Not: Windows ve macOS, ortam değişkenlerini ve değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="ae48b-115">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="ae48b-116">Linux ortam değişkenlerini ve değerleri **büyük küçük harfe duyarlı** varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="ae48b-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="ae48b-117">Geliştirme</span><span class="sxs-lookup"><span data-stu-id="ae48b-117">Development</span></span>

<span data-ttu-id="ae48b-118">Geliştirme ortamı üretimde kullanıma sunulan döndürmemelidir özellikleri etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae48b-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="ae48b-119">Örneğin, ASP.NET Core Şablonları etkinleştirmek [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling#the-developer-exception-page) geliştirme ortamında.</span><span class="sxs-lookup"><span data-stu-id="ae48b-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="ae48b-120">Yerel makine geliştirme ortamını ayarlanabilir *Properties\launchSettings.json* projenin dosya.</span><span class="sxs-lookup"><span data-stu-id="ae48b-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="ae48b-121">Ortam değerleri kümesinde *launchSettings.json* sistem ortama değerlerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ae48b-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="ae48b-122">Aşağıdaki JSON üç profillerden gösteren bir *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ae48b-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> <span data-ttu-id="ae48b-123">`applicationUrl` Özelliğinde *launchSettings.json* sunucu URL'lerin bir listesini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ae48b-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="ae48b-124">Noktalı virgül listedeki URL'leri arasında kullanın:</span><span class="sxs-lookup"><span data-stu-id="ae48b-124">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

<span data-ttu-id="ae48b-125">Ne zaman uygulama başlatıldığında ile [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run), ilk profiliyle `"commandName": "Project"` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae48b-125">When the application is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="ae48b-126">Değeri `commandName` başlatmak için web sunucusunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="ae48b-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="ae48b-127">`commandName` aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="ae48b-127">`commandName` can be one of :</span></span>

* <span data-ttu-id="ae48b-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="ae48b-128">IIS Express</span></span>
* <span data-ttu-id="ae48b-129">IIS</span><span class="sxs-lookup"><span data-stu-id="ae48b-129">IIS</span></span>
* <span data-ttu-id="ae48b-130">(Kestrel başlatır) projesi</span><span class="sxs-lookup"><span data-stu-id="ae48b-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="ae48b-131">Ne zaman bir uygulama başlatıldığında ile [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="ae48b-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="ae48b-132">*launchSettings.json* okunur varsa.</span><span class="sxs-lookup"><span data-stu-id="ae48b-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="ae48b-133">`environmentVariables` ayarlarında *launchSettings.json* ortam değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="ae48b-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="ae48b-134">Barındırma ortamı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ae48b-134">The hosting environment is displayed.</span></span>


<span data-ttu-id="ae48b-135">Aşağıdaki çıkış kullanmaya uygulama gösterir [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="ae48b-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="ae48b-136">Visual Studio **hata ayıklama** sekmesi sağlar düzenlemek için bir GUI *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ae48b-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proje Özellikleri ayarını ortam değişkenleri](environments/_static/project-properties-debug.png)

<span data-ttu-id="ae48b-138">Web sunucu yeniden başlatılana kadar proje profillere yapılan değişiklikler etkilerini göstermeyebilir.</span><span class="sxs-lookup"><span data-stu-id="ae48b-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="ae48b-139">Kestrel, ortama yapılan değişiklikleri algılar önce başlatılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ae48b-139">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="ae48b-140">*launchSettings.json* parolaları depolamak döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="ae48b-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="ae48b-141">[Gizli Yöneticisi aracını](xref:security/app-secrets) yerel geliştirme için parolaları depolamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ae48b-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="ae48b-142">Üretim</span><span class="sxs-lookup"><span data-stu-id="ae48b-142">Production</span></span>

<span data-ttu-id="ae48b-143">Üretim ortamında, güvenlik, performans ve uygulama sağlamlık en üst düzeye çıkarmak için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ae48b-143">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="ae48b-144">Geliştirme farklı bazı ortak ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ae48b-144">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="ae48b-145">Önbelleğe alma.</span><span class="sxs-lookup"><span data-stu-id="ae48b-145">Caching.</span></span>
* <span data-ttu-id="ae48b-146">İstemci-tarafı kaynaklar küçültülmüş, paketlenmiş ve potansiyel olarak bir CDN hizmet.</span><span class="sxs-lookup"><span data-stu-id="ae48b-146">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="ae48b-147">Tanılama hata sayfaları devre dışı.</span><span class="sxs-lookup"><span data-stu-id="ae48b-147">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="ae48b-148">Kolay hata sayfaları etkin.</span><span class="sxs-lookup"><span data-stu-id="ae48b-148">Friendly error pages enabled.</span></span>
* <span data-ttu-id="ae48b-149">Üretim günlüğe kaydetme ve izleme etkin.</span><span class="sxs-lookup"><span data-stu-id="ae48b-149">Production logging and monitoring enabled.</span></span> <span data-ttu-id="ae48b-150">Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="ae48b-150">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="ae48b-151">Ortamını ayarlama</span><span class="sxs-lookup"><span data-stu-id="ae48b-151">Setting the environment</span></span>

<span data-ttu-id="ae48b-152">Genellikle, test etmek için belirli bir ortam ayarlamak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="ae48b-152">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="ae48b-153">Ortam ayarlanmamışsa, varsayılan olur `Production` , devre dışı bırakır çoğu hata ayıklama özelliği.</span><span class="sxs-lookup"><span data-stu-id="ae48b-153">If the environment isn't set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="ae48b-154">Ortam ayarı yöntemi işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="ae48b-154">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="ae48b-155">Azure</span><span class="sxs-lookup"><span data-stu-id="ae48b-155">Azure</span></span>

<span data-ttu-id="ae48b-156">Azure uygulama hizmeti için:</span><span class="sxs-lookup"><span data-stu-id="ae48b-156">For Azure app service:</span></span>

* <span data-ttu-id="ae48b-157">Seçin **uygulama ayarları** dikey.</span><span class="sxs-lookup"><span data-stu-id="ae48b-157">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="ae48b-158">Anahtarın ekleyin ve değer **uygulama ayarları**.</span><span class="sxs-lookup"><span data-stu-id="ae48b-158">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="ae48b-159">Windows</span><span class="sxs-lookup"><span data-stu-id="ae48b-159">Windows</span></span>
<span data-ttu-id="ae48b-160">Ayarlamak için `ASPNETCORE_ENVIRONMENT` uygulamayı kullanmaya başladıysanız geçerli oturum için [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run), aşağıdaki komutları kullanılır</span><span class="sxs-lookup"><span data-stu-id="ae48b-160">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used</span></span>

<span data-ttu-id="ae48b-161">**Komut satırı**</span><span class="sxs-lookup"><span data-stu-id="ae48b-161">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="ae48b-162">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="ae48b-162">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="ae48b-163">Bu komutlar, yalnızca geçerli pencereyi için etkili olur.</span><span class="sxs-lookup"><span data-stu-id="ae48b-163">These commands take effect only for the current window.</span></span> <span data-ttu-id="ae48b-164">Pencere kapatıldığında ASPNETCORE_ENVIRONMENT ayarı varsayılan ayar veya bir makine değere geri döner.</span><span class="sxs-lookup"><span data-stu-id="ae48b-164">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="ae48b-165">Windows Aç değeri genel olarak ayarlamak için **Denetim Masası** > **sistem** > **Gelişmiş Sistem ayarları** ve ekleme veya düzenleme`ASPNETCORE_ENVIRONMENT` değeri.</span><span class="sxs-lookup"><span data-stu-id="ae48b-165">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

![ASP.NET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="ae48b-168">**Web.config**</span><span class="sxs-lookup"><span data-stu-id="ae48b-168">**web.config**</span></span>

<span data-ttu-id="ae48b-169">Bkz: *ortam değişkenlerini ayarlama* bölümünü [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) konu.</span><span class="sxs-lookup"><span data-stu-id="ae48b-169">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="ae48b-170">**Her IIS uygulama havuzu**</span><span class="sxs-lookup"><span data-stu-id="ae48b-170">**Per IIS Application Pool**</span></span>

<span data-ttu-id="ae48b-171">Ortam değişkenlerini (IIS 10.0 + desteklenir) yalıtılmış uygulama havuzlarında çalışan tek tek uygulamalar için ayarlamak için bkz: *AppCmd.exe komutunu* bölümünü [ortam değişkenleri \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konu.</span><span class="sxs-lookup"><span data-stu-id="ae48b-171">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="ae48b-172">MacOS</span><span class="sxs-lookup"><span data-stu-id="ae48b-172">macOS</span></span>
<span data-ttu-id="ae48b-173">Geçerli ortamı macOS için satır içi uygulama çalışırken yapılabilir;</span><span class="sxs-lookup"><span data-stu-id="ae48b-173">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="ae48b-174">veya kullanarak `export` uygulama çalıştırılmadan önce ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="ae48b-174">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="ae48b-175">Makine düzeyi ortam değişkenleri ayarlanır *.bashrc* veya *.bash_profile* dosya.</span><span class="sxs-lookup"><span data-stu-id="ae48b-175">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="ae48b-176">Herhangi bir metin düzenleyicisi kullanarak dosyasını düzenleyin ve aşağıdaki ifadeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ae48b-176">Edit the file using any text editor and add the following statment.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="ae48b-177">Linux</span><span class="sxs-lookup"><span data-stu-id="ae48b-177">Linux</span></span>
<span data-ttu-id="ae48b-178">Linux distro'lar için kullanmak `export` değişkeni ayarları oturum tabanlı için komut satırında komut ve *bash_profile* makine düzeyi ortam ayarları dosyası.</span><span class="sxs-lookup"><span data-stu-id="ae48b-178">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="ae48b-179">Ortam yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ae48b-179">Configuration by environment</span></span>

<span data-ttu-id="ae48b-180">Bkz: [yapılandırma ortamı tarafından](xref:fundamentals/configuration/index#configuration-by-environment) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ae48b-180">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="ae48b-181">Ortamı tabanlı, başlangıç sınıfı ve yöntemleri</span><span class="sxs-lookup"><span data-stu-id="ae48b-181">Environment based Startup class and methods</span></span>

<span data-ttu-id="ae48b-182">Bir ASP.NET Core uygulama başlatıldığında [başlangıç sınıfı](xref:fundamentals/startup) uygulama bootstraps.</span><span class="sxs-lookup"><span data-stu-id="ae48b-182">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="ae48b-183">Bir sınıf belirtilmemişse `Startup{EnvironmentName}` yoksa, sınıf için çağrılacağı `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="ae48b-183">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="ae48b-184">Not: Çağırma [WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) yapılandırma bölümlerinin geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ae48b-184">Note: Calling [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="ae48b-185">[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) ortam belirli formun sürümleri desteği `Configure{EnvironmentName}` ve `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="ae48b-185">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="ae48b-186">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ae48b-186">Additional resources</span></span>

* [<span data-ttu-id="ae48b-187">Uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="ae48b-187">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="ae48b-188">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ae48b-188">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="ae48b-189">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="ae48b-189">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
