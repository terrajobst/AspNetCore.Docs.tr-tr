---
title: ASP.NET Core Web ana bilgisayarı
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu olan ASP.NET Core Web ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: b391b5e514e750f64f30d33cf4eb91e489242eba
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901580"
---
# <a name="aspnet-core-web-host"></a>ASP.NET Core Web ana bilgisayarı

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core uygulamaları yapılandırmak ve başlatmak bir *konak*. Uygulama başlatma ve ömür yönetimi için konak sorumludur. En az bir sunucu ve istek işleme ardışık konak yapılandırır. Konak, günlüğe kaydetme, bağımlılık ekleme ve yapılandırma de ayarlayabilirsiniz.

::: moniker range="<= aspnetcore-1.1"

Bu konuda 1.1 sürümü için indirme [ASP.NET Core Web ana bilgisayarı (sürüm 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Bu makalede, ASP.NET Core Web ana bilgisayarı yer almaktadır (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), web uygulamalarını barındırmak için olduğu. .NET genel ana bilgisayar hakkında bilgi için ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), bkz: <xref:fundamentals/host/generic-host>.

::: moniker-end

::: moniker range="> aspnetcore-2.2"

Bu makalede, ASP.NET Core Web ana bilgisayarı yer almaktadır ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)). ASP.NET Core 3.0 sürümünde, Web barındırma genel ana bilgisayar değiştirir. Daha fazla bilgi için [konak](xref:fundamentals/index#host).

::: moniker-end

## <a name="set-up-a-host"></a>Bir ana bilgisayar kümesi

Bir konak örneği kullanarak oluşturma [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Bu, genellikle uygulamanın Giriş noktasında gerçekleştirilir `Main` yöntemi. Proje şablonlarındaki `Main` bulunan *Program.cs*. Tipik bir *Program.cs* çağrıları [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) bir konak kurulumunu başlatmak için:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

`CreateDefaultBuilder` Aşağıdaki görevleri gerçekleştirir:

* Yapılandırır [Kestrel](xref:fundamentals/servers/kestrel) uygulamayı kullanarak web sunucusu olarak yapılandırma sağlayıcıları barındıran. Kestrel'i sunucunun varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.
* İçerik kök tarafından döndürülen yol ayarlar [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).
* Yükleri [ana bilgisayar yapılandırması](#host-configuration-values) gelen:
  * Ortam değişkenlerini önekiyle `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`).
  * Komut satırı bağımsız değişkenleri.
* Uygulama yapılandırması ile aşağıdaki sırada yükler:
  * *appsettings.json*.
  * *appSettings. {Ortamı} .json*.
  * [Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` giriş bütünleştirilmiş kod kullanarak ortamı.
  * Ortam değişkenleri.
  * Komut satırı bağımsız değişkenleri.
* Yapılandırır [günlüğü](xref:fundamentals/logging/index) konsol ve hata ayıklama çıktı. Günlük kaydı içerir [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) günlüğe kaydetme yapılandırma bölümünde belirtilen kuralları bir *appsettings.json* veya *appsettings. { Ortam} .json* dosya.
* Arkasında IIS ile çalıştırırken [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` sağlayan [IIS tümleştirme](xref:host-and-deploy/iis/index), uygulamanın taban adresini ve bağlantı noktasını yapılandırır. IIS tümleştirme, ayrıca uygulamaya yapılandırır [yakalama başlatma hataları](#capture-startup-errors). IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.
* Kümeleri [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise. Daha fazla bilgi için [kapsam doğrulama](#scope-validation).

Tarafından tanımlanan yapılandırma `CreateDefaultBuilder` geçersiz kılındı ve tarafından Genişletilmiş [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), diğer yöntemler ve uzantı yöntemlerinin [ IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Aşağıda birkaç örnek verilmiştir:

* [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) ek belirtmek için kullanılan `IConfiguration` uygulama için. Aşağıdaki `ConfigureAppConfiguration` çağrıyı ekler uygulama yapılandırmasında dahil etmek için bir temsilci *appsettings.xml* dosya. `ConfigureAppConfiguration` birden çok kez çağrılabilir. Bu yapılandırma ana bilgisayara (örneğin, sunucu URL'leri veya ortam) geçerli değil olduğunu unutmayın. Bkz: [ana bilgisayar yapılandırma değerlerini](#host-configuration-values) bölümü.

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* Aşağıdaki `ConfigureLogging` en düşük günlük düzeyi yapılandıracak bir temsilci çağrısı ekler ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) için [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel). Bu ayar ayarları geçersiz kılar *appsettings. Development.JSON* (`LogLevel.Debug`) ve *appsettings. Production.JSON* (`LogLevel.Error`) tarafından yapılandırılan `CreateDefaultBuilder`. `ConfigureLogging` birden çok kez çağrılabilir.

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* Aşağıdaki çağrı `ConfigureKestrel` varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Aşağıdaki çağrı [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) varsayılan geçersiz kılmalar [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) Kestrel tarafından ne zaman yapılandırılan 30.000.000 bayt kurulan `CreateDefaultBuilder`:

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

*İçerik kök* konak MVC görünümü dosyaları gibi içerik dosyaları nerede arar belirler. Projenin kök klasöründen uygulama başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır. Kullanılan varsayılan [Visual Studio](https://visualstudio.microsoft.com) ve [dotnet yeni şablonlar](/dotnet/core/tools/dotnet-new).

Uygulama yapılandırması hakkında daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.

> [!NOTE]
> Statik kullanmaya alternatif olarak `CreateDefaultBuilder` konaktan oluşturma yöntemi, [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) desteklenen bir yaklaşım ile ASP.NET Core 2.x. Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.

Bir ana bilgisayar, ayarlarken [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) ve [Createservicereplicalisteners()](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) yöntemleri sağlanabilir. Varsa bir `Startup` sınıf belirtilmişse, tanımlamanız gerekir bir `Configure` yöntemi. Daha fazla bilgi için bkz. <xref:fundamentals/startup>. Birden çok çağrılar `ConfigureServices` birbirine ekleyin. Birden çok çağrılar `Configure` veya `UseStartup` üzerinde `WebHostBuilder` önceki ayarları değiştirin.

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:

* Ortam değişkenlerini biçiminde içerir konak Oluşturucu Yapılandırması `ASPNETCORE_{configurationKey}`. Örneğin: `ASPNETCORE_ENVIRONMENT`
* Uzantıları gibi [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (bkz [geçersiz kılma yapılandırmasını](#override-configuration) bölümü).
* [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar. Bir değerle ayarlarken `UseSetting`, değer türünden bağımsız olarak bir dize olarak ayarlanır.

Konak, bir değer, en son hangi seçeneği ayarlar kullanır. Daha fazla bilgi için [geçersiz kılma yapılandırmasını](#override-configuration) sonraki bölümde.

### <a name="application-key-name"></a>Uygulama anahtarı (ad)

[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanmış olduğunda [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) konak yapım sırasında çağrılır. Değer, uygulamanın giriş noktasını içeren derleme adına ayarlanır. Değeri açıkça ayarlamak için kullanın [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):

**Anahtar**: applicationName  
**Tür**: *dize*  
**Varsayılan**: Uygulamanın giriş içeren bütünleştirilmiş kodun ad'ı seçin.  
**Kullanılarak ayarlanan**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_APPLICATIONNAME`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a>Başlangıç hatalarını yakalama

Bu ayar, başlangıç hatalarını yakalama denetler.

**Anahtar**: captureStartupErrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: Varsayılan olarak `false` IIS, varsayılan olduğu arkasında Kestrel ile uygulamanın çalıştığı sürece `true`.  
**Kullanılarak ayarlanan**: `CaptureStartupErrors`  
**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`

Zaman `false`, çıkma konak başlatma sonucunda sırasında hatalar. Zaman `true`, konak başlatma sırasında özel durumları yakalar ve sunucu başlatmayı dener.

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a>İçerik kök

Bu ayar, ASP.NET Core MVC görünümleri gibi içerik dosyalarını aramasını başladığı belirler. 

**Anahtar**: contentRoot  
**Tür**: *dize*  
**Varsayılan**: Uygulama derleme bulunduğu klasör varsayılan olur.  
**Kullanılarak ayarlanan**: `UseContentRoot`  
**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`

İçerik kök taban yolu olarak da kullanılır [Web kök ayarı](#web-root). Yol mevcut değilse, konak başlatmak başarısız olur.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a>Ayrıntılı hatalar

Ayrıntılı hatalar yakalanmasının gerekip belirler.

**Anahtar**: detailedErrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
**Kullanılarak ayarlanan**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`

Etkin olduğunda (veya <a href="#environment">ortam</a> ayarlanır `Development`), uygulamanın ayrıntılı özel durumlar yakalar.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a>Ortam

Uygulamanın ortamı ayarlar.

**Anahtar**: ortam  
**Tür**: *dize*  
**Varsayılan**: Üretim  
**Kullanılarak ayarlanan**: `UseEnvironment`  
**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`

Ortamı, herhangi bir değere ayarlanabilir. Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`. Değerler büyük küçük harfe duyarlı değildir. Varsayılan olarak, *ortam* okuma `ASPNETCORE_ENVIRONMENT` ortam değişkeni. Kullanırken [Visual Studio](https://visualstudio.microsoft.com), ortam değişkenleri ayarlanabilir *launchSettings.json* dosya. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a>Başlangıç derlemeleri barındırma

Uygulamanın barındırma başlangıç derlemeleri ayarlar.

**Anahtar**: hostingStartupAssemblies  
**Tür**: *dize*  
**Varsayılan**: Boş dize  
**Kullanılarak ayarlanan**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Başlangıçta yüklenecek başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.

Yapılandırma değeri boş dize olarak varsayılan olarak olsa da, barındırma başlangıç derlemeler her zaman uygulamanın bütünleştirilmiş kod ekleyin. Barındırma başlangıç derlemeler sağlandığında, uygulama başlatma sırasında yaygın hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a>HTTPS bağlantı noktası

HTTPS, yeniden yönlendirme bağlantı noktasını ayarlayın. Kullanılan [HTTPS zorlama](xref:security/enforcing-ssl).

**Anahtar**: https_port **türü**: *dize*
**varsayılan**: Varsayılan bir değer ayarlanmamış.
**Kullanılarak ayarlanan**: `UseSetting`
**Ortam değişkeni**: `ASPNETCORE_HTTPS_PORT`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a>Başlangıç dışlama derlemeleri barındırma

Başlangıçta hariç tutmak için başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.

**Anahtar**: hostingStartupExcludeAssemblies  
**Tür**: *dize*  
**Varsayılan**: Boş dize  
**Kullanılarak ayarlanan**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a>URL'leri barındırma tercih et

Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `WebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.

**Anahtar**: preferHostingUrls  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: true  
**Kullanılarak ayarlanan**: `PreferHostingUrls`  
**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a>Başlangıç barındırma engelle

Başlangıç derlemeler, uygulamanın derleme tarafından yapılandırılan başlangıç derlemeleri barındırma da dahil olmak üzere barındırma otomatik yüklenmesini engeller. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

**Anahtar**: preventHostingStartup  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
**Kullanılarak ayarlanan**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a>Sunucu URL'leri

IP adresi veya konak bağlantı noktalarını ve sunucu üzerinde istekleri dinlemesi gereken protokolleri adresleriyle gösterir.

**Anahtar**: URL'ler  
**Tür**: *dize*  
**Varsayılan**: http://localhost:5000  
**Kullanılarak ayarlanan**: `UseUrls`  
**Ortam değişkeni**: `ASPNETCORE_URLS`

Bir noktalı virgülle ayrılmış ayarlayın (;) listesini URL ön ekleri sunucunun yanıt vermelidir. Örneğin: `http://localhost:123` Kullanım "\*" sunucu herhangi bir IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokol kullanılarak isteklerini dinlemesi gerektiğini belirtmek için (örneğin, `http://*:5000`). Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir. Desteklenen biçimler sunucular arasında farklılık gösterir.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

Kestrel'i kendi API uç nokta yapılandırması vardır. Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.

### <a name="shutdown-timeout"></a>Kapatma zaman aşımı

Web ana bilgisayarı kapatmak beklenecek süreyi belirtir.

**Anahtar**: shutdownTimeoutSeconds  
**Tür**: *int*  
**Varsayılan**: 5  
**Kullanılarak ayarlanan**: `UseShutdownTimeout`  
**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

Anahtarı kabul eder ancak bir *int* ile `UseSetting` (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi alır bir [TimeSpan](/dotnet/api/system.timespan).

Sırasında zaman aşımı süresi, barındırma:

* Tetikleyiciler [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).
* Barındırılan hizmetler, durdurmak için başarısız olan hizmetler için herhangi bir hata günlüğü durdurmaya çalışır.

Tüm barındırılan hizmetlerin Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında tüm kalan etkin hizmet durdurulur. İşlem tamamlandı olmasanız bile hizmetleri durdurun. Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımını artırın.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a>Başlangıç derleme

Aramak için derleme belirler `Startup` sınıfı.

**Anahtar**: startupAssembly  
**Tür**: *dize*  
**Varsayılan**: Uygulamanın derleme  
**Kullanılarak ayarlanan**: `UseStartup`  
**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`

Ada göre bütünleştirilmiş kod (`string`) veya türü (`TStartup`) başvurulabilir. Birden çok `UseStartup` yöntemleri çağrıldığında, sonuncu önceliklidir.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a>Web kökü

Uygulamanın statik varlıklar için göreli yolunu ayarlar.

**Anahtar**: webroot  
**Tür**: *dize*  
**Varsayılan**: Belirtilmezse, varsayılan "(Content Root)/wwwroot olur.", yol varsa. Yol mevcut değilse, ardından İşlemsiz dosya sağlayıcısı kullanılır.  
**Kullanılarak ayarlanan**: `UseWebRoot`  
**Ortam değişkeni**: `ASPNETCORE_WEBROOT`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a>Yapılandırma geçersiz kıl

Kullanım [yapılandırma](xref:fundamentals/configuration/index) Web ana bilgisayarı yapılandırılamadı. Aşağıdaki örnekte, ana bilgisayar yapılandırması isteğe bağlı olarak belirtilen bir *hostsettings.json* dosya. Herhangi bir yapılandırma öğesinden yüklenen *hostsettings.json* dosya tarafından komut satırı bağımsız değişkenleri geçersiz. Yerleşik yapılandırma (içinde `config`) ana bilgisayarı yapılandırmak için kullanılan [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration). `IWebHostBuilder` yapılandırma, uygulamanın yapılandırma için eklenir, ancak listesiyse true değil&mdash; `ConfigureAppConfiguration` etkilemez `IWebHostBuilder` yapılandırma.

Tarafından sağlanan yapılandırma geçersiz kılma `UseUrls` ile *hostsettings.json* config ilk, komut satırı bağımsız değişkeni config ikinci:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            });
    }
}
```

*hostsettings.JSON*:

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan `GetSection` (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`. `GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:urls`, `section:environment`). `UseConfiguration` Yöntemi bekliyor eşleştirilecek anahtarları `WebHostBuilder` anahtarları (örneğin, `urls`, `environment`). Varlığı bölüm adı anahtarlar, ana bilgisayar yapılandırmalarını bölümün değerleri engeller. Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir. Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).
>
> `UseConfiguration` anahtarlar yalnızca sağlanan akış kopyalar `IConfiguration` konak Oluşturucu yapılandırması. Bu nedenle, ayarı `reloadOnChange: true` JSON INI ve XML ayar dosyaları için hiçbir etkisi olmaz.

Ana belirli URL'yi belirtmek için istenen değeri bir komut istemi'nden yürütülürken geçirilebilir [çalıştırma dotnet](/dotnet/core/tools/dotnet-run). Komut satırı bağımsız değişkeni geçersiz kılmalar `urls` değerini *hostsettings.json* dosya ve sunucu, 8080 bağlantı noktasında dinler:

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a>Konağı yönetme

**Çalıştır**

`Run` Yöntemi web uygulaması başlatır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:

```csharp
host.Run();
```

**Start**

Konak çağırarak engelleyici olmayan bir biçimde çalıştırın, `Start` yöntemi:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

URL'lerin bir listesini iletilmezse `Start` yöntemi, belirtilen URL'leri dinler:

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

Uygulamayı başlatın ve önceden yapılandırılmış varsayılan kullanarak yeni bir ana bilgisayarı başlatılamıyor `CreateDefaultBuilder` statik kolaylık yöntemi kullanarak. Bu yöntemler konsol çıktısı olmadan ve sunucuyu başlatın [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT veya SIGTERM) için mola bekleyin:

**Başlangıç (RequestDelegate uygulaması)**

İle başlayan bir `RequestDelegate`:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için `WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller. Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.

**Başlangıç (dize url, RequestDelegate uygulama)**

Bir URL ile başlayın ve `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Aynı sonucu üretir **başlangıç (RequestDelegate uygulama)**, uygulama dışında yanıt üzerinde `http://localhost:8080`.

**Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**

Bir örneğini kullanması `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) yönlendirme ara yazılımı kullanmak için:

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Aşağıdaki tarayıcı isteklerini ile örneği kullanın:

| İstek                                    | Yanıt                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Martin Merhaba!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | "Ooops!" dizesi ile bir özel durum oluşturur |
| `http://localhost:5000/throw`              | Bir özel durum dizesi ile oluşturur "Uh oh!" |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Merhaba Dünya!                             |

`WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller. Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.

**Başlat (eylem url dize&lt;IRouteBuilder&gt; routeBuilder)**

Bir URL örneği kullanıp `IRouteBuilder`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

Aynı sonucu üretir **Başlat (Eylem&lt;IRouteBuilder&gt; routeBuilder)**, uygulama dışında yanıt `http://localhost:8080`.

**StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**

Yapılandıracak bir temsilci sağlayan bir `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

Tarayıcıda istekte `http://localhost:5000` "Hello World!" yanıtını almak için `WaitForShutdown` (Ctrl-C/SIGINT veya SIGTERM) bir kesme verilene kadar engeller. Uygulama görüntüler `Console.WriteLine` iletisi ve bir tuş basışı çıkmak bekler.

**StartWith (url, eylem dizesi&lt;IApplicationBuilder&gt; uygulama)**

Bir URL ve yapılandıracak bir temsilci sağlar bir `IApplicationBuilder`:

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

Aynı sonucu üretir **StartWith (Eylem&lt;IApplicationBuilder&gt; uygulama)**, uygulama dışında yanıt üzerinde `http://localhost:8080`.

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment arabirimi

[IHostingEnvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) uygulamanın web barındırma ortamı hakkında bilgi sağlar. Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

A [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) ortama bağlı başlangıçta uygulamayı yapılandırmak için kullanılabilir. Alternatif olarak, ekleme `IHostingEnvironment` içine `Startup` Oluşturucusu kullanılmak üzere `ConfigureServices`:

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> Ek olarak `IsDevelopment` genişletme yöntemi `IHostingEnvironment` sunar `IsStaging`, `IsProduction`, ve `IsEnvironment(string environmentName)` yöntemleri. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

`IHostingEnvironment` Hizmet de eklenen doğrudan `Configure` işleme işlem hattı ayarlamaya yönelik yöntemi:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the Developer Exception Page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

`IHostingEnvironment` içine yerleştirilebilir `Invoke` özel oluştururken yöntemi [ara yazılım](xref:fundamentals/middleware/write):

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime arabirimi

[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) sonrası başlatma ve kapatma etkinlikler için sağlar. Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.

| İptal belirteci    | Ne zaman tetiklenir&#8230; |
| --------------------- | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Ana bilgisayarın tam olarak başlatılmış. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Konak bir şekilde kapatılmasını tamamlıyor. Tüm isteklerin işlenmesi. Bu olay tamamlanıncaya kadar kapatma engeller. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Konak bir şekilde kapatılmasını gerçekleştiriyor. İstekler hala işleniyor. Bu olay tamamlanıncaya kadar kapatma engeller. |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) sonlandırma uygulamanın ister. Aşağıdaki sınıf kullandığı `StopApplication` düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

## <a name="scope-validation"></a>Kapsam doğrulama

[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ayarlar [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) için `true` uygulamanın ortamı geliştirme ise.

Zaman `ValidateScopes` ayarlanır `true`, varsayılan hizmet sağlayıcısı, doğrulamak üzere denetler:

* Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözülmüş değildir.
* Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değildir.

Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrılır. Kök hizmet sağlayıcısının bir ömür zaman sağlayıcısı uygulamayla başlar ve uygulama kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.

Kapsamlı Hizmetleri, onları oluşturan kapsayıcı tarafından elden çıkarılmasını. Kapsamlı bir hizmet içinde kök kapsayıcı oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından bırakılmadan olduğundan hizmet ömrü tekliye etkili bir şekilde yükseltilir. Hizmet kapsamları doğrulama yakalar bu gibi durumlarda, `BuildServiceProvider` çağrılır.

Kapsamlar üretim ortamında dahil olmak üzere, her zaman doğrulamak için yapılandırma [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) ile [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) konak oluşturucu üzerinde:

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
