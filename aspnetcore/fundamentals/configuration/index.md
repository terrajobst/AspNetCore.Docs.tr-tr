---
title: "ASP.NET çekirdek yapılandırması"
author: rick-anderson
description: "Yapılandırma API'si tarafından birden çok yöntem ASP.NET Core uygulamayı yapılandırmak için kullanın."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a>Bir ASP.NET Core uygulamayı yapılandırma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [işareti Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)

Yapılandırma API'si ad-değer çiftlerinin listesini temel web uygulamasını ASP.NET Core yapılandırmak için bir yol sağlar. Yapılandırma, birden fazla kaynaktan çalışma zamanında okuyun. Bu ad-değer çiftleri çok düzeyli bir hiyerarşi gruplandırabilirsiniz. 

İçin yapılandırma sağlayıcısı vardır:

* Dosya biçimleri (INI, JSON ve XML)
* Komut satırı bağımsız değişkenleri
* Ortam değişkenleri
* Bellek içi .NET nesneleri
* Bir şifrelenmiş kullanıcı deposu
* [Azure anahtar kasası](xref:security/key-vault-configuration)
* (Yüklü veya oluşturulan) özel sağlayıcılar

Her yapılandırma değeri bir dize anahtarı eşler. Özel bir ayarları seri durumdan çıkarılacak yerleşik bağlama Destek [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesne (basit bir .NET sınıf özelliklerine sahip).

Seçenekler düzeni seçenekleri sınıfları ilgili ayar gruplarını göstermek için kullanır. Seçenekler kullanılması daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>JSON yapılandırma

Aşağıdaki konsol uygulaması JSON yapılandırma sağlayıcısı kullanır:

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

Uygulama okur ve aşağıdaki yapılandırma ayarları görüntüler:

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

Hiyerarşik ad-değer çiftleri düğümleri bir virgülle ayrılmış listesini yapılandırması oluşur. Bir değer almak için erişim `Configuration` karşılık gelen öğenin anahtarı ile dizinleyici:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

JSON biçimli yapılandırma kaynaklarını dizilerde çalışmak için iki nokta üst üste ayrılmış dize bir parçası olarak bir dizi dizini kullanın. Aşağıdaki örnek önceki ilk öğe adını alır `wizards` dizi:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Ad-değer çiftleri için yerleşik yazılmış `Configuration` sağlayıcılarıdır **değil** kalıcı. Ancak, değerleri kaydeder özel bir sağlayıcı oluşturabilirsiniz. Bkz: [özel yapılandırma sağlayıcısının](xref:fundamentals/configuration/index#custom-config-providers).

Yukarıdaki örnek yapılandırma dizin oluşturucu değerleri okumak için kullanır. Erişim yapılandırması dışında `Startup`, kullanın *seçenekleri düzeni*. Daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.

Örneğin, geliştirme, test ve üretim farklı ortamlar için farklı yapılandırma ayarlarını sağlamak için genel bir durumdur. `CreateDefaultBuilder` Bir ASP.NET Core 2.x uygulamasında genişletme yöntemi (veya kullanarak `AddJsonFile` ve `AddEnvironmentVariables` bir ASP.NET Core 1.x uygulamasındaki doğrudan) JSON dosyaları ve sistem yapılandırma kaynaklarını okumak için bir yapılandırma sağlayıcısı ekler:

* *appSettings.JSON*
* *appSettings. \<EnvironmentName > .json*
* Ortam değişkenleri

Bkz: [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) için bir açıklama parametreleri. `reloadOnChange`yalnızca ASP.NET Core 1.1 ve sonraki sürümlerinde desteklenir. 

Yapılandırma kaynaklarını belirtilen sırada salt okunurdur. Yukarıdaki kod, ortam değişkenleri son salt okunurdur. Herhangi bir yapılandırma değeri ortamı Değiştir iki önceki sağlayıcıları belirlenen ayarlayın.

Ortam genellikle ayarlamak `Development`, `Staging`, veya `Production`. Bkz: [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) daha fazla bilgi için.

Yapılandırma dikkate alınacak noktalar:

* `IOptionsSnapshot`değişiklik yapıldığında yapılandırma verileri yeniden yükleyebilirsiniz. Bkz: [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) daha fazla bilgi için.
* Yapılandırma anahtarları büyük/küçük harfe duyarsızdır.
* Yerel ortamdaki dağıtılan yapılandırma dosyalarında ayarlarını geçersiz kılabilir için ortam değişkenleri son belirtin.
* **Hiçbir zaman** parolalar ve diğer hassas verileri yapılandırma sağlayıcısı kodu veya düz metin yapılandırma dosyalarını depolar. Verme geliştirmede üretim gizlilikleri kullanın veya sınama ortamlarında. Bunun yerine, böylece bunlar deponuza yanlışlıkla uygulanamıyor gizli proje dışında belirtin. Daha fazla bilgi edinmek [birden çok ortamlarıyla çalışma](xref:fundamentals/environments) ve yönetme [geliştirme sırasında uygulama sırrı güvenli depolama](xref:security/app-secrets).
* İki nokta varsa (`:`) olamaz, sistem ortam değişkenlerini kullanıldığında, iki nokta üst üste değiştirin (`:`) bir çift alt çizgi (`__`).

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Bellek içi sağlayıcısı ve bağlama için bir POCO sınıfı

Aşağıdaki örnek, bellek içi Sağlayıcısı'nı kullanın ve bir sınıfa bağlamak gösterilmektedir:

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

Yapılandırma değerleri dize olarak döndürülür, ancak bağlama nesnelerin yapımı sağlar. Bağlama POCO nesneleri veya hatta tüm nesne grafiklerinin almanıza olanak tanır.

### <a name="getvalue"></a>GetValue

Aşağıdaki örnek gösterilmektedir [GetValue&lt;T&gt; ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) genişletme yöntemi:

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

ConfigurationBinder's `GetValue<T>` yöntemi, varsayılan değer (80 örnekteki) belirtmenize olanak sağlar. `GetValue<T>`Basit senaryolar için ve tüm bölümleri bağlamaz. `GetValue<T>`skaler değerleri alır `GetSection(key).Value` belirli bir türüne dönüştürülemiyor.

## <a name="bind-to-an-object-graph"></a>Bir nesne grafiğinin bağlama

Bir sınıftaki her nesneyi yinelemeli olarak bağlama kullanabilirsiniz. Aşağıdakileri göz önünde bulundurun `AppSettings` sınıfı:

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

Aşağıdaki örnek bağlar `AppSettings` sınıfı:

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** ve daha yüksek kullanabilirsiniz `Get<T>`, tüm bölümleri ile çalışır. `Get<T>`kullanmaktan daha kullanışlı olabilir `Bind`. Aşağıdaki kodu nasıl kullanılacağını gösterir `Get<T>` yukarıdaki örnekle:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Aşağıdakileri kullanarak *appsettings.json* dosyası:

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

Program görüntüler `Height 11`.

Aşağıdaki kod birimine kullanılabilir test yapılandırması:

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a>Entity Framework özel sağlayıcı oluşturma

Bu bölümde, ad-değer çiftleri EF kullanan bir veritabanından okur bir temel yapılandırma sağlayıcısı oluşturulur. 

Tanımlayan bir `ConfigurationValue` yapılandırma değerlerini veritabanında depolamak için varlık:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Ekleme bir `ConfigurationContext` depolamak ve yapılandırılmış değerlerine erişmek için:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Arabirimini uygulayan bir sınıf oluşturun [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Özel yapılandırma sağlayıcısının devralarak oluşturma [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  Boş olduğunda yapılandırma sağlayıcısı veritabanı başlatır:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Örneği çalıştırdığınızda ("value_from_ef_1" ve "value_from_ef_2") veritabanından vurgulanan değerler görüntülenir.

Ekleyebileceğiniz bir `EFConfigSource` genişletme yöntemi yapılandırma kaynağı eklemek için:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Aşağıdaki kod özel kullanmayı gösterir `EFConfigProvider`:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Örnek ekler özel Not `EFConfigProvider` JSON sağlayıcısı sonra bu nedenle veritabanından herhangi bir ayarı geçersiz kılar ayarlarından *appsettings.json* dosya.

Aşağıdakileri kullanarak *appsettings.json* dosyası:

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

Aşağıdaki görüntülenir:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Komut satırı yapılandırma sağlayıcısı

[CommandLine yapılandırma sağlayıcısı](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) çalışma zamanında yapılandırması için komut satırı bağımsız değişkeni anahtar-değer çiftleri alır.

[Görüntülemek veya komut satırı yapılandırma örneği indirin](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Kurulum ve komut satırı yapılandırma sağlayıcısı kullanın

# <a name="basic-configurationtabbasicconfiguration"></a>[Temel yapılandırma](#tab/basicconfiguration)

Komut satırı yapılandırmasını etkinleştirmek için arama `AddCommandLine` genişletme yöntemi örneği üzerinde [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Kod çalıştırmadan, aşağıdaki çıkış görüntülenir:

```console
MachineName: MairaPC
Left: 1980
```

Komut satırında anahtar-değer çiftleri bağımsız değişken geçirme değerlerini değiştirir `Profile:MachineName` ve `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

Konsol penceresinde görüntüler:

```console
MachineName: BartPC
Left: 1979
```

Komut satırı yapılandırması ile diğer yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için arama `AddCommandLine` üzerinde son `ConfigurationBuilder`:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

Tipik ASP.NET Core 2.x uygulamaları kullanma statik kolaylık metodunun `CreateDefaultBuilder` konak oluşturmak için:

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder`İsteğe bağlı yapılandırmasından yükler *appsettings.json*, *appsettings. { Ortam} .json*, [kullanıcı parolaları](xref:security/app-secrets) (içinde `Development` ortamı), ortam değişkenleri ve komut satırı bağımsız değişkenleri. Komut satırı yapılandırma sağlayıcısı son çağrılır. Sağlayıcı son çağırma yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için çalışma zamanında geçirilen komut satırı bağımsız değişkenleri önceki adlı sağlar.

İçin unutmayın *appsettings* dosyaları `reloadOnChange` etkinleştirilir. Eşleşen bir yapılandırma değeri, komut satırı bağımsız değişkenleri geçersiz kılınmış bir *appsettings* dosya uygulama başladıktan sonra değiştirilir.

> [!NOTE]
> Kullanmaya alternatif olarak `CreateDefaultBuilder` kullanarak bir ana bilgisayar oluşturma yöntemi, [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ve el ile yapılandırma oluşturma [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ASP.NET Core desteklenen 2.x. Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Oluşturma bir [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ve arama `AddCommandLine` yöntemi CommandLine yapılandırma sağlayıcısı kullanın. Sağlayıcı son çağırma yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için çalışma zamanında geçirilen komut satırı bağımsız değişkenleri önceki adlı sağlar. Yapılandırmasını uygulamak [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ile `UseConfiguration` yöntemi:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Arguments

Komut satırına geçirilen bağımsız değişkenler aşağıdaki tabloda gösterilen iki biçim birine uymalıdır.

| Bağımsız değişken biçimi                                                     | Örnek        |
| ------------------------------------------------------------------- | :------------: |
| Tek bağımsız değişken: bir anahtar-değer çifti ayrılmış bir eşittir işareti (`=`) | `key1=value`   |
| İki bağımsız değişken bir dizi: boşlukla ayrılmış bir anahtar-değer çifti    | `/key1 value1` |

**Tek bir bağımsız değişken**

Değer bir eşittir işareti gelmelidir (`=`). Değer null olabilir (örneğin, `mykey=`).

Anahtar bir önek olabilir.

| Anahtar öneki               | Örnek         |
| ------------------------ | :-------------: |
| Önek                | `key1=value1`   |
| Tek bir tire (`-`) &#8224; | `-key2=value2`  |
| İki kısa çizgi (`--`)        | `--key3=value3` |
| Eğik çizgi (`/`)      | `/key4=value4`  |

&#8224; Tek tire öneke sahip bir anahtar (`-`) içinde sağlanmalıdır [geçiş eşlemeleri](#switch-mappings), aşağıda açıklanmıştır.

Örnek komut:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı için verilen bir `FormatException` oluşturulur.

**İki bağımsız değişken dizisi**

Değer null olamaz ve boşlukla ayrılmış anahtar izlemeniz gerekir.

Anahtar bir önekine sahip olmalıdır.

| Anahtar öneki               | Örnek         |
| ------------------------ | :-------------: |
| Tek bir tire (`-`) &#8224; | `-key1 value1`  |
| İki kısa çizgi (`--`)        | `--key2 value2` |
| Eğik çizgi (`/`)      | `/key3 value3`  |

&#8224; Tek tire öneke sahip bir anahtar (`-`) içinde sağlanmalıdır [geçiş eşlemeleri](#switch-mappings), aşağıda açıklanmıştır.

Örnek komut:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı için verilen bir `FormatException` oluşturulur.

### <a name="duplicate-keys"></a>Yinelenen anahtarlar

Yinelenen anahtarlarla verdiyse, son anahtar-değer çifti kullanılır.

### <a name="switch-mappings"></a>Geçiş eşlemeleri

El ile yapılandırma ile oluştururken `ConfigurationBuilder`, isteğe bağlı olarak bir anahtar eşlemeleri sözlüğe sağlayabilir `AddCommandLine` yöntemi. Anahtar eşlemeleri anahtar adı değiştirme mantığın olanak sağlar.

Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük komut satırı bağımsız değişkeni tarafından sağlanan anahtarıyla eşleşen bir anahtarı için denetlenir. Komut satırı anahtarı sözlük içinde bulunursa, sözlük değeri (Anahtar değişimini) yapılandırma döndürülmek geçirilir. Tek bir çizgiyle önekli herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).

Eşlemeleri sözlüğü anahtar kuralları anahtarı:

* Anahtarları kısa çizgiyle başlamalıdır (`-`) veya çift tire (`--`).
* Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.

Aşağıdaki örnekte, `GetSwitchMappings` yöntemi sağlayan tek bir çizgi kullanmak, komut satırı bağımsız değişkenleri (`-`) anahtarı öneki ve önde gelen alt anahtar önekleri kaçının.

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Komut satırı bağımsız değişkenleri sağlamadan sözlüğü için sağlanan `AddInMemoryCollection` yapılandırma değerlerini ayarlar. Uygulama ile aşağıdaki komutu çalıştırın:

```console
dotnet run
```

Konsol penceresinde görüntüler:

```console
MachineName: RickPC
Left: 1980
```

Yapılandırma ayarlarını geçirmek için aşağıdakileri kullanın:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

Konsol penceresinde görüntüler:

```console
MachineName: DahliaPC
Left: 1984
```

Anahtar eşlemeleri sözlüğü oluşturulduktan sonra aşağıdaki tabloda gösterilen veriler içerir.

| Anahtar            | Değer                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Anahtar geçişi sözlüğünü kullanarak göstermek için aşağıdaki komutu çalıştırın:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Komut satırı anahtarları değiştirilen. Konsol penceresinde yapılandırma değerlerini görüntüler `Profile:MachineName` ve `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>Web.config dosyası

A *web.config* dosya, IIS veya IIS Express uygulamasında barındırdığınızda gereklidir. *Web.config* uygulamanızı başlatma IIS'de AspNetCoreModule açar. Ayarlarında *web.config* uygulamanızı başlatın ve diğer IIS ayarlarını ve modülleri yapılandırmak için IIS'de AspNetCoreModule etkinleştirin. Visual Studio kullanarak ve silerseniz *web.config*, Visual Studio yeni bir tane oluşturur.

## <a name="additional-notes"></a>Ek Notlar

* Bağımlılık ekleme (dı) ayarlı değil kadar yukarı sonra `ConfigureServices` çağrılır.
* Yapılandırma sistemi dı uyumlu değil.
* `IConfiguration`iki özelleştirmeleri sahiptir:
  * `IConfigurationRoot`Kök düğüm için kullanılır. Bir yeniden yükleme tetikleyebilir.
  * `IConfigurationSection`Yapılandırma değerlerini bir bölümünü temsil eder. `GetSection` Ve `GetChildren` yöntemleri döndürür bir `IConfigurationSection`.

## <a name="additional-resources"></a>Ek kaynaklar

* [Seçenekler](xref:fundamentals/configuration/options)
* [Birden çok ortamı ile çalışma](xref:fundamentals/environments)
* [Geliştirme sırasında uygulama sırrı güvenli depolama](xref:security/app-secrets)
* [ASP.NET çekirdek barındırma](xref:fundamentals/hosting)
* [Bağımlılık ekleme](xref:fundamentals/dependency-injection)
* [Azure anahtar kasası yapılandırma sağlayıcısı](xref:security/key-vault-configuration)
