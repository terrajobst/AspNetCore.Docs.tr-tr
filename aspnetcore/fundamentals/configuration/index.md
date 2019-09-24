---
title: ASP.NET Core yapılandırma
author: guardrex
description: ASP.NET Core uygulamasını yapılandırmak için yapılandırma API 'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 357a3d89648086f0329cd16bc9d72863df9bdcd6
ms.sourcegitcommit: 8a36be1bfee02eba3b07b7a86085ec25c38bae6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71217792"
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core yapılandırma

Tarafından [Luke Latham](https://github.com/guardrex)

ASP.NET Core içindeki uygulama yapılandırması, *yapılandırma sağlayıcıları*tarafından belirlenen anahtar-değer çiftlerini temel alır. Yapılandırma sağlayıcıları yapılandırma verilerini çeşitli yapılandırma kaynaklarından anahtar-değer çiftlerine okur:

* Azure Key Vault
* Azure Uygulama yapılandırması
* Komut satırı bağımsız değişkenleri
* Özel sağlayıcılar (yüklü veya oluşturulmuş)
* Dizin dosyaları
* Ortam değişkenleri
* Bellek içi .NET nesneleri
* Ayarlar dosyaları

::: moniker range=">= aspnetcore-3.0"

Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), Framework tarafından örtük olarak dahil edilir.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.

::: moniker-end

Örnek uygulamada ve aşağıdaki kod örnekleri <xref:Microsoft.Extensions.Configuration> ad alanını kullanır:

```csharp
using Microsoft.Extensions.Configuration;
```

*Seçenekler stili* , bu konuda açıklanan yapılandırma kavramlarının bir uzantısıdır. Seçenekler, ilgili ayarların gruplarını temsil etmek için sınıfları kullanır. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>Uygulama yapılandırmasına karşı konak

Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır. Uygulama başlatma ve ömür yönetimi için konak sorumludur. Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır. Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir. Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz <xref:fundamentals/index#host>.

## <a name="default-configuration"></a>Varsayılan yapılandırma

::: moniker range=">= aspnetcore-3.0"

Ana bilgisayar oluştururken ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlar çağrısı <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> tabanlı Web Apps. `CreateDefaultBuilder`uygulama için varsayılan yapılandırmayı aşağıdaki sırayla sağlar:

[Genel ana bilgisayar](xref:fundamentals/host/generic-host)kullanan uygulamalar için aşağıdakiler geçerlidir. [Web konağını](xref:fundamentals/host/web-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [bu konunun ASP.NET Core 2,2 sürümüne](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)bakın.

* Ana bilgisayar yapılandırması şuradan sağlanır:
  * Ortam değişkenleri `DOTNET_` [yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak (örneğin, `DOTNET_ENVIRONMENT`) ön eki olan ortam değişkenleri. Yapılandırma anahtar-`DOTNET_`değer çiftleri yüklendiğinde önek () çıkarılır.
  * Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.
* Web ana bilgisayar varsayılan yapılandırması oluşturuldu (`ConfigureWebHostDefaults`):
  * Kestrel, Web sunucusu olarak kullanılır ve uygulamanın yapılandırma sağlayıcıları kullanılarak yapılandırılır.
  * Konak filtreleme ara yazılımı ekleyin.
  * `ASPNETCORE_FORWARDEDHEADERS_ENABLED` Ortam değişkeni olarak `true`ayarlanmışsa iletilen üstbilgiler ara yazılımı ekleyin.
  * IIS tümleştirmesini etkinleştirin.
* Uygulama yapılandırması şuradan sağlanır:
  * [dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .
  * *appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.
  * Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .
  * Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.
  * Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Ana bilgisayar oluştururken ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlar çağrısı <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> tabanlı Web Apps. `CreateDefaultBuilder`uygulama için varsayılan yapılandırmayı aşağıdaki sırayla sağlar:

[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanan uygulamalar için aşağıdakiler geçerlidir. [Genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [Bu konunun en son sürümüne](xref:fundamentals/configuration/index)bakın.

* Ana bilgisayar yapılandırması şuradan sağlanır:
  * Ortam değişkenleri `ASPNETCORE_` [yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak (örneğin, `ASPNETCORE_ENVIRONMENT`) ön eki olan ortam değişkenleri. Yapılandırma anahtar-`ASPNETCORE_`değer çiftleri yüklendiğinde önek () çıkarılır.
  * Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.
* Uygulama yapılandırması şuradan sağlanır:
  * [dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .
  * *appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.
  * Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .
  * Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.
  * Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.

::: moniker-end

## <a name="security"></a>Güvenlik

Hassas yapılandırma verilerini güvenli hale getirmek için aşağıdaki uygulamaları benimseyin:

* Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.
* Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.
* Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.

Daha fazla bilgi için aşağıdaki konulara bakın:

* <xref:fundamentals/environments>
* <xref:security/app-secrets>&ndash; Hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir. Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için dosya yapılandırma sağlayıcısını kullanır. Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen kısımlarında açıklanmıştır.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ASP.NET Core uygulamalar için uygulama gizli dizilerini güvenli bir şekilde depolar. Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.

## <a name="hierarchical-configuration-data"></a>Hiyerarşik yapılandırma verileri

Yapılandırma API 'SI, hiyerarşik verileri yapılandırma anahtarlarında bir sınırlayıcı kullanımıyla birlikte düzleştirerek hiyerarşik yapılandırma verilerini muhafaza ediyor.

Aşağıdaki JSON dosyasında, iki bölümün yapılandırılmış hiyerarşisinde dört anahtar mevcuttur:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

Dosya yapılandırmaya okunduğu zaman, yapılandırma kaynağının özgün hiyerarşik veri yapısını sürdürmek için benzersiz anahtarlar oluşturulur. Özgün yapıyı sürdürmek için bölümler ve anahtarlar iki nokta (`:`) kullanımıyla düzleştirilir:

* section0:key0
* section0: KEY1
* section1:key0
* Section1: KEY1

<xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yapılandırma verileri bölümünün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir. Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection-getchildren-and-exists)içinde açıklanmıştır.

## <a name="conventions"></a>Kurallar

### <a name="sources-and-providers"></a>Kaynaklar ve sağlayıcılar

Uygulama başlangıcında yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.

Değişiklik algılamayı uygulayan yapılandırma sağlayıcılarının, temel alınan bir ayar değiştirildiğinde yapılandırmayı yeniden yükleme yeteneği vardır. Örneğin, dosya yapılandırma sağlayıcısı (Bu konunun ilerleyen kısımlarında açıklanmıştır) ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılamayı uygular.

<xref:Microsoft.Extensions.Configuration.IConfiguration>uygulamanın [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında kullanılabilir. <xref:Microsoft.Extensions.Configuration.IConfiguration>, sınıfının yapılandırmasını elde etmek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> bir Razor Pages eklenebilir:

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

Yapılandırma sağlayıcıları, ana bilgisayar tarafından ayarlandıklarında kullanılamadığından, DI 'yi kullanamaz.

### <a name="keys"></a>Belirlenmesine

Yapılandırma anahtarları aşağıdaki kuralları benimseyin:

* Anahtarlar büyük/küçük harfe duyarlıdır. Örneğin, `ConnectionString` ve `connectionstring` eşdeğer anahtarlar olarak değerlendirilir.
* Aynı anahtar için bir değer aynı veya farklı yapılandırma sağlayıcıları tarafından ayarlandıysa, anahtardaki en son değer kullanılan değerdir.
* Hiyerarşik anahtarlar
  * Yapılandırma API 'si içinde, iki nokta üst üste`:`ayırıcı () tüm platformlarda çalışmaktadır.
  * Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir. Çift alt çizgi (`__`) tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta olarak dönüştürülür.
  * Azure Key Vault, hiyerarşik anahtarlar ayırıcı olarak `--` (iki tire) kullanır. Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde tirelerin yerini iki nokta ile değiştirmek için kod sağlamanız gerekir.
* , <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> Yapılandırma anahtarlarındaki dizi dizinlerini kullanarak nesnelere dizileri bağlamayı destekler. Dizi bağlama, [diziyi bir sınıfa bağlama](#bind-an-array-to-a-class) bölümünde açıklanmıştır.

### <a name="values"></a>Değerler

Yapılandırma değerleri aşağıdaki kuralları benimseyin:

* Değerler dizelerdir.
* Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.

## <a name="providers"></a>sağlayıcılarla

Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.

| Sağlayıcı | Şuradan yapılandırma sağlar&hellip; |
| -------- | ----------------------------------- |
| [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*Güvenlik* konuları) | Azure Key Vault |
| [Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure belgeleri) | Azure Uygulama yapılandırması |
| [Komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider) | Komut satırı parametreleri |
| [Özel yapılandırma sağlayıcısı](#custom-configuration-provider) | Özel kaynak |
| [Ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider) | Ortam değişkenleri |
| [Dosya yapılandırma sağlayıcısı](#file-configuration-provider) | Dosyalar (ıNı, JSON, XML) |
| [Dosya başına anahtar yapılandırma sağlayıcısı](#key-per-file-configuration-provider) | Dizin dosyaları |
| [Bellek yapılandırma sağlayıcısı](#memory-configuration-provider) | Bellek içi Koleksiyonlar |
| [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (*Güvenlik* konuları) | Kullanıcı profili dizinindeki dosya |

Yapılandırma kaynakları, başlangıçta yapılandırma sağlayıcılarının belirtilme sırasına göre okundu. Bu konuda açıklanan yapılandırma sağlayıcıları, kodunuzun onları düzenleyebilir sırada değil alfabetik sırayla açıklanmıştır. Kodunuzda yapılandırma sağlayıcılarını, temeldeki yapılandırma kaynakları için önceliklerinize uyacak şekilde sıralayın.

Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:

1. Dosyalar (*appSettings. JSON*, *appSettings. { Environment}. JSON*, `{Environment}` uygulamanın geçerli barındırma ortamıdır
1. [Azure Anahtar Kasası.](xref:security/key-vault-configuration)
1. [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (Yalnızca geliştirme ortamı)
1. Ortam değişkenleri
1. Komut satırı bağımsız değişkenleri

Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasını sağlamak üzere komut satırı yapılandırma sağlayıcısını bir sağlayıcı serisinde en son konumlandırmaktır.

Önceki sağlayıcı dizisi, ile `CreateDefaultBuilder`yeni bir konak Oluşturucu başlattığınızda kullanılır. Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a>Konak oluşturucuyu ConfigureHostConfiguration ile yapılandırma

Konak oluşturucuyu yapılandırmak için, yapılandırmayı çağırın <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> ve sağlayın. `ConfigureHostConfiguration`, <xref:Microsoft.Extensions.Hosting.IHostEnvironment> derleme sürecinde daha sonra kullanılmak üzere başlatmak için kullanılır. `ConfigureHostConfiguration`, eklenebilir sonuçlarla birden çok kez çağrılabilir.

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a>Konak oluşturucuyu UseConfiguration ile yapılandırma

Konak oluşturucuyu yapılandırmak için, yapılandırma ile <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> konak Oluşturucu üzerinde çağırın.

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args)
{
    var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

    var config = new ConfigurationBuilder()
        .AddInMemoryCollection(dict)
        .Build();

    return WebHost.CreateDefaultBuilder(args)
        .UseConfiguration(config)
        .UseStartup<Startup>();
}
```

::: moniker-end

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

Ana `ConfigureAppConfiguration` bilgisayarı oluşturma sırasında `CreateDefaultBuilder`otomatik olarak eklenenlere ek olarak, uygulamanın yapılandırma sağlayıcılarını belirtmek için çağırın:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a>Önceki yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kıl

Komut satırı bağımsız değişkenleriyle geçersiz kılınabilen uygulama yapılandırması sağlamak için, en son ' `AddCommandLine` u çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="consume-configuration-during-app-startup"></a>Uygulama başlatma sırasında yapılandırmayı kullan

Uygulamasında `ConfigureAppConfiguration` uygulamaya sağlanan yapılandırma, uygulamanın başlangıcında, dahil olmak üzere `Startup.ConfigureServices`kullanılabilir. Daha fazla bilgi için [başlatma sırasında erişim yapılandırması](#access-configuration-during-startup) bölümüne bakın.

## <a name="command-line-configuration-provider"></a>Komut satırı yapılandırma sağlayıcısı

, <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> Çalışma zamanında anahtar-değer çiftinden yapılandırma yükler.

Komut satırı yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemi çağırılır.

`AddCommandLine`çağrıldığında otomatik olarak çağrılır `CreateDefaultBuilder(string [])` . Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

`CreateDefaultBuilder`Ayrıca yüklenir:

* *AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.
* Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .
* Ortam değişkenleri.

`CreateDefaultBuilder`Komut satırı yapılandırma sağlayıcısını son ekler. Diğer sağlayıcılar tarafından ayarlanan çalışma zamanında geçersiz kılma yapılandırmasında komut satırı bağımsız değişkenleri geçirildi.

`CreateDefaultBuilder`Ana bilgisayar oluşturulduğunda davranır. Bu nedenle, tarafından `CreateDefaultBuilder` etkinleştirilen komut satırı yapılandırması konağın nasıl yapılandırıldığını etkileyebilir.

ASP.NET Core şablonlarına dayalı uygulamalar için, `AddCommandLine` tarafından `CreateDefaultBuilder`zaten çağırılır. Ek yapılandırma sağlayıcıları eklemek ve bu sağlayıcılardan yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kılmak için uygulamanın ek sağlayıcılarını `ConfigureAppConfiguration` çağırın ve en son çağrısı `AddCommandLine` yapın.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

**Örnek**

Örnek uygulama, ' a `CreateDefaultBuilder` <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>çağrı içeren konağı oluşturmak için statik kolaylık yönteminden yararlanır.

1. Projenin dizininde bir komut istemi açın.
1. `dotnet run` Komutuna bir komut satırı bağımsız değişkeni sağlayın, `dotnet run CommandLineKey=CommandLineValue`.
1. Uygulama çalıştırıldıktan sonra, konumundaki `http://localhost:5000`uygulamaya bir tarayıcı açın.
1. Çıktının ' de belirtilen `dotnet run`yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.

### <a name="arguments"></a>Arguments

Değer bir eşittir işaretini (`=`) izlemelidir veya değer bir boşluk izleyen anahtar bir ön eke (`--` ya da `/`) sahip olmalıdır. Eşittir işareti kullanılırsa değer gerekli değildir (örneğin, `CommandLineKey=`).

| Anahtar ön eki               | Örnek                                                |
| ------------------------ | ------------------------------------------------------ |
| Ön ek yok                | `CommandLineKey1=value1`                               |
| İki kısa çizgi`--`()        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Eğik çizgi (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile bir eşittir işareti kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.

Örnek komutlar:

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Eşleme Değiştir

Anahtar eşlemeleri anahtar adı değiştirme mantığına izin verir. İle <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>yapılandırmayı el ile oluşturduğunuzda, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> yöntemine anahtar değiştirmeler sözlüğü sağlayabilirsiniz.

Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir. Komut satırı anahtarı sözlükte bulunursa, sözlük değeri (anahtar değiştirme), anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir. Tek tireyle (`-`) ön eki eklenmiş herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.

Anahtar eşlemeleri sözlük anahtarı kuralları:

* Anahtarlar bir tire (`-`) veya çift tireyle (`--`) başlamalıdır.
* Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.

Anahtar eşlemeleri sözlüğü oluşturun. Aşağıdaki örnekte, iki anahtar eşlemesi oluşturulur:

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

Konak oluşturulduğunda, anahtar eşlemeleri sözlüğüne çağırın `AddCommandLine` :

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

Anahtar eşlemeleri kullanan uygulamalar için, ' ye `CreateDefaultBuilder` yapılan çağrı bağımsız değişkenleri geçirmez. Yöntemin çağrısı eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü öğesine `CreateDefaultBuilder`geçirmenin bir yolu yoktur. `AddCommandLine` `CreateDefaultBuilder` Çözüm, bağımsız değişkenleri öğesine `CreateDefaultBuilder` geçirmektir, bunun yerine `ConfigurationBuilder` metodun `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlar.

Anahtar eşlemeleri sözlüğü oluşturulduktan sonra, aşağıdaki tabloda gösterilen verileri içerir.

| Anahtar       | Değer             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

Uygulama başlatılırken anahtar eşlenmiş anahtarlar kullanılıyorsa, yapılandırma sözlük tarafından sağlanan anahtardaki yapılandırma değerini alır:

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

Önceki komutu çalıştırdıktan sonra, yapılandırma aşağıdaki tabloda gösterilen değerleri içerir.

| Anahtar               | Değer    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a>Ortam değişkenleri yapılandırma sağlayıcısı

, <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> Çalışma zamanında anahtar-değer çiftlerinde bulunan yapılandırmayı yükler.

Ortam değişkenleri yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

[Azure App Service](https://azure.microsoft.com/services/app-service/) , Azure portalında ortam değişkenleri yapılandırma sağlayıcısını kullanarak uygulama yapılandırmasını geçersiz kılabileceğiniz ortam değişkenlerini ayarlamanıza izin verir. Daha fazla bilgi için bkz [. Azure uygulamaları: Azure portalını](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)kullanarak uygulama yapılandırmasını geçersiz kılın.

::: moniker range=">= aspnetcore-3.0"

`AddEnvironmentVariables`, [genel konakla](xref:fundamentals/host/generic-host) yeni bir ana bilgisayar Oluşturucu `DOTNET_` başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için önekli ortam değişkenlerini yüklemek için kullanılır. Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

`AddEnvironmentVariables`, [Web ana](xref:fundamentals/host/web-host) bilgisayarıyla yeni bir konak Oluşturucu `ASPNETCORE_` başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için önekli ortam değişkenlerini yüklemek için kullanılır. Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

::: moniker-end

`CreateDefaultBuilder`Ayrıca yüklenir:

* Önek olmadan çağırarak `AddEnvironmentVariables` ön eki edilmemiş ortam değişkenlerinden uygulama yapılandırması.
* *AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.
* Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .
* Komut satırı bağımsız değişkenleri.

Ortam değişkenleri yapılandırma sağlayıcısı, Kullanıcı gizli dizileri ve *appSettings* dosyalarından yapılandırma kurulduktan sonra çağrılır. Bu konumda sağlayıcıyı çağırmak, çalışma zamanında ortam değişkenlerinin Kullanıcı parolaları ve *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılmak için okumasına izin verir.

Ek ortam değişkenlerinden uygulama yapılandırması sağlamanız gerekiyorsa, uygulamasının uygulamasındaki `ConfigureAppConfiguration` ek sağlayıcılarını çağırın ve önekiyle çağırın. `AddEnvironmentVariables`

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call additional providers here as needed.
    // Call AddEnvironmentVariables last if you need to allow
    // environment variables to override values from other 
    // providers.
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
}
```

**Örnek**

Örnek uygulama, ' a `CreateDefaultBuilder` `AddEnvironmentVariables`çağrı içeren konağı oluşturmak için statik kolaylık yönteminden yararlanır.

1. Örnek uygulamayı çalıştırın. Üzerinde `http://localhost:5000`uygulama için bir tarayıcı açın.
1. Çıktının ortam değişkeni `ENVIRONMENT`için anahtar-değer çiftini içerdiğini gözlemleyin. Değer, genellikle `Development` yerel olarak çalışırken uygulamanın çalıştığı ortamı yansıtır.

Uygulama kısaltması tarafından oluşturulan ortam değişkenlerinin listesini tutmak için, uygulama ortam değişkenlerini filtreler. Örnek uygulamanın *Pages/Index. cshtml. cs* dosyasına bakın.

Uygulama için kullanılabilir tüm ortam değişkenlerini göstermek istiyorsanız, `FilteredConfiguration` *Pages/Index. cshtml. cs* ' deki ' ı şu şekilde değiştirin:

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>Ön Ekler

Uygulamanın yapılandırmasına yüklenen ortam değişkenleri, `AddEnvironmentVariables` yöntemine bir ön ek sağladığınız zaman filtrelenir. Örneğin, önekte `CUSTOM_`ortam değişkenlerini filtrelemek için, yapılandırma sağlayıcısına öneki sağlayın:

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek çıkarılır.

Konak Oluşturucu oluşturulduğunda, ana bilgisayar yapılandırması ortam değişkenleri tarafından sağlanır. Bu ortam değişkenleri için kullanılan önek hakkında daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

**Bağlantı dizesi önekleri**

Yapılandırma API 'SI, uygulama ortamı için Azure bağlantı dizelerini yapılandırma ile ilgili dört bağlantı dizesi ortam değişkeni için özel işlem kuralları içerir. Üzerinde bir önek sağlanmazsa `AddEnvironmentVariables`, tabloda gösterilen öneklere sahip ortam değişkenleri uygulamaya yüklenir.

| Bağlantı dizesi öneki | Sağlayıcı |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Özel sağlayıcı |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:

* Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.
* Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (için `CUSTOMCONNSTR_`, belirtilen sağlayıcı olmayan).

| Ortam değişkeni anahtarı | Dönüştürülen yapılandırma anahtarı | Sağlayıcı yapılandırma girişi                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | Yapılandırma girişi oluşturulmamış.                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | Anahtar: `ConnectionStrings:<KEY>_ProviderName`:<br>Değer:`MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | Anahtar: `ConnectionStrings:<KEY>_ProviderName`:<br>Değer:`System.Data.SqlClient`  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | Anahtar: `ConnectionStrings:<KEY>_ProviderName`:<br>Değer:`System.Data.SqlClient`  |

## <a name="file-configuration-provider"></a>Dosya yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır. Aşağıdaki yapılandırma sağlayıcıları belirli dosya türlerine ayrılmıştır:

* [INı yapılandırma sağlayıcısı](#ini-configuration-provider)
* [JSON yapılandırma sağlayıcısı](#json-configuration-provider)
* [XML yapılandırma sağlayıcısı](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>INı yapılandırma sağlayıcısı

, <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> Çalışma zamanında INI dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.

INI dosya yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.

İki nokta üst üste, ıNı dosya yapılandırmasındaki bir bölüm sınırlayıcısı olarak kullanılabilir.

Aşırı yüklemeler belirtmeye izin ver:

* Dosyanın isteğe bağlı olup olmadığı.
* Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.
* Dosyaya erişmek için kullanılır. <xref:Microsoft.Extensions.FileProviders.IFileProvider>

Uygulamanın `ConfigureAppConfiguration` yapılandırmasını belirtmek için Konağı oluştururken çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

Taban yolu ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>ayarlanır.

Bir ıNı yapılandırma dosyasına genel bir örnek:

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

Önceki yapılandırma dosyası aşağıdaki anahtarları ile `value`yükler:

* section0:key0
* section0: KEY1
* Section1: alt bölüm: anahtar
* section2: subsection0: anahtar
* section2: subsection1: anahtar

### <a name="json-configuration-provider"></a>JSON yapılandırma sağlayıcısı

, <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> Çalışma zamanı sırasında JSON dosya anahtar-değer çiftlerinden yapılandırmayı yükler.

JSON dosya yapılandırmasını etkinleştirmek için bir <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.

Aşırı yüklemeler belirtmeye izin ver:

* Dosyanın isteğe bağlı olup olmadığı.
* Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.
* Dosyaya erişmek için kullanılır. <xref:Microsoft.Extensions.FileProviders.IFileProvider>

`AddJsonFile`, ile `CreateDefaultBuilder`yeni bir konak Oluşturucu başlattığınızda otomatik olarak iki kez çağrılır. Yöntemi, yapılandırmayı şuradan yüklemek için çağrılır:

* *appSettings. JSON* &ndash; bu dosya ilk kez okundu. Dosyanın ortam sürümü, *appSettings. JSON* dosyası tarafından belirtilen değerleri geçersiz kılabilir.
* *appSettings. {Environment}. JSON* &ndash; dosyanın ortam sürümü [ıhostingenvironment. environmentname](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir.

Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

`CreateDefaultBuilder`Ayrıca yüklenir:

* Ortam değişkenleri.
* Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .
* Komut satırı bağımsız değişkenleri.

JSON yapılandırma sağlayıcısı önce oluşturulur. Bu nedenle, Kullanıcı gizli dizileri, ortam değişkenleri ve komut satırı bağımsız değişkenleri, *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılar.

Ana `ConfigureAppConfiguration` bilgisayarı, *appSettings. JSON* ve appSettings dışındaki dosyalar için uygulamanın yapılandırmasını belirtmek üzere oluştururken çağırın *. { Environment}. JSON*:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

Taban yolu ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>ayarlanır.

**Örnek**

Örnek uygulama, ' ye `CreateDefaultBuilder` `AddJsonFile`iki çağrı içeren konağı oluşturmak için statik kolaylık yönteminden yararlanır. Yapılandırma *appSettings. JSON* ve appSettings 'ten yüklendi *. { Environment}. JSON*.

1. Örnek uygulamayı çalıştırın. Üzerinde `http://localhost:5000`uygulama için bir tarayıcı açın.
1. Çıktının, ortama bağlı olarak tabloda gösterilen yapılandırma için anahtar-değer çiftleri içerdiğini gözlemleyin. Günlüğe kaydetme yapılandırması anahtarları hiyerarşik ayırıcı olarak`:`iki nokta () kullanır.

| Anahtar                        | Geliştirme değeri | Üretim değeri |
| -------------------------- | :---------------: | :--------------: |
| Günlüğe kaydetme: LogLevel: System    | Bilgiler       | Bilgiler      |
| Günlüğe kaydetme: LogLevel: Microsoft | Bilgiler       | Bilgiler      |
| Günlüğe kaydetme: LogLevel: default   | Hata ayıklama             | Hata            |
| Allowedkonakları               | *                 | *                |

### <a name="xml-configuration-provider"></a>XML yapılandırma sağlayıcısı

, <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> Çalışma zamanında XML dosya anahtar-değer çiftlerinden yapılandırmayı yükler.

XML dosya yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.

Aşırı yüklemeler belirtmeye izin ver:

* Dosyanın isteğe bağlı olup olmadığı.
* Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.
* Dosyaya erişmek için kullanılır. <xref:Microsoft.Extensions.FileProviders.IFileProvider>

Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayılır. Dosyada bir belge türü tanımı (DTD) veya ad alanı belirtmeyin.

Uygulamanın `ConfigureAppConfiguration` yapılandırmasını belirtmek için Konağı oluştururken çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

Taban yolu ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>ayarlanır.

XML yapılandırma dosyaları, yinelenen bölümler için farklı öğe adları kullanabilir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

Önceki yapılandırma dosyası aşağıdaki anahtarları ile `value`yükler:

* section0:key0
* section0: KEY1
* section1:key0
* Section1: KEY1

Aynı öğe adını kullanan yinelenen öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılacaksa çalışır:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

Önceki yapılandırma dosyası aşağıdaki anahtarları ile `value`yükler:

* Bölüm: section0: Key: Key0
* Bölüm: section0: Key: KEY1
* Bölüm: Section1: Key: Key0
* Bölüm: Section1: Key: KEY1

Öznitelikler, değerler sağlamak için kullanılabilir:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

Önceki yapılandırma dosyası aşağıdaki anahtarları ile `value`yükler:

* anahtar: öznitelik
* Section: Key: özniteliği

## <a name="key-per-file-configuration-provider"></a>Dosya başına anahtar yapılandırma sağlayıcısı

, <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> Dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır. Anahtar, dosya adıdır. Değer, dosyanın içeriğini içerir. Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.

Dosya başına anahtar yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın. `directoryPath` Dosyaların mutlak bir yol olması gerekir.

Aşırı yüklemeler belirtmeye izin ver:

* Kaynağı `Action<KeyPerFileConfigurationSource>` yapılandıran bir temsilci.
* Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.

Çift alt çizgi (`__`), dosya adlarında yapılandırma anahtarı sınırlayıcısı olarak kullanılır. Örneğin, dosya adı `Logging__LogLevel__System` yapılandırma anahtarını `Logging:LogLevel:System`üretir.

Uygulamanın `ConfigureAppConfiguration` yapılandırmasını belirtmek için Konağı oluştururken çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.SetBasePath(Directory.GetCurrentDirectory());
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

Taban yolu ile <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>ayarlanır.

## <a name="memory-configuration-provider"></a>Bellek yapılandırma sağlayıcısı

, <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> Yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.

Bellek içi koleksiyon yapılandırmasını etkinleştirmek için, bir <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde genişletme yöntemini çağırın.

Yapılandırma sağlayıcısı bir `IEnumerable<KeyValuePair<String,String>>`ile başlatılabilir.

Uygulamanın `ConfigureAppConfiguration` yapılandırmasını belirtmek için Konağı oluştururken çağırın.

Aşağıdaki örnekte bir yapılandırma sözlüğü oluşturulur:

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

Sözlük, yapılandırmayı sağlamak `AddInMemoryCollection` için çağrısı ile kullanılır:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a>GetValue

[Configurationciltçi. GetValue\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) , yapılandırmadan belirtilen anahtarla bir değer ayıklar ve belirtilen türe dönüştürür. Bir aşırı yükleme, anahtar bulunamazsa varsayılan bir değer sağlamanıza izin verir.

Aşağıdaki örnek:

* Yapılandırmadaki dize değerini anahtarla `NumberKey`ayıklar. Yapılandırma anahtarlarında bulunmazsa, varsayılan `99` değeri kullanılır. `NumberKey`
* Değeri olarak bir `int`.
* Değeri, sayfanın kullanımı için `NumberConfig` özelliği içinde depolar.

```csharp
public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a>GetSection, GetChildren ve Exists

İzleyen örnekler için aşağıdaki JSON dosyasını göz önünde bulundurun. İki bölüm arasında dört anahtar bulunur ve bunlardan biri alt bölümleri çifti içerir:

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

Dosya yapılandırmaya okunduğu zaman yapılandırma değerlerini tutmak için aşağıdaki benzersiz hiyerarşik anahtarlar oluşturulur:

* section0:key0
* section0: KEY1
* section1:key0
* Section1: KEY1
* section2:subsection0:key0
* section2: subsection0: KEY1
* section2:subsection1:key0
* section2: subsection1: KEY1

### <a name="getsection"></a>GetSection

[Iconation. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) , belirtilen alt bölüm anahtarıyla bir yapılandırma alt bölümü ayıklar.

Yalnızca içindeki <xref:Microsoft.Extensions.Configuration.IConfigurationSection> `section1`anahtar-değer çiftlerini içeren bir öğesini döndürmek için, bölüm `GetSection` adını çağırın ve sağlayın:

```csharp
var configSection = _config.GetSection("section1");
```

Bir değere sahip değil, yalnızca bir anahtar ve yol yoktur. `configSection`

Benzer şekilde, içindeki `section2:subsection0`anahtarların değerlerini almak için, Bölüm yolunu çağırın `GetSection` ve sağlayın:

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection`hiçbir süre `null`geri döndürmez. Eşleşen bir bölüm bulunamazsa boş `IConfigurationSection` değer döndürülür.

Eşleşen bir bölüm <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value>döndürdüğünde,doldurulmuyor `GetSection` . Bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve<xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> , bölüm varsa döndürülür.

### <a name="getchildren"></a>GetChildren

Üzerinde`section2` [bir iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) çağrısı ,şunlarıiçerir:`IEnumerable<IConfigurationSection>`

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a>Var

Bir yapılandırma bölümünün mevcut olup olmadığını anlamak için [Configurationextensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) kullanın:

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

Örnek veriler `sectionExists` `false` verildiğinde, yapılandırma verilerinde bir `section2:subsection2` bölüm olmadığı için.

## <a name="bind-to-a-class"></a>Bir sınıfa bağlama

Yapılandırma, *Seçenekler modelini*kullanarak ilgili ayarların gruplarını temsil eden sınıflara bağlanabilir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.

Yapılandırma değerleri dizeler olarak döndürülür, ancak çağırma <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) nesnelerinin oluşturulmasını mümkün yapar.

Örnek uygulama bir `Starship` model içerir (*modeller/starsevk. cs*):

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

*Starsevk. JSON* dosyasının bölümü,örnekuygulamayapılandırmayıyüklemekiçinJSONyapılandırmasağlayıcısınıkullandığındayapılandırmayıoluşturur:`starship`

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

Aşağıdaki yapılandırma anahtar-değer çiftleri oluşturulur:

| Anahtar                   | Değer                                             |
| --------------------- | ------------------------------------------------- |
| starsevk: ad         | USS kurumsal                                    |
| starsevk: kayıt defteri     | NCC-1701                                          |
| starsevk: sınıfı        | Anaytion                                      |
| başlangıçgönder: Uzunluk       | 304,8                                             |
| starsevkiyat: Commissioned | False                                             |
| dır             | Paramount resimleri Corp. https://www.paramount.com |

Örnek uygulama, `starship` anahtarla `GetSection` çağırır. `starship` Anahtar-değer çiftleri yalıtılmıştır. Yöntemi, `Starship` sınıfının bir örneğinde geçen alt bölümde çağrılır. `Bind` Örnek değerlerini bağladıktan sonra örnek, işleme için bir özelliğe atanır:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a>Bir nesne grafiğine bağlama

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, bir POCO nesne grafiğinin tamamına bağlama yeteneğine sahiptir.

Örnek, nesne grafı `TvShow` içeren `Metadata` ve `Actors` sınıfları olan bir model içerir (*modeller/tvshow. cs*):

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

Örnek uygulama, yapılandırma verilerini içeren bir *tvshow. xml* dosyasına sahiptir:

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

Yapılandırma, `TvShow` `Bind` yöntemiyle nesne grafiğinin tamamına bağlanır. Bağlantılı örnek, işleme için bir özelliğe atandı:

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[Configurationciltçi. Get\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlar ve belirtilen türü döndürür. `Get<T>`, kullanmaktan `Bind`daha uygundur. Aşağıdaki kod, ilişkili örneğin işleme için `Get<T>` kullanılan özelliğe doğrudan atanmasını sağlayan önceki örnekle birlikte nasıl kullanılacağını gösterir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a>Bir diziyi sınıfa bağlama

*Örnek uygulama, bu bölümde açıklanan kavramları gösterir.*

, <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> Yapılandırma anahtarlarındaki dizi dizinlerini kullanarak nesnelere dizileri bağlamayı destekler. Bir sayısal anahtar segmenti`:0:`(, `:1:`, &hellip; `:{n}:`) sunan herhangi bir dizi biçimi, bir poco sınıfı dizisine dizi bağlama özelliğine sahiptir.

> [!NOTE]
> Bağlama, kural tarafından sağlanır. Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.

**Bellek içi dizi işleme**

Aşağıdaki tabloda gösterilen yapılandırma anahtarlarını ve değerlerini göz önünde bulundurun.

| Anahtar             | Değer  |
| :-------------: | :----: |
| dizi: girdiler: 0 | value0 |
| dizi: girdiler: 1 | value1 |
| dizi: girdiler: 2 | value2 |
| dizi: girdiler: 4 | value4 |
| dizi: girdiler: 5 | value5 |

Bu anahtarlar ve değerler, bellek yapılandırma sağlayıcısı kullanılarak örnek uygulamaya yüklenir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,23)]

::: moniker-end

Dizi, Dizin &num;3 için bir değeri atlar. Yapılandırma Bağlayıcısı, null değerleri bağlama veya bağlantılı nesnelerde null girişler oluşturma yeteneğine sahip değildir. Bu, bu diziyi bir nesneye bağlamanın sonucu gösterildiği sırada bir süre açık hale gelir.

Örnek uygulamada, bir POCO sınıfı, bağlantılı yapılandırma verilerini tutmak için kullanılabilir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

Yapılandırma verileri nesnesine bağlanır:

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

[Configurationciltçi. Get\<T >](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) sözdizimi de kullanılabilir ve bu da daha küçük kod elde edilebilir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

Bir örneği olan `ArrayExample`bağlantılı nesne, yapılandırmadan dizi verilerini alır.

| `ArrayExample.Entries`İndeks | `ArrayExample.Entries`Deeri |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1\.                            | value1                       |
| 2                            | value2                       |
| 3                            | value4                       |
| 4                            | value5                       |

İlişkili &num;nesnede Dizin 3 `array:4` yapılandırma `value4`anahtarı ve değeri için yapılandırma verilerini barındırır. Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri yalnızca nesne oluşturulurken yapılandırma verilerini yinelemek için kullanılır. Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.

Yapılandırmada doğru anahtar-değer çiftini &num;üreten herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğe bağlamadan önce dizin 3 için eksik yapılandırma öğesi sağlanabilir. Örnek, eksik anahtar-değer çiftine `ArrayExample.Entries` sahip ek bir JSON yapılandırma sağlayıcısı içeriyorsa, tam yapılandırma dizisiyle eşleşir:

*missing_value. JSON*:

```json
{
  "array:entries:3": "value3"
}
```

İçinde `ConfigureAppConfiguration`:

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

Tabloda gösterilen anahtar-değer çifti, yapılandırmaya yüklendi.

| Anahtar             | Değer  |
| :-------------: | :----: |
| dizi: girdiler: 3 | value3 |

Sınıf örneği, JSON yapılandırma sağlayıcısı Dizin &num;3 ' ü içeriyorsa, `ArrayExample.Entries` dizi değerini içerir. `ArrayExample`

| `ArrayExample.Entries`İndeks | `ArrayExample.Entries`Deeri |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1\.                            | value1                       |
| 2                            | value2                       |
| 3                            | value3                       |
| 4                            | value4                       |
| 5                            | value5                       |

**JSON dizi işleme**

JSON dosyası bir dizi içeriyorsa, sıfır tabanlı bölüm diziniyle dizi öğeleri için yapılandırma anahtarları oluşturulur. Aşağıdaki yapılandırma dosyasında `subsection` bir dizidir:

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

JSON yapılandırma sağlayıcısı, yapılandırma verilerini aşağıdaki anahtar-değer çiftlerine okur:

| Anahtar                     | Değer  |
| ----------------------- | :----: |
| json_array: anahtar          | değer EA |
| json_array: alt bölüm: 0 | valueB |
| json_array: alt bölüm: 1 | değer EC |
| json_array: alt bölüm: 2 | Değerler |

Örnek uygulamada, yapılandırma anahtar-değer çiftlerini bağlamak için aşağıdaki POCO sınıfı kullanılabilir:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

Bağlama `JsonArrayExample.Key` işleminden sonra değeri `valueA`barındırır. Alt bölüm değerleri POCO dizisi özelliğinde `Subsection`depolanır.

| `JsonArrayExample.Subsection`İndeks | `JsonArrayExample.Subsection`Deeri |
| :---------------------------------: | :---------------------------------: |
| 0                                   | valueB                              |
| 1\.                                   | değer EC                              |
| 2                                   | Değerler                              |

## <a name="custom-configuration-provider"></a>Özel yapılandırma sağlayıcısı

Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.

Sağlayıcı aşağıdaki özelliklere sahiptir:

* EF bellek içi veritabanı, tanıtım amacıyla kullanılır. Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı `ConfigurationBuilder` dizesini sağlamak üzere bir ikincil uygulayın.
* Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur. Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.
* Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.

Yapılandırma değerlerini `EFConfigurationValue` veritabanında depolamak için bir varlık tanımlayın.

::: moniker range=">= aspnetcore-3.0"

*Modeller/EFConfigurationValue. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Bir `EFConfigurationContext` mağazaya ekleyin ve yapılandırılan değerlere erişin.

*Efconfigurationprovider/EFConfigurationContext. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Uygulayan <xref:Microsoft.Extensions.Configuration.IConfigurationSource>bir sınıf oluşturun.

*Efconfigurationprovider/EFConfigurationSource. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Öğesinden <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralarak özel yapılandırma sağlayıcısını oluşturun. Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.

*Efconfigurationprovider/efconfigurationprovider. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Genişletme yöntemi, yapılandırma kaynağını bir `ConfigurationBuilder`öğesine eklemeye izin verir. `AddEFConfiguration`

*Uzantılar/EntityFrameworkExtensions. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

Aşağıdaki kod, `EFConfigurationProvider` *program.cs*içinde özel kullanımı göstermektedir:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

*Modeller/EFConfigurationValue. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Bir `EFConfigurationContext` mağazaya ekleyin ve yapılandırılan değerlere erişin.

*Efconfigurationprovider/EFConfigurationContext. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

Uygulayan <xref:Microsoft.Extensions.Configuration.IConfigurationSource>bir sınıf oluşturun.

*Efconfigurationprovider/EFConfigurationSource. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

Öğesinden <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralarak özel yapılandırma sağlayıcısını oluşturun. Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.

*Efconfigurationprovider/efconfigurationprovider. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

Genişletme yöntemi, yapılandırma kaynağını bir `ConfigurationBuilder`öğesine eklemeye izin verir. `AddEFConfiguration`

*Uzantılar/EntityFrameworkExtensions. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

Aşağıdaki kod, `EFConfigurationProvider` *program.cs*içinde özel kullanımı göstermektedir:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=30-31)]

::: moniker-end

## <a name="access-configuration-during-startup"></a>Başlangıç sırasında yapılandırmaya erişim

İçindeki `IConfiguration` yapılandırmadeğerlerineerişmek`Startup.ConfigureServices`için `Startup` oluşturucuya ekleme. ' Da yapılandırmaya `Startup.Configure`erişmek için, `IConfiguration` doğrudan yönteme ekleyin veya örneği oluşturucudan kullanın:

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz [. uygulama başlatma: Kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>Razor Pages sayfasında veya MVC görünümünde erişim yapılandırması

Razor Pages sayfasındaki veya MVC görünümündeki yapılandırma ayarlarına erişmek için, <xref:Microsoft.Extensions.Configuration.IConfiguration> [Microsoft. Extensions. Configuration ad alanı](xref:Microsoft.Extensions.Configuration) için bir [using yönergesi](xref:mvc/views/razor#using) ([ C# başvuru: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) ekleyin ve sayfaya ekleyin veya görünümü.

Razor Pages sayfasında:

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

MVC görünümünde:

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
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a>Bir dış derlemeden yapılandırma Ekle

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> Uygulama ,`Startup` uygulamanın sınıfı dışında bir dış derlemeden başlatma sırasında bir uygulamaya iyileştirmeler eklenmesine olanak sağlar. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/options>
