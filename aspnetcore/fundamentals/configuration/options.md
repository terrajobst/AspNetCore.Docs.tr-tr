---
title: ASP.NET Core desende seçenekleri
author: guardrex
description: ASP.NET Core uygulamalarında ilgili ayar gruplarını temsil etmek için seçenekleri deseni kullanmayı keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 9566ed75375bdfaa9d6d8bf898b9fb2054356017
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56899326"
---
# <a name="options-pattern-in-aspnet-core"></a>ASP.NET Core desende seçenekleri

Tarafından [Luke Latham](https://github.com/guardrex)

::: moniker range="<= aspnetcore-1.1"

Bu konuda 1.1 sürümü için indirme [ASP.NET Core (sürüm 1.1, PDF) seçenekleri desende](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).

::: moniker-end

Seçenekleri deseni sınıfları, ilgili ayar gruplarını temsil etmek için kullanır. Zaman [yapılandırma ayarlarını](xref:fundamentals/configuration/index) yalıtılmış ayrı sınıf senaryoya göre uygulama için iki önemli yazılım Mühendisliği ilkeden uyar:

* [Arabirimi ayırma ilkesi (ISS) veya Kapsülleme](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; kullandıkları yapılandırma ayarlarını yapılandırma ayarlarına bağlı olan senaryoları (sınıflar) bağlıdır.
* [Görev ayrımı nettir](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; uygulamanın farklı kısımlarını ayarları bağımlı veya birbirine bağlı değil.

Seçenekler, ayrıca yapılandırma verilerini doğrulamak için bir mekanizma sağlar. Daha fazla bilgi için [seçeneklerini doğrulama](#options-validation) bölümü.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

Başvuru [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) veya paket başvurusu ekleme [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paket.

## <a name="options-interfaces"></a>Seçenekleri arabirimleri

<xref:Microsoft.Extensions.Options.IOptionsMonitor`1> seçenekleri almak ve yönetmek için seçenekleri bildirimleri için kullanılan `TOptions` örnekleri. <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> Aşağıdaki senaryoları destekler:

* Değişiklik bildirimleri verme
* [Adlandırılmış seçenekleri](#named-options-support-with-iconfigurenamedoptions)
* [Reloadable yapılandırma](#reload-configuration-data-with-ioptionssnapshot)
* Seçici seçenekleri geçersiz kılma (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)

[Yapılandırma sonrası](#options-post-configuration) senaryolar ayarlamak veya tüm seçenekleri değiştirmek izin <xref:Microsoft.Extensions.Options.IConfigureOptions`1> yapılandırma gerçekleşir.

<xref:Microsoft.Extensions.Options.IOptionsFactory`1> yeni seçenekler örnekleri oluşturmak için sorumludur. Tek bir sahip <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> yöntemi. Varsayılan uygulama, tüm kayıtlı alan <xref:Microsoft.Extensions.Options.IConfigureOptions`1> ve <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1> ve sonrası yapılandırma tarafından izlenen tüm yapılandırmaları ilk olarak çalıştırır. Bunu ayırt <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> ve <xref:Microsoft.Extensions.Options.IConfigureOptions`1> ve yalnızca uygun arabirimi çağırır.

<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> tarafından kullanılan <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> önbelleğine `TOptions` örnekleri. <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> Değeri yeniden böylece seçenekleri durumlarda, izleyici geçersiz kılar (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>). Değerleri el ile sunulan <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>. <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> Yöntemi tüm adlandırılmış örnekler isteğe bağlı olarak yeniden oluşturulması sırasında kullanılır.

<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> Burada seçenekleri her istekte yeniden senaryolarda yararlıdır. Daha fazla bilgi için [yeniden yapılandırma verileriyle IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) bölümü.

<xref:Microsoft.Extensions.Options.IOptions`1> destek seçenekleri için kullanılabilir. Ancak, <xref:Microsoft.Extensions.Options.IOptions`1> önceki senaryoları desteklemez <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>. Uygulamasını kullanmaya devam edebilir <xref:Microsoft.Extensions.Options.IOptions`1> mevcut altyapılarınız ve zaten kullandığınız kitaplıklar <xref:Microsoft.Extensions.Options.IOptions`1> arabirim ve tarafından sağlanan senaryoları gerektirmeyen <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.

## <a name="general-options-configuration"></a>Genel Seçenekler yapılandırma

Genel seçenekleri yapılandırma, örnek olarak gösterilmiştir &num;örnek uygulamada 1.

Bir seçenek sınıfı soyut olmayan olmalıdır genel parametresiz oluşturucusu ile. Aşağıdaki sınıf `MyOptions`, iki özelliğe sahiptir `Option1` ve `Option2`. Varsayılan değerleri ayarlama, isteğe bağlıdır, ancak aşağıdaki örnekte sınıf oluşturucu varsayılan değerini ayarlar `Option1`. `Option2` özelliği doğrudan başlatarak ayarlanmış varsayılan değerine sahip (*Models/MyOptions.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

`MyOptions` Sınıfı ile hizmet kapsayıcıya eklenir <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmasına bağlıdır:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

Sayfa modeli kullanan aşağıdaki [Oluşturucusu bağımlılık ekleme](xref:mvc/controllers/dependency-injection) ile <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> ayarlara erişmek için (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Örnek kullanıcının *appsettings.json* dosya için değerler belirten `option1` ve `option2`:

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

Ne zaman uygulamayı çalıştırın, sayfa modelin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> Özel bir kullanırken <xref:System.Configuration.ConfigurationBuilder> seçenekleri yapılandırma ayarları dosyadan yüklemek için temel yolu doğru şekilde ayarlandığından emin olun:
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
> Temel yol açık olarak ayarlama gerekli değildir seçenekleri yapılandırma ile ayarları dosyasından yüklenirken <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.

## <a name="configure-simple-options-with-a-delegate"></a>Bir temsilci ile basit seçeneklerini yapılandırın

Bir temsilci ile basit seçeneklerini yapılandırma örnek olarak gösterilmiştir &num;örnek uygulamada 2.

Bir temsilci seçenekleri değerleri ayarlamak için kullanın. Örnek uygulama kullandığı `MyOptionsWithDelegateConfig` sınıfı (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

Aşağıdaki kodda, ikinci bir <xref:Microsoft.Extensions.Options.IConfigureOptions`1> hizmet, hizmet kapsayıcıya eklenir. Bir temsilci ile yapılandırmak için kullandığı `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Birden çok yapılandırma sağlayıcısı ekleyebilirsiniz. Yapılandırma sağlayıcıları NuGet paketleri kullanılabilir ve kayıtlı sırayla uygulanır. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.

Her çağrı <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> ekler bir <xref:Microsoft.Extensions.Options.IConfigureOptions`1> hizmet kapsayıcıya hizmet. Yukarıdaki örnekte, değerlerini `Option1` ve `Option2` her ikisi de belirtilmiş *appsettings.json*, ancak değerlerini `Option1` ve `Option2` yapılandırılmış temsilci tarafından geçersiz kılınır.

Son yapılandırma kaynağı birden fazla Yapılandırma hizmeti etkinleştirildiğinde, belirtilen *WINS* ve yapılandırma değeri ayarlar. Ne zaman uygulamayı çalıştırın, sayfa modelin `OnGet` yöntemi seçenek sınıfı değerleri gösteren bir dize döndürür:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Suboptions yapılandırma

Suboptions yapılandırma, örnek olarak gösterilmiştir &num;örnek uygulamada 3.

Uygulamalar, uygulamadaki belirli bir senaryoyu grupları (sınıflar) ilgili seçenekleri sınıflar oluşturmanız gerekir. Yapılandırma değerleri gerektiren uygulama bölümleri, yalnızca kullandıkları yapılandırma değerleri için erişimi olmalıdır.

Seçenekleri yapılandırmayı bağlanırken, seçenek türünün her bir özellik form için bir yapılandırma anahtarı bağlı `property[:sub-property:]`. Örneğin, `MyOptions.Option1` özelliğe anahtarına `Option1`, den okunan `option1` özelliğinde *appsettings.json*.

Aşağıdaki kodda, üçüncü <xref:Microsoft.Extensions.Options.IConfigureOptions`1> hizmet, hizmet kapsayıcıya eklenir. Bunu bağlar `MySubOptions` bölümüne `subsection` , *appsettings.json* dosyası:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

`GetSection` Genişletme yöntemi gerektiren [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet paketi. Uygulama kullanıyorsa [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 veya üzeri), paket otomatik olarak eklenir.

Örnek'ın *appsettings.json* dosyasını tanımlayan bir `subsection` tuşları üyesiyle `suboption1` ve `suboption2`:

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

`MySubOptions` Sınıf özelliklerini tanımlayan `SubOption1` ve `SubOption2`seçenekleri değerleri tutmak için (*Models/MySubOptions.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

Sayfa modeli `OnGet` yöntemi seçenekleri değerlere sahip bir dize döndürür (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Uygulamayı çalıştırdığınızda `OnGet` yöntemi suboption sınıfı değerleri gösteren bir dize döndürür:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Bir görünüm modeli veya doğrudan görünümü ekleme ile sağlanan seçenekleri

Seçenekler ile doğrudan görünümü ekleme veya bir görünüm modeli tarafından sağlanan örnek olarak gösterilen &num;örnek uygulamada 4.

Bir görünüm modeli veya ekleme seçenekleri sağlanabilir <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> doğrudan bir görünüm (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Örnek uygulama, ekleme işlemi gösterilmektedir `IOptionsMonitor<MyOptions>` ile bir `@inject` yönergesi:

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

Uygulama çalıştırıldığında, oluşturulan sayfada seçenekleri değerler gösterilir:

![Seçenek değerleri Seçenek1: value1_from_json ve Seçenek2: -1 modelinden ve ekleme görünümüne tarafından yüklenir.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Yapılandırma verileri IOptionsSnapshot ile yeniden yükleyin

Yapılandırma verileri ile yeniden yüklemeyi <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> gösterildiği gibi &num;örnek uygulamada 5.

<xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> en az işlem ek yükü ile seçenekleri yeniden yüklemeyi destekler.

Seçenekler, erişilebilir ve istek ömrü boyunca önbelleğe istek başına bir kez hesaplanır.

Aşağıdaki örnek, yeni bir nasıl gösterir <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> sonra oluşturulan *appsettings.json* değişiklikleri (*Pages/Index.cshtml.cs*). Sunucuya birden çok istek tarafından sağlanan sabit değerler döndürür *appsettings.json* yapılandırma yeniden yükler ve dosya değiştirildiğinde kadar dosya.

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Aşağıdaki resimde gösterilmektedir ilk `option1` ve `option2` yüklenen değerler *appsettings.json* dosyası:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Değerlerinde değişiklik *appsettings.json* dosyasını `value1_from_json UPDATED` ve `200`. Kaydet *appsettings.json* dosya. Seçenekleri değerleri güncelleştirildiğini görmek için tarayıcıyı yenileyin:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Adlı IConfigureNamedOptions seçenekleri desteği

Seçenekleri desteğiyle adlı <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> örnek olarak gösterilmiştir &num;örnek uygulamada 6.

*Seçenekleri adlı* adlandırılmış seçeneklerini yapılandırmaları arasında ayrım yapmak uygulama desteği sağlar. Örnek uygulamada, adlandırılmış seçenekleri ile bildirilen [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), çağıran [ConfigureNamedOptions\<TOptions >. Yapılandırma](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) genişletme yöntemi:

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

Örnek uygulamayı adlandırılmış seçeneklerle erişen <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Örnek uygulamayı çalıştırma, adlandırılmış seçenekleri döndürülür:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1` hangi yüklenen değerleri yapılandırmadan okuyoruz, sağlanan *appsettings.json* dosya. `named_options_2` değerler tarafından sağlanır:

* `named_options_2` İçindeki temsilci `ConfigureServices` için `Option1`.
* İçin varsayılan değer `Option2` tarafından sağlanan `MyOptions` sınıfı.

## <a name="configure-all-options-with-the-configureall-method"></a>Tüm seçenekleri ConfigureAll yöntemi

Tüm seçenekleri örnekleriyle yapılandırma <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> yöntemi. Aşağıdaki kod yapılandırır `Option1` için tüm yapılandırma örnekleri ortak bir değere sahip. Aşağıdaki kodu el ile ekleyin `Startup.ConfigureServices` yöntemi:

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
> Tüm seçenekleri örneği olarak adlandırılır. Varolan <xref:Microsoft.Extensions.Options.IConfigureOptions`1> örnekleri hedefleyen olarak kabul edilir `Options.DefaultName` olan örneği `string.Empty`. <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> Ayrıca uygulayan <xref:Microsoft.Extensions.Options.IConfigureOptions`1>. Varsayılan uygulaması <xref:Microsoft.Extensions.Options.IOptionsFactory`1> her uygun şekilde kullanmak için mantığı vardır. `null` Adlandırılmış seçeneği tüm adlandırılmış örnek yerine adlandırılmış örneği belirli bir hedef için kullanılır (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> ve <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> bu kuralı kullanın).

## <a name="optionsbuilder-api"></a>OptionsBuilder API

<xref:Microsoft.Extensions.Options.OptionsBuilder`1> yapılandırmak için kullanılan `TOptions` örnekleri. `OptionsBuilder` yalnızca tek bir parametre için ilk olarak seçenekleri adlı oluşturma kolaylaştırır `AddOptions<TOptions>(string optionsName)` çağırmak yerine görünen tüm sonraki çağrılar. Doğrulama seçenekleri ve `ConfigureOptions` Hizmet bağımlılıkları kabul eden aşırı yüklemeler aracılığıyla yalnızca `OptionsBuilder`.

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a>Seçeneklerini yapılandırmak için DI hizmetlerini kullanma

İki yolla Seçenekleri'ni yapılandırırken bağımlılık ekleme diğer hizmetlere erişebilirsiniz:

* Bir yapılandırma temsilciye geçirmek [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) üzerinde [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1). [OptionsBuilder\<TOptions >](xref:Microsoft.Extensions.Options.OptionsBuilder`1) aşırı sağlar [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) seçeneklerini yapılandırmak için en fazla beş kullanmanıza izin ver:

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* Uygulayan kendi tür oluşturma <xref:Microsoft.Extensions.Options.IConfigureOptions`1> veya <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> ve türünü bir hizmet olarak kaydedin.

Bir yapılandırma temsilciye geçirme öneririz [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), hizmet oluşturma daha karmaşık olduğundan. Kendi tür oluşturma kullandığınızda framework sizin için ne yaptığını için eşdeğer [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*). Çağırma [yapılandırma](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) geçici genel kaydeder <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, genel hizmet türlerini kabul eden bir oluşturucuya belirtildiği. 

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
* Bu doğrulama başka bir seçenek türüne bağlıdır: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`

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

Veri ek açıklama tabanlı doğrulama kullanılabilir [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) çağırarak paket `ValidateDataAnnotations` metodunda `OptionsBuilder<TOptions>`. `Microsoft.Extensions.Options.DataAnnotations` yer aldığı [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 veya üzeri).

```csharp
private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}

[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

Gelecek sürümlerden meselesi istekli doğrulama (hızlı başlangıçta başarısız) altındadır.

::: moniker-end

## <a name="options-post-configuration"></a>Yapılandırma sonrası seçenekleri

Yapılandırma sonrası ile ayarlanmış <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>. Yapılandırma sonrası çalıştıran tüm <xref:Microsoft.Extensions.Options.IConfigureOptions`1> yapılandırma gerçekleşir:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> Adlandırılmış seçeneklerini sonrası yapılandırmak kullanılabilir:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Kullanım <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> sonrası tüm yapılandırma örnekleri yapılandırmak için:

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a>Başlatma sırasında erişilebilirlik seçenekleri

<xref:Microsoft.Extensions.Options.IOptions`1> ve <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> kullanılabilir `Startup.Configure`, hizmetleri önce oluşturulan bu yana `Configure` yöntemini yürütür.

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

Kullanmayın <xref:Microsoft.Extensions.Options.IOptions`1> veya <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> içinde `Startup.ConfigureServices`. Hizmet Kayıtları sıralama nedeniyle tutarsız seçenekleri durumu bulunabilir.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/index>
