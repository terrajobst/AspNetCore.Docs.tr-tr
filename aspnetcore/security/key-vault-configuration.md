---
title: ASP.NET Core Azure Key Vault yapılandırma sağlayıcısı
author: guardrex
description: Çalışma zamanında yüklenen ad-değer çiftlerini kullanarak bir uygulamayı yapılandırmak için Azure Key Vault yapılandırma sağlayıcısını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2019
uid: security/key-vault-configuration
ms.openlocfilehash: 0d0b6e20a1901d4a2630ce263b5fd0cd7bcca8fe
ms.sourcegitcommit: 4fe3ae892f54dc540859bff78741a28c2daa9a38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2019
ms.locfileid: "68776648"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>ASP.NET Core Azure Key Vault yapılandırma sağlayıcısı

[Luke Latham](https://github.com/guardrex) ve [Andrew Stanton-nurte](https://github.com/anurse)

Bu belgede, Azure Key Vault gizliliklerden uygulama yapılandırma değerlerini yüklemek için [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısının nasıl kullanılacağı açıklanmaktadır. Azure Key Vault, uygulama ve hizmetler tarafından kullanılan şifreleme anahtarlarını ve gizli dizileri koruma konusunda yardımcı olan bulut tabanlı bir hizmettir. ASP.NET Core uygulamalarla Azure Key Vault kullanmaya yönelik yaygın senaryolar şunlardır:

* Hassas yapılandırma verilerine erişimi denetleme.
* Yapılandırma verilerini depolarken FIPS 140-2 düzey 2 doğrulanan donanım güvenlik modülleri (HSM 'ler) gereksinimini karşılarsınız.

Bu senaryo, ASP.NET Core 2,1 veya sonraki bir sürümü hedefleyen uygulamalar için kullanılabilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/key-vault-configuration/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="packages"></a>Paketler

Azure Key Vault yapılandırma sağlayıcısını kullanmak için [Microsoft. Extensions. Configuration. AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paketine bir paket başvurusu ekleyin.

[Azure kaynakları Için Yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview) senaryosunu benimsemek için [Microsoft. Azure. Services. appauthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) paketine bir paket başvurusu ekleyin.

> [!NOTE]
> Uygulamasının en son kararlı sürümü olan `Microsoft.Azure.Services.AppAuthentication` `1.0.3`yazma sırasında, [sistem tarafından atanan Yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work)için destek sağlar. *Kullanıcı tarafından atanan Yönetilen kimlikler* için destek `1.2.0-preview2` paketinde bulunur. Bu konu, sistem tarafından yönetilen kimliklerin kullanımını ve belirtilen örnek uygulamanın `1.0.3` `Microsoft.Azure.Services.AppAuthentication` paketin sürümünü kullandığını gösterir.

## <a name="sample-app"></a>Örnek uygulama

Örnek uygulama, `#define` *program.cs* dosyasının en üstünde yer aldığı ifadeye göre belirlenen iki moddan birinde çalışır:

* `Certificate`&ndash; Azure Key Vault depolanan gizli dizileri erişmek için Azure Key Vault istemci kimliği ve X. 509.952 sertifikası kullanımını gösterir. Örneğin bu sürümü, Azure App Service dağıtılan herhangi bir konumdan veya ASP.NET Core uygulamasına hizmet veren herhangi bir konağa çalıştırılabilir.
* `Managed`Uygulamanın kodunda veya yapılandırmasında depolanan kimlik bilgileri olmadan Azure AD kimlik doğrulamasıyla Azure Key Vault üzere uygulamanın kimliğini doğrulamak için [Azure kaynakları için yönetilen kimliklerin](/azure/active-directory/managed-identities-azure-resources/overview) nasıl kullanılacağını gösterir. &ndash; Kimlik doğrulaması için Yönetilen kimlikler kullanıldığında, bir Azure AD uygulama KIMLIĞI ve parolası (gizli anahtar) gerekli değildir. Örneğin `Managed` sürümünün Azure 'a dağıtılması gerekir. [Azure kaynakları Için Yönetilen kimlikler bölümünü kullanma](#use-managed-identities-for-azure-resources) bölümündeki yönergeleri izleyin.

Önişlemci yönergeleri (`#define`) kullanarak örnek bir uygulamanın nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Geliştirme ortamında gizli dizi

[Gizli dizi Yöneticisi aracını](xref:security/app-secrets)kullanarak gizli dizileri yerel olarak ayarlayın. Örnek uygulama, geliştirme ortamındaki yerel makinede çalıştığında, gizli anahtar Yöneticisi deposundan gizli diziler yüklenir.

Gizli dizi Yöneticisi Aracı, uygulamanın `<UserSecretsId>` proje dosyasında bir özellik gerektirir. Özellik değerini (`{GUID}`) herhangi bir benzersiz GUID 'ye ayarlayın:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

Gizlilikler ad-değer çiftleri olarak oluşturulur. Hiyerarşik değerler (yapılandırma bölümleri) `:` [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index) anahtarı adlarında ayırıcı olarak bir (iki nokta üst üste) kullanır.

Gizli dizi, projenin içerik köküne açılan bir komut kabuğundan kullanılır; burada `{SECRET NAME}` ad ve `{SECRET VALUE}` değerdir:

```console
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

Örnek uygulamanın gizli dizilerini ayarlamak için, projenin içerik kökünden bir komut kabuğu 'nda aşağıdaki komutları yürütün:

```console
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Bu gizlilikler, [Azure Key Vault bölümündeki üretim ortamındaki gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) alanında Azure Key Vault depolandığında, `_dev` sonek olarak `_prod`değiştirilir. Sonek, uygulamanın çıktısında yapılandırma değerlerinin kaynağını gösteren bir görsel ipucu sağlar.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Azure Key Vault ile üretim ortamında gizli dizi

[Hızlı başlangıç tarafından sunulan yönergeler: Azure CLI](/azure/key-vault/quick-create-cli) 'yı kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma, örnek uygulama tarafından kullanılan gizli dizileri depolamak ve Azure Key Vault oluşturmak için burada özetlenmiştir. Daha ayrıntılı bilgi için konusuna bakın.

1. [Azure Portal](https://portal.azure.com/)aşağıdaki yöntemlerden birini kullanarak Azure Cloud Shell 'i açın:

   * Bir kod bloğunun sağ üst köşesinde **deneyin** öğesini seçin. Metin kutusunda "Azure CLı" arama dizesini kullanın.
   * **Cloud Shell Başlat** düğmesini kullanarak tarayıcınızda Cloud Shell açın.
   * Azure portal sağ üst köşesindeki menüdeki **Cloud Shell** düğmesini seçin.

   Daha fazla bilgi için bkz. [Azure komut satırı arabirimi (CLI)](/cli/azure/) ve [Azure Cloud Shell Genel Bakış](/azure/cloud-shell/overview).

1. Henüz kimlik doğrulamasından geçirilmemişse `az login` komutuyla oturum açın.

1. Aşağıdaki komutla bir kaynak grubu oluşturun; burada `{RESOURCE GROUP NAME}` , yeni kaynak grubu için kaynak grubu adı ve `{LOCATION}` Azure bölgesi (Datacenter) olur:

   ```console
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Aşağıdaki komutla kaynak grubunda bir Anahtar Kasası oluşturun; burada `{KEY VAULT NAME}` yeni anahtar kasasının adı ve `{LOCATION}` Azure bölgesi (Datacenter) bulunur:

   ```console
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Anahtar kasasında ad-değer çiftleri olarak gizli diziler oluşturun.

   Azure Key Vault gizli dizi adları, alfasayısal karakterler ve tireler ile sınırlıdır. Hiyerarşik değerler (yapılandırma bölümleri) ayırıcı `--` olarak (iki tire) kullanır. Genellikle [ASP.NET Core yapılandırmasındaki](xref:fundamentals/configuration/index)bir alt anahtardan bir bölümü sınırlandırmak için kullanılan iki nokta üst üste, Anahtar Kasası gizli adlarında izin verilmez. Bu nedenle, gizlilikler uygulamanın yapılandırmasına yüklendiğinde iki tire kullanılır ve iki nokta üst üste bulunur.

   Aşağıdaki gizlilikler örnek uygulamayla kullanım içindir. Değerler, Kullanıcı parolalarından geliştirme ortamında yüklenen `_prod` `_dev` sonek değerlerinden ayırt etmek için bir sonek içerir. Önceki `{KEY VAULT NAME}` adımda oluşturduğunuz anahtar kasasının adıyla değiştirin:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanın

**Uygulama Azure dışında barındırıldığı zaman**bir anahtar kasasında kimlik doğrulaması yapmak IÇIN Azure AD, Azure Key Vault ve uygulamayı bir Azure ACTIVE DIRECTORY uygulama kimliği ve X. 509.440 sertifikası kullanacak şekilde yapılandırın. Daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](/azure/key-vault/about-keys-secrets-and-certificates).

> [!NOTE]
> Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanılması desteklenmekle birlikte, Azure 'da bir uygulama barındırılırken [Azure kaynakları Için Yönetilen kimlikler](#use-managed-identities-for-azure-resources) kullanmanızı öneririz. Yönetilen kimlikler, uygulamada veya geliştirme ortamında bir sertifikanın depolanmasını gerektirmez.

Örnek uygulama, `#define` *program.cs* dosyasının en üstündeki ifade olarak `Certificate`ayarlandığında bir uygulama kimliği ve X. 509.440 sertifikası kullanır.

1. PKCS # 12 Arşivi ( *. pfx*) sertifikası oluşturun. Sertifika oluşturma seçenekleri Windows ve [OpenSSL](https://www.openssl.org/) [üzerinde MakeCert](/windows/desktop/seccrypto/makecert) içerir.
1. Sertifikayı geçerli kullanıcının kişisel sertifika deposuna yükler. Anahtarı verilebilir olarak işaretlemek isteğe bağlıdır. Sertifikanın daha sonra bu işlemde kullanılan parmak izini aklınızda bulunur.
1. PKCS # 12 Arşivi ( *. pfx*) sertifikasını der kodlu bir sertifika ( *. cer*) olarak dışarı aktarın.
1. Uygulamayı Azure AD 'ye kaydedin (**uygulama kayıtları**).
1. DER kodlu sertifikayı ( *. cer*) Azure AD 'ye yükleyin:
   1. Azure AD 'de uygulamayı seçin.
   1. **Sertifikalar & gizli**dizi sayfasına gidin.
   1. Ortak anahtarı içeren sertifikayı karşıya yüklemek için **sertifikayı karşıya yükle** ' yi seçin. *. Cer*, *. pek*veya *. CRT* sertifikası kabul edilebilir.
1. Anahtar Kasası adı, uygulama KIMLIĞI ve sertifika parmak izini uygulamanın *appSettings. JSON* dosyasında depolayın.
1. Azure portal **ana** kasaları ' ne gidin.
1. Azure Key Vault bölümünde, [üretim ortamındaki gizli dizi deposunda](#secret-storage-in-the-production-environment-with-azure-key-vault) oluşturduğunuz anahtar kasasını seçin.
1. **Erişim ilkeleri**' ni seçin.
1. **Yeni Ekle**' yi seçin.
1. **Sorumlu Seç** ' i seçin ve kayıtlı uygulamayı ada göre seçin. **Seç** düğmesini seçin.
1. **Gizli izinleri** açın ve uygulamaya **Get** ve **list** izinleri sağlayın.
1. **Tamam**’ı seçin.
1. **Kaydet**’i seçin.
1. Uygulamayı dağıtın.

Örnek uygulama, yapılandırma `IConfigurationRoot` değerlerini, parola adı ile aynı adla alır: `Certificate`

* Hiyerarşik olmayan değerler: İçin `SecretName` değeri ile `config["SecretName"]`elde edilir.
* Hiyerarşik değerler (bölümler): ( `:` İki nokta üst üste) gösterimini `GetSection` veya genişletme yöntemini kullanın. Yapılandırma değerini elde etmek için şu yaklaşımlardan birini kullanın:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

X. 509.440 sertifikası işletim sistemi tarafından yönetiliyor. Uygulama, `AddAzureKeyVault` *appSettings. JSON* dosyası tarafından sağlanan değerlerle çağırır:

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

Örnek değerler:

* Anahtar Kasası adı:`contosovault`
* Uygulama KIMLIĞI:`627e911e-43cc-61d4-992e-12db9c81b413`
* Sertifika parmak izi:`fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*appSettings. JSON*:

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir. Geliştirme ortamında, gizli anahtar değerleri `_dev` sonek ile yüklenir. Üretim ortamında, değerler `_prod` sonek ile yüklenir.

## <a name="use-managed-identities-for-azure-resources"></a>Azure kaynakları için Yönetilen kimlikler kullanma

Azure **'a dağıtılan bir uygulama** , [Azure kaynakları için yönetilen kimliklerden](/azure/active-directory/managed-identities-azure-resources/overview)yararlanarak uygulamanın KIMLIK bilgileri olmadan Azure AD kimlik doğrulamasını kullanarak Azure Key Vault kimlik doğrulaması yapmasına olanak sağlar (uygulama kimliği ve parola/istemci gizli anahtarı) uygulamada depolanır.

Örnek uygulama, `#define` *program.cs* dosyasının en üstündeki ifade olarak `Managed`ayarlandığında Azure kaynakları için Yönetilen kimlikler kullanır.

Uygulamanın *appSettings. JSON* dosyasına kasa adını girin. Örnek uygulama `Managed` sürüme ayarlandığında bir uygulama kimliği ve parola (istemci gizli anahtarı) gerektirmez, bu nedenle bu yapılandırma girişlerini yoksayabilirsiniz. Uygulama Azure 'a dağıtılır ve Azure, yalnızca *appSettings. JSON* dosyasında depolanan kasa adını kullanarak Azure Key Vault erişmek için uygulamanın kimliğini doğrular.

Azure App Service için örnek uygulamayı dağıtın.

Azure App Service dağıtılan bir uygulama, hizmet oluşturulduğunda Azure AD 'ye otomatik olarak kaydedilir. Aşağıdaki komutta kullanılmak üzere dağıtımdan nesne KIMLIĞINI edinin. Nesne KIMLIĞI, App Service **kimlik** panelinde Azure Portal gösterilir.

Azure CLI ve uygulamanın nesne kimliğini kullanarak, uygulama `list` ve `get` anahtar kasasına erişim izinleri sağlayın:

```console
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

Azure CLı, PowerShell veya Azure portal kullanarak **uygulamayı yeniden başlatın** .

Örnek uygulama:

* Bir bağlantı dizesi olmadan `AzureServiceTokenProvider` sınıfın bir örneğini oluşturur. Bir bağlantı dizesi sağlanmazsa, sağlayıcı Azure kaynakları için yönetilen kimliklerden bir erişim belirteci almaya çalışır.
* Örnek belirteci `KeyVaultClient` geri çağırması ile yeni bir oluşturulur. `AzureServiceTokenProvider`
* Örnek, tüm gizli değerleri yükleyen ve çift tire`:`(`--`) değerini anahtar adlarında iki nokta () ile değiştirir. `IKeyVaultSecretManager` `KeyVaultClient`

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir. Geliştirme ortamında, gizli değerler, Kullanıcı gizli dizileri `_dev` tarafından sağlandıklarından, soneke sahiptir. Üretim ortamında, değerler Azure Key Vault tarafından sağlandıklarından `_prod` sonek ile yüklenir.

Bir `Access denied` hata alırsanız, uygulamanın Azure AD 'ye kayıtlı olduğunu ve anahtar kasasına erişim sağladıklarını doğrulayın. Azure 'da hizmeti yeniden başlattığınızdan emin olun.

## <a name="use-a-key-name-prefix"></a>Anahtar adı öneki kullanın

`AddAzureKeyVault``IKeyVaultSecretManager`, Anahtar Kasası gizli dizilerini yapılandırma anahtarlarına nasıl dönüştürdüğünü denetlemenize olanak tanıyan, uygulamasını kabul eden bir aşırı yükleme sağlar. Örneğin, uygulama başlangıcında sağladığınız önek değerine göre gizli değerleri yüklemek için arabirimini uygulayabilirsiniz. Bu, örneğin, uygulama sürümüne göre gizli dizileri yüklemeyi sağlar.

> [!WARNING]
> Birden çok uygulama için gizli dizileri aynı kasaya yerleştirmek veya çevresel gizli dizileri (örneğin, *geliştirme* ve *Üretim* gizlilikleri) aynı kasaya yerleştirmek için Anahtar Kasası gizli dizileri üzerinde ön ekleri kullanmayın. Farklı uygulama ve geliştirme/üretim ortamlarının, uygulama ortamlarını en yüksek düzeyde güvenlik için yalıtmak üzere ayrı anahtar kasaları kullanmasını öneririz.

Aşağıdaki örnekte, için `5000-AppSecret` anahtar kasasında (ve geliştirme ortamı için gizli Yönetim Aracı kullanılarak) bir gizli dizi oluşturulur (Anahtar Kasası gizli adlarında dönemlere izin verilmez). Bu gizli anahtar, uygulamanın 5.0.0.0 sürümü için bir uygulama gizli anahtarı temsil eder. Uygulamanın başka bir sürümü olan 5.1.0.0, anahtar kasasına (ve gizli Yönetici Aracı kullanılarak) `5100-AppSecret`bir gizli dizi eklenir. Her bir uygulama sürümü sürümü sürümlü gizli değerini yapılandırma olarak `AppSecret`yükler, bu, gizli anahtarı yüklerken sürümü de kapatıyor.

`AddAzureKeyVault`özel `IKeyVaultSecretManager`bir ile çağrılır:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

`IKeyVaultSecretManager` Uygulama, doğru gizli anahtarı yapılandırmaya yüklemek için gizli dizi sürüm öneklerine tepki verir:

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

`Load` Yöntemi, sürüm ön ekine sahip olanları bulmak için kasa gizli dizileri aracılığıyla yinelenen bir sağlayıcı algoritması tarafından çağırılır. İle `Load`bir sürüm ön eki bulunduğunda, algoritma, gizli anahtar adının `GetKey` yapılandırma adını döndürmek için yöntemini kullanır. Gizlilik adından sürüm önekini kaldırır ve uygulamanın yapılandırma adı-değer çiftlerine yüklemek için gizli dizi adının geri kalanını döndürür.

Bu yaklaşım uygulandığında:

1. Uygulamanın proje dosyasında belirtilen sürümü. Aşağıdaki örnekte, uygulamanın sürümü olarak `5.0.0.0`ayarlanır:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Uygulamanın proje dosyasında `<UserSecretsId>` bir özelliğin var olduğunu, burada `{GUID}` Kullanıcı tarafından sağlanan bir GUID olduğunu onaylayın:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   [Gizli dizi yöneticisi aracıyla](xref:security/app-secrets)aşağıdaki gizli dizileri yerel olarak kaydedin:

   ```console
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. Gizlilikler, aşağıdaki Azure CLı komutları kullanılarak Azure Key Vault kaydedilir:

   ```console
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. Uygulama çalıştırıldığında, Anahtar Kasası gizli dizileri yüklenir. İçin `5000-AppSecret` dize gizli dizisi, uygulamanın proje dosyasında (`5.0.0.0`) belirtilen uygulamanın sürümüyle eşleştirilir.

1. Sürüm `5000` (tireyle birlikte), anahtar adından çıkarılır. Uygulamanın tamamında, anahtarla `AppSecret` yapılandırmayı okumak gizli değeri yükler.

1. Uygulamanın sürümü proje dosyasında olarak `5.1.0.0` değiştirilirse ve uygulama yeniden çalıştırıldığında, döndürülen gizli değer geliştirme ortamında ve `5.1.0.0_secret_value_prod` üretimde bulunur `5.1.0.0_secret_value_dev` .

> [!NOTE]
> Ayrıca, kendi `KeyVaultClient` `AddAzureKeyVault`uygulamanızı da sağlayabilirsiniz. Özel istemci, uygulama genelinde istemcinin tek bir örneğini paylaşıma izin verir.

## <a name="bind-an-array-to-a-class"></a>Bir diziyi sınıfa bağlama

Sağlayıcı, bir POCO dizisine bağlamak için yapılandırma değerlerini bir diziye okuyabilme özelliğine sahiptir.

Anahtarların iki nokta (`:`) ayırıcılar içermesine izin veren bir yapılandırma kaynağından okurken, bir diziyi oluşturan anahtarları ayırt etmek için bir sayısal anahtar kesimi kullanılır (`:0:`, `:1:`,... `:{n}:`). Daha fazla bilgi için bkz [. yapılandırma: Bir diziyi sınıfa](xref:fundamentals/configuration/index#bind-an-array-to-a-class)bağlayın.

Azure Key Vault anahtarlar ayırıcı olarak iki nokta üst üste kullanamaz. Bu konuda açıklanan yaklaşım, hiyerarşik değerler (bölümler)`--`için bir ayırıcı olarak çift tire () kullanır. Dizi anahtarları çift tireler ve sayısal anahtar kesimleri`--0--`(, `--1--`, &hellip; `--{n}--`) ile birlikte Azure Key Vault depolanır.

Bir JSON dosyası tarafından sunulan aşağıdaki [Serilog](https://serilog.net/) günlük sağlayıcısı yapılandırmasını inceleyin. Dizide günlüğe kaydetme hedeflerini açıklayan iki Serilog girişi yansıtan iki nesne değişmezdeğeri vardır: `WriteTo`

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

Önceki json dosyasında gösterilen yapılandırma, Çift tire (`--`) gösterimi ve sayısal kesimleri kullanarak Azure Key Vault depolanır:

| Anahtar | Değer |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>Gizli dizileri yeniden yükleme

Gizli diziler çağrılana `IConfigurationRoot.Reload()` kadar önbelleğe alınır. Anahtar kasasındaki zaman aşımına uğradı, devre dışı ve güncelleştirilmiş gizli dizileri, yürütülene kadar `Reload` uygulama tarafından dikkate alınmıyor.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Devre dışı ve süre dolma parolaları

Devre dışı ve süresi biten gizlilikler `KeyVaultClientException` bir at çalışma zamanı oluşturur. Uygulamanın üretilmesini engellemek için, farklı bir yapılandırma sağlayıcısı kullanarak yapılandırmayı sağlayın veya devre dışı ya da süre dolma parolasını güncelleştirin.

## <a name="troubleshoot"></a>Sorun giderme

Uygulama, sağlayıcıyı kullanarak yapılandırmayı yükleyemediğinde, [ASP.NET Core günlük altyapısına](xref:fundamentals/logging/index)bir hata iletisi yazılır. Aşağıdaki koşullar yapılandırmanın yüklenmesine engel olur:

* Uygulama veya sertifika Azure Active Directory içinde doğru yapılandırılmamış.
* Anahtar Kasası Azure Key Vault içinde yok.
* Uygulamanın anahtar kasasına erişme yetkisi yok.
* Erişim ilkesi ve `List` izinleri içermez `Get` .
* Anahtar kasasında yapılandırma verileri (ad-değer çifti) yanlış olarak adlandırılmış, eksik, devre dışı veya zaman aşımına uğradı.
* Uygulamanın Anahtar Kasası adı (`KeyVaultName`), Azure AD uygulama kimliği (`AzureADApplicationId`) veya Azure AD sertifika parmak izi (`AzureADCertThumbprint`) yanlış.
* Yüklemeye çalıştığınız değer için uygulamada yapılandırma anahtarı (ad) yanlış.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Key Vault belgeleri](/azure/key-vault/)
* [Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient sınıfı](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [Hızlı Başlangıç: .NET Web uygulaması kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma](/azure/key-vault/quick-create-net)
* [Öğretici: .NET ' te Azure Windows sanal makinesi ile Azure Key Vault kullanma](/azure/key-vault/tutorial-net-windows-virtual-machine)
