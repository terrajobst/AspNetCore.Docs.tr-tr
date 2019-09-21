---
title: ASP.NET Core Blazor bağımlılığı ekleme
author: guardrex
description: Bkz. Blazor Apps 'in bileşenlere nasıl hizmet ekleyebilmesi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/dependency-injection
ms.openlocfilehash: 074d7a669c900eb242c8329147b28d1c50652915
ms.sourcegitcommit: e5a74f882c14eaa0e5639ff082355e130559ba83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71168093"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET Core Blazor bağımlılığı ekleme

[Rainer Stropek](https://www.timecockpit.com) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor, [bağımlılık ekleme işlemini (dı)](xref:fundamentals/dependency-injection)destekler. Uygulamalar, yerleşik hizmetleri ekleme tarafından bileşenlere kullanabilir. Uygulamalar Ayrıca özel hizmetleri tanımlayabilir ve kaydedebilir ve bu Hizmetleri uygulama genelinde DI aracılığıyla kullanılabilir hale getirebilirsiniz.

DI, merkezi bir konumda yapılandırılmış hizmetlere erişmek için bir tekniktir. Bu, Blazor uygulamalarında şu şekilde yararlı olabilir:

* Hizmet sınıfının tek bir *örneğini tek bir hizmet olarak* bilinen birçok bileşen arasında paylaşabilirsiniz.
* Başvuru soyutlamalarını kullanarak somut hizmet sınıflarından bileşenleri ayırın. Örneğin, uygulamadaki verilere erişim için `IDataAccess` bir arabirim düşünün. Arabirim somut `DataAccess` bir sınıf tarafından uygulanır ve uygulamanın hizmet kapsayıcısında bir hizmet olarak kaydedilir. Bir bileşen bir `IDataAccess` uygulama almak için dı kullandığında, bileşen somut tür ile eşleştirilmez. Uygulama, büyük olasılıkla birim testlerinde bir sahte uygulama için değiştirilebilir.

## <a name="default-services"></a>Varsayılan hizmetler

Varsayılan hizmetler, uygulamanın hizmet koleksiyonuna otomatik olarak eklenir.

| Hizmet | Ömür | Açıklama |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | Adet | HTTP istekleri göndermek ve bir URI tarafından tanımlanan bir kaynaktan HTTP yanıtlarını almak için yöntemler sağlar. Bu örneğinin `HttpClient` , arka planda HTTP trafiğini işlemek için tarayıcıyı kullandığını unutmayın. [HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) , UYGULAMANıN temel URI ön ekine otomatik olarak ayarlanır. Daha fazla bilgi için bkz. <xref:blazor/call-web-api>. |
| `IJSRuntime` | Adet | JavaScript çağrılarının dağıtıldığı bir JavaScript çalışma zamanının örneğini temsil eder. Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>. |
| `NavigationManager` | Adet | URI 'Ler ve gezinme durumu ile çalışmaya yönelik yardımcıları içerir. Daha fazla bilgi için bkz. [URI ve gezinti durumu yardımcıları](xref:blazor/routing#uri-and-navigation-state-helpers). |

Özel bir hizmet sağlayıcı, tabloda listelenen varsayılan Hizmetleri otomatik olarak sağlamaz. Özel bir hizmet sağlayıcısı kullanır ve tabloda gösterilen hizmetlerden herhangi birini gerekliyse, gerekli hizmetleri yeni hizmet sağlayıcısına ekleyin.

## <a name="add-services-to-an-app"></a>Uygulamaya hizmet ekleme

Yeni bir uygulama oluşturduktan sonra, `Startup.ConfigureServices` yöntemini inceleyin:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

Yöntemi, hizmet açıklayıcı nesnelerinin <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>bir listesi olan bir (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>) iletilir. `ConfigureServices` Hizmetler, hizmet koleksiyonuna hizmet tanımlayıcıları sağlayarak eklenir. Aşağıdaki örnek, `IDataAccess` arabirimini ve somut uygulamasını `DataAccess`içeren kavramı gösterir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Hizmetler, aşağıdaki tabloda gösterilen ömürlerle yapılandırılabilir.

| Ömür | Açıklama |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor WebAssembly uygulamalarında Şu anda bir dı kapsamları kavramı yoktur. `Scoped`-kayıtlı hizmetler hizmetler gibi `Singleton` davranır. Ancak, Blazor sunucusu barındırma modeli `Scoped` yaşam süresini destekler. Blazor sunucu uygulamalarında, kapsamlı bir hizmet kaydı *bağlantının*kapsamına alınır. Bu nedenle, geçerli amaç tarayıcıda istemci tarafı çalıştırmak olsa bile, kapsama alınmış hizmetlerin kullanılması geçerli kullanıcı kapsamında olması gereken hizmetler için tercih edilir. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | Dı, hizmetin *tek bir örneğini* oluşturur. `Singleton` Hizmet gerektiren tüm bileşenler aynı hizmetin bir örneğini alır. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Bir bileşen hizmet kapsayıcısından bir `Transient` hizmetin örneğini edindiğinde, hizmetin yeni bir *örneğini* alır. |

Dı sistemi ASP.NET Core içindeki DI sistemini temel alır. Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

## <a name="request-a-service-in-a-component"></a>Bir bileşende hizmet isteme

Hizmetler hizmet koleksiyonuna eklendikten sonra, Razor [ \@Ekle](xref:mvc/views/razor#inject) yönergesini kullanarak hizmetleri bileşenlere ekleyin. `@inject`iki parametreye sahiptir:

* Eklenecek &ndash; hizmetin türünü yazın.
* Özelliği &ndash; eklenen App Service 'i alan özelliğin adı. Özelliği el ile oluşturma gerektirmez. Derleyici özelliği oluşturur.

Daha fazla bilgi için bkz. <xref:mvc/views/dependency-injection>.

Farklı hizmetler `@inject` eklemek için birden çok deyim kullanın.

Aşağıdaki örnek nasıl kullanılacağını `@inject`göstermektedir. Uygulayan `Services.IDataAccess` hizmet bileşenin özelliğine `DataRepository`eklenir. Kodun yalnızca `IDataAccess` soyutlamayı nasıl kullandığını aklınızda yapın:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Dahili olarak, oluşturulan özelliği (`DataRepository`) `InjectAttribute` özniteliğiyle birlikte tasarlanmıştır. Genellikle, bu öznitelik doğrudan kullanılmaz. Bileşenler için bir temel sınıf gerekliyse ve temel sınıf için de eklenen özellikler gerekliyse, şunu el ile ekleyin `InjectAttribute`:

```csharp
public class ComponentBase : IComponent
{
    // DI works even if using the InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

Temel sınıftan türetilmiş bileşenlerde, `@inject` yönergesi gerekli değildir. `InjectAttribute` Taban sınıfının yeterli olması yeterlidir:

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="use-di-in-services"></a>Hizmetler 'de dı kullanma

Karmaşık hizmetler için ek hizmetler gerekebilir. Önceki örnekte, `DataAccess` `HttpClient` varsayılan hizmet gerekebilir. `@inject`(veya) `InjectAttribute`, hizmetlerde kullanılamaz. Bunun yerine *Oluşturucu Ekleme* kullanılmalıdır. Gerekli hizmetler, hizmetin oluşturucusuna parametreler eklenerek eklenir. Dı hizmeti oluşturduğunda, oluşturucuda gereken hizmetleri algılar ve bunlara göre sağlar.

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

ASP.NET Core uygulamalarda, kapsamlı hizmetler genellikle geçerli isteğin kapsamlandırılır. İstek tamamlandıktan sonra, tüm kapsamlı veya geçici hizmetler dı sistemi tarafından silinir. Blazor Server uygulamalarında istek kapsamı, istemci bağlantısı süresince sürer, bu da geçici ve kapsamlı hizmetlerin beklenenden çok daha uzun sürebileceği anlamına gelir.

Hizmetlerin bir bileşenin kullanım ömrüne göre kapsamını atamak için `OwningComponentBase` ve `OwningComponentBase<TService>` temel sınıfları kullanabilir. Bu temel sınıflar, bileşenin `ScopedServices` kullanım ömrü kapsamındaki `IServiceProvider` Hizmetleri çözümlemek için türünde bir özellik sunar. Razor içindeki bir taban sınıftan devralan bir bileşeni yazmak için `@inherits` yönergesini kullanın.

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
> Veya kullanılarak `@inject`bileşeneeklenen Hizmetler,bileşenkapsamındaoluşturulmazveistekkapsamınabağlıdır.`InjectAttribute`

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
