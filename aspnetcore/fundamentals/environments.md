---
title: ASP.NET Core çoklu ortamları kullanma
author: rick-anderson
description: ASP.NET Core uygulamalarında birden çok ortamda uygulama davranışını denetlemeyi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/05/2019
uid: fundamentals/environments
ms.openlocfilehash: 91fa2a78e62dff65704a3dda826f45f27bad6064
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634090"
---
# <a name="use-multiple-environments-in-aspnet-core"></a>ASP.NET Core çoklu ortamları kullanma

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

ASP.NET Core, bir ortam değişkeni kullanarak çalışma zamanı ortamı temelinde uygulama davranışını yapılandırır.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="environments"></a>Lý

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core, uygulama başlangıcında `ASPNETCORE_ENVIRONMENT` ortam değişkenini okur ve değeri [ıwebhostenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName)içinde depolar. `ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir, ancak Framework tarafından üç değer sağlanır:

* <xref:Microsoft.Extensions.Hosting.Environments.Development>
* <xref:Microsoft.Extensions.Hosting.Environments.Staging>
* <xref:Microsoft.Extensions.Hosting.Environments.Production> (varsayılan)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core, uygulama başlangıcında `ASPNETCORE_ENVIRONMENT` ortam değişkenini okur ve değeri [ıhostingenvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName)içinde depolar. `ASPNETCORE_ENVIRONMENT` herhangi bir değere ayarlanabilir, ancak Framework tarafından üç değer sağlanır:

* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Staging>
* <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Production> (varsayılan)

::: moniker-end

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet)]

Önceki kod:

* `ASPNETCORE_ENVIRONMENT` `Development`olarak ayarlandığında, [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) çağırır.
* `ASPNETCORE_ENVIRONMENT` değeri aşağıdakilerden birini ayarladığınızda [Useexceptionhandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler) ' i çağırır:

  * `Staging`
  * `Production`
  * `Staging_2`

[Ortam etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) , öğesinde biçimlendirme eklemek veya dışlamak için `IHostingEnvironment.EnvironmentName` değerini kullanır:

[!code-cshtml[](environments/sample-snapshot/EnvironmentsSample/Pages/About.cshtml)]

Windows ve macOS 'ta, ortam değişkenleri ve değerleri büyük/küçük harfe duyarlı değildir. Linux ortam değişkenleri ve değerleri varsayılan olarak **büyük/küçük harfe duyarlıdır** .

### <a name="development"></a>Geliştirme

Geliştirme ortamı, üretimde gösterilmemelidir özellikleri etkinleştirebilir. Örneğin, ASP.NET Core Şablonlar geliştirme ortamında [Geliştirici özel durum sayfasını](xref:fundamentals/error-handling#developer-exception-page) etkinleştirir.

Yerel makine geliştirme ortamı, projenin *Properties\launchSettings.JSON* dosyasında ayarlanabilir. *Launchsettings. JSON* geçersiz kılma değerlerini sistem ortamında ayarlanan ortam değerleri.

Aşağıdaki JSON, bir *Launchsettings. JSON* dosyasından üç profil gösterir:

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
> *Launchsettings. JSON* içindeki `applicationUrl` özelliği sunucu URL 'lerinin bir listesini belirtebilir. Listedeki URL 'Ler arasında noktalı virgül kullanın:
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

Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında, `"commandName": "Project"` ilk profili kullanılır. `commandName` değeri, başlatılacak Web sunucusunu belirtir. `commandName` aşağıdakilerden biri olabilir:

* `IISExpress`
* `IIS`
* `Project` (Kestrel Başlatan)

Bir uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatıldığında:

* *Launchsettings. JSON* varsa okundu. *launchsettings. JSON* geçersiz kılma ortamı değişkenlerine `environmentVariables` ayarları.
* Barındırma ortamı görüntülenir.

Aşağıdaki çıktıda, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)ile başlatılan bir uygulama gösterilmektedir:

```bash
PS C:\Websites\EnvironmentsSample> dotnet run
Using launch settings from C:\Websites\EnvironmentsSample\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Websites\EnvironmentsSample
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio proje özellikleri **hata ayıklama** sekmesi, *launchsettings. JSON* dosyasını düzenlemek için bir GUI sağlar:

![Proje özellikleri ayar ortamı değişkenleri](environments/_static/project-properties-debug.png)

Proje profillerinde yapılan değişiklikler, Web sunucusu yeniden başlatılana kadar etkili olmayabilir. Kestrel, ortamında yapılan değişiklikleri algılayabilmesi için yeniden başlatılmalıdır.

> [!WARNING]
> *Launchsettings. JSON* gizli dizileri depolamamamalıdır. Gizli anahtar geliştirme için gizli dizileri depolamak için [gizli dizi Yöneticisi aracı](xref:security/app-secrets) kullanılabilir.

[Visual Studio Code](https://code.visualstudio.com/)kullanırken, ortam değişkenleri *. vscode/Launch. JSON* dosyasında ayarlanabilir. Aşağıdaki örnek, `Development`için ortamı ayarlar:

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

Uygulama `dotnet run` *Özellikler/launchSettings. JSON*ile aynı şekilde başlatılırken, projedeki bir *. vscode/Launch. JSON* dosyası okunamaz. Geliştirme sırasında *Launchsettings. JSON* dosyası olmayan bir uygulama başlatırken, ortam değişkeni veya bir komut satırı bağımsız değişkeni olan ortamı `dotnet run` komutuna ayarlayın.

### <a name="production"></a>Üretiminden

Üretim ortamının güvenliği, performansı ve uygulama sağlamlık düzeyini en üst düzeye çıkarmak için yapılandırılması gerekir. Geliştirmeden farklı bazı yaygın ayarlar şunlardır:

* Önbelleği.
* İstemci tarafı kaynaklar paketlenmiş, küçültülmüş ve potansiyel olarak bir CDN 'den sunulan.
* Tanılama hata sayfaları devre dışı.
* Kolay hata sayfaları etkin.
* Üretim günlüğü ve izleme etkin. Örneğin, [Application Insights](/azure/application-insights/app-insights-asp-net-core).

## <a name="set-the-environment"></a>Ortamı ayarlama

Bir ortam değişkeni veya platform ayarıyla test için belirli bir ortam ayarlamak genellikle yararlıdır. Ortam ayarlanmamışsa, çoğu hata ayıklama özelliğini devre dışı bırakan `Production`varsayılan olarak ayarlanır. Ortamı ayarlama yöntemi işletim sistemine bağlıdır.

Konak yapılandırıldığında, uygulama tarafından okunan son ortam ayarı, uygulamanın ortamını belirler. Uygulama çalışırken uygulamanın ortamı değiştirilemez.

### <a name="environment-variable-or-platform-setting"></a>Ortam değişkeni veya platform ayarı

#### <a name="azure-app-service"></a>Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/)ortamında ortamı ayarlamak için aşağıdaki adımları gerçekleştirin:

1. Uygulama **Hizmetleri** dikey penceresinden uygulamayı seçin.
1. **Ayarlar** grubunda, **uygulama ayarları** dikey penceresini seçin.
1. **Uygulama ayarları** alanında **yeni ayar Ekle**' yi seçin.
1. **Ad girin**için `ASPNETCORE_ENVIRONMENT`sağlayın. **Değer girin**için ortamı sağlayın (örneğin, `Staging`).
1. Ortam ayarının, dağıtım yuvaları takas edildiğinde geçerli yuvada kalmasını istiyorsanız, **yuva ayarı** onay kutusunu işaretleyin. Daha fazla bilgi için bkz. [Azure belgeleri: hangi ayarları değiştirmiş?](/azure/app-service/web-sites-staged-publishing).
1. Dikey pencerenin en üstünde **Kaydet** ' i seçin.

Azure App Service, Azure portal bir uygulama ayarı (ortam değişkeni) eklendikten, değiştirildikten veya silindikten sonra uygulamayı otomatik olarak yeniden başlatır.

#### <a name="windows"></a>Windows

Uygulama [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)kullanılarak başlatıldığında geçerli oturumun `ASPNETCORE_ENVIRONMENT` ayarlamak için aşağıdaki komutlar kullanılır:

**Komut istemi**

```console
set ASPNETCORE_ENVIRONMENT=Development
```

**PowerShell**

```powershell
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Bu komutlar yalnızca geçerli pencere için etkili olur. Pencere kapatıldığında, `ASPNETCORE_ENVIRONMENT` ayarı varsayılan ayar veya makine değerine geri döner.

Windows 'da genel değeri ayarlamak için aşağıdaki yaklaşımlardan birini kullanın:

* **Sistem** > **gelişmiş sistem ayarları** > **denetim masası** 'nı açın ve `ASPNETCORE_ENVIRONMENT` değerini ekleyin veya düzenleyin:

  ![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

  ![ASPNET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png)

* Bir yönetim komut istemi açın ve `setx` komutunu kullanın veya bir yönetim PowerShell komut istemi açın ve `[Environment]::SetEnvironmentVariable`kullanın:

  **Komut istemi**

  ```console
  setx ASPNETCORE_ENVIRONMENT Development /M
  ```

  `/M` anahtarı, ortam değişkenini sistem düzeyinde ayarlamaya yönelik olduğunu gösterir. `/M` anahtarı kullanılmazsa, ortam değişkeni Kullanıcı hesabı için ayarlanır.

  **PowerShell**

  ```powershell
  [Environment]::SetEnvironmentVariable("ASPNETCORE_ENVIRONMENT", "Development", "Machine")
  ```

  `Machine` seçenek değeri, ortam değişkeninin sistem düzeyinde ayarlandığını gösterir. Seçenek değeri `User`olarak değiştirilirse, ortam değişkeni Kullanıcı hesabı için ayarlanır.

`ASPNETCORE_ENVIRONMENT` ortam değişkeni genel olarak ayarlandığında, değer ayarlandıktan sonra açılan herhangi bir komut penceresinde `dotnet run` için geçerli olur.

**Web. config**

`ASPNETCORE_ENVIRONMENT` ortam değişkenini *Web. config*ile ayarlamak için, <xref:host-and-deploy/aspnet-core-module#setting-environment-variables>*ortam değişkenlerini ayarlama* bölümüne bakın.

::: moniker range=">= aspnetcore-2.2"

**Proje dosyası veya yayımlama profili**

**WINDOWS IIS dağıtımları için:** `<EnvironmentName>` özelliğini Publish profile ( *. pubxml*) veya proje dosyasına ekleyin. Bu yaklaşım, proje yayımlandığında *Web. config* içinde ortamı ayarlar:

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

::: moniker-end

**IIS uygulama havuzu başına**

Yalıtılmış uygulama havuzunda çalışan bir uygulamanın `ASPNETCORE_ENVIRONMENT` ortam değişkenini ayarlamak için (IIS 10,0 veya üzeri sürümlerde desteklenir), [ortam değişkenlerinin &lt;environmentVariables&gt;](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) konusunun *Appcmd. exe komut* bölümüne bakın. `ASPNETCORE_ENVIRONMENT` ortam değişkeni bir uygulama havuzu için ayarlandığında, değeri sistem düzeyindeki bir ayarı geçersiz kılar.

> [!IMPORTANT]
> IIS 'de bir uygulama barındırırken ve `ASPNETCORE_ENVIRONMENT` ortam değişkenini ekleyerek veya değiştirirken, yeni değerin uygulamalar tarafından çekilmek için aşağıdaki yaklaşımlardan birini kullanın:
>
> * `net stop was /y` ve ardından komut isteminden `net start w3svc` yürütün.
> * Sunucuyu yeniden başlatın.

#### <a name="macos"></a>macOS

MacOS için geçerli ortamın ayarlanması, uygulamayı çalıştırırken satır içinde gerçekleştirilebilir:

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```

Alternatif olarak, uygulamayı çalıştırmadan önce ortamı `export` ayarlayın:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

Makine düzeyinde ortam değişkenleri *. bashrc* veya *. bash_profile* dosyasında ayarlanır. Herhangi bir metin düzenleyicisini kullanarak dosyayı düzenleyin. Aşağıdaki ifadeyi ekleyin:

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

#### <a name="linux"></a>Linux

Linux distros için, makine düzeyindeki ortam ayarları için oturum tabanlı değişken ayarları ve *bash_profile* dosyası için bir komut isteminde `export` komutunu kullanın.

### <a name="set-the-environment-in-code"></a>Kodda ortam ayarlama

::: moniker range=">= aspnetcore-3.0"

Konağı oluştururken <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseEnvironment*> çağırın. Bkz. <xref:fundamentals/host/generic-host#environmentname>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Konağı oluştururken <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseEnvironment*> çağırın. Bkz. <xref:fundamentals/host/web-host#environment>.

::: moniker-end

### <a name="configuration-by-environment"></a>Ortama göre yapılandırma

Yapılandırmayı ortama göre yüklemek için şunları yapmanızı öneririz:

::: moniker range=">= aspnetcore-3.0"

* *appSettings* dosyaları (*appSettings. { Environment}. JSON*). Bkz. <xref:fundamentals/configuration/index#json-configuration-provider>.
* Ortam değişkenleri (uygulamanın barındırıldığı her bir sistemde ayarlanır). Bkz. <xref:fundamentals/host/generic-host#environmentname> ve <xref:security/app-secrets#environment-variables>.
* Gizli dizi Yöneticisi (yalnızca geliştirme ortamında). Bkz. <xref:security/app-secrets>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* *appSettings* dosyaları (*appSettings. { Environment}. JSON*). Bkz. <xref:fundamentals/configuration/index#json-configuration-provider>.
* Ortam değişkenleri (uygulamanın barındırıldığı her bir sistemde ayarlanır). Bkz. <xref:fundamentals/host/web-host#environment> ve <xref:security/app-secrets#environment-variables>.
* Gizli dizi Yöneticisi (yalnızca geliştirme ortamında). Bkz. <xref:security/app-secrets>.

::: moniker-end

## <a name="environment-based-startup-class-and-methods"></a>Ortam tabanlı başlangıç sınıfı ve yöntemleri

::: moniker range=">= aspnetcore-3.0"

### <a name="inject-iwebhostenvironment-into-startupconfigure"></a>Iwebhostenvironment 'ı başlatmaya ekleme. configure

<xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> `Startup.Configure`ekleme. Bu yaklaşım, uygulama yalnızca, ortam başına en az kod farklılığı olan birkaç ortam için `Startup.Configure` ayarlamayı gerektirdiğinde yararlıdır.

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

### <a name="inject-iwebhostenvironment-into-the-startup-class"></a>Başlangıç sınıfına ıwebhostenvironment ekleme

`Startup` oluşturucusuna <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> ekleyin. Bu yaklaşım, uygulama her ortam için en az kod farklılığı olan birkaç ortam için `Startup` yapılandırmayı gerektirdiğinde yararlıdır.

Aşağıdaki örnekte:

* Ortam `_env` alanında tutulur.
* `_env`, `ConfigureServices` ve `Configure` uygulamanın ortamına göre başlangıç yapılandırmasını uygulamak için kullanılır.

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

### <a name="inject-ihostingenvironment-into-startupconfigure"></a>Başlangıç olarak ıhostingenvironment ekleme. yapılandırma

<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> `Startup.Configure`ekleme. Bu yaklaşım, uygulama yalnızca, ortam başına en az kod farklılığı olan birkaç ortam için `Startup.Configure` yapılandırmayı gerektirdiğinde yararlıdır.

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

### <a name="inject-ihostingenvironment-into-the-startup-class"></a>Başlangıç sınıfına ıhostingenvironment ekleme

`Startup` oluşturucusuna <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment> ekleyin ve hizmeti `Startup` sınıfı boyunca kullanmak üzere bir alana atayın. Bu yaklaşım, uygulama her ortam için en az kod farklılığı olan birkaç ortam için başlatma yapılandırması gerektirdiğinde faydalıdır.

Aşağıdaki örnekte:

* Ortam `_env` alanında tutulur.
* `_env`, `ConfigureServices` ve `Configure` uygulamanın ortamına göre başlangıç yapılandırmasını uygulamak için kullanılır.

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

### <a name="startup-class-conventions"></a>Başlangıç sınıfı kuralları

ASP.NET Core bir uygulama başlatıldığında, [Başlangıç sınıfı](xref:fundamentals/startup) uygulamayı önyükleme. Uygulama farklı ortamlar için ayrı `Startup` sınıfları tanımlayabilir (örneğin, `StartupDevelopment`). Uygun `Startup` sınıfı çalışma zamanında seçildi. Geçerli ortamla eşleşen ad sonekine sahip olan sınıf önceliklendirilir. Eşleşen bir `Startup{EnvironmentName}` sınıfı bulunamazsa, `Startup` sınıfı kullanılır. Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.

Ortam tabanlı `Startup` sınıfları uygulamak için, kullanımdaki her ortam için bir `Startup{EnvironmentName}` sınıfı ve bir geri dönüş `Startup` sınıfı oluşturun:

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

Derleme adını kabul eden [Usestartup (ıwebhostbuilder, String)](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usestartup) aşırı yüklemesini kullanın:

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

### <a name="startup-method-conventions"></a>Başlangıç yöntemi kuralları

[Yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices) , form `Configure<EnvironmentName>` ve `Configure<EnvironmentName>Services`ortama özgü sürümlerini destekler. Bu yaklaşım, uygulama başına çok sayıda kod farklılığı olan birkaç ortam için başlangıç yapılandırması gerektirdiğinde yararlıdır.

[!code-csharp[](environments/sample/EnvironmentsSample/Startup.cs?name=snippet_all&highlight=15,42)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/startup>
* <xref:fundamentals/configuration/index>
