---
title: Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Bu öğreticide bir ASP.NET Core ile dış kimlik doğrulama sağlayıcıları için OAuth 2.0 kullanarak 2.x uygulamasının nasıl oluşturulacağını gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: e2d68ac93bdcfa2fc015e8447ea38626787cdb02
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65451050"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici, OAuth 2.0 kullanarak dış kimlik sağlayıcılarına ait kimlik bilgileriyle oturum açmasını sağlayan bir ASP.NET Core 2.2 uygulamasının nasıl oluşturulacağını gösterir.

[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), ve [Microsoft](xref:security/authentication/microsoft-logins) sağlayıcıları aşağıdaki bölümlerde ele alınmıştır. Diğer sağlayıcılar gibi üçüncü taraf paketleri kullanılabilir [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) ve [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Facebook, Twitter, Google artı ve Windows için sosyal medya simgeler](index/_static/social.png)

Kullanıcıların mevcut kimlik bilgileriyle oturum açmasına etkinleştirme:
* Kullanıcılar için uygundur.
* Birçok üzerine bir üçüncü taraf oturum açma işlemi yönetmenin karmaşıklığını kaydırır. 

Örnek olay incelemeleri tarafından nasıl sosyal oturum açma bilgileri, örnek trafiği ve müşteri dönüştürmeler yönlendirebilirsiniz için bkz: [Facebook](https://www.facebook.com/unsupportedbrowser) ve [Twitter](https://dev.twitter.com/resources/case-studies).

## <a name="create-a-new-aspnet-core-project"></a>Yeni bir ASP.NET Core projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* Yeni bir ASP.NET Core Web uygulaması oluşturun.
* Seçin **ASP.NET Core 2.2** açılır ve ardından **Web uygulaması**.
* Seçin **kimlik doğrulamayı Değiştir** ve kimlik doğrulaması için **bireysel kullanıcı hesapları**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Dizinleri (`cd`) proje içeren bir klasör.

* Aşağıdaki komutları çalıştırın:

  ```console
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * `dotnet new` Komut yeni bir Razor sayfaları projesindeki oluşturur *WebApp1* klasör.
  * `-uld` LocalDB yerine SQLite kullanır. Atlamak `-uld` SQLite kullanılacak.
  * `-au Individual` Bireysel kimlik doğrulama için kod oluşturur.
  * `code` Komutu açılır *WebApp1* Visual Studio Code yeni bir örneğini klasöründe.

  Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'WebApp1' eksik. Bunları eklensin mi?**

* Seçin **Evet**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Bir terminalde aşağıdaki komutu çalıştırın:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1 -au Individual
```

Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) Razor sayfaları projesi oluşturmak için.

## <a name="open-the-project"></a>Projeyi açın

Visual Studio'dan seçin **Dosya > Aç**ve ardından *WebApp1.csproj* dosya.

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a>Geçişleri Uygula

* Uygulamayı çalıştırmak ve seçmek **kaydetme** bağlantı.
* Yeni hesap için e-posta ve parola girin ve ardından **kaydetme**.
* Geçişleri uygulamak için yönergeleri izleyin.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Oturum açma sağlayıcıları tarafından atanan belirteçlerini depolamak üzere SecretManager kullanın

Sosyal oturum açma sağlayıcılarıyla atama **uygulama kimliği** ve **uygulama gizli anahtarı** kayıt işlemi sırasında belirteçleri. Tam bir belirteç adları, sağlayıcı tarafından farklılık gösterir. Bu belirteçler, uygulamanızı kendi API'ye erişmek için kullandığı kimlik bilgilerini temsil eder. "Yardım uygulama yapılandırmanıza bağlı gizli dizileri" belirteçleri oluşturan [gizli dizi Yöneticisi](xref:security/app-secrets#secret-manager). Gizli dizi yöneticisidir belirteçleri gibi bir yapılandırma dosyasında saklamak için daha güvenli bir alternatif *appsettings.json*.

> [!IMPORTANT]
> Gizli dizi Yöneticisi, yalnızca geliştirme amaçlıdır. Depolama ve Azure test ve üretim parolalarını ile korumak [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration).

Bağlantısındaki [ASP.NET core'da geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması](xref:security/app-secrets) her oturum açma sağlayıcısı atanan belirteçlerini depolamak üzere konu.

## <a name="setup-login-providers-required-by-your-application"></a>Uygulamanız için gereken kurulum oturum açma sağlayıcıları

Uygulamanızı ilgili sağlayıcı kullanacak şekilde yapılandırmak için aşağıdaki konulara bakın:

* [Facebook](xref:security/authentication/facebook-logins) yönergeleri
* [Twitter](xref:security/authentication/twitter-logins) yönergeleri
* [Google](xref:security/authentication/google-logins) yönergeleri
* [Microsoft](xref:security/authentication/microsoft-logins) yönergeleri
* [Diğer sağlayıcı](xref:security/authentication/otherlogins) yönergeleri

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a>İsteğe bağlı olarak parolasını ayarlayın

Bir dış oturum açma sağlayıcısıyla kaydolduğunuzda, bir parola ile uygulamanın kayıtlı sahip değilsiniz. Bu, oluşturma ve site için bir parola hatırlamak azaltır, ancak Ayrıca, dış oturum açma sağlayıcısına bağımlı olur. Dış oturum sağlayıcısının kullanılamıyorsa, web sitesine oturum açmanız mümkün olmayacaktır.

Bir parola oluşturmasını ve dış sağlayıcılarıyla oturum açma işlemi sırasında ayarladığınız e-postanızı kullanarak oturum için:

* Seçin **Hello &lt;e-posta diğer&gt;**  bağlantı gitmek için sağ üst köşesinde **Yönet** görünümü.

![Web uygulaması yönetme görünümü](index/_static/pass1a.png)

* **Oluştur**’u seçin

![Parola sayfanızı ayarlama](index/_static/pass2a.png)

* Geçerli bir parola ayarlayın ve bu oturum, e-postayı imzalamak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, dış kimlik doğrulama sunulan ve ASP.NET Core uygulamanızı harici oturum açmaları eklemek için gereken önkoşulları açıklanmaktadır.

* Uygulamanızın gerektirdiği sağlayıcıları için oturum açma bilgileri yapılandırmak için sağlayıcıya özel sayfa başvurusu.

* Kullanıcı ve bunların erişim ve yenileme belirteçleri ile ilgili ek veriler kalıcı hale getirmek isteyebilirsiniz. Daha fazla bilgi için bkz. <xref:security/authentication/social/additional-claims>.
