---
title: Razor bileşenleri bağımlılık ekleme
author: guardrex
description: Bileşenlerine eklenen sağlayarak Blazor ve Razor bileşenleri uygulamaları yerleşik hizmetleri nasıl kullanabileceğinizi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/dependency-injection
ms.openlocfilehash: 04be8371b6e746929448b24487b07c08cf03cb78
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668187"
---
# <a name="razor-components-dependency-injection"></a>Razor bileşenleri bağımlılık ekleme

By [Rainer Stropek](https://www.timecockpit.com)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

[Bağımlılık ekleme (dı)](/aspnet/core/fundamentals/dependency-injection) yerleşiktir. Uygulamaları, eklenen bileşenlerine sağlayarak yerleşik hizmetlerini kullanabilirsiniz. Uygulamalar ayrıca özel hizmetler tanımlar ve DI aracılığıyla kullanılabilir hale.

## <a name="dependency-injection"></a>Bağımlılık ekleme

DI, merkezi bir konumda yapılandırılmış hizmetlerine erişmek için kullanılan bir tekniktir. Bu yararlı olabilir:

* Bir hizmet sınıfı tek bir örneği arasında birçok bileşen paylaşma (olarak bilinen bir *singleton* hizmeti).
* Belirli somut hizmet sınıflardan bileşenleri ayırın ve yalnızca soyutlama başvurun. Örneğin, bir arabirim `IDataAccess` somut bir sınıf tarafından uygulanan `DataAccess`. Bir bileşen kullandığında DI almak için bir `IDataAccess` uygulaması bileşeni, somut bir türde eşleşmiş değil. Uygulama, belki de birim testlerinde sahte bir uygulama için değişiklik yapılabilir.

DI sistemi bileşenlerine services örneğini sağlama için sorumludur. Böylece Hizmetleri kendilerini hizmetleri hakkında daha fazla güvenebileceğiniz DI de bağımlılıkları yinelemeli olarak çözümler. DI uygulama başlangıcı sırasında yapılandırılır. Örnek bu konunun ilerleyen bölümlerinde gösterilmektedir.

## <a name="add-services-to-di"></a>DI için hizmet ekleyin

Yeni bir uygulama oluşturduktan sonra incelemek `Startup.ConfigureServices` yöntemi:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

`ConfigureServices` Yöntemi geçirilir bir [IServiceCollection](/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection), hizmet tanımlayıcısı nesneleri listesi verilmiştir ([ServiceDescriptor](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor)). Hizmet, hizmet koleksiyonu için hizmet tanımlayıcıları sağlayarak eklenir. Aşağıdaki kod örneği, bir kavramı gösterir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Hizmetler aşağıdaki yaşam süreleri ile yapılandırılabilir:

| Yöntem      | Açıklama |
| ----------- | ----------- |
| [singleton](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.singleton#Microsoft_Extensions_DependencyInjection_ServiceDescriptor_Singleton__1_System_Func_System_IServiceProvider___0__) | DI oluşturur bir *tek örnek* hizmeti. Bu hizmet gerektiren tüm bileşenleri bu örneğe bir başvuru alırsınız. |
| [Geçici](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.transient) | Bu hizmet bir bileşen gereken her durumda aldığı bir *yeni örneği* hizmeti. |
| [Kapsamlı](/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor.scoped) | İstemci tarafı Blazor şu anda DI kapsamları kavramı yoktur. `Scoped` gibi davranır `Singleton`. Ancak, ASP.NET Core Razor bileşenleri desteklemek `Scoped` yaşam süresi. Bir Razor bileşeninde bir kapsamlı hizmet kayıt bağlantısı kapsamlıdır. Kapsamlı hizmetlerini kullanarak bu nedenle, geçerli kullanıcıya kapsamlı hizmetler için tercih edilir (istemci-tarafı çalıştırmak için geçerli amaç olsa bile tarayıcıda). |

ASP.NET Core DI sistemde DI sistem dayanır. Daha fazla bilgi için [ASP.NET Core bağımlılık ekleme](/aspnet/core/fundamentals/dependency-injection).

## <a name="default-services"></a>Varsayılan hizmetler

Varsayılan hizmetler, bir app service koleksiyonunu otomatik olarak eklenir. Aşağıdaki tabloda sağlanan yararlı varsayılan hizmetlerden bazıları gösterilmektedir.

| Yöntem       | Açıklama |
| ------------ | ----------- |
| [HttpClient](/dotnet/api/system.net.http.httpclient) | HTTP istekleri göndermek ve bir URI (tekli) tarafından tanımlanan bir kaynaktan HTTP yanıtları almak için yöntemler sağlar. Unutmayın, bu örneği `HttpClient` tarayıcı arka planda HTTP trafiğini işlemek için kullanır. [HttpClient.BaseAddress](/dotnet/api/system.net.http.httpclient.baseaddress) uygulama taban URI öneki otomatik olarak ayarlanır. `HttpClient` yalnızca istemci-tarafı Blazor uygulamalar için sağlanır. |
| `IJSRuntime` | Çağrıları gönderilen bir JavaScript çalışma zamanı örneğini temsil eder. Daha fazla bilgi için bkz. <xref:razor-components/javascript-interop>. |
| `IUriHelper` | URI ve gezinti durumu (tekli) ile çalışmak için Yardımcıları. `IUriHelper` Her iki istemci-tarafı Blazor ve ASP.NET Core Razor bileşenleri uygulama sağlanır. |

Varsayılan şablon tarafından eklenen varsayılan hizmet sağlayıcısı yerine özel hizmetler sağlayıcısı kullanmak mümkün olduğunu unutmayın. Bir özel hizmet sağlayıcısı, tabloda listelenen varsayılan hizmetleri otomatik olarak sağlamaz. Bu hizmetler yeni hizmet sağlayıcısına açıkça eklenmelidir.

## <a name="request-a-service-in-a-component"></a>Bir bileşen içinde bir hizmet isteği

Hizmetleri hizmet koleksiyonuna eklendikten sonra bileşenlerin Razor şablonlarına kullanarak yerleştirilebilir `@inject` Razor yönergesi. `@inject` iki parametreye sahiptir:

* Tür adı: Eklenecek hizmetin türü.
* Özellik adı: Eklenen app service alma özelliğinin adı. Not, özelliği el ile oluşturma gerektirmez. Derleyici, bir özellik oluşturur.

Birden çok `@inject` deyimleri, farklı hizmetlerde eklenmek üzere kullanılabilir.

Aşağıdaki örnek nasıl kullanılacağını gösterir `@inject`. Uygulama hizmeti `Services.IDataAccess` bileşenin özelliğine eklenen `DataRepository`. Kodu yalnızca nasıl kullandığını unutmayın `IDataAccess` Özet:

```csharp
@page "/customer-list"
@using Services
@inject IDataAccess DataRepository

<ul>
    @if (Customers != null)
    {
        @foreach (var customer in Customers)
        {
            <li>@customer.FirstName @customer.LastName</li>
        }
    }
</ul>

@functions {
    private IReadOnlyList<Customer> Customers;

    protected override async Task OnInitAsync()
    {
        // The property DataRepository received an implementation
        // of IDataAccess through dependency injection. Use 
        // DataRepository to obtain data from the server.
        Customers = await DataRepository.GetAllCustomersAsync();
    }
}
```

Dahili olarak oluşturulan özelliğe (`DataRepository`) ile donatılmış `InjectAttribute` özniteliği. Genellikle, bu öznitelik, doğrudan kullanılmaz. Bir temel sınıf bileşenleri için gereklidir ve eklenen özellikler için temel sınıf, gerekli ayrıca `InjectAttribute` elle eklenebilir:

```csharp
public class ComponentBase : BlazorComponent
{
    // Dependency injection works even if using the
    // InjectAttribute in a component's base class.
    [Inject]
    protected IDataAccess DataRepository { get; set; }
    ...
}
```

Temel sınıftan türetilmiş bileşenlerinde `@inject` yönergesi gerekli değildir. `InjectAttribute` Temel sınıfını yeterlidir:

```csharp
@page "/demo"
@inherits ComponentBase

<h1>...</h1>
...
```

## <a name="dependency-injection-in-services"></a>Hizmetleri bağımlılık ekleme

Karmaşık services ek hizmetleri gerektirebilir. Önceki örnekte, `DataAccess` gerektirebilir `HttpClient` varsayılan hizmeti. `@inject` veya `InjectAttribute` Hizmetleri'nde kullanılamaz. *Oluşturucu ekleme* yerine kullanılmalıdır. Gerekli hizmetler, hizmetin oluşturucusuna parametre ekleyerek eklenir. Bağımlılık ekleme hizmet oluşturduğu zaman, oluşturucuda gerektirir ve uygun şekilde sağlayan hizmetler tanır.

Aşağıdaki kod örneği, bir kavramı gösterir:

```csharp
public class DataAccess : IDataAccess
{
    // The constructor receives an HttpClient via dependency
    // injection. HttpClient is a default service.
    public DataAccess(HttpClient client)
    {
        ...
    }
    ...
}
```

Oluşturucu ekleme için aşağıdaki önkoşulları göz önünde bulundurun:

* Bağımsız değişkenleri tüm bağımlılık ekleme tarafından yerine getirilmesi bir oluşturucusu olmalıdır. DI tarafından kapsanmayan ek parametreler için varsayılan değerleri belirtilirse izin verildiğini unutmayın.
* Geçerli bir oluşturucusu olmalıdır *genel*.
* Yalnızca geçerli bir oluşturucusu olmalıdır. Bir belirsizlik durumunda DI özel durum oluşturur.

## <a name="additional-resources"></a>Ek kaynaklar

* [ASP.NET core'da bağımlılık ekleme](/aspnet/core/fundamentals/dependency-injection)
