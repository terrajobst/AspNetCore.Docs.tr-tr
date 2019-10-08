---
title: ASP.NET Core Web ana bilgisayarı
author: rick-anderson
description: Uygulama başlatma ve ömür yönetiminden sorumlu olan ASP.NET Core Web ana bilgisayarı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: bc18b5490d232758b796d33a62cd8d1a7dd7289f
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007101"
---
# <a name="aspnet-core-web-host"></a>ASP.NET Core Web ana bilgisayarı

ASP.NET Core uygulamalar bir *Konağı*yapılandırıp başlatır. Uygulama başlatma ve ömür yönetimi için konak sorumludur. Ana bilgisayar, en azından bir sunucu ve bir istek işleme işlem hattı yapılandırır. Konak, günlüğe kaydetme, bağımlılık ekleme ve yapılandırma de ayarlayabilir.

::: moniker range=">= aspnetcore-3.0"

Bu makalede, yalnızca geriye dönük uyumluluk için kullanılabilen Web ana bilgisayarı ele alınmaktadır. [Genel ana bilgisayar](xref:fundamentals/host/generic-host) tüm uygulama türleri için önerilir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Bu makale, Web uygulamalarını barındırmak için olan Web konağını ele alır. Diğer uygulama türleri için [genel Konağı](xref:fundamentals/host/generic-host)kullanın.

::: moniker-end

## <a name="set-up-a-host"></a>Konak ayarlama

[Iwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın bir örneğini kullanarak bir konak oluşturun. Bu, genellikle uygulamanın giriş noktasında, `Main` yönteminde gerçekleştirilir.

Proje şablonlarında, `Main` *program.cs*içinde bulunur. Tipik bir uygulama, bir konak ayarlamaya başlamak için [Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) çağırır:

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

@No__t-0 ' ı çağıran kod, Oluşturucu nesnesinde `Run` ' ü çağıran `Main` ' deki koddan ayıran `CreateWebHostBuilder` adlı bir yöntemde bulunur. [Entity Framework Core araçlarını](/ef/core/miscellaneous/cli/)kullanıyorsanız bu ayrım gereklidir. Araçlar, uygulamayı çalıştırmadan ana bilgisayarı yapılandırmak için tasarım zamanında çağrlayabilecekleri bir `CreateWebHostBuilder` yöntemi bulmayı bekler. Diğer bir seçenek de `IDesignTimeDbContextFactory` ' a uygulanır. Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).

`CreateDefaultBuilder` aşağıdaki görevleri gerçekleştirir:

* [Kestrel](xref:fundamentals/servers/kestrel) sunucusunu, uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak Web sunucusu olarak yapılandırır. Kestrel sunucusunun varsayılan seçenekleri için bkz. <xref:fundamentals/servers/kestrel#kestrel-options>.
* [İçerik kökünü](xref:fundamentals/index#content-root) [Directory. GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)tarafından döndürülen yola ayarlar.
* [Ana bilgisayar yapılandırmasını](#host-configuration-values) şuradan yükler:
  * @No__t-0 (örneğin, `ASPNETCORE_ENVIRONMENT`) önekli ortam değişkenleri.
  * Komut satırı bağımsız değişkenleri.
* Aşağıdaki sırayla uygulama yapılandırmasını yükler:
  * *appSettings. JSON*.
  * *appSettings. {Environment}. JSON*.
  * Uygulama, giriş derlemesini kullanarak `Development` ortamında çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .
  * Ortam değişkenleri.
  * Komut satırı bağımsız değişkenleri.
* Konsol ve hata ayıklama çıkışı için [günlüğe kaydetmeyi](xref:fundamentals/logging/index) yapılandırır. Günlüğe kaydetme, bir *appSettings. JSON* veya appSettings 'in günlük yapılandırma bölümünde belirtilen [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) kurallarını içerir *. { Environment}. JSON* dosyası.
* [ASP.NET Core modülle](xref:host-and-deploy/aspnet-core-module)IIS 'nin arkasında çalışırken, `CreateDefaultBuilder`, uygulamanın temel adresini ve bağlantı noktasını yapılandıran [IIS tümleştirmesine](xref:host-and-deploy/iis/index)izin vermez. IIS tümleştirmesi, uygulamayı [başlatma hatalarını yakalamaya](#capture-startup-errors)de yapılandırır. IIS varsayılan seçenekleri için bkz. <xref:host-and-deploy/iis/index#iis-options>.
* Uygulamanın ortamı geliştirmelerde, [Serviceprovideroptions. ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) `true` olarak ayarlar. Daha fazla bilgi için bkz. [kapsam doğrulaması](#scope-validation).

@No__t-0 tarafından tanımlanan yapılandırma, [Configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [configurelogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)ve [ıwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın diğer yöntemleri ve genişletme yöntemleri tarafından geçersiz kılınabilir ve genişletilebilir. Birkaç örnek aşağıda verilmiştir:

* [Configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) , uygulama için ek `IConfiguration` belirtmek için kullanılır. Aşağıdaki `ConfigureAppConfiguration` çağrısı, *appSettings. xml* dosyasına uygulama yapılandırmasını dahil etmek için bir temsilci ekler. `ConfigureAppConfiguration` birden çok kez çağrılabilir. Bu yapılandırmanın ana bilgisayar için (örneğin, sunucu URL 'Leri veya ortam) uygulanmadığını unutmayın. [Konak yapılandırma değerleri](#host-configuration-values) bölümüne bakın.

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* Aşağıdaki `ConfigureLogging` çağrısı, minimum günlük düzeyi ([Setminimumlevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) değerini [LogLevel. Warning](/dotnet/api/microsoft.extensions.logging.loglevel)olarak yapılandırmak için bir temsilci ekler. Bu ayar appSettings 'teki ayarları geçersiz kılar *. Development. JSON* (`LogLevel.Debug`) ve *appSettings. Production. JSON* (`LogLevel.Error`) `CreateDefaultBuilder` tarafından yapılandırıldı. `ConfigureLogging` birden çok kez çağrılabilir.

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* @No__t-0 ' a yönelik aşağıdaki çağrı varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30.000.000 Byte, Kestrel `CreateDefaultBuilder` tarafından yapılandırıldığında belirlenir:

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Aşağıdaki [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) çağrısı, varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) , Kestrel `CreateDefaultBuilder` tarafından yapılandırıldığında oluşturulan 30.000.000 bayttan oluşur:

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

[İçerik kökü](xref:fundamentals/index#content-root) , konağın MVC görünüm dosyaları gibi içerik dosyalarını arayacağı yeri belirler. Uygulama, projenin kök klasöründen başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır. Bu, [Visual Studio](https://visualstudio.microsoft.com) 'da ve [DotNet yeni şablonlarda](/dotnet/core/tools/dotnet-new)kullanılan varsayılandır.

Uygulama yapılandırması hakkında daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.

> [!NOTE]
> Statik `CreateDefaultBuilder` yönteminin kullanılmasına alternatif olarak, [Webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 'dan bir konak oluşturmak, ASP.NET Core 2. x ile desteklenen bir yaklaşımdır.

Bir ana bilgisayar ayarlanırken, [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) yöntemleri bulunabilir. @No__t-0 sınıfı belirtilmişse, bir `Configure` yöntemi tanımlamalıdır. Daha fazla bilgi için bkz. <xref:fundamentals/startup>. @No__t-0 ' a birden çok çağrı bir diğerine eklenir. @No__t-2 ' de `Configure` veya `UseStartup` ' e birden çok çağrı önceki ayarları değiştirir.

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[Webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) , ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımları kullanır:

* @No__t-0 biçimindeki ortam değişkenlerini içeren konak Oluşturucu yapılandırması. Örneğin, `ASPNETCORE_ENVIRONMENT`.
* [Usecontentroot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) gibi uzantılar ( [geçersiz kılma yapılandırması](#override-configuration) bölümüne bakın).
* [Usesetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar. @No__t-0 ile bir değer ayarlarken, değer türünden bağımsız olarak bir dize olarak ayarlanır.

Konak, bir değeri en son ayarlayan seçeneği kullanır. Daha fazla bilgi için, sonraki bölümde [yapılandırmayı geçersiz kılma](#override-configuration) bölümüne bakın.

### <a name="application-key-name"></a>Uygulama anahtarı (ad)

::: moniker range=">= aspnetcore-3.0"

@No__t-0 özelliği, konak oluşturma sırasında [Usestartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) çağrıldığında otomatik olarak ayarlanır. Değer, uygulamanın giriş noktasını içeren derlemenin adına ayarlanır. Değeri açıkça ayarlamak için [Webhostdefaults. ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)kullanın:

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Konak oluşturma sırasında [Usestartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) çağrıldığında, [ıhostingenvironment. ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanır. Değer, uygulamanın giriş noktasını içeren derlemenin adına ayarlanır. Değeri açıkça ayarlamak için [Webhostdefaults. ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)kullanın:

::: moniker-end

**Anahtar**: ApplicationName  
**Tür**: *dize*  
**Varsayılan**: Uygulamanın giriş noktasını içeren derlemenin adı.  
Şunu **kullanarak ayarla**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_APPLICATIONNAME`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a>Yakalama başlatma hataları

Bu ayar, başlatma hatalarının yakalanmasını denetler.

**Anahtar**: capturestartuperrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: Uygulama IIS arkasındaki Kestrel ile çalıştırılmadığı müddetçe `false` ' dır ve varsayılan olarak `true` ' dir.  
Şunu **kullanarak ayarla**: `CaptureStartupErrors`  
**Ortam değişkeni**: `ASPNETCORE_CAPTURESTARTUPERRORS`

@No__t-0 olduğunda, başlangıçtaki ana bilgisayardaki hatalar çıkıyor. @No__t-0 olduğunda, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a>İçerik kökü

Bu ayar ASP.NET Core içerik dosyalarını aramaya başladığı yeri belirler.

**Anahtar**: contentroot  
**Tür**: *dize*  
**Varsayılan**: Uygulama derlemesinin bulunduğu klasörü varsayılan olarak belirler.  
Şunu **kullanarak ayarla**: `UseContentRoot`  
**Ortam değişkeni**: `ASPNETCORE_CONTENTROOT`

İçerik kökü, [Web kökünün](xref:fundamentals/index#web-root)temel yolu olarak da kullanılır. İçerik kök yolu yoksa, ana bilgisayar başlatılamaz.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

Daha fazla bilgi için bkz.

* [Temelleri: İçerik kökü @ no__t-0
* [Web kökü](#web-root)

### <a name="detailed-errors"></a>Ayrıntılı hatalar

Ayrıntılı hataların yakalanıp yakalanmayacağını belirler.

**Anahtar**: detailederrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
Şunu **kullanarak ayarla**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_DETAILEDERRORS`

Etkinleştirildiğinde (veya <a href="#environment">ortam</a> `Development` olarak ayarlandığında), uygulama ayrıntılı özel durumları yakalar.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a>Ortam

Uygulamanın ortamını ayarlar.

**Anahtar**: ortam  
**Tür**: *dize*  
**Varsayılan**: Üretiminden  
Şunu **kullanarak ayarla**: `UseEnvironment`  
**Ortam değişkeni**: `ASPNETCORE_ENVIRONMENT`

Ortam herhangi bir değere ayarlanabilir. Çerçeve tanımlı değerler `Development`, `Staging` ve `Production` içerir. Değerler büyük/küçük harfe duyarlı değildir. Varsayılan olarak, *ortam* `ASPNETCORE_ENVIRONMENT` ortam değişkeninden okunurdur. [Visual Studio](https://visualstudio.microsoft.com)kullanılırken, *launchsettings. JSON* dosyasında ortam değişkenleri ayarlanabilir. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a>Başlangıç derlemelerini barındırma

Uygulamanın barındırma başlangıç derlemelerini ayarlar.

**Anahtar**: hostingStartupAssemblies  
**Tür**: *dize*  
**Varsayılan**: Boş dize  
Şunu **kullanarak ayarla**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

Başlangıçta yüklenecek başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.

Yapılandırma değeri boş bir dize olarak varsayılan olsa da, barındırma başlangıç derlemeleri her zaman uygulamanın derlemesini içerir. Barındırma başlangıç derlemeleri sağlandığında, uygulama başlangıç sırasında ortak hizmetlerini oluşturduğunda yükleme için uygulamanın derlemesine eklenir.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a>HTTPS bağlantı noktası

HTTPS yeniden yönlendirme bağlantı noktasını ayarlayın. [Https zorlama](xref:security/enforcing-ssl)bölümünde kullanılır.

**Anahtar**: https_port **Type**: *dize*
**varsayılan**: Varsayılan değer ayarlı değildir.
Şunu **kullanarak ayarla**: `UseSetting` @ no__t-1**ortam değişkeni**: `ASPNETCORE_HTTPS_PORT`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a>Barındırma başlatma derlemeleri dışlama

Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.

**Anahtar**: hostingstartupexcludeassemblies  
**Tür**: *dize*  
**Varsayılan**: Boş dize  
Şunu **kullanarak ayarla**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a>Barındırma URL 'Lerini tercih et

Konağın `IServer` uygulamasıyla yapılandırılanlar yerine `WebHostBuilder` ile yapılandırılan URL 'Leri dinlemesi gerekip gerekmediğini gösterir.

**Anahtar**: preferhostingurl 'leri  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: true  
Şunu **kullanarak ayarla**: `PreferHostingUrls`  
**Ortam değişkeni**: `ASPNETCORE_PREFERHOSTINGURLS`

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a>Barındırma başlangıcını engelle

Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

**Anahtar**: koruyucu thostingstartup  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
Şunu **kullanarak ayarla**: `UseSetting`  
**Ortam değişkeni**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a>Sunucu URL 'Leri

Sunucunun istekler için dinlemesi gereken bağlantı noktaları ve protokoller içeren IP adreslerini veya ana bilgisayar adreslerini gösterir.

**Anahtar**: URL 'ler  
**Tür**: *dize*  
**Varsayılan**: http://localhost:5000  
Şunu **kullanarak ayarla**: `UseUrls`  
**Ortam değişkeni**: `ASPNETCORE_URLS`

Noktalı virgülle ayrılmış olarak ayarlayın (;) sunucunun yanıtlaması gereken URL ön eklerinin listesi. Örneğin, `http://localhost:123`. Sunucunun belirtilen bağlantı noktasını ve protokolü kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "\*" kullanın (örneğin, `http://*:5000`). Protokol (`http://` veya `https://`) her URL 'ye dahil olmalıdır. Desteklenen biçimler sunucular arasında farklılık gösterir.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

Kestrel kendi uç nokta yapılandırması API 'sine sahiptir. Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.

### <a name="shutdown-timeout"></a>Kapatılma zaman aşımı

Web konağının kapanması için beklenecek süreyi belirtir.

**Anahtar**: shutdowntimeoutseconds  
**Tür**: *int*  
**Varsayılan**: 5  
Şunu **kullanarak ayarla**: `UseShutdownTimeout`  
**Ortam değişkeni**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

Anahtar, `UseSetting` ile bir *int* kabul etse de (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [useshutdowntimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi bir [TimeSpan](/dotnet/api/system.timespan)alır.

Zaman aşımı süresi boyunca barındırma:

* [Iapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)öğesini tetikler.
* Üzerinde durmayacak hizmetlerin hatalarını günlüğe kaydetmek için barındırılan hizmetleri durdurmaya çalışır.

Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur. Hizmetler, işlemeyi tamamlamadıklarında bile durur. Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a>Başlangıç derlemesi

@No__t-0 sınıfı için arama yapılacak derlemeyi belirler.

**Anahtar**: startupassembly  
**Tür**: *dize*  
**Varsayılan**: Uygulamanın derlemesi  
Şunu **kullanarak ayarla**: `UseStartup`  
**Ortam değişkeni**: `ASPNETCORE_STARTUPASSEMBLY`

Ada (`string`) veya türe (`TStartup`) göre derlemeye başvurulabilir. Birden çok `UseStartup` yöntemi çağrılırsa, son bir öncelik alır.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a>Web kökü

Uygulamanın statik varlıklarının göreli yolunu ayarlar.

**Anahtar**: Webroot  
**Tür**: *dize*  
**Varsayılan**: Varsayılan, `wwwroot` değeridir. *{Content root}/Wwwroot* yolu var olmalıdır. Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.  
Şunu **kullanarak ayarla**: `UseWebRoot`  
**Ortam değişkeni**: `ASPNETCORE_WEBROOT`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

Daha fazla bilgi için bkz.

* [Temelleri: Web kök @ no__t-0
* [İçerik kökü](#content-root)

## <a name="override-configuration"></a>Geçersiz kılma yapılandırması

Web konağını yapılandırmak için [yapılandırma](xref:fundamentals/configuration/index) kullanın. Aşağıdaki örnekte, konak yapılandırması isteğe bağlı olarak bir *HostSettings. JSON* dosyasında belirtilir. *HostSettings. JSON* dosyasından yüklenen herhangi bir yapılandırma komut satırı bağımsız değişkenleri tarafından geçersiz kılınabilir. Oluşturulan yapılandırma (`config`), Konağı [Useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)ile yapılandırmak için kullanılır. `IWebHostBuilder` yapılandırması uygulamanın yapılandırmasına eklenir, ancak bu değer doğru değil @ no__t-1 @ no__t-2 `IWebHostBuilder` yapılandırmasını etkilemez.

@No__t-0 tarafından belirtilen yapılandırmayı *HostSettings* ile geçersiz kılma. önce JSON config, komut satırı bağımsız değişkeni yapılandırma saniyesi:

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

*HostSettings. JSON*:

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> [Useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) uzantı yöntemi şu anda `GetSection` tarafından döndürülen bir yapılandırma bölümünü ayrıştırma özelliğine sahip değil (örneğin, `.UseConfiguration(Configuration.GetSection("section"))`. @No__t-0 yöntemi, yapılandırma anahtarlarını istenen bölüme süzer ancak bölüm adını anahtarlar üzerinde bırakır (örneğin, `section:urls`, `section:environment`). @No__t-0 yöntemi, anahtarların `WebHostBuilder` anahtarlarıyla eşleşmesini bekler (örneğin, `urls`, `environment`). Anahtarlarda bölüm adının varlığı, bölümün değerlerinin konak yapılandırmasını engeller. Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir. Daha fazla bilgi ve geçici çözüm için bkz [. yapılandırma bölümünü WebHostBuilder 'A geçirme. UseConfiguration tam anahtarları kullanır](https://github.com/aspnet/Hosting/issues/839).
>
> `UseConfiguration` yalnızca belirtilen `IConfiguration` ' den konak Oluşturucu yapılandırmasına anahtar kopyalar. Bu nedenle, JSON, INI ve XML ayarları dosyaları için `reloadOnChange: true` ayarlarının hiçbir etkisi yoktur.

Belirli bir URL 'de çalıştırılacak Konağı belirtmek için, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)yürütürken istenen değer bir komut isteminden geçirilebilir. Komut satırı bağımsız değişkeni *HostSettings. JSON* dosyasından `urls` değerini geçersiz kılar ve sunucu 8080 numaralı bağlantı noktasını dinler:

```dotnetcli
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a>Konağı yönetme

**Çalıştırma**

@No__t-0 yöntemi, Web uygulamasını başlatır ve konak kapanana kadar çağıran iş parçacığını engeller:

```csharp
host.Run();
```

**Start**

@No__t-0 yöntemini çağırarak Konağı engellenmeyen bir şekilde çalıştırın:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

@No__t-0 yöntemine bir URL listesi geçirilirse, belirtilen URL 'lerde dinler:

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

Uygulama, statik bir kolaylık yöntemi kullanarak `CreateDefaultBuilder` ' ın önceden yapılandırılmış varsayılanlarını kullanarak yeni bir ana bilgisayar başlatabilir ve başlatabilir. Bu yöntemler, konsol çıktısı olmadan sunucuyu başlatır ve [Waitforkapatmadan](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) bir kesme (CTRL-C/sigint veya sigterim) bekler:

**Başlat (RequestDelegate uygulaması)**

@No__t ile başlayın-0:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

"Merhaba Dünya!" yanıtını almak için tarayıcıda `http://localhost:5000` ' a bir istek yapın bir kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar `WaitForShutdown` blok. Uygulama `Console.WriteLine` iletisini görüntüler ve bir KeyPress 'den çıkmak için bekler.

**Başlangıç (dize URL 'si, RequestDelegate uygulaması)**

URL ile başlayın ve `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Uygulamanın `http://localhost:8080` ' de yanıt vermesi dışında, **Başlangıç (RequestDelegate uygulaması)** ile aynı sonucu üretir.

**Başlat (eylem @ no__t-1IRouteBuilder > routeBuilder)**

Yönlendirme ara yazılımını kullanmak için `IRouteBuilder` ([Microsoft. AspNetCore. Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) örneğini kullanın:

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

Aşağıdaki tarayıcı isteklerini örnekle birlikte kullanın:

| İstek                                    | Yanıt                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | Merhaba, martın!                           |
| `http://localhost:5000/buenosdias/Catrina` | Buenos dias, Catrina!                    |
| `http://localhost:5000/throw/ooops!`       | "Ooops!" dizesiyle bir özel durum oluşturur |
| `http://localhost:5000/throw`              | "Uh oh!" dizesiyle bir özel durum oluşturur |
| `http://localhost:5000/Sante/Kevin`        | Sante, Kevin!                            |
| `http://localhost:5000`                    | Merhaba Dünya!                             |

bir kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar `WaitForShutdown` blok. Uygulama `Console.WriteLine` iletisini görüntüler ve bir KeyPress 'den çıkmak için bekler.

**Başlat (dize URL 'si, eylem @ no__t-1ıroutebuilder > routeBuilder)**

Bir URL ve @no__t örneği kullanın-0:

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

Uygulamanın `http://localhost:8080` ' de yanıt vermesi dışında, **Başlangıç (eylem @ no__t-1IRouteBuilder > routebuilder)** ile aynı sonucu üretir.

**StartWith (Action @ no__t-1IApplicationBuilder > App)**

@No__t yapılandırmak için bir temsilci sağlayın-0:

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

"Merhaba Dünya!" yanıtını almak için tarayıcıda `http://localhost:5000` ' a bir istek yapın bir kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar `WaitForShutdown` blok. Uygulama `Console.WriteLine` iletisini görüntüler ve bir KeyPress 'den çıkmak için bekler.

**StartWith (dize URL 'si, Action @ no__t-1IApplicationBuilder > App)**

@No__t yapılandırmak için bir URL ve temsilci sağlayın-0:

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

Uygulamanın `http://localhost:8080` ' de yanıt vermesi dışında, **StartWith ile aynı sonucu üretir (Action @ no__t-1IApplicationBuilder > App)** .

::: moniker range=">= aspnetcore-3.0"

## <a name="iwebhostenvironment-interface"></a>Iwebhostenvironment arabirimi

@No__t-0 arabirimi, uygulamanın Web barındırma ortamı hakkında bilgi sağlar. Özelliklerini ve uzantı yöntemlerini kullanmak için `IWebHostEnvironment` ' i almak için [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) kullanın:

```csharp
public class CustomFileReader
{
    private readonly IWebHostEnvironment _env;

    public CustomFileReader(IWebHostEnvironment env)
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

Uygulamayı ortama göre başlangıçta yapılandırmak için [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) kullanılabilir. Alternatif olarak, `IWebHostEnvironment` ' ı `ConfigureServices` ' de kullanılmak üzere `Startup` oluşturucusuna ekleyin:

```csharp
public class Startup
{
    public Startup(IWebHostEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IWebHostEnvironment HostingEnvironment { get; }

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
> @No__t-0 uzantısı yöntemine ek olarak, `IWebHostEnvironment` `IsStaging`, `IsProduction` ve `IsEnvironment(string environmentName)` yöntemleri sunar. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

@No__t-0 hizmeti ayrıca işlem ardışık düzenini ayarlamak için doğrudan `Configure` yöntemine eklenebilir:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
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

`IWebHostEnvironment`, özel [Ara yazılım](xref:fundamentals/middleware/write)oluştururken `Invoke` yöntemine eklenebilir:

```csharp
public async Task Invoke(HttpContext context, IWebHostEnvironment env)
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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="ihostingenvironment-interface"></a>Ihostingenvironment arabirimi

[Ihostingenvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) , uygulamanın Web barındırma ortamı hakkında bilgi sağlar. Özelliklerini ve uzantı yöntemlerini kullanmak için `IHostingEnvironment` ' i almak için [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) kullanın:

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

Uygulamayı ortama göre başlangıçta yapılandırmak için [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) kullanılabilir. Alternatif olarak, `IHostingEnvironment` ' ı `ConfigureServices` ' de kullanılmak üzere `Startup` oluşturucusuna ekleyin:

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
> @No__t-0 uzantısı yöntemine ek olarak, `IHostingEnvironment` `IsStaging`, `IsProduction` ve `IsEnvironment(string environmentName)` yöntemleri sunar. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

@No__t-0 hizmeti ayrıca işlem ardışık düzenini ayarlamak için doğrudan `Configure` yöntemine eklenebilir:

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

`IHostingEnvironment`, özel [Ara yazılım](xref:fundamentals/middleware/write)oluştururken `Invoke` yöntemine eklenebilir:

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

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="ihostapplicationlifetime-interface"></a>Ihostapplicationlifetime arabirimi

`IHostApplicationLifetime`, başlatma sonrası ve kapalı etkinliklere izin verir. Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan `Action` yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.

| İptal belirteci    | Tetiklendiği zaman&#8230; |
| --------------------- | --------------------- |
| `ApplicationStarted`  | Konak tam olarak başlatıldı. |
| `ApplicationStopped`  | Ana bilgisayar düzgün kapanma işlemini tamamlıyor. Tüm isteklerin işlenmesi gerekir. Bu olay tamamlanana kadar kapalı bloklar. |
| `ApplicationStopping` | Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor. İstekler hala işliyor olabilir. Bu olay tamamlanana kadar kapalı bloklar. |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IHostApplicationLifetime appLifetime)
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

`StopApplication`, uygulamanın sonlandırılmasını ister. Aşağıdaki sınıf, sınıfın `Shutdown` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için `StopApplication` kullanır:

```csharp
public class MyClass
{
    private readonly IHostApplicationLifetime _appLifetime;

    public MyClass(IHostApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="iapplicationlifetime-interface"></a>Iapplicationlifetime arabirimi

[Iapplicationlifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) , başlatma sonrası ve kapalı etkinlikler için izin verir. Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan `Action` yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.

| İptal belirteci    | Tetiklendiği zaman&#8230; |
| --------------------- | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Konak tam olarak başlatıldı. |
| [Applicationdurdurulan](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Ana bilgisayar düzgün kapanma işlemini tamamlıyor. Tüm isteklerin işlenmesi gerekir. Bu olay tamamlanana kadar kapalı bloklar. |
| [Applicationdurduruluyor](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor. İstekler hala işliyor olabilir. Bu olay tamamlanana kadar kapalı bloklar. |

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

[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) , uygulamanın sonlandırılmasını ister. Aşağıdaki sınıf, sınıfın `Shutdown` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için `StopApplication` kullanır:

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

::: moniker-end

## <a name="scope-validation"></a>Kapsam doğrulaması

Uygulamanın ortamı geliştirmede [Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) , [Serviceprovideroptions. validatescopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) öğesini `true` olarak ayarlar.

@No__t-0 `true` olarak ayarlandığında, varsayılan hizmet sağlayıcısı şunları doğrulamak için denetimler gerçekleştirir:

* Kapsamlı hizmetler doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözümlenmez.
* Kapsamlı hizmetler doğrudan veya dolaylı olarak Singleton 'a eklenmiş değildir.

[Buildserviceprovider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrıldığında kök hizmet sağlayıcısı oluşturulur. Kök hizmet sağlayıcısının ömrü, sağlayıcının uygulamayla başladığı ve uygulama kapandığında bırakıldığı uygulama/sunucunun yaşam süresine karşılık gelir.

Kapsamlı hizmetler kendilerini oluşturan kapsayıcı tarafından atılmış. Kök kapsayıcıda kapsamlı bir hizmet oluşturulduysa, hizmetin ömrü etkin şekilde tek başına yükseltilir çünkü yalnızca uygulama/sunucu kapatıldığında kök kapsayıcı tarafından atılmış olur. @No__t-0 çağrıldığında, hizmet kapsamlarını doğrulamak bu durumları yakalar.

Üretim ortamında da dahil olmak üzere kapsamları her zaman doğrulamak için, ana bilgisayar Oluşturucu 'da [Serviceprovideroptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) 'ı [usedefaultserviceprovider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) ile yapılandırın:

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
