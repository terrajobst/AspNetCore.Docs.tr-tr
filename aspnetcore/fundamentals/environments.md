---
title: ASP.NET Core kullanan birden çok ortamlar
author: rick-anderson
description: Birden çok ortamlar genelinde uygulamanızın davranışını denetlemek için ASP.NET Core destek nasıl sağladığını öğrenin.
ms.author: riande
ms.date: 12/25/2017
uid: fundamentals/environments
ms.openlocfilehash: 5a4caeeba045cb93dec9c73c931dae8a352bede9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276938"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>ASP.NET Core kullanan birden çok ortamlar

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core, ortam değişkenleri ile çalışma zamanında uygulama davranışını ayarlamak için destek sağlar.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Ortamlar

ASP.NET Core okur ortam değişkeni `ASPNETCORE_ENVIRONMENT` uygulama başlangıçta ve değer depoları [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName). `ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir ancak [üç değerden](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0) framework tarafından desteklenir: [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0), [hazırlama](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0), ve [üretim](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0). Varsa `ASPNETCORE_ENVIRONMENT` , varsayılan için ayarlı değil `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Önceki kod:

* Çağrıları [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) zaman `ASPNETCORE_ENVIRONMENT` ayarlanır `Development`.
* Çağrıları [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) zaman değerini `ASPNETCORE_ENVIRONMENT` aşağıdakilerden birini ayarlayın:

    * `Staging`
    * `Production`
    * `Staging_2`

[Ortam etiketi yardımcı ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) değerini kullanır `IHostingEnvironment.EnvironmentName` dahil etmek veya hariç biçimlendirme öğesi içinde:

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

Not: Windows ve macOS, ortam değişkenlerini ve değerleri büyük küçük harfe duyarlı değildir. Linux ortam değişkenlerini ve değerleri **büyük küçük harfe duyarlı** varsayılan olarak.

### <a name="development"></a>Geliştirme

Geliştirme ortamı üretimde kullanıma sunulan döndürmemelidir özellikleri etkinleştirebilirsiniz. Örneğin, ASP.NET Core Şablonları etkinleştirmek [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling#the-developer-exception-page) geliştirme ortamında.

Yerel makine geliştirme ortamını ayarlanabilir *Properties\launchSettings.json* projenin dosya. Ortam değerleri kümesinde *launchSettings.json* sistem ortama değerlerini geçersiz kılar.

Aşağıdaki JSON üç profillerden gösteren bir *launchSettings.json* dosyası:

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> `applicationUrl` Özelliğinde *launchSettings.json* sunucu URL'lerin bir listesini belirtebilirsiniz. Noktalı virgül listedeki URL'leri arasında kullanın:
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

Ne zaman uygulama başlatıldığında ile [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run), ilk profiliyle `"commandName": "Project"` kullanılır. Değeri `commandName` başlatmak için web sunucusunu belirtir. `commandName` aşağıdakilerden biri olabilir:

* IIS Express
* IIS
* (Kestrel başlatır) projesi

Ne zaman bir uygulama başlatıldığında ile [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run):

* *launchSettings.json* okunur varsa. `environmentVariables` ayarlarında *launchSettings.json* ortam değişkenleri geçersiz.
* Barındırma ortamı görüntülenir.

Aşağıdaki çıkış kullanmaya uygulama gösterir [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio **hata ayıklama** sekmesi sağlar düzenlemek için bir GUI *launchSettings.json* dosyası:

![Proje Özellikleri ayarını ortam değişkenleri](environments/_static/project-properties-debug.png)

Web sunucu yeniden başlatılana kadar proje profillere yapılan değişiklikler etkilerini göstermeyebilir. Kestrel, ortama yapılan değişiklikleri algılar önce başlatılması gerekir.

> [!WARNING]
> *launchSettings.json* parolaları depolamak döndürmemelidir. [Gizli Yöneticisi aracını](xref:security/app-secrets) yerel geliştirme için parolaları depolamak için kullanılır.

Kullanırken [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* dosya. Aşağıdaki örnek ortamını ayarlar `Development`:

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

A *.vscode/launch.json* proje dosyasında değil okuma uygulamayla başlatırken `dotnet run` aynı şekilde *Properties/launchSettings.json*. Bir uygulama yok geliştirme başlatılırken bir *launchSettings.json* dosya, bir ortam değişkeni veya bir komut satırı bağımsız değişkeni ortamıyla ayarladıktan `dotnet run` komutu.

### <a name="production"></a>Üretim

Üretim ortamında, güvenlik, performans ve uygulama sağlamlık en üst düzeye çıkarmak için yapılandırılmalıdır. Geliştirme farklı bazı ortak ayarlar şunlardır:

* Önbelleğe alma.
* İstemci-tarafı kaynaklar küçültülmüş, paketlenmiş ve potansiyel olarak bir CDN hizmet.
* Tanılama hata sayfaları devre dışı.
* Kolay hata sayfaları etkin.
* Üretim günlüğe kaydetme ve izleme etkin. Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="setting-the-environment"></a>Ortamını ayarlama

Genellikle, test etmek için belirli bir ortam ayarlamak yararlıdır. Ortam ayarlanmamışsa, varsayılan olur `Production` , devre dışı bırakır çoğu hata ayıklama özelliği.

Ortam ayarı yöntemi işletim sistemine bağlıdır.

### <a name="azure"></a>Azure

Azure uygulama hizmeti için:

* Seçin **uygulama ayarları** dikey.
* Anahtarın ekleyin ve değer **uygulama ayarları**.


### <a name="windows"></a>Windows
Ayarlamak için `ASPNETCORE_ENVIRONMENT` uygulamayı kullanmaya başladıysanız geçerli oturum için [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run), aşağıdaki komutları kullanılır

**Komut satırı**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Bu komutlar, yalnızca geçerli pencereyi için etkili olur. Pencere kapatıldığında ASPNETCORE_ENVIRONMENT ayarı varsayılan ayar veya bir makine değere geri döner. Windows Aç değeri genel olarak ayarlamak için **Denetim Masası** > **sistem** > **Gelişmiş Sistem ayarları** ve ekleme veya düzenleme`ASPNETCORE_ENVIRONMENT` değeri.

![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

![ASP.NET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)


**Web.config**

Bkz: *ortam değişkenlerini ayarlama* bölümünü [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) konu.

**Her IIS uygulama havuzu**

Ortam değişkenlerini (IIS 10.0 + desteklenir) yalıtılmış uygulama havuzlarında çalışan tek tek uygulamalar için ayarlamak için bkz: *AppCmd.exe komutunu* bölümünü [ortam değişkenleri \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konu.

### <a name="macos"></a>MacOS
Geçerli ortamı macOS için satır içi uygulama çalışırken yapılabilir;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
veya kullanarak `export` uygulama çalıştırılmadan önce ayarlamak için.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
Makine düzeyi ortam değişkenleri ayarlanır *.bashrc* veya *.bash_profile* dosya. Herhangi bir metin düzenleyicisi kullanarak dosyasını düzenleyin ve aşağıdaki ifadeyi ekleyin.

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Linux distro'lar için kullanmak `export` değişkeni ayarları oturum tabanlı için komut satırında komut ve *bash_profile* makine düzeyi ortam ayarları dosyası.

### <a name="configuration-by-environment"></a>Ortam yapılandırma

Bkz: [yapılandırma ortamı tarafından](xref:fundamentals/configuration/index#configuration-by-environment) daha fazla bilgi için.

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>Ortamı tabanlı, başlangıç sınıfı ve yöntemleri

Bir ASP.NET Core uygulama başlatıldığında [başlangıç sınıfı](xref:fundamentals/startup) uygulama bootstraps. Bir sınıf belirtilmemişse `Startup{EnvironmentName}` yoksa, sınıf için çağrılacağı `EnvironmentName`:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

Not: Çağırma [WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) yapılandırma bölümlerinin geçersiz kılar.

[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) ortam belirli formun sürümleri desteği `Configure{EnvironmentName}` ve `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Uygulama başlatma](xref:fundamentals/startup)
* [Yapılandırma](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
