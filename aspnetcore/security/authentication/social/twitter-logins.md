---
title: ASP.NET Core twitter dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğretici, var olan bir ASP.NET Core uygulamaya Twitter hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterir.
manager: wpickett
ms.author: riande
ms.date: 11/01/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/twitter-logins
ms.openlocfilehash: 3f0eb9abce067108b82cf8b639cea3b120ca4b5a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a>ASP.NET Core twitter dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, kullanıcılarınıza etkinleştirme gösterilir [kendi Twitter hesabıyla oturum](https://dev.twitter.com/web/sign-in/desktop-browser) örnek ASP.NET Core 2.0 proje kullanılarak oluşturulan üzerinde [önceki sayfaya](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Twitter içinde uygulaması oluşturma

* Gidin [ https://apps.twitter.com/ ](https://apps.twitter.com/) ve oturum açın. Bir Twitter hesabı zaten yoksa kullanın **[şimdi kaydolun](https://twitter.com/signup)** bağlantı oluşturun. ' De oturum açtıktan sonra **Uygulama Yönetimi** sayfası gösterilir:

![Microsoft Edge'de açık twitter uygulama yönetimi](index/_static/TwitterAppManage.png)

* Dokunun **yeni uygulama oluşturma** ve uygulamayı doldurun **adı**, **açıklama** ve ortak **Web sitesi** (Bu olabilir geçici dek URI etki alanı adını kaydettirerek):

![Uygulama sayfası oluşturma](index/_static/TwitterCreate.png)

* Geliştirme URI girin ile */signin-twitter* içine eklenen **geçerli OAuth yeniden yönlendirme URI'ler** alan (örneğin: `https://localhost:44320/signin-twitter`). Daha sonra Bu öğreticide yapılandırılmış Twitter kimlik doğrulama şeması istekleri otomatik olarak işleyecek */signin-twitter* OAuth akış uygulamak için rota.

* Kalan formu doldurun ve dokunun **Twitter uygulamanızı oluşturma**. Yeni uygulama ayrıntıları görüntülenir:

![Ayrıntılar sekmesinde uygulama sayfası](index/_static/TwitterAppDetails.png)

* Site dağıtırken yeniden ziyaret etmeniz gerekir **Uygulama Yönetimi** sayfasında ve yeni bir ortak URI kaydedin.

## <a name="storing-twitter-consumerkey-and-consumersecret"></a>Twitter ConsumerKey ve ConsumerSecret depolama

Bağlantı Twitter gibi hassas ayarları `Consumer Key` ve `Consumer Secret` kullanarak uygulamanızı yapılandırma için [gizli Yöneticisi](xref:security/app-secrets). Bu öğreticinin amaçları doğrultusunda belirteçleri ad `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret`.

Bu belirteçler bulunabilir **anahtarları ve erişim belirteçleri** yeni Twitter uygulamanızı oluşturduktan sonra sekmesi:

![Anahtarlar ve erişim belirteçleri sekmesi](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a>Twitter kimlik doğrulamasını yapılandırma

Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) paket zaten yüklü.

* Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.
* .NET Core CLI ile yüklemek için proje dizininde aşağıdakileri yürütün:

   `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
Twitter hizmetinde eklemek `ConfigureServices` yönteminde *haline* dosyası:

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

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
Twitter Ara yazılımında eklemek `Configure` yönteminde *haline* dosyası:

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

* * *
Bkz: [TwitterOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.twitteroptions) Twitter kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. Bu kullanıcı hakkında farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-twitter"></a>Oturum Twitter ile oturum aç

Uygulamanızı çalıştırın ve tıklatın **oturum**. Twitter ile oturum aç imzalamak için bir seçenek görünür:

![Web uygulaması: doğrulanmayan kullanıcı](index/_static/DoneTwitter.png)

Tıklayarak **Twitter** için Twitter kimlik doğrulaması için yeniden yönlendirir:

![Twitter kimlik doğrulaması sayfası](index/_static/TwitterLogin.png)

Twitter kimlik bilgilerinizi girdikten sonra geri e-postanızı ayarlayabileceğiniz web sitesine yönlendirilirsiniz.

Twitter kimlik bilgilerinizi kullanarak şimdi kaydedilir:

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

## <a name="troubleshooting"></a>Sorun giderme

* **ASP.NET Core 2.x yalnızca:** ise kimlik çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması gerçekleştirmeye neden olur *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu öğreticide kullanılan proje şablonu, bu yapılır sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız oldu istek işlenirken* hata. Dokunun **uygulamak geçişler** veritabanını oluşturmak ve hata devam etmek için yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Twitter ile nasıl doğrulayabilir gösterdi. Listelenen diğer sağlayıcıları, kimlik doğrulaması için benzer bir yaklaşım izleyebilirsiniz [önceki sayfaya](xref:security/authentication/social/index).

* Azure web uygulaması için web sitenizi yayımladıktan sonra sıfırlamalıdır `ConsumerSecret` Twitter Geliştirici Portalı'nda.

* Ayarlama `Authentication:Twitter:ConsumerKey` ve `Authentication:Twitter:ConsumerSecret` olarak Azure portalında uygulama ayarları. Yapılandırma sistemi ortam değişkenlerinin anahtarları okumak için ayarlanır.
