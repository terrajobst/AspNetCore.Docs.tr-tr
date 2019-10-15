---
title: .NET genel ana bilgisayar
author: rick-anderson
description: Uygulama başlatma ve ömür yönetiminden sorumlu .NET Core genel ana bilgisayarı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 57c09d52e058f0355849793b16d430bda0deb3a6
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333608"
---
# <a name="net-generic-host"></a>.NET genel ana bilgisayar

::: moniker range=">= aspnetcore-3.0"

Bu makalede, .NET Core genel ana bilgisayarı (<xref:Microsoft.Extensions.Hosting.HostBuilder>) tanıtılmakta ve nasıl kullanılacağı hakkında rehberlik sunulmaktadır.

## <a name="whats-a-host"></a>Ana bilgisayar nedir?

*Ana bilgisayar* , bir uygulamanın kaynaklarını kapsülleyen bir nesnedir, örneğin:

* Bağımlılık ekleme (dı)
* Günlüğe Kaydetme
* Yapılandırma
* `IHostedService` uygulamaları

Bir konak başlatıldığında, DI kapsayıcısında bulduğu her bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamasında `IHostedService.StartAsync` ' ı çağırır. Bir Web uygulamasında, `IHostedService` uygulamalarından biri [http sunucu uygulaması](xref:fundamentals/index#servers)Başlatan bir Web hizmetidir.

Uygulamanın tüm birbirine bağlı kaynaklarını tek bir nesnede dahil etmek için başlıca neden, yaşam süresi yönetimi: uygulama başlatma ve düzgün kapanma üzerinde denetim.

3,0 ' den önceki ASP.NET Core sürümlerinde, [Web ana BILGISAYARı](xref:fundamentals/host/web-host) http iş yükleri için kullanılır. Web ana bilgisayarı artık Web uygulamaları için önerilmez ve yalnızca geriye dönük uyumluluk için kullanılabilir durumda kalır.

## <a name="set-up-a-host"></a>Konak ayarlama

Konak genellikle `Program` sınıfındaki kodla yapılandırılır, oluşturulur ve çalıştırılır. @No__t-0 yöntemi:

* Bir Oluşturucu nesnesi oluşturmak ve yapılandırmak için `CreateHostBuilder` yöntemini çağırır.
* Oluşturucu nesnesinde `Build` ve `Run` yöntemlerini çağırır.

Bu, *program.cs* , tek bir `IHostedService` uygulama olan ve dı KAPSAYıCıSıNA eklenen http olmayan bir iş yükü için bir kod sağlar. 

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

Bir HTTP iş yükü için `Main` yöntemi aynıdır, ancak `CreateHostBuilder` çağrıları `ConfigureWebHostDefaults`:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

Uygulama Entity Framework Core kullanıyorsa `CreateHostBuilder` yönteminin adını veya imzasını değiştirmeyin. [Entity Framework Core araçları](/ef/core/miscellaneous/cli/) , uygulamayı çalıştırmadan Konağı yapılandıran bir `CreateHostBuilder` yöntemi bulmayı bekler. Daha fazla bilgi için bkz. [Tasarım zamanı DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).

## <a name="default-builder-settings"></a>Varsayılan Oluşturucu ayarları

@No__t-0 yöntemi:

* [İçerik kökünü](xref:fundamentals/index#content-root) <xref:System.IO.Directory.GetCurrentDirectory*> tarafından döndürülen yola ayarlar.
* Ana bilgisayar yapılandırmasını şuradan yükler:
  * "DOTNET_" önekli ortam değişkenleri.
  * Komut satırı bağımsız değişkenleri.
* Uygulama yapılandırmasını şuradan yükler:
  * *appSettings. JSON*.
  * *appSettings. {Environment}. JSON*.
  * Uygulama `Development` ortamında çalıştığında [gizli yönetici](xref:security/app-secrets) .
  * Ortam değişkenleri.
  * Komut satırı bağımsız değişkenleri.
* Aşağıdaki [günlük](xref:fundamentals/logging/index) sağlayıcılarını ekler:
  * Konsolu
  * Hata ayıklama
  * EventSource
  * Olay günlüğü (yalnızca Windows üzerinde çalışırken)
* Ortam geliştirme sırasında [kapsam doğrulaması](xref:fundamentals/dependency-injection#scope-validation) ve [bağımlılık doğrulaması](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) etkinleştirilir.

@No__t-0 yöntemi:

* "ASPNETCORE_" önekli ortam değişkenlerinden ana bilgisayar yapılandırmasını yükler.
* [Kestrel](xref:fundamentals/servers/kestrel) sunucusunu Web sunucusu olarak ayarlar ve uygulamanın barındırma yapılandırma sağlayıcılarını kullanarak yapılandırır. Kestrel sunucusunun varsayılan seçenekleri için bkz. <xref:fundamentals/servers/kestrel#kestrel-options>.
* [Ana bilgisayar filtreleme ara yazılımı](xref:fundamentals/servers/kestrel#host-filtering)ekler.
* ASPNETCORE_FORWARDEDHEADERS_ENABLED = true ise [Iletilen üstbilgiler ara yazılımı](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) ekler.
* IIS tümleştirmesini etkinleştirilir. IIS varsayılan seçenekleri için bkz. <xref:host-and-deploy/iis/index#iis-options>.

Bu makalenin ilerleyen kısımlarında [Web Apps bölümlerine yönelik](#settings-for-web-apps) [tüm uygulama türleri](#settings-for-all-app-types) ve ayarlarının ayarları, varsayılan Oluşturucu ayarlarının nasıl geçersiz kılınacağını göstermektedir.

## <a name="framework-provided-services"></a>Framework tarafından sunulan hizmetler

Kayıtlı hizmetler otomatik olarak şunları içerir:

* [Ihostapplicationlifetime](#ihostapplicationlifetime)
* [Ihostlifetime](#ihostlifetime)
* [Ihostenvironment/ıwebhostenvironment](#ihostenvironment)

Framework tarafından sunulan hizmetler hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection#framework-provided-services>.

## <a name="ihostapplicationlifetime"></a>Ihostapplicationlifetime

Başlatma sonrası ve düzgün kapanma görevlerini işlemek için <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (eski adıyla `IApplicationLifetime`) hizmetini herhangi bir sınıfa ekleyin. Arabirimdeki üç özellik, uygulama başlatma ve uygulama durdurma olay işleyicisi yöntemlerini kaydetmek için kullanılan iptal belirteçleridir. Arabirim Ayrıca bir `StopApplication` yöntemi içerir.

Aşağıdaki örnek, `IHostApplicationLifetime` olaylarını kaydeden `IHostedService` uygulamasıdır:

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a>Ihostlifetime

@No__t 0 uygulama, ana bilgisayar başladığında ve durdurulduğunda denetler. Kaydedilen son uygulama kullanılır.

<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> varsayılan `IHostLifetime` uygulamasıdır. `ConsoleLifetime`:

* başlatma işlemini başlatmak için CTRL + C/SIGINT veya SIGTERM ve <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> aramalarını dinler.
* [RunAsync](#runasync) ve [Waitforshutdownasync](#waitforshutdownasync)gibi uzantıları kaldırır.

## <a name="ihostenvironment"></a>Ihostenvironment

@No__t-0 hizmetini bir sınıfa ekleyin ve aşağıdakiler hakkında bilgi alın:

* [ApplicationName](#applicationname)
* [EnvironmentName](#environmentname)
* [Contentrootyolu](#contentrootpath)

Web Apps, `IHostEnvironment` ' i devralan `IWebHostEnvironment` arabirimini uygular ve şunları ekler:

* [WebRootPath](#webroot)

## <a name="host-configuration"></a>Konak yapılandırması

Ana bilgisayar yapılandırması <xref:Microsoft.Extensions.Hosting.IHostEnvironment> uygulamasının özellikleri için kullanılır.

Konak yapılandırması, <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> ' deki [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) içinden kullanılabilir. @No__t-0 ' dan sonra, `HostBuilderContext.Configuration`, uygulama yapılandırması ile değiştirilmiştir.

Konak yapılandırması eklemek için, `IHostBuilder` üzerinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> ' ı çağırın. `ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir. Ana bilgisayar, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.

@No__t-0 ve komut satırı bağımsız değişkenleri ön ekine sahip ortam değişkeni sağlayıcısı, CreateDefaultBuilder tarafından eklenmiştir. Web Apps için, `ASPNETCORE_` ön ekine sahip ortam değişkeni sağlayıcısı eklenir. Ortam değişkenleri okurken ön ek kaldırılır. Örneğin, `ASPNETCORE_ENVIRONMENT` için ortam değişkeni değeri, `environment` anahtarı için ana bilgisayar yapılandırma değeri haline gelir.

Aşağıdaki örnek ana bilgisayar yapılandırması oluşturur:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a>Uygulama yapılandırması

Uygulama yapılandırması, `IHostBuilder` üzerinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> çağırarak oluşturulur. `ConfigureAppConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir. Uygulama, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır. 

@No__t-0 tarafından oluşturulan yapılandırma, sonraki işlemler ve DI hizmeti olarak [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) ' da kullanılabilir. Konak yapılandırması, uygulama yapılandırmasına de eklenir.

Daha fazla bilgi için [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index#configureappconfiguration)konusuna bakın.

## <a name="settings-for-all-app-types"></a>Tüm uygulama türleri için ayarlar

Bu bölüm, hem HTTP hem de HTTP olmayan iş yükleri için uygulanan konak ayarlarını listeler. Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin `DOTNET_` veya `ASPNETCORE_` öneki olabilir.

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a>ApplicationName

[Ihostenvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) özelliği konak oluşturma sırasında konak yapılandırmasından ayarlanır.

**Anahtar**: ApplicationName  
**Tür**: *dize*  
**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.
**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME`

Bu değeri ayarlamak için ortam değişkenini kullanın. 

### <a name="contentrootpath"></a>Contentrootyolu

[Ihostenvironment. ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) özelliği, konağın içerik dosyalarını aramaya başladığı yeri belirler. Yol yoksa, ana bilgisayar başlatılamaz.

**Anahtar**: contentroot  
**Tür**: *dize*  
**Varsayılan**: uygulama derlemesinin bulunduğu klasör.  
**Ortam değişkeni**: `<PREFIX_>CONTENTROOT`

Bu değeri ayarlamak için, ortam değişkenini kullanın veya `IHostBuilder` üzerinde `UseContentRoot` ' ı çağırın:

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

Daha fazla bilgi için bkz.:

* [Temel bilgiler: Içerik kökü](xref:fundamentals/index#content-root)
* [WebRoot](#webroot)

### <a name="environmentname"></a>EnvironmentName

[Ihostenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) özelliği herhangi bir değere ayarlanabilir. Çerçeve tanımlı değerler `Development`, `Staging` ve `Production` içerir. Değerler büyük/küçük harfe duyarlı değildir.

**Anahtar**: ortam  
**Tür**: *dize*  
**Varsayılan**: üretim  
**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT`

Bu değeri ayarlamak için, ortam değişkenini kullanın veya `IHostBuilder` üzerinde `UseEnvironment` ' ı çağırın:

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a>ShutdownTimeout

[Hostoptions. shutdowntimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> için zaman aşımını ayarlar. Varsayılan değer beş saniyedir.  Zaman aşımı süresi boyunca ana bilgisayar:

* [Ihostapplicationlifetime. Applicationdurduruluyor](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping)tetikler.
* Üzerinde durmayacak hizmetler için barındırılan Hizmetleri durdurma ve hataları günlüğe kaydetme girişimleri.

Tüm barındırılan hizmetler durmadan önce zaman aşımı süresi dolarsa, uygulama kapandığında kalan etkin hizmetler durdurulur. Hizmetler, işlemeyi tamamlamadıklarında bile durur. Hizmetlerin durdurulması için ek süre gerekiyorsa, zaman aşımını artırın.

**Anahtar**: shutdowntimeoutseconds  
**Tür**: *int*  
**Varsayılan**: 5 saniye **ortam değişkeni**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`

Bu değeri ayarlamak için, ortam değişkenini kullanın veya `HostOptions` ' ı yapılandırın. Aşağıdaki örnek, zaman aşımını 20 saniye olarak ayarlar:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a>Web Apps ayarları

Bazı konak ayarları yalnızca HTTP iş yükleri için geçerlidir. Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerinin `DOTNET_` veya `ASPNETCORE_` öneki olabilir.

@No__t-0 ' da uzantı yöntemleri bu ayarlar için kullanılabilir. Uzantı yöntemlerinin nasıl çağrılacağını gösteren kod örnekleri, aşağıdaki örnekte olduğu gibi `webBuilder` `IWebHostBuilder` ' in bir örneğidir:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a>CaptureStartupErrors

@No__t-0 olduğunda, başlangıçtaki ana bilgisayardaki hatalar çıkıyor. @No__t-0 olduğunda, ana bilgisayar başlangıç sırasında özel durumları yakalar ve sunucuyu başlatmaya çalışır.

**Anahtar**: capturestartuperrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: uygulama IIS arkasındaki Kestrel ile çalıştırılmadığı müddetçe `false` ' dir. varsayılan olarak, `true` ' dir.  
**Ortam değişkeni**: `<PREFIX_>CAPTURESTARTUPERRORS`

Bu değeri ayarlamak için yapılandırma veya çağrı @no__t kullanın-0:

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a>DetailedErrors

Etkinleştirildiğinde veya ortam `Development` olduğunda, uygulama ayrıntılı hataları yakalar.

**Anahtar**: detailederrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
**Ortam değişkeni**: `<PREFIX_>_DETAILEDERRORS`

Bu değeri ayarlamak için yapılandırma veya çağrı @no__t kullanın-0:

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a>HostingStartupAssemblies

Başlangıçta yüklenecek başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize. Yapılandırma değeri boş bir dize olarak varsayılan olsa da, barındırma başlangıç derlemeleri her zaman uygulamanın derlemesini içerir. Barındırma başlangıç derlemeleri sağlandığında, uygulama başlangıç sırasında ortak hizmetlerini oluşturduğunda yükleme için uygulamanın derlemesine eklenir.

**Anahtar**: hostingStartupAssemblies  
**Tür**: *dize*  
**Varsayılan**: boş dize  
**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`

Bu değeri ayarlamak için yapılandırma veya çağrı @no__t kullanın-0:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a>HostingStartupExcludeAssemblies

Başlangıçta dışlamak üzere başlangıç derlemelerinin barındırılması için noktalı virgülle ayrılmış bir dize.

**Anahtar**: hostingstartupexcludeassemblies  
**Tür**: *dize*  
**Varsayılan**: boş dize  
**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

Bu değeri ayarlamak için yapılandırma veya çağrı @no__t kullanın-0:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a>HTTPS_Port

HTTPS yeniden yönlendirme bağlantı noktası. [Https zorlama](xref:security/enforcing-ssl)bölümünde kullanılır.

**Anahtar**: https_port  
**Tür**: *dize*  
**Varsayılan**: varsayılan değer ayarlı değildir.  
**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT`

Bu değeri ayarlamak için yapılandırma veya çağrı @no__t kullanın-0:

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a>Tercih Hostingurl 'Leri

Konağın `IServer` uygulamasıyla yapılandırılanlar yerine `IWebHostBuilder` ile yapılandırılan URL 'Leri dinlemesi gerekip gerekmediğini gösterir.

**Anahtar**: preferhostingurl 'leri  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: true  
**Ortam değişkeni**: `<PREFIX_>_PREFERHOSTINGURLS`

Bu değeri ayarlamak için, ortam değişkenini kullanın veya `PreferHostingUrls` ' ı çağırın:

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a>Koruyucu Thostınstartup

Uygulamanın derlemesi tarafından yapılandırılan başlatma derlemelerinin barındırılması dahil olmak üzere, barındırma başlangıç derlemelerinin otomatik yüklenmesini engeller. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

**Anahtar**: koruyucu thostingstartup  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
**Ortam değişkeni**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`

Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseSetting` ' ı çağırın:

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a>StartupAssembly

@No__t-0 sınıfının aranacağı derleme.

**Anahtar**: startupassembly  
**Tür**: *dize*  
**Varsayılan**: uygulamanın derlemesi  
**Ortam değişkeni**: `<PREFIX_>STARTUPASSEMBLY`

Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseStartup` ' ı çağırın. `UseStartup`, bir derleme adı (`string`) veya bir tür (`TStartup`) alabilir. Birden çok `UseStartup` yöntemi çağrılırsa, son bir öncelik alır.

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a>URL'ler

Sunucu istekleri için dinlemesi gereken bağlantı noktaları ve protokollerle, noktalı virgülle ayrılmış IP adresleri listesi veya ana bilgisayar adresleri. Örneğin, `http://localhost:123`. Sunucunun belirtilen bağlantı noktasını ve protokolü kullanarak herhangi bir IP adresi veya ana bilgisayar için istekleri dinlemesi gerektiğini belirtmek için "\*" kullanın (örneğin, `http://*:5000`). Protokol (`http://` veya `https://`) her URL 'ye dahil olmalıdır. Desteklenen biçimler sunucular arasında farklılık gösterir.

**Anahtar**: URL 'ler  
**Tür**: *dize*  
**Varsayılan**: `http://localhost:5000` ve `https://localhost:5001`  
**Ortam değişkeni**: `<PREFIX_>URLS`

Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseUrls` ' ı çağırın:

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

Kestrel kendi uç nokta yapılandırması API 'sine sahiptir. Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.

### <a name="webroot"></a>WebRoot

Uygulamanın statik varlıklarının göreli yolu.

**Anahtar**: Webroot  
**Tür**: *dize*  
**Varsayılan**: varsayılan değer `wwwroot` ' dir. *{Content root}/Wwwroot* yolu var olmalıdır. Yol yoksa, Hayır-op dosya sağlayıcısı kullanılır.  
**Ortam değişkeni**: `<PREFIX_>WEBROOT`

Bu değeri ayarlamak için, ortam değişkenini kullanın veya `UseWebRoot` ' ı çağırın:

```csharp
webBuilder.UseWebRoot("public");
```

Daha fazla bilgi için bkz.:

* [Temel bilgiler: Web kökü](xref:fundamentals/index#web-root)
* [Contentrootyolu](#contentrootpath)

## <a name="manage-the-host-lifetime"></a>Konak ömrünü yönetme

Uygulamayı başlatmak ve durdurmak için oluşturulan <xref:Microsoft.Extensions.Hosting.IHost> uygulamasında Yöntemleri çağırın. Bu yöntemler, hizmet kapsayıcısında kayıtlı olan tüm <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamalarını etkiler.

### <a name="run"></a>Çalıştır

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*>, uygulamayı çalıştırır ve ana bilgisayarı kapatıncaya kadar çağıran iş parçacığını engeller.

### <a name="runasync"></a>RunAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> uygulamayı çalıştırır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlanmış bir <xref:System.Threading.Tasks.Task> döndürür.

### <a name="runconsoleasync"></a>RunConsoleAsync

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>, konsol desteği sağlar, Konağı oluşturur ve başlatır ve CTRL + C/SIGINT veya SIGDÖNEM için bekler.

### <a name="start"></a>Başlat

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Konağı zaman uyumlu olarak başlatır.

### <a name="startasync"></a>StartAsync

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, Konağı başlatır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlanmış bir <xref:System.Threading.Tasks.Task> döndürür. 

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> `StartAsync` ' in başlangıcında çağrılır. Bu, devam etmeden önce tamamlanana kadar bekler. Bu, bir dış olay tarafından sinyallene kadar başlatmayı geciktirmek için kullanılabilir.

### <a name="stopasync"></a>StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>, belirtilen zaman aşımı süresi içinde Konağı durdurmaya çalışır.

### <a name="waitforshutdown"></a>Waitforkapatması

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>, kapatmadan sonra CTRL + C/SIGINT veya SIGTERM gibi ıhostlifetime tarafından tetiklenene kadar çağıran iş parçacığını engeller.

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>, "verilen belirteç aracılığıyla bir işlem tetiklendiğinde, sonra da <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> ' i çağırarak tamamlanan bir <xref:System.Threading.Tasks.Task> döndürür.

### <a name="external-control"></a>Dış denetim

Ana bilgisayar ömrünün doğrudan denetimi dışarıdan çağrılabilen yöntemler kullanılarak sağlanabilir:

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core uygulamalar bir konağı yapılandırıp başlatır. Ana bilgisayar, uygulama başlatma ve ömür yönetiminden sorumludur.

Bu makalede, HTTP isteklerini işlemeyin uygulamalar için kullanılan ASP.NET Core genel ana bilgisayar (<xref:Microsoft.Extensions.Hosting.HostBuilder>) ele alınmaktadır.

Genel konağın amacı, daha geniş bir konak senaryolarını etkinleştirmek üzere Web ana bilgisayar API 'sinden HTTP işlem hattını ayırmak için kullanılır. Yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özelliğinden genel ana bilgisayar avantajı temel alınarak mesajlaşma, arka plan görevleri ve diğer HTTP olmayan iş yükleri.

Genel ana bilgisayar ASP.NET Core 2,1 ' de yenidir ve Web barındırma senaryolarında uygun değildir. Web barındırma senaryolarında [Web konağını](xref:fundamentals/host/web-host)kullanın. Genel ana bilgisayar gelecek bir sürümdeki Web konağını değiştirecek ve hem HTTP hem de HTTP olmayan senaryolarda birincil ana bilgisayar API 'SI olarak görev yapacak.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulamayı [Visual Studio Code](https://code.visualstudio.com/)' de çalıştırırken, *dış veya tümleşik bir Terminal*kullanın. Örneği bir @no__t çalıştırılmadı-0.

Konsolu Visual Studio Code ayarlamak için:

1. *. Vscode/Launch. JSON* dosyasını açın.
1. **.NET Core başlatma (konsol)** yapılandırmasında **konsol** girişini bulun. Değeri `externalTerminal` ya da `integratedTerminal` olarak ayarlayın.

## <a name="introduction"></a>Giriş

Genel ana bilgisayar kitaplığı <xref:Microsoft.Extensions.Hosting> ad alanında kullanılabilir ve [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paketi tarafından sağlanır. @No__t-0 paketi [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e dahildir (ASP.NET Core 2,1 veya üzeri).

<xref:Microsoft.Extensions.Hosting.IHostedService>, kod yürütmeye yönelik giriş noktasıdır. Her @no__t 0 uygulama, [ConfigureServices içinde hizmet kaydı](#configureservices)sırasında yürütülür. <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*>, konak başladığında her <xref:Microsoft.Extensions.Hosting.IHostedService> ' de çağrılır ve ana bilgisayar düzgün bir şekilde kapandığında <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> ters kayıt sırasında çağrılır.

## <a name="set-up-a-host"></a>Konak ayarlama

<xref:Microsoft.Extensions.Hosting.IHostBuilder>, kitaplıkların ve uygulamaların Konağı başlatmak, derlemek ve çalıştırmak için kullandığı ana bileşendir:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a>Seçenekler

<xref:Microsoft.Extensions.Hosting.HostOptions> <xref:Microsoft.Extensions.Hosting.IHost> için seçenekleri yapılandırın.

### <a name="shutdown-timeout"></a>Kapatılma zaman aşımı

<xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> için zaman aşımını ayarlar. Varsayılan değer beş saniyedir.

@No__t-0 ' daki aşağıdaki seçenek yapılandırması, varsayılan beş saniyelik kapatılma zaman aşımını 20 saniyeye yükseltir:

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a>Varsayılan hizmetler

Aşağıdaki hizmetler konak başlatma sırasında kaydedilir:

* [Ortam](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* [Yapılandırma](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)
* <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)
* <xref:Microsoft.Extensions.Hosting.IHost>
* [Seçenekler](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)
* [Günlüğe kaydetme](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)

## <a name="host-configuration"></a>Konak yapılandırması

Ana bilgisayar yapılandırması şu şekilde oluşturulur:

* [İçerik kökünü](#content-root) ve [ortamı](#environment)ayarlamak için <xref:Microsoft.Extensions.Hosting.IHostBuilder> ' da uzantı yöntemleri çağrılıyor.
* @No__t-0 ' daki yapılandırma sağlayıcılarından yapılandırma okunuyor.

### <a name="extension-methods"></a>Uzantı yöntemleri

### <a name="application-key-name"></a>Uygulama anahtarı (ad)

[Ihostingenvironment. ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) özelliği konak oluşturma sırasında konak yapılandırmasından ayarlanır. Değeri açıkça ayarlamak için, [Hostdefaults. ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey)kullanın:

**Anahtar**: ApplicationName  
**Tür**: *dize*  
**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.  
Şunu **kullanarak ayarla**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` [isteğe bağlıdır ve Kullanıcı tanımlı](#configurehostconfiguration))

### <a name="content-root"></a>İçerik kökü

Bu ayar, konağın içerik dosyalarını aramaya başladığı yeri belirler.

**Anahtar**: contentroot  
**Tür**: *dize*  
**Varsayılan**: uygulama derlemesinin bulunduğu klasörü varsayılan olarak belirler.  
Şunu **kullanarak ayarla**: `UseContentRoot`  
**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` [isteğe bağlıdır ve Kullanıcı tanımlı](#configurehostconfiguration))

Yol yoksa, ana bilgisayar başlatılamaz.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

Daha fazla bilgi için bkz. [temel bilgiler: içerik kökü](xref:fundamentals/index#content-root).

### <a name="environment"></a>Ortam

Uygulamanın [ortamını](xref:fundamentals/environments)ayarlar.

**Anahtar**: ortam  
**Tür**: *dize*  
**Varsayılan**: üretim  
Şunu **kullanarak ayarla**: `UseEnvironment`  
**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` [isteğe bağlıdır ve Kullanıcı tanımlı](#configurehostconfiguration))

Ortam herhangi bir değere ayarlanabilir. Çerçeve tanımlı değerler `Development`, `Staging` ve `Production` içerir. Değerler büyük/küçük harfe duyarlı değildir.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, ana bilgisayar için <xref:Microsoft.Extensions.Configuration.IConfiguration> oluşturmak üzere bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanır. Ana bilgisayar yapılandırması, uygulamanın derleme sürecinde kullanılmak üzere <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> başlatmak için kullanılır.

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, eklenebilir sonuçlarla birden çok kez çağrılabilir. Ana bilgisayar, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır.

Varsayılan olarak hiçbir sağlayıcı dahil değildir. Uygulamanın <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> ' da gereken her türlü yapılandırma sağlayıcısını açıkça belirtmeniz gerekir; örneğin:

* Dosya yapılandırması (örneğin, *HostSettings. JSON* dosyasından).
* Ortam değişkeni yapılandırması.
* Komut satırı bağımsız değişken yapılandırması.
* Diğer tüm gerekli yapılandırma sağlayıcıları.

Ana bilgisayarın dosya yapılandırması, uygulamanın temel yolu `SetBasePath` ile ve ardından [dosya yapılandırma sağlayıcılarından](xref:fundamentals/configuration/index#file-configuration-provider)birine yapılan bir çağrı tarafından belirtilerek etkinleştirilir. Örnek uygulama, *HostSettings. JSON*json dosyasını kullanır ve dosyanın konak yapılandırma ayarlarını kullanmak için <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> ' i çağırır.

Konağın [ortam değişkeni yapılandırmasını](xref:fundamentals/configuration/index#environment-variables-configuration-provider) eklemek için konak oluşturucu üzerinde <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> ' i çağırın. `AddEnvironmentVariables`, Kullanıcı tanımlı isteğe bağlı bir ön eki kabul eder. Örnek uygulama `PREFIX_` önekini kullanır. Ortam değişkenleri okurken ön ek kaldırılır. Örnek uygulamanın ana bilgisayarı yapılandırıldığında, `PREFIX_ENVIRONMENT` için ortam değişkeni değeri `environment` anahtarı için ana bilgisayar yapılandırma değeri olur.

[Visual Studio](https://visualstudio.microsoft.com) kullanırken veya `dotnet run` ile bir uygulama çalıştırırken geliştirme sırasında, *Özellikler/launchsettings. JSON* dosyasında ortam değişkenleri ayarlanabilir. [Visual Studio Code](https://code.visualstudio.com/), geliştirme sırasında *. vscode/Launch. JSON* dosyasında ortam değişkenleri ayarlanabilir. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

[Komut satırı yapılandırması](xref:fundamentals/configuration/index#command-line-configuration-provider) <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> çağırarak eklenir. Komut satırı yapılandırması, önceki yapılandırma sağlayıcıları tarafından belirtilen yapılandırmayı geçersiz kılmak için komut satırı bağımsız değişkenlerine izin vermek üzere en son eklenir.

*HostSettings. JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Ek yapılandırma [ApplicationName](#application-key-name) ve [contentroot](#content-root) anahtarlarıyla birlikte etkinleştirilebilir.

Örnek `HostBuilder` <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> kullanılarak yapılandırma:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Uygulama yapılandırması, <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulamasında <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> çağırarak oluşturulur. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, uygulama için bir @no__t 2 oluşturmak için bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> kullanır. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, eklenebilir sonuçlarla birden çok kez çağrılabilir. Uygulama, belirli bir anahtardaki bir değeri en son belirleyen seçeneği kullanır. @No__t-0 tarafından oluşturulan yapılandırma, sonraki işlemler ve <xref:Microsoft.Extensions.Hosting.IHost.Services*> ' de [Hostbuildercontext. Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) ' da kullanılabilir.

Uygulama yapılandırması, [Configurehostconfiguration](#configurehostconfiguration)tarafından belirtilen konak yapılandırmasını otomatik olarak alır.

@No__t-0 kullanarak örnek uygulama yapılandırması:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appSettings. JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Development. JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production. JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

Ayar dosyalarını çıkış dizinine taşımak için, ayarlar dosyalarını proje dosyasında [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) olarak belirtin. Örnek uygulama, JSON uygulama ayarları dosyalarını ve *HostSettings. JSON* dosyasını şu `<Content>` öğesiyle taşıdıkça:

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> @No__t-0 ve <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> gibi yapılandırma uzantısı yöntemleri, [Microsoft. Extensions. Configuration. JSON](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) ve [Microsoft. Extensions. Configuration. EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables)gibi ek NuGet paketleri gerektirir. Uygulama [Microsoft. AspNetCore. app metapackage](xref:fundamentals/metapackage-app)'i kullanmadığı takdirde, bu paketlerin temel [Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) paketine ek olarak projeye eklenmesi gerekir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.

## <a name="configureservices"></a>ConfigureServices

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>, uygulamanın [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısına hizmet ekler. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.

Barındırılan bir hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır. Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.

[Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) , yaşam süresi olayları, `LifetimeEventsHostedService` ve zaman aşımına uğramış bir arka plan görevi olan `TimedHostedService` ' i uygulamaya bir hizmet eklemek için `AddHostedService` genişletme yöntemini kullanır:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>, belirtilen <xref:Microsoft.Extensions.Logging.ILoggingBuilder> ' i yapılandırmak için bir temsilci ekler. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>, CTRL + C/SIGINT veya SIGDÖNEM için dinler ve <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> çağırarak, bilgisayarı kapatmaya başlar. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*>, [RunAsync](#runasync) ve [Waitforshutdownasync](#waitforshutdownasync)gibi uzantıları kaldırır. <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> varsayılan ömür uygulamasıyla önceden kaydedilir. Kaydedilen son yaşam süresi kullanılır.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Kapsayıcı yapılandırması

Diğer kapsayıcıları takmayı desteklemek için, ana bilgisayar <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601> kabul edebilir. Fabrika sağlamak, dı kapsayıcı kaydının bir parçası değildir, ancak bunun yerine somut dı kapsayıcısını oluşturmak için kullanılan bir konak iç sıdır. [Useserviceproviderfactory (ıvıceproviderfactory @ no__t-1TContainerBuilder @ no__t-2)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) , uygulamanın hizmet sağlayıcısını oluşturmak için kullanılan varsayılan fabrikası geçersiz kılar.

Özel kapsayıcı yapılandırması <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> yöntemiyle yönetilir. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>, temel ana bilgisayar API 'sinin en üstünde kapsayıcıyı yapılandırmak için kesin olarak belirlenmiş bir deneyim sağlar. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>, eklenebilir sonuçlarla birden çok kez çağrılabilir.

Uygulama için bir hizmet kapsayıcısı oluşturun:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Hizmet kapsayıcısı fabrikası sağlama:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Fabrika 'yi kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Genişletilebilirlik

Konak genişletilebilirliği <xref:Microsoft.Extensions.Hosting.IHostBuilder> ' da uzantı yöntemleriyle gerçekleştirilir. Aşağıdaki örnek, bir genişletme yönteminin, <xref:fundamentals/host/hosted-services> ' de gösterilen [Timedhostedservice](xref:fundamentals/host/hosted-services#timed-background-tasks) örneği ile <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulamasını nasıl genişlettiğini gösterir.

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

Uygulama, `T` ' e geçirilen barındırılan hizmeti kaydetmek için `UseHostedService` genişletme yöntemi oluşturur:

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a>Konağı yönetme

@No__t-0 uygulaması, hizmet kapsayıcısında kayıtlı <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamalarının başlatılmasının ve durdurulmasından sorumludur.

### <a name="run"></a>Çalıştır

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*>, uygulamayı çalıştırır ve konak kapanana kadar çağıran iş parçacığını engeller:

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a>RunAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*>, uygulamayı çalıştırır ve iptal belirteci veya kapanışı tetiklendiğinde tamamlanmış bir <xref:System.Threading.Tasks.Task> döndürür:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a>RunConsoleAsync

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*>, konsol desteği sağlar, Konağı oluşturur ve başlatır ve CTRL + C/SIGINT veya SIGDÖNEM için bekler.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a>Başlangıç ve StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> Konağı zaman uyumlu olarak başlatır.

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*>, belirtilen zaman aşımı süresi içinde Konağı durdurmaya çalışır.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a>StartAsync ve StopAsync

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, uygulamayı başlatır.

<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> uygulamayı durduruyor.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a>Waitforkapatması

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*>, <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (CTRL + C/SIGINT veya SIGTERIM dinler) gibi <xref:Microsoft.Extensions.Hosting.IHostLifetime> aracılığıyla tetiklenir. <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> ' i çağırır.

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*>, "verilen belirteç aracılığıyla bir işlem tetiklendiğinde, sonra da <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> ' i çağırarak tamamlanan bir <xref:System.Threading.Tasks.Task> döndürür.

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a>Dış denetim

Ana bilgisayarın dış denetimine, dışarıdan çağrılabilen yöntemler kullanılarak ulaşılabilecek:

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> ' in başlangıcında çağrılır. Bu, devam etmeden önce tamamlanana kadar bekler. Bu, bir dış olay tarafından sinyallene kadar başlatmayı geciktirmek için kullanılabilir.

## <a name="ihostingenvironment-interface"></a>Ihostingenvironment arabirimi

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>, uygulamanın barındırma ortamı hakkında bilgi sağlar. Özelliklerini ve uzantı yöntemlerini kullanmak için <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> ' i almak için [Oluşturucu Ekleme](xref:fundamentals/dependency-injection) kullanın:

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

## <a name="iapplicationlifetime-interface"></a>Iapplicationlifetime arabirimi

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime>, düzgün kapanma istekleri dahil olmak üzere başlatma sonrası ve kapanma etkinliklerine izin verir. Arabirimdeki üç özellik, başlangıç ve kapalı olayları tanımlayan <xref:System.Action> yöntemlerini kaydetmek için kullanılan iptal belirteçleridir.

| İptal belirteci | Tetiklendiği zaman&#8230; |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | Konak tam olarak başlatıldı. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | Ana bilgisayar düzgün kapanma işlemini tamamlıyor. Tüm isteklerin işlenmesi gerekir. Bu olay tamamlanana kadar kapalı bloklar. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | Ana bilgisayar düzgün bir şekilde kapanma gerçekleştiriyor. İstekler hala işliyor olabilir. Bu olay tamamlanana kadar kapalı bloklar. |

Constructor-<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> hizmetini herhangi bir sınıfa ekleyin. [Örnek uygulama](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) , olayları kaydetmek için bir `LifetimeEventsHostedService` sınıfına (<xref:Microsoft.Extensions.Hosting.IHostedService> uygulama) ekleme oluşturucusunu kullanır.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*>, uygulamanın sonlandırılmasını ister. Aşağıdaki sınıf, sınıfın `Shutdown` yöntemi çağrıldığında bir uygulamayı düzgün bir şekilde kapatmak için <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kullanır:

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

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/host/hosted-services>
