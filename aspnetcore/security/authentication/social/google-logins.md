---
title: ASP.NET Core Google dış oturum açma Kurulumu
author: rick-anderson
description: Bu öğretici, var olan bir ASP.NET Core uygulamaya Google hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterir.
manager: wpickett
ms.author: riande
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/google-logins
ms.openlocfilehash: e3d0f0c058dd7395aebf1c97821867bae3af7c54
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="google-external-login-setup-in-aspnet-core"></a>ASP.NET Core Google dış oturum açma Kurulumu

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, kullanıcılarınızın üzerinde oluşturulmuş bir örnek ASP.NET Core 2.0 projeyi kullanarak Google + hesaplarında oturum olanak tanımak nasıl gösterilir [önceki sayfaya](xref:security/authentication/social/index). İzleyerek Başlangıç [resmi adımları](https://developers.google.com/identity/sign-in/web/devconsole-project) Google API Konsolu'nda yeni bir uygulama oluşturmak için.

## <a name="create-the-app-in-google-api-console"></a>Google API Konsolu'nda uygulaması oluşturma

* Gidin [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) ve oturum açın. Bir Google hesabınız zaten yoksa, kullanın **daha fazla seçenek** > **[hesabı oluşturma](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  bağlantı oluşturmak için:

![Google API Konsolu](index/_static/GoogleConsoleLogin.png)

* Yönlendirilirsiniz **API Yöneticisi Kitaplığı** sayfa:

![API Yöneticisi kitaplığı sayfası](index/_static/GoogleConsoleSwitchboard.png)

* Dokunun **oluşturma** ve girin, **proje adı**:

![Yeni Proje iletişim kutusu](index/_static/GoogleConsoleNewProj.png)

* İletişim kabul ettikten sonra yeni uygulamanız için özellikler seçmenizi sağlayan kitaplığı sayfasına yönlendirilirsiniz. Bul **Google + API** listesi ve API özelliği eklemek için bağlantıyı tıklatın:

![API Yöneticisi kitaplığı sayfası](index/_static/GoogleConsoleChooseApi.png)

* Yeni eklenen API sayfası görüntülenir. Dokunun **etkinleştirmek** Google + oturum özelliği uygulamanıza eklemek için:

![API Yöneticisi Google + API sayfası](index/_static/GoogleConsoleEnableApi.png)

* API etkinleştirdikten sonra dokunun **kimlik bilgileri oluşturma** gizli yapılandırmak için:

![API Yöneticisi Google + API sayfası](index/_static/GoogleConsoleGoCredentials.png)

* Şunu seçin:
   * **Google + API**
   * **Web sunucusu (örneğin node.js, Tomcat)**, ve
   * **Kullanıcı verilerini**:

![API Yöneticisi kimlik bilgileri sayfası: ne tür, kimlik bilgileri çıkışı Bul paneli gerekir](index/_static/GoogleConsoleChooseCred.png)

* Dokunun **kimlik bilgileri ne yapmalıyım?** aldığı, uygulama yapılandırması, ikinci adım **OAuth 2.0 istemci kimlik oluşturun**:

![API Yöneticisi kimlik bilgileri sayfası: OAuth 2.0 istemci kimlik oluşturun](index/_static/GoogleConsoleCreateClient.png)

* Tek bir özellik (biz girebilirsiniz aynı oturum açma), bir Google + proje oluşturuyoruz çünkü **adı** OAuth 2.0 istemci kimliği olarak proje için kullandık.

* Geliştirme URI girin ile */signin-google* içine eklenen **yetkili yönlendirmek URI'ler** alan (örneğin: `https://localhost:44320/signin-google`). Daha sonra Bu öğreticide yapılandırılmış Google kimlik doğrulama istekleri otomatik olarak işleyecek */signin-google* OAuth akış uygulamak için rota.

* Eklemek için SEKME tuşuna basın **yetkili yönlendirmek URI'ler** girişi.

* Dokunun **istemci kimliği oluşturma**, aldığı, üçüncü adım **OAuth 2.0 izin ekranı ayarlayabilirsiniz**:

![API Yöneticisi kimlik bilgileri sayfası: OAuth 2.0 izin ekranı ayarlayabilirsiniz](index/_static/GoogleConsoleAddCred.png)

* Ortak yönelik girin **e-posta adresi** ve **ürün adı** uygulamanız için gösterilen Google + oturum açmak için kullanıcıya sorar. Ek Seçenekler altında kullanılabilir **daha fazla özelleştirme seçenekleri**.

* Dokunun **devam** son adım, devam etmek için **Download credentials**:

![API Yöneticisi kimlik bilgileri sayfası: kimlik bilgilerini indirme](index/_static/GoogleConsoleFinish.png)

* Dokunun **karşıdan** uygulama parolaları ile JSON dosyasının kaydedileceği ve **Bitti** yeni uygulama oluşturma işlemini tamamlamak için.

* Site dağıtırken yeniden ziyaret etmeniz gerekir **Google konsol** ve yeni genel bir url kaydedin.

## <a name="store-google-clientid-and-clientsecret"></a>Mağaza Google ClientID ve ClientSecret

Bağlantı Google gibi hassas ayarları `Client ID` ve `Client Secret` kullanarak uygulamanızı yapılandırma için [gizli Yöneticisi](xref:security/app-secrets). Bu öğreticinin amaçları doğrultusunda belirteçleri ad `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret`.

Bu belirteçler için değerleri altında önceki adımda indirdiğiniz JSON dosyasını bulunabilir `web.client_id` ve `web.client_secret`.

## <a name="configure-google-authentication"></a>Google kimlik doğrulamasını yapılandırma

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Google hizmetinde eklemek `ConfigureServices` yönteminde *haline* dosyası:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) paketi yüklenir.

* Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.
* .NET Core CLI ile yüklemek için proje dizininde aşağıdakileri yürütün:

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Google Ara yazılımında eklemek `Configure` yönteminde *haline* dosyası:

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

---

Bkz: [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) Google kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. Bu kullanıcı hakkında farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-google"></a>Oturum Google ile oturum aç

Uygulamanızı çalıştırın ve tıklatın **oturum**. Google ile oturum aç imzalamak için bir seçenek görünür:

![Microsoft Edge'de çalışan web uygulaması: doğrulanmayan kullanıcı](index/_static/DoneGoogle.png)

Google üzerinde tıkladığınızda, Google için kimlik doğrulaması için yönlendirilir:

![Google kimlik doğrulama iletişim](index/_static/GoogleLogin.png)

Google kimlik bilgilerinizi girdikten sonra daha sonra geri e-postanızı ayarlayabileceğiniz web sitesine yönlendirilirsiniz.

Google kimlik bilgilerinizi kullanarak şimdi kaydedilir:

![Microsoft Edge'de çalışan web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

## <a name="troubleshooting"></a>Sorun giderme

* Alırsanız, bir `403 (Forbidden)` geliştirme modunu (veya aynı hata hata ayıklayıcısı içine sonu) çalıştığından emin olun, kendi uygulama hata sayfasından **Google + API** içindeki etkin **API Yöneticisi Kitaplığı** listelenen adımları izleyerek [bu sayfadaki önceki](#create-the-app-in-google-api-console). Oturum açma çalışmaz ve hataları alma değil, sorunu hata ayıklama kolay hale getirmek için geliştirme moduna geçin.
* **ASP.NET Core 2.x yalnızca:** ise kimlik çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması gerçekleştirmeye neden olur *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu öğreticide kullanılan proje şablonu, bu yapılır sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız oldu istek işlenirken* hata. Dokunun **uygulamak geçişler** veritabanını oluşturmak ve hata devam etmek için yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Google ile nasıl doğrulayabilir gösterdi. Listelenen diğer sağlayıcıları, kimlik doğrulaması için benzer bir yaklaşım izleyebilirsiniz [önceki sayfaya](xref:security/authentication/social/index).

* Azure web uygulaması için web sitenizi yayımladıktan sonra sıfırlamalıdır `ClientSecret` Google API Konsolu'nda.

* Ayarlama `Authentication:Google:ClientId` ve `Authentication:Google:ClientSecret` olarak Azure portalında uygulama ayarları. Yapılandırma sistemi ortam değişkenlerinin anahtarları okumak için ayarlanır.
