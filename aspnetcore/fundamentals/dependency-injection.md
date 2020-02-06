---
title: ASP.NET Core bağımlılık ekleme
author: guardrex
description: ASP.NET Core bağımlılık ekleme ve nasıl kullanılacağı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/05/2020
uid: fundamentals/dependency-injection
ms.openlocfilehash: 7c0789dafcb7dfacd15ac448a39bad94649963c8
ms.sourcegitcommit: bd896935e91236e03241f75e6534ad6debcecbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77044927"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="6d2e1-103">ASP.NET Core bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="6d2e1-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="6d2e1-104">[Steve Smith](https://ardalis.com/), [Scott Ade](https://scottaddie.com)ve [Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="6d2e1-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6d2e1-105">ASP.NET Core, sınıflar ve bunların bağımlılıkları arasında [denetimin INVERSION (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) elde etmek için bir teknik olan bağımlılık ekleme (dı) yazılım tasarım modelini destekler.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="6d2e1-106">MVC denetleyicileri içindeki bağımlılık eklenmesine özgü daha fazla bilgi için bkz. <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="6d2e1-107">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6d2e1-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="6d2e1-108">Bağımlılık eklenmesine genel bakış</span><span class="sxs-lookup"><span data-stu-id="6d2e1-108">Overview of dependency injection</span></span>

<span data-ttu-id="6d2e1-109">*Bağımlılık* , başka bir nesnenin gerektirdiği herhangi bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="6d2e1-110">Aşağıdaki `MyDependency` sınıfını bir uygulamadaki diğer sınıfların bağlı olduğu bir `WriteMessage` yöntemiyle inceleyin:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="6d2e1-111">`WriteMessage` yöntemini bir sınıf için kullanılabilir hale getirmek için `MyDependency` sınıfının bir örneği oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="6d2e1-112">`MyDependency` sınıfı, `IndexModel` sınıfının bir bağımlılığı olur:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="6d2e1-113">Sınıf oluşturur ve doğrudan `MyDependency` örneğine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="6d2e1-114">Kod bağımlılıkları (önceki örnekte olduğu gibi) sorunlu olur ve aşağıdaki nedenlerden dolayı kaçınılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="6d2e1-115">`MyDependency` farklı bir uygulamayla değiştirmek için, sınıfın değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="6d2e1-116">`MyDependency` bağımlılıklar içeriyorsa, sınıfı tarafından yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="6d2e1-117">`MyDependency`bağlı olarak, birden çok sınıfa sahip büyük bir projede yapılandırma kodu uygulama genelinde dağılmış hale gelir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="6d2e1-118">Bu uygulamanın birim testi zordur.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="6d2e1-119">Uygulama, bu yaklaşımla mümkün olmayan bir sahte veya saplama `MyDependency` sınıfı kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="6d2e1-120">Bağımlılık ekleme bu sorunları şu şekilde giderir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="6d2e1-121">Bağımlılık uygulamasını soyutlamak için bir arabirim veya temel sınıf kullanımı.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="6d2e1-122">Bir hizmet kapsayıcısına bağımlılığın kaydı.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="6d2e1-123">ASP.NET Core yerleşik bir hizmet kapsayıcısı sağlar <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="6d2e1-124">Hizmetler, uygulamanın `Startup.ConfigureServices` yöntemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="6d2e1-125">Hizmetin kullanıldığı sınıf oluşturucusuna *ekleme* .</span><span class="sxs-lookup"><span data-stu-id="6d2e1-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="6d2e1-126">Çerçeve, bağımlılığın bir örneğini oluşturma ve artık gerekli olmadığında bu uygulamayı atma sorumluluğunu alır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="6d2e1-127">[Örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` arabirimi hizmetin uygulamaya sağladığı bir yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6d2e1-128">Bu arabirim somut bir tür tarafından uygulanır, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6d2e1-129">`MyDependency` kurucusunda bir <xref:Microsoft.Extensions.Logging.ILogger`1> ister.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="6d2e1-130">Bağımlılık ekleme işlemini zincirleme bir biçimde kullanmak olağan dışı değildir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="6d2e1-131">Her istenen bağımlılık, kendi bağımlılıklarını ister.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="6d2e1-132">Kapsayıcı grafikteki bağımlılıkları çözer ve tamamen çözümlenen hizmeti döndürür.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="6d2e1-133">Çözümlenmesi gereken, genellikle *bağımlılık ağacı*, *bağımlılık grafiği*veya *nesne grafiği*olarak adlandırılan toplu bağımlılıklar kümesi.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="6d2e1-134">`IMyDependency` ve `ILogger<TCategoryName>`, hizmet kapsayıcısında kayıtlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="6d2e1-135">`IMyDependency` `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6d2e1-136">`ILogger<TCategoryName>`, günlük soyut öğeler altyapısı tarafından kaydedilir. bu nedenle, Framework tarafından varsayılan olarak kaydedilen [Framework tarafından sağlanmış bir hizmettir](#framework-provided-services) .</span><span class="sxs-lookup"><span data-stu-id="6d2e1-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="6d2e1-137">Kapsayıcı, [(genel) açık türlerden](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types)yararlanarak `ILogger<TCategoryName>` çözer, her [(genel) oluşturulan türü](/dotnet/csharp/language-reference/language-specification/types#constructed-types)kaydetme ihtiyacını ortadan kaldırır:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="6d2e1-138">Örnek uygulamada `IMyDependency` hizmeti somut tür `MyDependency`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="6d2e1-139">Kayıt, hizmet ömrünü tek bir isteğin kullanım ömrüne göre kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="6d2e1-140">[Hizmet yaşam süreleri](#service-lifetimes) bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="6d2e1-141">Her `services.Add{SERVICE_NAME}` uzantısı yöntemi Hizmetleri ekler (ve potansiyel olarak yapılandırır).</span><span class="sxs-lookup"><span data-stu-id="6d2e1-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="6d2e1-142">Örneğin `services.AddMvc()`, Razor Pages ve MVC 'nin gerektirdiği Hizmetleri ekler.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="6d2e1-143">Uygulamaların bu kuralı izlemesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="6d2e1-144">Hizmet kaydı gruplarını kapsüllemek için uzantı yöntemlerini [Microsoft. Extensions. Dependencyınjection](/dotnet/api/microsoft.extensions.dependencyinjection) ad alanına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="6d2e1-145">Hizmetin Oluşturucusu `string`gibi [yerleşik bir tür](/dotnet/csharp/language-reference/keywords/built-in-types-table)gerektiriyorsa, tür [yapılandırma](xref:fundamentals/configuration/index) veya [Seçenekler düzeniyle](xref:fundamentals/configuration/options)eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="6d2e1-146">Hizmetin bir örneği, hizmetin kullanıldığı ve özel bir alana atandığı bir sınıfın Oluşturucusu aracılığıyla istenir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="6d2e1-147">Alanı, sınıfına gereken şekilde hizmete erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="6d2e1-148">Örnek uygulamada, `IMyDependency` örneği istenir ve hizmetin `WriteMessage` yöntemini çağırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

::: moniker-end

## <a name="services-injected-into-startup"></a><span data-ttu-id="6d2e1-149">Başlangıca eklenen hizmetler</span><span class="sxs-lookup"><span data-stu-id="6d2e1-149">Services injected into Startup</span></span>

<span data-ttu-id="6d2e1-150">Genel ana bilgisayar (<xref:Microsoft.Extensions.Hosting.IHostBuilder>) kullanılırken `Startup` oluşturucusuna yalnızca aşağıdaki hizmet türleri eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="6d2e1-151">Hizmetler `Startup.Configure`eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="6d2e1-152">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="6d2e1-153">Framework tarafından sunulan hizmetler</span><span class="sxs-lookup"><span data-stu-id="6d2e1-153">Framework-provided services</span></span>

<span data-ttu-id="6d2e1-154">`Startup.ConfigureServices` yöntemi, uygulamanın kullandığı hizmetlerin (Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri de dahil) tanımlanmasından sorumludur.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="6d2e1-155">Başlangıçta, `ConfigureServices` için belirtilen `IServiceCollection` [konağın nasıl yapılandırıldığına](xref:fundamentals/index#host)bağlı olarak Framework tarafından tanımlanan hizmetlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="6d2e1-156">Çerçeve tarafından kaydedilmiş yüzlerce hizmete sahip olmak ASP.NET Core şablona dayalı bir uygulama için sık görülen bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="6d2e1-157">Aşağıdaki tabloda çerçeve kayıtlı hizmetlerden oluşan küçük bir örnek listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-157">A small sample of framework-registered services is listed in the following table.</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="6d2e1-158">Hizmet Türü</span><span class="sxs-lookup"><span data-stu-id="6d2e1-158">Service Type</span></span> | <span data-ttu-id="6d2e1-159">Ömür</span><span class="sxs-lookup"><span data-stu-id="6d2e1-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="6d2e1-160">Geçici</span><span class="sxs-lookup"><span data-stu-id="6d2e1-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="6d2e1-161">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="6d2e1-162">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="6d2e1-163">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="6d2e1-164">Geçici</span><span class="sxs-lookup"><span data-stu-id="6d2e1-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="6d2e1-165">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="6d2e1-166">Geçici</span><span class="sxs-lookup"><span data-stu-id="6d2e1-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="6d2e1-167">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="6d2e1-168">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="6d2e1-169">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="6d2e1-170">Geçici</span><span class="sxs-lookup"><span data-stu-id="6d2e1-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="6d2e1-171">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="6d2e1-172">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="6d2e1-173">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-173">Singleton</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="6d2e1-174">Hizmet Türü</span><span class="sxs-lookup"><span data-stu-id="6d2e1-174">Service Type</span></span> | <span data-ttu-id="6d2e1-175">Ömür</span><span class="sxs-lookup"><span data-stu-id="6d2e1-175">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="6d2e1-176">Geçici</span><span class="sxs-lookup"><span data-stu-id="6d2e1-176">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="6d2e1-177">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-177">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="6d2e1-178">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-178">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="6d2e1-179">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-179">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="6d2e1-180">Geçici</span><span class="sxs-lookup"><span data-stu-id="6d2e1-180">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="6d2e1-181">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-181">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="6d2e1-182">Geçici</span><span class="sxs-lookup"><span data-stu-id="6d2e1-182">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="6d2e1-183">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-183">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="6d2e1-184">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-184">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="6d2e1-185">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-185">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="6d2e1-186">Geçici</span><span class="sxs-lookup"><span data-stu-id="6d2e1-186">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="6d2e1-187">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-187">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="6d2e1-188">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-188">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="6d2e1-189">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-189">Singleton</span></span> |

::: moniker-end

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="6d2e1-190">Uzantı yöntemleriyle ek hizmetleri kaydetme</span><span class="sxs-lookup"><span data-stu-id="6d2e1-190">Register additional services with extension methods</span></span>

<span data-ttu-id="6d2e1-191">Bir hizmet koleksiyonu genişletme yöntemi (ve gerekirse bağımlı hizmetleri) kaydetmek için kullanılabilir olduğunda, bu hizmet için gereken tüm hizmetleri kaydetmek üzere tek bir `Add{SERVICE_NAME}` uzantısı yöntemi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-191">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="6d2e1-192">Aşağıdaki kod, [Adddbcontext\<tcontext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) ve <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>uzantı yöntemlerini kullanarak kapsayıcıya ek hizmetler eklemenin bir örneğidir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-192">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    ...
}
```

<span data-ttu-id="6d2e1-193">Daha fazla bilgi için API belgelerindeki <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> sınıfına bakın.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-193">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="6d2e1-194">Hizmet yaşam süreleri</span><span class="sxs-lookup"><span data-stu-id="6d2e1-194">Service lifetimes</span></span>

<span data-ttu-id="6d2e1-195">Kayıtlı her hizmet için uygun bir yaşam süresi seçin.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-195">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="6d2e1-196">ASP.NET Core hizmetler aşağıdaki yaşam süreleri ile yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-196">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="6d2e1-197">Geçici</span><span class="sxs-lookup"><span data-stu-id="6d2e1-197">Transient</span></span>

<span data-ttu-id="6d2e1-198">Geçici ömür Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>), hizmet kapsayıcısından her istenilişinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-198">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="6d2e1-199">Bu ömür, hafif ve durumsuz hizmetler için en iyi şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-199">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="6d2e1-200">Yayıl</span><span class="sxs-lookup"><span data-stu-id="6d2e1-200">Scoped</span></span>

<span data-ttu-id="6d2e1-201">Kapsamlı ömür Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>), istemci isteği başına bir kez oluşturulur (bağlantı).</span><span class="sxs-lookup"><span data-stu-id="6d2e1-201">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="6d2e1-202">Bir ara yazılım içinde kapsamlı bir hizmet kullanırken, hizmeti `Invoke` veya `InvokeAsync` yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-202">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="6d2e1-203">Oluşturucu ekleme yoluyla ekleme, hizmeti tek bir gibi davranmaya zoryor.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-203">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="6d2e1-204">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-204">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="6d2e1-205">Adet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-205">Singleton</span></span>

<span data-ttu-id="6d2e1-206">Tek yaşam süresi Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>), ilk istendiğinde oluşturulur (veya `Startup.ConfigureServices` çalıştırıldığında ve hizmet kaydıyla bir örnek belirtildiğinde).</span><span class="sxs-lookup"><span data-stu-id="6d2e1-206">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="6d2e1-207">Her sonraki istek aynı örneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-207">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="6d2e1-208">Uygulama tek davranış gerektiriyorsa, hizmet kapsayıcısının hizmetin ömrünü yönetmesine izin verilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-208">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="6d2e1-209">Tekil tasarım modelini uygulamayın ve nesnenin sınıfındaki ömrünü yönetmek için Kullanıcı kodu sağlayın.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-209">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="6d2e1-210">Kapsamlı bir hizmetin tek bir bilgisayardan çözümlenmesi tehlikelidir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-210">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="6d2e1-211">Bu, sonraki istekleri işlerken hizmetin yanlış duruma gelmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-211">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="6d2e1-212">Hizmet kayıt yöntemleri</span><span class="sxs-lookup"><span data-stu-id="6d2e1-212">Service registration methods</span></span>

<span data-ttu-id="6d2e1-213">Hizmet kayıt uzantısı yöntemleri, belirli senaryolarda yararlı olan aşırı yüklemeler sunar.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-213">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="6d2e1-214">Yöntem</span><span class="sxs-lookup"><span data-stu-id="6d2e1-214">Method</span></span> | <span data-ttu-id="6d2e1-215">Otomatik</span><span class="sxs-lookup"><span data-stu-id="6d2e1-215">Automatic</span></span><br><span data-ttu-id="6d2e1-216">nesne</span><span class="sxs-lookup"><span data-stu-id="6d2e1-216">object</span></span><br><span data-ttu-id="6d2e1-217">elden</span><span class="sxs-lookup"><span data-stu-id="6d2e1-217">disposal</span></span> | <span data-ttu-id="6d2e1-218">Birden Çok</span><span class="sxs-lookup"><span data-stu-id="6d2e1-218">Multiple</span></span><br><span data-ttu-id="6d2e1-219">uygulamalar</span><span class="sxs-lookup"><span data-stu-id="6d2e1-219">implementations</span></span> | <span data-ttu-id="6d2e1-220">Geçiş bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="6d2e1-220">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="6d2e1-221">Örnek:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-221">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="6d2e1-222">Evet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-222">Yes</span></span> | <span data-ttu-id="6d2e1-223">Evet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-223">Yes</span></span> | <span data-ttu-id="6d2e1-224">Hayır</span><span class="sxs-lookup"><span data-stu-id="6d2e1-224">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="6d2e1-225">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-225">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="6d2e1-226">Evet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-226">Yes</span></span> | <span data-ttu-id="6d2e1-227">Evet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-227">Yes</span></span> | <span data-ttu-id="6d2e1-228">Evet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-228">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="6d2e1-229">Örnek:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-229">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="6d2e1-230">Evet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-230">Yes</span></span> | <span data-ttu-id="6d2e1-231">Hayır</span><span class="sxs-lookup"><span data-stu-id="6d2e1-231">No</span></span> | <span data-ttu-id="6d2e1-232">Hayır</span><span class="sxs-lookup"><span data-stu-id="6d2e1-232">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="6d2e1-233">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-233">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="6d2e1-234">Hayır</span><span class="sxs-lookup"><span data-stu-id="6d2e1-234">No</span></span> | <span data-ttu-id="6d2e1-235">Evet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-235">Yes</span></span> | <span data-ttu-id="6d2e1-236">Evet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-236">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="6d2e1-237">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-237">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="6d2e1-238">Hayır</span><span class="sxs-lookup"><span data-stu-id="6d2e1-238">No</span></span> | <span data-ttu-id="6d2e1-239">Hayır</span><span class="sxs-lookup"><span data-stu-id="6d2e1-239">No</span></span> | <span data-ttu-id="6d2e1-240">Evet</span><span class="sxs-lookup"><span data-stu-id="6d2e1-240">Yes</span></span> |

<span data-ttu-id="6d2e1-241">Tür çıkarma hakkında daha fazla bilgi için [Hizmetler 'In aktiften çıkarılması](#disposal-of-services) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-241">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="6d2e1-242">Birden çok uygulama için yaygın bir senaryo, [test için bir sahte işlem türüdür](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="6d2e1-242">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="6d2e1-243">`TryAdd{LIFETIME}` Yöntemler, zaten kayıtlı bir uygulama yoksa hizmeti kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-243">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="6d2e1-244">Aşağıdaki örnekte, ilk satır `IMyDependency`için `MyDependency` kaydettirir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-244">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="6d2e1-245">`IMyDependency` zaten kayıtlı bir uygulamaya sahip olduğundan ikinci satır etkisizdir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-245">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="6d2e1-246">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-246">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="6d2e1-247">[TryAddEnumerable (ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) yöntemleri yalnızca *aynı türde*bir uygulama yoksa hizmeti kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-247">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="6d2e1-248">Birden çok hizmet `IEnumerable<{SERVICE}>`ile çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-248">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="6d2e1-249">Hizmetleri kaydederken, geliştirici yalnızca aynı türden biri zaten eklenmediyse bir örnek eklemek istemektedir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-249">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="6d2e1-250">Genellikle, bu yöntem, kapsayıcıda bir örneğin iki kopyasını kaydetmemek için kitaplık yazarları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-250">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="6d2e1-251">Aşağıdaki örnekte, ilk satır `IMyDep1`için `MyDep` kaydettirir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-251">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="6d2e1-252">İkinci satır, `IMyDep2`için `MyDep` kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-252">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="6d2e1-253">`IMyDep1` `MyDep`kayıtlı bir uygulamasına zaten sahip olduğundan, üçüncü satırın etkisi yoktur:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-253">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="6d2e1-254">Oluşturucu Ekleme davranışı</span><span class="sxs-lookup"><span data-stu-id="6d2e1-254">Constructor injection behavior</span></span>

<span data-ttu-id="6d2e1-255">Hizmetler, iki mekanizma tarafından çözülebilir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-255">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="6d2e1-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash;, bağımlılık ekleme kapsayıcısına hizmet kaydı olmadan nesne oluşturulmasına Izin verir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-256"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="6d2e1-257">`ActivatorUtilities` etiket yardımcıları, MVC denetleyicileri ve model ciltler gibi kullanıcı tarafından ilgili soyutlamalar ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-257">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="6d2e1-258">Oluşturucular bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bağımsız değişkenlerin varsayılan değerleri ataması gerekir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-258">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="6d2e1-259">Hizmetler `IServiceProvider` veya `ActivatorUtilities`tarafından çözümlendiğinde, Oluşturucu Ekleme *ortak* bir Oluşturucu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-259">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="6d2e1-260">Hizmetler `ActivatorUtilities`tarafından çözümlendiğinde, Oluşturucu ekleme yalnızca bir adet geçerli oluşturucunun var olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-260">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="6d2e1-261">Oluşturucu aşırı yüklemeleri desteklenir, ancak bağımsız değişkenleri bağımlılık ekleme tarafından yerine yalnızca bir aşırı yükleme bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-261">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="6d2e1-262">Entity Framework bağlamları</span><span class="sxs-lookup"><span data-stu-id="6d2e1-262">Entity Framework contexts</span></span>

<span data-ttu-id="6d2e1-263">Entity Framework bağlamlar genellikle, Web uygulaması veritabanı işlemleri normalde istemci isteği kapsamında olduğundan [kapsamlı ömür](#service-lifetimes) kullanılarak hizmet kapsayıcısına eklenir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-263">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="6d2e1-264">Veritabanı bağlamı kaydedilirken bir [Adddbcontext\<tcontext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) aşırı yüklemesi tarafından bir yaşam süresi belirtilmemişse varsayılan yaşam süresi kapsamındadır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-264">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="6d2e1-265">Belirli bir yaşam süresinin Hizmetleri, hizmetten daha kısa bir yaşam süresine sahip bir veritabanı bağlamı kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-265">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="6d2e1-266">Ömür ve kayıt seçenekleri</span><span class="sxs-lookup"><span data-stu-id="6d2e1-266">Lifetime and registration options</span></span>

<span data-ttu-id="6d2e1-267">Ömür ve kayıt seçenekleri arasındaki farkı göstermek için, görevleri benzersiz bir tanımlayıcıya sahip bir işlem olarak temsil eden aşağıdaki arayüzleri göz önünde bulundurun `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-267">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="6d2e1-268">Bir işlem hizmetinin yaşam süresinin aşağıdaki arabirimler için nasıl yapılandırıldığına bağlı olarak kapsayıcı, bir sınıf tarafından istendiğinde aynı ya da farklı bir hizmet örneği sağlar:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-268">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6d2e1-269">Arabirimler `Operation` sınıfında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-269">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="6d2e1-270">`Operation` Oluşturucusu bir GUID sağlanmamışsa bir GUID oluşturur:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-270">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6d2e1-271">Diğer `Operation` türlerinin her birine bağlı olan bir `OperationService` kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-271">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="6d2e1-272">Bağımlılık ekleme yoluyla `OperationService` istendiğinde, her bir hizmetin yeni bir örneğini ya da bağımlı hizmetin kullanım ömrü temelinde mevcut bir örneği alır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-272">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="6d2e1-273">Kapsayıcıda istendiğinde geçici hizmetler oluşturulduğunda, `IOperationTransient` hizmetinin `OperationId` `OperationService``OperationId` farklıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-273">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="6d2e1-274">`OperationService`, `IOperationTransient` sınıfının yeni bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-274">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="6d2e1-275">Yeni örnek farklı bir `OperationId`verir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-275">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="6d2e1-276">İstemci isteği başına kapsamlı hizmetler oluşturulduğunda, `IOperationScoped` hizmetinin `OperationId` istemci isteği içindeki `OperationService` ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-276">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="6d2e1-277">İstemci istekleri arasında her iki hizmet de farklı bir `OperationId` değeri paylaşır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-277">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="6d2e1-278">Tek ve tek örnekli hizmetler bir kez oluşturulduğunda ve tüm istemci isteklerinde ve tüm hizmetlerde kullanıldığında, `OperationId` tüm hizmet istekleri arasında sabittir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-278">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6d2e1-279">`Startup.ConfigureServices`, her tür kapsayıcıya, adlandırılmış ömrüne göre eklenir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-279">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

::: moniker-end

<span data-ttu-id="6d2e1-280">`IOperationSingletonInstance` hizmeti, bilinen bir `Guid.Empty`KIMLIĞIYLE belirli bir örnek kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-280">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="6d2e1-281">Bu tür kullanımda olduğunda (GUID 'sinin tümü sıfırlardan tamamen) Bu bir şey vardır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-281">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="6d2e1-282">Örnek uygulama, bireysel istekler içindeki ve içindeki nesne yaşam sürelerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-282">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="6d2e1-283">Örnek uygulamanın `IndexModel` her tür `IOperation` türü ve `OperationService`ister.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-283">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="6d2e1-284">Daha sonra sayfa, tüm sayfa modeli sınıfının ve hizmetin `OperationId` değerlerini özellik atamaları aracılığıyla görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-284">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

::: moniker-end

<span data-ttu-id="6d2e1-285">Aşağıdaki iki çıktıda iki isteğin sonuçları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-285">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="6d2e1-286">**İlk istek:**</span><span class="sxs-lookup"><span data-stu-id="6d2e1-286">**First request:**</span></span>

<span data-ttu-id="6d2e1-287">Denetleyici işlemleri:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-287">Controller operations:</span></span>

<span data-ttu-id="6d2e1-288">Geçici: d233e165-f417-469B-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="6d2e1-288">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="6d2e1-289">Kapsam: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="6d2e1-289">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="6d2e1-290">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="6d2e1-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="6d2e1-291">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="6d2e1-291">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="6d2e1-292">`OperationService` işlemler:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-292">`OperationService` operations:</span></span>

<span data-ttu-id="6d2e1-293">Geçici: c6b049eb-1318-4E31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="6d2e1-293">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="6d2e1-294">Kapsam: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="6d2e1-294">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="6d2e1-295">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="6d2e1-295">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="6d2e1-296">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="6d2e1-296">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="6d2e1-297">**İkinci istek:**</span><span class="sxs-lookup"><span data-stu-id="6d2e1-297">**Second request:**</span></span>

<span data-ttu-id="6d2e1-298">Denetleyici işlemleri:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-298">Controller operations:</span></span>

<span data-ttu-id="6d2e1-299">Geçici: b63bd538-0a37-4FF1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="6d2e1-299">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="6d2e1-300">Kapsam: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="6d2e1-300">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="6d2e1-301">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="6d2e1-301">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="6d2e1-302">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="6d2e1-302">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="6d2e1-303">`OperationService` işlemler:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-303">`OperationService` operations:</span></span>

<span data-ttu-id="6d2e1-304">Geçici: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="6d2e1-304">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="6d2e1-305">Kapsam: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="6d2e1-305">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="6d2e1-306">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="6d2e1-306">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="6d2e1-307">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="6d2e1-307">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="6d2e1-308">`OperationId` değerlerinden hangisinin bir istek içinde ve istekler arasında değiştiğini gözlemleyin:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-308">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="6d2e1-309">*Geçici* nesneler her zaman farklıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-309">*Transient* objects are always different.</span></span> <span data-ttu-id="6d2e1-310">Hem birinci hem de ikinci istemci isteklerinin geçici `OperationId` değeri hem `OperationService` işlemleri hem de istemci istekleri için farklıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-310">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="6d2e1-311">Her hizmet isteğine ve istemci isteğine yeni bir örnek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-311">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="6d2e1-312">*Kapsamlı* nesneler istemci isteği içinde aynıdır ancak istemci istekleri arasında farklıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-312">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="6d2e1-313">*Tek* nesneler her nesne için aynıdır ve `Startup.ConfigureServices`bir `Operation` örneğinin sağlanmadığına bakılmaksızın her istek vardır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-313">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="6d2e1-314">Ana bilgisayardan Hizmetleri çağır</span><span class="sxs-lookup"><span data-stu-id="6d2e1-314">Call services from main</span></span>

<span data-ttu-id="6d2e1-315">Uygulamanın kapsamındaki bir kapsamlı hizmeti çözümlemek için [ıvicescopefactory. CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) ile bir <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-315">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="6d2e1-316">Bu yaklaşım, başlatma görevlerini çalıştırmak üzere başlangıçta kapsamlı bir hizmete erişmek için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-316">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="6d2e1-317">Aşağıdaki örnek, `Program.Main``MyScopedService` için nasıl bağlam alınacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-317">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static async Task Main(string[] args)
    {
        var host = CreateHostBuilder(args).Build();

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
    
        await host.RunAsync();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            });
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

public class Program
{
    public static async Task Main(string[] args)
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
    
        await host.RunAsync();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

::: moniker-end

## <a name="scope-validation"></a><span data-ttu-id="6d2e1-318">Kapsam doğrulaması</span><span class="sxs-lookup"><span data-stu-id="6d2e1-318">Scope validation</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6d2e1-319">Uygulama geliştirme ortamında çalışırken ve Konağı derlemek için [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) çağırdığında, varsayılan hizmet sağlayıcı aşağıdakileri doğrulamak için denetimler gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-319">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) to build the host, the default service provider performs checks to verify that:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6d2e1-320">Uygulama geliştirme ortamında çalışırken ve Konağı derlemek için [Createdefaultbuilder](xref:fundamentals/host/web-host#set-up-a-host) çağırdığında, varsayılan hizmet sağlayıcı aşağıdakileri doğrulamak için denetimler gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-320">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) to build the host, the default service provider performs checks to verify that:</span></span>

::: moniker-end

* <span data-ttu-id="6d2e1-321">Kapsamlı hizmetler doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-321">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="6d2e1-322">Kapsamlı hizmetler doğrudan veya dolaylı olarak Singleton 'a eklenmiş değildir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-322">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="6d2e1-323"><xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> çağrıldığında kök hizmet sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-323">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="6d2e1-324">Kök hizmet sağlayıcısının ömrü, sağlayıcının uygulamayla başladığı ve uygulama kapandığında bırakıldığı uygulama/sunucunun yaşam süresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-324">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="6d2e1-325">Kapsamlı hizmetler kendilerini oluşturan kapsayıcı tarafından atılmış.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-325">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="6d2e1-326">Kök kapsayıcıda kapsamlı bir hizmet oluşturulduysa, hizmetin ömrü etkin şekilde tek başına yükseltilir çünkü yalnızca uygulama/sunucu kapatıldığında kök kapsayıcı tarafından atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-326">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="6d2e1-327">Hizmet kapsamlarını doğrulamak `BuildServiceProvider` çağrıldığında bu durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-327">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="6d2e1-328">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-328">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="6d2e1-329">İstek Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="6d2e1-329">Request Services</span></span>

<span data-ttu-id="6d2e1-330">`HttpContext` bir ASP.NET Core isteği içinde kullanılabilen hizmetler, [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) koleksiyonu aracılığıyla sunulur.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-330">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="6d2e1-331">İstek Hizmetleri, uygulamanın bir parçası olarak yapılandırılan ve istenen hizmetleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-331">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="6d2e1-332">Nesneler bağımlılıklar belirttiğinizde, bunlar `ApplicationServices`değil `RequestServices`bulunan türler tarafından karşılanır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-332">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="6d2e1-333">Genellikle, uygulamanın bu özellikleri doğrudan kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-333">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="6d2e1-334">Bunun yerine, sınıfların Sınıf oluşturucuları aracılığıyla gerektirdiği türleri isteyin ve çerçevenin bağımlılıkları eklemesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-334">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="6d2e1-335">Bu, test etmek daha kolay olan sınıfları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-335">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="6d2e1-336">`RequestServices` koleksiyonuna erişmek için Oluşturucu parametreleri olarak bağımlılıklar istemeyi tercih edin.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-336">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="6d2e1-337">Bağımlılık ekleme için tasarım hizmetleri</span><span class="sxs-lookup"><span data-stu-id="6d2e1-337">Design services for dependency injection</span></span>

<span data-ttu-id="6d2e1-338">En iyi uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-338">Best practices are to:</span></span>

* <span data-ttu-id="6d2e1-339">Bağımlılıklarını almak için bağımlılık ekleme 'yi kullanmak üzere Hizmetleri tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-339">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="6d2e1-340">Durum bilgisi olan statik sınıflar ve Üyeler kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-340">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="6d2e1-341">Genel durum oluşturulmasını önlemek yerine, tek tek Hizmetleri kullanmak için uygulamaları tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-341">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="6d2e1-342">Hizmetler içindeki bağımlı sınıfların doğrudan örneklenmesini önleyin.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-342">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="6d2e1-343">Doğrudan örnekleme kodu belirli bir uygulamaya bağar.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-343">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="6d2e1-344">Uygulama sınıflarını küçük, iyi bir şekilde ve kolayca test edin.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-344">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="6d2e1-345">Bir sınıfta çok fazla sayıda bağımlılık varsa, bu genellikle sınıfta çok fazla sorumluluk olduğu ve [tek sorumluluk ilkesini (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility)ihlal eden bir imzadır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-345">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="6d2e1-346">Bazı sorumlulukları yeni bir sınıfa taşıyarak sınıfı yeniden düzenleme girişimi.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-346">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="6d2e1-347">Razor Pages sayfa modeli sınıfları ve MVC denetleyici sınıflarının UI kaygılarıyla odaklanıp ilgilenmeyeceğini aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-347">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="6d2e1-348">İş kuralları ve veri erişimi uygulama ayrıntıları, bu [ayrı kaygılara](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)uygun sınıflarda tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-348">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="6d2e1-349">Hizmetlerin elden çıkarılması</span><span class="sxs-lookup"><span data-stu-id="6d2e1-349">Disposal of services</span></span>

<span data-ttu-id="6d2e1-350">Kapsayıcı, oluşturduğu <xref:System.IDisposable> türleri için <xref:System.IDisposable.Dispose*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-350">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="6d2e1-351">Kapsayıcıda Kullanıcı kodu tarafından bir örnek eklenirse, otomatik olarak atılamaz.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-351">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="6d2e1-352">Varsayılan hizmet kapsayıcısı değiştirme</span><span class="sxs-lookup"><span data-stu-id="6d2e1-352">Default service container replacement</span></span>

<span data-ttu-id="6d2e1-353">Yerleşik hizmet kapsayıcısı, çerçeve ihtiyaçlarına ve çoğu tüketici uygulamasına hizmet vermek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-353">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="6d2e1-354">Yerleşik kapsayıcının desteklemediği belirli bir özelliğe ihtiyacınız yoksa, yerleşik kapsayıcının kullanılması önerilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-354">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="6d2e1-355">Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="6d2e1-355">Property injection</span></span>
* <span data-ttu-id="6d2e1-356">Ada göre ekleme</span><span class="sxs-lookup"><span data-stu-id="6d2e1-356">Injection based on name</span></span>
* <span data-ttu-id="6d2e1-357">Alt kapsayıcılar</span><span class="sxs-lookup"><span data-stu-id="6d2e1-357">Child containers</span></span>
* <span data-ttu-id="6d2e1-358">Özel ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="6d2e1-358">Custom lifetime management</span></span>
* <span data-ttu-id="6d2e1-359">yavaş başlatma için `Func<T>` desteği</span><span class="sxs-lookup"><span data-stu-id="6d2e1-359">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="6d2e1-360">Kural tabanlı kayıt</span><span class="sxs-lookup"><span data-stu-id="6d2e1-360">Convention-based registration</span></span>

<span data-ttu-id="6d2e1-361">Aşağıdaki 3. taraf kapsayıcıları ASP.NET Core uygulamalarla kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-361">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="6d2e1-362">Autofac</span><span class="sxs-lookup"><span data-stu-id="6d2e1-362">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="6d2e1-363">Drıioc</span><span class="sxs-lookup"><span data-stu-id="6d2e1-363">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="6d2e1-364">Yetkisiz kullanım</span><span class="sxs-lookup"><span data-stu-id="6d2e1-364">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="6d2e1-365">Açık Inject</span><span class="sxs-lookup"><span data-stu-id="6d2e1-365">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="6d2e1-366">E</span><span class="sxs-lookup"><span data-stu-id="6d2e1-366">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="6d2e1-367">Stakıbox</span><span class="sxs-lookup"><span data-stu-id="6d2e1-367">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="6d2e1-368">Unity</span><span class="sxs-lookup"><span data-stu-id="6d2e1-368">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="6d2e1-369">İş parçacığı güvenliği</span><span class="sxs-lookup"><span data-stu-id="6d2e1-369">Thread safety</span></span>

<span data-ttu-id="6d2e1-370">İş parçacığı güvenli Singleton Hizmetleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-370">Create thread-safe singleton services.</span></span> <span data-ttu-id="6d2e1-371">Tek bir hizmetin geçici bir hizmete bağımlılığı varsa, geçici hizmet aynı zamanda tek tek tarafından nasıl kullanıldığına bağlı olarak iş parçacığı güvenliği de gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-371">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="6d2e1-372">Tek bir hizmetin fabrika yöntemi (örneğin, [AddSingleton\<TService > (ısevicecollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), iş parçacığı açısından güvenli olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-372">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="6d2e1-373">Bir tür (`static`) Oluşturucusu gibi, tek bir iş parçacığı tarafından bir kez çağrılması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-373">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="6d2e1-374">Öneriler</span><span class="sxs-lookup"><span data-stu-id="6d2e1-374">Recommendations</span></span>

* <span data-ttu-id="6d2e1-375">`async/await` ve `Task` tabanlı hizmet çözümlemesi desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-375">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="6d2e1-376">C#zaman uyumsuz oluşturucuları desteklemez; Bu nedenle, önerilen model hizmeti zaman uyumlu olarak çözümledikten sonra zaman uyumsuz yöntemler kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-376">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="6d2e1-377">Veri ve yapılandırmayı doğrudan hizmet kapsayıcısında saklamaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-377">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="6d2e1-378">Örneğin, bir kullanıcının alışveriş sepeti genellikle hizmet kapsayıcısına eklenmemelidir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-378">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="6d2e1-379">Yapılandırma, [Seçenekler modelini](xref:fundamentals/configuration/options)kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-379">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="6d2e1-380">Benzer şekilde, yalnızca başka bir nesneye erişime izin vermek için mevcut olan "veri sahibi" nesnelerinden kaçının.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-380">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="6d2e1-381">DI aracılığıyla gerçek öğe istemek daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-381">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="6d2e1-382">Hizmetlere statik erişimi önleyin (örneğin, başka bir yerde kullanmak üzere, statik olarak yazılan [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) ).</span><span class="sxs-lookup"><span data-stu-id="6d2e1-382">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="6d2e1-383">*Hizmet bulucu deseninin*kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-383">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="6d2e1-384">Örneğin, yerine şunu kullandığınızda bir hizmet örneği elde etmek için <xref:System.IServiceProvider.GetService*> çağırmayın:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-384">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="6d2e1-385">**Olmayan**</span><span class="sxs-lookup"><span data-stu-id="6d2e1-385">**Incorrect:**</span></span>

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

  <span data-ttu-id="6d2e1-386">**Doğru**:</span><span class="sxs-lookup"><span data-stu-id="6d2e1-386">**Correct**:</span></span>

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

* <span data-ttu-id="6d2e1-387">Önlemek için başka bir hizmet bulucu çeşitlemesi, çalışma zamanında bağımlılıkları çözümleyen bir ekleme.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-387">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="6d2e1-388">Bu uygulamalardan her ikisi de [Denetim stratejilerini geçersiz kılar](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .</span><span class="sxs-lookup"><span data-stu-id="6d2e1-388">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="6d2e1-389">`HttpContext` statik erişimden kaçının (örneğin, [ıhttpcontextaccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="6d2e1-389">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="6d2e1-390">Tüm öneri kümeleri gibi, bir öneriyi yok saymayı yok saymış durumlarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-390">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="6d2e1-391">Özel durumlar genellikle Framework içindeki özel durumlar&mdash;nadir olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-391">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="6d2e1-392">Dı, statik/genel nesne erişim desenlerinin bir *alternatifidir* .</span><span class="sxs-lookup"><span data-stu-id="6d2e1-392">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="6d2e1-393">Statik nesne erişimi ile karıştırırsanız, dı 'nin avantajlarını fark edemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6d2e1-393">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6d2e1-394">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="6d2e1-394">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="6d2e1-395">Bağımlılık ekleme (MSDN) ile ASP.NET Core temizleme kodu yazma</span><span class="sxs-lookup"><span data-stu-id="6d2e1-395">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="6d2e1-396">Açık bağımlılıklar Ilkesi</span><span class="sxs-lookup"><span data-stu-id="6d2e1-396">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="6d2e1-397">Denetim kapsayıcıları ve bağımlılık ekleme deseninin Inversion 'ı (Marwler)</span><span class="sxs-lookup"><span data-stu-id="6d2e1-397">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="6d2e1-398">ASP.NET Core DI 'de birden çok arabirime sahip bir hizmeti kaydetme</span><span class="sxs-lookup"><span data-stu-id="6d2e1-398">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
