# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Anahtar kasası yapılandırma sağlayıcısı örnek uygulama önek (ASP.NET Core 1.x)

Bu örnek ASP.NET Core için Azure anahtar kasası yapılandırma sağlayıcısı kullanımını göstermektedir anahtar adı önekleri kullanarak 1.x. ASP.NET Core 2.x örnek için bkz: [önek anahtar kasası yapılandırma sağlayıcısı örnek uygulaması (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x).

> [!NOTE]
> Yapılandırma sağlayıcısı, ASP.NET Core 1.0 için kullanılamaz. Yapılandırma sağlayıcısı uygulamak istediğinize ve ASP.NET Core 1.0 uygulama uygulama ise, uygulama 1.1 veya üstünün ilk yükseltin.

Örnek nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure anahtar kasası yapılandırma sağlayıcısı](xref:security/key-vault-configuration) konu.

## <a name="using-the-sample"></a>Örnek kullanma
1. Bir anahtar kasası oluşturun ve yer alan yönergeleri izleyerek uygulama için Azure Active Directory'yi (Azure AD) ayarlama ayarlayın [Azure anahtar kasası ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Gizli anahtarları Azure PowerShell modülü, Azure yönetim API'si veya Azure Portalı'nı kullanarak anahtar Kasası'na ekleyin. Gizli ya da oluşturulan *el ile* veya *sertifika* gizli. *Sertifika* gizli uygulamalar ve hizmetler tarafından kullanılması için sertifikalar ancak yapılandırma sağlayıcısı tarafından desteklenmiyor. Kullanmanız gereken *el ile* yapılandırma sağlayıcısı ile kullanmak için ad-değer çifti parolaları oluşturmak için seçeneği.
    * Hiyerarşik değerleri (yapılandırma bölümlerinin) kullanmak `--` (iki kısa çizgi) ayırıcı olarak.
    * Örnek uygulama için iki oluşturmak *el ile* parolaları aşağıdaki ad-değer çiftleri ile:
      * `5000-AppSecret`: `5.0.0.0_secret_value`
      * `5100-AppSecret`: `5.1.0.0_secret_value`
  * Örnek uygulamayı Azure Active Directory ile kaydedin.
  * Anahtar kasası erişmek için uygulamasını yetkilendirin. Kullandığınızda `Set-AzureRmKeyVaultAccessPolicy` anahtar kasası erişmek için uygulamasını yetkilendirmek için PowerShell cmdlet sağlamak `List` ve `Get` gizli ile erişimi `-PermissionsToSecrets list,get`.
2. Uygulamanın güncelleştirme *appsettings.json* değerlerini dosyasıyla `Vault`, `ClientId`, ve `ClientSecret`.
3. Kendi yapılandırma değerlerini alır örnek uygulamayı çalıştırma `IConfigurationRoot` ile aynı adı taşıyan önekli gizli. Bu örnekte, için sağlanan uygulamanın sürüm önektir `PrefixKeyVaultSecretManager` Azure anahtar kasası yapılandırma sağlayıcısı eklediğiniz zaman. Değeri `AppSecret` ile elde `config["AppSecret"]`.
4. Proje dosyasından uygulama derlemede sürümünü değiştirmek `5.0.0.0` için `5.1.0.0` ve uygulamayı yeniden çalıştırın. Bu süre, döndürülen gizli değerdir `5.1.0.0_secret_value`.
