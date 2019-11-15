---
title: ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri
author: guardrex
description: ASP.NET Core içinde barındırılan hizmetlerle arka plan görevlerinin nasıl uygulanacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 0fdf503e4a5f6f73d5488261707180cfb5967492
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115948"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="5b02f-103">ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="5b02f-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="5b02f-104">, [Luke Latham](https://github.com/guardrex) ve [Jeow li Hua](https://github.com/huan086) tarafından</span><span class="sxs-lookup"><span data-stu-id="5b02f-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5b02f-105">ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="5b02f-106">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="5b02f-107">Bu konuda üç barındırılan hizmet örneği sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="5b02f-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="5b02f-108">Bir Zamanlayıcı üzerinde çalışan arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="5b02f-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="5b02f-109">[Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="5b02f-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="5b02f-110">Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="5b02f-111">Sıralı olarak çalışan sıraya alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="5b02f-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="5b02f-112">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b02f-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="5b02f-113">Çalışan hizmeti şablonu</span><span class="sxs-lookup"><span data-stu-id="5b02f-113">Worker Service template</span></span>

<span data-ttu-id="5b02f-114">ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b02f-114">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="5b02f-115">Çalışan hizmeti şablonundan oluşturulan bir uygulama, çalışan SDK 'sını proje dosyasında belirtir:</span><span class="sxs-lookup"><span data-stu-id="5b02f-115">An app created from the Worker Service template specifies the Worker SDK in its project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Worker">
```

<span data-ttu-id="5b02f-116">Şablonu bir barındırılan hizmetler uygulamasının temeli olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="5b02f-116">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

## <a name="package"></a><span data-ttu-id="5b02f-117">Paket</span><span class="sxs-lookup"><span data-stu-id="5b02f-117">Package</span></span>

<span data-ttu-id="5b02f-118">Çalışan hizmeti şablonunu temel alan bir uygulama `Microsoft.NET.Sdk.Worker` SDK kullanır ve [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine açık bir paket başvurusu içerir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-118">An app based on the Worker Service template uses the `Microsoft.NET.Sdk.Worker` SDK and has an explicit package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span> <span data-ttu-id="5b02f-119">Örneğin, örnek uygulamanın proje dosyasına (*Backgroundtaskssample. csproj*) bakın.</span><span class="sxs-lookup"><span data-stu-id="5b02f-119">For example, see the sample app's project file (*BackgroundTasksSample.csproj*).</span></span>

<span data-ttu-id="5b02f-120">`Microsoft.NET.Sdk.Web` SDK kullanan Web uygulamaları için, [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine, paylaşılan çerçeveden dolaylı olarak başvurulur.</span><span class="sxs-lookup"><span data-stu-id="5b02f-120">For web apps that use the `Microsoft.NET.Sdk.Web` SDK, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is referenced implicitly from the shared framework.</span></span> <span data-ttu-id="5b02f-121">Uygulamanın proje dosyasındaki açık bir paket başvurusu gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-121">An explicit package reference in the app's project file isn't required.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="5b02f-122">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="5b02f-122">IHostedService interface</span></span>

<span data-ttu-id="5b02f-123"><xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi, konak tarafından yönetilen nesneler için iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="5b02f-123">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="5b02f-124">[Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevinin başlatılacağı mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="5b02f-125">`StartAsync` şu kadar *çağrılır:*</span><span class="sxs-lookup"><span data-stu-id="5b02f-125">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="5b02f-126">Uygulamanın istek işleme işlem hattı yapılandırıldı (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="5b02f-126">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="5b02f-127">Sunucu başlatıldı ve [ıapplicationlifetime. ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-127">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="5b02f-128">Varsayılan davranış, barındırılan hizmetin `StartAsync`, uygulamanın işlem hattı yapılandırıldıktan ve `ApplicationStarted` çağrıldıktan sonra çalışması için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-128">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="5b02f-129">Varsayılan davranışı değiştirmek için, `ConfigureWebHostDefaults`çağrıldıktan sonra barındırılan hizmeti (aşağıdaki örnekte`VideosWatcher`) ekleyin:</span><span class="sxs-lookup"><span data-stu-id="5b02f-129">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="5b02f-130">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-130">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="5b02f-131">`StopAsync` arka plan görevinin sonundaki mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-131">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="5b02f-132">Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.</span><span class="sxs-lookup"><span data-stu-id="5b02f-132">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="5b02f-133">İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-133">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="5b02f-134">Belirteç üzerinde iptal istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="5b02f-134">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="5b02f-135">Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-135">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="5b02f-136">`StopAsync` içinde çağrılan yöntemler hemen döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-136">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="5b02f-137">Ancak,&mdash;istek iptal edildikten sonra, çağıran tüm görevlerin tamamlanmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="5b02f-137">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="5b02f-138">Uygulama beklenmedik şekilde kapanıyorsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrımayabilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-138">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="5b02f-139">Bu nedenle, `StopAsync` veya içinde gerçekleştirilen işlemler tarafından çağrılan yöntemler gerçekleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-139">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="5b02f-140">Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="5b02f-140">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="5b02f-141">Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>.</span><span class="sxs-lookup"><span data-stu-id="5b02f-141"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="5b02f-142">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="5b02f-142">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="5b02f-143">Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="5b02f-143">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="5b02f-144">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="5b02f-144">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="5b02f-145">Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-145">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="5b02f-146">Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-146">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="5b02f-147">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="5b02f-147">BackgroundService</span></span>

<span data-ttu-id="5b02f-148">`BackgroundService`, uzun süre çalışan bir <xref:Microsoft.Extensions.Hosting.IHostedService>uygulamaya yönelik temel bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-148">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="5b02f-149">`BackgroundService`, hizmetin mantığını içerecek `ExecuteAsync(CancellationToken stoppingToken)` soyut yöntemi sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b02f-149">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="5b02f-150">`stoppingToken`, [ıhostedservice. StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) çağrıldığında tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-150">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="5b02f-151">Bu yöntemin uygulanması, arka plan hizmetinin tüm ömrünü temsil eden bir `Task` döndürür.</span><span class="sxs-lookup"><span data-stu-id="5b02f-151">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="5b02f-152">Ayrıca, *isteğe bağlı* olarak, hizmetiniz için başlatma ve kapatılma kodunu çalıştırmak üzere `IHostedService` tanımlı yöntemleri geçersiz kılın:</span><span class="sxs-lookup"><span data-stu-id="5b02f-152">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="5b02f-153">Uygulama ana bilgisayarı düzgün kapanma yaparken `StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-153">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="5b02f-154">`cancellationToken`, ana bilgisayar hizmeti zorla sonlandırmak için karar verdiğinde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-154">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="5b02f-155">Bu yöntem geçersiz kılınırsa, hizmetin düzgün kapatılmasını sağlamak için temel sınıf yöntemini çağırmanız (ve `await`) **gerekir** .</span><span class="sxs-lookup"><span data-stu-id="5b02f-155">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="5b02f-156">arka plan hizmetini başlatmak için `StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` çağırılır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-156">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="5b02f-157">Başlangıç işlemi kesintiye uğrarsa `cancellationToken` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-157">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="5b02f-158">Uygulama, hizmetin başlatma sürecini temsil eden bir `Task` döndürür.</span><span class="sxs-lookup"><span data-stu-id="5b02f-158">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="5b02f-159">Bu `Task` tamamlanana kadar başka hizmet başlatılamaz.</span><span class="sxs-lookup"><span data-stu-id="5b02f-159">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="5b02f-160">Bu yöntem geçersiz kılınırsa, hizmetin düzgün şekilde başlamasını sağlamak için temel sınıf yöntemini çağırmanız (ve `await`) **gerekir** .</span><span class="sxs-lookup"><span data-stu-id="5b02f-160">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="5b02f-161">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="5b02f-161">Timed background tasks</span></span>

<span data-ttu-id="5b02f-162">Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b02f-162">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="5b02f-163">Zamanlayıcı, görevin `DoWork` yöntemini tetikler.</span><span class="sxs-lookup"><span data-stu-id="5b02f-163">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="5b02f-164">Zamanlayıcı `StopAsync` devre dışı bırakılır ve hizmet kapsayıcısı `Dispose`bırakıldığında atıldı:</span><span class="sxs-lookup"><span data-stu-id="5b02f-164">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="5b02f-165">Hizmet, `AddHostedService` uzantısı yöntemiyle `IHostBuilder.ConfigureServices` (*program.cs*) kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="5b02f-165">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="5b02f-166">Bir arka plan görevinde kapsamlı bir hizmeti kullanma</span><span class="sxs-lookup"><span data-stu-id="5b02f-166">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="5b02f-167">[Kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) bir `BackgroundService`içinde kullanmak için bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5b02f-167">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="5b02f-168">Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b02f-168">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="5b02f-169">Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-169">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="5b02f-170">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="5b02f-170">In the following example:</span></span>

* <span data-ttu-id="5b02f-171">Hizmet zaman uyumsuz.</span><span class="sxs-lookup"><span data-stu-id="5b02f-171">The service is asynchronous.</span></span> <span data-ttu-id="5b02f-172">`DoWork` yöntemi bir `Task`döndürür.</span><span class="sxs-lookup"><span data-stu-id="5b02f-172">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="5b02f-173">Tanıtım amacıyla, `DoWork` yönteminde on saniyelik bir gecikme beklenir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-173">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="5b02f-174">Hizmete bir <xref:Microsoft.Extensions.Logging.ILogger> eklenir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-174">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="5b02f-175">Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5b02f-175">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="5b02f-176">`DoWork`, `ExecuteAsync`beklemiş olan bir `Task`döndürür:</span><span class="sxs-lookup"><span data-stu-id="5b02f-176">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="5b02f-177">Hizmetler `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="5b02f-177">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="5b02f-178">Barındırılan hizmet `AddHostedService` uzantısı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="5b02f-178">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="5b02f-179">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="5b02f-179">Queued background tasks</span></span>

<span data-ttu-id="5b02f-180">Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> temel alır ([ASP.NET Core için yerleşik](https://github.com/aspnet/Hosting/issues/1280)olarak, kesin olarak zamanlandı):</span><span class="sxs-lookup"><span data-stu-id="5b02f-180">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="5b02f-181">Aşağıdaki `QueueHostedService` örnekte:</span><span class="sxs-lookup"><span data-stu-id="5b02f-181">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="5b02f-182">`BackgroundProcessing` yöntemi, `ExecuteAsync`beklemiş olan bir `Task`döndürür.</span><span class="sxs-lookup"><span data-stu-id="5b02f-182">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="5b02f-183">Kuyruktaki arka plan görevleri, `BackgroundProcessing`sırada kuyruğa alınır ve yürütülür.</span><span class="sxs-lookup"><span data-stu-id="5b02f-183">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="5b02f-184">İş öğeleri, hizmet `StopAsync`' de durdurulmadan önce bekletildi.</span><span class="sxs-lookup"><span data-stu-id="5b02f-184">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="5b02f-185">Bir `MonitorLoop` hizmeti, giriş cihazında `w` anahtarı seçildiğinde barındırılan hizmet için görevleri sıraya alır:</span><span class="sxs-lookup"><span data-stu-id="5b02f-185">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="5b02f-186">`IBackgroundTaskQueue`, `MonitorLoop` hizmetine eklenir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-186">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="5b02f-187">`IBackgroundTaskQueue.QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-187">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="5b02f-188">Çalışma öğesi uzun süre çalışan bir arka plan görevinin benzetimini yapar:</span><span class="sxs-lookup"><span data-stu-id="5b02f-188">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="5b02f-189">Üç 5-ikinci gecikme yürütülür (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="5b02f-189">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="5b02f-190">`try-catch` bir ifade, görev iptal edildiğinde <xref:System.OperationCanceledException> yakalar.</span><span class="sxs-lookup"><span data-stu-id="5b02f-190">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="5b02f-191">Hizmetler `IHostBuilder.ConfigureServices` kaydedilir (*program.cs*).</span><span class="sxs-lookup"><span data-stu-id="5b02f-191">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="5b02f-192">Barındırılan hizmet `AddHostedService` uzantısı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="5b02f-192">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5b02f-193">ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-193">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="5b02f-194">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygulayan bir arka plan görevi mantığı içeren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-194">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="5b02f-195">Bu konuda üç barındırılan hizmet örneği sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="5b02f-195">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="5b02f-196">Bir Zamanlayıcı üzerinde çalışan arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="5b02f-196">Background task that runs on a timer.</span></span>
* <span data-ttu-id="5b02f-197">[Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="5b02f-197">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="5b02f-198">Kapsamlı hizmet [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kullanabilir</span><span class="sxs-lookup"><span data-stu-id="5b02f-198">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="5b02f-199">Sıralı olarak çalışan sıraya alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="5b02f-199">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="5b02f-200">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b02f-200">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="5b02f-201">Paket</span><span class="sxs-lookup"><span data-stu-id="5b02f-201">Package</span></span>

<span data-ttu-id="5b02f-202">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5b02f-202">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="5b02f-203">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="5b02f-203">IHostedService interface</span></span>

<span data-ttu-id="5b02f-204">Barındırılan hizmetler <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="5b02f-204">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="5b02f-205">Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="5b02f-205">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="5b02f-206">[Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevinin başlatılacağı mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-206">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="5b02f-207">[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanılırken, sunucu başlatıldıktan ve ıapplicationlifetime 'dan sonra `StartAsync` çağrılır [. applicationstarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-207">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="5b02f-208">[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanılırken, `ApplicationStarted` tetiklenene kadar `StartAsync` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-208">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="5b02f-209">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash;, ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-209">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="5b02f-210">`StopAsync` arka plan görevinin sonundaki mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-210">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="5b02f-211">Yönetilmeyen kaynakların atılmaya yönelik <xref:System.IDisposable> ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) uygulayın.</span><span class="sxs-lookup"><span data-stu-id="5b02f-211">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="5b02f-212">İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-212">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="5b02f-213">Belirteç üzerinde iptal istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="5b02f-213">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="5b02f-214">Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-214">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="5b02f-215">`StopAsync` içinde çağrılan yöntemler hemen döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-215">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="5b02f-216">Ancak,&mdash;istek iptal edildikten sonra, çağıran tüm görevlerin tamamlanmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="5b02f-216">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="5b02f-217">Uygulama beklenmedik şekilde kapanıyorsa (örneğin, uygulamanın işlemi başarısız olursa) `StopAsync` çağrımayabilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-217">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="5b02f-218">Bu nedenle, `StopAsync` veya içinde gerçekleştirilen işlemler tarafından çağrılan yöntemler gerçekleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-218">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="5b02f-219">Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="5b02f-219">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="5b02f-220">Genel ana bilgisayar kullanılırken <xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>.</span><span class="sxs-lookup"><span data-stu-id="5b02f-220"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="5b02f-221">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="5b02f-221">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="5b02f-222">Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="5b02f-222">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="5b02f-223">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="5b02f-223">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="5b02f-224">Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-224">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="5b02f-225">Arka plan görevinin yürütülmesi sırasında bir hata oluşturulursa, `StopAsync` çağrılmasa bile `Dispose` çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-225">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="5b02f-226">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="5b02f-226">Timed background tasks</span></span>

<span data-ttu-id="5b02f-227">Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="5b02f-227">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="5b02f-228">Zamanlayıcı, görevin `DoWork` yöntemini tetikler.</span><span class="sxs-lookup"><span data-stu-id="5b02f-228">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="5b02f-229">Zamanlayıcı `StopAsync` devre dışı bırakılır ve hizmet kapsayıcısı `Dispose`bırakıldığında atıldı:</span><span class="sxs-lookup"><span data-stu-id="5b02f-229">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="5b02f-230">Hizmet, `AddHostedService` uzantısı yöntemiyle `Startup.ConfigureServices` kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="5b02f-230">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="5b02f-231">Bir arka plan görevinde kapsamlı bir hizmeti kullanma</span><span class="sxs-lookup"><span data-stu-id="5b02f-231">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="5b02f-232">[Kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) bir `IHostedService`içinde kullanmak için bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="5b02f-232">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="5b02f-233">Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="5b02f-233">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="5b02f-234">Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-234">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="5b02f-235">Aşağıdaki örnekte, hizmetine bir <xref:Microsoft.Extensions.Logging.ILogger> eklenmiş olur:</span><span class="sxs-lookup"><span data-stu-id="5b02f-235">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="5b02f-236">Barındırılan hizmet, `DoWork` yöntemini çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5b02f-236">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="5b02f-237">Hizmetler `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-237">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5b02f-238">`IHostedService` uygulama `AddHostedService` uzantısı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="5b02f-238">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="5b02f-239">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="5b02f-239">Queued background tasks</span></span>

<span data-ttu-id="5b02f-240">Arka plan görev kuyruğu .NET Framework 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> temel alır ([ASP.NET Core için kesin olarak zamanlandı](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="5b02f-240">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="5b02f-241">`QueueHostedService`, kuyruktaki arka plan görevleri, uzun süre çalışan bir `IHostedService`uygulamak için temel bir sınıf olan bir <xref:Microsoft.Extensions.Hosting.BackgroundService>olarak kuyruğa alınır ve yürütülür:</span><span class="sxs-lookup"><span data-stu-id="5b02f-241">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="5b02f-242">Hizmetler `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="5b02f-242">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5b02f-243">`IHostedService` uygulama `AddHostedService` uzantısı yöntemiyle kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="5b02f-243">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="5b02f-244">Dizin sayfası model sınıfında:</span><span class="sxs-lookup"><span data-stu-id="5b02f-244">In the Index page model class:</span></span>

* <span data-ttu-id="5b02f-245">`IBackgroundTaskQueue` oluşturucuya eklenir ve `Queue`atanır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-245">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="5b02f-246"><xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> eklenen ve `_serviceScopeFactory`atanmış.</span><span class="sxs-lookup"><span data-stu-id="5b02f-246">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="5b02f-247">Fabrika, bir kapsam içinde hizmet oluşturmak için kullanılan <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>örnekleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5b02f-247">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="5b02f-248">Bir kapsam, veritabanı kayıtlarını `IBackgroundTaskQueue` (bir tek hizmet) yazmak için uygulamanın `AppDbContext` ( [kapsamlı bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes)) kullanmak üzere oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5b02f-248">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="5b02f-249">Dizin sayfasında **Görev Ekle** düğmesi seçildiğinde `OnPostAddTask` yöntemi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="5b02f-249">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="5b02f-250">`QueueBackgroundWorkItem` bir iş öğesini kuyruğa almak için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="5b02f-250">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5b02f-251">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5b02f-251">Additional resources</span></span>

* [<span data-ttu-id="5b02f-252">Ihostedservice ve BackgroundService sınıfıyla mikro hizmetlerde arka plan görevleri uygulama</span><span class="sxs-lookup"><span data-stu-id="5b02f-252">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
