---
title: ASP.NET core'da Facebook dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Facebook hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: 8bb22dc6df9879e827ff9a5ac11e9e3ad5346dc2
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121511"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>ASP.NET core'da Facebook dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, oluşturulan bir örnek ASP.NET Core 2.0 proje kullanarak kendi Facebook hesabı ile oturum açmalarına işlemini göstermektedir [önceki sayfaya](xref:security/authentication/social/index). Facebook kimlik doğrulaması gerektiren [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet paketi. İzleyerek bir Facebook uygulama kimliği oluşturarak başlayın [resmi adımları](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>İçinde Facebook uygulaması oluşturma

* Gidin [Facebook geliştiricilerin uygulama](https://developers.facebook.com/apps/) sayfasında ve oturum açın. Bir Facebook hesabınız yoksa kullanırsanız **Facebook'a kaydolma** bağlantısı oluşturmak için oturum açma sayfası.

* Dokunun **yeni uygulama Ekle** yeni bir uygulama kimliği oluşturmak için sağ üst köşedeki düğmesi

   ![Microsoft Edge'de Facebook geliştiriciler portalı açın](index/_static/FBMyApps.png)

* Formu doldurun ve dokunun **uygulama kimliği oluşturma** düğmesi.

  ![Yeni uygulama Kimliğini formu oluşturma](index/_static/FBNewAppId.png)

* Üzerinde **bir ürün seçin** sayfasında **Ayarla** üzerinde **Facebook oturum açma** kart.

  ![Ürün kurulum sayfası](index/_static/FBProductSetup.png)

* **Hızlı** Sihirbazı ile başlar **Platform seçin** ilk sayfası. Sihirbaz tıklayarak şimdilik atla **ayarları** sol taraftaki menüden bağlantı:

  ![Ziyaretimde hızlı başlangıç](index/_static/FBSkipQuickStart.png)

* Size sunulan **istemci OAuth ayarları** sayfası:

  ![İstemci OAuth Ayarları sayfası](index/_static/FBOAuthSetup.png)

* Geliştirme sürecinizi URI girin ile */signin-facebook* içine eklenen **geçerli OAuth yeniden yönlendirme URI'leri** alan (örneğin: `https://localhost:44320/signin-facebook`). Bu öğreticinin ilerleyen bölümlerinde yapılandırılmış Facebook kimlik doğrulaması istekleri otomatik olarak işleyecek */signin-facebook* OAuth akışını uygulamak için rota.

> [!NOTE]
> URI */signin-facebook* Facebook kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır. Facebook kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) sınıf.

* Tıklayın **değişiklikleri kaydetmek**.

* Tıklayın **ayarları** > **temel** sol gezinti bağlantısı.

  Bu sayfada, Not, `App ID` ve `App Secret`. Sonraki bölümde, ASP.NET Core uygulamasına hem de ekleyeceksiniz:

* Sitenin dağıtım yaparken yeniden ziyaret etmeniz gerekir **Facebook oturum açma** Kurulum sayfasında ve yeni bir ortak URI kaydedin.

## <a name="store-facebook-app-id-and-app-secret"></a>Facebook uygulama Kimliğiniz ve parolanız App Store

Bağlantı Facebook gibi hassas ayarları `App ID` ve `App Secret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets). Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret`.

Güvenli bir şekilde depolamak için aşağıdaki komutları yürütün `App ID` ve `App Secret` gizli dizi Yöneticisi'ni kullanarak:

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Facebook kimlik doğrulamasını yapılandırma

::: moniker range=">= aspnetcore-2.0"

Facebook hizmetinde ekleme `ConfigureServices` yönteminde *Startup.cs* dosyası:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Yükleme [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) paket.

* Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.
* .NET Core CLI ile yüklemek için proje dizininizde aşağıdakileri yürütün:

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Facebook Ara yazılımında ekleme `Configure` yönteminde *Startup.cs* dosyası:

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

Bkz: [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) Facebook kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. İçin yapılandırma seçenekleri kullanılabilir:

* Kullanıcı ile ilgili farklı bilgi isteyin.
* Oturum açma deneyimi özelleştirmek için sorgu dizesi bağımsız değişkenleri ekleyin.

## <a name="sign-in-with-facebook"></a>Facebook ile oturum açma

Uygulamanızı çalıştırın ve tıklayın **oturum**. Facebook ile oturum açmak için bir seçenek görürsünüz.

![Web uygulaması: kullanıcı kimliği](index/_static/DoneFacebook.png)

Tıkladığınızda **Facebook**, kimlik doğrulaması için Facebook için yönlendirilirsiniz:

![Facebook kimlik doğrulaması sayfası](index/_static/FBLogin.png)

Facebook kimlik doğrulaması varsayılan olarak genel profil ve e-posta adresi ister:

![Facebook kimlik doğrulaması sayfası onay ekranı](index/_static/FBLoginDone.png)

Facebook kimlik bilgilerinizi girdikten sonra e-postanızı ayarlayabildiğiniz sitenize geri yönlendirilir.

Şimdi, Facebook kimlik bilgilerinizi kullanarak kaydedilir:

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* **ASP.NET Core 2.x yalnızca:** , kimlik, çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış varsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Facebook ile kimliğini nasıl doğrulayabileceğiniz gösterdi. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).

* Web sitenizi Azure web uygulaması yayımladıktan sonra sıfırlamalısınız `AppSecret` Facebook Geliştirici Portalı.

* Ayarlama `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.
