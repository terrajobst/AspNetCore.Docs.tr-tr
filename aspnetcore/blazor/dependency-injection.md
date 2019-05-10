---
title: Blazor bağımlılık ekleme
author: guardrex
description: Bkz. nasıl Hizmetleri bileşenlerine Blazor uygulamaları ekleyebilir.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 49948cd8e31473a4901957356d372d49fc3b0f5f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898469"
---
# <a name="blazor-dependency-injection"></a><span data-ttu-id="56256-103">Blazor bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="56256-103">Blazor dependency injection</span></span>

<span data-ttu-id="56256-104">By [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="56256-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="56256-105">Blazor destekler [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="56256-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="56256-106">Uygulamalar, yerleşik hizmetlere bileşenlerine ekleyerek kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="56256-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="56256-107">Uygulamalar ayrıca tanımlayın ve özel hizmetlerle kaydedebilir veya DI aracılığıyla uygulamayı kullanılabilir hale getirmek.</span><span class="sxs-lookup"><span data-stu-id="56256-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="56256-108">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="56256-108">Dependency injection</span></span>

<span data-ttu-id="56256-109">DI, merkezi bir konumda yapılandırılmış hizmetlerine erişmek için kullanılan bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="56256-109">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="56256-110">Bu, Blazor uygulamalarında için yararlı olabilir:</span><span class="sxs-lookup"><span data-stu-id="56256-110">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="56256-111">Bir hizmet sınıfı tek bir örneği olarak bilinen, birçok bileşen paylaşılmasını bir *singleton* hizmeti.</span><span class="sxs-lookup"><span data-stu-id="56256-111">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="56256-112">Somut hizmet sınıflardan bileşenleri, başvuru soyutlama kullanarak ayırın.</span><span class="sxs-lookup"><span data-stu-id="56256-112">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="56256-113">Örneğin, bir arabirim düşünün `IDataAccess` uygulamadaki verilere erişmek için.</span><span class="sxs-lookup"><span data-stu-id="56256-113">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="56256-114">Bir somut tarafından uygulanan arabirimi `DataAccess` sınıfı ve bir uygulamanın hizmet kapsayıcı hizmetinde olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="56256-114">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="56256-115">Bir bileşen kullandığında DI almak için bir `IDataAccess` uygulaması bileşeni, somut bir türde eşleşmiş değil.</span><span class="sxs-lookup"><span data-stu-id="56256-115">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="56256-116">Uygulama, belki de birim testlerinde sahte bir uygulama için değişiklik yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="56256-116">The implementation can be swapped, perhaps to a mock implementation in unit tests.</span></span>

<span data-ttu-id="56256-117">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="56256-117">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="add-services-to-di"></a><span data-ttu-id="56256-118">DI için hizmet ekleyin</span><span class="sxs-lookup"><span data-stu-id="56256-118">Add services to DI</span></span>

<span data-ttu-id="56256-119">Yeni bir uygulama oluşturduktan sonra incelemek `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="56256-119">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="56256-120">`ConfigureServices` Yöntemi geçirilir bir <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, hizmet tanımlayıcısı nesneleri listesi verilmiştir (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="56256-120">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="56256-121">Hizmet, hizmet koleksiyonu için hizmet tanımlayıcıları sağlayarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="56256-121">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="56256-122">Aşağıdaki örnek, kavramı gösterir `IDataAccess` arabirimi ile somut uygulaması `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="56256-122">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="56256-123">Aşağıdaki tabloda gösterilen ömürleriyle Hizmetleri yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="56256-123">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="56256-124">Ömür</span><span class="sxs-lookup"><span data-stu-id="56256-124">Lifetime</span></span> | <span data-ttu-id="56256-125">Açıklama</span><span class="sxs-lookup"><span data-stu-id="56256-125">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="56256-126">DI oluşturur bir *tek örnek* hizmeti.</span><span class="sxs-lookup"><span data-stu-id="56256-126">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="56256-127">Tüm bileşenleri gerektiren bir `Singleton` hizmeti aynı Hizmeti'nin bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="56256-127">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="56256-128">Her bir bileşen örneği alır bir `Transient` hizmet hizmet kapsayıcısından aldığı bir *yeni örneği* hizmeti.</span><span class="sxs-lookup"><span data-stu-id="56256-128">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="56256-129">Blazor istemci-tarafı şu anda DI kapsamları kavramı yoktur.</span><span class="sxs-lookup"><span data-stu-id="56256-129">Blazor client-side doesn't currently have the concept of DI scopes.</span></span> <span data-ttu-id="56256-130">`Scoped` gibi davranır `Singleton`.</span><span class="sxs-lookup"><span data-stu-id="56256-130">`Scoped` behaves like `Singleton`.</span></span> <span data-ttu-id="56256-131">Ancak, sunucu tarafı barındırma modelini destekleyen `Scoped` yaşam süresi.</span><span class="sxs-lookup"><span data-stu-id="56256-131">However, the server-side hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="56256-132">Bir Razor bileşeninde bir kapsamlı hizmet kayıt bağlantısı kapsamlıdır.</span><span class="sxs-lookup"><span data-stu-id="56256-132">In a Razor component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="56256-133">İstemci tarafı çalıştırmak için geçerli amaç olsa bile, bu nedenle, kapsamlı hizmetlerini kullanarak geçerli kullanıcı için kapsamlı hizmetler için tercih edilir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="56256-133">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |

<span data-ttu-id="56256-134">ASP.NET Core DI sistemde DI sistem dayanır.</span><span class="sxs-lookup"><span data-stu-id="56256-134">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="56256-135">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="56256-135">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="default-services"></a><span data-ttu-id="56256-136">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="56256-136">Default services</span></span>

<span data-ttu-id="56256-137">Varsayılan hizmetler otomatik olarak uygulamanın hizmet koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="56256-137">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="56256-138">Hizmet</span><span class="sxs-lookup"><span data-stu-id="56256-138">Service</span></span> | <span data-ttu-id="56256-139">Ömür</span><span class="sxs-lookup"><span data-stu-id="56256-139">Lifetime</span></span> | <span data-ttu-id="56256-140">Açıklama</span><span class="sxs-lookup"><span data-stu-id="56256-140">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="56256-141">singleton</span><span class="sxs-lookup"><span data-stu-id="56256-141">Singleton</span></span> | <span data-ttu-id="56256-142">HTTP istekleri göndermek ve bir URI tarafından tanımlanan bir kaynaktan HTTP yanıtları almak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="56256-142">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="56256-143">Unutmayın, bu örneği `HttpClient` tarayıcı arka planda HTTP trafiğini işlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="56256-143">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="56256-144">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) uygulama taban URI öneki otomatik olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="56256-144">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="56256-145">`HttpClient` yalnızca Blazor istemci tarafı uygulamalar için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="56256-145">`HttpClient` is only provided to Blazor client-side apps.</span></span> |
| `IJSRuntime` | <span data-ttu-id="56256-146">singleton</span><span class="sxs-lookup"><span data-stu-id="56256-146">Singleton</span></span> | <span data-ttu-id="56256-147">JavaScript çağrılarını burada dağıtılmadan bir JavaScript çalışma zamanı örneğini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="56256-147">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="56256-148">Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="56256-148">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="56256-149">singleton</span><span class="sxs-lookup"><span data-stu-id="56256-149">Singleton</span></span> | <span data-ttu-id="56256-150">URI ve gezinti durumu ile çalışmak için Yardımcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="56256-150">Contains helpers for working with URIs and navigation state.</span></span> |

<span data-ttu-id="56256-151">Bir özel hizmet sağlayıcısı, tabloda listelenen varsayılan hizmetleri otomatik olarak sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="56256-151">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="56256-152">Bir özel hizmet sağlayıcısı ve tabloda gösterilen hizmetlerinden herhangi birinin gerektirir, gerekli hizmetler yeni hizmet sağlayıcısına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="56256-152">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="56256-153">Bir bileşen içinde bir hizmet isteği</span><span class="sxs-lookup"><span data-stu-id="56256-153">Request a service in a component</span></span>

<span data-ttu-id="56256-154">Hizmetleri hizmet koleksiyonuna eklendikten sonra hizmetleri kullanarak bileşenleri Razor şablonlarına ekleme [ \@ekleme](xref:mvc/views/razor#section-4) Razor yönergesi.</span><span class="sxs-lookup"><span data-stu-id="56256-154">After services are added to the service collection, inject the services into the components' Razor templates using the [\@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="56256-155">`@inject` iki parametreye sahiptir:</span><span class="sxs-lookup"><span data-stu-id="56256-155">`@inject` has two parameters:</span></span>

* <span data-ttu-id="56256-156">Tür: Eklenecek hizmetin türü.</span><span class="sxs-lookup"><span data-stu-id="56256-156">Type: The type of the service to inject.</span></span>
* <span data-ttu-id="56256-157">Özellik: Eklenen app service alma özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="56256-157">Property: The name of the property receiving the injected app service.</span></span> <span data-ttu-id="56256-158">El ile oluşturma özelliğini gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="56256-158">The property doesn't require manual creation.</span></span> <span data-ttu-id="56256-159">Derleyici, bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="56256-159">The compiler creates the property.</span></span>

<span data-ttu-id="56256-160">Daha fazla bilgi için bkz. <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="56256-160">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="56256-161">Birden çok kullanın `@inject` farklı Hizmetleri eklemesine deyimleri.</span><span class="sxs-lookup"><span data-stu-id="56256-161">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="56256-162">Aşağıdaki örnek nasıl kullanılacağını gösterir `@inject`.</span><span class="sxs-lookup"><span data-stu-id="56256-162">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="56256-163">Uygulama hizmeti `Services.IDataAccess` bileşenin özelliğine eklenen `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="56256-163">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="56256-164">Kodu yalnızca nasıl kullandığını unutmayın `IDataAccess` Özet:</span><span class="sxs-lookup"><span data-stu-id="56256-164">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="56256-165">Dahili olarak oluşturulan özelliğe (`DataRepository`) ile donatılmış `InjectAttribute` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="56256-165">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="56256-166">Genellikle, bu öznitelik, doğrudan kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="56256-166">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="56256-167">Bir temel sınıf bileşenleri için gereklidir ve eklenen özellikler için temel sınıf gerekli Ayrıca, el ile eklemeniz `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="56256-167">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="56256-168">Temel sınıftan türetilmiş bileşenlerinde `@inject` yönergesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="56256-168">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="56256-169">`InjectAttribute` Temel sınıfını yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="56256-169">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a><span data-ttu-id="56256-170">Hizmetleri bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="56256-170">Dependency injection in services</span></span>

<span data-ttu-id="56256-171">Karmaşık services ek hizmetleri gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="56256-171">Complex services might require additional services.</span></span> <span data-ttu-id="56256-172">Önceki örnekte, `DataAccess` gerektirebilir `HttpClient` varsayılan hizmeti.</span><span class="sxs-lookup"><span data-stu-id="56256-172">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="56256-173">`@inject` (veya `InjectAttribute`) hizmetlerini kullanmak için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="56256-173">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="56256-174">*Oluşturucu ekleme* yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="56256-174">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="56256-175">Gerekli hizmetler, hizmetin oluşturucusuna parametre ekleyerek eklenir.</span><span class="sxs-lookup"><span data-stu-id="56256-175">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="56256-176">Bağımlılık ekleme hizmet oluşturduğu zaman, oluşturucuda gerektirir ve uygun şekilde sağlayan hizmetler tanır.</span><span class="sxs-lookup"><span data-stu-id="56256-176">When dependency injection creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
}
```

<span data-ttu-id="56256-177">Oluşturucu ekleme önkoşulları:</span><span class="sxs-lookup"><span data-stu-id="56256-177">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="56256-178">Bir oluşturucu bağımsız değişkenleri tüm bağımlılık ekleme tarafından yerine getirilmesi mevcut olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="56256-178">One constructor must exist whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="56256-179">Bunlar varsayılan değerleri belirtirseniz DI tarafından kapsanmayan ek parametreler izin verilir.</span><span class="sxs-lookup"><span data-stu-id="56256-179">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="56256-180">Geçerli bir oluşturucusu olmalıdır *genel*.</span><span class="sxs-lookup"><span data-stu-id="56256-180">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="56256-181">Geçerli bir oluşturucusu bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="56256-181">One applicable constructor must exist.</span></span> <span data-ttu-id="56256-182">Bir belirsizlik durumunda DI özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="56256-182">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56256-183">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="56256-183">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
