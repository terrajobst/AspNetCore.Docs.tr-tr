---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN OAuth 2.0 yetkilendirme sunucusu | Microsoft Docs
author: hongyes
description: Bu öğreticide bir OAuth 2.0 yetkilendirme sunucusu Ara OWIN OAuth kullanarak uygulama konusunda size yol gösterir. Bu, yalnızca Gr Gelişmiş bir öğreticidir...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: b8451d2d9e346bd5e2f51ba45e48030a5221b549
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667654"
---
# <a name="owin-oauth-20-authorization-server"></a>OWIN OAuth 2.0 Yetkilendirme Sunucusu

> Bu öğreticide bir OAuth 2.0 yetkilendirme sunucusu Ara OWIN OAuth kullanarak uygulama konusunda size yol gösterir. Bu yalnızca bir OWIN OAuth 2.0 yetkilendirme sunucusu oluşturma adımlarını özetleyen Gelişmiş bir öğreticidir. Bu adım adım öğretici değildir. [Örnek kodu indirdikten](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
>
> > [!NOTE]
> > Bu ana hat güvenli bir üretim uygulaması oluşturmak için kullanılacak amaçlanmamış. Bu öğreticide yalnızca anahat bir OAuth 2.0 yetkilendirme sunucusu Ara OWIN OAuth kullanarak uygulama hakkında sağlamaya yönelik.
>
>
> ## <a name="software-versions"></a>Yazılım sürümleri
>
> | **Öğreticide gösterilen** | **İle de çalışır.** |
> | --- | --- |
> | Windows 8.1 | Windows 10, Windows 8, Windows 7 |
> | [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
> | .NET 4.7.2 |  |
>
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
>
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, onları gönderebilir [github'da Katana projesini](https://github.com/aspnet/AspNetKatana/). Sorularınız ve yorumlarınız ilgili öğretici için sayfanın alt kısmındaki Açıklamalar bölümüne bakın.


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) HTTP hizmetine sınırlı erişim elde etmek üçüncü taraf bir uygulama sağlar. Korunan bir kaynağa erişmek için kaynak sahibinin kimlik bilgilerini kullanmak yerine, istemci bir erişim belirteci alır (bir dize olan belirli bir kapsamda, yaşam süresi ve diğer erişim özniteliklerini belirten). Erişim belirteçleri için üçüncü taraf istemcileri, kaynak sahibinin onayı ile bir yetkilendirme sunucusu tarafından verilir.

Bu öğreticide ele alınacaktır:

- Dört yetkilendirme desteklemek için bir yetkilendirme sunucusu oluşturma, verme türlerini ve yenileme belirteçlerini:
    - Yetkilendirme kodu verme
    - Örtülü izin
    - Kaynak sahibi parolası kimlik bilgileri verme
    - İstemci kimlik bilgileri verme
- Bir erişim belirteci tarafından korunan bir kaynak sunucusu oluşturuluyor.
- OAuth 2.0 istemcilerinin oluşturuluyor.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) de gösterildiği gibi **yazılım sürümleri** sayfanın üstünde.
- OWIN ile aşinalık. Bkz: [Katana projesi ile çalışmaya başlama](https://msdn.microsoft.com/magazine/dn451439.aspx) ve [OWIN ve Katana yenilikler](index.md).
- Konusunda [OAuth](http://tools.ietf.org/html/rfc6749) terminolojisinde dahil olmak üzere [rolleri](http://tools.ietf.org/html/rfc6749#section-1.1), [Protokolü akış](http://tools.ietf.org/html/rfc6749#section-1.2), ve [yetkilendirme verme](http://tools.ietf.org/html/rfc6749#section-1.3). [OAuth 2.0 giriş](http://tools.ietf.org/html/rfc6749#section-1) iyi bir giriş sağlar.

## <a name="create-an-authorization-server"></a>Yetkilendirme sunucusu oluşturma

Bu öğreticide, biz kabaca nasıl yararlanılacağını taslak [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) ve bir yetkilendirme sunucusu oluşturmak için ASP.NET MVC. Bu öğreticide her adımı içermez gibi bir yükleme için tamamlanan örnek, yakında sağlamak umuyoruz. İlk olarak, adlı bir boş web uygulaması oluşturma *AuthorizationServer* ve aşağıdaki paketleri yükleyin:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (veya herhangi bir sosyal oturum paketini Microsoft.Owin.Security.Facebook gibi)

Ekleme bir [OWIN başlangıç sınıfı](owin-startup-class-detection.md) adlı proje kök klasörü altında *başlangıç*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Oluşturma bir *uygulama\_Başlat* klasör. Seçin *uygulama\_Başlat* klasörü ve kullanım indirilen sürümü eklemek için Shift + Alt + A *AuthorizationServer\App\_Start\Startup.Auth.cs* dosya.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Yukarıdaki kod, uygulama/dış oturum açma yetkilendirme sunucusu tarafından kendi hesaplarını yönetmek için kullanılan tanımlama bilgileri ve Google kimlik doğrulaması sağlar.

`UseOAuthAuthorizationServer` Genişletme yöntemi olduğunu yetkilendirme sunucusu kurmak için. Kurulum seçenekleri şunlardır:

- `AuthorizeEndpointPath`: İstek yolu istemci uygulamaları kullanıcıları almak için nereye Kullanıcı aracısını yönlendireceği bir belirteç veya kod vermek için onay. Bu örneğin, bir eğik çizgiyle başlamalıdır "`/Authorize`".
- `TokenEndpointPath`: İstek yolu istemci uygulamaları, erişim belirteci almak için doğrudan iletişim kurar. Önde gelen eğik çizgiyle "/ Token" gibi ile başlamalı. İstemci verilirse bir [istemci\_gizli](http://tools.ietf.org/html/rfc6749#appendix-A.2), bu uç noktaya sağlanmalıdır.
- `ApplicationCanDisplayErrors`: Kümesine `true` web uygulaması üzerinde özel hata sayfası için istemci doğrulama hatalarının oluşturmak isterse `/Authorize` uç noktası. Çalışmaları yönlendirilmeyen her yere tarayıcı istemci uygulamaya örneğin yedekleme için yalnızca bu gereklidir, `client_id` veya `redirect_uri` yanlış. `/Authorize` "Oauth. görmek uç nokta beklediğiniz Hata","oauth. ErrorDescription"ve"oauth. ErrorUri"özelliklerinin OWIN ortamına eklenir.

    > [!NOTE]
    > Aksi takdirde true, yetkilendirme sunucusu varsayılan bir hata sayfası ile hata ayrıntılarını döndürür.
- `AllowInsecureHttp`: Yetkilendirme ve belirteç isteklerinin HTTP URI adreslerine ulaşmak ve gelen izin vermek için izin verilecekse true `redirect_uri` HTTP URI adreslerine sahip olmasına istek parametrelerini yetkilendirin.

    > [!WARNING]
    > Güvenlik - yalnızca geliştirme için budur.
- `Provider`: Yetkilendirme sunucusu ara yazılımı tarafından başlatılan olayları işlemek üzere uygulama tarafından sağlanan nesne. Uygulama arabirimi tamamen uygulayabilir veya bir örneğini oluşturabilir `OAuthAuthorizationServerProvider` ve temsilcileri destekleyen bu sunucunun OAuth akışlar için gerekli atayabilir.
- `AuthorizationCodeProvider`: İstemci uygulamaya geri dönmek için tek kullanımlık bir yetki kodu oluşturur. Uygulama olacak şekilde OAuth sunucusunun güvenli **gerekir** sağlamak için bir örneği `AuthorizationCodeProvider` nerede belirteci tarafından üretilen `OnCreate/OnCreateAsync` olay tek bir çağrı için geçerli kabul `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Gerektiğinde yeni bir erişim belirteci oluşturmak için kullanılabilen bir yenileme belirteci oluşturur. Sağlanmadı yetkilendirme sunucusu yenileme belirteçleri döndürmez varsa `/Token` uç noktası.

## <a name="account-management"></a>Hesap Yönetimi

OAuth olduğu ya da nasıl kullanıcı hesap bilgilerini yönetmek önemli değil. Sahip [ASP.NET Identity](../../../identity/index.md) sorumlu olduğu. Bu öğreticide, biz hesabı yönetim kodu basitleştirmek ve OWIN tanımlama bilgisi ara yazılımın kullanılması, kullanıcı oturum açabilir emin olmanız yeterlidir. Basitleştirilmiş örnek kod için işte `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` İstemci, kayıtlı yeniden yönlendirme URL'si ile doğrulamak için kullanılır. `ValidateClientAuthentication` temel düzeni üst bilgi ve istemci kimlik bilgilerini almak için form gövdesini denetler.

Oturum açma sayfası aşağıda gösterilmektedir:

![](owin-oauth-20-authorization-server/_static/image1.png)

IETF'ın OAuth 2 gözden [yetkilendirme kodu verme](http://tools.ietf.org/html/rfc6749#section-4.1) now bölümü.

**Sağlayıcı** (aşağıdaki tabloda) olan [oauthauthorizationserveroptions öğelerinde](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Tür sağlayıcısı `OAuthAuthorizationServerProvider`, tüm OAuth sunucu olaylarını içerir.

| Yetkilendirme kodu verme bölümünden akış adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (A) istemci, yetkilendirme uç noktasına kaynak sahibinin kullanıcı aracısını yönlendirerek akışı başlatır. İstemci, istemci tanımlayıcısını, istenen kapsamı, yerel durumu ve geri erişim verilen (veya reddedilen sonra), yetkilendirme sunucusu kullanıcı aracısını göndereceğiniz bir yeniden yönlendirme URI'si içerir. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) yetkilendirme sunucusu (kullanıcı aracısı) aracılığıyla kaynak sahibi kimliğini doğrular ve kaynak sahibi erişim izni verilir veya istemci erişim isteğini reddederse olup olmadığını belirler. | **&lt;Kullanıcı erişim izni verirse&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) varsayılarak kaynak sahibi erişim izni verirse yetkilendirme sunucusu kullanıcı aracısını kullanarak sağlanan URI yeniden yönlendirmesi istemciye yönlendirir (istekte veya istemci kaydında) daha eski. ... |  |
|  |  |
| (D) istemci, önceki adımda alınan yetkilendirme kodunu ekleyerek yetkilendirme sunucusunun belirteç uç noktasından bir erişim belirteci ister. İsteği yaparken, istemci yetkilendirme sunucusunda kimliğini doğrular. İstemci doğrulama yetkilendirme kodunu almak için URI kullanılan yeniden yönlendirme ekler. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Örnek uygulama için `AuthorizationCodeProvider.CreateAsync` ve `ReceiveAsync` oluşturulmasını ve yetkilendirme kodu doğrulama denetlemek için aşağıda gösterilmiştir.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Yukarıdaki kod, kod ve kimlik anahtarı depolamak ve kimlik kodu aldıktan sonra geri yüklemek için bellek içi eşzamanlı dizin kullanır. Gerçek bir uygulamada kalıcı veri deposu tarafından geçecekti. Kaynak sahibi istemciye erişim vermek için yetkilendirme uç noktadır. Genellikle, bir düğmeye basıp verme onaylamak izin vermek için bir kullanıcı arabirimi gerekir. OWIN OAuth ara yazılım, yetkilendirme uç noktası işlemek uygulama kodu sağlar. Adlı bir MVC denetleyicisi kullandığımız örnek uygulamamızı `OAuthController` işlenmesi için. Örnek uygulamada şu şekildedir:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize` Eylemi, kullanıcı için yetkilendirme sunucusu girdikten sonra ilk denetleyecek. Değilse, kimlik doğrulaması ara yazılımı tanımlama bilgisinin "Uygulama" kullanarak kimlik doğrulaması için çağıranı sorunları ve oturum açma sayfasına yönlendirir. (Bkz. Yukarıdaki vurgulanmış kodu.) Kullanıcı oturum açtı, aşağıda gösterildiği gibi Authorize görünümü işlemek:

![](owin-oauth-20-authorization-server/_static/image2.png)

Varsa **Grant** düğmesi seçili `Authorize` yeni bir "Bearer" kimliği ve oturum açın. Bu eylem oluşturur. Yetkilendirme sunucusu bir taşıyıcı belirteç oluşturun ve JSON yükü istemciye geri göndermek için tetikler.

### <a name="implicit-grant"></a>Örtülü izin

IETF'ın OAuth 2'ye bakın [örtük vermeyi](http://tools.ietf.org/html/rfc6749#section-4.2) now bölümü.

 [Örtük vermeyi](http://tools.ietf.org/html/rfc6749#section-4.2) Şekil 4'teki flow, flow ve ara yazılım, OWIN OAuth eşleme izler.

| Örtülü izin bölümünden akış adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (A) istemci, yetkilendirme uç noktasına kaynak sahibinin kullanıcı aracısını yönlendirerek akışı başlatır. İstemci, istemci tanımlayıcısını, istenen kapsamı, yerel durumu ve geri erişim verilen (veya reddedilen sonra), yetkilendirme sunucusu kullanıcı aracısını göndereceğiniz bir yeniden yönlendirme URI'si içerir. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) yetkilendirme sunucusu (kullanıcı aracısı) aracılığıyla kaynak sahibi kimliğini doğrular ve kaynak sahibi erişim izni verilir veya istemci erişim isteğini reddederse olup olmadığını belirler. | **&lt;Kullanıcı erişim izni verirse&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) varsayılarak kaynak sahibi erişim izni verirse yetkilendirme sunucusu kullanıcı aracısını kullanarak sağlanan URI yeniden yönlendirmesi istemciye yönlendirir (istekte veya istemci kaydında) daha eski. ... |  |
|  |  |
| (D) istemci, önceki adımda alınan yetkilendirme kodunu ekleyerek yetkilendirme sunucusunun belirteç uç noktasından bir erişim belirteci ister. İsteği yaparken, istemci yetkilendirme sunucusunda kimliğini doğrular. İstemci doğrulama yetkilendirme kodunu almak için URI kullanılan yeniden yönlendirme ekler. |  |

Yetkilendirme uç noktası zaten uyguladık bu yana (`OAuthController.Authorize` eylem) yetkilendirme kodu verme için otomatik olarak örtük akış etkinleştirir. Not: `Provider.ValidateClientRedirectUri` erişim belirtecini kötü amaçlı istemciler göndermesini örtük verme akışı korur, yeniden yönlendirme URL'si ile istemci Kimliğini doğrulamak için kullanılır ([adam-de-ortadaki adam saldırısı](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Kaynak sahibi parolası kimlik bilgileri verme

IETF'ın OAuth 2'ye bakın [kaynak sahibi parola kimlik bilgileri verme](http://tools.ietf.org/html/rfc6749#section-4.3) now bölümü.

 [Kaynak sahibi parola kimlik bilgileri verme](http://tools.ietf.org/html/rfc6749#section-4.3) Şekil 5'te gösterilen flow, flow ve ara yazılım, OWIN OAuth eşleme izler.

| Kaynak sahibi parola kimlik bilgileri verme bölümünden akış adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (A) kaynak sahibi istemciye kendi kullanıcı adı ve parola ile sağlar. |  |
|  |  |
| (B) istemci, kaynak sahibinden alınan kimlik bilgilerini ekleyerek yetkilendirme sunucusunun belirteç uç noktasından bir erişim belirteci ister. İsteği yaparken, istemci yetkilendirme sunucusunda kimliğini doğrular. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) yetkilendirme sunucusu istemcinin kimliğini doğrular ve kaynak sahibinin kimlik bilgilerini doğrular ve geçerliyse bir erişim belirteci verir. |  |

Örnek uygulama için işte `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Yukarıdaki kod, öğreticinin bu bölümünde açıklamak için tasarlanmıştır ve güvenliğini kullanılmamalıdır veya üretim uygulamaları. Kaynak sahipleri kimlik bilgilerini denetlemez. Bu, her kimlik bilgisi geçerliyse ve bunun için yeni bir kimliği oluşturan varsayar. Yeni kimlik ve erişim belirteci oluştur ve belirteci yenilemek için kullanılır. Lütfen kodu kendi güvenli hesap yönetimi kodla değiştirin.


### <a name="client-credentials-grant"></a>İstemci kimlik bilgileri verme

IETF'ın OAuth 2'ye bakın [istemci kimlik bilgileri verme](http://tools.ietf.org/html/rfc6749#section-4.4) now bölümü.

 [İstemci kimlik bilgileri verme](http://tools.ietf.org/html/rfc6749#section-4.4) Şekil 6 üzerinde gösterilen flow, flow ve ara yazılım, OWIN OAuth eşleme izler.

| İstemci kimlik bilgileri verme bölümünden akış adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (A) istemci yetkilendirme sunucusunda kimlik doğrulaması yapar ve belirteç uç noktasından bir erişim belirteci ister. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) yetkilendirme sunucusu istemcinin kimliğini doğrular ve geçerliyse bir erişim belirteci verir. |  |

Örnek uygulama için işte `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Yukarıdaki kod, öğreticinin bu bölümünde açıklamak için tasarlanmıştır ve güvenliğini kullanılmamalıdır veya üretim uygulamaları. Lütfen kodu kendi güvenli istemci yönetim kodla değiştirin.


### <a name="refresh-token"></a>Belirteci Yenile

IETF'ın OAuth 2'ye bakın [yenileme belirteci](http://tools.ietf.org/html/rfc6749#section-1.5) now bölümü.

 [Yenileme belirteci](http://tools.ietf.org/html/rfc6749#section-1.5) Şekil 2'de gösterilen flow, flow ve ara yazılım, OWIN OAuth eşleme izler.

| İstemci kimlik bilgileri verme bölümünden akış adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (G) istemci yetkilendirme sunucusunda kimlik doğrulaması ve yenileme belirtecini sunarak yeni bir erişim belirteci ister. İstemci kimlik doğrulaması gereksinimleri, istemci türü ve yetkilendirme sunucusu ilkelerini temel alır. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) yetkilendirme sunucusu istemcinin kimliğini doğrular ve yenileme belirteci doğrular ve geçerliyse, yeni bir erişim belirteci (ve isteğe bağlı olarak, yeni bir yenileme belirteci) verir. |  |

Örnek uygulama için işte `Provider.GrantRefreshToken`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Erişim belirteci tarafından korunan kaynak sunucu oluşturma

Bir boş web uygulaması projesi oluşturma ve projedeki paketler aşağıdaki yükleyin:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Başlangıç sınıfı oluşturun ve kimlik doğrulaması ve Web API yapılandırın. Bkz: *AuthorizationServer\ResourceServer\Startup.cs* indirmesindeki örnek.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Bkz: *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* indirmesindeki örnek.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Bkz: *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* indirmesindeki örnek.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` yöntemi, tüm etki alanları için CORS sağlar.
- `UseOAuthBearerAuthentication` yöntemi, alma ve yetkilendirme üst bilgisi istekte taşıyıcı belirtecinden doğrulama OAuth taşıyıcı belirteci kimlik doğrulaması ara yazılımı sağlar.
- `Config.SuppressDefaultHostAuthenticaiton` Varsayılan bastırır konak kimliği doğrulanmış sorumlusundan uygulama, tüm istekleri Bu çağrıdan sonra bu nedenle anonim olacaktır.
- `HostAuthenticationFilter` yalnızca belirtilen kimlik doğrulaması türü için kimlik doğrulamasını etkinleştirir. Bu durumda, taşıyıcı kimlik doğrulaması türü olur.

Kimlik doğrulaması göstermek için geçerli kullanıcının talepler çıktı olarak bir ApiController oluştururuz.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Yetkilendirme sunucusu ve kaynak sunucuda aynı bilgisayarda değilse, OAuth ara yazılım farklı makine anahtarlarını şifrelemek ve şifresini taşıyıcı erişim belirteci için kullanır. Aynı özel anahtar hem projeler arasında paylaşmak için aynı eklediğimiz `machinekey` hem de ayarlama *web.config* dosyaları.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>OAuth 2.0 istemcileri oluşturma

 Kullandığımız [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) istemci kodu basitleştirmek için NuGet paketi.

### <a name="authorization-code-grant-client"></a>Yetkilendirme kodu verme istemci

 Bu istemci, bir MVC uygulamasıdır. Arka ucunuzdan erişim belirteci almak için bir yetkilendirme kodu verme akışı tetikler. Aşağıda gösterildiği gibi bir tek sayfalı sahiptir:

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Authorize** düğmesi, bu istemciye erişim vermek için kaynak sahibi bildirmek için yetkilendirme sunucusu için tarayıcı yönlendirir.
- **Yenile** düğmesi yeni bir erişim belirteci ve yenileme belirteci geçerli yenilemeyi kullanarak belirteci alın.
- **Erişim korumalı kaynak API'si** düğme, geçerli kullanıcının talep verileri almak ve sayfada göstermek için kaynak sunucuda çağıracaktır.

Örnek kodunu işte `HomeController` istemcinin.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` Varsayılan olarak SSL gerektirir. Eklemenize gerek tanıtımımızı HTTP kullandığından, aşağıdaki yapılandırma dosyasında ayarı:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Güvenlik - hiçbir zaman devre dışı bırakma SSL bir üretim uygulaması. Oturum açma kimlik bilgilerinizi artık kablo düz metin olarak gönderiliyor. Yukarıdaki kod, yalnızca yerel örnek hata ayıklama ve araştırma için ' dir.


### <a name="implicit-grant-client"></a>Örtük verme istemci

Bu istemci için JavaScript kullanır:

1. Yeni bir pencere açın ve yetkilendirme sunucusu authorize son nokta yönlendirin.
2. Geri yönlendirir, URL parçaları erişim belirteci elde edersiniz.

Bu işlem aşağıdaki resimde gösterilmektedir:

![](owin-oauth-20-authorization-server/_static/image4.png)

İki sayfa istemci olmalıdır: biri giriş sayfası, diğeri için geri çağırma. JavaScript içinde bulunan kod örneği aşağıdadır *Index.cshtml* dosyası:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Kod içinde işleme geri çağırma işte *SignIn.cshtml* dosyası:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> En iyi uygulama, JavaScript harici bir dosyaya taşıma ve Razor işaretlemesi ile ekleme değil sağlamaktır. Bu örneği basit tutmak için bunlar birleştirilmiştir.


### <a name="resource-owner-password-credentials-grant-client"></a>Kaynak sahibi parolası kimlik bilgileri verme istemci

Biz bir konsol uygulaması bu istemci tanıtım için kullanır. Kod aşağıdaki gibidir:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>İstemci kimlik bilgileri verme istemci

Kaynak sahibi parola kimlik bilgileri verme, burada benzer konsol uygulama kodu:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
