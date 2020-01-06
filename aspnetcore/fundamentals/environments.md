---
title: ASP.NET Core çoklu ortamları kullanma
author: rick-anderson
description: ASP.NET Core uygulamalarında birden çok ortamda uygulama davranışını denetlemeyi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/17/2019
uid: fundamentals/environments
ms.openlocfilehash: 30e2771c0a24fcbf6490d08c7028566314b6c011
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358727"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="a01d4-103">ASP.NET Core çoklu ortamları kullanma</span><span class="sxs-lookup"><span data-stu-id="a01d4-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a01d4-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a01d4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a01d4-105">ASP.NET Core, bir ortam değişkeni kullanarak çalışma zamanı ortamı temelinde uygulama davranışını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="a01d4-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a01d4-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="a01d4-107">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="a01d4-107">Environments</span></span>

<span data-ttu-id="a01d4-108">ASP.NET Core, uygulama başlangıcında `ASPNETCORE_ENVIRONMENT` ortam değişkenini okur ve değeri [ıwebhostenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="a01d4-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="a01d4-109">`ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir, ancak Framework tarafından üç değer sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="a01d4-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="a01d4-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="a01d4-111">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="a01d4-111">The preceding code:</span></span>

* <span data-ttu-id="a01d4-112">`ASPNETCORE_ENVIRONMENT` `Development`olarak ayarlandığında, [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) çağırır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="a01d4-113">`ASPNETCORE_ENVIRONMENT` değeri aşağıdakilerden birini ayarladığınızda [Useexceptionhandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="a01d4-114">[Ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) , öğesinde biçimlendirme eklemek veya dışlamak için `IHostingEnvironment.EnvironmentName` değerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="a01d4-115">Windows ve macOS 'ta, ortam değişkenleri ve değerleri büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="a01d4-116">Linux ortam değişkenleri ve değerleri varsayılan olarak **büyük/küçük harfe duyarlıdır** .</span><span class="sxs-lookup"><span data-stu-id="a01d4-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="a01d4-117">Geliştirme</span><span class="sxs-lookup"><span data-stu-id="a01d4-117">Development</span></span>

<span data-ttu-id="a01d4-118">Geliştirme ortamı, üretimde gösterilmemelidir özellikleri etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="a01d4-119">Örneğin, ASP.NET Core Şablonlar geliştirme ortamında [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling#developer-exception-page) etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="a01d4-120">Yerel makine geliştirme ortamı, projenin *Properties\launchSettings.JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="a01d4-121">*Launchsettings. JSON* geçersiz kılma değerlerini sistem ortamında ayarlanan ortam değerleri.</span><span class="sxs-lookup"><span data-stu-id="a01d4-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="a01d4-122">Aşağıdaki JSON, bir *Launchsettings. JSON* dosyasından üç profil gösterir:</span><span class="sxs-lookup"><span data-stu-id="a01d4-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="a01d4-123">*Launchsettings. JSON* içindeki `applicationUrl` özelliği sunucu URL 'lerinin bir listesini belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="a01d4-124">Listedeki URL 'Ler arasında noktalı virgül kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="a01d4-125">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında, `"commandName": "Project"` ilk profili kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="a01d4-126">`commandName` değeri, başlatılacak Web sunucusunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="a01d4-127">`commandName` aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="a01d4-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="a01d4-128">`Project` (Kestrel Başlatan)</span><span class="sxs-lookup"><span data-stu-id="a01d4-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="a01d4-129">Bir uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında:</span><span class="sxs-lookup"><span data-stu-id="a01d4-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="a01d4-130">*Launchsettings. JSON* varsa okundu.</span><span class="sxs-lookup"><span data-stu-id="a01d4-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="a01d4-131">*launchsettings. JSON* geçersiz kılma ortamı değişkenlerine `environmentVariables` ayarları.</span><span class="sxs-lookup"><span data-stu-id="a01d4-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="a01d4-132">Barındırma ortamı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="a01d4-133">Aşağıdaki çıktıda, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatılan bir uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a01d4-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="a01d4-134">Visual Studio proje özellikleri **hata ayıklama** sekmesi, *launchsettings. JSON* dosyasını düzenlemek için bir GUI sağlar:</span><span class="sxs-lookup"><span data-stu-id="a01d4-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proje özellikleri ayar ortamı değişkenleri](environments/_static/project-properties-debug.png)

<span data-ttu-id="a01d4-136">Proje profillerinde yapılan değişiklikler, Web sunucusu yeniden başlatılana kadar etkili olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="a01d4-137">Kestrel, ortamında yapılan değişiklikleri algılayabilmesi için yeniden başlatılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="a01d4-138">*Launchsettings. JSON* gizli dizileri depolamamamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="a01d4-139">Gizli anahtar geliştirme için gizli dizileri depolamak için [gizli dizi Yöneticisi aracı](xref:security/app-secrets) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="a01d4-140">[Visual Studio Code](https://code.visualstudio.com/)kullanırken, ortam değişkenleri *. vscode/Launch. JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="a01d4-141">Aşağıdaki örnek, `Development`için ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="a01d4-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="a01d4-142">Uygulama `dotnet run` *Özellikler/launchSettings. JSON*ile aynı şekilde başlatılırken, projedeki bir *. vscode/Launch. JSON* dosyası okunamaz.</span><span class="sxs-lookup"><span data-stu-id="a01d4-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="a01d4-143">Geliştirme sırasında *Launchsettings. JSON* dosyası olmayan bir uygulama başlatırken, ortam değişkeni veya bir komut satırı bağımsız değişkeni olan ortamı `dotnet run` komutuna ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="a01d4-144">Üretim</span><span class="sxs-lookup"><span data-stu-id="a01d4-144">Production</span></span>

<span data-ttu-id="a01d4-145">Üretim ortamının güvenliği, performansı ve uygulama sağlamlık düzeyini en üst düzeye çıkarmak için yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="a01d4-146">Geliştirmeden farklı bazı yaygın ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="a01d4-147">Önbelleği.</span><span class="sxs-lookup"><span data-stu-id="a01d4-147">Caching.</span></span>
* <span data-ttu-id="a01d4-148">İstemci tarafı kaynaklar paketlenmiş, küçültülmüş ve potansiyel olarak bir CDN 'den sunulan.</span><span class="sxs-lookup"><span data-stu-id="a01d4-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="a01d4-149">Tanılama hata sayfaları devre dışı.</span><span class="sxs-lookup"><span data-stu-id="a01d4-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="a01d4-150">Kolay hata sayfaları etkin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="a01d4-151">Üretim günlüğü ve izleme etkin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="a01d4-152">Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="a01d4-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="a01d4-153">Ortamı ayarlama</span><span class="sxs-lookup"><span data-stu-id="a01d4-153">Set the environment</span></span>

<span data-ttu-id="a01d4-154">Bir ortam değişkeni veya platform ayarıyla test için belirli bir ortam ayarlamak genellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="a01d4-155">Ortam ayarlanmamışsa, çoğu hata ayıklama özelliğini devre dışı bırakan `Production`varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="a01d4-156">Ortamı ayarlama yöntemi işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="a01d4-157">Konak yapılandırıldığında, uygulama tarafından okunan son ortam ayarı, uygulamanın ortamını belirler.</span><span class="sxs-lookup"><span data-stu-id="a01d4-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="a01d4-158">Uygulama çalışırken uygulamanın ortamı değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="a01d4-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="a01d4-159">Ortam değişkeni veya platform ayarı</span><span class="sxs-lookup"><span data-stu-id="a01d4-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="a01d4-160">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="a01d4-160">Azure App Service</span></span>

<span data-ttu-id="a01d4-161">[Azure App Service](https://azure.microsoft.com/services/app-service/)ortamında ortamı ayarlamak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="a01d4-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="a01d4-162">Uygulama **Hizmetleri** dikey penceresinden uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="a01d4-163">**Ayarlar** grubunda **yapılandırma** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-163">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="a01d4-164">**Uygulama ayarları** sekmesinde **Yeni uygulama ayarı**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-164">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="a01d4-165">**Uygulama ayarı Ekle/Düzenle** penceresinde **ad**için `ASPNETCORE_ENVIRONMENT` sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-165">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="a01d4-166">**Değer**için ortamı sağlayın (örneğin, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="a01d4-166">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="a01d4-167">Dağıtım yuvaları takas edildiğinde ortam ayarının geçerli yuvada kalmasını istiyorsanız **dağıtım yuvası ayarını** onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-167">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="a01d4-168">Daha fazla bilgi için bkz. Azure belgelerindeki [Azure App Service hazırlama ortamlarını ayarlama](/azure/app-service/web-sites-staged-publishing) .</span><span class="sxs-lookup"><span data-stu-id="a01d4-168">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="a01d4-169">**Tamam ' ı** seçerek **uygulama ayarı Ekle/Düzenle** penceresini kapatın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-169">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="a01d4-170">**Yapılandırma** dikey penceresinin en üstünde **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-170">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="a01d4-171">Azure App Service, Azure portal bir uygulama ayarı (ortam değişkeni) eklendikten, değiştirildikten veya silindikten sonra uygulamayı otomatik olarak yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-171">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="a01d4-172">Windows</span><span class="sxs-lookup"><span data-stu-id="a01d4-172">Windows</span></span>

<span data-ttu-id="a01d4-173">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)kullanılarak başlatıldığında geçerli oturumun `ASPNETCORE_ENVIRONMENT` ayarlamak için aşağıdaki komutlar kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-173">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="a01d4-174">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="a01d4-174">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="a01d4-175">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a01d4-175">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="a01d4-176">Bu komutlar yalnızca geçerli pencere için etkili olur.</span><span class="sxs-lookup"><span data-stu-id="a01d4-176">These commands only take effect for the current window.</span></span> <span data-ttu-id="a01d4-177">Pencere kapatıldığında, `ASPNETCORE_ENVIRONMENT` ayarı varsayılan ayar veya makine değerine geri döner.</span><span class="sxs-lookup"><span data-stu-id="a01d4-177">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="a01d4-178">Windows 'da genel değeri ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-178">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="a01d4-179">**Sistem** > **gelişmiş sistem ayarları** > **denetim masası** 'nı açın ve `ASPNETCORE_ENVIRONMENT` değerini ekleyin veya düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="a01d4-179">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

  ![ASPNET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="a01d4-182">Bir yönetim komut istemi açın ve `setx` komutunu kullanın veya bir yönetim PowerShell komut istemi açın ve `[Environment]::SetEnvironmentVariable`kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-182">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="a01d4-183">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="a01d4-183">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="a01d4-184">`/M` anahtarı, ortam değişkenini sistem düzeyinde ayarlamaya yönelik olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-184">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="a01d4-185">`/M` anahtarı kullanılmazsa, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-185">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="a01d4-186">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a01d4-186">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="a01d4-187">`Machine` seçenek değeri, ortam değişkeninin sistem düzeyinde ayarlandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-187">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="a01d4-188">Seçenek değeri `User`olarak değiştirilirse, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-188">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="a01d4-189">`ASPNETCORE_ENVIRONMENT` ortam değişkeni genel olarak ayarlandığında, değer ayarlandıktan sonra açılan herhangi bir komut penceresinde `dotnet run` için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="a01d4-189">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="a01d4-190">**Web. config**</span><span class="sxs-lookup"><span data-stu-id="a01d4-190">**web.config**</span></span>

<span data-ttu-id="a01d4-191">`ASPNETCORE_ENVIRONMENT` ortam değişkenini *Web. config*ile ayarlamak için, <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>*ortam değişkenlerini ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-191">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="a01d4-192">**Proje dosyası veya yayımlama profili**</span><span class="sxs-lookup"><span data-stu-id="a01d4-192">**Project file or publish profile**</span></span>

<span data-ttu-id="a01d4-193">**WINDOWS IIS dağıtımları için:** `<EnvironmentName>` özelliğini [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-193">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="a01d4-194">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="a01d4-194">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="a01d4-195">**IIS uygulama havuzu başına**</span><span class="sxs-lookup"><span data-stu-id="a01d4-195">**Per IIS Application Pool**</span></span>

<span data-ttu-id="a01d4-196">Yalıtılmış uygulama havuzunda çalışan bir uygulamanın `ASPNETCORE_ENVIRONMENT` ortam değişkenini ayarlamak için (IIS 10,0 veya üzeri sürümlerde desteklenir), [ortam değişkenlerinin &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konusunun *Appcmd. exe komut* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-196">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="a01d4-197">`ASPNETCORE_ENVIRONMENT` ortam değişkeni bir uygulama havuzu için ayarlandığında, değeri sistem düzeyindeki bir ayarı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="a01d4-197">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a01d4-198">IIS 'de bir uygulama barındırırken ve `ASPNETCORE_ENVIRONMENT` ortam değişkenini ekleyerek veya değiştirirken, yeni değerin uygulamalar tarafından çekilmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-198">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="a01d4-199">`net stop was /y` ve ardından komut isteminden `net start w3svc` yürütün.</span><span class="sxs-lookup"><span data-stu-id="a01d4-199">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="a01d4-200">Sunucuyu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-200">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="a01d4-201">macOS</span><span class="sxs-lookup"><span data-stu-id="a01d4-201">macOS</span></span>

<span data-ttu-id="a01d4-202">MacOS için geçerli ortamın ayarlanması, uygulamayı çalıştırırken satır içinde gerçekleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="a01d4-202">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="a01d4-203">Alternatif olarak, uygulamayı çalıştırmadan önce ortamı `export` ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-203">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="a01d4-204">Makine düzeyinde ortam değişkenleri *. bashrc* veya *. bash_profile* dosyasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-204">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="a01d4-205">Herhangi bir metin düzenleyicisini kullanarak dosyayı düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-205">Edit the file using any text editor.</span></span> <span data-ttu-id="a01d4-206">Aşağıdaki ifadeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a01d4-206">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="a01d4-207">Linux</span><span class="sxs-lookup"><span data-stu-id="a01d4-207">Linux</span></span>

<span data-ttu-id="a01d4-208">Linux distros için, oturum tabanlı değişken ayarları ve makine düzeyindeki ortam ayarları için *bash_profile* dosyası için bir komut isteminde `export` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-208">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="a01d4-209">Kodda ortam ayarlama</span><span class="sxs-lookup"><span data-stu-id="a01d4-209">Set the environment in code</span></span>

<span data-ttu-id="a01d4-210">Konağı oluştururken <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-210">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="a01d4-211">Bkz. <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="a01d4-211">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="a01d4-212">Ortama göre yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a01d4-212">Configuration by environment</span></span>

<span data-ttu-id="a01d4-213">Yapılandırmayı ortama göre yüklemek için şunları yapmanızı öneririz:</span><span class="sxs-lookup"><span data-stu-id="a01d4-213">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="a01d4-214">*appSettings* dosyaları (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="a01d4-214">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="a01d4-215">Bkz. <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="a01d4-215">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="a01d4-216">Ortam değişkenleri (uygulamanın barındırıldığı her bir sistemde ayarlanır).</span><span class="sxs-lookup"><span data-stu-id="a01d4-216">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="a01d4-217">Bkz. <xref:fundamentals/host/generic-host#environmentname> ve <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="a01d4-217">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="a01d4-218">Gizli dizi Yöneticisi (yalnızca geliştirme ortamında).</span><span class="sxs-lookup"><span data-stu-id="a01d4-218">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="a01d4-219">Bkz. <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="a01d4-219">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="a01d4-220">Ortam tabanlı başlangıç sınıfı ve yöntemleri</span><span class="sxs-lookup"><span data-stu-id="a01d4-220">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="a01d4-221">Iwebhostenvironment 'ı başlatmaya ekleme. configure</span><span class="sxs-lookup"><span data-stu-id="a01d4-221">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="a01d4-222"><xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> `Startup.Configure`ekleme.</span><span class="sxs-lookup"><span data-stu-id="a01d4-222">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="a01d4-223">Bu yaklaşım, uygulama yalnızca, ortam başına en az kod farklılığı olan birkaç ortam için `Startup.Configure` ayarlamayı gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-223">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="a01d4-224">Başlangıç sınıfına ıwebhostenvironment ekleme</span><span class="sxs-lookup"><span data-stu-id="a01d4-224">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="a01d4-225">`Startup` oluşturucusuna <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-225">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="a01d4-226">Bu yaklaşım, uygulama her ortam için en az kod farklılığı olan birkaç ortam için `Startup` yapılandırmayı gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-226">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="a01d4-227">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="a01d4-227">In the following example:</span></span>

* <span data-ttu-id="a01d4-228">Ortam `_env` alanında tutulur.</span><span class="sxs-lookup"><span data-stu-id="a01d4-228">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="a01d4-229">`_env`, `ConfigureServices` ve `Configure` uygulamanın ortamına göre başlangıç yapılandırmasını uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-229">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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
### <a name="startup-class-conventions"></a><span data-ttu-id="a01d4-230">Başlangıç sınıfı kuralları</span><span class="sxs-lookup"><span data-stu-id="a01d4-230">Startup class conventions</span></span>

<span data-ttu-id="a01d4-231">ASP.NET Core bir uygulama başlatıldığında, [Başlangıç sınıfı](xref:fundamentals/startup) uygulamayı önyükleme.</span><span class="sxs-lookup"><span data-stu-id="a01d4-231">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="a01d4-232">Uygulama farklı ortamlar için ayrı `Startup` sınıfları tanımlayabilir (örneğin, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="a01d4-232">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="a01d4-233">Uygun `Startup` sınıfı çalışma zamanında seçildi.</span><span class="sxs-lookup"><span data-stu-id="a01d4-233">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="a01d4-234">Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-234">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="a01d4-235">Eşleşen bir `Startup{EnvironmentName}` sınıfı bulunamazsa, `Startup` sınıfı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-235">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="a01d4-236">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-236">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="a01d4-237">Ortam tabanlı `Startup` sınıfları uygulamak için, kullanımdaki her ortam için bir `Startup{EnvironmentName}` sınıfı ve bir geri dönüş `Startup` sınıfı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a01d4-237">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="a01d4-238">Derleme adını kabul eden [Usestartup (ıwebhostbuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) aşırı yüklemesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-238">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="a01d4-239">Başlangıç yöntemi kuralları</span><span class="sxs-lookup"><span data-stu-id="a01d4-239">Startup method conventions</span></span>

<span data-ttu-id="a01d4-240">[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) , form `Configure<EnvironmentName>` ve `Configure<EnvironmentName>Services`ortama özgü sürümlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="a01d4-240">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="a01d4-241">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-241">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="a01d4-242">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a01d4-242">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a01d4-243">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a01d4-243">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a01d4-244">ASP.NET Core, bir ortam değişkeni kullanarak çalışma zamanı ortamı temelinde uygulama davranışını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-244">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="a01d4-245">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a01d4-245">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="a01d4-246">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="a01d4-246">Environments</span></span>

<span data-ttu-id="a01d4-247">ASP.NET Core, uygulama başlangıcında `ASPNETCORE_ENVIRONMENT` ortam değişkenini okur ve değeri [ıhostingenvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName)içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="a01d4-247">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="a01d4-248">`ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir, ancak Framework tarafından üç değer sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-248">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="a01d4-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="a01d4-249"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="a01d4-250">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="a01d4-250">The preceding code:</span></span>

* <span data-ttu-id="a01d4-251">`ASPNETCORE_ENVIRONMENT` `Development`olarak ayarlandığında, [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) çağırır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-251">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="a01d4-252">`ASPNETCORE_ENVIRONMENT` değeri aşağıdakilerden birini ayarladığınızda [Useexceptionhandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-252">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="a01d4-253">[Ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) , öğesinde biçimlendirme eklemek veya dışlamak için `IHostingEnvironment.EnvironmentName` değerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-253">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="a01d4-254">Windows ve macOS 'ta, ortam değişkenleri ve değerleri büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-254">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="a01d4-255">Linux ortam değişkenleri ve değerleri varsayılan olarak **büyük/küçük harfe duyarlıdır** .</span><span class="sxs-lookup"><span data-stu-id="a01d4-255">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="a01d4-256">Geliştirme</span><span class="sxs-lookup"><span data-stu-id="a01d4-256">Development</span></span>

<span data-ttu-id="a01d4-257">Geliştirme ortamı, üretimde gösterilmemelidir özellikleri etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-257">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="a01d4-258">Örneğin, ASP.NET Core Şablonlar geliştirme ortamında [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling#developer-exception-page) etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-258">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="a01d4-259">Yerel makine geliştirme ortamı, projenin *Properties\launchSettings.JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-259">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="a01d4-260">*Launchsettings. JSON* geçersiz kılma değerlerini sistem ortamında ayarlanan ortam değerleri.</span><span class="sxs-lookup"><span data-stu-id="a01d4-260">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="a01d4-261">Aşağıdaki JSON, bir *Launchsettings. JSON* dosyasından üç profil gösterir:</span><span class="sxs-lookup"><span data-stu-id="a01d4-261">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="a01d4-262">*Launchsettings. JSON* içindeki `applicationUrl` özelliği sunucu URL 'lerinin bir listesini belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-262">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="a01d4-263">Listedeki URL 'Ler arasında noktalı virgül kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-263">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="a01d4-264">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında, `"commandName": "Project"` ilk profili kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-264">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="a01d4-265">`commandName` değeri, başlatılacak Web sunucusunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-265">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="a01d4-266">`commandName` aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="a01d4-266">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="a01d4-267">`Project` (Kestrel Başlatan)</span><span class="sxs-lookup"><span data-stu-id="a01d4-267">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="a01d4-268">Bir uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında:</span><span class="sxs-lookup"><span data-stu-id="a01d4-268">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="a01d4-269">*Launchsettings. JSON* varsa okundu.</span><span class="sxs-lookup"><span data-stu-id="a01d4-269">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="a01d4-270">*launchsettings. JSON* geçersiz kılma ortamı değişkenlerine `environmentVariables` ayarları.</span><span class="sxs-lookup"><span data-stu-id="a01d4-270">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="a01d4-271">Barındırma ortamı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-271">The hosting environment is displayed.</span></span>

<span data-ttu-id="a01d4-272">Aşağıdaki çıktıda, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatılan bir uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="a01d4-272">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="a01d4-273">Visual Studio proje özellikleri **hata ayıklama** sekmesi, *launchsettings. JSON* dosyasını düzenlemek için bir GUI sağlar:</span><span class="sxs-lookup"><span data-stu-id="a01d4-273">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proje özellikleri ayar ortamı değişkenleri](environments/_static/project-properties-debug.png)

<span data-ttu-id="a01d4-275">Proje profillerinde yapılan değişiklikler, Web sunucusu yeniden başlatılana kadar etkili olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-275">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="a01d4-276">Kestrel, ortamında yapılan değişiklikleri algılayabilmesi için yeniden başlatılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-276">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="a01d4-277">*Launchsettings. JSON* gizli dizileri depolamamamalıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-277">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="a01d4-278">Gizli anahtar geliştirme için gizli dizileri depolamak için [gizli dizi Yöneticisi aracı](xref:security/app-secrets) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-278">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="a01d4-279">[Visual Studio Code](https://code.visualstudio.com/)kullanırken, ortam değişkenleri *. vscode/Launch. JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-279">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="a01d4-280">Aşağıdaki örnek, `Development`için ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="a01d4-280">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="a01d4-281">Uygulama `dotnet run` *Özellikler/launchSettings. JSON*ile aynı şekilde başlatılırken, projedeki bir *. vscode/Launch. JSON* dosyası okunamaz.</span><span class="sxs-lookup"><span data-stu-id="a01d4-281">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="a01d4-282">Geliştirme sırasında *Launchsettings. JSON* dosyası olmayan bir uygulama başlatırken, ortam değişkeni veya bir komut satırı bağımsız değişkeni olan ortamı `dotnet run` komutuna ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-282">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="a01d4-283">Üretim</span><span class="sxs-lookup"><span data-stu-id="a01d4-283">Production</span></span>

<span data-ttu-id="a01d4-284">Üretim ortamının güvenliği, performansı ve uygulama sağlamlık düzeyini en üst düzeye çıkarmak için yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-284">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="a01d4-285">Geliştirmeden farklı bazı yaygın ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-285">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="a01d4-286">Önbelleği.</span><span class="sxs-lookup"><span data-stu-id="a01d4-286">Caching.</span></span>
* <span data-ttu-id="a01d4-287">İstemci tarafı kaynaklar paketlenmiş, küçültülmüş ve potansiyel olarak bir CDN 'den sunulan.</span><span class="sxs-lookup"><span data-stu-id="a01d4-287">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="a01d4-288">Tanılama hata sayfaları devre dışı.</span><span class="sxs-lookup"><span data-stu-id="a01d4-288">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="a01d4-289">Kolay hata sayfaları etkin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-289">Friendly error pages enabled.</span></span>
* <span data-ttu-id="a01d4-290">Üretim günlüğü ve izleme etkin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-290">Production logging and monitoring enabled.</span></span> <span data-ttu-id="a01d4-291">Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="a01d4-291">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="a01d4-292">Ortamı ayarlama</span><span class="sxs-lookup"><span data-stu-id="a01d4-292">Set the environment</span></span>

<span data-ttu-id="a01d4-293">Bir ortam değişkeni veya platform ayarıyla test için belirli bir ortam ayarlamak genellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-293">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="a01d4-294">Ortam ayarlanmamışsa, çoğu hata ayıklama özelliğini devre dışı bırakan `Production`varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-294">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="a01d4-295">Ortamı ayarlama yöntemi işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-295">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="a01d4-296">Konak yapılandırıldığında, uygulama tarafından okunan son ortam ayarı, uygulamanın ortamını belirler.</span><span class="sxs-lookup"><span data-stu-id="a01d4-296">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="a01d4-297">Uygulama çalışırken uygulamanın ortamı değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="a01d4-297">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="a01d4-298">Ortam değişkeni veya platform ayarı</span><span class="sxs-lookup"><span data-stu-id="a01d4-298">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="a01d4-299">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="a01d4-299">Azure App Service</span></span>

<span data-ttu-id="a01d4-300">[Azure App Service](https://azure.microsoft.com/services/app-service/)ortamında ortamı ayarlamak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="a01d4-300">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="a01d4-301">Uygulama **Hizmetleri** dikey penceresinden uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-301">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="a01d4-302">**Ayarlar** grubunda **yapılandırma** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-302">In the **Settings** group, select the **Configuration** blade.</span></span>
1. <span data-ttu-id="a01d4-303">**Uygulama ayarları** sekmesinde **Yeni uygulama ayarı**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-303">In the **Application settings** tab, select **New application setting**.</span></span>
1. <span data-ttu-id="a01d4-304">**Uygulama ayarı Ekle/Düzenle** penceresinde **ad**için `ASPNETCORE_ENVIRONMENT` sağlayın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-304">In the **Add/Edit application setting** window, provide `ASPNETCORE_ENVIRONMENT` for the **Name**.</span></span> <span data-ttu-id="a01d4-305">**Değer**için ortamı sağlayın (örneğin, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="a01d4-305">For **Value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="a01d4-306">Dağıtım yuvaları takas edildiğinde ortam ayarının geçerli yuvada kalmasını istiyorsanız **dağıtım yuvası ayarını** onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-306">Select the **Deployment slot setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="a01d4-307">Daha fazla bilgi için bkz. Azure belgelerindeki [Azure App Service hazırlama ortamlarını ayarlama](/azure/app-service/web-sites-staged-publishing) .</span><span class="sxs-lookup"><span data-stu-id="a01d4-307">For more information, see [Set up staging environments in Azure App Service](/azure/app-service/web-sites-staged-publishing) in the Azure documentation.</span></span>
1. <span data-ttu-id="a01d4-308">**Tamam ' ı** seçerek **uygulama ayarı Ekle/Düzenle** penceresini kapatın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-308">Select **OK** to close the **Add/Edit application setting** window.</span></span>
1. <span data-ttu-id="a01d4-309">**Yapılandırma** dikey penceresinin en üstünde **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-309">Select **Save** at the top of the **Configuration** blade.</span></span>

<span data-ttu-id="a01d4-310">Azure App Service, Azure portal bir uygulama ayarı (ortam değişkeni) eklendikten, değiştirildikten veya silindikten sonra uygulamayı otomatik olarak yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-310">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="a01d4-311">Windows</span><span class="sxs-lookup"><span data-stu-id="a01d4-311">Windows</span></span>

<span data-ttu-id="a01d4-312">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)kullanılarak başlatıldığında geçerli oturumun `ASPNETCORE_ENVIRONMENT` ayarlamak için aşağıdaki komutlar kullanılır:</span><span class="sxs-lookup"><span data-stu-id="a01d4-312">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="a01d4-313">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="a01d4-313">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="a01d4-314">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a01d4-314">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="a01d4-315">Bu komutlar yalnızca geçerli pencere için etkili olur.</span><span class="sxs-lookup"><span data-stu-id="a01d4-315">These commands only take effect for the current window.</span></span> <span data-ttu-id="a01d4-316">Pencere kapatıldığında, `ASPNETCORE_ENVIRONMENT` ayarı varsayılan ayar veya makine değerine geri döner.</span><span class="sxs-lookup"><span data-stu-id="a01d4-316">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="a01d4-317">Windows 'da genel değeri ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-317">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="a01d4-318">**Sistem** > **gelişmiş sistem ayarları** > **denetim masası** 'nı açın ve `ASPNETCORE_ENVIRONMENT` değerini ekleyin veya düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="a01d4-318">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

  ![ASPNET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="a01d4-321">Bir yönetim komut istemi açın ve `setx` komutunu kullanın veya bir yönetim PowerShell komut istemi açın ve `[Environment]::SetEnvironmentVariable`kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-321">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="a01d4-322">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="a01d4-322">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="a01d4-323">`/M` anahtarı, ortam değişkenini sistem düzeyinde ayarlamaya yönelik olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-323">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="a01d4-324">`/M` anahtarı kullanılmazsa, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-324">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="a01d4-325">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a01d4-325">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="a01d4-326">`Machine` seçenek değeri, ortam değişkeninin sistem düzeyinde ayarlandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-326">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="a01d4-327">Seçenek değeri `User`olarak değiştirilirse, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-327">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="a01d4-328">`ASPNETCORE_ENVIRONMENT` ortam değişkeni genel olarak ayarlandığında, değer ayarlandıktan sonra açılan herhangi bir komut penceresinde `dotnet run` için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="a01d4-328">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="a01d4-329">**Web. config**</span><span class="sxs-lookup"><span data-stu-id="a01d4-329">**web.config**</span></span>

<span data-ttu-id="a01d4-330">`ASPNETCORE_ENVIRONMENT` ortam değişkenini *Web. config*ile ayarlamak için, <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>*ortam değişkenlerini ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-330">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="a01d4-331">**Proje dosyası veya yayımlama profili**</span><span class="sxs-lookup"><span data-stu-id="a01d4-331">**Project file or publish profile**</span></span>

<span data-ttu-id="a01d4-332">**WINDOWS IIS dağıtımları için:** `<EnvironmentName>` özelliğini [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-332">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="a01d4-333">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="a01d4-333">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="a01d4-334">**IIS uygulama havuzu başına**</span><span class="sxs-lookup"><span data-stu-id="a01d4-334">**Per IIS Application Pool**</span></span>

<span data-ttu-id="a01d4-335">Yalıtılmış uygulama havuzunda çalışan bir uygulamanın `ASPNETCORE_ENVIRONMENT` ortam değişkenini ayarlamak için (IIS 10,0 veya üzeri sürümlerde desteklenir), [ortam değişkenlerinin &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konusunun *Appcmd. exe komut* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-335">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="a01d4-336">`ASPNETCORE_ENVIRONMENT` ortam değişkeni bir uygulama havuzu için ayarlandığında, değeri sistem düzeyindeki bir ayarı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="a01d4-336">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a01d4-337">IIS 'de bir uygulama barındırırken ve `ASPNETCORE_ENVIRONMENT` ortam değişkenini ekleyerek veya değiştirirken, yeni değerin uygulamalar tarafından çekilmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-337">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="a01d4-338">`net stop was /y` ve ardından komut isteminden `net start w3svc` yürütün.</span><span class="sxs-lookup"><span data-stu-id="a01d4-338">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="a01d4-339">Sunucuyu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-339">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="a01d4-340">macOS</span><span class="sxs-lookup"><span data-stu-id="a01d4-340">macOS</span></span>

<span data-ttu-id="a01d4-341">MacOS için geçerli ortamın ayarlanması, uygulamayı çalıştırırken satır içinde gerçekleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="a01d4-341">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="a01d4-342">Alternatif olarak, uygulamayı çalıştırmadan önce ortamı `export` ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-342">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="a01d4-343">Makine düzeyinde ortam değişkenleri *. bashrc* veya *. bash_profile* dosyasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-343">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="a01d4-344">Herhangi bir metin düzenleyicisini kullanarak dosyayı düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="a01d4-344">Edit the file using any text editor.</span></span> <span data-ttu-id="a01d4-345">Aşağıdaki ifadeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a01d4-345">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="a01d4-346">Linux</span><span class="sxs-lookup"><span data-stu-id="a01d4-346">Linux</span></span>

<span data-ttu-id="a01d4-347">Linux distros için, oturum tabanlı değişken ayarları ve makine düzeyindeki ortam ayarları için *bash_profile* dosyası için bir komut isteminde `export` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-347">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="a01d4-348">Kodda ortam ayarlama</span><span class="sxs-lookup"><span data-stu-id="a01d4-348">Set the environment in code</span></span>

<span data-ttu-id="a01d4-349">Konağı oluştururken <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-349">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="a01d4-350">Bkz. <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="a01d4-350">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="a01d4-351">Ortama göre yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a01d4-351">Configuration by environment</span></span>

<span data-ttu-id="a01d4-352">Yapılandırmayı ortama göre yüklemek için şunları yapmanızı öneririz:</span><span class="sxs-lookup"><span data-stu-id="a01d4-352">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="a01d4-353">*appSettings* dosyaları (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="a01d4-353">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="a01d4-354">Bkz. <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="a01d4-354">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="a01d4-355">Ortam değişkenleri (uygulamanın barındırıldığı her bir sistemde ayarlanır).</span><span class="sxs-lookup"><span data-stu-id="a01d4-355">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="a01d4-356">Bkz. <xref:fundamentals/host/web-host#environment> ve <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="a01d4-356">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="a01d4-357">Gizli dizi Yöneticisi (yalnızca geliştirme ortamında).</span><span class="sxs-lookup"><span data-stu-id="a01d4-357">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="a01d4-358">Bkz. <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="a01d4-358">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="a01d4-359">Ortam tabanlı başlangıç sınıfı ve yöntemleri</span><span class="sxs-lookup"><span data-stu-id="a01d4-359">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="a01d4-360">Başlangıç olarak ıhostingenvironment ekleme. yapılandırma</span><span class="sxs-lookup"><span data-stu-id="a01d4-360">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="a01d4-361"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> `Startup.Configure`ekleme.</span><span class="sxs-lookup"><span data-stu-id="a01d4-361">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="a01d4-362">Bu yaklaşım, uygulama yalnızca, ortam başına en az kod farklılığı olan birkaç ortam için `Startup.Configure` yapılandırmayı gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-362">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="a01d4-363">Başlangıç sınıfına ıhostingenvironment ekleme</span><span class="sxs-lookup"><span data-stu-id="a01d4-363">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="a01d4-364">`Startup` oluşturucusuna <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> ekleyin ve hizmeti `Startup` sınıfı boyunca kullanmak üzere bir alana atayın.</span><span class="sxs-lookup"><span data-stu-id="a01d4-364">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="a01d4-365">Bu yaklaşım, uygulama her ortam için en az kod farklılığı olan birkaç ortam için başlatma yapılandırması gerektirdiğinde faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-365">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="a01d4-366">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="a01d4-366">In the following example:</span></span>

* <span data-ttu-id="a01d4-367">Ortam `_env` alanında tutulur.</span><span class="sxs-lookup"><span data-stu-id="a01d4-367">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="a01d4-368">`_env`, `ConfigureServices` ve `Configure` uygulamanın ortamına göre başlangıç yapılandırmasını uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-368">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

### <a name="startup-class-conventions"></a><span data-ttu-id="a01d4-369">Başlangıç sınıfı kuralları</span><span class="sxs-lookup"><span data-stu-id="a01d4-369">Startup class conventions</span></span>

<span data-ttu-id="a01d4-370">ASP.NET Core bir uygulama başlatıldığında, [Başlangıç sınıfı](xref:fundamentals/startup) uygulamayı önyükleme.</span><span class="sxs-lookup"><span data-stu-id="a01d4-370">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="a01d4-371">Uygulama farklı ortamlar için ayrı `Startup` sınıfları tanımlayabilir (örneğin, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="a01d4-371">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="a01d4-372">Uygun `Startup` sınıfı çalışma zamanında seçildi.</span><span class="sxs-lookup"><span data-stu-id="a01d4-372">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="a01d4-373">Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir.</span><span class="sxs-lookup"><span data-stu-id="a01d4-373">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="a01d4-374">Eşleşen bir `Startup{EnvironmentName}` sınıfı bulunamazsa, `Startup` sınıfı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-374">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="a01d4-375">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-375">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="a01d4-376">Ortam tabanlı `Startup` sınıfları uygulamak için, kullanımdaki her ortam için bir `Startup{EnvironmentName}` sınıfı ve bir geri dönüş `Startup` sınıfı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="a01d4-376">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="a01d4-377">Derleme adını kabul eden [Usestartup (ıwebhostbuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) aşırı yüklemesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="a01d4-377">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="a01d4-378">Başlangıç yöntemi kuralları</span><span class="sxs-lookup"><span data-stu-id="a01d4-378">Startup method conventions</span></span>

<span data-ttu-id="a01d4-379">[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) , form `Configure<EnvironmentName>` ve `Configure<EnvironmentName>Services`ortama özgü sürümlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="a01d4-379">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="a01d4-380">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a01d4-380">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="a01d4-381">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a01d4-381">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end
