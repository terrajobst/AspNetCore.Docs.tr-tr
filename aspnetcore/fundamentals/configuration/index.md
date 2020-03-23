---
title: ASP.NET Core yapılandırma
author: rick-anderson
description: ASP.NET Core uygulamasını yapılandırmak için yapılandırma API 'sini kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: b4fa082c5a53bc9ecb3c7b8ddcbf243ef0d94ba7
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989686"
---
# <a name="configuration-in-aspnet-core"></a>ASP.NET Core yapılandırma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Kirk larkabağı](https://twitter.com/serpent5)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core yapılandırma bir veya daha fazla [yapılandırma sağlayıcısı](#cp)kullanılarak gerçekleştirilir. Yapılandırma sağlayıcıları çeşitli yapılandırma kaynakları kullanarak anahtar-değer çiftlerinden yapılandırma verilerini okur:

* *AppSettings. JSON* gibi ayarlar dosyaları
* Ortam değişkenleri
* Azure Key Vault
* Azure Uygulama Yapılandırması
* Komut satırı bağımsız değişkenleri
* Özel sağlayıcılar, yüklendi veya oluşturuldu
* Dizin dosyaları
* Bellek içi .NET nesneleri

[Görüntüleme veya indirme örnek kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

<a name="default"></a>

## <a name="default-configuration"></a>Varsayılan yapılandırma

[DotNet New](/dotnet/core/tools/dotnet-new) veya Visual Studio ile oluşturulan web Apps ASP.NET Core aşağıdaki kodu oluşturur:

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet&highlight=9)]

 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:

1. [Chainedconfigurationprovider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) :  Mevcut bir `IConfiguration` kaynak olarak ekler. Varsayılan yapılandırma durumunda, [ana bilgisayar](#hvac) yapılandırmasını ekler ve _uygulama_ yapılandırması için ilk kaynak olarak ayarlar.
1. [JSON yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak [appSettings. JSON](#appsettingsjson) .
1. *appSettings.* [JSON yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak`Environment` *. JSON* . Örneğin, *appSettings*. ***Üretim***. *JSON* ve *appSettings*. ***Geliştirme***. *JSON*.
1. Uygulama `Development` ortamda çalıştığında [uygulama gizli](xref:security/app-secrets) dizileri.
1. Ortam [değişkenleri yapılandırma sağlayıcısını](#evcp)kullanarak ortam değişkenleri.
1. Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.

Daha sonra eklenen yapılandırma sağlayıcıları önceki anahtar ayarlarını geçersiz kılar. Örneğin, `MyKey` hem *appSettings. JSON* hem de ortamda ayarlandıysa, ortam değeri kullanılır. Varsayılan yapılandırma sağlayıcılarını kullanarak, [komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider) diğer tüm sağlayıcıları geçersiz kılar.

`CreateDefaultBuilder`hakkında daha fazla bilgi için bkz. [Varsayılan Oluşturucu ayarları](xref:fundamentals/host/generic-host#default-builder-settings).

Aşağıdaki kod, etkin yapılandırma sağlayıcılarını eklendiği sırayla görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Index2.cshtml.cs?name=snippet)]

### <a name="appsettingsjson"></a>appSettings. JSON

Aşağıdaki *appSettings. JSON* dosyasını göz önünde bulundurun:

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarından birkaçını görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

Varsayılan <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> yapılandırmayı aşağıdaki sırayla yükler:

1. *appsettings.json*
1. *appSettings.* `Environment` *. JSON* : Örneğin, *appSettings*. ***Üretim***. *JSON* ve *appSettings*. ***Geliştirme***. *JSON* dosyaları. Dosyanın ortam sürümü, [ıhostingenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir. Daha fazla bilgi için bkz. <xref:fundamentals/environments>.

*appSettings*.`Environment`. *JSON* değerleri *appSettings. JSON*içindeki anahtarları geçersiz kılar. Örneğin, varsayılan olarak:

* Geliştirme sürümünde *appSettings*. ***Geliştirme***. *JSON* yapılandırması *appSettings. JSON*içinde bulunan değerlerin üzerine yazar.
* Üretimde, *appSettings*. ***Üretim***. *JSON* yapılandırması *appSettings. JSON*içinde bulunan değerlerin üzerine yazar. Örneğin, uygulamayı Azure 'a dağıtma.

<a name="optpat"></a>

#### <a name="bind-hierarchical-configuration-data-using-the-options-pattern"></a>Seçenek modelini kullanarak hiyerarşik yapılandırma verilerini bağlama

İlgili yapılandırma değerlerini okumak için tercih edilen yol, [Seçenekler modelini](xref:fundamentals/configuration/options)kullanmaktır. Örneğin, aşağıdaki yapılandırma değerlerini okumak için:

```json
  "Position": {
    "Title": "Editor",
    "Name": "Joe Smith"
  }
```

Aşağıdaki `PositionOptions` sınıfını oluşturun:

[!code-csharp[](index/samples/3.x/ConfigSample/Options/PositionOptions.cs?name=snippet)]

Türün tüm genel okuma-yazma özellikleri bağlanır. Alanlar bağlanmadı ***.***

Aşağıdaki kod:

* `PositionOptions` sınıfını `Position` bölümüne bağlamak için [Configurationciltçi. Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) ' i çağırır.
* `Position` yapılandırma verilerini görüntüler.

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test22.cshtml.cs?name=snippet)]

[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlanır ve belirtilen türü döndürür. `ConfigurationBinder.Get<T>` `ConfigurationBinder.Bind`kullanmaktan daha kullanışlı olabilir. Aşağıdaki kod, `ConfigurationBinder.Get<T>` `PositionOptions` sınıfıyla nasıl kullanılacağını gösterir:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test21.cshtml.cs?name=snippet)]

***Seçenekler deseninin*** kullanılması, `Position` bölümünü bağlamak ve [bağımlılık ekleme hizmeti kapsayıcısına](xref:fundamentals/dependency-injection)eklemektir. Aşağıdaki kodda, `PositionOptions` hizmet kapsayıcısına <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> ve yapılandırmaya bağlandığına eklenir:

[!code-csharp[](index/samples/3.x/ConfigSample/Startup.cs?name=snippet)]

Önceki kodu kullanarak, aşağıdaki kod konum seçeneklerini okur:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test2.cshtml.cs?name=snippet)]

[Varsayılan](#default) yapılandırmayı kullanarak, *appSettings. JSON* ve *appSettings.* `Environment` *. JSON* dosyaları [reloadonchange: true](https://github.com/dotnet/extensions/blob/release/3.1/src/Hosting/Hosting/src/Host.cs#L74-L75)ile etkinleştirilir. Uygulama başladıktan ***sonra*** *appSettings. JSON* ve *appSettings.* `Environment` *. JSON* dosyasında yapılan değişiklikler [JSON yapılandırma sağlayıcısı](#jcp)tarafından okundu.

Ek JSON yapılandırma dosyaları ekleme hakkında bilgi için bu belgede [JSON yapılandırma sağlayıcısına](#jcp) bakın.

<a name="security"></a>

## <a name="security-and-secret-manager"></a>Güvenlik ve gizli dizi Yöneticisi

Yapılandırma verileri yönergeleri:

* Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin. [Gizli](xref:security/app-secrets) dizi, geliştirmelerde gizli dizileri depolamak için kullanılabilir.
* Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.
* Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.

[Varsayılan](#default)olarak, [gizli yönetici](xref:security/app-secrets) *appSettings. JSON* ve *appSettings* .`Environment` *. JSON*sonrasında yapılandırma ayarlarını okur.

Parolaları veya diğer hassas verileri depolama hakkında daha fazla bilgi için:

* <xref:fundamentals/environments>
* <xref:security/app-secrets>:  Hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler içerir. Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için [dosya yapılandırma sağlayıcısını](#fcp) kullanır.

[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ASP.NET Core uygulamalar için uygulama gizli dizilerini güvenli bir şekilde depolar. Daha fazla bilgi için bkz. <xref:security/key-vault-configuration>.

<a name="evcp"></a>

## <a name="environment-variables"></a>Ortam değişkenleri

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>, [varsayılan](#default) yapılandırmayı kullanarak, *appSettings. JSON*, *appSettings.* `Environment` *. JSON*ve [gizli yönetici](xref:security/app-secrets)okumadan sonra anahtar-değer çiftlerinden yapılandırmayı yükler. Bu nedenle, ortamdan okunan anahtar değerleri *appSettings. JSON*, *appSettings.* `Environment` *. JSON*ve gizli yöneticisinden okunan değerleri geçersiz kılar.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Aşağıdaki `set` komutları:

* Windows üzerinde [önceki örneğin](#appsettingsjson) ortam anahtarlarını ve değerlerini ayarlayın.
* [Örnek indirmeyi](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample)kullanırken ayarları test edin. `dotnet run` komutunun proje dizininde çalıştırılması gerekir.

```dotnetcli
set MyKey="My key from Environment"
set Position__Title=Environment_Editor
set Position__Name=Environment_Rick
dotnet run
```

Önceki ortam ayarları:

* Yalnızca ' de ayarlanan komut penceresinden başlatılan işlemlerde ayarlanır.
* Visual Studio ile başlatılan tarayıcılar tarafından okunmayacaktır.

Aşağıdaki [Setx](/windows-server/administration/windows-commands/setx) komutları Windows üzerinde ortam anahtarlarını ve değerlerini ayarlamak için kullanılabilir. `set`farklı olarak, `setx` ayarları kalıcı hale getirilir. `/M`, Sistem ortamındaki değişkeni ayarlar. `/M` anahtarı kullanılmazsa, bir kullanıcı ortam değişkeni ayarlanır.

```cmd
setx MyKey "My key from setx Environment" /M
setx Position__Title Setx_Environment_Editor /M
setx Position__Name Environment_Rick /M
```

Yukarıdaki komutların *appSettings. JSON* ve *appSettings* .`Environment` *. JSON*' i geçersiz kılmasını test etmek için:

* Visual Studio ile: Çıkın ve Visual Studio 'Yu yeniden başlatın.
* CLı ile: Yeni bir komut penceresi başlatın ve `dotnet run`girin.

Ortam değişkenlerinin önekini belirtmek için bir dizeyle <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> çağırın:

[!code-csharp[](index/samples/3.x/ConfigSample/Program.cs?name=snippet4&highlight=12)]

Önceki kodda:

* `config.AddEnvironmentVariables(prefix: "MyCustomPrefix_")`, [varsayılan yapılandırma sağlayıcılarından](#default)sonra eklenir. Yapılandırma sağlayıcılarını sipariş eden bir örnek için bkz. [JSON yapılandırma sağlayıcısı](#jcp).
* `MyCustomPrefix_` önekiyle ayarlanan ortam değişkenleri [varsayılan yapılandırma sağlayıcılarını](#default)geçersiz kılar. Bu, öneki olmayan ortam değişkenlerini içerir.

Yapılandırma anahtar-değer çiftleri okunduktan sonra önek çıkarılır.

Aşağıdaki komutlar özel öneki test et:

```dotnetcli
set MyCustomPrefix_MyKey="My key with MyCustomPrefix_ Environment"
set MyCustomPrefix_Position__Title=Editor_with_customPrefix
set MyCustomPrefix_Position__Name=Environment_Rick_cp
dotnet run
```

[Varsayılan yapılandırma](#default) , `DOTNET_` ve `ASPNETCORE_`önekli ortam değişkenlerini ve komut satırı bağımsız değişkenlerini yükler. `DOTNET_` ve `ASPNETCORE_` önekleri [konak ve uygulama yapılandırması](xref:fundamentals/host/generic-host#host-configuration)için ASP.NET Core tarafından kullanılır, ancak kullanıcı yapılandırması için kullanılmaz. Konak ve uygulama yapılandırması hakkında daha fazla bilgi için bkz. [.NET genel ana bilgisayar](xref:fundamentals/host/generic-host).

[Azure App Service](https://azure.microsoft.com/services/app-service/), **Ayarlar > yapılandırma** sayfasında **Yeni uygulama ayarı** ' nı seçin. Azure App Service uygulama ayarları şunlardır:

* Rest 'de şifrelenir ve şifreli bir kanal üzerinden iletilir.
* Ortam değişkenleri olarak sunulur.

Daha fazla bilgi için bkz. Azure uygulamaları [: Azure portalını](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)kullanarak uygulama yapılandırmasını geçersiz kılın.

Azure veritabanı bağlantı dizeleri hakkında bilgi için bkz. [bağlantı dizesi önekleri](#constr) .

<a name="clcp"></a>

## <a name="command-line"></a>Komut Satırı

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> [varsayılan](#default) yapılandırmayı kullanarak, aşağıdaki yapılandırma kaynaklarından sonra komut satırı bağımsız değişkeni anahtar-değer çiftleriyle yapılandırma yükler:

* *appSettings. JSON* ve *appSettings*.`Environment`. *JSON* dosyaları.
* Geliştirme ortamında [uygulama gizli dizileri (gizli yönetici)](xref:security/app-secrets) .
* Ortam değişkenleri.

[Varsayılan](#default)olarak, komut satırı geçersiz kılma yapılandırma değerleri, diğer tüm yapılandırma sağlayıcılarıyla ayarlanan yapılandırma değerleri olarak ayarlanır.

### <a name="command-line-arguments"></a>Komut satırı bağımsız değişkenleri

Aşağıdaki komut `=`kullanarak anahtarları ve değerleri ayarlar:

```dotnetcli
dotnet run MyKey="My key from command line" Position:Title=Cmd Position:Name=Cmd_Rick
```

Aşağıdaki komut `/`kullanarak anahtarları ve değerleri ayarlar:

```dotnetcli
dotnet run /MyKey "Using /" /Position:Title=Cmd_ /Position:Name=Cmd_Rick
```

Aşağıdaki komut `--`kullanarak anahtarları ve değerleri ayarlar:

```dotnetcli
dotnet run --MyKey "Using --" --Position:Title=Cmd-- --Position:Name=Cmd--Rick
```

Anahtar değeri:

* `=`izlemelidir ya da anahtarın bir boşluk izleyen bir `--` veya `/` ön ekine sahip olması gerekir.
* `=` kullanılıyorsa gerekli değildir. Örneğin, `MySetting=`.

Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile `=` kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.

### <a name="switch-mappings"></a>Eşleme Değiştir

Anahtar eşlemeleri **anahtar** adı değiştirme mantığına izin verir. <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoduna anahtar değiştirmeler sözlüğü sağlayın.

Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir. Komut satırı anahtarı sözlükte bulunursa, sözlük değeri, anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir. Tek tire (`-`) ön eki olan herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.

Anahtar eşlemeleri sözlük anahtarı kuralları:

* Anahtarlar `-` veya `--`başlamalıdır.
* Anahtar eşlemeleri sözlüğü yinelenen anahtarlar içermemelidir.

Anahtar eşlemeleri sözlüğünü kullanmak için, `AddCommandLine`çağrıya geçirin:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramSwitch.cs?name=snippet&highlight=10-18,23)]

Aşağıdaki kod, değiştirilmiş anahtarların anahtar değerlerini gösterir:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test3.cshtml.cs?name=snippet)]

Anahtar değişimini test etmek için aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet run -k1=value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

Not: Şu anda `=`, anahtar değiştirme değerlerini tek bir tire `-`ayarlamak için kullanılamaz. [Bu GitHub sorununa](https://github.com/dotnet/extensions/issues/3059)bakın.

Aşağıdaki komut, anahtar değişimini test etmek için işe yarar:

```dotnetcli
dotnet run -k1 value1 -k2 value2 --alt3=value2 /alt4=value3 --alt5 value5 /alt6 value6
```

Anahtar eşlemeleri kullanan uygulamalar için `CreateDefaultBuilder` çağrısı bağımsız değişkenleri iletmemelidir. `CreateDefaultBuilder` yönteminin `AddCommandLine` çağrısı, eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü `CreateDefaultBuilder`geçirmenin bir yolu yoktur. Çözüm `CreateDefaultBuilder` bağımsız değişkenleri geçirmektir, ancak `ConfigurationBuilder` yönteminin `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlamak için.

## <a name="hierarchical-configuration-data"></a>Hiyerarşik yapılandırma verileri

Yapılandırma API 'SI, hiyerarşik verileri, yapılandırma anahtarlarında bir sınırlayıcı kullanımıyla birlikte düzleştirerek hiyerarşik yapılandırma verilerini okur.

[Örnek indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki *appSettings. JSON* dosyasını içerir:

[!code-json[](index/samples/3.x/ConfigSample/appsettings.json)]

[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, yapılandırma ayarlarından birkaçını görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

Hiyerarşik yapılandırma verilerini okumak için tercih edilen yol, Seçenekler modelini kullanmaktır. Daha fazla bilgi için, bkz. bu belgedeki [Hiyerarşik yapılandırma verilerini bağlama](#optpat) .

<xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemleri, yapılandırma verilerinde bir bölümün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir. Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection)içinde açıklanmıştır.

<!--
[Azure Key Vault configuration provider](xref:security/key-vault-configuration) implement change detection.
-->

## <a name="configuration-keys-and-values"></a>Yapılandırma anahtarları ve değerleri

Yapılandırma anahtarları:

* Büyük/küçük harfe duyarsızdır. Örneğin, `ConnectionString` ve `connectionstring` denk anahtarlar olarak değerlendirilir.
* Bir anahtar ve değer birden fazla yapılandırma sağlayıcısından ayarlandıysa, eklenen son sağlayıcıdan alınan değer kullanılır. Daha fazla bilgi için bkz. [varsayılan yapılandırma](#default).
* Hiyerarşik anahtarlar
  * Yapılandırma API 'SI içinde, tüm platformlarda bir iki nokta ayırıcı (`:`) kullanılır.
  * Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir. Çift alt çizgi, `__`tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta üst üste `:`dönüştürülür.
  * Azure Key Vault hiyerarşik anahtarlar ayırıcı olarak `--` kullanır. Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde `:` `--` değiştirmek için kod yazın.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler. Dizi bağlama, [diziyi bir sınıfa bağlama](#boa) bölümünde açıklanmıştır.

Yapılandırma değerleri:

* Dizeler.
* Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.

<a name="cp"></a>

## <a name="configuration-providers"></a>Yapılandırma sağlayıcıları

Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.

| Sağlayıcı | Şuradan yapılandırma sağlar |
| -------- | ----------------------------------- |
| [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) | Azure Key Vault |
| [Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) | Azure Uygulama Yapılandırması |
| [Komut satırı yapılandırma sağlayıcısı](#clcp) | Komut satırı parametreleri |
| [Özel yapılandırma sağlayıcısı](#custom-configuration-provider) | Özel kaynak |
| [Ortam değişkenleri yapılandırma sağlayıcısı](#evcp) | Ortam değişkenleri |
| [Dosya yapılandırma sağlayıcısı](#file-configuration-provider) | INı, JSON ve XML dosyaları |
| [Dosya başına anahtar yapılandırma sağlayıcısı](#key-per-file-configuration-provider) | Dizin dosyaları |
| [Bellek yapılandırma sağlayıcısı](#memory-configuration-provider) | Bellek içi Koleksiyonlar |
| [Gizli dizi Yöneticisi](xref:security/app-secrets)  | Kullanıcı profili dizinindeki dosya |

Yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu. Koddaki yapılandırma sağlayıcılarını, uygulamanın gerektirdiği temel yapılandırma kaynakları için önceliklere uyacak şekilde sıralayın.

Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:

1. *appsettings.json*
1. *appSettings*.`Environment`. *JSON*
1. [Gizli dizi Yöneticisi](xref:security/app-secrets)
1. Ortam [değişkenleri yapılandırma sağlayıcısını](#evcp)kullanarak ortam değişkenleri.
1. Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.

Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasına izin vermek için komut satırı yapılandırma sağlayıcısını en son bir sağlayıcı dizisine eklemektir.

Önceki sağlayıcı dizisi [Varsayılan yapılandırmada](#default)kullanılır.

<a name="constr"></a>

### <a name="connection-string-prefixes"></a>Bağlantı dizesi önekleri

Yapılandırma API 'sinin dört bağlantı dizesi ortam değişkeni için özel işleme kuralları vardır. Bu bağlantı dizeleri, uygulama ortamı için Azure bağlantı dizelerini yapılandırmaya dahil edilir. Tabloda gösterilen öneklere sahip ortam değişkenleri [varsayılan yapılandırmayla](#default) uygulamaya yüklenir veya `AddEnvironmentVariables`için hiç önek sağlanmaz.

| Bağlantı dizesi öneki | Sağlayıcı |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Özel sağlayıcı |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:

* Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.
* Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (`CUSTOMCONNSTR_`hariç, belirtilen sağlayıcı olmayan).

| Ortam değişkeni anahtarı | Dönüştürülen yapılandırma anahtarı | Sağlayıcı yapılandırma girişi                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | Yapılandırma girişi oluşturulmamış.                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | Anahtar: `ConnectionStrings:{KEY}_ProviderName`:<br>Değer:`MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | Anahtar: `ConnectionStrings:{KEY}_ProviderName`:<br>Değer:`System.Data.SqlClient`  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | Anahtar: `ConnectionStrings:{KEY}_ProviderName`:<br>Değer:`System.Data.SqlClient`  |

<a name="jcp"></a>

### <a name="json-configuration-provider"></a>JSON yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>, yapılandırmayı JSON dosya anahtar-değer çiftlerinden yükler.

Aşırı yüklemeler şunları belirtebilir:

* Dosyanın isteğe bağlı olup olmadığı.
* Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.

Aşağıdaki kodu göz önünde bulundurun:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON.cs?name=snippet&highlight=12-14)]

Yukarıdaki kod:

* JSON yapılandırma sağlayıcısını, *MyConfig. JSON* dosyasını yüklemek için aşağıdaki seçeneklerle yapılandırır:
  * `optional: true`: Dosya isteğe bağlıdır.
  * `reloadOnChange: true` : Değişiklikler kaydedildiğinde dosya yeniden yüklenir.
* *MyConfig. JSON* dosyasından önce [varsayılan yapılandırma sağlayıcılarını](#default) okur. [Ortam değişkenleri yapılandırma sağlayıcısı](#evcp) ve [komut satırı yapılandırma sağlayıcısı](#clcp)da dahil olmak üzere varsayılan yapılandırma sağlayıcılarındaki *MyConfig. JSON* dosyası geçersiz kılma ayarındaki ayarlar.

Genellikle, [ortam değişkenleri Yapılandırma sağlayıcısında](#evcp) ve [komut satırı yapılandırma sağlayıcısında](#clcp)ayarlanmış özel bir JSON dosyası değerlerini geçersiz ***kılmayı istemezsiniz.***

Aşağıdaki kod tüm yapılandırma sağlayıcılarını temizler ve çeşitli yapılandırma sağlayıcıları ekler:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSON2.cs?name=snippet)]

Yukarıdaki kodda, *MyConfig. JSON* ve *MyConfig*içindeki ayarlar.`Environment`. *JSON* dosyaları:

* *AppSettings. JSON* ve *appSettings*içindeki ayarları geçersiz kılın.`Environment`. *JSON* dosyaları.
* , [Ortam değişkenleri yapılandırma sağlayıcısı](#evcp) ve [komut satırı yapılandırma sağlayıcısı](#clcp)ayarları tarafından geçersiz kılınır.

[Örnek indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) , şu *MyConfig. JSON* dosyasını içerir:

[!code-json[](index/samples/3.x/ConfigSample/MyConfig.json)]

[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarından birkaçını görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

<a name="fcp"></a>

## <a name="file-configuration-provider"></a>Dosya yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır. Aşağıdaki yapılandırma sağlayıcıları `FileConfigurationProvider`türetilir:

* [INı yapılandırma sağlayıcısı](#ini-configuration-provider)
* [JSON yapılandırma sağlayıcısı](#jcp)
* [XML yapılandırma sağlayıcısı](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>INı yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider>, çalışma zamanında ıNı dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.

Aşağıdaki kod tüm yapılandırma sağlayıcılarını temizler ve çeşitli yapılandırma sağlayıcıları ekler:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramINI.cs?name=snippet&highlight=10-30)]

Yukarıdaki kodda, *myıniconfig. ini* ve *myıniconfig*içindeki ayarlar.`Environment`. *ını* dosyaları, içindeki ayarlar tarafından geçersiz kılınır:

* [Ortam değişkenleri yapılandırma sağlayıcısı](#evcp)
* [Komut satırı yapılandırma sağlayıcısı](#clcp).

[Örnek indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) , aşağıdaki *Myıniconfig. ini* dosyasını içerir:

[!code-ini[](index/samples/3.x/ConfigSample/MyIniConfig.ini)]

[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarından birkaçını görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

### <a name="xml-configuration-provider"></a>XML yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider>, çalışma zamanında XML dosya anahtar-değer çiftinden yapılandırma yükler.

Aşağıdaki kod tüm yapılandırma sağlayıcılarını temizler ve çeşitli yapılandırma sağlayıcıları ekler:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramXML.cs?name=snippet)]

Yukarıdaki kodda, *myXMLfile. xml* ve *myXMLfile*içindeki ayarlar.`Environment`. *XML* dosyaları, içindeki ayarlar tarafından geçersiz kılınır:

* [Ortam değişkenleri yapılandırma sağlayıcısı](#evcp)
* [Komut satırı yapılandırma sağlayıcısı](#clcp).

[Örnek indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) , şu *myXMLfile. xml* dosyasını içerir:

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile.xml)]

[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarından birkaçını görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

Aynı öğe adını kullanan tekrarlanan öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılırsa çalışır:

[!code-xml[](index/samples/3.x/ConfigSample/MyXMLFile3.xml)]

Aşağıdaki kod, önceki yapılandırma dosyasını okur ve anahtarları ve değerleri görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/XML/Index.cshtml.cs?name=snippet)]

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

Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:

* anahtar: öznitelik
* Section: Key: özniteliği

## <a name="key-per-file-configuration-provider"></a>Dosya başına anahtar yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider>, dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır. Anahtar, dosya adıdır. Değer, dosyanın içeriğini içerir. Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.

Dosya başına anahtar yapılandırması 'nı etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> uzantısı metodunu çağırın. Dosyaların `directoryPath` mutlak bir yol olmalıdır.

Aşırı yüklemeler belirtmeye izin ver:

* Kaynağı yapılandıran bir `Action<KeyPerFileConfigurationSource>` temsilcisi.
* Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.

Çift alt çizgi (`__`), dosya adlarında bir yapılandırma anahtarı sınırlayıcısı olarak kullanılır. Örneğin, `Logging__LogLevel__System` dosya adı `Logging:LogLevel:System`yapılandırma anahtarını üretir.

Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

<a name="mcp"></a>

## <a name="memory-configuration-provider"></a>Bellek yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider>, yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.

Aşağıdaki kod, yapılandırma sistemine bir bellek koleksiyonu ekler:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet6)]

[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) aşağıdaki kodu, önceki yapılandırma ayarlarını görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Test.cshtml.cs?name=snippet)]

Yukarıdaki kodda, `config.AddInMemoryCollection(Dict)` [varsayılan yapılandırma sağlayıcılarından](#default)sonra eklenir. Yapılandırma sağlayıcılarını sipariş eden bir örnek için bkz. [JSON yapılandırma sağlayıcısı](#jcp).

Yapılandırma sağlayıcılarını sipariş eden bir örnek için bkz. [JSON yapılandırma sağlayıcısı](#jcp).

Bkz. `MemoryConfigurationProvider`kullanarak bir diziyi başka bir örnek için [bağlama](#boa) .

## <a name="getvalue"></a>GetValue

[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) , belirli bir anahtarla yapılandırmadan tek bir değer ayıklar ve belirtilen türe dönüştürür:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestNum.cshtml.cs?name=snippet)]

Yukarıdaki kodda, yapılandırmada `NumberKey` bulunmazsa, varsayılan `99` değeri kullanılır.

## <a name="getsection-getchildren-and-exists"></a>GetSection, GetChildren ve Exists

İzleyen örnekler için aşağıdaki *Myalt bölüm. JSON* dosyasını göz önünde bulundurun:

[!code-json[](index/samples/3.x/ConfigSample/MySubsection.json)]

Aşağıdaki kod, *Myalt bölüm. JSON* öğesini yapılandırma sağlayıcılarına ekler:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONsection.cs?name=snippet)]

### <a name="getsection"></a>GetSection

[Iconation. GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) , belirtilen alt bölüm anahtarıyla bir yapılandırma alt bölümü döndürüyor.

Aşağıdaki kod `section1`için değerler döndürür:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection.cshtml.cs?name=snippet)]

Aşağıdaki kod `section2:subsection0`için değerler döndürür:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection2.cshtml.cs?name=snippet)]

`GetSection` hiçbir şekilde `null`döndürmez. Eşleşen bir bölüm bulunamazsa boş bir `IConfigurationSection` döndürülür.

`GetSection` eşleşen bir bölüm döndürdüğünde <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuyor. Bölüm mevcut olduğunda bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> döndürülür.

### <a name="getchildren-and-exists"></a>GetChildren ve Exists

Aşağıdaki kod, [Iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) 'ı çağırır ve `section2:subsection0`için değerleri döndürür:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/TestSection4.cshtml.cs?name=snippet)]

Yukarıdaki kod, bir bölümü doğrulamak için [Configurationextensions. Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) öğesini çağırır:

 <a name="boa"></a>

## <a name="bind-an-array"></a>Bir diziyi bağlama

[Configurationciltçi. Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*) , yapılandırma anahtarlarındaki dizi dizinlerini kullanarak dizilere dizi nesneleri bağlamayı destekler. Sayısal anahtar segmentini ortaya çıkaran herhangi bir dizi biçimi, [poco](https://wikipedia.org/wiki/Plain_Old_CLR_Object) sınıf dizisine dizi bağlama özelliğine sahiptir.

[Örnek indirmenizde](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples/3.x/ConfigSample) *myArray. JSON* ' i göz önünde bulundurun:

[!code-json[](index/samples/3.x/ConfigSample/MyArray.json)]

Aşağıdaki kod, yapılandırma sağlayıcılarına *myArray. JSON* ekler:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramJSONarray.cs?name=snippet)]

Aşağıdaki kod yapılandırmayı okur ve değerleri görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

Yukarıdaki kod aşağıdaki çıktıyı döndürür:

```text
Index: 0  Value: value00
Index: 1  Value: value10
Index: 2  Value: value20
Index: 3  Value: value40
Index: 4  Value: value50
```

Önceki çıktıda, Dizin 3 ' te, *myArray. JSON*içinde `"4": "value40",` karşılık gelen `value40`değeri vardır. Bağlantılı dizi dizinleri sürekli ve yapılandırma anahtarı diziniyle bağlantılı değildir. Yapılandırma Bağlayıcısı, bağlantılı nesnelerde null değerleri bağlama veya null girişler oluşturma yeteneğine sahip değil

Aşağıdaki kod, <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> uzantısı yöntemiyle `array:entries` yapılandırmasını yükler:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet)]

Aşağıdaki kod `arrayDict` `Dictionary` yapılandırmayı okur ve değerleri görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

Yukarıdaki kod aşağıdaki çıktıyı döndürür:

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value4
Index: 4  Value: value5
```

İlişkili nesnede &num;3 dizini, `array:4` yapılandırma anahtarı ve `value4`değeri için yapılandırma verilerini tutar. Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri, nesne oluştururken yapılandırma verilerini yinelemek için kullanılır. Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.

&num;3 dizini için eksik yapılandırma öğesi, Dizin &num;3 anahtar/değer çiftini okuyan herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğine bağlamadan önce sağlanabilir. Örnek indirmenin aşağıdaki *Value3. JSON* dosyasını göz önünde bulundurun:

[!code-json[](index/samples/3.x/ConfigSample/Value3.json)]

Aşağıdaki kod *Value3. JSON* yapılandırmasını ve `arrayDict` `Dictionary`içerir:

[!code-csharp[](index/samples/3.x/ConfigSample/ProgramArray.cs?name=snippet2)]

Aşağıdaki kod önceki yapılandırmayı okur ve değerleri görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/Pages/Array.cshtml.cs?name=snippet)]

Yukarıdaki kod aşağıdaki çıktıyı döndürür:

```text
Index: 0  Value: value0
Index: 1  Value: value1
Index: 2  Value: value2
Index: 3  Value: value3
Index: 4  Value: value4
Index: 5  Value: value5
```

Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.

## <a name="custom-configuration-provider"></a>Özel yapılandırma sağlayıcısı

Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.

Sağlayıcı aşağıdaki özelliklere sahiptir:

* EF bellek içi veritabanı, tanıtım amacıyla kullanılır. Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı dizesini sağlamak üzere ikincil `ConfigurationBuilder` uygulayın.
* Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur. Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.
* Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.

Yapılandırma değerlerini veritabanında depolamak için bir `EFConfigurationValue` varlığı tanımlayın.

*Modeller/EFConfigurationValue. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.

*Efconfigurationprovider/EFConfigurationContext. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.

*Efconfigurationprovider/EFConfigurationSource. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun. Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır. [Yapılandırma anahtarları büyük/küçük harfe duyarsız](#keys)olduğundan, veritabanını başlatmak için kullanılan sözlük, büyük/küçük harf duyarsız karşılaştırıcı ([StringComparer. OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)) ile oluşturulur.

*Efconfigurationprovider/efconfigurationprovider. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.

*Uzantılar/EntityFrameworkExtensions. cs*:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

<a name="acs"></a>

## <a name="access-configuration-in-startup"></a>Başlangıçta erişim yapılandırması

Aşağıdaki kod `Startup` yöntemlerde yapılandırma verilerini görüntüler:

[!code-csharp[](index/samples/3.x/ConfigSample/StartupKey.cs?name=snippet&highlight=13,18)]

Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz. uygulama başlatma [. Kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).

## <a name="access-configuration-in-razor-pages"></a>Razor Pages 'de erişim yapılandırması

Aşağıdaki kod, bir Razor sayfasında yapılandırma verilerini görüntüler:

[!code-cshtml[](index/samples/3.x/ConfigSample/Pages/Test5.cshtml)]

## <a name="access-configuration-in-a-mvc-view-file"></a>MVC görünüm dosyasındaki erişim yapılandırması

Aşağıdaki kod, yapılandırma verilerini bir MVC görünümünde görüntüler:

[!code-cshtml[](index/samples/3.x/ConfigSample/Views/Home2/Index.cshtml)]

<a name="hvac"></a>

## <a name="host-versus-app-configuration"></a>Uygulama yapılandırmasına karşı konak

Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır. Uygulama başlatma ve ömür yönetimi için konak sorumludur. Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır. Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir. Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz. <xref:fundamentals/index#host>.

<a name="dhc"></a>

## <a name="default-host-configuration"></a>Varsayılan konak yapılandırması

[Web konağını](xref:fundamentals/host/web-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [bu konunun ASP.NET Core 2,2 sürümüne](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2)bakın.

* Ana bilgisayar yapılandırması şuradan sağlanır:
  * Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `DOTNET_` (örneğin, `DOTNET_ENVIRONMENT`) ön ekine sahiptir. Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`DOTNET_`) çıkarılır.
  * Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.
* Web ana bilgisayar varsayılan yapılandırması oluşturuldu (`ConfigureWebHostDefaults`):
  * Kestrel, Web sunucusu olarak kullanılır ve uygulamanın yapılandırma sağlayıcıları kullanılarak yapılandırılır.
  * Konak filtreleme ara yazılımı ekleyin.
  * `ASPNETCORE_FORWARDEDHEADERS_ENABLED` ortam değişkeni `true`olarak ayarlandıysa, Iletilen üstbilgiler ara yazılımı ekleyin.
  * IIS tümleştirmesini etkinleştirin.

## <a name="other-configuration"></a>Diğer yapılandırma

Bu konu yalnızca *uygulama yapılandırması*ile ilgilidir. ASP.NET Core uygulamalarını çalıştırmanın diğer yönleri, bu konuda kapsanmayan yapılandırma dosyaları kullanılarak yapılandırılır:

* *Launch. json*/*launchsettings. JSON* , geliştirme ortamı için yapılandırma dosyalarını araçlar, açıklanmıştır:
  * <xref:fundamentals/environments#development>.
  * Dosyaların geliştirme senaryolarında ASP.NET Core Uygulamaları yapılandırmak için kullanıldığı belge kümesi boyunca.
* *Web. config* aşağıdaki konularda açıklanan bir sunucu yapılandırma dosyasıdır:
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

ASP.NET 'in önceki sürümlerinden uygulama yapılandırmasını geçirme hakkında daha fazla bilgi için bkz. <xref:migration/proper-to-2x/index#store-configurations>.

## <a name="add-configuration-from-an-external-assembly"></a>Bir dış derlemeden yapılandırma Ekle

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Ek kaynaklar

* [Yapılandırma kaynak kodu](https://github.com/dotnet/extensions/tree/master/src/Configuration)
* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core içindeki uygulama yapılandırması, *yapılandırma sağlayıcıları*tarafından belirlenen anahtar-değer çiftlerini temel alır. Yapılandırma sağlayıcıları yapılandırma verilerini çeşitli yapılandırma kaynaklarından anahtar-değer çiftlerine okur:

* Azure Key Vault
* Azure Uygulama Yapılandırması
* Komut satırı bağımsız değişkenleri
* Özel sağlayıcılar (yüklü veya oluşturulmuş)
* Dizin dosyaları
* Ortam değişkenleri
* Bellek içi .NET nesneleri
* Ayarlar dosyaları

Ortak yapılandırma sağlayıcısı senaryoları için yapılandırma paketleri ([Microsoft. Extensions. Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)), [Microsoft. Aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içinde bulunur.

Örnek uygulamada ve aşağıdaki kod örnekleri <xref:Microsoft.Extensions.Configuration> ad alanını kullanır:

```csharp
using Microsoft.Extensions.Configuration;
```

*Seçenekler stili* , bu konuda açıklanan yapılandırma kavramlarının bir uzantısıdır. Seçenekler, ilgili ayarların gruplarını temsil etmek için sınıfları kullanır. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/options>.

[Görüntüleme veya indirme örnek kodu](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="host-versus-app-configuration"></a>Uygulama yapılandırmasına karşı konak

Uygulama yapılandırıldıktan ve başlatılmadan önce, bir *konak* yapılandırılır ve başlatılır. Uygulama başlatma ve ömür yönetimi için konak sorumludur. Hem uygulama hem de ana bilgisayar, bu konuda açıklanan yapılandırma sağlayıcıları kullanılarak yapılandırılır. Ana bilgisayar yapılandırma anahtar-değer çiftleri, uygulamanın yapılandırmasına de dahildir. Konak oluşturulduğunda yapılandırma sağlayıcılarının nasıl kullanıldığı ve yapılandırma kaynaklarının konak yapılandırmasını nasıl etkilediği hakkında daha fazla bilgi için bkz. <xref:fundamentals/index#host>.

## <a name="other-configuration"></a>Diğer yapılandırma

Bu konu yalnızca *uygulama yapılandırması*ile ilgilidir. ASP.NET Core uygulamalarını çalıştırmanın diğer yönleri, bu konuda kapsanmayan yapılandırma dosyaları kullanılarak yapılandırılır:

* *Launch. json*/*launchsettings. JSON* , geliştirme ortamı için yapılandırma dosyalarını araçlar, açıklanmıştır:
  * <xref:fundamentals/environments#development>.
  * Dosyaların geliştirme senaryolarında ASP.NET Core Uygulamaları yapılandırmak için kullanıldığı belge kümesi boyunca.
* *Web. config* aşağıdaki konularda açıklanan bir sunucu yapılandırma dosyasıdır:
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

ASP.NET 'in önceki sürümlerinden uygulama yapılandırmasını geçirme hakkında daha fazla bilgi için bkz. <xref:migration/proper-to-2x/index#store-configurations>.

## <a name="default-configuration"></a>Varsayılan yapılandırma

ASP.NET Core [DotNet yeni](/dotnet/core/tools/dotnet-new) şablonlara dayalı Web Apps, bir konak oluştururken <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> çağırır. `CreateDefaultBuilder`, uygulama için aşağıdaki sırayla varsayılan yapılandırmayı sağlar:

[Web ana bilgisayarı](xref:fundamentals/host/web-host)kullanan uygulamalar için aşağıdakiler geçerlidir. [Genel ana bilgisayarı](xref:fundamentals/host/generic-host)kullanırken varsayılan yapılandırmayla ilgili ayrıntılar için, [Bu konunun en son sürümüne](xref:fundamentals/configuration/index)bakın.

* Ana bilgisayar yapılandırması şuradan sağlanır:
  * Ortam değişkenleri, [ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider)kullanılarak `ASPNETCORE_` (örneğin, `ASPNETCORE_ENVIRONMENT`) ön ekine sahiptir. Yapılandırma anahtar-değer çiftleri yüklendiğinde önek (`ASPNETCORE_`) çıkarılır.
  * Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.
* Uygulama yapılandırması şuradan sağlanır:
  * [dosya yapılandırma sağlayıcısını](#file-configuration-provider)kullanarak *appSettings. JSON* .
  * *appSettings.*  [Dosya yapılandırma sağlayıcısı](#file-configuration-provider)kullanılarak {Environment}. JSON.
  * Uygulama, giriş derlemesini kullanarak `Development` ortamda çalıştırıldığında [gizli Yöneticisi](xref:security/app-secrets) .
  * Ortam [değişkenleri yapılandırma sağlayıcısını](#environment-variables-configuration-provider)kullanarak ortam değişkenleri.
  * Komut satırı [yapılandırma sağlayıcısını](#command-line-configuration-provider)kullanan komut satırı bağımsız değişkenleri.

## <a name="security"></a>Güvenlik

Hassas yapılandırma verilerini güvenli hale getirmek için aşağıdaki uygulamaları benimseyin:

* Yapılandırma sağlayıcısı kodunda veya düz metin yapılandırma dosyalarında parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.
* Geliştirme veya test ortamlarında üretim gizli dizileri kullanmayın.
* Yanlışlıkla bir kaynak kodu deposuna uygulanamazlar için proje dışındaki gizli dizileri belirtin.

Daha fazla bilgi için aşağıdaki konulara bakın:

* <xref:fundamentals/environments>
* <xref:security/app-secrets> &ndash;, hassas verileri depolamak için ortam değişkenlerini kullanma hakkında öneriler Içerir. Gizli dizi Yöneticisi, Kullanıcı gizli dizilerini yerel sistemdeki bir JSON dosyasında depolamak için dosya yapılandırma sağlayıcısını kullanır. Dosya yapılandırma sağlayıcısı, bu konunun ilerleyen kısımlarında açıklanmıştır.

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

Dosya yapılandırmaya okunduğu zaman, yapılandırma kaynağının özgün hiyerarşik veri yapısını sürdürmek için benzersiz anahtarlar oluşturulur. Bölüm ve anahtarlar, özgün yapıyı sürdürmek için iki nokta üst üste (`:`) kullanımıyla düzleştirilir:

* section0:key0
* section0: KEY1
* section1:key0
* Section1: KEY1

<xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> ve <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> yöntemleri, yapılandırma verilerinde bir bölümün bölümlerini ve alt öğelerini yalıtmak için kullanılabilir. Bu yöntemler daha sonra [GetSection, GetChildren ve Exists](#getsection-getchildren-and-exists)içinde açıklanmıştır.

## <a name="conventions"></a>Kurallar

### <a name="sources-and-providers"></a>Kaynaklar ve sağlayıcılar

Uygulama başlangıcında yapılandırma kaynakları, yapılandırma sağlayıcılarının belirtilme sırasına göre okundu.

Değişiklik algılamayı uygulayan yapılandırma sağlayıcılarının, temel alınan bir ayar değiştirildiğinde yapılandırmayı yeniden yükleme yeteneği vardır. Örneğin, dosya yapılandırma sağlayıcısı (Bu konunun ilerleyen kısımlarında açıklanmıştır) ve [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) değişiklik algılamayı uygular.

<xref:Microsoft.Extensions.Configuration.IConfiguration>, uygulamanın [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection) kapsayıcısında kullanılabilir. <xref:Microsoft.Extensions.Configuration.IConfiguration>, sınıfın yapılandırmasını elde etmek için bir Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> veya MVC <xref:Microsoft.AspNetCore.Mvc.Controller> eklenebilir.

Aşağıdaki örneklerde `_config` alanı yapılandırma değerlerine erişmek için kullanılır:

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

Yapılandırma sağlayıcıları, ana bilgisayar tarafından ayarlandıklarında kullanılamadığından, DI 'yi kullanamaz.

### <a name="keys"></a>Anahtarlar

Yapılandırma anahtarları aşağıdaki kuralları benimseyin:

* Anahtarlar büyük/küçük harfe duyarlıdır. Örneğin, `ConnectionString` ve `connectionstring` denk anahtarlar olarak değerlendirilir.
* Aynı anahtar için bir değer aynı veya farklı yapılandırma sağlayıcıları tarafından ayarlandıysa, anahtardaki en son değer kullanılan değerdir.
* Hiyerarşik anahtarlar
  * Yapılandırma API 'SI içinde, tüm platformlarda bir iki nokta ayırıcı (`:`) kullanılır.
  * Ortam değişkenlerinde, tüm platformlarda bir iki nokta ayırıcı çalışmayabilir. Çift alt çizgi (`__`) tüm platformlar tarafından desteklenir ve otomatik olarak iki nokta olarak dönüştürülür.
  * Azure Key Vault hiyerarşik anahtarlar ayırıcı olarak `--` (iki tire) kullanır. Gizli dizileri uygulamanın yapılandırmasına yüklendiğinde tireleri bir iki nokta ile değiştirmek için kod yazın.
* <xref:Microsoft.Extensions.Configuration.ConfigurationBinder>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler. Dizi bağlama, [diziyi bir sınıfa bağlama](#bind-an-array-to-a-class) bölümünde açıklanmıştır.

### <a name="values"></a>Değerler

Yapılandırma değerleri aşağıdaki kuralları benimseyin:

* Değerler dizelerdir.
* Null değerler yapılandırmada saklanamaz veya nesnelere bağlanabilir.

## <a name="providers"></a>Sağlayıcılar

Aşağıdaki tabloda ASP.NET Core uygulamalar için kullanılabilen yapılandırma sağlayıcıları gösterilmektedir.

| Sağlayıcı | &hellip; yapılandırma sağlar |
| -------- | ----------------------------------- |
| [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration) (*güvenlik* konuları) | Azure Key Vault |
| [Azure uygulama yapılandırma sağlayıcısı](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure belgeleri) | Azure Uygulama Yapılandırması |
| [Komut satırı yapılandırma sağlayıcısı](#command-line-configuration-provider) | Komut satırı parametreleri |
| [Özel yapılandırma sağlayıcısı](#custom-configuration-provider) | Özel kaynak |
| [Ortam değişkenleri yapılandırma sağlayıcısı](#environment-variables-configuration-provider) | Ortam değişkenleri |
| [Dosya yapılandırma sağlayıcısı](#file-configuration-provider) | Dosyalar (ıNı, JSON, XML) |
| [Dosya başına anahtar yapılandırma sağlayıcısı](#key-per-file-configuration-provider) | Dizin dosyaları |
| [Bellek yapılandırma sağlayıcısı](#memory-configuration-provider) | Bellek içi Koleksiyonlar |
| [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (*güvenlik* konuları) | Kullanıcı profili dizinindeki dosya |

Yapılandırma kaynakları, başlangıçta yapılandırma sağlayıcılarının belirtilme sırasına göre okundu. Bu konu başlığı altında açıklanan yapılandırma sağlayıcıları, kodun onları düzenler sırasına göre değil alfabetik sırayla açıklanmıştır. Koddaki yapılandırma sağlayıcılarını, uygulamanın gerektirdiği temel yapılandırma kaynakları için önceliklere uyacak şekilde sıralayın.

Yapılandırma sağlayıcılarının tipik bir sırası şunlardır:

1. Dosyalar (*appSettings. JSON*, *appSettings. { Environment}. JSON*, `{Environment}` uygulamanın geçerli barındırma ortamıdır.
1. [Azure Anahtar Kasası.](xref:security/key-vault-configuration)
1. [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) (yalnızca geliştirme ortamı)
1. Ortam değişkenleri
1. Komut satırı bağımsız değişkenleri

Ortak bir uygulama, komut satırı bağımsız değişkenlerinin diğer sağlayıcılar tarafından ayarlanan yapılandırmayı geçersiz kılmasını sağlamak üzere komut satırı yapılandırma sağlayıcısını bir sağlayıcı serisinde en son konumlandırmaktır.

Yeni bir ana bilgisayar Oluşturucusu `CreateDefaultBuilder`ile başlatıldığında yukarıdaki sağlayıcılar dizisi kullanılır. Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

## <a name="configure-the-host-builder-with-useconfiguration"></a>Konak oluşturucuyu UseConfiguration ile yapılandırma

Konak oluşturucuyu yapılandırmak için, yapılandırma ile konak Oluşturucu üzerinde <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> çağırın.

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

## <a name="configureappconfiguration"></a>ConfigureAppConfiguration

`CreateDefaultBuilder`tarafından otomatik olarak eklenenlere ek olarak, uygulamanın yapılandırma sağlayıcılarını belirlemek için konak oluştururken `ConfigureAppConfiguration` çağırın:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a>Önceki yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kıl

Komut satırı bağımsız değişkenleriyle geçersiz kılınabilen uygulama yapılandırması sağlamak için `AddCommandLine` son ' u çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a>CreateDefaultBuilder tarafından eklenen sağlayıcıları kaldır

`CreateDefaultBuilder`tarafından eklenen sağlayıcıları kaldırmak için, önce [ılisteationbuilder. Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) üzerinde [clear](/dotnet/api/system.collections.generic.icollection-1.clear) öğesini çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a>Uygulama başlatma sırasında yapılandırmayı kullan

`ConfigureAppConfiguration` içinde uygulamaya sağlanan yapılandırma, uygulamanın başlangıcında `Startup.ConfigureServices`dahil olmak üzere kullanılabilir. Daha fazla bilgi için [başlatma sırasında erişim yapılandırması](#access-configuration-during-startup) bölümüne bakın.

## <a name="command-line-configuration-provider"></a>Komut satırı yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider>, çalışma zamanında komut satırı bağımsız değişkeni anahtar-değer çiftinden yapılandırma yükler.

Komut satırı yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> uzantısı yöntemi bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde çağrılır.

`AddCommandLine`, `CreateDefaultBuilder(string [])` çağrıldığında otomatik olarak çağrılır. Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

`CreateDefaultBuilder` de yüklenir:

* *AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.
* Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .
* Ortam değişkenleri.

`CreateDefaultBuilder`, komut satırı yapılandırma sağlayıcısını en son ekler. Diğer sağlayıcılar tarafından ayarlanan çalışma zamanında geçersiz kılma yapılandırmasında komut satırı bağımsız değişkenleri geçirildi.

`CreateDefaultBuilder` ana bilgisayar oluşturulduğunda davranır. Bu nedenle, `CreateDefaultBuilder` tarafından etkinleştirilen komut satırı yapılandırması konağın nasıl yapılandırıldığını etkileyebilir.

ASP.NET Core şablonlarına dayalı uygulamalar için, `AddCommandLine` `CreateDefaultBuilder`tarafından zaten çağırılır. Ek yapılandırma sağlayıcıları eklemek ve bu sağlayıcılardan yapılandırmayı komut satırı bağımsız değişkenleriyle geçersiz kılmak için, `ConfigureAppConfiguration` 'de uygulamanın ek sağlayıcılarını çağırın ve `AddCommandLine` son ' u çağırın.

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

**Örnek**

Örnek uygulama, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.

1. Projenin dizininde bir komut istemi açın.
1. `dotnet run` komutuna bir komut satırı bağımsız değişkeni sağlayın, `dotnet run CommandLineKey=CommandLineValue`.
1. Uygulama çalıştıktan sonra, `http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.
1. Çıktının `dotnet run`için belirtilen yapılandırma komut satırı bağımsız değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin.

### <a name="arguments"></a>Bağımsız Değişkenler

Değer bir eşittir işareti (`=`) izlemelidir veya değer bir boşluk izleyen anahtarın bir ön eki (`--` veya `/`) olmalıdır. Eşittir işareti kullanılırsa değer gerekli değildir (örneğin, `CommandLineKey=`).

| Anahtar ön eki               | Örnek                                                |
| ------------------------ | ------------------------------------------------------ |
| Ön ek yok                | `CommandLineKey1=value1`                               |
| İki kısa çizgi (`--`)        | `--CommandLineKey2=value2`, `--CommandLineKey2 value2` |
| Eğik çizgi (`/`)      | `/CommandLineKey3=value3`, `/CommandLineKey3 value3`   |

Aynı komut içinde, bir boşluk kullanan anahtar-değer çiftleri ile bir eşittir işareti kullanan komut satırı bağımsız değişkeni anahtar-değer çiftlerini karıştırmayın.

Örnek komutlar:

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a>Eşleme Değiştir

Anahtar eşlemeleri anahtar adı değiştirme mantığına izin verir. Bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>el ile yapılandırma oluştururken, <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> metoduna geçiş değiştirme sözlüğü sağlar.

Anahtar eşlemeleri sözlüğü kullanıldığında, sözlük bir komut satırı bağımsız değişkeni tarafından sunulan anahtarla eşleşen bir anahtar için denetlenir. Komut satırı anahtarı sözlükte bulunursa, sözlük değeri (anahtar değiştirme), anahtar-değer çiftini uygulamanın yapılandırmasına ayarlamak için geri geçirilir. Tek tire (`-`) ön eki olan herhangi bir komut satırı anahtarı için bir anahtar eşlemesi gereklidir.

Anahtar eşlemeleri sözlük anahtarı kuralları:

* Anahtarlar tireyle (`-`) veya çift tireyle başlamalıdır (`--`).
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

Konak oluşturulduğunda, anahtar eşlemeleri sözlüğüne `AddCommandLine` çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

Anahtar eşlemeleri kullanan uygulamalar için `CreateDefaultBuilder` çağrısı bağımsız değişkenleri iletmemelidir. `CreateDefaultBuilder` yönteminin `AddCommandLine` çağrısı, eşlenmiş anahtarlar içermez ve anahtar eşleme sözlüğünü `CreateDefaultBuilder`'e iletmenin bir yolu yoktur. Çözüm `CreateDefaultBuilder` bağımsız değişkenleri geçirmektir, ancak `ConfigurationBuilder` yönteminin `AddCommandLine` yönteminin hem bağımsız değişkenleri hem de anahtar eşleme sözlüğünü işlemesini sağlamak için.

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

<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider>, çalışma zamanında anahtar-değer çiftlerinde bulunan yapılandırmayı yükler.

Ortam değişkenleri yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> uzantısı metodunu çağırın.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

[Azure App Service](https://azure.microsoft.com/services/app-service/) , Azure portalında ortam değişkenleri yapılandırma sağlayıcısını kullanarak uygulama yapılandırmasını geçersiz kılabileceği ortam değişkenlerini ayarlamaya izin verir. Daha fazla bilgi için bkz. Azure uygulamaları [: Azure portalını](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal)kullanarak uygulama yapılandırmasını geçersiz kılın.

`AddEnvironmentVariables`, [Web ana](xref:fundamentals/host/web-host) bilgisayarıyla yeni bir ana bilgisayar Oluşturucu başlatıldığında ve `CreateDefaultBuilder` çağrıldığında, [ana bilgisayar yapılandırması](#host-versus-app-configuration) için `ASPNETCORE_` ön eki eklenmiş ortam değişkenlerini yüklemek için kullanılır. Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

`CreateDefaultBuilder` de yüklenir:

* Önek olmadan `AddEnvironmentVariables` çağırarak, ön eki edilmemiş ortam değişkenlerinden uygulama yapılandırması.
* *AppSettings. JSON* ve appSettings 'ten isteğe bağlı yapılandırma *. { Environment}. JSON* dosyaları.
* Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .
* Komut satırı bağımsız değişkenleri.

Ortam değişkenleri yapılandırma sağlayıcısı, Kullanıcı gizli dizileri ve *appSettings* dosyalarından yapılandırma kurulduktan sonra çağrılır. Bu konumda sağlayıcıyı çağırmak, çalışma zamanında ortam değişkenlerinin Kullanıcı parolaları ve *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılmak için okumasına izin verir.

Ek ortam değişkenlerinden uygulama yapılandırması sağlamak için, uygulamanın `ConfigureAppConfiguration` ek sağlayıcılarını çağırın ve önekiyle birlikte `AddEnvironmentVariables` çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

Diğer sağlayıcılardan gelen değerleri geçersiz kılmak için verilen öneke sahip ortam değişkenlerine izin vermek üzere en son `AddEnvironmentVariables` çağırın.

**Örnek**

Örnek uygulama, `AddEnvironmentVariables`bir çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır.

1. Örnek uygulamayı çalıştırın. `http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.
1. Çıktının `ENVIRONMENT`ortam değişkeni için anahtar-değer çiftini içerdiğini gözlemleyin. Değer, uygulamanın çalıştığı ortamı yansıtır, genellikle yerel olarak çalışırken `Development`.

Uygulama kısaltması tarafından oluşturulan ortam değişkenlerinin listesini tutmak için, uygulama ortam değişkenlerini filtreler. Örnek uygulamanın *Pages/Index. cshtml. cs* dosyasına bakın.

Uygulama için kullanılabilir tüm ortam değişkenlerini göstermek için, *Pages/Index. cshtml. cs* ' deki `FilteredConfiguration` aşağıdaki gibi değiştirin:

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a>Ön Ekler

Uygulamanın yapılandırmasına yüklenen ortam değişkenleri, `AddEnvironmentVariables` yöntemine bir ön ek sağlanırken filtrelenir. Örneğin, önek `CUSTOM_`ortam değişkenlerini filtrelemek için, yapılandırma sağlayıcısına öneki sağlayın:

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

Yapılandırma anahtar-değer çiftleri oluşturulduğunda ön ek çıkarılır.

Konak Oluşturucu oluşturulduğunda, ana bilgisayar yapılandırması ortam değişkenleri tarafından sağlanır. Bu ortam değişkenleri için kullanılan önek hakkında daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

**Bağlantı dizesi önekleri**

Yapılandırma API 'SI, uygulama ortamı için Azure bağlantı dizelerini yapılandırma ile ilgili dört bağlantı dizesi ortam değişkeni için özel işlem kuralları içerir. `AddEnvironmentVariables`için bir önek sağlanmazsa, tabloda gösterilen öneklere sahip ortam değişkenleri uygulamaya yüklenir.

| Bağlantı dizesi öneki | Sağlayıcı |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | Özel sağlayıcı |
| `MYSQLCONNSTR_` | [MySQL](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [SQL Server](https://www.microsoft.com/sql-server/) |

Bir ortam değişkeni keşfedildiğinde ve tabloda gösterilen dört önekle yapılandırmaya yüklendiğinde:

* Yapılandırma anahtarı, ortam değişkeni öneki kaldırılarak ve bir yapılandırma anahtarı bölümü (`ConnectionStrings`) eklenerek oluşturulur.
* Veritabanı bağlantı sağlayıcısını temsil eden yeni bir yapılandırma anahtar-değer çifti oluşturulur (`CUSTOMCONNSTR_`hariç, belirtilen sağlayıcı olmayan).

| Ortam değişkeni anahtarı | Dönüştürülen yapılandırma anahtarı | Sağlayıcı yapılandırma girişi                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | Yapılandırma girişi oluşturulmamış.                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | Anahtar: `ConnectionStrings:{KEY}_ProviderName`:<br>Değer:`MySql.Data.MySqlClient` |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | Anahtar: `ConnectionStrings:{KEY}_ProviderName`:<br>Değer:`System.Data.SqlClient`  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | Anahtar: `ConnectionStrings:{KEY}_ProviderName`:<br>Değer:`System.Data.SqlClient`  |

**Örnek**

Sunucuda özel bir bağlantı dizesi ortam değişkeni oluşturulur:

* Ad &ndash; `CUSTOMCONNSTR_ReleaseDB`
* Değer &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`

`IConfiguration` eklenen ve `_config`adlı bir alana atanmışsa, şu değeri okuyun:

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a>Dosya yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.FileConfigurationProvider>, dosya sisteminden yapılandırma yüklemeye yönelik temel sınıftır. Aşağıdaki yapılandırma sağlayıcıları belirli dosya türlerine ayrılmıştır:

* [INı yapılandırma sağlayıcısı](#ini-configuration-provider)
* [JSON yapılandırma sağlayıcısı](#json-configuration-provider)
* [XML yapılandırma sağlayıcısı](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a>INı yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider>, çalışma zamanında ıNı dosyası anahtar-değer çiftlerinden yapılandırmayı yükler.

INI dosya yapılandırmasını etkinleştirmek için bir <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>örneğinde <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> uzantısı metodunu çağırın.

İki nokta üst üste, ıNı dosya yapılandırmasındaki bir bölüm sınırlayıcısı olarak kullanılabilir.

Aşırı yüklemeler belirtmeye izin ver:

* Dosyanın isteğe bağlı olup olmadığı.
* Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.
* Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.

Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

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

Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:

* section0:key0
* section0: KEY1
* Section1: alt bölüm: anahtar
* section2: subsection0: anahtar
* section2: subsection1: anahtar

### <a name="json-configuration-provider"></a>JSON yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider>, çalışma zamanı sırasında JSON dosya anahtar-değer çiftlerinden yapılandırmayı yükler.

JSON dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> uzantısı metodunu çağırın.

Aşırı yüklemeler belirtmeye izin ver:

* Dosyanın isteğe bağlı olup olmadığı.
* Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.
* Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.

`CreateDefaultBuilder`ile yeni bir ana bilgisayar Oluşturucu başlatıldığında `AddJsonFile` otomatik olarak iki kez çağrılır. Yöntemi, yapılandırmayı şuradan yüklemek için çağrılır:

* *appSettings. json* &ndash; bu dosya ilk kez okundu. Dosyanın ortam sürümü, *appSettings. JSON* dosyası tarafından belirtilen değerleri geçersiz kılabilir.
* *appSettings. {Environment}. JSON* &ndash; dosyanın ortam sürümü [ıhostingenvironment. EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*)temel alınarak yüklenir.

Daha fazla bilgi için [varsayılan yapılandırma](#default-configuration) bölümüne bakın.

`CreateDefaultBuilder` de yüklenir:

* Ortam değişkenleri.
* Geliştirme ortamında [Kullanıcı gizli dizileri (gizli yönetici)](xref:security/app-secrets) .
* Komut satırı bağımsız değişkenleri.

JSON yapılandırma sağlayıcısı önce oluşturulur. Bu nedenle, Kullanıcı gizli dizileri, ortam değişkenleri ve komut satırı bağımsız değişkenleri, *appSettings* dosyaları tarafından ayarlanan yapılandırmayı geçersiz kılar.

Ana bilgisayarı derlerken, *appSettings. JSON* ve appSettings dışındaki dosyalar için uygulamanın yapılandırmasını belirtecek `ConfigureAppConfiguration` çağrısı yapın *. { Environment}. JSON*:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

**Örnek**

Örnek uygulama, `AddJsonFile`iki çağrı içeren konağı oluşturmak için `CreateDefaultBuilder` statik kolaylık yönteminden yararlanır:

* `AddJsonFile` ilk çağrısı *appSettings. JSON*' dan yapılandırma yükler:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* `AddJsonFile` ikinci çağrısı, appSettings 'ten yapılandırma yükler *. { Environment}. JSON*. *AppSettings için. Geliştirme. JSON* örnek uygulamada aşağıdaki dosya yüklenir:

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. Örnek uygulamayı çalıştırın. `http://localhost:5000`konumundaki uygulamaya bir tarayıcı açın.
1. Çıktı, uygulamanın ortamına göre yapılandırma için anahtar-değer çiftleri içerir. Uygulama geliştirme ortamında çalıştırılırken anahtar `Logging:LogLevel:Default` günlük düzeyi `Debug`.
1. Örnek uygulamayı üretim ortamında yeniden çalıştırın:
   1. *Properties/launchSettings. JSON* dosyasını açın.
   1. `ConfigurationSample` profilinde, `ASPNETCORE_ENVIRONMENT` ortam değişkeninin değerini `Production`olarak değiştirin.
   1. Dosyayı kaydedin ve bir komut kabuğunda `dotnet run` uygulamayı çalıştırın.
1. AppSettings içindeki ayarlar *. Development. JSON* artık *appSettings. JSON*içindeki ayarları geçersiz kılmaz. Anahtar `Logging:LogLevel:Default` için günlük düzeyi `Warning`.

### <a name="xml-configuration-provider"></a>XML yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider>, çalışma zamanında XML dosya anahtar-değer çiftinden yapılandırma yükler.

XML dosya yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> uzantısı metodunu çağırın.

Aşırı yüklemeler belirtmeye izin ver:

* Dosyanın isteğe bağlı olup olmadığı.
* Dosya değişirse yapılandırmanın yeniden yüklenip yüklenmediğini belirtir.
* Dosyaya erişmek için kullanılan <xref:Microsoft.Extensions.FileProviders.IFileProvider>.

Yapılandırma anahtar-değer çiftleri oluşturulduğunda yapılandırma dosyasının kök düğümü yok sayılır. Dosyada bir belge türü tanımı (DTD) veya ad alanı belirtmeyin.

Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

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

Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:

* section0:key0
* section0: KEY1
* section1:key0
* Section1: KEY1

Aynı öğe adını kullanan tekrarlanan öğeler, `name` özniteliği öğeleri ayırt etmek için kullanılırsa çalışır:

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

Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:

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

Önceki yapılandırma dosyası `value`aşağıdaki anahtarları yükler:

* anahtar: öznitelik
* Section: Key: özniteliği

## <a name="key-per-file-configuration-provider"></a>Dosya başına anahtar yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider>, dizin dosyalarını yapılandırma anahtar-değer çiftleri olarak kullanır. Anahtar, dosya adıdır. Değer, dosyanın içeriğini içerir. Dosya başına anahtar yapılandırma sağlayıcısı Docker barındırma senaryolarında kullanılır.

Dosya başına anahtar yapılandırması 'nı etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> uzantısı metodunu çağırın. Dosyaların `directoryPath` mutlak bir yol olmalıdır.

Aşırı yüklemeler belirtmeye izin ver:

* Kaynağı yapılandıran bir `Action<KeyPerFileConfigurationSource>` temsilcisi.
* Dizinin isteğe bağlı olup olmadığını ve dizinin yolunu belirtir.

Çift alt çizgi (`__`), dosya adlarında bir yapılandırma anahtarı sınırlayıcısı olarak kullanılır. Örneğin, `Logging__LogLevel__System` dosya adı `Logging:LogLevel:System`yapılandırma anahtarını üretir.

Uygulamanın yapılandırmasını belirtmek için konak oluştururken `ConfigureAppConfiguration` çağırın:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a>Bellek yapılandırma sağlayıcısı

<xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider>, yapılandırma anahtar-değer çiftleri olarak bellek içi koleksiyon kullanır.

Bellek içi koleksiyon yapılandırmasını etkinleştirmek için, <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>bir örneği üzerinde <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> uzantısı metodunu çağırın.

Yapılandırma sağlayıcısı bir `IEnumerable<KeyValuePair<String,String>>`başlatılabilir.

Uygulamanın yapılandırmasını belirtmek için Konağı derlerken `ConfigureAppConfiguration` çağrısı yapın.

Aşağıdaki örnekte bir yapılandırma sözlüğü oluşturulur:

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

Sözlük, yapılandırmayı sağlamak için bir `AddInMemoryCollection` çağrısıyla kullanılır:

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a>GetValue

[`ConfigurationBinder.GetValue<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) , belirli bir anahtarla yapılandırmadan tek bir değer ayıklar ve belirtilen koleksiyon olmayan türe dönüştürür. Aşırı yükleme varsayılan bir değeri kabul eder.

Aşağıdaki örnek:

* Anahtar `NumberKey`, yapılandırmadan dize değerini ayıklar. Yapılandırma anahtarlarında `NumberKey` bulunmazsa, varsayılan `99` değeri kullanılır.
* Değeri bir `int`olarak türler.
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

`section1`yalnızca anahtar-değer çiftlerini içeren bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection> döndürmek için `GetSection` çağırın ve bölüm adını sağlayın:

```csharp
var configSection = _config.GetSection("section1");
```

`configSection` bir değer, yalnızca bir anahtar ve yol yoktur.

Benzer şekilde, `section2:subsection0`anahtarlar için değerleri almak için, `GetSection` çağırın ve Bölüm yolunu sağlayın:

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

`GetSection` hiçbir şekilde `null`döndürmez. Eşleşen bir bölüm bulunamazsa boş bir `IConfigurationSection` döndürülür.

`GetSection` eşleşen bir bölüm döndürdüğünde <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> doldurulmuyor. Bölüm mevcut olduğunda bir <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> ve <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> döndürülür.

### <a name="getchildren"></a>GetChildren

`section2` üzerinde [Iconation. GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) çağrısı, şunları içeren bir `IEnumerable<IConfigurationSection>` edinir:

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

Örnek veriler verildiğinde, yapılandırma verilerinde bir `section2:subsection2` bölümü olmadığından `sectionExists` `false`.

## <a name="bind-to-an-object-graph"></a>Bir nesne grafiğine bağlama

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, tüm POCO nesne grafiğini bağlama yeteneğine sahiptir. Basit bir nesne bağlamakla birlikte yalnızca genel okuma/yazma özellikleri bağlanır.

Örnek, nesne grafı `Metadata` ve `Actors` sınıfları (*modeller/TvShow. cs*) içeren bir `TvShow` modeli içerir:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

Örnek uygulama, yapılandırma verilerini içeren bir *tvshow. xml* dosyasına sahiptir:

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

Yapılandırma, `Bind` yöntemi ile `TvShow` nesne grafiğinin tamamına bağlanır. Bağlantılı örnek, işleme için bir özelliğe atandı:

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) bağlanır ve belirtilen türü döndürür. `Get<T>`, `Bind`kullanmaktan daha uygundur. Aşağıdaki kod, `Get<T>` önceki örnekle nasıl kullanacağınızı gösterir:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a>Bir diziyi sınıfa bağlama

*Örnek uygulama, bu bölümde açıklanan kavramları gösterir.*

<xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*>, yapılandırma anahtarlarındaki dizi dizinlerini kullanan nesnelere dizileri bağlamayı destekler. Sayısal anahtar segmentini (`:0:`, `:1:`, &hellip; `:{n}:`) sunan herhangi bir dizi biçimi, bir POCO sınıf dizisine dizi bağlama özelliğine sahiptir.

> [!NOTE]
> Bağlama, kural tarafından sağlanır. Dizi bağlamayı uygulamak için özel yapılandırma sağlayıcıları gerekli değildir.

**Bellek içi dizi işleme**

Aşağıdaki tabloda gösterilen yapılandırma anahtarlarını ve değerlerini göz önünde bulundurun.

| Anahtar             | Değer  |
| :-------------: | :----: |
| dizi: girdiler: 0 | value0 |
| dizi: girdiler: 1 | Value1 |
| dizi: girdiler: 2 | Value2 |
| dizi: girdiler: 4 | value4 |
| dizi: girdiler: 5 | value5 |

Bu anahtarlar ve değerler, bellek yapılandırma sağlayıcısı kullanılarak örnek uygulamaya yüklenir:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

Dizi, Dizin &num;3 için bir değer atlıyor. Yapılandırma Bağlayıcısı, null değerleri bağlama veya bağlantılı nesnelerde null girişler oluşturma yeteneğine sahip değildir. Bu, bu diziyi bir nesneye bağlamanın sonucu gösterildiği sırada bir süre açık hale gelir.

Örnek uygulamada, bir POCO sınıfı, bağlantılı yapılandırma verilerini tutmak için kullanılabilir:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

Yapılandırma verileri nesnesine bağlanır:

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) sözdizimi de kullanılabilir ve bu da daha küçük kod elde edilebilir:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

`ArrayExample`bir örneği olan bağlantılı nesne, yapılandırmadan dizi verilerini alır.

| `ArrayExample.Entries` dizini | `ArrayExample.Entries` değeri |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1\.                            | Value1                       |
| 2                            | Value2                       |
| 3                            | value4                       |
| 4                            | value5                       |

İlişkili nesnede &num;3 dizini, `array:4` yapılandırma anahtarı ve `value4`değeri için yapılandırma verilerini tutar. Bir diziyi içeren yapılandırma verileri bağlandığında, yapılandırma anahtarlarındaki dizi dizinleri yalnızca nesne oluşturulurken yapılandırma verilerini yinelemek için kullanılır. Yapılandırma verilerinde null değer korunmaz ve bir yapılandırma anahtarlarındaki bir dizi bir veya daha fazla dizini atlamazsanız, bağlantılı nesnede NULL değerli bir giriş oluşturulmaz.

Yapılandırmada doğru anahtar-değer çiftini üreten herhangi bir yapılandırma sağlayıcısı tarafından `ArrayExample` örneğine bağlamadan önce, &num;3 dizini için eksik yapılandırma öğesi sağlanabilir. Örnek, eksik anahtar-değer çiftine sahip ek bir JSON yapılandırma sağlayıcısı içeriyorsa, `ArrayExample.Entries` tam yapılandırma dizisiyle eşleşir:

*missing_value. JSON*:

```json
{
  "array:entries:3": "value3"
}
```

`ConfigureAppConfiguration` içinde:

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

Tabloda gösterilen anahtar-değer çifti, yapılandırmaya yüklendi.

| Anahtar             | Değer  |
| :-------------: | :----: |
| dizi: girdiler: 3 | value3 |

`ArrayExample` sınıf örneği, JSON yapılandırma sağlayıcısı Dizin &num;3 ' ün girdisini içeriyorsa, `ArrayExample.Entries` dizisi değeri içerir.

| `ArrayExample.Entries` dizini | `ArrayExample.Entries` değeri |
| :--------------------------: | :--------------------------: |
| 0                            | value0                       |
| 1\.                            | Value1                       |
| 2                            | Value2                       |
| 3                            | value3                       |
| 4                            | value4                       |
| 5                            | value5                       |

**JSON dizi işleme**

JSON dosyası bir dizi içeriyorsa, sıfır tabanlı bölüm diziniyle dizi öğeleri için yapılandırma anahtarları oluşturulur. Aşağıdaki yapılandırma dosyasında, `subsection` bir dizidir:

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

JSON yapılandırma sağlayıcısı, yapılandırma verilerini aşağıdaki anahtar-değer çiftlerine okur:

| Anahtar                     | Değer  |
| ----------------------- | :----: |
| json_array: anahtar          | değer EA |
| json_array: alt bölüm: 0 | valueB |
| json_array: alt bölüm: 1 | değer EC |
| json_array: alt bölüm: 2 | Değerler |

Örnek uygulamada, yapılandırma anahtar-değer çiftlerini bağlamak için aşağıdaki POCO sınıfı kullanılabilir:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

Bağlamadan sonra, `JsonArrayExample.Key` `valueA`değerini tutar. Alt bölüm değerleri, `Subsection`POCO dizisi özelliğinde depolanır.

| `JsonArrayExample.Subsection` dizini | `JsonArrayExample.Subsection` değeri |
| :---------------------------------: | :---------------------------------: |
| 0                                   | valueB                              |
| 1\.                                   | değer EC                              |
| 2                                   | Değerler                              |

## <a name="custom-configuration-provider"></a>Özel yapılandırma sağlayıcısı

Örnek uygulama, [Entity Framework (EF)](/ef/core/)kullanarak bir veritabanından yapılandırma anahtar-değer çiftlerini okuyan temel bir yapılandırma sağlayıcısı oluşturmayı gösterir.

Sağlayıcı aşağıdaki özelliklere sahiptir:

* EF bellek içi veritabanı, tanıtım amacıyla kullanılır. Bağlantı dizesi gerektiren bir veritabanını kullanmak için, başka bir yapılandırma sağlayıcısından bağlantı dizesini sağlamak üzere ikincil `ConfigurationBuilder` uygulayın.
* Sağlayıcı bir veritabanı tablosunu başlangıçta yapılandırmaya okur. Sağlayıcı, her anahtar temelinde veritabanını sorgulayamaz.
* Değişiklik değişikliği uygulanmadı, bu nedenle uygulama başladıktan sonra veritabanının güncelleştirilmesi uygulamanın yapılandırması üzerinde hiçbir etkiye sahip değildir.

Yapılandırma değerlerini veritabanında depolamak için bir `EFConfigurationValue` varlığı tanımlayın.

*Modeller/EFConfigurationValue. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

Yapılandırılan değerleri depolamak ve erişmek için bir `EFConfigurationContext` ekleyin.

*Efconfigurationprovider/EFConfigurationContext. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<xref:Microsoft.Extensions.Configuration.IConfigurationSource>uygulayan bir sınıf oluşturun.

*Efconfigurationprovider/EFConfigurationSource. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<xref:Microsoft.Extensions.Configuration.ConfigurationProvider>devralan özel yapılandırma sağlayıcısını oluşturun. Yapılandırma sağlayıcısı boş olduğunda veritabanını başlatır.

*Efconfigurationprovider/efconfigurationprovider. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

`AddEFConfiguration` uzantısı yöntemi, yapılandırma kaynağının bir `ConfigurationBuilder`eklenmesine izin verir.

*Uzantılar/EntityFrameworkExtensions. cs*:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

Aşağıdaki kod, *program.cs*içinde özel `EFConfigurationProvider` nasıl kullanacağınızı gösterir:

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a>Başlangıç sırasında yapılandırmaya erişim

`Startup.ConfigureServices`yapılandırma değerlerine erişmek için `Startup` oluşturucusuna `IConfiguration` ekleyin. `Startup.Configure`yapılandırmaya erişmek için `IConfiguration` doğrudan yönteme ekleyin ya da oluşturucuyu kullanarak örneği kullanın:

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

Başlangıç kolaylığı yöntemlerini kullanarak yapılandırmaya erişme örneği için bkz. uygulama başlatma [. Kullanışlı yöntemler](xref:fundamentals/startup#convenience-methods).

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a>Razor Pages sayfasında veya MVC görünümünde erişim yapılandırması

Razor Pages sayfasındaki veya MVC görünümündeki yapılandırma ayarlarına erişmek için, [Microsoft. Extensions. Configuration ad alanı](xref:Microsoft.Extensions.Configuration) için bir [using yönergesi](xref:mvc/views/razor#using) ([ C# başvuru: using yönergesi](/dotnet/csharp/language-reference/keywords/using-directive)) ekleyin ve sayfa ya da görünüme <xref:Microsoft.Extensions.Configuration.IConfiguration> ekleyin.

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

<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> bir uygulama, uygulamanın `Startup` sınıfının dışında bir dış derlemeden başlangıçta bir uygulamaya iyileştirmeler eklenmesine izin verir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/platform-specific-configuration>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/options>

::: moniker-end
