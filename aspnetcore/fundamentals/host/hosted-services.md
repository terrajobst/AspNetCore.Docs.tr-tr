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
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="da900-103">ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="da900-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="da900-104">, [Luke Latham](https://github.com/guardrex) ve [Jeow li Hua](https://github.com/huan086) tarafından</span><span class="sxs-lookup"><span data-stu-id="da900-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="da900-105">ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="da900-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="da900-106">Barındırılan bir hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="da900-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="da900-107">Bu konuda üç barındırılan hizmet örneği sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="da900-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="da900-108">Bir Zamanlayıcı üzerinde çalışan arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="da900-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="da900-109">[Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="da900-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="da900-110">Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="da900-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="da900-111">Sıralı olarak çalışan sıraya alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="da900-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="da900-112">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="da900-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="da900-113">Örnek uygulama iki sürümde sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="da900-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="da900-114">Web ana bilgisayarı &ndash; Web ana bilgisayarı Web uygulamalarını barındırmak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="da900-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="da900-115">Bu konu başlığında gösterilen örnek kod, örneğin web ana bilgisayar sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="da900-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="da900-116">Daha fazla bilgi için bkz. [Web ana bilgisayarı](xref:fundamentals/host/web-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="da900-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="da900-117">Genel ana bilgisayar &ndash; genel ana bilgisayar ASP.NET Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="da900-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="da900-118">Daha fazla bilgi için bkz. [genel ana bilgisayar](xref:fundamentals/host/generic-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="da900-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="da900-119">Çalışan hizmeti şablonu</span><span class="sxs-lookup"><span data-stu-id="da900-119">Worker Service template</span></span>

<span data-ttu-id="da900-120">ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="da900-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="da900-121">Şablonu bir barındırılan hizmetler uygulamasının temeli olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="da900-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="da900-122">Paket</span><span class="sxs-lookup"><span data-stu-id="da900-122">Package</span></span>

<span data-ttu-id="da900-123">[Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine yönelik bir paket başvurusu, ASP.NET Core uygulamalar için örtük olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="da900-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="da900-124">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="da900-124">IHostedService interface</span></span>

<span data-ttu-id="da900-125">@No__t-0 arabirimi, konak tarafından yönetilen nesneler için iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="da900-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="da900-126">[Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevini başlatma mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="da900-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="da900-127">`StartAsync` şu kadar *çağrılır:*</span><span class="sxs-lookup"><span data-stu-id="da900-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="da900-128">Uygulamanın istek işleme işlem hattı yapılandırıldı (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="da900-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="da900-129">Sunucu başlatıldı ve [ıapplicationlifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="da900-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="da900-130">Varsayılan davranış, barındırılan hizmetin `StartAsync` ' ı, uygulamanın işlem hattı yapılandırıldıktan ve `ApplicationStarted` çağrıldıktan sonra çalışacak şekilde değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="da900-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="da900-131">Varsayılan davranışı değiştirmek için, `ConfigureWebHostDefaults` ' i çağırdıktan sonra barındırılan hizmeti (aşağıdaki örnekte `VideosWatcher`) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="da900-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="da900-132">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) @no__t-konak düzgün kapanma yaparken tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="da900-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="da900-133">`StopAsync`, arka plan görevinin bitiş mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="da900-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="da900-134">Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcıları (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.</span><span class="sxs-lookup"><span data-stu-id="da900-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="da900-135">İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="da900-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="da900-136">Belirteç üzerinde iptal istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="da900-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="da900-137">Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="da900-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="da900-138">@No__t-0 ' da çağrılan tüm yöntemler hemen döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="da900-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="da900-139">Ancak, iptal edildikten sonra görevler terk edilmez @ no__t-0çağıran tüm görevlerin tamamlanmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="da900-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="da900-140">Uygulama beklenmedik bir şekilde kapanırsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrılmayabilir.</span><span class="sxs-lookup"><span data-stu-id="da900-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="da900-141">Bu nedenle, `StopAsync` ' da yürütülen tüm yöntemler veya işlem gerçekleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="da900-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="da900-142">Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="da900-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="da900-143">Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>.</span><span class="sxs-lookup"><span data-stu-id="da900-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="da900-144">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="da900-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="da900-145">Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="da900-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="da900-146">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="da900-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="da900-147">Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır.</span><span class="sxs-lookup"><span data-stu-id="da900-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="da900-148">Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="da900-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="da900-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="da900-149">BackgroundService</span></span>

<span data-ttu-id="da900-150">`BackgroundService`, uzun süre çalışan <xref:Microsoft.Extensions.Hosting.IHostedService> uygulamak için bir temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="da900-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="da900-151">`BackgroundService` ' ın, hizmetin mantığını içermesi için `ExecuteAsync(CancellationToken stoppingToken)` soyut yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="da900-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="da900-152">@No__t-0, [ıhostedservice. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) çağrıldığında tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="da900-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="da900-153">Bu yöntemin uygulanması, arka plan hizmetinin tüm ömrünü temsil eden bir `Task` döndürür.</span><span class="sxs-lookup"><span data-stu-id="da900-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="da900-154">Ayrıca, *isteğe bağlı* olarak, hizmetiniz için başlatma ve kapatılma kodunu çalıştırmak üzere `IHostedService` ' de tanımlanan yöntemleri geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="da900-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="da900-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync`, uygulama konağı düzgün kapanma gerçekleştirirken çağrılır.</span><span class="sxs-lookup"><span data-stu-id="da900-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="da900-156">@No__t-0, ana bilgisayar hizmeti zorla sonlandırmaya karar verdiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="da900-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="da900-157">Bu yöntem geçersiz kılınırsa, hizmetin düzgün bir şekilde kapatılmasını sağlamak için temel sınıf yöntemini (ve `await`) çağırmanız **gerekir** .</span><span class="sxs-lookup"><span data-stu-id="da900-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="da900-158">arka plan hizmetini başlatmak için `StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` çağırılır.</span><span class="sxs-lookup"><span data-stu-id="da900-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="da900-159">Başlangıç işlemi kesintiye uğrarsa `cancellationToken` ' a işaret edilir.</span><span class="sxs-lookup"><span data-stu-id="da900-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="da900-160">Uygulama, hizmetin başlatma sürecini temsil eden bir `Task` döndürür.</span><span class="sxs-lookup"><span data-stu-id="da900-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="da900-161">Bu @no__t 0 tamamlanana kadar başka hizmet başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="da900-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="da900-162">Bu yöntem geçersiz kılınırsa, hizmetin düzgün şekilde başlamasını sağlamak için temel sınıf yöntemini (ve `await`) çağırmanız **gerekir** .</span><span class="sxs-lookup"><span data-stu-id="da900-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="da900-163">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="da900-163">Timed background tasks</span></span>

<span data-ttu-id="da900-164">Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="da900-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="da900-165">Zamanlayıcı, görevin `DoWork` yöntemini tetikler.</span><span class="sxs-lookup"><span data-stu-id="da900-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="da900-166">Süreölçer `StopAsync` ' da devre dışı bırakılır ve hizmet kapsayıcısı `Dispose` ' de bırakıldığında bırakıldı:</span><span class="sxs-lookup"><span data-stu-id="da900-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="da900-167">Hizmet, `AddHostedService` uzantı yöntemiyle `IHostBuilder.ConfigureServices` (*program.cs*) öğesine kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="da900-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="da900-168">Bir arka plan görevinde kapsamlı bir hizmeti kullanma</span><span class="sxs-lookup"><span data-stu-id="da900-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="da900-169">@No__t-1 içinde [kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) kullanmak için bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da900-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="da900-170">Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="da900-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="da900-171">Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="da900-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="da900-172">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="da900-172">In the following example:</span></span>

* <span data-ttu-id="da900-173">Hizmet zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="da900-173">The service is asynchronous.</span></span> <span data-ttu-id="da900-174">@No__t-0 yöntemi `Task` döndürür.</span><span class="sxs-lookup"><span data-stu-id="da900-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="da900-175">Tanıtım amacıyla, on saniyelik bir gecikme `DoWork` yönteminde bekletildi.</span><span class="sxs-lookup"><span data-stu-id="da900-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="da900-176">@No__t-0, hizmete eklenir.</span><span class="sxs-lookup"><span data-stu-id="da900-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="da900-177">Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur.</span><span class="sxs-lookup"><span data-stu-id="da900-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="da900-178">`DoWork`, `ExecuteAsync` ' de beklemiş olan bir `Task` döndürür:</span><span class="sxs-lookup"><span data-stu-id="da900-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="da900-179">Hizmetler `IHostBuilder.ConfigureServices` ' a (*program.cs*) kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="da900-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="da900-180">Barındırılan hizmet `AddHostedService` genişletme yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="da900-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="da900-181">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="da900-181">Queued background tasks</span></span>

<span data-ttu-id="da900-182">Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ' ı temel alır ([ASP.NET Core için yerleşik](https://github.com/aspnet/Hosting/issues/1280)olarak, geçici olarak zamanlandı):</span><span class="sxs-lookup"><span data-stu-id="da900-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="da900-183">Aşağıdaki `QueueHostedService` örnek:</span><span class="sxs-lookup"><span data-stu-id="da900-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="da900-184">@No__t-0 yöntemi, `ExecuteAsync` ' de beklemiş olan bir `Task` döndürür.</span><span class="sxs-lookup"><span data-stu-id="da900-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="da900-185">Kuyruktaki arka plan görevleri, `BackgroundProcessing` ' da kuyruğa alınır ve yürütülür.</span><span class="sxs-lookup"><span data-stu-id="da900-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="da900-186">@No__t-0 ' da hizmet durdurulmadan önce iş öğeleri bekletildi.</span><span class="sxs-lookup"><span data-stu-id="da900-186">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="da900-187">@No__t-0 hizmeti, giriş cihazında `w` anahtarı seçildiğinde barındırılan hizmet için görevleri sıraya alır:</span><span class="sxs-lookup"><span data-stu-id="da900-187">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="da900-188">@No__t-0 `MonitorLoop` hizmetine eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="da900-188">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="da900-189">`IBackgroundTaskQueue.QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="da900-189">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="da900-190">Çalışma öğesi uzun süre çalışan bir arka plan görevinin benzetimini yapar:</span><span class="sxs-lookup"><span data-stu-id="da900-190">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="da900-191">Üç 5-ikinci gecikme yürütülür (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="da900-191">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="da900-192">@No__t-0 ifadesinde, görev iptal edilirse, <xref:System.OperationCanceledException> tuzakları yakalar.</span><span class="sxs-lookup"><span data-stu-id="da900-192">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="da900-193">Hizmetler `IHostBuilder.ConfigureServices` ' a (*program.cs*) kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="da900-193">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="da900-194">Barındırılan hizmet `AddHostedService` genişletme yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="da900-194">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="da900-195">ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="da900-195">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="da900-196">Barındırılan bir hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="da900-196">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="da900-197">Bu konuda üç barındırılan hizmet örneği sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="da900-197">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="da900-198">Bir Zamanlayıcı üzerinde çalışan arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="da900-198">Background task that runs on a timer.</span></span>
* <span data-ttu-id="da900-199">[Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="da900-199">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="da900-200">Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanabilir</span><span class="sxs-lookup"><span data-stu-id="da900-200">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="da900-201">Sıralı olarak çalışan sıraya alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="da900-201">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="da900-202">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="da900-202">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="da900-203">Örnek uygulama iki sürümde sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="da900-203">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="da900-204">Web ana bilgisayarı &ndash; Web ana bilgisayarı Web uygulamalarını barındırmak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="da900-204">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="da900-205">Bu konu başlığında gösterilen örnek kod, örneğin web ana bilgisayar sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="da900-205">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="da900-206">Daha fazla bilgi için bkz. [Web ana bilgisayarı](xref:fundamentals/host/web-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="da900-206">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="da900-207">Genel ana bilgisayar &ndash; genel ana bilgisayar ASP.NET Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="da900-207">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="da900-208">Daha fazla bilgi için bkz. [genel ana bilgisayar](xref:fundamentals/host/generic-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="da900-208">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="da900-209">Paket</span><span class="sxs-lookup"><span data-stu-id="da900-209">Package</span></span>

<span data-ttu-id="da900-210">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="da900-210">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="da900-211">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="da900-211">IHostedService interface</span></span>

<span data-ttu-id="da900-212">Barındırılan hizmetler <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="da900-212">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="da900-213">Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="da900-213">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="da900-214">[Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevini başlatma mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="da900-214">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="da900-215">[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanılırken, sunucu başlatıldıktan ve ıapplicationlifetime 'a `StartAsync` çağrılır [. applicationstarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="da900-215">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="da900-216">[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanılırken, `ApplicationStarted` tetiklenene kadar `StartAsync` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="da900-216">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="da900-217">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) @no__t-konak düzgün kapanma yaparken tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="da900-217">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="da900-218">`StopAsync`, arka plan görevinin bitiş mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="da900-218">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="da900-219">Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcıları (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.</span><span class="sxs-lookup"><span data-stu-id="da900-219">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="da900-220">İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="da900-220">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="da900-221">Belirteç üzerinde iptal istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="da900-221">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="da900-222">Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="da900-222">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="da900-223">@No__t-0 ' da çağrılan tüm yöntemler hemen döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="da900-223">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="da900-224">Ancak, iptal edildikten sonra görevler terk edilmez @ no__t-0çağıran tüm görevlerin tamamlanmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="da900-224">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="da900-225">Uygulama beklenmedik bir şekilde kapanırsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrılmayabilir.</span><span class="sxs-lookup"><span data-stu-id="da900-225">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="da900-226">Bu nedenle, `StopAsync` ' da yürütülen tüm yöntemler veya işlem gerçekleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="da900-226">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="da900-227">Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="da900-227">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="da900-228">Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>.</span><span class="sxs-lookup"><span data-stu-id="da900-228"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="da900-229">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="da900-229">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="da900-230">Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="da900-230">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="da900-231">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="da900-231">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="da900-232">Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır.</span><span class="sxs-lookup"><span data-stu-id="da900-232">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="da900-233">Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="da900-233">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="da900-234">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="da900-234">Timed background tasks</span></span>

<span data-ttu-id="da900-235">Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="da900-235">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="da900-236">Zamanlayıcı, görevin `DoWork` yöntemini tetikler.</span><span class="sxs-lookup"><span data-stu-id="da900-236">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="da900-237">Süreölçer `StopAsync` ' da devre dışı bırakılır ve hizmet kapsayıcısı `Dispose` ' de bırakıldığında bırakıldı:</span><span class="sxs-lookup"><span data-stu-id="da900-237">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="da900-238">Hizmet, `AddHostedService` uzantı yöntemiyle `Startup.ConfigureServices` ' a kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="da900-238">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="da900-239">Bir arka plan görevinde kapsamlı bir hizmeti kullanma</span><span class="sxs-lookup"><span data-stu-id="da900-239">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="da900-240">@No__t-1 içinde [kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) kullanmak için bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="da900-240">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="da900-241">Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="da900-241">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="da900-242">Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="da900-242">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="da900-243">Aşağıdaki örnekte, hizmetine bir <xref:Microsoft.Extensions.Logging.ILogger> eklenir:</span><span class="sxs-lookup"><span data-stu-id="da900-243">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="da900-244">Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur:</span><span class="sxs-lookup"><span data-stu-id="da900-244">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="da900-245">Hizmetler `Startup.ConfigureServices` ' a kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="da900-245">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="da900-246">@No__t 0 uygulama `AddHostedService` uzantı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="da900-246">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="da900-247">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="da900-247">Queued background tasks</span></span>

<span data-ttu-id="da900-248">Arka plan görev kuyruğu .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ' dır ([ASP.NET Core için](https://github.com/aspnet/Hosting/issues/1280)isteğe bağlı olarak zamanlanmış olarak zamanlandı):</span><span class="sxs-lookup"><span data-stu-id="da900-248">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="da900-249">@No__t-0 ' da, kuyruktaki arka plan görevleri kuyruğa alınır ve bir <xref:Microsoft.Extensions.Hosting.BackgroundService> olarak yürütülür ve bu, uzun süre çalışan bir `IHostedService` ' yi uygulamaya yönelik temel bir sınıftır:</span><span class="sxs-lookup"><span data-stu-id="da900-249">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="da900-250">Hizmetler `Startup.ConfigureServices` ' a kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="da900-250">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="da900-251">@No__t 0 uygulama `AddHostedService` uzantı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="da900-251">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="da900-252">Dizin sayfası model sınıfında:</span><span class="sxs-lookup"><span data-stu-id="da900-252">In the Index page model class:</span></span>

* <span data-ttu-id="da900-253">@No__t-0 oluşturucuya eklenir ve `Queue` ' e atanır.</span><span class="sxs-lookup"><span data-stu-id="da900-253">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="da900-254">@No__t-0, `_serviceScopeFactory` ' e eklenir ve atanır.</span><span class="sxs-lookup"><span data-stu-id="da900-254">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="da900-255">Fabrika, bir kapsam içinde hizmet oluşturmak için kullanılan <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> örnekleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="da900-255">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="da900-256">@No__t-2 ' deki (tek bir hizmet) veritabanı kayıtlarını yazmak için uygulamanın `AppDbContext` ( [kapsamlı bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes)) kullanmak üzere bir kapsam oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="da900-256">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="da900-257">Dizin sayfasında **Görev Ekle** düğmesi seçildiğinde `OnPostAddTask` yöntemi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="da900-257">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="da900-258">`QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="da900-258">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="da900-259">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="da900-259">Additional resources</span></span>

* [<span data-ttu-id="da900-260">Ihostedservice ve BackgroundService sınıfıyla mikro hizmetlerde arka plan görevleri uygulama</span><span class="sxs-lookup"><span data-stu-id="da900-260">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
