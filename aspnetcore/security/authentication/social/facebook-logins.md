---
title: ASP.NET Core 'da Facebook dış oturum açma kurulumu
author: rick-anderson
description: Facebook hesabı kullanıcı kimlik doğrulamasının mevcut bir ASP.NET Core uygulamasına tümleştirilmesini gösteren kod örnekleri ile öğretici.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717152"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>ASP.NET Core 'da Facebook dış oturum açma kurulumu

Tarafından [Valeriy Novyıtskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Kod örnekleri ile bu öğreticide, kullanıcılarınızın [önceki sayfada](xref:security/authentication/social/index)oluşturulmuş bir örnek ASP.NET Core 3,0 projesi kullanarak Facebook hesabıyla oturum açmasını nasıl etkinleştireceğinizi gösterilmektedir. [Resmi adımları](https://developers.facebook.com)Izleyerek Facebook uygulama kimliği oluşturarak başladık.

## <a name="create-the-app-in-facebook"></a>Facebook 'ta uygulama oluşturma

* Projeye [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet paketini ekleyin.

* [Facebook geliştiricileri uygulama](https://developers.facebook.com/apps/) sayfasına gidin ve oturum açın. Henüz bir Facebook hesabınız yoksa, bir tane oluşturmak için oturum açma sayfasındaki **Facebook 'A kaydolun** bağlantısını kullanın.  Facebook hesabınız olduğunda, Facebook geliştiricisi olarak kaydolmak için yönergeleri izleyin.

* Yeni bir uygulama KIMLIĞI oluşturmak için **uygulamalarım** menüsünde **uygulama oluştur** ' u seçin.

   ![Microsoft Edge 'de açık geliştiriciler portalı için Facebook](index/_static/FBMyApps.png)

* Formu doldurun ve **uygulama kimliği oluştur** düğmesine dokunun.

  ![Yeni bir uygulama KIMLIĞI formu oluşturun](index/_static/FBNewAppId.png)

* Yeni uygulama kartında **Ürün Ekle**' yi seçin.  **Facebook oturum açma** kartında **Ayarla** ' ya tıklayın. 

  ![Ürün kurulum sayfası](index/_static/FBProductSetup.png)

* **Hızlı başlangıç** Sihirbazı, ilk sayfa olarak **bir platform Seç** ile başlatılır. Sol alt taraftaki menüdeki **Facebook oturum açma** **ayarları** bağlantısına tıklayarak Sihirbazı Şimdi atlayın:

  ![hızlı başlangıç atla](index/_static/FBSkipQuickStart.png)

* **Istemci OAuth ayarları** sayfası görüntülenir:

  ![İstemci OAuth ayarları sayfası](index/_static/FBOAuthSetup.png)

* **Geçerli OAuth yeniden yönlendirme URI 'leri** alanına */SignIn-Facebook* eklenmiş olan geliştirme URI 'nizi girin (örneğin: `https://localhost:44320/signin-facebook`). Bu öğreticide daha sonra yapılandırılan Facebook kimlik doğrulaması, OAuth akışını uygulamak için */SignIn-Facebook* rotasındaki istekleri otomatik olarak işleymeyecektir.

> [!NOTE]
> URI */SignIn-Facebook* , Facebook kimlik doğrulama sağlayıcısı 'nın varsayılan geri çağırması olarak ayarlanır. Facebook kimlik doğrulama ara yazılımını, çok [yönlü önyükleme](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) sınıfının devralınan [remoteauthenticationoptions. callbackpath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği aracılığıyla YAPıLANDıRıRKEN varsayılan geri çağırma URI 'sini değiştirebilirsiniz.

* **Değişiklikleri Kaydet**' e tıklayın.

* Sol gezinti bölmesinde **ayarlar** > **temel** bağlantı ' ya tıklayın.

  Bu sayfada, `App ID` ve `App Secret`bir kopyasını alın. Bir sonraki bölümde ASP.NET Core uygulamanıza her ikisini de ekleyeceksiniz:

* Siteyi dağıttığınızda **Facebook oturum açma** kurulumu sayfasını yeniden ziyaret etmeniz ve yeni BIR ortak URI kaydetmeniz gerekir.

## <a name="store-facebook-app-id-and-app-secret"></a>Facebook uygulama KIMLIĞI ve uygulama gizli anahtarını depolayın

Gizli ayarları, Facebook `App ID` ve `App Secret` gibi hassas ayarları, uygulama yapılandırmanıza [gizli yönetici](xref:security/app-secrets)kullanarak bağlayın. Bu öğreticinin amaçları doğrultusunda belirteçleri `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret`adlandırın.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

`App ID` ve `App Secret` gizli Yöneticisi kullanarak güvenli bir şekilde depolamak için aşağıdaki komutları yürütün:

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

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

Facebook kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için bkz. çok [yönlü Bookoptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API başvurusu. Yapılandırma seçenekleri şu şekilde kullanılabilir:

* Kullanıcı hakkında farklı bilgiler isteyin.
* Oturum açma deneyimini özelleştirmek için sorgu dizesi bağımsız değişkenleri ekleyin.

## <a name="sign-in-with-facebook"></a>Facebook ile oturum açma

Uygulamanızı çalıştırın ve **oturum aç**' a tıklayın. Facebook ile oturum açma seçeneği görürsünüz.

![Web uygulaması: kimliği doğrulanmamış Kullanıcı](index/_static/DoneFacebook.png)

**Facebook**'a tıkladığınızda, kimlik doğrulama için Facebook 'a yönlendirilirsiniz:

![Facebook kimlik doğrulama sayfası](index/_static/FBLogin.png)

Facebook kimlik doğrulaması, varsayılan olarak genel profil ve e-posta adresini ister:

![Facebook kimlik doğrulama sayfası onay ekranı](index/_static/FBLoginDone.png)

Facebook kimlik bilgilerinizi girdikten sonra, e-postanızı ayarlayabileceğiniz sitenize geri yönlendirilirsiniz.

Artık Facebook kimlik bilgilerinizi kullanarak oturumunuz açıldı:

![Web uygulaması: kullanıcının kimliği doğrulandı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* **Yalnızca 2. x ASP.NET Core:** Kimlik, `ConfigureServices``services.AddIdentity` çağırarak yapılandırılmamışsa, kimlik doğrulamaya çalışmak ArgumentException ile sonuçlanır *: ' Signınscheme ' seçeneği sağlanmalıdır*. Bu öğreticide kullanılan proje şablonu bunun yapılmasını sağlar.
* Site veritabanı ilk geçiş uygulanarak oluşturulmadıysa, *istek hatasını Işlerken bir veritabanı işlemi başarısız oldu* . Veritabanını oluşturmak için **geçişleri Uygula** ' ya dokunun ve hatanın ötesinde devam etmek için yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede Facebook ile kimlik doğrulaması yapabilirsiniz. [Önceki sayfada](xref:security/authentication/social/index)listelenen diğer sağlayıcılarla kimlik doğrulaması yapmak için benzer bir yaklaşımı izleyebilirsiniz.

* Web sitenizi Azure Web App 'e yayımladığınızda, Facebook Geliştirici portalındaki `AppSecret` sıfırlamanız gerekir.

* `Authentication:Facebook:AppId` ve `Authentication:Facebook:AppSecret` Azure portal uygulama ayarları olarak ayarlayın. Yapılandırma sistemi, ortam değişkenlerinden anahtarları okumak üzere ayarlanır.
