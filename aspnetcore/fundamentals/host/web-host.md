---
title: ASP.NET Core Web ana bilgisayarı
author: guardrex
description: Uygulama başlatma ve ömür yönetiminden sorumlu olan ASP.NET Core Web ana bilgisayarı hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: d387098662cc832cc0e49b6a1636f0ebcc7308de
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081685"
---
# <a name="aspnet-core-web-host"></a>ASP.NET Core Web ana bilgisayarı

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core uygulamalar bir *Konağı*yapılandırıp başlatır. Uygulama başlatma ve ömür yönetimi için konak sorumludur. Ana bilgisayar, en azından bir sunucu ve bir istek işleme işlem hattı yapılandırır. Konak, günlüğe kaydetme, bağımlılık ekleme ve yapılandırma de ayarlayabilir.

::: moniker range=">= aspnetcore-3.0"

Bu makalede, yalnızca geriye dönük uyumluluk için kullanılabilen Web ana bilgisayarı ele alınmaktadır. [Genel ana bilgisayar](xref:fundamentals/host/generic-host) tüm uygulama türleri için önerilir.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Bu makale, Web uygulamalarını barındırmak için olan Web konağını ele alır. Diğer uygulama türleri için [genel Konağı](xref:fundamentals/host/generic-host)kullanın.

::: moniker-end

## <a name="set-up-a-host"></a>Konak ayarlama

[Iwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın bir örneğini kullanarak bir konak oluşturun. Bu, genellikle uygulamanın giriş noktasında ve `Main` yöntemi olarak gerçekleştirilir.

Proje şablonlarında, `Main` *program.cs*konumunda bulunur. Tipik bir uygulama, bir konak ayarlamaya başlamak için [Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) çağırır:

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

Çağıran `CreateDefaultBuilder` kod, Oluşturucu nesnesindeki çağrılarındaki `Main` `Run` koddan ayıran `CreateWebHostBuilder`adlı bir yöntemde bulunur. [Entity Framework Core araçlarını](/ef/core/miscellaneous/cli/)kullanıyorsanız bu ayrım gereklidir. Araçlar, uygulamayı çalıştırmadan Konağı yapılandırmak `CreateWebHostBuilder` için tasarım zamanında çağırabilecekleri bir yöntemi bulmayı bekler. Diğer bir seçenek de uygulanır `IDesignTimeDbContextFactory`. Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).

`CreateDefaultBuilder`Aşağıdaki görevleri gerçekleştirir:

* [Kestrel](xref:fundamentals/servers/kestrel) sunucusunu, uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak Web sunucusu olarak yapılandırır. Kestrel sunucusunun varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.
* İçerik kökünü [Directory. GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory)tarafından döndürülen yola ayarlar.
* [Ana bilgisayar yapılandırmasını](#host-configuration-values) şuradan yükler:
  * Ön eki olan `ASPNETCORE_` ortam değişkenleri (örneğin, `ASPNETCORE_ENVIRONMENT`).
  * Komut satırı bağımsız değişkenleri.
* Aşağıdaki sırayla uygulama yapılandırmasını yükler:
  * *appSettings. JSON*.
  * *appSettings. {Environment}. JSON*.
  * Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .
  * Ortam değişkenleri.
  * Komut satırı bağımsız değişkenleri.
* Konsol ve hata ayıklama çıkışı için [günlüğe kaydetmeyi](xref:fundamentals/logging/index) yapılandırır. Günlüğe kaydetme, bir *appSettings. JSON* veya appSettings 'in günlük yapılandırma bölümünde belirtilen [günlük filtreleme](xref:fundamentals/logging/index#log-filtering) kurallarını içerir *. { Environment}. JSON* dosyası.
* [ASP.NET Core modülle](xref:host-and-deploy/aspnet-core-module)IIS 'nin arkasında çalışırken, `CreateDefaultBuilder` uygulamanın temel adresini ve bağlantı noktasını yapılandıran [IIS tümleştirmesini](xref:host-and-deploy/iis/index)da sunar. IIS tümleştirmesi, uygulamayı [başlatma hatalarını yakalamaya](#capture-startup-errors)de yapılandırır. IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.
* Uygulamanın ortamı geliştirme ortamındaysanız, [serviceprovideroptions. validatescopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) öğesini olarak `true` ayarlar. Daha fazla bilgi için bkz. [kapsam doğrulaması](#scope-validation).

Tarafından `CreateDefaultBuilder` tanımlanan yapılandırma, [configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [configurelogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging)ve [ıwebhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)'ın diğer yöntemleri ve genişletme yöntemleri tarafından geçersiz kılınabilir ve genişletilebilir. Birkaç örnek aşağıda verilmiştir:

* [Configureappconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) , uygulama için ek `IConfiguration` belirtmek için kullanılır. Aşağıdaki `ConfigureAppConfiguration` çağrı, *appSettings. xml* dosyasına uygulama yapılandırmasını dahil etmek için bir temsilci ekler. `ConfigureAppConfiguration`birden çok kez çağrılabilir. Bu yapılandırmanın ana bilgisayar için (örneğin, sunucu URL 'Leri veya ortam) uygulanmadığını unutmayın. [Konak yapılandırma değerleri](#host-configuration-values) bölümüne bakın.

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* Aşağıdaki `ConfigureLogging` çağrı, en düşük günlük düzeyi ([setminimumlevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) değerini [LogLevel. Warning](/dotnet/api/microsoft.extensions.logging.loglevel)olarak yapılandırmak için bir temsilci ekler. Bu ayar appSettings 'teki ayarları geçersiz kılar *. Development. JSON* (`LogLevel.Debug`) ve *appSettings. Production. JSON* (`LogLevel.Error`) tarafından `CreateDefaultBuilder`yapılandırıldı. `ConfigureLogging`birden çok kez çağrılabilir.

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* Aşağıdaki çağrı `ConfigureKestrel` varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) , `CreateDefaultBuilder`Kestrel yapılandırıldığında oluşturulan 30.000.000 bayt:

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Aşağıdaki [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) çağrısı, varsayılan limitleri geçersiz kılar [. MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) , Kestrel tarafından `CreateDefaultBuilder`yapılandırıldığında oluşturulan 30.000.000 bayttan oluşur:

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

*İçerik kökü* , konağın MVC görünüm dosyaları gibi içerik dosyalarını arayacağı yeri belirler. Uygulama, projenin kök klasöründen başlatıldığında, projenin kök klasörü içerik kökü olarak kullanılır. Bu, [Visual Studio](https://visualstudio.microsoft.com) 'da ve [DotNet yeni şablonlarda](/dotnet/core/tools/dotnet-new)kullanılan varsayılandır.

Uygulama yapılandırması hakkında daha fazla bilgi için bkz <xref:fundamentals/configuration/index>.

> [!NOTE]
> Statik `CreateDefaultBuilder` yöntemin kullanılmasına alternatif olarak, [webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) 'dan bir konak oluşturmak, ASP.NET Core 2. x ile desteklenen bir yaklaşımdır.

Bir ana bilgisayar ayarlanırken, [yapılandırma](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure) ve [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices) yöntemleri bulunabilir. Bir `Startup` sınıf belirtilmişse, bir `Configure` yöntemi tanımlamalıdır. Daha fazla bilgi için bkz. <xref:fundamentals/startup>. Birbirine eklenecek birden `ConfigureServices` çok çağrı. Önceki ayarları `WebHostBuilder` Değiştir `Configure` üzerinde `UseStartup` veya üzerine birden çok çağrı.

## <a name="host-configuration-values"></a>Ana bilgisayar yapılandırma değerleri

[Webhostbuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) , ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımları kullanır:

* Bu biçimdeki `ASPNETCORE_{configurationKey}`ortam değişkenlerini içeren konak Oluşturucu yapılandırması. Örneğin: `ASPNETCORE_ENVIRONMENT`.
* [Usecontentroot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) ve [useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) gibi uzantılar ( [geçersiz kılma yapılandırması](#override-configuration) bölümüne bakın).
* [Usesetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) ve ilişkili anahtar. İle `UseSetting`bir değer ayarlarken, değer türünden bağımsız olarak bir dize olarak ayarlanır.

Konak, bir değeri en son ayarlayan seçeneği kullanır. Daha fazla bilgi için, sonraki bölümde [yapılandırmayı geçersiz kılma](#override-configuration) bölümüne bakın.

### <a name="application-key-name"></a>Uygulama anahtarı (ad)

Konak oluşturma sırasında [Usestartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) veya [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) çağrıldığında, [ıhostingenvironment. ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği otomatik olarak ayarlanır. Değer, uygulamanın giriş noktasını içeren derlemenin adına ayarlanır. Değeri açıkça ayarlamak için [Webhostdefaults. ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey)kullanın:

**Anahtar**: ApplicationName  
**Tür**: *dize*  
**Varsayılan**: Uygulamanın giriş noktasını içeren derlemenin adı.  
Şunu **kullanarak ayarla**:`UseSetting`  
**Ortam değişkeni**:`ASPNETCORE_APPLICATIONNAME`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a>Yakalama başlatma hataları

Bu ayar, başlatma hatalarının yakalanmasını denetler.

**Anahtar**: capturestartuperrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: Uygulamanın IIS 'nin arkasında Kestrel, varsayılan olarak olduğu `true` durumlardışındaçalışır.`false`  
Şunu **kullanarak ayarla**:`CaptureStartupErrors`  
**Ortam değişkeni**:`ASPNETCORE_CAPTURESTARTUPERRORS`

Ne `false`zaman, başlatma sırasında oluşan hata, ana bilgisayardan çıkılıyor. Ne `true`zaman, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a>İçerik kökü

Bu ayar, ASP.NET Core MVC görünümleri gibi içerik dosyalarını aramaya başladığı yeri belirler. 

**Anahtar**: contentroot  
**Tür**: *dize*  
**Varsayılan**: Uygulama derlemesinin bulunduğu klasörü varsayılan olarak belirler.  
Şunu **kullanarak ayarla**:`UseContentRoot`  
**Ortam değişkeni**:`ASPNETCORE_CONTENTROOT`

İçerik kökü, [Web kök ayarı](#web-root)için temel yol olarak da kullanılır. Yol yoksa, ana bilgisayar başlatılamaz.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a>Ayrıntılı hatalar

Ayrıntılı hataların yakalanıp yakalanmayacağını belirler.

**Anahtar**: detailederrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
Şunu **kullanarak ayarla**:`UseSetting`  
**Ortam değişkeni**:`ASPNETCORE_DETAILEDERRORS`

Etkinleştirildiğinde (veya <a href="#environment">ortam</a> olarak `Development`ayarlandığında), uygulama ayrıntılı özel durumları yakalar.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a>Ortam

Uygulamanın ortamını ayarlar.

**Anahtar**: ortam  
**Tür**: *dize*  
**Varsayılan**: Üretiminden  
Şunu **kullanarak ayarla**:`UseEnvironment`  
**Ortam değişkeni**:`ASPNETCORE_ENVIRONMENT`

Ortam herhangi bir değere ayarlanabilir. Çerçeve tanımlı değerler, `Development` `Staging`, ve `Production`içerir. Değerler büyük/küçük harfe duyarlı değildir. Varsayılan olarak *ortam* , `ASPNETCORE_ENVIRONMENT` ortam değişkeninden okunurdur. [Visual Studio](https://visualstudio.microsoft.com)kullanılırken, *launchsettings. JSON* dosyasında ortam değişkenleri ayarlanabilir. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a>Başlangıç derlemelerini barındırma

Uygulamanın barındırma başlangıç derlemelerini ayarlar.

**Anahtar**: hostingStartupAssemblies  
**Tür**: *dize*  
**Varsayılan**: Boş dize  
Şunu **kullanarak ayarla**:`UseSetting`  
**Ortam değişkeni**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`

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
Şunu **kullanarak ayarla**: `UseSetting`
**Ortam değişkeni**:`ASPNETCORE_HTTPS_PORT`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a>Barındırma başlatma derlemeleri dışlama

Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.

**Anahtar**: hostingstartupexcludeassemblies  
**Tür**: *dize*  
**Varsayılan**: Boş dize  
Şunu **kullanarak ayarla**:`UseSetting`  
**Ortam değişkeni**:`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a>Barındırma URL 'Lerini tercih et

Konağın `WebHostBuilder` `IServer` uygulamayla yapılandırılanlar yerine ile yapılandırılan URL 'lerde dinleme yapıp kullanmayacağını belirtir.

**Anahtar**: preferhostingurl 'leri  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: true  
Şunu **kullanarak ayarla**:`PreferHostingUrls`  
**Ortam değişkeni**:`ASPNETCORE_PREFERHOSTINGURLS`

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a>Barındırma başlangıcını engelle

Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

**Anahtar**: koruyucu thostingstartup  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
Şunu **kullanarak ayarla**:`UseSetting`  
**Ortam değişkeni**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a>Sunucu URL 'Leri

Sunucunun istekler için dinlemesi gereken bağlantı noktaları ve protokoller içeren IP adreslerini veya ana bilgisayar adreslerini gösterir.

**Anahtar**: URL 'ler  
**Tür**: *dize*  
**Varsayılan**: http://localhost:5000  
Şunu **kullanarak ayarla**:`UseUrls`  
**Ortam değişkeni**:`ASPNETCORE_URLS`

Noktalı virgülle ayrılmış olarak ayarlayın (;) sunucunun yanıtlaması gereken URL ön eklerinin listesi. Örneğin: `http://localhost:123`. Sunucunun belirtilen\*bağlantı noktasını ve Protokolü (örneğin, `http://*:5000`) kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "" kullanın. Protokol (`http://` veya `https://`) her URL 'ye dahil edilmiş olmalıdır. Desteklenen biçimler sunucular arasında farklılık gösterir.

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
Şunu **kullanarak ayarla**:`UseShutdownTimeout`  
**Ortam değişkeni**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`

Anahtar, ile `UseSetting` bir *int* kabul etse de (örneğin, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), [useshutdowntimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) genişletme yöntemi bir [TimeSpan](/dotnet/api/system.timespan)alır.

Zaman aşımı süresi boyunca barındırma:

* [Iapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)öğesini tetikler.
* Üzerinde durmayacak hizmetlerin hatalarını günlüğe kaydetmek için barındırılan hizmetleri durdurmaya çalışır.

Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur. Hizmetler, işlemeyi tamamlamadıklarında bile durur. Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a>Başlangıç derlemesi

`Startup` Sınıfın aranacağı derlemeyi belirler.

**Anahtar**: startupassembly  
**Tür**: *dize*  
**Varsayılan**: Uygulamanın derlemesi  
Şunu **kullanarak ayarla**:`UseStartup`  
**Ortam değişkeni**:`ASPNETCORE_STARTUPASSEMBLY`

Ada (`string`) veya türe (`TStartup`) göre derlemeye başvurulabilir. Birden çok `UseStartup` yöntem çağrılırsa, son bir öncelik alır.

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
**Varsayılan**: Belirtilmemişse, varsayılan olarak "(Içerik kökü)/Wwwroot" olur. Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.  
Şunu **kullanarak ayarla**:`UseWebRoot`  
**Ortam değişkeni**:`ASPNETCORE_WEBROOT`

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a>Geçersiz kılma yapılandırması

Web konağını yapılandırmak için [yapılandırma](xref:fundamentals/configuration/index) kullanın. Aşağıdaki örnekte, konak yapılandırması isteğe bağlı olarak bir *HostSettings. JSON* dosyasında belirtilir. *HostSettings. JSON* dosyasından yüklenen herhangi bir yapılandırma komut satırı bağımsız değişkenleri tarafından geçersiz kılınabilir. Oluşturulan yapılandırma (içinde `config`), Konağı [useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration)ile yapılandırmak için kullanılır. `IWebHostBuilder`yapılandırma uygulamanın yapılandırmasına eklenir, ancak dönüştürme doğru&mdash; `ConfigureAppConfiguration` değildir, `IWebHostBuilder` yapılandırmayı etkilemez.

*HostSettings. JSON* config `UseUrls` ile tarafından belirtilen yapılandırmayı geçersiz kılma, komut satırı bağımsız değişkeni yapılandırma saniyesi:

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
> [Useconfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) genişletme yöntemi şu anda tarafından `GetSection` döndürülen bir yapılandırma bölümünü ayrıştırma özelliğine sahip değil (örneğin,. `.UseConfiguration(Configuration.GetSection("section"))` Yöntemi, yapılandırma anahtarlarını istenen bölüm ile filtreleyerek, ancak anahtarlar üzerinde bölüm adını bırakır (örneğin `section:environment`, `section:urls`,). `GetSection` Yöntemi anahtarların `WebHostBuilder`anahtarlarlaeşleşmesini bekler (örneğin, `environment` ,).`urls` `UseConfiguration` Anahtarlarda bölüm adının varlığı, bölümün değerlerinin konak yapılandırmasını engeller. Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir. Daha fazla bilgi ve geçici çözüm için bkz [. yapılandırma bölümünü WebHostBuilder 'A geçirme. UseConfiguration tam anahtarları kullanır](https://github.com/aspnet/Hosting/issues/839).
>
> `UseConfiguration`yalnızca belirtilen `IConfiguration` içindeki anahtarları ana bilgisayar Oluşturucu yapılandırmasına kopyalar. Bu nedenle, `reloadOnChange: true` JSON, ını ve xml ayarları dosyalarının ayarlanması etkisizdir.

Belirli bir URL 'de çalıştırılacak Konağı belirtmek için, [DotNet çalıştırması](/dotnet/core/tools/dotnet-run)yürütürken istenen değer bir komut isteminden geçirilebilir. Komut satırı bağımsız değişkeni `urls` *HostSettings. JSON* dosyasından değeri geçersiz kılar ve sunucu 8080 numaralı bağlantı noktasını dinler:

```dotnetcli
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a>Konağı yönetme

**Çalıştır**

`Run` Yöntemi, Web uygulamasını başlatır ve konak kapanana kadar çağıran iş parçacığını engeller:

```csharp
host.Run();
```

**Start**

`Start` Yöntemini çağırarak, Konağı engellenmeyen bir şekilde çalıştırın:

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

`Start` Yöntemine bir URL listesi geçirilirse, belirtilen URL 'leri dinler:

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

Uygulama, statik bir kolaylık yöntemi `CreateDefaultBuilder` kullanarak önceden yapılandırılmış varsayılan ayarları kullanarak yeni bir ana bilgisayar başlatabilir ve başlatabilir. Bu yöntemler, konsol çıktısı olmadan sunucuyu başlatır ve [Waitforkapatmadan](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) bir kesme (CTRL-C/sigint veya sigterim) bekler:

**Başlat (RequestDelegate uygulaması)**

Kullanmaya `RequestDelegate`başlayın:

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

"Merhaba Dünya!" yanıtını almak `http://localhost:5000` için tarayıcıda bir istek yapın `WaitForShutdown`kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar engeller. Uygulama `Console.WriteLine` iletiyi görüntüler ve bir tuş basışını, çıkış için bekler.

**Başlangıç (dize URL 'si, RequestDelegate uygulaması)**

URL ile başlayın ve `RequestDelegate`:

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

Uygulamanın yanıt `http://localhost:8080`vermesi dışında, **Başlangıç (requestdelegate uygulaması)** ile aynı sonucu üretir.

**Başlat (eylem&lt;ıroutebuilder&gt; routebuilder)**

Yönlendirme ara yazılımını kullanmak `IRouteBuilder` için ([Microsoft. aspnetcore. Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) örneğini kullanın:

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

`WaitForShutdown`kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar engeller. Uygulama `Console.WriteLine` iletiyi görüntüler ve bir tuş basışını, çıkış için bekler.

**Başlat (dize URL 'si,&lt;eylem ıroutebuilder&gt; routebuilder)**

Bir URL ve örneği `IRouteBuilder`kullanın:

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

Uygulamanın Yanıt`http://localhost:8080`vermesi dışında, **Başlangıç (eylem&lt;ıroutebuilder&gt; routebuilder)** ile aynı sonucu üretir.

**StartWith (eylem&lt;IApplicationBuilder&gt; uygulaması)**

Yapılandırmak için bir temsilci sağlayın `IApplicationBuilder`:

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

"Merhaba Dünya!" yanıtını almak `http://localhost:5000` için tarayıcıda bir istek yapın `WaitForShutdown`kesme (CTRL-C/SIGINT veya SIGTERM) verilene kadar engeller. Uygulama `Console.WriteLine` iletiyi görüntüler ve bir tuş basışını, çıkış için bekler.

**StartWith (dize URL 'si,&lt;eylem IApplicationBuilder&gt; uygulaması)**

Yapılandırmak için bir URL ve temsilci sağlayın `IApplicationBuilder`:

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

Uygulamanın Yanıt`http://localhost:8080`vermesi dışında, **StartWith (Action&lt;iapplicationbuilder&gt; uygulaması) ile**aynı sonucu üretir.

## <a name="ihostingenvironment-interface"></a>Ihostingenvironment arabirimi

[Ihostingenvironment arabirimi](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) , uygulamanın Web barındırma ortamı hakkında bilgi sağlar. Özelliklerini ve uzantı yöntemlerini kullanmak üzere `IHostingEnvironment` sağlamak için [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) özelliğini kullanın:

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

Uygulamayı ortama göre başlangıçta yapılandırmak için [kural tabanlı bir yaklaşım](xref:fundamentals/environments#environment-based-startup-class-and-methods) kullanılabilir. Alternatif olarak, `IHostingEnvironment` ' de `ConfigureServices`kullanım `Startup` için oluşturucuya ekleyin:

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
> `IsDevelopment` Uzantı yöntemine`IsProduction` ekolarak`IsEnvironment(string environmentName)` ,, ve yöntemleri. `IHostingEnvironment` `IsStaging` Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

Hizmet ayrıca işlem ardışık düzenini ayarlama `Configure` yöntemine doğrudan eklenebilir: `IHostingEnvironment`

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

`IHostingEnvironment`özel [Ara yazılım](xref:fundamentals/middleware/write)oluşturulurken yöntemineeklenebilir:`Invoke`

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

## <a name="iapplicationlifetime-interface"></a>Iapplicationlifetime arabirimi

[Iapplicationlifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) , başlatma sonrası ve kapalı etkinlikler için izin verir. Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan yöntemleri kaydetmek `Action` için kullanılan iptal belirteçleridir.

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

[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) , uygulamanın sonlandırılmasını ister. Aşağıdaki sınıf, `Shutdown` sınıfın `StopApplication` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için kullanır:

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

## <a name="scope-validation"></a>Kapsam doğrulaması

[Createdefaultbuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) , uygulamanın ortamı geliştirmede `true` [serviceprovideroptions. validatescopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) ' i ayarlar.

`ValidateScopes` , Olarak`true`ayarlandığında, varsayılan hizmet sağlayıcısı şunları doğrulamak için denetimler gerçekleştirir:

* Kapsamlı hizmetler doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözümlenmez.
* Kapsamlı hizmetler doğrudan veya dolaylı olarak Singleton 'a eklenmiş değildir.

[Buildserviceprovider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrıldığında kök hizmet sağlayıcısı oluşturulur. Kök hizmet sağlayıcısının ömrü, sağlayıcının uygulamayla başladığı ve uygulama kapandığında bırakıldığı uygulama/sunucunun yaşam süresine karşılık gelir.

Kapsamlı hizmetler kendilerini oluşturan kapsayıcı tarafından atılmış. Kök kapsayıcıda kapsamlı bir hizmet oluşturulduysa, hizmetin ömrü etkin şekilde tek başına yükseltilir çünkü yalnızca uygulama/sunucu kapatıldığında kök kapsayıcı tarafından atılmış olur. Hizmet kapsamlarını doğrulamak `BuildServiceProvider` , çağrıldığında bu durumları yakalar.

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
