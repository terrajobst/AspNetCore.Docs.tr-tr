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
# <a name="use-multiple-environments-in-aspnet-core"></a>ASP.NET Core kullanan birden çok ortamlar

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core bir ortam değişkeni kullanarak çalışma zamanı ortamı tabanlı uygulama davranışını yapılandırır.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="environments"></a>Ortamlar

ASP.NET Core okur ortam değişkeni `ASPNETCORE_ENVIRONMENT` uygulama başlangıçta ve değeri depolar [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Ayarlayabileceğiniz `ASPNETCORE_ENVIRONMENT` herhangi bir değere ancak [üç değerden](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) framework tarafından desteklenir: [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [hazırlama](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), ve [üretim](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Varsa `ASPNETCORE_ENVIRONMENT` , varsayılan olarak, ayarlı değil `Production`.

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

Önceki kod:

* Çağrıları [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) ve [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink) zaman `ASPNETCORE_ENVIRONMENT` ayarlanır `Development`.
* Çağrıları [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) zaman değerini `ASPNETCORE_ENVIRONMENT` aşağıdakilerden birini ayarlayın:

    * `Staging`
    * `Production`
    * `Staging_2`

[Ortam etiketi yardımcı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) değerini kullanır `IHostingEnvironment.EnvironmentName` dahil etmek veya hariç biçimlendirme öğesi içinde:

[!code-cshtml[](environments/sample-snapshot/WebApp1/Pages/About.cshtml)]

Windows ve macOS, ortam değişkenlerini ve değerleri büyük küçük harfe duyarlı değildir. Linux ortam değişkenlerini ve değerleri **büyük küçük harfe duyarlı** varsayılan olarak.

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

Ne zaman uygulama başlatıldığında ile [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run), ilk profiliyle `"commandName": "Project"` kullanılır. Değeri `commandName` başlatmak için web sunucusunu belirtir. `commandName` aşağıdakilerden herhangi biri olabilir:

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

Web sunucu yeniden başlatılana kadar proje profillere yapılan değişiklikler etkilerini göstermeyebilir. Kestrel, ortama yapılan değişiklikleri algılayabilir önce başlatılması gerekir.

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

Genellikle, test etmek için belirli bir ortam ayarlamak yararlıdır. Ortam ayarlanmamışsa, varsayılan olarak `Production`, hangi devre dışı bırakır çoğu hata ayıklama özelliği. Ortam ayarı yöntemi işletim sistemine bağlıdır.

### <a name="azure-app-service"></a>Azure uygulama hizmeti

Ortam ayarlamak için [Azure App Service](https://azure.microsoft.com/services/app-service/), aşağıdaki adımları gerçekleştirin:

1. Uygulamadan seçin **uygulama hizmetleri** dikey.
1. İçinde **ayarları** grup, select **uygulama ayarları** dikey.
1. İçinde **uygulama ayarları** alanında **yeni ayar Ekle**.
1. İçin **bir ad girin**, sağlayan `ASPNETCORE_ENVIRONMENT`. İçin **bir değer girin**, ortam sağlamak (örneğin, `Staging`).
1. Seçin **yuva ayarı** dağıtım yuvaları takas olduğunda geçerli yuvasıyla kalmasına ortamı ayarı istiyorsanız kutuyu. Daha fazla bilgi için bkz: [Azure belgelerine: hangi ayarların takas?](/azure/app-service/web-sites-staged-publishing).
1. Seçin **kaydetmek** dikey pencerenin üstündeki.

Bir uygulama ayarı (ortam değişkeni) eklendiğinde, değiştirilen veya Azure portalında silindiğinde sonra azure uygulama hizmeti uygulaması otomatik olarak yeniden başlatır.

### <a name="windows"></a>Windows

Ayarlamak için `ASPNETCORE_ENVIRONMENT` uygulama başlatıldığında geçerli oturum için kullanarak [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run), aşağıdaki komutları kullanılır:

**Komut İstemi**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Bu komutlar, yalnızca geçerli penceresinin etkili olur. Pencere kapatıldığında `ASPNETCORE_ENVIRONMENT` ayar varsayılan ayarı veya makine değeri döner. Değer Windows'da genel olarak ayarlamak için açık **Denetim Masası** > **sistem** > **Gelişmiş Sistem ayarları** ve ekleme veya düzenleme `ASPNETCORE_ENVIRONMENT`değeri:

![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

![ASP.NET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

**Web.config**

Bkz: *ortam değişkenlerini ayarlama* bölümünü [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) konu.

**Her IIS uygulama havuzu**

Ortam değişkenlerini (IIS 10.0 + desteklenir) yalıtılmış uygulama havuzlarında çalışan tek tek uygulamalar için ayarlamak için bkz: *AppCmd.exe komutunu* bölümünü [ortam değişkenleri &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konu.

### <a name="macos"></a>macOS

MacOS olabilir geçerli ortamı ayarı satır içi uygulama çalıştırıldığında, gerçekleştirilen:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Alternatif olarak, ortamıyla kümesinin `export` uygulama çalıştırılmadan önce:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Makine düzeyinde ortam değişkenlerini ayarlama *.bashrc* veya *.bash_profile* dosya. Herhangi bir metin düzenleyicisi kullanarak dosyayı düzenleyin. Aşağıdaki deyim ekleyin:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

Linux distro'lar için kullanmak `export` oturum tabanlı değişken ayarları için bir komut isteminde komutunu ve *bash_profile* makine düzeyinde ortam ayarları dosyası.

### <a name="configuration-by-environment"></a>Ortam yapılandırma

Bkz: [yapılandırma ortamı tarafından](xref:fundamentals/configuration/index#configuration-by-environment) daha fazla bilgi için.

## <a name="environment-based-startup-class-and-methods"></a>Ortam tabanlı başlangıç sınıfı ve yöntemleri

Bir ASP.NET Core uygulama başlatıldığında [başlangıç sınıfı](xref:fundamentals/startup) uygulama bootstraps. Varsa bir `Startup{EnvironmentName}` sınıfı yok, sınıf için adlandırılır `EnvironmentName`:

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

[WebHostBuilder.UseStartup<TStartup> ](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) yapılandırma bölümlerinin geçersiz kılar.

[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) biçiminde ortama özgü sürümleri destekler `Configure{EnvironmentName}` ve `Configure{EnvironmentName}Services`:

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Uygulama başlatma](xref:fundamentals/startup)
* [Yapılandırma](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
