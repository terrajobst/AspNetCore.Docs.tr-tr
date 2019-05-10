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
# <a name="blazor-dependency-injection"></a>Blazor bağımlılık ekleme

By [Rainer Stropek](https://www.timecockpit.com)

Blazor destekler [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection). Uygulamalar, yerleşik hizmetlere bileşenlerine ekleyerek kullanabilirsiniz. Uygulamalar ayrıca tanımlayın ve özel hizmetlerle kaydedebilir veya DI aracılığıyla uygulamayı kullanılabilir hale getirmek.

## <a name="dependency-injection"></a>Bağımlılık ekleme

DI, merkezi bir konumda yapılandırılmış hizmetlerine erişmek için kullanılan bir tekniktir. Bu, Blazor uygulamalarında için yararlı olabilir:

* Bir hizmet sınıfı tek bir örneği olarak bilinen, birçok bileşen paylaşılmasını bir *singleton* hizmeti.
* Somut hizmet sınıflardan bileşenleri, başvuru soyutlama kullanarak ayırın. Örneğin, bir arabirim düşünün `IDataAccess` uygulamadaki verilere erişmek için. Bir somut tarafından uygulanan arabirimi `DataAccess` sınıfı ve bir uygulamanın hizmet kapsayıcı hizmetinde olarak kaydedildi. Bir bileşen kullandığında DI almak için bir `IDataAccess` uygulaması bileşeni, somut bir türde eşleşmiş değil. Uygulama, belki de birim testlerinde sahte bir uygulama için değişiklik yapılabilir.

Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

## <a name="add-services-to-di"></a>DI için hizmet ekleyin

Yeni bir uygulama oluşturduktan sonra incelemek `Startup.ConfigureServices` yöntemi:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add custom services here
}
```

`ConfigureServices` Yöntemi geçirilir bir <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>, hizmet tanımlayıcısı nesneleri listesi verilmiştir (<xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor>). Hizmet, hizmet koleksiyonu için hizmet tanımlayıcıları sağlayarak eklenir. Aşağıdaki örnek, kavramı gösterir `IDataAccess` arabirimi ile somut uygulaması `DataAccess`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IDataAccess, DataAccess>();
}
```

Aşağıdaki tabloda gösterilen ömürleriyle Hizmetleri yapılandırılabilir.

| Ömür | Açıklama |
| -------- | ----------- |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Singleton*> | DI oluşturur bir *tek örnek* hizmeti. Tüm bileşenleri gerektiren bir `Singleton` hizmeti aynı Hizmeti'nin bir örneğini alır. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Transient*> | Her bir bileşen örneği alır bir `Transient` hizmet hizmet kapsayıcısından aldığı bir *yeni örneği* hizmeti. |
| <xref:Microsoft.Extensions.DependencyInjection.ServiceDescriptor.Scoped*> | Blazor istemci-tarafı şu anda DI kapsamları kavramı yoktur. `Scoped` gibi davranır `Singleton`. Ancak, sunucu tarafı barındırma modelini destekleyen `Scoped` yaşam süresi. Bir Razor bileşeninde bir kapsamlı hizmet kayıt bağlantısı kapsamlıdır. İstemci tarafı çalıştırmak için geçerli amaç olsa bile, bu nedenle, kapsamlı hizmetlerini kullanarak geçerli kullanıcı için kapsamlı hizmetler için tercih edilir tarayıcıda. |

ASP.NET Core DI sistemde DI sistem dayanır. Daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

## <a name="default-services"></a>Varsayılan hizmetler

Varsayılan hizmetler otomatik olarak uygulamanın hizmet koleksiyona eklenir.

| Hizmet | Ömür | Açıklama |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | singleton | HTTP istekleri göndermek ve bir URI tarafından tanımlanan bir kaynaktan HTTP yanıtları almak için yöntemler sağlar. Unutmayın, bu örneği `HttpClient` tarayıcı arka planda HTTP trafiğini işlemek için kullanır. [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress) uygulama taban URI öneki otomatik olarak ayarlanır. `HttpClient` yalnızca Blazor istemci tarafı uygulamalar için sağlanır. |
| `IJSRuntime` | singleton | JavaScript çağrılarını burada dağıtılmadan bir JavaScript çalışma zamanı örneğini temsil eder. Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>. |
| `IUriHelper` | singleton | URI ve gezinti durumu ile çalışmak için Yardımcıları içerir. |

Bir özel hizmet sağlayıcısı, tabloda listelenen varsayılan hizmetleri otomatik olarak sağlamaz. Bir özel hizmet sağlayıcısı ve tabloda gösterilen hizmetlerinden herhangi birinin gerektirir, gerekli hizmetler yeni hizmet sağlayıcısına ekleyin.

## <a name="request-a-service-in-a-component"></a>Bir bileşen içinde bir hizmet isteği

Hizmetleri hizmet koleksiyonuna eklendikten sonra hizmetleri kullanarak bileşenleri Razor şablonlarına ekleme [ \@ekleme](xref:mvc/views/razor#section-4) Razor yönergesi. `@inject` iki parametreye sahiptir:

* Tür: Eklenecek hizmetin türü.
* Özellik: Eklenen app service alma özelliğinin adı. El ile oluşturma özelliğini gerektirmez. Derleyici, bir özellik oluşturur.

Daha fazla bilgi için bkz. <xref:mvc/views/dependency-injection>.

Birden çok kullanın `@inject` farklı Hizmetleri eklemesine deyimleri.

Aşağıdaki örnek nasıl kullanılacağını gösterir `@inject`. Uygulama hizmeti `Services.IDataAccess` bileşenin özelliğine eklenen `DataRepository`. Kodu yalnızca nasıl kullandığını unutmayın `IDataAccess` Özet:

[!code-cshtml[](dependency-injection/samples_snapshot/3.x/CustomerList.razor?highlight=2-3,23)]

Dahili olarak oluşturulan özelliğe (`DataRepository`) ile donatılmış `InjectAttribute` özniteliği. Genellikle, bu öznitelik, doğrudan kullanılmaz. Bir temel sınıf bileşenleri için gereklidir ve eklenen özellikler için temel sınıf gerekli Ayrıca, el ile eklemeniz `InjectAttribute`:

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

Temel sınıftan türetilmiş bileşenlerinde `@inject` yönergesi gerekli değildir. `InjectAttribute` Temel sınıfını yeterlidir:

```cshtml
@page "/demo"
@inherits ComponentBase

<h1>Demo Component</h1>
```

## <a name="dependency-injection-in-services"></a>Hizmetleri bağımlılık ekleme

Karmaşık services ek hizmetleri gerektirebilir. Önceki örnekte, `DataAccess` gerektirebilir `HttpClient` varsayılan hizmeti. `@inject` (veya `InjectAttribute`) hizmetlerini kullanmak için kullanılamaz. *Oluşturucu ekleme* yerine kullanılmalıdır. Gerekli hizmetler, hizmetin oluşturucusuna parametre ekleyerek eklenir. Bağımlılık ekleme hizmet oluşturduğu zaman, oluşturucuda gerektirir ve uygun şekilde sağlayan hizmetler tanır.

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

Oluşturucu ekleme önkoşulları:

* Bir oluşturucu bağımsız değişkenleri tüm bağımlılık ekleme tarafından yerine getirilmesi mevcut olması gerekir. Bunlar varsayılan değerleri belirtirseniz DI tarafından kapsanmayan ek parametreler izin verilir.
* Geçerli bir oluşturucusu olmalıdır *genel*.
* Geçerli bir oluşturucusu bulunmalıdır. Bir belirsizlik durumunda DI özel durum oluşturur.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
