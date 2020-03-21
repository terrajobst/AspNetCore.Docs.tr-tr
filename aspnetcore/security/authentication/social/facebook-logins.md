---
title: ASP.NET core'da Facebook dış oturum açma Kurulumu
author: rick-anderson
description: Facebook hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesini gösteren kod örnekleri ile öğretici.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: bb26a27f026e744c7d4925aa2281bf0625fff8a2
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989789"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>ASP.NET core'da Facebook dış oturum açma Kurulumu

Tarafından [Valeriy Novyıtskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Kod örnekleri ile bu öğreticide, kullanıcılarınızın [önceki sayfada](xref:security/authentication/social/index)oluşturulmuş bir örnek ASP.NET Core 3,0 projesi kullanarak Facebook hesabıyla oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir. [Resmi adımları](https://developers.facebook.com)Izleyerek Facebook uygulama kimliği oluşturarak başladık.

## <a name="create-the-app-in-facebook"></a>İçinde Facebook uygulaması oluşturma

* Projeye [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet paketini ekleyin.

* [Facebook geliştiricileri uygulama](https://developers.facebook.com/apps/) sayfasına gidin ve oturum açın. Henüz bir Facebook hesabınız yoksa, bir tane oluşturmak için oturum açma sayfasındaki **Facebook 'A kaydolun** bağlantısını kullanın.  Facebook hesabınız olduğunda, Facebook geliştiricisi olarak kaydolmak için yönergeleri izleyin.

* Yeni bir uygulama KIMLIĞI oluşturmak için **uygulamalarım** menüsünde **uygulama oluştur** ' u seçin.

   ![Microsoft Edge'de Facebook geliştiriciler portalı açın](index/_static/FBMyApps.png)

* Formu doldurun ve **uygulama kimliği oluştur** düğmesine dokunun.

  ![Yeni uygulama Kimliğini formu oluşturma](index/_static/FBNewAppId.png)

* Yeni uygulama kartında **Ürün Ekle**' yi seçin.  **Facebook oturum açma** kartında **Ayarla** ' ya tıklayın. 

  ![Ürün kurulum sayfası](index/_static/FBProductSetup.png)

* **Hızlı başlangıç** Sihirbazı, ilk sayfa olarak **bir platform Seç** ile başlatılır. Sol alt taraftaki menüdeki **Facebook oturum açma** **ayarları** bağlantısına tıklayarak Sihirbazı Şimdi atlayın:

  ![Ziyaretimde hızlı başlangıç](index/_static/FBSkipQuickStart.png)

* **Istemci OAuth ayarları** sayfası görüntülenir:

  ![İstemci OAuth Ayarları sayfası](index/_static/FBOAuthSetup.png)

* **Geçerli OAuth yeniden yönlendirme URI 'leri** alanına */SignIn-Facebook* eklenmiş olan geliştirme URI 'nizi girin (örneğin: `https://localhost:44320/signin-facebook`). Bu öğreticide daha sonra yapılandırılan Facebook kimlik doğrulaması, OAuth akışını uygulamak için */SignIn-Facebook* rotasındaki istekleri otomatik olarak işleymeyecektir.

> [!NOTE]
> URI */SignIn-Facebook* , Facebook kimlik doğrulama sağlayıcısı 'nın varsayılan geri çağırması olarak ayarlanır. Facebook kimlik doğrulama ara yazılımını, çok [yönlü önyükleme](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) sınıfının devralınan [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği aracılığıyla YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.

* **Değişiklikleri Kaydet**' e tıklayın.

* Sol gezinti bölmesinde **ayarlar** > **temel** bağlantı ' ya tıklayın.

  Bu sayfada, `App ID` ve `App Secret`bir kopyasını alın. Sonraki bölümde, ASP.NET Core uygulamasına hem de ekleyeceksiniz:

* Siteyi dağıttığınızda **Facebook oturum açma** kurulumu sayfasını yeniden ziyaret etmeniz ve yeni BIR ortak URI kaydetmeniz gerekir.

## <a name="store-the-facebook-app-id-and-secret"></a>Facebook uygulama KIMLIĞI ve gizli anahtarını depolayın

Facebook uygulama KIMLIĞI ve gizli anahtar değerleri gibi hassas ayarları [gizli bir yöneticiye](xref:security/app-secrets)depolayın. Bu örnek için aşağıdaki adımları kullanın:

1. [Gizli depolamayı etkinleştirme](xref:security/app-secrets#enable-secret-storage)konusundaki yönergeler temelinde projeyi gizli depolama için başlatın.
1. Hassas ayarları, gizli anahtarlar `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret`ile yerel gizli depolama alanına depolayın:

    ```dotnetcli
    dotnet user-secrets set "Authentication:Facebook:AppId" "<app-id>"
    dotnet user-secrets set "Authentication:Facebook:AppSecret" "<app-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-facebook-authentication"></a>Facebook kimlik doğrulamasını yapılandırma

Facebook hizmetini *Startup.cs* dosyasındaki `ConfigureServices` yöntemine ekleyin:

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Facebook kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. çok [yönlü Bookoptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API başvurusu. İçin yapılandırma seçenekleri kullanılabilir:

* Kullanıcı ile ilgili farklı bilgi isteyin.
* Oturum açma deneyimi özelleştirmek için sorgu dizesi bağımsız değişkenleri ekleyin.

## <a name="sign-in-with-facebook"></a>Facebook ile oturum açma

Uygulamanızı çalıştırın ve **oturum aç**' a tıklayın. Facebook ile oturum açmak için bir seçenek görürsünüz.

![Web uygulaması: kullanıcı kimliği](index/_static/DoneFacebook.png)

**Facebook**'a tıkladığınızda, kimlik doğrulama için Facebook 'a yönlendirilirsiniz:

![Facebook kimlik doğrulaması sayfası](index/_static/FBLogin.png)

Facebook kimlik doğrulaması varsayılan olarak genel profil ve e-posta adresi ister:

![Facebook kimlik doğrulaması sayfası onay ekranı](index/_static/FBLoginDone.png)

Facebook kimlik bilgilerinizi girdikten sonra e-postanızı ayarlayabildiğiniz sitenize geri yönlendirilir.

Şimdi, Facebook kimlik bilgilerinizi kullanarak kaydedilir:

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* **Yalnızca 2. x ASP.NET Core:** Kimlik, `ConfigureServices``services.AddIdentity` çağırarak yapılandırılmamışsa, kimlik doğrulamaya çalışmak ArgumentException ile sonuçlanır *: ' Signınscheme ' seçeneği sağlanmalıdır*. Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.
* Site veritabanı ilk geçiş uygulanarak oluşturulmadıysa, *istek hatasını Işlerken bir veritabanı işlemi başarısız oldu* . Veritabanını oluşturmak için **geçişleri Uygula** ' ya dokunun ve hatanın ötesinde devam etmek için yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Facebook ile kimliğini nasıl doğrulayabileceğiniz gösterdi. [Önceki sayfada](xref:security/authentication/social/index)listelenen diğer sağlayıcılarla kimlik doğrulaması yapmak için benzer bir yaklaşımı izleyebilirsiniz.

* Web sitenizi Azure Web App 'e yayımladığınızda, Facebook Geliştirici portalındaki `AppSecret` sıfırlamanız gerekir.

* `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret` Azure portal uygulama ayarları olarak ayarlayın. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.
