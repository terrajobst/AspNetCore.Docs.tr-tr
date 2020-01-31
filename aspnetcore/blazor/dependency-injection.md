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
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET Core Blazor bağımlılığı ekleme

[Rainer Stropek](https://www.timecockpit.com) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor, [bağımlılık ekleme işlemini (dı)](xref:fundamentals/dependency-injection)destekler. Uygulamalar, yerleşik hizmetleri ekleme tarafından bileşenlere kullanabilir. Uygulamalar Ayrıca özel hizmetleri tanımlayabilir ve kaydedebilir ve bu Hizmetleri uygulama genelinde DI aracılığıyla kullanılabilir hale getirebilirsiniz.

DI, merkezi bir konumda yapılandırılmış hizmetlere erişmek için bir tekniktir. Bu, Blazor uygulamalarında şu şekilde yararlı olabilir:

* Hizmet sınıfının tek bir *örneğini tek bir hizmet olarak* bilinen birçok bileşen arasında paylaşabilirsiniz.
* Başvuru soyutlamalarını kullanarak somut hizmet sınıflarından bileşenleri ayırın. Örneğin, uygulamadaki verilere erişmek için bir arabirim `IDataAccess` düşünün. Arabirim somut bir `DataAccess` sınıfı tarafından uygulanır ve uygulamanın hizmet kapsayıcısında bir hizmet olarak kaydedilir. Bir bileşen bir `IDataAccess` uygulamasını almak için DI kullandığında, bileşen somut tür ile eşleştirilmez. Uygulama, büyük olasılıkla birim testlerinde bir sahte uygulama için değiştirilebilir.

## <a name="default-services"></a>Varsayılan hizmetler

Varsayılan hizmetler, uygulamanın hizmet koleksiyonuna otomatik olarak eklenir.

| Hizmet | Ömür | Açıklama |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Adet | HTTP istekleri göndermek ve bir URI tarafından tanımlanan bir kaynaktan HTTP yanıtlarını almak için yöntemler sağlar.<br><br>Bir Blazor WebAssembly uygulamasındaki `HttpClient` örneği, arka planda HTTP trafiğini işlemek için tarayıcıyı kullanır.<br><br>Blazor sunucu uygulamaları, varsayılan olarak hizmet olarak yapılandırılmış bir `HttpClient` içermez. Bir Blazor sunucu uygulamasına `HttpClient` sağlayın.<br><br>Daha fazla bilgi için bkz. <xref:blazor/call-web-api>. |
| `IJSRuntime` | Singleton (Blazor WebAssembly)<br>Kapsamlı (Blazor sunucusu) | JavaScript çağrılarının dağıtıldığı bir JavaScript çalışma zamanının örneğini temsil eder. Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>. |
| `NavigationManager` | Singleton (Blazor WebAssembly)<br>Kapsamlı (Blazor sunucusu) | URI 'Ler ve gezinme durumu ile çalışmaya yönelik yardımcıları içerir. Daha fazla bilgi için bkz. [URI ve gezinti durumu yardımcıları](xref:blazor/routing#uri-and-navigation-state-helpers). |

Özel bir hizmet sağlayıcı, tabloda listelenen varsayılan Hizmetleri otomatik olarak sağlamaz. Özel bir hizmet sağlayıcısı kullanır ve tabloda gösterilen hizmetlerden herhangi birini gerekliyse, gerekli hizmetleri yeni hizmet sağlayıcısına ekleyin.

## <a name="add-services-to-an-app"></a>Uygulamaya hizmet ekleme

Yeni bir uygulama oluşturduktan sonra `Startup.ConfigureServices` yöntemini inceleyin:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

`ConfigureServices` yöntemi, hizmet açıklayıcı nesnelerinin bir listesi olan bir <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>geçirilir (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Hizmetler, hizmet koleksiyonuna hizmet tanımlayıcıları sağlayarak eklenir. Aşağıdaki örnek, `IDataAccess` arabirimiyle kavramı ve somut uygulama `DataAccess`gösterir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Hizmetler, aşağıdaki tabloda gösterilen ömürlerle yapılandırılabilir.

| Ömür | Açıklama |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor WebAssembly uygulamalarının Şu anda bir dı kapsamları kavramı yoktur. `Scoped`kayıtlı hizmetler `Singleton` hizmetleri gibi davranır. Ancak, Blazor sunucusu barındırma modeli `Scoped` ömrünü destekler. Blazor Server uygulamalarında, kapsamlı bir hizmet kaydı *bağlantının*kapsamına alınır. Bu nedenle, geçerli amaç tarayıcıda istemci tarafı çalıştırmak olsa bile, kapsama alınmış hizmetlerin kullanılması geçerli kullanıcı kapsamında olması gereken hizmetler için tercih edilir. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | Dı, hizmetin *tek bir örneğini* oluşturur. Bir `Singleton` hizmeti gerektiren tüm bileşenler aynı hizmetin bir örneğini alır. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Bir bileşen hizmet kapsayıcısından bir `Transient` hizmeti örneği aldığında, hizmetin *Yeni bir örneğini* alır. |

Dı sistemi ASP.NET Core içindeki DI sistemini temel alır. Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Bir bileşende hizmet isteme

Hizmetler hizmet koleksiyonuna eklendikten sonra, [\@Ekle](xref:mvc/views/razor#inject) Razor yönergesini kullanarak hizmetleri bileşenlere ekleyin. `@inject` iki parametreye sahiptir:

* Eklenecek hizmetin türünü &ndash; yazın.
* Özelliği, eklenen App Service 'i alan özelliğin adını &ndash;. Özelliği el ile oluşturma gerektirmez. Derleyici özelliği oluşturur.

Daha fazla bilgi için bkz. <xref:mvc/views/dependency-injection>.

Farklı hizmetler eklemek için birden çok `@inject` deyimi kullanın.

Aşağıdaki örnek `@inject`nasıl kullanacağınızı gösterir. `Services.IDataAccess` uygulayan hizmet bileşenin Özellik `DataRepository`eklenir. Kodun yalnızca `IDataAccess` soyutlamasını nasıl kullandığını aklınızda yapın:

[!code-razor[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Dahili olarak, oluşturulan Özellik (`DataRepository`) `InjectAttribute` özniteliğini kullanır. Genellikle, bu öznitelik doğrudan kullanılmaz. Bileşenler için bir temel sınıf gerekliyse ve temel sınıf için eklenen özellikler de gerekliyse, `InjectAttribute`el ile ekleyin:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

Temel sınıftan türetilmiş bileşenlerde `@inject` yönergesi gerekli değildir. Temel sınıfın `InjectAttribute` yeterlidir:

```razor
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Hizmetler 'de dı kullanma

Karmaşık hizmetler için ek hizmetler gerekebilir. Önceki örnekte, `DataAccess` `HttpClient` varsayılan hizmeti gerektirebilir. `@inject` (veya `InjectAttribute`), hizmetlerde kullanılamaz. Bunun yerine *Oluşturucu Ekleme* kullanılmalıdır. Gerekli hizmetler, hizmetin oluşturucusuna parametreler eklenerek eklenir. Dı hizmeti oluşturduğunda, oluşturucuda gereken hizmetleri algılar ve bunlara göre sağlar.

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

Oluşturucu Ekleme önkoşulları:

* Bağımsız değişkenlerinin tümü DI tarafından yerine getirilme için tek bir Oluşturucu bulunmalıdır. Varsayılan değerleri belirttiklerinde, DI tarafından kapsanmayan ek parametrelere izin verilir.
* Uygulanabilir Oluşturucu *ortak*olmalıdır.
* Uygulanabilir bir Oluşturucu var olmalıdır. Belirsizlik söz konusu olduğunda, bir özel durum oluşturur.

## <a name="utility-base-component-classes-to-manage-a-di-scope"></a>Bir dı kapsamını yönetmek için yardımcı program temel bileşen sınıfları

ASP.NET Core uygulamalarda, kapsamlı hizmetler genellikle geçerli isteğin kapsamlandırılır. İstek tamamlandıktan sonra, tüm kapsamlı veya geçici hizmetler dı sistemi tarafından silinir. Blazor Server uygulamalarında istek kapsamı, istemci bağlantısı süresince sürer ve bu da geçici ve kapsamlı hizmetlerin beklenenden çok daha uzun sürebileceği anlamına gelir.

Hizmetlerin bir bileşenin kullanım ömrüne göre kapsamını atamak için, `OwningComponentBase` ve `OwningComponentBase<TService>` temel sınıfları kullanabilirsiniz. Bu temel sınıflar, bileşenin kullanım ömrü kapsamındaki Hizmetleri çözümlemek `IServiceProvider` türünde bir `ScopedServices` özelliğini kullanıma sunar. Razor 'teki bir taban sınıftan devralan bir bileşeni yazmak için `@inherits` yönergesini kullanın.

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
> `@inject` veya `InjectAttribute` kullanılarak bileşene eklenen hizmetler bileşen kapsamında oluşturulmaz ve istek kapsamına bağlıdır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
