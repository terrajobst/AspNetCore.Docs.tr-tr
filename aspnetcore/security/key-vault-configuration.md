---
title: ASP.NET Core, Azure Key Vault yapılandırma sağlayıcısı
author: guardrex
description: Azure Key Vault yapılandırma sağlayıcısı, çalışma zamanında yüklenen ad-değer çiftleri kullanarak bir uygulamayı yapılandırmak için kullanmayı öğrenin.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 06445eb2ecec4cf101b23a4bfe131b2c56a18f62
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090312"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>ASP.NET Core, Azure Key Vault yapılandırma sağlayıcısı

Tarafından [Luke Latham](https://github.com/guardrex) ve [Andrew Stanton-Nurse](https://github.com/anurse)

Bu belgenin nasıl kullanıldığını açıklar [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısı, Azure Key Vault gizli diziler uygulama yapılandırma değeri yüklenemiyor. Azure Key Vault, şifreleme anahtarlarını ve gizli uygulamaları ve Hizmetleri tarafından kullanılan korumanıza yardımcı olan bir bulut tabanlı bir hizmettir. Yapılandırma hassas verilere erişimi denetleme yaygın senaryolar şunlardır ve FIPS 140-2 gereksinimini karşılayan 2. düzey donanım güvenlik modülleri (HSM's) yapılandırma verileri depolarken doğrulandı. Bu özellik, daha yüksek veya ASP.NET Core 1.1 hedefleyen uygulamalar için kullanılabilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="package"></a>Paket

Sağlayıcıyı kullanmak için bir başvuru ekleyin. [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paket.

## <a name="app-configuration"></a>Uygulama yapılandırması

Sağlayıcı ile keşfedebilirsiniz [örnek uygulamalar](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Bir anahtar Kasası'kurmak ve gizli dizileri kasaya oluşturma sonra örnek uygulamaları güvenli bir şekilde gizli değerleri yapılandırmalarına yüklemek ve bunları sayfalarında görüntüler.

Sağlayıcı uygulamanın yapılandırmasına eklenen `AddAzureKeyVault` uzantısı. Örnek uygulamalar, uzantı konumundan yüklendi üç yapılandırma değerlerini kullanır. *appsettings.json* dosya.

| Uygulama ayarı    | Açıklama                    | Örnek                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure Key Vault adı           | contosovault                                 |
| `ClientId`     | Azure Active Directory Uygulama Kimliği  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory Uygulama anahtarı | g58K3dtg59o1Pa + e59v2Tx829w6VxTB2yv9sv/101di = |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a>Anahtar kasasına gizli anahtarları oluşturma ve yapılandırma değerleri (basic-sample) yükleme

1. Key vault oluşturma ve Azure Active Directory'yi (Azure AD) ayarlama, yönergeleri izleyerek uygulamayı ayarlamak [Azure anahtar kasası ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Gizli anahtar kasası kullanmaya ekleme [AzureRM anahtar kasası PowerShell modülünü](/powershell/module/azurerm.keyvault) kullanılabilir [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [Azure anahtar kasası REST API](/rest/api/keyvault/), veya [Azure portalında](https://portal.azure.com/). Gizli dizileri olarak oluşturulur *el ile* veya *sertifika* gizli dizileri. *Sertifika* gizli uygulamaları ve Hizmetleri tarafından kullanılmak üzere sertifikalar, ancak yapılandırma sağlayıcısı tarafından desteklenmiyor. Kullanmanız gereken *el ile* ad-değer çifti gizli dizileri kullanmak için yapılandırma sağlayıcısı ile oluşturmak için seçeneği.
     * Basit parolaları ad-değer çiftleri olarak oluşturulur. Azure Key Vault'a gizli dizi adları yalnızca alfasayısal karakter ve tire sınırlıdır.
     * Hiyerarşik değerleri (yapılandırma bölümlerini) kullanmak `--` (iki kısa çizgi) örnekteki ayırıcı olarak. Normalde bir alt anahtarında bölümünden sınırlandırmak için kullanılan iki nokta üst üste, [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index), gizli adlarında izin verilmez. Bu nedenle, iki kısa çizgi kullanılan ve gizli dizileri uygulama yapılandırma yüklendiğinde bir iki nokta üst üste takas.
     * İki *el ile* aşağıdaki ad-değer çiftleri ile gizli dizileri. İlk gizli dizi basit bir ad ve değer ve ikinci gizli bir bölüm ve alt anahtarında gizli anahtar adı ile gizli değer oluşturur.:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Örnek uygulamayı Azure Active Directory ile kaydedin.
   * Anahtar kasasına erişmek için uygulamayı yetkilendirme. Kullanırken `Set-AzureRmKeyVaultAccessPolicy` uygulamanın anahtar kasasına erişim yetkisi vermek için PowerShell cmdlet'i sağlar `List` ve `Get` ile gizli dizilere erişim `-PermissionsToSecrets list,get`.

2. Uygulamanın güncelleştirme *appsettings.json* değerlerini dosyasıyla `Vault`, `ClientId`, ve `ClientSecret`.
3. Kendi yapılandırma değerlerinden alır örnek uygulamayı çalıştırma `IConfigurationRoot` gizli dizi adı olarak aynı ada sahip.
   * Hiyerarşik olmayan değerleri: değeri `SecretName` ile elde edilen `config["SecretName"]`.
   * Hiyerarşik değerleri (bölümleri): kullanım `:` (virgül) gösterimi veya `GetSection` genişletme yöntemi. Yapılandırma değeri elde etmek için Bu yaklaşımlardan birini kullanın:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Uygulamayı çalıştırdığınızda, bir Web sayfası yüklü gizli değerleri gösterir:

![Azure Key Vault yapılandırma sağlayıcısı yüklenen gizli değerlerini gösteren tarayıcı penceresi](key-vault-configuration/_static/sample1.png)

## <a name="bind-an-array-to-a-class"></a>Bir dizi bir sınıfa Bağla

Sağlayıcı bir POCO diziye bağlama için bir dizi halinde yapılandırma değerlerini okuma yeteneğine sahiptir.

İki nokta üst üste içerecek şekilde anahtarları izin veren bir yapılandırma kaynaktan okunurken (`:`) ayırıcı, sayısal bir anahtar kesimi bir dizi kurulumu yapın anahtarları ayırt etmek için kullanılır (`:0:`, `:1:`,... `:{n}:`). Daha fazla bilgi için [yapılandırma: bir sınıf için bir dizi bağlama](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Azure anahtar kasası anahtarlarını bir iki nokta üst üste ayırıcı olarak kullanamazsınız. Bu konuda açıklanan yaklaşımı çift tire kullanır (`--`) hiyerarşik değerleri (bölümleri) için ayırıcı olarak. Dizi anahtarları, Azure anahtar Kasası'nda depolanır, çift çizgi ve sayısal anahtar kesimlerini (`--0--`, `--1--`,... `--{n}--`).

Aşağıdaki inceleyin [Serilog](https://serilog.net/) bir JSON dosyası tarafından sağlanan sağlayıcı yapılandırması günlüğe kaydetme. İki nesne içinde tanımlanan sabit değerler vardır `WriteTo` iki Serilog yansıtan bir dizi *havuzlarını*, günlük çıktısı hedefler açıklanmaktadır:

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

Yukarıdaki JSON dosyasında gösterilen yapılandırma çift tire kullanarak Azure Key Vault içinde depolanır (`--`) gösterimi ve sayısal segmentleri:

| Anahtar | Değer |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a>Önekli anahtar kasasına gizli anahtarları oluşturma ve yapılandırma değerleri (anahtar-adı-önek-sample) yükleme

`AddAzureKeyVault` Ayrıca uygulaması kabul eden bir aşırı sağlar `IKeyVaultSecretManager`, nasıl anahtar kasa gizli dizilerini denetlemenize olanak sağlayan yapılandırma anahtarları dönüştürülür. Örneğin, uygulama başlatma sırasında sağladığınız bir önek değere göre gizli değerlerini yüklemek için arabirim uygulayabilir. Bu, örneğin, gizli dizileri uygulama sürümüne yüklemek için sağlar.

> [!WARNING]
> Ön ekleri için birden fazla uygulama gizli dizilerini aynı anahtar kasasının yerleştirmek için veya çevre gizli dizileri yerleştirmek için anahtar kasası parolaları kullanmayın (örneğin, *geliştirme* karşı *üretim* gizli Diziler) aynı içine Kasa. Farklı uygulama ve geliştirme veya üretim ortamları ayrı anahtar kasalarını yüksek düzeyde güvenlik için uygulama ortamları ayırmak için kullanmanızı öneririz.

İkinci örnek uygulaması kullanarak, bir gizli dizi için anahtar kasasını oluşturduğunuz `5000-AppSecret` (anahtar kasası gizli dizi adları nokta izin verilmiyor) temsil eden bir uygulama gizli anahtarı için uygulamanızın 5.0.0.0 sürümü. Gizli dizi için oluşturduğunuz başka bir sürümü için 5.1.0.0, `5100-AppSecret`. Her uygulama sürümü kendi gizli değer yapılandırmasına yükler `AppSecret`, çıkarma sürümü gibi gizli dizi yükler. Örnek'ın uygulama aşağıda gösterilmiştir:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load` Yöntemi sürüm önekine sahip olanları bulmak için kasa gizli dizilerini yinelenen bir sağlayıcı algoritması tarafından çağrılır. Ne zaman bir sürüm ön eki bulundu ile `Load`, algoritmasını `GetKey` gizli dizi adı yapılandırma adını döndürmek için yöntemi. Parolanın adı sürüm önekten kapalı kaldırır ve gizli dizi adı yüklenmesi için kalan ad-değer çiftleri uygulamanın yapılandırmasını döndürür.

Bu yaklaşım uyguladığınızda:

1. Anahtar kasası gizli dizileri yüklenir.
2. Dize gizliliğini `5000-AppSecret` eşleştirilir.
3. Sürüm `5000` (ile dash), anahtar adına bırakarak yapılandırıldıktan `AppSecret` gizli değer ile uygulamanın yapılandırma yüklenemedi.

> [!NOTE]
> Ayrıca kendi sağlayabilirsiniz `KeyVaultClient` uygulamasına `AddAzureKeyVault`. Özel bir istemci sağlama istemci yapılandırma sağlayıcısı ve uygulamanızın diğer bölümleri arasındaki tek bir örneğini paylaşmanıza olanak tanır.

1. Key vault oluşturma ve Azure Active Directory'yi (Azure AD) ayarlama, yönergeleri izleyerek uygulamayı ayarlamak [Azure anahtar kasası ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Gizli anahtar kasası kullanmaya ekleme [AzureRM anahtar kasası PowerShell modülünü](/powershell/module/azurerm.keyvault) kullanılabilir [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [Azure anahtar kasası REST API](/rest/api/keyvault/), veya [Azure portalında](https://portal.azure.com/). Gizli dizileri olarak oluşturulur *el ile* veya *sertifika* gizli dizileri. *Sertifika* gizli uygulamaları ve Hizmetleri tarafından kullanılmak üzere sertifikalar, ancak yapılandırma sağlayıcısı tarafından desteklenmiyor. Kullanmanız gereken *el ile* ad-değer çifti gizli dizileri kullanmak için yapılandırma sağlayıcısı ile oluşturmak için seçeneği.
     * Hiyerarşik değerleri (yapılandırma bölümlerini) kullanmak `--` (iki kısa çizgi) ayırıcı olarak.
     * İki *el ile* aşağıdaki ad-değer çiftleri sahip gizli diziler:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Örnek uygulamayı Azure Active Directory ile kaydedin.
   * Anahtar kasasına erişmek için uygulamayı yetkilendirme. Kullanırken `Set-AzureRmKeyVaultAccessPolicy` uygulamanın anahtar kasasına erişim yetkisi vermek için PowerShell cmdlet'i sağlar `List` ve `Get` ile gizli dizilere erişim `-PermissionsToSecrets list,get`.

2. Uygulamanın güncelleştirme *appsettings.json* değerlerini dosyasıyla `Vault`, `ClientId`, ve `ClientSecret`.
3. Kendi yapılandırma değerlerinden alır örnek uygulamayı çalıştırma `IConfigurationRoot` önekli gizli dizi adı olarak aynı ada sahip. Bu örnekte, için sağlanan uygulamanın sürüm önekidir `PrefixKeyVaultSecretManager` Azure Key Vault yapılandırma sağlayıcısı eklediğiniz zaman. Değeri `AppSecret` ile elde edilen `config["AppSecret"]`. Yüklenen değer uygulama tarafından oluşturulan Web sayfası gösterilmektedir:

   ![Uygulamanın sürümü 5.0.0.0 olduğunda Azure Key Vault yapılandırma sağlayıcısı yüklenen bir gizli değer gösteren tarayıcı penceresi](key-vault-configuration/_static/sample2-1.png)

4. Proje dosyasından Uygulama derlemesinde sürümünü değiştirmek `5.0.0.0` için `5.1.0.0` ve uygulamayı yeniden çalıştırın. Bu süre, döndürülen gizli değerdir `5.1.0.0_secret_value`. Yüklenen değer uygulama tarafından oluşturulan Web sayfası gösterilmektedir:

   ![Uygulamanın sürümü 5.1.0.0 olduğunda Azure Key Vault yapılandırma sağlayıcısı yüklenen bir gizli değer gösteren tarayıcı penceresi](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a>ClientSecret erişimi denetleme

Kullanım [gizli dizi Yöneticisi aracını](xref:security/app-secrets) korumak için `ClientSecret` proje kaynak ağacınız dışında. Gizli dizi Yöneticisi ile uygulama gizli anahtarlarının belirli bir proje ile ilişkilendirmek ve birden çok projede paylaşın.

Sertifikalar'ı destekleyen bir ortamda bir .NET Framework uygulama geliştirirken, Azure anahtar Kasası'na bir X.509 sertifikası ile kimlik doğrulaması yapabilir. X.509 sertifikasının özel anahtar, işletim sistemi tarafından yönetilir. Daha fazla bilgi için [bir sertifika yerine istemci gizli anahtarı ile kimlik doğrulama](/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Kullanım `AddAzureKeyVault` kabul eden aşırı bir `X509Certificate2` (`_env` aşağıdaki örnekte:

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reload-secrets"></a>Gizli dizileri yeniden yükleyin

Gizli dizileri kadar önbelleğe alınır `IConfigurationRoot.Reload()` çağrılır. Süresi dolan, devre dışı bırakıldı ve güncelleştirilmiş gizli anahtarları key vault'ta değil kadar uygulama tarafından dikkate `Reload` yürütülür.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Devre dışı bırakılmış ve süresi dolan gizli diziler

Devre dışı bırakılmış ve süresi dolan gizli diziler throw bir `KeyVaultClientException`. Uygulamanızı oluşturma gelen önlemek için uygulamanızı değiştirin veya devre dışı bırakılmış/süresi dolmuş gizli anahtarı güncelleştirme.

## <a name="troubleshoot"></a>Sorun giderme

Yapılandırma Sağlayıcısı'nı kullanarak yüklemek uygulama başarısız olduğunda, bir hata iletisi yazılan [ASP.NET Core günlüğü altyapı](xref:fundamentals/logging/index). Aşağıdaki koşullar yapılandırma yüklenmesini engeller.

* Uygulamayı Azure Active Directory'de doğru şekilde yapılandırılmamış.
* Anahtar kasası, Azure anahtar Kasası'nda mevcut değil.
* Uygulama, anahtar kasasına erişmek için yetkili değil.
* Erişim İlkesi içermez `Get` ve `List` izinleri.
* Anahtar Kasası'nda yapılandırma verileri (ad-değer çifti) yanlış, eksik, devre dışı veya süresi dolmuş olarak adlandırılır.
* Uygulama, yanlış anahtar kasası adına sahip (`Vault`), Azure AD uygulama kimliği (`ClientId`), veya Azure AD anahtarı (`ClientSecret`).
* Azure AD anahtarı (`ClientSecret`) süresi doldu.
* Yapılandırma anahtarı (ad), uygulamayı yüklemeye çalıştığınız değeri geçersiz.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Anahtar kasası](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Anahtar kasası belgeleri](/azure/key-vault/)
* [Azure anahtar kasası için nasıl oluşturma ve aktarma HSM korumalı anahtarlar](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient sınıfı](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
