---
title: ASP.NET Core Blazor bağımlılığı ekleme
author: guardrex
description: Blazor uygulamalarının bileşenlere nasıl hizmet ekleyebilmesi için bkz.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2020
no-loc:
- Blazor
- SignalR
uid: blazor/dependency-injection
ms.openlocfilehash: 4cdde9ee8c9fd9adf00894a067d32965b180e5ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658076"
---
# <a name="aspnet-core-blazor-dependency-injection"></a>ASP.NET Core Blazor bağımlılığı ekleme

Tarafından [Rainer Stropek](https://www.timecockpit.com) ve [Mike rousos](https://github.com/mjrousos)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor, [bağımlılık ekleme işlemini (dı)](xref:fundamentals/dependency-injection)destekler. Uygulamalar, yerleşik hizmetleri ekleme tarafından bileşenlere kullanabilir. Uygulamalar Ayrıca özel hizmetleri tanımlayabilir ve kaydedebilir ve bu Hizmetleri uygulama genelinde DI aracılığıyla kullanılabilir hale getirebilirsiniz.

DI, merkezi bir konumda yapılandırılmış hizmetlere erişmek için bir tekniktir. Bu, Blazor uygulamalarında şu şekilde yararlı olabilir:

* Hizmet sınıfının tek bir *örneğini tek bir hizmet olarak* bilinen birçok bileşen arasında paylaşabilirsiniz.
* Başvuru soyutlamalarını kullanarak somut hizmet sınıflarından bileşenleri ayırın. Örneğin, uygulamadaki verilere erişmek için bir arabirim `IDataAccess` düşünün. Arabirim somut bir `DataAccess` sınıfı tarafından uygulanır ve uygulamanın hizmet kapsayıcısında bir hizmet olarak kaydedilir. Bir bileşen bir `IDataAccess` uygulamasını almak için DI kullandığında, bileşen somut tür ile eşleştirilmez. Uygulama, büyük olasılıkla birim testlerinde bir sahte uygulama için değiştirilebilir.

## <a name="default-services"></a>Varsayılan hizmetler

Varsayılan hizmetler, uygulamanın hizmet koleksiyonuna otomatik olarak eklenir.

| Hizmet | Ömür | Açıklama |
| ------- | -------- | ----------- |
| <xref:System.Net.Http.HttpClient> | adet | HTTP istekleri göndermek ve bir URI tarafından tanımlanan bir kaynaktan HTTP yanıtlarını almak için yöntemler sağlar.<br><br>Bir Blazor WebAssembly uygulamasındaki `HttpClient` örneği, arka planda HTTP trafiğini işlemek için tarayıcıyı kullanır.<br><br>Blazor sunucu uygulamaları, varsayılan olarak hizmet olarak yapılandırılmış bir `HttpClient` içermez. Bir Blazor sunucu uygulamasına `HttpClient` sağlayın.<br><br>Daha fazla bilgi için bkz. <xref:blazor/call-web-api>. |
| `IJSRuntime` | Singleton (Blazor WebAssembly)<br>Kapsamlı (Blazor sunucusu) | JavaScript çağrılarının dağıtıldığı bir JavaScript çalışma zamanının örneğini temsil eder. Daha fazla bilgi için bkz. <xref:blazor/call-javascript-from-dotnet>. |
| `NavigationManager` | Singleton (Blazor WebAssembly)<br>Kapsamlı (Blazor sunucusu) | URI 'Ler ve gezinme durumu ile çalışmaya yönelik yardımcıları içerir. Daha fazla bilgi için bkz. [URI ve gezinti durumu yardımcıları](xref:blazor/routing#uri-and-navigation-state-helpers). |

Özel bir hizmet sağlayıcı, tabloda listelenen varsayılan Hizmetleri otomatik olarak sağlamaz. Özel bir hizmet sağlayıcısı kullanır ve tabloda gösterilen hizmetlerden herhangi birini gerekliyse, gerekli hizmetleri yeni hizmet sağlayıcısına ekleyin.

## <a name="add-services-to-an-app"></a>Uygulamaya hizmet ekleme

### <a name="blazor-webassembly"></a>Blazor WebAssembly

*Program.cs*'ın `Main` yönteminde uygulamanın hizmet koleksiyonu için Hizmetleri yapılandırın. Aşağıdaki örnekte, `MyDependency` uygulama `IMyDependency`için kaydedilir:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<IMyDependency, MyDependency>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

Konak oluşturulduktan sonra, herhangi bir bileşen işlenmeden önce kök dı kapsamından hizmetlere erişilebilir. Bu, içerik işlemeden önce başlatma mantığını çalıştırmak için yararlı olabilir:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync();

        await host.RunAsync();
    }
}
```

Konak, uygulama için bir merkezi yapılandırma örneği de sağlar. Yukarıdaki örnekte derleme yaparken, hava durumu hizmetinin URL 'SI, `InitializeWeatherAsync`için varsayılan bir yapılandırma kaynağından (örneğin, *appSettings. JSON*) geçirilir:

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddSingleton<WeatherService>();
        builder.RootComponents.Add<App>("app");

        var host = builder.Build();

        var weatherService = host.Services.GetRequiredService<WeatherService>();
        await weatherService.InitializeWeatherAsync(
            host.Configuration["WeatherServiceUrl"]);

        await host.RunAsync();
    }
}
```

### <a name="blazor-server"></a>Blazor Server

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

### <a name="service-lifetime"></a>Hizmet ömrü

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

ASP.NET Core uygulamalarda, kapsamlı hizmetler genellikle geçerli isteğin kapsamlandırılır. İstek tamamlandıktan sonra, tüm kapsamlı veya geçici hizmetler dı sistemi tarafından silinir. Blazor Server uygulamalarında istek kapsamı, istemci bağlantısı süresince sürer ve bu da geçici ve kapsamlı hizmetlerin beklenenden çok daha uzun sürebileceği anlamına gelir. Blazor WebAssembly uygulamalarında, kapsamlı bir ömürle kaydedilen hizmetler tekton olarak değerlendirilir, bu nedenle tipik ASP.NET Core uygulamalardaki kapsamlı hizmetlerden daha uzun bir süre yaşarlar.

Blazor uygulamalarında bir hizmet ömrünü sınırlayan bir yaklaşım, `OwningComponentBase` türünü kullanmaktır. `OwningComponentBase`, bileşenin ömrüne karşılık gelen bir dı kapsamı oluşturan `ComponentBase` türetilmiş bir soyut türdür. Bu kapsamı kullanarak, dı hizmetlerini kapsamlı bir ömür ile kullanmak mümkündür ve bileşen olarak bu uygulamaları canlı hale gelir. Bileşen yok edildiğinde, bileşenin kapsamlı hizmet sağlayıcısından gelen hizmetler de silinir. Bu, şu hizmetler için yararlı olabilir:

* Geçici ömür uygun olmadığından, bir bileşen içinde yeniden kullanılmalıdır.
* Tek yaşam süresi uygun olmadığından, bileşenler arasında paylaşılmamalıdır.

`OwningComponentBase` türünün iki sürümü kullanılabilir:

* `OwningComponentBase`, `IServiceProvider`türünde korumalı bir `ScopedServices` özelliğine sahip `ComponentBase` türünün soyut, atılabilir alt öğesidir. Bu sağlayıcı, bileşenin kullanım ömrü kapsamındaki Hizmetleri çözümlemek için kullanılabilir.

  `@inject` veya `InjectAttribute` (`[Inject]`) kullanılarak bileşene eklenen dı Hizmetleri bileşen kapsamında oluşturulmaz. Bileşenin kapsamını kullanmak için, `ScopedServices.GetRequiredService` veya `ScopedServices.GetService`kullanılarak hizmetler çözümlenmelidir. `ScopedServices` sağlayıcısı kullanılarak çözümlenen hizmetlerin bağımlılıkları, aynı kapsamdan sağlanmış olmalıdır.

  ```razor
  @page "/preferences"
  @using Microsoft.Extensions.DependencyInjection
  @inherits OwningComponentBase

  <h1>User (@UserService.Name)</h1>

  <ul>
      @foreach (var setting in SettingService.GetSettings())
      {
          <li>@setting.SettingName: @setting.SettingValue</li>
      }
  </ul>

  @code {
      private IUserService UserService { get; set; }
      private ISettingService SettingService { get; set; }

      protected override void OnInitialized()
      {
          UserService = ScopedServices.GetRequiredService<IUserService>();
          SettingService = ScopedServices.GetRequiredService<ISettingService>();
      }
  }
  ```

* `OwningComponentBase<T>` `OwningComponentBase` türetilir ve kapsamdaki dı sağlayıcısından `T` örneğini döndüren bir özellik `Service` ekler. Bu tür, uygulamanın, bileşenin kapsamını kullanan bir birincil hizmet olduğunda bir `IServiceProvider` örneğini kullanmadan kapsamlı hizmetlere erişmenin kolay bir yoludur. `ScopedServices` özelliği kullanılabilir, bu sayede uygulama, gerekirse diğer türlerde hizmetleri alabilir.

  ```razor
  @page "/users"
  @attribute [Authorize]
  @inherits OwningComponentBase<AppDbContext>

  <h1>Users (@Service.Users.Count())</h1>

  <ul>
      @foreach (var user in Service.Users)
      {
          <li>@user.UserName</li>
      }
  </ul>
  ```

## <a name="use-of-entity-framework-dbcontext-from-di"></a>Dı Entity Framework DbContext kullanımı

Web Apps 'ten gelen sunucudan alınacak bir ortak hizmet türü Entity Framework (EF) `DbContext` nesneleri. `IServiceCollection.AddDbContext` kullanarak EF hizmetlerini kaydetme, varsayılan olarak `DbContext` kapsamlı bir hizmet olarak ekler. Kapsamlı bir hizmet olarak kaydetme, Blazor uygulamalardaki sorunlara yol açabilir çünkü `DbContext` örneklerinin uzun süreli ve uygulama genelinde paylaşılmasına neden olur. `DbContext`, iş parçacığı açısından güvenli değildir ve aynı anda kullanılmamalıdır.

Uygulamaya bağlı olarak, bir `DbContext` kapsamını tek bir bileşenle sınırlandırmak için `OwningComponentBase` kullanmak *sorunu çözebilir.* Bir bileşen paralel olarak bir `DbContext` kullanmıyorsa, bileşeni `OwningComponentBase` türetirmez ve `ScopedServices` `DbContext` almak yeterlidir çünkü şunları sağlar:

* Ayrı bileşenler `DbContext`paylaşmaz.
* `DbContext`, bu bileşene bağlı olarak yalnızca bileşen gibi sürer.

Tek bir bileşen aynı anda bir `DbContext` kullanabilir (örneğin, bir Kullanıcı bir düğme seçtiğinde), `OwningComponentBase` kullanılması de eşzamanlı EF işlemlerinde sorunlardan kaçınmaz. Bu durumda, her mantıksal EF işlemi için farklı bir `DbContext` kullanın. Aşağıdaki yaklaşımlardan birini kullanın:

* Bir bağımsız değişken olarak `DbContextOptions<TContext>` kullanarak `DbContext` doğrudan oluşturun, bu, dı 'dan alınabilecek ve iş parçacığı güvenlidir.

    ```razor
    @page "/example"
    @inject DbContextOptions<AppDbContext> DbContextOptions

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = new AppDbContext(DbContextOptions))
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

* Hizmet kapsayıcısına geçici bir yaşam süresi ile `DbContext` kaydedin:
  * Bağlamı kaydederken `ServiceLifetime.Transient`kullanın. `AddDbContext` uzantısı yöntemi `ServiceLifetime`türünde iki isteğe bağlı parametre alır. Bu yaklaşımı kullanmak için yalnızca `contextLifetime` parametresinin `ServiceLifetime.Transient`olması gerekir. `optionsLifetime`, `ServiceLifetime.Scoped`varsayılan değerini tutabilir.

    ```csharp
    services.AddDbContext<AppDbContext>(options =>
         options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")),
         ServiceLifetime.Transient);
    ```  

  * Geçici `DbContext`, paralel olarak birden çok EF işlemini yürütemeyecek bileşenlere normal (`@inject`kullanılarak) eklenebilir. Aynı anda birden çok EF işlemi gerçekleştirebilecek olanlar, `IServiceProvider.GetRequiredService`kullanarak her paralel işlem için ayrı `DbContext` nesneleri isteyebilir.

    ```razor
    @page "/example"
    @using Microsoft.Extensions.DependencyInjection
    @inject IServiceProvider ServiceProvider

    <ul>
        @foreach (var item in _data)
        {
            <li>@item</li>
        }
    </ul>

    <button @onclick="LoadData">Load Data</button>

    @code {
        private List<string> _data = new List<string>();

        private async Task LoadData()
        {
            _data = await GetAsync();
            StateHasChanged();
        }

        public async Task<List<string>> GetAsync()
        {
            using (var context = ServiceProvider.GetRequiredService<AppDbContext>())
            {
                return await context.Products.Select(p => p.Name).ToListAsync();
            }
        }
    }
    ```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/dependency-injection>
* <xref:mvc/views/dependency-injection>
