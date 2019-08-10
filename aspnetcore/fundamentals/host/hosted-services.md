---
title: ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri
author: guardrex
description: ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 3d4279a291182da60c0cb2fbb93a3922ed673cde
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914018"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="42415-103">ASP.NET Core içinde barındırılan hizmetlerle arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="42415-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="42415-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="42415-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="42415-105">ASP.NET Core, arka plan görevleri *barındırılan hizmetler*olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="42415-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="42415-106">Barındırılan hizmet, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi uygulayan bir arka plan görevi mantığı olan bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="42415-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="42415-107">Bu konuda üç barındırılan hizmet örneği sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="42415-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="42415-108">Bir Zamanlayıcı üzerinde çalışan arka plan görevi.</span><span class="sxs-lookup"><span data-stu-id="42415-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="42415-109">[Kapsamlı bir hizmeti](xref:fundamentals/dependency-injection#service-lifetimes)etkinleştiren barındırılan hizmet.</span><span class="sxs-lookup"><span data-stu-id="42415-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="42415-110">Kapsamlı hizmet bağımlılık ekleme işlemini kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="42415-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="42415-111">Sıralı olarak çalışan sıraya alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="42415-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="42415-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="42415-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="42415-113">Örnek uygulama iki sürümde sunulmaktadır:</span><span class="sxs-lookup"><span data-stu-id="42415-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="42415-114">Web ana &ndash; bilgisayarı Web ana bilgisayarı Web uygulamalarını barındırmak için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="42415-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="42415-115">Bu konu başlığında gösterilen örnek kod, örneğin web ana bilgisayar sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="42415-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="42415-116">Daha fazla bilgi için bkz. [Web ana bilgisayarı](xref:fundamentals/host/web-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="42415-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="42415-117">Genel ana &ndash; bilgisayar genel ana bilgisayarı ASP.NET Core 2,1 ' de yenidir.</span><span class="sxs-lookup"><span data-stu-id="42415-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="42415-118">Daha fazla bilgi için bkz. [genel ana bilgisayar](xref:fundamentals/host/generic-host) konusu.</span><span class="sxs-lookup"><span data-stu-id="42415-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="42415-119">Çalışan hizmeti şablonu</span><span class="sxs-lookup"><span data-stu-id="42415-119">Worker Service template</span></span>

<span data-ttu-id="42415-120">ASP.NET Core Worker hizmeti şablonu, uzun süre çalışan hizmet uygulamalarını yazmak için bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="42415-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="42415-121">Şablonu bir barındırılan hizmetler uygulamasının temeli olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="42415-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="42415-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="42415-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="42415-123">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="42415-123">Create a new project.</span></span>
1. <span data-ttu-id="42415-124">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="42415-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="42415-125">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="42415-125">Select **Next**.</span></span>
1. <span data-ttu-id="42415-126">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="42415-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="42415-127">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="42415-127">Select **Create**.</span></span>
1. <span data-ttu-id="42415-128">**Yeni bir ASP.NET Core Web uygulaması oluştur** iletişim kutusunda, **.net Core** ve **ASP.NET Core 3,0** ' un seçili olduğunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="42415-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="42415-129">**Çalışan hizmeti** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="42415-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="42415-130">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="42415-130">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="42415-131">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="42415-131">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="42415-132">Çalışan hizmeti (`worker`) şablonunu bir komut kabuğundan [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla birlikte kullanın.</span><span class="sxs-lookup"><span data-stu-id="42415-132">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="42415-133">Aşağıdaki örnekte, adlı `ContosoWorkerService`bir çalışan hizmeti uygulaması oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="42415-133">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="42415-134">Komut yürütüldüğünde, `ContosoWorkerService` uygulamanın bir klasörü otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="42415-134">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="package"></a><span data-ttu-id="42415-135">Paket</span><span class="sxs-lookup"><span data-stu-id="42415-135">Package</span></span>

<span data-ttu-id="42415-136">Microsoft. [AspNetCore. app metapackage](xref:fundamentals/metapackage-app) 'e başvurun veya [Microsoft. Extensions. Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paketine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="42415-136">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="42415-137">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="42415-137">IHostedService interface</span></span>

<span data-ttu-id="42415-138">Barındırılan hizmetler, <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimini uygular.</span><span class="sxs-lookup"><span data-stu-id="42415-138">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="42415-139">Arabirim, konak tarafından yönetilen nesneler için iki yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="42415-139">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="42415-140">[Startasync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; arkaplan`StartAsync` görevinin başlatılacağı mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="42415-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="42415-141">[Web konağını](xref:fundamentals/host/web-host) `StartAsync` kullanırken sunucu başlatıldıktan ve [ıapplicationlifetime değerinden sonra çağrılır. applicationstarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="42415-141">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="42415-142">[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanılırken, `StartAsync` tetiklendikten önce `ApplicationStarted` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="42415-142">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="42415-143">[StopAsync (CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Ana bilgisayar düzgün bir şekilde kapanma gerçekleştirirken tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="42415-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="42415-144">`StopAsync`arka plan görevinin bitiş mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="42415-144">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="42415-145">Yönetilmeyen <xref:System.IDisposable> kaynakların atılmaya yönelik uygulama ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) .</span><span class="sxs-lookup"><span data-stu-id="42415-145">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="42415-146">İptal belirtecinin, kapanma işleminin artık düzgün şekilde olmaması gerektiğini göstermek için varsayılan beş saniyelik bir zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="42415-146">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="42415-147">Belirteç üzerinde iptal istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="42415-147">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="42415-148">Uygulamanın gerçekleştirdiği diğer arka plan işlemleri iptal edilmelidir.</span><span class="sxs-lookup"><span data-stu-id="42415-148">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="42415-149">İçinde `StopAsync` çağrılan tüm yöntemler hemen döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="42415-149">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="42415-150">Ancak, bir iptal işlemi istendikten&mdash;sonra görevler terk edilmez ve bu, çağıran tüm görevlerin tamamlanmasını bekler.</span><span class="sxs-lookup"><span data-stu-id="42415-150">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="42415-151">Uygulama beklenmedik bir şekilde kapanırsa (örneğin, uygulamanın işlemi başarısız olur), `StopAsync` çağrılmayabilir.</span><span class="sxs-lookup"><span data-stu-id="42415-151">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="42415-152">Bu nedenle, veya üzerinde `StopAsync` gerçekleştirilen işlemleri çağıran Yöntemler gerçekleşmeyebilir.</span><span class="sxs-lookup"><span data-stu-id="42415-152">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="42415-153">Varsayılan beş saniyelik kapatılma zaman aşımını uzatmak için, şunu ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="42415-153">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="42415-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*>Genel ana bilgisayar kullanırken.</span><span class="sxs-lookup"><span data-stu-id="42415-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="42415-155">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="42415-155">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="42415-156">Web ana bilgisayarı kullanılırken, kapatılma zaman aşımı konak yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="42415-156">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="42415-157">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="42415-157">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="42415-158">Barındırılan hizmet, uygulama başlangıcında bir kez etkinleştirilir ve uygulama kapatılırken düzgün şekilde kapanır.</span><span class="sxs-lookup"><span data-stu-id="42415-158">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="42415-159">Arka plan görevinin yürütülmesi sırasında bir hata oluşursa, `Dispose` çağrılmadıysa `StopAsync` bile çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="42415-159">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="42415-160">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="42415-160">Timed background tasks</span></span>

<span data-ttu-id="42415-161">Zamanlanmış bir arka plan görevi, [System. Threading. Timer](xref:System.Threading.Timer) sınıfından kullanımını sağlar.</span><span class="sxs-lookup"><span data-stu-id="42415-161">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="42415-162">Zamanlayıcı, görevin `DoWork` metodunu tetikler.</span><span class="sxs-lookup"><span data-stu-id="42415-162">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="42415-163">Zamanlayıcı devre dışı bırakılır `StopAsync` ve hizmet kapsayıcısı `Dispose`bırakıldığında bırakıldı:</span><span class="sxs-lookup"><span data-stu-id="42415-163">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="42415-164">Hizmet, `AddHostedService` uzantı yöntemiyle kaydedilir `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="42415-164">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="42415-165">Bir arka plan görevinde kapsamlı bir hizmeti kullanma</span><span class="sxs-lookup"><span data-stu-id="42415-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="42415-166">[Kapsamlı hizmetleri](xref:fundamentals/dependency-injection#service-lifetimes) bir `IHostedService`içinde kullanmak için bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="42415-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="42415-167">Barındırılan hizmet için varsayılan olarak kapsam oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="42415-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="42415-168">Kapsamlı arka plan görev hizmeti, arka plan görevinin mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="42415-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="42415-169">Aşağıdaki örnekte, hizmetine bir <xref:Microsoft.Extensions.Logging.ILogger> hizmet eklenmiş olur:</span><span class="sxs-lookup"><span data-stu-id="42415-169">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="42415-170">Barındırılan hizmet, kendi `DoWork` metodunu çağırmak için kapsamlı arka plan görev hizmetini çözümlemek üzere bir kapsam oluşturur:</span><span class="sxs-lookup"><span data-stu-id="42415-170">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="42415-171">Hizmetler ' de `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="42415-171">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="42415-172">Uygulama, `AddHostedService` uzantı yöntemiyle kaydedilir: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="42415-172">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="42415-173">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="42415-173">Queued background tasks</span></span>

<span data-ttu-id="42415-174">Arka plan görev kuyruğu, .NET 4. x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> 'i temel alır ([ASP.NET Core 3,0 için yerleşik olarak zamanlanmış olarak zamanlandı](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="42415-174">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="42415-175">' `QueueHostedService`De, kuyruktaki arka plan görevleri <xref:Microsoft.Extensions.Hosting.BackgroundService>, uzun süre çalışan `IHostedService`uygulama için temel bir sınıf olan olarak kuyruğa alınır ve yürütülür.</span><span class="sxs-lookup"><span data-stu-id="42415-175">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="42415-176">Hizmetler ' de `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="42415-176">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="42415-177">Uygulama, `AddHostedService` uzantı yöntemiyle kaydedilir: `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="42415-177">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="42415-178">Dizin sayfası model sınıfında:</span><span class="sxs-lookup"><span data-stu-id="42415-178">In the Index page model class:</span></span>

* <span data-ttu-id="42415-179">Oluşturucuya eklenir ve öğesine `Queue`atanır. `IBackgroundTaskQueue`</span><span class="sxs-lookup"><span data-stu-id="42415-179">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="42415-180">Eklenen ve öğesine `_serviceScopeFactory`atandı. <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory></span><span class="sxs-lookup"><span data-stu-id="42415-180">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="42415-181">Fabrika, bir kapsam içinde hizmet oluşturmak için <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>kullanılan örnekleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="42415-181">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="42415-182">Uygulama `AppDbContext` (bir tekil hizmet) içinde `IBackgroundTaskQueue` veritabanı kayıtları yazmak için uygulamanın ( [kapsamlı bir hizmet](xref:fundamentals/dependency-injection#service-lifetimes)) kullanılabilmesi için bir kapsam oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="42415-182">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="42415-183">Dizin `OnPostAddTask` sayfasında **Görev Ekle** düğmesi seçildiğinde Yöntem yürütülür.</span><span class="sxs-lookup"><span data-stu-id="42415-183">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="42415-184">`QueueBackgroundWorkItem`iş öğesini kuyruğa almak için çağrılır:</span><span class="sxs-lookup"><span data-stu-id="42415-184">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="42415-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="42415-185">Additional resources</span></span>

* [<span data-ttu-id="42415-186">Ihostedservice ve BackgroundService sınıfıyla mikro hizmetlerde arka plan görevleri uygulama</span><span class="sxs-lookup"><span data-stu-id="42415-186">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
