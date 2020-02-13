---
title: ASP.NET Core ile Twitter dışarıdan oturum açma kurulumu
author: rick-anderson
description: Bu öğreticide, Twitter hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesi gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 4710c033018710ce3620f8d7221ae2253b2c0b69
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172516"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>ASP.NET Core ile Twitter dışarıdan oturum açma kurulumu

Tarafından [Valeriy Novyıtskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu örnek, [önceki sayfada](xref:security/authentication/social/index)oluşturulmuş bir örnek ASP.NET Core 3,0 projesi kullanarak kullanıcıların [Twitter hesabıyla oturum açmasını](https://dev.twitter.com/web/sign-in/desktop-browser) nasıl olanaklı hale kullanabileceğinizi gösterir.

## <a name="create-the-app-in-twitter"></a>Uygulamayı Twitter 'da oluşturma

* Projeye [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet paketini ekleyin.

* [https://apps.twitter.com/](https://apps.twitter.com/) gidin ve oturum açın. Zaten bir Twitter hesabınız yoksa, oluşturmak için **[Şimdi kaydolun](https://twitter.com/signup)** bağlantısını kullanın.

* **Uygulama oluştur**' u seçin. **Uygulama adı**, **uygulama açıklaması** ve genel **Web sitesi** URI 'sini doldurun (etki alanı adı kaydoluncaya kadar geçici olabilir):

* **Twitter Ile oturum açmayı etkinleştir** seçeneğinin yanındaki kutuyu işaretleyin

* Microsoft. AspNetCore. Identity, kullanıcıların varsayılan olarak bir e-posta adresine sahip olmasını gerektirir. **İzinler** sekmesine gidin, **Düzenle** düğmesine tıklayın ve **kullanıcılardan e-posta adresi iste**' nin yanındaki kutuyu işaretleyin.

* **Geri arama URL 'leri** alanına eklenen `/signin-twitter` geliştirme URI 'nizi girin (örneğin: `https://webapp128.azurewebsites.net/signin-twitter`). Bu örnekte daha sonra yapılandırılan Twitter kimlik doğrulaması şeması, OAuth akışını uygulamak için `/signin-twitter` rotadaki istekleri otomatik olarak işleymeyecektir.

  > [!NOTE]
  > `/signin-twitter` URI segmenti Twitter kimlik doğrulama sağlayıcısının varsayılan geri çağırması olarak ayarlanır. Twitter kimlik doğrulama ara [yazılımını, '](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) ın devralınan [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliğini kullanarak YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.

* Formun geri kalanını doldurun ve **Oluştur**' u seçin. Yeni uygulama ayrıntıları görüntülenir:

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Twitter tüketici API anahtarı ve gizli dizisi depolanıyor

`ClientId` ve `ClientSecret` [gizli Yöneticisi](xref:security/app-secrets)kullanarak güvenli bir şekilde depolamak için aşağıdaki komutları çalıştırın:

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Gizli ayarları, Twitter `Consumer Key` ve `Consumer Secret` [gizli Yöneticisi](xref:security/app-secrets)kullanarak uygulama yapılandırmanıza bağlayın. Bu örneğin amaçları doğrultusunda belirteçleri `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`olarak adlandırın.

Bu belirteçler, yeni bir Twitter uygulaması oluşturduktan sonra **anahtarlar ve erişim belirteçleri** sekmesinde bulunabilir:

## <a name="configure-twitter-authentication"></a>Twitter kimlik doğrulamasını yapılandırma

Twitter hizmetini *Startup.cs* dosyasındaki `ConfigureServices` yöntemine ekleyin:

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

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

* **Yalnızca 2. x ASP.NET Core:** Kimlik, `ConfigureServices``services.AddIdentity` çağırarak yapılandırılmamışsa, kimlik doğrulamaya çalışmak ArgumentException ile sonuçlanır *: ' Signınscheme ' seçeneği sağlanmalıdır*. Bu örnekte kullanılan proje şablonu bunun yapılmasını sağlar.
* Site veritabanı ilk geçiş uygulanarak oluşturulmadıysa, *istek hatasını Işlerken bir veritabanı işlemi başarısız* olur. Veritabanını oluşturmak için **geçişleri Uygula** ' ya dokunun ve hatanın ötesinde devam etmek için yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede Twitter ile kimlik doğrulaması yapabilirsiniz. [Önceki sayfada](xref:security/authentication/social/index)listelenen diğer sağlayıcılarla kimlik doğrulaması yapmak için benzer bir yaklaşımı izleyebilirsiniz.

* Web sitenizi Azure Web App 'e yayımladığınızda, Twitter geliştirici portalındaki `ConsumerSecret` sıfırlamanız gerekir.

* `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` Azure portal uygulama ayarları olarak ayarlayın. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.
