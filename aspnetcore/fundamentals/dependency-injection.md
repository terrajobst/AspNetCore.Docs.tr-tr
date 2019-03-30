---
title: ASP.NET core'da bağımlılık ekleme
author: guardrex
description: ASP.NET Core bağımlılık ekleme nasıl uyguladığını ve nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: 8312f3375296a8530ac2db3db46d062b7b9e76b9
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750596"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="c6eeb-103">ASP.NET core'da bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="c6eeb-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="c6eeb-104">Tarafından [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c6eeb-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c6eeb-105">ASP.NET Core destekleyen bir tekniktir elde etmek için bağımlılık ekleme (dı) yazılım tasarım deseni [denetimi tersine çevirme (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) sınıfları ve bunların bağımlılıklarını arasında.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="c6eeb-106">Özel bağımlılık ekleme MVC denetleyicileri içinde daha fazla bilgi için bkz. <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="c6eeb-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c6eeb-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="c6eeb-108">Bağımlılık ekleme genel bakış</span><span class="sxs-lookup"><span data-stu-id="c6eeb-108">Overview of dependency injection</span></span>

<span data-ttu-id="c6eeb-109">A *bağımlılık* olan başka bir nesneye gerektiren herhangi bir nesne.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="c6eeb-110">Aşağıdaki inceleyin `MyDependency` sınıfıyla birlikte bir `WriteMessage` diğer sınıfların bir uygulamada bağlı yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="c6eeb-111">Örneği `MyDependency` sınıfı hale getirmek için oluşturulabilir `WriteMessage` yöntemi bir sınıf için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="c6eeb-112">`MyDependency` Sınıfı, bağımlılık olarak `IndexModel` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

```csharp
public class IndexModel : PageModel
{
    MyDependency _dependency = new MyDependency();

    public async Task OnGetAsync()
    {
        await _dependency.WriteMessage(
            "IndexModel.OnGetAsync created this message.");
    }
}
```

<span data-ttu-id="c6eeb-113">Bir sınıf oluşturur ve doğrudan bağlı `MyDependency` örneği.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="c6eeb-114">Kod bağımlılıklarını (örneğin, önceki örnekte) sorunlu ve aşağıdaki nedenlerle kaçınılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="c6eeb-115">Değiştirilecek `MyDependency` sınıfı farklı bir uygulama ile değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="c6eeb-116">Varsa `MyDependency` bağımlılıkları varsa, sınıf tarafından yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="c6eeb-117">Yapılandırmanıza bağlı olarak birden çok sınıf ile büyük bir projedeki `MyDependency`, uygulama yapılandırma kodu dağılmış olur.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="c6eeb-118">Bu uygulama için birim testi zordur.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="c6eeb-119">Uygulamayı bir sahte kullanın veya saplama `MyDependency` sınıfı bu yaklaşımı mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="c6eeb-120">Bağımlılık ekleme aracılığıyla bu sorunları ele alır:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="c6eeb-121">Bağımlılık uygulama soyutlamak için arabirim kullanımı.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-121">The use of an interface to abstract the dependency implementation.</span></span>
* <span data-ttu-id="c6eeb-122">Bir hizmet kapsayıcısı bağımlılığı kaydı.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="c6eeb-123">ASP.NET Core sağlayan bir yerleşik hizmet kapsayıcı [IServiceProvider](/dotnet/api/system.iserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="c6eeb-123">ASP.NET Core provides a built-in service container, [IServiceProvider](/dotnet/api/system.iserviceprovider).</span></span> <span data-ttu-id="c6eeb-124">Hizmetleri uygulamanın kayıtlı `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="c6eeb-125">*Ekleme* hizmetinin, kullanıldığı sınıfının oluşturucusu, içine.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="c6eeb-126">Framework bağımlılığı örneği oluşturma ve artık gerekli değilse bunun atılması sorumluluğu alır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="c6eeb-127">İçinde [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` arabirim uygulamasına hizmet sağlayan bir yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-127">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="c6eeb-128">Bu arabirimin somut bir tür tarafından uygulanan `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="c6eeb-129">`MyDependency` istekler bir [ILogger&lt;TCategoryName&gt; ](/dotnet/api/microsoft.extensions.logging.ilogger-1) kendi oluşturucusuna.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-129">`MyDependency` requests an [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1) in its constructor.</span></span> <span data-ttu-id="c6eeb-130">Bağımlılık ekleme zincirleme bir şekilde kullanmak alışılmadık bir durum değildir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="c6eeb-131">İstenen her bağımlılık sırayla kendi bağımlılıkları ister.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="c6eeb-132">Kapsayıcı graftaki bağımlılıklarını çözen ve tam olarak çözümlenmiş hizmeti döndürür.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="c6eeb-133">Toplu kümesi çözümlenemedi bağımlılıklar, tipik olarak adlandırılır bir *Bağımlılık ağacı*, *bağımlılık grafiği*, veya *Nesne grafiği*.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="c6eeb-134">`IMyDependency` ve `ILogger<TCategoryName>` service kapsayıcısında kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="c6eeb-135">`IMyDependency` kaydedilmiştir `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c6eeb-136">`ILogger<TCategoryName>` senaryomuz için günlük soyutlama altyapısı tarafından kayıtlı bir [framework tarafından sağlanan hizmet](#framework-provided-services) framework tarafından varsayılan olarak kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="c6eeb-137">Kapsayıcı çözümler `ILogger<TCategoryName>` yararlanarak [(Genel) açık türler](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), kaydedilecek ihtiyacını her [(Genel) oluşturulan türü](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="c6eeb-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="c6eeb-138">Örnek uygulamada `IMyDependency` hizmet somut bir türde ile kayıtlı `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="c6eeb-139">Tek bir isteğin ömrü hizmet ömrü kayıt kapsamları.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="c6eeb-140">[Hizmet ömrü](#service-lifetimes) bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="c6eeb-141">Her `services.Add{SERVICE_NAME}` genişletme yöntemi ekler (ve büyük olasılıkla yapılandırır) Hizmetleri.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="c6eeb-142">Örneğin, `services.AddMvc()` Razor sayfaları ve MVC gerekli hizmetleri ekler.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="c6eeb-143">Uygulamalar bu kurala uymayan önerilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="c6eeb-144">Yerleştirin alanında uzantı yöntemlerini [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) hizmet kayıtları grupları kapsüllemek için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="c6eeb-145">Hizmetin Oluşturucusu gerektiriyorsa bir [yerleşik tür](/dotnet/csharp/language-reference/keywords/built-in-types-table), gibi bir `string`, türü kullanarak yerleştirilebilir [yapılandırma](xref:fundamentals/configuration/index) veya [seçenekleri deseni](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="c6eeb-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="c6eeb-146">Hizmet nerede ve özel bir alana atanan bir sınıfın Oluşturucusu aracılığıyla Hizmeti'nin bir örneğini istenir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="c6eeb-147">Alan sınıfı boyunca gerektiği gibi hizmete erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="c6eeb-148">Örnek uygulamada `IMyDependency` örneği istenen ve hizmetin çağırmak için kullanılan `WriteMessage` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a><span data-ttu-id="c6eeb-149">Framework tarafından sağlanan hizmetleri</span><span class="sxs-lookup"><span data-stu-id="c6eeb-149">Framework-provided services</span></span>

<span data-ttu-id="c6eeb-150">`Startup.ConfigureServices` Yöntemi, Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri dahil olmak üzere uygulamanın kullandığı hizmetleri tanımlamak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-150">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="c6eeb-151">Başlangıçta `IServiceCollection` sağlanan `ConfigureServices` tanımlanan aşağıdaki Hizmetleri (bağlı olarak [konak nasıl yapılandırılan](xref:fundamentals/index#host)):</span><span class="sxs-lookup"><span data-stu-id="c6eeb-151">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="c6eeb-152">Hizmet Türü</span><span class="sxs-lookup"><span data-stu-id="c6eeb-152">Service Type</span></span> | <span data-ttu-id="c6eeb-153">Ömür</span><span class="sxs-lookup"><span data-stu-id="c6eeb-153">Lifetime</span></span> |
| ------------ | -------- |
| [<span data-ttu-id="c6eeb-154">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="c6eeb-154">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="c6eeb-155">Geçici</span><span class="sxs-lookup"><span data-stu-id="c6eeb-155">Transient</span></span> |
| [<span data-ttu-id="c6eeb-156">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="c6eeb-156">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="c6eeb-157">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-157">Singleton</span></span> |
| [<span data-ttu-id="c6eeb-158">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="c6eeb-158">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="c6eeb-159">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-159">Singleton</span></span> |
| [<span data-ttu-id="c6eeb-160">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="c6eeb-160">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="c6eeb-161">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-161">Singleton</span></span> |
| [<span data-ttu-id="c6eeb-162">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="c6eeb-162">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="c6eeb-163">Geçici</span><span class="sxs-lookup"><span data-stu-id="c6eeb-163">Transient</span></span> |
| [<span data-ttu-id="c6eeb-164">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="c6eeb-164">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="c6eeb-165">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-165">Singleton</span></span> |
| [<span data-ttu-id="c6eeb-166">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="c6eeb-166">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="c6eeb-167">Geçici</span><span class="sxs-lookup"><span data-stu-id="c6eeb-167">Transient</span></span> |
| [<span data-ttu-id="c6eeb-168">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="c6eeb-168">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="c6eeb-169">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-169">Singleton</span></span> |
| [<span data-ttu-id="c6eeb-170">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="c6eeb-170">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="c6eeb-171">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-171">Singleton</span></span> |
| [<span data-ttu-id="c6eeb-172">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="c6eeb-172">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="c6eeb-173">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-173">Singleton</span></span> |
| [<span data-ttu-id="c6eeb-174">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="c6eeb-174">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="c6eeb-175">Geçici</span><span class="sxs-lookup"><span data-stu-id="c6eeb-175">Transient</span></span> |
| [<span data-ttu-id="c6eeb-176">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="c6eeb-176">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="c6eeb-177">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-177">Singleton</span></span> |
| [<span data-ttu-id="c6eeb-178">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="c6eeb-178">System.Diagnostics.DiagnosticSource</span></span>](/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="c6eeb-179">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-179">Singleton</span></span> |
| [<span data-ttu-id="c6eeb-180">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="c6eeb-180">System.Diagnostics.DiagnosticListener</span></span>](/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="c6eeb-181">singleton</span><span class="sxs-lookup"><span data-stu-id="c6eeb-181">Singleton</span></span> |

<span data-ttu-id="c6eeb-182">Hizmet koleksiyonu genişletme yöntemi, kayıt hizmeti (ve gerekirse, bağımlı hizmetler) kullanılabilir olduğunda, kuralı tek bir kullanmaktır `Add{SERVICE_NAME}` genişletme yöntemi bu hizmet tarafından gerekli tüm hizmetleri kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-182">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="c6eeb-183">Aşağıdaki kod, genişletme yöntemleri kullanarak kapsayıcıya ek hizmetleri ekleme örneğidir [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), ve [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span><span class="sxs-lookup"><span data-stu-id="c6eeb-183">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity), and [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc):</span></span>

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

<span data-ttu-id="c6eeb-184">Daha fazla bilgi için [ServiceCollection sınıfı](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) API belgelerinde.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-184">For more information, see the [ServiceCollection Class](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="c6eeb-185">Hizmet yaşam süresi yok</span><span class="sxs-lookup"><span data-stu-id="c6eeb-185">Service lifetimes</span></span>

<span data-ttu-id="c6eeb-186">Kayıtlı her hizmet için uygun bir yaşam süresi'ni seçin.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-186">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="c6eeb-187">ASP.NET Core Hizmetleri ile aşağıdaki ömürleri yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-187">ASP.NET Core services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="c6eeb-188">**Geçici**</span><span class="sxs-lookup"><span data-stu-id="c6eeb-188">**Transient**</span></span>

<span data-ttu-id="c6eeb-189">Geçici ömrü Hizmetleri hizmet kapsayıcısından istenen her zaman oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-189">Transient lifetime services are created each time they're requested from the service container.</span></span> <span data-ttu-id="c6eeb-190">Bu yaşam süresi, basit, durum bilgisi olmayan hizmetler için en iyi çalışır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-190">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="c6eeb-191">**Kapsamlı**</span><span class="sxs-lookup"><span data-stu-id="c6eeb-191">**Scoped**</span></span>

<span data-ttu-id="c6eeb-192">Kapsamlı ömrü Hizmetleri (bağlantı) istemci istek bir kez oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-192">Scoped lifetime services are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="c6eeb-193">Kapsamlı bir hizmeti bir ara yazılımında kullanılırken, hizmette oturum ekleme `Invoke` veya `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-193">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="c6eeb-194">Bir tekliyi gibi davranmaya hizmet zorladığından Oluşturucu ekleme ekleme yok.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-194">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="c6eeb-195">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-195">For more information, see <xref:fundamentals/middleware/index>.</span></span>

<span data-ttu-id="c6eeb-196">**singleton**</span><span class="sxs-lookup"><span data-stu-id="c6eeb-196">**Singleton**</span></span>

<span data-ttu-id="c6eeb-197">Singleton ömrü Hizmetleri istenen ilk kez oluşturulur (veya `ConfigureServices` çalıştırılır ve örneği hizmet kaydı ile belirtilir).</span><span class="sxs-lookup"><span data-stu-id="c6eeb-197">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="c6eeb-198">Sonraki her istek, aynı örneğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-198">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="c6eeb-199">Uygulama tekil davranışı gerektiriyorsa, hizmetin ömrünü yönetmek hizmet kapsayıcı izin vererek önerilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-199">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="c6eeb-200">Yoksa, tekil tasarım desenini uygulama ve sınıfında nesnenin ömrünü yönetmek üzere kullanıcı kodunun sağlayın.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-200">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="c6eeb-201">Tek bir kapsamlı hizmet çözümlenecek tehlikelidir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-201">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="c6eeb-202">Bu, sonraki istekleri işleme sırasında hatalı duruma sahip hizmetinin neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-202">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="c6eeb-203">Oluşturucu yerleştirme davranışı</span><span class="sxs-lookup"><span data-stu-id="c6eeb-203">Constructor injection behavior</span></span>

<span data-ttu-id="c6eeb-204">Hizmetleri iki mekanizma tarafından çözülebilir:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-204">Services can be resolved by two mechanisms:</span></span>

* `IServiceProvider`
* <span data-ttu-id="c6eeb-205">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; nesne oluşturma bağımlılık ekleme kapsayıcısına hizmet kaydı olmadan izin verir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-205">[ActivatorUtilities](/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities) &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="c6eeb-206">`ActivatorUtilities` Etiket Yardımcıları, MVC denetleyicileri ve model bağlayıcılar gibi kullanıcıya yönelik soyutlama ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-206">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="c6eeb-207">Oluşturucular, bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bağımsız değişken varsayılan değerleri atamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-207">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="c6eeb-208">Ne zaman Hizmetleri çözülmüş tarafından `IServiceProvider` veya `ActivatorUtilities`, oluşturucu ekleme gerektiren bir *genel* Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-208">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="c6eeb-209">Ne zaman Hizmetleri çözülmüş tarafından `ActivatorUtilities`, oluşturucu ekleme gerektirir, yalnızca bir geçerli oluşturucusu yok.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-209">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="c6eeb-210">Oluşturucu aşırı yüklemeleri tarafından desteklenir, ancak yalnızca bir aşırı yükleme bağımsız değişkenleri tüm bağımlılık ekleme tarafından yerine getirilmesi bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-210">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="c6eeb-211">Entity Framework bağlamları</span><span class="sxs-lookup"><span data-stu-id="c6eeb-211">Entity Framework contexts</span></span>

<span data-ttu-id="c6eeb-212">Entity Framework bağlamları, hizmeti kullanarak kapsayıcı genellikle eklenir [kapsamlı ömrü](#service-lifetimes) web uygulama veritabanı işlemlerini normalde istemci isteğini kapsamındaki olduğundan.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-212">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="c6eeb-213">Bir yaşam süresi tarafından belirtilmezse varsayılan yaşam süresi kapsamlıdır bir <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> veritabanı bağlamı kaydederken aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-213">The default lifetime is scoped if a lifetime isn't specified by an <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> overload when registering the database context.</span></span> <span data-ttu-id="c6eeb-214">Belirli bir yaşam süresi Hizmetleri, hizmet daha kısa bir yaşam süresi ile bir veritabanı bağlamı kullanmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-214">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="c6eeb-215">Yaşam süresi ve kayıt seçenekleri</span><span class="sxs-lookup"><span data-stu-id="c6eeb-215">Lifetime and registration options</span></span>

<span data-ttu-id="c6eeb-216">Yaşam süresi ve kayıt seçenekleri arasındaki farkı göstermek için benzersiz bir tanımlayıcıya sahip bir işlem olarak görevleri temsil eden aşağıdaki arabirimlerinden göz önünde bulundurun. `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-216">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="c6eeb-217">Kapsayıcı, bir işlem hizmeti kullanım ömrü için aşağıdaki arabirimlerinden nasıl yapılandırıldığına bağlı olarak, aynı veya farklı bir örneğine bir sınıf tarafından istendiğinde hizmetinin sağlar:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-217">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="c6eeb-218">Arabirimler uygulanan `Operation` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-218">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="c6eeb-219">`Operation` Oluşturucusu bir sağlanan değilse bir GUID oluşturur:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-219">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="c6eeb-220">Bir `OperationService` kayıtlı, bağlı her diğer `Operation` türleri.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-220">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="c6eeb-221">Zaman `OperationService` istenen bağımlılık ekleme aldığı her hizmetin yeni bir örneğini veya bağımlı hizmetin lifetime öğesine göre var olan bir örneği.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-221">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="c6eeb-222">Geçici Hizmetleri kapsayıcısından istendiğinde oluşturulduğunda `OperationId` , `IOperationTransient` hizmetidir farklı `OperationId` , `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-222">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="c6eeb-223">`OperationService` Yeni bir örneğini alır `IOperationTransient` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-223">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="c6eeb-224">Yeni örnek farklı bir verir `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-224">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="c6eeb-225">Her istemci isteği, kapsamı belirlenmiş hizmetler oluştururken `OperationId` , `IOperationScoped` hizmeti aynı olan `OperationService` içinde bir istemci isteği.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-225">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="c6eeb-226">İstemci istekleri arasında her iki hizmet de farklı bir paylaşım `OperationId` değeri.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-226">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="c6eeb-227">Singleton ve tek örnekli Hizmetleri oluşturulduktan sonra ve tüm istemci isteklerini ve tüm hizmetlerde kullanılan `OperationId` tüm hizmet istekler genelinde sabittir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-227">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="c6eeb-228">İçinde `Startup.ConfigureServices`, her tür kapsayıcı adlandırılmış ömrü göre eklenir:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-228">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="c6eeb-229">`IOperationSingletonInstance` Hizmetinin belirli bir örneği, bilinen bir kimliği ile kullandığı `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-229">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="c6eeb-230">Bu tür (GUID'sine sıfır olur) kullanımda olmadığında işaretlenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-230">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="c6eeb-231">Örnek uygulama, nesne kullanım ömrü içindeki ve arasındaki tek tek istekleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-231">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="c6eeb-232">Örnek uygulamanın `IndexModel` her tür istekleri `IOperation` türü ve `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-232">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="c6eeb-233">Sayfa sonra tüm sayfa modeli sınıfın ve hizmetin görüntüler `OperationId` özelliği aracılığıyla değerleri:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-233">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="c6eeb-234">İki aşağıdaki çıktıda gösterildiği iki isteği sonuçları:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-234">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="c6eeb-235">**İlk istek:**</span><span class="sxs-lookup"><span data-stu-id="c6eeb-235">**First request:**</span></span>

<span data-ttu-id="c6eeb-236">Denetleyici işlemler:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-236">Controller operations:</span></span>

<span data-ttu-id="c6eeb-237">Transient: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="c6eeb-237">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="c6eeb-238">Kapsamı: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="c6eeb-238">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="c6eeb-239">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="c6eeb-239">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="c6eeb-240">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="c6eeb-240">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="c6eeb-241">`OperationService` İşlemler:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-241">`OperationService` operations:</span></span>

<span data-ttu-id="c6eeb-242">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="c6eeb-242">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="c6eeb-243">Kapsamı: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="c6eeb-243">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="c6eeb-244">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="c6eeb-244">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="c6eeb-245">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="c6eeb-245">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="c6eeb-246">**İkinci isteği:**</span><span class="sxs-lookup"><span data-stu-id="c6eeb-246">**Second request:**</span></span>

<span data-ttu-id="c6eeb-247">Denetleyici işlemler:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-247">Controller operations:</span></span>

<span data-ttu-id="c6eeb-248">Geçici: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="c6eeb-248">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="c6eeb-249">Kapsamı: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="c6eeb-249">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="c6eeb-250">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="c6eeb-250">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="c6eeb-251">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="c6eeb-251">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="c6eeb-252">`OperationService` İşlemler:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-252">`OperationService` operations:</span></span>

<span data-ttu-id="c6eeb-253">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="c6eeb-253">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="c6eeb-254">Kapsamı: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="c6eeb-254">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="c6eeb-255">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="c6eeb-255">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="c6eeb-256">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="c6eeb-256">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="c6eeb-257">Hangi gözlemleyin `OperationId` değerleri, bir istek içinde ve istekler arasında değişir:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-257">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="c6eeb-258">*Geçici* nesneleri farklı her zaman.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-258">*Transient* objects are always different.</span></span> <span data-ttu-id="c6eeb-259">Geçici `OperationId` değeri için birinci ve ikinci istemci istekleri için hem de farklı `OperationService` işlemleri ve istemci istekleri arasında.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-259">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="c6eeb-260">Yeni bir örneği, her bir hizmet isteğini ve istemci isteği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-260">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="c6eeb-261">*Kapsamlı* nesneleri, ancak farklı bir istemci isteği içinde aynı istemci isteklerinde.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-261">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="c6eeb-262">*Singleton* nesneleri, her nesne ve bağımsız olarak her istek için aynı bir `Operation` örneği sağlanır `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-262">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="c6eeb-263">Ana arama hizmetleri</span><span class="sxs-lookup"><span data-stu-id="c6eeb-263">Call services from main</span></span>

<span data-ttu-id="c6eeb-264">Oluşturma bir [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) ile [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) uygulamanın kapsamı içinde kapsamlı bir hizmet çözümlenecek.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-264">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="c6eeb-265">Bu yaklaşım, başlangıçta başlatma görevleri çalıştırmak için kapsamlı bir hizmete erişmek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-265">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="c6eeb-266">Aşağıdaki örnek için bir bağlam elde etme gösterir `MyScopedService` içinde `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-266">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="c6eeb-267">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="c6eeb-267">Scope validation</span></span>

<span data-ttu-id="c6eeb-268">Uygulama geliştirme ortamında çalışırken, varsayılan hizmet sağlayıcısını doğrulamak için denetimleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-268">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="c6eeb-269">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-269">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="c6eeb-270">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değildir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-270">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="c6eeb-271">Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-271">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="c6eeb-272">Kök hizmet sağlayıcısının bir ömür zaman sağlayıcısı uygulamayla başlar ve uygulama kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-272">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="c6eeb-273">Kapsamlı Hizmetleri, onları oluşturan kapsayıcı tarafından elden çıkarılmasını.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-273">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="c6eeb-274">Kapsamlı bir hizmet içinde kök kapsayıcı oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından bırakılmadan olduğundan hizmet ömrü tekliye etkili bir şekilde yükseltilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-274">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="c6eeb-275">Hizmet kapsamları doğrulama yakalar bu gibi durumlarda, `BuildServiceProvider` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-275">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="c6eeb-276">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-276">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="c6eeb-277">Hizmet isteği</span><span class="sxs-lookup"><span data-stu-id="c6eeb-277">Request Services</span></span>

<span data-ttu-id="c6eeb-278">Bir ASP.NET Core içinde kullanılabilen hizmetler request `HttpContext` aracılığıyla kullanıma [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-278">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](/dotnet/api/microsoft.aspnetcore.http.httpcontext.requestservices) collection.</span></span>

<span data-ttu-id="c6eeb-279">İstek Hizmetleri yapılandırılmış ve uygulamanın bir parçası istenen Hizmetleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-279">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="c6eeb-280">Nesneleri bağımlılıkları belirttiğinizde, bu bulunan türleri tarafından karşılandığından `RequestServices`değil `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-280">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="c6eeb-281">Genel olarak, uygulamayı doğrudan bu özellikleri kullanmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-281">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="c6eeb-282">Bunun yerine, sınıfları sınıf oluşturucuları gerektirir ve framework izin türlerini bağımlılıkları ekleme isteği.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-282">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="c6eeb-283">Bu, test etmek daha kolay olan sınıfları verir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-283">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="c6eeb-284">Erişim için oluşturucu parametresi olarak bağımlılıkları isteyen tercih `RequestServices` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-284">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="c6eeb-285">Tasarım Hizmetleri için bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="c6eeb-285">Design services for dependency injection</span></span>

<span data-ttu-id="c6eeb-286">İçin en iyi uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-286">Best practices are to:</span></span>

* <span data-ttu-id="c6eeb-287">Bağımlılık ekleme bağımlılıklarını almak için kullanmak üzere hizmetlerin tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-287">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="c6eeb-288">Durum bilgisi olan ve statik yöntem çağrıları kaçının.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-288">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="c6eeb-289">Bağımlı hizmetleri sınıfları doğrudan örneğinin kaçının.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-289">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="c6eeb-290">Doğrudan, kodu belirli bir uygulamaya couples.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-290">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="c6eeb-291">Uygulama sınıfları küçük, katsayıları iyi belirlenmiş ve test edilmiş kolayca yapın.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-291">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="c6eeb-292">Bir sınıf çok fazla eklenen bağımlılıklara sahip gibi görünüyor. Bu genellikle bir oturum sınıfı için çok fazla sorumluluklara sahiptir ve ihlal varsa, [tek sorumluluk İlkesi'ni (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="c6eeb-292">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="c6eeb-293">Sınıfının yeni bir sınıf bazı sorumlulukları taşıyarak yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-293">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="c6eeb-294">Razor sayfaları sayfa modeli sınıfları ve MVC denetleyici sınıflarına kullanıcı Arabirimi konuları üzerinde durmalısınız aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-294">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="c6eeb-295">İş kuralları ve veri erişim uygulama ayrıntılarını saklanır bunları uygun sınıflardaki [konuları ayrı](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="c6eeb-295">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="c6eeb-296">Bırakma Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="c6eeb-296">Disposal of services</span></span>

<span data-ttu-id="c6eeb-297">Kapsayıcı çağrıları `Dispose` için `IDisposable` türleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-297">The container calls `Dispose` for the `IDisposable` types it creates.</span></span> <span data-ttu-id="c6eeb-298">Bir örneği, kullanıcı kodu tarafından kapsayıcıya eklenirse, otomatik olarak elden değil.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-298">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="c6eeb-299">Varsayılan hizmet kapsayıcısını değiştirme</span><span class="sxs-lookup"><span data-stu-id="c6eeb-299">Default service container replacement</span></span>

<span data-ttu-id="c6eeb-300">Yerleşik hizmet kapsayıcı framework ve çoğu tüketici uygulamalarına gereksinimlerini karşılamak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-300">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="c6eeb-301">Bunu desteklemiyor belirli bir özellik gerekmedikçe yerleşik kapsayıcı kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-301">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="c6eeb-302">Yerleşik kapsayıcısında bulunamadı 3 taraf kapsayıcıları destekleyen özelliklerden bazıları:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-302">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="c6eeb-303">Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="c6eeb-303">Property injection</span></span>
* <span data-ttu-id="c6eeb-304">Adına göre ekleme</span><span class="sxs-lookup"><span data-stu-id="c6eeb-304">Injection based on name</span></span>
* <span data-ttu-id="c6eeb-305">Alt kapsayıcılar</span><span class="sxs-lookup"><span data-stu-id="c6eeb-305">Child containers</span></span>
* <span data-ttu-id="c6eeb-306">Özel ömür Yönetimi</span><span class="sxs-lookup"><span data-stu-id="c6eeb-306">Custom lifetime management</span></span>
* <span data-ttu-id="c6eeb-307">`Func<T>` Yavaş başlatma desteği</span><span class="sxs-lookup"><span data-stu-id="c6eeb-307">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="c6eeb-308">Bkz: [bağımlılık ekleme Benioku.MD dosyası](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) bazı bağdaştırıcıları destekleyen kapsayıcıların listesi.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-308">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="c6eeb-309">Aşağıdaki örnek yerleşik kapsayıcıyla değiştirir [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="c6eeb-309">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="c6eeb-310">Uygun bir kapsayıcı paketleri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-310">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="c6eeb-311">Autofac</span><span class="sxs-lookup"><span data-stu-id="c6eeb-311">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="c6eeb-312">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="c6eeb-312">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="c6eeb-313">Kapsayıcıda yapılandırma `Startup.ConfigureServices` ve dönüş bir `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-313">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

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

    <span data-ttu-id="c6eeb-314">3 bir taraf kapsayıcı kullanılacak `Startup.ConfigureServices` döndürmelidir `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-314">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="c6eeb-315">İçinde Autofac yapılandırma `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="c6eeb-315">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="c6eeb-316">Çalışma zamanında Autofac, türleri çözümlemek ve bağımlılıkları ekleme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-316">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="c6eeb-317">ASP.NET Core ile Autofac kullanma hakkında daha fazla bilgi edinmek için [Autofac belgeleri](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="c6eeb-317">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="c6eeb-318">İş parçacığı güvenliği</span><span class="sxs-lookup"><span data-stu-id="c6eeb-318">Thread safety</span></span>

<span data-ttu-id="c6eeb-319">İş parçacığı açısından güvenli tekil Hizmetleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-319">Create thread-safe singleton services.</span></span> <span data-ttu-id="c6eeb-320">Tek bir hizmet üzerinde geçici bir hizmet bağımlılığı varsa, geçici hizmet tekli tarafından nasıl kullanıldığını bağlı olarak iş parçacığı güvenliği de gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-320">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="c6eeb-321">İkinci bağımsız değişkeni gibi tek hizmet Üreteç yöntemi [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider, TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), değil iş parçacığı açısından güvenli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-321">The factory method of single service, such as the second argument to [AddSingleton&lt;TService&gt;(IServiceCollection, Func&lt;IServiceProvider,TService&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton#Microsoft_Extensions_DependencyInjection_ServiceCollectionServiceExtensions_AddSingleton__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Func_System_IServiceProvider___0__), doesn't need to be thread-safe.</span></span> <span data-ttu-id="c6eeb-322">Gibi bir tür (`static`) oluşturucusu, onu garanti tek bir iş parçacığı tarafından bir kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-322">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="c6eeb-323">Öneriler</span><span class="sxs-lookup"><span data-stu-id="c6eeb-323">Recommendations</span></span>

* <span data-ttu-id="c6eeb-324">`async/await` ve `Task` tabanlı hizmet çözümlemesi desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-324">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="c6eeb-325">C#, zaman uyumsuz oluşturucuları desteklemez, dolayısıyla önerilen Düzen zaman uyumsuz yöntemleri zaman uyumlu olarak hizmet çözdükten sonra kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-325">C# does not support asynchronous constructors, therefore the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="c6eeb-326">Verileri ve Yapılandırma hizmeti kapsayıcısında doğrudan depolama kaçının.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-326">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="c6eeb-327">Örneğin, bir kullanıcının alışveriş sepeti genellikle hizmet kapsayıcıya eklenen olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-327">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="c6eeb-328">Yapılandırma kullanması gereken [seçenekleri deseni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="c6eeb-328">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="c6eeb-329">Benzer şekilde, yalnızca başka bir nesnenin erişmesine izin vermek için mevcut "veri sahibi" nesneleri kaçının.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-329">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="c6eeb-330">İstek DI aracılığıyla gerçek öğesi daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-330">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="c6eeb-331">Statik hizmetlere erişimi önlemek (örneğin, statik olarak yazmaya [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) kullanan başka bir yerde için).</span><span class="sxs-lookup"><span data-stu-id="c6eeb-331">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices) for use elsewhere).</span></span>

* <span data-ttu-id="c6eeb-332">Kullanmaktan kaçının *hizmet bulucu deseni*.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-332">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="c6eeb-333">Örneğin, çağırma yoksa <xref:System.IServiceProvider.GetService*> DI yerine kullandığınızda, bir hizmet örneği elde edilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-333">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead.</span></span> <span data-ttu-id="c6eeb-334">Çalışma zamanında bağımlılıklarını çözen bir Fabrika önlemek için başka bir hizmet bulucu değişim çalıştırıyorsunuzdur.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-334">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="c6eeb-335">Bu yöntemler karışımı [tersine çevirme denetim](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) stratejiler.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-335">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="c6eeb-336">Statik erişimi önlemek `HttpContext` (örneğin, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span><span class="sxs-lookup"><span data-stu-id="c6eeb-336">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor.httpcontext)).</span></span>

<span data-ttu-id="c6eeb-337">Öneriler tüm kümesi gibi bir öneri yok sayılıyor gerekli olduğu durumlarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-337">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="c6eeb-338">Özel durumlar nadir&mdash;çoğunlukla framework içinde özel durumlar.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-338">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="c6eeb-339">DI olduğu bir *alternatif* statik/genel nesne erişim desenleri.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-339">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="c6eeb-340">Statik nesne erişimi ile karıştırmak istiyorsanız DI avantajlarından mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="c6eeb-340">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6eeb-341">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="c6eeb-341">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="c6eeb-342">Bağımlılık ekleme (MSDN) ile ASP.NET Core kod yazma</span><span class="sxs-lookup"><span data-stu-id="c6eeb-342">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="c6eeb-343">Özel bağımlılıklar İlkesi</span><span class="sxs-lookup"><span data-stu-id="c6eeb-343">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="c6eeb-344">Tersine çevirme denetimi kapsayıcıları ve bağımlılık ekleme desenini (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="c6eeb-344">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="c6eeb-345">Bir hizmet birden fazla arabirimde ASP.NET Core DI ile kaydetme</span><span class="sxs-lookup"><span data-stu-id="c6eeb-345">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
