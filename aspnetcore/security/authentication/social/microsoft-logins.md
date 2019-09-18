---
title: ASP.NET Core ile Microsoft hesabı dış oturum açma kurulumu
author: rick-anderson
description: Bu örnek, Microsoft hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesini gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 91ace293fd16cd180b3d5c183c637af6db1d08c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082335"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>ASP.NET Core ile Microsoft hesabı dış oturum açma kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu örnekte, kullanıcıların [önceki sayfada](xref:security/authentication/social/index)oluşturulan ASP.NET Core 2,2 projesini kullanarak Microsoft hesabı oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir.

## <a name="create-the-app-in-microsoft-developer-portal"></a>Uygulamayı Microsoft Geliştirici Portalında oluşturma

* [Azure portal uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin ve bir Microsoft hesabı oluşturun veya oturum açın:

Microsoft hesabı yoksa **bir tane oluştur**' u seçin. Oturum açtıktan sonra **uygulama kayıtları** sayfasına yönlendirilirsiniz:

* **Yeni kayıt** Seç
* Bir **ad**girin.
* **Desteklenen hesap türleri**için bir seçenek belirleyin.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* **Yeniden yönlendirme URI 'si**altında, `/signin-microsoft` eklenen geliştirme URL 'nizi girin. Örneğin: `https://localhost:44389/signin-microsoft`. Bu örnekte daha sonra yapılandırılan Microsoft kimlik doğrulama şeması, OAuth akışını uygulamak için `/signin-microsoft` rotadaki istekleri otomatik olarak işleymeyecektir.
* **Kaydol** ' u seçin

### <a name="create-client-secret"></a>İstemci parolası oluştur

* Sol bölmede **sertifikalar & gizlilikler**' ı seçin.
* **İstemci gizli**dizileri altında **yeni istemci parolası** ' nı seçin.

  * İstemci parolası için bir açıklama ekleyin.
  * **Ekle** düğmesini seçin.

* **İstemci**gizli dizileri altında, istemci parolasının değerini kopyalayın.

> [!NOTE]
> URI segmenti `/signin-microsoft` , Microsoft kimlik doğrulama sağlayıcısı 'nın varsayılan geri çağırması olarak ayarlanır. Microsoft kimlik doğrulama ara yazılımını, [Microsoftaccountoptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) sınıfının devralınmış [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği aracılığıyla YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Microsoft istemci KIMLIĞINI ve istemci gizli anahtarını depolayın

`ClientId` [Gizli yönetimi](xref:security/app-secrets)güvenli bir şekilde depolamak ve `ClientSecret` kullanmak için aşağıdaki komutları çalıştırın:

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Gizli ayarları, `ClientId` [gizli Yöneticisi](xref:security/app-secrets)kullanarak `ClientSecret` Microsoft ve uygulama yapılandırmanıza bağlayın. Bu örneğin amaçları doğrultusunda belirteçleri `Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret`' ı adlandırın.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Microsoft hesabı kimlik doğrulamasını yapılandırma

Microsoft hesabı hizmetini şu şekilde `Startup.ConfigureServices`ekleyin:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Microsoft hesabı kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. [Microsoftaccountoptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API başvurusu. Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-microsoft-account"></a>Microsoft hesabıyla oturum açın hesabı

Öğesini çalıştırın ve **oturum aç**' a tıklayın. Microsoft ile oturum açma seçeneği görüntülenir. Microsoft 'a tıkladığınızda, kimlik doğrulaması için Microsoft 'a yönlendirilirsiniz. Microsoft hesabınızla oturum açtıktan sonra (henüz oturum açmadıysanız), uygulamanın bilgilerinize erişmesine izin vermek isteyip istemediğiniz sorulur:

**Evet** ' e dokunduktan sonra, e-postanızı ayarlayabileceğiniz Web sitesine geri yönlendirilirsiniz.

Şu anda Microsoft kimlik bilgilerinizi kullanarak oturum açtınız:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* Microsoft hesabı sağlayıcısı sizi bir oturum açma hatası sayfasına yönlendirirse, URI 'deki `#` (hashtag) hata başlığına ve açıklama sorgu dizesi parametrelerine göz önüne alın.

  Hata iletisi Microsoft kimlik doğrulamasıyla ilgili bir sorun olduğunu gösteriyorsa, en sık karşılaşılan neden, uygulama URI 'niz **Web** platformu Için belirtilen **yeniden yönlendirme URI** 'lerinden hiçbiriyle eşleşmemedir.
* Kimlik ' de `services.AddIdentity` `ConfigureServices`çağırarak yapılandırılmamışsa, kimlik doğrulaması girişimi *ArgumentException ile sonuçlanır: ' Signınscheme ' seçeneği sağlanmalıdır*. Bu örnekte kullanılan proje şablonu bunun yapılmasını sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makale, Microsoft ile nasıl kimlik doğrulayacağınızı gösterdi. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).

* Web sitenizi Azure Web App 'e yayımladığınızda, Microsoft Geliştirici Portalında yeni bir istemci gizli dizileri oluşturun.

* Ayarlama `Authentication:Microsoft:ClientId` ve `Authentication:Microsoft:ClientSecret` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.