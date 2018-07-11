---
title: ASP.NET Core birden çok ortam kullanma
author: rick-anderson
description: ASP.NET Core uygulamaları birden fazla ortam arasında uygulama davranışını denetleme konusunda bilgi edinin.
ms.author: riande
ms.date: 07/03/2018
uid: fundamentals/environments
ms.openlocfilehash: b0e001b50ada85a183590fbee1ad1f3b895004d5
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938439"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="e9b04-103">ASP.NET Core birden çok ortam kullanma</span><span class="sxs-lookup"><span data-stu-id="e9b04-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="e9b04-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e9b04-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e9b04-105">ASP.NET Core, bir ortam değişkeni kullanarak çalışma zamanı ortama göre uygulama davranışını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="e9b04-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="e9b04-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e9b04-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="e9b04-107">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="e9b04-107">Environments</span></span>

<span data-ttu-id="e9b04-108">ASP.NET Core ortam değişkenini okur `ASPNETCORE_ENVIRONMENT` uygulamanın başlangıcında ve değeri depolar [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="e9b04-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="e9b04-109">Ayarlayabileceğiniz `ASPNETCORE_ENVIRONMENT` herhangi bir değere ancak [üç değerden](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) framework tarafından desteklenir: [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [hazırlama](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), ve [üretim](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="e9b04-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="e9b04-110">Varsa `ASPNETCORE_ENVIRONMENT` değil, varsayılan olarak, belirlenen `Production`.</span><span class="sxs-lookup"><span data-stu-id="e9b04-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

<span data-ttu-id="e9b04-111">Yukarıdaki kod:</span><span class="sxs-lookup"><span data-stu-id="e9b04-111">The preceding code:</span></span>

* <span data-ttu-id="e9b04-112">Çağrıları [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) ve [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) olduğunda `ASPNETCORE_ENVIRONMENT` ayarlanır `Development`.</span><span class="sxs-lookup"><span data-stu-id="e9b04-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="e9b04-113">Çağrıları [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) zaman değerini `ASPNETCORE_ENVIRONMENT` aşağıdakilerden birine ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="e9b04-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="e9b04-114">[Ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) değerini kullanır `IHostingEnvironment.EnvironmentName` dahil edin veya dışlayın öğesinde bulunan işaretleme:</span><span class="sxs-lookup"><span data-stu-id="e9b04-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

<span data-ttu-id="e9b04-115">Windows ve macOS üzerinde ortam değişkenlerini ve değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="e9b04-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="e9b04-116">Linux ortam değişkenlerini ve değerleri **büyük küçük harfe duyarlı** varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="e9b04-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="e9b04-117">Geliştirme</span><span class="sxs-lookup"><span data-stu-id="e9b04-117">Development</span></span>

<span data-ttu-id="e9b04-118">Geliştirme ortamının, üretim ortamında kullanıma sunulan olmamalıdır özellikleri etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9b04-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="e9b04-119">Örneğin, ASP.NET Core şablonları etkinleştirme [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling#the-developer-exception-page) geliştirme ortamında.</span><span class="sxs-lookup"><span data-stu-id="e9b04-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="e9b04-120">Yerel makine geliştirme ortamını ayarlanabilir *Properties\launchSettings.json* proje dosyası.</span><span class="sxs-lookup"><span data-stu-id="e9b04-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="e9b04-121">Ortam değerlerini kümesinde *launchSettings.json* sistemi ortamında ayarlanan değerleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="e9b04-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="e9b04-122">Aşağıdaki JSON üç profillerden gösterir bir *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="e9b04-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

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

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="e9b04-123">`applicationUrl` Özelliğinde *launchSettings.json* sunucu URL'lerin bir listesini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e9b04-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="e9b04-124">Listedeki URL'leri arasında noktalı virgül kullanın:</span><span class="sxs-lookup"><span data-stu-id="e9b04-124">Use a semicolon between the URLs in the list:</span></span>
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

::: moniker-end

<span data-ttu-id="e9b04-125">Ne zaman uygulama başlatıldığında ile [çalıştırma dotnet](/dotnet/core/tools/dotnet-run), ilk profiliyle `"commandName": "Project"` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e9b04-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="e9b04-126">Değerini `commandName` başlatmak için web sunucusunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="e9b04-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="e9b04-127">`commandName` aşağıdakilerden herhangi biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="e9b04-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="e9b04-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="e9b04-128">IIS Express</span></span>
* <span data-ttu-id="e9b04-129">IIS</span><span class="sxs-lookup"><span data-stu-id="e9b04-129">IIS</span></span>
* <span data-ttu-id="e9b04-130">(Bu Kestrel başlatır) projesi</span><span class="sxs-lookup"><span data-stu-id="e9b04-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="e9b04-131">Ne zaman bir uygulama başlatıldığında ile [çalıştırma dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="e9b04-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="e9b04-132">*launchSettings.json* okunur varsa.</span><span class="sxs-lookup"><span data-stu-id="e9b04-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="e9b04-133">`environmentVariables` ayarlarında *launchSettings.json* ortam değişkenlerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="e9b04-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="e9b04-134">Barındırma ortamı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e9b04-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="e9b04-135">Bir uygulama kullanmaya aşağıdaki çıktıyı gösterir [çalıştırma dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="e9b04-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="e9b04-136">Visual Studio Proje özelliklerini **hata ayıklama** sekmesi düzenlemek için bir GUI sağlar *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="e9b04-136">The Visual Studio project properties **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proje Özellikleri ayarı ortam değişkenleri](environments/_static/project-properties-debug.png)

<span data-ttu-id="e9b04-138">Proje profillere yapılan değişiklikler web sunucu yeniden başlatılana kadar etkili değildir.</span><span class="sxs-lookup"><span data-stu-id="e9b04-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="e9b04-139">Kestrel'i, ortama yapılan değişiklikleri algılayabilir önce başlatılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e9b04-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="e9b04-140">*launchSettings.json* gizli dizileri depolamak olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="e9b04-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="e9b04-141">[Gizli dizi Yöneticisi aracını](xref:security/app-secrets) yerel geliştirme için gizli dizileri depolamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e9b04-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="e9b04-142">Kullanırken [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="e9b04-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="e9b04-143">Aşağıdaki örnek ortamı ayarlar `Development`:</span><span class="sxs-lookup"><span data-stu-id="e9b04-143">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="e9b04-144">A *.vscode/launch.json* proje dosyasında değil okuma uygulamayı başlatırken `dotnet run` aynı şekilde *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e9b04-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="e9b04-145">Bir uygulamaya sahip olmayan geliştirme başlatırken bir *launchSettings.json* dosyası ortamında bir ortam değişkeni veya bir komut satırı bağımsız değişkeni ile ayarlanan `dotnet run` komutu.</span><span class="sxs-lookup"><span data-stu-id="e9b04-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="e9b04-146">Üretim</span><span class="sxs-lookup"><span data-stu-id="e9b04-146">Production</span></span>

<span data-ttu-id="e9b04-147">Üretim ortamında, güvenlik, performans ve uygulama sağlamlık en üst düzeye çıkarmak için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e9b04-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="e9b04-148">Geliştirme farklı bazı ortak ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e9b04-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="e9b04-149">Önbelleğe alma.</span><span class="sxs-lookup"><span data-stu-id="e9b04-149">Caching.</span></span>
* <span data-ttu-id="e9b04-150">İstemci tarafı kaynakları küçültülmüş, toplanmış ve potansiyel olarak cdn'den.</span><span class="sxs-lookup"><span data-stu-id="e9b04-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="e9b04-151">Tanılama hata sayfalarını devre dışı.</span><span class="sxs-lookup"><span data-stu-id="e9b04-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="e9b04-152">Kolay hata sayfalarını etkin.</span><span class="sxs-lookup"><span data-stu-id="e9b04-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="e9b04-153">Üretim günlüğe kaydetme ve izleme etkin.</span><span class="sxs-lookup"><span data-stu-id="e9b04-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="e9b04-154">Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="e9b04-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="e9b04-155">Ortamı ayarlama</span><span class="sxs-lookup"><span data-stu-id="e9b04-155">Set the environment</span></span>

<span data-ttu-id="e9b04-156">Genellikle, test etmek için belirli bir ortamı kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="e9b04-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="e9b04-157">Ortam ayarlanmadıysa, varsayılan `Production`, hangi devre dışı bırakır çoğu hata ayıklama özellikleri.</span><span class="sxs-lookup"><span data-stu-id="e9b04-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="e9b04-158">Ortamını yöntemi işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="e9b04-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="e9b04-159">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="e9b04-159">Azure App Service</span></span>

<span data-ttu-id="e9b04-160">Ortamı ayarlamak için [Azure App Service](https://azure.microsoft.com/services/app-service/), aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="e9b04-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="e9b04-161">Uygulamadan seçin **uygulama hizmetleri** dikey penceresi.</span><span class="sxs-lookup"><span data-stu-id="e9b04-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="e9b04-162">İçinde **ayarları** grubu, select **uygulama ayarları** dikey penceresi.</span><span class="sxs-lookup"><span data-stu-id="e9b04-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="e9b04-163">İçinde **uygulama ayarları** alanında **yeni ayar Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e9b04-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="e9b04-164">İçin **bir ad girin**, sağlayan `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="e9b04-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="e9b04-165">İçin **bir değer girin**, ortamı sağlayın (örneğin, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="e9b04-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="e9b04-166">Seçin **yuva ayarı** ortam ayarını, dağıtım yuvaları takas zaman ile geçerli yuvadaki kalmasına istiyorsanız kutuyu.</span><span class="sxs-lookup"><span data-stu-id="e9b04-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="e9b04-167">Daha fazla bilgi için [Azure belgeleri: hangi ayarların takas?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="e9b04-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="e9b04-168">Seçin **Kaydet** dikey penceresinin üstünde.</span><span class="sxs-lookup"><span data-stu-id="e9b04-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="e9b04-169">Bir uygulama ayarı (ortam değişkeni) eklenmesine, değiştirilmesine veya Azure portalda silinen sonra azure App Service uygulama otomatik olarak yeniden başlatılır.</span><span class="sxs-lookup"><span data-stu-id="e9b04-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="e9b04-170">Windows</span><span class="sxs-lookup"><span data-stu-id="e9b04-170">Windows</span></span>

<span data-ttu-id="e9b04-171">Ayarlanacak `ASPNETCORE_ENVIRONMENT` uygulama başlatıldığında geçerli oturum için kullanarak [çalıştırma dotnet](/dotnet/core/tools/dotnet-run), aşağıdaki komutları kullanılır:</span><span class="sxs-lookup"><span data-stu-id="e9b04-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="e9b04-172">**Komut İstemi**</span><span class="sxs-lookup"><span data-stu-id="e9b04-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="e9b04-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="e9b04-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="e9b04-174">Bu komutları yalnızca geçerli pencere için etkili.</span><span class="sxs-lookup"><span data-stu-id="e9b04-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="e9b04-175">Penceresi kapatıldığında `ASPNETCORE_ENVIRONMENT` ayarı varsayılan ayar veya bir makine değere döner.</span><span class="sxs-lookup"><span data-stu-id="e9b04-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="e9b04-176">Değerini genel olarak Windows içinde ayarlamak için açık **Denetim Masası** > **sistem** > **Gelişmiş Sistem ayarları** ve ekleme veya düzenleme `ASPNETCORE_ENVIRONMENT`değeri:</span><span class="sxs-lookup"><span data-stu-id="e9b04-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![Sistem Gelişmiş özellikleri](environments/_static/systemsetting_environment.png)

![ASP.NET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="e9b04-179">**Web.config**</span><span class="sxs-lookup"><span data-stu-id="e9b04-179">**web.config**</span></span>

<span data-ttu-id="e9b04-180">Bkz: *ortam değişkenlerini ayarlama* bölümünü [ASP.NET Core Module yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) konu.</span><span class="sxs-lookup"><span data-stu-id="e9b04-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="e9b04-181">**IIS uygulama havuzu başına**</span><span class="sxs-lookup"><span data-stu-id="e9b04-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="e9b04-182">Yalıtılmış uygulama havuzlarında (IIS 10.0 + desteklenir) çalıştıran tek tek uygulamalar için ortam değişkenlerini ayarlamak için bkz: *AppCmd.exe komut* bölümünü [ortam değişkenlerini &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konu.</span><span class="sxs-lookup"><span data-stu-id="e9b04-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="e9b04-183">MacOS</span><span class="sxs-lookup"><span data-stu-id="e9b04-183">macOS</span></span>

<span data-ttu-id="e9b04-184">MacOS olabilir geçerli ortamı ayarı satır içi uygulamayı çalıştırırken gerçekleştirmiştir:</span><span class="sxs-lookup"><span data-stu-id="e9b04-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="e9b04-185">Alternatif olarak, ortam kümesi `export` uygulamayı çalıştırmadan önce:</span><span class="sxs-lookup"><span data-stu-id="e9b04-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="e9b04-186">Makine düzeyinde ortam değişkenlerini ayarlanır *.bashrc* veya *.bash_profile* dosya.</span><span class="sxs-lookup"><span data-stu-id="e9b04-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="e9b04-187">Herhangi bir metin düzenleyicisi kullanarak dosyayı düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="e9b04-187">Edit the file using any text editor.</span></span> <span data-ttu-id="e9b04-188">Aşağıdaki deyimi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e9b04-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="e9b04-189">Linux</span><span class="sxs-lookup"><span data-stu-id="e9b04-189">Linux</span></span>

<span data-ttu-id="e9b04-190">Linux dağıtımları için kullanmak `export` oturum tabanlı değişken ayarları için bir komut isteminde komutunu ve *bash_profile* makine düzeyinde ortam ayarları dosyası.</span><span class="sxs-lookup"><span data-stu-id="e9b04-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="e9b04-191">Ortama göre yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e9b04-191">Configuration by environment</span></span>

<span data-ttu-id="e9b04-192">Bkz: [ortama göre yapılandırma](xref:fundamentals/configuration/index#configuration-by-environment) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="e9b04-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="e9b04-193">Ortam tabanlı başlangıç sınıfı ve yöntemleri</span><span class="sxs-lookup"><span data-stu-id="e9b04-193">Environment-based Startup class and methods</span></span>

### <a name="startup-class-conventions"></a><span data-ttu-id="e9b04-194">Başlangıç sınıfı kuralları</span><span class="sxs-lookup"><span data-stu-id="e9b04-194">Startup class conventions</span></span>

<span data-ttu-id="e9b04-195">ASP.NET Core uygulaması başladığında [başlangıç sınıfı](xref:fundamentals/startup) uygulama bootstraps.</span><span class="sxs-lookup"><span data-stu-id="e9b04-195">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="e9b04-196">Uygulamayı ayrı tanımlayabilirsiniz `Startup` sınıflar farklı ortamlar için (örneğin, `StartupDevelopment`) ve uygun `Startup` sınıfı, çalışma zamanında seçilidir.</span><span class="sxs-lookup"><span data-stu-id="e9b04-196">The app can define separate `Startup` classes for different environments (for example, `StartupDevelopment`), and the appropriate `Startup` class is selected at runtime.</span></span> <span data-ttu-id="e9b04-197">Geçerli ortamı olan adı sonekiyle sınıfı kurtarılmasına öncelik verilir.</span><span class="sxs-lookup"><span data-stu-id="e9b04-197">The class whose name suffix matches the current environment is prioritized.</span></span> <span data-ttu-id="e9b04-198">Eşleşen bir `Startup{EnvironmentName}` sınıfı değil bulundu, `Startup` sınıfı kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e9b04-198">If a matching `Startup{EnvironmentName}` class isn't found, the `Startup` class is used.</span></span>

<span data-ttu-id="e9b04-199">Ortam tabanlı uygulamak için `Startup` sınıfları oluşturma bir `Startup{EnvironmentName}` her ortamda kullanın ve bir geri dönüş için sınıf `Startup` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e9b04-199">To implement environment-based `Startup` classes, create a `Startup{EnvironmentName}` class for each environment in use and a fallback `Startup` class:</span></span>

```csharp
// Startup class to use in the Development environment
public class StartupDevelopment
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Startup class to use in the Production environment
public class StartupProduction
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}

// Fallback Startup class
// Selected if the environment doesn't match a Startup{EnvironmentName} class
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        ...
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        ...
    }
}
```

<span data-ttu-id="e9b04-200">Kullanım [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) bir derleme adı kabul eden aşırı yükleme:</span><span class="sxs-lookup"><span data-stu-id="e9b04-200">Use the [UseStartup(IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) overload that accepts an assembly name:</span></span>

::: moniker range=">= aspnetcore-2.1"

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

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    CreateWebHost(args).Run();
}

public static IWebHost CreateWebHost(string[] args)
{
    var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

    return WebHost.CreateDefaultBuilder(args)
        .UseStartup(assemblyName)
        .Build();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var assemblyName = typeof(Startup).GetTypeInfo().Assembly.FullName;

        var host = new WebHostBuilder()
            .UseStartup(assemblyName)
            .Build();

        host.Run();
    }
}
```

::: moniker-end

### <a name="startup-method-conventions"></a><span data-ttu-id="e9b04-201">Başlangıç yöntem kuralları</span><span class="sxs-lookup"><span data-stu-id="e9b04-201">Startup method conventions</span></span>

<span data-ttu-id="e9b04-202">[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) formun ortama özgü sürümlerini destekleyen `Configure<EnvironmentName>` ve `Configure<EnvironmentName>Services`:</span><span class="sxs-lookup"><span data-stu-id="e9b04-202">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure<EnvironmentName>` and `Configure<EnvironmentName>Services`:</span></span>

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a><span data-ttu-id="e9b04-203">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="e9b04-203">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [<span data-ttu-id="e9b04-204">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="e9b04-204">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
