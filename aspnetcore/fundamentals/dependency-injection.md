---
title: ASP.NET Core bağımlılık ekleme
author: guardrex
description: ASP.NET Core bağımlılık ekleme ve nasıl kullanılacağı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: a984bb766e6876db4f8ed4c850a1984ba87d627d
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022283"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="272b0-103">ASP.NET Core bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="272b0-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="272b0-104">[Steve Smith](https://ardalis.com/), [Scott Ade](https://scottaddie.com)ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="272b0-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="272b0-105">ASP.NET Core, sınıflar ve bunların bağımlılıkları arasında [denetimin INVERSION (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) elde etmek için bir teknik olan bağımlılık ekleme (dı) yazılım tasarım modelini destekler.</span><span class="sxs-lookup"><span data-stu-id="272b0-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="272b0-106">MVC denetleyicileri içindeki bağımlılık eklenmesine özgü daha fazla bilgi için bkz <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="272b0-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="272b0-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="272b0-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="272b0-108">Bağımlılık eklenmesine genel bakış</span><span class="sxs-lookup"><span data-stu-id="272b0-108">Overview of dependency injection</span></span>

<span data-ttu-id="272b0-109">*Bağımlılık* , başka bir nesnenin gerektirdiği herhangi bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="272b0-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="272b0-110">Aşağıdaki `MyDependency` sınıfı, bir uygulamadaki diğer `WriteMessage` sınıfların bağlı olduğu bir yöntemle inceleyin:</span><span class="sxs-lookup"><span data-stu-id="272b0-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="272b0-111">Yöntemi bir sınıf için `MyDependency` kullanılabilir hale getirmek için sınıfının bir örneği oluşturulabilir. `WriteMessage`</span><span class="sxs-lookup"><span data-stu-id="272b0-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="272b0-112">Sınıfı, `IndexModel` sınıfının bir bağımlılığı olur: `MyDependency`</span><span class="sxs-lookup"><span data-stu-id="272b0-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="272b0-113">Sınıf oluşturur ve `MyDependency` örneğe doğrudan bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="272b0-114">Kod bağımlılıkları (önceki örnekte olduğu gibi) sorunlu olur ve aşağıdaki nedenlerden dolayı kaçınılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="272b0-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="272b0-115">Farklı bir `MyDependency` uygulamayla değiştirmek için, sınıfın değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="272b0-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="272b0-116">`MyDependency` Bağımlılıkları varsa, sınıfı tarafından yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="272b0-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="272b0-117">Uygulamasına bağlı olarak `MyDependency`, birden çok sınıfı olan büyük bir projede yapılandırma kodu uygulama genelinde dağılmış hale gelir.</span><span class="sxs-lookup"><span data-stu-id="272b0-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="272b0-118">Bu uygulamanın birim testi zordur.</span><span class="sxs-lookup"><span data-stu-id="272b0-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="272b0-119">Uygulamanın bu yaklaşım ile mümkün olmayan bir sahte `MyDependency` veya saplama sınıfı kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="272b0-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="272b0-120">Bağımlılık ekleme bu sorunları şu şekilde giderir:</span><span class="sxs-lookup"><span data-stu-id="272b0-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="272b0-121">Bağımlılık uygulamasını soyutlamak için bir arabirim veya temel sınıf kullanımı.</span><span class="sxs-lookup"><span data-stu-id="272b0-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="272b0-122">Bir hizmet kapsayıcısına bağımlılığın kaydı.</span><span class="sxs-lookup"><span data-stu-id="272b0-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="272b0-123">ASP.NET Core, <xref:System.IServiceProvider>yerleşik bir hizmet kapsayıcısı sağlar.</span><span class="sxs-lookup"><span data-stu-id="272b0-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="272b0-124">Hizmetler, uygulamanın `Startup.ConfigureServices` yöntemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="272b0-125">Hizmetin kullanıldığı sınıf oluşturucusuna *ekleme* .</span><span class="sxs-lookup"><span data-stu-id="272b0-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="272b0-126">Çerçeve, bağımlılığın bir örneğini oluşturma ve artık gerekli olmadığında bu uygulamayı atma sorumluluğunu alır.</span><span class="sxs-lookup"><span data-stu-id="272b0-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="272b0-127">[Örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` arabirim, hizmetin uygulamaya sağladığı bir yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="272b0-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="272b0-128">Bu arabirim somut bir tür tarafından uygulanır, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="272b0-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="272b0-129">`MyDependency`kendi oluşturucusunda <xref:Microsoft.Extensions.Logging.ILogger`1> bir ister.</span><span class="sxs-lookup"><span data-stu-id="272b0-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="272b0-130">Bağımlılık ekleme işlemini zincirleme bir biçimde kullanmak olağan dışı değildir.</span><span class="sxs-lookup"><span data-stu-id="272b0-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="272b0-131">Her istenen bağımlılık, kendi bağımlılıklarını ister.</span><span class="sxs-lookup"><span data-stu-id="272b0-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="272b0-132">Kapsayıcı grafikteki bağımlılıkları çözer ve tamamen çözümlenen hizmeti döndürür.</span><span class="sxs-lookup"><span data-stu-id="272b0-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="272b0-133">Çözümlenmesi gereken, genellikle *bağımlılık ağacı*, *bağımlılık grafiği*veya *nesne grafiği*olarak adlandırılan toplu bağımlılıklar kümesi.</span><span class="sxs-lookup"><span data-stu-id="272b0-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="272b0-134">`IMyDependency`ve `ILogger<TCategoryName>` hizmet kapsayıcısında kayıtlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="272b0-135">`IMyDependency``Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="272b0-136">`ILogger<TCategoryName>`günlüğe kaydetme soyutlamaları altyapısı tarafından kaydedilir. bu nedenle, Framework tarafından varsayılan olarak kaydedilen [Framework tarafından sağlanmış bir hizmettir](#framework-provided-services) .</span><span class="sxs-lookup"><span data-stu-id="272b0-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="272b0-137">Kapsayıcı `ILogger<TCategoryName>` [(genel) açık türlerden](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types)yararlanarak çözümlenir, her [(genel) oluşturulan türü](/dotnet/csharp/language-reference/language-specification/types#constructed-types)kaydetme ihtiyacını ortadan kaldırır:</span><span class="sxs-lookup"><span data-stu-id="272b0-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="272b0-138">Örnek uygulamada, `IMyDependency` hizmet somut tür `MyDependency`ile kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="272b0-139">Kayıt, hizmet ömrünü tek bir isteğin kullanım ömrüne göre kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="272b0-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="272b0-140">[Hizmet yaşam süreleri](#service-lifetimes) bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="272b0-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="272b0-141">Her `services.Add{SERVICE_NAME}` uzantı yöntemi Hizmetleri ekler (ve potansiyel olarak yapılandırır).</span><span class="sxs-lookup"><span data-stu-id="272b0-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="272b0-142">Örneğin, `services.AddMvc()` Razor Pages ve MVC 'nin gerektirdiği Hizmetleri ekler.</span><span class="sxs-lookup"><span data-stu-id="272b0-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="272b0-143">Uygulamaların bu kuralı izlemesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="272b0-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="272b0-144">Hizmet kaydı gruplarını kapsüllemek için uzantı yöntemlerini [Microsoft. Extensions. Dependencyınjection](/dotnet/api/microsoft.extensions.dependencyinjection) ad alanına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="272b0-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="272b0-145">Hizmetin Oluşturucusu, gibi [yerleşik bir tür](/dotnet/csharp/language-reference/keywords/built-in-types-table) `string`gerektiriyorsa, tür [yapılandırma](xref:fundamentals/configuration/index) veya [Seçenekler düzeniyle](xref:fundamentals/configuration/options)eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="272b0-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="272b0-146">Hizmetin bir örneği, hizmetin kullanıldığı ve özel bir alana atandığı bir sınıfın Oluşturucusu aracılığıyla istenir.</span><span class="sxs-lookup"><span data-stu-id="272b0-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="272b0-147">Alanı, sınıfına gereken şekilde hizmete erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="272b0-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="272b0-148">Örnek uygulamada, `IMyDependency` örnek istenir ve `WriteMessage` hizmetin yöntemini çağırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="272b0-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a><span data-ttu-id="272b0-149">Framework tarafından sunulan hizmetler</span><span class="sxs-lookup"><span data-stu-id="272b0-149">Framework-provided services</span></span>

<span data-ttu-id="272b0-150">`Startup.ConfigureServices` Yöntemi, Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri de dahil olmak üzere, uygulamanın kullandığı hizmetleri tanımlamaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="272b0-150">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="272b0-151">Başlangıçta, `IServiceCollection` için `ConfigureServices` belirtilen hizmetler tanımlı ( [konağın nasıl yapılandırıldığına](xref:fundamentals/index#host)bağlı olarak):</span><span class="sxs-lookup"><span data-stu-id="272b0-151">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="272b0-152">Hizmet Türü</span><span class="sxs-lookup"><span data-stu-id="272b0-152">Service Type</span></span> | <span data-ttu-id="272b0-153">Ömür</span><span class="sxs-lookup"><span data-stu-id="272b0-153">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="272b0-154">Geçici</span><span class="sxs-lookup"><span data-stu-id="272b0-154">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="272b0-155">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-155">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="272b0-156">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-156">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="272b0-157">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-157">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="272b0-158">Geçici</span><span class="sxs-lookup"><span data-stu-id="272b0-158">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="272b0-159">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-159">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="272b0-160">Geçici</span><span class="sxs-lookup"><span data-stu-id="272b0-160">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="272b0-161">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-161">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="272b0-162">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-162">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="272b0-163">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-163">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="272b0-164">Geçici</span><span class="sxs-lookup"><span data-stu-id="272b0-164">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="272b0-165">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-165">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="272b0-166">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-166">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="272b0-167">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-167">Singleton</span></span> |

<span data-ttu-id="272b0-168">Bir hizmet koleksiyonu genişletme yöntemi (ve gerekirse bağımlı hizmetleri) kaydetmek için kullanılabilir olduğunda, bu hizmet için gereken tüm hizmetleri kaydetmek üzere kural tek `Add{SERVICE_NAME}` bir genişletme yöntemi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="272b0-168">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="272b0-169">Aşağıdaki kod, [adddbcontext\<tcontext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>ve <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>uzantı yöntemlerini kullanarak kapsayıcıya ek hizmetler ekleme örneğidir.</span><span class="sxs-lookup"><span data-stu-id="272b0-169">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, and <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span></span>

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

<span data-ttu-id="272b0-170">Daha fazla bilgi için API belgelerindeki <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> sınıfına bakın.</span><span class="sxs-lookup"><span data-stu-id="272b0-170">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="272b0-171">Hizmet yaşam süreleri</span><span class="sxs-lookup"><span data-stu-id="272b0-171">Service lifetimes</span></span>

<span data-ttu-id="272b0-172">Kayıtlı her hizmet için uygun bir yaşam süresi seçin.</span><span class="sxs-lookup"><span data-stu-id="272b0-172">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="272b0-173">ASP.NET Core hizmetler aşağıdaki yaşam süreleri ile yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="272b0-173">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="272b0-174">Geçici</span><span class="sxs-lookup"><span data-stu-id="272b0-174">Transient</span></span>

<span data-ttu-id="272b0-175">Geçici ömür Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>), hizmet kapsayıcısından her istenilişinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="272b0-175">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="272b0-176">Bu ömür, hafif ve durumsuz hizmetler için en iyi şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="272b0-176">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="272b0-177">Yayıl</span><span class="sxs-lookup"><span data-stu-id="272b0-177">Scoped</span></span>

<span data-ttu-id="272b0-178">Kapsamlı ömür Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>), istemci isteği başına bir kez oluşturulur (bağlantı).</span><span class="sxs-lookup"><span data-stu-id="272b0-178">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="272b0-179">Bir ara yazılım içinde kapsamlı bir hizmet kullanırken, hizmeti `Invoke` veya `InvokeAsync` yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="272b0-179">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="272b0-180">Oluşturucu ekleme yoluyla ekleme, hizmeti tek bir gibi davranmaya zoryor.</span><span class="sxs-lookup"><span data-stu-id="272b0-180">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="272b0-181">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="272b0-181">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="272b0-182">Adet</span><span class="sxs-lookup"><span data-stu-id="272b0-182">Singleton</span></span>

<span data-ttu-id="272b0-183">Tek yaşam süresi Hizmetleri<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>() her istendiğinde oluşturulur ( `Startup.ConfigureServices` veya çalıştırıldığında ve hizmet kaydıyla bir örnek belirtildiğinde).</span><span class="sxs-lookup"><span data-stu-id="272b0-183">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="272b0-184">Her sonraki istek aynı örneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="272b0-184">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="272b0-185">Uygulama tek davranış gerektiriyorsa, hizmet kapsayıcısının hizmetin ömrünü yönetmesine izin verilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-185">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="272b0-186">Tekil tasarım modelini uygulamayın ve nesnenin sınıfındaki ömrünü yönetmek için Kullanıcı kodu sağlayın.</span><span class="sxs-lookup"><span data-stu-id="272b0-186">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="272b0-187">Kapsamlı bir hizmetin tek bir bilgisayardan çözümlenmesi tehlikelidir.</span><span class="sxs-lookup"><span data-stu-id="272b0-187">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="272b0-188">Bu, sonraki istekleri işlerken hizmetin yanlış duruma gelmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-188">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="272b0-189">Hizmet kayıt yöntemleri</span><span class="sxs-lookup"><span data-stu-id="272b0-189">Service registration methods</span></span>

<span data-ttu-id="272b0-190">Her hizmet kayıt uzantısı yöntemi, belirli senaryolarda yararlı olan aşırı yüklemeler sunar.</span><span class="sxs-lookup"><span data-stu-id="272b0-190">Each service registration extension method offers overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="272b0-191">Yöntem</span><span class="sxs-lookup"><span data-stu-id="272b0-191">Method</span></span> | <span data-ttu-id="272b0-192">Otomatik</span><span class="sxs-lookup"><span data-stu-id="272b0-192">Automatic</span></span><br><span data-ttu-id="272b0-193">nesne</span><span class="sxs-lookup"><span data-stu-id="272b0-193">object</span></span><br><span data-ttu-id="272b0-194">elden</span><span class="sxs-lookup"><span data-stu-id="272b0-194">disposal</span></span> | <span data-ttu-id="272b0-195">Birden Çok</span><span class="sxs-lookup"><span data-stu-id="272b0-195">Multiple</span></span><br><span data-ttu-id="272b0-196">uygulamalar</span><span class="sxs-lookup"><span data-stu-id="272b0-196">implementations</span></span> | <span data-ttu-id="272b0-197">Geçiş bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="272b0-197">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="272b0-198">Örnek:</span><span class="sxs-lookup"><span data-stu-id="272b0-198">Example:</span></span><br>`services.AddScoped<IMyDep, MyDep>();` | <span data-ttu-id="272b0-199">Evet</span><span class="sxs-lookup"><span data-stu-id="272b0-199">Yes</span></span> | <span data-ttu-id="272b0-200">Evet</span><span class="sxs-lookup"><span data-stu-id="272b0-200">Yes</span></span> | <span data-ttu-id="272b0-201">Hayır</span><span class="sxs-lookup"><span data-stu-id="272b0-201">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="272b0-202">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="272b0-202">Examples:</span></span><br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="272b0-203">Evet</span><span class="sxs-lookup"><span data-stu-id="272b0-203">Yes</span></span> | <span data-ttu-id="272b0-204">Evet</span><span class="sxs-lookup"><span data-stu-id="272b0-204">Yes</span></span> | <span data-ttu-id="272b0-205">Evet</span><span class="sxs-lookup"><span data-stu-id="272b0-205">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="272b0-206">Örnek:</span><span class="sxs-lookup"><span data-stu-id="272b0-206">Example:</span></span><br>`services.AddScoped<MyDep>();` | <span data-ttu-id="272b0-207">Evet</span><span class="sxs-lookup"><span data-stu-id="272b0-207">Yes</span></span> | <span data-ttu-id="272b0-208">Hayır</span><span class="sxs-lookup"><span data-stu-id="272b0-208">No</span></span> | <span data-ttu-id="272b0-209">Hayır</span><span class="sxs-lookup"><span data-stu-id="272b0-209">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="272b0-210">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="272b0-210">Examples:</span></span><br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="272b0-211">Hayır</span><span class="sxs-lookup"><span data-stu-id="272b0-211">No</span></span> | <span data-ttu-id="272b0-212">Evet</span><span class="sxs-lookup"><span data-stu-id="272b0-212">Yes</span></span> | <span data-ttu-id="272b0-213">Evet</span><span class="sxs-lookup"><span data-stu-id="272b0-213">Yes</span></span> |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br><span data-ttu-id="272b0-214">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="272b0-214">Examples:</span></span><br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | <span data-ttu-id="272b0-215">Hayır</span><span class="sxs-lookup"><span data-stu-id="272b0-215">No</span></span> | <span data-ttu-id="272b0-216">Hayır</span><span class="sxs-lookup"><span data-stu-id="272b0-216">No</span></span> | <span data-ttu-id="272b0-217">Evet</span><span class="sxs-lookup"><span data-stu-id="272b0-217">Yes</span></span> |

<span data-ttu-id="272b0-218">Tür çıkarma hakkında daha fazla bilgi için [Hizmetler 'In aktiften çıkarılması](#disposal-of-services) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="272b0-218">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="272b0-219">Birden çok uygulama için yaygın bir senaryo, [test için bir sahte işlem türüdür](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="272b0-219">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="272b0-220">`TryAdd{LIFETIME}`Yöntemler, zaten kayıtlı bir uygulama yoksa hizmeti kaydeder.</span><span class="sxs-lookup"><span data-stu-id="272b0-220">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="272b0-221">Aşağıdaki örnekte, ilk satır için `MyDependency` `IMyDependency`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-221">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="272b0-222">Zaten kayıtlı bir uygulamaya sahip olduğundan `IMyDependency` ikinci satır etkisizdir:</span><span class="sxs-lookup"><span data-stu-id="272b0-222">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="272b0-223">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="272b0-223">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="272b0-224">[TryAddEnumerable (ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) yöntemleri yalnızca *aynı türde*bir uygulama yoksa hizmeti kaydeder.</span><span class="sxs-lookup"><span data-stu-id="272b0-224">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="272b0-225">Aracılığıyla `IEnumerable<{SERVICE}>`birden çok hizmet çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="272b0-225">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="272b0-226">Hizmetleri kaydederken, geliştirici yalnızca aynı türden biri zaten eklenmediyse bir örnek eklemek istemektedir.</span><span class="sxs-lookup"><span data-stu-id="272b0-226">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="272b0-227">Genellikle, bu yöntem, kapsayıcıda bir örneğin iki kopyasını kaydetmemek için kitaplık yazarları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="272b0-227">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="272b0-228">Aşağıdaki örnekte, ilk satır için `MyDep` `IMyDep1`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-228">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="272b0-229">İkinci satır için `IMyDep2`kaydedilir `MyDep` .</span><span class="sxs-lookup"><span data-stu-id="272b0-229">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="272b0-230">Zaten kayıtlı bir `IMyDep1` `MyDep`uygulamasına sahip olduğundan, üçüncü satırın etkisi yoktur:</span><span class="sxs-lookup"><span data-stu-id="272b0-230">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="272b0-231">Oluşturucu Ekleme davranışı</span><span class="sxs-lookup"><span data-stu-id="272b0-231">Constructor injection behavior</span></span>

<span data-ttu-id="272b0-232">Hizmetler, iki mekanizma tarafından çözülebilir:</span><span class="sxs-lookup"><span data-stu-id="272b0-232">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="272b0-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities>&ndash; Bağımlılık ekleme kapsayıcısında hizmet kaydı olmadan nesne oluşturulmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="272b0-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="272b0-234">`ActivatorUtilities`Etiket Yardımcıları, MVC denetleyicileri ve model ciltler gibi kullanıcıya yönelik soyutlamalar ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="272b0-234">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="272b0-235">Oluşturucular bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bağımsız değişkenlerin varsayılan değerleri ataması gerekir.</span><span class="sxs-lookup"><span data-stu-id="272b0-235">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="272b0-236">Hizmetler veya `IServiceProvider` `ActivatorUtilities`tarafından çözümlendiğinde, Oluşturucu Ekleme *ortak* bir Oluşturucu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="272b0-236">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="272b0-237">Hizmetler tarafından `ActivatorUtilities`çözümlendiğinde, Oluşturucu ekleme yalnızca bir adet geçerli oluşturucunun var olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="272b0-237">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="272b0-238">Oluşturucu aşırı yüklemeleri desteklenir, ancak bağımsız değişkenleri bağımlılık ekleme tarafından yerine yalnızca bir aşırı yükleme bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-238">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="272b0-239">Entity Framework bağlamları</span><span class="sxs-lookup"><span data-stu-id="272b0-239">Entity Framework contexts</span></span>

<span data-ttu-id="272b0-240">Entity Framework bağlamlar genellikle, Web uygulaması veritabanı işlemleri normalde istemci isteği kapsamında olduğundan [kapsamlı ömür](#service-lifetimes) kullanılarak hizmet kapsayıcısına eklenir.</span><span class="sxs-lookup"><span data-stu-id="272b0-240">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="272b0-241">Veritabanı bağlamı kaydedilirken aşırı yükleme [> bir\<adddbcontext tcontext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) tarafından belirtilmemişse, varsayılan yaşam süresi kapsamındadır.</span><span class="sxs-lookup"><span data-stu-id="272b0-241">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="272b0-242">Belirli bir yaşam süresinin Hizmetleri, hizmetten daha kısa bir yaşam süresine sahip bir veritabanı bağlamı kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-242">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="272b0-243">Ömür ve kayıt seçenekleri</span><span class="sxs-lookup"><span data-stu-id="272b0-243">Lifetime and registration options</span></span>

<span data-ttu-id="272b0-244">Ömür ve kayıt seçenekleri arasındaki farkı göstermek için, görevleri benzersiz bir tanımlayıcıya `OperationId`sahip bir işlem olarak temsil eden aşağıdaki arayüzleri göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="272b0-244">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="272b0-245">Bir işlem hizmetinin yaşam süresinin aşağıdaki arabirimler için nasıl yapılandırıldığına bağlı olarak kapsayıcı, bir sınıf tarafından istendiğinde aynı ya da farklı bir hizmet örneği sağlar:</span><span class="sxs-lookup"><span data-stu-id="272b0-245">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="272b0-246">Arabirimler `Operation` sınıfında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="272b0-246">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="272b0-247">Bir tane sağlanmazsa Oluşturucu bir GUID oluşturur: `Operation`</span><span class="sxs-lookup"><span data-stu-id="272b0-247">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="272b0-248">`OperationService` , Diğer`Operation` türlerin her birine bağlı olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-248">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="272b0-249">`OperationService` Bağımlılık ekleme yoluyla istendiğinde, her bir hizmetin yeni bir örneğini ya da bağımlı hizmetin kullanım ömrü temelinde mevcut bir örneği alır.</span><span class="sxs-lookup"><span data-stu-id="272b0-249">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="272b0-250">Kapsayıcıda istendiğinde `OperationId` geçici hizmetler oluşturulduğunda, `IOperationTransient` `OperationId` hizmet öğesinden `OperationService`farklı olur.</span><span class="sxs-lookup"><span data-stu-id="272b0-250">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="272b0-251">`OperationService``IOperationTransient` sınıfının yeni bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="272b0-251">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="272b0-252">Yeni örnek farklı `OperationId`bir şekilde oluşturur.</span><span class="sxs-lookup"><span data-stu-id="272b0-252">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="272b0-253">İstemci isteği başına kapsamlı hizmetler oluşturulduğunda, `OperationId` `IOperationScoped` hizmetin istemci isteği `OperationService` içindeki ile aynı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="272b0-253">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="272b0-254">İstemci istekleri arasında her iki hizmet de farklı `OperationId` bir değer paylaşır.</span><span class="sxs-lookup"><span data-stu-id="272b0-254">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="272b0-255">Tek ve tek örnekli hizmetler bir kez oluşturulduğunda ve tüm istemci isteklerinde ve tüm hizmetlerde `OperationId` kullanıldığında, tüm hizmet istekleri genelinde sabittir.</span><span class="sxs-lookup"><span data-stu-id="272b0-255">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="272b0-256">' `Startup.ConfigureServices`De, her tür kapsayıcısına, adlandırılmış ömrüne göre eklenir:</span><span class="sxs-lookup"><span data-stu-id="272b0-256">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="272b0-257">Hizmet, bilinen `Guid.Empty`kimliği olan belirli bir örnek kullanıyor. `IOperationSingletonInstance`</span><span class="sxs-lookup"><span data-stu-id="272b0-257">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="272b0-258">Bu tür kullanımda olduğunda (GUID 'sinin tümü sıfırlardan tamamen) Bu bir şey vardır.</span><span class="sxs-lookup"><span data-stu-id="272b0-258">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="272b0-259">Örnek uygulama, bireysel istekler içindeki ve içindeki nesne yaşam sürelerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="272b0-259">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="272b0-260">Örnek uygulama `IndexModel` , her `IOperation` tür ve ' i `OperationService`ister.</span><span class="sxs-lookup"><span data-stu-id="272b0-260">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="272b0-261">Daha sonra sayfa, tüm sayfa modeli sınıfının ve hizmetin `OperationId` değerlerini özellik atamaları aracılığıyla görüntüler:</span><span class="sxs-lookup"><span data-stu-id="272b0-261">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="272b0-262">Aşağıdaki iki çıktıda iki isteğin sonuçları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="272b0-262">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="272b0-263">**İlk istek:**</span><span class="sxs-lookup"><span data-stu-id="272b0-263">**First request:**</span></span>

<span data-ttu-id="272b0-264">Denetleyici işlemleri:</span><span class="sxs-lookup"><span data-stu-id="272b0-264">Controller operations:</span></span>

<span data-ttu-id="272b0-265">Geçici: d233e165-f417-469B-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="272b0-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="272b0-266">Yayıl 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="272b0-266">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="272b0-267">Adet 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="272b0-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="272b0-268">Instance 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="272b0-268">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="272b0-269">`OperationService`operasyonları</span><span class="sxs-lookup"><span data-stu-id="272b0-269">`OperationService` operations:</span></span>

<span data-ttu-id="272b0-270">Geçici: c6b049eb-1318-4E31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="272b0-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="272b0-271">Yayıl 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="272b0-271">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="272b0-272">Adet 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="272b0-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="272b0-273">Instance 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="272b0-273">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="272b0-274">**İkinci istek:**</span><span class="sxs-lookup"><span data-stu-id="272b0-274">**Second request:**</span></span>

<span data-ttu-id="272b0-275">Denetleyici işlemleri:</span><span class="sxs-lookup"><span data-stu-id="272b0-275">Controller operations:</span></span>

<span data-ttu-id="272b0-276">Geçici: b63bd538-0a37-4FF1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="272b0-276">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="272b0-277">Yayıl 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="272b0-277">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="272b0-278">Adet 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="272b0-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="272b0-279">Instance 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="272b0-279">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="272b0-280">`OperationService`operasyonları</span><span class="sxs-lookup"><span data-stu-id="272b0-280">`OperationService` operations:</span></span>

<span data-ttu-id="272b0-281">Geçici: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="272b0-281">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="272b0-282">Yayıl 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="272b0-282">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="272b0-283">Adet 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="272b0-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="272b0-284">Instance 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="272b0-284">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="272b0-285">`OperationId` Değerlerin bir istek içinde ve istekler arasında değiştiğini gözlemleyin:</span><span class="sxs-lookup"><span data-stu-id="272b0-285">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="272b0-286">*Geçici* nesneler her zaman farklıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-286">*Transient* objects are always different.</span></span> <span data-ttu-id="272b0-287">Hem birinci `OperationId` hem de ikinci istemci isteklerinin geçici değeri hem işlemler hem de `OperationService` istemci istekleri arasında farklıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-287">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="272b0-288">Her hizmet isteğine ve istemci isteğine yeni bir örnek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="272b0-288">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="272b0-289">*Kapsamlı* nesneler istemci isteği içinde aynıdır ancak istemci istekleri arasında farklıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-289">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="272b0-290">*Tek* nesneler, ' de `Operation` `Startup.ConfigureServices`bir örneğin sağlanmadığına bakılmaksızın her nesne için ve her istek için aynıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-290">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="272b0-291">Ana bilgisayardan Hizmetleri çağır</span><span class="sxs-lookup"><span data-stu-id="272b0-291">Call services from main</span></span>

<span data-ttu-id="272b0-292">Uygulamanın kapsamındaki <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> bir kapsamlı hizmeti çözümlemek için [ıvicescopefactory. CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) ile bir oluşturun.</span><span class="sxs-lookup"><span data-stu-id="272b0-292">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="272b0-293">Bu yaklaşım, başlatma görevlerini çalıştırmak üzere başlangıçta kapsamlı bir hizmete erişmek için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-293">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="272b0-294">Aşağıdaki örnek, `MyScopedService` içinde `Program.Main`için nasıl bağlam alınacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="272b0-294">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="272b0-295">Kapsam doğrulaması</span><span class="sxs-lookup"><span data-stu-id="272b0-295">Scope validation</span></span>

<span data-ttu-id="272b0-296">Uygulama geliştirme ortamında çalışırken, varsayılan hizmet sağlayıcısı şunları doğrulamak için denetimler gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="272b0-296">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="272b0-297">Kapsamlı hizmetler doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="272b0-297">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="272b0-298">Kapsamlı hizmetler doğrudan veya dolaylı olarak Singleton 'a eklenmiş değildir.</span><span class="sxs-lookup"><span data-stu-id="272b0-298">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="272b0-299">Kök hizmet sağlayıcısı çağrıldığında oluşturulur <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> .</span><span class="sxs-lookup"><span data-stu-id="272b0-299">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="272b0-300">Kök hizmet sağlayıcısının ömrü, sağlayıcının uygulamayla başladığı ve uygulama kapandığında bırakıldığı uygulama/sunucunun yaşam süresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="272b0-300">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="272b0-301">Kapsamlı hizmetler kendilerini oluşturan kapsayıcı tarafından atılmış.</span><span class="sxs-lookup"><span data-stu-id="272b0-301">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="272b0-302">Kök kapsayıcıda kapsamlı bir hizmet oluşturulduysa, hizmetin ömrü etkin şekilde tek başına yükseltilir çünkü yalnızca uygulama/sunucu kapatıldığında kök kapsayıcı tarafından atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="272b0-302">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="272b0-303">Hizmet kapsamlarını doğrulamak `BuildServiceProvider` , çağrıldığında bu durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="272b0-303">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="272b0-304">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="272b0-304">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="272b0-305">İstek Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="272b0-305">Request Services</span></span>

<span data-ttu-id="272b0-306">ASP.NET Core isteği `HttpContext` içinde bulunan hizmetler, [HttpContext. requestservices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) koleksiyonu aracılığıyla sunulur.</span><span class="sxs-lookup"><span data-stu-id="272b0-306">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="272b0-307">İstek Hizmetleri, uygulamanın bir parçası olarak yapılandırılan ve istenen hizmetleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="272b0-307">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="272b0-308">Nesneler bağımlılıklar belirttiğinizde, bunlar ' de `RequestServices` `ApplicationServices`bulunan türler tarafından karşılanır.</span><span class="sxs-lookup"><span data-stu-id="272b0-308">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="272b0-309">Genellikle, uygulamanın bu özellikleri doğrudan kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-309">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="272b0-310">Bunun yerine, sınıfların Sınıf oluşturucuları aracılığıyla gerektirdiği türleri isteyin ve çerçevenin bağımlılıkları eklemesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="272b0-310">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="272b0-311">Bu, test etmek daha kolay olan sınıfları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="272b0-311">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="272b0-312">`RequestServices` Koleksiyona erişmek için Oluşturucu parametreleri olarak bağımlılıklar istemeyi tercih edin.</span><span class="sxs-lookup"><span data-stu-id="272b0-312">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="272b0-313">Bağımlılık ekleme için tasarım hizmetleri</span><span class="sxs-lookup"><span data-stu-id="272b0-313">Design services for dependency injection</span></span>

<span data-ttu-id="272b0-314">En iyi uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="272b0-314">Best practices are to:</span></span>

* <span data-ttu-id="272b0-315">Bağımlılıklarını almak için bağımlılık ekleme 'yi kullanmak üzere Hizmetleri tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="272b0-315">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="272b0-316">Durum bilgisi olan statik yöntem çağrılarından kaçının.</span><span class="sxs-lookup"><span data-stu-id="272b0-316">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="272b0-317">Hizmetler içindeki bağımlı sınıfların doğrudan örneklenmesini önleyin.</span><span class="sxs-lookup"><span data-stu-id="272b0-317">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="272b0-318">Doğrudan örnekleme kodu belirli bir uygulamaya bağar.</span><span class="sxs-lookup"><span data-stu-id="272b0-318">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="272b0-319">Uygulama sınıflarını küçük, iyi bir şekilde ve kolayca test edin.</span><span class="sxs-lookup"><span data-stu-id="272b0-319">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="272b0-320">Bir sınıfta çok fazla sayıda bağımlılık varsa, bu genellikle sınıfta çok fazla sorumluluk olduğu ve [tek sorumluluk ilkesini (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility)ihlal eden bir imzadır.</span><span class="sxs-lookup"><span data-stu-id="272b0-320">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="272b0-321">Bazı sorumlulukları yeni bir sınıfa taşıyarak sınıfı yeniden düzenleme girişimi.</span><span class="sxs-lookup"><span data-stu-id="272b0-321">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="272b0-322">Razor Pages sayfa modeli sınıfları ve MVC denetleyici sınıflarının UI kaygılarıyla odaklanıp ilgilenmeyeceğini aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="272b0-322">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="272b0-323">İş kuralları ve veri erişimi uygulama ayrıntıları, bu [ayrı kaygılara](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)uygun sınıflarda tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-323">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="272b0-324">Hizmetlerin elden çıkarılması</span><span class="sxs-lookup"><span data-stu-id="272b0-324">Disposal of services</span></span>

<span data-ttu-id="272b0-325">Kapsayıcı, oluşturduğu <xref:System.IDisposable.Dispose*> <xref:System.IDisposable> türleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="272b0-325">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="272b0-326">Kapsayıcıda Kullanıcı kodu tarafından bir örnek eklenirse, otomatik olarak atılamaz.</span><span class="sxs-lookup"><span data-stu-id="272b0-326">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="272b0-327">Varsayılan hizmet kapsayıcısı değiştirme</span><span class="sxs-lookup"><span data-stu-id="272b0-327">Default service container replacement</span></span>

<span data-ttu-id="272b0-328">Yerleşik hizmet kapsayıcısı, çerçeve ihtiyaçlarını ve çoğu tüketici uygulamayı sunmaktır.</span><span class="sxs-lookup"><span data-stu-id="272b0-328">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="272b0-329">Desteklemediği belirli bir özelliğe ihtiyaç duymadığınız takdirde, yerleşik kapsayıcının kullanılması önerilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-329">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="272b0-330">3\. taraf kapsayıcılarında desteklenen özelliklerden bazıları yerleşik kapsayıcıda bulunamadı:</span><span class="sxs-lookup"><span data-stu-id="272b0-330">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="272b0-331">Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="272b0-331">Property injection</span></span>
* <span data-ttu-id="272b0-332">Ada göre ekleme</span><span class="sxs-lookup"><span data-stu-id="272b0-332">Injection based on name</span></span>
* <span data-ttu-id="272b0-333">Alt kapsayıcılar</span><span class="sxs-lookup"><span data-stu-id="272b0-333">Child containers</span></span>
* <span data-ttu-id="272b0-334">Özel ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="272b0-334">Custom lifetime management</span></span>
* <span data-ttu-id="272b0-335">`Func<T>`yavaş başlatma desteği</span><span class="sxs-lookup"><span data-stu-id="272b0-335">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="272b0-336">Bağdaştırıcıları destekleyen bazı kapsayıcıların bir listesi için [bağımlılık ekleme Readme.MD dosyasına](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) bakın.</span><span class="sxs-lookup"><span data-stu-id="272b0-336">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="272b0-337">Aşağıdaki örnek, [Autofac](https://autofac.org/)ile yerleşik kapsayıcının yerini alır:</span><span class="sxs-lookup"><span data-stu-id="272b0-337">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="272b0-338">Uygun kapsayıcı paketlerini yükler:</span><span class="sxs-lookup"><span data-stu-id="272b0-338">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="272b0-339">Autofac</span><span class="sxs-lookup"><span data-stu-id="272b0-339">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="272b0-340">Autofac. Extensions. Dependencyınjection</span><span class="sxs-lookup"><span data-stu-id="272b0-340">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="272b0-341">' De `Startup.ConfigureServices` kapsayıcıyı yapılandırın ve şunu `IServiceProvider`döndürün:</span><span class="sxs-lookup"><span data-stu-id="272b0-341">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

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

    <span data-ttu-id="272b0-342">3\. taraf kapsayıcısını `Startup.ConfigureServices` kullanmak için döndürmelidir. `IServiceProvider`</span><span class="sxs-lookup"><span data-stu-id="272b0-342">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="272b0-343">Autofac 'i `DefaultModule`yapılandırma:</span><span class="sxs-lookup"><span data-stu-id="272b0-343">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="272b0-344">Çalışma zamanında, Autofac türleri ve ekleme bağımlılıklarını çözümlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="272b0-344">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="272b0-345">ASP.NET Core ile Autofac kullanma hakkında daha fazla bilgi edinmek için [Autofac belgelerine](https://docs.autofac.org/en/latest/integration/aspnetcore.html)bakın.</span><span class="sxs-lookup"><span data-stu-id="272b0-345">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="272b0-346">İş parçacığı güvenliği</span><span class="sxs-lookup"><span data-stu-id="272b0-346">Thread safety</span></span>

<span data-ttu-id="272b0-347">İş parçacığı güvenli Singleton Hizmetleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="272b0-347">Create thread-safe singleton services.</span></span> <span data-ttu-id="272b0-348">Tek bir hizmetin geçici bir hizmete bağımlılığı varsa, geçici hizmet aynı zamanda tek tek tarafından nasıl kullanıldığına bağlı olarak iş parçacığı güvenliği de gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-348">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="272b0-349">Tek bir hizmetin fabrika yöntemi (örneğin, [\<AddSingleton TService > için ikinci bağımsız değişken) (ıseviecollection,\<Func IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), iş parçacığı açısından güvenli olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="272b0-349">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="272b0-350">Bir Type (`static`) Oluşturucusu gibi, tek bir iş parçacığı tarafından bir kez çağrılması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="272b0-350">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="272b0-351">Öneriler</span><span class="sxs-lookup"><span data-stu-id="272b0-351">Recommendations</span></span>

* <span data-ttu-id="272b0-352">`async/await`ve `Task` tabanlı hizmet çözümlemesi desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="272b0-352">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="272b0-353">C#zaman uyumsuz oluşturucuları desteklemez; Bu nedenle, önerilen model hizmeti zaman uyumlu olarak çözümledikten sonra zaman uyumsuz yöntemler kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="272b0-353">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="272b0-354">Veri ve yapılandırmayı doğrudan hizmet kapsayıcısında saklamaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="272b0-354">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="272b0-355">Örneğin, bir kullanıcının alışveriş sepeti genellikle hizmet kapsayıcısına eklenmemelidir.</span><span class="sxs-lookup"><span data-stu-id="272b0-355">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="272b0-356">Yapılandırma, [Seçenekler modelini](xref:fundamentals/configuration/options)kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="272b0-356">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="272b0-357">Benzer şekilde, yalnızca başka bir nesneye erişime izin vermek için mevcut olan "veri sahibi" nesnelerinden kaçının.</span><span class="sxs-lookup"><span data-stu-id="272b0-357">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="272b0-358">DI aracılığıyla gerçek öğe istemek daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="272b0-358">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="272b0-359">Hizmetlere statik erişimi önleyin (örneğin, başka bir yerde kullanmak üzere, statik olarak yazılan [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) ).</span><span class="sxs-lookup"><span data-stu-id="272b0-359">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="272b0-360">*Hizmet bulucu deseninin*kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="272b0-360">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="272b0-361">Örneğin, yerine şunu kullandığınızda <xref:System.IServiceProvider.GetService*> bir hizmet örneği elde etme çağrısı yapmayın:</span><span class="sxs-lookup"><span data-stu-id="272b0-361">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="272b0-362">**Olmayan**</span><span class="sxs-lookup"><span data-stu-id="272b0-362">**Incorrect:**</span></span>

  ```csharp
  public class MyClass()
  {
      public void MyMethod()
      {
          var optionsMonitor = 
              _services.GetService<IOptionsMonitor<MyOptions>>();
          var option = optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

  <span data-ttu-id="272b0-363">**Doğru**:</span><span class="sxs-lookup"><span data-stu-id="272b0-363">**Correct**:</span></span>

  ```csharp
  public class MyClass
  {
      private readonly IOptionsMonitor<MyOptions> _optionsMonitor;

      public MyClass(IOptionsMonitor<MyOptions> optionsMonitor)
      {
          _optionsMonitor = optionsMonitor;
      }

      public void MyMethod()
      {
          var option = _optionsMonitor.CurrentValue.Option;

          ...
      }
  }
  ```

* <span data-ttu-id="272b0-364">Önlemek için başka bir hizmet bulucu çeşitlemesi, çalışma zamanında bağımlılıkları çözümleyen bir ekleme.</span><span class="sxs-lookup"><span data-stu-id="272b0-364">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="272b0-365">Bu uygulamalardan her ikisi de [Denetim stratejilerini geçersiz kılar](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .</span><span class="sxs-lookup"><span data-stu-id="272b0-365">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="272b0-366">Uygulamasına `HttpContext` statik erişimi önleyin (örneğin, [ıhttpcontextaccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="272b0-366">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="272b0-367">Tüm öneri kümeleri gibi, bir öneriyi yok saymayı yok saymış durumlarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="272b0-367">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="272b0-368">Özel durumlar&mdash;genellikle çerçevenin kendisi içinde özel durumlardır.</span><span class="sxs-lookup"><span data-stu-id="272b0-368">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="272b0-369">Dı, statik/genel nesne erişim desenlerinin bir *alternatifidir* .</span><span class="sxs-lookup"><span data-stu-id="272b0-369">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="272b0-370">Statik nesne erişimi ile karıştırırsanız, dı 'nin avantajlarını fark edemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="272b0-370">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="272b0-371">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="272b0-371">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="272b0-372">Bağımlılık ekleme (MSDN) ile ASP.NET Core temizleme kodu yazma</span><span class="sxs-lookup"><span data-stu-id="272b0-372">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="272b0-373">Açık bağımlılıklar Ilkesi</span><span class="sxs-lookup"><span data-stu-id="272b0-373">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="272b0-374">Denetim kapsayıcıları ve bağımlılık ekleme deseninin Inversion 'ı (Marwler)</span><span class="sxs-lookup"><span data-stu-id="272b0-374">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="272b0-375">ASP.NET Core DI 'de birden çok arabirime sahip bir hizmeti kaydetme</span><span class="sxs-lookup"><span data-stu-id="272b0-375">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
