---
title: ASP.NET core'da Google dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğreticide, mevcut bir ASP.NET Core uygulamasına Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterilmektedir.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: 372504eb4f6fea412b5b160e0d5e9251dafe0d56
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284500"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET core'da Google dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, oluşturulan bir örnek ASP.NET Core 2.0 proje kullanarak kendi Google + hesabı ile oturum açmalarına işlemini göstermektedir [önceki sayfaya](xref:security/authentication/social/index). İzleyerek Başlangıç [resmi adımları](https://developers.google.com/identity/sign-in/web/devconsole-project) Google API konsolu içinde yeni bir uygulama oluşturmak için.

## <a name="create-the-app-in-google-api-console"></a>Google API konsolunda uygulaması oluşturma

* Gidin [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) ve oturum açın. Bir Google hesabınız yoksa kullanırsanız **daha fazla seçenek** > **[hesabı oluşturma](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  bağlantı oluşturmak için:

![Google API Konsolu](index/_static/GoogleConsoleLogin.png)

* Yönlendirilirsiniz **API Yöneticisi Kitaplığı** sayfası:

![API Yöneticisi kitaplık sayfasında giriş](index/_static/GoogleConsoleSwitchboard.png)

* Dokunun **Oluştur** girin, **proje adı**:

![Yeni Proje iletişim kutusu](index/_static/GoogleConsoleNewProj.png)

* İletişim kutusunu kabul ettikten sonra yeni uygulamanız için Özellikler'i seçmenize olanak tanıyan kitaplığı sayfasına yönlendirilirsiniz. Bulma **Google + API** listesi ve API özelliği eklemek için kendi bağlantısına tıklayın:

!["Google + API için" API Yöneticisi kitaplığı sayfada arama](index/_static/GoogleConsoleChooseApi.png)

* Yeni eklenen API için sayfa görüntülenir. Dokunun **etkinleştirme** Google + oturum özelliği uygulamanıza eklemek için:

![API Yöneticisi Google + API sayfasındaki giriş](index/_static/GoogleConsoleEnableApi.png)

* API'ı etkinleştirdikten sonra dokunun **kimlik bilgilerini oluştur** gizli dizileri yapılandırmak için:

![API Yöneticisi Google + API sayfasında kimlik bilgilerini düğme oluşturma](index/_static/GoogleConsoleGoCredentials.png)

* Şunu seçin:
  * **Google + API**
  * **Web sunucusu (örn: node.js, Tomcat)**, ve
  * **Kullanıcı verilerini**:

![API Yöneticisi kimlik bilgileri sayfası: Ne tür, kimlik bilgileri kullanıma Bul paneli gerekir](index/_static/GoogleConsoleChooseCred.png)

* Dokunun **hangi kimlik bilgileri gerekiyor?** aldığı, uygulama yapılandırması, ikinci adım için **OAuth 2.0 istemci kimlik oluşturma**:

![API Yöneticisi kimlik bilgileri sayfası: OAuth 2.0 istemci kimlik oluşturma](index/_static/GoogleConsoleCreateClient.png)

* Tek bir özellik (biz girebilirsiniz aynı oturum açma), Google + proje oluşturuyoruz çünkü **adı** proje için kullandığımız bir OAuth 2.0 istemci kimliği.

* Geliştirme sürecinizi URI girin ile `/signin-google` içine eklenen **yetkili yeniden yönlendirme URI'leri** alan (örneğin: `https://localhost:44320/signin-google`). Bu öğreticinin ilerleyen bölümlerinde yapılandırılmış Google kimlik doğrulama istekleri otomatik olarak işleyecek `/signin-google` OAuth akışını uygulamak için rota.

> [!NOTE]
> URI segmenti `/signin-google` Google kimlik doğrulama sağlayıcısı varsayılan geri arama olarak ayarlanır. Google kimlik doğrulaması ara yazılımı üzerinden devralınan yapılandırırken, varsayılan geri arama URI'si değiştirebilirsiniz [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) özelliği [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) sınıfı.

* Eklemek için SEKME tuşuna basın **yetkili yeniden yönlendirme URI'leri** girişi.

* Dokunun **istemci kodu oluşturma**, aldığı, üçüncü adım **OAuth 2.0 onay ekranı ayarlama**:

![API Yöneticisi kimlik bilgileri sayfası: OAuth 2.0 onay ekranı ayarlama](index/_static/GoogleConsoleAddCred.png)

* Genel kullanıma girin **e-posta adresi** ve **ürün adı** uygulamanız için gösterilen Google + oturum açmak için kullanıcıya sorar. Ek seçenekleri altında kullanılabilir **daha fazla özelleştirme seçenekleri**.

* Dokunun **devam** son adım, devam etmek için **kimlik bilgilerini indirme**:

![API Yöneticisi kimlik bilgileri sayfası: Kimlik bilgilerini indirin.](index/_static/GoogleConsoleFinish.png)

* Dokunun **indirme** uygulama gizli öğeleri bir JSON dosyası kaydetmek için ve **Bitti** yeni uygulama oluşturmayı tamamlamak için.

* Site dağıtırken yeniden ziyaret etmeniz gerekir **Google konsolunu** ve yeni bir genel url kaydedin.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID ve ClientSecret

Bağlantı Google gibi hassas ayarları `Client ID` ve `Client Secret` yapılandırma kullanarak uygulama [gizli dizi Yöneticisi](xref:security/app-secrets). Bu öğreticinin amaçları doğrultusunda, belirteçleri ad `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`.

Bu belirteçler değerlerini altında önceki adımda yüklenen JSON dosyasında bulunabilir `web.client_id` ve `web.client_secret`.

## <a name="configure-google-authentication"></a>Google kimlik doğrulamasını yapılandırma

::: moniker range=">= aspnetcore-2.0"

Google hizmet ekleme `ConfigureServices` yönteminde *Startup.cs* dosyası:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) paket yüklenir.

* Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.
* .NET Core CLI ile yüklemek için proje dizininizde aşağıdakileri yürütün:

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Google Ara yazılımında ekleme `Configure` yönteminde *Startup.cs* dosyası:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

Bkz: [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Google kimlik doğrulama tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. Bu kullanıcı ile ilgili farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-google"></a>Google ile oturum aç

Uygulamanızı çalıştırın ve tıklayın **oturum**. Google ile oturum açmak için bir seçenek görüntülenir:

![Microsoft Edge'de çalışan web uygulaması: Kullanıcı Kimliği](index/_static/DoneGoogle.png)

Google'da tıkladığınızda, kimlik doğrulaması için Google'a yönlendirilir:

![Google kimlik doğrulama iletişim](index/_static/GoogleLogin.png)

Google kimlik bilgilerinizi girdikten sonra ardından web e-postanızı ayarlayabileceğiniz siteye geri yönlendirilir.

Şimdi, Google kimlik bilgilerinizi kullanarak kaydedilir:

![Microsoft Edge'de çalışan web uygulaması: Kimliği doğrulanmış kullanıcı](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Sorun giderme

* Alırsanız bir `403 (Forbidden)` geliştirme modu (veya aynı hata ayıklayıcısına kesme) çalışıyor olun, kendi uygulamanızı hata sayfasından **Google + API** içinde etkin **APIYöneticisikitaplığı** listelenen adımları takip ederek [bu sayfadaki önceki](#create-the-app-in-google-api-console). Hataları almıyor olabilirler ve oturum açma işe yaramazsa, sorunu hata ayıklama daha kolay hale getirmek için geliştirme moduna geçin.
* **ASP.NET Core 2.x yalnızca:** Kimlik çağırarak yapılandırılmazsa `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması yapmaya sonuçlanır *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu öğreticide kullanılan proje şablonu, bu gerçekleştirilir sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız istek işlenirken* hata. Dokunun **geçerli geçişleri** veritabanı oluşturma ve hata devam etmek için yenilemek için.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Google ile kimliğini nasıl doğrulayabileceğiniz gösterdi. Listelenen diğer sağlayıcıları ile kimlik doğrulaması için benzer bir yaklaşım izleyerek [önceki sayfaya](xref:security/authentication/social/index).

* Web sitenizi Azure web uygulaması yayımladıktan sonra sıfırlamalısınız `ClientSecret` Google API konsolunda.

* Ayarlama `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` Azure portalında uygulama ayarları olarak. Yapılandırma sistemi ortam değişkenlerinden anahtarları okumak için ayarlanır.
