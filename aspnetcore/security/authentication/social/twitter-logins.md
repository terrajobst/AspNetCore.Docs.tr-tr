---
title: ASP.NET Core ile twitter dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Twitter hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708432"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>ASP.NET Core ile twitter dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici, kullanıcılarınıza etkinleştirme gösterir [kendi Twitter hesabıyla oturum](https://dev.twitter.com/web/sign-in/desktop-browser) üzerinde oluşturulmuş bir örnek ASP.NET Core 2.0 proje kullanarak [önceki sayfaya](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>İçinde bir Twitter uygulaması oluşturma

* Gidin [ https://apps.twitter.com/ ](https://apps.twitter.com/) ve oturum açın. Twitter hesabıyla yoksa kullanın **[şimdi kaydolun](https://twitter.com/signup)** bağlantı oluşturmak için. Oturum açtıktan sonra **Uygulama Yönetimi** sayfası gösterilir:

  ![Microsoft Edge'de açık twitter uygulama yönetimi](index/_static/TwitterAppManage.png)

* Dokunun **yeni uygulama oluştur** ve uygulamanın ölçeğini doldurun **adı**, **açıklama** ve genel **Web sitesi** (Bu olabilir geçici dek URI'si etki alanı adı kayıt):

  ![Uygulama sayfası oluşturma](index/_static/TwitterCreate.png)

* Geliştirme sürecinizi URI girin ile `/signin-twitter` içine eklenen **geçerli OAuth yeniden yönlendirme URI'leri** alan (örneğin: `https://localhost:44320/signin-twitter`). Bu öğreticinin ilerleyen bölümlerinde yapılandırılmış Twitter kimlik doğrulaması düzeni istekleri otomatik olarak işleyecek `/signin-twitter` OAuth akışını uygulamak için rota.

  > [!NOTE]
  > URI segmenti `/signin-twitter` Twitter kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır. Twitter kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) sınıf.

* Kalan formu doldurun ve dokunun **kendi Twitter uygulamanızı oluşturun**. Yeni uygulama ayrıntıları görüntülenir:

  ![Uygulama sayfasında Ayrıntılar sekmesi](index/_static/TwitterAppDetails.png)

* Site dağıtırken yeniden ziyaret etmeniz gerekir **Uygulama Yönetimi** sayfasında ve yeni bir ortak URI kaydedin.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Twitter ConsumerKey ve ConsumerSecret depolama

Bağlantı Twitter gibi hassas ayarları `Consumer Key` ve `Consumer Secret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets). Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`.

Bu belirteçler bulunabilir **anahtarlar ve erişim belirteçleri** yeni Twitter uygulamanızı oluşturduktan sonra sekmesinde:

![Anahtarlar ve erişim belirteçleri sekmesi](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Twitter kimlik doğrulamasını yapılandırma

Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) paket zaten yüklü.

* Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.
* .NET Core CLI ile yüklemek için proje dizininizde aşağıdakileri yürütün:

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

Twitter hizmetinde ekleme `ConfigureServices` yönteminde *Startup.cs* dosyası:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Twitter Ara yazılımında ekleme `Configure` yönteminde *Startup.cs* dosyası:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

Bkz: [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) Twitter kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-twitter"></a>Oturum Twitter ile oturum aç

Uygulamanızı çalıştırın ve tıklayın **oturum**. Twitter ile oturum açmak için bir seçenek görüntülenir:

![Web uygulaması: kullanıcı kimliği](index/_static/DoneTwitter.png)

Tıklayarak **Twitter** kimlik doğrulaması için Twitter'a yönlendirir:

![Twitter kimlik doğrulaması sayfası](index/_static/TwitterLogin.png)

Twitter kimlik bilgilerinizi girdikten sonra web, e-posta ayarlayabileceğiniz siteye geri yönlendirilir.

Artık Twitter kimlik bilgilerinizi kullanarak günlüğe kaydedilir:

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* **ASP.NET Core 2.x yalnızca:** , kimlik, çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Twitter ile kimliğini nasıl doğrulayabileceğiniz gösterdi. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).

* Web sitenizi Azure web uygulaması yayımladıktan sonra sıfırlamalısınız `ConsumerSecret` Twitter Geliştirici Portalı.

* Ayarlama `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.
