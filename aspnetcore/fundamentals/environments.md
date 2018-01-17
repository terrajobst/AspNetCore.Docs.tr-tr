---
title: "Birden çok ASP.NET Core ortamlarda ile çalışma"
author: rick-anderson
description: "Birden çok ortamlar genelinde uygulamanızın davranışını denetlemek için ASP.NET Core destek nasıl sağladığını öğrenin."
keywords: "ASP.NET Core, ortam ayarları, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a><span data-ttu-id="98704-104">Birden çok ortamı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="98704-104">Working with multiple environments</span></span>

<span data-ttu-id="98704-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="98704-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="98704-106">ASP.NET Core, ortam değişkenleri ile çalışma zamanında uygulama davranışını ayarlamak için destek sağlar.</span><span class="sxs-lookup"><span data-stu-id="98704-106">ASP.NET Core provides support for setting application behavior at runtime with environment variables.</span></span>

<span data-ttu-id="98704-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="98704-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="98704-108">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="98704-108">Environments</span></span>

<span data-ttu-id="98704-109">ASP.NET Core okur ortam değişkeni `ASPNETCORE_ENVIRONMENT` uygulama başlangıçta ve değer depoları [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span><span class="sxs-lookup"><span data-stu-id="98704-109">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at application startup and stores that value in [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName).</span></span> <span data-ttu-id="98704-110">`ASPNETCORE_ENVIRONMENT`herhangi bir değere ayarlanabilir ancak [üç değerden](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) framework tarafından desteklenir: [geliştirme](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [hazırlama](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), ve [üretim](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="98704-110">`ASPNETCORE_ENVIRONMENT` can be set to any value, but [three values](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) are supported by the framework: [Development](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [Staging](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), and [Production](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0).</span></span> <span data-ttu-id="98704-111">Varsa `ASPNETCORE_ENVIRONMENT` ayarlanmazsa varsayılan için `Production`.</span><span class="sxs-lookup"><span data-stu-id="98704-111">If `ASPNETCORE_ENVIRONMENT` is not set, it will default to `Production`.</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="98704-112">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="98704-112">The preceding code:</span></span>

* <span data-ttu-id="98704-113">Çağrıları [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) zaman `ASPNETCORE_ENVIRONMENT` ayarlanır `Development`.</span><span class="sxs-lookup"><span data-stu-id="98704-113">Calls [UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="98704-114">Çağrıları [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) zaman değerini `ASPNETCORE_ENVIRONMENT` aşağıdakilerden birini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="98704-114">Calls [UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="98704-115">[Ortam etiketi yardımcı ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) değerini kullanır `IHostingEnvironment.EnvironmentName` dahil etmek veya hariç biçimlendirme öğesi içinde:</span><span class="sxs-lookup"><span data-stu-id="98704-115">The [Environment Tag Helper ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="98704-116">Not: Windows ve macOS, ortam değişkenlerini ve değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="98704-116">Note: On Windows and macOS, environment variables and values are not case sensitive.</span></span> <span data-ttu-id="98704-117">Linux ortam değişkenlerini ve değerleri **büyük küçük harfe duyarlı** varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="98704-117">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="98704-118">Geliştirme</span><span class="sxs-lookup"><span data-stu-id="98704-118">Development</span></span>

<span data-ttu-id="98704-119">Geliştirme ortamı üretimde gösterilmemesi özellikleri etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="98704-119">The development environment can enable features that should not be exposed in production.</span></span> <span data-ttu-id="98704-120">Örneğin, ASP.NET Core Şablonları etkinleştirmek [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling#the-developer-exception-page) geliştirme ortamında.</span><span class="sxs-lookup"><span data-stu-id="98704-120">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="98704-121">Yerel makine geliştirme ortamını ayarlanabilir *Properties\launchSettings.json* projenin dosya.</span><span class="sxs-lookup"><span data-stu-id="98704-121">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="98704-122">Ortam değerleri kümesinde *launchSettings.json* sistem ortama değerlerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="98704-122">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="98704-123">Aşağıdaki XML üç profillerden gösterir bir *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="98704-123">The following XML shows three profiles from a *launchSettings.json* file:</span></span>

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

<span data-ttu-id="98704-124">Ne zaman uygulama başlatıldığında ile `dotnet run`, ilk profiliyle `"commandName": "Project"` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98704-124">When the application is launched with `dotnet run`, the first profile with `"commandName": "Project"` will be used.</span></span> <span data-ttu-id="98704-125">Değeri `commandName` başlatmak için web sunucusunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="98704-125">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="98704-126">`commandName`aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="98704-126">`commandName` can be one of :</span></span>

* <span data-ttu-id="98704-127">IIS Express</span><span class="sxs-lookup"><span data-stu-id="98704-127">IIS Express</span></span>
* <span data-ttu-id="98704-128">IIS</span><span class="sxs-lookup"><span data-stu-id="98704-128">IIS</span></span>
* <span data-ttu-id="98704-129">(Kestrel başlatır) projesi</span><span class="sxs-lookup"><span data-stu-id="98704-129">Project (which launches Kestrel)</span></span>

<span data-ttu-id="98704-130">Ne zaman bir uygulama başlatıldığında ile `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="98704-130">When an app is launched with `dotnet run`:</span></span>

* <span data-ttu-id="98704-131">*launchSettings.json* okunur varsa.</span><span class="sxs-lookup"><span data-stu-id="98704-131">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="98704-132">`environmentVariables` settings in *launchSettings.json* override environment variables.</span><span class="sxs-lookup"><span data-stu-id="98704-132">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="98704-133">Barındırma ortamı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="98704-133">The hosting environment is displayed.</span></span>


<span data-ttu-id="98704-134">Aşağıdaki çıkış kullanmaya uygulama gösterir `dotnet run`:</span><span class="sxs-lookup"><span data-stu-id="98704-134">The following output shows an app started with `dotnet run`:</span></span>
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="98704-135">Visual Studio **hata ayıklama** sekmesi sağlar düzenlemek için bir GUI *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="98704-135">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proje Özellikleri ayarını ortam değişkenleri](environments/_static/project-properties-debug.png)

<span data-ttu-id="98704-137">Web sunucu yeniden başlatılana kadar proje profillere yapılan değişiklikler etkilerini göstermeyebilir.</span><span class="sxs-lookup"><span data-stu-id="98704-137">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="98704-138">Kestrel, ortama yapılan değişiklikleri algılar önce başlatılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="98704-138">Kestrel must be restarted before it will detect changes made to its environment.</span></span>

>[!WARNING]
> <span data-ttu-id="98704-139">*launchSettings.json* gizli saklamalısınız değil.</span><span class="sxs-lookup"><span data-stu-id="98704-139">*launchSettings.json* should not store secrets.</span></span> <span data-ttu-id="98704-140">[Gizli Yöneticisi aracını](xref:security/app-secrets) yerel geliştirme için parolaları depolamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="98704-140">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

### <a name="production"></a><span data-ttu-id="98704-141">Üretim</span><span class="sxs-lookup"><span data-stu-id="98704-141">Production</span></span>

<span data-ttu-id="98704-142">Üretim ortamında, güvenlik, performans ve uygulama sağlamlık en üst düzeye çıkarmak için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="98704-142">The production environment should be configured to maximize security, performance, and application robustness.</span></span> <span data-ttu-id="98704-143">Geliştirme farklı bir üretim ortamında olabilecek bazı genel ayarları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="98704-143">Some common settings that a production environment might have that would differ from development include:</span></span>

* <span data-ttu-id="98704-144">Önbelleğe alma.</span><span class="sxs-lookup"><span data-stu-id="98704-144">Caching.</span></span>
* <span data-ttu-id="98704-145">İstemci-tarafı kaynaklar küçültülmüş, paketlenmiş ve potansiyel olarak bir CDN hizmet.</span><span class="sxs-lookup"><span data-stu-id="98704-145">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="98704-146">Tanılama hata sayfaları devre dışı.</span><span class="sxs-lookup"><span data-stu-id="98704-146">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="98704-147">Kolay hata sayfaları etkin.</span><span class="sxs-lookup"><span data-stu-id="98704-147">Friendly error pages enabled.</span></span>
* <span data-ttu-id="98704-148">Üretim günlüğe kaydetme ve izleme etkin.</span><span class="sxs-lookup"><span data-stu-id="98704-148">Production logging and monitoring enabled.</span></span> <span data-ttu-id="98704-149">Örneğin, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span><span class="sxs-lookup"><span data-stu-id="98704-149">For example, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="98704-150">Ortamını ayarlama</span><span class="sxs-lookup"><span data-stu-id="98704-150">Setting the environment</span></span>

<span data-ttu-id="98704-151">Genellikle, test etmek için belirli bir ortam ayarlamak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="98704-151">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="98704-152">Ortamında ayarlanmazsa, varsayılan olur `Production` , devre dışı bırakır çoğu hata ayıklama özelliği.</span><span class="sxs-lookup"><span data-stu-id="98704-152">If the environment is not set, it will default to `Production` which disables most debugging features.</span></span>

<span data-ttu-id="98704-153">Ortam ayarı yöntemi işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="98704-153">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure"></a><span data-ttu-id="98704-154">Azure</span><span class="sxs-lookup"><span data-stu-id="98704-154">Azure</span></span>

<span data-ttu-id="98704-155">Azure uygulama hizmeti için:</span><span class="sxs-lookup"><span data-stu-id="98704-155">For Azure app service:</span></span>

* <span data-ttu-id="98704-156">Seçin **uygulama ayarları** dikey.</span><span class="sxs-lookup"><span data-stu-id="98704-156">Select the **Application settings** blade.</span></span>
* <span data-ttu-id="98704-157">Anahtarın ekleyin ve değer **uygulama ayarları**.</span><span class="sxs-lookup"><span data-stu-id="98704-157">Add the key and value in **App settings**.</span></span>


### <a name="windows"></a><span data-ttu-id="98704-158">Windows</span><span class="sxs-lookup"><span data-stu-id="98704-158">Windows</span></span>
<span data-ttu-id="98704-159">Ayarlamak için `ASPNETCORE_ENVIRONMENT` uygulamayı kullanmaya başladıysanız geçerli oturum için `dotnet run`, aşağıdaki komutları kullanılır</span><span class="sxs-lookup"><span data-stu-id="98704-159">To set the `ASPNETCORE_ENVIRONMENT` for the current session, if the app is started using `dotnet run`, the following commands are used</span></span>

<span data-ttu-id="98704-160">**Komut satırı**</span><span class="sxs-lookup"><span data-stu-id="98704-160">**Command line**</span></span>
```
set ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="98704-161">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="98704-161">**PowerShell**</span></span>
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="98704-162">Bu komutlar, yalnızca geçerli pencereyi için etkili olur.</span><span class="sxs-lookup"><span data-stu-id="98704-162">These commands take effect only for the current window.</span></span> <span data-ttu-id="98704-163">Pencere kapatıldığında ASPNETCORE_ENVIRONMENT ayarı varsayılan ayar veya bir makine değere geri döner.</span><span class="sxs-lookup"><span data-stu-id="98704-163">When the window is closed, the ASPNETCORE_ENVIRONMENT setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="98704-164">Windows Aç değeri genel olarak ayarlamak için **Denetim Masası** > **sistem** > **Gelişmiş Sistem ayarları** ve ekleme veya düzenleme`ASPNETCORE_ENVIRONMENT` değeri.</span><span class="sxs-lookup"><span data-stu-id="98704-164">In order to set the value globally on Windows open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value.</span></span>

![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

![ASP.NET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)


<span data-ttu-id="98704-167">**web.config**</span><span class="sxs-lookup"><span data-stu-id="98704-167">**web.config**</span></span>

<span data-ttu-id="98704-168">Bkz: *ortam değişkenlerini ayarlama* bölümünü [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) konu.</span><span class="sxs-lookup"><span data-stu-id="98704-168">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="98704-169">**Her IIS uygulama havuzu**</span><span class="sxs-lookup"><span data-stu-id="98704-169">**Per IIS Application Pool**</span></span>

<span data-ttu-id="98704-170">Ortam değişkenlerini (IIS 10.0 + desteklenir) yalıtılmış uygulama havuzlarında çalışan tek tek uygulamalar için ayarlamak için bkz: *AppCmd.exe komutunu* bölümünü [ortam değişkenleri \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konu.</span><span class="sxs-lookup"><span data-stu-id="98704-170">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="98704-171">macOS</span><span class="sxs-lookup"><span data-stu-id="98704-171">macOS</span></span>
<span data-ttu-id="98704-172">Geçerli ortamı macOS için satır içi uygulama çalışırken yapılabilir;</span><span class="sxs-lookup"><span data-stu-id="98704-172">Setting the current environment for macOS can be done in-line when running the application;</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
<span data-ttu-id="98704-173">veya kullanarak `export` uygulama çalıştırılmadan önce ayarlamak için.</span><span class="sxs-lookup"><span data-stu-id="98704-173">or using `export` to set it prior to running the app.</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
<span data-ttu-id="98704-174">Makine düzeyi ortam değişkenleri ayarlanır *.bashrc* veya *.bash_profile* dosya.</span><span class="sxs-lookup"><span data-stu-id="98704-174">Machine level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="98704-175">Herhangi bir metin düzenleyicisi kullanarak dosyasını düzenleyin ve aşağıdaki ifadeyi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="98704-175">Edit the file using any text editor and add the following statment.</span></span>

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="98704-176">Linux</span><span class="sxs-lookup"><span data-stu-id="98704-176">Linux</span></span>
<span data-ttu-id="98704-177">Linux distro'lar için kullanmak `export` değişkeni ayarları oturum tabanlı için komut satırında komut ve *bash_profile* makine düzeyi ortam ayarları dosyası.</span><span class="sxs-lookup"><span data-stu-id="98704-177">For Linux distros, use the `export` command at the command line for session based variable settings and *bash_profile* file for machine level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="98704-178">Ortam yapılandırma</span><span class="sxs-lookup"><span data-stu-id="98704-178">Configuration by environment</span></span>

<span data-ttu-id="98704-179">Bkz: [yapılandırma ortamı tarafından](xref:fundamentals/configuration/index#configuration-by-environment) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="98704-179">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="98704-180">Ortamı tabanlı, başlangıç sınıfı ve yöntemleri</span><span class="sxs-lookup"><span data-stu-id="98704-180">Environment based Startup class and methods</span></span>

<span data-ttu-id="98704-181">Bir ASP.NET Core uygulama başlatıldığında [başlangıç sınıfı](xref:fundamentals/startup) uygulama bootstraps.</span><span class="sxs-lookup"><span data-stu-id="98704-181">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="98704-182">Bir sınıf belirtilmemişse `Startup{EnvironmentName}` yoksa, sınıf için çağrılacağı `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="98704-182">If a class `Startup{EnvironmentName}` exists, that class will be called for that `EnvironmentName`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="98704-183">Not: Çağırma [WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) yapılandırma bölümlerinin geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="98704-183">Note: Calling [WebHostBuilder.UseStartup<TStartup>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="98704-184">[Yapılandırma](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) ortam belirli formun sürümleri desteği `Configure{EnvironmentName}` ve `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="98704-184">[Configure](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) support environment specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="98704-185">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="98704-185">Additional Resources</span></span>

* [<span data-ttu-id="98704-186">Uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="98704-186">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="98704-187">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="98704-187">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="98704-188">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="98704-188">IHostingEnvironment.EnvironmentName</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
