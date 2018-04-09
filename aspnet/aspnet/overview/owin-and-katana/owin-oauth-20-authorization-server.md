---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: OWIN OAuth 2.0 yetkilendirme sunucusu | Microsoft Docs
author: hongyes
description: Bu öğretici OWIN OAuth ara yazılımı kullanarak bir OAuth 2.0 yetkilendirme sunucusu uygulama konusunda size yol gösterecektir. Bu, yalnızca Gr Gelişmiş bir öğreticidir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e5968f8d19191c3f44e9bd58f8e22a39d8d8faff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="owin-oauth-20-authorization-server"></a>OWIN OAuth 2.0 yetkilendirme sunucusu
====================
tarafından [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici OWIN OAuth ara yazılımı kullanarak bir OAuth 2.0 yetkilendirme sunucusu uygulama konusunda size yol gösterecektir. Yalnızca bir OWIN OAuth 2.0 yetkilendirme sunucusu oluşturmak için gereken adımları özetler Gelişmiş bir öğreticidir. Bu adım adım öğretici değildir. [Örnek kodu indirdikten](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > Bu anahat güvenli bir üretim uygulaması oluşturmak için kullanılacak amaçlanmamış. Bu öğretici OWIN OAuth ara yazılımı kullanarak bir OAuth 2.0 yetkilendirme sunucusu uygulamak yalnızca ana hattı sağlamak için tasarlanmıştır.
> 
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> | **Öğreticide gösterilen** | **İle de çalışır** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Masaüstü için Visual Studio 2013 Express](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012 en son güncelleştirme çalışması gerekir, ancak öğreticinin ile test edilmemiştir ve bazı menü seçimlerini ve iletişim kutuları farklı. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Öğretici için doğrudan ilgili olmayan sorularınız varsa, onları nakledebilirsiniz [Katana proje github'da](https://github.com/aspnet/AspNetKatana/). Sorularınız ve yorumlarınız ilgili öğretici için sayfanın sonundaki Açıklamalar bölümüne bakın.


[OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) bir HTTP hizmeti sınırlı erişim elde etmek bir üçüncü taraf uygulama sağlar. Korunan bir kaynağa erişmek için kaynak sahibinin kimlik bilgilerini kullanmak yerine, istemci bir erişim belirteci alır (bir dize olduğu belirli bir kapsamda, yaşam süresi ve diğer erişim öznitelikleri belirten). Erişim belirteçleri üçüncü taraf istemcilere kaynak sahibi onay ile bir yetkilendirme sunucusu tarafından verilir.

Bu öğretici ele alınacaktır:

- Dört yetkilendirme desteklemek için bir yetkilendirme sunucusu oluşturma, verme türlerini ve yenileme belirteçlerini:
    - Yetkilendirme kodu verme
    - Kapalı verin
    - Kaynak sahibi parolası kimlik bilgileri verin
    - İstemci kimlik bilgileri verin
- Bir erişim belirteci tarafından korunan bir kaynak sunucusu oluşturuluyor.
- OAuth 2.0 istemcilerinin oluşturuluyor.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) veya ücretsiz [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express)belirtilen gibi **yazılım sürümleri** sayfanın üst kısmındaki.
- OWIN hakkında bilgi. Bkz: [Katana proje ile çalışmaya başlama](https://msdn.microsoft.com/magazine/dn451439.aspx) ve [OWIN ve Katana yenilikler](index.md).
- Aşina [OAuth](http://tools.ietf.org/html/rfc6749) terminolojisinde dahil olmak üzere [rolleri](http://tools.ietf.org/html/rfc6749#section-1.1), [Protokolü akış](http://tools.ietf.org/html/rfc6749#section-1.2), ve [yetkilendirme verme](http://tools.ietf.org/html/rfc6749#section-1.3). [OAuth 2.0 giriş](http://tools.ietf.org/html/rfc6749#section-1) iyi bir giriş sağlar.

## <a name="create-an-authorization-server"></a>Bir yetkilendirme sunucusu oluşturma

Bu öğreticide, biz kabaca nasıl kullanacağınızı taslak [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) ve bir yetkilendirme sunucusu oluşturmak için ASP.NET MVC. Bu öğretici her adımı içermez gibi yakında tamamlanmış bir örnek için bir yükleme sağlamak umuyoruz. İlk olarak, adlandırılmış bir boş web uygulaması oluşturma *AuthorizationServer* ve aşağıdaki paketler yükleyin:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (veya diğer sosyal oturum paketini Microsoft.Owin.Security.Facebook gibi)

Ekleme bir [OWIN başlangıç sınıfı](owin-startup-class-detection.md) adlı proje kök klasörü altında *başlangıç*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Oluşturma bir *uygulama\_Başlat* klasör. Seçin *uygulama\_Başlat* klasörü ve kullanım indirilen sürümü eklemek için Shift + Alt + A *AuthorizationServer\App\_Start\Startup.Auth.cs* dosya.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Yukarıdaki kod uygulama/dış oturum açma yetkilendirme sunucusu tarafından kendi hesaplarını yönetmek için kullanılan tanımlama bilgileri ve Google kimlik doğrulaması sağlar.

`UseOAuthAuthorizationServer` Uzantısı yöntemdir yetkilendirme sunucusu kurmak için. Kurulum seçenekleri şunlardır:

- `AuthorizeEndpointPath`: İstemci uygulamalarının kullanıcıların elde etmek için Kullanıcı aracısını burada yönlendirir istek yolu onay kodu ya da belirteci vermek için. Bir eğik çizgiyle, örneğin, başlamalıdır "`/Authorize`".
- `TokenEndpointPath`: Erişim belirteci almak için istek yolu istemci uygulamaları doğrudan iletişim kurar. Bir eğik "/ Token" gibi çizgiyle başlamalıdır. İstemci verilirse bir [istemci\_gizli](http://tools.ietf.org/html/rfc6749#appendix-A.2), bu uç noktaya sağlanmalıdır.
- `ApplicationCanDisplayErrors`: Kümesine `true` web uygulaması için istemci doğrulama hatalarının bir özel hata sayfası oluştur isterse `/Authorize` uç noktası. Burada tarayıcı olmayan yönlendirildiği durumlarda istemci uygulamaya örneğin yedekleme için yalnızca bu gereklidir, ne zaman `client_id` veya `redirect_uri` yanlış. `/Authorize` Uç nokta bekler "oauth. görmek Hata","oauth. ErrorDescription"ve"oauth. ErrorUri"özelliklerinin OWIN ortamına eklenir. 

    > [!NOTE]
    > Değilse true, yetkilendirme sunucusu varsayılan hata sayfası ile hata ayrıntılarını döndürür.
- `AllowInsecureHttp`: Girintili için yetkilendirme ve belirteç isteklerinin HTTP URI adreslerine gelmesi ve gelen izin vermek için izin `redirect_uri` İstek parametreleri HTTP URI adreslerine sahip yetkilendirin. 

    > [!WARNING]
    > Güvenlik - yalnızca geliştirme budur.
- `Provider`: Yetkilendirme sunucusu ara yazılımı tarafından başlatılan olayları işlemek üzere uygulama tarafından sağlanan nesne. Uygulama arabirimi tamamen uygulayabilir ya da bir örneğini oluşturabilir `OAuthAuthorizationServerProvider` ve Temsilciler bu sunucunun desteklediği OAuth akışlar için gerekli atayın.
- `AuthorizationCodeProvider`: İstemci uygulamasına geri dönmek için bir tek kullanımlık bir yetki kodu oluşturur. Uygulama olmasını OAuth sunucusunun güvenli **gerekir** sağlamak için bir örneği `AuthorizationCodeProvider` burada belirteç üretilen tarafından `OnCreate/OnCreateAsync` olay yalnızca bir çağrı için geçerli kabul `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Gerektiğinde yeni bir erişim belirteci oluşturmak için kullanılabilen bir yenileme belirteci oluşturur. Sağlanmamış yetkilendirme sunucusu gelen yenileme belirteçleri döndürmez varsa `/Token` uç noktası.

## <a name="account-management"></a>Hesap Yönetimi

OAuth nereye veya nasıl kullanıcı hesap bilgilerini yönetmek önemli değildir. Bunun [ASP.NET Identity](../../../identity/index.md) sorumlu olduğu. Bu öğreticide, biz hesap yönetimi kodu basitleştirmek ve yalnızca o kullanıcı OWIN tanımlama bilgisi Ara kullanarak oturum açabilir emin olun. Basitleştirilmiş örnek kodunu işte `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` kayıtlı yeniden yönlendirme URL'sini istemcisiyle doğrulamak için kullanılır. `ValidateClientAuthentication` temel düzeni üstbilgi ve istemcinin kimlik bilgilerini almak için form gövdesini denetler.

Oturum açma sayfasına aşağıda gösterilmiştir:

![](owin-oauth-20-authorization-server/_static/image1.png)

IETF's OAuth 2 gözden [yetkilendirme kodu verme](http://tools.ietf.org/html/rfc6749#section-4.1) now bölümü. 

**Sağlayıcı** (aşağıdaki tabloda) olan [oauthauthorizationserveroptions öğelerinde](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Tür sağlayıcısı `OAuthAuthorizationServerProvider`, tüm OAuth sunucu olaylarını içerir. 

| Yetkilendirme kodu verme bölümünden akışı adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (A) istemci yetkilendirme uç noktası için kaynak sahibinin kullanıcı aracısını yönlendirerek akışını başlatır. İstemci, istemci tanıtıcısını, istenen kapsamı, yerel durumu ve geri erişim izni (engellendi veya sonra), yetkilendirme sunucusu kullanıcı aracısını göndereceğiniz bir yeniden yönlendirme URI'si içerir. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B yetkilendirme sunucusu (aracılığıyla kullanıcı aracısı) kaynak sahibinin kimliğini doğrular ve kaynak sahibi verir veya istemcinin erişim isteği reddeder oluşturur. | **&lt;Kullanıcı erişim verirse&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) kaynak sahibi olduğu varsayılarak erişim verir, yetkilendirme sunucusu kullanıcı aracısını yeniden yönlendirme URI'si sağlanan kullanarak istemciye yönlendirir önceki (istek veya istemci kaydı sırasında). ... |  |
|  |  |
| (D) istemci, önceki adımda alınan yetkilendirme kodu dahil ederek, yetkilendirme sunucunun belirteç uç noktasından bir erişim belirteci ister. İsteği yaparken yetkilendirme sunucusu ile istemci kimliğini doğrular. İstemci doğrulaması için yetkilendirme kodu almak için URI kullanılan yeniden yönlendirme içerir. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Örnek uygulama için `AuthorizationCodeProvider.CreateAsync` ve `ReceiveAsync` oluşturulmasını ve yetkilendirme kodu doğrulanması denetlemek için aşağıda gösterilmiştir.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Yukarıdaki kod bir bellek içi eşzamanlı dizin kod ve kimlik bilet depolamak ve kimlik kodu aldıktan sonra geri yüklemek için kullanır. Gerçek bir uygulamada kalıcı veri deposu değiştirilmesi. Kaynak sahibi istemciye erişim vermek için yetkilendirme uç noktadır. Genellikle, bir düğmeye tıklayın ve grant onaylamak izin vermek için bir kullanıcı arabirimi gerekir. OWIN OAuth Ara yetkilendirme uç noktası işlemek uygulama kodu sağlar. Adlı bir MVC denetleyicisi kullanırız bizim örnek uygulamamız `OAuthController` işlenmesi için. Örnek uygulama şöyledir:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

`Authorize` Eylemi, kullanıcı yetkilendirme sunucu üzerinde oturum açan değilse ilk denetleyecek. Değilse, kimlik doğrulaması ara yazılımı "Uygulama" tanımlama bilgisi kullanarak kimlik doğrulaması için arayan sınar ve oturum açma sayfasına yönlendirir. (Bkz. vurgulanmış kodu yukarıdaki.) Kullanıcının oturum açtığı, aşağıda gösterildiği gibi Authorize görünümü işlemek:

![](owin-oauth-20-authorization-server/_static/image2.png)

Varsa **Grant** düğmesi seçilirse, `Authorize` eylemi, yeni bir "Bearer" kimlik ve bunu oturum oluşturacaktır. Bir taşıyıcı belirteç oluşturmak ve JSON yükü istemciye geri göndermek için yetkilendirme sunucusu tetikler. 

### <a name="implicit-grant"></a>Kapalı verin

IETF's OAuth 2'ye başvurmak [örtük Grant](http://tools.ietf.org/html/rfc6749#section-4.2) now bölümü.

 [Örtük Grant](http://tools.ietf.org/html/rfc6749#section-4.2) Şekil 4'te gösterilen akış akışıdır ve hangi OWIN OAuth eşleme ara yazılım izler.  

| Örtük Grant bölümünden akışı adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (A) istemci yetkilendirme uç noktası için kaynak sahibinin kullanıcı aracısını yönlendirerek akışını başlatır. İstemci, istemci tanıtıcısını, istenen kapsamı, yerel durumu ve geri erişim izni (engellendi veya sonra), yetkilendirme sunucusu kullanıcı aracısını göndereceğiniz bir yeniden yönlendirme URI'si içerir. | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B yetkilendirme sunucusu (aracılığıyla kullanıcı aracısı) kaynak sahibinin kimliğini doğrular ve kaynak sahibi verir veya istemcinin erişim isteği reddeder oluşturur. | **&lt;Kullanıcı erişim verirse&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) kaynak sahibi olduğu varsayılarak erişim verir, yetkilendirme sunucusu kullanıcı aracısını yeniden yönlendirme URI'si sağlanan kullanarak istemciye yönlendirir önceki (istek veya istemci kaydı sırasında). ... |  |
|  |  |
| (D) istemci, önceki adımda alınan yetkilendirme kodu dahil ederek, yetkilendirme sunucunun belirteç uç noktasından bir erişim belirteci ister. İsteği yaparken yetkilendirme sunucusu ile istemci kimliğini doğrular. İstemci doğrulaması için yetkilendirme kodu almak için URI kullanılan yeniden yönlendirme içerir. |  |

Biz yetkilendirme uç nokta zaten uygulanmış olduğundan (`OAuthController.Authorize` eylemi) yetkilendirme kodu verme için otomatik olarak örtük akışını etkinleştirir. Not: `Provider.ValidateClientRedirectUri` erişim belirteci kötü amaçlı olarak istemciler göndermesini dolaylı grant akış korur, yeniden yönlendirme URL'si ile istemci Kimliğini doğrulamak için kullanılır ([Man-in--middle saldırı](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Kaynak sahibi parolası kimlik bilgileri verin

IETF's OAuth 2'ye başvurmak [kaynak sahibi parolası kimlik bilgileri sağlama](http://tools.ietf.org/html/rfc6749#section-4.3) now bölümü.

 [Kaynak sahibi parolası kimlik bilgileri sağlama](http://tools.ietf.org/html/rfc6749#section-4.3) Şekil 5'te gösterilen akış akışıdır ve hangi OWIN OAuth eşleme ara yazılım izler.  

| Kaynak sahibi parolası kimlik bilgileri sağlama bölümünden akışı adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (A) kaynak sahibi, kullanıcı adı ve parola ile istemci sağlar. |  |
|  |  |
| (B) istemci bir erişim belirteci kaynak sahibinden alınan kimlik bilgileri dahil ederek yetkilendirme sunucusunun belirteç uç noktasından ister. İsteği yaparken yetkilendirme sunucusu ile istemci kimliğini doğrular. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) yetkilendirme sunucusu istemcinin kimliğini doğrular ve kaynak sahibi kimlik bilgilerini doğrular ve geçerliyse, bir erişim belirteci verir. |  |

Örnek uygulama için işte `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Yukarıdaki kod öğreticinin bu bölümünde açıklamak için tasarlanmıştır ve güvenliğini kullanılmamalıdır veya üretim uygulamaları. Kaynak sahipleri kimlik bilgilerini denetlemez. Her kimlik bilgilerinin geçerli olduğunu ve bunun için yeni bir kimlik oluşturur varsayar. Yeni kimlik, erişim belirtecini oluşturmak ve belirteç yenilemek için kullanılır. Lütfen kodu kendi güvenli hesap yönetimi kod ile değiştirin.


### <a name="client-credentials-grant"></a>İstemci kimlik bilgileri verin

IETF's OAuth 2'ye başvurmak [istemci kimlik bilgileri sağlama](http://tools.ietf.org/html/rfc6749#section-4.4) now bölümü.

 [İstemci kimlik bilgileri sağlama](http://tools.ietf.org/html/rfc6749#section-4.4) Şekil 6'da gösterilen akış akışıdır ve hangi OWIN OAuth eşleme ara yazılım izler.  

| İstemci kimlik bilgileri sağlama bölümünden akışı adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (A) istemci yetkilendirme ile kimlik doğrulamasını ve belirteç uç noktasından bir erişim belirteci ister. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B yetkilendirme sunucusu istemci kimliğini doğrular ve geçerliyse, bir erişim belirteci verir. |  |

Örnek uygulama için işte `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Yukarıdaki kod öğreticinin bu bölümünde açıklamak için tasarlanmıştır ve güvenliğini kullanılmamalıdır veya üretim uygulamaları. Lütfen kodu kendi güvenli istemci Yönetimi kod ile değiştirin.


### <a name="refresh-token"></a>Belirteç Yenile

IETF's OAuth 2'ye başvurmak [yenileme belirteci](http://tools.ietf.org/html/rfc6749#section-1.5) now bölümü.

 [Yenileme belirteci](http://tools.ietf.org/html/rfc6749#section-1.5) Şekil 2'de gösterilen akış akışıdır ve hangi OWIN OAuth eşleme ara yazılım izler.  

| İstemci kimlik bilgileri sağlama bölümünden akışı adımları | Örnek indirme ile aşağıdaki adımları gerçekleştirir: |
| --- | --- |
|  |  |
| (G) istemci, yetkilendirme sunucusu ile kimlik doğrulaması ve yenileme belirtecini sunan yeni bir erişim belirteci ister. İstemci kimlik doğrulaması gereksinimleri, istemci türüne ve yetkilendirme sunucusu ilkelerini temel alır. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (Y yetkilendirme sunucusu istemcinin kimliğini doğrular ve yenileme belirteci doğrular ve geçerliyse, yeni bir erişim belirteci (ve isteğe bağlı olarak, yeni bir yenileme belirteci) verir. |  |

Örnek uygulama için işte `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Erişim belirteci tarafından korunan bir kaynak sunucu oluşturma

Bir boş web uygulama projesi oluşturun ve projeyi paketlerinde aşağıdaki yükleyin:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Başlangıç sınıfı oluşturun ve kimlik doğrulama ve Web API yapılandırın. Bkz: *AuthorizationServer\ResourceServer\Startup.cs* örnek indirme içinde.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Bkz: *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* örnek indirme içinde.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Bkz: *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* örnek indirme içinde.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` yöntemi, tüm etki alanları için CORS sağlar.
- `UseOAuthBearerAuthentication` yöntemi, alır ve talep yetkilendirme başlığından taşıyıcı belirtecini doğrula OAuth taşıyıcı belirteci kimlik doğrulaması ara yazılımı sağlar.
- `Config.SuppressDefaultHostAuthenticaiton` Varsayılan bastırır ana bilgisayar kimliği doğrulanmış sorumlu uygulamasından, tüm istekler bu çağrısından sonra bu nedenle anonim olacaktır.
- `HostAuthenticationFilter` yalnızca belirtilen kimlik doğrulama türü için kimlik doğrulamasını etkinleştirir. Bu durumda, taşıyıcı kimlik doğrulaması türü değil.

Kimlik doğrulaması göstermek için geçerli kullanıcının talepleri çıktısını almak için bir ApiController oluşturuyoruz.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Yetkilendirme sunucusu ve kaynak sunucuda aynı bilgisayarda değilse, OAuth Ara şifrelemek ve taşıyıcı erişim belirteci şifresini çözmek için farklı makine anahtarları kullanır. Aynı özel anahtarı her iki projeler arasında paylaşmak için aynı eklediğimiz `machinekey` hem de ayarlama *web.config* dosyaları.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>OAuth 2.0 istemcileri oluşturma

 Kullanırız [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) istemci kodu basitleştirmek için NuGet paketi.

### <a name="authorization-code-grant-client"></a>Yetkilendirme kodu verme istemci

 Bir MVC uygulaması istemcidir. Arka ucunuzdan erişim belirteci almak üzere bir yetkilendirme kodu verme akışını tetikler. Aşağıda gösterildiği gibi tek bir sayfayla sahiptir:

![](owin-oauth-20-authorization-server/_static/image3.png)

- **Authorize** düğmesi yönlendirmek tarayıcı bu istemciye erişim vermek üzere kaynak sahibi bildirmek için yetkilendirme sunucusu için.
- **Yenileme** düğmesi elde edersiniz yeni bir erişim belirteci ve yenileme belirteci geçerli yenileme kullanarak belirteci.
- **Erişim korumalı kaynak API** düğmesi, geçerli kullanıcının talep verileri almak ve bunları sayfada göstermek için kaynak sunucunun çağıracaktır.

Örnek kodunu işte `HomeController` istemcinin.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` Varsayılan olarak SSL gerektirir. Kendi Tanıtımı HTTP kullandığından, eklemek istediğiniz ayar yapılandırma dosyasında aşağıdaki:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Güvenlik - hiçbir zaman devre dışı bırak SSL bir üretim uygulamasında. Oturum açma kimlik bilgilerinizi şimdi ağ üzerinden düz metin olarak gönderiliyor. Yukarıdaki kod yalnızca yerel örnek hata ayıklama ve araştırması içindir.


### <a name="implicit-grant-client"></a>Örtük Grant istemci

Bu istemci için JavaScript kullanıyor:

1. Yeni bir pencerede açmak ve yetkilendirme sunucusu authorize son noktası yönlendirin.
2. Yeniden yönlendirildiklerinde erişim URL'si parçaları belirteci alın.

Aşağıdaki resimde bu süreci göstermektedir:

![](owin-oauth-20-authorization-server/_static/image4.png)

İstemci iki sayfa olması gerekir: bir giriş sayfası diğeri geri çağırma. JavaScript kodu bulunan örnek işte *Index.cshtml* dosyası:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Kodda işleme geri çağırma işte *SignIn.cshtml* dosyası:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> En iyi uygulama, JavaScript harici bir dosyaya taşıma ve Razor biçimlendirme ile embed değil olmaktır. Bu örneği basit tutmak için bunlar birleştirilmiştir.


### <a name="resource-owner-password-credentials-grant-client"></a>Kaynak sahibi parolası Grant istemci kimlik bilgileri

Biz bir konsol uygulaması bu istemci gösteri için kullanır. Kod aşağıdaki gibidir:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>İstemci kimlik bilgileri verin istemci

Kaynak sahibi parolası kimlik bilgileri sağlama, burada benzer konsol uygulama kodu:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
