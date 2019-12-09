---
title: ASP.NET Core çoklu ortamları kullanma
author: rick-anderson
description: ASP.NET Core uygulamalarında birden çok ortamda uygulama davranışını denetlemeyi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/07/2019
uid: fundamentals/environments
ms.openlocfilehash: affbb95273c91fe5bf452e0e1ebefa669297304c
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944327"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="19bc9-103">ASP.NET Core çoklu ortamları kullanma</span><span class="sxs-lookup"><span data-stu-id="19bc9-103">Use multiple environments in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="19bc9-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="19bc9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="19bc9-105">ASP.NET Core, bir ortam değişkeni kullanarak çalışma zamanı ortamı temelinde uygulama davranışını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="19bc9-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19bc9-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="19bc9-107">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="19bc9-107">Environments</span></span>

<span data-ttu-id="19bc9-108">ASP.NET Core, uygulama başlangıcında `ASPNETCORE_ENVIRONMENT` ortam değişkenini okur ve değeri [ıwebhostenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="19bc9-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IWebHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName).</span></span> <span data-ttu-id="19bc9-109">`ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir, ancak Framework tarafından üç değer sağlanır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-109">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <span data-ttu-id="19bc9-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="19bc9-110"><xref:Microsoft.Extensions.Hosting.Environments.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="19bc9-111">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="19bc9-111">The preceding code:</span></span>

* <span data-ttu-id="19bc9-112">`ASPNETCORE_ENVIRONMENT` `Development`olarak ayarlandığında, [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) çağırır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="19bc9-113">`ASPNETCORE_ENVIRONMENT` değeri aşağıdakilerden birini ayarladığınızda [Useexceptionhandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="19bc9-114">[Ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) , öğesinde biçimlendirme eklemek veya dışlamak için `IHostingEnvironment.EnvironmentName` değerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="19bc9-115">Windows ve macOS 'ta, ortam değişkenleri ve değerleri büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="19bc9-116">Linux ortam değişkenleri ve değerleri varsayılan olarak **büyük/küçük harfe duyarlıdır** .</span><span class="sxs-lookup"><span data-stu-id="19bc9-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="19bc9-117">Geliştirme</span><span class="sxs-lookup"><span data-stu-id="19bc9-117">Development</span></span>

<span data-ttu-id="19bc9-118">Geliştirme ortamı, üretimde gösterilmemelidir özellikleri etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="19bc9-119">Örneğin, ASP.NET Core Şablonlar geliştirme ortamında [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling#developer-exception-page) etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-119">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="19bc9-120">Yerel makine geliştirme ortamı, projenin *Properties\launchSettings.JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="19bc9-121">*Launchsettings. JSON* geçersiz kılma değerlerini sistem ortamında ayarlanan ortam değerleri.</span><span class="sxs-lookup"><span data-stu-id="19bc9-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="19bc9-122">Aşağıdaki JSON, bir *Launchsettings. JSON* dosyasından üç profil gösterir:</span><span class="sxs-lookup"><span data-stu-id="19bc9-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="19bc9-123">*Launchsettings. JSON* içindeki `applicationUrl` özelliği sunucu URL 'lerinin bir listesini belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="19bc9-124">Listedeki URL 'Ler arasında noktalı virgül kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="19bc9-125">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında, `"commandName": "Project"` ilk profili kullanılır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="19bc9-126">`commandName` değeri, başlatılacak Web sunucusunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="19bc9-127">`commandName` aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="19bc9-127">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="19bc9-128">`Project` (Kestrel Başlatan)</span><span class="sxs-lookup"><span data-stu-id="19bc9-128">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="19bc9-129">Bir uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında:</span><span class="sxs-lookup"><span data-stu-id="19bc9-129">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="19bc9-130">*Launchsettings. JSON* varsa okundu.</span><span class="sxs-lookup"><span data-stu-id="19bc9-130">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="19bc9-131">*launchsettings. JSON* geçersiz kılma ortamı değişkenlerine `environmentVariables` ayarları.</span><span class="sxs-lookup"><span data-stu-id="19bc9-131">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="19bc9-132">Barındırma ortamı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-132">The hosting environment is displayed.</span></span>

<span data-ttu-id="19bc9-133">Aşağıdaki çıktıda, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatılan bir uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="19bc9-133">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="19bc9-134">Visual Studio proje özellikleri **hata ayıklama** sekmesi, *launchsettings. JSON* dosyasını düzenlemek için bir GUI sağlar:</span><span class="sxs-lookup"><span data-stu-id="19bc9-134">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proje özellikleri ayar ortamı değişkenleri](environments/_static/project-properties-debug.png)

<span data-ttu-id="19bc9-136">Proje profillerinde yapılan değişiklikler, Web sunucusu yeniden başlatılana kadar etkili olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-136">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="19bc9-137">Kestrel, ortamında yapılan değişiklikleri algılayabilmesi için yeniden başlatılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-137">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="19bc9-138">*Launchsettings. JSON* gizli dizileri depolamamamalıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-138">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="19bc9-139">Gizli anahtar geliştirme için gizli dizileri depolamak için [gizli dizi Yöneticisi aracı](xref:security/app-secrets) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-139">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="19bc9-140">[Visual Studio Code](https://code.visualstudio.com/)kullanırken, ortam değişkenleri *. vscode/Launch. JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-140">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="19bc9-141">Aşağıdaki örnek, `Development`için ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="19bc9-141">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="19bc9-142">Uygulama `dotnet run` *Özellikler/launchSettings. JSON*ile aynı şekilde başlatılırken, projedeki bir *. vscode/Launch. JSON* dosyası okunamaz.</span><span class="sxs-lookup"><span data-stu-id="19bc9-142">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="19bc9-143">Geliştirme sırasında *Launchsettings. JSON* dosyası olmayan bir uygulama başlatırken, ortam değişkeni veya bir komut satırı bağımsız değişkeni olan ortamı `dotnet run` komutuna ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-143">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="19bc9-144">Üretim</span><span class="sxs-lookup"><span data-stu-id="19bc9-144">Production</span></span>

<span data-ttu-id="19bc9-145">Üretim ortamının güvenliği, performansı ve uygulama sağlamlık düzeyini en üst düzeye çıkarmak için yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-145">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="19bc9-146">Geliştirmeden farklı bazı yaygın ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-146">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="19bc9-147">Önbelleği.</span><span class="sxs-lookup"><span data-stu-id="19bc9-147">Caching.</span></span>
* <span data-ttu-id="19bc9-148">İstemci tarafı kaynaklar paketlenmiş, küçültülmüş ve potansiyel olarak bir CDN 'den sunulan.</span><span class="sxs-lookup"><span data-stu-id="19bc9-148">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="19bc9-149">Tanılama hata sayfaları devre dışı.</span><span class="sxs-lookup"><span data-stu-id="19bc9-149">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="19bc9-150">Kolay hata sayfaları etkin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-150">Friendly error pages enabled.</span></span>
* <span data-ttu-id="19bc9-151">Üretim günlüğü ve izleme etkin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-151">Production logging and monitoring enabled.</span></span> <span data-ttu-id="19bc9-152">Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="19bc9-152">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="19bc9-153">Ortamı ayarlama</span><span class="sxs-lookup"><span data-stu-id="19bc9-153">Set the environment</span></span>

<span data-ttu-id="19bc9-154">Bir ortam değişkeni veya platform ayarıyla test için belirli bir ortam ayarlamak genellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-154">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="19bc9-155">Ortam ayarlanmamışsa, çoğu hata ayıklama özelliğini devre dışı bırakan `Production`varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-155">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="19bc9-156">Ortamı ayarlama yöntemi işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-156">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="19bc9-157">Konak yapılandırıldığında, uygulama tarafından okunan son ortam ayarı, uygulamanın ortamını belirler.</span><span class="sxs-lookup"><span data-stu-id="19bc9-157">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="19bc9-158">Uygulama çalışırken uygulamanın ortamı değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="19bc9-158">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="19bc9-159">Ortam değişkeni veya platform ayarı</span><span class="sxs-lookup"><span data-stu-id="19bc9-159">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="19bc9-160">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="19bc9-160">Azure App Service</span></span>

<span data-ttu-id="19bc9-161">[Azure App Service](https://azure.microsoft.com/services/app-service/)ortamında ortamı ayarlamak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="19bc9-161">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="19bc9-162">Uygulama **Hizmetleri** dikey penceresinden uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-162">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="19bc9-163">**Ayarlar** grubunda, **uygulama ayarları** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-163">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="19bc9-164">**Uygulama ayarları** alanında **yeni ayar Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-164">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="19bc9-165">**Ad girin**için `ASPNETCORE_ENVIRONMENT`sağlayın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-165">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="19bc9-166">**Değer girin**için ortamı sağlayın (örneğin, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="19bc9-166">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="19bc9-167">Ortam ayarının, dağıtım yuvaları takas edildiğinde geçerli yuvada kalmasını istiyorsanız, **yuva ayarı** onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-167">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="19bc9-168">Daha fazla bilgi için bkz. [Azure belgeleri: hangi ayarları değiştirmiş?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="19bc9-168">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="19bc9-169">Dikey pencerenin en üstünde **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-169">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="19bc9-170">Azure App Service, Azure portal bir uygulama ayarı (ortam değişkeni) eklendikten, değiştirildikten veya silindikten sonra uygulamayı otomatik olarak yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-170">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="19bc9-171">Windows</span><span class="sxs-lookup"><span data-stu-id="19bc9-171">Windows</span></span>

<span data-ttu-id="19bc9-172">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)kullanılarak başlatıldığında geçerli oturumun `ASPNETCORE_ENVIRONMENT` ayarlamak için aşağıdaki komutlar kullanılır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-172">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="19bc9-173">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="19bc9-173">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="19bc9-174">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="19bc9-174">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="19bc9-175">Bu komutlar yalnızca geçerli pencere için etkili olur.</span><span class="sxs-lookup"><span data-stu-id="19bc9-175">These commands only take effect for the current window.</span></span> <span data-ttu-id="19bc9-176">Pencere kapatıldığında, `ASPNETCORE_ENVIRONMENT` ayarı varsayılan ayar veya makine değerine geri döner.</span><span class="sxs-lookup"><span data-stu-id="19bc9-176">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="19bc9-177">Windows 'da genel değeri ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-177">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="19bc9-178">**Sistem** > **gelişmiş sistem ayarları** > **denetim masası** 'nı açın ve `ASPNETCORE_ENVIRONMENT` değerini ekleyin veya düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="19bc9-178">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

  ![ASPNET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="19bc9-181">Bir yönetim komut istemi açın ve `setx` komutunu kullanın veya bir yönetim PowerShell komut istemi açın ve `[Environment]::SetEnvironmentVariable`kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-181">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="19bc9-182">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="19bc9-182">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="19bc9-183">`/M` anahtarı, ortam değişkenini sistem düzeyinde ayarlamaya yönelik olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-183">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="19bc9-184">`/M` anahtarı kullanılmazsa, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-184">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="19bc9-185">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="19bc9-185">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="19bc9-186">`Machine` seçenek değeri, ortam değişkeninin sistem düzeyinde ayarlandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-186">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="19bc9-187">Seçenek değeri `User`olarak değiştirilirse, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-187">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="19bc9-188">`ASPNETCORE_ENVIRONMENT` ortam değişkeni genel olarak ayarlandığında, değer ayarlandıktan sonra açılan herhangi bir komut penceresinde `dotnet run` için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="19bc9-188">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="19bc9-189">**Web. config**</span><span class="sxs-lookup"><span data-stu-id="19bc9-189">**web.config**</span></span>

<span data-ttu-id="19bc9-190">`ASPNETCORE_ENVIRONMENT` ortam değişkenini *Web. config*ile ayarlamak için, <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>*ortam değişkenlerini ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-190">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="19bc9-191">**Proje dosyası veya yayımlama profili**</span><span class="sxs-lookup"><span data-stu-id="19bc9-191">**Project file or publish profile**</span></span>

<span data-ttu-id="19bc9-192">**WINDOWS IIS dağıtımları için:** `<EnvironmentName>` özelliğini [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-192">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="19bc9-193">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="19bc9-193">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="19bc9-194">**IIS uygulama havuzu başına**</span><span class="sxs-lookup"><span data-stu-id="19bc9-194">**Per IIS Application Pool**</span></span>

<span data-ttu-id="19bc9-195">Yalıtılmış uygulama havuzunda çalışan bir uygulamanın `ASPNETCORE_ENVIRONMENT` ortam değişkenini ayarlamak için (IIS 10,0 veya üzeri sürümlerde desteklenir), [ortam değişkenlerinin &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konusunun *Appcmd. exe komut* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-195">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="19bc9-196">`ASPNETCORE_ENVIRONMENT` ortam değişkeni bir uygulama havuzu için ayarlandığında, değeri sistem düzeyindeki bir ayarı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="19bc9-196">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19bc9-197">IIS 'de bir uygulama barındırırken ve `ASPNETCORE_ENVIRONMENT` ortam değişkenini ekleyerek veya değiştirirken, yeni değerin uygulamalar tarafından çekilmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-197">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="19bc9-198">`net stop was /y` ve ardından komut isteminden `net start w3svc` yürütün.</span><span class="sxs-lookup"><span data-stu-id="19bc9-198">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="19bc9-199">Sunucuyu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-199">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="19bc9-200">macOS</span><span class="sxs-lookup"><span data-stu-id="19bc9-200">macOS</span></span>

<span data-ttu-id="19bc9-201">MacOS için geçerli ortamın ayarlanması, uygulamayı çalıştırırken satır içinde gerçekleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="19bc9-201">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="19bc9-202">Alternatif olarak, uygulamayı çalıştırmadan önce ortamı `export` ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-202">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="19bc9-203">Makine düzeyinde ortam değişkenleri *. bashrc* veya *. bash_profile* dosyasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-203">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="19bc9-204">Herhangi bir metin düzenleyicisini kullanarak dosyayı düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-204">Edit the file using any text editor.</span></span> <span data-ttu-id="19bc9-205">Aşağıdaki ifadeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="19bc9-205">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="19bc9-206">Linux</span><span class="sxs-lookup"><span data-stu-id="19bc9-206">Linux</span></span>

<span data-ttu-id="19bc9-207">Linux distros için, oturum tabanlı değişken ayarları ve makine düzeyindeki ortam ayarları için *bash_profile* dosyası için bir komut isteminde `export` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-207">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="19bc9-208">Kodda ortam ayarlama</span><span class="sxs-lookup"><span data-stu-id="19bc9-208">Set the environment in code</span></span>

<span data-ttu-id="19bc9-209">Konağı oluştururken <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-209">Call <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="19bc9-210">Bkz. <xref:fundamentals/host/generic-host#environmentname>.</span><span class="sxs-lookup"><span data-stu-id="19bc9-210">See <xref:fundamentals/host/generic-host#environmentname>.</span></span>


### <a name="configuration-by-environment"></a><span data-ttu-id="19bc9-211">Ortama göre yapılandırma</span><span class="sxs-lookup"><span data-stu-id="19bc9-211">Configuration by environment</span></span>

<span data-ttu-id="19bc9-212">Yapılandırmayı ortama göre yüklemek için şunları yapmanızı öneririz:</span><span class="sxs-lookup"><span data-stu-id="19bc9-212">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="19bc9-213">*appSettings* dosyaları (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="19bc9-213">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="19bc9-214">Bkz. <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="19bc9-214">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="19bc9-215">Ortam değişkenleri (uygulamanın barındırıldığı her bir sistemde ayarlanır).</span><span class="sxs-lookup"><span data-stu-id="19bc9-215">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="19bc9-216">Bkz. <xref:fundamentals/host/generic-host#environmentname> ve <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="19bc9-216">See <xref:fundamentals/host/generic-host#environmentname> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="19bc9-217">Gizli dizi Yöneticisi (yalnızca geliştirme ortamında).</span><span class="sxs-lookup"><span data-stu-id="19bc9-217">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="19bc9-218">Bkz. <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="19bc9-218">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="19bc9-219">Ortam tabanlı başlangıç sınıfı ve yöntemleri</span><span class="sxs-lookup"><span data-stu-id="19bc9-219">Environment-based Startup class and methods</span></span>

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a><span data-ttu-id="19bc9-220">Iwebhostenvironment 'ı başlatmaya ekleme. configure</span><span class="sxs-lookup"><span data-stu-id="19bc9-220">Inject IWebHostEnvironment into Startup.Configure</span></span>

<span data-ttu-id="19bc9-221"><xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> `Startup.Configure`ekleme.</span><span class="sxs-lookup"><span data-stu-id="19bc9-221">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="19bc9-222">Bu yaklaşım, uygulama yalnızca, ortam başına en az kod farklılığı olan birkaç ortam için `Startup.Configure` ayarlamayı gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-222">This approach is useful when the app only requires adjusting `Startup.Configure` for a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a><span data-ttu-id="19bc9-223">Başlangıç sınıfına ıwebhostenvironment ekleme</span><span class="sxs-lookup"><span data-stu-id="19bc9-223">Inject IWebHostEnvironment into the Startup class</span></span>

<span data-ttu-id="19bc9-224">`Startup` oluşturucusuna <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> ekleyin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-224">Inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into the `Startup` constructor.</span></span> <span data-ttu-id="19bc9-225">Bu yaklaşım, uygulama her ortam için en az kod farklılığı olan birkaç ortam için `Startup` yapılandırmayı gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-225">This approach is useful when the app requires configuring `Startup` for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="19bc9-226">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="19bc9-226">In the following example:</span></span>

* <span data-ttu-id="19bc9-227">Ortam `_env` alanında tutulur.</span><span class="sxs-lookup"><span data-stu-id="19bc9-227">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="19bc9-228">`_env`, `ConfigureServices` ve `Configure` uygulamanın ortamına göre başlangıç yapılandırmasını uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-228">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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
### <a name="startup-class-conventions"></a><span data-ttu-id="19bc9-229">Başlangıç sınıfı kuralları</span><span class="sxs-lookup"><span data-stu-id="19bc9-229">Startup class conventions</span></span>

<span data-ttu-id="19bc9-230">ASP.NET Core bir uygulama başlatıldığında, [Başlangıç sınıfı](xref:fundamentals/startup) uygulamayı önyükleme.</span><span class="sxs-lookup"><span data-stu-id="19bc9-230">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="19bc9-231">Uygulama farklı ortamlar için ayrı `Startup` sınıfları tanımlayabilir (örneğin, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="19bc9-231">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="19bc9-232">Uygun `Startup` sınıfı çalışma zamanında seçildi.</span><span class="sxs-lookup"><span data-stu-id="19bc9-232">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="19bc9-233">Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-233">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="19bc9-234">Eşleşen bir `Startup{EnvironmentName}` sınıfı bulunamazsa, `Startup` sınıfı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-234">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="19bc9-235">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-235">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="19bc9-236">Ortam tabanlı `Startup` sınıfları uygulamak için, kullanımdaki her ortam için bir `Startup{EnvironmentName}` sınıfı ve bir geri dönüş `Startup` sınıfı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="19bc9-236">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="19bc9-237">Derleme adını kabul eden [Usestartup (ıwebhostbuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) aşırı yüklemesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-237">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="19bc9-238">Başlangıç yöntemi kuralları</span><span class="sxs-lookup"><span data-stu-id="19bc9-238">Startup method conventions</span></span>

<span data-ttu-id="19bc9-239">[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) , form `Configure<EnvironmentName>` ve `Configure<EnvironmentName>Services`ortama özgü sürümlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="19bc9-239">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="19bc9-240">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-240">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="19bc9-241">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="19bc9-241">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="19bc9-242">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="19bc9-242">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="19bc9-243">ASP.NET Core, bir ortam değişkeni kullanarak çalışma zamanı ortamı temelinde uygulama davranışını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-243">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="19bc9-244">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19bc9-244">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="19bc9-245">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="19bc9-245">Environments</span></span>

<span data-ttu-id="19bc9-246">ASP.NET Core, uygulama başlangıcında `ASPNETCORE_ENVIRONMENT` ortam değişkenini okur ve değeri [ıhostingenvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName)içinde depolar.</span><span class="sxs-lookup"><span data-stu-id="19bc9-246">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName).</span></span> <span data-ttu-id="19bc9-247">`ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir, ancak Framework tarafından üç değer sağlanır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-247">`ASPNETCORE_ENVIRONMENT` can be set to any value, but three values are provided by the framework:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <span data-ttu-id="19bc9-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="19bc9-248"><xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (default)</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="19bc9-249">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="19bc9-249">The preceding code:</span></span>

* <span data-ttu-id="19bc9-250">`ASPNETCORE_ENVIRONMENT` `Development`olarak ayarlandığında, [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) çağırır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-250">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="19bc9-251">`ASPNETCORE_ENVIRONMENT` değeri aşağıdakilerden birini ayarladığınızda [Useexceptionhandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) ' i çağırır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-251">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

  * `Staging`
  * `Production`
  * `Staging_2`

<span data-ttu-id="19bc9-252">[Ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) , öğesinde biçimlendirme eklemek veya dışlamak için `IHostingEnvironment.EnvironmentName` değerini kullanır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-252">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="19bc9-253">Windows ve macOS 'ta, ortam değişkenleri ve değerleri büyük/küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-253">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="19bc9-254">Linux ortam değişkenleri ve değerleri varsayılan olarak **büyük/küçük harfe duyarlıdır** .</span><span class="sxs-lookup"><span data-stu-id="19bc9-254">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="19bc9-255">Geliştirme</span><span class="sxs-lookup"><span data-stu-id="19bc9-255">Development</span></span>

<span data-ttu-id="19bc9-256">Geliştirme ortamı, üretimde gösterilmemelidir özellikleri etkinleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-256">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="19bc9-257">Örneğin, ASP.NET Core Şablonlar geliştirme ortamında [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling#developer-exception-page) etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-257">For example, the ASP.NET Core templates enable the [Developer Exception Page](xref:fundamentals/error-handling#developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="19bc9-258">Yerel makine geliştirme ortamı, projenin *Properties\launchSettings.JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-258">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="19bc9-259">*Launchsettings. JSON* geçersiz kılma değerlerini sistem ortamında ayarlanan ortam değerleri.</span><span class="sxs-lookup"><span data-stu-id="19bc9-259">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="19bc9-260">Aşağıdaki JSON, bir *Launchsettings. JSON* dosyasından üç profil gösterir:</span><span class="sxs-lookup"><span data-stu-id="19bc9-260">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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
> <span data-ttu-id="19bc9-261">*Launchsettings. JSON* içindeki `applicationUrl` özelliği sunucu URL 'lerinin bir listesini belirtebilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-261">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="19bc9-262">Listedeki URL 'Ler arasında noktalı virgül kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-262">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="19bc9-263">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında, `"commandName": "Project"` ilk profili kullanılır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-263">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="19bc9-264">`commandName` değeri, başlatılacak Web sunucusunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-264">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="19bc9-265">`commandName` aşağıdakilerden biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="19bc9-265">`commandName` can be any one of the following:</span></span>

* `IISExpress`
* `IIS`
* <span data-ttu-id="19bc9-266">`Project` (Kestrel Başlatan)</span><span class="sxs-lookup"><span data-stu-id="19bc9-266">`Project` (which launches Kestrel)</span></span>

<span data-ttu-id="19bc9-267">Bir uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında:</span><span class="sxs-lookup"><span data-stu-id="19bc9-267">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="19bc9-268">*Launchsettings. JSON* varsa okundu.</span><span class="sxs-lookup"><span data-stu-id="19bc9-268">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="19bc9-269">*launchsettings. JSON* geçersiz kılma ortamı değişkenlerine `environmentVariables` ayarları.</span><span class="sxs-lookup"><span data-stu-id="19bc9-269">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="19bc9-270">Barındırma ortamı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-270">The hosting environment is displayed.</span></span>

<span data-ttu-id="19bc9-271">Aşağıdaki çıktıda, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatılan bir uygulama gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="19bc9-271">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="19bc9-272">Visual Studio proje özellikleri **hata ayıklama** sekmesi, *launchsettings. JSON* dosyasını düzenlemek için bir GUI sağlar:</span><span class="sxs-lookup"><span data-stu-id="19bc9-272">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proje özellikleri ayar ortamı değişkenleri](environments/_static/project-properties-debug.png)

<span data-ttu-id="19bc9-274">Proje profillerinde yapılan değişiklikler, Web sunucusu yeniden başlatılana kadar etkili olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-274">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="19bc9-275">Kestrel, ortamında yapılan değişiklikleri algılayabilmesi için yeniden başlatılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-275">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="19bc9-276">*Launchsettings. JSON* gizli dizileri depolamamamalıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-276">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="19bc9-277">Gizli anahtar geliştirme için gizli dizileri depolamak için [gizli dizi Yöneticisi aracı](xref:security/app-secrets) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-277">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="19bc9-278">[Visual Studio Code](https://code.visualstudio.com/)kullanırken, ortam değişkenleri *. vscode/Launch. JSON* dosyasında ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-278">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="19bc9-279">Aşağıdaki örnek, `Development`için ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="19bc9-279">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="19bc9-280">Uygulama `dotnet run` *Özellikler/launchSettings. JSON*ile aynı şekilde başlatılırken, projedeki bir *. vscode/Launch. JSON* dosyası okunamaz.</span><span class="sxs-lookup"><span data-stu-id="19bc9-280">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="19bc9-281">Geliştirme sırasında *Launchsettings. JSON* dosyası olmayan bir uygulama başlatırken, ortam değişkeni veya bir komut satırı bağımsız değişkeni olan ortamı `dotnet run` komutuna ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-281">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="19bc9-282">Üretim</span><span class="sxs-lookup"><span data-stu-id="19bc9-282">Production</span></span>

<span data-ttu-id="19bc9-283">Üretim ortamının güvenliği, performansı ve uygulama sağlamlık düzeyini en üst düzeye çıkarmak için yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-283">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="19bc9-284">Geliştirmeden farklı bazı yaygın ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-284">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="19bc9-285">Önbelleği.</span><span class="sxs-lookup"><span data-stu-id="19bc9-285">Caching.</span></span>
* <span data-ttu-id="19bc9-286">İstemci tarafı kaynaklar paketlenmiş, küçültülmüş ve potansiyel olarak bir CDN 'den sunulan.</span><span class="sxs-lookup"><span data-stu-id="19bc9-286">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="19bc9-287">Tanılama hata sayfaları devre dışı.</span><span class="sxs-lookup"><span data-stu-id="19bc9-287">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="19bc9-288">Kolay hata sayfaları etkin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-288">Friendly error pages enabled.</span></span>
* <span data-ttu-id="19bc9-289">Üretim günlüğü ve izleme etkin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-289">Production logging and monitoring enabled.</span></span> <span data-ttu-id="19bc9-290">Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="19bc9-290">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="19bc9-291">Ortamı ayarlama</span><span class="sxs-lookup"><span data-stu-id="19bc9-291">Set the environment</span></span>

<span data-ttu-id="19bc9-292">Bir ortam değişkeni veya platform ayarıyla test için belirli bir ortam ayarlamak genellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-292">It's often useful to set a specific environment for testing with an environment variable or platform setting.</span></span> <span data-ttu-id="19bc9-293">Ortam ayarlanmamışsa, çoğu hata ayıklama özelliğini devre dışı bırakan `Production`varsayılan olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-293">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="19bc9-294">Ortamı ayarlama yöntemi işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-294">The method for setting the environment depends on the operating system.</span></span>

<span data-ttu-id="19bc9-295">Konak yapılandırıldığında, uygulama tarafından okunan son ortam ayarı, uygulamanın ortamını belirler.</span><span class="sxs-lookup"><span data-stu-id="19bc9-295">When the host is built, the last environment setting read by the app determines the app's environment.</span></span> <span data-ttu-id="19bc9-296">Uygulama çalışırken uygulamanın ortamı değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="19bc9-296">The app's environment can't be changed while the app is running.</span></span>

### <a name="environment-variable-or-platform-setting"></a><span data-ttu-id="19bc9-297">Ortam değişkeni veya platform ayarı</span><span class="sxs-lookup"><span data-stu-id="19bc9-297">Environment variable or platform setting</span></span>

#### <a name="azure-app-service"></a><span data-ttu-id="19bc9-298">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="19bc9-298">Azure App Service</span></span>

<span data-ttu-id="19bc9-299">[Azure App Service](https://azure.microsoft.com/services/app-service/)ortamında ortamı ayarlamak için aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="19bc9-299">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="19bc9-300">Uygulama **Hizmetleri** dikey penceresinden uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-300">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="19bc9-301">**Ayarlar** grubunda, **uygulama ayarları** dikey penceresini seçin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-301">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="19bc9-302">**Uygulama ayarları** alanında **yeni ayar Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-302">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="19bc9-303">**Ad girin**için `ASPNETCORE_ENVIRONMENT`sağlayın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-303">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="19bc9-304">**Değer girin**için ortamı sağlayın (örneğin, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="19bc9-304">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="19bc9-305">Ortam ayarının, dağıtım yuvaları takas edildiğinde geçerli yuvada kalmasını istiyorsanız, **yuva ayarı** onay kutusunu işaretleyin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-305">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="19bc9-306">Daha fazla bilgi için bkz. [Azure belgeleri: hangi ayarları değiştirmiş?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="19bc9-306">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="19bc9-307">Dikey pencerenin en üstünde **Kaydet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-307">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="19bc9-308">Azure App Service, Azure portal bir uygulama ayarı (ortam değişkeni) eklendikten, değiştirildikten veya silindikten sonra uygulamayı otomatik olarak yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-308">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

#### <a name="windows"></a><span data-ttu-id="19bc9-309">Windows</span><span class="sxs-lookup"><span data-stu-id="19bc9-309">Windows</span></span>

<span data-ttu-id="19bc9-310">Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)kullanılarak başlatıldığında geçerli oturumun `ASPNETCORE_ENVIRONMENT` ayarlamak için aşağıdaki komutlar kullanılır:</span><span class="sxs-lookup"><span data-stu-id="19bc9-310">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="19bc9-311">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="19bc9-311">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="19bc9-312">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="19bc9-312">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="19bc9-313">Bu komutlar yalnızca geçerli pencere için etkili olur.</span><span class="sxs-lookup"><span data-stu-id="19bc9-313">These commands only take effect for the current window.</span></span> <span data-ttu-id="19bc9-314">Pencere kapatıldığında, `ASPNETCORE_ENVIRONMENT` ayarı varsayılan ayar veya makine değerine geri döner.</span><span class="sxs-lookup"><span data-stu-id="19bc9-314">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span>

<span data-ttu-id="19bc9-315">Windows 'da genel değeri ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-315">To set the value globally in Windows, use either of the following approaches:</span></span>

* <span data-ttu-id="19bc9-316">**Sistem** > **gelişmiş sistem ayarları** > **denetim masası** 'nı açın ve `ASPNETCORE_ENVIRONMENT` değerini ekleyin veya düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="19bc9-316">Open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

  ![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

  ![ASPNET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

* <span data-ttu-id="19bc9-319">Bir yönetim komut istemi açın ve `setx` komutunu kullanın veya bir yönetim PowerShell komut istemi açın ve `[Environment]::SetEnvironmentVariable`kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-319">Open an administrative command prompt and use the `setx` command or open an administrative PowerShell command prompt and use `[Environment]::SetEnvironmentVariable`:</span></span>

  <span data-ttu-id="19bc9-320">**Komut istemi**</span><span class="sxs-lookup"><span data-stu-id="19bc9-320">**Command prompt**</span></span>

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  <span data-ttu-id="19bc9-321">`/M` anahtarı, ortam değişkenini sistem düzeyinde ayarlamaya yönelik olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-321">The `/M` switch indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="19bc9-322">`/M` anahtarı kullanılmazsa, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-322">If the `/M` switch isn't used, the environment variable is set for the user account.</span></span>

  <span data-ttu-id="19bc9-323">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="19bc9-323">**PowerShell**</span></span>

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  <span data-ttu-id="19bc9-324">`Machine` seçenek değeri, ortam değişkeninin sistem düzeyinde ayarlandığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-324">The `Machine` option value indicates to set the environment variable at the system level.</span></span> <span data-ttu-id="19bc9-325">Seçenek değeri `User`olarak değiştirilirse, ortam değişkeni Kullanıcı hesabı için ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-325">If the option value is changed to `User`, the environment variable is set for the user account.</span></span>

<span data-ttu-id="19bc9-326">`ASPNETCORE_ENVIRONMENT` ortam değişkeni genel olarak ayarlandığında, değer ayarlandıktan sonra açılan herhangi bir komut penceresinde `dotnet run` için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="19bc9-326">When the `ASPNETCORE_ENVIRONMENT` environment variable is set globally, it takes effect for `dotnet run` in any command window opened after the value is set.</span></span>

<span data-ttu-id="19bc9-327">**Web. config**</span><span class="sxs-lookup"><span data-stu-id="19bc9-327">**web.config**</span></span>

<span data-ttu-id="19bc9-328">`ASPNETCORE_ENVIRONMENT` ortam değişkenini *Web. config*ile ayarlamak için, <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>*ortam değişkenlerini ayarlama* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-328">To set the `ASPNETCORE_ENVIRONMENT` environment variable with *web.config*, see the *Setting environment variables* section of <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>.</span></span>

<span data-ttu-id="19bc9-329">**Proje dosyası veya yayımlama profili**</span><span class="sxs-lookup"><span data-stu-id="19bc9-329">**Project file or publish profile**</span></span>

<span data-ttu-id="19bc9-330">**WINDOWS IIS dağıtımları için:** `<EnvironmentName>` özelliğini [Publish profile (. pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) veya proje dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-330">**For Windows IIS deployments:** Include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="19bc9-331">Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:</span><span class="sxs-lookup"><span data-stu-id="19bc9-331">This approach sets the environment in *web.config* when the project is published:</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="19bc9-332">**IIS uygulama havuzu başına**</span><span class="sxs-lookup"><span data-stu-id="19bc9-332">**Per IIS Application Pool**</span></span>

<span data-ttu-id="19bc9-333">Yalıtılmış uygulama havuzunda çalışan bir uygulamanın `ASPNETCORE_ENVIRONMENT` ortam değişkenini ayarlamak için (IIS 10,0 veya üzeri sürümlerde desteklenir), [ortam değişkenlerinin &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konusunun *Appcmd. exe komut* bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-333">To set the `ASPNETCORE_ENVIRONMENT` environment variable for an app running in an isolated Application Pool (supported on IIS 10.0 or later), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span> <span data-ttu-id="19bc9-334">`ASPNETCORE_ENVIRONMENT` ortam değişkeni bir uygulama havuzu için ayarlandığında, değeri sistem düzeyindeki bir ayarı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="19bc9-334">When the `ASPNETCORE_ENVIRONMENT` environment variable is set for an app pool, its value overrides a setting at the system level.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19bc9-335">IIS 'de bir uygulama barındırırken ve `ASPNETCORE_ENVIRONMENT` ortam değişkenini ekleyerek veya değiştirirken, yeni değerin uygulamalar tarafından çekilmek için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-335">When hosting an app in IIS and adding or changing the `ASPNETCORE_ENVIRONMENT` environment variable, use any one of the following approaches to have the new value picked up by apps:</span></span>
>
> * <span data-ttu-id="19bc9-336">`net stop was /y` ve ardından komut isteminden `net start w3svc` yürütün.</span><span class="sxs-lookup"><span data-stu-id="19bc9-336">Execute `net stop was /y` followed by `net start w3svc` from a command prompt.</span></span>
> * <span data-ttu-id="19bc9-337">Sunucuyu yeniden başlatın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-337">Restart the server.</span></span>

#### <a name="macos"></a><span data-ttu-id="19bc9-338">macOS</span><span class="sxs-lookup"><span data-stu-id="19bc9-338">macOS</span></span>

<span data-ttu-id="19bc9-339">MacOS için geçerli ortamın ayarlanması, uygulamayı çalıştırırken satır içinde gerçekleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="19bc9-339">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="19bc9-340">Alternatif olarak, uygulamayı çalıştırmadan önce ortamı `export` ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-340">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="19bc9-341">Makine düzeyinde ortam değişkenleri *. bashrc* veya *. bash_profile* dosyasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-341">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="19bc9-342">Herhangi bir metin düzenleyicisini kullanarak dosyayı düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="19bc9-342">Edit the file using any text editor.</span></span> <span data-ttu-id="19bc9-343">Aşağıdaki ifadeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="19bc9-343">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a><span data-ttu-id="19bc9-344">Linux</span><span class="sxs-lookup"><span data-stu-id="19bc9-344">Linux</span></span>

<span data-ttu-id="19bc9-345">Linux distros için, oturum tabanlı değişken ayarları ve makine düzeyindeki ortam ayarları için *bash_profile* dosyası için bir komut isteminde `export` komutunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-345">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="set-the-environment-in-code"></a><span data-ttu-id="19bc9-346">Kodda ortam ayarlama</span><span class="sxs-lookup"><span data-stu-id="19bc9-346">Set the environment in code</span></span>

<span data-ttu-id="19bc9-347">Konağı oluştururken <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> çağırın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-347">Call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> when building the host.</span></span> <span data-ttu-id="19bc9-348">Bkz. <xref:fundamentals/host/web-host#environment>.</span><span class="sxs-lookup"><span data-stu-id="19bc9-348">See <xref:fundamentals/host/web-host#environment>.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="19bc9-349">Ortama göre yapılandırma</span><span class="sxs-lookup"><span data-stu-id="19bc9-349">Configuration by environment</span></span>

<span data-ttu-id="19bc9-350">Yapılandırmayı ortama göre yüklemek için şunları yapmanızı öneririz:</span><span class="sxs-lookup"><span data-stu-id="19bc9-350">To load configuration by environment, we recommend:</span></span>

* <span data-ttu-id="19bc9-351">*appSettings* dosyaları (*appSettings. { Environment}. JSON*).</span><span class="sxs-lookup"><span data-stu-id="19bc9-351">*appsettings* files (*appsettings.{Environment}.json*).</span></span> <span data-ttu-id="19bc9-352">Bkz. <xref:fundamentals/configuration/index#json-configuration-provider>.</span><span class="sxs-lookup"><span data-stu-id="19bc9-352">See <xref:fundamentals/configuration/index#json-configuration-provider>.</span></span>
* <span data-ttu-id="19bc9-353">Ortam değişkenleri (uygulamanın barındırıldığı her bir sistemde ayarlanır).</span><span class="sxs-lookup"><span data-stu-id="19bc9-353">Environment variables (set on each system where the app is hosted).</span></span> <span data-ttu-id="19bc9-354">Bkz. <xref:fundamentals/host/web-host#environment> ve <xref:security/app-secrets#environment-variables>.</span><span class="sxs-lookup"><span data-stu-id="19bc9-354">See <xref:fundamentals/host/web-host#environment> and <xref:security/app-secrets#environment-variables>.</span></span>
* <span data-ttu-id="19bc9-355">Gizli dizi Yöneticisi (yalnızca geliştirme ortamında).</span><span class="sxs-lookup"><span data-stu-id="19bc9-355">Secret Manager (in the Development environment only).</span></span> <span data-ttu-id="19bc9-356">Bkz. <xref:security/app-secrets>.</span><span class="sxs-lookup"><span data-stu-id="19bc9-356">See <xref:security/app-secrets>.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="19bc9-357">Ortam tabanlı başlangıç sınıfı ve yöntemleri</span><span class="sxs-lookup"><span data-stu-id="19bc9-357">Environment-based Startup class and methods</span></span>

### <a name="inject-ihostingenvironment-into-startupconfigure"></a><span data-ttu-id="19bc9-358">Başlangıç olarak ıhostingenvironment ekleme. yapılandırma</span><span class="sxs-lookup"><span data-stu-id="19bc9-358">Inject IHostingEnvironment into Startup.Configure</span></span>

<span data-ttu-id="19bc9-359"><xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> `Startup.Configure`ekleme.</span><span class="sxs-lookup"><span data-stu-id="19bc9-359">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into `Startup.Configure`.</span></span> <span data-ttu-id="19bc9-360">Bu yaklaşım, uygulama yalnızca, ortam başına en az kod farklılığı olan birkaç ortam için `Startup.Configure` yapılandırmayı gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-360">This approach is useful when the app only requires configuring `Startup.Configure` for only a few environments with minimal code differences per environment.</span></span>

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a><span data-ttu-id="19bc9-361">Başlangıç sınıfına ıhostingenvironment ekleme</span><span class="sxs-lookup"><span data-stu-id="19bc9-361">Inject IHostingEnvironment into the Startup class</span></span>

<span data-ttu-id="19bc9-362">`Startup` oluşturucusuna <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> ekleyin ve hizmeti `Startup` sınıfı boyunca kullanmak üzere bir alana atayın.</span><span class="sxs-lookup"><span data-stu-id="19bc9-362">Inject <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> into the `Startup` constructor and assign the service to a field for use throughout the `Startup` class.</span></span> <span data-ttu-id="19bc9-363">Bu yaklaşım, uygulama her ortam için en az kod farklılığı olan birkaç ortam için başlatma yapılandırması gerektirdiğinde faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-363">This approach is useful when the app requires configuring startup for only a few environments with minimal code differences per environment.</span></span>

<span data-ttu-id="19bc9-364">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="19bc9-364">In the following example:</span></span>

* <span data-ttu-id="19bc9-365">Ortam `_env` alanında tutulur.</span><span class="sxs-lookup"><span data-stu-id="19bc9-365">The environment is held in the `_env` field.</span></span>
* <span data-ttu-id="19bc9-366">`_env`, `ConfigureServices` ve `Configure` uygulamanın ortamına göre başlangıç yapılandırmasını uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-366">`_env` is used in `ConfigureServices` and `Configure` to apply startup configuration based on the app's environment.</span></span>

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

### <a name="startup-class-conventions"></a><span data-ttu-id="19bc9-367">Başlangıç sınıfı kuralları</span><span class="sxs-lookup"><span data-stu-id="19bc9-367">Startup class conventions</span></span>

<span data-ttu-id="19bc9-368">ASP.NET Core bir uygulama başlatıldığında, [Başlangıç sınıfı](xref:fundamentals/startup) uygulamayı önyükleme.</span><span class="sxs-lookup"><span data-stu-id="19bc9-368">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="19bc9-369">Uygulama farklı ortamlar için ayrı `Startup` sınıfları tanımlayabilir (örneğin, `StartupDevelopment`).</span><span class="sxs-lookup"><span data-stu-id="19bc9-369">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`).</span></span> <span data-ttu-id="19bc9-370">Uygun `Startup` sınıfı çalışma zamanında seçildi.</span><span class="sxs-lookup"><span data-stu-id="19bc9-370">The appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="19bc9-371">Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir.</span><span class="sxs-lookup"><span data-stu-id="19bc9-371">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="19bc9-372">Eşleşen bir `Startup{EnvironmentName}` sınıfı bulunamazsa, `Startup` sınıfı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-372">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span> <span data-ttu-id="19bc9-373">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-373">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

<span data-ttu-id="19bc9-374">Ortam tabanlı `Startup` sınıfları uygulamak için, kullanımdaki her ortam için bir `Startup{EnvironmentName}` sınıfı ve bir geri dönüş `Startup` sınıfı oluşturun:</span><span class="sxs-lookup"><span data-stu-id="19bc9-374">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

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

<span data-ttu-id="19bc9-375">Derleme adını kabul eden [Usestartup (ıwebhostbuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) aşırı yüklemesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="19bc9-375">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

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

### <a name="startup-method-conventions"></a><span data-ttu-id="19bc9-376">Başlangıç yöntemi kuralları</span><span class="sxs-lookup"><span data-stu-id="19bc9-376">Startup method conventions</span></span>

<span data-ttu-id="19bc9-377">[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) , form `Configure<EnvironmentName>` ve `Configure<EnvironmentName>Services`ortama özgü sürümlerini destekler.</span><span class="sxs-lookup"><span data-stu-id="19bc9-377">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`.</span></span> <span data-ttu-id="19bc9-378">Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="19bc9-378">This approach is useful when the app requires configuring startup for several environments with many code differences per environment.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a><span data-ttu-id="19bc9-379">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="19bc9-379">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>

::: moniker-end