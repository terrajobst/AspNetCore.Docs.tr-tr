---
title: "Facebook, Google ve diğer dış sağlayıcılarını kullanarak kimlik doğrulamasını etkinleştirme"
author: rick-anderson
description: "Bu öğretici, bir ASP.NET Core Dış kimlik doğrulama sağlayıcıları ile OAuth 2.0 kullanan 2.x uygulamasının nasıl oluşturulacağını gösterir."
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: d6daad6298267956e7ac1f4b304c5eeaae231ba8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a>Facebook, Google ve diğer dış sağlayıcılarını kullanarak kimlik doğrulamasını etkinleştirme

<a name="security-authentication-social-logins"></a>

Tarafından [Valeriy Novytskyy](https://github.com/01binary) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici, bir ASP.NET Core, OAuth 2.0 Dış kimlik doğrulama sağlayıcıları kimlik bilgilerini kullanarak oturum açmalarını sağlar 2.x uygulamasının nasıl oluşturulacağını gösterir.

[Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md), ve [Microsoft](microsoft-logins.md) sağlayıcıları aşağıdaki bölümlerde ele alınmıştır. Diğer sağlayıcıları gibi üçüncü taraf paketlerden kullanılabilir [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) ve [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Facebook, Twitter, Google artı ve Windows için sosyal medya simgeleri](index/_static/social.png)

Var olan kullanıcıların kimlik bilgileriyle oturum kullanıcıları etkinleştirme kullanıcılar için uygun olan ve birçok üzerine bir üçüncü taraf oturum açma işlemini yönetme karmaşıklığını kaydırır. Örnek olay incelemeleri tarafından nasıl sosyal oturum açmalar örnekleri trafiği ve müşteri dönüşümleri sürücü için bkz: [Facebook](https://www.facebook.com/unsupportedbrowser) ve [Twitter](https://dev.twitter.com/resources/case-studies).

Not: Burada sunulan paketleri OAuth kimlik doğrulaması akışı karmaşıklığını önemli miktarda soyut ancak Ayrıntıları Anlama sorunlarını giderirken gerekli olabilir. Birçok kaynaklar kullanılabilir; Örneğin, [OAuth 2 giriş](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) veya [anlama OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/). Bazı sorunlar bakarak çözülebilir [ASP.NET Core sağlayıcısı paketler için kaynak kodunu](https://github.com/aspnet/Security/tree/dev/src).

## <a name="create-a-new-aspnet-core-project"></a>Yeni bir ASP.NET Core projesi oluşturma

* Başlangıç sayfasından veya aracılığıyla Visual Studio 2017 ' yeni bir proje oluşturun **Dosya > Yeni > Proje**.

* Seçin **ASP.NET çekirdek Web uygulaması** şablonu kullanılabilir **Visual C# > .NET Core** kategorisi:

![Yeni Proje iletişim kutusu](index/_static/new-project.png)

* Dokunun **Web uygulaması** ve doğrulama **kimlik doğrulaması** ayarlanır **tek tek kullanıcı hesaplarını**:

![Yeni Web uygulaması iletişim kutusu](index/_static/select-project.png)

Not: Bu öğreticide Sihirbazı'nın en üstte seçilebilir ASP.NET Core 2.0 SDK sürümü için geçerlidir.

## <a name="apply-migrations"></a>Geçişleri uygulayın

* Uygulamayı çalıştırın ve seçin **oturum** bağlantı.
* Seçin **kayıt yeni bir kullanıcı olarak** bağlantı.
* Yeni hesap için e-posta ve parola girin ve ardından **kaydetmek**.
* Geçişler uygulamak için yönergeleri izleyin.

## <a name="require-ssl"></a>SSL iste

OAuth 2.0 HTTPS protokolü üzerinden kimlik doğrulaması için SSL kullanımını gerektirir.

Not: kullanılarak oluşturulan projeleri **Web uygulaması** veya **Web API** ASP.NET 2.x SSL etkinleştirmek ve varsa https URL'si ile başlatmak için otomatik olarak yapılandırılır çekirdek için proje şablonları **tek tek Kullanıcı hesaplarını** seçeneği seçildiğinde üzerinde **kimlik doğrulamayı Değiştir iletişim** yukarıda gösterildiği gibi Proje Sihirbazı'nda.

* İçindeki adımları izleyerek, sitenizde SSL iste [zorlamayı SSL bir ASP.NET Core uygulamasında](xref:security/enforcing-ssl) konu.

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Oturum açma sağlayıcıları tarafından atanan belirteçleri depolamak için SecretManager kullanın

Sosyal oturum açma sağlayıcılarıyla atamak **uygulama kimliği** ve **uygulama gizli anahtarı** (sağlayıcı tarafından değişir tam adlandırma) kayıt işlemi sırasında belirteçleri.

Bu değerler etkili bir şekilde *kullanıcı adı* ve *parola* uygulamanız kendi API erişip "uygulama yapılandırmasında yardımıyla bağlı gizli" oluşturan kullanır **Gizli Yöneticisi** doğrudan yapılandırma dosyalarını depolamak veya bunlara kodlamak yerine.

Adımları [ASP.NET Core geliştirme sırasında uygulama sırrı güvenli depolama](xref:security/app-secrets) konu böylece her oturum açma sağlayıcısı atanan belirteçleri depolayabilirsiniz.

## <a name="setup-login-providers-required-by-your-application"></a>Uygulamanızın gerektirdiği oturum açma sağlayıcılarıyla Kurulumu

Uygulamanızı ilgili sağlayıcılarını kullanacak şekilde yapılandırmak için aşağıdaki konulara bakın:

* [Facebook](facebook-logins.md) yönergeleri
* [Twitter](twitter-logins.md) yönergeleri
* [Google](google-logins.md) yönergeleri
* [Microsoft](microsoft-logins.md) yönergeleri
* [Diğer sağlayıcı](other-logins.md) yönergeleri

## <a name="optionally-set-password"></a>İsteğe bağlı olarak parola ayarlama

Bir dış oturum açma sağlayıcısıyla kaydederken uygulamayla kayıtlı bir parola yok. Bu, oluşturma ve site için bir parola anımsama azaltır, ancak aynı zamanda, dış oturum açma sağlayıcısı üzerinde bağımlı kolaylaştırır. Dış oturum açma sağlayıcısı kullanılamıyorsa, web sitesine oturum açamaz olmayacaktır.

Bir parola oluşturmasını ve dış sağlayıcıları ile oturum açma işlemi sırasında ayarlanan e-posta kullanarak oturum için:

* Dokunun **Hello <email alias>**  gitmek için sağ üst köşedeki bağlantıda **Yönet** görünümü.

![Web uygulaması yönet görünümü](index/_static/pass1a.png)

* Dokunun **oluşturma**

![Parola sayfanızı ayarlama](index/_static/pass2a.png)

* Geçerli bir parola ayarlayın ve bu e-posta ile imzalamak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bu makalede, dış kimlik doğrulama sunulan ve dış oturumlar ASP.NET Core uygulamanıza eklemek için gereken önkoşulları açıklanmıştır.

* Uygulamanız tarafından gerekli sağlayıcıları için oturum açma bilgileri yapılandırmak için başvuru sağlayıcıya özgü sayfaları.
