---
title: ASP.NET Core Blazor bağımlılık ekleme
author: guardrex
description: Bkz. nasıl Hizmetleri bileşenlerine Blazor uygulamaları ekleyebilir.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 394d99656ba52f6c561007121e63634c7cb84f37
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538472"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="d525e-103">ASP.NET Core Blazor bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="d525e-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="d525e-104">By [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="d525e-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="d525e-105">Blazor destekler [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d525e-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d525e-106">Uygulamalar, yerleşik hizmetlere bileşenlerine ekleyerek kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d525e-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="d525e-107">Uygulamalar ayrıca tanımlayın ve özel hizmetlerle kaydedebilir veya DI aracılığıyla uygulamayı kullanılabilir hale getirmek.</span><span class="sxs-lookup"><span data-stu-id="d525e-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="d525e-108">DI, merkezi bir konumda yapılandırılmış hizmetlerine erişmek için kullanılan bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="d525e-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="d525e-109">Bu, Blazor uygulamalarında için yararlı olabilir:</span><span class="sxs-lookup"><span data-stu-id="d525e-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="d525e-110">Bir hizmet sınıfı tek bir örneği olarak bilinen, birçok bileşen paylaşılmasını bir *singleton* hizmeti.</span><span class="sxs-lookup"><span data-stu-id="d525e-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="d525e-111">Somut hizmet sınıflardan bileşenleri, başvuru soyutlama kullanarak ayırın.</span><span class="sxs-lookup"><span data-stu-id="d525e-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="d525e-112">Örneğin, bir arabirim düşünün `IDataAccess` uygulamadaki verilere erişmek için.</span><span class="sxs-lookup"><span data-stu-id="d525e-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="d525e-113">Bir somut tarafından uygulanan arabirimi `DataAccess` sınıfı ve bir uygulamanın hizmet kapsayıcı hizmetinde olarak kaydedildi.</span><span class="sxs-lookup"><span data-stu-id="d525e-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="d525e-114">Bir bileşen kullandığında DI almak için bir `IDataAccess` uygulaması bileşeni, somut bir türde eşleşmiş değil.</span><span class="sxs-lookup"><span data-stu-id="d525e-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="d525e-115">Uygulama, belki de birim testlerinde sahte bir uygulama için değişiklik yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="d525e-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="d525e-116">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="d525e-116">Default services</span></span>

<span data-ttu-id="d525e-117">Varsayılan hizmetler otomatik olarak uygulamanın hizmet koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="d525e-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="d525e-118">Hizmet</span><span class="sxs-lookup"><span data-stu-id="d525e-118">Service</span></span> | <span data-ttu-id="d525e-119">Ömür</span><span class="sxs-lookup"><span data-stu-id="d525e-119">Lifetime</span></span> | <span data-ttu-id="d525e-120">Açıklama</span><span class="sxs-lookup"><span data-stu-id="d525e-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="d525e-121">singleton</span><span class="sxs-lookup"><span data-stu-id="d525e-121">Singleton</span></span> | <span data-ttu-id="d525e-122">HTTP istekleri göndermek ve bir URI tarafından tanımlanan bir kaynaktan HTTP yanıtları almak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="d525e-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="d525e-123">Unutmayın, bu örneği `HttpClient` tarayıcı arka planda HTTP trafiğini işlemek için kullanır.</span><span class="sxs-lookup"><span data-stu-id="d525e-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="d525e-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) uygulama taban URI öneki otomatik olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="d525e-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="d525e-125">Daha fazla bilgi için bkz. <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="d525e-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="d525e-126">singleton</span><span class="sxs-lookup"><span data-stu-id="d525e-126">Singleton</span></span> | <span data-ttu-id="d525e-127">JavaScript çağrılarını burada dağıtılmadan bir JavaScript çalışma zamanı örneğini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d525e-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="d525e-128">Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="d525e-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="d525e-129">singleton</span><span class="sxs-lookup"><span data-stu-id="d525e-129">Singleton</span></span> | <span data-ttu-id="d525e-130">URI ve gezinti durumu ile çalışmak için Yardımcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="d525e-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="d525e-131">Daha fazla bilgi için [URI ve gezinti durumu Yardımcıları](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="d525e-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="d525e-132">Bir özel hizmet sağlayıcısı, tabloda listelenen varsayılan hizmetleri otomatik olarak sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="d525e-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="d525e-133">Bir özel hizmet sağlayıcısı ve tabloda gösterilen hizmetlerinden herhangi birinin gerektirir, gerekli hizmetler yeni hizmet sağlayıcısına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d525e-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="d525e-134">Hizmetler için uygulama ekleme</span><span class="sxs-lookup"><span data-stu-id="d525e-134">Add services to an app</span></span>

<span data-ttu-id="d525e-135">Yeni bir uygulama oluşturduktan sonra incelemek `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="d525e-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="d525e-136">`ConfigureServices` Yöntemi geçirilir bir <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, hizmet tanımlayıcısı nesneleri listesi verilmiştir (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span><span class="sxs-lookup"><span data-stu-id="d525e-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="d525e-137">Hizmet, hizmet koleksiyonu için hizmet tanımlayıcıları sağlayarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="d525e-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="d525e-138">Aşağıdaki örnek, kavramı gösterir `IDataAccess` arabirimi ile somut uygulaması `DataAccess`:</span><span class="sxs-lookup"><span data-stu-id="d525e-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="d525e-139">Aşağıdaki tabloda gösterilen ömürleriyle Hizmetleri yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="d525e-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="d525e-140">Ömür</span><span class="sxs-lookup"><span data-stu-id="d525e-140">Lifetime</span></span> | <span data-ttu-id="d525e-141">Açıklama</span><span class="sxs-lookup"><span data-stu-id="d525e-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="d525e-142">Blazor istemci-tarafı şu anda DI kapsamları kavramı yoktur.</span><span class="sxs-lookup"><span data-stu-id="d525e-142">Blazor client-side doesn't currently have a concept of DI scopes.</span></span> <span data-ttu-id="d525e-143">`Scoped`-kaydedilen Hizmetleri davranır gibi `Singleton` Hizmetleri.</span><span class="sxs-lookup"><span data-stu-id="d525e-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="d525e-144">Ancak, sunucu tarafı barındırma modelini destekleyen `Scoped` yaşam süresi.</span><span class="sxs-lookup"><span data-stu-id="d525e-144">However, the server-side hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="d525e-145">Bir Razor bileşeninde bir kapsamlı hizmet kayıt bağlantısı kapsamlıdır.</span><span class="sxs-lookup"><span data-stu-id="d525e-145">In a Razor component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="d525e-146">İstemci tarafı çalıştırmak için geçerli amaç olsa bile, bu nedenle, kapsamlı hizmetlerini kullanarak geçerli kullanıcı için kapsamlı hizmetler için tercih edilir tarayıcıda.</span><span class="sxs-lookup"><span data-stu-id="d525e-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="d525e-147">DI oluşturur bir *tek örnek* hizmeti.</span><span class="sxs-lookup"><span data-stu-id="d525e-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="d525e-148">Tüm bileşenleri gerektiren bir `Singleton` hizmeti aynı Hizmeti'nin bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="d525e-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="d525e-149">Her bir bileşen örneği alır bir `Transient` hizmet hizmet kapsayıcısından aldığı bir *yeni örneği* hizmeti.</span><span class="sxs-lookup"><span data-stu-id="d525e-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="d525e-150">ASP.NET Core DI sistemde DI sistem dayanır.</span><span class="sxs-lookup"><span data-stu-id="d525e-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="d525e-151">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="d525e-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="d525e-152">Bir bileşen içinde bir hizmet isteği</span><span class="sxs-lookup"><span data-stu-id="d525e-152">Request a service in a component</span></span>

<span data-ttu-id="d525e-153">Hizmetleri hizmet koleksiyonuna eklendikten sonra hizmetleri kullanarak bileşenlere ekleme [ \@ekleme](xref:mvc/views/razor#section-4) Razor yönergesi.</span><span class="sxs-lookup"><span data-stu-id="d525e-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#section-4) Razor directive.</span></span> <span data-ttu-id="d525e-154">`@inject` iki parametreye sahiptir:</span><span class="sxs-lookup"><span data-stu-id="d525e-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="d525e-155">Tür &ndash; eklenecek hizmetin türü.</span><span class="sxs-lookup"><span data-stu-id="d525e-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="d525e-156">Özellik &ndash; eklenen app service alma özelliğinin adı.</span><span class="sxs-lookup"><span data-stu-id="d525e-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="d525e-157">El ile oluşturma özelliğini gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="d525e-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="d525e-158">Derleyici, bir özellik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d525e-158">The compiler creates the property.</span></span>

<span data-ttu-id="d525e-159">Daha fazla bilgi için bkz. <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="d525e-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="d525e-160">Birden çok kullanın `@inject` farklı Hizmetleri eklemesine deyimleri.</span><span class="sxs-lookup"><span data-stu-id="d525e-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="d525e-161">Aşağıdaki örnek nasıl kullanılacağını gösterir `@inject`.</span><span class="sxs-lookup"><span data-stu-id="d525e-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="d525e-162">Uygulama hizmeti `Services.IDataAccess` bileşenin özelliğine eklenen `DataRepository`.</span><span class="sxs-lookup"><span data-stu-id="d525e-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="d525e-163">Kodu yalnızca nasıl kullandığını unutmayın `IDataAccess` Özet:</span><span class="sxs-lookup"><span data-stu-id="d525e-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="d525e-164">Dahili olarak oluşturulan özelliğe (`DataRepository`) ile donatılmış `InjectAttribute` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d525e-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="d525e-165">Genellikle, bu öznitelik, doğrudan kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="d525e-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="d525e-166">Bir temel sınıf bileşenleri için gereklidir ve eklenen özellikler için temel sınıf gerekli Ayrıca, el ile eklemeniz `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="d525e-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="d525e-167">Temel sınıftan türetilmiş bileşenlerinde `@inject` yönergesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="d525e-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="d525e-168">`InjectAttribute` Temel sınıfını yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="d525e-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="d525e-169">DI hizmetlerde kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d525e-169">Use DI in services</span></span>

<span data-ttu-id="d525e-170">Karmaşık services ek hizmetleri gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="d525e-170">Complex services might require additional services.</span></span> <span data-ttu-id="d525e-171">Önceki örnekte, `DataAccess` gerektirebilir `HttpClient` varsayılan hizmeti.</span><span class="sxs-lookup"><span data-stu-id="d525e-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="d525e-172">`@inject` (veya `InjectAttribute`) hizmetlerini kullanmak için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="d525e-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="d525e-173">*Oluşturucu ekleme* yerine kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d525e-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="d525e-174">Gerekli hizmetler, hizmetin oluşturucusuna parametre ekleyerek eklenir.</span><span class="sxs-lookup"><span data-stu-id="d525e-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="d525e-175">DI hizmet oluşturduğu zaman, oluşturucuda gerektirir ve uygun şekilde sağlayan hizmetler tanır.</span><span class="sxs-lookup"><span data-stu-id="d525e-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="d525e-176">Oluşturucu ekleme önkoşulları:</span><span class="sxs-lookup"><span data-stu-id="d525e-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="d525e-177">Bir oluşturucu bağımsız değişkenleri tüm DI tarafından yerine getirilmesi mevcut olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d525e-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="d525e-178">Bunlar varsayılan değerleri belirtirseniz DI tarafından kapsanmayan ek parametreler izin verilir.</span><span class="sxs-lookup"><span data-stu-id="d525e-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="d525e-179">Geçerli bir oluşturucusu olmalıdır *genel*.</span><span class="sxs-lookup"><span data-stu-id="d525e-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="d525e-180">Geçerli bir oluşturucusu bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d525e-180">One applicable constructor must exist.</span></span> <span data-ttu-id="d525e-181">Bir belirsizlik durumunda DI özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d525e-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d525e-182">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d525e-182">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
