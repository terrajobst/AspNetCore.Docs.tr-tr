---
uid: config-builder
title: ASP.NET için yapılandırma oluşturucular
author: rick-anderson
description: Dış kaynaklardan web.config değerleri dışındaki kaynaklardan yapılandırma verilerini almayı öğrenin.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: d5a3916c3df9778d14be80342bafbc3456a69a03
ms.sourcegitcommit: f2d14a7518d6ee51aca9333818ac1276e7b5ecef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50134541"
---
# <a name="configuration-builders-for-aspnet"></a>ASP.NET için yapılandırma oluşturucular

Tarafından [Stephen Molloy](https://github.com/StephenMolloy) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Yapılandırma Oluşturucular, dış kaynaklardan yapılandırma değerlerini almak ASP.NET uygulamaları için modern ve Çevik bir mekanizma sağlar.

Yapılandırma oluşturucular:

* .NET Framework 4.7.1 kullanılabilir ve sonrasında bulunmaktadır.
* Yapılandırma değerleri okumak için esnek bir mekanizma sağlar.
* Bir kapsayıcıya Taşı ve bulut odaklı bir ortam gibi bazı uygulamalarının ihtiyaçlarını temel adres.
* Yapılandırma verileri (örneğin, Azure Key Vault ve ortam değişkenleri) önceden kullanılamayan kaynaklardan çizerek geliştirmek için .NET yapılandırma sistemini kullanılabilir.

## <a name="keyvalue-configuration-builders"></a>Anahtar/değer yapılandırma oluşturucular

Yapılandırma oluşturucular tarafından işlenebilen yaygın bir senaryo, bir anahtar/değer desenler izleyen yapılandırma bölümlerinin temel anahtar/değer değiştirme mekanizma sağlamaktır. .NET Framework kavramı ConfigurationBuilders, belirli yapılandırma bölümlerini veya düzenleri için sınırlı değildir. Ancak, birçok yapılandırma oluşturucular içinde `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders) anahtar/değer desen içindeki iş.

## <a name="keyvalue-configuration-builders-settings"></a>Anahtar/değer yapılandırma oluşturucular ayarları

Tüm anahtar/değer yapılandırma oluşturucular aşağıdaki ayarları uygulamak `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Mod

Yapılandırma oluşturucular bir dış bilgi kaynağı olan anahtar/değer yapılandırma sistemi seçilen anahtar/değer öğeleri doldurmak için kullanın. Özellikle, `<appSettings/>` ve `<connectionStrings/>` bölümleri yapılandırma oluşturucular özel olarak değerlendirilmesi alırsınız. Oluşturucular, üç modlarında çalışır:

* `Strict` -Varsayılan modu. Bu modda, yapılandırma Oluşturucu yalnızca iyi bilinen bir anahtar/değer merkezli yapılandırma bölümleri üzerinde çalışır. `Strict` Her anahtar bölümü modunu numaralandırır. Dış kaynak olarak eşleşen bir anahtar bulunursa:

   * Yapılandırma oluşturucular elde edilen yapılandırma bölümündeki değeri bir dış kaynaktan değeriyle değiştirin.
* `Greedy` -Bu modda yakından ilgilidir `Strict` modu. Orijinal yapılandırmada bulunan anahtarlarına sınırlı kalmak yerine:

  * Yapılandırma oluşturucular bir dış kaynaktan tüm anahtar/değer çiftleri elde edilen yapılandırma bölümüne ekler.

* `Expand` -Ham XML yapılandırma bölümü nesnesine ayrıştırılmadan önce çalışır. Bu, bir dizedeki belirteçlerin bir genişletme olarak düşünülebilir. Ham XML dizesi desenle eşleşen herhangi bir bölümünü `${token}` belirteci genişletme bir adaydır. Karşılık gelen değer, dış kaynak bulunamadı, belirteç değiştirilmez. Oluşturucular bu modda, bunlarla sınırlı değildir `<appSettings/>` ve `<connectionStrings/>` bölümler.

Aşağıdaki biçimlendirmeden *web.config* sağlayan [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) içinde `Strict` modu:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Aşağıdaki kod okuma `<appSettings/>` ve `<connectionStrings/>` önceki gösterilen *web.config* dosyası:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Yukarıdaki kod özellik değerlerini ayarlar:

* Değerler *web.config* dosyası anahtarları ortam değişkenleri ayarlanmamış.
* Ortam değerlerini değişken, eğer ayarlayın.

Örneğin, `ServiceID` içerir:

* "ServiceID value" web.config öğesinden, ortam değişkenini `ServiceID` ayarlı değil.
* Değerini `ServiceID` ortam değişkeni, eğer ayarlayın.

Aşağıdaki görüntüde `<appSettings/>` önceki anahtarları/değerleri *web.config* ortam Düzenleyicisi'nde dosya kümesi:

![ortam Düzenleyicisi](config-builder/static/env.png)

Not: Çıkın ve ortam değişkenleri içindeki değişiklikleri görmek için Visual Studio'yu yeniden başlatmanız gerekebilir.

### <a name="prefix-handling"></a>Ön işleme

Anahtar ön ayarı anahtarlarının çünkü basitleştirebilirsiniz:

* .NET Framework yapılandırma karmaşık ve iç içe geçmiş ' dir.
* Dış anahtar/değer kaynakları, temel ve düz yapısı tarafından yaygın olarak. Örneğin, ortam değişkenleri içe değil.

Hem de ekleme için aşağıdaki yaklaşımlardan birini kullanın `<appSettings/>` ve `<connectionStrings/>` içine ortam değişkenlerini aracılığıyla yapılandırma:

* İle `EnvironmentConfigBuilder` varsayılan `Strict` modu ve yapılandırma dosyasında uygun anahtar adları. Önceki kod ve biçimlendirme, bu yaklaşım alır. Yapabilecekleriniz bu yaklaşımı kullanarak **değil** anahtarları hem de aynı şekilde adlandırılmış sahiptir `<appSettings/>` ve `<connectionStrings/>`.
* İki kullanın `EnvironmentConfigBuilder`s'te `Greedy` farklı öneklerle modu ve `stripPrefix`. Bu yaklaşımda, uygulamanın okuyabileceği `<appSettings/>` ve `<connectionStrings/>` yapılandırma dosyasını güncelleştirmek gerek olmadan. Sonraki bölümde [stripPrefix](#stripprefix), bunun nasıl yapılacağı gösterilmektedir.
* İki kullanın `EnvironmentConfigBuilder`s'te `Greedy` farklı öneklerle modu. Bu yaklaşımla ön eke göre anahtar adları farklı olmalıdır gibi yinelenen anahtar adlara sahip olamaz.  Örneğin:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Önceki biçimlendirme, aynı düz anahtar/değer kaynak yapılandırması için iki farklı bölümlerde doldurmak için kullanılabilir.

Aşağıdaki görüntüde `<appSettings/>` ve `<connectionStrings/>` önceki anahtarları/değerleri *web.config* ortam Düzenleyicisi'nde dosya kümesi:

![ortam Düzenleyicisi](config-builder/static/prefix.png)

Aşağıdaki kod okuma `<appSettings/>` ve `<connectionStrings/>` anahtarları/değerleri önceki yer alan *web.config* dosyası:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Yukarıdaki kod özellik değerlerini ayarlar:

* Değerler *web.config* dosyası anahtarları ortam değişkenleri ayarlanmamış.
* Ortam değerlerini değişken, eğer ayarlayın.

Örneğin, önceki'yi kullanarak *web.config* dosya, anahtarları/değerleri önceki ortam Düzenleyicisi resmi ve önceki kod, aşağıdaki değerleri ayarlayın:

|  Anahtar              | Değer |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | Env değişkenlerinden AppSetting_ServiceID|
|    AppSetting_default            | Env AppSetting_default değeri |
|       ConnStr_default         | Env gelen ConnStr_default val|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: boolean, varsayılan olarak `false`. 

Önceki XML işaretlemesini bağlantı dizeleri uygulama ayarlarından ayırır, ancak tüm anahtarları gerektirir *web.config* dosyasını belirtilen önek kullanın. Örneğin, ön eki `AppSetting` eklenmelidir `ServiceID` anahtarı ("AppSetting_ServiceID"). İle `stripPrefix`, ön eki kullanılmaz *web.config* dosya. Önek yapılandırma Oluşturucu kaynağı (örneğin, ortamdaki.) gereklidir Çoğu geliştirici kullanacağı tahmin `stripPrefix`.

Uygulamalar genellikle önek Şerit. Aşağıdaki *web.config* önek kaldırır:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

Önceki içinde *web.config* dosyası `default` anahtardır hem de `<appSettings/>` ve `<connectionStrings/>`.

Aşağıdaki görüntüde `<appSettings/>` ve `<connectionStrings/>` önceki anahtarları/değerleri *web.config* ortam Düzenleyicisi'nde dosya kümesi:

![ortam Düzenleyicisi](config-builder/static/prefix.png)

Aşağıdaki kod okuma `<appSettings/>` ve `<connectionStrings/>` anahtarları/değerleri önceki yer alan *web.config* dosyası:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Yukarıdaki kod özellik değerlerini ayarlar:

* Değerler *web.config* dosyası anahtarları ortam değişkenleri ayarlanmamış.
* Ortam değerlerini değişken, eğer ayarlayın.

Örneğin, önceki'yi kullanarak *web.config* dosya, anahtarları/değerleri önceki ortam Düzenleyicisi resmi ve önceki kod, aşağıdaki değerleri ayarlayın:

|  Anahtar              | Değer |
| ----------------- | ------------ |
|     ServiceId           | Env değişkenlerinden AppSetting_ServiceID|
|    default            | Env AppSetting_default değeri |
|    default         | Env gelen ConnStr_default val|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Dize, varsayılan olarak `@"\$\{(\w+)\}"`

`Expand` Oluşturucular davranışını nasıl görüneceğini belirteçleri için ham XML arar `${token}`. Arama varsayılan normal ifade ile yapılır `@"\$\{(\w+)\}"`. Eşleşen karakter kümesini `\w` XML ve birçok yapılandırma kaynaklarını izin verilenden daha katı. Kullanım `tokenPattern` ne zaman daha fazla karakter daha `@"\$\{(\w+)\}"` belirteç adı gereklidir.

`tokenPattern`: Dize:

* Geliştiricilerin belirteci eşleştirmek için kullanılan normal ifade değiştirme olanak tanır.
* İyi biçimlendirilmiş tehlikeli olmayan bir normal ifade olduğundan emin olmak için doğrulama gerçekleştirilir.
* Bir yakalama grubu içermelidir. Tüm belirteç tüm normal ifade ile eşleşmelidir. İlk yakalama yapılandırma kaynağında aramak için belirteç adı olmalıdır.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Yapılandırma oluşturucular Microsoft.Configuration.ConfigurationBuilders içinde

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Yapılandırma oluşturucular çalıştırmaktır.
* Değerleri ortamından okur.
* Herhangi bir ek yapılandırma seçeneği yok.
* `name` Rastgele öznitelik değeri.

**Not:** bir Windows kapsayıcı ortamında, çalışma zamanında ayarlama değişkenleri yalnızca EntryPoint işlem ortamına. Uygulamaları, hizmet olarak çalıştırmak veya bir giriş noktası olmayan işlem değil çekme bu değişkenleri, aksi takdirde kapsayıcıdaki bir mekanizma aracılığıyla eklenmiş sürece. İçin [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-kapsayıcılara, geçerli sürümü [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) bu işleme *DefaultAppPool* yalnızca. Diğer Windows tabanlı kapsayıcı çeşitleri, giriş noktası olmayan işlemler için kendi ekleme mekanizması geliştirmeniz gerekebilir.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Hiçbir zaman parolaları, hassas bağlantı dizelerini ve diğer hassas verileri kaynak kodundaki. Üretim gizli dizileri kullanılmamalıdır geliştirme veya test için.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Bu yapılandırma oluşturucusu benzer bir özellik sağlar [ASP.NET Core gizli dizi Yöneticisi](/aspnet/core/security/app-secrets).

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) .NET Framework projelerinde kullanılabilir, ancak bir gizli dizi dosyası belirtilmelidir. Alternatif olarak, tanımlayabileceğiniz `UserSecretsId` projesi özelliğinde dosya ve doğru konumda okuma için ham gizli dizi dosyası oluşturun. Dış bağımlılıkları projenize dışında tutmak için gizli olarak biçimlendirilmiş bir XML dosyasıdır. Bir uygulama ayrıntısı olan XML biçimlendirmesi ve biçimi üzerinde kullanılmamalıdır. Paylaşmanız gerekiyorsa bir *secrets.json* dosya ile .NET Core projelerinde, kullanmayı [SimpleJsonConfigBuilder](#simplejsonconfig). `SimpleJsonConfigBuilder` Biçim .NET Core, bir uygulama ayrıntısı değişebilir düşünülmelidir.

Yapılandırma öznitelikleri için `UserSecretsConfigBuilder`:

* `userSecretsId` -Bu, bir XML gizli dizi dosyası tanımlamak için tercih edilen yöntemdir. Kullanan .NET Core için benzer çalışır bir `UserSecretsId` proje özelliği bu tanımlayıcıyı depolamak için. Dize benzersiz olmalıdır, bu bir GUID olması gerekmez. Bu öznitelik ile `UserSecretsConfigBuilder` iyi bilinen bir yerel konumda arayın (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) bu tanımlayıcısına ait olan bir gizli dizi dosyası için.
* `userSecretsFile` Gizli dizileri içeren dosyayı belirtmek isteğe bağlı öznitelik. `~` Karakter başına uygulama kökü başvuru için kullanılabilir. Bu öznitelik veya `userSecretsId` özniteliği gereklidir. Her ikisi de belirtilirse, `userSecretsFile` önceliklidir.
* `optional`: boolean, varsayılan değer `true` -gizli dizi dosyası bulunamazsa özel durum engeller. 
* `name` Rastgele öznitelik değeri.

Gizli dizi dosyası aşağıdaki biçime sahiptir:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) depolanan değerlerini okur [Azure anahtar kasası](/azure/key-vault/key-vault-whatis).

`vaultName` olduğundan (kasa adı) ya da kasaya bir URI gerekiyor. Diğer öznitelikleri bağlanmak için hangi kasası hakkında daha fazla denetime izin ver, ancak uygulama ile birlikte çalışan bir ortamda çalışır durumda değilse, yalnızca gerekli olan `Microsoft.Azure.Services.AppAuthentication`. Azure Hizmetleri kimlik doğrulaması kitaplığı otomatik olarak bağlantı bilgilerini Yürütme Ortamı'ndan mümkünse almak için kullanılır. Otomatik olarak geçersiz bir bağlantı dizesi sağlayarak bağlantı bilgilerini almak.

* `vaultName` -Gerekli if `uri` içinde sağlanmadı. Anahtar/değer çiftleri okunacağı Azure aboneliğinizdeki kasasının adını belirtir.
* `connectionString` -Bir bağlantı dizesi tarafından kullanılabilmesine [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -Diğer Key Vault sağlayıcılarıyla belirtilen bağlandığı `uri` değeri. Belirtilmezse, Azure (`vaultName`) kasası sağlayıcısıdır.
* `version` -Azure Key Vault gizli dizileri için bir sürüm oluşturma özelliği sağlar. Varsa `version` belirtilirse, oluşturucu yalnızca bu sürümüyle eşleşen gizli anahtarları alır.
* `preloadSecretNames` -Varsayılan olarak, bu oluşturucu querys **tüm** başlatıldığında adları anahtar kasasında anahtar. Tüm anahtar değerlerin okuma önlemek için bu öznitelik ayarlanan `false`. Bu ayar `false` gizli bir anda okur. Bir kerede yararlı kasa "Get" erişim sağlar, ancak değil "List" erişim için gizli dizileri okunuyor. **Not:** kullanırken `Greedy` modu `preloadSecretNames` olmalıdır `true` (varsayılan)

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) değerler kaynağı olarak bir dizin dosyalarını kullanan bir temel yapılandırma oluşturucusu. Bir dosyanın adını anahtarı ve içeriği değerlerdir. Bu yapılandırma Oluşturucu düzenlenmiş kapsayıcı ortamında çalışırken yararlı olabilir. Docker Swarm ve Kubernetes gibi sistemleri `secrets` kendi düzenlenmiş windows kapsayıcılarına bu dosya başına anahtar şekilde.

Öznitelik ayrıntıları:

* `directoryPath` -Gerekli. Değerleri için aranacak bir yolunu belirtir. Gizli dizileri depolanır Windows için docker *C:\ProgramData\Docker\secrets* varsayılan dizin.
* `ignorePrefix` -Bu ön ekine sahip dosyalar hariç tutulur. "Yoksay." varsayılan kullanır.
* `keyDelimiter` -Varsayılan değer `null`. Belirtilmişse yapılandırma Oluşturucu anahtar adları bu sınırlayıcı ile oluşturma dizinin birden çok düzeyi erişir. Bu değer ise `null`, yapılandırma Oluşturucu yalnızca en üst düzeyinde dizinin arar.
* `optional` -Varsayılan değer `false`. Kaynak dizin yoksa yapılandırma Oluşturucu hatalara neden belirtir.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Hiçbir zaman parolaları, hassas bağlantı dizelerini ve diğer hassas verileri kaynak kodundaki. Üretim gizli dizileri kullanılmamalıdır geliştirme veya test için.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

.NET core projeleri, sık yapılandırmanız için JSON dosyalarını kullanın. [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) Oluşturucu .NET Framework kullanılacak .NET Core JSON dosyaları sağlar. Bu yapılandırma oluşturucusu olan belirli bir anahtar/değer .NET Framework yapılandırma alanlarının bir düz anahtar/değer kaynağından temel bir eşleme sağlar. Bu yapılandırma oluşturucusu mu **değil** hiyerarşik yapılandırmalarını sağlayın. JSON yedekleme dosyası, karmaşık bir hiyerarşik nesnesi değil bir sözlüğe benzerdir. Çok düzeyli, hiyerarşik bir dosya kullanılabilir. Bu sağlayıcı `flatten`s her kullanarak düzeyinde özellik adı ekleyerek derinliği `:` sınırlayıcı olarak.

Öznitelik ayrıntıları:

* `jsonFile` -Gerekli. Okunacak JSON dosyasını belirtir. `~` Karakter başında uygulama kök başvurmak için kullanılabilir.
* `optional` -Boolean, varsayılan değer: `true`. Engelleyen bir JSON dosyası bulunamazsa özel durumları atma.
* `jsonMode` - `[Flat|Sectional]`. `Flat` varsayılandır. Zaman `jsonMode` olduğu `Flat`, JSON dosyası bir düz tek anahtar/değer kaynağıdır. `EnvironmentConfigBuilder` Ve `AzureKeyVaultConfigBuilder` düz tek anahtar/değer kaynaklardır. Zaman `SimpleJsonConfigBuilder` yapılandırılan `Sectional` modu:

  * JSON dosyası, yalnızca en üst düzeyde birden fazla sözlüklerine kavramsal olarak ayrılmıştır.
  * Her sözlük yalnızca kendilerine iliştirilmiş en üst düzey özellik adıyla eşleşen bir yapılandırma bölümü uygulanır. Örneğin:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Bir özel anahtar/değer yapılandırma Oluşturucu uygulama

Yapılandırma oluşturucular gereksinimlerinizi karşılamıyorsa, özel bir tane yazabilirsiniz. `KeyValueConfigBuilder` Temel sınıf değiştirme modları ve çoğu ön eki sorunları işler. Yalnızca uygulama projesinde gerekir:

* Temel sınıfından devralır ve anahtar/değer çiftleri ile temel bir kaynağı uygulama `GetValue` ve `GetAllValues`:
* Ekleme [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) projeye.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

`KeyValueConfigBuilder` Taban sınıfı sağlar iş ve tutarlı bir davranış çok anahtar/değer arasında yapılandırma oluşturucular.

## <a name="additional-resources"></a>Ek kaynaklar

* [Yapılandırma oluşturucular GitHub deposu](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [.NET kullanarak Azure Key Vault hizmetten hizmete kimlik doğrulaması](/azure/key-vault/service-to-service-authentication#connection-string-support)
