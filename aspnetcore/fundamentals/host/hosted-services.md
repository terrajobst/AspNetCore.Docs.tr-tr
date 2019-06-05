---
title: ASP.NET core'da barındırılan hizmetler ile arka plan görevleri
author: guardrex
description: ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/03/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 2dbb1a84a380ab06a4be7ecf628799a070afc9e3
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692519"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="a51e9-103">ASP.NET core'da barındırılan hizmetler ile arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="a51e9-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="a51e9-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a51e9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a51e9-105">ASP.NET Core, arka plan görevleri olarak uygulanabilir *barındırılan hizmetleri*.</span><span class="sxs-lookup"><span data-stu-id="a51e9-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="a51e9-106">Barındırılan hizmet arka plan görevi uygulayan bir mantıksal ile bir sınıftır <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="a51e9-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a51e9-107">Bu konu başlığı altında üç barındırılan hizmet örnekleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="a51e9-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="a51e9-108">Bir zamanlayıcıyı temel çalışan arka plan görev.</span><span class="sxs-lookup"><span data-stu-id="a51e9-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="a51e9-109">Barındırılan hizmet etkinleştiren bir [hizmet kapsamlı](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="a51e9-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="a51e9-110">Kapsamlı hizmet, bağımlılık ekleme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a51e9-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="a51e9-111">Sırayla çalışır kuyruğa alınmış arka plan görevleri.</span><span class="sxs-lookup"><span data-stu-id="a51e9-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="a51e9-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a51e9-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a51e9-113">Örnek uygulama, iki sürümde sağlanır:</span><span class="sxs-lookup"><span data-stu-id="a51e9-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="a51e9-114">Web ana bilgisayar &ndash; Web ana bilgisayarı, web uygulamalarını barındırmak için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="a51e9-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="a51e9-115">Bu konu başlığında gösterilen örnek kodunu Web ana bilgisayar örneği sürümünden ' dir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="a51e9-116">Daha fazla bilgi için [Web ana bilgisayarı](xref:fundamentals/host/web-host) konu.</span><span class="sxs-lookup"><span data-stu-id="a51e9-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="a51e9-117">Genel konak &ndash; ASP.NET Core 2.1 içinde genel ana bilgisayar içindir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a51e9-118">Daha fazla bilgi için [genel ana bilgisayar](xref:fundamentals/host/generic-host) konu.</span><span class="sxs-lookup"><span data-stu-id="a51e9-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="worker-service-template"></a><span data-ttu-id="a51e9-119">Çalışan hizmet şablonu</span><span class="sxs-lookup"><span data-stu-id="a51e9-119">Worker Service template</span></span>

<span data-ttu-id="a51e9-120">ASP.NET Core çalışan hizmet şablonu yazmak için bir başlangıç noktası sağlar. hizmet uygulamaları uzun süre çalışan.</span><span class="sxs-lookup"><span data-stu-id="a51e9-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="a51e9-121">Şablon, bir barındırılan hizmet uygulaması için temel olarak kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="a51e9-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a51e9-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a51e9-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a51e9-123">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a51e9-123">Create a new project.</span></span>
1. <span data-ttu-id="a51e9-124">Seçin **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="a51e9-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a51e9-125">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="a51e9-125">Select **Next**.</span></span>
1. <span data-ttu-id="a51e9-126">Bir proje adı belirtin **proje adı** alan veya varsayılan proje adı kabul edin.</span><span class="sxs-lookup"><span data-stu-id="a51e9-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="a51e9-127">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="a51e9-127">Select **Create**.</span></span>
1. <span data-ttu-id="a51e9-128">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim kutusunda onaylayın **.NET Core** ve **ASP.NET Core 3.0** seçilir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="a51e9-129">Seçin **çalışan hizmet** şablonu.</span><span class="sxs-lookup"><span data-stu-id="a51e9-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="a51e9-130">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="a51e9-130">Select **Create**.</span></span>

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[<span data-ttu-id="a51e9-131">Visual Studio Code / .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a51e9-131">Visual Studio Code / .NET Core CLI</span></span>](#tab/visual-studio-code+netcore-cli)

<span data-ttu-id="a51e9-132">Çalışan hizmetin kullanın (`worker`) şablonuyla [yeni dotnet](/dotnet/core/tools/dotnet-new) komut kabuğu komutunu.</span><span class="sxs-lookup"><span data-stu-id="a51e9-132">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="a51e9-133">Aşağıdaki örnekte, bir çalışan hizmet uygulaması adlandırılmış oluşturulduğunda `ContosoWorkerService`.</span><span class="sxs-lookup"><span data-stu-id="a51e9-133">In the following example, a Worker Service app is created named `ContosoWorkerService`.</span></span> <span data-ttu-id="a51e9-134">İçin bir klasör `ContosoWorkerService` uygulama komut yürütülürken otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a51e9-134">A folder for the `ContosoWorkerService` app is created automatically when the command is executed.</span></span>

```console
dotnet new worker -o ContosoWorkerService
```

---

::: moniker-end

## <a name="package"></a><span data-ttu-id="a51e9-135">Paket</span><span class="sxs-lookup"><span data-stu-id="a51e9-135">Package</span></span>

<span data-ttu-id="a51e9-136">Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) paket.</span><span class="sxs-lookup"><span data-stu-id="a51e9-136">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="a51e9-137">Ihostedservice arabirimi</span><span class="sxs-lookup"><span data-stu-id="a51e9-137">IHostedService interface</span></span>

<span data-ttu-id="a51e9-138">Barındırılan hizmetler uygulama <xref:Microsoft.Extensions.Hosting.IHostedService> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="a51e9-138">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a51e9-139">Ana bilgisayar tarafından yönetilen nesneler için iki yöntem arabirimin tanımlar:</span><span class="sxs-lookup"><span data-stu-id="a51e9-139">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="a51e9-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` arka plan görevini başlatmak için mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-140">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="a51e9-141">Kullanırken [Web ana bilgisayarı](xref:fundamentals/host/web-host), `StartAsync` sunucu yeniden başlatıldıktan sonra çağrılır ve [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-141">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="a51e9-142">Kullanırken [genel ana bilgisayar](xref:fundamentals/host/generic-host), `StartAsync` önce çağrılır `ApplicationStarted` tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-142">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="a51e9-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; konak normal şekilde kapatılmasını gerçekleştirirken tetiklendi.</span><span class="sxs-lookup"><span data-stu-id="a51e9-143">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="a51e9-144">`StopAsync` arka plan görevini sonlandırmak için mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-144">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="a51e9-145">Uygulama <xref:System.IDisposable> ve [sonlandırıcılar (Yıkıcılar)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) yönetilmeyen tüm kaynaklarını silmek için.</span><span class="sxs-lookup"><span data-stu-id="a51e9-145">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="a51e9-146">İptal belirteci kapatma işlemi artık normal gerektiğini belirtmek için beş varsayılan bir ikinci zaman aşımı vardır.</span><span class="sxs-lookup"><span data-stu-id="a51e9-146">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="a51e9-147">İptal belirteci üzerinde güncel olduğunda isteniyor:</span><span class="sxs-lookup"><span data-stu-id="a51e9-147">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="a51e9-148">Uygulama gerçekleştiren kalan arka plan işlemleri durduruldu.</span><span class="sxs-lookup"><span data-stu-id="a51e9-148">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="a51e9-149">Çağrılma yeri herhangi bir yöntem `StopAsync` en kısa sürede döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-149">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="a51e9-150">Ancak, İptal işlemi istendikten sonra görev terk olmayan&mdash;çağıran tüm görevleri tamamlamak için bekler.</span><span class="sxs-lookup"><span data-stu-id="a51e9-150">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="a51e9-151">Uygulama beklenmedik bir şekilde (örneğin, uygulamanın işlemi başarısız olur), kapanırsa `StopAsync` değil olarak adlandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-151">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="a51e9-152">Bu nedenle, herhangi bir yöntem de çağrıldığında veya gerçekleştirilen işlemleri de `StopAsync` değil oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-152">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="a51e9-153">Varsayılan beş ikinci kapatma zaman aşımını uzatmak için aşağıdakileri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="a51e9-153">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="a51e9-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> Genel konak kullanırken.</span><span class="sxs-lookup"><span data-stu-id="a51e9-154"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="a51e9-155">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a51e9-155">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="a51e9-156">Web ana bilgisayarı kullanırken kapatma zaman aşımı ana bilgisayar yapılandırma ayarı.</span><span class="sxs-lookup"><span data-stu-id="a51e9-156">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="a51e9-157">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a51e9-157">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="a51e9-158">Barındırılan hizmet uygulamasını başlangıçta kez etkinleştirilir ve uygulama kapatma sırasında düzgün biçimde kapatılabilir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-158">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="a51e9-159">Arka plan görevi yürütme sırasında bir hata oluşturulursa `Dispose` çağrılmalıdır bile `StopAsync` adı değil.</span><span class="sxs-lookup"><span data-stu-id="a51e9-159">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="a51e9-160">Zamanlanmış arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="a51e9-160">Timed background tasks</span></span>

<span data-ttu-id="a51e9-161">Zamanlanmış arka plan görevi kullanır [süre System.Threading.Timer](xref:System.Threading.Timer) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a51e9-161">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="a51e9-162">Görev Zamanlayıcı tetiklendikten `DoWork` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a51e9-162">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="a51e9-163">Zamanlayıcıyı devre dışı `StopAsync` ve hizmet kapsayıcısı üzerinde silinmediğinde `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="a51e9-163">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="a51e9-164">Hizmet kayıtlı `Startup.ConfigureServices` ile `AddHostedService` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a51e9-164">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="a51e9-165">Kapsamlı bir hizmet bir arka plan görevi olarak kullanma</span><span class="sxs-lookup"><span data-stu-id="a51e9-165">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="a51e9-166">Kullanılacak [Hizmetleri kapsamlı](xref:fundamentals/dependency-injection#service-lifetimes) içinde bir `IHostedService`, bir kapsam oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a51e9-166">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="a51e9-167">Kapsam, bir barındırılan hizmet için varsayılan olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a51e9-167">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="a51e9-168">Kapsamlı bir arka plan görev hizmeti arka plan görev mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="a51e9-168">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="a51e9-169">Aşağıdaki örnekte, bir <xref:Microsoft.Extensions.Logging.ILogger> hizmetinde eklenmiş olur:</span><span class="sxs-lookup"><span data-stu-id="a51e9-169">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="a51e9-170">Barındırılan hizmet çağırmak için kapsamlı bir arka plan görev hizmeti çözümlemek için bir kapsam oluşturur, `DoWork` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a51e9-170">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="a51e9-171">Hizmetleri kayıtlı `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a51e9-171">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a51e9-172">`IHostedService` Uygulaması ile kayıtlı `AddHostedService` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a51e9-172">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="a51e9-173">Sıraya alınan arka plan görevleri</span><span class="sxs-lookup"><span data-stu-id="a51e9-173">Queued background tasks</span></span>

<span data-ttu-id="a51e9-174">Arka plan görev sırası, .NET tabanlı 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([ASP.NET Core 3.0 için yerleşik olarak geçici olarak zamanlanmış](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="a51e9-174">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="a51e9-175">İçinde `QueueHostedService`, arka plan görevleri sırasındaki sıradan çıkarılan ve olarak yürütülen bir <xref:Microsoft.Extensions.Hosting.BackgroundService>, uzun çalışan bir uygulama için bir temel sınıf olan `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a51e9-175">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="a51e9-176">Hizmetleri kayıtlı `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a51e9-176">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a51e9-177">`IHostedService` Uygulaması ile kayıtlı `AddHostedService` genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a51e9-177">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="a51e9-178">Dizin Sayfası model sınıfı:</span><span class="sxs-lookup"><span data-stu-id="a51e9-178">In the Index page model class:</span></span>

* <span data-ttu-id="a51e9-179">`IBackgroundTaskQueue` Oluşturucuya eklenen ve atanan `Queue`.</span><span class="sxs-lookup"><span data-stu-id="a51e9-179">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="a51e9-180">Bir <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> eklenen ve atanan `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="a51e9-180">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="a51e9-181">Factory örnekleri oluşturmak için kullanılan <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, bir kapsamdaki hizmetleri oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="a51e9-181">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="a51e9-182">Uygulamanın kullanmak için bir kapsam oluşturulan `AppDbContext` (bir [hizmet kapsamlı](xref:fundamentals/dependency-injection#service-lifetimes)) veritabanı kayıtları yazmak için `IBackgroundTaskQueue` (tek bir hizmet).</span><span class="sxs-lookup"><span data-stu-id="a51e9-182">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="a51e9-183">Zaman **Görev Ekle** düğmesi seçili dizin sayfasında `OnPostAddTask` yöntemi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="a51e9-183">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="a51e9-184">`QueueBackgroundWorkItem` İş öğesini kuyruğa çağrılır:</span><span class="sxs-lookup"><span data-stu-id="a51e9-184">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="a51e9-185">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a51e9-185">Additional resources</span></span>

* [<span data-ttu-id="a51e9-186">Ihostedservice ve BackgroundService sınıfı ile mikro hizmetler içindeki arka plan görevlerini uygulama</span><span class="sxs-lookup"><span data-stu-id="a51e9-186">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
