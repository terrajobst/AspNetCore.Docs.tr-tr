---
title: ASP.NET Core desende seçenekleri
author: guardrex
description: ASP.NET Core uygulamalarda ilgili ayar gruplarını göstermek için seçenekleri düzeni kullanmayı keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
uid: fundamentals/configuration/options
ms.openlocfilehash: 1fe05fbc5035ffa2d01bc6be55436146f1434d17
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278550"
---
# <a name="options-pattern-in-aspnet-core"></a>ASP.NET Core desende seçenekleri

Tarafından [Luke Latham](https://github.com/guardrex)

Seçenekler düzeni sınıfları ilgili ayar gruplarını göstermek için kullanır. Yapılandırma ayarları ayrı sınıflara özelliğiyle yalıtılır, uygulama için iki önemli yazılım mühendislik ilkeden aynılarını:

* [Arabirimi arasında ayrım yapma ilkesine (ISS)](http://deviq.com/interface-segregation-principle/): yapılandırma ayarlarına bağlıdır (sınıflar) özelliklerine bağlıdır, kullandıkları yapılandırma ayarları.
* [Sorunları ayrılması](http://deviq.com/separation-of-concerns/): uygulamanın farklı bölümleri için ayarları bağımlı veya birbiriyle eşleşmiş değil.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) Bu makalede örnek uygulama ile izleyin daha kolaydır.

## <a name="basic-options-configuration"></a>Temel seçeneklerini yapılandırma

Temel Seçenekler yapılandırma, örnek olarak gösterilmiştir &num;1'de [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Özet olmayan bir seçenek sınıfı olmalıdır genel bir parametresiz oluşturucuya sahip. Aşağıdaki sınıf `MyOptions`, iki özelliğe sahip `Option1` ve `Option2`. Varsayılan değerleri ayarlama isteğe bağlı olmakla birlikte, aşağıdaki örnekte sınıfı oluşturucusu varsayılan değerini ayarlar `Option1`. `Option2` özellik doğrudan başlatarak ayarlamak varsayılan değeri (*Models/MyOptions.cs*):

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` Sınıf ile hizmet kapsayıcısı eklenen [yapılandırma&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) ve yapılandırmasına bağlıdır:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

Aşağıdaki modelinin kullandığı sayfa [Oluşturucusu bağımlılık ekleme](xref:fundamentals/dependency-injection#what-is-dependency-injection) ile [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) ayarlarına erişmek için (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Örnek 's *appsettings.json* dosyayı belirtir değerlerini `option1` ve `option2`:

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Bir temsilci ile basit seçeneklerini yapılandırma

Bir temsilci ile basit seçeneklerini yapılandırma örnek olarak gösterilmiştir &num;2'de [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Bir temsilci seçenekleri değerleri ayarlamak için kullanın. Örnek uygulama kullandığı `MyOptionsWithDelegateConfig` sınıfı (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

Aşağıdaki kodda, ikinci bir `IConfigureOptions<TOptions>` hizmeti için hizmet kapsayıcısı eklenir. Bağlama ile yapılandırmak için bir temsilci kullanır `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz. Yapılandırma sağlayıcıları NuGet paketleri kullanılabilir. Kayıtlı edebilmesi uygulanmasıyla.

Her çağrı [yapılandırma&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) ekler bir `IConfigureOptions<TOptions>` service hizmet kapsayıcısı. Önceki örnekte, değerlerini `Option1` ve `Option2` her ikisi de belirtilen *appsettings.json*, ancak değerlerini `Option1` ve `Option2` yapılandırılmış temsilci tarafından geçersiz kılınır.

Son yapılandırma kaynağı birden fazla Yapılandırma hizmeti etkinleştirildiğinde, belirtilen *WINS* ve yapılandırma değeri ayarlar. Uygulama çalıştırıldığında, sayfa modelinin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Suboptions yapılandırma

Suboptions yapılandırma örnek olarak gösterilmiştir &num;3'te [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Uygulamalar, uygulama (sınıflar) belirli özellik gruplarına ait seçenekleri sınıfları oluşturmanız gerekir. Yapılandırma değerlerini gerektiren uygulama bölümleri yalnızca kullandıkları yapılandırma değerlerini erişiminiz olması.

İçin yapılandırma seçenekleri bağlama sırasında her bir özellik seçenekleri türü form için bir yapılandırma anahtarı bağlı `property[:sub-property:]`. Örneğin, `MyOptions.Option1` özellik anahtarına bağlı `Option1`, den okunan `option1` özelliğinde *appsettings.json*.

Aşağıdaki kodda, üçüncü `IConfigureOptions<TOptions>` hizmeti için hizmet kapsayıcısı eklenir. Bunu bağlar `MySubOptions` bölümüne `subsection` , *appsettings.json* dosyası:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

`GetSection` Genişletme yöntemi gerektiren [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet paketi. Uygulama kullanıyorsa [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya sonrası), paketi otomatik olarak eklenir.

Örnek 's *appsettings.json* dosya tanımlayan bir `subsection` tuşları üyesiyle `suboption1` ve `suboption2`:

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

`MySubOptions` Sınıfı tanımlayan özellikleri, `SubOption1` ve `SubOption2`, alt seçenek değerleri tutmak için (*Models/MySubOptions.cs*):

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

Sayfa modelinin `OnGet` yöntemi alt seçeneği değerleri içeren bir dize döndürür (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Uygulama çalıştırıldığında `OnGet` yöntemi alt seçeneği sınıfı değerlerini gösteren bir dize döndürür:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Bir görünüm modeli veya doğrudan görünümü ekleme ile sağlanan seçenekleri

Bir görünüm modeli veya doğrudan görünümü ekleme ile sağlanan seçenekleri, örnek olarak gösterilmiştir &num;4'te [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Seçenekler sağlanan bir görünüm modeli veya injecting `IOptions<TOptions>` bir görünüm doğrudan (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Doğrudan ekleme işlemi için ekleme `IOptions<MyOptions>` ile bir `@inject` yönergesi:

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Uygulama çalıştırıldığında seçenek değerlerinin oluşturulan sayfada gösterilir:

![Seçenek değerleri seçenek 1: value1_from_json ve Seçenek2: -1 modelinden ve görünümüne yerleştirme tarafından yüklenir.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Yapılandırma verileri IOptionsSnapshot ile yeniden yükleyin

Yapılandırma verileri ile yeniden `IOptionsSnapshot` gösterildiği gibi &num;5'te [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*ASP.NET Core 1.1 veya üstünü gerektirir.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) en az işlem ek yüküne ile seçenekleri görüntülemeyi destekler. ASP.NET Core 1. 1'da, `IOptionsSnapshot` anlık görüntüsüdür [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) ve güncelleştirmeleri otomatik olarak her izleyici tetikleyen verileri değiştirme kaynağı bağlı olarak değişir. ASP.NET Core 2.0 ve daha sonra seçenekleri erişilen ve istek ömrü boyunca önbelleğe alınan istek başına bir kez hesaplanır.

Aşağıdaki örnek yeni bir nasıl gösterir `IOptionsSnapshot` sonra oluşturulan *appsettings.json* değişiklikleri (*Pages/Index.cshtml.cs*). Sunucuya birden çok istek dönüş tarafından sağlanan sabit değerleri *appsettings.json* dosya değiştirildi ve yapılandırmayı yeniden yükler kadar dosya.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Aşağıdaki resimde ilk gösterilmiştir `option1` ve `option2` değerleri yüklenen *appsettings.json* dosyası:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Değerlerinde değişiklik *appsettings.json* dosya `value1_from_json UPDATED` ve `200`. Kaydet *appsettings.json* dosya. Seçenekler değerler güncelleştirilir görmek için tarayıcıyı yenileyin:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>IConfigureNamedOptions seçenekleri desteğiyle adlı

Seçenekler desteğiyle adlı [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) örnek olarak gösterilmiştir &num;6 ' [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*ASP.NET Core 2.0 veya sonraki sürümünü gerektirir.*

*Seçenekler adlı* adlandırılmış seçeneklerini yapılandırmaları arasında ayrım yapmak uygulama desteği sağlar. Örnek uygulaması adlandırılmış seçenekleri ile bildirilen [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, dize, eylem&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure)genişletme yöntemi sırayla çağıran [ConfigureNamedOptions&lt;TOptions&gt;. Yapılandırma](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) yöntemi:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

Örnek uygulaması adlandırılmış seçenekleriyle eriştiği [IOptionsSnapshot&lt;TOptions&gt;. Alma](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Örnek uygulama çalışırken, adlandırılmış seçenekleri döndürülür:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` değerleri, gelen yüklenen yapılandırmasından sağlanan *appsettings.json* dosya. `named_options_2` değerleri tarafından sağlanır:

* `named_options_2` İçinde temsilci `ConfigureServices` için `Option1`.
* İçin varsayılan değer `Option2` tarafından sağlanan `MyOptions` sınıfı.

Tüm adlandırılmış seçenekleri örnekleriyle yapılandırma [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) yöntemi. Aşağıdaki kod yapılandırır `Option1` tüm yapılandırma örnekleri ortak bir değerle adlı. Aşağıdaki kodu el ile ekleyebilirsiniz `Configure` yöntemi:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Kod ekledikten sonra örnek uygulaması çalıştıran aşağıdaki sonucu üretir:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> ASP.NET Core 2.0 ve daha sonra tüm seçenekleri örnekleri adlandırılır. Varolan `IConfigureOption` örnekleri hedefleme olarak kabul edilir `Options.DefaultName` olan örneği `string.Empty`. `IConfigureNamedOptions` Ayrıca `IConfigureOptions`. Varsayılan uygulaması [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([başvuru kaynağı](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) her uygun şekilde kullanmak için mantığı vardır. `null` Adlandırılmış seçeneği tüm adlandırılmış örnek yerine adlandırılmış örneği belirli bir hedef için kullanılır ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) ve [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) bu yöntemi kullanın).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*ASP.NET Core 2.0 veya sonraki sürümünü gerektirir.*

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

Kullanım [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) yapılandırma örnekleri adlı sonrası yapılandırmak için:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Seçenekler Fabrika, izleme ve önbellek

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) bildirimler için kullanılan zaman `TOptions` örnekleri değiştirin. `IOptionsMonitor` reloadable seçeneklerini destekler, bildirimler, değiştirmek ve `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) yeni oluşturma örnekleri Seçenekler (ASP.NET Core 2.0 veya üstü) sorumlu. Tek bir sahip [oluşturma](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) yöntemi. Varsayılan uygulamasını tüm kayıtlı alır `IConfigureOptions` ve `IPostConfigureOptions` ve tüm çalıştırır önce yapılandırır, arkasından sonrası yapılandırır. Arasında ayırt `IConfigureNamedOptions` ve `IConfigureOptions` ve yalnızca uygun arabirimi çağırır.

[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 veya üstü) tarafından kullanılan `IOptionsMonitor` önbelleğe `TOptions` örnekleri. `IOptionsMonitorCache` Değeri yeniden böylece seçenekleri örnekleri İzleyicisi'nde geçersiz kılar ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Değerleri el ile de ile sunulan [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). [Temizle](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) yöntemi, tüm adlandırılmış örnekleri isteğe bağlı olarak yeniden oluşturulması olduğunda kullanılır.

## <a name="accessing-options-during-startup"></a>Başlatma sırasında erişilebilirlik seçenekleri

`IOptions` kullanılabilir `Configure`, önce Hizmetleri yerleşiktir bu yana `Configure` yöntemini yürütür. Bir hizmet sağlayıcısı oluşturulursa `ConfigureServices` seçeneklerine erişmek için onu içeremez olmayacaktır seçenekleri hizmet sağlayıcısı oluşturulduktan sonra sağlanan yapılandırmaları. Bu nedenle, bir tutarsız seçenekleri durum hizmet kayıtları sıralama nedeniyle olabilir.

Seçenekleri genellikle yapılandırmasından yüklendiğinden bu yana yapılandırma hem de başlangıç kullanılabilir `Configure` ve `ConfigureServices`. Başlatma sırasında yapılandırmayla örnekler için bkz: [uygulama başlangıç](xref:fundamentals/startup) konu.

## <a name="see-also"></a>Ayrıca bkz.

* [Yapılandırma](xref:fundamentals/configuration/index)
