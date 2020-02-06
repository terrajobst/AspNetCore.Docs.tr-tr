---
title: ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri
author: guardrex
description: ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/05/2020
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 6a88e56afc4fb1b4f673c362f83d948eda84b930
ms.sourcegitcommit: bd896935e91236e03241f75e6534ad6debcecbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77044879"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri

, [Luke Latham](https://github.com/guardrex) ve [Jeow li Hua](https://github.com/huan086) tarafından

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir. Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır. Bu konuda üç barındırılan hizmet örneği sunulmaktadır:

* Bir Zamanlayıcı üzerinde çalışan arka plan görevi.
* [Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet. Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanabilir.
* Sıralı olarak çalışan sıraya alınmış arka plan görevleri.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="worker-service-template"></a>Çalışan hizmeti şablonu

ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar. Çalışan hizmeti şablonundan oluşturulan bir uygulama, çalışan SDK 'sını proje dosyasında belirtir:

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

Şablonu bir barındırılan hizmetler uygulamasının temeli olarak kullanmak için:

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a>Paket

Çalışan hizmeti şablonunu temel alan bir uygulama `Microsoft.NET.Sdk.Worker` SDK kullanır ve [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine açık bir paket başvurusu içerir. Örneğin, örnek uygulamanın proje dosyasına (*Backgroundtaskssample. csproj*) bakın.

`Microsoft.NET.Sdk.Web` SDK kullanan Web uygulamaları için, [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine, paylaşılan çerçeveden dolaylı olarak başvurulur. Uygulamanın proje dosyasındaki açık bir paket başvurusu gerekli değildir.

## <a name="ihostedservice-interface"></a>Ihostedservice arabirimi

<xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi, konak tarafından yönetilen nesneler için iki yöntem tanımlar:

* [Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevinin başlatılacağı mantığı içerir. `StartAsync` şu kadar *çağrılır:*

  * Uygulamanın istek işleme işlem hattı yapılandırıldı (`Startup.Configure`).
  * Sunucu başlatıldı ve [ıapplicationlifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.

  Varsayılan davranış, barındırılan hizmetin `StartAsync`, uygulamanın işlem hattı yapılandırıldıktan ve `ApplicationStarted` çağrıldıktan sonra çalışması için değiştirilebilir. Varsayılan davranışı değiştirmek için, `ConfigureWebHostDefaults`çağrıldıktan sonra barındırılan hizmeti (aşağıdaki örnekte`VideosWatcher`) ekleyin:

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

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir. `StopAsync` arka plan görevinin sonundaki mantığı içerir. Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.

  İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır. Belirteç üzerinde iptal istendiğinde:

  * Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.
  * `StopAsync` içinde çağrılan yöntemler hemen döndürmelidir.

  Ancak,&mdash;istek iptal edildikten sonra, çağıran tüm görevlerin tamamlanmasını bekler.

  Uygulama beklenmedik şekilde kapanıyorsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrımayabilir. Bu nedenle, `StopAsync` veya içinde gerçekleştirilen işlemler tarafından çağrılan yöntemler gerçekleşmeyebilir.

  Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:

  * Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.

Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır. Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.

## <a name="backgroundservice-base-class"></a>BackgroundService temel sınıfı

<xref:Microsoft.Extensions.Hosting.BackgroundService>, uzun süre çalışan bir <xref:Microsoft.Extensions.Hosting.IHostedService>uygulamaya yönelik temel bir sınıftır.

Arka plan hizmetini çalıştırmak için [ExecuteAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) çağırılır. Uygulama, arka plan hizmetinin tüm ömrünü temsil eden bir <xref:System.Threading.Tasks.Task> döndürür. ExecuteAsync, `await`çağırarak [zaman uyumsuz hale](https://github.com/dotnet/extensions/issues/2149)gelene kadar başka bir hizmet başlatılamaz. `ExecuteAsync`içinde başlatma işini uzun süre gerçekleştirmekten kaçının. [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) içindeki ana bilgisayar blokları `ExecuteAsync` tamamlanmasını bekliyor.

[Ihostedservice. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) çağrıldığında iptal belirteci tetiklenir. `ExecuteAsync` uygulamanız, hizmeti sorunsuz bir şekilde kapatmak için iptal belirteci tetiklendiğinde hemen bitebilmelidir. Aksi takdirde, hizmet, kapanmanın zaman aşımından sonra kapanmadığını kaldırır. Daha fazla bilgi için [ıhostedservice arabirimi](#ihostedservice-interface) bölümüne bakın.

## <a name="timed-background-tasks"></a>Zamanlanmış arka plan görevleri

Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar. Zamanlayıcı, görevin `DoWork` yöntemini tetikler. Zamanlayıcı `StopAsync` devre dışı bırakılır ve hizmet kapsayıcısı `Dispose`bırakıldığında atıldı:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-17,34,41)]

<xref:System.Threading.Timer>, önceki `DoWork` yürütmelerinin bitmesini beklemez, bu nedenle gösterilen yaklaşım her senaryo için uygun olmayabilir. [Interkilitli. Increment](xref:System.Threading.Interlocked.Increment*) , birden çok iş parçacığının aynı anda `executionCount` güncelleştirmemesini sağlayan atomik bir işlem olarak yürütme sayacını artırmak için kullanılır.

Hizmet, `AddHostedService` uzantısı yöntemiyle `IHostBuilder.ConfigureServices` (*program.cs*) kaydedilir:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Bir arka plan görevinde kapsamlı bir hizmeti kullanma

Bir [backgroundservice](#backgroundservice-base-class)içinde [kapsamlı hizmetler](xref:fundamentals/dependency-injection#service-lifetimes) kullanmak için bir kapsam oluşturun. Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.

Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir. Aşağıdaki örnekte:

* Hizmet zaman uyumsuz. `DoWork` yöntemi bir `Task`döndürür. Tanıtım amacıyla, `DoWork` yönteminde on saniyelik bir gecikme beklenir.
* Hizmete bir <xref:Microsoft.Extensions.Logging.ILogger> eklenir.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur. `DoWork`, `ExecuteAsync`beklemiş olan bir `Task`döndürür:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

Hizmetler `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*). Barındırılan hizmet `AddHostedService` uzantısı yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Sıraya alınan arka plan görevleri

Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> temel alır ([ASP.NET Core için yerleşik](https://github.com/aspnet/Hosting/issues/1280)olarak, kesin olarak zamanlandı):

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

Aşağıdaki `QueueHostedService` örnekte:

* `BackgroundProcessing` yöntemi, `ExecuteAsync`beklemiş olan bir `Task`döndürür.
* Kuyruktaki arka plan görevleri, `BackgroundProcessing`sırada kuyruğa alınır ve yürütülür.
* İş öğeleri, hizmet `StopAsync`' de durdurulmadan önce bekletildi.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

Bir `MonitorLoop` hizmeti, giriş cihazında `w` anahtarı seçildiğinde barındırılan hizmet için görevleri sıraya alır:

* `IBackgroundTaskQueue`, `MonitorLoop` hizmetine eklenir.
* `IBackgroundTaskQueue.QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır.
* Çalışma öğesi uzun süre çalışan bir arka plan görevinin benzetimini yapar:
  * Üç 5-ikinci gecikme yürütülür (`Task.Delay`).
  * `try-catch` bir ifade, görev iptal edildiğinde <xref:System.OperationCanceledException> yakalar.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

Hizmetler `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*). Barındırılan hizmet `AddHostedService` uzantısı yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir. Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır. Bu konuda üç barındırılan hizmet örneği sunulmaktadır:

* Bir Zamanlayıcı üzerinde çalışan arka plan görevi.
* [Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet. Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanabilir
* Sıralı olarak çalışan sıraya alınmış arka plan görevleri.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="package"></a>Paket

Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine bir paket başvurusu ekleyin.

## <a name="ihostedservice-interface"></a>Ihostedservice arabirimi

Barındırılan hizmetler <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygular. Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:

* [Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevinin başlatılacağı mantığı içerir. [Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanılırken, sunucu başlatıldıktan ve ıapplicationlifetime 'dan sonra `StartAsync` çağrılır [. applicationstarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir. [Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanılırken, `ApplicationStarted` tetiklenene kadar `StartAsync` çağrılır.

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir. `StopAsync` arka plan görevinin sonundaki mantığı içerir. Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.

  İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır. Belirteç üzerinde iptal istendiğinde:

  * Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.
  * `StopAsync` içinde çağrılan yöntemler hemen döndürmelidir.

  Ancak,&mdash;istek iptal edildikten sonra, çağıran tüm görevlerin tamamlanmasını bekler.

  Uygulama beklenmedik şekilde kapanıyorsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrımayabilir. Bu nedenle, `StopAsync` veya içinde gerçekleştirilen işlemler tarafından çağrılan yöntemler gerçekleşmeyebilir.

  Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:

  * Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.

Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır. Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.

## <a name="timed-background-tasks"></a>Zamanlanmış arka plan görevleri

Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar. Zamanlayıcı, görevin `DoWork` yöntemini tetikler. Zamanlayıcı `StopAsync` devre dışı bırakılır ve hizmet kapsayıcısı `Dispose`bırakıldığında atıldı:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<xref:System.Threading.Timer>, önceki `DoWork` yürütmelerinin bitmesini beklemez, bu nedenle gösterilen yaklaşım her senaryo için uygun olmayabilir.

Hizmet, `AddHostedService` uzantısı yöntemiyle `Startup.ConfigureServices` kaydedilir:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Bir arka plan görevinde kapsamlı bir hizmeti kullanma

[Kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) bir `IHostedService`içinde kullanmak için bir kapsam oluşturun. Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.

Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir. Aşağıdaki örnekte, hizmetine bir <xref:Microsoft.Extensions.Logging.ILogger> eklenmiş olur:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Hizmetler `Startup.ConfigureServices`kaydedilir. `IHostedService` uygulama `AddHostedService` uzantısı yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Sıraya alınan arka plan görevleri

Arka plan görev kuyruğu .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> temel alır ([ASP.NET Core için kesin olarak zamanlandı](https://github.com/aspnet/Hosting/issues/1280)):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

`QueueHostedService`, sıradaki arka plan görevleri, uzun süre çalışan bir `IHostedService`uygulamaya yönelik temel bir sınıf olan bir [Backgroundservice](#backgroundservice-base-class)olarak kuyruklanmış ve yürütülür:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Hizmetler `Startup.ConfigureServices`kaydedilir. `IHostedService` uygulama `AddHostedService` uzantısı yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

Dizin sayfası model sınıfında:

* `IBackgroundTaskQueue` oluşturucuya eklenir ve `Queue`atanır.
* <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> eklenen ve `_serviceScopeFactory`atanmış. Fabrika, bir kapsam içinde hizmet oluşturmak için kullanılan <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>örnekleri oluşturmak için kullanılır. Bir kapsam, veritabanı kayıtlarını `IBackgroundTaskQueue` (bir tek hizmet) yazmak için uygulamanın `AppDbContext` ( [kapsamlı bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes)) kullanmak üzere oluşturulur.

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

Dizin sayfasında **Görev Ekle** düğmesi seçildiğinde `OnPostAddTask` yöntemi yürütülür. `QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Ihostedservice ve BackgroundService sınıfıyla mikro hizmetlerde arka plan görevleri uygulama](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [Azure App Service Web Işleri ile arka plan görevleri çalıştırma](/azure/app-service/webjobs-create)
* <xref:System.Threading.Timer>
