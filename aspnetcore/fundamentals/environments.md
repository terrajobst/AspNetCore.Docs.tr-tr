---
title: ASP.NET Core birden çok ortam kullanma
author: rick-anderson
description: ASP.NET Core uygulamaları birden fazla ortam arasında uygulama davranışını denetleme konusunda bilgi edinin.
ms.author: riande
ms.date: 01/22/2019
uid: fundamentals/environments
ms.openlocfilehash: 5982f3e51a68dfa29af482067156c42006f50c0c
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208465"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>ASP.NET Core birden çok ortam kullanma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core, bir ortam değişkeni kullanarak çalışma zamanı ortama göre uygulama davranışını yapılandırır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="environments"></a>Ortamlar

ASP.NET Core ortam değişkenini okur `ASPNETCORE_ENVIRONMENT` uygulamanın başlangıcında ve değeri depolar [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname). Ayarlayabileceğiniz `ASPNETCORE_ENVIRONMENT` herhangi bir değere ancak [üç değerden](/dotnet/api/microsoft.aspnetcore.hosting.environmentname) framework tarafından desteklenir: [Geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development), [hazırlama](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging), ve [üretim](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production). Varsa `ASPNETCORE_ENVIRONMENT` değil, varsayılan olarak, belirlenen `Production`.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

Yukarıdaki kod:

* Çağrıları [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) olduğunda `ASPNETCORE_ENVIRONMENT` ayarlanır `Development`.
* Çağrıları [UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) zaman değerini `ASPNETCORE_ENVIRONMENT` aşağıdakilerden birine ayarlanır:

  * `Staging`
  * `Production`
  * `Staging_2`

[Ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) değerini kullanır `IHostingEnvironment.EnvironmentName` dahil edin veya dışlayın öğesinde bulunan işaretleme:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

Windows ve macOS üzerinde ortam değişkenlerini ve değerleri büyük küçük harfe duyarlı değildir. Linux ortam değişkenlerini ve değerleri **büyük küçük harfe duyarlı** varsayılan olarak.

### <a name="development"></a>Geliştirme

Geliştirme ortamının, üretim ortamında kullanıma sunulan olmamalıdır özellikleri etkinleştirebilirsiniz. Örneğin, ASP.NET Core şablonları etkinleştirme [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling#developer-exception-page) geliştirme ortamında.

Yerel makine geliştirme ortamını ayarlanabilir *Properties\launchSettings.json* proje dosyası. Ortam değerlerini kümesinde *launchSettings.json* sistemi ortamında ayarlanan değerleri geçersiz.

Aşağıdaki JSON üç profillerden gösterir bir *launchSettings.json* dosyası:

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
> `applicationUrl` Özelliğinde *launchSettings.json* sunucu URL'lerin bir listesini belirtebilirsiniz. Listedeki URL'leri arasında noktalı virgül kullanın:
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

Ne zaman uygulama başlatıldığında ile [çalıştırma dotnet](/dotnet/core/tools/dotnet-run), ilk profiliyle `"commandName": "Project"` kullanılır. Değerini `commandName` başlatmak için web sunucusunu belirtir. `commandName` aşağıdakilerden herhangi biri olabilir:

* `IISExpress`
* `IIS`
* `Project` (hangi Kestrel başlatır)

Ne zaman bir uygulama başlatıldığında ile [çalıştırma dotnet](/dotnet/core/tools/dotnet-run):

* *launchSettings.json* okunur varsa. `environmentVariables` ayarlarında *launchSettings.json* ortam değişkenlerini geçersiz kılar.
* Barındırma ortamı görüntülenir.

Bir uygulama kullanmaya aşağıdaki çıktıyı gösterir [çalıştırma dotnet](/dotnet/core/tools/dotnet-run):

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio Proje özelliklerini **hata ayıklama** sekmesi düzenlemek için bir GUI sağlar *launchSettings.json* dosyası:

![Proje Özellikleri ayarı ortam değişkenleri](environments/_static/project-properties-debug.png)

Proje profillere yapılan değişiklikler web sunucu yeniden başlatılana kadar etkili değildir. Kestrel'i, ortama yapılan değişiklikleri algılayabilir önce başlatılması gerekir.

> [!WARNING]
> *launchSettings.json* gizli dizileri depolamak olmamalıdır. [Gizli dizi Yöneticisi aracını](xref:security/app-secrets) yerel geliştirme için gizli dizileri depolamak için kullanılabilir.

Kullanırken [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* dosya. Aşağıdaki örnek ortamı ayarlar `Development`:

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

A *.vscode/launch.json* proje dosyasında değil okuma uygulamayı başlatırken `dotnet run` aynı şekilde *Properties/launchSettings.json*. Bir uygulamaya sahip olmayan geliştirme başlatırken bir *launchSettings.json* dosyası ortamında bir ortam değişkeni veya bir komut satırı bağımsız değişkeni ile ayarlanan `dotnet run` komutu.

### <a name="production"></a>Üretim

Üretim ortamında, güvenlik, performans ve uygulama sağlamlık en üst düzeye çıkarmak için yapılandırılmalıdır. Geliştirme farklı bazı ortak ayarlar şunlardır:

* Önbelleğe alma.
* İstemci tarafı kaynakları küçültülmüş, toplanmış ve potansiyel olarak cdn'den.
* Tanılama hata sayfalarını devre dışı.
* Kolay hata sayfalarını etkin.
* Üretim günlüğe kaydetme ve izleme etkin. Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Ortamı ayarlama

Genellikle, test etmek için belirli bir ortamı kullanışlıdır. Ortam ayarlanmadıysa, varsayılan `Production`, hangi devre dışı bırakır çoğu hata ayıklama özellikleri. Ortamını yöntemi işletim sistemine bağlıdır.

### <a name="azure-app-service"></a>Azure uygulama hizmeti

Ortamı ayarlamak için [Azure App Service](https://azure.microsoft.com/services/app-service/), aşağıdaki adımları gerçekleştirin:

1. Uygulamadan seçin **uygulama hizmetleri** dikey penceresi.
1. İçinde **ayarları** grubu, select **uygulama ayarları** dikey penceresi.
1. İçinde **uygulama ayarları** alanında **yeni ayar Ekle**.
1. İçin **bir ad girin**, sağlayan `ASPNETCORE_ENVIRONMENT`. İçin **bir değer girin**, ortamı sağlayın (örneğin, `Staging`).
1. Seçin **yuva ayarı** ortam ayarını, dağıtım yuvaları takas zaman ile geçerli yuvadaki kalmasına istiyorsanız kutuyu. Daha fazla bilgi için [Azure belgeleri: Hangi ayarların değiştirilir? ](/azure/app-service/web-sites-staged-publishing).
1. Seçin **Kaydet** dikey penceresinin üstünde.

Bir uygulama ayarı (ortam değişkeni) eklenmesine, değiştirilmesine veya Azure portalda silinen sonra azure App Service uygulama otomatik olarak yeniden başlatılır.

### <a name="windows"></a>Windows

Ayarlanacak `ASPNETCORE_ENVIRONMENT` uygulama başlatıldığında geçerli oturum için kullanarak [çalıştırma dotnet](/dotnet/core/tools/dotnet-run), aşağıdaki komutları kullanılır:

**Komut İstemi**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Bu komutları yalnızca geçerli pencere için etkili. Penceresi kapatıldığında `ASPNETCORE_ENVIRONMENT` ayarı varsayılan ayar veya bir makine değere döner.

Değerini genel olarak Windows içinde ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:

* Açık **Denetim Masası** > **sistem** > **Gelişmiş Sistem ayarları** ve ekleme veya düzenleme `ASPNETCORE_ENVIRONMENT` değeri:

  ![Sistem Gelişmiş özellikleri](environments/_static/systemsetting_environment.png)

  ![ASP.NET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

* Bir yönetici komut istemi açın ve kullanmak `setx` komutu veya bir yönetici PowerShell komut istemi açın ve kullanmak `[Environment]::SetEnvironmentVariable`:

  **Komut İstemi**

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  `/M` Sistem düzeyinde ortam değişkenini ayarlamak için anahtar belirtir. Varsa `/M` anahtar kullanılmaz, kullanıcı hesabı için ortam değişkeni ayarlanır.

  **PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  `Machine` Sistem düzeyinde ortam değişkenini ayarlamak için seçeneği değeri gösterir. Seçenek değeri değiştirilirse `User`, kullanıcı hesabı için ortam değişkeni ayarlanır.

Zaman `ASPNETCORE_ENVIRONMENT` ortam değişkeni genel olarak ayarlandığında, etkili olur `dotnet run` değer ayarlandıktan sonra herhangi bir komut penceresinde açılır.

**Web.config**

Ayarlanacak `ASPNETCORE_ENVIRONMENT` ortam değişkeni ile *web.config*, bkz: *ortam değişkenlerini ayarlama* bölümünü <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>. Zaman `ASPNETCORE_ENVIRONMENT` ile ortam değişkeninin ayarlı *web.config*, sistem düzeyindeki bir ayarı değerini geçersiz kılar.

::: moniker range=">= aspnetcore-2.2"

**Proje dosyası veya yayımlama profili**

**Windows IIS dağıtımlar için:** Dahil `<EnvironmentName>` yayımlama profilini özelliğinde (*.pubxml*) ya da proje dosyası. Bu yaklaşım ortamı ayarlar *web.config* proje yayımlandığında ne zaman:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

::: moniker-end

**IIS uygulama havuzu başına**

Ayarlanacak `ASPNETCORE_ENVIRONMENT` bir yalıtılmış uygulama (IIS 10.0 veya sonraki sürümlerde desteklenir) havuzunda, bkz: çalışan bir uygulama için ortam değişkenini *AppCmd.exe komut* bölümünü [ortam değişkenlerini &lt; environmentVariables&gt; ](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konu. Zaman `ASPNETCORE_ENVIRONMENT` ortam değişkeni için bir uygulama havuzu ayarlandığında, sistem düzeyindeki bir ayarı değerini geçersiz kılar.

> [!IMPORTANT]
> IIS'de bir uygulamanın barındırma ve ekleme veya değiştirme `ASPNETCORE_ENVIRONMENT` aşağıdakilerden herhangi birini yaklaşıyor uygulamalar tarafından toplanmış yeni değerine sahip olacak şekilde ortam değişkeni kullanın:
>
> * Yürütme `net stop was /y` ardından `net start w3svc` bir komut isteminden.
> * Sunucuyu yeniden başlatın.

### <a name="macos"></a>macOS

MacOS olabilir geçerli ortamı ayarı satır içi uygulamayı çalıştırırken gerçekleştirmiştir:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Alternatif olarak, ortam kümesi `export` uygulamayı çalıştırmadan önce:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Makine düzeyinde ortam değişkenlerini ayarlanır *.bashrc* veya *.bash_profile* dosya. Herhangi bir metin düzenleyicisi kullanarak dosyayı düzenleyin. Aşağıdaki deyimi ekleyin:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux

Linux dağıtımları için kullanmak `export` oturum tabanlı değişken ayarları için bir komut isteminde komutunu ve *bash_profile* makine düzeyinde ortam ayarları dosyası.

### <a name="configuration-by-environment"></a>Ortama göre yapılandırma

Yapılandırma ortamı tarafından yüklenecek öneririz:

* *appSettings* dosyaları (*appsettings.\< Ortam > .json*). Bkz: [yapılandırma: Dosya yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#file-configuration-provider).
* ortam değişkenleri (her sisteminde uygulamanın barındırıldığı ayarlanır). Bkz: [yapılandırma: Dosya yapılandırma sağlayıcısı](xref:fundamentals/configuration/index#file-configuration-provider) ve [geliştirmede uygulama gizli anahtarlarının Güvenli Depolama: Ortam değişkenlerini](xref:security/app-secrets#environment-variables).
* Gizli dizi Yöneticisi (geliştirme ortamındaki yalnızca). Bkz. <xref:security/app-secrets>.

## <a name="environment-based-startup-class-and-methods"></a>Ortam tabanlı başlangıç sınıfı ve yöntemleri

### <a name="startup-class-conventions"></a>Başlangıç sınıfı kuralları

ASP.NET Core uygulaması başladığında [başlangıç sınıfı](xref:fundamentals/startup) uygulama bootstraps. Uygulamayı ayrı tanımlayabilirsiniz `Startup` sınıflar farklı ortamlar için (örneğin, `StartupDevelopment`) ve uygun `Startup` sınıfı, çalışma zamanında seçilidir. Geçerli ortamı olan adı sonekiyle sınıfı kurtarılmasına öncelik verilir. Eşleşen bir `Startup{EnvironmentName}` sınıfı değil bulundu, `Startup` sınıfı kullanılır.

Ortam tabanlı uygulamak için `Startup` sınıfları oluşturma bir `Startup{EnvironmentName}` her ortamda kullanın ve bir geri dönüş için sınıf `Startup` sınıfı:

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

Kullanım [UseStartup (IWebHostBuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) bir derleme adı kabul eden aşırı yükleme:

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

### <a name="startup-method-conventions"></a>Başlangıç yöntem kuralları

[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) formun ortama özgü sürümlerini destekleyen `Configure<EnvironmentName>` ve `Configure<EnvironmentName>Services`:

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,51)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname)
