---
title: ASP.NET core'da bağımlılık ekleme
author: guardrex
description: ASP.NET Core bağımlılık ekleme nasıl uyguladığını ve nasıl kullanılacağını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/dependency-injection
ms.openlocfilehash: df5bc21f9b93206b3cfc97a052df26b891930d23
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902573"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="5af32-103">ASP.NET core'da bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="5af32-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="5af32-104">Tarafından [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5af32-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5af32-105">ASP.NET Core destekleyen bir tekniktir elde etmek için bağımlılık ekleme (dı) yazılım tasarım deseni [denetimi tersine çevirme (IOC)](https://deviq.com/inversion-of-control/) sınıfları ve bunların bağımlılıklarını arasında.</span><span class="sxs-lookup"><span data-stu-id="5af32-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](https://deviq.com/inversion-of-control/) between classes and their dependencies.</span></span>

<span data-ttu-id="5af32-106">Özel bağımlılık ekleme MVC denetleyicileri içinde daha fazla bilgi için bkz. <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="5af32-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="5af32-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5af32-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="5af32-108">Bağımlılık ekleme genel bakış</span><span class="sxs-lookup"><span data-stu-id="5af32-108">Overview of dependency injection</span></span>

<span data-ttu-id="5af32-109">A *bağımlılık* olan başka bir nesneye gerektiren herhangi bir nesne.</span><span class="sxs-lookup"><span data-stu-id="5af32-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="5af32-110">Aşağıdaki inceleyin `MyDependency` sınıfıyla birlikte bir `WriteMessage` diğer sınıfların bir uygulamada bağlı yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5af32-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

```csharp
public class MyDependency
{
    public MyDependency()
    {
    }

    public Task WriteMessage(string message)
    {
        Console.WriteLine(
            $"MyDependency.WriteMessage called. Message: {message}");

        return Task.FromResult(0);
    }
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5af32-111">Örneği `MyDependency` sınıfı hale getirmek için oluşturulabilir `WriteMessage` yöntemi bir sınıf için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="5af32-112">`MyDependency` Sınıfı, bağımlılık olarak `IndexModel` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="5af32-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _myDependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="5af32-113">Örneği `MyDependency` sınıfı hale getirmek için oluşturulabilir `WriteMessage` yöntemi bir sınıf için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-113">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="5af32-114">`MyDependency` Sınıfı, bağımlılık olarak `HomeController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="5af32-114">The `MyDependency` class is a dependency of the `HomeController` class:</span></span>

```csharp
public class HomeController : Controller
{
    MyDependency _dependency = new MyDependency();

    public async Task<IActionResult> Index()
    {
        await _myDependency.WriteMessage(
            "HomeController.Index created this message.");

        return View();
    }
}
```

::: moniker-end

<span data-ttu-id="5af32-115">Bir sınıf oluşturur ve doğrudan bağlı `MyDependency` örneği.</span><span class="sxs-lookup"><span data-stu-id="5af32-115">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="5af32-116">Kod bağımlılıklarını (örneğin, önceki örnekte) sorunlu ve aşağıdaki nedenlerle kaçınılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="5af32-116">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="5af32-117">Değiştirilecek `MyDependency` sınıfı farklı bir uygulama ile değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5af32-117">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="5af32-118">Varsa `MyDependency` bağımlılıkları varsa, sınıf tarafından yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5af32-118">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="5af32-119">Yapılandırmanıza bağlı olarak birden çok sınıf ile büyük bir projedeki `MyDependency`, uygulama yapılandırma kodu dağılmış olur.</span><span class="sxs-lookup"><span data-stu-id="5af32-119">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="5af32-120">Bu uygulama için birim testi zordur.</span><span class="sxs-lookup"><span data-stu-id="5af32-120">This implementation is difficult to unit test.</span></span> <span data-ttu-id="5af32-121">Uygulamayı bir sahte kullanın veya saplama `MyDependency` sınıfı bu yaklaşımı mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="5af32-121">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="5af32-122">Bağımlılık ekleme aracılığıyla bu sorunları ele alır:</span><span class="sxs-lookup"><span data-stu-id="5af32-122">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="5af32-123">Bağımlılık uygulama soyutlamak için arabirim kullanımı.</span><span class="sxs-lookup"><span data-stu-id="5af32-123">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="5af32-124">Bir hizmet kapsayıcısı bağımlılığı kaydı.</span><span class="sxs-lookup"><span data-stu-id="5af32-124">Registration of the dependency in a service container.</span></span> <span data-ttu-id="5af32-125">ASP.NET Core sağlayan bir yerleşik hizmet kapsayıcı [IServiceProvider](/dotnet/api/system.iserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="5af32-125">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="5af32-126">Hizmetleri uygulamanın kayıtlı `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5af32-126">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="5af32-127">*Ekleme* hizmetinin, kullanıldığı sınıfının oluşturucusu, içine.</span><span class="sxs-lookup"><span data-stu-id="5af32-127">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="5af32-128">Framework bağımlılığı örneği oluşturma ve artık gerekli değilse bunun atılması sorumluluğu alır.</span><span class="sxs-lookup"><span data-stu-id="5af32-128">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="5af32-129">İçinde [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` arabirim uygulamasına hizmet sağlayan bir yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="5af32-129">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5af32-130">Bu arabirimin somut bir tür tarafından uygulanan `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="5af32-130">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5af32-131">`MyDependency` istekler bir [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) kendi oluşturucusuna.</span><span class="sxs-lookup"><span data-stu-id="5af32-131">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="5af32-132">Bağımlılık ekleme zincirleme bir şekilde kullanmak alışılmadık bir durum değildir.</span><span class="sxs-lookup"><span data-stu-id="5af32-132">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="5af32-133">İstenen her bağımlılık sırayla kendi bağımlılıkları ister.</span><span class="sxs-lookup"><span data-stu-id="5af32-133">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="5af32-134">Kapsayıcı graftaki bağımlılıklarını çözen ve tam olarak çözümlenmiş hizmeti döndürür.</span><span class="sxs-lookup"><span data-stu-id="5af32-134">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="5af32-135">Toplu kümesi çözümlenemedi bağımlılıklar, tipik olarak adlandırılır bir *Bağımlılık ağacı*, *bağımlılık grafiği*, veya *Nesne grafiği*.</span><span class="sxs-lookup"><span data-stu-id="5af32-135">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="5af32-136">`IMyDependency` ve `ILogger<TCategoryName>` service kapsayıcısında kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5af32-136">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="5af32-137">`IMyDependency` kaydedilmiştir `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5af32-137">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5af32-138">`ILogger<TCategoryName>` senaryomuz için günlük soyutlama altyapısı tarafından kayıtlı bir [framework tarafından sağlanan hizmet](#framework-provided-services) framework tarafından varsayılan olarak kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="5af32-138">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="5af32-139">Örnek uygulamada `IMyDependency` hizmet somut bir türde ile kayıtlı `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="5af32-139">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="5af32-140">Tek bir isteğin ömrü hizmet ömrü kayıt kapsamları.</span><span class="sxs-lookup"><span data-stu-id="5af32-140">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="5af32-141">[Hizmet ömrü](#service-lifetimes) bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5af32-141">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="5af32-142">Her `services.Add{SERVICE_NAME}` genişletme yöntemi ekler (ve büyük olasılıkla yapılandırır) Hizmetleri.</span><span class="sxs-lookup"><span data-stu-id="5af32-142">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="5af32-143">Örneğin, `services.AddMvc()` Razor sayfaları ve MVC gerekli hizmetleri ekler.</span><span class="sxs-lookup"><span data-stu-id="5af32-143">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="5af32-144">Uygulamalar bu kurala uymayan önerilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-144">We recommended that apps follow this convention.</span></span> <span data-ttu-id="5af32-145">Yerleştirin alanında uzantı yöntemlerini [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) hizmet kayıtları grupları kapsüllemek için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="5af32-145">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="5af32-146">Hizmetin Oluşturucusu basit bir tür gibi gerektiriyorsa bir `string`, temel kullanarak yerleştirilebilir [yapılandırma](xref:fundamentals/configuration/index) veya [seçenekleri deseni](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="5af32-146">If the service's constructor requires a primitive, such as a `string`, the primitive can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

```csharp
public class MyDependency : IMyDependency
{
    public MyDependency(IConfiguration config)
    {
        var myStringValue = config["MyStringKey"];

        // Use myStringValue
    }

    ...
}
```

<span data-ttu-id="5af32-147">Hizmet nerede ve özel bir alana atanan bir sınıfın Oluşturucusu aracılığıyla Hizmeti'nin bir örneğini istenir.</span><span class="sxs-lookup"><span data-stu-id="5af32-147">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="5af32-148">Alan sınıfı boyunca gerektiği gibi hizmete erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5af32-148">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="5af32-149">Örnek uygulamada `IMyDependency` örneği istenen ve hizmetin çağırmak için kullanılan `WriteMessage` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5af32-149">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/MyDependencyController.cs?name=snippet1&highlight=3,5-8,13-14)]

::: moniker-end

## <a name="framework-provided-services"></a><span data-ttu-id="5af32-150">Framework tarafından sağlanan hizmetleri</span><span class="sxs-lookup"><span data-stu-id="5af32-150">Framework-provided services</span></span>

<span data-ttu-id="5af32-151">`Startup.ConfigureServices` Yöntemi, Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri dahil olmak üzere uygulamanın kullandığı hizmetleri tanımlamak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="5af32-151">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="5af32-152">Başlangıçta `IServiceCollection` sağlanan `ConfigureServices` tanımlanan aşağıdaki Hizmetleri (bağlı olarak [konak nasıl yapılandırılan](xref:fundamentals/host/index)):</span><span class="sxs-lookup"><span data-stu-id="5af32-152">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/host/index)):</span></span>

| <span data-ttu-id="5af32-153">Hizmet Türü</span><span class="sxs-lookup"><span data-stu-id="5af32-153">Service Type</span></span> | <span data-ttu-id="5af32-154">Ömür</span><span class="sxs-lookup"><span data-stu-id="5af32-154">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="5af32-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="5af32-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="5af32-156">Geçici</span><span class="sxs-lookup"><span data-stu-id="5af32-156">Transient</span></span> |
| [<span data-ttu-id="5af32-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="5af32-157">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="5af32-158">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-158">Singleton</span></span> |
| [<span data-ttu-id="5af32-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="5af32-159">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="5af32-160">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-160">Singleton</span></span> |
| [<span data-ttu-id="5af32-161">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="5af32-161">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="5af32-162">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-162">Singleton</span></span> |
| [<span data-ttu-id="5af32-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="5af32-163">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="5af32-164">Geçici</span><span class="sxs-lookup"><span data-stu-id="5af32-164">Transient</span></span> |
| [<span data-ttu-id="5af32-165">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="5af32-165">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="5af32-166">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-166">Singleton</span></span> |
| [<span data-ttu-id="5af32-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="5af32-167">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="5af32-168">Geçici</span><span class="sxs-lookup"><span data-stu-id="5af32-168">Transient</span></span> |
| [<span data-ttu-id="5af32-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="5af32-169">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="5af32-170">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-170">Singleton</span></span> |
| [<span data-ttu-id="5af32-171">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="5af32-171">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="5af32-172">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-172">Singleton</span></span> |
| [<span data-ttu-id="5af32-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="5af32-173">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="5af32-174">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-174">Singleton</span></span> |
| [<span data-ttu-id="5af32-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="5af32-175">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="5af32-176">Geçici</span><span class="sxs-lookup"><span data-stu-id="5af32-176">Transient</span></span> |
| [<span data-ttu-id="5af32-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="5af32-177">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="5af32-178">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-178">Singleton</span></span> |
| [<span data-ttu-id="5af32-179">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="5af32-179">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="5af32-180">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-180">Singleton</span></span> |
| [<span data-ttu-id="5af32-181">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="5af32-181">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="5af32-182">singleton</span><span class="sxs-lookup"><span data-stu-id="5af32-182">Singleton</span></span> |

<span data-ttu-id="5af32-183">Hizmet koleksiyonu genişletme yöntemi, kayıt hizmeti (ve gerekirse, bağımlı hizmetler) kullanılabilir olduğunda, kuralı tek bir kullanmaktır `Add{SERVICE_NAME}` genişletme yöntemi bu hizmet tarafından gerekli tüm hizmetleri kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="5af32-183">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="5af32-184">Aşağıdaki kod, genişletme yöntemleri kullanarak kapsayıcıya ek hizmetleri ekleme örneğidir [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), ve [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span><span class="sxs-lookup"><span data-stu-id="5af32-184">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddMvc();
}
```

<span data-ttu-id="5af32-185">Daha fazla bilgi için [ServiceCollection sınıfı](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) API belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="5af32-185">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="5af32-186">Hizmet yaşam süresi yok</span><span class="sxs-lookup"><span data-stu-id="5af32-186">Service lifetimes</span></span>

<span data-ttu-id="5af32-187">Kayıtlı her hizmet için uygun bir yaşam süresi'ni seçin.</span><span class="sxs-lookup"><span data-stu-id="5af32-187">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="5af32-188">ASP.NET Core Hizmetleri ile aşağıdaki ömürleri yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="5af32-188">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="5af32-189">**Geçici**</span><span class="sxs-lookup"><span data-stu-id="5af32-189">**Transient**</span></span>

<span data-ttu-id="5af32-190">Geçici ömrü Hizmetleri, bunlar istenen her zaman oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5af32-190">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="5af32-191">Bu yaşam süresi, basit, durum bilgisi olmayan hizmetler için en iyi çalışır.</span><span class="sxs-lookup"><span data-stu-id="5af32-191">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="5af32-192">**Kapsamlı**</span><span class="sxs-lookup"><span data-stu-id="5af32-192">**Scoped**</span></span>

<span data-ttu-id="5af32-193">Kapsamlı ömrü Hizmetleri, istek başına bir kez oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5af32-193">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="5af32-194">Kapsamlı bir hizmeti bir ara yazılımında kullanılırken, hizmette oturum ekleme `Invoke` veya `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5af32-194">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="5af32-195">Bir tekliyi gibi davranmaya hizmet zorladığından Oluşturucu ekleme ekleme yok.</span><span class="sxs-lookup"><span data-stu-id="5af32-195">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="5af32-196">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="5af32-196">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="5af32-197">**singleton**</span><span class="sxs-lookup"><span data-stu-id="5af32-197">**Singleton**</span></span>

<span data-ttu-id="5af32-198">Singleton ömrü Hizmetleri istenen ilk kez oluşturulur (veya `ConfigureServices` çalıştırılır ve örneği hizmet kaydı ile belirtilir).</span><span class="sxs-lookup"><span data-stu-id="5af32-198">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="5af32-199">Sonraki her istek, aynı örneğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5af32-199">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="5af32-200">Uygulama tekil davranışı gerektiriyorsa, hizmetin ömrünü yönetmek hizmet kapsayıcı izin vererek önerilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-200">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="5af32-201">Yoksa, tekil tasarım desenini uygulama ve sınıfında nesnenin ömrünü yönetmek üzere kullanıcı kodunun sağlayın.</span><span class="sxs-lookup"><span data-stu-id="5af32-201">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="5af32-202">Tek bir kapsamlı hizmet çözümlenecek tehlikelidir.</span><span class="sxs-lookup"><span data-stu-id="5af32-202">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="5af32-203">Bu, sonraki istekleri işleme sırasında hatalı duruma sahip hizmetinin neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-203">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="5af32-204">Oluşturucu yerleştirme davranışı</span><span class="sxs-lookup"><span data-stu-id="5af32-204">Constructor injection behavior</span></span>

<span data-ttu-id="5af32-205">Hizmetleri iki mekanizma tarafından çözülebilir:</span><span class="sxs-lookup"><span data-stu-id="5af32-205">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="5af32-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; nesne oluşturma bağımlılık ekleme kapsayıcısına hizmet kaydı olmadan izin verir.</span><span class="sxs-lookup"><span data-stu-id="5af32-206">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="5af32-207">`ActivatorUtilities` Etiket Yardımcıları, MVC denetleyicileri, SignalR hub'ları ve model bağlayıcılar gibi kullanıcıya yönelik soyutlama ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5af32-207">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, SignalR Hubs, and model binders.</span></span>

<span data-ttu-id="5af32-208">Oluşturucular, bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bağımsız değişken varsayılan değerleri atamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5af32-208">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="5af32-209">Ne zaman Hizmetleri çözülmüş tarafından `IServiceProvider` veya `ActivatorUtilities`, oluşturucu ekleme gerektiren bir *genel* Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="5af32-209">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="5af32-210">Ne zaman Hizmetleri çözülmüş tarafından `ActivatorUtilities`, oluşturucu ekleme gerektirir, yalnızca bir geçerli oluşturucusu yok.</span><span class="sxs-lookup"><span data-stu-id="5af32-210">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="5af32-211">Oluşturucu aşırı yüklemeleri tarafından desteklenir, ancak yalnızca bir aşırı yükleme bağımsız değişkenleri tüm bağımlılık ekleme tarafından yerine getirilmesi bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-211">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="5af32-212">Entity Framework bağlamları</span><span class="sxs-lookup"><span data-stu-id="5af32-212">Entity Framework contexts</span></span>

<span data-ttu-id="5af32-213">Entity Framework bağlamları kapsamlı ömrü kullanarak hizmet kapsayıcıya eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5af32-213">Entity Framework contexts should be added to the service container using the scoped lifetime.</span></span> <span data-ttu-id="5af32-214">Bu otomatik olarak bir çağrı ile işlenir [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) veritabanı bağlamı kaydederken yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5af32-214">This is handled automatically with a call to the [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) method when registering the database context.</span></span> <span data-ttu-id="5af32-215">Veritabanı bağlamı kullanan hizmetler ayrıca kapsamlı ömrü kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="5af32-215">Services that use the database context should also use the scoped lifetime.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="5af32-216">Yaşam süresi ve kayıt seçenekleri</span><span class="sxs-lookup"><span data-stu-id="5af32-216">Lifetime and registration options</span></span>

<span data-ttu-id="5af32-217">Yaşam süresi ve kayıt seçenekleri arasındaki farkı göstermek için benzersiz bir tanımlayıcıya sahip bir işlem olarak görevleri temsil eden aşağıdaki arabirimlerinden göz önünde bulundurun. `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="5af32-217">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="5af32-218">Kapsayıcı, bir işlem hizmeti kullanım ömrü için aşağıdaki arabirimlerinden nasıl yapılandırıldığına bağlı olarak, aynı veya farklı bir örneğine bir sınıf tarafından istendiğinde hizmetinin sağlar:</span><span class="sxs-lookup"><span data-stu-id="5af32-218">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5af32-219">Arabirimler uygulanan `Operation` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5af32-219">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="5af32-220">`Operation` Oluşturucusu bir sağlanan değilse bir GUID oluşturur:</span><span class="sxs-lookup"><span data-stu-id="5af32-220">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5af32-221">Bir `OperationService` kayıtlı, bağlı her diğer `Operation` türleri.</span><span class="sxs-lookup"><span data-stu-id="5af32-221">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="5af32-222">Zaman `OperationService` istenen bağımlılık ekleme aldığı her hizmetin yeni bir örneğini veya bağımlı hizmetin lifetime öğesine göre var olan bir örneği.</span><span class="sxs-lookup"><span data-stu-id="5af32-222">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="5af32-223">İstendiğinde, geçici Hizmetleri oluşturulursa `OperationsId` , `IOperationTransient` hizmetidir farklı `OperationsId` , `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="5af32-223">If transient services are created when requested, the `OperationsId` of the `IOperationTransient` service is different than the `OperationsId` of the `OperationService`.</span></span> <span data-ttu-id="5af32-224">`OperationService` Yeni bir örneğini alır `IOperationTransient` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5af32-224">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="5af32-225">Yeni örnek farklı bir verir `OperationsId`.</span><span class="sxs-lookup"><span data-stu-id="5af32-225">The new instance yields a different `OperationsId`.</span></span>
* <span data-ttu-id="5af32-226">İstek başına kapsamlı Hizmetleri oluşturduysanız `OperationsId` , `IOperationScoped` hizmeti aynı olan `OperationService` istek içinde.</span><span class="sxs-lookup"><span data-stu-id="5af32-226">If scoped services are created per request, the `OperationsId` of the `IOperationScoped` service is the same as that of `OperationService` within a request.</span></span> <span data-ttu-id="5af32-227">Her iki hizmete istekler genelinde farklı bir paylaşım `OperationsId` değeri.</span><span class="sxs-lookup"><span data-stu-id="5af32-227">Across requests, both services share a different `OperationsId` value.</span></span>
* <span data-ttu-id="5af32-228">Singleton ve tek örnekli Hizmetleri oluşturulduktan sonra ve tüm istekler ve tüm hizmetlerde kullanılan `OperationsId` tüm hizmet istekler genelinde sabittir.</span><span class="sxs-lookup"><span data-stu-id="5af32-228">If singleton and singleton-instance services are created once and used across all requests and all services, the `OperationsId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5af32-229">İçinde `Startup.ConfigureServices`, her tür kapsayıcı adlandırılmış ömrü göre eklenir:</span><span class="sxs-lookup"><span data-stu-id="5af32-229">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=12-15,18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="5af32-230">`IOperationSingletonInstance` Hizmetinin belirli bir örneği, bilinen bir kimliği ile kullandığı `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="5af32-230">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="5af32-231">Bu tür (GUID'sine sıfır olur) kullanımda olmadığında işaretlenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="5af32-231">It's clear when this type is in use (its GUID is all zeroes).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5af32-232">Örnek uygulama, nesne kullanım ömrü içindeki ve arasındaki tek tek istekleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="5af32-232">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="5af32-233">Örnek uygulamanın `IndexModel` her tür istekleri `IOperation` türü ve `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="5af32-233">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="5af32-234">Sayfa sonra tüm sayfa modeli sınıfın ve hizmetin görüntüler `OperationId` özelliği aracılığıyla değerleri:</span><span class="sxs-lookup"><span data-stu-id="5af32-234">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="5af32-235">Örnek uygulama, nesne kullanım ömrü içindeki ve arasındaki tek tek istekleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="5af32-235">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="5af32-236">Örnek uygulamayı içeren bir `OperationsController` her tür isteklerine `IOperation` türü ve `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="5af32-236">The sample app includes an `OperationsController` that requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="5af32-237">`Index` Eylem ayarlar hizmetlerine `ViewBag` hizmetin görüntülenmesi için `OperationId` değerleri:</span><span class="sxs-lookup"><span data-stu-id="5af32-237">The `Index` action sets the services into the `ViewBag` for display of the service's `OperationId` values:</span></span>

[!code-csharp[](dependency-injection/samples/1.x/DependencyInjectionSample/Controllers/OperationsController.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5af32-238">İki aşağıdaki çıktıda gösterildiği iki isteği sonuçları:</span><span class="sxs-lookup"><span data-stu-id="5af32-238">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="5af32-239">**İlk istek:**</span><span class="sxs-lookup"><span data-stu-id="5af32-239">**First request:**</span></span>

<span data-ttu-id="5af32-240">Denetleyici işlemler:</span><span class="sxs-lookup"><span data-stu-id="5af32-240">Controller operations:</span></span>

<span data-ttu-id="5af32-241">Geçici: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="5af32-241">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="5af32-242">Kapsamlı: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="5af32-242">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="5af32-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="5af32-243">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="5af32-244">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="5af32-244">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="5af32-245">`OperationService` İşlemler:</span><span class="sxs-lookup"><span data-stu-id="5af32-245">`OperationService` operations:</span></span>

<span data-ttu-id="5af32-246">Geçici: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="5af32-246">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="5af32-247">Kapsamlı: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="5af32-247">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="5af32-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="5af32-248">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="5af32-249">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="5af32-249">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="5af32-250">**İkinci isteği:**</span><span class="sxs-lookup"><span data-stu-id="5af32-250">**Second request:**</span></span>

<span data-ttu-id="5af32-251">Denetleyici işlemler:</span><span class="sxs-lookup"><span data-stu-id="5af32-251">Controller operations:</span></span>

<span data-ttu-id="5af32-252">Geçici: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="5af32-252">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="5af32-253">Kapsamlı: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="5af32-253">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="5af32-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="5af32-254">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="5af32-255">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="5af32-255">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="5af32-256">`OperationService` İşlemler:</span><span class="sxs-lookup"><span data-stu-id="5af32-256">`OperationService` operations:</span></span>

<span data-ttu-id="5af32-257">Geçici: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="5af32-257">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="5af32-258">Kapsamlı: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="5af32-258">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="5af32-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="5af32-259">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="5af32-260">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="5af32-260">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="5af32-261">Hangi gözlemleyin `OperationId` değerleri, bir istek içinde ve istekler arasında değişir:</span><span class="sxs-lookup"><span data-stu-id="5af32-261">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="5af32-262">*Geçici* nesneleri farklı her zaman.</span><span class="sxs-lookup"><span data-stu-id="5af32-262">*Transient* objects are always different.</span></span> <span data-ttu-id="5af32-263">Unutmayın geçici `OperationId` için birinci ve ikinci istekleri için her ikisi de farklı bir değer `OperationService` işlemleri ve istekleri arasında.</span><span class="sxs-lookup"><span data-stu-id="5af32-263">Note that the transient `OperationId` value for both the first and second requests are different for both `OperationService` operations and across requests.</span></span> <span data-ttu-id="5af32-264">Her bir hizmet ve istek için yeni bir örnek sağlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5af32-264">A new instance is provided to each service and request.</span></span>
* <span data-ttu-id="5af32-265">*Kapsamlı* nesneleri, ancak farklı bir istek içinde aynı istekler arasında.</span><span class="sxs-lookup"><span data-stu-id="5af32-265">*Scoped* objects are the same within a request but different across requests.</span></span>
* <span data-ttu-id="5af32-266">*Singleton* nesneleri, her nesne ve bağımsız olarak her istek için aynı bir `Operation` örneği sağlanır `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5af32-266">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="5af32-267">Ana arama hizmetleri</span><span class="sxs-lookup"><span data-stu-id="5af32-267">Call services from main</span></span>

<span data-ttu-id="5af32-268">Oluşturma bir [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) ile [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) uygulamanın kapsamı içinde kapsamlı bir hizmet çözümlenecek.</span><span class="sxs-lookup"><span data-stu-id="5af32-268">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="5af32-269">Bu yaklaşım, başlangıçta başlatma görevleri çalıştırmak için kapsamlı bir hizmete erişmek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="5af32-269">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="5af32-270">Aşağıdaki örnek için bir bağlam elde etme gösterir `MyScopedService` içinde `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="5af32-270">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = CreateWebHostBuilder(args).Build();

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a><span data-ttu-id="5af32-271">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="5af32-271">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5af32-272">Uygulama geliştirme ortamında çalışırken, varsayılan hizmet sağlayıcısını doğrulamak için denetimleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="5af32-272">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="5af32-273">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="5af32-273">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="5af32-274">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değildir.</span><span class="sxs-lookup"><span data-stu-id="5af32-274">Scoped services aren't directly or indirectly injected into singletons.</span></span>

::: moniker-end

<span data-ttu-id="5af32-275">Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5af32-275">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="5af32-276">Kök hizmet sağlayıcısının bir ömür zaman sağlayıcısı uygulamayla başlar ve uygulama kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="5af32-276">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="5af32-277">Kapsamlı Hizmetleri, onları oluşturan kapsayıcı tarafından elden çıkarılmasını.</span><span class="sxs-lookup"><span data-stu-id="5af32-277">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="5af32-278">Kapsamlı bir hizmet içinde kök kapsayıcı oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından bırakılmadan olduğundan hizmet ömrü tekliye etkili bir şekilde yükseltilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-278">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="5af32-279">Hizmet kapsamları doğrulama yakalar bu gibi durumlarda, `BuildServiceProvider` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5af32-279">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="5af32-280">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="5af32-280">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="5af32-281">Hizmet isteği</span><span class="sxs-lookup"><span data-stu-id="5af32-281">Request Services</span></span>

<span data-ttu-id="5af32-282">Bir ASP.NET Core içinde kullanılabilen hizmetler request `HttpContext` aracılığıyla kullanıma [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="5af32-282">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="5af32-283">İstek Hizmetleri yapılandırılmış ve uygulamanın bir parçası istenen Hizmetleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="5af32-283">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="5af32-284">Nesneleri bağımlılıkları belirttiğinizde, bu bulunan türleri tarafından karşılandığından `RequestServices`değil `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="5af32-284">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="5af32-285">Genel olarak, uygulamayı doğrudan bu özellikleri kullanmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="5af32-285">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="5af32-286">Bunun yerine, sınıfları sınıf oluşturucuları gerektirir ve framework izin türlerini bağımlılıkları ekleme isteği.</span><span class="sxs-lookup"><span data-stu-id="5af32-286">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="5af32-287">Bu, test etmek daha kolay sınıfları sağlar (bkz [Test ve hata ayıklama](xref:test/index) konuları).</span><span class="sxs-lookup"><span data-stu-id="5af32-287">This yields classes that are easier to test (see the [Test and debug](xref:test/index) topics).</span></span>

> [!NOTE]
> <span data-ttu-id="5af32-288">Erişim için oluşturucu parametresi olarak bağımlılıkları isteyen tercih `RequestServices` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="5af32-288">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="5af32-289">Tasarım Hizmetleri için bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="5af32-289">Design services for dependency injection</span></span>

<span data-ttu-id="5af32-290">İçin en iyi uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="5af32-290">Best practices are to:</span></span>

* <span data-ttu-id="5af32-291">Bağımlılık ekleme bağımlılıklarını almak için kullanmak üzere hizmetlerin tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="5af32-291">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="5af32-292">Durum bilgisi olan ve statik yöntem çağrıları kaçının (olarak bilinen bir yöntem [statik cling](https://deviq.com/static-cling/)).</span><span class="sxs-lookup"><span data-stu-id="5af32-292">Avoid stateful, static method calls (a practice known as [static cling](https://deviq.com/static-cling/)).</span></span>
* <span data-ttu-id="5af32-293">Bağımlı hizmetleri sınıfları doğrudan örneğinin kaçının.</span><span class="sxs-lookup"><span data-stu-id="5af32-293">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="5af32-294">Doğrudan, kodu belirli bir uygulamaya couples.</span><span class="sxs-lookup"><span data-stu-id="5af32-294">Direct instantiation couples the code to a particular implementation.</span></span>

<span data-ttu-id="5af32-295">İzleyerek [KATI ilkeler, nesne yönelimli tasarım](https://deviq.com/solid/), uygulama sınıfları doğal olarak eğilimli küçük, katsayıları iyi belirlenmiş ve kolayca test edilmiş olması.</span><span class="sxs-lookup"><span data-stu-id="5af32-295">By following the [SOLID Principles of Object Oriented Design](https://deviq.com/solid/), app classes naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="5af32-296">Bir sınıf çok fazla eklenen bağımlılıklara sahip gibi görünüyor. Bu genellikle bir oturum sınıfı için çok fazla sorumluluklara sahiptir ve ihlal varsa, [tek sorumluluk İlkesi'ni (SRP)](https://deviq.com/single-responsibility-principle/).</span><span class="sxs-lookup"><span data-stu-id="5af32-296">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](https://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="5af32-297">Sınıfının yeni bir sınıf bazı sorumlulukları taşıyarak yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="5af32-297">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="5af32-298">Razor sayfaları sayfa modeli sınıfları ve MVC denetleyici sınıflarına kullanıcı Arabirimi konuları üzerinde durmalısınız aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="5af32-298">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="5af32-299">İş kuralları ve veri erişim uygulama ayrıntılarını saklanır bunları uygun sınıflardaki [konuları ayrı](https://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="5af32-299">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](https://deviq.com/separation-of-concerns/).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="5af32-300">Bırakma Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="5af32-300">Disposal of services</span></span>

<span data-ttu-id="5af32-301">Kapsayıcı çağrıları `Dispose` için `IDisposable` türleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="5af32-301">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="5af32-302">Bir örneği, kullanıcı kodu tarafından kapsayıcıya eklenirse, otomatik olarak elden değil.</span><span class="sxs-lookup"><span data-stu-id="5af32-302">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

```csharp
// Services that implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    // The container creates the following instances and disposes them automatically:
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // The container doesn't create the following instances, so it doesn't dispose of
    // the instances automatically:
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

::: moniker range="= aspnetcore-1.0"

> [!NOTE]
> <span data-ttu-id="5af32-303">ASP.NET Core 1.0, kapsayıcı çağırır dispose üzerinde *tüm* `IDisposable` yaramadı oluşturma dahil olmak üzere nesneleri.</span><span class="sxs-lookup"><span data-stu-id="5af32-303">In ASP.NET Core 1.0, the container calls dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

::: moniker-end

## <a name="default-service-container-replacement"></a><span data-ttu-id="5af32-304">Varsayılan hizmet kapsayıcısını değiştirme</span><span class="sxs-lookup"><span data-stu-id="5af32-304">Default service container replacement</span></span>

<span data-ttu-id="5af32-305">Yerleşik hizmet kapsayıcı framework ve çoğu tüketici uygulamalarına gereksinimlerini karşılamak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="5af32-305">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="5af32-306">Bunu desteklemiyor belirli bir özellik gerekmedikçe yerleşik kapsayıcı kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="5af32-306">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="5af32-307">Yerleşik kapsayıcısında bulunamadı 3 taraf kapsayıcıları destekleyen özelliklerden bazıları:</span><span class="sxs-lookup"><span data-stu-id="5af32-307">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="5af32-308">Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="5af32-308">Property injection</span></span>
* <span data-ttu-id="5af32-309">Adına göre ekleme</span><span class="sxs-lookup"><span data-stu-id="5af32-309">Injection based on name</span></span>
* <span data-ttu-id="5af32-310">Alt kapsayıcılar</span><span class="sxs-lookup"><span data-stu-id="5af32-310">Child containers</span></span>
* <span data-ttu-id="5af32-311">Özel ömür Yönetimi</span><span class="sxs-lookup"><span data-stu-id="5af32-311">Custom lifetime management</span></span>
* <span data-ttu-id="5af32-312">`Func<T>` Yavaş başlatma desteği</span><span class="sxs-lookup"><span data-stu-id="5af32-312">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="5af32-313">Bkz: [bağımlılık ekleme Benioku.MD dosyası](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection) bazı bağdaştırıcıları destekleyen kapsayıcıların listesi.</span><span class="sxs-lookup"><span data-stu-id="5af32-313">See the [Dependency Injection readme.md file](https://github.com/aspnet/DependencyInjection#using-other-containers-with-microsoftextensionsdependencyinjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="5af32-314">Aşağıdaki örnek yerleşik kapsayıcıyla değiştirir [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="5af32-314">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="5af32-315">Uygun bir kapsayıcı paketleri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="5af32-315">Install the appropriate container package(s):</span></span>

    * [<span data-ttu-id="5af32-316">Autofac</span><span class="sxs-lookup"><span data-stu-id="5af32-316">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
    * [<span data-ttu-id="5af32-317">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="5af32-317">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="5af32-318">Kapsayıcıda yapılandırma `Startup.ConfigureServices` ve dönüş bir `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="5af32-318">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

    ```csharp
    public IServiceProvider ConfigureServices(IServiceCollection services)
    {
        services.AddMvc();
        // Add other framework services

        // Add Autofac
        var containerBuilder = new ContainerBuilder();
        containerBuilder.RegisterModule<DefaultModule>();
        containerBuilder.Populate(services);
        var container = containerBuilder.Build();
        return new AutofacServiceProvider(container);
    }
    ```

    <span data-ttu-id="5af32-319">3 bir taraf kapsayıcı kullanılacak `Startup.ConfigureServices` döndürmelidir `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="5af32-319">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="5af32-320">İçinde Autofac yapılandırma `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="5af32-320">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="5af32-321">Çalışma zamanında Autofac, türleri çözümlemek ve bağımlılıkları ekleme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5af32-321">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="5af32-322">ASP.NET Core ile Autofac kullanma hakkında daha fazla bilgi edinmek için [Autofac belgeleri](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="5af32-322">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="5af32-323">İş parçacığı güvenliği</span><span class="sxs-lookup"><span data-stu-id="5af32-323">Thread safety</span></span>

<span data-ttu-id="5af32-324">Singleton hizmetinin iş parçacığı güvenli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5af32-324">Singleton services need to be thread safe.</span></span> <span data-ttu-id="5af32-325">Tek bir hizmet üzerinde geçici bir hizmet bağımlılığı varsa, geçici hizmet iş parçacığı tekli tarafından nasıl kullanıldığını bağlı olarak güvenli olacak şekilde de gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-325">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

<span data-ttu-id="5af32-326">İkinci bağımsız değişkeni gibi tek hizmet Üreteç yöntemi [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider, TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), değil iş parçacığı açısından güvenli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5af32-326">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="5af32-327">Gibi bir tür (`static`) oluşturucusu, onu garanti tek bir iş parçacığı tarafından bir kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-327">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="5af32-328">Önerileri</span><span class="sxs-lookup"><span data-stu-id="5af32-328">Recommendations</span></span>

<span data-ttu-id="5af32-329">Bağımlılık ekleme ile çalışırken, aşağıdaki önerileri göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5af32-329">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="5af32-330">Verileri ve Yapılandırma hizmeti kapsayıcısında doğrudan depolama kaçının.</span><span class="sxs-lookup"><span data-stu-id="5af32-330">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="5af32-331">Örneğin, bir kullanıcının alışveriş sepeti genellikle hizmet kapsayıcıya eklenen olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="5af32-331">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="5af32-332">Yapılandırma kullanması gereken [seçenekleri deseni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="5af32-332">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="5af32-333">Benzer şekilde, yalnızca başka bir nesnenin erişmesine izin vermek için mevcut "veri sahibi" nesneleri kaçının.</span><span class="sxs-lookup"><span data-stu-id="5af32-333">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="5af32-334">Bağımlılık ekleme aracılığıyla gerçek öğe mümkünse istemek daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="5af32-334">It's better to request the actual item via dependency injection, if possible.</span></span>

* <span data-ttu-id="5af32-335">Statik hizmetlere erişimi önlemek (örneğin, statik olarak yazmaya [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) kullanan başka bir yerde için).</span><span class="sxs-lookup"><span data-stu-id="5af32-335">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="5af32-336">Hizmet bulucu deseni kullanmaktan kaçının (örneğin, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span><span class="sxs-lookup"><span data-stu-id="5af32-336">Avoid using the service locator pattern (for example, [IServiceProvider.GetService](/dotnet/api/system.iserviceprovider.getservice)).</span></span>

* <span data-ttu-id="5af32-337">Statik erişimi önlemek `HttpContext` (örneğin, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span><span class="sxs-lookup"><span data-stu-id="5af32-337">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="5af32-338">Öneriler tüm kümesi gibi bir öneri yok sayılıyor gerekli olduğu durumlarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5af32-338">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="5af32-339">Özel durumlar nadir&mdash;çoğunlukla framework içinde özel durumlar.</span><span class="sxs-lookup"><span data-stu-id="5af32-339">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="5af32-340">Bağımlılık ekleme, bir *alternatif* statik/genel nesne erişim desenleri.</span><span class="sxs-lookup"><span data-stu-id="5af32-340">Dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="5af32-341">Statik nesne erişimi ile karışımı varsa, bağımlılık ekleme avantajlarından mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="5af32-341">You may not be able to realize the benefits of dependency injection if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5af32-342">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5af32-342">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/repository-pattern>
* <xref:fundamentals/startup>
* <xref:test/index>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="5af32-343">Bağımlılık ekleme (MSDN) ile ASP.NET Core kod yazma</span><span class="sxs-lookup"><span data-stu-id="5af32-343">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="5af32-344">Kapsayıcı-yönetilen uygulama tasarımı, tanıtımlar: Nereye ait kapsayıcı mu?</span><span class="sxs-lookup"><span data-stu-id="5af32-344">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [<span data-ttu-id="5af32-345">Özel bağımlılıklar İlkesi</span><span class="sxs-lookup"><span data-stu-id="5af32-345">Explicit Dependencies Principle</span></span>](https://deviq.com/explicit-dependencies-principle/)
* [<span data-ttu-id="5af32-346">Tersine çevirme denetimi kapsayıcıları ve bağımlılık ekleme desenini (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="5af32-346">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="5af32-347">Birleştirici (belirli bir uygulama kodu "yapıştırmak") yenilikler</span><span class="sxs-lookup"><span data-stu-id="5af32-347">New is Glue ("gluing" code to a particular implementation)</span></span>](https://ardalis.com/new-is-glue)
