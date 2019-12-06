---
title: ASP.NET Core Blazor bağımlılığı ekleme
author: guardrex
description: Blazor uygulamalarının bileşenlere nasıl hizmet ekleyebilmesi için bkz.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/dependency-injection
ms.openlocfilehash: 17dd0f927064ae7c2b1e3e439fd93e2cb220a5a4
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74879776"
---
# <a name="aspnet-core-opno-locblazor-dependency-injection"></a><span data-ttu-id="27b16-103">ASP.NET Core Blazor bağımlılığı ekleme</span><span class="sxs-lookup"><span data-stu-id="27b16-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="27b16-104">[Rainer Stropek](https://www.timecockpit.com) tarafından</span><span class="sxs-lookup"><span data-stu-id="27b16-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="27b16-105"> [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)işlemini destekler.</span><span class="sxs-lookup"><span data-stu-id="27b16-105"> supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="27b16-106">Uygulamalar, yerleşik hizmetleri ekleme tarafından bileşenlere kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="27b16-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="27b16-107">Uygulamalar Ayrıca özel hizmetleri tanımlayabilir ve kaydedebilir ve bu Hizmetleri uygulama genelinde DI aracılığıyla kullanılabilir hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27b16-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="27b16-108">DI, merkezi bir konumda yapılandırılmış hizmetlere erişmek için bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="27b16-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="27b16-109">Bu, Blazor uygulamalarda şu şekilde yararlı olabilir:</span><span class="sxs-lookup"><span data-stu-id="27b16-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="27b16-110">Hizmet sınıfının tek bir *örneğini tek bir hizmet olarak* bilinen birçok bileşen arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27b16-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="27b16-111">Başvuru soyutlamalarını kullanarak somut hizmet sınıflarından bileşenleri ayırın.</span><span class="sxs-lookup"><span data-stu-id="27b16-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="27b16-112">Örneğin, uygulamadaki verilere erişmek için bir arabirim `IDataAccess` düşünün.</span><span class="sxs-lookup"><span data-stu-id="27b16-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="27b16-113">Arabirim somut bir `DataAccess` sınıfı tarafından uygulanır ve uygulamanın hizmet kapsayıcısında bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="27b16-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="27b16-114">Bir bileşen bir `IDataAccess` uygulamasını almak için DI kullandığında, bileşen somut tür ile eşleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="27b16-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="27b16-115">Uygulama, büyük olasılıkla birim testlerinde bir sahte uygulama için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="27b16-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="27b16-116">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="27b16-116">Default services</span></span>

<span data-ttu-id="27b16-117">Varsayılan hizmetler, uygulamanın hizmet koleksiyonuna otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="27b16-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="27b16-118">Hizmet</span><span class="sxs-lookup"><span data-stu-id="27b16-118">Service</span></span> | <span data-ttu-id="27b16-119">Ömür</span><span class="sxs-lookup"><span data-stu-id="27b16-119">Lifetime</span></span> | <span data-ttu-id="27b16-120">Açıklama</span><span class="sxs-lookup"><span data-stu-id="27b16-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="27b16-121">Adet</span><span class="sxs-lookup"><span data-stu-id="27b16-121">Singleton</span></span> | <span data-ttu-id="27b16-122">HTTP istekleri göndermek ve bir URI tarafından tanımlanan bir kaynaktan HTTP yanıtlarını almak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="27b16-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="27b16-123">Bir Blazor WebAssembly uygulamasındaki `HttpClient` örneği, arka planda HTTP trafiğini işlemek için tarayıcıyı kullanır.</span><span class="sxs-lookup"><span data-stu-id="27b16-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br>Blazor<span data-ttu-id="27b16-124"> Server uygulamaları, varsayılan olarak hizmet olarak yapılandırılmış bir `HttpClient` içermez.</span><span class="sxs-lookup"><span data-stu-id="27b16-124"> Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="27b16-125">Blazor sunucusu uygulamasına bir `HttpClient` sağlayın.</span><span class="sxs-lookup"><span data-stu-id="27b16-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="27b16-126">Daha fazla bilgi için bkz. <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="27b16-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="27b16-127">Adet</span><span class="sxs-lookup"><span data-stu-id="27b16-127">Singleton</span></span> | <span data-ttu-id="27b16-128">JavaScript çağrılarının dağıtıldığı bir JavaScript çalışma zamanının örneğini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="27b16-128">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="27b16-129">Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="27b16-129">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="27b16-130">Adet</span><span class="sxs-lookup"><span data-stu-id="27b16-130">Singleton</span></span> | <span data-ttu-id="27b16-131">URI 'Ler ve gezinme durumu ile çalışmaya yönelik yardımcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="27b16-131">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="27b16-132">Daha fazla bilgi için bkz. [URI ve gezinti durumu yardımcıları](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="27b16-132">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="27b16-133">Özel bir hizmet sağlayıcı, tabloda listelenen varsayılan Hizmetleri otomatik olarak sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="27b16-133">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="27b16-134">Özel bir hizmet sağlayıcısı kullanır ve tabloda gösterilen hizmetlerden herhangi birini gerekliyse, gerekli hizmetleri yeni hizmet sağlayıcısına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="27b16-134">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="27b16-135">Uygulamaya hizmet ekleme</span><span class="sxs-lookup"><span data-stu-id="27b16-135">Add services to an app</span></span>

<span data-ttu-id="27b16-136">Yeni bir uygulama oluşturduktan sonra `Startup.ConfigureServices` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="27b16-136">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="27b16-137">`ConfigureServices` yöntemi, hizmet açıklayıcı nesnelerinin bir listesi olan bir <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>geçirilir (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="27b16-137">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="27b16-138">Hizmetler, hizmet koleksiyonuna hizmet tanımlayıcıları sağlayarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="27b16-138">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="27b16-139">Aşağıdaki örnek, `IDataAccess` arabirimiyle kavramı ve somut uygulama `DataAccess`gösterir:</span><span class="sxs-lookup"><span data-stu-id="27b16-139">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="27b16-140">Hizmetler, aşağıdaki tabloda gösterilen ömürlerle yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="27b16-140">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="27b16-141">Ömür</span><span class="sxs-lookup"><span data-stu-id="27b16-141">Lifetime</span></span> | <span data-ttu-id="27b16-142">Açıklama</span><span class="sxs-lookup"><span data-stu-id="27b16-142">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="27b16-143"> WebAssembly uygulamalarının Şu anda bir dı kapsamları kavramı yoktur.</span><span class="sxs-lookup"><span data-stu-id="27b16-143"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="27b16-144">`Scoped`kayıtlı hizmetler `Singleton` hizmetleri gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="27b16-144">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="27b16-145">Ancak, Blazor sunucusu barındırma modeli `Scoped` ömrünü destekler.</span><span class="sxs-lookup"><span data-stu-id="27b16-145">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="27b16-146">Blazor Server uygulamalarında, kapsamlı bir hizmet kaydı *bağlantının*kapsamına alınır.</span><span class="sxs-lookup"><span data-stu-id="27b16-146">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="27b16-147">Bu nedenle, geçerli amaç tarayıcıda istemci tarafı çalıştırmak olsa bile, kapsama alınmış hizmetlerin kullanılması geçerli kullanıcı kapsamında olması gereken hizmetler için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="27b16-147">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="27b16-148">Dı, hizmetin *tek bir örneğini* oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27b16-148">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="27b16-149">Bir `Singleton` hizmeti gerektiren tüm bileşenler aynı hizmetin bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="27b16-149">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="27b16-150">Bir bileşen hizmet kapsayıcısından bir `Transient` hizmeti örneği aldığında, hizmetin *Yeni bir örneğini* alır.</span><span class="sxs-lookup"><span data-stu-id="27b16-150">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="27b16-151">Dı sistemi ASP.NET Core içindeki DI sistemini temel alır.</span><span class="sxs-lookup"><span data-stu-id="27b16-151">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="27b16-152">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="27b16-152">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="27b16-153">Bir bileşende hizmet isteme</span><span class="sxs-lookup"><span data-stu-id="27b16-153">Request a service in a component</span></span>

<span data-ttu-id="27b16-154">Hizmetler hizmet koleksiyonuna eklendikten sonra, [\@Ekle](xref:mvc/views/razor#inject) Razor yönergesini kullanarak hizmetleri bileşenlere ekleyin.</span><span class="sxs-lookup"><span data-stu-id="27b16-154">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="27b16-155">`@inject` iki parametreye sahiptir:</span><span class="sxs-lookup"><span data-stu-id="27b16-155">`@inject` has two parameters:</span></span>

* <span data-ttu-id="27b16-156">Eklenecek hizmetin türünü &ndash; yazın.</span><span class="sxs-lookup"><span data-stu-id="27b16-156">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="27b16-157">Özelliği, eklenen App Service 'i alan özelliğin adını &ndash;.</span><span class="sxs-lookup"><span data-stu-id="27b16-157">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="27b16-158">Özelliği el ile oluşturma gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="27b16-158">The property doesn't require manual creation.</span></span> <span data-ttu-id="27b16-159">Derleyici özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27b16-159">The compiler creates the property.</span></span>

<span data-ttu-id="27b16-160">Daha fazla bilgi için bkz. <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="27b16-160">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="27b16-161">Farklı hizmetler eklemek için birden çok `@inject` deyimi kullanın.</span><span class="sxs-lookup"><span data-stu-id="27b16-161">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="27b16-162">Aşağıdaki örnek `@inject`nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="27b16-162">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="27b16-163">`Services.IDataAccess` uygulayan hizmet bileşenin Özellik `DataRepository`eklenir.</span><span class="sxs-lookup"><span data-stu-id="27b16-163">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="27b16-164">Kodun yalnızca `IDataAccess` soyutlamasını nasıl kullandığını aklınızda yapın:</span><span class="sxs-lookup"><span data-stu-id="27b16-164">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="27b16-165">Dahili olarak, oluşturulan Özellik (`DataRepository`) `InjectAttribute` özniteliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="27b16-165">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="27b16-166">Genellikle, bu öznitelik doğrudan kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="27b16-166">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="27b16-167">Bileşenler için bir temel sınıf gerekliyse ve temel sınıf için eklenen özellikler de gerekliyse, `InjectAttribute`el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="27b16-167">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="27b16-168">Temel sınıftan türetilmiş bileşenlerde `@inject` yönergesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="27b16-168">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="27b16-169">Temel sınıfın `InjectAttribute` yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="27b16-169">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="27b16-170">Hizmetler 'de dı kullanma</span><span class="sxs-lookup"><span data-stu-id="27b16-170">Use DI in services</span></span>

<span data-ttu-id="27b16-171">Karmaşık hizmetler için ek hizmetler gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="27b16-171">Complex services might require additional services.</span></span> <span data-ttu-id="27b16-172">Önceki örnekte, `DataAccess` `HttpClient` varsayılan hizmeti gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="27b16-172">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="27b16-173">`@inject` (veya `InjectAttribute`), hizmetlerde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="27b16-173">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="27b16-174">Bunun yerine *Oluşturucu Ekleme* kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="27b16-174">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="27b16-175">Gerekli hizmetler, hizmetin oluşturucusuna parametreler eklenerek eklenir.</span><span class="sxs-lookup"><span data-stu-id="27b16-175">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="27b16-176">Dı hizmeti oluşturduğunda, oluşturucuda gereken hizmetleri algılar ve bunlara göre sağlar.</span><span class="sxs-lookup"><span data-stu-id="27b16-176">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="27b16-177">Oluşturucu Ekleme önkoşulları:</span><span class="sxs-lookup"><span data-stu-id="27b16-177">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="27b16-178">Bağımsız değişkenlerinin tümü DI tarafından yerine getirilme için tek bir Oluşturucu bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="27b16-178">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="27b16-179">Varsayılan değerleri belirttiklerinde, DI tarafından kapsanmayan ek parametrelere izin verilir.</span><span class="sxs-lookup"><span data-stu-id="27b16-179">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="27b16-180">Uygulanabilir Oluşturucu *ortak*olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="27b16-180">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="27b16-181">Uygulanabilir bir Oluşturucu var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="27b16-181">One applicable constructor must exist.</span></span> <span data-ttu-id="27b16-182">Belirsizlik söz konusu olduğunda, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27b16-182">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="27b16-183">Bir dı kapsamını yönetmek için yardımcı program temel bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="27b16-183">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="27b16-184">ASP.NET Core uygulamalarda, kapsamlı hizmetler genellikle geçerli isteğin kapsamlandırılır.</span><span class="sxs-lookup"><span data-stu-id="27b16-184">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="27b16-185">İstek tamamlandıktan sonra, tüm kapsamlı veya geçici hizmetler dı sistemi tarafından silinir.</span><span class="sxs-lookup"><span data-stu-id="27b16-185">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="27b16-186">Blazor Server uygulamalarında istek kapsamı, istemci bağlantısı süresince sürer ve bu da geçici ve kapsamlı hizmetlerin beklenenden çok daha uzun sürebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="27b16-186">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="27b16-187">Hizmetlerin bir bileşenin kullanım ömrüne göre kapsamını atamak için `OwningComponentBase` ve `OwningComponentBase<TService>` temel sınıfları kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="27b16-187">To scope services to the lifetime of a component, can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="27b16-188">Bu temel sınıflar, bileşenin kullanım ömrü kapsamındaki Hizmetleri çözümlemek `IServiceProvider` türünde bir `ScopedServices` özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="27b16-188">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="27b16-189">Razor 'teki bir taban sınıftan devralan bir bileşeni yazmak için `@inherits` yönergesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="27b16-189">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```cshtml
@page "/users"
@attribute [Authorize]
@inherits OwningComponentBase<Data.ApplicationDbContext>

<h1>Users (@Service.Users.Count())</h1>
<ul>
    @foreach (var user in Service.Users)
    {
        <li>@user.UserName</li>
    }
</ul>
```

> [!NOTE]
> <span data-ttu-id="27b16-190">`@inject` veya `InjectAttribute` kullanılarak bileşene eklenen hizmetler bileşen kapsamında oluşturulmaz ve istek kapsamına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="27b16-190">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27b16-191">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="27b16-191">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
