---
title: ASP.NET core'da Azure anahtar kasası yapılandırma sağlayıcısı
author: guardrex
description: Azure Key Vault yapılandırma sağlayıcısı, çalışma zamanında yüklenen ad-değer çiftleri kullanarak bir uygulamayı yapılandırmak için kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/13/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 78c63cf135ca92f0b5f6c6828b2ae34a44a7b36c
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621028"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>ASP.NET core'da Azure anahtar kasası yapılandırma sağlayıcısı

Tarafından [Luke Latham](https://github.com/guardrex) ve [Andrew Stanton-Nurse](https://github.com/anurse)

Bu belgenin nasıl kullanıldığını açıklar [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısı, Azure Key Vault gizli diziler uygulama yapılandırma değeri yüklenemiyor. Azure Key Vault, şifreleme anahtarlarını ve gizli uygulamaları ve Hizmetleri tarafından kullanılan koruma içinde yönetmenize yardımcı olan bir bulut tabanlı bir hizmettir. Azure Key Vault ile ASP.NET Core uygulamaları kullanmaya yönelik yaygın senaryolar şunlardır:

* Yapılandırma hassas verilere erişimi denetleme.
* FIPS gereksinimini karşılayan 140-2 Düzey 2 donanım güvenlik modülleri (HSM's) yapılandırma verileri depolarken doğrulandı.

Bu senaryo, ASP.NET Core 2.1 hedefleyen uygulamalar için veya sonraki kullanılabilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Paketler

Azure Key Vault yapılandırma sağlayıcısı kullanmak için bir paket başvurusu ekleme [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paket.

Benimsemeye [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview) senaryosu için bir paket başvurusu ekleme [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) paket.

> [!NOTE]
> En son kararlı sürümünü yazma zamanında `Microsoft.Azure.Services.AppAuthentication`, sürüm `1.0.3`, için destek sağlar [sistem tarafından atanan kimlikleri yönetilen](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-worka-namehow-does-it-worka). Destek *kullanıcı tarafından atanan kimlikleri yönetilen* kullanılabilir `1.2.0-preview2` paket. Bu konu, sistem tarafından yönetilen kimlikleri kullanımını gösterir ve sağlanan örnek uygulama sürümünü kullanan `1.0.3` , `Microsoft.Azure.Services.AppAuthentication` paket.

## <a name="sample-app"></a>Örnek uygulama

Örnek uygulama tarafından belirlenen iki moddan birini çalıştıran `#define` en üstündeki deyimi *Program.cs* dosyası:

* `Certificate` &ndash; Bir Azure anahtar kasası istemci kimliği ve X.509 sertifikası için erişim gizli dizilerini Azure Key Vault'ta depolanan kullanımını gösterir. Örnek bu sürümü, Azure App Service veya ASP.NET Core uygulaması hizmet herhangi bir konağa dağıtılan herhangi bir yerden çalıştırılabilir.
* `Managed` &ndash; Nasıl kullanılacağını gösteren [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview) uygulamanın kod veya yapılandırma depolanan kimlik bilgileri olmadan Azure anahtar kasası uygulama Azure AD kimlik doğrulaması ile kimlik doğrulaması için. Yönetilen kimlik doğrulamaya kullanırken, bir Azure AD uygulama kimliği ve parolası (gizli) gerekli değildir. `Managed` Sürümünü Azure'a dağıtılması gerekir. Sunulan yönergeleri [Azure kaynakları için yönetilen kimlikleri kullanmak](#use-managed-identities-for-azure-resources) bölümü.

Önişlemci yönergeleri kullanarak örnek bir uygulama yapılandırma hakkında daha fazla bilgi için (`#define`), bkz: <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Geliştirme ortamındaki gizli depolama

Gizli dizileri kullanarak yerel olarak ayarlamak [gizli dizi Yöneticisi aracını](xref:security/app-secrets). Örnek uygulama geliştirme ortamında yerel makinede çalıştırıldığında, gizli dizileri yüklenen yerel gizli dizi Yöneticisi deposu.

Gizli dizi Yöneticisi Aracı gerektiren bir `<UserSecretsId>` uygulamanın proje dosyasında özelliği. Özellik değerini ayarlayın (`{GUID}`) benzersiz bir GUID için:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

Gizli dizileri ad-değer çiftleri olarak oluşturulur. Hiyerarşik değerleri (yapılandırma bölümlerini) kullanan bir `:` (virgül) ayırıcı olarak [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index) anahtar adları.

Gizli dizi Yöneticisi, projenin içeriği köke açılmış bir komut kabuğundan kullanılır nerede `{SECRET NAME}` adıdır ve `{SECRET VALUE}` değerdir:

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

Bir projenin içerik kökünden örnek uygulaması için gizli dizileri ayarlamak için komut kabuğunda aşağıdaki komutları yürütün:

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Ne zaman bu gizli dizileri depolanır Azure anahtar Kasası'nda [Azure Key Vault ile üretim ortamında gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) bölümünde `_dev` soneki değiştiğinde `_prod`. Sonek uygulamanın çıkışında kaynak yapılandırma değerlerini gösteren görsel bir ipucu sağlar.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Azure Key Vault ile üretim ortamında gizli depolama

Tarafından sağlanan yönergeleri [hızlı başlangıç: Ayarlayın ve Azure CLI kullanarak Azure Key Vault gizli dizi alma](/azure/key-vault/quick-create-cli) konuda özetlenir burada bir Azure Key Vault oluşturma ve örnek uygulama tarafından kullanılan gizli dizileri depolamak için. Daha fazla ayrıntı için konusuna bakın.

1. Aşağıdaki yöntemlerden birini kullanarak açık Azure Cloud Shell'i [Azure portalında](https://portal.azure.com/):

   * Seçin **deneyin** bir kod bloğunun sağ üst köşedeki. Arama dizesi "Azure CLI" metin kutusunu kullanın.
   * Cloud Shell ile kendi tarayıcınızda açın **Cloud Shell'i Başlat** düğmesi.
   * Seçin **Cloud Shell** düğmesi Azure portalının sağ üst köşesinde yer alan menüdeki.

   Daha fazla bilgi için [Azure komut satırı arabirimi (CLI)](/cli/azure/) ve [Azure Cloud shell'e genel bakış](/azure/cloud-shell/overview).

1. İle önceden doğrulanmış değil, oturum `az login` komutu.

1. Aşağıdaki komutla bir kaynak grubu oluşturmak burada `{RESOURCE GROUP NAME}` yeni kaynak grubu için kaynak grubu adı ve `{LOCATION}` Azure bölgesi (veri merkezi):

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Aşağıdaki komutla, kaynak grubundaki anahtar kasası oluşturma yeri `{KEY VAULT NAME}` yeni anahtar kasası adı ve `{LOCATION}` Azure bölgesi (veri merkezi):

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Gizli anahtar Kasası'nda ad-değer çiftleri olarak oluşturun.

   Azure Key Vault'a gizli dizi adları yalnızca alfasayısal karakter ve tire sınırlıdır. Hiyerarşik değerleri (yapılandırma bölümlerini) kullanmak `--` (iki kısa çizgi) ayırıcı olarak. Normalde bir alt anahtarında bölümünden sınırlandırmak için kullanılan iki nokta üst üste, [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index), anahtar kasası gizli adlarında izin verilmez. Bu nedenle, iki kısa çizgi kullanılan ve gizli dizileri uygulama yapılandırma yüklendiğinde bir iki nokta üst üste takas.

   Örnek uygulama ile kullanmak için aşağıdaki gizli diziler var. Değerler bir `_prod` bunları ayırt etmek için soneki `_dev` soneki kullanıcı parolalarını geliştirme ortamından yüklenmiş değerleri. Değiştirin `{KEY VAULT NAME}` önceki adımda oluşturduğunuz anahtar kasasının adı:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>Azure'da barındırılan uygulamalar için uygulama kimliği ve X.509 sertifika kullan

Azure AD'yi yapılandırma, bir anahtar kasasına kimlik doğrulaması için Azure anahtar kasası ve uygulamayı bir Azure Active Directory Uygulama kimliği ve X.509 Sertifika **uygulamayı Azure dışında barındırılan zaman**. Daha fazla bilgi için [anahtarlara, parolalara ve sertifikalara hakkında](/azure/key-vault/about-keys-secrets-and-certificates).

> [!NOTE]
> Azure'da barındırılan uygulamalar için bir uygulama kimliği ve X.509 sertifikası kullanarak desteklenir, ancak kullanarak öneririz [kimliklerini Azure kaynakları için yönetilen](#use-managed-identities-for-azure-resources) uygulamanızı Azure'a barındırırken. Yönetilen kimlikleri, uygulamaya veya geliştirme ortamında bir sertifika depolanması gerekmez.

Bir uygulama kimliği ve X.509 sertifikası olduğunda örnek uygulamanın kullandığı `#define` en üstündeki deyimi *Program.cs* dosya ayarlanmış `Certificate`.

1. Bir PKCS #12 arşiv oluştur (*.pfx*) sertifika. Sertifikaları oluşturmaya yönelik seçenekleri içeren [Windows üzerinde MakeCert](/windows/desktop/seccrypto/makecert) ve [OpenSSL](https://www.openssl.org/).
1. Sertifikayı geçerli kullanıcının kişisel sertifika deposuna yükleyin. İşaretleme anahtar dışarı aktarılabilir olarak isteğe bağlıdır. Daha sonra bu işlemde kullanılan sertifikanın parmak izini unutmayın.
1. PKCS #12 arşiv dışarı aktar (*.pfx*) sertifikası olarak DER kodlu bir sertifika (*.cer*).
1. Uygulamayı Azure AD'ye kaydetme (**uygulama kayıtları**).
1. DER ile kodlanmış sertifikasını karşıya yükle (*.cer*) Azure AD'ye:
   1. Azure AD'de uygulamayı seçin.
   1. Gidin **sertifikaları ve parolaları**.
   1. Seçin **sertifikayı karşıya yükle** ortak anahtarı içeren sertifikayı karşıya yüklemek için. A *.cer*, *.pem*, veya *.crt* sertifikadır kabul edilebilir.
1. Anahtar kasası adı, uygulama kimliği ve sertifika parmak izi uygulamanın Store *appsettings.json* dosya.
1. Gidin **anahtar kasalarını** Azure portalında.
1. Oluşturduğunuz anahtar kasasını seçin [Azure Key Vault ile üretim ortamında gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) bölümü.
1. Seçin **erişim ilkeleri**.
1. Seçin **yeni Ekle**.
1. Seçin **Select sorumlusu** ve kayıtlı uygulama adına göre seçin. Seçin **seçin** düğmesi.
1. Açık **gizli dizi izinleri** ve uygulamayla **alma** ve **listesi** izinleri.
1. **Tamam**’ı seçin.
1. **Kaydet**’i seçin.
1. Uygulamayı dağıtın.

`Certificate` Örnek uygulaması edinir, yapılandırma değerlerinden `IConfigurationRoot` gizli dizi adı olarak aynı ada sahip:

* Hiyerarşik olmayan değerler: Değeri `SecretName` ile elde edilen `config["SecretName"]`.
* Hiyerarşik değerleri (bölümleri): Kullanım `:` (virgül) gösterimi veya `GetSection` genişletme yöntemi. Yapılandırma değeri elde etmek için Bu yaklaşımlardan birini kullanın:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

X.509 Sertifika, işletim sistemi tarafından yönetilir. Uygulama çağrıları `AddAzureKeyVault` tarafından sağlanan değerlerle *appsettings.json* dosyası:

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

Örnek değerler:

* Anahtar kasası adı: `contosovault`
* Uygulama Kimliği: `627e911e-43cc-61d4-992e-12db9c81b413`
* Sertifika parmak izi: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*appsettings.json*:

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Uygulamayı çalıştırdığınızda, bir Web sayfası yüklü gizli değerleri gösterir. Geliştirme ortamında ile gizli değerleri yük `_dev` soneki. İle üretim ortamında, değerleri yükleme `_prod` soneki.

## <a name="use-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikleri kullanmak

**Azure'a dağıtılan bir uygulama** yararlanabilirsiniz [kimliklerini Azure kaynakları için yönetilen](/azure/active-directory/managed-identities-azure-resources/overview), kimlik bilgileri olmadan Azure AD kimlik doğrulamasını kullanarak Azure Key Vault ile kimlik doğrulaması bir uygulama sağlar (uygulama kimliği ve Password/Client gizli) uygulamada depolanan.

Örnek uygulama, Azure kaynakları için yönetilen kimlikleri kullanır, `#define` en üstündeki deyimi *Program.cs* dosya ayarlanmış `Managed`.

Uygulamanın kasa adını girin *appsettings.json* dosya. Örnek uygulama, bir uygulama kimliği ve parolası (gizli) ayarlandığında gerektirmeyen `Managed` yapılandırma girişler yoksayabilirsiniz. Bu nedenle sürüm. Uygulamayı Azure'a dağıtılır ve Azure kimlik doğrulaması, Azure anahtar Kasası'nın içinde depolanan yalnızca kasa adını kullanarak erişmek için uygulamayı *appsettings.json* dosya.

Örnek uygulaması, Azure App Service'e dağıtın.

Azure App Service'e dağıtılan bir uygulama, hizmet oluşturulduğunda Azure AD'ye otomatik olarak kaydedilir. Aşağıdaki komutta kullanılacak dağıtım nesne Kimliğini almak. Nesne Kimliği üzerindeki Azure portalında gösterilen **kimlik** App Service'in paneli.

Azure CLI ve uygulamanın nesne Kimliğini kullanarak sağlamak uygulamayla `list` ve `get` anahtar kasasına erişim izinleri:

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

**Uygulamayı yeniden başlatması** Azure CLI, PowerShell veya Azure portalını kullanarak.

Örnek uygulama:

* Örneği oluşturur `AzureServiceTokenProvider` sınıf olmayan bir bağlantı dizesi. Bir bağlantı dizesi sağlanmayan, sağlayıcıyı Azure kaynakları için yönetilen kimlikleri bir erişim belirteci almak çalışır.
* Yeni bir `KeyVaultClient` ile oluşturulan `AzureServiceTokenProvider` örneği belirteci geri çağırma.
* `KeyVaultClient` Örneği varsayılan bir uygulama ile kullanılan `IKeyVaultSecretManager` tüm gizli değerleri yükler ve çift tire değiştirir (`--`) iki nokta üst üste ile (`:`) anahtar adları.

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

Uygulamayı çalıştırdığınızda, bir Web sayfası yüklü gizli değerleri gösterir. Geliştirme ortamında gizli değerlere sahip `_dev` kullanıcı parolaları tarafından sağlanan çünkü soneki. İle üretim ortamında, değerleri yükleme `_prod` Azure Key Vault tarafından sağlanan çünkü soneki.

Alırsanız bir `Access denied` hata, uygulamayı Azure AD'ye kayıtlı ve anahtar kasasına erişim sağlanan onaylayın. Azure'da hizmet yeniden onaylayın.

## <a name="use-a-key-name-prefix"></a>Anahtar adı ön ekini kullanın

`AddAzureKeyVault` uygulanışı kabul eden bir aşırı sağlar `IKeyVaultSecretManager`, nasıl anahtar kasa gizli dizilerini denetlemenize olanak sağlayan yapılandırma anahtarları dönüştürülür. Örneğin, uygulama başlatma sırasında sağladığınız bir önek değere göre gizli değerlerini yüklemek için arabirim uygulayabilir. Bu, örneğin, gizli dizileri uygulama sürümüne yüklemek için sağlar.

> [!WARNING]
> Ön ekleri için birden fazla uygulama gizli dizilerini aynı anahtar kasasının yerleştirmek için veya çevre gizli dizileri yerleştirmek için anahtar kasası parolaları kullanmayın (örneğin, *geliştirme* karşı *üretim* gizli Diziler) aynı içine Kasa. Farklı uygulama ve geliştirme veya üretim ortamları ayrı anahtar kasalarını yüksek düzeyde güvenlik için uygulama ortamları ayırmak için kullanmanızı öneririz.

Aşağıdaki örnekte, gizli anahtar kurulur kasası (ve geliştirme ortamı için gizli dizi Yöneticisi aracını kullanarak) için `5000-AppSecret` (anahtar kasası gizli dizi adları nokta izin verilmiyor). Bu gizli dizi sürümü 5.0.0.0 uygulama için bir uygulama gizli anahtarı temsil eder. 5.1.0.0, uygulama başka bir sürümü için gizli anahtarına eklenen kasası (ve gizli dizi Yöneticisi aracını kullanarak) için `5100-AppSecret`. Her uygulama sürümü tutulan gizli değeri yapılandırmasına yükler `AppSecret`, çıkarma sürümü gibi gizli dizi yükler.

`AddAzureKeyVault` özel bir adlı `IKeyVaultSecretManager`:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

`IKeyVaultSecretManager` Uygulama parolaları doğru parolayı yapılandırmasını yüklemek için sürüm öneklerini tepki verir:

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

`Load` Yöntemi sürüm önekine sahip olanları bulmak için kasa gizli dizilerini yinelenen bir sağlayıcı algoritması tarafından çağrılır. Ne zaman bir sürüm ön eki bulundu ile `Load`, algoritmasını `GetKey` gizli dizi adı yapılandırma adını döndürmek için yöntemi. Parolanın adı sürüm önekten kapalı kaldırır ve gizli dizi adı yüklenmesi için kalan ad-değer çiftleri uygulamanın yapılandırmasını döndürür.

Bu yaklaşım ne zaman uygulanır:

1. Uygulamanın proje dosyasında belirtilen uygulamanın sürümü. Aşağıdaki örnekte, uygulamanın sürüm kümesine `5.0.0.0`:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Onaylayın bir `<UserSecretsId>` uygulamanın proje dosyasında özelliği varsa burada `{GUID}` kullanıcı tarafından sağlanan bir GUID değeridir:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   Aşağıdaki gizli dizileri ile yerel ortamda kaydedin [gizli dizi Yöneticisi aracını](xref:security/app-secrets):

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. Gizli dizileri Azure Key Vault'ta aşağıdaki Azure CLI komutları kullanarak kaydedilir:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. Uygulamayı çalıştırdığınızda, anahtar kasası gizli dizileri yüklenir. Dize gizliliğini `5000-AppSecret` uygulamanın proje dosyasında belirtilen uygulamanın sürümle eşleşen (`5.0.0.0`).

1. Sürüm `5000` (ile dash), anahtar adı çıkartılır. Anahtarıyla yapılandırmasını okuma, uygulama genelinde `AppSecret` gizli değer yükler.

1. Uygulamanın sürümü proje dosyasında değişip değişmediğini `5.1.0.0` ve uygulamayı yeniden çalıştırın, döndürülen gizli değer `5.1.0.0_secret_value_dev` geliştirme ortamında ve `5.1.0.0_secret_value_prod` üretimde.

> [!NOTE]
> Ayrıca kendi sağlayabilirsiniz `KeyVaultClient` uygulamasına `AddAzureKeyVault`. Özel bir istemci, istemcinin tek bir örnek uygulama paylaşımı izin verir.

## <a name="bind-an-array-to-a-class"></a>Bir dizi bir sınıfa Bağla

Sağlayıcı bir POCO diziye bağlama için bir dizi halinde yapılandırma değerlerini okuma yeteneğine sahiptir.

İki nokta üst üste içerecek şekilde anahtarları izin veren bir yapılandırma kaynaktan okunurken (`:`) ayırıcı, sayısal bir anahtar kesimi bir dizi kurulumu yapın anahtarları ayırt etmek için kullanılır (`:0:`, `:1:`,... `:{n}:`). Daha fazla bilgi için [yapılandırma: Bir sınıf için bir dizi bağlama](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Azure anahtar kasası anahtarlarını bir iki nokta üst üste ayırıcı olarak kullanamazsınız. Bu konuda açıklanan yaklaşımı çift tire kullanır (`--`) hiyerarşik değerleri (bölümleri) için ayırıcı olarak. Dizi anahtarları, Azure anahtar Kasası'nda depolanır, çift çizgi ve sayısal anahtar kesimlerini (`--0--`, `--1--`, &hellip; `--{n}--`).

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

## <a name="reload-secrets"></a>Gizli dizileri yeniden yükleyin

Gizli dizileri kadar önbelleğe alınır `IConfigurationRoot.Reload()` çağrılır. Süresi dolan, devre dışı bırakıldı ve güncelleştirilmiş gizli anahtarları key vault'ta değil kadar uygulama tarafından dikkate `Reload` yürütülür.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Devre dışı bırakılmış ve süresi dolan gizli diziler

Devre dışı bırakılmış ve süresi dolan gizli diziler throw bir `KeyVaultClientException`. Uygulamanızı oluşturma gelen önlemek için uygulamanızı değiştirin veya devre dışı bırakılmış/süresi dolmuş gizli anahtarı güncelleştirme.

## <a name="troubleshoot"></a>Sorun giderme

Yapılandırma Sağlayıcısı'nı kullanarak yüklemek uygulama başarısız olduğunda, bir hata iletisi yazılan [ASP.NET Core günlüğü altyapı](xref:fundamentals/logging/index). Aşağıdaki koşullar yapılandırma yüklenmesini engeller.

* Uygulama veya sertifika, Azure Active Directory'de doğru şekilde yapılandırılmamış.
* Anahtar kasası, Azure anahtar Kasası'nda mevcut değil.
* Uygulama, anahtar kasasına erişmek için yetkili değil.
* Erişim İlkesi içermez `Get` ve `List` izinleri.
* Anahtar Kasası'nda yapılandırma verileri (ad-değer çifti) yanlış, eksik, devre dışı veya süresi dolmuş olarak adlandırılır.
* Uygulama, yanlış anahtar kasası adına sahip (`KeyVaultName`), Azure AD uygulama kimliği (`AzureADApplicationId`), veya Azure AD sertifika parmak izi (`AzureADCertThumbprint`).
* Yapılandırma anahtarı (ad), uygulamayı yüklemeye çalıştığınız değeri geçersiz.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Anahtar kasası](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Anahtar kasası belgeleri](/azure/key-vault/)
* [Azure anahtar kasası için nasıl oluşturma ve aktarma HSM korumalı anahtarlar](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient Class](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [Hızlı Başlangıç: .NET web uygulaması kullanarak Azure Key Vault'tan bir gizli dizi alma ve ayarlama](/azure/key-vault/quick-create-net)
* [Öğretici: Azure Key Vault ile Azure Windows sanal makinesine .NET kullanma](/azure/key-vault/tutorial-net-windows-virtual-machine)
