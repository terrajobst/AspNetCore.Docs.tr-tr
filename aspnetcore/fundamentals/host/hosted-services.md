---
title: ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri
author: guardrex
description: ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0eaa3a62370c1e413840bb65f597dc664adafc38
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71688088"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="2f471-103">ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="2f471-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="2f471-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2f471-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f471-105">ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="2f471-106">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi uygulayan bir arka plan görevi mantığı olan bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="2f471-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2f471-107">Bu konuda üç barındırılan hizmet örneği sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="2f471-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="2f471-108">Bir Zamanlayıcı üzerinde çalışan arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="2f471-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="2f471-109">[Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="2f471-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="2f471-110">Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="2f471-111">Sıralı olarak çalışan sıraya alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="2f471-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="2f471-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2f471-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2f471-113">Örnek uygulama iki sürümde sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="2f471-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="2f471-114">Web ana &ndash; bilgisayarı Web ana bilgisayarı Web uygulamalarını barındırmak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="2f471-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="2f471-115">Bu konu başlığında gösterilen örnek kod, örneğin web ana bilgisayar sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="2f471-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="2f471-116">Daha fazla bilgi için bkz. [Web ana bilgisayarı](xref:fundamentals/host/web-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="2f471-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="2f471-117">Genel ana &ndash; bilgisayar genel ana bilgisayarı ASP.NET Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="2f471-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="2f471-118">Daha fazla bilgi için bkz. [genel ana bilgisayar](xref:fundamentals/host/generic-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="2f471-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="2f471-119">Çalışan hizmeti şablonu</span><span class="sxs-lookup"><span data-stu-id="2f471-119">Worker Service template</span></span>

<span data-ttu-id="2f471-120">ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f471-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="2f471-121">Şablonu bir barındırılan hizmetler uygulamasının temeli olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="2f471-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="2f471-122">Paket</span><span class="sxs-lookup"><span data-stu-id="2f471-122">Package</span></span>

<span data-ttu-id="2f471-123">[Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine yönelik bir paket başvurusu, ASP.NET Core uygulamalar için örtük olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="2f471-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="2f471-124">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="2f471-124">IHostedService interface</span></span>

<span data-ttu-id="2f471-125"><xref:Microsoft.Extensions.Hosting.IHostedService> Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2f471-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="2f471-126">[Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; arkaplan`StartAsync` görevinin başlatılacağı mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="2f471-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="2f471-127">`StartAsync`Şu kadar *çağrılır:*</span><span class="sxs-lookup"><span data-stu-id="2f471-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="2f471-128">Uygulamanın istek işleme işlem hattı yapılandırıldı (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="2f471-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="2f471-129">Sunucu başlatıldı ve [ıapplicationlifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="2f471-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="2f471-130">Varsayılan davranış, barındırılan hizmetin `StartAsync` , uygulamanın işlem hattı yapılandırıldıktan ve `ApplicationStarted` çağrıldıktan sonra çalışması için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="2f471-131">Varsayılan davranışı değiştirmek için, öğesini çağırdıktan`VideosWatcher` `ConfigureWebHostDefaults`sonra barındırılan hizmeti (aşağıdaki örnekte) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2f471-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="2f471-132">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="2f471-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="2f471-133">`StopAsync`arka plan görevinin bitiş mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="2f471-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="2f471-134">Yönetilmeyen <xref:System.IDisposable> kaynakların atılmaya yönelik uygulama ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) .</span><span class="sxs-lookup"><span data-stu-id="2f471-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="2f471-135">İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="2f471-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="2f471-136">Belirteç üzerinde iptal istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="2f471-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="2f471-137">Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2f471-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="2f471-138">İçinde `StopAsync` çağrılan tüm yöntemler hemen döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="2f471-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="2f471-139">Ancak, bir iptal işlemi istendikten&mdash;sonra görevler terk edilmez ve bu, çağıran tüm görevlerin tamamlanmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="2f471-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="2f471-140">Uygulama beklenmedik bir şekilde kapanırsa (örneğin, uygulamanın işlemi başarısız olur), `StopAsync` çağrılmayabilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="2f471-141">Bu nedenle, veya üzerinde `StopAsync` gerçekleştirilen işlemleri çağıran Yöntemler gerçekleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="2f471-142">Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="2f471-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="2f471-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>Genel ana bilgisayar kullanırken.</span><span class="sxs-lookup"><span data-stu-id="2f471-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="2f471-144">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2f471-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="2f471-145">Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="2f471-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="2f471-146">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2f471-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="2f471-147">Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır.</span><span class="sxs-lookup"><span data-stu-id="2f471-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="2f471-148">Arka plan görevinin yürütülmesi sırasında bir hata oluşursa, `Dispose` çağrılmadıysa `StopAsync` bile çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2f471-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="2f471-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="2f471-149">BackgroundService</span></span>

<span data-ttu-id="2f471-150">`BackgroundService`, uzun süre çalışan <xref:Microsoft.Extensions.Hosting.IHostedService>uygulamaya yönelik bir temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="2f471-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="2f471-151">`BackgroundService`hizmetin mantığını içeren soyut yöntemi sağlar. `ExecuteAsync(CancellationToken stoppingToken)`</span><span class="sxs-lookup"><span data-stu-id="2f471-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="2f471-152">, `stoppingToken` [Ihostedservice. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) çağrıldığında tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="2f471-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="2f471-153">Bu yöntemin uygulanması, arka plan hizmetinin `Task` tüm ömrünü temsil eden bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="2f471-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="2f471-154">Ayrıca, *isteğe bağlı* olarak, hizmetiniz için başlatma `IHostedService` ve kapatılma kodunu çalıştırmak için tanımlanan yöntemleri geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="2f471-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="2f471-155">`StopAsync(CancellationToken cancellationToken)`&ndash; UygulamaKonağı`StopAsync` düzgün kapanma gerçekleştirirken çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2f471-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="2f471-156">Ana `cancellationToken` bilgisayar, hizmeti zorla sonlandırmaya karar verdiğinde, bu, işaret edilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="2f471-157">Bu yöntem geçersiz kılınırsa, hizmetin düzgün şekilde kapatılmasını sağlamak `await`için temel sınıf yöntemini çağırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2f471-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="2f471-158">`StartAsync(CancellationToken cancellationToken)`&ndash; arkaplan`StartAsync` hizmetini başlatmak için çağırılır.</span><span class="sxs-lookup"><span data-stu-id="2f471-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="2f471-159">Başlatma `cancellationToken` işlemi kesintiye uğrarsa, bu öğesine işaret edilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="2f471-160">Uygulama, hizmet için `Task` başlatma sürecini temsil eden bir döndürür.</span><span class="sxs-lookup"><span data-stu-id="2f471-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="2f471-161">Bu `Task` tamamlanana kadar başka hizmet başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="2f471-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="2f471-162">Bu yöntem geçersiz kılınırsa, hizmetin düzgün şekilde başlamasını sağlamak `await`için temel sınıf yöntemini çağırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2f471-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="2f471-163">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="2f471-163">Timed background tasks</span></span>

<span data-ttu-id="2f471-164">Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f471-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="2f471-165">Zamanlayıcı, görevin `DoWork` metodunu tetikler.</span><span class="sxs-lookup"><span data-stu-id="2f471-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="2f471-166">Zamanlayıcı devre dışı bırakılır `StopAsync` ve hizmet kapsayıcısı `Dispose`bırakıldığında bırakıldı:</span><span class="sxs-lookup"><span data-stu-id="2f471-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="2f471-167">Hizmet, `IHostBuilder.ConfigureServices` (*program.cs*) `AddHostedService` öğesine uzantı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="2f471-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="2f471-168">Bir arka plan görevinde kapsamlı bir hizmeti kullanma</span><span class="sxs-lookup"><span data-stu-id="2f471-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="2f471-169">İçindeki`BackgroundService` [kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) kullanmak için bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2f471-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="2f471-170">Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="2f471-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="2f471-171">Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="2f471-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="2f471-172">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="2f471-172">In the following example:</span></span>

* <span data-ttu-id="2f471-173">Hizmet zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="2f471-173">The service is asynchronous.</span></span> <span data-ttu-id="2f471-174">`DoWork` Yöntemi bir`Task`döndürür.</span><span class="sxs-lookup"><span data-stu-id="2f471-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="2f471-175">Tanıtım amacıyla, `DoWork` yöntemde on saniyelik bir gecikme beklenir.</span><span class="sxs-lookup"><span data-stu-id="2f471-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="2f471-176">Hizmete <xref:Microsoft.Extensions.Logging.ILogger> eklenen bir.</span><span class="sxs-lookup"><span data-stu-id="2f471-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="2f471-177">Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2f471-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="2f471-178">`DoWork``Task` şunu`ExecuteAsync`döndürür:</span><span class="sxs-lookup"><span data-stu-id="2f471-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="2f471-179">Hizmetler ' de `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="2f471-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="2f471-180">Barındırılan hizmet, `AddHostedService` uzantı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="2f471-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="2f471-181">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="2f471-181">Queued background tasks</span></span>

<span data-ttu-id="2f471-182">Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 'i temel alır ([ASP.NET Core için isteğe](https://github.com/aspnet/Hosting/issues/1280)bağlı olarak zamanlanmış olarak zamanlandı):</span><span class="sxs-lookup"><span data-stu-id="2f471-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="2f471-183">Aşağıdaki `QueueHostedService` örnekte:</span><span class="sxs-lookup"><span data-stu-id="2f471-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="2f471-184">Yöntemi, ' de `Task` `ExecuteAsync`beklemiş bir döndürür. `BackgroundProcessing`</span><span class="sxs-lookup"><span data-stu-id="2f471-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="2f471-185">Kuyruktaki arka plan görevleri, içinde `BackgroundProcessing`kuyruklanmış ve yürütülür.</span><span class="sxs-lookup"><span data-stu-id="2f471-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="2f471-186">Bir `MonitorLoop` hizmet, `w` giriş cihazında anahtar her seçildiğinde barındırılan hizmet için görevleri sıraya alır:</span><span class="sxs-lookup"><span data-stu-id="2f471-186">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="2f471-187">, `IBackgroundTaskQueue` `MonitorLoop` Hizmete eklenir.</span><span class="sxs-lookup"><span data-stu-id="2f471-187">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="2f471-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem`bir iş öğesini kuyruğa almak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2f471-188">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="2f471-189">Hizmetler ' de `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="2f471-189">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="2f471-190">Barındırılan hizmet, `AddHostedService` uzantı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="2f471-190">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2f471-191">ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-191">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="2f471-192">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi uygulayan bir arka plan görevi mantığı olan bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="2f471-192">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2f471-193">Bu konuda üç barındırılan hizmet örneği sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="2f471-193">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="2f471-194">Bir Zamanlayıcı üzerinde çalışan arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="2f471-194">Background task that runs on a timer.</span></span>
* <span data-ttu-id="2f471-195">[Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="2f471-195">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="2f471-196">Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanabilir</span><span class="sxs-lookup"><span data-stu-id="2f471-196">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="2f471-197">Sıralı olarak çalışan sıraya alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="2f471-197">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="2f471-198">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2f471-198">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2f471-199">Örnek uygulama iki sürümde sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="2f471-199">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="2f471-200">Web ana &ndash; bilgisayarı Web ana bilgisayarı Web uygulamalarını barındırmak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="2f471-200">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="2f471-201">Bu konu başlığında gösterilen örnek kod, örneğin web ana bilgisayar sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="2f471-201">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="2f471-202">Daha fazla bilgi için bkz. [Web ana bilgisayarı](xref:fundamentals/host/web-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="2f471-202">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="2f471-203">Genel ana &ndash; bilgisayar genel ana bilgisayarı ASP.NET Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="2f471-203">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="2f471-204">Daha fazla bilgi için bkz. [genel ana bilgisayar](xref:fundamentals/host/generic-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="2f471-204">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="2f471-205">Paket</span><span class="sxs-lookup"><span data-stu-id="2f471-205">Package</span></span>

<span data-ttu-id="2f471-206">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2f471-206">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="2f471-207">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="2f471-207">IHostedService interface</span></span>

<span data-ttu-id="2f471-208">Barındırılan hizmetler, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="2f471-208">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="2f471-209">Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="2f471-209">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="2f471-210">[Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; arkaplan`StartAsync` görevinin başlatılacağı mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="2f471-210">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="2f471-211">[Web konağını](xref:fundamentals/host/web-host) `StartAsync` kullanırken sunucu başlatıldıktan ve [ıapplicationlifetime değerinden sonra çağrılır. applicationstarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="2f471-211">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="2f471-212">[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanılırken, `StartAsync` tetiklendikten önce `ApplicationStarted` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="2f471-212">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="2f471-213">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="2f471-213">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="2f471-214">`StopAsync`arka plan görevinin bitiş mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="2f471-214">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="2f471-215">Yönetilmeyen <xref:System.IDisposable> kaynakların atılmaya yönelik uygulama ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) .</span><span class="sxs-lookup"><span data-stu-id="2f471-215">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="2f471-216">İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="2f471-216">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="2f471-217">Belirteç üzerinde iptal istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="2f471-217">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="2f471-218">Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="2f471-218">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="2f471-219">İçinde `StopAsync` çağrılan tüm yöntemler hemen döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="2f471-219">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="2f471-220">Ancak, bir iptal işlemi istendikten&mdash;sonra görevler terk edilmez ve bu, çağıran tüm görevlerin tamamlanmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="2f471-220">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="2f471-221">Uygulama beklenmedik bir şekilde kapanırsa (örneğin, uygulamanın işlemi başarısız olur), `StopAsync` çağrılmayabilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-221">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="2f471-222">Bu nedenle, veya üzerinde `StopAsync` gerçekleştirilen işlemleri çağıran Yöntemler gerçekleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-222">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="2f471-223">Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="2f471-223">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="2f471-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>Genel ana bilgisayar kullanırken.</span><span class="sxs-lookup"><span data-stu-id="2f471-224"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="2f471-225">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2f471-225">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="2f471-226">Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="2f471-226">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="2f471-227">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="2f471-227">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="2f471-228">Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır.</span><span class="sxs-lookup"><span data-stu-id="2f471-228">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="2f471-229">Arka plan görevinin yürütülmesi sırasında bir hata oluşursa, `Dispose` çağrılmadıysa `StopAsync` bile çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2f471-229">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="2f471-230">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="2f471-230">Timed background tasks</span></span>

<span data-ttu-id="2f471-231">Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f471-231">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="2f471-232">Zamanlayıcı, görevin `DoWork` metodunu tetikler.</span><span class="sxs-lookup"><span data-stu-id="2f471-232">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="2f471-233">Zamanlayıcı devre dışı bırakılır `StopAsync` ve hizmet kapsayıcısı `Dispose`bırakıldığında bırakıldı:</span><span class="sxs-lookup"><span data-stu-id="2f471-233">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="2f471-234">Hizmet, `AddHostedService` uzantı yöntemiyle kaydedilir `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="2f471-234">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="2f471-235">Bir arka plan görevinde kapsamlı bir hizmeti kullanma</span><span class="sxs-lookup"><span data-stu-id="2f471-235">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="2f471-236">[Kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) bir `IHostedService`içinde kullanmak için bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2f471-236">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="2f471-237">Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="2f471-237">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="2f471-238">Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="2f471-238">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="2f471-239">Aşağıdaki örnekte, hizmetine bir <xref:Microsoft.Extensions.Logging.ILogger> hizmet eklenmiş olur:</span><span class="sxs-lookup"><span data-stu-id="2f471-239">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="2f471-240">Barındırılan hizmet, kendi `DoWork` metodunu çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur:</span><span class="sxs-lookup"><span data-stu-id="2f471-240">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="2f471-241">Hizmetler ' de `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-241">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2f471-242">Uygulama, `AddHostedService` uzantı yöntemiyle kaydedilir: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="2f471-242">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="2f471-243">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="2f471-243">Queued background tasks</span></span>

<span data-ttu-id="2f471-244">Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 'i temel alır ([ASP.NET Core için isteğe](https://github.com/aspnet/Hosting/issues/1280)bağlı olarak zamanlanmış olarak zamanlandı):</span><span class="sxs-lookup"><span data-stu-id="2f471-244">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="2f471-245">' `QueueHostedService`De, kuyruktaki arka plan görevleri <xref:Microsoft.Extensions.Hosting.BackgroundService>, uzun süre çalışan `IHostedService`uygulama için temel bir sınıf olan olarak kuyruğa alınır ve yürütülür.</span><span class="sxs-lookup"><span data-stu-id="2f471-245">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="2f471-246">Hizmetler ' de `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="2f471-246">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="2f471-247">Uygulama, `AddHostedService` uzantı yöntemiyle kaydedilir: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="2f471-247">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="2f471-248">Dizin sayfası model sınıfında:</span><span class="sxs-lookup"><span data-stu-id="2f471-248">In the Index page model class:</span></span>

* <span data-ttu-id="2f471-249">Oluşturucuya eklenir ve öğesine `Queue`atanır. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="2f471-249">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="2f471-250">Eklenen ve öğesine `_serviceScopeFactory`atandı. <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory></span><span class="sxs-lookup"><span data-stu-id="2f471-250">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="2f471-251">Fabrika, bir kapsam içinde hizmet oluşturmak için <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>kullanılan örnekleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2f471-251">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="2f471-252">Uygulama `AppDbContext` (bir tekil hizmet) içinde `IBackgroundTaskQueue` veritabanı kayıtları yazmak için uygulamanın ( [kapsamlı bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes)) kullanılabilmesi için bir kapsam oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2f471-252">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="2f471-253">Dizin `OnPostAddTask` sayfasında **Görev Ekle** düğmesi seçildiğinde Yöntem yürütülür.</span><span class="sxs-lookup"><span data-stu-id="2f471-253">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="2f471-254">`QueueBackgroundWorkItem`bir iş öğesini kuyruğa almak için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="2f471-254">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2f471-255">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2f471-255">Additional resources</span></span>

* [<span data-ttu-id="2f471-256">Ihostedservice ve BackgroundService sınıfıyla mikro hizmetlerde arka plan görevleri uygulama</span><span class="sxs-lookup"><span data-stu-id="2f471-256">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
