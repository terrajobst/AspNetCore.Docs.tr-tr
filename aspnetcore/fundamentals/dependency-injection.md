---
title: ASP.NET Core bağımlılık ekleme
author: rick-anderson
description: ASP.NET Core bağımlılık ekleme ve nasıl kullanılacağı hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
uid: fundamentals/dependency-injection
ms.openlocfilehash: d24807d1ea675448536ab865d41ae46f80043666
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306515"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="50c6a-103">ASP.NET Core bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="50c6a-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="50c6a-104">, [Steve Smith](https://ardalis.com/) ve [Scott Ade](https://scottaddie.com) tarafından</span><span class="sxs-lookup"><span data-stu-id="50c6a-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="50c6a-105">ASP.NET Core, sınıflar ve bunların bağımlılıkları arasında [denetimin INVERSION (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) elde etmek için bir teknik olan bağımlılık ekleme (dı) yazılım tasarım modelini destekler.</span><span class="sxs-lookup"><span data-stu-id="50c6a-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="50c6a-106">MVC denetleyicileri içindeki bağımlılık eklenmesine özgü daha fazla bilgi için bkz. <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="50c6a-107">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="50c6a-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="50c6a-108">Bağımlılık eklenmesine genel bakış</span><span class="sxs-lookup"><span data-stu-id="50c6a-108">Overview of dependency injection</span></span>

<span data-ttu-id="50c6a-109">*Bağımlılık* , başka bir nesnenin gerektirdiği herhangi bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="50c6a-110">Aşağıdaki `MyDependency` sınıfını bir uygulamadaki diğer sınıfların bağlı olduğu bir `WriteMessage` yöntemiyle inceleyin:</span><span class="sxs-lookup"><span data-stu-id="50c6a-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="50c6a-111">`WriteMessage` yöntemini bir sınıf için kullanılabilir hale getirmek için `MyDependency` sınıfının bir örneği oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="50c6a-112">`MyDependency` sınıfı, `IndexModel` sınıfının bir bağımlılığı olur:</span><span class="sxs-lookup"><span data-stu-id="50c6a-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="50c6a-113">Sınıf oluşturur ve doğrudan `MyDependency` örneğine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="50c6a-114">Kod bağımlılıkları (önceki örnekte olduğu gibi) sorunlu olur ve aşağıdaki nedenlerden dolayı kaçınılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="50c6a-115">`MyDependency` farklı bir uygulamayla değiştirmek için, sınıfın değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="50c6a-116">`MyDependency` bağımlılıklar içeriyorsa, sınıfı tarafından yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="50c6a-117">`MyDependency`bağlı olarak, birden çok sınıfa sahip büyük bir projede yapılandırma kodu uygulama genelinde dağılmış hale gelir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="50c6a-118">Bu uygulamanın birim testi zordur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="50c6a-119">Uygulama, bu yaklaşımla mümkün olmayan bir sahte veya saplama `MyDependency` sınıfı kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="50c6a-120">Bağımlılık ekleme bu sorunları şu şekilde giderir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="50c6a-121">Bağımlılık uygulamasını soyutlamak için bir arabirim veya temel sınıf kullanımı.</span><span class="sxs-lookup"><span data-stu-id="50c6a-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="50c6a-122">Bir hizmet kapsayıcısına bağımlılığın kaydı.</span><span class="sxs-lookup"><span data-stu-id="50c6a-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="50c6a-123">ASP.NET Core yerleşik bir hizmet kapsayıcısı sağlar <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="50c6a-124">Hizmetler, uygulamanın `Startup.ConfigureServices` yöntemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="50c6a-125">Hizmetin kullanıldığı sınıf oluşturucusuna *ekleme* .</span><span class="sxs-lookup"><span data-stu-id="50c6a-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="50c6a-126">Çerçeve, bağımlılığın bir örneğini oluşturma ve artık gerekli olmadığında bu uygulamayı atma sorumluluğunu alır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="50c6a-127">[Örnek uygulamada](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` arabirimi hizmetin uygulamaya sağladığı bir yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="50c6a-127">In the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="50c6a-128">Bu arabirim somut bir tür tarafından uygulanır, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="50c6a-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="50c6a-129">`MyDependency` kurucusunda bir <xref:Microsoft.Extensions.Logging.ILogger`1> ister.</span><span class="sxs-lookup"><span data-stu-id="50c6a-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="50c6a-130">Bağımlılık ekleme işlemini zincirleme bir biçimde kullanmak olağan dışı değildir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="50c6a-131">Her istenen bağımlılık, kendi bağımlılıklarını ister.</span><span class="sxs-lookup"><span data-stu-id="50c6a-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="50c6a-132">Kapsayıcı grafikteki bağımlılıkları çözer ve tamamen çözümlenen hizmeti döndürür.</span><span class="sxs-lookup"><span data-stu-id="50c6a-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="50c6a-133">Çözümlenmesi gereken, genellikle *bağımlılık ağacı*, *bağımlılık grafiği*veya *nesne grafiği*olarak adlandırılan toplu bağımlılıklar kümesi.</span><span class="sxs-lookup"><span data-stu-id="50c6a-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="50c6a-134">`IMyDependency` ve `ILogger<TCategoryName>`, hizmet kapsayıcısında kayıtlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="50c6a-135">`IMyDependency` `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="50c6a-136">`ILogger<TCategoryName>`, günlük soyut öğeler altyapısı tarafından kaydedilir. bu nedenle, Framework tarafından varsayılan olarak kaydedilen [Framework tarafından sağlanmış bir hizmettir](#framework-provided-services) .</span><span class="sxs-lookup"><span data-stu-id="50c6a-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="50c6a-137">Kapsayıcı, [(genel) açık türlerden](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types)yararlanarak `ILogger<TCategoryName>` çözer, her [(genel) oluşturulan türü](/dotnet/csharp/language-reference/language-specification/types#constructed-types)kaydetme ihtiyacını ortadan kaldırır:</span><span class="sxs-lookup"><span data-stu-id="50c6a-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="50c6a-138">Örnek uygulamada `IMyDependency` hizmeti somut tür `MyDependency`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="50c6a-139">Kayıt, hizmet ömrünü tek bir isteğin kullanım ömrüne göre kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="50c6a-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="50c6a-140">[Hizmet yaşam süreleri](#service-lifetimes) bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="50c6a-141">Her `services.Add{SERVICE_NAME}` uzantısı yöntemi Hizmetleri ekler (ve potansiyel olarak yapılandırır).</span><span class="sxs-lookup"><span data-stu-id="50c6a-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="50c6a-142">Örneğin `services.AddMvc()`, Razor Pages ve MVC 'nin gerektirdiği Hizmetleri ekler.</span><span class="sxs-lookup"><span data-stu-id="50c6a-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="50c6a-143">Uygulamaların bu kuralı izlemesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="50c6a-144">Hizmet kaydı gruplarını kapsüllemek için uzantı yöntemlerini [Microsoft. Extensions. Dependencyınjection](/dotnet/api/microsoft.extensions.dependencyinjection) ad alanına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="50c6a-145">Hizmetin Oluşturucusu `string`gibi [yerleşik bir tür](/dotnet/csharp/language-reference/keywords/built-in-types-table)gerektiriyorsa, tür [yapılandırma](xref:fundamentals/configuration/index) veya [Seçenekler düzeniyle](xref:fundamentals/configuration/options)eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="50c6a-146">Hizmetin bir örneği, hizmetin kullanıldığı ve özel bir alana atandığı bir sınıfın Oluşturucusu aracılığıyla istenir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="50c6a-147">Alanı, sınıfına gereken şekilde hizmete erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="50c6a-148">Örnek uygulamada, `IMyDependency` örneği istenir ve hizmetin `WriteMessage` yöntemini çağırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="50c6a-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="50c6a-149">Başlangıca eklenen hizmetler</span><span class="sxs-lookup"><span data-stu-id="50c6a-149">Services injected into Startup</span></span>

<span data-ttu-id="50c6a-150">Genel ana bilgisayar (<xref:Microsoft.Extensions.Hosting.IHostBuilder>) kullanılırken `Startup` oluşturucusuna yalnızca aşağıdaki hizmet türleri eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-150">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="50c6a-151">Hizmetler `Startup.Configure`eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-151">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="50c6a-152">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-152">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="50c6a-153">Framework tarafından sunulan hizmetler</span><span class="sxs-lookup"><span data-stu-id="50c6a-153">Framework-provided services</span></span>

<span data-ttu-id="50c6a-154">`Startup.ConfigureServices` yöntemi, uygulamanın kullandığı hizmetlerin (Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri de dahil) tanımlanmasından sorumludur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-154">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="50c6a-155">Başlangıçta, `ConfigureServices` için belirtilen `IServiceCollection` [konağın nasıl yapılandırıldığına](xref:fundamentals/index#host)bağlı olarak Framework tarafından tanımlanan hizmetlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-155">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="50c6a-156">Çerçeve tarafından kaydedilmiş yüzlerce hizmete sahip olmak ASP.NET Core şablona dayalı bir uygulama için sık görülen bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-156">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="50c6a-157">Aşağıdaki tabloda çerçeve kayıtlı hizmetlerden oluşan küçük bir örnek listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-157">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="50c6a-158">Hizmet Türü</span><span class="sxs-lookup"><span data-stu-id="50c6a-158">Service Type</span></span> | <span data-ttu-id="50c6a-159">Ömür</span><span class="sxs-lookup"><span data-stu-id="50c6a-159">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="50c6a-160">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-160">Transient</span></span> |
| `IHostApplicationLifetime` | <span data-ttu-id="50c6a-161">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-161">Singleton</span></span> |
| `IWebHostEnvironment` | <span data-ttu-id="50c6a-162">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-162">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="50c6a-163">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-163">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="50c6a-164">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-164">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="50c6a-165">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-165">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="50c6a-166">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-166">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="50c6a-167">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-167">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="50c6a-168">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-168">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="50c6a-169">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-169">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="50c6a-170">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-170">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="50c6a-171">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-171">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="50c6a-172">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-172">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="50c6a-173">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-173">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="50c6a-174">Uzantı yöntemleriyle ek hizmetleri kaydetme</span><span class="sxs-lookup"><span data-stu-id="50c6a-174">Register additional services with extension methods</span></span>

<span data-ttu-id="50c6a-175">Bir hizmet koleksiyonu genişletme yöntemi (ve gerekirse bağımlı hizmetleri) kaydetmek için kullanılabilir olduğunda, bu hizmet için gereken tüm hizmetleri kaydetmek üzere tek bir `Add{SERVICE_NAME}` uzantısı yöntemi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-175">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="50c6a-176">Aşağıdaki kod, [Adddbcontext\<tcontext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) ve <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>uzantı yöntemlerini kullanarak kapsayıcıya ek hizmetler eklemenin bir örneğidir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-176">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

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

<span data-ttu-id="50c6a-177">Daha fazla bilgi için API belgelerindeki <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> sınıfına bakın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-177">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="50c6a-178">Hizmet yaşam süreleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-178">Service lifetimes</span></span>

<span data-ttu-id="50c6a-179">Kayıtlı her hizmet için uygun bir yaşam süresi seçin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-179">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="50c6a-180">ASP.NET Core hizmetler aşağıdaki yaşam süreleri ile yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-180">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="50c6a-181">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-181">Transient</span></span>

<span data-ttu-id="50c6a-182">Geçici ömür Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>), hizmet kapsayıcısından her istenilişinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-182">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="50c6a-183">Bu ömür, hafif ve durumsuz hizmetler için en iyi şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-183">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="50c6a-184">Yayıl</span><span class="sxs-lookup"><span data-stu-id="50c6a-184">Scoped</span></span>

<span data-ttu-id="50c6a-185">Kapsamlı ömür Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>), istemci isteği başına bir kez oluşturulur (bağlantı).</span><span class="sxs-lookup"><span data-stu-id="50c6a-185">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="50c6a-186">Bir ara yazılım içinde kapsamlı bir hizmet kullanırken, hizmeti `Invoke` veya `InvokeAsync` yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-186">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="50c6a-187">Oluşturucu ekleme yoluyla ekleme, hizmeti tek bir gibi davranmaya zoryor.</span><span class="sxs-lookup"><span data-stu-id="50c6a-187">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="50c6a-188">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-188">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="50c6a-189">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-189">Singleton</span></span>

<span data-ttu-id="50c6a-190">Tek yaşam süresi Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>), ilk istendiğinde oluşturulur (veya `Startup.ConfigureServices` çalıştırıldığında ve hizmet kaydıyla bir örnek belirtildiğinde).</span><span class="sxs-lookup"><span data-stu-id="50c6a-190">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="50c6a-191">Her sonraki istek aynı örneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-191">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="50c6a-192">Uygulama tek davranış gerektiriyorsa, hizmet kapsayıcısının hizmetin ömrünü yönetmesine izin verilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-192">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="50c6a-193">Tekil tasarım modelini uygulamayın ve nesnenin sınıfındaki ömrünü yönetmek için Kullanıcı kodu sağlayın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-193">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="50c6a-194">Kapsamlı bir hizmetin tek bir bilgisayardan çözümlenmesi tehlikelidir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-194">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="50c6a-195">Bu, sonraki istekleri işlerken hizmetin yanlış duruma gelmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-195">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="50c6a-196">Hizmet kayıt yöntemleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-196">Service registration methods</span></span>

<span data-ttu-id="50c6a-197">Hizmet kayıt uzantısı yöntemleri, belirli senaryolarda yararlı olan aşırı yüklemeler sunar.</span><span class="sxs-lookup"><span data-stu-id="50c6a-197">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="50c6a-198">Yöntem</span><span class="sxs-lookup"><span data-stu-id="50c6a-198">Method</span></span> | <span data-ttu-id="50c6a-199">Otomatik</span><span class="sxs-lookup"><span data-stu-id="50c6a-199">Automatic</span></span><br><span data-ttu-id="50c6a-200">nesne</span><span class="sxs-lookup"><span data-stu-id="50c6a-200">object</span></span><br><span data-ttu-id="50c6a-201">elden</span><span class="sxs-lookup"><span data-stu-id="50c6a-201">disposal</span></span> | <span data-ttu-id="50c6a-202">Birden Çok</span><span class="sxs-lookup"><span data-stu-id="50c6a-202">Multiple</span></span><br><span data-ttu-id="50c6a-203">uygulamalar</span><span class="sxs-lookup"><span data-stu-id="50c6a-203">implementations</span></span> | <span data-ttu-id="50c6a-204">Geçiş bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-204">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="50c6a-205">Örnek:</span><span class="sxs-lookup"><span data-stu-id="50c6a-205">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="50c6a-206">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-206">Yes</span></span> | <span data-ttu-id="50c6a-207">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-207">Yes</span></span> | <span data-ttu-id="50c6a-208">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-208">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="50c6a-209">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-209">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="50c6a-210">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-210">Yes</span></span> | <span data-ttu-id="50c6a-211">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-211">Yes</span></span> | <span data-ttu-id="50c6a-212">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-212">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="50c6a-213">Örnek:</span><span class="sxs-lookup"><span data-stu-id="50c6a-213">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="50c6a-214">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-214">Yes</span></span> | <span data-ttu-id="50c6a-215">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-215">No</span></span> | <span data-ttu-id="50c6a-216">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-216">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="50c6a-217">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-217">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="50c6a-218">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-218">No</span></span> | <span data-ttu-id="50c6a-219">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-219">Yes</span></span> | <span data-ttu-id="50c6a-220">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-220">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="50c6a-221">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-221">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="50c6a-222">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-222">No</span></span> | <span data-ttu-id="50c6a-223">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-223">No</span></span> | <span data-ttu-id="50c6a-224">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-224">Yes</span></span> |

<span data-ttu-id="50c6a-225">Tür çıkarma hakkında daha fazla bilgi için [Hizmetler 'In aktiften çıkarılması](#disposal-of-services) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-225">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="50c6a-226">Birden çok uygulama için yaygın bir senaryo, [test için bir sahte işlem türüdür](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="50c6a-226">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="50c6a-227">`TryAdd{LIFETIME}` Yöntemler, zaten kayıtlı bir uygulama yoksa hizmeti kaydeder.</span><span class="sxs-lookup"><span data-stu-id="50c6a-227">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="50c6a-228">Aşağıdaki örnekte, ilk satır `IMyDependency`için `MyDependency` kaydettirir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-228">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="50c6a-229">`IMyDependency` zaten kayıtlı bir uygulamaya sahip olduğundan ikinci satır etkisizdir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-229">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="50c6a-230">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="50c6a-230">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="50c6a-231">[TryAddEnumerable (ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) yöntemleri yalnızca *aynı türde*bir uygulama yoksa hizmeti kaydeder.</span><span class="sxs-lookup"><span data-stu-id="50c6a-231">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="50c6a-232">Birden çok hizmet `IEnumerable<{SERVICE}>`ile çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-232">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="50c6a-233">Hizmetleri kaydederken, geliştirici yalnızca aynı türden biri zaten eklenmediyse bir örnek eklemek istemektedir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-233">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="50c6a-234">Genellikle, bu yöntem, kapsayıcıda bir örneğin iki kopyasını kaydetmemek için kitaplık yazarları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-234">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="50c6a-235">Aşağıdaki örnekte, ilk satır `IMyDep1`için `MyDep` kaydettirir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-235">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="50c6a-236">İkinci satır, `IMyDep2`için `MyDep` kaydeder.</span><span class="sxs-lookup"><span data-stu-id="50c6a-236">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="50c6a-237">`IMyDep1` `MyDep`kayıtlı bir uygulamasına zaten sahip olduğundan, üçüncü satırın etkisi yoktur:</span><span class="sxs-lookup"><span data-stu-id="50c6a-237">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="50c6a-238">Oluşturucu Ekleme davranışı</span><span class="sxs-lookup"><span data-stu-id="50c6a-238">Constructor injection behavior</span></span>

<span data-ttu-id="50c6a-239">Hizmetler, iki mekanizma tarafından çözülebilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-239">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="50c6a-240"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash;, bağımlılık ekleme kapsayıcısına hizmet kaydı olmadan nesne oluşturulmasına Izin verir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-240"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="50c6a-241">`ActivatorUtilities` etiket yardımcıları, MVC denetleyicileri ve model ciltler gibi kullanıcı tarafından ilgili soyutlamalar ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-241">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="50c6a-242">Oluşturucular bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bağımsız değişkenlerin varsayılan değerleri ataması gerekir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-242">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="50c6a-243">Hizmetler `IServiceProvider` veya `ActivatorUtilities`tarafından çözümlendiğinde, Oluşturucu Ekleme *ortak* bir Oluşturucu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-243">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="50c6a-244">Hizmetler `ActivatorUtilities`tarafından çözümlendiğinde, Oluşturucu ekleme yalnızca bir adet geçerli oluşturucunun var olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-244">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="50c6a-245">Oluşturucu aşırı yüklemeleri desteklenir, ancak bağımsız değişkenleri bağımlılık ekleme tarafından yerine yalnızca bir aşırı yükleme bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-245">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="50c6a-246">Entity Framework bağlamları</span><span class="sxs-lookup"><span data-stu-id="50c6a-246">Entity Framework contexts</span></span>

<span data-ttu-id="50c6a-247">Entity Framework bağlamlar genellikle, Web uygulaması veritabanı işlemleri normalde istemci isteği kapsamında olduğundan [kapsamlı ömür](#service-lifetimes) kullanılarak hizmet kapsayıcısına eklenir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-247">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="50c6a-248">Veritabanı bağlamı kaydedilirken bir [Adddbcontext\<tcontext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) aşırı yüklemesi tarafından bir yaşam süresi belirtilmemişse varsayılan yaşam süresi kapsamındadır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-248">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="50c6a-249">Belirli bir yaşam süresinin Hizmetleri, hizmetten daha kısa bir yaşam süresine sahip bir veritabanı bağlamı kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-249">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="50c6a-250">Ömür ve kayıt seçenekleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-250">Lifetime and registration options</span></span>

<span data-ttu-id="50c6a-251">Ömür ve kayıt seçenekleri arasındaki farkı göstermek için, görevleri benzersiz bir tanımlayıcıya sahip bir işlem olarak temsil eden aşağıdaki arayüzleri göz önünde bulundurun `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="50c6a-251">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="50c6a-252">Bir işlem hizmetinin yaşam süresinin aşağıdaki arabirimler için nasıl yapılandırıldığına bağlı olarak kapsayıcı, bir sınıf tarafından istendiğinde aynı ya da farklı bir hizmet örneği sağlar:</span><span class="sxs-lookup"><span data-stu-id="50c6a-252">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="50c6a-253">Arabirimler `Operation` sınıfında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-253">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="50c6a-254">`Operation` Oluşturucusu bir GUID sağlanmamışsa bir GUID oluşturur:</span><span class="sxs-lookup"><span data-stu-id="50c6a-254">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="50c6a-255">Diğer `Operation` türlerinin her birine bağlı olan bir `OperationService` kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-255">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="50c6a-256">Bağımlılık ekleme yoluyla `OperationService` istendiğinde, her bir hizmetin yeni bir örneğini ya da bağımlı hizmetin kullanım ömrü temelinde mevcut bir örneği alır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-256">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="50c6a-257">Kapsayıcıda istendiğinde geçici hizmetler oluşturulduğunda, `IOperationTransient` hizmetinin `OperationId` `OperationService``OperationId` farklıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-257">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="50c6a-258">`OperationService`, `IOperationTransient` sınıfının yeni bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-258">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="50c6a-259">Yeni örnek farklı bir `OperationId`verir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-259">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="50c6a-260">İstemci isteği başına kapsamlı hizmetler oluşturulduğunda, `IOperationScoped` hizmetinin `OperationId` istemci isteği içindeki `OperationService` ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-260">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="50c6a-261">İstemci istekleri arasında her iki hizmet de farklı bir `OperationId` değeri paylaşır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-261">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="50c6a-262">Tek ve tek örnekli hizmetler bir kez oluşturulduğunda ve tüm istemci isteklerinde ve tüm hizmetlerde kullanıldığında, `OperationId` tüm hizmet istekleri arasında sabittir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-262">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="50c6a-263">`Startup.ConfigureServices`, her tür kapsayıcıya, adlandırılmış ömrüne göre eklenir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-263">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="50c6a-264">`IOperationSingletonInstance` hizmeti, bilinen bir `Guid.Empty`KIMLIĞIYLE belirli bir örnek kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="50c6a-264">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="50c6a-265">Bu tür kullanımda olduğunda (GUID 'sinin tümü sıfırlardan tamamen) Bu bir şey vardır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-265">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="50c6a-266">Örnek uygulama, bireysel istekler içindeki ve içindeki nesne yaşam sürelerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-266">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="50c6a-267">Örnek uygulamanın `IndexModel` her tür `IOperation` türü ve `OperationService`ister.</span><span class="sxs-lookup"><span data-stu-id="50c6a-267">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="50c6a-268">Daha sonra sayfa, tüm sayfa modeli sınıfının ve hizmetin `OperationId` değerlerini özellik atamaları aracılığıyla görüntüler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-268">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/3.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="50c6a-269">Aşağıdaki iki çıktıda iki isteğin sonuçları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-269">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="50c6a-270">**İlk istek:**</span><span class="sxs-lookup"><span data-stu-id="50c6a-270">**First request:**</span></span>

<span data-ttu-id="50c6a-271">Denetleyici işlemleri:</span><span class="sxs-lookup"><span data-stu-id="50c6a-271">Controller operations:</span></span>

<span data-ttu-id="50c6a-272">Geçici: d233e165-f417-469B-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="50c6a-272">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="50c6a-273">Kapsam: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="50c6a-273">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="50c6a-274">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="50c6a-274">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="50c6a-275">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="50c6a-275">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="50c6a-276">`OperationService` işlemler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-276">`OperationService` operations:</span></span>

<span data-ttu-id="50c6a-277">Geçici: c6b049eb-1318-4E31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="50c6a-277">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="50c6a-278">Kapsam: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="50c6a-278">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="50c6a-279">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="50c6a-279">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="50c6a-280">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="50c6a-280">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="50c6a-281">**İkinci istek:**</span><span class="sxs-lookup"><span data-stu-id="50c6a-281">**Second request:**</span></span>

<span data-ttu-id="50c6a-282">Denetleyici işlemleri:</span><span class="sxs-lookup"><span data-stu-id="50c6a-282">Controller operations:</span></span>

<span data-ttu-id="50c6a-283">Geçici: b63bd538-0a37-4FF1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="50c6a-283">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="50c6a-284">Kapsam: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="50c6a-284">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="50c6a-285">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="50c6a-285">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="50c6a-286">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="50c6a-286">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="50c6a-287">`OperationService` işlemler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-287">`OperationService` operations:</span></span>

<span data-ttu-id="50c6a-288">Geçici: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="50c6a-288">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="50c6a-289">Kapsam: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="50c6a-289">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="50c6a-290">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="50c6a-290">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="50c6a-291">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="50c6a-291">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="50c6a-292">`OperationId` değerlerinden hangisinin bir istek içinde ve istekler arasında değiştiğini gözlemleyin:</span><span class="sxs-lookup"><span data-stu-id="50c6a-292">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="50c6a-293">*Geçici* nesneler her zaman farklıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-293">*Transient* objects are always different.</span></span> <span data-ttu-id="50c6a-294">Hem birinci hem de ikinci istemci isteklerinin geçici `OperationId` değeri hem `OperationService` işlemleri hem de istemci istekleri için farklıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-294">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="50c6a-295">Her hizmet isteğine ve istemci isteğine yeni bir örnek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-295">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="50c6a-296">*Kapsamlı* nesneler istemci isteği içinde aynıdır ancak istemci istekleri arasında farklıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-296">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="50c6a-297">*Tek* nesneler her nesne için aynıdır ve `Startup.ConfigureServices`bir `Operation` örneğinin sağlanmadığına bakılmaksızın her istek vardır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-297">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="50c6a-298">Ana bilgisayardan Hizmetleri çağır</span><span class="sxs-lookup"><span data-stu-id="50c6a-298">Call services from main</span></span>

<span data-ttu-id="50c6a-299">Uygulamanın kapsamındaki bir kapsamlı hizmeti çözümlemek için [ıvicescopefactory. CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) ile bir <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> oluşturun.</span><span class="sxs-lookup"><span data-stu-id="50c6a-299">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="50c6a-300">Bu yaklaşım, başlatma görevlerini çalıştırmak üzere başlangıçta kapsamlı bir hizmete erişmek için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-300">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="50c6a-301">Aşağıdaki örnek, `Program.Main``MyScopedService` için nasıl bağlam alınacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-301">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="50c6a-302">Kapsam doğrulaması</span><span class="sxs-lookup"><span data-stu-id="50c6a-302">Scope validation</span></span>

<span data-ttu-id="50c6a-303">Uygulama geliştirme ortamında çalışırken ve Konağı derlemek için [Createdefaultbuilder](xref:fundamentals/host/generic-host#default-builder-settings) çağırdığında, varsayılan hizmet sağlayıcı aşağıdakileri doğrulamak için denetimler gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-303">When the app is running in the Development environment and calls [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) to build the host, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="50c6a-304">Kapsamlı hizmetler doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="50c6a-304">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="50c6a-305">Kapsamlı hizmetler doğrudan veya dolaylı olarak Singleton 'a eklenmiş değildir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-305">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="50c6a-306"><xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> çağrıldığında kök hizmet sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-306">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="50c6a-307">Kök hizmet sağlayıcısının ömrü, sağlayıcının uygulamayla başladığı ve uygulama kapandığında bırakıldığı uygulama/sunucunun yaşam süresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-307">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="50c6a-308">Kapsamlı hizmetler kendilerini oluşturan kapsayıcı tarafından atılmış.</span><span class="sxs-lookup"><span data-stu-id="50c6a-308">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="50c6a-309">Kök kapsayıcıda kapsamlı bir hizmet oluşturulduysa, hizmetin ömrü etkin şekilde tek başına yükseltilir çünkü yalnızca uygulama/sunucu kapatıldığında kök kapsayıcı tarafından atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-309">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="50c6a-310">Hizmet kapsamlarını doğrulamak `BuildServiceProvider` çağrıldığında bu durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="50c6a-310">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="50c6a-311">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-311">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="50c6a-312">İstek Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-312">Request Services</span></span>

<span data-ttu-id="50c6a-313">`HttpContext` bir ASP.NET Core isteği içinde kullanılabilen hizmetler, [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) koleksiyonu aracılığıyla sunulur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-313">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="50c6a-314">İstek Hizmetleri, uygulamanın bir parçası olarak yapılandırılan ve istenen hizmetleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="50c6a-314">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="50c6a-315">Nesneler bağımlılıklar belirttiğinizde, bunlar `ApplicationServices`değil `RequestServices`bulunan türler tarafından karşılanır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-315">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="50c6a-316">Genellikle, uygulamanın bu özellikleri doğrudan kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-316">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="50c6a-317">Bunun yerine, sınıfların Sınıf oluşturucuları aracılığıyla gerektirdiği türleri isteyin ve çerçevenin bağımlılıkları eklemesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-317">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="50c6a-318">Bu, test etmek daha kolay olan sınıfları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-318">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="50c6a-319">`RequestServices` koleksiyonuna erişmek için Oluşturucu parametreleri olarak bağımlılıklar istemeyi tercih edin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-319">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="50c6a-320">Bağımlılık ekleme için tasarım hizmetleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-320">Design services for dependency injection</span></span>

<span data-ttu-id="50c6a-321">En iyi uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="50c6a-321">Best practices are to:</span></span>

* <span data-ttu-id="50c6a-322">Bağımlılıklarını almak için bağımlılık ekleme 'yi kullanmak üzere Hizmetleri tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-322">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="50c6a-323">Durum bilgisi olan statik sınıflar ve Üyeler kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="50c6a-323">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="50c6a-324">Genel durum oluşturulmasını önlemek yerine, tek tek Hizmetleri kullanmak için uygulamaları tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-324">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="50c6a-325">Hizmetler içindeki bağımlı sınıfların doğrudan örneklenmesini önleyin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-325">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="50c6a-326">Doğrudan örnekleme kodu belirli bir uygulamaya bağar.</span><span class="sxs-lookup"><span data-stu-id="50c6a-326">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="50c6a-327">Uygulama sınıflarını küçük, iyi bir şekilde ve kolayca test edin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-327">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="50c6a-328">Bir sınıfta çok fazla sayıda bağımlılık varsa, bu genellikle sınıfta çok fazla sorumluluk olduğu ve [tek sorumluluk ilkesini (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility)ihlal eden bir imzadır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-328">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="50c6a-329">Bazı sorumlulukları yeni bir sınıfa taşıyarak sınıfı yeniden düzenleme girişimi.</span><span class="sxs-lookup"><span data-stu-id="50c6a-329">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="50c6a-330">Razor Pages sayfa modeli sınıfları ve MVC denetleyici sınıflarının UI kaygılarıyla odaklanıp ilgilenmeyeceğini aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="50c6a-330">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="50c6a-331">İş kuralları ve veri erişimi uygulama ayrıntıları, bu [ayrı kaygılara](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)uygun sınıflarda tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-331">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="50c6a-332">Hizmetlerin elden çıkarılması</span><span class="sxs-lookup"><span data-stu-id="50c6a-332">Disposal of services</span></span>

<span data-ttu-id="50c6a-333">Kapsayıcı, oluşturduğu <xref:System.IDisposable> türleri için <xref:System.IDisposable.Dispose*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-333">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="50c6a-334">Kapsayıcıda Kullanıcı kodu tarafından bir örnek eklenirse, otomatik olarak atılamaz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-334">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="50c6a-335">Aşağıdaki örnekte, hizmetler hizmet kapsayıcısı tarafından oluşturulur ve otomatik olarak elden alınır:</span><span class="sxs-lookup"><span data-stu-id="50c6a-335">In the following example, the services are created by the service container and disposed automatically:</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

<span data-ttu-id="50c6a-336">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="50c6a-336">In the following example:</span></span>

* <span data-ttu-id="50c6a-337">Hizmet örnekleri hizmet kapsayıcısı tarafından oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-337">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="50c6a-338">Hedeflenen hizmet yaşam süreleri Framework tarafından bilinmemektedir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-338">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="50c6a-339">Framework Hizmetleri otomatik olarak atmaz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-339">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="50c6a-340">Hizmetler, geliştirici kodunda açıkça atılmadıklarında, uygulama kapatılıncaya kadar kalır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-340">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

<span data-ttu-id="50c6a-341">Hizmet verme seçeneklerinin bir tartışması için bkz. [ASP.NET Core ' de, Ndisposaveya ' ı atılamayan dört yol](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/).</span><span class="sxs-lookup"><span data-stu-id="50c6a-341">For a discussion of service disposal options, see [Four ways to dispose IDisposables in ASP.NET Core](https://andrewlock.net/four-ways-to-dispose-idisposables-in-asp-net-core/).</span></span>

## <a name="default-service-container-replacement"></a><span data-ttu-id="50c6a-342">Varsayılan hizmet kapsayıcısı değiştirme</span><span class="sxs-lookup"><span data-stu-id="50c6a-342">Default service container replacement</span></span>

<span data-ttu-id="50c6a-343">Yerleşik hizmet kapsayıcısı, çerçeve ihtiyaçlarına ve çoğu tüketici uygulamasına hizmet vermek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-343">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="50c6a-344">Yerleşik kapsayıcının desteklemediği belirli bir özelliğe ihtiyacınız yoksa, yerleşik kapsayıcının kullanılması önerilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="50c6a-344">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="50c6a-345">Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="50c6a-345">Property injection</span></span>
* <span data-ttu-id="50c6a-346">Ada göre ekleme</span><span class="sxs-lookup"><span data-stu-id="50c6a-346">Injection based on name</span></span>
* <span data-ttu-id="50c6a-347">Alt kapsayıcılar</span><span class="sxs-lookup"><span data-stu-id="50c6a-347">Child containers</span></span>
* <span data-ttu-id="50c6a-348">Özel ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="50c6a-348">Custom lifetime management</span></span>
* <span data-ttu-id="50c6a-349">yavaş başlatma için `Func<T>` desteği</span><span class="sxs-lookup"><span data-stu-id="50c6a-349">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="50c6a-350">Kural tabanlı kayıt</span><span class="sxs-lookup"><span data-stu-id="50c6a-350">Convention-based registration</span></span>

<span data-ttu-id="50c6a-351">Aşağıdaki 3. taraf kapsayıcıları ASP.NET Core uygulamalarla kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-351">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="50c6a-352">Autofac</span><span class="sxs-lookup"><span data-stu-id="50c6a-352">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="50c6a-353">Drıioc</span><span class="sxs-lookup"><span data-stu-id="50c6a-353">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="50c6a-354">Yetkisiz kullanım</span><span class="sxs-lookup"><span data-stu-id="50c6a-354">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="50c6a-355">Açık Inject</span><span class="sxs-lookup"><span data-stu-id="50c6a-355">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="50c6a-356">E</span><span class="sxs-lookup"><span data-stu-id="50c6a-356">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="50c6a-357">Stakıbox</span><span class="sxs-lookup"><span data-stu-id="50c6a-357">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="50c6a-358">Unity</span><span class="sxs-lookup"><span data-stu-id="50c6a-358">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="50c6a-359">İş parçacığı güvenliği</span><span class="sxs-lookup"><span data-stu-id="50c6a-359">Thread safety</span></span>

<span data-ttu-id="50c6a-360">İş parçacığı güvenli Singleton Hizmetleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="50c6a-360">Create thread-safe singleton services.</span></span> <span data-ttu-id="50c6a-361">Tek bir hizmetin geçici bir hizmete bağımlılığı varsa, geçici hizmet aynı zamanda tek tek tarafından nasıl kullanıldığına bağlı olarak iş parçacığı güvenliği de gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-361">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="50c6a-362">Tek bir hizmetin fabrika yöntemi (örneğin, [AddSingleton\<TService > (ısevicecollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), iş parçacığı açısından güvenli olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="50c6a-362">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="50c6a-363">Bir tür (`static`) Oluşturucusu gibi, tek bir iş parçacığı tarafından bir kez çağrılması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-363">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="50c6a-364">Öneriler</span><span class="sxs-lookup"><span data-stu-id="50c6a-364">Recommendations</span></span>

* <span data-ttu-id="50c6a-365">`async/await` ve `Task` tabanlı hizmet çözümlemesi desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="50c6a-365">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="50c6a-366">C#zaman uyumsuz oluşturucuları desteklemez; Bu nedenle, önerilen model hizmeti zaman uyumlu olarak çözümledikten sonra zaman uyumsuz yöntemler kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-366">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="50c6a-367">Veri ve yapılandırmayı doğrudan hizmet kapsayıcısında saklamaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="50c6a-367">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="50c6a-368">Örneğin, bir kullanıcının alışveriş sepeti genellikle hizmet kapsayıcısına eklenmemelidir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-368">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="50c6a-369">Yapılandırma, [Seçenekler modelini](xref:fundamentals/configuration/options)kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-369">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="50c6a-370">Benzer şekilde, yalnızca başka bir nesneye erişime izin vermek için mevcut olan "veri sahibi" nesnelerinden kaçının.</span><span class="sxs-lookup"><span data-stu-id="50c6a-370">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="50c6a-371">DI aracılığıyla gerçek öğe istemek daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-371">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="50c6a-372">Hizmetlere statik erişimi önleyin (örneğin, başka bir yerde kullanmak üzere, statik olarak yazılan [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) ).</span><span class="sxs-lookup"><span data-stu-id="50c6a-372">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="50c6a-373">*Hizmet bulucu deseninin*kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="50c6a-373">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="50c6a-374">Örneğin, yerine şunu kullandığınızda bir hizmet örneği elde etmek için <xref:System.IServiceProvider.GetService*> çağırmayın:</span><span class="sxs-lookup"><span data-stu-id="50c6a-374">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="50c6a-375">**Olmayan**</span><span class="sxs-lookup"><span data-stu-id="50c6a-375">**Incorrect:**</span></span>

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

  <span data-ttu-id="50c6a-376">**Doğru**:</span><span class="sxs-lookup"><span data-stu-id="50c6a-376">**Correct**:</span></span>

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

* <span data-ttu-id="50c6a-377">Önlemek için başka bir hizmet bulucu çeşitlemesi, çalışma zamanında bağımlılıkları çözümleyen bir ekleme.</span><span class="sxs-lookup"><span data-stu-id="50c6a-377">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="50c6a-378">Bu uygulamalardan her ikisi de [Denetim stratejilerini geçersiz kılar](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) .</span><span class="sxs-lookup"><span data-stu-id="50c6a-378">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="50c6a-379">`HttpContext` statik erişimden kaçının (örneğin, [ıhttpcontextaccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="50c6a-379">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="50c6a-380">Tüm öneri kümeleri gibi, bir öneriyi yok saymayı yok saymış durumlarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-380">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="50c6a-381">Özel durumlar genellikle Framework içindeki özel durumlar&mdash;nadir olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-381">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="50c6a-382">Dı, statik/genel nesne erişim desenlerinin bir *alternatifidir* .</span><span class="sxs-lookup"><span data-stu-id="50c6a-382">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="50c6a-383">Statik nesne erişimi ile karıştırırsanız, dı 'nin avantajlarını fark edemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-383">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="50c6a-384">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="50c6a-384">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="50c6a-385">Bağımlılık ekleme (MSDN) ile ASP.NET Core temizleme kodu yazma</span><span class="sxs-lookup"><span data-stu-id="50c6a-385">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="50c6a-386">Açık bağımlılıklar Ilkesi</span><span class="sxs-lookup"><span data-stu-id="50c6a-386">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="50c6a-387">Denetim kapsayıcıları ve bağımlılık ekleme deseninin Inversion 'ı (Marwler)</span><span class="sxs-lookup"><span data-stu-id="50c6a-387">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="50c6a-388">ASP.NET Core DI 'de birden çok arabirime sahip bir hizmeti kaydetme</span><span class="sxs-lookup"><span data-stu-id="50c6a-388">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="50c6a-389">ASP.NET Core, sınıflar ve bunların bağımlılıkları arasında [denetimin INVERSION (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) elde etmek için bir teknik olan bağımlılık ekleme (dı) yazılım tasarım modelini destekler.</span><span class="sxs-lookup"><span data-stu-id="50c6a-389">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="50c6a-390">MVC denetleyicileri içindeki bağımlılık eklenmesine özgü daha fazla bilgi için bkz. <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-390">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="50c6a-391">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="50c6a-391">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="50c6a-392">Bağımlılık eklenmesine genel bakış</span><span class="sxs-lookup"><span data-stu-id="50c6a-392">Overview of dependency injection</span></span>

<span data-ttu-id="50c6a-393">*Bağımlılık* , başka bir nesnenin gerektirdiği herhangi bir nesnedir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-393">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="50c6a-394">Aşağıdaki `MyDependency` sınıfını bir uygulamadaki diğer sınıfların bağlı olduğu bir `WriteMessage` yöntemiyle inceleyin:</span><span class="sxs-lookup"><span data-stu-id="50c6a-394">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="50c6a-395">`WriteMessage` yöntemini bir sınıf için kullanılabilir hale getirmek için `MyDependency` sınıfının bir örneği oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-395">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="50c6a-396">`MyDependency` sınıfı, `IndexModel` sınıfının bir bağımlılığı olur:</span><span class="sxs-lookup"><span data-stu-id="50c6a-396">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="50c6a-397">Sınıf oluşturur ve doğrudan `MyDependency` örneğine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-397">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="50c6a-398">Kod bağımlılıkları (önceki örnekte olduğu gibi) sorunlu olur ve aşağıdaki nedenlerden dolayı kaçınılması gerekir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-398">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="50c6a-399">`MyDependency` farklı bir uygulamayla değiştirmek için, sınıfın değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-399">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="50c6a-400">`MyDependency` bağımlılıklar içeriyorsa, sınıfı tarafından yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-400">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="50c6a-401">`MyDependency`bağlı olarak, birden çok sınıfa sahip büyük bir projede yapılandırma kodu uygulama genelinde dağılmış hale gelir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-401">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="50c6a-402">Bu uygulamanın birim testi zordur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-402">This implementation is difficult to unit test.</span></span> <span data-ttu-id="50c6a-403">Uygulama, bu yaklaşımla mümkün olmayan bir sahte veya saplama `MyDependency` sınıfı kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-403">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="50c6a-404">Bağımlılık ekleme bu sorunları şu şekilde giderir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-404">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="50c6a-405">Bağımlılık uygulamasını soyutlamak için bir arabirim veya temel sınıf kullanımı.</span><span class="sxs-lookup"><span data-stu-id="50c6a-405">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="50c6a-406">Bir hizmet kapsayıcısına bağımlılığın kaydı.</span><span class="sxs-lookup"><span data-stu-id="50c6a-406">Registration of the dependency in a service container.</span></span> <span data-ttu-id="50c6a-407">ASP.NET Core yerleşik bir hizmet kapsayıcısı sağlar <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-407">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="50c6a-408">Hizmetler, uygulamanın `Startup.ConfigureServices` yöntemine kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-408">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="50c6a-409">Hizmetin kullanıldığı sınıf oluşturucusuna *ekleme* .</span><span class="sxs-lookup"><span data-stu-id="50c6a-409">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="50c6a-410">Çerçeve, bağımlılığın bir örneğini oluşturma ve artık gerekli olmadığında bu uygulamayı atma sorumluluğunu alır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-410">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="50c6a-411">[Örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` arabirimi hizmetin uygulamaya sağladığı bir yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="50c6a-411">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="50c6a-412">Bu arabirim somut bir tür tarafından uygulanır, `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="50c6a-412">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="50c6a-413">`MyDependency` kurucusunda bir <xref:Microsoft.Extensions.Logging.ILogger`1> ister.</span><span class="sxs-lookup"><span data-stu-id="50c6a-413">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="50c6a-414">Bağımlılık ekleme işlemini zincirleme bir biçimde kullanmak olağan dışı değildir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-414">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="50c6a-415">Her istenen bağımlılık, kendi bağımlılıklarını ister.</span><span class="sxs-lookup"><span data-stu-id="50c6a-415">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="50c6a-416">Kapsayıcı grafikteki bağımlılıkları çözer ve tamamen çözümlenen hizmeti döndürür.</span><span class="sxs-lookup"><span data-stu-id="50c6a-416">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="50c6a-417">Çözümlenmesi gereken, genellikle *bağımlılık ağacı*, *bağımlılık grafiği*veya *nesne grafiği*olarak adlandırılan toplu bağımlılıklar kümesi.</span><span class="sxs-lookup"><span data-stu-id="50c6a-417">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="50c6a-418">`IMyDependency` ve `ILogger<TCategoryName>`, hizmet kapsayıcısında kayıtlı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-418">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="50c6a-419">`IMyDependency` `Startup.ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-419">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="50c6a-420">`ILogger<TCategoryName>`, günlük soyut öğeler altyapısı tarafından kaydedilir. bu nedenle, Framework tarafından varsayılan olarak kaydedilen [Framework tarafından sağlanmış bir hizmettir](#framework-provided-services) .</span><span class="sxs-lookup"><span data-stu-id="50c6a-420">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="50c6a-421">Kapsayıcı, [(genel) açık türlerden](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types)yararlanarak `ILogger<TCategoryName>` çözer, her [(genel) oluşturulan türü](/dotnet/csharp/language-reference/language-specification/types#constructed-types)kaydetme ihtiyacını ortadan kaldırır:</span><span class="sxs-lookup"><span data-stu-id="50c6a-421">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<>), typeof(Logger<>));
```

<span data-ttu-id="50c6a-422">Örnek uygulamada `IMyDependency` hizmeti somut tür `MyDependency`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-422">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="50c6a-423">Kayıt, hizmet ömrünü tek bir isteğin kullanım ömrüne göre kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="50c6a-423">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="50c6a-424">[Hizmet yaşam süreleri](#service-lifetimes) bu konunun ilerleyen kısımlarında açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-424">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="50c6a-425">Her `services.Add{SERVICE_NAME}` uzantısı yöntemi Hizmetleri ekler (ve potansiyel olarak yapılandırır).</span><span class="sxs-lookup"><span data-stu-id="50c6a-425">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="50c6a-426">Örneğin `services.AddMvc()`, Razor Pages ve MVC 'nin gerektirdiği Hizmetleri ekler.</span><span class="sxs-lookup"><span data-stu-id="50c6a-426">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="50c6a-427">Uygulamaların bu kuralı izlemesini öneririz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-427">We recommended that apps follow this convention.</span></span> <span data-ttu-id="50c6a-428">Hizmet kaydı gruplarını kapsüllemek için uzantı yöntemlerini [Microsoft. Extensions. Dependencyınjection](/dotnet/api/microsoft.extensions.dependencyinjection) ad alanına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-428">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="50c6a-429">Hizmetin Oluşturucusu `string`gibi [yerleşik bir tür](/dotnet/csharp/language-reference/keywords/built-in-types-table)gerektiriyorsa, tür [yapılandırma](xref:fundamentals/configuration/index) veya [Seçenekler düzeniyle](xref:fundamentals/configuration/options)eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-429">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="50c6a-430">Hizmetin bir örneği, hizmetin kullanıldığı ve özel bir alana atandığı bir sınıfın Oluşturucusu aracılığıyla istenir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-430">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="50c6a-431">Alanı, sınıfına gereken şekilde hizmete erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-431">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="50c6a-432">Örnek uygulamada, `IMyDependency` örneği istenir ve hizmetin `WriteMessage` yöntemini çağırmak için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="50c6a-432">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="services-injected-into-startup"></a><span data-ttu-id="50c6a-433">Başlangıca eklenen hizmetler</span><span class="sxs-lookup"><span data-stu-id="50c6a-433">Services injected into Startup</span></span>

<span data-ttu-id="50c6a-434">Genel ana bilgisayar (<xref:Microsoft.Extensions.Hosting.IHostBuilder>) kullanılırken `Startup` oluşturucusuna yalnızca aşağıdaki hizmet türleri eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-434">Only the following service types can be injected into the `Startup` constructor when using the Generic Host (<xref:Microsoft.Extensions.Hosting.IHostBuilder>):</span></span>

* `IWebHostEnvironment`
* <xref:Microsoft.Extensions.Hosting.IHostEnvironment>
* <xref:Microsoft.Extensions.Configuration.IConfiguration>

<span data-ttu-id="50c6a-435">Hizmetler `Startup.Configure`eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-435">Services can be injected into `Startup.Configure`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> options)
{
    ...
}
```

<span data-ttu-id="50c6a-436">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-436">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="50c6a-437">Framework tarafından sunulan hizmetler</span><span class="sxs-lookup"><span data-stu-id="50c6a-437">Framework-provided services</span></span>

<span data-ttu-id="50c6a-438">`Startup.ConfigureServices` yöntemi, uygulamanın kullandığı hizmetlerin (Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri de dahil) tanımlanmasından sorumludur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-438">The `Startup.ConfigureServices` method is responsible for defining the services that the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="50c6a-439">Başlangıçta, `ConfigureServices` için belirtilen `IServiceCollection` [konağın nasıl yapılandırıldığına](xref:fundamentals/index#host)bağlı olarak Framework tarafından tanımlanan hizmetlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-439">Initially, the `IServiceCollection` provided to `ConfigureServices` has services defined by the framework depending on [how the host was configured](xref:fundamentals/index#host).</span></span> <span data-ttu-id="50c6a-440">Çerçeve tarafından kaydedilmiş yüzlerce hizmete sahip olmak ASP.NET Core şablona dayalı bir uygulama için sık görülen bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-440">It's not uncommon for an app based on an ASP.NET Core template to have hundreds of services registered by the framework.</span></span> <span data-ttu-id="50c6a-441">Aşağıdaki tabloda çerçeve kayıtlı hizmetlerden oluşan küçük bir örnek listelenmiştir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-441">A small sample of framework-registered services is listed in the following table.</span></span>

| <span data-ttu-id="50c6a-442">Hizmet Türü</span><span class="sxs-lookup"><span data-stu-id="50c6a-442">Service Type</span></span> | <span data-ttu-id="50c6a-443">Ömür</span><span class="sxs-lookup"><span data-stu-id="50c6a-443">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="50c6a-444">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-444">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="50c6a-445">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-445">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="50c6a-446">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-446">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="50c6a-447">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-447">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="50c6a-448">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-448">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="50c6a-449">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-449">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="50c6a-450">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-450">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="50c6a-451">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-451">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="50c6a-452">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-452">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="50c6a-453">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-453">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="50c6a-454">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-454">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="50c6a-455">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-455">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="50c6a-456">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-456">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="50c6a-457">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-457">Singleton</span></span> |

## <a name="register-additional-services-with-extension-methods"></a><span data-ttu-id="50c6a-458">Uzantı yöntemleriyle ek hizmetleri kaydetme</span><span class="sxs-lookup"><span data-stu-id="50c6a-458">Register additional services with extension methods</span></span>

<span data-ttu-id="50c6a-459">Bir hizmet koleksiyonu genişletme yöntemi (ve gerekirse bağımlı hizmetleri) kaydetmek için kullanılabilir olduğunda, bu hizmet için gereken tüm hizmetleri kaydetmek üzere tek bir `Add{SERVICE_NAME}` uzantısı yöntemi kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-459">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="50c6a-460">Aşağıdaki kod, [Adddbcontext\<tcontext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) ve <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>uzantı yöntemlerini kullanarak kapsayıcıya ek hizmetler eklemenin bir örneğidir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-460">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) and <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>:</span></span>

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

<span data-ttu-id="50c6a-461">Daha fazla bilgi için API belgelerindeki <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> sınıfına bakın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-461">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="50c6a-462">Hizmet yaşam süreleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-462">Service lifetimes</span></span>

<span data-ttu-id="50c6a-463">Kayıtlı her hizmet için uygun bir yaşam süresi seçin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-463">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="50c6a-464">ASP.NET Core hizmetler aşağıdaki yaşam süreleri ile yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-464">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="50c6a-465">Geçici</span><span class="sxs-lookup"><span data-stu-id="50c6a-465">Transient</span></span>

<span data-ttu-id="50c6a-466">Geçici ömür Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>), hizmet kapsayıcısından her istenilişinde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-466">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="50c6a-467">Bu ömür, hafif ve durumsuz hizmetler için en iyi şekilde kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-467">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="50c6a-468">Yayıl</span><span class="sxs-lookup"><span data-stu-id="50c6a-468">Scoped</span></span>

<span data-ttu-id="50c6a-469">Kapsamlı ömür Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>), istemci isteği başına bir kez oluşturulur (bağlantı).</span><span class="sxs-lookup"><span data-stu-id="50c6a-469">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="50c6a-470">Bir ara yazılım içinde kapsamlı bir hizmet kullanırken, hizmeti `Invoke` veya `InvokeAsync` yöntemine ekleyin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-470">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="50c6a-471">Oluşturucu ekleme yoluyla ekleme, hizmeti tek bir gibi davranmaya zoryor.</span><span class="sxs-lookup"><span data-stu-id="50c6a-471">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="50c6a-472">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-472">For more information, see <xref:fundamentals/middleware/write#per-request-middleware-dependencies>.</span></span>

### <a name="singleton"></a><span data-ttu-id="50c6a-473">adet</span><span class="sxs-lookup"><span data-stu-id="50c6a-473">Singleton</span></span>

<span data-ttu-id="50c6a-474">Tek yaşam süresi Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>), ilk istendiğinde oluşturulur (veya `Startup.ConfigureServices` çalıştırıldığında ve hizmet kaydıyla bir örnek belirtildiğinde).</span><span class="sxs-lookup"><span data-stu-id="50c6a-474">Singleton lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="50c6a-475">Her sonraki istek aynı örneği kullanır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-475">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="50c6a-476">Uygulama tek davranış gerektiriyorsa, hizmet kapsayıcısının hizmetin ömrünü yönetmesine izin verilmesi önerilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-476">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="50c6a-477">Tekil tasarım modelini uygulamayın ve nesnenin sınıfındaki ömrünü yönetmek için Kullanıcı kodu sağlayın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-477">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="50c6a-478">Kapsamlı bir hizmetin tek bir bilgisayardan çözümlenmesi tehlikelidir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-478">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="50c6a-479">Bu, sonraki istekleri işlerken hizmetin yanlış duruma gelmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-479">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="50c6a-480">Hizmet kayıt yöntemleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-480">Service registration methods</span></span>

<span data-ttu-id="50c6a-481">Hizmet kayıt uzantısı yöntemleri, belirli senaryolarda yararlı olan aşırı yüklemeler sunar.</span><span class="sxs-lookup"><span data-stu-id="50c6a-481">Service registration extension methods offer overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="50c6a-482">Yöntem</span><span class="sxs-lookup"><span data-stu-id="50c6a-482">Method</span></span> | <span data-ttu-id="50c6a-483">Otomatik</span><span class="sxs-lookup"><span data-stu-id="50c6a-483">Automatic</span></span><br><span data-ttu-id="50c6a-484">nesne</span><span class="sxs-lookup"><span data-stu-id="50c6a-484">object</span></span><br><span data-ttu-id="50c6a-485">elden</span><span class="sxs-lookup"><span data-stu-id="50c6a-485">disposal</span></span> | <span data-ttu-id="50c6a-486">Birden Çok</span><span class="sxs-lookup"><span data-stu-id="50c6a-486">Multiple</span></span><br><span data-ttu-id="50c6a-487">uygulamalar</span><span class="sxs-lookup"><span data-stu-id="50c6a-487">implementations</span></span> | <span data-ttu-id="50c6a-488">Geçiş bağımsız değişkenleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-488">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="50c6a-489">Örnek:</span><span class="sxs-lookup"><span data-stu-id="50c6a-489">Example:</span></span><br>`services.AddSingleton<IMyDep, MyDep>();` | <span data-ttu-id="50c6a-490">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-490">Yes</span></span> | <span data-ttu-id="50c6a-491">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-491">Yes</span></span> | <span data-ttu-id="50c6a-492">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-492">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="50c6a-493">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-493">Examples:</span></span><br>`services.AddSingleton<IMyDep>(sp => new MyDep());`<br>`services.AddSingleton<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="50c6a-494">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-494">Yes</span></span> | <span data-ttu-id="50c6a-495">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-495">Yes</span></span> | <span data-ttu-id="50c6a-496">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-496">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="50c6a-497">Örnek:</span><span class="sxs-lookup"><span data-stu-id="50c6a-497">Example:</span></span><br>`services.AddSingleton<MyDep>();` | <span data-ttu-id="50c6a-498">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-498">Yes</span></span> | <span data-ttu-id="50c6a-499">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-499">No</span></span> | <span data-ttu-id="50c6a-500">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-500">No</span></span> |
| `AddSingleton<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="50c6a-501">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-501">Examples:</span></span><br>`services.AddSingleton<IMyDep>(new MyDep());`<br>`services.AddSingleton<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="50c6a-502">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-502">No</span></span> | <span data-ttu-id="50c6a-503">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-503">Yes</span></span> | <span data-ttu-id="50c6a-504">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-504">Yes</span></span> |
| `AddSingleton(new {IMPLEMENTATION})`<br><span data-ttu-id="50c6a-505">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-505">Examples:</span></span><br>`services.AddSingleton(new MyDep());`<br>`services.AddSingleton(new MyDep("A string!"));` | <span data-ttu-id="50c6a-506">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-506">No</span></span> | <span data-ttu-id="50c6a-507">Hayır</span><span class="sxs-lookup"><span data-stu-id="50c6a-507">No</span></span> | <span data-ttu-id="50c6a-508">Evet</span><span class="sxs-lookup"><span data-stu-id="50c6a-508">Yes</span></span> |

<span data-ttu-id="50c6a-509">Tür çıkarma hakkında daha fazla bilgi için [Hizmetler 'In aktiften çıkarılması](#disposal-of-services) bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-509">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="50c6a-510">Birden çok uygulama için yaygın bir senaryo, [test için bir sahte işlem türüdür](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="50c6a-510">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="50c6a-511">`TryAdd{LIFETIME}` Yöntemler, zaten kayıtlı bir uygulama yoksa hizmeti kaydeder.</span><span class="sxs-lookup"><span data-stu-id="50c6a-511">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="50c6a-512">Aşağıdaki örnekte, ilk satır `IMyDependency`için `MyDependency` kaydettirir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-512">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="50c6a-513">`IMyDependency` zaten kayıtlı bir uygulamaya sahip olduğundan ikinci satır etkisizdir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-513">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="50c6a-514">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="50c6a-514">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="50c6a-515">[TryAddEnumerable (ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) yöntemleri yalnızca *aynı türde*bir uygulama yoksa hizmeti kaydeder.</span><span class="sxs-lookup"><span data-stu-id="50c6a-515">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="50c6a-516">Birden çok hizmet `IEnumerable<{SERVICE}>`ile çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-516">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="50c6a-517">Hizmetleri kaydederken, geliştirici yalnızca aynı türden biri zaten eklenmediyse bir örnek eklemek istemektedir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-517">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="50c6a-518">Genellikle, bu yöntem, kapsayıcıda bir örneğin iki kopyasını kaydetmemek için kitaplık yazarları tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-518">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="50c6a-519">Aşağıdaki örnekte, ilk satır `IMyDep1`için `MyDep` kaydettirir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-519">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="50c6a-520">İkinci satır, `IMyDep2`için `MyDep` kaydeder.</span><span class="sxs-lookup"><span data-stu-id="50c6a-520">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="50c6a-521">`IMyDep1` `MyDep`kayıtlı bir uygulamasına zaten sahip olduğundan, üçüncü satırın etkisi yoktur:</span><span class="sxs-lookup"><span data-stu-id="50c6a-521">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="50c6a-522">Oluşturucu Ekleme davranışı</span><span class="sxs-lookup"><span data-stu-id="50c6a-522">Constructor injection behavior</span></span>

<span data-ttu-id="50c6a-523">Hizmetler, iki mekanizma tarafından çözülebilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-523">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="50c6a-524"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash;, bağımlılık ekleme kapsayıcısına hizmet kaydı olmadan nesne oluşturulmasına Izin verir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-524"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="50c6a-525">`ActivatorUtilities` etiket yardımcıları, MVC denetleyicileri ve model ciltler gibi kullanıcı tarafından ilgili soyutlamalar ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-525">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="50c6a-526">Oluşturucular bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bağımsız değişkenlerin varsayılan değerleri ataması gerekir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-526">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="50c6a-527">Hizmetler `IServiceProvider` veya `ActivatorUtilities`tarafından çözümlendiğinde, Oluşturucu Ekleme *ortak* bir Oluşturucu gerektirir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-527">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="50c6a-528">Hizmetler `ActivatorUtilities`tarafından çözümlendiğinde, Oluşturucu ekleme yalnızca bir adet geçerli oluşturucunun var olmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-528">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="50c6a-529">Oluşturucu aşırı yüklemeleri desteklenir, ancak bağımsız değişkenleri bağımlılık ekleme tarafından yerine yalnızca bir aşırı yükleme bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-529">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="50c6a-530">Entity Framework bağlamları</span><span class="sxs-lookup"><span data-stu-id="50c6a-530">Entity Framework contexts</span></span>

<span data-ttu-id="50c6a-531">Entity Framework bağlamlar genellikle, Web uygulaması veritabanı işlemleri normalde istemci isteği kapsamında olduğundan [kapsamlı ömür](#service-lifetimes) kullanılarak hizmet kapsayıcısına eklenir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-531">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="50c6a-532">Veritabanı bağlamı kaydedilirken bir [Adddbcontext\<tcontext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) aşırı yüklemesi tarafından bir yaşam süresi belirtilmemişse varsayılan yaşam süresi kapsamındadır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-532">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="50c6a-533">Belirli bir yaşam süresinin Hizmetleri, hizmetten daha kısa bir yaşam süresine sahip bir veritabanı bağlamı kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-533">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="50c6a-534">Ömür ve kayıt seçenekleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-534">Lifetime and registration options</span></span>

<span data-ttu-id="50c6a-535">Ömür ve kayıt seçenekleri arasındaki farkı göstermek için, görevleri benzersiz bir tanımlayıcıya sahip bir işlem olarak temsil eden aşağıdaki arayüzleri göz önünde bulundurun `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="50c6a-535">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="50c6a-536">Bir işlem hizmetinin yaşam süresinin aşağıdaki arabirimler için nasıl yapılandırıldığına bağlı olarak kapsayıcı, bir sınıf tarafından istendiğinde aynı ya da farklı bir hizmet örneği sağlar:</span><span class="sxs-lookup"><span data-stu-id="50c6a-536">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="50c6a-537">Arabirimler `Operation` sınıfında uygulanır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-537">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="50c6a-538">`Operation` Oluşturucusu bir GUID sağlanmamışsa bir GUID oluşturur:</span><span class="sxs-lookup"><span data-stu-id="50c6a-538">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="50c6a-539">Diğer `Operation` türlerinin her birine bağlı olan bir `OperationService` kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-539">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="50c6a-540">Bağımlılık ekleme yoluyla `OperationService` istendiğinde, her bir hizmetin yeni bir örneğini ya da bağımlı hizmetin kullanım ömrü temelinde mevcut bir örneği alır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-540">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="50c6a-541">Kapsayıcıda istendiğinde geçici hizmetler oluşturulduğunda, `IOperationTransient` hizmetinin `OperationId` `OperationService``OperationId` farklıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-541">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="50c6a-542">`OperationService`, `IOperationTransient` sınıfının yeni bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-542">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="50c6a-543">Yeni örnek farklı bir `OperationId`verir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-543">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="50c6a-544">İstemci isteği başına kapsamlı hizmetler oluşturulduğunda, `IOperationScoped` hizmetinin `OperationId` istemci isteği içindeki `OperationService` ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-544">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="50c6a-545">İstemci istekleri arasında her iki hizmet de farklı bir `OperationId` değeri paylaşır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-545">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="50c6a-546">Tek ve tek örnekli hizmetler bir kez oluşturulduğunda ve tüm istemci isteklerinde ve tüm hizmetlerde kullanıldığında, `OperationId` tüm hizmet istekleri arasında sabittir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-546">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="50c6a-547">`Startup.ConfigureServices`, her tür kapsayıcıya, adlandırılmış ömrüne göre eklenir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-547">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="50c6a-548">`IOperationSingletonInstance` hizmeti, bilinen bir `Guid.Empty`KIMLIĞIYLE belirli bir örnek kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="50c6a-548">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="50c6a-549">Bu tür kullanımda olduğunda (GUID 'sinin tümü sıfırlardan tamamen) Bu bir şey vardır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-549">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="50c6a-550">Örnek uygulama, bireysel istekler içindeki ve içindeki nesne yaşam sürelerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-550">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="50c6a-551">Örnek uygulamanın `IndexModel` her tür `IOperation` türü ve `OperationService`ister.</span><span class="sxs-lookup"><span data-stu-id="50c6a-551">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="50c6a-552">Daha sonra sayfa, tüm sayfa modeli sınıfının ve hizmetin `OperationId` değerlerini özellik atamaları aracılığıyla görüntüler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-552">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="50c6a-553">Aşağıdaki iki çıktıda iki isteğin sonuçları gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-553">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="50c6a-554">**İlk istek:**</span><span class="sxs-lookup"><span data-stu-id="50c6a-554">**First request:**</span></span>

<span data-ttu-id="50c6a-555">Denetleyici işlemleri:</span><span class="sxs-lookup"><span data-stu-id="50c6a-555">Controller operations:</span></span>

<span data-ttu-id="50c6a-556">Geçici: d233e165-f417-469B-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="50c6a-556">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="50c6a-557">Kapsam: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="50c6a-557">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="50c6a-558">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="50c6a-558">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="50c6a-559">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="50c6a-559">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="50c6a-560">`OperationService` işlemler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-560">`OperationService` operations:</span></span>

<span data-ttu-id="50c6a-561">Geçici: c6b049eb-1318-4E31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="50c6a-561">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="50c6a-562">Kapsam: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="50c6a-562">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="50c6a-563">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="50c6a-563">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="50c6a-564">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="50c6a-564">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="50c6a-565">**İkinci istek:**</span><span class="sxs-lookup"><span data-stu-id="50c6a-565">**Second request:**</span></span>

<span data-ttu-id="50c6a-566">Denetleyici işlemleri:</span><span class="sxs-lookup"><span data-stu-id="50c6a-566">Controller operations:</span></span>

<span data-ttu-id="50c6a-567">Geçici: b63bd538-0a37-4FF1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="50c6a-567">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="50c6a-568">Kapsam: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="50c6a-568">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="50c6a-569">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="50c6a-569">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="50c6a-570">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="50c6a-570">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="50c6a-571">`OperationService` işlemler:</span><span class="sxs-lookup"><span data-stu-id="50c6a-571">`OperationService` operations:</span></span>

<span data-ttu-id="50c6a-572">Geçici: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="50c6a-572">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="50c6a-573">Kapsam: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="50c6a-573">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="50c6a-574">Tek: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="50c6a-574">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="50c6a-575">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="50c6a-575">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="50c6a-576">`OperationId` değerlerinden hangisinin bir istek içinde ve istekler arasında değiştiğini gözlemleyin:</span><span class="sxs-lookup"><span data-stu-id="50c6a-576">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="50c6a-577">*Geçici* nesneler her zaman farklıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-577">*Transient* objects are always different.</span></span> <span data-ttu-id="50c6a-578">Hem birinci hem de ikinci istemci isteklerinin geçici `OperationId` değeri hem `OperationService` işlemleri hem de istemci istekleri için farklıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-578">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="50c6a-579">Her hizmet isteğine ve istemci isteğine yeni bir örnek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-579">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="50c6a-580">*Kapsamlı* nesneler istemci isteği içinde aynıdır ancak istemci istekleri arasında farklıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-580">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="50c6a-581">*Tek* nesneler her nesne için aynıdır ve `Startup.ConfigureServices`bir `Operation` örneğinin sağlanmadığına bakılmaksızın her istek vardır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-581">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="50c6a-582">Ana bilgisayardan Hizmetleri çağır</span><span class="sxs-lookup"><span data-stu-id="50c6a-582">Call services from main</span></span>

<span data-ttu-id="50c6a-583">Uygulamanın kapsamındaki bir kapsamlı hizmeti çözümlemek için [ıvicescopefactory. CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) ile bir <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> oluşturun.</span><span class="sxs-lookup"><span data-stu-id="50c6a-583">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="50c6a-584">Bu yaklaşım, başlatma görevlerini çalıştırmak üzere başlangıçta kapsamlı bir hizmete erişmek için yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-584">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="50c6a-585">Aşağıdaki örnek, `Program.Main``MyScopedService` için nasıl bağlam alınacağını gösterir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-585">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="50c6a-586">Kapsam doğrulaması</span><span class="sxs-lookup"><span data-stu-id="50c6a-586">Scope validation</span></span>

<span data-ttu-id="50c6a-587">Uygulama geliştirme ortamında çalışırken, varsayılan hizmet sağlayıcısı şunları doğrulamak için denetimler gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-587">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="50c6a-588">Kapsamlı hizmetler doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="50c6a-588">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="50c6a-589">Kapsamlı hizmetler doğrudan veya dolaylı olarak Singleton 'a eklenmiş değildir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-589">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="50c6a-590"><xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> çağrıldığında kök hizmet sağlayıcısı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-590">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="50c6a-591">Kök hizmet sağlayıcısının ömrü, sağlayıcının uygulamayla başladığı ve uygulama kapandığında bırakıldığı uygulama/sunucunun yaşam süresine karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-591">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="50c6a-592">Kapsamlı hizmetler kendilerini oluşturan kapsayıcı tarafından atılmış.</span><span class="sxs-lookup"><span data-stu-id="50c6a-592">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="50c6a-593">Kök kapsayıcıda kapsamlı bir hizmet oluşturulduysa, hizmetin ömrü etkin şekilde tek başına yükseltilir çünkü yalnızca uygulama/sunucu kapatıldığında kök kapsayıcı tarafından atılmış olur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-593">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="50c6a-594">Hizmet kapsamlarını doğrulamak `BuildServiceProvider` çağrıldığında bu durumları yakalar.</span><span class="sxs-lookup"><span data-stu-id="50c6a-594">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="50c6a-595">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="50c6a-595">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="50c6a-596">İstek Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-596">Request Services</span></span>

<span data-ttu-id="50c6a-597">`HttpContext` bir ASP.NET Core isteği içinde kullanılabilen hizmetler, [HttpContext. RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) koleksiyonu aracılığıyla sunulur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-597">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="50c6a-598">İstek Hizmetleri, uygulamanın bir parçası olarak yapılandırılan ve istenen hizmetleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="50c6a-598">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="50c6a-599">Nesneler bağımlılıklar belirttiğinizde, bunlar `ApplicationServices`değil `RequestServices`bulunan türler tarafından karşılanır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-599">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="50c6a-600">Genellikle, uygulamanın bu özellikleri doğrudan kullanmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-600">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="50c6a-601">Bunun yerine, sınıfların Sınıf oluşturucuları aracılığıyla gerektirdiği türleri isteyin ve çerçevenin bağımlılıkları eklemesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-601">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="50c6a-602">Bu, test etmek daha kolay olan sınıfları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="50c6a-602">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="50c6a-603">`RequestServices` koleksiyonuna erişmek için Oluşturucu parametreleri olarak bağımlılıklar istemeyi tercih edin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-603">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="50c6a-604">Bağımlılık ekleme için tasarım hizmetleri</span><span class="sxs-lookup"><span data-stu-id="50c6a-604">Design services for dependency injection</span></span>

<span data-ttu-id="50c6a-605">En iyi uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="50c6a-605">Best practices are to:</span></span>

* <span data-ttu-id="50c6a-606">Bağımlılıklarını almak için bağımlılık ekleme 'yi kullanmak üzere Hizmetleri tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-606">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="50c6a-607">Durum bilgisi olan statik sınıflar ve Üyeler kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="50c6a-607">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="50c6a-608">Genel durum oluşturulmasını önlemek yerine, tek tek Hizmetleri kullanmak için uygulamaları tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="50c6a-608">Design apps to use singleton services instead, which avoid creating global state.</span></span>
* <span data-ttu-id="50c6a-609">Hizmetler içindeki bağımlı sınıfların doğrudan örneklenmesini önleyin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-609">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="50c6a-610">Doğrudan örnekleme kodu belirli bir uygulamaya bağar.</span><span class="sxs-lookup"><span data-stu-id="50c6a-610">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="50c6a-611">Uygulama sınıflarını küçük, iyi bir şekilde ve kolayca test edin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-611">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="50c6a-612">Bir sınıfta çok fazla sayıda bağımlılık varsa, bu genellikle sınıfta çok fazla sorumluluk olduğu ve [tek sorumluluk ilkesini (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility)ihlal eden bir imzadır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-612">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="50c6a-613">Bazı sorumlulukları yeni bir sınıfa taşıyarak sınıfı yeniden düzenleme girişimi.</span><span class="sxs-lookup"><span data-stu-id="50c6a-613">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="50c6a-614">Razor Pages sayfa modeli sınıfları ve MVC denetleyici sınıflarının UI kaygılarıyla odaklanıp ilgilenmeyeceğini aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="50c6a-614">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="50c6a-615">İş kuralları ve veri erişimi uygulama ayrıntıları, bu [ayrı kaygılara](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)uygun sınıflarda tutulmalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-615">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="50c6a-616">Hizmetlerin elden çıkarılması</span><span class="sxs-lookup"><span data-stu-id="50c6a-616">Disposal of services</span></span>

<span data-ttu-id="50c6a-617">Kapsayıcı, oluşturduğu <xref:System.IDisposable> türleri için <xref:System.IDisposable.Dispose*> çağırır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-617">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="50c6a-618">Kapsayıcıda Kullanıcı kodu tarafından bir örnek eklenirse, otomatik olarak atılamaz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-618">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

<span data-ttu-id="50c6a-619">Aşağıdaki örnekte, hizmetler hizmet kapsayıcısı tarafından oluşturulur ve otomatik olarak elden alınır:</span><span class="sxs-lookup"><span data-stu-id="50c6a-619">In the following example, the services are created by the service container and disposed automatically:</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public interface IService3 {}
public class Service3 : IService3, IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<IService3>(sp => new Service3());
}
```

<span data-ttu-id="50c6a-620">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="50c6a-620">In the following example:</span></span>

* <span data-ttu-id="50c6a-621">Hizmet örnekleri hizmet kapsayıcısı tarafından oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-621">The service instances aren't created by the service container.</span></span>
* <span data-ttu-id="50c6a-622">Hedeflenen hizmet yaşam süreleri Framework tarafından bilinmemektedir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-622">The intended service lifetimes aren't known by the framework.</span></span>
* <span data-ttu-id="50c6a-623">Framework Hizmetleri otomatik olarak atmaz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-623">The framework doesn't dispose of the services automatically.</span></span>
* <span data-ttu-id="50c6a-624">Hizmetler, geliştirici kodunda açıkça atılmadıklarında, uygulama kapatılıncaya kadar kalır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-624">If the services aren't explicitly disposed in developer code, they persist until the app shuts down.</span></span>

```csharp
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<Service1>(new Service1());
    services.AddSingleton(new Service2());
}
```

## <a name="default-service-container-replacement"></a><span data-ttu-id="50c6a-625">Varsayılan hizmet kapsayıcısı değiştirme</span><span class="sxs-lookup"><span data-stu-id="50c6a-625">Default service container replacement</span></span>

<span data-ttu-id="50c6a-626">Yerleşik hizmet kapsayıcısı, çerçeve ihtiyaçlarına ve çoğu tüketici uygulamasına hizmet vermek için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-626">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="50c6a-627">Yerleşik kapsayıcının desteklemediği belirli bir özelliğe ihtiyacınız yoksa, yerleşik kapsayıcının kullanılması önerilir, örneğin:</span><span class="sxs-lookup"><span data-stu-id="50c6a-627">We recommend using the built-in container unless you need a specific feature that the built-in container doesn't support, such as:</span></span>

* <span data-ttu-id="50c6a-628">Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="50c6a-628">Property injection</span></span>
* <span data-ttu-id="50c6a-629">Ada göre ekleme</span><span class="sxs-lookup"><span data-stu-id="50c6a-629">Injection based on name</span></span>
* <span data-ttu-id="50c6a-630">Alt kapsayıcılar</span><span class="sxs-lookup"><span data-stu-id="50c6a-630">Child containers</span></span>
* <span data-ttu-id="50c6a-631">Özel ömür yönetimi</span><span class="sxs-lookup"><span data-stu-id="50c6a-631">Custom lifetime management</span></span>
* <span data-ttu-id="50c6a-632">yavaş başlatma için `Func<T>` desteği</span><span class="sxs-lookup"><span data-stu-id="50c6a-632">`Func<T>` support for lazy initialization</span></span>
* <span data-ttu-id="50c6a-633">Kural tabanlı kayıt</span><span class="sxs-lookup"><span data-stu-id="50c6a-633">Convention-based registration</span></span>

<span data-ttu-id="50c6a-634">Aşağıdaki 3. taraf kapsayıcıları ASP.NET Core uygulamalarla kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="50c6a-634">The following 3rd party containers can be used with ASP.NET Core apps:</span></span>

* [<span data-ttu-id="50c6a-635">Autofac</span><span class="sxs-lookup"><span data-stu-id="50c6a-635">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
* [<span data-ttu-id="50c6a-636">Drıioc</span><span class="sxs-lookup"><span data-stu-id="50c6a-636">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
* [<span data-ttu-id="50c6a-637">Yetkisiz kullanım</span><span class="sxs-lookup"><span data-stu-id="50c6a-637">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
* [<span data-ttu-id="50c6a-638">Açık Inject</span><span class="sxs-lookup"><span data-stu-id="50c6a-638">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
* [<span data-ttu-id="50c6a-639">E</span><span class="sxs-lookup"><span data-stu-id="50c6a-639">Lamar</span></span>](https://jasperfx.github.io/lamar/)
* [<span data-ttu-id="50c6a-640">Stakıbox</span><span class="sxs-lookup"><span data-stu-id="50c6a-640">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
* [<span data-ttu-id="50c6a-641">Unity</span><span class="sxs-lookup"><span data-stu-id="50c6a-641">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

### <a name="thread-safety"></a><span data-ttu-id="50c6a-642">İş parçacığı güvenliği</span><span class="sxs-lookup"><span data-stu-id="50c6a-642">Thread safety</span></span>

<span data-ttu-id="50c6a-643">İş parçacığı güvenli Singleton Hizmetleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="50c6a-643">Create thread-safe singleton services.</span></span> <span data-ttu-id="50c6a-644">Tek bir hizmetin geçici bir hizmete bağımlılığı varsa, geçici hizmet aynı zamanda tek tek tarafından nasıl kullanıldığına bağlı olarak iş parçacığı güvenliği de gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-644">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="50c6a-645">Tek bir hizmetin fabrika yöntemi (örneğin, [AddSingleton\<TService > (ısevicecollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), iş parçacığı açısından güvenli olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="50c6a-645">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="50c6a-646">Bir tür (`static`) Oluşturucusu gibi, tek bir iş parçacığı tarafından bir kez çağrılması garanti edilir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-646">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="50c6a-647">Öneriler</span><span class="sxs-lookup"><span data-stu-id="50c6a-647">Recommendations</span></span>

* <span data-ttu-id="50c6a-648">`async/await` ve `Task` tabanlı hizmet çözümlemesi desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="50c6a-648">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="50c6a-649">C#zaman uyumsuz oluşturucuları desteklemez; Bu nedenle, önerilen model hizmeti zaman uyumlu olarak çözümledikten sonra zaman uyumsuz yöntemler kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-649">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="50c6a-650">Veri ve yapılandırmayı doğrudan hizmet kapsayıcısında saklamaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="50c6a-650">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="50c6a-651">Örneğin, bir kullanıcının alışveriş sepeti genellikle hizmet kapsayıcısına eklenmemelidir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-651">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="50c6a-652">Yapılandırma, [Seçenekler modelini](xref:fundamentals/configuration/options)kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-652">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="50c6a-653">Benzer şekilde, yalnızca başka bir nesneye erişime izin vermek için mevcut olan "veri sahibi" nesnelerinden kaçının.</span><span class="sxs-lookup"><span data-stu-id="50c6a-653">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="50c6a-654">DI aracılığıyla gerçek öğe istemek daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="50c6a-654">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="50c6a-655">Hizmetlere statik erişimi önleyin (örneğin, başka bir yerde kullanmak üzere, statik olarak yazılan [IApplicationBuilder. ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) ).</span><span class="sxs-lookup"><span data-stu-id="50c6a-655">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="50c6a-656">[Denetim](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) stratejilerini kapsayan *hizmet bulucu deseninin*kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="50c6a-656">Avoid using the *service locator pattern*, which mixes [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

  * <span data-ttu-id="50c6a-657">Yerine şunu kullandığınızda bir hizmet örneği almak için <xref:System.IServiceProvider.GetService*> çağırmayın:</span><span class="sxs-lookup"><span data-stu-id="50c6a-657">Don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

    <span data-ttu-id="50c6a-658">**Olmayan**</span><span class="sxs-lookup"><span data-stu-id="50c6a-658">**Incorrect:**</span></span>

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

    <span data-ttu-id="50c6a-659">**Doğru**:</span><span class="sxs-lookup"><span data-stu-id="50c6a-659">**Correct**:</span></span>

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

  * <span data-ttu-id="50c6a-660"><xref:System.IServiceProvider.GetService*>kullanarak çalışma zamanında bağımlılıkları çözümleyen bir fabrika ekleme önleyin.</span><span class="sxs-lookup"><span data-stu-id="50c6a-660">Avoid injecting a factory that resolves dependencies at runtime using <xref:System.IServiceProvider.GetService*>.</span></span>

* <span data-ttu-id="50c6a-661">`HttpContext` statik erişimden kaçının (örneğin, [ıhttpcontextaccessor. HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="50c6a-661">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="50c6a-662">Tüm öneri kümeleri gibi, bir öneriyi yok saymayı yok saymış durumlarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-662">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="50c6a-663">Özel durumlar genellikle Framework içindeki özel durumlar&mdash;nadir olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="50c6a-663">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="50c6a-664">Dı, statik/genel nesne erişim desenlerinin bir *alternatifidir* .</span><span class="sxs-lookup"><span data-stu-id="50c6a-664">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="50c6a-665">Statik nesne erişimi ile karıştırırsanız, dı 'nin avantajlarını fark edemeyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50c6a-665">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="50c6a-666">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="50c6a-666">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="50c6a-667">Bağımlılık ekleme (MSDN) ile ASP.NET Core temizleme kodu yazma</span><span class="sxs-lookup"><span data-stu-id="50c6a-667">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="50c6a-668">Açık bağımlılıklar Ilkesi</span><span class="sxs-lookup"><span data-stu-id="50c6a-668">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="50c6a-669">Denetim kapsayıcıları ve bağımlılık ekleme deseninin Inversion 'ı (Marwler)</span><span class="sxs-lookup"><span data-stu-id="50c6a-669">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="50c6a-670">ASP.NET Core DI 'de birden çok arabirime sahip bir hizmeti kaydetme</span><span class="sxs-lookup"><span data-stu-id="50c6a-670">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)

::: moniker-end
