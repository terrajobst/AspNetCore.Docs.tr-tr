---
title: ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri
author: guardrex
description: ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/19/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: da3c2679005714a3d82de94cf3bc3c809aa3500d
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239729"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="aa269-103">ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="aa269-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="aa269-104">, [Luke Latham](https://github.com/guardrex) ve [Jeow li Hua](https://github.com/huan086) tarafından</span><span class="sxs-lookup"><span data-stu-id="aa269-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="aa269-105">ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="aa269-106">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="aa269-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="aa269-107">Bu konuda üç barındırılan hizmet örneği sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="aa269-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="aa269-108">Bir Zamanlayıcı üzerinde çalışan arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="aa269-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="aa269-109">[Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="aa269-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="aa269-110">Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="aa269-111">Sıralı olarak çalışan sıraya alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="aa269-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="aa269-112">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aa269-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="aa269-113">Çalışan hizmeti şablonu</span><span class="sxs-lookup"><span data-stu-id="aa269-113">Worker Service template</span></span>

<span data-ttu-id="aa269-114">ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="aa269-114">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="aa269-115">Çalışan hizmeti şablonundan oluşturulan bir uygulama, çalışan SDK 'sını proje dosyasında belirtir:</span><span class="sxs-lookup"><span data-stu-id="aa269-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="aa269-116">Şablonu bir barındırılan hizmetler uygulamasının temeli olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="aa269-116">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="aa269-117">Paket</span><span class="sxs-lookup"><span data-stu-id="aa269-117">Package</span></span>

<span data-ttu-id="aa269-118">Çalışan hizmeti şablonunu temel alan bir uygulama `Microsoft.NET.Sdk.Worker` SDK kullanır ve [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine açık bir paket başvurusu içerir.</span><span class="sxs-lookup"><span data-stu-id="aa269-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="aa269-119">Örneğin, örnek uygulamanın proje dosyasına (*Backgroundtaskssample. csproj*) bakın.</span><span class="sxs-lookup"><span data-stu-id="aa269-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="aa269-120">`Microsoft.NET.Sdk.Web` SDK kullanan Web uygulamaları için, [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine, paylaşılan çerçeveden dolaylı olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="aa269-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="aa269-121">Uygulamanın proje dosyasındaki açık bir paket başvurusu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="aa269-121">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="aa269-122">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="aa269-122">IHostedService interface</span></span>

<span data-ttu-id="aa269-123"><xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi, konak tarafından yönetilen nesneler için iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="aa269-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="aa269-124">[Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevinin başlatılacağı mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="aa269-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="aa269-125">`StartAsync` şu kadar *çağrılır:*</span><span class="sxs-lookup"><span data-stu-id="aa269-125">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="aa269-126">Uygulamanın istek işleme işlem hattı yapılandırıldı (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="aa269-126">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="aa269-127">Sunucu başlatıldı ve [ıapplicationlifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="aa269-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="aa269-128">Varsayılan davranış, barındırılan hizmetin `StartAsync`, uygulamanın işlem hattı yapılandırıldıktan ve `ApplicationStarted` çağrıldıktan sonra çalışması için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="aa269-129">Varsayılan davranışı değiştirmek için, `ConfigureWebHostDefaults`çağrıldıktan sonra barındırılan hizmeti (aşağıdaki örnekte`VideosWatcher`) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="aa269-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="aa269-130">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="aa269-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="aa269-131">`StopAsync` arka plan görevinin sonundaki mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="aa269-131">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="aa269-132">Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.</span><span class="sxs-lookup"><span data-stu-id="aa269-132">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="aa269-133">İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="aa269-133">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="aa269-134">Belirteç üzerinde iptal istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="aa269-134">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="aa269-135">Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="aa269-135">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="aa269-136">`StopAsync` içinde çağrılan yöntemler hemen döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="aa269-136">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="aa269-137">Ancak,&mdash;istek iptal edildikten sonra, çağıran tüm görevlerin tamamlanmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="aa269-137">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="aa269-138">Uygulama beklenmedik şekilde kapanıyorsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrımayabilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-138">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="aa269-139">Bu nedenle, `StopAsync` veya içinde gerçekleştirilen işlemler tarafından çağrılan yöntemler gerçekleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-139">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="aa269-140">Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="aa269-140">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="aa269-141">Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>.</span><span class="sxs-lookup"><span data-stu-id="aa269-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="aa269-142">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="aa269-142">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="aa269-143">Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="aa269-143">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="aa269-144">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="aa269-144">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="aa269-145">Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır.</span><span class="sxs-lookup"><span data-stu-id="aa269-145">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="aa269-146">Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="aa269-146">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice-base-class"></a><span data-ttu-id="aa269-147">BackgroundService temel sınıfı</span><span class="sxs-lookup"><span data-stu-id="aa269-147">BackgroundService base class</span></span>

<span data-ttu-id="aa269-148"><xref:Microsoft.Extensions.Hosting.BackgroundService>, uzun süre çalışan bir <xref:Microsoft.Extensions.Hosting.IHostedService>uygulamaya yönelik temel bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="aa269-148"><xref:Microsoft.Extensions.Hosting.BackgroundService> is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span>

<span data-ttu-id="aa269-149">Arka plan hizmetini çalıştırmak için [ExecuteAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) çağırılır.</span><span class="sxs-lookup"><span data-stu-id="aa269-149">[ExecuteAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync*) is called to run the background service.</span></span> <span data-ttu-id="aa269-150">Uygulama, arka plan hizmetinin tüm ömrünü temsil eden bir <xref:System.Threading.Tasks.Task> döndürür.</span><span class="sxs-lookup"><span data-stu-id="aa269-150">The implementation returns a <xref:System.Threading.Tasks.Task> that represents the entire lifetime of the background service.</span></span> <span data-ttu-id="aa269-151">ExecuteAsync, `await`çağırarak [zaman uyumsuz hale](https://github.com/aspnet/Extensions/issues/2149)gelene kadar başka bir hizmet başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="aa269-151">No further services are started until [ExecuteAsync becomes asynchronous](https://github.com/aspnet/Extensions/issues/2149), such as by calling `await`.</span></span> <span data-ttu-id="aa269-152">`ExecuteAsync`içinde başlatma işini uzun süre gerçekleştirmekten kaçının.</span><span class="sxs-lookup"><span data-stu-id="aa269-152">Avoid performing long, blocking initialization work in `ExecuteAsync`.</span></span> <span data-ttu-id="aa269-153">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) içindeki ana bilgisayar blokları `ExecuteAsync` tamamlanmasını bekliyor.</span><span class="sxs-lookup"><span data-stu-id="aa269-153">The host blocks in [StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.BackgroundService.StopAsync*) waiting for `ExecuteAsync` to complete.</span></span>

<span data-ttu-id="aa269-154">[Ihostedservice. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) çağrıldığında iptal belirteci tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="aa269-154">The cancellation token is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="aa269-155">`ExecuteAsync` uygulamanız, hizmeti sorunsuz bir şekilde kapatmak için iptal belirteci tetiklendiğinde hemen bitebilmelidir.</span><span class="sxs-lookup"><span data-stu-id="aa269-155">Your implementation of `ExecuteAsync` should finish promptly when the cancellation token is fired in order to gracefully shut down the service.</span></span> <span data-ttu-id="aa269-156">Aksi takdirde, hizmet, kapanmanın zaman aşımından sonra kapanmadığını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="aa269-156">Otherwise, the service ungracefully shuts down at the shutdown timeout.</span></span> <span data-ttu-id="aa269-157">Daha fazla bilgi için [ıhostedservice arabirimi](#ihostedservice-interface) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="aa269-157">For more information, see the [IHostedService interface](#ihostedservice-interface) section.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="aa269-158">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="aa269-158">Timed background tasks</span></span>

<span data-ttu-id="aa269-159">Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="aa269-159">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="aa269-160">Zamanlayıcı, görevin `DoWork` yöntemini tetikler.</span><span class="sxs-lookup"><span data-stu-id="aa269-160">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="aa269-161">Zamanlayıcı `StopAsync` devre dışı bırakılır ve hizmet kapsayıcısı `Dispose`bırakıldığında atıldı:</span><span class="sxs-lookup"><span data-stu-id="aa269-161">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="aa269-162">Hizmet, `AddHostedService` uzantısı yöntemiyle `IHostBuilder.ConfigureServices` (*program.cs*) kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="aa269-162">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="aa269-163">Bir arka plan görevinde kapsamlı bir hizmeti kullanma</span><span class="sxs-lookup"><span data-stu-id="aa269-163">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="aa269-164">Bir [backgroundservice](#backgroundservice-base-class)içinde [kapsamlı hizmetler](xref:fundamentals/dependency-injection#service-lifetimes) kullanmak için bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="aa269-164">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a [BackgroundService](#backgroundservice-base-class), create a scope.</span></span> <span data-ttu-id="aa269-165">Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="aa269-165">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="aa269-166">Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="aa269-166">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="aa269-167">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="aa269-167">In the following example:</span></span>

* <span data-ttu-id="aa269-168">Hizmet zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="aa269-168">The service is asynchronous.</span></span> <span data-ttu-id="aa269-169">`DoWork` yöntemi bir `Task`döndürür.</span><span class="sxs-lookup"><span data-stu-id="aa269-169">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="aa269-170">Tanıtım amacıyla, `DoWork` yönteminde on saniyelik bir gecikme beklenir.</span><span class="sxs-lookup"><span data-stu-id="aa269-170">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="aa269-171">Hizmete bir <xref:Microsoft.Extensions.Logging.ILogger> eklenir.</span><span class="sxs-lookup"><span data-stu-id="aa269-171">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="aa269-172">Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur.</span><span class="sxs-lookup"><span data-stu-id="aa269-172">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="aa269-173">`DoWork`, `ExecuteAsync`beklemiş olan bir `Task`döndürür:</span><span class="sxs-lookup"><span data-stu-id="aa269-173">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="aa269-174">Hizmetler `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="aa269-174">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="aa269-175">Barındırılan hizmet `AddHostedService` uzantısı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="aa269-175">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="aa269-176">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="aa269-176">Queued background tasks</span></span>

<span data-ttu-id="aa269-177">Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> temel alır ([ASP.NET Core için yerleşik](https://github.com/aspnet/Hosting/issues/1280)olarak, kesin olarak zamanlandı):</span><span class="sxs-lookup"><span data-stu-id="aa269-177">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="aa269-178">Aşağıdaki `QueueHostedService` örnekte:</span><span class="sxs-lookup"><span data-stu-id="aa269-178">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="aa269-179">`BackgroundProcessing` yöntemi, `ExecuteAsync`beklemiş olan bir `Task`döndürür.</span><span class="sxs-lookup"><span data-stu-id="aa269-179">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="aa269-180">Kuyruktaki arka plan görevleri, `BackgroundProcessing`sırada kuyruğa alınır ve yürütülür.</span><span class="sxs-lookup"><span data-stu-id="aa269-180">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="aa269-181">İş öğeleri, hizmet `StopAsync`' de durdurulmadan önce bekletildi.</span><span class="sxs-lookup"><span data-stu-id="aa269-181">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="aa269-182">Bir `MonitorLoop` hizmeti, giriş cihazında `w` anahtarı seçildiğinde barındırılan hizmet için görevleri sıraya alır:</span><span class="sxs-lookup"><span data-stu-id="aa269-182">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="aa269-183">`IBackgroundTaskQueue`, `MonitorLoop` hizmetine eklenir.</span><span class="sxs-lookup"><span data-stu-id="aa269-183">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="aa269-184">`IBackgroundTaskQueue.QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="aa269-184">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="aa269-185">Çalışma öğesi uzun süre çalışan bir arka plan görevinin benzetimini yapar:</span><span class="sxs-lookup"><span data-stu-id="aa269-185">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="aa269-186">Üç 5-ikinci gecikme yürütülür (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="aa269-186">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="aa269-187">`try-catch` bir ifade, görev iptal edildiğinde <xref:System.OperationCanceledException> yakalar.</span><span class="sxs-lookup"><span data-stu-id="aa269-187">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="aa269-188">Hizmetler `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="aa269-188">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="aa269-189">Barındırılan hizmet `AddHostedService` uzantısı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="aa269-189">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="aa269-190">ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-190">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="aa269-191">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="aa269-191">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="aa269-192">Bu konuda üç barındırılan hizmet örneği sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="aa269-192">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="aa269-193">Bir Zamanlayıcı üzerinde çalışan arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="aa269-193">Background task that runs on a timer.</span></span>
* <span data-ttu-id="aa269-194">[Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="aa269-194">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="aa269-195">Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanabilir</span><span class="sxs-lookup"><span data-stu-id="aa269-195">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="aa269-196">Sıralı olarak çalışan sıraya alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="aa269-196">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="aa269-197">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aa269-197">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="aa269-198">Paket</span><span class="sxs-lookup"><span data-stu-id="aa269-198">Package</span></span>

<span data-ttu-id="aa269-199">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="aa269-199">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="aa269-200">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="aa269-200">IHostedService interface</span></span>

<span data-ttu-id="aa269-201">Barındırılan hizmetler <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="aa269-201">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="aa269-202">Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="aa269-202">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="aa269-203">[Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevinin başlatılacağı mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="aa269-203">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="aa269-204">[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanılırken, sunucu başlatıldıktan ve ıapplicationlifetime 'dan sonra `StartAsync` çağrılır [. applicationstarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="aa269-204">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="aa269-205">[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanılırken, `ApplicationStarted` tetiklenene kadar `StartAsync` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="aa269-205">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="aa269-206">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="aa269-206">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="aa269-207">`StopAsync` arka plan görevinin sonundaki mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="aa269-207">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="aa269-208">Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.</span><span class="sxs-lookup"><span data-stu-id="aa269-208">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="aa269-209">İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="aa269-209">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="aa269-210">Belirteç üzerinde iptal istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="aa269-210">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="aa269-211">Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="aa269-211">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="aa269-212">`StopAsync` içinde çağrılan yöntemler hemen döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="aa269-212">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="aa269-213">Ancak,&mdash;istek iptal edildikten sonra, çağıran tüm görevlerin tamamlanmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="aa269-213">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="aa269-214">Uygulama beklenmedik şekilde kapanıyorsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrımayabilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-214">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="aa269-215">Bu nedenle, `StopAsync` veya içinde gerçekleştirilen işlemler tarafından çağrılan yöntemler gerçekleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-215">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="aa269-216">Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="aa269-216">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="aa269-217">Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>.</span><span class="sxs-lookup"><span data-stu-id="aa269-217"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="aa269-218">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="aa269-218">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="aa269-219">Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="aa269-219">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="aa269-220">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="aa269-220">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="aa269-221">Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır.</span><span class="sxs-lookup"><span data-stu-id="aa269-221">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="aa269-222">Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="aa269-222">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="aa269-223">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="aa269-223">Timed background tasks</span></span>

<span data-ttu-id="aa269-224">Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="aa269-224">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="aa269-225">Zamanlayıcı, görevin `DoWork` yöntemini tetikler.</span><span class="sxs-lookup"><span data-stu-id="aa269-225">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="aa269-226">Zamanlayıcı `StopAsync` devre dışı bırakılır ve hizmet kapsayıcısı `Dispose`bırakıldığında atıldı:</span><span class="sxs-lookup"><span data-stu-id="aa269-226">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="aa269-227">Hizmet, `AddHostedService` uzantısı yöntemiyle `Startup.ConfigureServices` kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="aa269-227">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="aa269-228">Bir arka plan görevinde kapsamlı bir hizmeti kullanma</span><span class="sxs-lookup"><span data-stu-id="aa269-228">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="aa269-229">[Kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) bir `IHostedService`içinde kullanmak için bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="aa269-229">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="aa269-230">Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="aa269-230">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="aa269-231">Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="aa269-231">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="aa269-232">Aşağıdaki örnekte, hizmetine bir <xref:Microsoft.Extensions.Logging.ILogger> eklenmiş olur:</span><span class="sxs-lookup"><span data-stu-id="aa269-232">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="aa269-233">Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur:</span><span class="sxs-lookup"><span data-stu-id="aa269-233">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="aa269-234">Hizmetler `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-234">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="aa269-235">`IHostedService` uygulama `AddHostedService` uzantısı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="aa269-235">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="aa269-236">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="aa269-236">Queued background tasks</span></span>

<span data-ttu-id="aa269-237">Arka plan görev kuyruğu .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> temel alır ([ASP.NET Core için kesin olarak zamanlandı](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="aa269-237">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="aa269-238">`QueueHostedService`, sıradaki arka plan görevleri, uzun süre çalışan bir `IHostedService`uygulamaya yönelik temel bir sınıf olan bir [Backgroundservice](#backgroundservice-base-class)olarak kuyruklanmış ve yürütülür:</span><span class="sxs-lookup"><span data-stu-id="aa269-238">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a [BackgroundService](#backgroundservice-base-class), which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="aa269-239">Hizmetler `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="aa269-239">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="aa269-240">`IHostedService` uygulama `AddHostedService` uzantısı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="aa269-240">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="aa269-241">Dizin sayfası model sınıfında:</span><span class="sxs-lookup"><span data-stu-id="aa269-241">In the Index page model class:</span></span>

* <span data-ttu-id="aa269-242">`IBackgroundTaskQueue` oluşturucuya eklenir ve `Queue`atanır.</span><span class="sxs-lookup"><span data-stu-id="aa269-242">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="aa269-243"><xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> eklenen ve `_serviceScopeFactory`atanmış.</span><span class="sxs-lookup"><span data-stu-id="aa269-243">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="aa269-244">Fabrika, bir kapsam içinde hizmet oluşturmak için kullanılan <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>örnekleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="aa269-244">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="aa269-245">Bir kapsam, veritabanı kayıtlarını `IBackgroundTaskQueue` (bir tek hizmet) yazmak için uygulamanın `AppDbContext` ( [kapsamlı bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes)) kullanmak üzere oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="aa269-245">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="aa269-246">Dizin sayfasında **Görev Ekle** düğmesi seçildiğinde `OnPostAddTask` yöntemi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="aa269-246">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="aa269-247">`QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="aa269-247">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="aa269-248">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="aa269-248">Additional resources</span></span>

* [<span data-ttu-id="aa269-249">Ihostedservice ve BackgroundService sınıfıyla mikro hizmetlerde arka plan görevleri uygulama</span><span class="sxs-lookup"><span data-stu-id="aa269-249">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
