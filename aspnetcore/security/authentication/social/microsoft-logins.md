---
title: ASP.NET Core ile Microsoft Account dış oturum açma Kurulumu
author: rick-anderson
description: Bu örnek, mevcut bir ASP.NET Core uygulamasına Microsoft hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 2c690e5bd8465806d42091616917cfdd747ef8f0
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815570"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>ASP.NET Core ile Microsoft Account dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu örnek üzerinde oluşturulan ASP.NET Core 2.2 projesini kullanarak kendi Microsoft hesabıyla oturum açmasını sağlamak gösterilmektedir [önceki sayfaya](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Microsoft Geliştirici portalında uygulaması oluşturma

* Gidin [Azure Portalı - Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasında oluşturarak veya mevcut bir Microsoft hesabı olarak oturum açın:

Bir Microsoft hesabınız yoksa, seçin **oluşturmak**. Oturum açtıktan sonra yönlendirilirsiniz **uygulama kayıtları** sayfası:

* Seçin **yeni kayıt**
* Girin bir **adı**.
* Bir seçeneğini **desteklenen hesap türleri**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* Altında **yeniden yönlendirme URI'si**, geliştirme URL'nizi girin `/signin-microsoft` eklenir. Örneğin: `https://localhost:44389/signin-microsoft`. Bu örnekte, yapılandırılmış Microsoft kimlik doğrulama düzeni istekleri otomatik olarak işleyecek `/signin-microsoft` OAuth akışını uygulamak için rota.
* Seçin **kaydetme**

### <a name="create-client-secret"></a>İstemci gizli anahtarı oluşturma

* Sol bölmede seçin **sertifikaları ve parolaları**.
* Altında **istemci gizli dizileri**seçin **yeni istemci gizli anahtarı**

  * İstemci gizli anahtarı için bir açıklama ekleyin.
  * Seçin **Ekle** düğmesi.

* Altında **istemci gizli dizileri**, gizli anahtar değerini kopyalayın.

> [!NOTE]
> URI segmenti `/signin-microsoft` Microsoft kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır. Microsoft kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) sınıfı.

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Microsoft istemci Kimliğini ve istemci gizli Store

Güvenli bir şekilde depolamak için aşağıdaki komutları çalıştırın `ClientId` ve `ClientSecret` kullanarak [gizli dizi Yöneticisi](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Bağlantı Microsoft gibi hassas ayarlar `ClientId` ve `ClientSecret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets). Bu örnek amacı doğrultusunda, belirteçleri ad `Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Microsoft hesabı kimlik doğrulamasını yapılandırma

Microsoft Account hizmetini için ekleme `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Bkz: [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Microsoft Account kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-microsoft-account"></a>Microsoft hesabıyla oturum açın

Çalıştırma tıklatıp **oturum**. Microsoft'ta oturum açmak için bir seçenek görüntülenir. Microsoft'a tıkladığınızda, kimlik doğrulaması için Microsoft'a yönlendirilir. (Zaten açtıysanız) Microsoft Account imzaladıktan sonra uygulamayı bilgilere erişmesine izin istenir:

Dokunun **Evet** ve web e-postanızı ayarlayabileceğiniz siteye geri yönlendirilir.

Artık, Microsoft kimlik bilgilerinizi kullanarak kaydedilir:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* Microsoft Account sağlayıcısı oturum açma hatası sayfasına yönlendirir, doğrudan aşağıdaki hata başlığı ve açıklamayı sorgu dizesi parametrelerini Not `#` (diyez etiketi) URI.

  Microsoft kimlik doğrulama ile ilgili bir sorun göstermek için hata iletisini gibi görünüyor ancak uygulamanızı herhangi bir eşleşmeyen URI en yaygın nedeni **yeniden yönlendirme URI'leri** için belirtilen **Web** platformu .
* Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu örnekte kullanılan proje şablonu, bu gerçekleştirilir sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Microsoft ile kimliğini nasıl doğrulayabileceğiniz gösterdi. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).

* Web sitenizi Azure web uygulaması yayımladıktan sonra yeni bir istemci gizli dizileri Microsoft Geliştirici Portalı'nda oluşturun.

* Ayarlama `Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.