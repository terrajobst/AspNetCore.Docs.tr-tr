---
title: Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması
author: rick-anderson
description: Bu öğreticide bir ASP.NET Core ile dış kimlik doğrulama sağlayıcıları için OAuth 2.0 kullanarak 2.x uygulamasının nasıl oluşturulacağını gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/index
ms.openlocfilehash: 19074d5014a09446ceec1b89449e78760fc8e7cf
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708380"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Facebook, Google ve ASP.NET Core dış sağlayıcı kimlik doğrulaması

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide bir ASP.NET Core, OAuth 2.0 ile dış kimlik sağlayıcılarına ait kimlik bilgilerini kullanarak oturum açmasına sağlayan 2.x uygulamasının nasıl oluşturulacağını gösterir.

[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), ve [Microsoft](xref:security/authentication/microsoft-logins) sağlayıcıları aşağıdaki bölümlerde ele alınmıştır. Diğer sağlayıcılar gibi üçüncü taraf paketleri kullanılabilir [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) ve [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Facebook, Twitter, Google artı ve Windows için sosyal medya simgeler](index/_static/social.png)

Kullanıcıların mevcut kimlik bilgileri ile oturum açmanız, kullanıcılar için uygun olan ve birçok üzerine bir üçüncü taraf oturum açma işlemi yönetmenin karmaşıklığını kaydırır. Örnek olay incelemeleri tarafından nasıl sosyal oturum açma bilgileri, örnek trafiği ve müşteri dönüştürmeler yönlendirebilirsiniz için bkz: [Facebook](https://www.facebook.com/unsupportedbrowser) ve [Twitter](https://dev.twitter.com/resources/case-studies).

Not: Burada sunulan paketler OAuth kimlik doğrulaması akışı karmaşıklığını büyük ölçüde abstract, ancak Ayrıntıları Anlama sorunlarını giderirken gerekebilir. Birçok kaynak bulunur; Örneğin, [OAuth 2 giriş](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) veya [anlama OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/). Bazı sorunlar bakarak çözülebilir [sağlayıcısı paketleri için ASP.NET Core kaynak kodu](https://github.com/aspnet/Security/tree/master/src).

## <a name="create-a-new-aspnet-core-project"></a>Yeni bir ASP.NET Core projesi oluşturma

* Başlangıç sayfasından ya da aracılığıyla Visual Studio 2017'de yeni bir proje oluşturma **dosya** > **yeni** > **proje**.

* Seçin **ASP.NET Core Web uygulaması** kullanılabilir şablon **Visual C#**   >  **.NET Core** kategorisi:

![Yeni Proje iletişim kutusu](index/_static/new-project.png)

* Dokunun **Web uygulaması** ve doğrulama **kimlik doğrulaması** ayarlanır **bireysel kullanıcı hesapları**:

![Yeni Web uygulaması iletişim kutusu](index/_static/select-project.png)

Not: Bu öğretici, sihirbazın başında seçtiğiniz ASP.NET Core 2.0 SDK'sı sürümü için geçerlidir.

## <a name="apply-migrations"></a>Geçişleri Uygula

* Uygulamayı çalıştırmak ve seçmek **oturum** bağlantı.
* Seçin **kayıt yeni bir kullanıcı olarak** bağlantı.
* Yeni hesap için e-posta ve parola girin ve ardından **kaydetme**.
* Geçişleri uygulamak için yönergeleri izleyin.

## <a name="require-ssl"></a>SSL iste

OAuth 2.0, HTTPS protokolü üzerinden kimlik doğrulaması için SSL kullanılmasını gerektirir.

Kullanılarak oluşturulan projeler **Web uygulaması** veya **Web API** proje şablonları ile ASP.NET Core 2.1 veya daha sonra otomatik olarak SSL'yi etkinleştirecek şekilde yapılandırılır. Uygulama, bir güvenli varsayılan uç nokta ile başlatılır **bireysel kullanıcı hesapları** seçeneği seçildiğinde, **kimlik doğrulamayı Değiştir iletişim** Proje Sihirbazı.

Daha fazla bilgi için bkz. <xref:security/enforcing-ssl>.

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

Bir dış oturum açma sağlayıcısıyla kaydolduğunuzda, bir parola ile uygulamanın kayıtlı sahip değilsiniz. Bu, oluşturma ve site için bir parola hatırlamak azaltır, ancak Ayrıca, dış oturum açma sağlayıcısına bağımlı olur. Dış oturum sağlayıcısının kullanılamıyorsa, web sitesine oturum açmak mümkün olmayacaktır.

Bir parola oluşturmasını ve dış sağlayıcılarıyla oturum açma işlemi sırasında ayarladığınız e-postanızı kullanarak oturum için:

* Dokunun **Hello &lt;e-posta diğer&gt;**  gitmek için sağ üst köşedeki bağlantıda **Yönet** görünümü.

![Web uygulaması yönetme görünümü](index/_static/pass1a.png)

* Dokunun **oluşturma**

![Parola sayfanızı ayarlama](index/_static/pass2a.png)

* Geçerli bir parola ayarlayın ve bu oturum, e-postayı imzalamak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, dış kimlik doğrulama sunulan ve ASP.NET Core uygulamanızı harici oturum açmaları eklemek için gereken önkoşulları açıklanmaktadır.

* Uygulamanızın gerektirdiği sağlayıcıları için oturum açma bilgileri yapılandırmak için sağlayıcıya özel sayfa başvurusu.

* Kullanıcı ve bunların erişim ve yenileme belirteçleri ile ilgili ek veriler kalıcı hale getirmek isteyebilirsiniz. Daha fazla bilgi için bkz. <xref:security/authentication/social/additional-claims>.
