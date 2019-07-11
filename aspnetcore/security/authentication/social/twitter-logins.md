---
title: ASP.NET Core ile twitter dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Twitter hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: d816ed27898639b0af6896a51ac035d5526c5d29
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814066"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>ASP.NET Core ile twitter dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu örnek, kullanıcıların etkinleştirmek gösterilmektedir [kendi Twitter hesabıyla oturum](https://dev.twitter.com/web/sign-in/desktop-browser) kullanarak örnek bir ASP.NET Core 2.2 projesi oluşturduğunuza [önceki sayfaya](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>İçinde bir Twitter uygulaması oluşturma

* Gidin [ https://apps.twitter.com/ ](https://apps.twitter.com/) ve oturum açın. Twitter hesabıyla yoksa kullanın **[şimdi kaydolun](https://twitter.com/signup)** bağlantı oluşturmak için.

* Dokunun **yeni uygulama oluştur** ve uygulamanın ölçeğini doldurun **adı**, **açıklama** ve genel **Web sitesi** (Bu olabilir geçici dek URI'si etki alanı adı kayıt):

* Geliştirme sürecinizi URI girin ile `/signin-twitter` içine eklenen **geçerli OAuth yeniden yönlendirme URI'leri** alan (örneğin: `https://webapp128.azurewebsites.net/signin-twitter`). Bu örnekte, yapılandırılmış Twitter kimlik doğrulaması düzeni istekleri otomatik olarak işleyecek `/signin-twitter` OAuth akışını uygulamak için rota.

  > [!NOTE]
  > URI segmenti `/signin-twitter` Twitter kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır. Twitter kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) sınıf.

* Kalan formu doldurun ve dokunun **kendi Twitter uygulamanızı oluşturun**. Yeni uygulama ayrıntıları görüntülenir:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Twitter tüketici API anahtarınızı ve gizli dizi depolama

Güvenli bir şekilde depolamak için aşağıdaki komutları çalıştırın `ClientId` ve `ClientSecret` kullanarak [gizli dizi Yöneticisi](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

Bağlantı Twitter gibi hassas ayarları `Consumer Key` ve `Consumer Secret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets). Bu örnek amacı doğrultusunda, belirteçleri ad `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`.

Bu belirteçler bulunabilir **anahtarlar ve erişim belirteçleri** sekmesinde yeni bir Twitter uygulaması oluşturduktan sonra:

## <a name="configure-twitter-authentication"></a>Twitter kimlik doğrulamasını yapılandırma

Twitter hizmetinde ekleme `ConfigureServices` yönteminde *Startup.cs* dosyası:

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Bkz: [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Twitter kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-twitter"></a>Oturum Twitter ile oturum aç

Uygulamayı çalıştırmak ve seçmek **oturum**. Twitter ile oturum açmak için bir seçenek görüntülenir:

Tıklayarak **Twitter** kimlik doğrulaması için Twitter'a yönlendirir:

Twitter kimlik bilgilerinizi girdikten sonra web, e-posta ayarlayabileceğiniz siteye geri yönlendirilir.

Artık Twitter kimlik bilgilerinizi kullanarak günlüğe kaydedilir:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* **ASP.NET Core 2.x yalnızca:** Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu örnekte kullanılan proje şablonu, bu gerçekleştirilir sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Twitter ile kimliğini nasıl doğrulayabileceğiniz gösterdi. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).

* Web sitenizi Azure web uygulaması yayımladıktan sonra sıfırlamalısınız `ConsumerSecret` Twitter Geliştirici Portalı.

* Ayarlama `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.