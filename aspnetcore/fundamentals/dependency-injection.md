---
title: ASP.NET core'da bağımlılık ekleme
author: guardrex
description: ASP.NET Core bağımlılık ekleme nasıl uyguladığını ve nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/09/2019
uid: fundamentals/dependency-injection
ms.openlocfilehash: 1455aa9ce4ea24eaeb396134f91b6d089b346c17
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724432"
---
# <a name="dependency-injection-in-aspnet-core"></a>ASP.NET core'da bağımlılık ekleme

Tarafından [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), ve [Luke Latham](https://github.com/guardrex)

ASP.NET Core destekleyen bir tekniktir elde etmek için bağımlılık ekleme (dı) yazılım tasarım deseni [denetimi tersine çevirme (IOC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) sınıfları ve bunların bağımlılıklarını arasında.

Özel bağımlılık ekleme MVC denetleyicileri içinde daha fazla bilgi için bkz. <xref:mvc/controllers/dependency-injection>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="overview-of-dependency-injection"></a>Bağımlılık ekleme genel bakış

A *bağımlılık* olan başka bir nesneye gerektiren herhangi bir nesne. Aşağıdaki inceleyin `MyDependency` sınıfıyla birlikte bir `WriteMessage` diğer sınıfların bir uygulamada bağlı yöntemi:

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

Örneği `MyDependency` sınıfı hale getirmek için oluşturulabilir `WriteMessage` yöntemi bir sınıf için kullanılabilir. `MyDependency` Sınıfı, bağımlılık olarak `IndexModel` sınıfı:

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

Bir sınıf oluşturur ve doğrudan bağlı `MyDependency` örneği. Kod bağımlılıklarını (örneğin, önceki örnekte) sorunlu ve aşağıdaki nedenlerle kaçınılmalıdır:

* Değiştirilecek `MyDependency` sınıfı farklı bir uygulama ile değiştirilmesi gerekir.
* Varsa `MyDependency` bağımlılıkları varsa, sınıf tarafından yapılandırılması gerekir. Yapılandırmanıza bağlı olarak birden çok sınıf ile büyük bir projedeki `MyDependency`, uygulama yapılandırma kodu dağılmış olur.
* Bu uygulama için birim testi zordur. Uygulamayı bir sahte kullanın veya saplama `MyDependency` sınıfı bu yaklaşımı mümkün değildir.

Bağımlılık ekleme aracılığıyla bu sorunları ele alır:

* Bağımlılık uygulama soyutlamak için bir arabirim veya temel sınıfının kullanın.
* Bir hizmet kapsayıcısı bağımlılığı kaydı. ASP.NET Core sağlayan bir yerleşik hizmet kapsayıcı <xref:System.IServiceProvider>. Hizmetleri uygulamanın kayıtlı `Startup.ConfigureServices` yöntemi.
* *Ekleme* hizmetinin, kullanıldığı sınıfının oluşturucusu, içine. Framework bağımlılığı örneği oluşturma ve artık gerekli değilse bunun atılması sorumluluğu alır.

İçinde [örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/dependency-injection/samples), `IMyDependency` arabirim uygulamasına hizmet sağlayan bir yöntem tanımlar:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IMyDependency.cs?name=snippet1)]

Bu arabirimin somut bir tür tarafından uygulanan `MyDependency`:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/MyDependency.cs?name=snippet1)]

`MyDependency` istekler bir <xref:Microsoft.Extensions.Logging.ILogger`1> kendi oluşturucusuna. Bağımlılık ekleme zincirleme bir şekilde kullanmak alışılmadık bir durum değildir. İstenen her bağımlılık sırayla kendi bağımlılıkları ister. Kapsayıcı graftaki bağımlılıklarını çözen ve tam olarak çözümlenmiş hizmeti döndürür. Toplu kümesi çözümlenemedi bağımlılıklar, tipik olarak adlandırılır bir *Bağımlılık ağacı*, *bağımlılık grafiği*, veya *Nesne grafiği*.

`IMyDependency` ve `ILogger<TCategoryName>` service kapsayıcısında kayıtlı olması gerekir. `IMyDependency` kaydedilmiştir `Startup.ConfigureServices`. `ILogger<TCategoryName>` senaryomuz için günlük soyutlama altyapısı tarafından kayıtlı bir [framework tarafından sağlanan hizmet](#framework-provided-services) framework tarafından varsayılan olarak kayıtlı.

Kapsayıcı çözümler `ILogger<TCategoryName>` yararlanarak [(Genel) açık türler](/dotnet/csharp/language-reference/language-specification/types#open-and-closed-types), kaydedilecek ihtiyacını her [(Genel) oluşturulan türü](/dotnet/csharp/language-reference/language-specification/types#constructed-types):

```csharp
services.AddSingleton(typeof(ILogger<T>), typeof(Logger<T>));
```

Örnek uygulamada `IMyDependency` hizmet somut bir türde ile kayıtlı `MyDependency`. Tek bir isteğin ömrü hizmet ömrü kayıt kapsamları. [Hizmet ömrü](#service-lifetimes) bu konunun ilerleyen bölümlerinde açıklanmıştır.

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=5)]

> [!NOTE]
> Her `services.Add{SERVICE_NAME}` genişletme yöntemi ekler (ve büyük olasılıkla yapılandırır) Hizmetleri. Örneğin, `services.AddMvc()` Razor sayfaları ve MVC gerekli hizmetleri ekler. Uygulamalar bu kurala uymayan önerilir. Yerleştirin alanında uzantı yöntemlerini <xref:Microsoft.Extensions.DependencyInjection?displayProperty=fullName> hizmet kayıtları grupları kapsüllemek için ad alanı.

Hizmetin Oluşturucusu gerektiriyorsa bir [yerleşik tür](/dotnet/csharp/language-reference/keywords/built-in-types-table), gibi bir `string`, türü kullanarak yerleştirilebilir [yapılandırma](xref:fundamentals/configuration/index) veya [seçenekleri deseni](xref:fundamentals/configuration/options):

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

Hizmet nerede ve özel bir alana atanan bir sınıfın Oluşturucusu aracılığıyla Hizmeti'nin bir örneğini istenir. Alan sınıfı boyunca gerektiği gibi hizmete erişmek için kullanılır.

Örnek uygulamada `IMyDependency` örneği istenen ve hizmetin çağırmak için kullanılan `WriteMessage` yöntemi:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3,6,13,29-30)]

## <a name="framework-provided-services"></a>Framework tarafından sağlanan hizmetleri

`Startup.ConfigureServices` Yöntemi, Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri dahil olmak üzere uygulamanın kullandığı hizmetleri tanımlamak için sorumludur. Başlangıçta `IServiceCollection` sağlanan `ConfigureServices` tanımlanan aşağıdaki Hizmetleri (bağlı olarak [konak nasıl yapılandırılan](xref:fundamentals/index#host)):

| Hizmet Türü | Ömür |
| ------------ | -------- |
| <xref:Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory?displayProperty=fullName> | Geçici |
| <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime?displayProperty=fullName> | singleton |
| <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment?displayProperty=fullName> | singleton |
| <xref:Microsoft.AspNetCore.Hosting.IStartup?displayProperty=fullName> | singleton |
| <xref:Microsoft.AspNetCore.Hosting.IStartupFilter?displayProperty=fullName> | Geçici |
| <xref:Microsoft.AspNetCore.Hosting.Server.IServer?displayProperty=fullName> | singleton |
| <xref:Microsoft.AspNetCore.Http.IHttpContextFactory?displayProperty=fullName> | Geçici |
| <xref:Microsoft.Extensions.Logging.ILogger`1?displayProperty=fullName> | singleton |
| <xref:Microsoft.Extensions.Logging.ILoggerFactory?displayProperty=fullName> | singleton |
| <xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider?displayProperty=fullName> | singleton |
| <xref:Microsoft.Extensions.Options.IConfigureOptions`1?displayProperty=fullName> | Geçici |
| <xref:Microsoft.Extensions.Options.IOptions`1?displayProperty=fullName> | singleton |
| <xref:System.Diagnostics.DiagnosticSource?displayProperty=fullName> | singleton |
| <xref:System.Diagnostics.DiagnosticListener?displayProperty=fullName> | singleton |

Hizmet koleksiyonu genişletme yöntemi, kayıt hizmeti (ve gerekirse, bağımlı hizmetler) kullanılabilir olduğunda, kuralı tek bir kullanmaktır `Add{SERVICE_NAME}` genişletme yöntemi bu hizmet tarafından gerekli tüm hizmetleri kaydedilecek. Aşağıdaki kod, genişletme yöntemleri kullanarak kapsayıcıya ek hizmetleri ekleme örneğidir <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*>, <xref:Microsoft.Extensions.DependencyInjection.IdentityServiceCollectionExtensions.AddIdentityCore*>, ve <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>:

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

Daha fazla bilgi için <xref:Microsoft.Extensions.DependencyInjection.ServiceCollection> clss API belgelerinde.

## <a name="service-lifetimes"></a>Hizmet yaşam süresi yok

Kayıtlı her hizmet için uygun bir yaşam süresi'ni seçin. ASP.NET Core Hizmetleri ile aşağıdaki ömürleri yapılandırılabilir:

### <a name="transient"></a>Geçici

Geçici ömrü Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddTransient*>) hizmet kapsayıcısından talep her zaman oluşturulur. Bu yaşam süresi, basit, durum bilgisi olmayan hizmetler için en iyi çalışır.

### <a name="scoped"></a>Kapsamlı

Kapsamlı ömrü Hizmetleri (<xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddScoped*>) (bağlantı) istemci istek bir kez oluşturulur.

> [!WARNING]
> Kapsamlı bir hizmeti bir ara yazılımında kullanılırken, hizmette oturum ekleme `Invoke` veya `InvokeAsync` yöntemi. Bir tekliyi gibi davranmaya hizmet zorladığından Oluşturucu ekleme ekleme yok. Daha fazla bilgi için bkz. <xref:fundamentals/middleware/index>.

### <a name="singleton"></a>singleton

Singleton ömrü Hizmetleri (<xref:Microsoft.AspNet.OData.Builder.ODataModelBuilder.AddSingleton*>) istenen ilk kez oluşturulur (veya `Startup.ConfigureServices` çalıştırılır ve örneği hizmet kaydı ile belirtilir). Sonraki her istek, aynı örneğini kullanır. Uygulama tekil davranışı gerektiriyorsa, hizmetin ömrünü yönetmek hizmet kapsayıcı izin vererek önerilir. Yoksa, tekil tasarım desenini uygulama ve sınıfında nesnenin ömrünü yönetmek üzere kullanıcı kodunun sağlayın.

> [!WARNING]
> Tek bir kapsamlı hizmet çözümlenecek tehlikelidir. Bu, sonraki istekleri işleme sırasında hatalı duruma sahip hizmetinin neden olabilir.

## <a name="service-registration-methods"></a>Hizmet kayıt yöntemleri

Her bir hizmet kayıt genişletme yöntemi, belirli senaryolarda yararlı olan aşırı yüklemeler sağlar.

| Yöntem | Otomatik<br>nesne<br>çıkarma | Birden Çok<br>uygulamalar | Bağımsız değişken geçirme |
| ------ | :-----------------------------: | :-------------------------: | :-------: |
| `Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()`<br>Örnek:<br>`services.AddScoped<IMyDep, MyDep>();` | Evet | Evet | Hayır |
| `Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})`<br>Örnekler:<br>`services.AddScoped<IMyDep>(sp => new MyDep());`<br>`services.AddScoped<IMyDep>(sp => new MyDep("A string!"));` | Evet | Evet | Evet |
| `Add{LIFETIME}<{IMPLEMENTATION}>()`<br>Örnek:<br>`services.AddScoped<MyDep>();` | Evet | Hayır | Hayır |
| `Add{LIFETIME}<{SERVICE}>(new {IMPLEMENTATION})`<br>Örnekler:<br>`services.AddScoped<IMyDep>(new MyDep());`<br>`services.AddScoped<IMyDep>(new MyDep("A string!"));` | Hayır | Evet | Evet |
| `Add{LIFETIME}(new {IMPLEMENTATION})`<br>Örnekler:<br>`services.AddScoped(new MyDep());`<br>`services.AddScoped(new MyDep("A string!"));` | Hayır | Hayır | Evet |

Tür çıkarma hakkında daha fazla bilgi için bkz. [Hizmetleri elden](#disposal-of-services) bölümü. Birden çok uygulamaları için yaygın bir senaryodur [türleri test etmek için sahte işlem](xref:test/integration-tests#inject-mock-services).

`TryAdd{LIFETIME}` kayıtlı bir uygulama zaten yoksa yöntem yalnızca hizmet kaydedin.

Aşağıdaki örnekte, ilk satırı kaydeder `MyDependency` için `IMyDependency`. İkinci satır, çünkü hiçbir etkisi olmaz `IMyDependency` zaten kayıtlı bir uygulama vardır:

```csharp
services.AddSingleton<IMyDependency, MyDependency>();
// The following line has no effect:
services.TryAddSingleton<IMyDependency, DifferentDependency>();
```

Daha fazla bilgi için bkz.:

* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAdd*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddTransient*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddScoped*>
* <xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddSingleton*>

[TryAddEnumerable(ServiceDescriptor)](xref:Microsoft.Extensions.DependencyInjection.Extensions.ServiceCollectionDescriptorExtensions.TryAddEnumerable*) yöntemi uygulaması zaten yoksa hizmet yalnızca kaydetmek *aynı türde*. Birden çok hizmeti aracılığıyla çözümlenir `IEnumerable<{SERVICE}>`. Geliştirici Hizmetleri kaydedilirken yalnızca bir aynı türden zaten eklenmemişse, örnek eklemek istiyor. Genellikle, bu yöntem, kitaplık yazarları tarafından örneği iki kopyasını kapsayıcıda kaydetme önlemek için kullanılır.

Aşağıdaki örnekte, ilk satırı kaydeder `MyDep` için `IMyDep1`. İkinci satır kaydeder `MyDep` için `IMyDep2`. Üçüncü satır, çünkü hiçbir etkisi olmaz `IMyDep1` kayıtlı bir uygulaması zaten `MyDep`:

```csharp
public interface IMyDep1 {}
public interface IMyDep2 {}

public class MyDep : IMyDep1, IMyDep2 {}

services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep2, MyDep>());
// Two registrations of MyDep for IMyDep1 is avoided by the following line:
services.TryAddEnumerable(ServiceDescriptor.Singleton<IMyDep1, MyDep>());
```

### <a name="constructor-injection-behavior"></a>Oluşturucu yerleştirme davranışı

Hizmetleri iki mekanizma tarafından çözülebilir:

* <xref:System.IServiceProvider>
* <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities> &ndash; Nesne oluşturma bağımlılık ekleme kapsayıcısına hizmet kaydı olmadan izin verir. `ActivatorUtilities` Etiket Yardımcıları, MVC denetleyicileri ve model bağlayıcılar gibi kullanıcıya yönelik soyutlama ile kullanılır.

Oluşturucular, bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bağımsız değişken varsayılan değerleri atamanız gerekir.

Ne zaman Hizmetleri çözülmüş tarafından `IServiceProvider` veya `ActivatorUtilities`, oluşturucu ekleme gerektiren bir *genel* Oluşturucusu.

Ne zaman Hizmetleri çözülmüş tarafından `ActivatorUtilities`, oluşturucu ekleme gerektirir, yalnızca bir geçerli oluşturucusu yok. Oluşturucu aşırı yüklemeleri tarafından desteklenir, ancak yalnızca bir aşırı yükleme bağımsız değişkenleri tüm bağımlılık ekleme tarafından yerine getirilmesi bulunabilir.

## <a name="entity-framework-contexts"></a>Entity Framework bağlamları

Entity Framework bağlamları, hizmeti kullanarak kapsayıcı genellikle eklenir [kapsamlı ömrü](#service-lifetimes) web uygulama veritabanı işlemlerini normalde istemci isteğini kapsamındaki olduğundan. Bir yaşam süresi tarafından belirtilmezse varsayılan yaşam süresi kapsamlıdır bir <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext*> veritabanı bağlamı kaydederken aşırı yükleme. Belirli bir yaşam süresi Hizmetleri, hizmet daha kısa bir yaşam süresi ile bir veritabanı bağlamı kullanmamalısınız.

## <a name="lifetime-and-registration-options"></a>Yaşam süresi ve kayıt seçenekleri

Yaşam süresi ve kayıt seçenekleri arasındaki farkı göstermek için benzersiz bir tanımlayıcıya sahip bir işlem olarak görevleri temsil eden aşağıdaki arabirimlerinden göz önünde bulundurun. `OperationId`. Kapsayıcı, bir işlem hizmeti kullanım ömrü için aşağıdaki arabirimlerinden nasıl yapılandırıldığına bağlı olarak, aynı veya farklı bir örneğine bir sınıf tarafından istendiğinde hizmetinin sağlar:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Interfaces/IOperation.cs?name=snippet1)]

Arabirimler uygulanan `Operation` sınıfı. `Operation` Oluşturucusu bir sağlanan değilse bir GUID oluşturur:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Models/Operation.cs?name=snippet1)]

Bir `OperationService` kayıtlı, bağlı her diğer `Operation` türleri. Zaman `OperationService` istenen bağımlılık ekleme aldığı her hizmetin yeni bir örneğini veya bağımlı hizmetin lifetime öğesine göre var olan bir örneği.

* Geçici Hizmetleri kapsayıcısından istendiğinde oluşturulduğunda `OperationId` , `IOperationTransient` hizmetidir farklı `OperationId` , `OperationService`. `OperationService` Yeni bir örneğini alır `IOperationTransient` sınıfı. Yeni örnek farklı bir verir `OperationId`.
* Her istemci isteği, kapsamı belirlenmiş hizmetler oluştururken `OperationId` , `IOperationScoped` hizmeti aynı olan `OperationService` içinde bir istemci isteği. İstemci istekleri arasında her iki hizmet de farklı bir paylaşım `OperationId` değeri.
* Singleton ve tek örnekli Hizmetleri oluşturulduktan sonra ve tüm istemci isteklerini ve tüm hizmetlerde kullanılan `OperationId` tüm hizmet istekler genelinde sabittir.

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Services/OperationService.cs?name=snippet1)]

İçinde `Startup.ConfigureServices`, her tür kapsayıcı adlandırılmış ömrü göre eklenir:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Startup.cs?name=snippet1&highlight=6-9,12)]

`IOperationSingletonInstance` Hizmetinin belirli bir örneği, bilinen bir kimliği ile kullandığı `Guid.Empty`. Bu tür (GUID'sine sıfır olur) kullanımda olmadığında işaretlenmemiştir.

Örnek uygulama, nesne kullanım ömrü içindeki ve arasındaki tek tek istekleri gösterir. Örnek uygulamanın `IndexModel` her tür istekleri `IOperation` türü ve `OperationService`. Sayfa sonra tüm sayfa modeli sınıfın ve hizmetin görüntüler `OperationId` özelliği aracılığıyla değerleri:

[!code-csharp[](dependency-injection/samples/2.x/DependencyInjectionSample/Pages/Index.cshtml.cs?name=snippet1&highlight=7-11,14-18,21-25)]

İki aşağıdaki çıktıda gösterildiği iki isteği sonuçları:

**İlk istek:**

Denetleyici işlemler:

Transient: d233e165-f417-469b-a866-1cf1935d2518  
Kapsamı: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Örnek: 00000000-0000-0000-0000-000000000000

`OperationService` İşlemler:

Transient: c6b049eb-1318-4e31-90f1-eb2dd849ff64  
Kapsamı: 5d997e2d-55f5-4a64-8388-51c4e3a1ad19  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Örnek: 00000000-0000-0000-0000-000000000000

**İkinci isteği:**

Denetleyici işlemler:

Geçici: b63bd538-0a37-4ff1-90ba-081c5138dda0  
Kapsamı: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Örnek: 00000000-0000-0000-0000-000000000000

`OperationService` İşlemler:

Transient: c4cbacb8-36a2-436d-81c8-8c1b78808aaf  
Kapsamı: 31e820c5-4834-4d22-83fc-a60118acb9f4  
Singleton: 01271bc1-9e31-48e7-8f7c-7261b040ded9  
Örnek: 00000000-0000-0000-0000-000000000000

Hangi gözlemleyin `OperationId` değerleri, bir istek içinde ve istekler arasında değişir:

* *Geçici* nesneleri farklı her zaman. Geçici `OperationId` değeri için birinci ve ikinci istemci istekleri için hem de farklı `OperationService` işlemleri ve istemci istekleri arasında. Yeni bir örneği, her bir hizmet isteğini ve istemci isteği sağlanır.
* *Kapsamlı* nesneleri, ancak farklı bir istemci isteği içinde aynı istemci isteklerinde.
* *Singleton* nesneleri, her nesne ve bağımsız olarak her istek için aynı bir `Operation` örneği sağlanır `Startup.ConfigureServices`.

## <a name="call-services-from-main"></a>Ana arama hizmetleri

Oluşturma bir <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> ile [IServiceScopeFactory.CreateScope](xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope*) uygulamanın kapsamı içinde kapsamlı bir hizmet çözümlenecek. Bu yaklaşım, başlangıçta başlatma görevleri çalıştırmak için kapsamlı bir hizmete erişmek yararlıdır. Aşağıdaki örnek için bir bağlam elde etme gösterir `MyScopedService` içinde `Program.Main`:

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

## <a name="scope-validation"></a>Kapsam doğrulama

Uygulama geliştirme ortamında çalışırken, varsayılan hizmet sağlayıcısını doğrulamak için denetimleri gerçekleştirir:

* Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök hizmet sağlayıcısından çözülmüş değildir.
* Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değildir.

Kök hizmet sağlayıcısı oluşturulur <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider*> çağrılır. Kök hizmet sağlayıcısının bir ömür zaman sağlayıcısı uygulamayla başlar ve uygulama kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.

Kapsamlı Hizmetleri, onları oluşturan kapsayıcı tarafından elden çıkarılmasını. Kapsamlı bir hizmet içinde kök kapsayıcı oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından bırakılmadan olduğundan hizmet ömrü tekliye etkili bir şekilde yükseltilir. Hizmet kapsamları doğrulama yakalar bu gibi durumlarda, `BuildServiceProvider` çağrılır.

Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host#scope-validation>.

## <a name="request-services"></a>Hizmet isteği

Bir ASP.NET Core içinde kullanılabilen hizmetler request `HttpContext` aracılığıyla kullanıma [HttpContext.RequestServices](xref:Microsoft.AspNetCore.Http.HttpContext.RequestServices) koleksiyonu.

İstek Hizmetleri yapılandırılmış ve uygulamanın bir parçası istenen Hizmetleri temsil eder. Nesneleri bağımlılıkları belirttiğinizde, bu bulunan türleri tarafından karşılandığından `RequestServices`değil `ApplicationServices`.

Genel olarak, uygulamayı doğrudan bu özellikleri kullanmamalısınız. Bunun yerine, sınıfları sınıf oluşturucuları gerektirir ve framework izin türlerini bağımlılıkları ekleme isteği. Bu, test etmek daha kolay olan sınıfları verir.

> [!NOTE]
> Erişim için oluşturucu parametresi olarak bağımlılıkları isteyen tercih `RequestServices` koleksiyonu.

## <a name="design-services-for-dependency-injection"></a>Tasarım Hizmetleri için bağımlılık ekleme

İçin en iyi uygulamalar şunlardır:

* Bağımlılık ekleme bağımlılıklarını almak için kullanmak üzere hizmetlerin tasarlayın.
* Durum bilgisi olan ve statik yöntem çağrıları kaçının.
* Bağımlı hizmetleri sınıfları doğrudan örneğinin kaçının. Doğrudan, kodu belirli bir uygulamaya couples.
* Uygulama sınıfları küçük, katsayıları iyi belirlenmiş ve test edilmiş kolayca yapın.

Bir sınıf çok fazla eklenen bağımlılıklara sahip gibi görünüyor. Bu genellikle bir oturum sınıfı için çok fazla sorumluluklara sahiptir ve ihlal varsa, [tek sorumluluk İlkesi'ni (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility). Sınıfının yeni bir sınıf bazı sorumlulukları taşıyarak yeniden deneyin. Razor sayfaları sayfa modeli sınıfları ve MVC denetleyici sınıflarına kullanıcı Arabirimi konuları üzerinde durmalısınız aklınızda bulundurun. İş kuralları ve veri erişim uygulama ayrıntılarını saklanır bunları uygun sınıflardaki [konuları ayrı](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns).

### <a name="disposal-of-services"></a>Bırakma Hizmetleri

Kapsayıcı çağrıları <xref:System.IDisposable.Dispose*> için <xref:System.IDisposable> türleri oluşturur. Bir örneği, kullanıcı kodu tarafından kapsayıcıya eklenirse, otomatik olarak elden değil.

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

## <a name="default-service-container-replacement"></a>Varsayılan hizmet kapsayıcısını değiştirme

Yerleşik hizmet kapsayıcı framework ve çoğu tüketici uygulamalarına gereksinimlerini karşılamak üzere tasarlanmıştır. Bunu desteklemiyor belirli bir özellik gerekmedikçe yerleşik kapsayıcı kullanmanızı öneririz. Yerleşik kapsayıcısında bulunamadı 3 taraf kapsayıcıları destekleyen özelliklerden bazıları:

* Özellik ekleme
* Adına göre ekleme
* Alt kapsayıcılar
* Özel ömür Yönetimi
* `Func<T>` Yavaş başlatma desteği

Bkz: [bağımlılık ekleme Benioku.MD dosyası](https://github.com/aspnet/Extensions/tree/master/src/DependencyInjection) bazı bağdaştırıcıları destekleyen kapsayıcıların listesi.

Aşağıdaki örnek yerleşik kapsayıcıyla değiştirir [Autofac](https://autofac.org/):

* Uygun bir kapsayıcı paketleri yükleyin:

  * [Autofac](https://www.nuget.org/packages/Autofac/)
  * [Autofac.Extensions.DependencyInjection](https://www.nuget.org/packages/Autofac.Extensions.DependencyInjection/)

* Kapsayıcıda yapılandırma `Startup.ConfigureServices` ve dönüş bir `IServiceProvider`:

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

    3 bir taraf kapsayıcı kullanılacak `Startup.ConfigureServices` döndürmelidir `IServiceProvider`.

* İçinde Autofac yapılandırma `DefaultModule`:

    ```csharp
    public class DefaultModule : Module
    {
        protected override void Load(ContainerBuilder builder)
        {
            builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
        }
    }
    ```

Çalışma zamanında Autofac, türleri çözümlemek ve bağımlılıkları ekleme için kullanılır. ASP.NET Core ile Autofac kullanma hakkında daha fazla bilgi edinmek için [Autofac belgeleri](https://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>İş parçacığı güvenliği

İş parçacığı açısından güvenli tekil Hizmetleri oluşturun. Tek bir hizmet üzerinde geçici bir hizmet bağımlılığı varsa, geçici hizmet tekli tarafından nasıl kullanıldığını bağlı olarak iş parçacığı güvenliği de gerektirebilir.

İkinci bağımsız değişkeni gibi tek hizmet Üreteç yöntemi [AddSingleton\<TService > (IServiceCollection, Func\<IServiceProvider, TService >)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*), iş parçacığı açısından güvenli olması gerekmez. Gibi bir tür (`static`) oluşturucusu, onu garanti tek bir iş parçacığı tarafından bir kez çağrılabilir.

## <a name="recommendations"></a>Öneriler

* `async/await` ve `Task` tabanlı hizmet çözümlemesi desteklenmiyor. C#zaman uyumsuz oluşturucuları desteklemez; Bu nedenle, önerilen Düzen zaman uyumsuz yöntemleri zaman uyumlu olarak hizmet çözdükten sonra kullanmaktır.

* Verileri ve Yapılandırma hizmeti kapsayıcısında doğrudan depolama kaçının. Örneğin, bir kullanıcının alışveriş sepeti genellikle hizmet kapsayıcıya eklenen olmamalıdır. Yapılandırma kullanması gereken [seçenekleri deseni](xref:fundamentals/configuration/options). Benzer şekilde, yalnızca başka bir nesnenin erişmesine izin vermek için mevcut "veri sahibi" nesneleri kaçının. İstek DI aracılığıyla gerçek öğesi daha iyidir.

* Statik hizmetlere erişimi önlemek (örneğin, statik olarak yazmaya [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) kullanan başka bir yerde için).

* Kullanmaktan kaçının *hizmet bulucu deseni*. Örneğin, çağırma yoksa <xref:System.IServiceProvider.GetService*> DI yerine kullandığınızda bir hizmet örneği elde etmek için:

  **Yanlış:**

  ```csharp
  public void MyMethod()
  {
      var options = 
          _services.GetService<IOptionsMonitor<MyOptions>>();
      var option = options.CurrentValue.Option;

      ...
  }
  ```

  **Doğru**:

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

* Çalışma zamanında bağımlılıklarını çözen bir Fabrika önlemek için başka bir hizmet bulucu değişim çalıştırıyorsunuzdur. Bu yöntemler karışımı [tersine çevirme denetim](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) stratejiler.

* Statik erişimi önlemek `HttpContext` (örneğin, [IHttpContextAccessor.HttpContext](xref:Microsoft.AspNetCore.Http.IHttpContextAccessor.HttpContext)).

Öneriler tüm kümesi gibi bir öneri yok sayılıyor gerekli olduğu durumlarla karşılaşabilirsiniz. Özel durumlar nadir&mdash;çoğunlukla framework içinde özel durumlar.

DI olduğu bir *alternatif* statik/genel nesne erişim desenleri. Statik nesne erişimi ile karıştırmak istiyorsanız DI avantajlarından mümkün olmayabilir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/views/dependency-injection>
* <xref:mvc/controllers/dependency-injection>
* <xref:security/authorization/dependencyinjection>
* <xref:blazor/dependency-injection>
* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/extensibility>
* [Bağımlılık ekleme (MSDN) ile ASP.NET Core kod yazma](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Özel bağımlılıklar İlkesi](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)
* [Tersine çevirme denetimi kapsayıcıları ve bağımlılık ekleme desenini (Martin Fowler)](https://www.martinfowler.com/articles/injection.html)
* [Bir hizmet birden fazla arabirimde ASP.NET Core DI ile kaydetme](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
