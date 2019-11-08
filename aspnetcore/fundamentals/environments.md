---
title: ASP.NET Core çoklu ortamları kullanma
author: rick-anderson
description: ASP.NET Core uygulamalarında birden çok ortamda uygulama davranışını denetlemeyi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/environments
ms.openlocfilehash: 7e49499e94fb9ea82a0ba17e4e9de05c6a2d4e98
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799307"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="198e3-103">ASP.NET Core çoklu ortamları kullanma</span><span class="sxs-lookup"><span data-stu-id="198e3-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="198e3-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="198e3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="198e3-105">ASP.NET Core, bir ortam değişkeni kullanarak çalışma zamanı ortamı temelinde uygulama davranışını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="198e3-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="198e3-106">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="198e3-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="198e3-107">Lý</span><span class="sxs-lookup"><span data-stu-id="198e3-107">Environments</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="198e3-108">ASP.NET Core, uygulama başlangıcında `ASPNETCORE_ENVIRONMENT` ortam değişkenini okur ve değeri [ıwebhostenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="198e3-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="198e3-109">`ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir, ancak Framework tarafından üç değer sağlanır:</span><span class="sxs-lookup"><span data-stu-id="198e3-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="198e3-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="198e3-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="198e3-111">ASP.NET Core, uygulama başlangıcında `ASPNETCORE_ENVIRONMENT` ortam değişkenini okur ve değeri [ıhostingenvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName)içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="198e3-111">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="198e3-112">`ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir, ancak Framework tarafından üç değer sağlanır:</span><span class="sxs-lookup"><span data-stu-id="198e3-112">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="198e3-113"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="198e3-113"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

::: moniker-end

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="198e3-114">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="198e3-114">The preceding code:</span></span>

* <span data-ttu-id="198e3-115">`ASPNETCORE_ENVIRONMENT` `Development`olarak ayarlandığında, [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) çağırır.</span><span class="sxs-lookup"><span data-stu-id="198e3-115">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="198e3-116">`ASPNETCORE_ENVIRONMENT` değeri aşağıdakilerden birini ayarladığınızda [Useexceptionhandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="198e3-116">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="198e3-117">[Ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) , öğesinde biçimlendirme eklemek veya dışlamak için `IHostingEnvironment.EnvironmentName` değerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="198e3-117">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="198e3-118">Windows ve macOS 'ta, ortam değişkenleri ve değerleri büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="198e3-118">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="198e3-119">Linux ortam değişkenleri ve değerleri varsayılan olarak **büyük/küçük harfe duyarlıdır** .</span><span class="sxs-lookup"><span data-stu-id="198e3-119">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="198e3-120">Geliştirme</span><span class="sxs-lookup"><span data-stu-id="198e3-120">Development</span></span>

<span data-ttu-id="198e3-121">Geliştirme ortamı, üretimde gösterilmemelidir özellikleri etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="198e3-121">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="198e3-122">Örneğin, ASP.NET Core Şablonlar geliştirme ortamında [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling#developer-exception-page) etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="198e3-122">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="198e3-123">Yerel makine geliştirme ortamı, projenin *Properties\launchSettings.JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="198e3-123">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="198e3-124">*Launchsettings. JSON* geçersiz kılma değerlerini sistem ortamında ayarlanan ortam değerleri.</span><span class="sxs-lookup"><span data-stu-id="198e3-124">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="198e3-125">Aşağıdaki JSON, bir *Launchsettings. JSON* dosyasından üç profil gösterir:</span><span class="sxs-lookup"><span data-stu-id="198e3-125">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:54339/",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS Express": {
      "commandName": "IISExpress",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      }
    },
    "EnvironmentsSample": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:54340/"
    },
    "Kestrel Staging": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_My_Environment": "1",
        "ASPNETCORE_DETAILEDERRORS": "1",
        "ASPNETCORE_ENVIRONMENT": "Staging"
      },
      "applicationUrl": "http://localhost:51997/"
    }
  }
}
```

> [!NOTE]
> <span data-ttu-id="198e3-126">*Launchsettings. JSON* içindeki `applicationUrl` özelliği sunucu URL 'lerinin bir listesini belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="198e3-126">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="198e3-127">Listedeki URL 'Ler arasında noktalı virgül kullanın:</span><span class="sxs-lookup"><span data-stu-id="198e3-127">Use a semicolon between the URLs in the list:</span></span>
>
> ```json
> "EnvironmentsSample": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```

<span data-ttu-id="198e3-128">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında, `"commandName": "Project"` ilk profili kullanılır.</span><span class="sxs-lookup"><span data-stu-id="198e3-128">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="198e3-129">`commandName` değeri, başlatılacak Web sunucusunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="198e3-129">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="198e3-130">`commandName` aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="198e3-130">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="198e3-131">`Project` (Kestrel Başlatan)</span><span class="sxs-lookup"><span data-stu-id="198e3-131">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="198e3-132">Bir uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında:</span><span class="sxs-lookup"><span data-stu-id="198e3-132">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="198e3-133">*Launchsettings. JSON* varsa okundu.</span><span class="sxs-lookup"><span data-stu-id="198e3-133">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="198e3-134">*launchsettings. JSON* geçersiz kılma ortamı değişkenlerine `environmentVariables` ayarları.</span><span class="sxs-lookup"><span data-stu-id="198e3-134">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="198e3-135">Barındırma ortamı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="198e3-135">The hosting environment is displayed.</span></span>

<span data-ttu-id="198e3-136">Aşağıdaki çıktıda, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatılan bir uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="198e3-136">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="198e3-137">Visual Studio proje özellikleri **hata ayıklama** sekmesi, *launchsettings. JSON* dosyasını düzenlemek için bir GUI sağlar:</span><span class="sxs-lookup"><span data-stu-id="198e3-137">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proje özellikleri ayar ortamı değişkenleri](environments/_static/project-properties-debug.png)

<span data-ttu-id="198e3-139">Proje profillerinde yapılan değişiklikler, Web sunucusu yeniden başlatılana kadar etkili olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="198e3-139">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="198e3-140">Kestrel, ortamında yapılan değişiklikleri algılayabilmesi için yeniden başlatılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-140">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="198e3-141">*Launchsettings. JSON* gizli dizileri depolamamamalıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-141">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="198e3-142">Gizli anahtar geliştirme için gizli dizileri depolamak için [gizli dizi Yöneticisi aracı](xref:security/app-secrets) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="198e3-142">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="198e3-143">[Visual Studio Code](https://code.visualstudio.com/)kullanırken, ortam değişkenleri *. vscode/Launch. JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="198e3-143">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="198e3-144">Aşağıdaki örnek, `Development`için ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="198e3-144">The following example sets the environment to `Development`:</span></span>

```json
{
   "version": "0.2.0",
   "configurations": [
        {
            "name": ".NET Core Launch (web)",

            ... additional VS Code configuration settings ...

            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

<span data-ttu-id="198e3-145">Uygulama `dotnet run` *Özellikler/launchSettings. JSON*ile aynı şekilde başlatılırken, projedeki bir *. vscode/Launch. JSON* dosyası okunamaz.</span><span class="sxs-lookup"><span data-stu-id="198e3-145">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="198e3-146">Geliştirme sırasında *Launchsettings. JSON* dosyası olmayan bir uygulama başlatırken, ortam değişkeni veya bir komut satırı bağımsız değişkeni olan ortamı `dotnet run` komutuna ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="198e3-146">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="198e3-147">Üretiminden</span><span class="sxs-lookup"><span data-stu-id="198e3-147">Production</span></span>

<span data-ttu-id="198e3-148">Üretim ortamının güvenliği, performansı ve uygulama sağlamlık düzeyini en üst düzeye çıkarmak için yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="198e3-148">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="198e3-149">Geliştirmeden farklı bazı yaygın ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="198e3-149">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="198e3-150">Önbelleği.</span><span class="sxs-lookup"><span data-stu-id="198e3-150">Caching.</span></span>
* <span data-ttu-id="198e3-151">İstemci tarafı kaynaklar paketlenmiş, küçültülmüş ve potansiyel olarak bir CDN 'den sunulan.</span><span class="sxs-lookup"><span data-stu-id="198e3-151">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="198e3-152">Tanılama hata sayfaları devre dışı.</span><span class="sxs-lookup"><span data-stu-id="198e3-152">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="198e3-153">Kolay hata sayfaları etkin.</span><span class="sxs-lookup"><span data-stu-id="198e3-153">Friendly error pages enabled.</span></span>
* <span data-ttu-id="198e3-154">Üretim günlüğü ve izleme etkin.</span><span class="sxs-lookup"><span data-stu-id="198e3-154">Production logging and monitoring enabled.</span></span> <span data-ttu-id="198e3-155">Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="198e3-155">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="198e3-156">Ortamı ayarlama</span><span class="sxs-lookup"><span data-stu-id="198e3-156">Set the environment</span></span>

<span data-ttu-id="198e3-157">Bir ortam değişkeni veya platform ayarıyla test için belirli bir ortam ayarlamak genellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-157">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="198e3-158">Ortam ayarlanmamışsa, çoğu hata ayıklama özelliğini devre dışı bırakan `Production`varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="198e3-158">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="198e3-159">Ortamı ayarlama yöntemi işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-159">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="198e3-160">Konak yapılandırıldığında, uygulama tarafından okunan son ortam ayarı, uygulamanın ortamını belirler.</span><span class="sxs-lookup"><span data-stu-id="198e3-160">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="198e3-161">Uygulama çalışırken uygulamanın ortamı değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="198e3-161">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="198e3-162">Ortam değişkeni veya platform ayarı</span><span class="sxs-lookup"><span data-stu-id="198e3-162">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="198e3-163">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="198e3-163">Azure App Service</span></span>

<span data-ttu-id="198e3-164">[Azure App Service](https://azure.microsoft.com/services/app-service/)ortamında ortamı ayarlamak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="198e3-164">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="198e3-165">Uygulama **Hizmetleri** dikey penceresinden uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="198e3-165">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="198e3-166">**Ayarlar** grubunda, **uygulama ayarları** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="198e3-166">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="198e3-167">**Uygulama ayarları** alanında **yeni ayar Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="198e3-167">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="198e3-168">**Ad girin**için `ASPNETCORE_ENVIRONMENT`sağlayın.</span><span class="sxs-lookup"><span data-stu-id="198e3-168">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="198e3-169">**Değer girin**için ortamı sağlayın (örneğin, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="198e3-169">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="198e3-170">Ortam ayarının, dağıtım yuvaları takas edildiğinde geçerli yuvada kalmasını istiyorsanız, **yuva ayarı** onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="198e3-170">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="198e3-171">Daha fazla bilgi için bkz. [Azure belgeleri: hangi ayarları değiştirmiş?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="198e3-171">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="198e3-172">Dikey pencerenin en üstünde **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="198e3-172">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="198e3-173">Azure App Service, Azure portal bir uygulama ayarı (ortam değişkeni) eklendikten, değiştirildikten veya silindikten sonra uygulamayı otomatik olarak yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="198e3-173">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="198e3-174">Windows</span><span class="sxs-lookup"><span data-stu-id="198e3-174">Windows</span></span>

<span data-ttu-id="198e3-175">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)kullanılarak başlatıldığında geçerli oturumun `ASPNETCORE_ENVIRONMENT` ayarlamak için aşağıdaki komutlar kullanılır:</span><span class="sxs-lookup"><span data-stu-id="198e3-175">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="198e3-176">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="198e3-176">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="198e3-177">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="198e3-177">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="198e3-178">Bu komutlar yalnızca geçerli pencere için etkili olur.</span><span class="sxs-lookup"><span data-stu-id="198e3-178">These commands only take effect for the current window.</span></span> <span data-ttu-id="198e3-179">Pencere kapatıldığında, `ASPNETCORE_ENVIRONMENT` ayarı varsayılan ayar veya makine değerine geri döner.</span><span class="sxs-lookup"><span data-stu-id="198e3-179">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="198e3-180">Windows 'da genel değeri ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="198e3-180">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="198e3-181">**Sistem** > **gelişmiş sistem ayarları** > **denetim masası** 'nı açın ve `ASPNETCORE_ENVIRONMENT` değerini ekleyin veya düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="198e3-181">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

  ![ASPNET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="198e3-184">Bir yönetim komut istemi açın ve `setx` komutunu kullanın veya bir yönetim PowerShell komut istemi açın ve `[Environment]::SetEnvironmentVariable`kullanın:</span><span class="sxs-lookup"><span data-stu-id="198e3-184">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="198e3-185">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="198e3-185">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="198e3-186">`/M` anahtarı, ortam değişkenini sistem düzeyinde ayarlamaya yönelik olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="198e3-186">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="198e3-187">`/M` anahtarı kullanılmazsa, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="198e3-187">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="198e3-188">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="198e3-188">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="198e3-189">`Machine` seçenek değeri, ortam değişkeninin sistem düzeyinde ayarlandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="198e3-189">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="198e3-190">Seçenek değeri `User`olarak değiştirilirse, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="198e3-190">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="198e3-191">`ASPNETCORE_ENVIRONMENT` ortam değişkeni genel olarak ayarlandığında, değer ayarlandıktan sonra açılan herhangi bir komut penceresinde `dotnet run` için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="198e3-191">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="198e3-192">**Web. config**</span><span class="sxs-lookup"><span data-stu-id="198e3-192">**web.config**</span></span>

<span data-ttu-id="198e3-193">`ASPNETCORE_ENVIRONMENT` ortam değişkenini *Web. config*ile ayarlamak için, <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>*ortam değişkenlerini ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="198e3-193">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="198e3-194">**Proje dosyası veya yayımlama profili**</span><span class="sxs-lookup"><span data-stu-id="198e3-194">**Project file or publish profile**</span></span>

<span data-ttu-id="198e3-195">**WINDOWS IIS dağıtımları için:** `<EnvironmentName>` özelliğini [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="198e3-195">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="198e3-196">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="198e3-196">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

::: moniker-end

<span data-ttu-id="198e3-197">**IIS uygulama havuzu başına**</span><span class="sxs-lookup"><span data-stu-id="198e3-197">**Per IIS Application Pool**</span></span>

<span data-ttu-id="198e3-198">Yalıtılmış uygulama havuzunda çalışan bir uygulamanın `ASPNETCORE_ENVIRONMENT` ortam değişkenini ayarlamak için (IIS 10,0 veya üzeri sürümlerde desteklenir), [ortam değişkenlerinin &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konusunun *Appcmd. exe komut* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="198e3-198">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="198e3-199">`ASPNETCORE_ENVIRONMENT` ortam değişkeni bir uygulama havuzu için ayarlandığında, değeri sistem düzeyindeki bir ayarı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="198e3-199">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="198e3-200">IIS 'de bir uygulama barındırırken ve `ASPNETCORE_ENVIRONMENT` ortam değişkenini ekleyerek veya değiştirirken, yeni değerin uygulamalar tarafından çekilmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="198e3-200">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="198e3-201">`net stop was /y` ve ardından komut isteminden `net start w3svc` yürütün.</span><span class="sxs-lookup"><span data-stu-id="198e3-201">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="198e3-202">Sunucuyu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="198e3-202">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="198e3-203">macOS</span><span class="sxs-lookup"><span data-stu-id="198e3-203">macOS</span></span>

<span data-ttu-id="198e3-204">MacOS için geçerli ortamın ayarlanması, uygulamayı çalıştırırken satır içinde gerçekleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="198e3-204">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="198e3-205">Alternatif olarak, uygulamayı çalıştırmadan önce ortamı `export` ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="198e3-205">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="198e3-206">Makine düzeyinde ortam değişkenleri *. bashrc* veya *. bash_profile* dosyasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="198e3-206">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="198e3-207">Herhangi bir metin düzenleyicisini kullanarak dosyayı düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="198e3-207">Edit the file using any text editor.</span></span> <span data-ttu-id="198e3-208">Aşağıdaki ifadeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="198e3-208">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="198e3-209">Linux</span><span class="sxs-lookup"><span data-stu-id="198e3-209">Linux</span></span>

<span data-ttu-id="198e3-210">Linux distros için, makine düzeyindeki ortam ayarları için oturum tabanlı değişken ayarları ve *bash_profile* dosyası için bir komut isteminde `export` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="198e3-210">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="198e3-211">Kodda ortam ayarlama</span><span class="sxs-lookup"><span data-stu-id="198e3-211">Set the environment in code</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="198e3-212">Konağı oluştururken <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="198e3-212">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="198e3-213">Bkz. <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="198e3-213">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="198e3-214">Konağı oluştururken <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="198e3-214">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="198e3-215">Bkz. <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="198e3-215">See <xref:fundamentals/host/web-host#environment>.</span></span>

::: moniker-end

### <a name="configuration-by-environment"></a><span data-ttu-id="198e3-216">Ortama göre yapılandırma</span><span class="sxs-lookup"><span data-stu-id="198e3-216">Configuration by environment</span></span>

<span data-ttu-id="198e3-217">Yapılandırmayı ortama göre yüklemek için şunları yapmanızı öneririz:</span><span class="sxs-lookup"><span data-stu-id="198e3-217">To load configuration by environment, we recommend:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="198e3-218">*appSettings* dosyaları (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="198e3-218">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="198e3-219">Bkz. <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="198e3-219">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="198e3-220">Ortam değişkenleri (uygulamanın barındırıldığı her bir sistemde ayarlanır).</span><span class="sxs-lookup"><span data-stu-id="198e3-220">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="198e3-221">Bkz. <xref:fundamentals/host/generic-host#environmentname> ve <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="198e3-221">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="198e3-222">Gizli dizi Yöneticisi (yalnızca geliştirme ortamında).</span><span class="sxs-lookup"><span data-stu-id="198e3-222">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="198e3-223">Bkz. <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="198e3-223">See <xref:security/app-secrets>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="198e3-224">*appSettings* dosyaları (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="198e3-224">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="198e3-225">Bkz. <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="198e3-225">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="198e3-226">Ortam değişkenleri (uygulamanın barındırıldığı her bir sistemde ayarlanır).</span><span class="sxs-lookup"><span data-stu-id="198e3-226">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="198e3-227">Bkz. <xref:fundamentals/host/web-host#environment> ve <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="198e3-227">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="198e3-228">Gizli dizi Yöneticisi (yalnızca geliştirme ortamında).</span><span class="sxs-lookup"><span data-stu-id="198e3-228">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="198e3-229">Bkz. <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="198e3-229">See <xref:security/app-secrets>.</span></span>

::: moniker-end

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="198e3-230">Ortam tabanlı başlangıç sınıfı ve yöntemleri</span><span class="sxs-lookup"><span data-stu-id="198e3-230">Environment-based Startup class and methods</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="198e3-231">Iwebhostenvironment 'ı başlatmaya ekleme. configure</span><span class="sxs-lookup"><span data-stu-id="198e3-231">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="198e3-232"><xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> `Startup.Configure`ekleme.</span><span class="sxs-lookup"><span data-stu-id="198e3-232">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="198e3-233">Bu yaklaşım, uygulama yalnızca, ortam başına en az kod farklılığı olan birkaç ortam için `Startup.Configure` ayarlamayı gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-233">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="198e3-234">Başlangıç sınıfına ıwebhostenvironment ekleme</span><span class="sxs-lookup"><span data-stu-id="198e3-234">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="198e3-235">`Startup` oluşturucusuna <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="198e3-235">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="198e3-236">Bu yaklaşım, uygulama her ortam için en az kod farklılığı olan birkaç ortam için `Startup` yapılandırmayı gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-236">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="198e3-237">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="198e3-237">In the following example:</span></span>

* <span data-ttu-id="198e3-238">Ortam `_env` alanında tutulur.</span><span class="sxs-lookup"><span data-stu-id="198e3-238">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="198e3-239">`_env`, `ConfigureServices` ve `Configure` uygulamanın ortamına göre başlangıç yapılandırmasını uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="198e3-239">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IWebHostEnvironment _env;

    public Startup(IWebHostEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="198e3-240">Başlangıç olarak ıhostingenvironment ekleme. yapılandırma</span><span class="sxs-lookup"><span data-stu-id="198e3-240">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="198e3-241"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> `Startup.Configure`ekleme.</span><span class="sxs-lookup"><span data-stu-id="198e3-241">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="198e3-242">Bu yaklaşım, uygulama yalnızca, ortam başına en az kod farklılığı olan birkaç ortam için `Startup.Configure` yapılandırmayı gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-242">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Development environment code
    }
    else
    {
        // Code for all other environments
    }
}
```

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="198e3-243">Başlangıç sınıfına ıhostingenvironment ekleme</span><span class="sxs-lookup"><span data-stu-id="198e3-243">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="198e3-244">`Startup` oluşturucusuna <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> ekleyin ve hizmeti `Startup` sınıfı boyunca kullanmak üzere bir alana atayın.</span><span class="sxs-lookup"><span data-stu-id="198e3-244">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="198e3-245">Bu yaklaşım, uygulama her ortam için en az kod farklılığı olan birkaç ortam için başlatma yapılandırması gerektirdiğinde faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-245">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="198e3-246">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="198e3-246">In the following example:</span></span>

* <span data-ttu-id="198e3-247">Ortam `_env` alanında tutulur.</span><span class="sxs-lookup"><span data-stu-id="198e3-247">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="198e3-248">`_env`, `ConfigureServices` ve `Configure` uygulamanın ortamına göre başlangıç yapılandırmasını uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="198e3-248">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

```csharp
public class Startup
{
    private readonly IHostingEnvironment _env;

    public Startup(IHostingEnvironment env)
    {
        _env = env;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else if (_env.IsStaging())
        {
            // Staging environment code
        }
        else
        {
            // Code for all other environments
        }
    }

    public void Configure(IApplicationBuilder app)
    {
        if (_env.IsDevelopment())
        {
            // Development environment code
        }
        else
        {
            // Code for all other environments
        }
    }
}
```

::: moniker-end

### <a name="startup-class-conventions"></a><span data-ttu-id="198e3-249">Başlangıç sınıfı kuralları</span><span class="sxs-lookup"><span data-stu-id="198e3-249">Startup class conventions</span></span>

<span data-ttu-id="198e3-250">ASP.NET Core bir uygulama başlatıldığında, [Başlangıç sınıfı](xref:fundamentals/startup) uygulamayı önyükleme.</span><span class="sxs-lookup"><span data-stu-id="198e3-250">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="198e3-251">Uygulama farklı ortamlar için ayrı `Startup` sınıfları tanımlayabilir (örneğin, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="198e3-251">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="198e3-252">Uygun `Startup` sınıfı çalışma zamanında seçildi.</span><span class="sxs-lookup"><span data-stu-id="198e3-252">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="198e3-253">Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir.</span><span class="sxs-lookup"><span data-stu-id="198e3-253">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="198e3-254">Eşleşen bir `Startup{EnvironmentName}` sınıfı bulunamazsa, `Startup` sınıfı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="198e3-254">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="198e3-255">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-255">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="198e3-256">Ortam tabanlı `Startup` sınıfları uygulamak için, kullanımdaki her ortam için bir `Startup{EnvironmentName}` sınıfı ve bir geri dönüş `Startup` sınıfı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="198e3-256">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
    }
}
```

<span data-ttu-id="198e3-257">Derleme adını kabul eden [Usestartup (ıwebhostbuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) aşırı yüklemesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="198e3-257">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName);
}
```

### <a name="startup-method-conventions"></a><span data-ttu-id="198e3-258">Başlangıç yöntemi kuralları</span><span class="sxs-lookup"><span data-stu-id="198e3-258">Startup method conventions</span></span>

<span data-ttu-id="198e3-259">[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) , form `Configure<EnvironmentName>` ve `Configure<EnvironmentName>Services`ortama özgü sürümlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="198e3-259">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="198e3-260">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="198e3-260">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="198e3-261">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="198e3-261">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
