---
title: ASP.NET Core ile Twitter dışarıdan oturum açma kurulumu
author: rick-anderson
description: Bu öğreticide, Twitter hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesi gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 6b6fa3e50f602a92fec9112ac3ba43583de33a70
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994276"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>ASP.NET Core ile Twitter dışarıdan oturum açma kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu örnek, [önceki sayfada](xref:security/authentication/social/index)oluşturulmuş bir örnek ASP.NET Core 2,2 projesi kullanarak kullanıcıların [Twitter hesabıyla oturum açmasını](https://dev.twitter.com/web/sign-in/desktop-browser) nasıl olanaklı hale kullanabileceğinizi gösterir.

## <a name="create-the-app-in-twitter"></a>Uygulamayı Twitter 'da oluşturma

* Gidin [ https://apps.twitter.com/ ](https://apps.twitter.com/) ve oturum açın. Zaten bir Twitter hesabınız yoksa, oluşturmak için **[Şimdi kaydolun](https://twitter.com/signup)** bağlantısını kullanın.

* **Yeni uygulama oluştur** ' a dokunun ve uygulama **adı**, **Açıklama** ve genel **Web sitesi** URI 'sini doldurun (etki alanı adı kaydoluncaya kadar geçici olabilir):

* `/signin-twitter` **Geçerli OAuth yeniden yönlendirme URI 'leri** alanına eklenmiş olan geliştirme URI 'nizi girin (örneğin: `https://webapp128.azurewebsites.net/signin-twitter`). Bu örnekte daha sonra yapılandırılan Twitter kimlik doğrulama şeması, OAuth akışını uygulamak için `/signin-twitter` rotadaki istekleri otomatik olarak işler.

  > [!NOTE]
  > URI segmenti `/signin-twitter` Twitter kimlik doğrulama sağlayıcısının varsayılan geri çağırması olarak ayarlanır. Twitter kimlik doğrulama ara yazılımını, ' ın devralınan [Remoteauthenticationoptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliğini kullanarak yapılandırırken varsayılan GERI çağırma URI 'sini değiştirebilirsiniz. [](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions)

* Formun geri kalanını doldurun ve **Twitter uygulamanızı oluştur**' a dokunun. Yeni uygulama ayrıntıları görüntülenir:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Twitter tüketici API anahtarı ve gizli dizisi depolanıyor

`ClientId` [Gizli yönetimi](xref:security/app-secrets)güvenli bir şekilde depolamak ve `ClientSecret` kullanmak için aşağıdaki komutları çalıştırın:

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Gizli ayarları Twitter `Consumer Key` gibi hassas ayarları `Consumer Secret` ve uygulama yapılandırmanızı [gizli yönetici](xref:security/app-secrets)kullanarak bağlayın. Bu örneğin amaçları doğrultusunda belirteçleri `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`' ı adlandırın.

Bu belirteçler, yeni bir Twitter uygulaması oluşturduktan sonra **anahtarlar ve erişim belirteçleri** sekmesinde bulunabilir:

## <a name="configure-twitter-authentication"></a>Twitter kimlik doğrulamasını yapılandırma

Twitter hizmetini `ConfigureServices` *Startup.cs* dosyasındaki yöntemine ekleyin:

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Twitter kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için, bkz. [dallı ve seçenekleri](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API başvurusu. Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-twitter"></a>Twitter ile oturum açma

Uygulamayı çalıştırın ve **oturum aç '** ı seçin. Twitter ile oturum açma seçeneği görünür:

**Twitter** 'ya tıkladığınızda, kimlik doğrulaması için Twitter 'a yeniden yönlendirme yapılır:

Twitter kimlik bilgilerinizi girdikten sonra, e-postanızı ayarlayabileceğiniz Web sitesine geri yönlendirilirsiniz.

Twitter kimlik bilgilerinizi kullanarak oturumunuz açıldı:

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* **Yalnızca 2. x ASP.NET Core:** Kimlik ' de `services.AddIdentity` `ConfigureServices`çağırarak yapılandırılmamışsa, kimlik doğrulaması girişimi *ArgumentException ile sonuçlanır: ' Signınscheme ' seçeneği sağlanmalıdır*. Bu örnekte kullanılan proje şablonu bunun yapılmasını sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede Twitter ile kimlik doğrulaması yapabilirsiniz. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).

* Web sitenizi Azure Web App 'e yayımladığınızda, `ConsumerSecret` Twitter geliştirici portalındaki ' ı sıfırlamanız gerekir.

* Ayarlama `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.
