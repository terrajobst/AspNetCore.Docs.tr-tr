---
title: ASP.NET core'da yapılandırma
author: rick-anderson
description: ASP.NET Core uygulaması yapılandırmak için yapılandırma API'sini kullanmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228630"
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET core'da yapılandırma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [işareti Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), ve [Luke Latham](https://github.com/guardrex)

Yapılandırma API ad-değer çiftlerinin listesini temel web uygulaması bir ASP.NET Core yapılandırmak için bir yol sağlar. Yapılandırma, çalışma zamanında birden çok kaynaktan okuyun. Ad-değer çiftleri çok düzeyli bir hiyerarşi halinde gruplandırılabilir.

İçin yapılandırma sağlayıcısı vardır:

* Dosya biçimleri (INI, JSON ve XML).
* Komut satırı bağımsız değişkenleri.
* Ortam değişkenleri.
* Bellek içi .NET nesneleri.
* Şifrelenmemiş [gizli dizi Yöneticisi](xref:security/app-secrets) depolama.
* Gibi bir şifrelenmiş kullanıcı depolamak [Azure anahtar kasası](xref:security/key-vault-configuration).
* Özel sağlayıcılar (veya oluşturulan yüklü).

Her yapılandırma değeri bir dize anahtarına eşler. Ayarlar uygulamasına özel bir'seri durumdan çıkarmak için yerleşik bağlama desteği yoktur [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesne (basit bir .NET sınıf özelliklere sahip).

Seçenekleri deseni seçenekleri sınıfları, ilgili ayar gruplarını temsil etmek için kullanır. Seçenekleri desenini kullanarak, daha fazla bilgi için bkz: [seçenekleri](xref:fundamentals/configuration/options) konu.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

Bu konuda sağlanan örnekleri üzerine kullanır:

* Temel yol ile uygulama ayarı [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) paket.
* Yapılandırma dosyalarıyla bölümlerini çözümleme [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) paket.
* Bağlama yapılandırması ile [bağlama](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) paket.

Bu paketleri dahil [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Bu konuda sağlanan örnekleri üzerine kullanır:

* Temel yol ile uygulama ayarı [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) paket.
* Yapılandırma dosyalarıyla bölümlerini çözümleme [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) paket.
* Bağlama yapılandırması ile [bağlama](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) paket.

Bu paketleri dahil [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Bu konuda sağlanan örnekleri üzerine kullanır:

* Temel yol ile uygulama ayarı [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath). `SetBasePath` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) paket.
* Yapılandırma dosyalarıyla bölümlerini çözümleme [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection). `GetSection` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) paket.
* Bağlama yapılandırması ile [bağlama](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind). `Bind` başvurarak uygulamaya sunulacağını [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) paket.

::: moniker-end

## <a name="json-configuration"></a>JSON yapılandırma

Aşağıdaki konsol uygulaması, JSON yapılandırma sağlayıcısını kullanır:

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

Uygulama okur ve aşağıdaki yapılandırma ayarları görüntüler:

[!code-json[](index/sample/ConfigJson/appsettings.json)]

Yapılandırma, hiyerarşik düğümleri virgül ile ayrılır ad-değer çiftlerinin listesini oluşur (`:`). Bir değer almak için erişim `Configuration` Dizin Oluşturucu ile ilgili öğenin anahtarı:

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

JSON biçimli yapılandırma kaynaklarını dizilerde çalışmak için iki nokta ile ayrılmış dizesinin parçası olarak bir dizi dizini kullanın. Aşağıdaki örnek, önceki içindeki ilk öğeyi adını alır. `wizards` dizisi:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

Ad-değer çiftleri yerleşik olarak yazılmış [yapılandırma](/dotnet/api/microsoft.extensions.configuration) sağlayıcılar **değil** kalıcı. Ancak, değerler kaydeder özel bir sağlayıcı oluşturulabilir. Bkz: [özel yapılandırma sağlayıcısını](xref:fundamentals/configuration/index#custom-config-providers).

Yukarıdaki örnek yapılandırma dizin oluşturucu değerleri okumak için kullanır. Erişimi yapılandırma dışında `Startup`, kullanın *seçenekleri deseni*. Daha fazla bilgi için [seçenekleri](xref:fundamentals/configuration/options) konu.

## <a name="xml-configuration"></a>XML yapılandırması

XML biçimli yapılandırma kaynaklarını dizilerde çalışmak için sağlayan bir `name` her öğenin dizini. Değerlere erişmek için dizini kullanın:

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a>Ortama göre yapılandırma

Örneğin, geliştirme, test ve üretim gibi farklı ortamlar için farklı yapılandırma ayarları için tipik bir durumdur. `CreateDefaultBuilder` Bir ASP.NET Core 2.x uygulamasında genişletme yöntemi (veya bu adı kullanıyor `AddJsonFile` ve `AddEnvironmentVariables` bir ASP.NET Core 1.x uygulamada doğrudan) JSON dosyalarını ve sistem yapılandırma kaynaklarını okumak için bir yapılandırma sağlayıcısı ekler:

* *appsettings.json*
* *appSettings. \<EnvironmentName > .json*
* Ortam değişkenleri

Çağırmak için gereken ASP.NET Core 1.x uygulamaları `AddJsonFile` ve [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).

Bkz: [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) parametre açıklaması. `reloadOnChange` yalnızca ASP.NET Core 1.1 ve sonraki sürümlerde desteklenir.

Yapılandırma kaynaklarını belirtilen sırada okunur. Önceki kodda, ortam değişkenleri son okunur. Tüm yapılandırma değerleri ile ortam değiştirme iki önceki sağlayıcıları ayarlananlara ayarlayın.

Aşağıdakileri göz önünde bulundurun *appsettings. Staging.JSON* dosyası:

[!code-json[](index/sample/appsettings.Staging.json)]

Aşağıdaki kodda, `Configure` değerini okur `MyConfig`:

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

Ortamı genellikle kümesine `Development`, `Staging`, veya `Production`. Daha fazla bilgi için [birden fazla ortam kullanayım](xref:fundamentals/environments).

Yapılandırmada dikkat edilmesi gerekenler

* [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) yapılandırma verileri değiştiğinde yeniden yükleyebilirsiniz.
* Yapılandırma anahtarları **değil** büyük küçük harfe duyarlı.
* **Hiçbir zaman** yapılandırma sağlayıcısı kod veya yapılandırma dosyalarını düz metin parolalar ve diğer hassas verileri depolayın. Geliştirmede üretim gizli anahtarları kullanma veya test ortamları kullanmayın. Böylece bunlar için kaynak kodu deposu yanlışlıkla yürütülemiyor gizli proje dışında belirtin. Daha fazla bilgi edinin [birden çok ortam kullanma](xref:fundamentals/environments) ve yönetme [geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets).
* Hiyerarşik yapılandırma değerleri belirtilen ortam değişkenleri, bir iki nokta üst üste (`:`) tüm platformlarda çalışmayabilir. Çift alt çizgi (`__`) tüm platformları tarafından desteklenir.
* Yapılandırma API'si, bir iki nokta üst üste ile etkileşim kurulurken (`:`) tüm platformlarda çalışır.

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Bellek içi sağlayıcısı ve bağlama için bir POCO sınıfı

Aşağıdaki örnek, bellek içi sağlayıcısı kullanın ve bir sınıfa Bağla gösterilmektedir:

[!code-csharp[](index/sample/InMemory/Program.cs)]

Yapılandırma değerleri dize olarak döndürülür, ancak nesnelerin yapımı bağlamayı etkinleştirir. Bağlamanın POCO nesneleri veya hatta tüm nesne grafiklerini alınmasını sağlar.

### <a name="getvalue"></a>GetValue

Aşağıdaki örnekte [GetValue&lt;T&gt; ](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) genişletme yöntemi:

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

ConfigurationBinder'ın `GetValue<T>` yöntemi (örnekte 80) varsayılan değer belirtimi sağlar. `GetValue<T>` Basit senaryolar için ve tüm bölümleri için bağlamayan. `GetValue<T>` skaler değerden alır `GetSection(key).Value` belirli bir türüne dönüştürülür.

## <a name="bind-to-an-object-graph"></a>Bir nesne grafiği için bağlama

Her nesne bir sınıftaki yinelemeli olarak bağlı olabilir. Aşağıdakileri göz önünde bulundurun `AppSettings` sınıfı:

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

Aşağıdaki örnek bağlar `AppSettings` sınıfı:

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** ve daha yüksek kullanabilirsiniz `Get<T>`, tüm bölümleri ile çalışır. `Get<T>` kullanmaktan daha kullanışlı olabilir `Bind`. Aşağıdaki kod nasıl kullanılacağını gösterir `Get<T>` önceki örnekle:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Aşağıdakileri kullanarak *appsettings.json* dosyası:

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

Program görüntüler `Height 11`.

Aşağıdaki kod, birim için kullanılabilir test yapılandırması:

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

## <a name="create-an-entity-framework-custom-provider"></a>Entity Framework ile özel bir sağlayıcı oluşturma

Bu bölümde, ad-değer çiftlerini okur EF kullanarak bir veritabanından bir temel yapılandırma sağlayıcısı oluşturulur.

Tanımlayan bir `ConfigurationValue` veritabanında yapılandırma değerlerini depolamak için varlık:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Ekleme bir `ConfigurationContext` depolamak ve yapılandırılan değerlere erişmek için:

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Uygulayan bir sınıf oluşturma [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Özel yapılandırma sağlayıcısını devralarak oluşturma [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider). Boş olduğunda veritabanı yapılandırma sağlayıcısını başlatır:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Örneği çalıştırdığınızda bir veritabanından ("value_from_ef_1" ve "value_from_ef_2") vurgulanan değerler görüntülenir.

Bir `EFConfigSource` yapılandırma kaynağı eklemek için uzantı yöntemi kullanılabilir:

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Aşağıdaki kod özel kullanma işlemini gösterir `EFConfigProvider`:

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Örnek özel ekler Not `EFConfigProvider` JSON sağlayıcısı sonra bu nedenle veritabanından herhangi bir ayarı geçersiz kılar ayarlarından *appsettings.json* dosya.

Aşağıdakileri kullanarak *appsettings.json* dosyası:

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

Aşağıdaki çıktı görüntülenir:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Komut satırı yapılandırma sağlayıcısı

[CommandLine yapılandırma sağlayıcısı](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) komut satırı bağımsız değişkeni anahtar-değer çiftleri için çalışma zamanı yapılandırmasını alır.

[Görüntüleme veya komut satırı yapılandırma örneği indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Kurulum ve komut satırı yapılandırma sağlayıcısı kullanın

# <a name="basic-configurationtabbasicconfiguration"></a>[Temel yapılandırma](#tab/basicconfiguration/)

Komut satırı yapılandırmasını etkinleştirmek için çağrı `AddCommandLine` örneği genişletme yöntemini [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Kod çalıştırmak, aşağıdaki çıktıyı görüntülenir:

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

Komut satırı yapılandırmasıyla diğer yapılandırma sağlayıcıları tarafından sağlanan yapılandırma geçersiz kılmak için çağrı `AddCommandLine` son `ConfigurationBuilder`:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Tipik bir ASP.NET Core 2.x uygulamaları kullanmak statik kolaylık yöntemi `CreateDefaultBuilder` ana bilgisayar oluşturmak için:

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` İsteğe bağlı yapılandırmasından yükler *appsettings.json*, *appsettings. { Ortam} .json*, [kullanıcı parolalarını](xref:security/app-secrets) (içinde `Development` ortamı), ortam değişkenleri ve komut satırı bağımsız değişkenleri. Komut satırı yapılandırma sağlayıcısı en son çağrılır. Sağlayıcıya son çağrı çalışma zamanında yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için geçirilen komut satırı bağımsız değişkenlerini daha önce çağırılır sağlar.

İçin *appsettings* nerede dosyaları:

* `reloadOnChange` etkin.
* Komut satırı bağımsız değişkenleri aynı ayarda içerir ve bir *appsettings* dosya.
* *Appsettings* eşleşen komut satırı bağımsız değişkenini içeren bir dosya uygulama başladıktan sonra değiştirildi.

Yukarıdaki koşulların tümü doğruysa, komut satırı bağımsız değişkenleri geçersiz kılınır.

ASP.NET Core 2.x uygulaması kullanabileceğiniz [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) yerine `CreateDefaultBuilder`. Kullanırken `WebHostBuilder`, el ile set yapılandırma [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder). Daha fazla bilgi için ASP.NET Core 1.x sekmesine bakın.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Oluşturma bir [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) ve çağrı `AddCommandLine` CommandLine yapılandırma sağlayıcısı kullanmak için yöntemi. Sağlayıcıya son çağrı çalışma zamanında yapılandırması bir yapılandırma sağlayıcıları tarafından ayarlanmış geçersiz kılmak için geçirilen komut satırı bağımsız değişkenlerini daha önce çağırılır sağlar. Yapılandırmasını uygulamak [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ile `UseConfiguration` yöntemi:

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Arguments

Komut satırında geçirilen bağımsız değişkenler, aşağıdaki tabloda gösterilen iki biçim birine uymalıdır:

| Bağımsız değişken biçimi                                                     | Örnek        |
| ------------------------------------------------------------------- | :------------: |
| Tek bir bağımsız değişken: bir anahtar-değer çifti ayrılmış bir eşittir işareti (`=`) | `key1=value`   |
| İki bağımsız değişken dizisi: bir anahtar-değer çifti boşlukla ayrılmış    | `/key1 value1` |

**Tek bir bağımsız değişken**

Değer bir eşittir işareti gelmelidir (`=`). Değer null olabilir (örneğin, `mykey=`).

Anahtarı bir önek olabilir.

| Anahtar ön eki               | Örnek         |
| ------------------------ | :-------------: |
| Önek yok                | `key1=value1`   |
| Tek bir tire (`-`)&#8224; | `-key2=value2`  |
| İki kısa çizgi (`--`)        | `--key3=value3` |
| Eğik çizgi (`/`)      | `/key4=value4`  |

&#8224;Tek bir ön ekine sahip bir anahtar (`-`) belirtilmelidir [geçiş eşlemeleri](#switch-mappings)aşağıda açıklandığı gibi.

Örnek komut:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Not: Varsa `-key2` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı, verilen bir `FormatException` oluşturulur.

**İki bağımsız değişken dizisi**

Değer null olamaz ve bir boşluk ile ayrılan anahtarı izlemelisiniz.

Anahtarı bir ön eki olmalıdır.

| Anahtar ön eki               | Örnek         |
| ------------------------ | :-------------: |
| Tek bir tire (`-`)&#8224; | `-key1 value1`  |
| İki kısa çizgi (`--`)        | `--key2 value2` |
| Eğik çizgi (`/`)      | `/key3 value3`  |

&#8224;Tek bir ön ekine sahip bir anahtar (`-`) belirtilmelidir [geçiş eşlemeleri](#switch-mappings)aşağıda açıklandığı gibi.

Örnek komut:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Not: Varsa `-key1` mevcut değilse [geçiş eşlemeleri](#switch-mappings) yapılandırma sağlayıcısı, verilen bir `FormatException` oluşturulur.

### <a name="duplicate-keys"></a>Yinelenen anahtarlar

Yinelenen anahtarlar sağlanırsa, son anahtar-değer çifti kullanılır.

### <a name="switch-mappings"></a>Geçiş eşlemeleri

El ile yapılandırma ile derleme yaparken `ConfigurationBuilder`, bir anahtar eşlemeleri sözlük eklenebilir `AddCommandLine` yöntemi. Anahtar, anahtar adı değiştirme mantıksal eşlemeler.

Anahtar eşlemeleri sözlüğü kullanıldığında, komut satırı bağımsız değişkeni tarafından belirtilen anahtarla eşleşen bir anahtara sözlük denetlenir. Komut satırı anahtarı sözlüğünde bulunursa, sözlük değeri (Anahtar değişimini) yapılandırma döndürülmek geçirilir. Tek bir kısa çizgi ile önek herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir (`-`).

Eşlemeleri sözlüğü anahtar kuralları anahtarı:

* Anahtarlar, kısa çizgi ile başlamalıdır (`-`) veya çift tire (`--`).
* Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.

Aşağıdaki örnekte, `GetSwitchMappings` yöntemi tek bir çizgi kullanılacak komut satırı bağımsız değişkenleri sağlar (`-`) anahtar öneki ve önde gelen alt anahtar önekleri kaçının.

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Komut satırı bağımsız değişkenleri sağlamadan sözlük için sağlanan `AddInMemoryCollection` yapılandırma değerlerini ayarlar. Şu komutla uygulamayı çalıştırın:

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

Anahtar eşlemeleri sözlüğünü oluşturduktan sonra aşağıdaki tabloda gösterilen verileri içerir:

| Anahtar            | Değer                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Anahtar geçişi sözlüğünü kullanarak göstermek için aşağıdaki komutu çalıştırın:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Komut satırı anahtarları değiştirilir. Konsol penceresinde görüntüler için yapılandırma değerlerini `Profile:MachineName` ve `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>Web.config dosyası

A *web.config* dosya, uygulama IIS veya IIS Express'te barındırma sırasında gereklidir. Ayarlarında *web.config* etkinleştirme [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) uygulamayı başlatın ve diğer IIS ayarlarını ve modülleri yapılandırmak için. Varsa *web.config* dosyası mevcut değil ve proje dosyasını içeren `<Project Sdk="Microsoft.NET.Sdk.Web">`, proje yayımlama oluşturur bir *web.config* yayımlanan çıkış dosyasında ( *Yayımlama* klasörü). Daha fazla bilgi için [ana bilgisayar Windows IIS üzerinde ASP.NET Core](xref:host-and-deploy/iis/index#webconfig-file).

## <a name="access-configuration-during-startup"></a>Başlatma sırasında erişimi yapılandırma

Erişim Yapılandırması içinde `ConfigureServices` veya `Configure` başlatma sırasında örneklere bakın [uygulama başlatma](xref:fundamentals/startup) konu.

## <a name="adding-configuration-from-an-external-assembly"></a>Dış bütünleştirilmiş koddan ekleme yapılandırması

Bir [Ihostingstartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) uygulama sağlayan uygulamanın dışındaki dış bütünleştirilmiş koddan başlatma sırasında bir uygulama için geliştirmeler ekleme `Startup` sınıfı. Daha fazla bilgi için [dış bütünleştirilmiş koddan uygulama geliştiren](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a>Erişim yapılandırmasında bir Razor sayfası veya MVC görünümü

Razor sayfaları sayfası ya da bir MVC görünümü yapılandırma ayarlarına erişmek için ekleme bir [using yönergesi](xref:mvc/views/razor#using) ([C# başvurusu: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) için [Microsoft.Extensions.Configuration ad alanı ](/dotnet/api/microsoft.extensions.configuration) ve ekleme [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) sayfası ya da görünümü.

Razor sayfaları sayfasında:

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

Bir MVC Görünümü'nde:

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a>Ek Notlar

* Bağımlılık ekleme (dı) ayarlanmamış kadar yukarı sonra `ConfigureServices` çağrılır.
* Yapılandırma sistemi DI farkında değildir.
* `IConfiguration` iki uzmanlıklar vardır:
  * `IConfigurationRoot` Kök düğümü için kullanılır. Bir yeniden tetikleyebilirsiniz.
  * `IConfigurationSection` Yapılandırma değerleri bir bölümünü temsil eder. `GetSection` Ve `GetChildren` yöntemleri döndürür bir `IConfigurationSection`.
  * Kullanım [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) yapılandırma yeniden yükleme ya da her sağlayıcısına erişim için. Bunlardan hiçbirine yaygındır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Seçenekler](xref:fundamentals/configuration/options)
* [Birden çok ortam kullanma](xref:fundamentals/environments)
* [Geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets)
* [ASP.NET core'da konak](xref:fundamentals/host/index)
* [Bağımlılık Ekleme](xref:fundamentals/dependency-injection)
* [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration)
