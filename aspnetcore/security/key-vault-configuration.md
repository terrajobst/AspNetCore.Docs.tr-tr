---
title: ASP.NET Core Azure Key Vault yapılandırma sağlayıcısı
author: guardrex
description: Çalışma zamanında yüklenen ad-değer çiftlerini kullanarak bir uygulamayı yapılandırmak için Azure Key Vault yapılandırma sağlayıcısını nasıl kullanacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: security/key-vault-configuration
ms.openlocfilehash: cc3894df4df169d941f54ef3dfad5d3e6f798aad
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007409"
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

[Azure kaynakları Için Yönetilen kimlikler senaryosunu benimsemek](/azure/active-directory/managed-identities-azure-resources/overview) için [Microsoft. Azure. Services. appauthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/) paketine bir paket başvurusu ekleyin.

> [!NOTE]
> Yazma sırasında, `Microsoft.Azure.Services.AppAuthentication`, sürüm `1.0.3` ' in en son kararlı sürümü, [sistem tarafından atanan Yönetilen kimlikler](/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work)için destek sağlar. @No__t-1 paketinde *Kullanıcı tarafından atanan Yönetilen kimlikler* için destek sunulmaktadır. Bu konu, sistem tarafından yönetilen kimliklerin kullanımını gösterir ve belirtilen örnek uygulama `Microsoft.Azure.Services.AppAuthentication` paketinin `1.0.3` sürümünü kullanır.

## <a name="sample-app"></a>Örnek uygulama

Örnek uygulama, *program.cs* dosyasının en üstünde `#define` ifadesiyle belirlenen iki moddan birinde çalışır:

* `Certificate` &ndash; Azure Key Vault depolanan gizli dizileri erişmek için Azure Key Vault Istemci KIMLIĞI ve X. 509.952 sertifikası kullanımını gösterir. Örneğin bu sürümü, Azure App Service dağıtılan herhangi bir konumdan veya ASP.NET Core uygulamasına hizmet veren herhangi bir konağa çalıştırılabilir.
* `Managed` &ndash; Azure [kaynakları Için yönetilen kimliklerin](/azure/active-directory/managed-identities-azure-resources/overview) , uygulamanın kodunda veya yapılandırmasında depolanan kimlik bilgileri olmadan Azure AD kimlik doğrulamasıyla Azure Key Vault için nasıl kullanılacağını gösterir. Kimlik doğrulaması için Yönetilen kimlikler kullanıldığında, bir Azure AD uygulama KIMLIĞI ve parolası (gizli anahtar) gerekli değildir. Örneğin `Managed` sürümünün Azure 'a dağıtılması gerekir. [Azure kaynakları Için Yönetilen kimlikler bölümünü kullanma](#use-managed-identities-for-azure-resources) bölümündeki yönergeleri izleyin.

Önişlemci yönergeleri kullanarak örnek bir uygulamayı yapılandırma hakkında daha fazla bilgi için (`#define`), bkz. <xref:index#preprocessor-directives-in-sample-code>.

## <a name="secret-storage-in-the-development-environment"></a>Geliştirme ortamında gizli dizi

[Gizli dizi Yöneticisi aracını](xref:security/app-secrets)kullanarak gizli dizileri yerel olarak ayarlayın. Örnek uygulama, geliştirme ortamındaki yerel makinede çalıştığında, gizli anahtar Yöneticisi deposundan gizli diziler yüklenir.

Gizli dizi Yöneticisi Aracı, uygulamanın proje dosyasında `<UserSecretsId>` özelliği gerektirir. Özellik değerini (`{GUID}`) herhangi bir benzersiz GUID 'ye ayarlayın:

```xml
<PropertyGroup>
  <UserSecretsId>{GUID}</UserSecretsId>
</PropertyGroup>
```

Gizlilikler ad-değer çiftleri olarak oluşturulur. Hiyerarşik değerler (yapılandırma bölümleri) [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index) anahtar adlarında ayırıcı olarak `:` (iki nokta) kullanır.

Gizli dizi, projenin [içerik köküne](xref:fundamentals/index#content-root)açılan bir komut kabuğundan kullanılır; burada `{SECRET NAME}` adı ve `{SECRET VALUE}` değeridir:

```dotnetcli
dotnet user-secrets set "{SECRET NAME}" "{SECRET VALUE}"
```

Örnek uygulamanın gizli dizilerini ayarlamak için, projenin [içerik kökünden](xref:fundamentals/index#content-root) bir komut kabuğu 'nda aşağıdaki komutları yürütün:

```dotnetcli
dotnet user-secrets set "SecretName" "secret_value_1_dev"
dotnet user-secrets set "Section:SecretName" "secret_value_2_dev"
```

Bu gizlilikler, [Azure Key Vault bölümü Ile üretim ortamındaki gizli depolama](#secret-storage-in-the-production-environment-with-azure-key-vault) alanında Azure Key Vault depolandığında `_dev` son eki `_prod` olarak değiştirilir. Sonek, uygulamanın çıktısında yapılandırma değerlerinin kaynağını gösteren bir görsel ipucu sağlar.

## <a name="secret-storage-in-the-production-environment-with-azure-key-vault"></a>Azure Key Vault ile üretim ortamında gizli dizi

@No__t-0Hızlı başlangıcı tarafından sunulan yönergeler: Azure CLı 'yı kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma, örnek uygulama tarafından kullanılan gizli dizileri depolamak ve Azure Key Vault oluşturmak için burada özetlenmiştir. Daha ayrıntılı bilgi için konusuna bakın.

1. [Azure Portal](https://portal.azure.com/)aşağıdaki yöntemlerden birini kullanarak Azure Cloud Shell 'i açın:

   * Bir kod bloğunun sağ üst köşesinde **deneyin** öğesini seçin. Metin kutusunda "Azure CLı" arama dizesini kullanın.
   * **Cloud Shell Başlat** düğmesini kullanarak tarayıcınızda Cloud Shell açın.
   * Azure portal sağ üst köşesindeki menüdeki **Cloud Shell** düğmesini seçin.

   Daha fazla bilgi için bkz. [Azure komut satırı arabirimi (CLI)](/cli/azure/) ve [Azure Cloud Shell Genel Bakış](/azure/cloud-shell/overview).

1. Henüz kimlik doğrulamasından geçirilmemişse `az login` komutuyla oturum açın.

1. Aşağıdaki komutla bir kaynak grubu oluşturun; burada `{RESOURCE GROUP NAME}`, yeni kaynak grubu için kaynak grubu adı ve `{LOCATION}` Azure bölgesidir (Datacenter):

   ```azure-cli
   az group create --name "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Aşağıdaki komutla kaynak grubunda bir Anahtar Kasası oluşturun; burada `{KEY VAULT NAME}` Yeni Anahtar Kasası için addır ve `{LOCATION}` Azure bölgesidir (veri merkezi):

   ```azure-cli
   az keyvault create --name "{KEY VAULT NAME}" --resource-group "{RESOURCE GROUP NAME}" --location {LOCATION}
   ```

1. Anahtar kasasında ad-değer çiftleri olarak gizli diziler oluşturun.

   Azure Key Vault gizli dizi adları, alfasayısal karakterler ve tireler ile sınırlıdır. Hiyerarşik değerler (yapılandırma bölümleri) ayırıcı olarak `--` (iki tire) kullanır. Genellikle [ASP.NET Core yapılandırmasındaki](xref:fundamentals/configuration/index)bir alt anahtardan bir bölümü sınırlandırmak için kullanılan iki nokta üst üste, Anahtar Kasası gizli adlarında izin verilmez. Bu nedenle, gizlilikler uygulamanın yapılandırmasına yüklendiğinde iki tire kullanılır ve iki nokta üst üste bulunur.

   Aşağıdaki gizlilikler örnek uygulamayla kullanım içindir. Değerler, Kullanıcı parolalarından geliştirme ortamında yüklenen `_dev` sonek değerlerinden ayırt etmek için bir `_prod` soneki içerir. @No__t-0 ' yı önceki adımda oluşturduğunuz anahtar kasasının adıyla değiştirin:

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "SecretName" --value "secret_value_1_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "Section--SecretName" --value "secret_value_2_prod"
   ```

## <a name="use-application-id-and-x509-certificate-for-non-azure-hosted-apps"></a>Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanın

**Uygulama Azure dışında barındırıldığı zaman**bir anahtar kasasında kimlik doğrulaması yapmak IÇIN Azure AD, Azure Key Vault ve uygulamayı bir Azure ACTIVE DIRECTORY uygulama kimliği ve X. 509.440 sertifikası kullanacak şekilde yapılandırın. Daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](/azure/key-vault/about-keys-secrets-and-certificates).

> [!NOTE]
> Azure 'da barındırılan uygulamalar için uygulama KIMLIĞI ve X. 509.440 sertifikası kullanılması desteklenmekle birlikte, Azure 'da bir uygulama barındırılırken [Azure kaynakları Için Yönetilen kimlikler](#use-managed-identities-for-azure-resources) kullanmanızı öneririz. Yönetilen kimlikler, uygulamada veya geliştirme ortamında bir sertifikanın depolanmasını gerektirmez.

Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` açıklaması `Certificate` olarak ayarlandığında BIR uygulama kimliği ve X. 509.440 sertifikası kullanır.

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

@No__t-0 örnek uygulaması, `IConfigurationRoot` ' den gelen yapılandırma değerlerini gizli adıyla aynı adla alır:

* Hiyerarşik olmayan değerler: @No__t-0 değeri `config["SecretName"]` ile elde edilir.
* Hiyerarşik değerler (bölümler): @No__t-0 (iki nokta üst üste) gösterimini veya `GetSection` uzantı yöntemini kullanın. Yapılandırma değerini elde etmek için şu yaklaşımlardan birini kullanın:
  * `config["Section:SecretName"]`
  * `config.GetSection("Section")["SecretName"]`

X. 509.440 sertifikası işletim sistemi tarafından yönetiliyor. Uygulama, *appSettings. JSON* dosyası tarafından sağlanan değerlerle `AddAzureKeyVault` ' yı çağırır:

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet1&highlight=20-23)]

Örnek değerler:

* Anahtar Kasası adı: `contosovault`
* Uygulama KIMLIĞI: `627e911e-43cc-61d4-992e-12db9c81b413`
* Sertifika parmak izi: `fe14593dd66b2406c5269d742d04b6e1ab03adb1`

*appSettings. JSON*:

[!code-json[](key-vault-configuration/sample/appsettings.json)]

Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir. Geliştirme ortamında, gizli değerler `_dev` sonekiyle yüklenir. Üretim ortamında, değerler `_prod` soneki ile yüklenir.

## <a name="use-managed-identities-for-azure-resources"></a>Azure kaynakları için Yönetilen kimlikler kullanma

Azure **'a dağıtılan bir uygulama** , [Azure kaynakları için yönetilen kimliklerden](/azure/active-directory/managed-identities-azure-resources/overview)yararlanarak uygulamanın KIMLIK bilgileri olmadan Azure AD kimlik doğrulamasını kullanarak Azure Key Vault kimlik doğrulaması yapmasına olanak sağlar (uygulama kimliği ve parola/istemci gizli anahtarı) uygulamada depolanır.

Örnek uygulama, *program.cs* dosyasının en üstündeki `#define` açıklaması `Managed` olarak ayarlandığında Azure kaynakları için Yönetilen kimlikler kullanır.

Uygulamanın *appSettings. JSON* dosyasına kasa adını girin. Örnek uygulama, `Managed` sürümüne ayarlandığında bir uygulama KIMLIĞI ve parolası (Istemci gizli dizisi) gerektirmez, bu nedenle bu yapılandırma girişlerini yoksayabilirsiniz. Uygulama Azure 'a dağıtılır ve Azure, yalnızca *appSettings. JSON* dosyasında depolanan kasa adını kullanarak Azure Key Vault erişmek için uygulamanın kimliğini doğrular.

Azure App Service için örnek uygulamayı dağıtın.

Azure App Service dağıtılan bir uygulama, hizmet oluşturulduğunda Azure AD 'ye otomatik olarak kaydedilir. Aşağıdaki komutta kullanılmak üzere dağıtımdan nesne KIMLIĞINI edinin. Nesne KIMLIĞI, App Service **kimlik** panelinde Azure Portal gösterilir.

Azure CLı ve uygulamanın nesne KIMLIĞINI kullanarak, anahtar kasasına erişmek için `list` ve `get` izinlerine sahip olan uygulamayı sağlayın:

```azure-cli
az keyvault set-policy --name '{KEY VAULT NAME}' --object-id {OBJECT ID} --secret-permissions get list
```

Azure CLı, PowerShell veya Azure portal kullanarak **uygulamayı yeniden başlatın** .

Örnek uygulama:

* Bağlantı dizesi olmadan `AzureServiceTokenProvider` sınıfının bir örneğini oluşturur. Bir bağlantı dizesi sağlanmazsa, sağlayıcı Azure kaynakları için yönetilen kimliklerden bir erişim belirteci almaya çalışır.
* Yeni bir `KeyVaultClient` `AzureServiceTokenProvider` örnek belirteci geri çağırması ile oluşturulur.
* @No__t-0 örneği, tüm gizli değerleri yükleyen ve çift tireleri (`--`) anahtar adlarında iki nokta (`:`) ile değiştirir `IKeyVaultSecretManager` varsayılan uygulamasıyla kullanılır.

[!code-csharp[](key-vault-configuration/sample/Program.cs?name=snippet2&highlight=13-21)]

Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir. Geliştirme ortamında, gizli değerler `_dev` sonekine sahiptir çünkü Kullanıcı gizli dizileri tarafından sağlanırlar. Üretim ortamında, Azure Key Vault tarafından sağlandıklarından, değerler `_prod` sonekiyle yüklenir.

@No__t-0 hatası alırsanız, uygulamanın Azure AD 'ye kayıtlı olduğunu ve anahtar kasasına erişim sağladıklarını doğrulayın. Azure 'da hizmeti yeniden başlattığınızdan emin olun.

## <a name="use-a-key-name-prefix"></a>Anahtar adı öneki kullanın

`AddAzureKeyVault`, Anahtar Kasası gizli dizilerini yapılandırma anahtarlarına nasıl dönüştürdüğünü denetlemenize olanak sağlayan `IKeyVaultSecretManager` uygulamasını kabul eden bir aşırı yükleme sağlar. Örneğin, uygulama başlangıcında sağladığınız önek değerine göre gizli değerleri yüklemek için arabirimini uygulayabilirsiniz. Bu, örneğin, uygulama sürümüne göre gizli dizileri yüklemeyi sağlar.

> [!WARNING]
> Birden çok uygulama için gizli dizileri aynı kasaya yerleştirmek veya çevresel gizli dizileri (örneğin, *geliştirme* ve *Üretim* gizlilikleri) aynı kasaya yerleştirmek için Anahtar Kasası gizli dizileri üzerinde ön ekleri kullanmayın. Farklı uygulama ve geliştirme/üretim ortamlarının, uygulama ortamlarını en yüksek düzeyde güvenlik için yalıtmak üzere ayrı anahtar kasaları kullanmasını öneririz.

Aşağıdaki örnekte, anahtar kasasında (ve geliştirme ortamı için gizli Yönetici Aracı kullanılarak) `5000-AppSecret` (Anahtar Kasası gizli adlarında döneme izin verilmez) için bir gizli dizi oluşturulur. Bu gizli anahtar, uygulamanın 5.0.0.0 sürümü için bir uygulama gizli anahtarı temsil eder. Uygulamanın başka bir sürümü olan 5.1.0.0, anahtar kasasına (ve gizli Yönetici Aracı kullanılarak) `5100-AppSecret` için bir gizli dizi eklenir. Her bir uygulama sürümü sürümlenmiş gizli değerini `AppSecret` olarak yükler ve gizli anahtarı yüklerken sürümü kapatır.

`AddAzureKeyVault`, özel bir `IKeyVaultSecretManager` ile çağrılır:

[!code-csharp[](key-vault-configuration/sample_snapshot/Program.cs?highlight=30-34)]

@No__t 0 uygulama, doğru gizli anahtarı yapılandırmaya yüklemek için gizli dizi sürüm öneklerine yeniden davranır:

[!code-csharp[](key-vault-configuration/sample_snapshot/Startup.cs?name=snippet1)]

@No__t-0 yöntemi, sürüm ön ekine sahip olanları bulmak için kasa gizli dizileri aracılığıyla yinelenen bir sağlayıcı algoritması tarafından çağırılır. @No__t-0 ile bir sürüm ön eki bulunduğunda, algoritma, parola adının yapılandırma adını döndürmek için `GetKey` yöntemini kullanır. Gizlilik adından sürüm önekini kaldırır ve uygulamanın yapılandırma adı-değer çiftlerine yüklemek için gizli dizi adının geri kalanını döndürür.

Bu yaklaşım uygulandığında:

1. Uygulamanın proje dosyasında belirtilen sürümü. Aşağıdaki örnekte, uygulamanın sürümü `5.0.0.0` olarak ayarlanmıştır:

   ```xml
   <PropertyGroup>
     <Version>5.0.0.0</Version>
   </PropertyGroup>
   ```

1. Uygulamanın proje dosyasında `<UserSecretsId>` özelliğinin bulunduğunu doğrulayın; burada `{GUID}` Kullanıcı tarafından sağlanan bir GUID 'dir:

   ```xml
   <PropertyGroup>
     <UserSecretsId>{GUID}</UserSecretsId>
   </PropertyGroup>
   ```

   [Gizli dizi yöneticisi aracıyla](xref:security/app-secrets)aşağıdaki gizli dizileri yerel olarak kaydedin:

   ```dotnetcli
   dotnet user-secrets set "5000-AppSecret" "5.0.0.0_secret_value_dev"
   dotnet user-secrets set "5100-AppSecret" "5.1.0.0_secret_value_dev"
   ```

1. Gizlilikler, aşağıdaki Azure CLı komutları kullanılarak Azure Key Vault kaydedilir:

   ```azure-cli
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5000-AppSecret" --value "5.0.0.0_secret_value_prod"
   az keyvault secret set --vault-name "{KEY VAULT NAME}" --name "5100-AppSecret" --value "5.1.0.0_secret_value_prod"
   ```

1. Uygulama çalıştırıldığında, Anahtar Kasası gizli dizileri yüklenir. @No__t-0 için dize gizli dizisi, uygulamanın proje dosyasında belirtilen sürümle (`5.0.0.0`) eşleşir.

1. @No__t-0 (Dash ile) sürümü, anahtar adından çıkarılır. Uygulamanın tamamında, `AppSecret` anahtarıyla yapılandırmayı okumak gizli değeri yükler.

1. Uygulamanın sürümü proje dosyasında `5.1.0.0` olarak değiştirilirse ve uygulama yeniden çalıştırıldığında, döndürülen gizli değer geliştirme ortamında ve üretimde `5.1.0.0_secret_value_prod` ' dir `5.1.0.0_secret_value_dev` ' dir.

> [!NOTE]
> Ayrıca, `AddAzureKeyVault` için kendi `KeyVaultClient` uygulamanızı sağlayabilirsiniz. Özel istemci, uygulama genelinde istemcinin tek bir örneğini paylaşıma izin verir.

## <a name="bind-an-array-to-a-class"></a>Bir diziyi sınıfa bağlama

Sağlayıcı, bir POCO dizisine bağlamak için yapılandırma değerlerini bir diziye okuyabilme özelliğine sahiptir.

Anahtarların iki nokta (`:`) ayırıcılar içermesine izin veren bir yapılandırma kaynağından okurken, bir diziyi oluşturan anahtarları ayırt etmek için bir sayısal anahtar kesimi kullanılır (`:0:`, `:1:`,... `:{n}:`). Daha fazla bilgi için bkz. [Configuration: Bir diziyi @ no__t-0 sınıfına bağlayın.

Azure Key Vault anahtarlar ayırıcı olarak iki nokta üst üste kullanamaz. Bu konuda açıklanan yaklaşım, hiyerarşik değerler (bölümler) için bir ayırıcı olarak çift tire (`--`) kullanır. Dizi anahtarları Çift tire ve sayısal anahtar kesimlerle Azure Key Vault depolanır (`--0--`, `--1--`, &hellip; `--{n}--`).

Bir JSON dosyası tarafından sunulan aşağıdaki [Serilog](https://serilog.net/) günlük sağlayıcısı yapılandırmasını inceleyin. @No__t-0 dizisinde, günlük çıkışı için hedefleri açıklayan *Iki Serilog girişi*yansıtan iki nesne sabit değeri mevcuttur:

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

Önceki JSON dosyasında gösterilen yapılandırma Çift tire (`--`) gösterimi ve sayısal kesimleri kullanılarak Azure Key Vault depolanır:

| Anahtar | Value |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="reload-secrets"></a>Gizli dizileri yeniden yükleme

Gizlilikler `IConfigurationRoot.Reload()` çağrılana kadar önbelleğe alınır. Anahtar kasasındaki süre sonu, devre dışı ve güncelleştirilmiş gizli dizileri `Reload` yürütülene kadar uygulama tarafından dikkate alınır.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Devre dışı ve süre dolma parolaları

Devre dışı ve süresi biten gizlilikler, çalışma zamanında bir `KeyVaultClientException` oluşturur. Uygulamanın üretilmesini engellemek için, farklı bir yapılandırma sağlayıcısı kullanarak yapılandırmayı sağlayın veya devre dışı ya da süre dolma parolasını güncelleştirin.

## <a name="troubleshoot"></a>Sorun giderme

Uygulama, sağlayıcıyı kullanarak yapılandırmayı yükleyemediğinde, [ASP.NET Core günlük altyapısına](xref:fundamentals/logging/index)bir hata iletisi yazılır. Aşağıdaki koşullar yapılandırmanın yüklenmesine engel olur:

* Uygulama veya sertifika Azure Active Directory içinde doğru yapılandırılmamış.
* Anahtar Kasası Azure Key Vault içinde yok.
* Uygulamanın anahtar kasasına erişme yetkisi yok.
* Erişim ilkesi `Get` ve `List` izinleri içermez.
* Anahtar kasasında yapılandırma verileri (ad-değer çifti) yanlış olarak adlandırılmış, eksik, devre dışı veya zaman aşımına uğradı.
* Uygulamanın Anahtar Kasası adı (`KeyVaultName`), Azure AD uygulama kimliği (`AzureADApplicationId`) veya Azure AD sertifika parmak izi (`AzureADCertThumbprint`) vardır.
* Yüklemeye çalıştığınız değer için uygulamada yapılandırma anahtarı (ad) yanlış.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Key Vault @ no__t-0
* [Microsoft Azure: Key Vault belgeleri @ no__t-0
* [Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient sınıfı](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
* [Hızlı Başlangıç: .NET Web uygulaması kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma @ no__t-0
* [Öğretici: ' De Azure Windows sanal makinesi ile Azure Key Vault kullanma. NET @ no__t-0
