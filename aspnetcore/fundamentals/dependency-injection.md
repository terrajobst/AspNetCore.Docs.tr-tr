---
title: ASP.NET core'da bağımlılık ekleme
author: guardrex
description: ASP.NET Core bağımlılık ekleme nasıl uyguladığını ve nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/09/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: 9293de38dcca1c0672f9cc3defa8d3c1b0b13d5a
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855893"
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="3c892-103">ASP.NET core'da bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="3c892-103">Dependency injection in ASP.NET Core</span></span>

<span data-ttu-id="3c892-104">Tarafından [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3c892-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3c892-105">ASP.NET Core destekleyen bir tekniktir elde etmek için bağımlılık ekleme (dı) yazılım tasarım deseni [denetimi tersine çevirme (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) sınıfları ve bunların bağımlılıklarını arasında.</span><span class="sxs-lookup"><span data-stu-id="3c892-105">ASP.NET Core supports the dependency injection (DI) software design pattern, which is a technique for achieving [Inversion of Control (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) between classes and their dependencies.</span></span>

<span data-ttu-id="3c892-106">Özel bağımlılık ekleme MVC denetleyicileri içinde daha fazla bilgi için bkz. <xref:mvc/controllers/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="3c892-106">For more information specific to dependency injection within MVC controllers, see <xref:mvc/controllers/dependency-injection>.</span></span>

<span data-ttu-id="3c892-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3c892-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="overview-of-dependency-injection"></a><span data-ttu-id="3c892-108">Bağımlılık ekleme genel bakış</span><span class="sxs-lookup"><span data-stu-id="3c892-108">Overview of dependency injection</span></span>

<span data-ttu-id="3c892-109">A *bağımlılık* olan başka bir nesneye gerektiren herhangi bir nesne.</span><span class="sxs-lookup"><span data-stu-id="3c892-109">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="3c892-110">Aşağıdaki inceleyin `MyDependency` sınıfıyla birlikte bir `WriteMessage` diğer sınıfların bir uygulamada bağlı yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c892-110">Examine the following `MyDependency` class with a `WriteMessage` method that other classes in an app depend upon:</span></span>

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

<span data-ttu-id="3c892-111">Örneği `MyDependency` sınıfı hale getirmek için oluşturulabilir `WriteMessage` yöntemi bir sınıf için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3c892-111">An instance of the `MyDependency` class can be created to make the `WriteMessage` method available to a class.</span></span> <span data-ttu-id="3c892-112">`MyDependency` Sınıfı, bağımlılık olarak `IndexModel` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3c892-112">The `MyDependency` class is a dependency of the `IndexModel` class:</span></span>

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

<span data-ttu-id="3c892-113">Bir sınıf oluşturur ve doğrudan bağlı `MyDependency` örneği.</span><span class="sxs-lookup"><span data-stu-id="3c892-113">The class creates and directly depends on the `MyDependency` instance.</span></span> <span data-ttu-id="3c892-114">Kod bağımlılıklarını (örneğin, önceki örnekte) sorunlu ve aşağıdaki nedenlerle kaçınılmalıdır:</span><span class="sxs-lookup"><span data-stu-id="3c892-114">Code dependencies (such as the previous example) are problematic and should be avoided for the following reasons:</span></span>

* <span data-ttu-id="3c892-115">Değiştirilecek `MyDependency` sınıfı farklı bir uygulama ile değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3c892-115">To replace `MyDependency` with a different implementation, the class must be modified.</span></span>
* <span data-ttu-id="3c892-116">Varsa `MyDependency` bağımlılıkları varsa, sınıf tarafından yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3c892-116">If `MyDependency` has dependencies, they must be configured by the class.</span></span> <span data-ttu-id="3c892-117">Yapılandırmanıza bağlı olarak birden çok sınıf ile büyük bir projedeki `MyDependency`, uygulama yapılandırma kodu dağılmış olur.</span><span class="sxs-lookup"><span data-stu-id="3c892-117">In a large project with multiple classes depending on `MyDependency`, the configuration code becomes scattered across the app.</span></span>
* <span data-ttu-id="3c892-118">Bu uygulama için birim testi zordur.</span><span class="sxs-lookup"><span data-stu-id="3c892-118">This implementation is difficult to unit test.</span></span> <span data-ttu-id="3c892-119">Uygulamayı bir sahte kullanın veya saplama `MyDependency` sınıfı bu yaklaşımı mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="3c892-119">The app should use a mock or stub `MyDependency` class, which isn't possible with this approach.</span></span>

<span data-ttu-id="3c892-120">Bağımlılık ekleme aracılığıyla bu sorunları ele alır:</span><span class="sxs-lookup"><span data-stu-id="3c892-120">Dependency injection addresses these problems through:</span></span>

* <span data-ttu-id="3c892-121">Bağımlılık uygulama soyutlamak için bir arabirim veya temel sınıfının kullanın.</span><span class="sxs-lookup"><span data-stu-id="3c892-121">The use of an interface or base class to abstract the dependency implementation.</span></span>
* <span data-ttu-id="3c892-122">Bir hizmet kapsayıcısı bağımlılığı kaydı.</span><span class="sxs-lookup"><span data-stu-id="3c892-122">Registration of the dependency in a service container.</span></span> <span data-ttu-id="3c892-123">ASP.NET Core sağlayan bir yerleşik hizmet kapsayıcı <xref:System.IServiceProvider>.</span><span class="sxs-lookup"><span data-stu-id="3c892-123">ASP.NET Core provides a built-in service container, <xref:System.IServiceProvider>.</span></span> <span data-ttu-id="3c892-124">Hizmetleri uygulamanın kayıtlı `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c892-124">Services are registered in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="3c892-125">*Ekleme* hizmetinin, kullanıldığı sınıfının oluşturucusu, içine.</span><span class="sxs-lookup"><span data-stu-id="3c892-125">*Injection* of the service into the constructor of the class where it's used.</span></span> <span data-ttu-id="3c892-126">Framework bağımlılığı örneği oluşturma ve artık gerekli değilse bunun atılması sorumluluğu alır.</span><span class="sxs-lookup"><span data-stu-id="3c892-126">The framework takes on the responsibility of creating an instance of the dependency and disposing of it when it's no longer needed.</span></span>

<span data-ttu-id="3c892-127">İçinde [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` arabirim uygulamasına hizmet sağlayan bir yöntem tanımlar:</span><span class="sxs-lookup"><span data-stu-id="3c892-127">In the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), the `IMyDependency` interface defines a method that the service provides to the app:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

<span data-ttu-id="3c892-128">Bu arabirimin somut bir tür tarafından uygulanan `MyDependency`:</span><span class="sxs-lookup"><span data-stu-id="3c892-128">This interface is implemented by a concrete type, `MyDependency`:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

<span data-ttu-id="3c892-129">`MyDependency` istekler bir <xref:Microsoft.Extensions.Logging.ILogger`1> kendi oluşturucusuna.</span><span class="sxs-lookup"><span data-stu-id="3c892-129">`MyDependency` requests an <xref:Microsoft.Extensions.Logging.ILogger`1> in its constructor.</span></span> <span data-ttu-id="3c892-130">Bağımlılık ekleme zincirleme bir şekilde kullanmak alışılmadık bir durum değildir.</span><span class="sxs-lookup"><span data-stu-id="3c892-130">It's not unusual to use dependency injection in a chained fashion.</span></span> <span data-ttu-id="3c892-131">İstenen her bağımlılık sırayla kendi bağımlılıkları ister.</span><span class="sxs-lookup"><span data-stu-id="3c892-131">Each requested dependency in turn requests its own dependencies.</span></span> <span data-ttu-id="3c892-132">Kapsayıcı graftaki bağımlılıklarını çözen ve tam olarak çözümlenmiş hizmeti döndürür.</span><span class="sxs-lookup"><span data-stu-id="3c892-132">The container resolves the dependencies in the graph and returns the fully resolved service.</span></span> <span data-ttu-id="3c892-133">Toplu kümesi çözümlenemedi bağımlılıklar, tipik olarak adlandırılır bir *Bağımlılık ağacı*, *bağımlılık grafiği*, veya *Nesne grafiği*.</span><span class="sxs-lookup"><span data-stu-id="3c892-133">The collective set of dependencies that must be resolved is typically referred to as a *dependency tree*, *dependency graph*, or *object graph*.</span></span>

<span data-ttu-id="3c892-134">`IMyDependency` ve `ILogger<TCategoryName>` service kapsayıcısında kayıtlı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3c892-134">`IMyDependency` and `ILogger<TCategoryName>` must be registered in the service container.</span></span> <span data-ttu-id="3c892-135">`IMyDependency` kaydedilmiştir `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3c892-135">`IMyDependency` is registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="3c892-136">`ILogger<TCategoryName>` senaryomuz için günlük soyutlama altyapısı tarafından kayıtlı bir [framework tarafından sağlanan hizmet](#framework-provided-services) framework tarafından varsayılan olarak kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="3c892-136">`ILogger<TCategoryName>` is registered by the logging abstractions infrastructure, so it's a [framework-provided service](#framework-provided-services) registered by default by the framework.</span></span>

<span data-ttu-id="3c892-137">Kapsayıcı çözümler `ILogger<TCategoryName>` yararlanarak [(Genel) açık türler](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), kaydedilecek ihtiyacını her [(Genel) oluşturulan türü](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span><span class="sxs-lookup"><span data-stu-id="3c892-137">The container resolves `ILogger<TCategoryName>` by taking advantage of [(generic) open types](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), eliminating the need to register every [(generic) constructed type](/dotnet/csharp/language-reference/language-specification/types#constructed-types):</span></span>

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

<span data-ttu-id="3c892-138">Örnek uygulamada `IMyDependency` hizmet somut bir türde ile kayıtlı `MyDependency`.</span><span class="sxs-lookup"><span data-stu-id="3c892-138">In the sample app, the `IMyDependency` service is registered with the concrete type `MyDependency`.</span></span> <span data-ttu-id="3c892-139">Tek bir isteğin ömrü hizmet ömrü kayıt kapsamları.</span><span class="sxs-lookup"><span data-stu-id="3c892-139">The registration scopes the service lifetime to the lifetime of a single request.</span></span> <span data-ttu-id="3c892-140">[Hizmet ömrü](#service-lifetimes) bu konunun ilerleyen bölümlerinde açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3c892-140">[Service lifetimes](#service-lifetimes) are described later in this topic.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> <span data-ttu-id="3c892-141">Her `services.Add{SERVICE_NAME}` genişletme yöntemi ekler (ve büyük olasılıkla yapılandırır) Hizmetleri.</span><span class="sxs-lookup"><span data-stu-id="3c892-141">Each `services.Add{SERVICE_NAME}` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="3c892-142">Örneğin, `services.AddMvc()` Razor sayfaları ve MVC gerekli hizmetleri ekler.</span><span class="sxs-lookup"><span data-stu-id="3c892-142">For example, `services.AddMvc()` adds the services Razor Pages and MVC require.</span></span> <span data-ttu-id="3c892-143">Uygulamalar bu kurala uymayan önerilir.</span><span class="sxs-lookup"><span data-stu-id="3c892-143">We recommended that apps follow this convention.</span></span> <span data-ttu-id="3c892-144">Yerleştirin alanında uzantı yöntemlerini [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) hizmet kayıtları grupları kapsüllemek için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="3c892-144">Place extension methods in the [Microsoft.Extensions.DependencyInjection](/dotnet/api/microsoft.extensions.dependencyinjection) namespace to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="3c892-145">Hizmetin Oluşturucusu gerektiriyorsa bir [yerleşik tür](/dotnet/csharp/language-reference/keywords/built-in-types-table), gibi bir `string`, türü kullanarak yerleştirilebilir [yapılandırma](xref:fundamentals/configuration/index) veya [seçenekleri deseni](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="3c892-145">If the service's constructor requires a [built in type](/dotnet/csharp/language-reference/keywords/built-in-types-table), such as a `string`, the type can be injected by using [configuration](xref:fundamentals/configuration/index) or the [options pattern](xref:fundamentals/configuration/options):</span></span>

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

<span data-ttu-id="3c892-146">Hizmet nerede ve özel bir alana atanan bir sınıfın Oluşturucusu aracılığıyla Hizmeti'nin bir örneğini istenir.</span><span class="sxs-lookup"><span data-stu-id="3c892-146">An instance of the service is requested via the constructor of a class where the service is used and assigned to a private field.</span></span> <span data-ttu-id="3c892-147">Alan sınıfı boyunca gerektiği gibi hizmete erişmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3c892-147">The field is used to access the service as necessary throughout the class.</span></span>

<span data-ttu-id="3c892-148">Örnek uygulamada `IMyDependency` örneği istenen ve hizmetin çağırmak için kullanılan `WriteMessage` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3c892-148">In the sample app, the `IMyDependency` instance is requested and used to call the service's `WriteMessage` method:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a><span data-ttu-id="3c892-149">Framework tarafından sağlanan hizmetleri</span><span class="sxs-lookup"><span data-stu-id="3c892-149">Framework-provided services</span></span>

<span data-ttu-id="3c892-150">`Startup.ConfigureServices` Yöntemi, Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri dahil olmak üzere uygulamanın kullandığı hizmetleri tanımlamak için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="3c892-150">The `Startup.ConfigureServices` method is responsible for defining the services the app uses, including platform features, such as Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="3c892-151">Başlangıçta `IServiceCollection` sağlanan `ConfigureServices` tanımlanan aşağıdaki Hizmetleri (bağlı olarak [konak nasıl yapılandırılan](xref:fundamentals/index#host)):</span><span class="sxs-lookup"><span data-stu-id="3c892-151">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/index#host)):</span></span>

| <span data-ttu-id="3c892-152">Hizmet Türü</span><span class="sxs-lookup"><span data-stu-id="3c892-152">Service Type</span></span> | <span data-ttu-id="3c892-153">Ömür</span><span class="sxs-lookup"><span data-stu-id="3c892-153">Lifetime</span></span> |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | <span data-ttu-id="3c892-154">Geçici</span><span class="sxs-lookup"><span data-stu-id="3c892-154">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | <span data-ttu-id="3c892-155">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-155">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | <span data-ttu-id="3c892-156">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-156">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | <span data-ttu-id="3c892-157">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-157">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | <span data-ttu-id="3c892-158">Geçici</span><span class="sxs-lookup"><span data-stu-id="3c892-158">Transient</span></span> |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | <span data-ttu-id="3c892-159">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-159">Singleton</span></span> |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | <span data-ttu-id="3c892-160">Geçici</span><span class="sxs-lookup"><span data-stu-id="3c892-160">Transient</span></span> |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | <span data-ttu-id="3c892-161">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-161">Singleton</span></span> |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | <span data-ttu-id="3c892-162">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-162">Singleton</span></span> |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | <span data-ttu-id="3c892-163">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-163">Singleton</span></span> |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | <span data-ttu-id="3c892-164">Geçici</span><span class="sxs-lookup"><span data-stu-id="3c892-164">Transient</span></span> |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | <span data-ttu-id="3c892-165">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-165">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | <span data-ttu-id="3c892-166">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-166">Singleton</span></span> |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | <span data-ttu-id="3c892-167">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-167">Singleton</span></span> |

<span data-ttu-id="3c892-168">Hizmet koleksiyonu genişletme yöntemi, kayıt hizmeti (ve gerekirse, bağımlı hizmetler) kullanılabilir olduğunda, kuralı tek bir kullanmaktır `Add{SERVICE_NAME}` genişletme yöntemi bu hizmet tarafından gerekli tüm hizmetleri kaydedilecek.</span><span class="sxs-lookup"><span data-stu-id="3c892-168">When a service collection extension method is available to register a service (and its dependent services, if required), the convention is to use a single `Add{SERVICE_NAME}` extension method to register all of the services required by that service.</span></span> <span data-ttu-id="3c892-169">Aşağıdaki kod, genişletme yöntemleri kullanarak kapsayıcıya ek hizmetleri ekleme örneğidir [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, ve <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span><span class="sxs-lookup"><span data-stu-id="3c892-169">The following code is an example of how to add additional services to the container using the extension methods [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext), <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, and <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:</span></span>

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

<span data-ttu-id="3c892-170">Daha fazla bilgi için <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> API belgelerinde sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3c892-170">For more information, see the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> class in the API documentation.</span></span>

## <a name="service-lifetimes"></a><span data-ttu-id="3c892-171">Hizmet yaşam süresi yok</span><span class="sxs-lookup"><span data-stu-id="3c892-171">Service lifetimes</span></span>

<span data-ttu-id="3c892-172">Kayıtlı her hizmet için uygun bir yaşam süresi'ni seçin.</span><span class="sxs-lookup"><span data-stu-id="3c892-172">Choose an appropriate lifetime for each registered service.</span></span> <span data-ttu-id="3c892-173">ASP.NET Core Hizmetleri ile aşağıdaki ömürleri yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="3c892-173">ASP.NET Core services can be configured with the following lifetimes:</span></span>

### <a name="transient"></a><span data-ttu-id="3c892-174">Geçici</span><span class="sxs-lookup"><span data-stu-id="3c892-174">Transient</span></span>

<span data-ttu-id="3c892-175">Geçici ömrü Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) hizmet kapsayıcısından talep her zaman oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3c892-175">Transient lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) are created each time they're requested from the service container.</span></span> <span data-ttu-id="3c892-176">Bu yaşam süresi, basit, durum bilgisi olmayan hizmetler için en iyi çalışır.</span><span class="sxs-lookup"><span data-stu-id="3c892-176">This lifetime works best for lightweight, stateless services.</span></span>

### <a name="scoped"></a><span data-ttu-id="3c892-177">Kapsamlı</span><span class="sxs-lookup"><span data-stu-id="3c892-177">Scoped</span></span>

<span data-ttu-id="3c892-178">Kapsamlı ömrü Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) (bağlantı) istemci istek bir kez oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3c892-178">Scoped lifetime services (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) are created once per client request (connection).</span></span>

> [!WARNING]
> <span data-ttu-id="3c892-179">Kapsamlı bir hizmeti bir ara yazılımında kullanılırken, hizmette oturum ekleme `Invoke` veya `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3c892-179">When using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="3c892-180">Bir tekliyi gibi davranmaya hizmet zorladığından Oluşturucu ekleme ekleme yok.</span><span class="sxs-lookup"><span data-stu-id="3c892-180">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span> <span data-ttu-id="3c892-181">Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="3c892-181">For more information, see <xref:fundamentals/middleware/index>.</span></span>

### <a name="singleton"></a><span data-ttu-id="3c892-182">singleton</span><span class="sxs-lookup"><span data-stu-id="3c892-182">Singleton</span></span>

<span data-ttu-id="3c892-183">Singleton ömrü Hizmetleri (<xref:Microsoft.AspNet.OData.Builder.ODataModelBuilder.AddSingleton*>) istenen ilk kez oluşturulur (veya `Startup.ConfigureServices` çalıştırılır ve örneği hizmet kaydı ile belirtilir).</span><span class="sxs-lookup"><span data-stu-id="3c892-183">Singleton lifetime services (<xref:Microsoft.AspNet.OData.Builder.ODataModelBuilder.AddSingleton*>) are created the first time they're requested (or when `Startup.ConfigureServices` is run and an instance is specified with the service registration).</span></span> <span data-ttu-id="3c892-184">Sonraki her istek, aynı örneğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="3c892-184">Every subsequent request uses the same instance.</span></span> <span data-ttu-id="3c892-185">Uygulama tekil davranışı gerektiriyorsa, hizmetin ömrünü yönetmek hizmet kapsayıcı izin vererek önerilir.</span><span class="sxs-lookup"><span data-stu-id="3c892-185">If the app requires singleton behavior, allowing the service container to manage the service's lifetime is recommended.</span></span> <span data-ttu-id="3c892-186">Yoksa, tekil tasarım desenini uygulama ve sınıfında nesnenin ömrünü yönetmek üzere kullanıcı kodunun sağlayın.</span><span class="sxs-lookup"><span data-stu-id="3c892-186">Don't implement the singleton design pattern and provide user code to manage the object's lifetime in the class.</span></span>

> [!WARNING]
> <span data-ttu-id="3c892-187">Tek bir kapsamlı hizmet çözümlenecek tehlikelidir.</span><span class="sxs-lookup"><span data-stu-id="3c892-187">It's dangerous to resolve a scoped service from a singleton.</span></span> <span data-ttu-id="3c892-188">Bu, sonraki istekleri işleme sırasında hatalı duruma sahip hizmetinin neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="3c892-188">It may cause the service to have incorrect state when processing subsequent requests.</span></span>

## <a name="service-registration-methods"></a><span data-ttu-id="3c892-189">Hizmet kayıt yöntemleri</span><span class="sxs-lookup"><span data-stu-id="3c892-189">Service registration methods</span></span>

<span data-ttu-id="3c892-190">Her bir hizmet kayıt genişletme yöntemi, belirli senaryolarda yararlı olan aşırı yüklemeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="3c892-190">Each service registration extension method offers overloads that are useful in specific scenarios.</span></span>

| <span data-ttu-id="3c892-191">Yöntem</span><span class="sxs-lookup"><span data-stu-id="3c892-191">Method</span></span> | <span data-ttu-id="3c892-192">Otomatik</span><span class="sxs-lookup"><span data-stu-id="3c892-192">Automatic</span></span><br><span data-ttu-id="3c892-193">nesne</span><span class="sxs-lookup"><span data-stu-id="3c892-193">object</span></span><br><span data-ttu-id="3c892-194">çıkarma</span><span class="sxs-lookup"><span data-stu-id="3c892-194">disposal</span></span> | <span data-ttu-id="3c892-195">Birden Çok</span><span class="sxs-lookup"><span data-stu-id="3c892-195">Multiple</span></span><br><span data-ttu-id="3c892-196">uygulamalar</span><span class="sxs-lookup"><span data-stu-id="3c892-196">implementations</span></span> | <span data-ttu-id="3c892-197">Bağımsız değişken geçirme</span><span class="sxs-lookup"><span data-stu-id="3c892-197">Pass args</span></span> |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br><span data-ttu-id="3c892-198">Örnek:</span><span class="sxs-lookup"><span data-stu-id="3c892-198">Example:</span></span><br>`services.AddScoped<IMyDep, MyDep>();` | <span data-ttu-id="3c892-199">Evet</span><span class="sxs-lookup"><span data-stu-id="3c892-199">Yes</span></span> | <span data-ttu-id="3c892-200">Evet</span><span class="sxs-lookup"><span data-stu-id="3c892-200">Yes</span></span> | <span data-ttu-id="3c892-201">Hayır</span><span class="sxs-lookup"><span data-stu-id="3c892-201">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br><span data-ttu-id="3c892-202">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="3c892-202">Examples:</span></span><br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | <span data-ttu-id="3c892-203">Evet</span><span class="sxs-lookup"><span data-stu-id="3c892-203">Yes</span></span> | <span data-ttu-id="3c892-204">Evet</span><span class="sxs-lookup"><span data-stu-id="3c892-204">Yes</span></span> | <span data-ttu-id="3c892-205">Evet</span><span class="sxs-lookup"><span data-stu-id="3c892-205">Yes</span></span> |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br><span data-ttu-id="3c892-206">Örnek:</span><span class="sxs-lookup"><span data-stu-id="3c892-206">Example:</span></span><br>`services.AddScoped<MyDep>();` | <span data-ttu-id="3c892-207">Evet</span><span class="sxs-lookup"><span data-stu-id="3c892-207">Yes</span></span> | <span data-ttu-id="3c892-208">Hayır</span><span class="sxs-lookup"><span data-stu-id="3c892-208">No</span></span> | <span data-ttu-id="3c892-209">Hayır</span><span class="sxs-lookup"><span data-stu-id="3c892-209">No</span></span> |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br><span data-ttu-id="3c892-210">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="3c892-210">Examples:</span></span><br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | <span data-ttu-id="3c892-211">Hayır</span><span class="sxs-lookup"><span data-stu-id="3c892-211">No</span></span> | <span data-ttu-id="3c892-212">Evet</span><span class="sxs-lookup"><span data-stu-id="3c892-212">Yes</span></span> | <span data-ttu-id="3c892-213">Evet</span><span class="sxs-lookup"><span data-stu-id="3c892-213">Yes</span></span> |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br><span data-ttu-id="3c892-214">Örnekler:</span><span class="sxs-lookup"><span data-stu-id="3c892-214">Examples:</span></span><br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | <span data-ttu-id="3c892-215">Hayır</span><span class="sxs-lookup"><span data-stu-id="3c892-215">No</span></span> | <span data-ttu-id="3c892-216">Hayır</span><span class="sxs-lookup"><span data-stu-id="3c892-216">No</span></span> | <span data-ttu-id="3c892-217">Evet</span><span class="sxs-lookup"><span data-stu-id="3c892-217">Yes</span></span> |

<span data-ttu-id="3c892-218">Tür çıkarma hakkında daha fazla bilgi için bkz. [Hizmetleri elden](#disposal-of-services) bölümü.</span><span class="sxs-lookup"><span data-stu-id="3c892-218">For more information on type disposal, see the [Disposal of services](#disposal-of-services) section.</span></span> <span data-ttu-id="3c892-219">Birden çok uygulamaları için yaygın bir senaryodur [türleri test etmek için sahte işlem](xref:test/integration-tests#inject-mock-services).</span><span class="sxs-lookup"><span data-stu-id="3c892-219">A common scenario for multiple implementations is [mocking types for testing](xref:test/integration-tests#inject-mock-services).</span></span>

<span data-ttu-id="3c892-220">`TryAdd{LIFETIME}` kayıtlı bir uygulama zaten yoksa yöntem yalnızca hizmet kaydedin.</span><span class="sxs-lookup"><span data-stu-id="3c892-220">`TryAdd{LIFETIME}` methods only register the service if there isn't already an implementation registered.</span></span>

<span data-ttu-id="3c892-221">Aşağıdaki örnekte, ilk satırı kaydeder `MyDependency` için `IMyDependency`.</span><span class="sxs-lookup"><span data-stu-id="3c892-221">In the following example, the first line registers `MyDependency` for `IMyDependency`.</span></span> <span data-ttu-id="3c892-222">İkinci satır, çünkü hiçbir etkisi olmaz `IMyDependency` zaten kayıtlı bir uygulama vardır:</span><span class="sxs-lookup"><span data-stu-id="3c892-222">The second line has no effect because `IMyDependency` already has a registered implementation:</span></span>

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

<span data-ttu-id="3c892-223">Daha fazla bilgi için bkz.:</span><span class="sxs-lookup"><span data-stu-id="3c892-223">For more information, see:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

<span data-ttu-id="3c892-224">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) yöntemi uygulaması zaten yoksa hizmet yalnızca kaydetmek *aynı türde*.</span><span class="sxs-lookup"><span data-stu-id="3c892-224">[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) methods only register the service if there isn't already an implementation *of the same type*.</span></span> <span data-ttu-id="3c892-225">Birden çok hizmeti aracılığıyla çözümlenir `IEnumerable<{SERVICE}>`.</span><span class="sxs-lookup"><span data-stu-id="3c892-225">Multiple services are resolved via `IEnumerable<{SERVICE}>`.</span></span> <span data-ttu-id="3c892-226">Geliştirici Hizmetleri kaydedilirken yalnızca bir aynı türden zaten eklenmemişse, örnek eklemek istiyor.</span><span class="sxs-lookup"><span data-stu-id="3c892-226">When registering services, the developer only wants to add an instance if one of the same type hasn't already been added.</span></span> <span data-ttu-id="3c892-227">Genellikle, bu yöntem, kitaplık yazarları tarafından örneği iki kopyasını kapsayıcıda kaydetme önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3c892-227">Generally, this method is used by library authors to avoid registering two copies of an instance in the container.</span></span>

<span data-ttu-id="3c892-228">Aşağıdaki örnekte, ilk satırı kaydeder `MyDep` için `IMyDep1`.</span><span class="sxs-lookup"><span data-stu-id="3c892-228">In the following example, the first line registers `MyDep` for `IMyDep1`.</span></span> <span data-ttu-id="3c892-229">İkinci satır kaydeder `MyDep` için `IMyDep2`.</span><span class="sxs-lookup"><span data-stu-id="3c892-229">The second line registers `MyDep` for `IMyDep2`.</span></span> <span data-ttu-id="3c892-230">Üçüncü satır, çünkü hiçbir etkisi olmaz `IMyDep1` kayıtlı bir uygulaması zaten `MyDep`:</span><span class="sxs-lookup"><span data-stu-id="3c892-230">The third line has no effect because `IMyDep1` already has a registered implementation of `MyDep`:</span></span>

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a><span data-ttu-id="3c892-231">Oluşturucu yerleştirme davranışı</span><span class="sxs-lookup"><span data-stu-id="3c892-231">Constructor injection behavior</span></span>

<span data-ttu-id="3c892-232">Hizmetleri iki mekanizma tarafından çözülebilir:</span><span class="sxs-lookup"><span data-stu-id="3c892-232">Services can be resolved by two mechanisms:</span></span>

* <xref:System.IServiceProvider>
* <span data-ttu-id="3c892-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Nesne oluşturma bağımlılık ekleme kapsayıcısına hizmet kaydı olmadan izin verir.</span><span class="sxs-lookup"><span data-stu-id="3c892-233"><xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Permits object creation without service registration in the dependency injection container.</span></span> <span data-ttu-id="3c892-234">`ActivatorUtilities` Etiket Yardımcıları, MVC denetleyicileri ve model bağlayıcılar gibi kullanıcıya yönelik soyutlama ile kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3c892-234">`ActivatorUtilities` is used with user-facing abstractions, such as Tag Helpers, MVC controllers, and model binders.</span></span>

<span data-ttu-id="3c892-235">Oluşturucular, bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bağımsız değişken varsayılan değerleri atamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3c892-235">Constructors can accept arguments that aren't provided by dependency injection, but the arguments must assign default values.</span></span>

<span data-ttu-id="3c892-236">Ne zaman Hizmetleri çözülmüş tarafından `IServiceProvider` veya `ActivatorUtilities`, oluşturucu ekleme gerektiren bir *genel* Oluşturucusu.</span><span class="sxs-lookup"><span data-stu-id="3c892-236">When services are resolved by `IServiceProvider` or `ActivatorUtilities`, constructor injection requires a *public* constructor.</span></span>

<span data-ttu-id="3c892-237">Ne zaman Hizmetleri çözülmüş tarafından `ActivatorUtilities`, oluşturucu ekleme gerektirir, yalnızca bir geçerli oluşturucusu yok.</span><span class="sxs-lookup"><span data-stu-id="3c892-237">When services are resolved by `ActivatorUtilities`, constructor injection requires that only one applicable constructor exists.</span></span> <span data-ttu-id="3c892-238">Oluşturucu aşırı yüklemeleri tarafından desteklenir, ancak yalnızca bir aşırı yükleme bağımsız değişkenleri tüm bağımlılık ekleme tarafından yerine getirilmesi bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="3c892-238">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span>

## <a name="entity-framework-contexts"></a><span data-ttu-id="3c892-239">Entity Framework bağlamları</span><span class="sxs-lookup"><span data-stu-id="3c892-239">Entity Framework contexts</span></span>

<span data-ttu-id="3c892-240">Entity Framework bağlamları, hizmeti kullanarak kapsayıcı genellikle eklenir [kapsamlı ömrü](#service-lifetimes) web uygulama veritabanı işlemlerini normalde istemci isteğini kapsamındaki olduğundan.</span><span class="sxs-lookup"><span data-stu-id="3c892-240">Entity Framework contexts are usually added to the service container using the [scoped lifetime](#service-lifetimes) because web app database operations are normally scoped to the client request.</span></span> <span data-ttu-id="3c892-241">Bir yaşam süresi tarafından belirtilmezse varsayılan yaşam süresi kapsamlıdır bir [AddDbContext\<TContext >](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) veritabanı bağlamı kaydederken aşırı yükleme.</span><span class="sxs-lookup"><span data-stu-id="3c892-241">The default lifetime is scoped if a lifetime isn't specified by an [AddDbContext\<TContext>](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) overload when registering the database context.</span></span> <span data-ttu-id="3c892-242">Belirli bir yaşam süresi Hizmetleri, hizmet daha kısa bir yaşam süresi ile bir veritabanı bağlamı kullanmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="3c892-242">Services of a given lifetime shouldn't use a database context with a shorter lifetime than the service.</span></span>

## <a name="lifetime-and-registration-options"></a><span data-ttu-id="3c892-243">Yaşam süresi ve kayıt seçenekleri</span><span class="sxs-lookup"><span data-stu-id="3c892-243">Lifetime and registration options</span></span>

<span data-ttu-id="3c892-244">Yaşam süresi ve kayıt seçenekleri arasındaki farkı göstermek için benzersiz bir tanımlayıcıya sahip bir işlem olarak görevleri temsil eden aşağıdaki arabirimlerinden göz önünde bulundurun. `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="3c892-244">To demonstrate the difference between the lifetime and registration options, consider the following interfaces that represent tasks as an operation with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="3c892-245">Kapsayıcı, bir işlem hizmeti kullanım ömrü için aşağıdaki arabirimlerinden nasıl yapılandırıldığına bağlı olarak, aynı veya farklı bir örneğine bir sınıf tarafından istendiğinde hizmetinin sağlar:</span><span class="sxs-lookup"><span data-stu-id="3c892-245">Depending on how the lifetime of an operations service is configured for the following interfaces, the container provides either the same or a different instance of the service when requested by a class:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

<span data-ttu-id="3c892-246">Arabirimler uygulanan `Operation` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3c892-246">The interfaces are implemented in the `Operation` class.</span></span> <span data-ttu-id="3c892-247">`Operation` Oluşturucusu bir sağlanan değilse bir GUID oluşturur:</span><span class="sxs-lookup"><span data-stu-id="3c892-247">The `Operation` constructor generates a GUID if one isn't supplied:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

<span data-ttu-id="3c892-248">Bir `OperationService` kayıtlı, bağlı her diğer `Operation` türleri.</span><span class="sxs-lookup"><span data-stu-id="3c892-248">An `OperationService` is registered that depends on each of the other `Operation` types.</span></span> <span data-ttu-id="3c892-249">Zaman `OperationService` istenen bağımlılık ekleme aldığı her hizmetin yeni bir örneğini veya bağımlı hizmetin lifetime öğesine göre var olan bir örneği.</span><span class="sxs-lookup"><span data-stu-id="3c892-249">When `OperationService` is requested via dependency injection, it receives either a new instance of each service or an existing instance based on the lifetime of the dependent service.</span></span>

* <span data-ttu-id="3c892-250">Geçici Hizmetleri kapsayıcısından istendiğinde oluşturulduğunda `OperationId` , `IOperationTransient` hizmetidir farklı `OperationId` , `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="3c892-250">When transient services are created when requested from the container, the `OperationId` of the `IOperationTransient` service is different than the `OperationId` of the `OperationService`.</span></span> <span data-ttu-id="3c892-251">`OperationService` Yeni bir örneğini alır `IOperationTransient` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3c892-251">`OperationService` receives a new instance of the `IOperationTransient` class.</span></span> <span data-ttu-id="3c892-252">Yeni örnek farklı bir verir `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="3c892-252">The new instance yields a different `OperationId`.</span></span>
* <span data-ttu-id="3c892-253">Her istemci isteği, kapsamı belirlenmiş hizmetler oluştururken `OperationId` , `IOperationScoped` hizmeti aynı olan `OperationService` içinde bir istemci isteği.</span><span class="sxs-lookup"><span data-stu-id="3c892-253">When scoped services are created per client request, the `OperationId` of the `IOperationScoped` service is the same as that of `OperationService` within a client request.</span></span> <span data-ttu-id="3c892-254">İstemci istekleri arasında her iki hizmet de farklı bir paylaşım `OperationId` değeri.</span><span class="sxs-lookup"><span data-stu-id="3c892-254">Across client requests, both services share a different `OperationId` value.</span></span>
* <span data-ttu-id="3c892-255">Singleton ve tek örnekli Hizmetleri oluşturulduktan sonra ve tüm istemci isteklerini ve tüm hizmetlerde kullanılan `OperationId` tüm hizmet istekler genelinde sabittir.</span><span class="sxs-lookup"><span data-stu-id="3c892-255">When singleton and singleton-instance services are created once and used across all client requests and all services, the `OperationId` is constant across all service requests.</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

<span data-ttu-id="3c892-256">İçinde `Startup.ConfigureServices`, her tür kapsayıcı adlandırılmış ömrü göre eklenir:</span><span class="sxs-lookup"><span data-stu-id="3c892-256">In `Startup.ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

<span data-ttu-id="3c892-257">`IOperationSingletonInstance` Hizmetinin belirli bir örneği, bilinen bir kimliği ile kullandığı `Guid.Empty`.</span><span class="sxs-lookup"><span data-stu-id="3c892-257">The `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty`.</span></span> <span data-ttu-id="3c892-258">Bu tür (GUID'sine sıfır olur) kullanımda olmadığında işaretlenmemiştir.</span><span class="sxs-lookup"><span data-stu-id="3c892-258">It's clear when this type is in use (its GUID is all zeroes).</span></span>

<span data-ttu-id="3c892-259">Örnek uygulama, nesne kullanım ömrü içindeki ve arasındaki tek tek istekleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="3c892-259">The sample app demonstrates object lifetimes within and between individual requests.</span></span> <span data-ttu-id="3c892-260">Örnek uygulamanın `IndexModel` her tür istekleri `IOperation` türü ve `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="3c892-260">The sample app's `IndexModel` requests each kind of `IOperation` type and the `OperationService`.</span></span> <span data-ttu-id="3c892-261">Sayfa sonra tüm sayfa modeli sınıfın ve hizmetin görüntüler `OperationId` özelliği aracılığıyla değerleri:</span><span class="sxs-lookup"><span data-stu-id="3c892-261">The page then displays all of the page model class's and service's `OperationId` values through property assignments:</span></span>

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

<span data-ttu-id="3c892-262">İki aşağıdaki çıktıda gösterildiği iki isteği sonuçları:</span><span class="sxs-lookup"><span data-stu-id="3c892-262">Two following output shows the results of two requests:</span></span>

<span data-ttu-id="3c892-263">**İlk istek:**</span><span class="sxs-lookup"><span data-stu-id="3c892-263">**First request:**</span></span>

<span data-ttu-id="3c892-264">Denetleyici işlemler:</span><span class="sxs-lookup"><span data-stu-id="3c892-264">Controller operations:</span></span>

<span data-ttu-id="3c892-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span><span class="sxs-lookup"><span data-stu-id="3c892-265">Transient: d233e165-f417-469b-a866-1cf1935d2518</span></span>  
<span data-ttu-id="3c892-266">Kapsamı: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="3c892-266">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="3c892-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="3c892-267">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="3c892-268">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="3c892-268">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="3c892-269">`OperationService` İşlemler:</span><span class="sxs-lookup"><span data-stu-id="3c892-269">`OperationService` operations:</span></span>

<span data-ttu-id="3c892-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span><span class="sxs-lookup"><span data-stu-id="3c892-270">Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64</span></span>  
<span data-ttu-id="3c892-271">Kapsamı: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span><span class="sxs-lookup"><span data-stu-id="3c892-271">Scoped: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19</span></span>  
<span data-ttu-id="3c892-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="3c892-272">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="3c892-273">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="3c892-273">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="3c892-274">**İkinci isteği:**</span><span class="sxs-lookup"><span data-stu-id="3c892-274">**Second request:**</span></span>

<span data-ttu-id="3c892-275">Denetleyici işlemler:</span><span class="sxs-lookup"><span data-stu-id="3c892-275">Controller operations:</span></span>

<span data-ttu-id="3c892-276">Geçici: b63bd538-0a37-4ff1-90ba-081c5138dda0</span><span class="sxs-lookup"><span data-stu-id="3c892-276">Transient: b63bd538-0a37-4ff1-90ba-081c5138dda0</span></span>  
<span data-ttu-id="3c892-277">Kapsamı: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="3c892-277">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="3c892-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="3c892-278">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="3c892-279">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="3c892-279">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="3c892-280">`OperationService` İşlemler:</span><span class="sxs-lookup"><span data-stu-id="3c892-280">`OperationService` operations:</span></span>

<span data-ttu-id="3c892-281">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span><span class="sxs-lookup"><span data-stu-id="3c892-281">Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf</span></span>  
<span data-ttu-id="3c892-282">Kapsamı: 31e820c5-4834-4d22-83fc-a60118acb9f4</span><span class="sxs-lookup"><span data-stu-id="3c892-282">Scoped: 31e820c5-4834-4d22-83fc-a60118acb9f4</span></span>  
<span data-ttu-id="3c892-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span><span class="sxs-lookup"><span data-stu-id="3c892-283">Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9</span></span>  
<span data-ttu-id="3c892-284">Örnek: 00000000-0000-0000-0000-000000000000</span><span class="sxs-lookup"><span data-stu-id="3c892-284">Instance: 00000000-0000-0000-0000-000000000000</span></span>

<span data-ttu-id="3c892-285">Hangi gözlemleyin `OperationId` değerleri, bir istek içinde ve istekler arasında değişir:</span><span class="sxs-lookup"><span data-stu-id="3c892-285">Observe which of the `OperationId` values vary within a request and between requests:</span></span>

* <span data-ttu-id="3c892-286">*Geçici* nesneleri farklı her zaman.</span><span class="sxs-lookup"><span data-stu-id="3c892-286">*Transient* objects are always different.</span></span> <span data-ttu-id="3c892-287">Geçici `OperationId` değeri için birinci ve ikinci istemci istekleri için hem de farklı `OperationService` işlemleri ve istemci istekleri arasında.</span><span class="sxs-lookup"><span data-stu-id="3c892-287">The transient `OperationId` value for both the first and second client requests are different for both `OperationService` operations and across client requests.</span></span> <span data-ttu-id="3c892-288">Yeni bir örneği, her bir hizmet isteğini ve istemci isteği sağlanır.</span><span class="sxs-lookup"><span data-stu-id="3c892-288">A new instance is provided to each service request and client request.</span></span>
* <span data-ttu-id="3c892-289">*Kapsamlı* nesneleri, ancak farklı bir istemci isteği içinde aynı istemci isteklerinde.</span><span class="sxs-lookup"><span data-stu-id="3c892-289">*Scoped* objects are the same within a client request but different across client requests.</span></span>
* <span data-ttu-id="3c892-290">*Singleton* nesneleri, her nesne ve bağımsız olarak her istek için aynı bir `Operation` örneği sağlanır `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="3c892-290">*Singleton* objects are the same for every object and every request regardless of whether an `Operation` instance is provided in `Startup.ConfigureServices`.</span></span>

## <a name="call-services-from-main"></a><span data-ttu-id="3c892-291">Ana arama hizmetleri</span><span class="sxs-lookup"><span data-stu-id="3c892-291">Call services from main</span></span>

<span data-ttu-id="3c892-292">Oluşturma bir <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> ile [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) uygulamanın kapsamı içinde kapsamlı bir hizmet çözümlenecek.</span><span class="sxs-lookup"><span data-stu-id="3c892-292">Create an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> with [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="3c892-293">Bu yaklaşım, başlangıçta başlatma görevleri çalıştırmak için kapsamlı bir hizmete erişmek yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="3c892-293">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="3c892-294">Aşağıdaki örnek için bir bağlam elde etme gösterir `MyScopedService` içinde `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="3c892-294">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="3c892-295">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="3c892-295">Scope validation</span></span>

<span data-ttu-id="3c892-296">Uygulama geliştirme ortamında çalışırken, varsayılan hizmet sağlayıcısını doğrulamak için denetimleri gerçekleştirir:</span><span class="sxs-lookup"><span data-stu-id="3c892-296">When the app is running in the Development environment, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="3c892-297">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="3c892-297">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="3c892-298">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değildir.</span><span class="sxs-lookup"><span data-stu-id="3c892-298">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="3c892-299">Kök hizmet sağlayıcısı oluşturulur <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3c892-299">The root service provider is created when <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> is called.</span></span> <span data-ttu-id="3c892-300">Kök hizmet sağlayıcısının bir ömür zaman sağlayıcısı uygulamayla başlar ve uygulama kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="3c892-300">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="3c892-301">Kapsamlı Hizmetleri, onları oluşturan kapsayıcı tarafından elden çıkarılmasını.</span><span class="sxs-lookup"><span data-stu-id="3c892-301">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="3c892-302">Kapsamlı bir hizmet içinde kök kapsayıcı oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından bırakılmadan olduğundan hizmet ömrü tekliye etkili bir şekilde yükseltilir.</span><span class="sxs-lookup"><span data-stu-id="3c892-302">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="3c892-303">Hizmet kapsamları doğrulama yakalar bu gibi durumlarda, `BuildServiceProvider` çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3c892-303">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="3c892-304">Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#scope-validation>.</span><span class="sxs-lookup"><span data-stu-id="3c892-304">For more information, see <xref:fundamentals/host/web-host#scope-validation>.</span></span>

## <a name="request-services"></a><span data-ttu-id="3c892-305">Hizmet isteği</span><span class="sxs-lookup"><span data-stu-id="3c892-305">Request Services</span></span>

<span data-ttu-id="3c892-306">Bir ASP.NET Core içinde kullanılabilen hizmetler request `HttpContext` aracılığıyla kullanıma [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="3c892-306">The services available within an ASP.NET Core request from `HttpContext` are exposed through the [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) collection.</span></span>

<span data-ttu-id="3c892-307">İstek Hizmetleri yapılandırılmış ve uygulamanın bir parçası istenen Hizmetleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3c892-307">Request Services represent the services configured and requested as part of the app.</span></span> <span data-ttu-id="3c892-308">Nesneleri bağımlılıkları belirttiğinizde, bu bulunan türleri tarafından karşılandığından `RequestServices`değil `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="3c892-308">When the objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="3c892-309">Genel olarak, uygulamayı doğrudan bu özellikleri kullanmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="3c892-309">Generally, the app shouldn't use these properties directly.</span></span> <span data-ttu-id="3c892-310">Bunun yerine, sınıfları sınıf oluşturucuları gerektirir ve framework izin türlerini bağımlılıkları ekleme isteği.</span><span class="sxs-lookup"><span data-stu-id="3c892-310">Instead, request the types that classes require via class constructors and allow the framework inject the dependencies.</span></span> <span data-ttu-id="3c892-311">Bu, test etmek daha kolay olan sınıfları verir.</span><span class="sxs-lookup"><span data-stu-id="3c892-311">This yields classes that are easier to test.</span></span>

> [!NOTE]
> <span data-ttu-id="3c892-312">Erişim için oluşturucu parametresi olarak bağımlılıkları isteyen tercih `RequestServices` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="3c892-312">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="3c892-313">Tasarım Hizmetleri için bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="3c892-313">Design services for dependency injection</span></span>

<span data-ttu-id="3c892-314">İçin en iyi uygulamalar şunlardır:</span><span class="sxs-lookup"><span data-stu-id="3c892-314">Best practices are to:</span></span>

* <span data-ttu-id="3c892-315">Bağımlılık ekleme bağımlılıklarını almak için kullanmak üzere hizmetlerin tasarlayın.</span><span class="sxs-lookup"><span data-stu-id="3c892-315">Design services to use dependency injection to obtain their dependencies.</span></span>
* <span data-ttu-id="3c892-316">Durum bilgisi olan ve statik yöntem çağrıları kaçının.</span><span class="sxs-lookup"><span data-stu-id="3c892-316">Avoid stateful, static method calls.</span></span>
* <span data-ttu-id="3c892-317">Bağımlı hizmetleri sınıfları doğrudan örneğinin kaçının.</span><span class="sxs-lookup"><span data-stu-id="3c892-317">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="3c892-318">Doğrudan, kodu belirli bir uygulamaya couples.</span><span class="sxs-lookup"><span data-stu-id="3c892-318">Direct instantiation couples the code to a particular implementation.</span></span>
* <span data-ttu-id="3c892-319">Uygulama sınıfları küçük, katsayıları iyi belirlenmiş ve test edilmiş kolayca yapın.</span><span class="sxs-lookup"><span data-stu-id="3c892-319">Make app classes small, well-factored, and easily tested.</span></span>

<span data-ttu-id="3c892-320">Bir sınıf çok fazla eklenen bağımlılıklara sahip gibi görünüyor. Bu genellikle bir oturum sınıfı için çok fazla sorumluluklara sahiptir ve ihlal varsa, [tek sorumluluk İlkesi'ni (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span><span class="sxs-lookup"><span data-stu-id="3c892-320">If a class seems to have too many injected dependencies, this is generally a sign that the class has too many responsibilities and is violating the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="3c892-321">Sınıfının yeni bir sınıf bazı sorumlulukları taşıyarak yeniden deneyin.</span><span class="sxs-lookup"><span data-stu-id="3c892-321">Attempt to refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="3c892-322">Razor sayfaları sayfa modeli sınıfları ve MVC denetleyici sınıflarına kullanıcı Arabirimi konuları üzerinde durmalısınız aklınızda bulundurun.</span><span class="sxs-lookup"><span data-stu-id="3c892-322">Keep in mind that Razor Pages page model classes and MVC controller classes should focus on UI concerns.</span></span> <span data-ttu-id="3c892-323">İş kuralları ve veri erişim uygulama ayrıntılarını saklanır bunları uygun sınıflardaki [konuları ayrı](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span><span class="sxs-lookup"><span data-stu-id="3c892-323">Business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="3c892-324">Bırakma Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="3c892-324">Disposal of services</span></span>

<span data-ttu-id="3c892-325">Kapsayıcı çağrıları <xref:System.IDisposable.Dispose*> için <xref:System.IDisposable> türleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3c892-325">The container calls <xref:System.IDisposable.Dispose*> for the <xref:System.IDisposable> types it creates.</span></span> <span data-ttu-id="3c892-326">Bir örneği, kullanıcı kodu tarafından kapsayıcıya eklenirse, otomatik olarak elden değil.</span><span class="sxs-lookup"><span data-stu-id="3c892-326">If an instance is added to the container by user code, it isn't disposed automatically.</span></span>

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

## <a name="default-service-container-replacement"></a><span data-ttu-id="3c892-327">Varsayılan hizmet kapsayıcısını değiştirme</span><span class="sxs-lookup"><span data-stu-id="3c892-327">Default service container replacement</span></span>

<span data-ttu-id="3c892-328">Yerleşik hizmet kapsayıcı framework ve çoğu tüketici uygulamalarına gereksinimlerini karşılamak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3c892-328">The built-in service container is meant to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="3c892-329">Bunu desteklemiyor belirli bir özellik gerekmedikçe yerleşik kapsayıcı kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="3c892-329">We recommend using the built-in container unless you need a specific feature that it doesn't support.</span></span> <span data-ttu-id="3c892-330">Yerleşik kapsayıcısında bulunamadı 3 taraf kapsayıcıları destekleyen özelliklerden bazıları:</span><span class="sxs-lookup"><span data-stu-id="3c892-330">Some of the features supported in 3rd party containers not found in the built-in container:</span></span>

* <span data-ttu-id="3c892-331">Özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="3c892-331">Property injection</span></span>
* <span data-ttu-id="3c892-332">Adına göre ekleme</span><span class="sxs-lookup"><span data-stu-id="3c892-332">Injection based on name</span></span>
* <span data-ttu-id="3c892-333">Alt kapsayıcılar</span><span class="sxs-lookup"><span data-stu-id="3c892-333">Child containers</span></span>
* <span data-ttu-id="3c892-334">Özel ömür Yönetimi</span><span class="sxs-lookup"><span data-stu-id="3c892-334">Custom lifetime management</span></span>
* <span data-ttu-id="3c892-335">`Func<T>` Yavaş başlatma desteği</span><span class="sxs-lookup"><span data-stu-id="3c892-335">`Func<T>` support for lazy initialization</span></span>

<span data-ttu-id="3c892-336">Bkz: [bağımlılık ekleme Benioku.MD dosyası](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) bazı bağdaştırıcıları destekleyen kapsayıcıların listesi.</span><span class="sxs-lookup"><span data-stu-id="3c892-336">See the [Dependency Injection readme.md file](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) for a list of some of the containers that support adapters.</span></span>

<span data-ttu-id="3c892-337">Aşağıdaki örnek yerleşik kapsayıcıyla değiştirir [Autofac](https://autofac.org/):</span><span class="sxs-lookup"><span data-stu-id="3c892-337">The following sample replaces the built-in container with [Autofac](https://autofac.org/):</span></span>

* <span data-ttu-id="3c892-338">Uygun bir kapsayıcı paketleri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="3c892-338">Install the appropriate container package(s):</span></span>

  * [<span data-ttu-id="3c892-339">Autofac</span><span class="sxs-lookup"><span data-stu-id="3c892-339">Autofac</span></span>](https://www.nuget.org/packages/Autofac/)
  * [<span data-ttu-id="3c892-340">Autofac.Extensions.DependencyInjection</span><span class="sxs-lookup"><span data-stu-id="3c892-340">Autofac.Extensions.DependencyInjection</span></span>](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* <span data-ttu-id="3c892-341">Kapsayıcıda yapılandırma `Startup.ConfigureServices` ve dönüş bir `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="3c892-341">Configure the container in `Startup.ConfigureServices` and return an `IServiceProvider`:</span></span>

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

    <span data-ttu-id="3c892-342">3 bir taraf kapsayıcı kullanılacak `Startup.ConfigureServices` döndürmelidir `IServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="3c892-342">To use a 3rd party container, `Startup.ConfigureServices` must return `IServiceProvider`.</span></span>

* <span data-ttu-id="3c892-343">İçinde Autofac yapılandırma `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="3c892-343">Configure Autofac in `DefaultModule`:</span></span>

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

<span data-ttu-id="3c892-344">Çalışma zamanında Autofac, türleri çözümlemek ve bağımlılıkları ekleme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3c892-344">At runtime, Autofac is used to resolve types and inject dependencies.</span></span> <span data-ttu-id="3c892-345">ASP.NET Core ile Autofac kullanma hakkında daha fazla bilgi edinmek için [Autofac belgeleri](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="3c892-345">To learn more about using Autofac with ASP.NET Core, see the [Autofac documentation](https://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="3c892-346">İş parçacığı güvenliği</span><span class="sxs-lookup"><span data-stu-id="3c892-346">Thread safety</span></span>

<span data-ttu-id="3c892-347">İş parçacığı açısından güvenli tekil Hizmetleri oluşturun.</span><span class="sxs-lookup"><span data-stu-id="3c892-347">Create thread-safe singleton services.</span></span> <span data-ttu-id="3c892-348">Tek bir hizmet üzerinde geçici bir hizmet bağımlılığı varsa, geçici hizmet tekli tarafından nasıl kullanıldığını bağlı olarak iş parçacığı güvenliği de gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="3c892-348">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending how it's used by the singleton.</span></span>

<span data-ttu-id="3c892-349">İkinci bağımsız değişkeni gibi tek hizmet Üreteç yöntemi [AddSingleton\<TService > (IServiceCollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), iş parçacığı açısından güvenli olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3c892-349">The factory method of single service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), doesn't need to be thread-safe.</span></span> <span data-ttu-id="3c892-350">Gibi bir tür (`static`) oluşturucusu, onu garanti tek bir iş parçacığı tarafından bir kez çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="3c892-350">Like a type (`static`) constructor, it's guaranteed to be called once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="3c892-351">Öneriler</span><span class="sxs-lookup"><span data-stu-id="3c892-351">Recommendations</span></span>

* <span data-ttu-id="3c892-352">`async/await` ve `Task` tabanlı hizmet çözümlemesi desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="3c892-352">`async/await` and `Task` based service resolution is not supported.</span></span> <span data-ttu-id="3c892-353">C#zaman uyumsuz oluşturucuları desteklemez; Bu nedenle, önerilen Düzen zaman uyumsuz yöntemleri zaman uyumlu olarak hizmet çözdükten sonra kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="3c892-353">C# does not support asynchronous constructors; therefore, the recommended pattern is to use asynchronous methods after synchronously resolving the service.</span></span>

* <span data-ttu-id="3c892-354">Verileri ve Yapılandırma hizmeti kapsayıcısında doğrudan depolama kaçının.</span><span class="sxs-lookup"><span data-stu-id="3c892-354">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="3c892-355">Örneğin, bir kullanıcının alışveriş sepeti genellikle hizmet kapsayıcıya eklenen olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="3c892-355">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="3c892-356">Yapılandırma kullanması gereken [seçenekleri deseni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="3c892-356">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="3c892-357">Benzer şekilde, yalnızca başka bir nesnenin erişmesine izin vermek için mevcut "veri sahibi" nesneleri kaçının.</span><span class="sxs-lookup"><span data-stu-id="3c892-357">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="3c892-358">İstek DI aracılığıyla gerçek öğesi daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="3c892-358">It's better to request the actual item via DI.</span></span>

* <span data-ttu-id="3c892-359">Statik hizmetlere erişimi önlemek (örneğin, statik olarak yazmaya [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) kullanan başka bir yerde için).</span><span class="sxs-lookup"><span data-stu-id="3c892-359">Avoid static access to services (for example, statically-typing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) for use elsewhere).</span></span>

* <span data-ttu-id="3c892-360">Kullanmaktan kaçının *hizmet bulucu deseni*.</span><span class="sxs-lookup"><span data-stu-id="3c892-360">Avoid using the *service locator pattern*.</span></span> <span data-ttu-id="3c892-361">Örneğin, çağırma yoksa <xref:System.IServiceProvider.GetService*> DI yerine kullandığınızda bir hizmet örneği elde etmek için:</span><span class="sxs-lookup"><span data-stu-id="3c892-361">For example, don't invoke <xref:System.IServiceProvider.GetService*> to obtain a service instance when you can use DI instead:</span></span>

  <span data-ttu-id="3c892-362">**Yanlış:**</span><span class="sxs-lookup"><span data-stu-id="3c892-362">**Incorrect:**</span></span>

  ```csharp
  public void MyMethod()
  {
      var options = 
          _services.GetService<IOptionsMonitor<MyOptions>>();
      var option = options.CurrentValue.Option;

      ...
  }
  ```

  <span data-ttu-id="3c892-363">**Doğru**:</span><span class="sxs-lookup"><span data-stu-id="3c892-363">**Correct**:</span></span>

  ```csharp
  private readonly MyOptions _options;

  public MyClass(IOptionsMonitor<MyOptions> options)
  {
      _options = options.CurrentValue;
  }

  public void MyMethod()
  {
      var option = _options.Option;

      ...
  }
  ```

* <span data-ttu-id="3c892-364">Çalışma zamanında bağımlılıklarını çözen bir Fabrika önlemek için başka bir hizmet bulucu değişim çalıştırıyorsunuzdur.</span><span class="sxs-lookup"><span data-stu-id="3c892-364">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="3c892-365">Bu yöntemler karışımı [tersine çevirme denetim](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) stratejiler.</span><span class="sxs-lookup"><span data-stu-id="3c892-365">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>

* <span data-ttu-id="3c892-366">Statik erişimi önlemek `HttpContext` (örneğin, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span><span class="sxs-lookup"><span data-stu-id="3c892-366">Avoid static access to `HttpContext` (for example, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).</span></span>

<span data-ttu-id="3c892-367">Öneriler tüm kümesi gibi bir öneri yok sayılıyor gerekli olduğu durumlarla karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3c892-367">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="3c892-368">Özel durumlar nadir&mdash;çoğunlukla framework içinde özel durumlar.</span><span class="sxs-lookup"><span data-stu-id="3c892-368">Exceptions are rare&mdash;mostly special cases within the framework itself.</span></span>

<span data-ttu-id="3c892-369">DI olduğu bir *alternatif* statik/genel nesne erişim desenleri.</span><span class="sxs-lookup"><span data-stu-id="3c892-369">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="3c892-370">Statik nesne erişimi ile karıştırmak istiyorsanız DI avantajlarından mümkün olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="3c892-370">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3c892-371">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3c892-371">Additional resources</span></span>

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [<span data-ttu-id="3c892-372">Bağımlılık ekleme (MSDN) ile ASP.NET Core kod yazma</span><span class="sxs-lookup"><span data-stu-id="3c892-372">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="3c892-373">Özel bağımlılıklar İlkesi</span><span class="sxs-lookup"><span data-stu-id="3c892-373">Explicit Dependencies Principle</span></span>](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [<span data-ttu-id="3c892-374">Tersine çevirme denetimi kapsayıcıları ve bağımlılık ekleme desenini (Martin Fowler)</span><span class="sxs-lookup"><span data-stu-id="3c892-374">Inversion of Control Containers and the Dependency Injection Pattern (Martin Fowler)</span></span>](https://www.martinfowler.com/articles/injection.html)
* [<span data-ttu-id="3c892-375">Bir hizmet birden fazla arabirimde ASP.NET Core DI ile kaydetme</span><span class="sxs-lookup"><span data-stu-id="3c892-375">How to register a service with multiple interfaces in ASP.NET Core DI</span></span>](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
