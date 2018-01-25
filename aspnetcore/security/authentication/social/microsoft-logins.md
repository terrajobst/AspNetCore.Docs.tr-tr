---
title: "Microsoft Account dış oturum açma Kurulumu"
author: rick-anderson
description: "Bu öğretici, var olan bir ASP.NET Core uygulamaya Microsoft hesabı kullanıcı kimlik doğrulaması tümleştirmesini gösterir."
ms.author: riande
manager: wpickett
ms.date: 08/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 330398dc945fc61e5fc94d55bf651e62e0963072
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="configuring-microsoft-account-authentication"></a>Microsoft Account kimlik doğrulamasını yapılandırma

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide, kullanıcılarınızın üzerinde oluşturulmuş bir örnek ASP.NET Core 2.0 projeyi kullanarak kendi Microsoft hesabıyla oturum olanak tanımak nasıl gösterilir [önceki sayfaya](index.md).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Microsoft Geliştirici Portalı'nda uygulama oluşturma

* Gidin [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) ve oluşturun veya bir Microsoft hesabı oturum açın:

![İletişim kutusunda oturum](index/_static/MicrosoftDevLogin.png)

Zaten bir Microsoft hesabınız yoksa, dokunun  **[oluşturun!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Oturum açtıktan sonra yönlendirilirsiniz **uygulamalarım** sayfa:

![Microsoft Geliştirici Portalı Microsoft Edge'de Aç](index/_static/MicrosoftDev.png)

* Dokunun **bir uygulama ekleyin** sağ üst köşe ve girin, **uygulama adı** ve **ilgili kişi e-posta**:

![Yeni bir uygulama kaydı iletişim kutusu](index/_static/MicrosoftDevAppCreate.png)

* Bu öğreticinin amaçları doğrultusunda temizleyin **destekli Kurulum** onay kutusu.

* Dokunun **oluşturma** devam etmek için **kayıt** sayfası. Sağlayan bir **adı** ve değerini not edin **uygulama kimliği**, olarak kullanacağınız `ClientId` öğreticide daha sonra:

![Kayıt sayfası](index/_static/MicrosoftDevAppReg.png)

* Dokunun **eklemek Platform** içinde **platformları** bölümünde ve seçin **Web** platform:

![Platform iletişim ekleyin](index/_static/MicrosoftDevAppPlatform.png)

* Yeni **Web** platform bölümünde, geliştirme URL'nizi girin */signin-microsoft* içine eklenen **yönlendirme URL'si** alan (örneğin: `https://localhost:44320/signin-microsoft`). Daha sonra Bu öğreticide yapılandırılmış Microsoft kimlik doğrulama şeması istekleri otomatik olarak işleyecek */signin-microsoft* OAuth akış uygulamak için rota:

![Web Platformu bölümünde](index/_static/MicrosoftRedirectUri.png)

* Dokunun **URL Ekle** URL eklenen emin olmak için.

* Gerekiyorsa, herhangi bir uygulama ayarı doldurun ve dokunun **kaydetmek** için uygulama yapılandırma değişiklikleri kaydetmek için sayfanın altındaki.

* Site dağıtırken yeniden ziyaret etmeniz gerekir **kayıt** sayfasında ve yeni bir ortak URL'sini ayarlayın.

## <a name="store-microsoft-application-id-and-password"></a>Microsoft uygulama kimliği ve parola depolama

* Not `Application Id` görüntülenen **kayıt** sayfası.

* Dokunun **yeni bir parola oluşturmak** içinde **uygulama parolaları** bölümü. Bu uygulama parolasını burada kopyalayabilirsiniz kutusunu görüntüler:

![Yeni oluşturulan parola iletişim kutusu](index/_static/MicrosoftDevPassword.png)

Bağlantı Microsoft gibi hassas ayarları `Application ID` ve `Password` kullanarak uygulamanızı yapılandırma için [gizli Yöneticisi](../../app-secrets.md). Bu öğreticinin amaçları doğrultusunda belirteçleri ad `Authentication:Microsoft:ApplicationId` ve `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Microsoft hesabı kimlik doğrulamasını yapılandırma

Bu öğreticide kullanılan proje şablonu sağlar [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) paket zaten yüklü.

* Visual Studio 2017 ile bu paketi yüklemek için projeyi sağ tıklatın ve **NuGet paketlerini Yönet**.
* .NET Core CLI ile yüklemek için proje dizininde aşağıdakileri yürütün:

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Microsoft Account hizmetini eklemek `ConfigureServices` yönteminde *haline* dosyası:

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Microsoft Account Ara eklemek `Configure` yönteminde *haline* dosyası:

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

---

Bu belirteçler Microsoft Geliştirici Portalı'ndaki kullanılan terminolojiyi adları olsa da `ApplicationId` ve `Password`, olarak sunulan `ClientId` ve `ClientSecret` yapılandırma API'si için.

Bkz: [MicrosoftAccountOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.microsoftaccountoptions) Microsoft Account kimlik doğrulaması tarafından desteklenen yapılandırma seçenekleri hakkında daha fazla bilgi için API Başvurusu. Bu kullanıcı hakkında farklı bilgi istemek için kullanılabilir.

## <a name="sign-in-with-microsoft-account"></a>Microsoft hesapla oturum açın

Uygulamanızı çalıştırın ve tıklatın **oturum**. Microsoft ile imzalamak için bir seçenek görünür:

![Web uygulaması günlük sayfasındaki: doğrulanmayan kullanıcı](index/_static/DoneMicrosoft.png)

Microsoft tıkladığınızda, kimlik doğrulaması için Microsoft'a yönlendirilir. (Henüz oturum açtıysanız) Microsoft Account oturum sonra bilgilere erişmesine izin verilsin istenir:

![Microsoft kimlik doğrulama iletişim](index/_static/MicrosoftLogin.png)

Dokunun **Evet** ve geri e-postanızı ayarlayabileceğiniz web sitesine yönlendirilirsiniz.

Microsoft kimlik bilgilerinizi kullanarak şimdi kaydedilir:

![Web uygulaması: kimliği doğrulanmış kullanıcı](index/_static/Done.png)

## <a name="troubleshooting"></a>Sorun giderme

* Microsoft Account sağlayıcısı, bir oturum açma hatası sayfasına yönlendirir, doğrudan aşağıdaki hata başlığı ve açıklamayı sorgu dizesi parametreleri Not `#` (diyez) URI.

  Microsoft kimlik doğrulaması ile ilgili bir sorun belirtmek için hata iletisini görünüyor en yaygın nedeni, uygulamanızın herhangi birini eşleşmeyen URI olsa da **yeniden yönlendirme URI'ler** için belirtilen **Web** platform .
* **ASP.NET Core 2.x yalnızca:** ise kimlik çağırarak yapılandırılmamış `services.AddIdentity` içinde `ConfigureServices`, kimlik doğrulaması gerçekleştirmeye neden olur *ArgumentException: 'SignInScheme' seçeneği belirtilmelidir*. Bu öğreticide kullanılan proje şablonu, bu yapılır sağlar.
* Site veritabanı, ilk geçiş uygulayarak oluşturulmamış alırsa *bir veritabanı işlemi başarısız oldu istek işlenirken* hata. Dokunun **uygulamak geçişler** veritabanını oluşturmak ve hata devam etmek için yenileyin.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, Microsoft ile nasıl doğrulayabilir gösterdi. Listelenen diğer sağlayıcıları, kimlik doğrulaması için benzer bir yaklaşım izleyebilirsiniz [önceki sayfaya](index.md).

* Azure web uygulaması için web sitenizi yayımladıktan sonra yeni bir oluşturmalısınız `Password` Microsoft Geliştirici Portalı'nda.

* Ayarlama `Authentication:Microsoft:ApplicationId` ve `Authentication:Microsoft:Password` olarak Azure portalında uygulama ayarları. Yapılandırma sistemi ortam değişkenlerinin anahtarları okumak için ayarlanır.
