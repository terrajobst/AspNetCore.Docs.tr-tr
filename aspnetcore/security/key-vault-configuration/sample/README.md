# <a name="key-vault-configuration-provider-sample-app"></a>Anahtar kasası yapılandırma sağlayıcısı örnek uygulaması

Bu örnek, Azure Key Vault yapılandırma sağlayıcısı kullanımını gösterir.

Örnek tarafından belirlenen iki moddan birini çalıştırır `#define` en üstündeki deyimi *Program.cs* dosya. Yönergeler için [önişlemci yönergeleri örnek kodda](https://docs.microsoft.com/aspnet/core#preprocessor-directives-in-sample-code):

* `Basic` &ndash; Bir Azure anahtar kasası istemci kimliği ve gizli dizi için erişim gizli dizilerini Azure Key Vault'ta depolanan kullanımını gösterir. Örnek bu sürümü, Azure App Service veya ASP.NET Core uygulaması hizmet herhangi bir konağa dağıtılan herhangi bir yerden çalıştırılabilir.
* `Managed` &ndash; Azure'nın nasıl yapılacağı açıklanır [yönetilen hizmet kimliği](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) uygulamanın kod veya yapılandırma kimlik bilgileri olmadan Azure AD kimlik doğrulaması ile Azure anahtar kasası uygulamaya kimlik doğrulaması için. Bir Azure AD İstemci Kimliğini ve gizli anahtarı Azure Key Vault ile kimlik doğrulaması yapmak uygulama için gerekli değildir. Bu örnek yönetilen kimliği scearnio keşfetmek için Azure App Service'e dağıtılması gerekir.

Daha fazla bilgi için [Azure Key Vault yapılandırma sağlayıcısı](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration).
