# <a name="key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Anahtar kasası yapılandırma sağlayıcısı örnek uygulaması (ASP.NET Core 1.x)

Bu örnek ASP.NET Core için Azure anahtar kasası yapılandırma sağlayıcısı kullanımını göstermektedir 1.x. ASP.NET Core 2.x örnek için bkz: [anahtar kasası yapılandırma sağlayıcısı örnek uygulaması (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x).

> [!NOTE]
> Yapılandırma sağlayıcısı, ASP.NET Core 1.0 için kullanılamaz. Yapılandırma sağlayıcısı uygulamak istediğinize ve ASP.NET Core 1.0 uygulama uygulama ise, uygulama 1.1 veya üstünün ilk yükseltin.

Örnek nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure anahtar kasası yapılandırma sağlayıcısı](xref:security/key-vault-configuration) konu.

## <a name="using-the-sample"></a>Örnek kullanma
1. Bir anahtar kasası oluşturun ve yer alan yönergeleri izleyerek uygulama için Azure Active Directory'yi (Azure AD) ayarlama ayarlayın [Azure anahtar kasası ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Gizli anahtar kasası kullanmaya eklemek [AzureRM anahtar kasası PowerShell Modülü](/powershell/module/azurerm.keyvault) kullanılabilir [PowerShell Galerisi](https://www.powershellgallery.com/packages/AzureRM.KeyVault), [Azure anahtar kasası REST API](/rest/api/keyvault/), veya [Azure Portal](https://portal.azure.com/). Gizli ya da oluşturulan *el ile* veya *sertifika* gizli. *Sertifika* gizli uygulamalar ve hizmetler tarafından kullanılması için sertifikalar ancak yapılandırma sağlayıcısı tarafından desteklenmiyor. Kullanmanız gereken *el ile* yapılandırma sağlayıcısı ile kullanmak için ad-değer çifti parolaları oluşturmak için seçeneği.
     * Basit gizli ad-değer çiftleri olarak oluşturulur. Azure anahtar kasası gizli adların, alfasayısal karakterler ve tire sınırlıdır.
     * Hiyerarşik değerleri (yapılandırma bölümlerinin) kullanmak `--` (iki kısa çizgi) örnek ayırıcı olarak. Normalde bir alt anahtarda bölümünden sınırlandırmak için kullanılan iki nokta üst üste, [ASP.NET Core yapılandırma](xref:fundamentals/configuration/index), gizli adlarında izin verilmez. Bu nedenle, iki kısa çizgi kullanılan ve gizli anahtarları uygulamanın yapılandırma yüklendiğinde bir iki nokta üst üste takas.
     * İki oluşturmak *el ile* aşağıdaki ad-değer çiftleri ile gizli. İlk gizli bir basit bir ad ve değer olmadığından ve ikinci gizli bir bölüm ve alt gizli adında gizli bir değer oluşturur:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Örnek uygulamayı Azure Active Directory ile kaydedin.
   * Anahtar kasası erişmek için uygulamasını yetkilendirin. Kullandığınızda `Set-AzureRmKeyVaultAccessPolicy` anahtar kasası erişmek için uygulamasını yetkilendirmek için PowerShell cmdlet sağlamak `List` ve `Get` gizli ile erişimi `-PermissionsToSecrets list,get`.

2. Uygulamanın güncelleştirme *appsettings.json* değerlerini dosyasıyla `Vault`, `ClientId`, ve `ClientSecret`.
3. Kendi yapılandırma değerlerini alır örnek uygulamayı çalıştırma `IConfigurationRoot` gizli adıyla aynı ada sahip.
   * Hiyerarşik olmayan değerleri: değeri `SecretName` ile elde `config["SecretName"]`.
   * Hiyerarşik değerleri (bölümler): kullanım `:` (iki nokta üst üste) gösterimi veya `GetSection` genişletme yöntemi. Yapılandırma değeri elde etmek için Bu yaklaşımlardan birini kullanın:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`
