---
title: .NET genel ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür boyu yönetimi için sorumlu olduğu .NET genel ana bilgisayar hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 40d297257895a4defeb89cef9c5ec6deea64a985
ms.sourcegitcommit: 7003d27b607e529642ded0400aa48ae692a0e666
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37033361"
---
# <a name="net-generic-host"></a>.NET genel ana bilgisayar

Tarafından [Luke Latham](https://github.com/guardrex)

.NET uygulamaları yapılandırmak ve başlatma bir *konak*. Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur. Bu konu, ASP.NET Core genel ana bilgisayar içerir ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), HTTP isteklerini işleyen olmayan uygulamaları barındırmak için yararlı olan. Web barındırma kapsamını için ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), bkz: [Web ana bilgisayarı](xref:fundamentals/host/web-host) konu.

Genel ana bilgisayar daha geniş bir dizi ana senaryoları etkinleştirmek için Web ana bilgisayar API HTTP ardışık düzen aynı şekilde hedefidir. Mesajlaşma, arka plan görevleri ve yapılandırma, bağımlılık ekleme (dı) ve günlüğe kaydetme gibi çapraz kesme özelliklerden genel ana avantajı göre diğer HTTP olmayan iş yükleri.

Genel ana bilgisayar ASP.NET Core 2.1 içinde yenidir ve web barındırma senaryolarında için uygun değil. Barındırma senaryolarında Web'deki kullanan [Web ana bilgisayarı](xref:fundamentals/host/web-host). Web ana bilgisayarı gelecek bir sürümde değiştirin ve hem HTTP hem de HTTP olmayan senaryolar birincil ana makinesi API olarak davranmak geliştirilme genel bir ana bilgisayardır.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulama çalışırken [Visual Studio Code](https://code.visualstudio.com/), kullanan bir *dış veya tümleşik terminal*. Örneği çalıştırma bir `internalConsole`.

Visual Studio kodda konsol ayarlamak için:

1. Açık *.vscode/launch.json* dosya.
1. İçinde **.NET Core başlatın (konsol)** yapılandırması bulun **konsol** girişi. Ya da değeri `externalTerminal` veya `integratedTerminal`.

## <a name="introduction"></a>Giriş

Genel ana bilgisayar kitaplığı kullanılabilir [Microsoft.Extensions.Hosting ad alanı](/dotnet/api/microsoft.extensions.hosting) ve tarafından sağlanan [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) paket. `Microsoft.Extensions.Hosting` Paket dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya sonrası).

[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) kod yürütülmesine giriş noktasıdır. Her `IHostedService` uygulama sırasına göre yürütüldüğünde [hizmet ConfigureServices kaydında](#configureservices). [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) her adlı `IHostedService` ana bilgisayar başlatıldığında ve [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) ters kayıt sırayla konak düzgün biçimde kapatıldığında çağrılır.

## <a name="set-up-a-host"></a>Bir ana bilgisayar kümesi

[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) kitaplıkları ve uygulamaları başlatmak, yapı ve ana bilgisayar çalıştırmak için kullandığınız ana bileşeni:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a>Ana Bilgisayar Yapılandırması

[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) ana bilgisayar yapılandırma değerlerini ayarlamak için aşağıdaki yaklaşımlardan kullanır:

* Yapılandırma oluşturucusu
* Uzantı yöntemi yapılandırması

### <a name="configuration-builder"></a>Yapılandırma oluşturucusu

Ana bilgisayar Oluşturucu yapılandırması çağırarak oluşturulur [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması. `ConfigureHostConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) ana bilgisayar için. Yapılandırma Oluşturucu başlatır [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) uygulamanın derleme işleminde kullanmak için.

Ortam değişkeni yapılandırma, varsayılan olarak eklenmez. Çağrı [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) ortam değişkenleri konaktan yapılandırmak için konak oluşturucu üzerinde. `AddEnvironmentVariables` İsteğe bağlı kullanıcı tanımlı önek kabul eder. Bir önek örnek uygulamanın kullandığı `PREFIX_`. Ortam değişkenleri okurken öneki kaldırılır. Örnek uygulamanın ana zaman yapılandırılmışsa, ortam değişken değeri `PREFIX_ENVIRONMENT` için ana bilgisayar yapılandırma değeri olur `environment` anahtarı.

Kullanırken geliştirme sırasında [Visual Studio](https://www.visualstudio.com/) veya bir uygulamayla çalıştıran `dotnet run`, ortam değişkenleri kümesinde *Properties/launchSettings.json* dosya. İçinde [Visual Studio Code](https://code.visualstudio.com/), ortam değişkenleri kümesinde *.vscode/launch.json* geliştirme sırasında dosya. Daha fazla bilgi için bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments).

`ConfigureHostConfiguration` toplama sonuçları ile birden çok kez çağrılabilir. Ana bilgisayar son hangi seçeneği bir değer ayarlar kullanır.

*hostsettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

Örnek `HostBuilder` Yapılandırması'nı kullanarak `ConfigureHostConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:environment`). `AddConfiguration` Yöntemi bekliyor eşleşecek şekilde anahtarları `HostBuilder` anahtarları (örneğin, `environment`). Bölüm adı anahtarlar varlığını ana bilgisayar yapılandırmalarını bölümün değerleri engeller. Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir. Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).

### <a name="extension-method-configuration"></a>Uzantı yöntemi yapılandırması

Uzantı yöntemleri çağrılmadan üzerinde `IHostBuilder` içerik kök ve ortamı yapılandırmak için uygulama.

#### <a name="content-root"></a>Kök içerik

Bu ayar, ana bilgisayar için içerik dosyaları arama başladığı belirler.

**Anahtar**: contentRoot  
**Tür**: *dize*  
**Varsayılan**: varsayılan olarak, uygulama derleme bulunduğu klasöre.  
**Kullanılarak ayarlanan**: `UseContentRoot`  
**Ortam değişkeni**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` olan [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))

Yol yoksa, konağı başlatmak başarısız olur.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a>Ortam

Uygulamanın ayarlar [ortam](xref:fundamentals/environments).

**Anahtar**: ortamı  
**Tür**: *dize*  
**Varsayılan**: üretim  
**Kullanılarak ayarlanan**: `UseEnvironment`  
**Ortam değişkeni**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` olan [isteğe bağlıdır ve kullanıcı tanımlı](#configuration-builder))

Ortam herhangi bir değere ayarlanabilir. Framework tanımlı değerler `Development`, `Staging`, ve `Production`. Değerleri büyük küçük harfe duyarlı değildir.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Uygulama Oluşturucu yapılandırması çağırarak oluşturulur [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) üzerinde [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) uygulaması. `ConfigureAppConfiguration` kullanan bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) oluşturmak için bir [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) uygulaması. `ConfigureAppConfiguration` toplama sonuçları ile birden çok kez çağrılabilir. Uygulama, en son hangi seçeneği bir değer ayarlar kullanır. Tarafından oluşturulan yapılandırma `ConfigureAppConfiguration` şu adresten edinilebilir [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) sonraki işlemleri hem de [Hizmetleri](/dotnet/api/microsoft.extensions.hosting.ihost.services).

Örnek Uygulama Yapılandırması kullanılarak `ConfigureAppConfiguration`:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

*appSettings.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

*appSettings. Development.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

*appSettings. Production.JSON*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) genişletme yöntemi tarafından döndürülen bir yapılandırma bölümü ayrıştırılırken şu anda yeteneğine sahip değil [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (örneğin, `.AddConfiguration(Configuration.GetSection("section"))`. `GetSection` Yöntemi istenen bölüm için yapılandırma anahtarları filtreler ancak bölüm adı tuşlar bırakır (örneğin, `section:Logging:LogLevel:Default`). `AddConfiguration` Yöntemi bekliyor yapılandırma anahtarları için tam bir eşleşme (örneğin, `Logging:LogLevel:Default`). Bölüm adı anahtarlar varlığını uygulama yapılandırmalarını bölümün değerleri engeller. Bu soruna önümüzdeki sürümlerden birinde çözüm getirilecektir. Daha fazla bilgi ve geçici çözümler için bkz: [yapılandırma bölümü WebHostBuilder.UseConfiguration geçirme tam anahtarları kullanan](https://github.com/aspnet/Hosting/issues/839).

## <a name="configureservices"></a>ConfigureServices

[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) Hizmetleri uygulamanın ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı. `ConfigureServices` toplama sonuçları ile birden çok kez çağrılabilir.

Barındırılan hizmet uygulayan arka plan görevi mantığı ile bir sınıftır [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi. Daha fazla bilgi için bkz: [arka plan görevleri barındırılan hizmetleri ile](xref:fundamentals/host/hosted-services) konu.

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) kullanan `AddHostedService` ömür olayları için bir hizmet eklemek için genişletme yöntemi `LifetimeEventsHostedService`ve bir zaman aşımına arka plan görevi `TimedHostedService`, uygulama için:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a>ConfigureLogging

[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) sağlanan yapılandırmak için bir temsilci ekler [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder). `ConfigureLogging` toplama sonuçları ile birden çok kez çağrılabilir.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a>UseConsoleLifetime

[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) dinler `Ctrl+C`/SIGINT veya SIGTERM ve çağrıları [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) kapatma işlemini başlatmak için. `UseConsoleLifetime` Uzantıları gibi engelini kaldırır [RunAsync](#runasync) ve [WaitForShutdownAsync](#waitforshutdownasync). [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) olarak varsayılan yaşam süresi uygulaması önceden kayıtlı değil. Kayıtlı son ömrü kullanılır.

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a>Kapsayıcı yapılandırma

Diğer kapsayıcılarında takma desteklemek için konak kabul edebilen bir [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1). Bir Fabrika sağlama dı kapsayıcı kayıt parçası değildir, ancak bunun yerine somut dı kapsayıcı oluşturmak için kullanılan bir iç yöneticisidir. [UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) uygulamanın hizmet sağlayıcısı oluşturmak için kullanılan varsayılan fabrika geçersiz kılar.

Özel kapsayıcısı yapılandırmasını yönetilir [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) yöntemi. `ConfigureContainer` altta yatan ana bilgisayar API üstünde kapsayıcı yapılandırmak için kesin türü belirtilmiş bir deneyim sağlar. `ConfigureContainer` toplama sonuçları ile birden çok kez çağrılabilir.

Uygulama için bir hizmet kapsayıcısı oluşturun:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

Bir hizmet kapsayıcı üreteci sağlar:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

Fabrika kullanın ve uygulama için özel hizmet kapsayıcı yapılandırın:

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a>Genişletilebilirlik

Ana bilgisayar genişletilebilirliği üzerinde genişletme yöntemleri ile gerçekleştirilir `IHostBuilder`. Aşağıdaki örnek, bir genişletme yöntemi nasıl genişlettiğini gösterir bir `IHostBuilder` uygulamasıyla [RabbitMQ](https://www.rabbitmq.com/). Bir RabbitMQ genişletme yöntemi (başka bir uygulamadaki) kaydeder `IHostedService`:

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a>Ana bilgisayar yönetmek

[IHost](/dotnet/api/microsoft.extensions.hosting.ihost) uygulamasıdır başlatma ve durdurma sorumlu `IHostedService` hizmet kapsayıcısında kayıtlı uygulamaları.

### <a name="run"></a>Çalıştır

[Çalıştırma](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) uygulama çalıştırır ve ana bilgisayar kapatılana kadar çağıran iş parçacığı engeller:

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

[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) uygulama çalıştırır ve döndürür bir `Task` kapatma ya da iptal belirteci tetiklendiğinde tamamlar:

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

[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) konsol desteğini etkinleştirir, oluşturur ve ana bilgisayarı başlatır ve bekler `Ctrl+C`/SIGINT veya SIGTERM kapatılamadı.

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

[Başlat](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) konak zaman uyumlu olarak başlatır.

[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) sağlanan zaman aşımı süresi içinde ana bilgisayar durdurmaya çalışır.

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

[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) uygulama durdurur.

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

[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) döndüren bir `Task` kapatma verilen belirteç ve çağrıları aracılığıyla tetiklendiğinde tamamlanan [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).

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

Ana bilgisayarın dış denetim dışarıdan çağrılabilir yöntemleri kullanılarak elde edilir:

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

[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) başlangıcında adlı [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), devam etmeden önce işlemi tamamlanana kadar bekler. Bu, bir dış olay işaret kadar başlatma gecikmesi için kullanılabilir.

## <a name="ihostingenvironment-interface"></a>IHostingEnvironment arabirimi

[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) barındırma ortamı uygulama hakkında bilgi sağlar. Kullanmak [Oluşturucu ekleme](xref:fundamentals/dependency-injection) almak için `IHostingEnvironment` genişletme yöntemleri ve özellikleri kullanmak için:

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

Daha fazla bilgi için bkz: [kullanan birden çok ortamlar](xref:fundamentals/environments).

## <a name="iapplicationlifetime-interface"></a>IApplicationLifetime arabirimi

[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) kapama istekleri dahil olmak üzere sonrası başlatma ve kapatma etkinlikler için sağlar. Üç arabirimde özelliklerdir kaydetmek için kullanılan iptal belirteçlerini `Action` başlatma ve kapatma olayları tanımlayan yöntemleri.

| İptal belirteci | Ne zaman tetiklendi&#8230; |
| ------------------ | --------------------- |
| [ApplicationStarted](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | Ana bilgisayar tam olarak başlatıldı. |
| [ApplicationStopped](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | Konak bir kapama üzeredir. Tüm isteklerin işlenmesi. Bu olay tamamlanana kadar kapatma engeller. |
| [ApplicationStopping](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | Konak bir kapama gerçekleştiriyor. İstekleri hala işliyor olabilir. Bu olay tamamlanana kadar kapatma engeller. |

Oluşturucu Ekle `IApplicationLifetime` herhangi bir sınıfın içine hizmet. [Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) içine Oluşturucu ekleme kullanan bir `LifetimeEventsHostedService` sınıfı (bir `IHostedService` uygulaması) olayları kaydetmek için.

*LifetimeEventsHostedService.cs*:

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) uygulama sonlandırılması ister. Aşağıdaki sınıf kullanır `StopApplication` düzgün biçimde bir uygulamasını kapatmak için zaman sınıfının `Shutdown` yöntemi çağrılır:

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

* [Barındırılan hizmetler ile arka plan görevleri](xref:fundamentals/host/hosted-services)
* [GitHub depo örnekleri barındırma](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
