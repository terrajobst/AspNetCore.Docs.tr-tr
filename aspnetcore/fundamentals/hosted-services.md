---
title: "ASP.NET Core barındırılan hizmetleri ile arka plan görevleri"
author: guardrex
description: "ASP.NET Core arka plan görevlerini barındırılan hizmetleri ile uygulama öğrenin."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosted-services
ms.openlocfilehash: 49d7780184e915bfbfd48cc9a2285f23cac20b34
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2018
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="1475b-103">ASP.NET Core barındırılan hizmetleri ile arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="1475b-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="1475b-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1475b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1475b-105">ASP.NET çekirdek arka plan görevleri olarak uygulanabilir *barındırılan hizmetlere*.</span><span class="sxs-lookup"><span data-stu-id="1475b-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="1475b-106">Barındırılan hizmet uygulayan arka plan görevi mantığı ile bir sınıftır [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="1475b-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="1475b-107">Bu konuda üç barındırılan hizmet örnekleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="1475b-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="1475b-108">Bir süreölçer çalıştıran arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="1475b-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="1475b-109">Kapsamlı bir hizmet etkinleştirir barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="1475b-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="1475b-110">Kapsamlı hizmet bağımlılık ekleme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1475b-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="1475b-111">Sırayla çalışır sıraya alınan arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="1475b-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="1475b-112">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1475b-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="1475b-113">IHostedService interface</span><span class="sxs-lookup"><span data-stu-id="1475b-113">IHostedService interface</span></span>

<span data-ttu-id="1475b-114">Barındırılan hizmetler uygulama [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) arabirimi.</span><span class="sxs-lookup"><span data-stu-id="1475b-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="1475b-115">Ana bilgisayar tarafından yönetilen nesneler için iki yöntem arabirimi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="1475b-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="1475b-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) -sunucu başlatıldıktan sonra çağrılır ve [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="1475b-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="1475b-117">`StartAsync` arka plan görevi başlatmak için mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="1475b-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="1475b-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) -ana bilgisayar normal şekilde kapatılmasını gerçekleştirirken tetiklendi.</span><span class="sxs-lookup"><span data-stu-id="1475b-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="1475b-119">`StopAsync` Son arka plan görevi ve yönetilmeyen kaynakları dispose mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="1475b-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="1475b-120">Uygulama beklenmedik biçimde (örneğin, uygulamanın işlemi başarısız olur), kapatılırsa `StopAsync` çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="1475b-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="1475b-121">Barındırılan hizmet uygulaması başlangıçta ve düzgün biçimde kapatma uygulama kapatma sırasında bir kez etkinleştirilmiş bir singleton ' dir.</span><span class="sxs-lookup"><span data-stu-id="1475b-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="1475b-122">Zaman [IDisposable](/dotnet/api/system.idisposable) olan hizmet kapsayıcısı uygulanan kaynakları silinmediğinde.</span><span class="sxs-lookup"><span data-stu-id="1475b-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="1475b-123">Arka plan Görev yürütülürken bir hata oluşursa `Dispose` çağrılmalıdır olsa bile `StopAsync` olarak adlandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="1475b-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="1475b-124">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="1475b-124">Timed background tasks</span></span>

<span data-ttu-id="1475b-125">Zamanlanmış arka plan görevi kullanır [süre System.Threading.Timer](/dotnet/api/system.threading.timer) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="1475b-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="1475b-126">Görev Zamanlayıcı tetikler `DoWork` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="1475b-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="1475b-127">Zamanlayıcı devre dışı bırakıldı `StopAsync` ve hizmet kapsayıcısı üzerinde silinmediğinde `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="1475b-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/sample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="1475b-128">Hizmet kayıtlı `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1475b-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/sample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="1475b-129">Kapsamlı bir arka plan görevi hizmetinde kullanma</span><span class="sxs-lookup"><span data-stu-id="1475b-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="1475b-130">Kapsamlı Hizmetleri içinde kullanmak için bir `IHostedService`, bir kapsamı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1475b-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="1475b-131">Kapsam bir barındırılan hizmet için varsayılan olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="1475b-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="1475b-132">Kapsamlı bir arka plan görev hizmeti arka plan görevin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="1475b-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="1475b-133">Aşağıdaki örnekte, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) hizmete eklenmiş:</span><span class="sxs-lookup"><span data-stu-id="1475b-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/sample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="1475b-134">Barındırılan hizmet çağırmak için kapsamlı bir arka plan görev hizmeti çözümlemek için bir kapsam oluşturur, `DoWork` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="1475b-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/sample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="1475b-135">Hizmetleri kayıtlı `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1475b-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/sample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="1475b-136">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="1475b-136">Queued background tasks</span></span>

<span data-ttu-id="1475b-137">Arka plan görev sırası .NET tabanlı 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([ASP.NET Core 2.2 yerleşik olarak kesin zamanlanmış](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="1475b-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/sample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="1475b-138">İçinde `QueueHostedService`, arka plan görevleri (`workItem`) sırasındaki kuyruktan çıkarıldı ve yürütülemiyor:</span><span class="sxs-lookup"><span data-stu-id="1475b-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/sample/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="1475b-139">Hizmetleri kayıtlı `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1475b-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/sample/Startup.cs?name=snippet3)]

<span data-ttu-id="1475b-140">Dizin sayfası modeli sınıfında `IBackgroundTaskQueue` oluşturucuya eklenen ve atanan `Queue`:</span><span class="sxs-lookup"><span data-stu-id="1475b-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/sample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="1475b-141">Zaman **Görev Ekle** düğmesi dizin sayfasında, seçili `OnPostAddTask` yöntemi yürütüldüğünde.</span><span class="sxs-lookup"><span data-stu-id="1475b-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="1475b-142">`QueueBackgroundWorkItem` sıraya alma, iş öğesi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="1475b-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/sample/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="1475b-143">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1475b-143">Additional resources</span></span>

* [<span data-ttu-id="1475b-144">Mikro IHostedService ve BackgroundService sınıfı ile arka plan görevlerini uygulama</span><span class="sxs-lookup"><span data-stu-id="1475b-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="1475b-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="1475b-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
