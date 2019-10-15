---
title: ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri
author: guardrex
description: ASP.NET Core içinde barındırılan hizmetlerle arka plan görevlerinin nasıl uygulanacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: c1fbb5ae8ffc4ee506f42df6a4cbbe845b2b903d
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333653"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a>ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri

, [Luke Latham](https://github.com/guardrex) ve [Jeow li Hua](https://github.com/huan086) tarafından

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir. Barındırılan bir hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır. Bu konuda üç barındırılan hizmet örneği sunulmaktadır:

* Bir Zamanlayıcı üzerinde çalışan arka plan görevi.
* [Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet. Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanabilir.
* Sıralı olarak çalışan sıraya alınmış arka plan görevleri.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulama iki sürümde sunulmaktadır:

* Web ana bilgisayarı &ndash; Web ana bilgisayarı Web uygulamalarını barındırmak için yararlıdır. Bu konu başlığında gösterilen örnek kod, örneğin web ana bilgisayar sürümüdür. Daha fazla bilgi için bkz. [Web ana bilgisayarı](xref:fundamentals/host/web-host) konusu.
* Genel ana bilgisayar &ndash; genel ana bilgisayar ASP.NET Core 2,1 ' de yenidir. Daha fazla bilgi için bkz. [genel ana bilgisayar](xref:fundamentals/host/generic-host) konusu.

## <a name="worker-service-template"></a>Çalışan hizmeti şablonu

ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar. Şablonu bir barındırılan hizmetler uygulamasının temeli olarak kullanmak için:

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a>Paket

[Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine yönelik bir paket başvurusu, ASP.NET Core uygulamalar için örtük olarak eklenir.

## <a name="ihostedservice-interface"></a>Ihostedservice arabirimi

@No__t-0 arabirimi, konak tarafından yönetilen nesneler için iki yöntem tanımlar:

* [Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevini başlatma mantığını içerir. `StartAsync` şu kadar *çağrılır:*

  * Uygulamanın istek işleme işlem hattı yapılandırıldı (`Startup.Configure`).
  * Sunucu başlatıldı ve [ıapplicationlifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.

  Varsayılan davranış, barındırılan hizmetin `StartAsync` ' ı, uygulamanın işlem hattı yapılandırıldıktan ve `ApplicationStarted` çağrıldıktan sonra çalışacak şekilde değiştirilebilir. Varsayılan davranışı değiştirmek için, `ConfigureWebHostDefaults` ' i çağırdıktan sonra barındırılan hizmeti (aşağıdaki örnekte `VideosWatcher`) ekleyin:

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

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) @no__t-konak düzgün kapanma yaparken tetiklenir. `StopAsync`, arka plan görevinin bitiş mantığını içerir. Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcıları (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.

  İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır. Belirteç üzerinde iptal istendiğinde:

  * Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.
  * @No__t-0 ' da çağrılan tüm yöntemler hemen döndürmelidir.

  Ancak, iptal edildikten sonra görevler terk edilmez @ no__t-0çağıran tüm görevlerin tamamlanmasını bekler.

  Uygulama beklenmedik bir şekilde kapanırsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrılmayabilir. Bu nedenle, `StopAsync` ' da yürütülen tüm yöntemler veya işlem gerçekleşmeyebilir.

  Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:

  * Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.

Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır. Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.

## <a name="backgroundservice"></a>BackgroundService

`BackgroundService`, uzun süre çalışan <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamak için bir temel sınıftır. `BackgroundService` ' ın, hizmetin mantığını içermesi için `ExecuteAsync(CancellationToken stoppingToken)` soyut yöntemi sağlar. @No__t-0, [ıhostedservice. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) çağrıldığında tetiklenir. Bu yöntemin uygulanması, arka plan hizmetinin tüm ömrünü temsil eden bir `Task` döndürür.

Ayrıca, *isteğe bağlı* olarak, hizmetiniz için başlatma ve kapatılma kodunu çalıştırmak üzere `IHostedService` ' de tanımlanan yöntemleri geçersiz kılın:

* `StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync`, uygulama konağı düzgün kapanma gerçekleştirirken çağrılır. @No__t-0, ana bilgisayar hizmeti zorla sonlandırmaya karar verdiğinde çağrılır. Bu yöntem geçersiz kılınırsa, hizmetin düzgün bir şekilde kapatılmasını sağlamak için temel sınıf yöntemini (ve `await`) çağırmanız **gerekir** .
* arka plan hizmetini başlatmak için `StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` çağırılır. Başlangıç işlemi kesintiye uğrarsa `cancellationToken` ' a işaret edilir. Uygulama, hizmetin başlatma sürecini temsil eden bir `Task` döndürür. Bu @no__t 0 tamamlanana kadar başka hizmet başlatılamaz. Bu yöntem geçersiz kılınırsa, hizmetin düzgün şekilde başlamasını sağlamak için temel sınıf yöntemini (ve `await`) çağırmanız **gerekir** .

## <a name="timed-background-tasks"></a>Zamanlanmış arka plan görevleri

Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar. Zamanlayıcı, görevin `DoWork` yöntemini tetikler. Süreölçer `StopAsync` ' da devre dışı bırakılır ve hizmet kapsayıcısı `Dispose` ' de bırakıldığında bırakıldı:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

Hizmet, `AddHostedService` uzantı yöntemiyle `IHostBuilder.ConfigureServices` (*program.cs*) öğesine kaydedilir:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Bir arka plan görevinde kapsamlı bir hizmeti kullanma

@No__t-1 içinde [kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) kullanmak için bir kapsam oluşturun. Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.

Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir. Aşağıdaki örnekte:

* Hizmet zaman uyumsuz. @No__t-0 yöntemi `Task` döndürür. Tanıtım amacıyla, on saniyelik bir gecikme `DoWork` yönteminde bekletildi.
* @No__t-0, hizmete eklenir.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur. `DoWork`, `ExecuteAsync` ' de beklemiş olan bir `Task` döndürür:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

Hizmetler `IHostBuilder.ConfigureServices` ' a (*program.cs*) kaydedilir. Barındırılan hizmet `AddHostedService` genişletme yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Sıraya alınan arka plan görevleri

Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ' ı temel alır ([ASP.NET Core için yerleşik](https://github.com/aspnet/Hosting/issues/1280)olarak, geçici olarak zamanlandı):

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

Aşağıdaki `QueueHostedService` örnek:

* @No__t-0 yöntemi, `ExecuteAsync` ' de beklemiş olan bir `Task` döndürür.
* Kuyruktaki arka plan görevleri, `BackgroundProcessing` ' da kuyruğa alınır ve yürütülür.
* @No__t-0 ' da hizmet durdurulmadan önce iş öğeleri bekletildi.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

@No__t-0 hizmeti, giriş cihazında `w` anahtarı seçildiğinde barındırılan hizmet için görevleri sıraya alır:

* @No__t-0 `MonitorLoop` hizmetine eklenmiş.
* `IBackgroundTaskQueue.QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır.
* Çalışma öğesi uzun süre çalışan bir arka plan görevinin benzetimini yapar:
  * Üç 5-ikinci gecikme yürütülür (`Task.Delay`).
  * @No__t-0 ifadesinde, görev iptal edilirse, <xref:System.OperationCanceledException> tuzakları yakalar.

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

Hizmetler `IHostBuilder.ConfigureServices` ' a (*program.cs*) kaydedilir. Barındırılan hizmet `AddHostedService` genişletme yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir. Barındırılan bir hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır. Bu konuda üç barındırılan hizmet örneği sunulmaktadır:

* Bir Zamanlayıcı üzerinde çalışan arka plan görevi.
* [Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet. Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanabilir
* Sıralı olarak çalışan sıraya alınmış arka plan görevleri.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Örnek uygulama iki sürümde sunulmaktadır:

* Web ana bilgisayarı &ndash; Web ana bilgisayarı Web uygulamalarını barındırmak için yararlıdır. Bu konu başlığında gösterilen örnek kod, örneğin web ana bilgisayar sürümüdür. Daha fazla bilgi için bkz. [Web ana bilgisayarı](xref:fundamentals/host/web-host) konusu.
* Genel ana bilgisayar &ndash; genel ana bilgisayar ASP.NET Core 2,1 ' de yenidir. Daha fazla bilgi için bkz. [genel ana bilgisayar](xref:fundamentals/host/generic-host) konusu.

## <a name="package"></a>Paket

Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine bir paket başvurusu ekleyin.

## <a name="ihostedservice-interface"></a>Ihostedservice arabirimi

Barındırılan hizmetler <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygular. Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:

* [Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevini başlatma mantığını içerir. [Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanılırken, sunucu başlatıldıktan ve ıapplicationlifetime 'a `StartAsync` çağrılır [. applicationstarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir. [Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanılırken, `ApplicationStarted` tetiklenene kadar `StartAsync` çağrılır.

* [StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) @no__t-konak düzgün kapanma yaparken tetiklenir. `StopAsync`, arka plan görevinin bitiş mantığını içerir. Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcıları (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.

  İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır. Belirteç üzerinde iptal istendiğinde:

  * Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.
  * @No__t-0 ' da çağrılan tüm yöntemler hemen döndürmelidir.

  Ancak, iptal edildikten sonra görevler terk edilmez @ no__t-0çağıran tüm görevlerin tamamlanmasını bekler.

  Uygulama beklenmedik bir şekilde kapanırsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrılmayabilir. Bu nedenle, `StopAsync` ' da yürütülen tüm yöntemler veya işlem gerçekleşmeyebilir.

  Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:

  * Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.
  * Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.

Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır. Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.

## <a name="timed-background-tasks"></a>Zamanlanmış arka plan görevleri

Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar. Zamanlayıcı, görevin `DoWork` yöntemini tetikler. Süreölçer `StopAsync` ' da devre dışı bırakılır ve hizmet kapsayıcısı `Dispose` ' de bırakıldığında bırakıldı:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

Hizmet, `AddHostedService` uzantı yöntemiyle `Startup.ConfigureServices` ' a kaydedilir:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a>Bir arka plan görevinde kapsamlı bir hizmeti kullanma

@No__t-1 içinde [kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) kullanmak için bir kapsam oluşturun. Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.

Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir. Aşağıdaki örnekte, hizmetine bir <xref:Microsoft.Extensions.Logging.ILogger> eklenir:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

Hizmetler `Startup.ConfigureServices` ' a kaydedilir. @No__t 0 uygulama `AddHostedService` uzantı yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a>Sıraya alınan arka plan görevleri

Arka plan görev kuyruğu .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ' dır ([ASP.NET Core için](https://github.com/aspnet/Hosting/issues/1280)isteğe bağlı olarak zamanlanmış olarak zamanlandı):

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

@No__t-0 ' da, kuyruktaki arka plan görevleri kuyruğa alınır ve bir <xref:Microsoft.Extensions.Hosting.BackgroundService> olarak yürütülür ve bu, uzun süre çalışan bir `IHostedService` ' yi uygulamaya yönelik temel bir sınıftır:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

Hizmetler `Startup.ConfigureServices` ' a kaydedilir. @No__t 0 uygulama `AddHostedService` uzantı yöntemiyle kaydedilir:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

Dizin sayfası model sınıfında:

* @No__t-0 oluşturucuya eklenir ve `Queue` ' e atanır.
* @No__t-0, `_serviceScopeFactory` ' e eklenir ve atanır. Fabrika, bir kapsam içinde hizmet oluşturmak için kullanılan <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> örnekleri oluşturmak için kullanılır. @No__t-2 ' deki (tek bir hizmet) veritabanı kayıtlarını yazmak için uygulamanın `AppDbContext` ( [kapsamlı bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes)) kullanmak üzere bir kapsam oluşturulur.

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

Dizin sayfasında **Görev Ekle** düğmesi seçildiğinde `OnPostAddTask` yöntemi yürütülür. `QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır:

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Ihostedservice ve BackgroundService sınıfıyla mikro hizmetlerde arka plan görevleri uygulama](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
