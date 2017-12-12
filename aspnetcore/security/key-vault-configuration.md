---
title: "Azure anahtar kasası yapılandırma sağlayıcısı"
author: guardrex
description: "Çalışma zamanında yüklenen ad-değer çiftleri kullanarak bir uygulamayı yapılandırmak için Azure anahtar kasası yapılandırma Sağlayıcısı'nı kullanmayı öğrenin."
keywords: "ASP.NET Core, yapılandırma, Azure anahtar kasası"
ms.author: riande
manager: wpickett
ms.date: 08/09/2017
ms.topic: article
ms.assetid: 0292bdae-b3ed-4637-bd67-19b9bb8b65cb
ms.prod: asp.net-core
uid: security/key-vault-configuration
ms.openlocfilehash: 352d125b9042c603b59ed9bda0e99b6a49c7ab9f
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="azure-key-vault-configuration-provider"></a>Azure anahtar kasası yapılandırma sağlayıcısı

Tarafından [Luke Latham](https://github.com/guardrex) ve [Barış Stanton-Nurse](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

Görüntülemek veya karşıdan 2.x için örnek kod:

* [Temel örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))-gizli değerleri uygulamaya okur.
* [Anahtar adı öneki örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) - gizli değerleri her uygulama sürümü için farklı bir kümesini yüklemek izin veren bir uygulamanın sürümünü temsil eden bir anahtar adı ön ekini kullanarak gizli değerleri okur.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

Görüntülemek veya karşıdan 1.x için örnek kod:

* [Temel örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))-gizli değerleri uygulamaya okur.
* [Anahtar adı öneki örnek](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)) - gizli değerleri her uygulama sürümü için farklı bir kümesini yüklemek izin veren bir uygulamanın sürümünü temsil eden bir anahtar adı ön ekini kullanarak gizli değerleri okur. 

---

Bu belge nasıl kullanılacağını açıklamaktadır [Microsoft Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) Azure anahtar kasası gizli uygulama yapılandırma değerlerini yüklemek için yapılandırma sağlayıcısı. Azure anahtar kasası, şifreleme anahtarları ve gizli anahtarları uygulamalar ve hizmetler tarafından kullanılan korumaya yardımcı olan bir bulut tabanlı bir hizmettir. Önemli yapılandırma verilerine erişimi denetleme yaygın senaryolar içerir ve FIPS 140-2 gereksinimini toplantı Düzey 2 donanım güvenlik modülleri (HSM's) yapılandırma verileri depolarken doğrulanabilir. Bu özellik, daha yüksek veya ASP.NET Core 1.1 hedefleyen uygulamalar için kullanılabilir.

## <a name="package"></a>Paket
Sağlayıcı kullanmak için bir başvuru ekleyin [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) paket.

## <a name="application-configuration"></a>Uygulama yapılandırması
Sağlayıcı ile keşfedebilirsiniz [örnek uygulamaları](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Bir anahtar kasası oluşturmak ve kasasına gizli anahtarları oluşturmak sonra örnek uygulamaları güvenli bir şekilde gizli değerlerini yapılandırmalarına yüklemek ve Web sayfalarındaki görüntüleme.

Sağlayıcı eklenen `ConfigurationBuilder` ile `AddAzureKeyVault` uzantısı. Örnek uygulamaları uzantısı gelen yüklenen üç yapılandırma değerlerini kullanır. *appsettings.json* dosya.

| Uygulama ayarı    | Açıklama                    | Örnek                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure anahtar kasası adı           | contosovault                                 |
| `ClientId`     | Azure Active Directory Uygulama Kimliği  | 627e911e-43CC-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory Uygulama anahtarı | g58K3dtg59o1Pa + e59v2Tx829w6VxTB2yv9sv/101di = |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Anahtar kasasına gizli anahtarları oluşturma ve yapılandırma değerlerini (basic örnek) yükleme
1. Bir anahtar kasası oluşturun ve yer alan yönergeleri izleyerek uygulama için Azure Active Directory'yi (Azure AD) ayarlama ayarlayın [Azure anahtar kasası ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Gizli anahtar kasası kullanmaya eklemek [AzureRM anahtar kasası PowerShell Modülü](/powershell/module/azurerm.keyvault) kullanılabilir [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [Azure anahtar kasası REST API](/rest/api/keyvault/), veya [Azure Portal](https://portal.azure.com/). Gizli ya da oluşturulan *el ile* veya *sertifika* gizli. *Sertifika* gizli uygulamalar ve hizmetler tarafından kullanılması için sertifikalar ancak yapılandırma sağlayıcısı tarafından desteklenmiyor. Kullanmanız gereken *el ile* yapılandırma sağlayıcısı ile kullanmak için ad-değer çifti parolaları oluşturmak için seçeneği.
    * Basit gizli ad-değer çiftleri olarak oluşturulur. Azure anahtar kasası gizli adların, alfasayısal karakterler ve tire sınırlıdır.
    * Hiyerarşik değerleri (yapılandırma bölümlerinin) kullanmak `--` (iki kısa çizgi) örnek ayırıcı olarak. Normalde bir alt anahtarda bölümünden sınırlandırmak için kullanılan iki nokta üst üste, [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index), gizli adlarında izin verilmez. Bu nedenle, iki kısa çizgi kullanılan ve gizli anahtarları uygulamanın yapılandırma yüklendiğinde bir iki nokta üst üste takas.
    * İki oluşturmak *el ile* aşağıdaki ad-değer çiftleri ile gizli. İlk gizli bir basit bir ad ve değer olmadığından ve ikinci gizli bir bölüm ve alt gizli adında gizli bir değer oluşturur:
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * Örnek uygulamayı Azure Active Directory ile kaydedin.
  * Anahtar kasası erişmek için uygulamasını yetkilendirin. Kullandığınızda `Set-AzureRmKeyVaultAccessPolicy` anahtar kasası erişmek için uygulamasını yetkilendirmek için PowerShell cmdlet sağlamak `List` ve `Get` gizli ile erişimi `-PermissionsToKeys list,get`.
2. Uygulamanın güncelleştirme *appsettings.json* değerlerini dosyasıyla `Vault`, `ClientId`, ve `ClientSecret`.
3. Kendi yapılandırma değerlerini alır örnek uygulamayı çalıştırma `IConfigurationRoot` gizli adıyla aynı ada sahip.
  * Hiyerarşik olmayan değerleri: değeri `SecretName` ile elde `config["SecretName"]`.
  * Hiyerarşik değerleri (bölümler): kullanım `:` (iki nokta üst üste) gösterimi veya `GetSection` genişletme yöntemi. Yapılandırma değeri elde etmek için Bu yaklaşımlardan birini kullanın:
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`

Uygulamayı çalıştırdığınızda, bir Web sayfası yüklenen gizli değerleri gösterir:

![Azure anahtar kasası yapılandırma sağlayıcısı aracılığıyla yüklenen gizli değerleri gösteren bir tarayıcı penceresi](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Önekli anahtar kasasına gizli anahtarları oluşturma ve yapılandırma değerlerini (anahtar-adı-önek-sample) yükleme
`AddAzureKeyVault`Ayrıca uygulaması kabul eden bir aşırı sağlar `IKeyVaultSecretManager`, nasıl anahtar kasasına gizli anahtarları denetlemenize olanak sağlayan yapılandırma anahtarlara dönüştürülür. Örneğin, uygulama başlatma sırasında sağladığınız bir önek değere göre gizli değerlerini yüklemek için arabirimi uygulayabilirsiniz. Bu, örneğin, uygulama sürümüne gizli yüklemek için sağlar.

> [!WARNING]
> Önekleri anahtar kasası parolaları parolaları birden fazla uygulama için aynı anahtar kasasını yerleştirmek için veya çevre gizli yerleştirmek üzere kullanma (örneğin, *geliştirme* verus *üretim* gizli) aynı içine Kasa. Farklı uygulamalar ve geliştirme/üretim ortamlarında ayrı anahtar kasalarını uygulama ortamları için yüksek düzeyde güvenlik yalıtmak için kullanmanızı öneririz.

İkinci örnek uygulaması kullanarak, bir gizli anahtar kasası için oluşturduğunuz `5000-AppSecret` (anahtar kasası gizli adlarında nokta izin verilmiyor) temsil eden 5.0.0.0 sürümü, uygulamanız için bir uygulama gizli anahtarı. Başka bir sürümü için 5.1.0.0, için gizli anahtar oluşturma `5100-AppSecret`. Her uygulamanın sürüm yapılandırmasıyla gizli değerini yükler `AppSecret`, çıkarma sürümü gizli yüklerken. Örnek 's uygulama aşağıda gösterilmiştir:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

`Load` Yöntemi sürüm önekine sahip olanları bulmak için kasa gizli tekrarlanan bir sağlayıcı algoritması tarafından çağrılır. Ne zaman bir sürüm önek bulundu ile `Load`, algoritma `GetKey` gizli yapılandırma adı döndürülecek yöntemi. Parolanın ad sürüm önekten kapalı kaldırır ve yüklenmesi için gizli anahtar adı kalan ad-değer çiftleri uygulamanın yapılandırma döndürür.

Ne zaman bu yaklaşımı uygulayın:

1. Anahtar kasasına gizli anahtarları yüklenir.
2. Dize gizliliğini `5000-AppSecret` eşleştirilir.
3. Sürüm `5000` (tireyle), bırakarak anahtar adı dışına yapılandırıldıktan `AppSecret` gizli değerle uygulamanın yapılandırma yüklenemedi.

> [!NOTE]
> Ayrıca kendi sağlayabilirsiniz `KeyVaultClient` uygulamasına `AddAzureKeyVault`. Özel bir istemci sağladığını istemci yapılandırma sağlayıcısı ve uygulamanızdaki diğer bölümleri arasında tek bir örneğini paylaşmanıza olanak tanır.

1. Bir anahtar kasası oluşturun ve yer alan yönergeleri izleyerek uygulama için Azure Active Directory'yi (Azure AD) ayarlama ayarlayın [Azure anahtar kasası ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Gizli anahtar kasası kullanmaya eklemek [AzureRM anahtar kasası PowerShell Modülü](/powershell/module/azurerm.keyvault) kullanılabilir [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [Azure anahtar kasası REST API](/rest/api/keyvault/), veya [Azure Portal](https://portal.azure.com/). Gizli ya da oluşturulan *el ile* veya *sertifika* gizli. *Sertifika* gizli uygulamalar ve hizmetler tarafından kullanılması için sertifikalar ancak yapılandırma sağlayıcısı tarafından desteklenmiyor. Kullanmanız gereken *el ile* yapılandırma sağlayıcısı ile kullanmak için ad-değer çifti parolaları oluşturmak için seçeneği.
    * Hiyerarşik değerleri (yapılandırma bölümlerinin) kullanmak `--` (iki kısa çizgi) ayırıcı olarak.
    * İki oluşturmak *el ile* parolaları aşağıdaki ad-değer çiftleri ile:
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Örnek uygulamayı Azure Active Directory ile kaydedin.
  * Anahtar kasası erişmek için uygulamasını yetkilendirin. Kullandığınızda `Set-AzureRmKeyVaultAccessPolicy` anahtar kasası erişmek için uygulamasını yetkilendirmek için PowerShell cmdlet sağlamak `List` ve `Get` gizli ile erişimi `-PermissionsToKeys list,get`.
2. Uygulamanın güncelleştirme *appsettings.json* değerlerini dosyasıyla `Vault`, `ClientId`, ve `ClientSecret`.
3. Kendi yapılandırma değerlerini alır örnek uygulamayı çalıştırma `IConfigurationRoot` ile aynı adı taşıyan önekli gizli. Bu örnekte, için sağlanan uygulamanın sürüm önektir `PrefixKeyVaultSecretManager` Azure anahtar kasası yapılandırma sağlayıcısı eklediğiniz zaman. Değeri `AppSecret` ile elde `config["AppSecret"]`. Uygulama tarafından oluşturulan Web sayfası yüklenen değer gösterir:

   ![Uygulamanın sürüm 5.0.0.0 olduğunda Azure anahtar kasası yapılandırma sağlayıcısı aracılığıyla yüklenen gizli bir değer gösteren bir tarayıcı penceresi](key-vault-configuration/_static/sample2-1.png)

4. Proje dosyasından uygulama derlemede sürümünü değiştirmek `5.0.0.0` için `5.1.0.0` ve uygulamayı yeniden çalıştırın. Bu süre, döndürülen gizli değerdir `5.1.0.0_secret_value`. Uygulama tarafından oluşturulan Web sayfası yüklenen değer gösterir:

   ![Uygulamanın sürüm 5.1.0.0 olduğunda Azure anahtar kasası yapılandırma sağlayıcısı aracılığıyla yüklenen gizli bir değer gösteren bir tarayıcı penceresi](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>ClientSecret erişimi denetleme
Kullanım [gizli Yöneticisi aracını](xref:security/app-secrets) korumak için `ClientSecret` proje kaynak ağacı dışında. Uygulama parolaları belirli bir proje ile ilişkilendirmek ve birden çok projeler arasında paylaşmak gizli Manager'la.

Sertifikaları destekleyen bir ortamda bir .NET Framework uygulama geliştirirken, Azure anahtar Kasası'na bir X.509 sertifikası ile doğrulanabilir. X.509 sertifikasının özel anahtarı işletim sistemi tarafından yönetilir. Daha fazla bilgi için bkz: [istemci parolası yerine bir sertifika ile kimlik doğrulama](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Kullanım `AddAzureKeyVault` kabul aşırı bir `X509Certificate2`.

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>Parolaları yeniden yükleniyor
Parolaları önbelleğe kadar `IConfigurationRoot.Reload()` olarak adlandırılır. Süresi doldu, devre dışı bırakıldı ve güncelleştirilmiş gizli anahtar kasasında değil kadar uygulama tarafından dikkate `Reload` yürütülür.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Devre dışı bırakılmış ve süresi dolan gizli
Devre dışı bırakılmış ve süresi dolan gizli throw bir `KeyVaultClientException`. Uygulamanızı atma gelen önlemek için uygulamanızı değiştirmek veya devre dışı bırakılmış/süresi dolmuş gizli güncelleştirin.

## <a name="troubleshooting"></a>Sorun giderme
Yapılandırma Sağlayıcısı'nı kullanarak yüklemek uygulama başarısız olduğunda bir hata iletisi yazılır [ASP.NET oturum altyapı](xref:fundamentals/logging/index). Aşağıdaki koşullar yapılandırma yüklenmesini engeller:
* Uygulama Azure Active Directory'de doğru yapılandırılmamış.
* Anahtar kasası Azure anahtar kasası yok.
* Uygulama anahtar kasası erişmek için yetkili değil.
* Erişim İlkesi içermeyen `Get` ve `List` izinleri.
* Anahtar kasasına yapılandırma verileri (ad-değer çifti) yanlış, eksik, devre dışı ya da süresi dolmuş olarak adlandırılır.
* Uygulama yanlış anahtar kasasının adına sahip (`Vault`), Azure AD uygulama kimliği (`ClientId`), ya da Azure AD anahtarı (`ClientSecret`).
* Azure AD anahtarı (`ClientSecret`) süresi doldu.
* Yapılandırma anahtarı (ad) uygulama yüklemeye çalıştığınız değer için geçersiz.

## <a name="additional-resources"></a>Ek kaynaklar
* [Yapılandırma](xref:fundamentals/configuration/index)
* [Microsoft Azure: Anahtar kasası](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Anahtar kasası belgeleri](https://docs.microsoft.com/azure/key-vault/)
* [Azure anahtar kasası için nasıl oluşturma ve aktarma HSM korumalı anahtarları](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
