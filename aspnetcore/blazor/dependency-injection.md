---
title: ASP.NET Core Blazor bağımlılığı ekleme
author: guardrex
description: Bkz. Blazor Apps 'in bileşenlere nasıl hizmet ekleyebilmesi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2019
uid: blazor/dependency-injection
ms.openlocfilehash: a9330caa81eec0910206312283b3424c70cd1289
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819773"
---
# <a name="aspnet-core-blazor-dependency-injection"></a><span data-ttu-id="3784c-103">ASP.NET Core Blazor bağımlılığı ekleme</span><span class="sxs-lookup"><span data-stu-id="3784c-103">ASP.NET Core Blazor dependency injection</span></span>

<span data-ttu-id="3784c-104">[Rainer Stropek](https://www.timecockpit.com) tarafından</span><span class="sxs-lookup"><span data-stu-id="3784c-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="3784c-105">Blazor, [bağımlılık ekleme işlemini (dı)](xref:fundamentals/dependency-injection)destekler.</span><span class="sxs-lookup"><span data-stu-id="3784c-105">Blazor supports [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3784c-106">Uygulamalar, yerleşik hizmetleri ekleme tarafından bileşenlere kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="3784c-106">Apps can use built-in services by injecting them into components.</span></span> <span data-ttu-id="3784c-107">Uygulamalar Ayrıca özel hizmetleri tanımlayabilir ve kaydedebilir ve bu Hizmetleri uygulama genelinde DI aracılığıyla kullanılabilir hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3784c-107">Apps can also define and register custom services and make them available throughout the app via DI.</span></span>

<span data-ttu-id="3784c-108">DI, merkezi bir konumda yapılandırılmış hizmetlere erişmek için bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="3784c-108">DI is a technique for accessing services configured in a central location.</span></span> <span data-ttu-id="3784c-109">Bu, Blazor uygulamalarında şu şekilde yararlı olabilir:</span><span class="sxs-lookup"><span data-stu-id="3784c-109">This can be useful in Blazor apps to:</span></span>

* <span data-ttu-id="3784c-110">Hizmet sınıfının tek bir örneğini tek bir hizmet olarak bilinen birçok bileşen arasında paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3784c-110">Share a single instance of a service class across many components, known as a *singleton* service.</span></span>
* <span data-ttu-id="3784c-111">Başvuru soyutlamalarını kullanarak somut hizmet sınıflarından bileşenleri ayırın.</span><span class="sxs-lookup"><span data-stu-id="3784c-111">Decouple components from concrete service classes by using reference abstractions.</span></span> <span data-ttu-id="3784c-112">Örneğin, uygulamadaki verilere erişim için `IDataAccess` bir arabirim düşünün.</span><span class="sxs-lookup"><span data-stu-id="3784c-112">For example, consider an interface `IDataAccess` for accessing data in the app.</span></span> <span data-ttu-id="3784c-113">Arabirim somut `DataAccess` bir sınıf tarafından uygulanır ve uygulamanın hizmet kapsayıcısında bir hizmet olarak kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="3784c-113">The interface is implemented by a concrete `DataAccess` class and registered as a service in the app's service container.</span></span> <span data-ttu-id="3784c-114">Bir bileşen bir `IDataAccess` uygulama almak için dı kullandığında, bileşen somut tür ile eşleştirilmez.</span><span class="sxs-lookup"><span data-stu-id="3784c-114">When a component uses DI to receive an `IDataAccess` implementation, the component isn't coupled to the concrete type.</span></span> <span data-ttu-id="3784c-115">Uygulama, büyük olasılıkla birim testlerinde bir sahte uygulama için değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="3784c-115">The implementation can be swapped, perhaps for a mock implementation in unit tests.</span></span>

## <a name="default-services"></a><span data-ttu-id="3784c-116">Varsayılan hizmetler</span><span class="sxs-lookup"><span data-stu-id="3784c-116">Default services</span></span>

<span data-ttu-id="3784c-117">Varsayılan hizmetler, uygulamanın hizmet koleksiyonuna otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="3784c-117">Default services are automatically added to the app's service collection.</span></span>

| <span data-ttu-id="3784c-118">Hizmet</span><span class="sxs-lookup"><span data-stu-id="3784c-118">Service</span></span> | <span data-ttu-id="3784c-119">Ömür</span><span class="sxs-lookup"><span data-stu-id="3784c-119">Lifetime</span></span> | <span data-ttu-id="3784c-120">Açıklama</span><span class="sxs-lookup"><span data-stu-id="3784c-120">Description</span></span> |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | <span data-ttu-id="3784c-121">Adet</span><span class="sxs-lookup"><span data-stu-id="3784c-121">Singleton</span></span> | <span data-ttu-id="3784c-122">HTTP istekleri göndermek ve bir URI tarafından tanımlanan bir kaynaktan HTTP yanıtlarını almak için yöntemler sağlar.</span><span class="sxs-lookup"><span data-stu-id="3784c-122">Provides methods for sending HTTP requests and receiving HTTP responses from a resource identified by a URI.</span></span> <span data-ttu-id="3784c-123">Bu örneğinin `HttpClient` , arka planda HTTP trafiğini işlemek için tarayıcıyı kullandığını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="3784c-123">Note that this instance of `HttpClient` uses the browser for handling the HTTP traffic in the background.</span></span> <span data-ttu-id="3784c-124">[HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) , UYGULAMANıN temel URI ön ekine otomatik olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="3784c-124">[HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) is automatically set to the base URI prefix of the app.</span></span> <span data-ttu-id="3784c-125">Daha fazla bilgi için bkz. <xref:blazor/call-web-api>.</span><span class="sxs-lookup"><span data-stu-id="3784c-125">For more information, see <xref:blazor/call-web-api>.</span></span> |
| `IJSRuntime` | <span data-ttu-id="3784c-126">Adet</span><span class="sxs-lookup"><span data-stu-id="3784c-126">Singleton</span></span> | <span data-ttu-id="3784c-127">JavaScript çağrılarının dağıtıldığı bir JavaScript çalışma zamanının örneğini temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3784c-127">Represents an instance of a JavaScript runtime where JavaScript calls are dispatched.</span></span> <span data-ttu-id="3784c-128">Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="3784c-128">For more information, see <xref:blazor/javascript-interop>.</span></span> |
| `IUriHelper` | <span data-ttu-id="3784c-129">Adet</span><span class="sxs-lookup"><span data-stu-id="3784c-129">Singleton</span></span> | <span data-ttu-id="3784c-130">URI 'Ler ve gezinme durumu ile çalışmaya yönelik yardımcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="3784c-130">Contains helpers for working with URIs and navigation state.</span></span> <span data-ttu-id="3784c-131">Daha fazla bilgi için bkz. [URI ve gezinti durumu yardımcıları](xref:blazor/routing#uri-and-navigation-state-helpers).</span><span class="sxs-lookup"><span data-stu-id="3784c-131">For more information, see [URI and navigation state helpers](xref:blazor/routing#uri-and-navigation-state-helpers).</span></span> |

<span data-ttu-id="3784c-132">Özel bir hizmet sağlayıcı, tabloda listelenen varsayılan Hizmetleri otomatik olarak sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="3784c-132">A custom service provider doesn't automatically provide the default services listed in the table.</span></span> <span data-ttu-id="3784c-133">Özel bir hizmet sağlayıcısı kullanır ve tabloda gösterilen hizmetlerden herhangi birini gerekliyse, gerekli hizmetleri yeni hizmet sağlayıcısına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3784c-133">If you use a custom service provider and require any of the services shown in the table, add the required services to the new service provider.</span></span>

## <a name="add-services-to-an-app"></a><span data-ttu-id="3784c-134">Uygulamaya hizmet ekleme</span><span class="sxs-lookup"><span data-stu-id="3784c-134">Add services to an app</span></span>

<span data-ttu-id="3784c-135">Yeni bir uygulama oluşturduktan sonra, `Startup.ConfigureServices` yöntemini inceleyin:</span><span class="sxs-lookup"><span data-stu-id="3784c-135">After creating a new app, examine the `Startup.ConfigureServices` method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

<span data-ttu-id="3784c-136">Yöntemi, hizmet açıklayıcı nesnelerinin <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>bir listesi olan bir (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>) iletilir. `ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="3784c-136">The `ConfigureServices` method is passed an <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, which is a list of service descriptor objects (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>).</span></span> <span data-ttu-id="3784c-137">Hizmetler, hizmet koleksiyonuna hizmet tanımlayıcıları sağlayarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="3784c-137">Services are added by providing service descriptors to the service collection.</span></span> <span data-ttu-id="3784c-138">Aşağıdaki örnek, `IDataAccess` arabirimini ve somut uygulamasını `DataAccess`içeren kavramı gösterir:</span><span class="sxs-lookup"><span data-stu-id="3784c-138">The following example demonstrates the concept with the `IDataAccess` interface and its concrete implementation `DataAccess`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

<span data-ttu-id="3784c-139">Hizmetler, aşağıdaki tabloda gösterilen ömürlerle yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="3784c-139">Services can be configured with the lifetimes shown in the following table.</span></span>

| <span data-ttu-id="3784c-140">Ömür</span><span class="sxs-lookup"><span data-stu-id="3784c-140">Lifetime</span></span> | <span data-ttu-id="3784c-141">Açıklama</span><span class="sxs-lookup"><span data-stu-id="3784c-141">Description</span></span> |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | <span data-ttu-id="3784c-142">Blazor istemci tarafında şu anda bir dı kapsamları kavramı yok.</span><span class="sxs-lookup"><span data-stu-id="3784c-142">Blazor client-side doesn't currently have a concept of DI scopes.</span></span> <span data-ttu-id="3784c-143">`Scoped`-kayıtlı hizmetler hizmetler gibi `Singleton` davranır.</span><span class="sxs-lookup"><span data-stu-id="3784c-143">`Scoped`-registered services behave like `Singleton` services.</span></span> <span data-ttu-id="3784c-144">Ancak, sunucu tarafı barındırma modeli `Scoped` yaşam süresini destekler.</span><span class="sxs-lookup"><span data-stu-id="3784c-144">However, the server-side hosting model supports the `Scoped` lifetime.</span></span> <span data-ttu-id="3784c-145">Razor bileşeninde, kapsamlı bir hizmet kaydı bağlantının kapsamına alınır.</span><span class="sxs-lookup"><span data-stu-id="3784c-145">In a Razor component, a scoped service registration is scoped to the connection.</span></span> <span data-ttu-id="3784c-146">Bu nedenle, geçerli amaç tarayıcıda istemci tarafı çalıştırmak olsa bile, kapsama alınmış hizmetlerin kullanılması geçerli kullanıcı kapsamında olması gereken hizmetler için tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="3784c-146">For this reason, using scoped services is preferred for services that should be scoped to the current user, even if the current intent is to run client-side in the browser.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | <span data-ttu-id="3784c-147">Dı, hizmetin *tek bir örneğini* oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3784c-147">DI creates a *single instance* of the service.</span></span> <span data-ttu-id="3784c-148">`Singleton` Hizmet gerektiren tüm bileşenler aynı hizmetin bir örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="3784c-148">All components requiring a `Singleton` service receive an instance of the same service.</span></span> |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | <span data-ttu-id="3784c-149">Bir bileşen hizmet kapsayıcısından bir `Transient` hizmetin örneğini edindiğinde, hizmetin yeni bir *örneğini* alır.</span><span class="sxs-lookup"><span data-stu-id="3784c-149">Whenever a component obtains an instance of a `Transient` service from the service container, it receives a *new instance* of the service.</span></span> |

<span data-ttu-id="3784c-150">Dı sistemi ASP.NET Core içindeki DI sistemini temel alır.</span><span class="sxs-lookup"><span data-stu-id="3784c-150">The DI system is based on the DI system in ASP.NET Core.</span></span> <span data-ttu-id="3784c-151">Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="3784c-151">For more information, see <xref:fundamentals/dependency-injection>.</span></span>

## <a name="request-a-service-in-a-component"></a><span data-ttu-id="3784c-152">Bir bileşende hizmet isteme</span><span class="sxs-lookup"><span data-stu-id="3784c-152">Request a service in a component</span></span>

<span data-ttu-id="3784c-153">Hizmetler hizmet koleksiyonuna eklendikten sonra, Razor [ \@Ekle](xref:mvc/views/razor#inject) yönergesini kullanarak hizmetleri bileşenlere ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3784c-153">After services are added to the service collection, inject the services into the components using the [\@inject](xref:mvc/views/razor#inject) Razor directive.</span></span> <span data-ttu-id="3784c-154">`@inject`iki parametreye sahiptir:</span><span class="sxs-lookup"><span data-stu-id="3784c-154">`@inject` has two parameters:</span></span>

* <span data-ttu-id="3784c-155">Eklenecek &ndash; hizmetin türünü yazın.</span><span class="sxs-lookup"><span data-stu-id="3784c-155">Type &ndash; The type of the service to inject.</span></span>
* <span data-ttu-id="3784c-156">Özelliği &ndash; eklenen App Service 'i alan özelliğin adı.</span><span class="sxs-lookup"><span data-stu-id="3784c-156">Property &ndash; The name of the property receiving the injected app service.</span></span> <span data-ttu-id="3784c-157">Özelliği el ile oluşturma gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="3784c-157">The property doesn't require manual creation.</span></span> <span data-ttu-id="3784c-158">Derleyici özelliği oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3784c-158">The compiler creates the property.</span></span>

<span data-ttu-id="3784c-159">Daha fazla bilgi için bkz. <xref:mvc/views/dependency-injection>.</span><span class="sxs-lookup"><span data-stu-id="3784c-159">For more information, see <xref:mvc/views/dependency-injection>.</span></span>

<span data-ttu-id="3784c-160">Farklı hizmetler `@inject` eklemek için birden çok deyim kullanın.</span><span class="sxs-lookup"><span data-stu-id="3784c-160">Use multiple `@inject` statements to inject different services.</span></span>

<span data-ttu-id="3784c-161">Aşağıdaki örnek nasıl kullanılacağını `@inject`göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="3784c-161">The following example shows how to use `@inject`.</span></span> <span data-ttu-id="3784c-162">Uygulayan `Services.IDataAccess` hizmet bileşenin özelliğine `DataRepository`eklenir.</span><span class="sxs-lookup"><span data-stu-id="3784c-162">The service implementing `Services.IDataAccess` is injected into the component's property `DataRepository`.</span></span> <span data-ttu-id="3784c-163">Kodun yalnızca `IDataAccess` soyutlamayı nasıl kullandığını aklınızda yapın:</span><span class="sxs-lookup"><span data-stu-id="3784c-163">Note how the code is only using the `IDataAccess` abstraction:</span></span>

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

<span data-ttu-id="3784c-164">Dahili olarak, oluşturulan özelliği (`DataRepository`) `InjectAttribute` özniteliğiyle birlikte tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3784c-164">Internally, the generated property (`DataRepository`) is decorated with the `InjectAttribute` attribute.</span></span> <span data-ttu-id="3784c-165">Genellikle, bu öznitelik doğrudan kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="3784c-165">Typically, this attribute isn't used directly.</span></span> <span data-ttu-id="3784c-166">Bileşenler için bir temel sınıf gerekliyse ve temel sınıf için de eklenen özellikler gerekliyse, şunu el ile ekleyin `InjectAttribute`:</span><span class="sxs-lookup"><span data-stu-id="3784c-166">If a base class is required for components and injected properties are also required for the base class, manually add the `InjectAttribute`:</span></span>

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

<span data-ttu-id="3784c-167">Temel sınıftan türetilmiş bileşenlerde, `@inject` yönergesi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="3784c-167">In components derived from the base class, the `@inject` directive isn't required.</span></span> <span data-ttu-id="3784c-168">`InjectAttribute` Taban sınıfının yeterli olması yeterlidir:</span><span class="sxs-lookup"><span data-stu-id="3784c-168">The `InjectAttribute` of the base class is sufficient:</span></span>

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a><span data-ttu-id="3784c-169">Hizmetler 'de dı kullanma</span><span class="sxs-lookup"><span data-stu-id="3784c-169">Use DI in services</span></span>

<span data-ttu-id="3784c-170">Karmaşık hizmetler için ek hizmetler gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3784c-170">Complex services might require additional services.</span></span> <span data-ttu-id="3784c-171">Önceki örnekte, `DataAccess` `HttpClient` varsayılan hizmet gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3784c-171">In the prior example, `DataAccess` might require the `HttpClient` default service.</span></span> <span data-ttu-id="3784c-172">`@inject`(veya) `InjectAttribute`, hizmetlerde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="3784c-172">`@inject` (or the `InjectAttribute`) isn't available for use in services.</span></span> <span data-ttu-id="3784c-173">Bunun yerine *Oluşturucu Ekleme* kullanılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3784c-173">*Constructor injection* must be used instead.</span></span> <span data-ttu-id="3784c-174">Gerekli hizmetler, hizmetin oluşturucusuna parametreler eklenerek eklenir.</span><span class="sxs-lookup"><span data-stu-id="3784c-174">Required services are added by adding parameters to the service's constructor.</span></span> <span data-ttu-id="3784c-175">Dı hizmeti oluşturduğunda, oluşturucuda gereken hizmetleri algılar ve bunlara göre sağlar.</span><span class="sxs-lookup"><span data-stu-id="3784c-175">When DI creates the service, it recognizes the services it requires in the constructor and provides them accordingly.</span></span>

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

<span data-ttu-id="3784c-176">Oluşturucu Ekleme önkoşulları:</span><span class="sxs-lookup"><span data-stu-id="3784c-176">Prerequisites for constructor injection:</span></span>

* <span data-ttu-id="3784c-177">Bağımsız değişkenlerinin tümü DI tarafından yerine getirilme için tek bir Oluşturucu bulunmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3784c-177">One constructor must exist whose arguments can all be fulfilled by DI.</span></span> <span data-ttu-id="3784c-178">Varsayılan değerleri belirttiklerinde, DI tarafından kapsanmayan ek parametrelere izin verilir.</span><span class="sxs-lookup"><span data-stu-id="3784c-178">Additional parameters not covered by DI are allowed if they specify default values.</span></span>
* <span data-ttu-id="3784c-179">Uygulanabilir Oluşturucu *ortak*olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3784c-179">The applicable constructor must be *public*.</span></span>
* <span data-ttu-id="3784c-180">Uygulanabilir bir Oluşturucu var olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3784c-180">One applicable constructor must exist.</span></span> <span data-ttu-id="3784c-181">Belirsizlik söz konusu olduğunda, bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3784c-181">In case of an ambiguity, DI throws an exception.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3784c-182">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3784c-182">Additional resources</span></span>

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
