---
title: ASP.NET Core kullanan birden çok ortamlar
author: rick-anderson
description: ASP.NET Core uygulamaları birden çok ortamlarda üzerinden uygulama davranışını denetlemek öğrenin.
ms.author: riande
ms.date: 06/21/2018
uid: fundamentals/environments
ms.openlocfilehash: 505f19d8b4df6e476b46a1fe7c49872d3c4acc1a
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314110"
---
# <a name="use-multiple-environments-in-aspnet-core"></a><span data-ttu-id="ac9f2-103">ASP.NET Core kullanan birden çok ortamlar</span><span class="sxs-lookup"><span data-stu-id="ac9f2-103">Use multiple environments in ASP.NET Core</span></span>

<span data-ttu-id="ac9f2-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ac9f2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ac9f2-105">ASP.NET Core bir ortam değişkeni kullanarak çalışma zamanı ortamı tabanlı uygulama davranışını yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-105">ASP.NET Core configures app behavior based on the runtime environment using an environment variable.</span></span>

<span data-ttu-id="ac9f2-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ac9f2-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="environments"></a><span data-ttu-id="ac9f2-107">Ortamlar</span><span class="sxs-lookup"><span data-stu-id="ac9f2-107">Environments</span></span>

<span data-ttu-id="ac9f2-108">ASP.NET Core okur ortam değişkeni `ASPNETCORE_ENVIRONMENT` uygulama başlangıçta ve değeri depolar [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span><span class="sxs-lookup"><span data-stu-id="ac9f2-108">ASP.NET Core reads the environment variable `ASPNETCORE_ENVIRONMENT` at app startup and stores the value in [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname).</span></span> <span data-ttu-id="ac9f2-109">Ayarlayabileceğiniz `ASPNETCORE_ENVIRONMENT` herhangi bir değere ancak [üç değerden](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) framework tarafından desteklenir: [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [hazırlama](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), ve [üretim](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span><span class="sxs-lookup"><span data-stu-id="ac9f2-109">You can set `ASPNETCORE_ENVIRONMENT` to any value, but [three values](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) are supported by the framework: [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), and [Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production).</span></span> <span data-ttu-id="ac9f2-110">Varsa `ASPNETCORE_ENVIRONMENT` , varsayılan olarak, ayarlı değil `Production`.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-110">If `ASPNETCORE_ENVIRONMENT` isn't set, it defaults to `Production`.</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="ac9f2-111">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-111">The preceding code:</span></span>

* <span data-ttu-id="ac9f2-112">Çağrıları [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) ve [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) zaman `ASPNETCORE_ENVIRONMENT` ayarlanır `Development`.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-112">Calls [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) and [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) when `ASPNETCORE_ENVIRONMENT` is set to `Development`.</span></span>
* <span data-ttu-id="ac9f2-113">Çağrıları [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) zaman değerini `ASPNETCORE_ENVIRONMENT` aşağıdakilerden birini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-113">Calls [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) when the value of `ASPNETCORE_ENVIRONMENT` is set one of the following:</span></span>

    * `Staging`
    * `Production`
    * `Staging_2`

<span data-ttu-id="ac9f2-114">[Ortam etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) değerini kullanır `IHostingEnvironment.EnvironmentName` dahil etmek veya hariç biçimlendirme öğesi içinde:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-114">The [Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) uses the value of `IHostingEnvironment.EnvironmentName` to include or exclude markup in the element:</span></span>

[!code-cshtml[](environments/sample-snapshot/WebApp1/Pages/About.cshtml)]

<span data-ttu-id="ac9f2-115">Windows ve macOS, ortam değişkenlerini ve değerleri büyük küçük harfe duyarlı değildir.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-115">On Windows and macOS, environment variables and values aren't case sensitive.</span></span> <span data-ttu-id="ac9f2-116">Linux ortam değişkenlerini ve değerleri **büyük küçük harfe duyarlı** varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-116">Linux environment variables and values are **case sensitive** by default.</span></span>

### <a name="development"></a><span data-ttu-id="ac9f2-117">Geliştirme</span><span class="sxs-lookup"><span data-stu-id="ac9f2-117">Development</span></span>

<span data-ttu-id="ac9f2-118">Geliştirme ortamı üretimde kullanıma sunulan döndürmemelidir özellikleri etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-118">The development environment can enable features that shouldn't be exposed in production.</span></span> <span data-ttu-id="ac9f2-119">Örneğin, ASP.NET Core Şablonları etkinleştirmek [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling#the-developer-exception-page) geliştirme ortamında.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-119">For example, the ASP.NET Core templates enable the [developer exception page](xref:fundamentals/error-handling#the-developer-exception-page) in the development environment.</span></span>

<span data-ttu-id="ac9f2-120">Yerel makine geliştirme ortamını ayarlanabilir *Properties\launchSettings.json* projenin dosya.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-120">The environment for local machine development can be set in the *Properties\launchSettings.json* file of the project.</span></span> <span data-ttu-id="ac9f2-121">Ortam değerleri kümesinde *launchSettings.json* sistem ortama değerlerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-121">Environment values set in *launchSettings.json* override values set in the system environment.</span></span>

<span data-ttu-id="ac9f2-122">Aşağıdaki JSON üç profillerden gösteren bir *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-122">The following JSON shows three profiles from a *launchSettings.json* file:</span></span>

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="ac9f2-123">`applicationUrl` Özelliğinde *launchSettings.json* sunucu URL'lerin bir listesini belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-123">The `applicationUrl` property in *launchSettings.json* can specify a list of server URLs.</span></span> <span data-ttu-id="ac9f2-124">Noktalı virgül listedeki URL'leri arasında kullanın:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-124">Use a semicolon between the URLs in the list:</span></span>
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

<span data-ttu-id="ac9f2-125">Ne zaman uygulama başlatıldığında ile [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run), ilk profiliyle `"commandName": "Project"` kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-125">When the app is launched with [dotnet run](/dotnet/core/tools/dotnet-run), the first profile with `"commandName": "Project"` is used.</span></span> <span data-ttu-id="ac9f2-126">Değeri `commandName` başlatmak için web sunucusunu belirtir.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-126">The value of `commandName` specifies the web server to launch.</span></span> <span data-ttu-id="ac9f2-127">`commandName` aşağıdakilerden herhangi biri olabilir:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-127">`commandName` can be any one of the following:</span></span>

* <span data-ttu-id="ac9f2-128">IIS Express</span><span class="sxs-lookup"><span data-stu-id="ac9f2-128">IIS Express</span></span>
* <span data-ttu-id="ac9f2-129">IIS</span><span class="sxs-lookup"><span data-stu-id="ac9f2-129">IIS</span></span>
* <span data-ttu-id="ac9f2-130">(Kestrel başlatır) projesi</span><span class="sxs-lookup"><span data-stu-id="ac9f2-130">Project (which launches Kestrel)</span></span>

<span data-ttu-id="ac9f2-131">Ne zaman bir uygulama başlatıldığında ile [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="ac9f2-131">When an app is launched with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

* <span data-ttu-id="ac9f2-132">*launchSettings.json* okunur varsa.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-132">*launchSettings.json* is read if available.</span></span> <span data-ttu-id="ac9f2-133">`environmentVariables` ayarlarında *launchSettings.json* ortam değişkenleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-133">`environmentVariables` settings in *launchSettings.json* override environment variables.</span></span>
* <span data-ttu-id="ac9f2-134">Barındırma ortamı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-134">The hosting environment is displayed.</span></span>

<span data-ttu-id="ac9f2-135">Aşağıdaki çıkış kullanmaya uygulama gösterir [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run):</span><span class="sxs-lookup"><span data-stu-id="ac9f2-135">The following output shows an app started with [dotnet run](/dotnet/core/tools/dotnet-run):</span></span>

```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="ac9f2-136">Visual Studio **hata ayıklama** sekmesi sağlar düzenlemek için bir GUI *launchSettings.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-136">The Visual Studio **Debug** tab provides a GUI to edit the *launchSettings.json* file:</span></span>

![Proje Özellikleri ayarını ortam değişkenleri](environments/_static/project-properties-debug.png)

<span data-ttu-id="ac9f2-138">Web sunucu yeniden başlatılana kadar proje profillere yapılan değişiklikler etkilerini göstermeyebilir.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-138">Changes made to project profiles may not take effect until the web server is restarted.</span></span> <span data-ttu-id="ac9f2-139">Kestrel, ortama yapılan değişiklikleri algılayabilir önce başlatılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-139">Kestrel must be restarted before it can detect changes made to its environment.</span></span>

> [!WARNING]
> <span data-ttu-id="ac9f2-140">*launchSettings.json* parolaları depolamak döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-140">*launchSettings.json* shouldn't store secrets.</span></span> <span data-ttu-id="ac9f2-141">[Gizli Yöneticisi aracını](xref:security/app-secrets) yerel geliştirme için parolaları depolamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-141">The [Secret Manager tool](xref:security/app-secrets) can be used to store secrets for local development.</span></span>

<span data-ttu-id="ac9f2-142">Kullanırken [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-142">When using [Visual Studio Code](https://code.visualstudio.com/), environment variables can be set in the *.vscode/launch.json* file.</span></span> <span data-ttu-id="ac9f2-143">Aşağıdaki örnek ortamını ayarlar `Development`:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-143">The following example sets the environment to `Development`:</span></span>

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

<span data-ttu-id="ac9f2-144">A *.vscode/launch.json* proje dosyasında değil okuma uygulamayla başlatırken `dotnet run` aynı şekilde *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-144">A *.vscode/launch.json* file in the project isn't read when starting the app with `dotnet run` in the same way as *Properties/launchSettings.json*.</span></span> <span data-ttu-id="ac9f2-145">Bir uygulama yok geliştirme başlatılırken bir *launchSettings.json* dosya, bir ortam değişkeni veya bir komut satırı bağımsız değişkeni ortamıyla ayarladıktan `dotnet run` komutu.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-145">When launching an app in development that doesn't have a *launchSettings.json* file, either set the environment with an environment variable or a command-line argument to the `dotnet run` command.</span></span>

### <a name="production"></a><span data-ttu-id="ac9f2-146">Üretim</span><span class="sxs-lookup"><span data-stu-id="ac9f2-146">Production</span></span>

<span data-ttu-id="ac9f2-147">Üretim ortamında, güvenlik, performans ve uygulama sağlamlık en üst düzeye çıkarmak için yapılandırılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-147">The production environment should be configured to maximize security, performance, and app robustness.</span></span> <span data-ttu-id="ac9f2-148">Geliştirme farklı bazı ortak ayarlar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-148">Some common settings that differ from development include:</span></span>

* <span data-ttu-id="ac9f2-149">Önbelleğe alma.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-149">Caching.</span></span>
* <span data-ttu-id="ac9f2-150">İstemci-tarafı kaynaklar küçültülmüş, paketlenmiş ve potansiyel olarak bir CDN hizmet.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-150">Client-side resources are bundled, minified, and potentially served from a CDN.</span></span>
* <span data-ttu-id="ac9f2-151">Tanılama hata sayfaları devre dışı.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-151">Diagnostic error pages disabled.</span></span>
* <span data-ttu-id="ac9f2-152">Kolay hata sayfaları etkin.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-152">Friendly error pages enabled.</span></span>
* <span data-ttu-id="ac9f2-153">Üretim günlüğe kaydetme ve izleme etkin.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-153">Production logging and monitoring enabled.</span></span> <span data-ttu-id="ac9f2-154">Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="ac9f2-154">For example, [Application Insights](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="setting-the-environment"></a><span data-ttu-id="ac9f2-155">Ortamını ayarlama</span><span class="sxs-lookup"><span data-stu-id="ac9f2-155">Setting the environment</span></span>

<span data-ttu-id="ac9f2-156">Genellikle, test etmek için belirli bir ortam ayarlamak yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-156">It's often useful to set a specific environment for testing.</span></span> <span data-ttu-id="ac9f2-157">Ortam ayarlanmamışsa, varsayılan olarak `Production`, hangi devre dışı bırakır çoğu hata ayıklama özelliği.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-157">If the environment isn't set, it defaults to `Production`, which disables most debugging features.</span></span> <span data-ttu-id="ac9f2-158">Ortam ayarı yöntemi işletim sistemine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-158">The method for setting the environment depends on the operating system.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="ac9f2-159">Azure uygulama hizmeti</span><span class="sxs-lookup"><span data-stu-id="ac9f2-159">Azure App Service</span></span>

<span data-ttu-id="ac9f2-160">Ortam ayarlamak için [Azure App Service](https://azure.microsoft.com/services/app-service/), aşağıdaki adımları gerçekleştirin:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-160">To set the environment in [Azure App Service](https://azure.microsoft.com/services/app-service/), perform the following steps:</span></span>

1. <span data-ttu-id="ac9f2-161">Uygulamadan seçin **uygulama hizmetleri** dikey.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-161">Select the app from the **App Services** blade.</span></span>
1. <span data-ttu-id="ac9f2-162">İçinde **ayarları** grup, select **uygulama ayarları** dikey.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-162">In the **SETTINGS** group, select the **Application settings** blade.</span></span>
1. <span data-ttu-id="ac9f2-163">İçinde **uygulama ayarları** alanında **yeni ayar Ekle**.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-163">In the **Application settings** area, select **Add new setting**.</span></span>
1. <span data-ttu-id="ac9f2-164">İçin **bir ad girin**, sağlayan `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-164">For **Enter a name**, provide `ASPNETCORE_ENVIRONMENT`.</span></span> <span data-ttu-id="ac9f2-165">İçin **bir değer girin**, ortam sağlamak (örneğin, `Staging`).</span><span class="sxs-lookup"><span data-stu-id="ac9f2-165">For **Enter a value**, provide the environment (for example, `Staging`).</span></span>
1. <span data-ttu-id="ac9f2-166">Seçin **yuva ayarı** dağıtım yuvaları takas olduğunda geçerli yuvasıyla kalmasına ortamı ayarı istiyorsanız kutuyu.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-166">Select the **Slot Setting** check box if you wish the environment setting to remain with the current slot when deployment slots are swapped.</span></span> <span data-ttu-id="ac9f2-167">Daha fazla bilgi için bkz: [Azure belgelerine: hangi ayarların takas?](/azure/app-service/web-sites-staged-publishing).</span><span class="sxs-lookup"><span data-stu-id="ac9f2-167">For more information, see [Azure Documentation: Which settings are swapped?](/azure/app-service/web-sites-staged-publishing).</span></span>
1. <span data-ttu-id="ac9f2-168">Seçin **kaydetmek** dikey pencerenin üstündeki.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-168">Select **Save** at the top of the blade.</span></span>

<span data-ttu-id="ac9f2-169">Bir uygulama ayarı (ortam değişkeni) eklendiğinde, değiştirilen veya Azure portalında silindiğinde sonra azure uygulama hizmeti uygulaması otomatik olarak yeniden başlatır.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-169">Azure App Service automatically restarts the app after an app setting (environment variable) is added, changed, or deleted in the Azure portal.</span></span>

### <a name="windows"></a><span data-ttu-id="ac9f2-170">Windows</span><span class="sxs-lookup"><span data-stu-id="ac9f2-170">Windows</span></span>

<span data-ttu-id="ac9f2-171">Ayarlamak için `ASPNETCORE_ENVIRONMENT` uygulama başlatıldığında geçerli oturum için kullanarak [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run), aşağıdaki komutları kullanılır:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-171">To set the `ASPNETCORE_ENVIRONMENT` for the current session when the app is started using [dotnet run](/dotnet/core/tools/dotnet-run), the following commands are used:</span></span>

<span data-ttu-id="ac9f2-172">**Komut İstemi**</span><span class="sxs-lookup"><span data-stu-id="ac9f2-172">**Command prompt**</span></span>

```console
set ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="ac9f2-173">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="ac9f2-173">**PowerShell**</span></span>

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

<span data-ttu-id="ac9f2-174">Bu komutlar, yalnızca geçerli penceresinin etkili olur.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-174">These commands only take effect for the current window.</span></span> <span data-ttu-id="ac9f2-175">Pencere kapatıldığında `ASPNETCORE_ENVIRONMENT` ayar varsayılan ayarı veya makine değeri döner.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-175">When the window is closed, the `ASPNETCORE_ENVIRONMENT` setting reverts to the default setting or machine value.</span></span> <span data-ttu-id="ac9f2-176">Değer Windows'da genel olarak ayarlamak için açık **Denetim Masası** > **sistem** > **Gelişmiş Sistem ayarları** ve ekleme veya düzenleme `ASPNETCORE_ENVIRONMENT`değeri:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-176">To set the value globally in Windows, open the **Control Panel** > **System** > **Advanced system settings** and add or edit the `ASPNETCORE_ENVIRONMENT` value:</span></span>

![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

![ASP.NET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

<span data-ttu-id="ac9f2-179">**Web.config**</span><span class="sxs-lookup"><span data-stu-id="ac9f2-179">**web.config**</span></span>

<span data-ttu-id="ac9f2-180">Bkz: *ortam değişkenlerini ayarlama* bölümünü [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) konu.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-180">See the *Setting environment variables* section of the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) topic.</span></span>

<span data-ttu-id="ac9f2-181">**Her IIS uygulama havuzu**</span><span class="sxs-lookup"><span data-stu-id="ac9f2-181">**Per IIS Application Pool**</span></span>

<span data-ttu-id="ac9f2-182">Ortam değişkenlerini (IIS 10.0 + desteklenir) yalıtılmış uygulama havuzlarında çalışan tek tek uygulamalar için ayarlamak için bkz: *AppCmd.exe komutunu* bölümünü [ortam değişkenleri &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konu.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-182">To set environment variables for individual apps running in isolated Application Pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic.</span></span>

### <a name="macos"></a><span data-ttu-id="ac9f2-183">macOS</span><span class="sxs-lookup"><span data-stu-id="ac9f2-183">macOS</span></span>

<span data-ttu-id="ac9f2-184">MacOS olabilir geçerli ortamı ayarı satır içi uygulama çalıştırıldığında, gerçekleştirilen:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-184">Setting the current environment for macOS can be performed in-line when running the app:</span></span>

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

<span data-ttu-id="ac9f2-185">Alternatif olarak, ortamıyla kümesinin `export` uygulama çalıştırılmadan önce:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-185">Alternatively, set the environment with `export` prior to running the app:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

<span data-ttu-id="ac9f2-186">Makine düzeyinde ortam değişkenlerini ayarlama *.bashrc* veya *.bash_profile* dosya.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-186">Machine-level environment variables are set in the *.bashrc* or *.bash_profile* file.</span></span> <span data-ttu-id="ac9f2-187">Herhangi bir metin düzenleyicisi kullanarak dosyayı düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-187">Edit the file using any text editor.</span></span> <span data-ttu-id="ac9f2-188">Aşağıdaki deyim ekleyin:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-188">Add the following statement:</span></span>

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a><span data-ttu-id="ac9f2-189">Linux</span><span class="sxs-lookup"><span data-stu-id="ac9f2-189">Linux</span></span>

<span data-ttu-id="ac9f2-190">Linux distro'lar için kullanmak `export` oturum tabanlı değişken ayarları için bir komut isteminde komutunu ve *bash_profile* makine düzeyinde ortam ayarları dosyası.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-190">For Linux distros, use the `export` command at a command prompt for session-based variable settings and *bash_profile* file for machine-level environment settings.</span></span>

### <a name="configuration-by-environment"></a><span data-ttu-id="ac9f2-191">Ortam yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ac9f2-191">Configuration by environment</span></span>

<span data-ttu-id="ac9f2-192">Bkz: [yapılandırma ortamı tarafından](xref:fundamentals/configuration/index#configuration-by-environment) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-192">See [Configuration by environment](xref:fundamentals/configuration/index#configuration-by-environment) for more information.</span></span>

## <a name="environment-based-startup-class-and-methods"></a><span data-ttu-id="ac9f2-193">Ortam tabanlı başlangıç sınıfı ve yöntemleri</span><span class="sxs-lookup"><span data-stu-id="ac9f2-193">Environment-based Startup class and methods</span></span>

<span data-ttu-id="ac9f2-194">Bir ASP.NET Core uygulama başlatıldığında [başlangıç sınıfı](xref:fundamentals/startup) uygulama bootstraps.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-194">When an ASP.NET Core app starts, the [Startup class](xref:fundamentals/startup) bootstraps the app.</span></span> <span data-ttu-id="ac9f2-195">Varsa bir `Startup{EnvironmentName}` sınıfı yok, sınıf için adlandırılır `EnvironmentName`:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-195">If a `Startup{EnvironmentName}` class exists, the class is called for that `EnvironmentName`:</span></span>

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

<span data-ttu-id="ac9f2-196">[WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) yapılandırma bölümlerinin geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ac9f2-196">[WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) overrides configuration sections.</span></span>

<span data-ttu-id="ac9f2-197">[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) biçiminde ortama özgü sürümleri destekler `Configure{EnvironmentName}` ve `Configure{EnvironmentName}Services`:</span><span class="sxs-lookup"><span data-stu-id="ac9f2-197">[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) support environment-specific versions of the form `Configure{EnvironmentName}` and `Configure{EnvironmentName}Services`:</span></span>

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a><span data-ttu-id="ac9f2-198">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ac9f2-198">Additional resources</span></span>

* [<span data-ttu-id="ac9f2-199">Uygulama başlatma</span><span class="sxs-lookup"><span data-stu-id="ac9f2-199">Application startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="ac9f2-200">Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ac9f2-200">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="ac9f2-201">IHostingEnvironment.EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="ac9f2-201">IHostingEnvironment.EnvironmentName</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
