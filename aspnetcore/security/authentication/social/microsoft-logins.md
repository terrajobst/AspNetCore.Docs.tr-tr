---
title: ASP.NET Core ile Microsoft Account dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Microsoft hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1733d049d6752c24d7749b5b5ae2a4b866492358
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751003"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>ASP.NET Core ile Microsoft Account dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, oluşturulan bir örnek ASP.NET Core 2.0 proje kullanarak, Microsoft hesabı ile oturum açmalarına işlemini göstermektedir [önceki sayfaya](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Microsoft Geliştirici portalında uygulaması oluşturma

* Gidin [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) oluşturarak veya mevcut bir Microsoft hesabı olarak oturum açın:

![İletişim kutusunda oturum](index/_static/MicrosoftDevLogin.png)

Bir Microsoft hesabınız yoksa, dokunun  **[oluşturun!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Oturum açtıktan sonra yönlendirilirsiniz **uygulamalarım** sayfası:

![Microsoft Geliştirici Portalı Microsoft Edge'de Aç](index/_static/MicrosoftDev.png)

* Dokunun **uygulama ekleme** sağ üst köşedeki ve girin, **uygulama adı** ve **ilgili kişi e-posta**:

![Yeni uygulama kaydı iletişim kutusu](index/_static/MicrosoftDevAppCreate.png)

* Bu öğreticinin amaçları doğrultusunda, Temizle **destekli Kurulum** onay kutusu.

* Dokunun **Oluştur** devam etmek için **kayıt** sayfası. Sağlayan bir **adı** ve değerini not edin **uygulama kimliği**, olarak kullanacağınız `ClientId` sonraki öğreticide:

![Kayıt sayfası](index/_static/MicrosoftDevAppReg.png)

* Dokunun **Platform Ekle** içinde **platformları** seçin ve bölüm **Web** platform:

![Platform iletişim kutusu Ekle](index/_static/MicrosoftDevAppPlatform.png)

* Yeni **Web** platform bölümünde, geliştirme URL'nizi girin `/signin-microsoft` içine eklenen **yeniden yönlendirme URL'lerini** alan (örneğin: `https://localhost:44320/signin-microsoft`). Bu öğreticinin ilerleyen bölümlerinde yapılandırılmış Microsoft kimlik doğrulama düzeni istekleri otomatik olarak işleyecek `/signin-microsoft` rota OAuth akışını uygulamak için:

![Web Platformu bölümü](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> URI segmenti `/signin-microsoft` Microsoft kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır. Microsoft kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) sınıfı.

* Dokunun **URL Ekle** URL eklenen emin olmak için.

* Gerekirse, diğer tüm uygulama ayarlarını doldurun ve dokunun **Kaydet** uygulama yapılandırma değişikliklerini kaydetmek için sayfanın alt kısmındaki.

* Site dağıtırken yeniden ziyaret etmeniz gerekir **kayıt** sayfasında ve yeni bir genel URL'ye ayarlayın.

## <a name="store-microsoft-application-id-and-password"></a>Microsoft uygulama kimliği ve parola Store

* Not `Application Id` görüntülenen **kayıt** sayfası.

* Dokunun **yeni parola oluştur** içinde **uygulama gizli dizilerini** bölümü. Böylece, uygulama parolasını burada kopyalayabilirsiniz kutusu görüntülenir:

![Yeni oluşturulan parolası iletişim kutusu](index/_static/MicrosoftDevPassword.png)

Bağlantı Microsoft gibi hassas ayarlar `Application ID` ve `Password` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets). Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Microsoft:ApplicationId` ve `Authentication:Microsoft:Password`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Microsoft hesabı kimlik doğrulamasını yapılandırma

Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) paket zaten yüklü.

* Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.
* .NET Core CLI ile yüklemek için proje dizininizde aşağıdakileri yürütün:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

Microsoft Account hizmetini de eklemek `ConfigureServices` yönteminde *Startup.cs* dosyası:

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Microsoft Account Ara yazılımında ekleme `Configure` yönteminde *Startup.cs* dosyası:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

Microsoft Geliştirici portalında kullanılan terimler, bu belirteçleri adları olsa da `ApplicationId` ve `Password`, olarak gösterilen `ClientId` ve `ClientSecret` yapılandırma API'si için.

Bkz: [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Microsoft Account kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-microsoft-account"></a>Microsoft hesabıyla oturum açın

Uygulamanızı çalıştırın ve tıklayın **oturum**. Microsoft'ta oturum açmak için bir seçenek görüntülenir:

![Web uygulaması günlüğü sayfasında: Kullanıcı Kimliği](index/_static/DoneMicrosoft.png)

Microsoft'a tıkladığınızda, kimlik doğrulaması için Microsoft'a yönlendirilir. (Zaten açtıysanız) Microsoft Account imzaladıktan sonra uygulamayı bilgilere erişmesine izin istenir:

![Microsoft kimlik doğrulama iletişim](index/_static/MicrosoftLogin.png)

Dokunun **Evet** ve web e-postanızı ayarlayabileceğiniz siteye geri yönlendirilir.

Artık, Microsoft kimlik bilgilerinizi kullanarak kaydedilir:

![Web uygulaması: Kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* Microsoft Account sağlayıcısı oturum açma hatası sayfasına yönlendirir, doğrudan aşağıdaki hata başlığı ve açıklamayı sorgu dizesi parametrelerini Not `#` (diyez etiketi) URI.

  Microsoft kimlik doğrulama ile ilgili bir sorun göstermek için hata iletisini gibi görünüyor ancak uygulamanızı herhangi bir eşleşmeyen URI en yaygın nedeni **yeniden yönlendirme URI'leri** için belirtilen **Web** platformu .
* **ASP.NET Core 2.x yalnızca:** Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Microsoft ile kimliğini nasıl doğrulayabileceğiniz gösterdi. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).

* Web sitenizi Azure web uygulaması yayımladıktan sonra yeni bir oluşturmalısınız `Password` Microsoft Geliştirici Portalı'nda.

* Ayarlama `Authentication:Microsoft:ApplicationId` ve `Authentication:Microsoft:Password` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.
