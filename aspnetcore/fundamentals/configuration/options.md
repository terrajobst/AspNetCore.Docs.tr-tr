---
title: ASP.NET Core desende seçenekleri
author: guardrex
description: ASP.NET Core uygulamalarında ilgili ayar gruplarını temsil etmek için seçenekleri deseni kullanmayı keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: 359bd438066aefcf572c91dacee99e85c0f10b1a
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207361"
---
# <a name="options-pattern-in-aspnet-core"></a>ASP.NET Core desende seçenekleri

Tarafından [Luke Latham](https://github.com/guardrex)

Seçenekleri deseni sınıfları, ilgili ayar gruplarını temsil etmek için kullanır. Zaman [yapılandırma ayarlarını](xref:fundamentals/configuration/index) yalıtılmış ayrı sınıf senaryoya göre uygulama için iki önemli yazılım Mühendisliği ilkeden uyar:

* [Arabirimi ayırma ilkesi (ISS)](http://deviq.com/interface-segregation-principle/): kullandıkları yapılandırma ayarlarını yapılandırma ayarlarına bağlı olan senaryoları (sınıflar) bağlıdır.
* [Görev ayrımı nettir](http://deviq.com/separation-of-concerns/): uygulamanın farklı kısımlarını ayarları bağımlı veya birbirine bağlı değil.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)) Bu makalede örnek uygulaması ile daha kolaydır.

## <a name="prerequisites"></a>Önkoşullar

::: moniker range=">= aspnetcore-2.1"

Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paket.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Başvuru [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) veya paket başvurusu ekleme [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paket.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Paket başvurusu ekleme [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paket.

::: moniker-end

## <a name="basic-options-configuration"></a>Temel Seçenekler yapılandırma

Temel Seçenekler yapılandırma, örnek olarak gösterilmiştir &num;1 [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Bir seçenek sınıfı soyut olmayan olmalıdır genel parametresiz oluşturucusu ile. Aşağıdaki sınıf `MyOptions`, iki özelliğe sahiptir `Option1` ve `Option2`. Varsayılan değerleri ayarlama, isteğe bağlıdır, ancak aşağıdaki örnekte sınıf oluşturucu varsayılan değerini ayarlar `Option1`. `Option2` özelliği doğrudan başlatarak ayarlanmış varsayılan değerine sahip (*Models/MyOptions.cs*):

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` Sınıfı ile hizmet kapsayıcıya eklenir [yapılandırma&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) ve yapılandırmasına bağlıdır:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

Sayfa modeli kullanan aşağıdaki [Oluşturucusu bağımlılık ekleme](xref:mvc/controllers/dependency-injection) ile [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) ayarlara erişmek için (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Örnek kullanıcının *appsettings.json* dosya için değerler belirten `option1` ve `option2`:

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

Ne zaman uygulamayı çalıştırın, sayfa modelin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> Özel bir kullanırken [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) seçenekleri yapılandırma ayarları dosyadan yüklemek için temel yolu doğru şekilde ayarlandığından emin olun:
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> Temel yol açık olarak ayarlama gerekli değildir seçenekleri yapılandırma ile ayarları dosyasından yüklenirken [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).

## <a name="configure-simple-options-with-a-delegate"></a>Bir temsilci ile basit seçeneklerini yapılandırın

Bir temsilci ile basit seçeneklerini yapılandırma örnek olarak gösterilmiştir &num;2'de [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Bir temsilci seçenekleri değerleri ayarlamak için kullanın. Örnek uygulama kullandığı `MyOptionsWithDelegateConfig` sınıfı (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

Aşağıdaki kodda, ikinci bir `IConfigureOptions<TOptions>` hizmet, hizmet kapsayıcıya eklenir. Bir temsilci ile yapılandırmak için kullandığı `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz. Yapılandırma sağlayıcıları NuGet paketleri içinde kullanılabilir. Kayıtlı sırayla uygulandıkları.

Her çağrı [yapılandırma&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) ekler bir `IConfigureOptions<TOptions>` hizmet kapsayıcıya hizmet. Yukarıdaki örnekte, değerlerini `Option1` ve `Option2` her ikisi de belirtilmiş *appsettings.json*, ancak değerlerini `Option1` ve `Option2` yapılandırılmış temsilci tarafından geçersiz kılınır.

Son yapılandırma kaynağı birden fazla Yapılandırma hizmeti etkinleştirildiğinde, belirtilen *WINS* ve yapılandırma değeri ayarlar. Ne zaman uygulamayı çalıştırın, sayfa modelin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Suboptions yapılandırma

Suboptions yapılandırma, örnek olarak gösterilmiştir &num;3'te [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Uygulamalar, uygulamadaki belirli bir senaryoyu grupları (sınıflar) ilgili seçenekleri sınıflar oluşturmanız gerekir. Yapılandırma değerleri gerektiren uygulama bölümleri, yalnızca kullandıkları yapılandırma değerleri için erişimi olmalıdır.

Seçenekleri yapılandırmayı bağlanırken, seçenek türünün her bir özellik form için bir yapılandırma anahtarı bağlı `property[:sub-property:]`. Örneğin, `MyOptions.Option1` özelliğe anahtarına `Option1`, den okunan `option1` özelliğinde *appsettings.json*.

Aşağıdaki kodda, üçüncü `IConfigureOptions<TOptions>` hizmet, hizmet kapsayıcıya eklenir. Bunu bağlar `MySubOptions` bölümüne `subsection` , *appsettings.json* dosyası:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection` Genişletme yöntemi gerektiren [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet paketi. Uygulama kullanıyorsa [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri), paket otomatik olarak eklenir.

Örnek'ın *appsettings.json* dosyasını tanımlayan bir `subsection` tuşları üyesiyle `suboption1` ve `suboption2`:

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions` Sınıf özelliklerini tanımlayan `SubOption1` ve `SubOption2`seçenekleri değerleri tutmak için (*Models/MySubOptions.cs*):

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

Sayfa modeli `OnGet` yöntemi seçenekleri değerlere sahip bir dize döndürür (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Uygulamayı çalıştırdığınızda `OnGet` yöntemi sınıfı değerleri alt seçeneğini gösteren bir dize döndürür:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Bir görünüm modeli veya doğrudan görünümü ekleme ile sağlanan seçenekleri

Seçenekler ile doğrudan görünümü ekleme veya bir görünüm modeli tarafından sağlanan örnek olarak gösterilen &num;4'te [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Bir görünüm modeli veya ekleme seçenekleri sağlanabilir `IOptions<TOptions>` doğrudan bir görünüm (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Doğrudan ekleme işlemi için ekleme `IOptions<MyOptions>` ile bir `@inject` yönergesi:

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Uygulama çalıştırıldığında, oluşturulan sayfada seçenekleri değerler gösterilir:

![Seçenek değerleri Seçenek1: value1_from_json ve Seçenek2: -1 modelinden ve ekleme görünümüne tarafından yüklenir.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Yapılandırma verileri IOptionsSnapshot ile yeniden yükleyin

Yapılandırma verileri ile yeniden yüklemeyi `IOptionsSnapshot` gösterildiği gibi &num;5'te [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) seçenekleri en az işlem ek yükü ile yeniden yüklemeyi destekler.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Seçenekler, erişilebilir ve istek ömrü boyunca önbelleğe istek başına bir kez hesaplanır.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`IOptionsSnapshot` anlık görüntüsüdür [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) ve güncelleştirmeleri otomatik olarak her izleyici tetikler. Bu veri kaynağını değiştirme üzerinde bağlı olarak değişir.

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

Aşağıdaki örnek, yeni bir nasıl gösterir `IOptionsSnapshot` sonra oluşturulan *appsettings.json* değişiklikleri (*Pages/Index.cshtml.cs*). Sunucuya birden çok istek tarafından sağlanan sabit değerler döndürür *appsettings.json* yapılandırma yeniden yükler ve dosya değiştirildiğinde kadar dosya.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Aşağıdaki resimde gösterilmektedir ilk `option1` ve `option2` yüklenen değerler *appsettings.json* dosyası:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Değerlerinde değişiklik *appsettings.json* dosyasını `value1_from_json UPDATED` ve `200`. Kaydet *appsettings.json* dosya. Seçenekleri değerleri güncelleştirildiğini görmek için tarayıcıyı yenileyin:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Adlı IConfigureNamedOptions seçenekleri desteği

Seçenekleri desteğiyle adlı [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) örnek olarak gösterilmiştir &num;6'da [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Seçenekleri adlı* adlandırılmış seçeneklerini yapılandırmaları arasında ayrım yapmak uygulama desteği sağlar. Örnek uygulamada, adlandırılmış seçenekleri ile bildirilen [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, dize, eylem&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure)genişletme yöntemini sırayla çağıran [ConfigureNamedOptions&lt;TOptions&gt;. Yapılandırma](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) yöntemi:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

Örnek uygulamayı adlandırılmış seçeneklerle erişen [IOptionsSnapshot&lt;TOptions&gt;. Alma](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Örnek uygulamayı çalıştırma, adlandırılmış seçenekleri döndürülür:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` hangi yüklenen değerleri yapılandırmadan okuyoruz, sağlanan *appsettings.json* dosya. `named_options_2` değerler tarafından sağlanır:

* `named_options_2` İçindeki temsilci `ConfigureServices` için `Option1`.
* İçin varsayılan değer `Option2` tarafından sağlanan `MyOptions` sınıfı.

## <a name="configure-all-options-with-the-configureall-method"></a>Tüm seçenekleri ConfigureAll yöntemi

Tüm seçenekleri örnekleriyle yapılandırma [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) yöntemi. Aşağıdaki kod yapılandırır `Option1` için tüm yapılandırma örnekleri ortak bir değere sahip. Aşağıdaki kodu el ile ekleyin `Configure` yöntemi:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Kod ekledikten sonra örnek uygulamayı çalıştırma aşağıdaki sonucu verir:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> Tüm seçenekleri örneği olarak adlandırılır. Varolan `IConfigureOption` örnekleri hedefleyen olarak kabul edilir `Options.DefaultName` olan örneği `string.Empty`. `IConfigureNamedOptions` Ayrıca uygulayan `IConfigureOptions`. Varsayılan uygulaması [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([başvuru kaynağı](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) her uygun şekilde kullanmak için mantığı vardır. `null` Adlandırılmış seçeneği tüm adlandırılmış örnek yerine adlandırılmış örneği belirli bir hedef için kullanılır ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) ve [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) bu kuralı kullanın).

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a>Doğrulama seçenekleri

Seçenekleri doğrulama seçenekleri yapılandırıldığında seçenekleri doğrulamanıza olanak sağlar. Çağrı `Validate` döndüren bir doğrulama yöntemiyle `true` seçenekleri geçerliyse ve `false` geçerli değilse:

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

Yukarıdaki örnekte adlandırılmış seçenekleri örneği ayarlar `optionalOptionsName`. Varsayılan seçenekleri örneği `Options.DefaultName`.

Doğrulama seçenekleri örneği oluşturulduğunda çalıştırır. Seçenekleri örneğinizin erişilebilir doğrulama ilk zaman geçmesi garanti edilir.

> [!IMPORTANT]
> Seçenekler başlangıçta yapılandırılmış ve doğrulanmış sonra seçeneklerini doğrulama seçenekleri değişikliklere karşı önlem değil.

`Validate` Yöntemi kabul bir `Func<TOptions, bool>`. Doğrulama tamamen özelleştirmek için uygulama `IValidateOptions<TOptions>`, sağlar:

* Seçenekleri birden çok doğrulama: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`
* Bu doğrulama başka bir seçenek türüne bağlıdır: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`

`IValidateOptions` doğrular:

* Belirli bir adlandırılmış seçenekleri örneği.
* Tüm seçenekleri zaman `name` olduğu `null`.

Döndürür bir `ValidateOptionsResult` arabiriminin, bir uygulamadan:

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

Gelecekteki sürümlerde sunulması istekli doğrulama (hızlı başlangıçta başarısız) ve veri ek açıklama tabanlı doğrulama zamanlanır.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

İle postconfiguration ayarlamak [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). Postconfiguration çalıştıran tüm [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) yapılandırma gerçekleşir:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) sonrası adlandırılmış seçeneklerini yapılandırmak kullanılabilir:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Kullanım [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) sonrası tüm yapılandırma örnekleri yapılandırmak için:

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a>Seçenekleri Fabrika, izleme ve önbellek

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) bildirimler için kullanılan zaman `TOptions` örnekleri değiştirin. `IOptionsMonitor` reloadable seçeneklerini destekler, bildirimler, değiştirmek ve `IPostConfigureOptions`.

::: moniker range=">= aspnetcore-2.0"

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) örnekleri yeni oluşturma seçenekleri için sorumludur. Tek bir sahip [Oluştur](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) yöntemi. Varsayılan uygulama, tüm kayıtlı alan `IConfigureOptions` ve `IPostConfigureOptions` ve tüm çalışmaların önce yapılandırır, ardından sonrası yapılandırır. Bunu ayırt `IConfigureNamedOptions` ve `IConfigureOptions` ve yalnızca uygun arabirimi çağırır.

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) tarafından kullanılan `IOptionsMonitor` önbelleğine `TOptions` örnekleri. `IOptionsMonitorCache` Değeri yeniden böylece seçenekleri durumlarda, izleyici geçersiz kılar ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Değerleri el ile yanı ile sunulan [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). [Temizle](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) yöntemi tüm adlandırılmış örnekler isteğe bağlı olarak yeniden oluşturulması sırasında kullanılır.

::: moniker-end

## <a name="accessing-options-during-startup"></a>Başlatma sırasında erişilebilirlik seçenekleri

`IOptions` kullanılabilir `Startup.Configure`, hizmetleri önce oluşturulan bu yana `Configure` yöntemini yürütür.

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

`IOptions` içinde kullanılmaması `Startup.ConfigureServices`. Hizmet Kayıtları sıralama nedeniyle tutarsız seçenekleri durumu bulunabilir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/index>
