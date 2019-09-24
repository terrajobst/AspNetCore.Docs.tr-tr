---
title: ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri
author: guardrex
description: ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/18/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 8df86b10d7ba853edb3265df0e02eabbf8a2c058
ms.sourcegitcommit: fa61d882be9d0c48bd681f2efcb97e05522051d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71205705"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri

Tarafından [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir. Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi uygulayan bir arka plan görevi mantığı olan bir sınıftır. Bu konuda üç barındırılan hizmet örneği sunulmaktadır:

* Bir Zamanlayıcı üzerinde çalışan arka plan görevi.
* [Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet. Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanabilir.
* Sıralı olarak çalışan sıraya alınmış arka plan görevleri.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulama iki sürümde sunulmaktadır:

* Web ana &ndash; bilgisayarı Web ana bilgisayarı Web uygulamalarını barındırmak için yararlıdır. Bu konu başlığında gösterilen örnek kod, örneğin web ana bilgisayar sürümüdür. Daha fazla bilgi için bkz. [Web ana bilgisayarı](xref:fundamentals/host/web-host) konusu.
* Genel ana &ndash; bilgisayar genel ana bilgisayarı ASP.NET Core 2,1 ' de yenidir. Daha fazla bilgi için bkz. [genel ana bilgisayar](xref:fundamentals/host/generic-host) konusu.

## <a name="worker-service-template"></a>Çalışan hizmeti şablonu

ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar. Şablonu bir barındırılan hizmetler uygulamasının temeli olarak kullanmak için:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Yeni bir proje oluşturun.
1. Seçin **ASP.NET Core Web uygulaması**. **İleri**’yi seçin.
1. **Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin. **Oluştur**’u seçin.
1. **Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.
1. **Çalışan hizmeti** şablonunu seçin. **Oluştur**’u seçin.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

1. Yeni bir proje oluşturun.
1. Kenar çubuğunda **.NET Core** altında **uygulama** ' yı seçin.
1. **ASP.NET Core**altında **çalışan** ' ı seçin. **İleri**’yi seçin.
1. **Hedef çerçeve**Için **.NET Core 3,0** ' ı seçin. **İleri**’yi seçin.
1. **Proje adı** alanına bir ad girin. **Oluştur**’u seçin.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Çalışan hizmeti (`worker`) şablonunu bir komut kabuğundan [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla birlikte kullanın. Aşağıdaki örnekte, adlı `ContosoWorker`bir çalışan hizmeti uygulaması oluşturulur. Komut yürütüldüğünde, `ContosoWorker` uygulamanın bir klasörü otomatik olarak oluşturulur.

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a>Paket

[Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine yönelik bir paket başvurusu, ASP.NET Core uygulamalar için örtük olarak eklenir.

## <a name="ihostedservice-interface"></a>Ihostedservice arabirimi

<xref:Microsoft.Extensions.Hosting.IHostedService> Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:

* [Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; arkaplan`StartAsync` görevinin başlatılacağı mantığı içerir. `StartAsync`Şu kadar *çağrılır:*

  * Uygulamanın istek işleme işlem hattı yapılandırıldı (`Startup.Configure`).
  * Sunucu başlatıldı ve [ıapplicationlifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.

  Varsayılan davranış, barındırılan hizmetin `StartAsync` , uygulamanın işlem hattı yapılandırıldıktan ve `ApplicationStarted` çağrıldıktan sonra çalışması için değiştirilebilir. Varsayılan davranışı değiştirmek için, öğesini çağırdıktan`VideosWatcher` `ConfigureWebHostDefaults`sonra barındırılan hizmeti (aşağıdaki örnekte) ekleyin:

  ```csharp
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.DependencyInjection;
  using Microsoft.Extensions.Hosting;

  public class Program
  {
      public static void Main(string[] args)
      {
          CreateHostBuilder(args).Build().Run();
      }

      public static IHostBuilder CreateHostBuilder(string[] args) =>
          Host.CreateDefaultBuilder(args)
              .ConfigureWebHostDefaults(webBuilder =>
              {
                  webBuilder.UseStartup<Startup>();
              })
              .ConfigureServices(services =>
              {
                  services.AddHostedService<VideosWatcher>();
              });
  }
  ```

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir. `StopAsync`arka plan görevinin bitiş mantığını içerir. Yönetilmeyen <xref:System.IDisposable> kaynakların atılmaya yönelik uygulama ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) .

  İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır. Belirteç üzerinde iptal istendiğinde:

  * Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.
  * İçinde `StopAsync` çağrılan tüm yöntemler hemen döndürmelidir.

  Ancak, bir iptal işlemi istendikten&mdash;sonra görevler terk edilmez ve bu, çağıran tüm görevlerin tamamlanmasını bekler.

  Uygulama beklenmedik bir şekilde kapanırsa (örneğin, uygulamanın işlemi başarısız olur), `StopAsync` çağrılmayabilir. Bu nedenle, veya üzerinde `StopAsync` gerçekleştirilen işlemleri çağıran Yöntemler gerçekleşmeyebilir.

  Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>Genel ana bilgisayar kullanırken. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.

Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır. Arka plan görevinin yürütülmesi sırasında bir hata oluşursa, `Dispose` çağrılmadıysa `StopAsync` bile çağrılmalıdır.

## <a name="backgroundservice"></a>BackgroundService

`BackgroundService`, uzun süre çalışan <xref:Microsoft.Extensions.Hosting.IHostedService>uygulamaya yönelik bir temel sınıftır. `BackgroundService`arka plan işlemleri için iki yöntem tanımlar:

* `ExecuteAsync(CancellationToken stoppingToken)`Başladığındaçağırılır&ndash;. `ExecuteAsync` <xref:Microsoft.Extensions.Hosting.IHostedService> Uygulama, gerçekleştirilen uzun süren `Task` işlemlerin ömrünü temsil eden bir döndürmelidir. [Ihostedservice. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) çağrıldığında tetiklenen.`stoppingToken`
* `StopAsync(CancellationToken stoppingToken)`&ndash; UygulamaKonağı`StopAsync` düzgün kapanma yaparken tetiklenir. , `stoppingToken` Kapanma işleminin artık düzgün şekilde olmaması gerektiğini gösterir.

## <a name="timed-background-tasks"></a>Zamanlanmış arka plan görevleri

Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar. Zamanlayıcı, görevin `DoWork` metodunu tetikler. Zamanlayıcı devre dışı bırakılır `StopAsync` ve hizmet kapsayıcısı `Dispose`bırakıldığında bırakıldı:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

Hizmet, `IHostBuilder.ConfigureServices` (*program.cs*) `AddHostedService` öğesine uzantı yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Bir arka plan görevinde kapsamlı bir hizmeti kullanma

İçindeki`BackgroundService` [kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) kullanmak için bir kapsam oluşturun. Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.

Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir. Aşağıdaki örnekte:

* Hizmet zaman uyumsuz. `DoWork` Yöntemi bir`Task`döndürür. Tanıtım amacıyla, `DoWork` yöntemde on saniyelik bir gecikme beklenir.
* Hizmete <xref:Microsoft.Extensions.Logging.ILogger> eklenen bir.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur. `DoWork``Task` şunu`ExecuteAsync`döndürür:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

Hizmetler ' de `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*). Barındırılan hizmet, `AddHostedService` uzantı yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Sıraya alınan arka plan görevleri

Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 'i temel alır ([ASP.NET Core için isteğe](https://github.com/aspnet/Hosting/issues/1280)bağlı olarak zamanlanmış olarak zamanlandı):

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

Aşağıdaki `QueueHostedService` örnekte:

* Yöntemi, ' de `Task` `ExecuteAsync`beklemiş bir döndürür. `BackgroundProcessing`
* Kuyruktaki arka plan görevleri, içinde `BackgroundProcessing`kuyruklanmış ve yürütülür.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

Bir `MonitorLoop` hizmet, `w` giriş cihazında anahtar her seçildiğinde barındırılan hizmet için görevleri sıraya alır:

* , `IBackgroundTaskQueue` `MonitorLoop` Hizmete eklenir.
* `IBackgroundTaskQueue.QueueBackgroundWorkItem`bir iş öğesini kuyruğa almak için çağrılır.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

Hizmetler ' de `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*). Barındırılan hizmet, `AddHostedService` uzantı yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir. Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi uygulayan bir arka plan görevi mantığı olan bir sınıftır. Bu konuda üç barındırılan hizmet örneği sunulmaktadır:

* Bir Zamanlayıcı üzerinde çalışan arka plan görevi.
* [Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet. Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanabilir
* Sıralı olarak çalışan sıraya alınmış arka plan görevleri.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulama iki sürümde sunulmaktadır:

* Web ana &ndash; bilgisayarı Web ana bilgisayarı Web uygulamalarını barındırmak için yararlıdır. Bu konu başlığında gösterilen örnek kod, örneğin web ana bilgisayar sürümüdür. Daha fazla bilgi için bkz. [Web ana bilgisayarı](xref:fundamentals/host/web-host) konusu.
* Genel ana &ndash; bilgisayar genel ana bilgisayarı ASP.NET Core 2,1 ' de yenidir. Daha fazla bilgi için bkz. [genel ana bilgisayar](xref:fundamentals/host/generic-host) konusu.

## <a name="package"></a>Paket

Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine bir paket başvurusu ekleyin.

## <a name="ihostedservice-interface"></a>Ihostedservice arabirimi

Barındırılan hizmetler, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygular. Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:

* [Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; arkaplan`StartAsync` görevinin başlatılacağı mantığı içerir. [Web konağını](xref:fundamentals/host/web-host) `StartAsync` kullanırken sunucu başlatıldıktan ve [ıapplicationlifetime değerinden sonra çağrılır. applicationstarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir. [Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanılırken, `StartAsync` tetiklendikten önce `ApplicationStarted` çağrılır.

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir. `StopAsync`arka plan görevinin bitiş mantığını içerir. Yönetilmeyen <xref:System.IDisposable> kaynakların atılmaya yönelik uygulama ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) .

  İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır. Belirteç üzerinde iptal istendiğinde:

  * Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.
  * İçinde `StopAsync` çağrılan tüm yöntemler hemen döndürmelidir.

  Ancak, bir iptal işlemi istendikten&mdash;sonra görevler terk edilmez ve bu, çağıran tüm görevlerin tamamlanmasını bekler.

  Uygulama beklenmedik bir şekilde kapanırsa (örneğin, uygulamanın işlemi başarısız olur), `StopAsync` çağrılmayabilir. Bu nedenle, veya üzerinde `StopAsync` gerçekleştirilen işlemleri çağıran Yöntemler gerçekleşmeyebilir.

  Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:

  * <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>Genel ana bilgisayar kullanırken. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.

Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır. Arka plan görevinin yürütülmesi sırasında bir hata oluşursa, `Dispose` çağrılmadıysa `StopAsync` bile çağrılmalıdır.

## <a name="timed-background-tasks"></a>Zamanlanmış arka plan görevleri

Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar. Zamanlayıcı, görevin `DoWork` metodunu tetikler. Zamanlayıcı devre dışı bırakılır `StopAsync` ve hizmet kapsayıcısı `Dispose`bırakıldığında bırakıldı:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Hizmet, `AddHostedService` uzantı yöntemiyle kaydedilir `Startup.ConfigureServices` :

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Bir arka plan görevinde kapsamlı bir hizmeti kullanma

[Kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) bir `IHostedService`içinde kullanmak için bir kapsam oluşturun. Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.

Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir. Aşağıdaki örnekte, hizmetine bir <xref:Microsoft.Extensions.Logging.ILogger> hizmet eklenmiş olur:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Barındırılan hizmet, kendi `DoWork` metodunu çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Hizmetler ' de `Startup.ConfigureServices`kaydedilir. Uygulama, `AddHostedService` uzantı yöntemiyle kaydedilir: `IHostedService`

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Sıraya alınan arka plan görevleri

Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 'i temel alır ([ASP.NET Core için isteğe](https://github.com/aspnet/Hosting/issues/1280)bağlı olarak zamanlanmış olarak zamanlandı):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

' `QueueHostedService`De, kuyruktaki arka plan görevleri <xref:Microsoft.Extensions.Hosting.BackgroundService>, uzun süre çalışan `IHostedService`uygulama için temel bir sınıf olan olarak kuyruğa alınır ve yürütülür.

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Hizmetler ' de `Startup.ConfigureServices`kaydedilir. Uygulama, `AddHostedService` uzantı yöntemiyle kaydedilir: `IHostedService`

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

Dizin sayfası model sınıfında:

* Oluşturucuya eklenir ve öğesine `Queue`atanır. `IBackgroundTaskQueue`
* Eklenen ve öğesine `_serviceScopeFactory`atandı. <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> Fabrika, bir kapsam içinde hizmet oluşturmak için <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>kullanılan örnekleri oluşturmak için kullanılır. Uygulama `AppDbContext` (bir tekil hizmet) içinde `IBackgroundTaskQueue` veritabanı kayıtları yazmak için uygulamanın ( [kapsamlı bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes)) kullanılabilmesi için bir kapsam oluşturulur.

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

Dizin `OnPostAddTask` sayfasında **Görev Ekle** düğmesi seçildiğinde Yöntem yürütülür. `QueueBackgroundWorkItem`bir iş öğesini kuyruğa almak için çağrılır:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Ihostedservice ve BackgroundService sınıfıyla mikro hizmetlerde arka plan görevleri uygulama](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
