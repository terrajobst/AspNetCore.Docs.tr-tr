---
title: ASP.NET core'da barındırılan hizmetler ile arka plan görevleri
author: guardrex
description: ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: f8e13e13af22f1be4f14d5e59807c4dae3b78e84
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708497"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="978cc-103">ASP.NET core'da barındırılan hizmetler ile arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="978cc-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="978cc-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="978cc-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="978cc-105">ASP.NET Core, arka plan görevleri olarak uygulanabilir *barındırılan hizmetleri*.</span><span class="sxs-lookup"><span data-stu-id="978cc-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="978cc-106">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="978cc-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="978cc-107">Bu konu başlığı altında üç barındırılan hizmet örnekleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="978cc-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="978cc-108">Bir zamanlayıcıyı temel çalışan arka plan görev.</span><span class="sxs-lookup"><span data-stu-id="978cc-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="978cc-109">Kapsamlı bir hizmet etkinleştirir, barındırılan hizmeti.</span><span class="sxs-lookup"><span data-stu-id="978cc-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="978cc-110">Kapsamlı hizmet, bağımlılık ekleme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="978cc-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="978cc-111">Sırayla çalışır kuyruğa alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="978cc-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="978cc-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="978cc-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="978cc-113">Örnek uygulama, iki sürümde sağlanır:</span><span class="sxs-lookup"><span data-stu-id="978cc-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="978cc-114">Web ana bilgisayar &ndash; Web ana bilgisayarı, web uygulamalarını barındırmak için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="978cc-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="978cc-115">Bu konuda gösterilen örnek kod örnek Web ana bilgisayarı sürümünden ' dir.</span><span class="sxs-lookup"><span data-stu-id="978cc-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="978cc-116">Daha fazla bilgi için [Web ana bilgisayarı](xref:fundamentals/host/web-host) konu.</span><span class="sxs-lookup"><span data-stu-id="978cc-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="978cc-117">Genel konak &ndash; ASP.NET Core 2.1 içinde genel ana bilgisayar içindir.</span><span class="sxs-lookup"><span data-stu-id="978cc-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="978cc-118">Daha fazla bilgi için [genel ana bilgisayar](xref:fundamentals/host/generic-host) konu.</span><span class="sxs-lookup"><span data-stu-id="978cc-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="978cc-119">Paket</span><span class="sxs-lookup"><span data-stu-id="978cc-119">Package</span></span>

<span data-ttu-id="978cc-120">Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paket.</span><span class="sxs-lookup"><span data-stu-id="978cc-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="978cc-121">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="978cc-121">IHostedService interface</span></span>

<span data-ttu-id="978cc-122">Barındırılan hizmetler uygulama <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="978cc-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="978cc-123">Ana bilgisayar tarafından yönetilen nesneler için iki yöntem arabirimin tanımlar:</span><span class="sxs-lookup"><span data-stu-id="978cc-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="978cc-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevini başlatmak için mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="978cc-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="978cc-125">Kullanırken [Web ana bilgisayarı](xref:fundamentals/host/web-host), `StartAsync` sunucu yeniden başlatıldıktan sonra çağrılır ve [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="978cc-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="978cc-126">Kullanırken [genel ana bilgisayar](xref:fundamentals/host/generic-host), `StartAsync` önce çağrılır `ApplicationStarted` tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="978cc-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="978cc-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; konak normal şekilde kapatılmasını gerçekleştirirken tetiklendi.</span><span class="sxs-lookup"><span data-stu-id="978cc-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="978cc-128">`StopAsync` arka plan görevini sonlandırmak için mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="978cc-128">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="978cc-129">Uygulama <xref:System.IDisposable> ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) yönetilmeyen tüm kaynaklarını silmek için.</span><span class="sxs-lookup"><span data-stu-id="978cc-129">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span> 

  <span data-ttu-id="978cc-130">İptal belirteci kapatma işlemi artık normal gerektiğini belirtmek için beş varsayılan bir ikinci zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="978cc-130">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="978cc-131">İptal belirteci üzerinde güncel olduğunda isteniyor:</span><span class="sxs-lookup"><span data-stu-id="978cc-131">When cancellation is requested on the token:</span></span>
  
  * <span data-ttu-id="978cc-132">Uygulama gerçekleştiren kalan arka plan işlemleri durduruldu.</span><span class="sxs-lookup"><span data-stu-id="978cc-132">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="978cc-133">Çağrılma yeri herhangi bir yöntem `StopAsync` en kısa sürede döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="978cc-133">Any methods called in `StopAsync` should return promptly.</span></span>
  
  <span data-ttu-id="978cc-134">Ancak, İptal işlemi istendikten sonra görev terk olmayan&mdash;çağıran tüm görevleri tamamlamak için bekler.</span><span class="sxs-lookup"><span data-stu-id="978cc-134">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="978cc-135">Uygulama beklenmedik bir şekilde (örneğin, uygulamanın işlemi başarısız olur), kapanırsa `StopAsync` değil olarak adlandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="978cc-135">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="978cc-136">Bu nedenle, herhangi bir yöntem de çağrıldığında veya gerçekleştirilen işlemleri de `StopAsync` değil oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="978cc-136">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

<span data-ttu-id="978cc-137">Barındırılan hizmet uygulamasını başlangıçta kez etkinleştirilir ve uygulama kapatma sırasında düzgün biçimde kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="978cc-137">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="978cc-138">Arka plan görevi yürütme sırasında bir hata oluşturulursa `Dispose` çağrılmalıdır bile `StopAsync` adı değil.</span><span class="sxs-lookup"><span data-stu-id="978cc-138">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="978cc-139">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="978cc-139">Timed background tasks</span></span>

<span data-ttu-id="978cc-140">Zamanlanmış arka plan görevi kullanır [süre System.Threading.Timer](xref:System.Threading.Timer) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="978cc-140">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="978cc-141">Görev Zamanlayıcı tetiklendikten `DoWork` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="978cc-141">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="978cc-142">Zamanlayıcıyı devre dışı `StopAsync` ve hizmet kapsayıcısı üzerinde silinmediğinde `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="978cc-142">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="978cc-143">Hizmet kayıtlı `Startup.ConfigureServices` ile `AddHostedService` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="978cc-143">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="978cc-144">Kapsamlı bir hizmet bir arka plan görevi olarak kullanma</span><span class="sxs-lookup"><span data-stu-id="978cc-144">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="978cc-145">Kapsamlı Hizmetleri içinde kullanmak için bir `IHostedService`, bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="978cc-145">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="978cc-146">Kapsam, bir barındırılan hizmet için varsayılan olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="978cc-146">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="978cc-147">Kapsamlı bir arka plan görev hizmeti arka plan görev mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="978cc-147">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="978cc-148">Aşağıdaki örnekte, bir <xref:Microsoft.Extensions.Logging.ILogger> hizmetinde eklenmiş olur:</span><span class="sxs-lookup"><span data-stu-id="978cc-148">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="978cc-149">Barındırılan hizmet çağırmak için kapsamlı bir arka plan görev hizmeti çözümlemek için bir kapsam oluşturur, `DoWork` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="978cc-149">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="978cc-150">Hizmetleri kayıtlı `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="978cc-150">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="978cc-151">`IHostedService` Uygulaması ile kayıtlı `AddHostedService` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="978cc-151">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="978cc-152">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="978cc-152">Queued background tasks</span></span>

<span data-ttu-id="978cc-153">Arka plan görev sırası, .NET tabanlı 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([ASP.NET Core 3.0 için yerleşik olarak geçici olarak zamanlanmış](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="978cc-153">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="978cc-154">İçinde `QueueHostedService`, arka plan görevleri sırasındaki sıradan çıkarılan ve olarak yürütülen bir <xref:Microsoft.Extensions.Hosting.BackgroundService>, uzun çalışan bir uygulama için bir temel sınıf olan `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="978cc-154">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="978cc-155">Hizmetleri kayıtlı `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="978cc-155">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="978cc-156">`IHostedService` Uygulaması ile kayıtlı `AddHostedService` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="978cc-156">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="978cc-157">Dizin Sayfası model sınıfında `IBackgroundTaskQueue` oluşturucuya eklenen ve atanan `Queue`:</span><span class="sxs-lookup"><span data-stu-id="978cc-157">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="978cc-158">Zaman **Görev Ekle** düğmesi seçili dizin sayfasında `OnPostAddTask` yöntemi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="978cc-158">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="978cc-159">`QueueBackgroundWorkItem` İş öğesini kuyruğa çağrılır:</span><span class="sxs-lookup"><span data-stu-id="978cc-159">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="978cc-160">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="978cc-160">Additional resources</span></span>

* [<span data-ttu-id="978cc-161">Ihostedservice ve BackgroundService sınıfı ile mikro hizmetler içindeki arka plan görevlerini uygulama</span><span class="sxs-lookup"><span data-stu-id="978cc-161">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="978cc-162">Süre System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="978cc-162">System.Threading.Timer</span></span>](xref:System.Threading.Timer)
