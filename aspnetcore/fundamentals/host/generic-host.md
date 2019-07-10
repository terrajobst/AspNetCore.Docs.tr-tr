---
title: .NET genel ana bilgisayar
author: tdykstra
description: Uygulama başlatma ve ömür yönetimi için sorumlu olduğu .NET Core genel konak hakkında öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/01/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: d787559eaecd6d4d6cfe01e37baf28774a90c5c3
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724420"
---
# <a name="net-generic-host"></a>.NET genel ana bilgisayar

::: moniker range=">= aspnetcore-3.0"

Bu makalede .NET Core genel ana bilgisayar sunar (<xref:Microsoft.Extensions.Hosting.HostBuilder>) nasıl kullanılacağını sağlamaktadır.

## <a name="whats-a-host"></a>Bir konak nedir?

A *konak* gibi bir uygulamanın kaynakları sarmalayan bir nesnedir:

* Bağımlılık ekleme (dı)
* Günlüğe Kaydetme
* Yapılandırma
* `IHostedService` Uygulamaları

Bir ana bilgisayar başlatıldığında çağırdığı `IHostedService.StartAsync` her uygulaması üzerinde <xref:Microsoft.Extensions.Hosting.IHostedService> DI kapsayıcısında bulduğu. Bir web uygulaması, aşağıdakilerden birini `IHostedService` uygulamaları başlatan bir web hizmeti olan bir [HTTP sunucusu uygulamasını](xref:fundamentals/index#servers).

Tüm bağımlı kaynakları uygulamanın bir nesnesinde de dahil olmak üzere ana nedeni ömrü yönetimi,: uygulama başlatma ve normal şekilde kapatılmasını üzerinde denetim.

ASP.NET Core 3.0,'dan önceki sürümlerinde [Web ana bilgisayarı](xref:fundamentals/host/web-host) HTTP iş yükleri için kullanılır. Web ana bilgisayarı artık web apps için önerilir ve yalnızca geriye dönük uyumluluk için kullanılabilir durumda kalır.

## <a name="set-up-a-host"></a>Bir ana bilgisayar kümesi

Ana bilgisayar genellikle, yerleşik ve çalışan kod tarafından yapılandırılan `Program` sınıfı. `Main` Yöntemi:

* Çağrıları bir `CreateHostBuilder` oluşturmak ve bir oluşturucu nesnesini yapılandırmak için yöntemi.
* Çağrıları `Build` ve `Run` Oluşturucu nesnesini yöntemleri.

İşte *Program.cs* tek bir HTTP olmayan bir iş yükü için kod `IHostedService` uygulama DI kapsayıcıya eklendi. 

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

Bir HTTP iş yükü için `Main` yöntemdir aynı ancak `CreateHostBuilder` çağrıları `ConfigureWebHostDefaults`:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

Uygulama, Entity Framework Core kullanıyorsa, ad veya imzası değişmez `CreateHostBuilder` yöntemi. [Entity Framework Core Araçları](/ef/core/miscellaneous/cli/) bulmayı beklediğinize bir `CreateHostBuilder` uygulamayı başlatmadan konak yapılandırır yöntemi. Daha fazla bilgi için [tasarım zamanında DbContext oluşturma](/ef/core/miscellaneous/cli/dbcontext-creation).

## <a name="default-builder-settings"></a>Varsayılan Oluşturucu ayarları 

<xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> Yöntemi:

* İçerik kök tarafından döndürülen yol ayarlar <xref:System.IO.Directory.GetCurrentDirectory*>.
* Yükleri yapılandırmasından barındırın:
  * Ortam değişkenleri "İle DOTNET_" öneki.
  * Komut satırı bağımsız değişkenleri.
* Yükleri uygulama yapılandırmasından:
  * *appsettings.json*.
  * *appSettings. {Ortamı} .json*.
  * [Gizli dizi Yöneticisi](xref:security/app-secrets) uygulamayı çalıştırdığında `Development` ortam.
  * Ortam değişkenleri.
  * Komut satırı bağımsız değişkenleri.
* Aşağıdaki ekler [günlüğü](xref:fundamentals/logging/index) sağlayıcıları:
  * Konsol
  * Hata ayıklama
  * EventSource
  * (Yalnızca Windows üzerinde çalışırken) olay günlüğü
* Sağlar [kapsam doğrulama](xref:fundamentals/dependency-injection#scope-validation) ve [bağımlılık doğrulaması](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) geliştirme ortamı olduğunda.

`ConfigureWebHostDefaults` Yöntemi:

* Yükler, ortam değişkenleri "İle ASPNETCORE_" önekli yapılandırmasından barındırın.
* Kümeleri [Kestrel](xref:fundamentals/servers/kestrel) web sunucusu olarak ve uygulamanın barındırma yapılandırma sağlayıcıları kullanarak yapılandırır. Kestrel'i sunucunun varsayılan seçenekleri için bkz <xref:fundamentals/servers/kestrel#kestrel-options>.
* Ekler [konak filtreleme ara yazılım](xref:fundamentals/servers/kestrel#host-filtering).
* Ekler [iletilen üstbilgileri ara yazılım](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) varsa ASPNETCORE_FORWARDEDHEADERS_ENABLED = true.
* IIS tümleştirme sağlar. IIS varsayılan seçenekleri için bkz <xref:host-and-deploy/iis/index#iis-options>.

[Tüm uygulama türleri için ayarları](#settings-for-all-app-types) ve [web apps için ayarlar](#settings-for-web-apps) bu makalenin sonraki bölümlerine varsayılan oluşturucu ayarlarının nasıl geçersiz kılınacağını gösterir.

## <a name="framework-provided-services"></a>Framework tarafından sağlanan hizmetleri

Otomatik olarak kaydedilen hizmetleri şunlardır:

* [IHostApplicationLifetime](#ihostapplicationlifetime)
* [IHostLifetime](#ihostlifetime)
* [IHostEnvironment / IWebHostEnvironment](#ihostenvironment)

Framework tarafından sağlanan tüm hizmetlerin listesi için bkz. <xref:fundamentals/dependency-injection#framework-provided-services>.

## <a name="ihostapplicationlifetime"></a>IHostApplicationLifetime

Ekleme <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (eski adıyla `IApplicationLifetime`) hizmet oturum kapatma sonrası başlatma ve normal görevlerini işlemek için herhangi bir sınıf. Arabirim üç özellikleri, uygulama başlangıç ve uygulama durdurma olay işleyicisi yöntemleri kaydetmek için kullanılan iptal belirteçleridir. Ayrıca arabirimin içerdiği bir `StopApplication` yöntemi.

Aşağıdaki örnek, bir `IHostedService` kaydeder uygulama `IApplicationLifetime` olayları:

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a>IHostLifetime

<xref:Microsoft.Extensions.Hosting.IHostLifetime> Uygulama, konak başlatıldığında ve ne zaman durdurur denetler. Kaydedilen son uygulaması kullanılır.

<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> Varsayılan değer `IHostLifetime` uygulaması. `ConsoleLifetime`:

* CTRL + C/SIGINT veya SIGTERM ve aramalar için dinler <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kapatma işlemi başlatılamadı.
* Uzantıları gibi engellemesinin kaldırıldığı [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync).

## <a name="ihostenvironment"></a>IHostEnvironment

Ekleme <xref:Microsoft.Extensions.Hosting.IHostEnvironment> hizmet aşağıdakiler hakkında bilgi almak için bir sınıf içinde:

* [ApplicationName](#applicationname)
* [EnvironmentName](#environmentname)
* [ContentRootPath](#contentrootpath)

Web apps uygulama `IWebHostEnvironment` devralan bir arayüzü `IHostEnvironment` ve ekler:

* [WebRootPath](#webroot)

## <a name="host-configuration"></a>Ana Bilgisayar Yapılandırması

Ana bilgisayar yapılandırma özellikleri için kullanılan <xref:Microsoft.Extensions.Hosting.IHostEnvironment> uygulaması.

Ana bilgisayar yapılandırması kullanılabilir [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) içinde <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>. Sonra `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` uygulama yapılandırması ile değiştirilir.

Ana bilgisayar yapılandırması eklemek için arama <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> üzerinde `IHostBuilder`. `ConfigureHostConfiguration` Ek sonuçlar birden çok kez çağrılabilir. Ana bilgisayarın hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.

Ortam değişkeni sağlayıcı önekiyle `DOTNET_` ve komut satırı değişkenleri tarafından CreateDefaultBuilder dahil edilir. Web uygulamaları, ortam değişkeni sağlayıcı önekiyle `ASPNETCORE_` eklenir. Ön ek ortam değişkenlerini okunduğunda kaldırılır. Örneğin, ortam değişken değeri `ASPNETCORE_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.

Aşağıdaki örnek, ana bilgisayar yapılandırması oluşturur:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a>Uygulama yapılandırması

Uygulama yapılandırması çağırılarak oluşturulur <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> üzerinde `IHostBuilder`. `ConfigureAppConfiguration` Ek sonuçlar birden çok kez çağrılabilir. Uygulama, hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır. 

Tarafından oluşturulan yapılandırmayı `ConfigureAppConfiguration` kullanılabilir [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) sonraki işlemleri ve hizmet olarak sunduğu DI. Ana bilgisayar yapılandırması için uygulama yapılandırma de eklenir.

Daha fazla bilgi için [ASP.NET Core yapılandırmasında](xref:fundamentals/configuration/index#configureappconfiguration).

## <a name="settings-for-all-app-types"></a>Tüm uygulama türleri için ayarları

Bu bölümde, hem HTTP hem de HTTP olmayan iş yükleri için geçerli bir ana bilgisayar ayarları listelenir. Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerini olabilir bir `DOTNET_` veya `ASPNETCORE_` önek.

### <a name="applicationname"></a>ApplicationName

[IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) özelliği konak yapılandırmadan ana bilgisayar oluşturma sırasında ayarlanır.

**Anahtar**: applicationName  
**Tür**: *dize*  
**Varsayılan**: Uygulamanın giriş içeren bütünleştirilmiş kodun ad'ı seçin.
**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME`

Bu değeri ayarlamak için ortam değişkenini kullanın. 

### <a name="contentrootpath"></a>ContentRootPath

[IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) özellik, ana içerik dosyalarını aramaya başladığı belirler. Yol mevcut değilse, konak başlatmak başarısız olur.

**Anahtar**: contentRoot  
**Tür**: *dize*  
**Varsayılan**: Uygulama derleme bulunduğu klasör.  
**Ortam değişkeni**: `<PREFIX_>CONTENTROOT`

Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseContentRoot` üzerinde `IHostBuilder`:

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

### <a name="environmentname"></a>EnvironmentName

[IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) özelliği herhangi bir değere ayarlanabilir. Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`. Değerler büyük küçük harfe duyarlı değildir.

**Anahtar**: ortam  
**Tür**: *dize*  
**Varsayılan**: Üretim  
**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT`

Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseEnvironment` üzerinde `IHostBuilder`:

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a>ShutdownTimeout

[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) zaman aşımı'için ayarlar <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>. Varsayılan değer beş saniyedir.  Zaman aşımı dönemi sırasında konak:

* Tetikleyiciler [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).
* Barındırılan hizmetler, durdurmak için başarısız olan hizmetlerin hatalarını günlüğe kaydetme durdurmaya çalışır.

Tüm barındırılan hizmetlerin Durdur önce zaman aşımı süresi dolarsa, uygulama kapatıldığında tüm kalan etkin hizmet durdurulur. İşlem tamamlandı olmasanız bile hizmetleri durdurun. Hizmetlerini durdurmak için ek süre gerektiriyorsa, zaman aşımını artırın.

**Anahtar**: shutdownTimeoutSeconds  
**Tür**: *int*  
**Varsayılan**: 5 saniye **ortam değişkeni**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`

Bu değeri ayarlamak için ortam değişkenini kullanmak veya yapılandırma `HostOptions`. Aşağıdaki örnekte, 20 saniye olarak zaman aşımı ayarlar:

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a>Web apps için ayarlar

Bazı konak ayarları, yalnızca HTTP iş yükleri için geçerlidir. Varsayılan olarak, bu ayarları yapılandırmak için kullanılan ortam değişkenlerini olabilir bir `DOTNET_` veya `ASPNETCORE_` önek.

Uzantı yöntemleri `IWebHostBuilder` bu ayarlar için kullanılabilir. Genişletme yöntemleri çağırmak nasıl gösteren kod örnekleri varsayar `webBuilder` örneğidir `IWebHostBuilder`, aşağıdaki örnekte olduğu gibi:

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

Zaman `false`, çıkma konak başlatma sonucunda sırasında hatalar. Zaman `true`, konak başlatma sırasında özel durumları yakalar ve sunucu başlatmayı dener.

**Anahtar**: captureStartupErrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: Varsayılan olarak `false` IIS, varsayılan olduğu arkasında Kestrel ile uygulamanın çalıştığı sürece `true`.  
**Ortam değişkeni**: `<PREFIX_>CAPTURESTARTUPERRORS`

Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `CaptureStartupErrors`:

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a>DetailedErrors

Etkin olduğunda ya da ortam `Development`, uygulamanın ayrıntılı hataları yakalar.

**Anahtar**: detailedErrors  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
**Ortam değişkeni**: `<PREFIX_>_DETAILEDERRORS`

Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `UseSetting`:

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a>HostingStartupAssemblies

Başlangıçta yüklenecek başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi. Yapılandırma değeri boş dize olarak varsayılan olarak olsa da, barındırma başlangıç derlemeler her zaman uygulamanın bütünleştirilmiş kod ekleyin. Barındırma başlangıç derlemeler sağlandığında, uygulama başlatma sırasında yaygın hizmetlerinin oluşturduğunda yüklenmesi için uygulamanın derlemeye eklenir.

**Anahtar**: hostingStartupAssemblies  
**Tür**: *dize*  
**Varsayılan**: Boş dize  
**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`

Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `UseSetting`:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a>HostingStartupExcludeAssemblies

Başlangıçta hariç tutmak için başlangıç derlemeleri barındırma bir noktalı virgülle ayrılmış dizesi.

**Anahtar**: hostingStartupExcludeAssemblies  
**Tür**: *dize*  
**Varsayılan**: Boş dize  
**Ortam değişkeni**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `UseSetting`:

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="httpsport"></a>HTTPS_Port

HTTPS bağlantı noktasına yönlendirir. Kullanılan [HTTPS zorlama](xref:security/enforcing-ssl).

**Anahtar**: https_port **türü**: *dize*
**varsayılan**: Varsayılan bir değer ayarlanmamış.
**Ortam değişkeni**: `<PREFIX_>HTTPS_PORT`

Bu değeri ayarlamak için yapılandırma veya çağrı kullanın `UseSetting`:

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a>PreferHostingUrls

Ana bilgisayarı, yapılandırılmış URL'leri dinleyecek olup olmadığını gösteren `IWebHostBuilder` ile yapılandırılan yerine `IServer` uygulaması.

**Anahtar**: preferHostingUrls  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: true  
**Ortam değişkeni**: `<PREFIX_>_PREFERHOSTINGURLS`

Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `PreferHostingUrls`:

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a>PreventHostingStartup

Başlangıç derlemeler, uygulamanın derleme tarafından yapılandırılan başlangıç derlemeleri barındırma da dahil olmak üzere barındırma otomatik yüklenmesini engeller. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

**Anahtar**: preventHostingStartup  
**Tür**: *bool* (`true` veya `1`)  
**Varsayılan**: false  
**Ortam değişkeni**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`

Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseSetting` :

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a>StartupAssembly

Aramak için derleme `Startup` sınıfı.

**Anahtar**: startupAssembly **türü**: *dize*  
**Varsayılan**: Uygulamanın derleme  
**Ortam değişkeni**: `<PREFIX_>STARTUPASSEMBLY`

Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseStartup`. `UseStartup` bir derleme adı alabilir (`string`) veya bir tür (`TStartup`). Birden çok `UseStartup` yöntemleri çağrıldığında, sonuncu önceliklidir.

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a>URL'ler

IP adresleri veya bağlantı noktası ve sunucu üzerinde istekleri dinlemesi gereken protokolleri konak adresleri noktalı virgülle ayrılmış listesi. Örneğin: `http://localhost:123` Kullanım "\*" sunucu herhangi bir IP adresi veya ana bilgisayar adı belirtilen bağlantı noktası ve protokol kullanılarak isteklerini dinlemesi gerektiğini belirtmek için (örneğin, `http://*:5000`). Protokol (`http://` veya `https://`) her URL ile dahil edilmelidir. Desteklenen biçimler sunucular arasında farklılık gösterir.

**Anahtar**: URL'ler  
**Tür**: *dize*  
**Varsayılan**: `http://localhost:5000` ve `https://localhost:5001` 
 **ortam değişkeni**: `<PREFIX_>URLS`

Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseUrls`:

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

Kestrel'i kendi API uç nokta yapılandırması vardır. Daha fazla bilgi için bkz. <xref:fundamentals/servers/kestrel#endpoint-configuration>.

### <a name="webroot"></a>WebRoot

Uygulamanın statik varlıklar göreli yolu.

**Anahtar**: webroot  
**Tür**: *dize*  
**Varsayılan**: *(Kök içerik) / wwwroot*, yol varsa. Yol mevcut değilse İşlemsiz dosya sağlayıcısı kullanılır.  
**Ortam değişkeni**: `<PREFIX_>WEBROOT`

Bu değeri ayarlamak için ortam değişkeni veya çağrı kullanın `UseWebRoot`:

```csharp
webBuilder.UseWebRoot("public");
```

## <a name="manage-the-host-lifetime"></a>Konak yaşam süresini yönetme

Yerleşik üzerinde yöntemleri çağırmak <xref:Microsoft.Extensions.Hosting.IHost> uygulamasını başlatın ve uygulamayı durdurun. Bu yöntemler tüm etkileyen <xref:Microsoft.Extensions.Hosting.IHostedService> service kapsayıcısında kayıtlı uygulamalar.

### <a name="run"></a>Çalıştır

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uygulama çalışır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller.

### <a name="runasync"></a>RunAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uygulama çalışır ve döndüren bir <xref:System.Threading.Tasks.Task> iptal belirteci veya kapatma tetiklendiğinde tamamlar.

### <a name="runconsoleasync"></a>RunConsoleAsync

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Konsol desteğini etkinleştirir, oluşturur ve konak başlatıldığında ve kapatmak Ctrl + C/SIGINT veya SIGTERM bekler.

### <a name="start"></a>Başlat

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> ana bilgisayar zaman uyumlu olarak başlar.

### <a name="startasync"></a>StartAsync

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> konak başlatıldığında ve döndüren bir <xref:System.Threading.Tasks.Task> iptal belirteci veya kapatma tetiklendiğinde tamamlar. 

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> başlangıcında çağırılır `StartAsync`, devam etmeden önce işlemi tamamlanana kadar bekler. Bu, dış bir olaya göre sinyal kadar başlangıç geciktirmek için kullanılabilir.

### <a name="stopasync"></a>StopAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> Belirtilen zaman aşımı süresi içinde konağı durdurmaya çalışır.

### <a name="waitforshutdown"></a>WaitForShutdown

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> kapatma IHostLifetime tarafından gibi Ctrl + C/SIGINT veya SIGTERM aracılığıyla tetiklenen kadar çağıran iş parçacığını engeller.

### <a name="waitforshutdownasync"></a>WaitForShutdownAsync

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> döndürür bir <xref:System.Threading.Tasks.Task> kapatma çağrıları ve verilen belirteci tetiklendiğinde tamamlanan <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

### <a name="external-control"></a>Dış denetimi

Konak ömrü doğrudan denetimi harici olarak çağrılabilir yöntemleri kullanılarak gerçekleştirilebilir:

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

ASP.NET Core uygulamaları yapılandırın ve bir konak başlatın. Uygulama başlatma ve ömür yönetimi için konak sorumludur.

Bu makalede, ASP.NET Core genel ana bilgisayar yer almaktadır (<xref:Microsoft.Extensions.Hosting.HostBuilder>), HTTP isteklerini mıdl'ye işleme uygulamaları için kullanılır.

Amacı genel ana bilgisayar, ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API'sinden HTTP ardışık düzen ayırmaktır. Mesajlaşma, arka plan görevleri ve diğer genel ana bilgisayar yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri avantajından temel HTTP olmayan iş yükleri.

Genel ana bilgisayar, ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryoları için uygun değildir. Web barındırma senaryoları için [Web ana bilgisayarı](xref:fundamentals/host/web-host). Genel konak Web ana bilgisayarı gelecekteki bir sürümde değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil konak API işlevi görür.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulamayı çalıştırırken [Visual Studio Code](https://code.visualstudio.com/), kullanan bir *dış veya tümleşik terminal*. Örneği çalıştırma bir `internalConsole`.

Visual Studio Code'da konsol ayarlamak için:

1. Açık *.vscode/launch.json* dosya.
1. İçinde **.NET Core başlatın (konsol)** yapılandırması bulun **konsol** girişi. Değer aşağıdaki seçeneklerden birine ayarlayın `externalTerminal` veya `integratedTerminal`.

## <a name="introduction"></a>Giriş

Genel Host kitaplığı kullanılabilir <xref:Microsoft.Extensions.Hosting> ad alanı tarafından sağlanan ve [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paket. `Microsoft.Extensions.Hosting` Paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).

<xref:Microsoft.Extensions.Hosting.IHostedService> kod yürütme için giriş noktasıdır. Her <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama sırasına göre yürütülür [hizmet Createservicereplicalisteners() kaydında](#configureservices). <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> Her çağrılır <xref:Microsoft.Extensions.Hosting.IHostedService> konak başlatıldığında ve <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> kayıt ters sırada konağın düzgün biçimde kapatıldığında çağrılır.

## <a name="set-up-a-host"></a>Bir ana bilgisayar kümesi

<xref:Microsoft.Extensions.Hosting.IHostBuilder> kitaplıkları ve uygulamaları, başlatmak, derleme ve konak çalıştırmak için kullandığınız ana bileşen şöyledir:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a>Seçenekler

<xref:Microsoft.Extensions.Hosting.HostOptions> seçeneklerini yapılandırmak <xref:Microsoft.Extensions.Hosting.IHost>.

### <a name="shutdown-timeout"></a>Kapatma zaman aşımı

<xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> zaman aşımı'için ayarlar <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>. Varsayılan değer beş saniyedir.

Aşağıdaki seçeneği yapılandırmada `Program.Main` varsayılan beş ikinci kapatma zaman aşımı süresi 20 saniye artırır:

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
* [Seçenekleri](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)
* [Günlüğe kaydetme](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)

## <a name="host-configuration"></a>Ana Bilgisayar Yapılandırması

Ana Bilgisayar Yapılandırması tarafından oluşturulur:

* Uzantı metotları çağırma <xref:Microsoft.Extensions.Hosting.IHostBuilder> ayarlanacak [içerik kök](#content-root) ve [ortam](#environment).
* Yapılandırma sağlayıcıları okuma yapılandırmasından <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.

### <a name="extension-methods"></a>Genişletme yöntemleri

### <a name="application-key-name"></a>Uygulama anahtarı (ad)

[IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) özelliği konak yapılandırmadan ana bilgisayar oluşturma sırasında ayarlanır. Değeri açıkça ayarlamak için kullanın [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):

**Anahtar**: applicationName  
**Tür**: *dize*  
**Varsayılan**: Uygulamanın giriş içeren bütünleştirilmiş kodun ad'ı seçin.  
**Kullanılarak ayarlanan**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Ortam değişkeni**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))

### <a name="content-root"></a>İçerik kök

Bu ayar, içerik dosyalarını aramaya konak başladığı belirler.

**Anahtar**: contentRoot  
**Tür**: *dize*  
**Varsayılan**: Uygulama derleme bulunduğu klasör varsayılan olur.  
**Kullanılarak ayarlanan**: `UseContentRoot`  
**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))

Yol mevcut değilse, konak başlatmak başarısız olur.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

### <a name="environment"></a>Ortam

Uygulamanın ayarlar [ortam](xref:fundamentals/environments).

**Anahtar**: ortam  
**Tür**: *dize*  
**Varsayılan**: Üretim  
**Kullanılarak ayarlanan**: `UseEnvironment`  
**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configurehostconfiguration))

Ortamı, herhangi bir değere ayarlanabilir. Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`. Değerler büyük küçük harfe duyarlı değildir.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a>ConfigureHostConfiguration

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> kullanan bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> oluşturmak için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> konak için. Konak yapılandırmasının başlatmak için kullanılan <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> kullanılmak üzere uygulamanın derleme işlemi.

<xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> Ek sonuçlar birden çok kez çağrılabilir. Ana bilgisayarın hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır.

Yok sağlayıcıları varsayılan olarak dahil edilir. Uygulama gerekiyor, herhangi bir yapılandırma sağlayıcısı açıkça belirtmeniz gerekir <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>de dahil olmak üzere:

* Dosya yapılandırması (örneğin, bir *hostsettings.json* dosyası).
* Ortam değişkeni yapılandırma.
* Komut satırı bağımsız yapılandırma.
* Herhangi diğer gerekli yapılandırma sağlayıcıları.

Ana bilgisayar dosya yapılandırması ile uygulamanın taban yolu belirterek etkin `SetBasePath` birine bir çağrı tarafından izlenen [yapılandırma sağlayıcıları dosya](xref:fundamentals/configuration/index#file-configuration-provider). Bir JSON dosyası örnek uygulamanın kullandığı *hostsettings.json*ve aramalar <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> dosyanın ana bilgisayar yapılandırma ayarlarını kullanmak için.

Eklenecek [ortam değişkeni yapılandırma](xref:fundamentals/configuration/index#environment-variables-configuration-provider) ana bilgisayarının çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> konak oluşturucu üzerinde. `AddEnvironmentVariables` Kullanıcı tanımlı isteğe bağlı bir önekin kabul eder. Bir önek örnek uygulamanın kullandığı `PREFIX_`. Ön ek ortam değişkenlerini okunduğunda kaldırılır. Örnek uygulama ana bilgisayarı, yapılandırılmış, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.

Kullanırken, geliştirme sırasında [Visual Studio](https://visualstudio.microsoft.com) veya çalışan bir uygulamayla `dotnet run`, ortam değişkenleri ayarlanabilir *Properties/launchSettings.json* dosya. İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* geliştirme sırasında dosya. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

[Komut satırı yapılandırma](xref:fundamentals/configuration/index#command-line-configuration-provider) çağrılarak eklenir <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>. Komut satırı yapılandırma önceki yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için komut satırı bağımsız değişkenlerine izin vermek için son eklenir.

*hostsettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Ek yapılandırma ile sağlanabilir [applicationName](#application-key-name) ve [contentRoot](#content-root) anahtarları.

Örnek `HostBuilder` yapılandırmayla <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Uygulama yapılandırması çağırılarak oluşturulur <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> üzerinde <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulaması. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> kullanan bir <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> oluşturmak için bir <xref:Microsoft.Extensions.Configuration.IConfiguration> uygulama için. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> Ek sonuçlar birden çok kez çağrılabilir. Uygulama, hangi seçeneği bir değer son belirli bir anahtar ayarlar kullanır. Tarafından oluşturulan yapılandırmayı <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> kullanılabilir [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) sonraki işlemleri ve buna <xref:Microsoft.Extensions.Hosting.IHost.Services*>.

Uygulama Yapılandırması tarafından sağlanan ana bilgisayar yapılandırması otomatik olarak aldığı [ConfigureHostConfiguration](#configurehostconfiguration).

Örnek uygulama yapılandırmasını kullanma <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appsettings.json*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Development.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

Ayarları dosyalar çıkış dizinine taşımak için ayarları dosyaları olarak belirtin. [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) proje dosyasındaki. Örnek uygulamasını kendi JSON uygulama ayarları dosyaları taşır ve *hostsettings.json* aşağıdaki `<Content>` öğesi:

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> Yapılandırma genişletme yöntemleri gibi <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> ve <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> ek NuGet paketleri gibi gereken [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) ve [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables). Uygulamanın kullandığı sürece [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), bu paketleri çekirdek ek olarak projeye eklenmelidir [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) paket. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.

## <a name="configureservices"></a>Createservicereplicalisteners()

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> uygulamanın Hizmetleri ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> Ek sonuçlar birden çok kez çağrılabilir.

Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi. Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.

[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanır `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zamanlanmış arka plan görevi `TimedHostedService`, uygulama için:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> sağlanan yapılandırmak için bir temsilci ekler <xref:Microsoft.Extensions.Logging.ILoggingBuilder>. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> Ek sonuçlar birden çok kez çağrılabilir.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> CTRL + C/SIGINT veya SIGTERM ve aramalar için dinler <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> kapatma işlemi başlatılamadı. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> Uzantıları gibi engellemesinin kaldırıldığı [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync). <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> Varsayılan yaşam süresi uygulaması önceden kaydedilir. Kaydedilen son ömrü kullanılır.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Kapsayıcı yapılandırması

Diğer kapsayıcılarda takma desteklemek için konak alabilen bir <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>. Fabrika sağlama DI kapsayıcı kaydı bir parçası değildir, ancak bunun yerine somut DI kapsayıcıyı oluşturmak için kullanılan bir iç yöneticisidir. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) uygulamanın hizmet sağlayıcısı oluşturmak için kullanılan varsayılan fabrika geçersiz kılar.

Özel kapsayıcı yapılandırması tarafından yönetilir <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> yöntemi. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> kapsayıcısının üstünde API altta yatan ana bilgisayar yapılandırma türü kesin belirlenmiş bir deneyim sağlar. <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> Ek sonuçlar birden çok kez çağrılabilir.

App service kapsayıcısı oluşturun:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Bir hizmet kapsayıcı üreteci sağlar:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Fabrika kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Genişletilebilirlik

Konak genişletilebilirliği üzerinde genişletme yöntemleri ile gerçekleştirilir <xref:Microsoft.Extensions.Hosting.IHostBuilder>. Aşağıdaki örnek, nasıl bir genişletme yönteminin genişlettiği gösterir. bir <xref:Microsoft.Extensions.Hosting.IHostBuilder> uygulamasıyla [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) gösterilen örnek içinde <xref:fundamentals/host/hosted-services>.

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

Bir uygulama oluşturur `UseHostedService` genişletme yöntemi, barındırılan hizmeti kaydetmek için geçirilen `T`:

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

<xref:Microsoft.Extensions.Hosting.IHost> Uygulamasıdır ve durdurmaktan sorumludur <xref:Microsoft.Extensions.Hosting.IHostedService> service kapsayıcısında kayıtlı uygulamalar.

### <a name="run"></a>Çalıştır

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> Uygulama çalışır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> Uygulama çalışır ve döndüren bir <xref:System.Threading.Tasks.Task> iptal belirteci veya kapatma tetiklendiğinde tamamlar:

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

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> Konsol desteğini etkinleştirir, oluşturur ve konak başlatıldığında ve kapatmak Ctrl + C/SIGINT veya SIGTERM bekler.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> ana bilgisayar zaman uyumlu olarak başlar.

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> Belirtilen zaman aşımı süresi içinde konağı durdurmaya çalışır.

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

<xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> uygulamayı başlatır.

<xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> uygulamayı durdurur.

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

### <a name="waitforshutdown"></a>WaitForShutdown

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> aracılığıyla tetiklenen <xref:Microsoft.Extensions.Hosting.IHostLifetime>, gibi <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (Ctrl + C/SIGINT veya SIGTERM dinler). <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> çağrıları <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

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

<xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> döndürür bir <xref:System.Threading.Tasks.Task> kapatma çağrıları ve verilen belirteci tetiklendiğinde tamamlanan <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.

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

### <a name="external-control"></a>Dış denetimi

Dış denetim konağının harici olarak çağrılabilir yöntemleri kullanılarak elde edilebilir:

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

<xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> başlangıcında çağırılır <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, devam etmeden önce işlemi tamamlanana kadar bekler. Bu, dış bir olaya göre sinyal kadar başlangıç geciktirmek için kullanılabilir.

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment arabirimi

<xref:Microsoft.Extensions.Hosting.IHostingEnvironment> uygulama hakkındaki bilgileri barındırma ortamı sağlar. Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> genişletme yöntemleri ve özellikleri kullanmak için:

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

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime arabirimi

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime> normal şekilde kapatılmasını istekleri dahil olmak üzere sonrası başlatma ve kapatma etkinlikler için sağlar. Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini <xref:System.Action> başlatma ve kapatma olayları tanımlayan yöntemleri.

| İptal belirteci | Ne zaman tetiklenir&#8230; |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | Ana bilgisayarın tam olarak başlatılmış. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | Konak bir şekilde kapatılmasını tamamlıyor. Tüm isteklerin işlenmesi. Bu olay tamamlanıncaya kadar kapatma engeller. |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | Konak bir şekilde kapatılmasını gerçekleştiriyor. İstekler hala işleniyor. Bu olay tamamlanıncaya kadar kapatma engeller. |

Oluşturucu Ekle <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> herhangi bir sınıf içinde hizmet. [Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uygulamasına Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama) olaylarını kaydetmek için.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> uygulamanın sonlandırma ister. Aşağıdaki sınıf kullandığı <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:

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
