---
title: .NET genel ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu .NET genel ana bilgisayar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: de9044875c8ebc62c80a129d721e7d37be5d846d
ms.sourcegitcommit: 25150f4398de83132965a89f12d3a030f6cce48d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2018
ms.locfileid: "42927815"
---
# <a name="net-generic-host"></a>.NET genel ana bilgisayar

Tarafından [Luke Latham](https://github.com/guardrex)

.NET core uygulamaları yapılandırmak ve başlatmak bir *konak*. Uygulama başlatma ve ömür yönetimi için konak sorumludur. Bu konu, ASP.NET Core genel ana bilgisayar kapsar ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), HTTP isteklerini mıdl'ye işleme uygulamaları barındırmak için kullanışlı olduğu. İçin Web ana bilgisayarı kapsamını ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), bkz: <xref:fundamentals/host/web-host>.

Amacı genel ana bilgisayar, ana senaryoları daha geniş bir dizi etkinleştirmek için Web ana bilgisayar API'sinden HTTP ardışık düzen ayırmaktır. Mesajlaşma, arka plan görevleri ve diğer yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özellikleri genel ana bilgisayar avantajından temel HTTP olmayan iş yükleri.

Genel ana bilgisayar, ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryoları için uygun değildir. Web barındırma senaryoları için [Web ana bilgisayarı](xref:fundamentals/host/web-host). Gelecek sürümlerden birinde Web ana bilgisayarı değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil konak API olarak davranmak için geliştirme aşamasındaki genel ana bilgisayardır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulamayı çalıştırırken [Visual Studio Code](https://code.visualstudio.com/), kullanan bir *dış veya tümleşik terminal*. Örneği çalıştırma bir `internalConsole`.

Visual Studio Code'da konsol ayarlamak için:

1. Açık *.vscode/launch.json* dosya.
1. İçinde **.NET Core başlatın (konsol)** yapılandırması bulun **konsol** girişi. Değer aşağıdaki seçeneklerden birine ayarlayın `externalTerminal` veya `integratedTerminal`.

## <a name="introduction"></a>Giriş

Genel Host kitaplığı kullanılabilir [Microsoft.Extensions.Hosting ad alanı](/dotnet/api/microsoft.extensions.hosting) ve tarafından sağlanan [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paket. `Microsoft.Extensions.Hosting` Paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri).

[Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) kod yürütme için giriş noktasıdır. Her `IHostedService` uygulama sırasına göre yürütülür [hizmet Createservicereplicalisteners() kaydında](#configureservices). [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) her çağrılır `IHostedService` konak başlatıldığında ve [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) kayıt ters sırada konağın düzgün biçimde kapatıldığında çağrılır.

## <a name="set-up-a-host"></a>Bir ana bilgisayar kümesi

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) kitaplıkları ve uygulamaları, başlatmak, derleme ve konak çalıştırmak için kullandığınız ana bileşeni:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Ana Bilgisayar Yapılandırması

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:

* Yapılandırma oluşturucusu
* Uzantı yöntemi yapılandırması

### <a name="configuration-builder"></a>Yapılandırma oluşturucusu

Ana bilgisayar Oluşturucu yapılandırması çağırılarak oluşturulur [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması. `ConfigureHostConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) konak için. Yapılandırma oluşturucuyu başlatır [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) kullanılmak üzere uygulamanın derleme işlemi.

Ortam değişkeni yapılandırma varsayılan olarak eklenmez. Çağrı [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) ortam değişkenlerini konaktan yapılandırmak için konak oluşturucu üzerinde. `AddEnvironmentVariables` Kullanıcı tanımlı isteğe bağlı bir önekin kabul eder. Bir önek örnek uygulamanın kullandığı `PREFIX_`. Ön ek ortam değişkenlerini okunduğunda kaldırılır. Örnek uygulama ana bilgisayarı, yapılandırılmış, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.

Kullanırken, geliştirme sırasında [Visual Studio](https://www.visualstudio.com/) veya çalışan bir uygulamayla `dotnet run`, ortam değişkenleri ayarlanabilir *Properties/launchSettings.json* dosya. İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri ayarlanabilir *.vscode/launch.json* geliştirme sırasında dosya. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

`ConfigureHostConfiguration` Ek sonuçlar birden çok kez çağrılabilir. Konak, bir değer, en son hangi seçeneği ayarlar kullanır.

*hostsettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Örnek `HostBuilder` yapılandırmayla `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:environment`). `AddConfiguration` Yöntemi bekliyor eşleştirilecek anahtarları `HostBuilder` anahtarları (örneğin, `environment`). Varlığı bölüm adı anahtarlar, ana bilgisayar yapılandırmalarını bölümün değerleri engeller. Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir. Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Uzantı yöntemi yapılandırması

Uzantı yöntemleri üzerinde çağrıldığında `IHostBuilder` içerik kök ve ortamı yapılandırmak için uygulama.

#### <a name="application-key-name"></a>Uygulama anahtarı (ad)

[IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) özelliği konak yapılandırmadan ana bilgisayar oluşturma sırasında ayarlanır. Değeri açıkça ayarlamak için kullanın [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):

**Anahtar**: applicationName  
**Tür**: *dize*  
**Varsayılan**: uygulamanın giriş noktasını içeren derlemenin adı.  
**Kullanılarak ayarlanan**: `HostBuilderContext.HostingEnvironment.ApplicationName`  
**Ortam değişkeni**: `<PREFIX_>APPLICATIONKEY` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a>İçerik kök

Bu ayar, içerik dosyalarını aramaya konak başladığı belirler.

**Anahtar**: contentRoot  
**Tür**: *dize*  
**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasör.  
**Kullanılarak ayarlanan**: `UseContentRoot`  
**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))

Yol mevcut değilse, konak başlatmak başarısız olur.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Ortam

Uygulamanın ayarlar [ortam](xref:fundamentals/environments).

**Anahtar**: ortam  
**Tür**: *dize*  
**Varsayılan**: üretim  
**Kullanılarak ayarlanan**: `UseEnvironment`  
**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` olduğu [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))

Ortamı, herhangi bir değere ayarlanabilir. Çerçeve tarafından tanımlanmış değerler `Development`, `Staging`, ve `Production`. Değerler büyük küçük harfe duyarlı değildir.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Uygulama Oluşturucu yapılandırması çağırılarak oluşturulur [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması. `ConfigureAppConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) uygulama için. `ConfigureAppConfiguration` Ek sonuçlar birden çok kez çağrılabilir. Uygulama, bir değer, en son hangi seçeneği ayarlar kullanır. Tarafından oluşturulan yapılandırmayı `ConfigureAppConfiguration` kullanılabilir [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) sonraki işlemleri ve buna [Hizmetleri](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Örnek uygulama yapılandırmasını kullanma `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appSettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Development.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken uyumlu olmayan [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Yöntemi istenen bölümüne yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:Logging:LogLevel:Default`). `AddConfiguration` Yöntemi bekliyor yapılandırma anahtarları için tam bir eşleşme (örneğin, `Logging:LogLevel:Default`). Bölüm adı anahtarlar varlığını, uygulama yapılandırmalarını bölümün değerleri engeller. Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir. Daha fazla bilgi ve geçici çözümleri görüntüleyin [kullanan tam anahtarları yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme](https://github.com/aspnet/Hosting/issues/839).

Ayarları dosyalar çıkış dizinine taşımak için ayarları dosyaları olarak belirtin. [MSBuild proje öğeleri](/visualstudio/msbuild/common-msbuild-project-items) proje dosyasındaki. Örnek uygulamasını kendi JSON uygulama ayarları dosyaları taşır ve *hostsettings.json* aşağıdaki **&lt;içerik:&gt;** öğesi:

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a>Createservicereplicalisteners()

[Createservicereplicalisteners()](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) Hizmetleri uygulamanın ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. `ConfigureServices` Ek sonuçlar birden çok kez çağrılabilir.

Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır [Ihostedservice](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi. Daha fazla bilgi için bkz. <xref:fundamentals/host/hosted-services>.

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanır `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zamanlanmış arka plan görevi `TimedHostedService`, uygulama için:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) sağlanan yapılandırmak için bir temsilci ekler [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder). `ConfigureLogging` Ek sonuçlar birden çok kez çağrılabilir.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) dinler `Ctrl+C`/SIGINT veya SIGTERM ve çağrıları [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) kapatma işlemi başlatılamadı. `UseConsoleLifetime` Uzantıları gibi engellemesinin kaldırıldığı [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) varsayılan yaşam süresi uygulaması önceden kaydedilir. Kaydedilen son ömrü kullanılır.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Kapsayıcı yapılandırması

Diğer kapsayıcılarda takma desteklemek için konak alabilen bir [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). Fabrika sağlama DI kapsayıcı kaydı bir parçası değildir, ancak bunun yerine somut DI kapsayıcıyı oluşturmak için kullanılan bir iç yöneticisidir. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) uygulamanın hizmet sağlayıcısı oluşturmak için kullanılan varsayılan fabrika geçersiz kılar.

Özel kapsayıcı yapılandırması tarafından yönetilir [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) yöntemi. `ConfigureContainer` kapsayıcısının üstünde API altta yatan ana bilgisayar yapılandırma türü kesin belirlenmiş bir deneyim sağlar. `ConfigureContainer` Ek sonuçlar birden çok kez çağrılabilir.

App service kapsayıcısı oluşturun:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Bir hizmet kapsayıcı üreteci sağlar:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Fabrika kullanın ve uygulama için özel hizmet kapsayıcısını yapılandırın:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Genişletilebilirlik

Konak genişletilebilirliği üzerinde genişletme yöntemleri ile gerçekleştirilir `IHostBuilder`. Aşağıdaki örnek, nasıl bir genişletme yönteminin genişlettiği gösterir. bir `IHostBuilder` uygulamasıyla [RabbitMQ](https://www.rabbitmq.com/). Genişletme yöntemi (uygulamada başka bir yerde) bir RabbitMQ kaydeder `IHostedService`:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Konağı yönetme

[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) uygulamasıdır ve durdurmaktan sorumludur `IHostedService` service kapsayıcısında kayıtlı uygulamalar.

### <a name="run"></a>Çalıştır

[Çalıştırma](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) uygulama çalışır ve ana bilgisayar kapatılana kadar çağıran iş parçacığını engeller:

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

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) uygulama çalışır ve döndüren bir `Task` iptal belirteci veya kapatma tetiklendiğinde tamamlar:

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

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) konsol desteğini etkinleştirir, oluşturur ve konak başlatıldığında ve bekler `Ctrl+C`/SIGINT veya SIGTERM kapatmak için.

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

[Başlangıç](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) konak zaman uyumlu olarak başlar.

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) sağlanan zaman aşımı süresi içinde konağı durdurmaya çalışır.

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

[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) uygulamayı başlatır.

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) uygulamayı durdurur.

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

[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) aracılığıyla tetiklenen [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), gibi [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (dinler `Ctrl+C`/SIGINT veya SIGTERM). `WaitForShutdown` çağrıları [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) döndürür bir `Task` kapatma çağrıları ve verilen belirteci tetiklendiğinde tamamlanan [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) başlangıcında çağırılır [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), devam etmeden önce işlemi tamamlanana kadar bekler. Bu, dış bir olaya göre sinyal kadar başlangıç geciktirmek için kullanılabilir.

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment arabirimi

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) barındırma ortamı uygulama hakkında bilgi sağlar. Kullanma [Oluşturucu ekleme](xref:fundamentals/dependency-injection) edinme `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:

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

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) kapatılmasını istekleri dahil olmak üzere sonrası başlatma ve kapatma etkinlikler için sağlar. Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.

| İptal belirteci | Ne zaman tetiklenir&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Ana bilgisayarın tam olarak başlatılmış. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Konak bir şekilde kapatılmasını tamamlıyor. Tüm isteklerin işlenmesi. Bu olay tamamlanıncaya kadar kapatma engeller. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Konak bir şekilde kapatılmasını gerçekleştiriyor. İstekler hala işleniyor. Bu olay tamamlanıncaya kadar kapatma engeller. |

Oluşturucu Ekle `IApplicationLifetime` herhangi bir sınıf içinde hizmet. [Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uygulamasına Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir `IHostedService` uygulama) olaylarını kaydetmek için.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) sonlandırma uygulamanın ister. Aşağıdaki sınıf kullandığı `StopApplication` düzgün bir şekilde bir uygulamasını kapatmak için sınıfın `Shutdown` yöntemi çağrılır:

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
* [GitHub örnekleri deposu barındırma](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
