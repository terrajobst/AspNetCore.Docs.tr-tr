---
title: ASP.NET Core barındırılan hizmetleri ile arka plan görevleri
author: guardrex
description: ASP.NET Core arka plan görevlerini barındırılan hizmetleri ile uygulama öğrenin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: cc39d125b639719599eca68d627fda014fb107e0
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2018
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="53015-103">ASP.NET Core barındırılan hizmetleri ile arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="53015-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="53015-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="53015-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="53015-105">ASP.NET çekirdek arka plan görevleri olarak uygulanabilir *barındırılan hizmetlere*.</span><span class="sxs-lookup"><span data-stu-id="53015-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="53015-106">Barındırılan hizmet uygulayan arka plan görevi mantığı ile bir sınıftır [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="53015-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="53015-107">Bu konuda üç barındırılan hizmet örnekleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="53015-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="53015-108">Bir süreölçer çalıştıran arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="53015-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="53015-109">Kapsamlı bir hizmet etkinleştirir barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="53015-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="53015-110">Kapsamlı hizmet bağımlılık ekleme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="53015-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="53015-111">Sırayla çalışır sıraya alınan arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="53015-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="53015-112">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="53015-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="53015-113">Örnek uygulaması iki sürümlerinde sağlanır:</span><span class="sxs-lookup"><span data-stu-id="53015-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="53015-114">Web ana bilgisayar &ndash; Web ana web uygulamalarını barındırmak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="53015-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="53015-115">Bu konuda gösterilen örnek kod örnek Web ana bilgisayarı sürümünden verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="53015-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="53015-116">Daha fazla bilgi için bkz: [Web ana bilgisayarı](xref:fundamentals/host/web-host) konu.</span><span class="sxs-lookup"><span data-stu-id="53015-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="53015-117">Genel ana bilgisayar &ndash; genel ana bilgisayar ASP.NET Core 2.1 içinde yenidir.</span><span class="sxs-lookup"><span data-stu-id="53015-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="53015-118">Daha fazla bilgi için bkz: [genel ana bilgisayar](xref:fundamentals/host/generic-host) konu.</span><span class="sxs-lookup"><span data-stu-id="53015-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="53015-119">IHostedService arabirimi</span><span class="sxs-lookup"><span data-stu-id="53015-119">IHostedService interface</span></span>

<span data-ttu-id="53015-120">Barındırılan hizmetler uygulama [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="53015-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="53015-121">Ana bilgisayar tarafından yönetilen nesneler için iki yöntem arabirimi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="53015-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="53015-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) -sunucu başlatıldıktan sonra çağrılır ve [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="53015-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="53015-123">`StartAsync` arka plan görevi başlatmak için mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="53015-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="53015-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -ana bilgisayar normal şekilde kapatılmasını gerçekleştirirken tetiklendi.</span><span class="sxs-lookup"><span data-stu-id="53015-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="53015-125">`StopAsync` Son arka plan görevi ve yönetilmeyen kaynakları dispose mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="53015-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="53015-126">Uygulama beklenmedik biçimde (örneğin, uygulamanın işlemi başarısız olur), kapatılırsa `StopAsync` çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="53015-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="53015-127">Barındırılan hizmet uygulaması başlangıçta ve düzgün biçimde kapatma uygulama kapatma sırasında bir kez etkinleştirilmiş bir singleton ' dir.</span><span class="sxs-lookup"><span data-stu-id="53015-127">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="53015-128">Zaman [IDisposable](/dotnet/api/system.idisposable) olan hizmet kapsayıcısı uygulanan kaynakları silinmediğinde.</span><span class="sxs-lookup"><span data-stu-id="53015-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="53015-129">Arka plan Görev yürütülürken bir hata oluşursa `Dispose` çağrılmalıdır olsa bile `StopAsync` olarak adlandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="53015-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="53015-130">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="53015-130">Timed background tasks</span></span>

<span data-ttu-id="53015-131">Zamanlanmış arka plan görevi kullanır [süre System.Threading.Timer](/dotnet/api/system.threading.timer) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="53015-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="53015-132">Görev Zamanlayıcı tetikler `DoWork` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="53015-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="53015-133">Zamanlayıcı devre dışı bırakıldı `StopAsync` ve hizmet kapsayıcısı üzerinde silinmediğinde `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="53015-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="53015-134">Hizmet kayıtlı `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53015-134">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="53015-135">Kapsamlı bir arka plan görevi hizmetinde kullanma</span><span class="sxs-lookup"><span data-stu-id="53015-135">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="53015-136">Kapsamlı Hizmetleri içinde kullanmak için bir `IHostedService`, bir kapsamı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="53015-136">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="53015-137">Kapsam bir barındırılan hizmet için varsayılan olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="53015-137">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="53015-138">Kapsamlı bir arka plan görev hizmeti arka plan görevin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="53015-138">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="53015-139">Aşağıdaki örnekte, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) hizmete eklenmiş:</span><span class="sxs-lookup"><span data-stu-id="53015-139">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="53015-140">Barındırılan hizmet çağırmak için kapsamlı bir arka plan görev hizmeti çözümlemek için bir kapsam oluşturur, `DoWork` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="53015-140">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="53015-141">Hizmetleri kayıtlı `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53015-141">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="53015-142">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="53015-142">Queued background tasks</span></span>

<span data-ttu-id="53015-143">Arka plan görev sırası .NET tabanlı 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([ASP.NET Core 2.2 yerleşik olarak kesin zamanlanmış](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="53015-143">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="53015-144">İçinde `QueueHostedService`, arka plan görevleri (`workItem`) sırasındaki kuyruktan çıkarıldı ve yürütülemiyor:</span><span class="sxs-lookup"><span data-stu-id="53015-144">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="53015-145">Hizmetleri kayıtlı `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53015-145">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="53015-146">Dizin sayfası modeli sınıfında `IBackgroundTaskQueue` oluşturucuya eklenen ve atanan `Queue`:</span><span class="sxs-lookup"><span data-stu-id="53015-146">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="53015-147">Zaman **Görev Ekle** düğmesi dizin sayfasında, seçili `OnPostAddTask` yöntemi yürütüldüğünde.</span><span class="sxs-lookup"><span data-stu-id="53015-147">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="53015-148">`QueueBackgroundWorkItem` sıraya alma, iş öğesi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="53015-148">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="53015-149">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="53015-149">Additional resources</span></span>

* [<span data-ttu-id="53015-150">Mikro IHostedService ve BackgroundService sınıfı ile arka plan görevlerini uygulama</span><span class="sxs-lookup"><span data-stu-id="53015-150">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="53015-151">Süre System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="53015-151">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
