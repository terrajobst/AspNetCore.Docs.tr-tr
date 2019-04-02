---
title: .NET genel ana bilgisayar
author: guardrex
description: ASP.NET Core'nın genel ana bilgisayar hakkında uygulama başlatma ve ömür yönetimi için sorumlu olduğu öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/31/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: bb6afe59fcad685d18cdc9c8d90cfcc7b3a6541d
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809201"
---
# <a name="net-generic-host"></a>.NET genel ana bilgisayar

Tarafından [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core uygulamaları yapılandırın ve bir konak başlatın. Uygulama başlatma ve ömür yönetimi için konak sorumludur.

Bu makalede, .NET Core genel ana bilgisayar yer almaktadır (<xref:Microsoft.Extensions.Hosting.HostBuilder>).

Genel konak Web Konaktan farklıdır HTTP ardışık düzen tarafından ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API ayırır. Mesajlaşma, arka plan görevleri ve diğer HTTP olmayan iş yükleri, genel ana bilgisayar kullanın ve yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri yararlanın.

ASP.NET Core 3. 0'dan başlayarak, genel ana bilgisayar hem HTTP hem de HTTP olmayan iş yükleri için önerilir. Bir HTTP sunucusu uygulamasını eklenirse, uygulaması çalışan <xref:Microsoft.Extensions.Hosting.IHostedService>. <xref:Microsoft.Extensions.Hosting.IHostedService> diğer iş yükleri için de kullanılabilir bir arabirimdir.

Web ana bilgisayarı artık web apps için önerilir, ancak geriye dönük uyumluluk için kullanılabilir durumda kalır.

> [!NOTE]
> Bu makalenin kalan kısmı için 3.0 güncelleştirilmedi.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core uygulamaları yapılandırın ve bir konak başlatın. Uygulama başlatma ve ömür yönetimi için konak sorumludur.

Bu makalede, ASP.NET Core genel ana bilgisayar yer almaktadır (<xref:Microsoft.Extensions.Hosting.HostBuilder>), HTTP isteklerini mıdl'ye işleme uygulamaları için kullanılır.

Amacı genel ana bilgisayar, ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API'sinden HTTP ardışık düzen ayırmaktır. Mesajlaşma, arka plan görevleri ve diğer genel ana bilgisayar yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri avantajından temel HTTP olmayan iş yükleri.

Genel ana bilgisayar, ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryoları için uygun değildir. Web barındırma senaryoları için [Web ana bilgisayarı](xref:fundamentals/host/web-host). Genel konak Web ana bilgisayarı gelecekteki bir sürümde değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil konak API işlevi görür.

::: moniker-end

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

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

Ana bilgisayar yapılandırması otomatik olarak uygulama yapılandırmasının akışları ([ConfigureAppConfiguration](#configureappconfiguration) ve uygulamayı geri kalanı).

Yok sağlayıcıları varsayılan olarak dahil edilir. Uygulama gerekiyor, herhangi bir yapılandırma sağlayıcısı açıkça belirtmeniz gerekir <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>de dahil olmak üzere:

* Dosya yapılandırması (örneğin, bir *hostsettings.json* dosyası).
* Ortam değişkeni yapılandırma.
* Komut satırı bağımsız yapılandırma.
* Herhangi diğer gerekli yapılandırma sağlayıcıları.

Ana bilgisayar dosya yapılandırması ile uygulamanın taban yolu belirterek etkin `SetBasePath` birine bir çağrı tarafından izlenen [yapılandırma sağlayıcıları dosya](xref:fundamentals/configuration/index#file-configuration-provider). Bir JSON dosyası örnek uygulamanın kullandığı *hostsettings.json*ve aramalar <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> dosyanın ana bilgisayar yapılandırma ayarlarını kullanmak için.

Eklenecek [ortam değişkeni yapılandırma](xref:fundamentals/configuration/index#environment-variables-configuration-provider) ana bilgisayarının çağrı <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> konak oluşturucu üzerinde. `AddEnvironmentVariables` Kullanıcı tanımlı isteğe bağlı bir önekin kabul eder. Bir önek örnek uygulamanın kullandığı `PREFIX_`. Ön ek ortam değişkenlerini okunduğunda kaldırılır. Örnek uygulama ana bilgisayarı, yapılandırılmış, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.

Kullanırken, geliştirme sırasında [Visual Studio](https://www.visualstudio.com/) veya çalışan bir uygulamayla `dotnet run`, ortam değişkenleri ayarlanabilir *Properties/launchSettings.json* dosya. İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* geliştirme sırasında dosya. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

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

## <a name="configureservices"></a>Createservicereplicalisteners()

<xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> uygulamanın Hizmetleri ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> Ek sonuçlar birden çok kez çağrılabilir.

Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi. Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanır `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zamanlanmış arka plan görevi `TimedHostedService`, uygulama için:

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

Oluşturucu Ekle <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> herhangi bir sınıf içinde hizmet. [Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uygulamasına Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir <xref:Microsoft.Extensions.Hosting.IHostedService> uygulama) olaylarını kaydetmek için.

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

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/host/hosted-services>
