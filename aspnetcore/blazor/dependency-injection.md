---
title: ASP.NET Core Blazor bağımlılığı ekleme
author: guardrex
description: Blazor uygulamalarının bileşenlere nasıl hizmet ekleyebilmesi için bkz.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: fa6762522c831c7fbe2742dbfe4e25a377988e1e
ms.sourcegitcommit: fe41cff0b99f3920b727286944e5b652ca301640
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76869570"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="3aa1c-103">ASP.NET Core Blazor bağımlılığı ekleme</span><span class="sxs-lookup"><span data-stu-id="3aa1c-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="3aa1c-104">[Rainer Stropek](https://www.timecockpit.com) tarafından</span><span class="sxs-lookup"><span data-stu-id="3aa1c-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3aa1c-105">Blazor, [bağımlılık ekleme işlemini (dı)](xref:fundamentals/dependency-injection)destekler.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3aa1c-106">Uygulamalar, yerleşik hizmetleri ekleme tarafından bileşenlere kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="3aa1c-107">Uygulamalar Ayrıca özel hizmetleri tanımlayabilir ve kaydedebilir ve bu Hizmetleri uygulama genelinde DI aracılığıyla kullanılabilir hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="3aa1c-108">DI, merkezi bir konumda yapılandırılmış hizmetlere erişmek için bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="3aa1c-109">Bu, Blazor uygulamalarında şu şekilde yararlı olabilir:</span><span class="sxs-lookup"><span data-stu-id="3aa1c-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="3aa1c-110">Hizmet sınıfının tek bir *örneğini tek bir hizmet olarak* bilinen birçok bileşen arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="3aa1c-111">Başvuru soyutlamalarını kullanarak somut hizmet sınıflarından bileşenleri ayırın.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="3aa1c-112">Örneğin, uygulamadaki verilere erişmek için bir arabirim `IDataAccess` düşünün.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="3aa1c-113">Arabirim somut bir `DataAccess` sınıfı tarafından uygulanır ve uygulamanın hizmet kapsayıcısında bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="3aa1c-114">Bir bileşen bir `IDataAccess` uygulamasını almak için DI kullandığında, bileşen somut tür ile eşleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="3aa1c-115">Uygulama, büyük olasılıkla birim testlerinde bir sahte uygulama için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="3aa1c-116">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="3aa1c-116">Default services</span></span>

<span data-ttu-id="3aa1c-117">Varsayılan hizmetler, uygulamanın hizmet koleksiyonuna otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="3aa1c-118">Hizmet</span><span class="sxs-lookup"><span data-stu-id="3aa1c-118">Service</span></span> | <span data-ttu-id="3aa1c-119">Ömür</span><span class="sxs-lookup"><span data-stu-id="3aa1c-119">Lifetime</span></span> | <span data-ttu-id="3aa1c-120">Açıklama</span><span class="sxs-lookup"><span data-stu-id="3aa1c-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="3aa1c-121">Adet</span><span class="sxs-lookup"><span data-stu-id="3aa1c-121">Singleton</span></span> | <span data-ttu-id="3aa1c-122">HTTP istekleri göndermek ve bir URI tarafından tanımlanan bir kaynaktan HTTP yanıtlarını almak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span><br><br><span data-ttu-id="3aa1c-123">Bir Blazor WebAssembly uygulamasındaki `HttpClient` örneği, arka planda HTTP trafiğini işlemek için tarayıcıyı kullanır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-123">The instance of `HttpClient` in a Blazor WebAssembly app uses the browser for handling the HTTP traffic in the background.</span></span><br><br><span data-ttu-id="3aa1c-124">Blazor sunucu uygulamaları, varsayılan olarak hizmet olarak yapılandırılmış bir `HttpClient` içermez.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-124">Blazor Server apps don't include an `HttpClient` configured as a service by default.</span></span> <span data-ttu-id="3aa1c-125">Bir Blazor sunucu uygulamasına `HttpClient` sağlayın.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-125">Provide an `HttpClient` to a Blazor Server app.</span></span><br><br><span data-ttu-id="3aa1c-126">Daha fazla bilgi için bkz. <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-126">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="3aa1c-127">Singleton (Blazor WebAssembly)</span><span class="sxs-lookup"><span data-stu-id="3aa1c-127">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="3aa1c-128">Kapsamlı (Blazor sunucusu)</span><span class="sxs-lookup"><span data-stu-id="3aa1c-128">Scoped (Blazor Server)</span></span> | <span data-ttu-id="3aa1c-129">JavaScript çağrılarının dağıtıldığı bir JavaScript çalışma zamanının örneğini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-129">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="3aa1c-130">Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-130">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `NavigationManager` | <span data-ttu-id="3aa1c-131">Singleton (Blazor WebAssembly)</span><span class="sxs-lookup"><span data-stu-id="3aa1c-131">Singleton (Blazor WebAssembly)</span></span><br><span data-ttu-id="3aa1c-132">Kapsamlı (Blazor sunucusu)</span><span class="sxs-lookup"><span data-stu-id="3aa1c-132">Scoped (Blazor Server)</span></span> | <span data-ttu-id="3aa1c-133">URI 'Ler ve gezinme durumu ile çalışmaya yönelik yardımcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-133">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="3aa1c-134">Daha fazla bilgi için bkz. [URI ve gezinti durumu yardımcıları](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="3aa1c-134">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="3aa1c-135">Özel bir hizmet sağlayıcı, tabloda listelenen varsayılan Hizmetleri otomatik olarak sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-135">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="3aa1c-136">Özel bir hizmet sağlayıcısı kullanır ve tabloda gösterilen hizmetlerden herhangi birini gerekliyse, gerekli hizmetleri yeni hizmet sağlayıcısına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-136">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="3aa1c-137">Uygulamaya hizmet ekleme</span><span class="sxs-lookup"><span data-stu-id="3aa1c-137">Add services to an app</span></span>

<span data-ttu-id="3aa1c-138">Yeni bir uygulama oluşturduktan sonra `Startup.ConfigureServices` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="3aa1c-138">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="3aa1c-139">`ConfigureServices` yöntemi, hizmet açıklayıcı nesnelerinin bir listesi olan bir <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>geçirilir (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="3aa1c-139">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="3aa1c-140">Hizmetler, hizmet koleksiyonuna hizmet tanımlayıcıları sağlayarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-140">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="3aa1c-141">Aşağıdaki örnek, `IDataAccess` arabirimiyle kavramı ve somut uygulama `DataAccess`gösterir:</span><span class="sxs-lookup"><span data-stu-id="3aa1c-141">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="3aa1c-142">Hizmetler, aşağıdaki tabloda gösterilen ömürlerle yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-142">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="3aa1c-143">Ömür</span><span class="sxs-lookup"><span data-stu-id="3aa1c-143">Lifetime</span></span> | <span data-ttu-id="3aa1c-144">Açıklama</span><span class="sxs-lookup"><span data-stu-id="3aa1c-144">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor<span data-ttu-id="3aa1c-145"> WebAssembly uygulamalarının Şu anda bir dı kapsamları kavramı yoktur.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-145"> WebAssembly apps don't currently have a concept of DI scopes.</span></span> <span data-ttu-id="3aa1c-146">`Scoped`kayıtlı hizmetler `Singleton` hizmetleri gibi davranır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-146">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="3aa1c-147">Ancak, Blazor sunucusu barındırma modeli `Scoped` ömrünü destekler.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-147">However, the Blazor Server hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="3aa1c-148">Blazor Server uygulamalarında, kapsamlı bir hizmet kaydı *bağlantının*kapsamına alınır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-148">In Blazor Server apps, a scoped service registration is scoped to the *connection*.</span></span> <span data-ttu-id="3aa1c-149">Bu nedenle, geçerli amaç tarayıcıda istemci tarafı çalıştırmak olsa bile, kapsama alınmış hizmetlerin kullanılması geçerli kullanıcı kapsamında olması gereken hizmetler için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-149">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="3aa1c-150">Dı, hizmetin *tek bir örneğini* oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-150">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="3aa1c-151">Bir `Singleton` hizmeti gerektiren tüm bileşenler aynı hizmetin bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-151">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="3aa1c-152">Bir bileşen hizmet kapsayıcısından bir `Transient` hizmeti örneği aldığında, hizmetin *Yeni bir örneğini* alır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-152">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="3aa1c-153">Dı sistemi ASP.NET Core içindeki DI sistemini temel alır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-153">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="3aa1c-154">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-154">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="3aa1c-155">Bir bileşende hizmet isteme</span><span class="sxs-lookup"><span data-stu-id="3aa1c-155">Request a service in a component</span></span>

<span data-ttu-id="3aa1c-156">Hizmetler hizmet koleksiyonuna eklendikten sonra, [\@Ekle](xref:mvc/views/razor#inject) Razor yönergesini kullanarak hizmetleri bileşenlere ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-156">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="3aa1c-157">`@inject` iki parametreye sahiptir:</span><span class="sxs-lookup"><span data-stu-id="3aa1c-157">`@inject` has two parameters:</span></span>

* <span data-ttu-id="3aa1c-158">Eklenecek hizmetin türünü &ndash; yazın.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-158">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="3aa1c-159">Özelliği, eklenen App Service 'i alan özelliğin adını &ndash;.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-159">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="3aa1c-160">Özelliği el ile oluşturma gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-160">The property doesn't require manual creation.</span></span> <span data-ttu-id="3aa1c-161">Derleyici özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-161">The compiler creates the property.</span></span>

<span data-ttu-id="3aa1c-162">Daha fazla bilgi için bkz. <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-162">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="3aa1c-163">Farklı hizmetler eklemek için birden çok `@inject` deyimi kullanın.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-163">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="3aa1c-164">Aşağıdaki örnek `@inject`nasıl kullanacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-164">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="3aa1c-165">`Services.IDataAccess` uygulayan hizmet bileşenin Özellik `DataRepository`eklenir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-165">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="3aa1c-166">Kodun yalnızca `IDataAccess` soyutlamasını nasıl kullandığını aklınızda yapın:</span><span class="sxs-lookup"><span data-stu-id="3aa1c-166">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="3aa1c-167">Dahili olarak, oluşturulan Özellik (`DataRepository`) `InjectAttribute` özniteliğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-167">Internally, the generated property (`DataRepository`) uses the `InjectAttribute` attribute.</span></span> <span data-ttu-id="3aa1c-168">Genellikle, bu öznitelik doğrudan kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-168">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="3aa1c-169">Bileşenler için bir temel sınıf gerekliyse ve temel sınıf için eklenen özellikler de gerekliyse, `InjectAttribute`el ile ekleyin:</span><span class="sxs-lookup"><span data-stu-id="3aa1c-169">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="3aa1c-170">Temel sınıftan türetilmiş bileşenlerde `@inject` yönergesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-170">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="3aa1c-171">Temel sınıfın `InjectAttribute` yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="3aa1c-171">The `InjectAttribute` of the base class is sufficient:</span></span>

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="3aa1c-172">Hizmetler 'de dı kullanma</span><span class="sxs-lookup"><span data-stu-id="3aa1c-172">Use DI in services</span></span>

<span data-ttu-id="3aa1c-173">Karmaşık hizmetler için ek hizmetler gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-173">Complex services might require additional services.</span></span> <span data-ttu-id="3aa1c-174">Önceki örnekte, `DataAccess` `HttpClient` varsayılan hizmeti gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-174">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="3aa1c-175">`@inject` (veya `InjectAttribute`), hizmetlerde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-175">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="3aa1c-176">Bunun yerine *Oluşturucu Ekleme* kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-176">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="3aa1c-177">Gerekli hizmetler, hizmetin oluşturucusuna parametreler eklenerek eklenir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-177">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="3aa1c-178">Dı hizmeti oluşturduğunda, oluşturucuda gereken hizmetleri algılar ve bunlara göre sağlar.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-178">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="3aa1c-179">Oluşturucu Ekleme önkoşulları:</span><span class="sxs-lookup"><span data-stu-id="3aa1c-179">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="3aa1c-180">Bağımsız değişkenlerinin tümü DI tarafından yerine getirilme için tek bir Oluşturucu bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-180">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="3aa1c-181">Varsayılan değerleri belirttiklerinde, DI tarafından kapsanmayan ek parametrelere izin verilir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-181">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="3aa1c-182">Uygulanabilir Oluşturucu *ortak*olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-182">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="3aa1c-183">Uygulanabilir bir Oluşturucu var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-183">One applicable constructor must exist.</span></span> <span data-ttu-id="3aa1c-184">Belirsizlik söz konusu olduğunda, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-184">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a><span data-ttu-id="3aa1c-185">Bir dı kapsamını yönetmek için yardımcı program temel bileşen sınıfları</span><span class="sxs-lookup"><span data-stu-id="3aa1c-185">Utility base component classes to manage a DI scope</span></span>

<span data-ttu-id="3aa1c-186">ASP.NET Core uygulamalarda, kapsamlı hizmetler genellikle geçerli isteğin kapsamlandırılır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-186">In ASP.NET Core apps, scoped services are typically scoped to the current request.</span></span> <span data-ttu-id="3aa1c-187">İstek tamamlandıktan sonra, tüm kapsamlı veya geçici hizmetler dı sistemi tarafından silinir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-187">After the request completes, any scoped or transient services are disposed by the DI system.</span></span> <span data-ttu-id="3aa1c-188">Blazor Server uygulamalarında istek kapsamı, istemci bağlantısı süresince sürer ve bu da geçici ve kapsamlı hizmetlerin beklenenden çok daha uzun sürebileceği anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-188">In Blazor Server apps, the request scope lasts for the duration of the client connection, which can result in transient and scoped services living much longer than expected.</span></span>

<span data-ttu-id="3aa1c-189">Hizmetlerin bir bileşenin kullanım ömrüne göre kapsamını atamak için, `OwningComponentBase` ve `OwningComponentBase<TService>` temel sınıfları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-189">To scope services to the lifetime of a component, you can use the `OwningComponentBase` and `OwningComponentBase<TService>` base classes.</span></span> <span data-ttu-id="3aa1c-190">Bu temel sınıflar, bileşenin kullanım ömrü kapsamındaki Hizmetleri çözümlemek `IServiceProvider` türünde bir `ScopedServices` özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-190">These base classes expose a `ScopedServices` property of type `IServiceProvider` that resolve services that are scoped to the lifetime of the component.</span></span> <span data-ttu-id="3aa1c-191">Razor 'teki bir taban sınıftan devralan bir bileşeni yazmak için `@inherits` yönergesini kullanın.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-191">To author a component that inherits from a base class in Razor, use the `@inherits` directive.</span></span>

```razor
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
> <span data-ttu-id="3aa1c-192">`@inject` veya `InjectAttribute` kullanılarak bileşene eklenen hizmetler bileşen kapsamında oluşturulmaz ve istek kapsamına bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="3aa1c-192">Services injected into the component using `@inject` or the `InjectAttribute` aren't created in the component's scope and are tied to the request scope.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3aa1c-193">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3aa1c-193">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
