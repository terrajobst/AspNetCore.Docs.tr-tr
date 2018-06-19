---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Bireysel hesaplar ve ASP.NET Web API 2.2 yerel oturum açma ile Web API güvenliğini sağlama | Microsoft Docs
author: MikeWasson
description: Bu konu, üyelik veritabanına karşı kimlik doğrulaması için OAuth2 kullanarak bir web API güvenliğini sağlama gösterir. Eğitmen Visual Studio 201 kullanılan yazılım sürümleri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e2056e769edf972cba830b31cf37f6418148ca73
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28038292"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Bireysel hesaplar ve ASP.NET Web API 2.2 yerel oturum açma ile Web API güvenliğini sağlama
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Örnek uygulamayı indirin](https://github.com/MikeWasson/LocalAccountsApp)

> Bu konu, üyelik veritabanına karşı kimlik doğrulaması için OAuth2 kullanarak bir web API güvenliğini sağlama gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013 Güncelleştirme 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


Visual Studio 2013'te Web API proje şablonu, kimlik doğrulaması için üç seçenek sunar:

- **Bireysel hesaplar.** Uygulama bir üyelik veritabanı kullanır.
- **Kurumsal hesaplar.** Kullanıcılar kendi Azure Active Directory, Office 365 veya şirket içi Active Directory kimlik bilgileri oturum açın.
- **Windows kimlik doğrulaması.** Bu seçenek, Intranet uygulamalara yöneliktir ve Windows kimlik doğrulaması IIS Modülü kullanır.

Bu seçenekler hakkında daha fazla ayrıntı için bkz: [Visual Studio 2013'da ASP.NET Web projeleri oluşturma](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Bireysel hesaplar oturum açmak bir kullanıcı için iki yol sağlar:

- **Yerel oturum açma**. Kullanıcı, bir kullanıcı adı ve parola girme sitede kaydeder. Uygulaması parola karması üyelik veritabanında depolar. Kullanıcı oturum açtığında ASP.NET Identity sistem parolayı doğrular.
- **Sosyal oturum açma**. Kullanıcı, Facebook, Microsoft veya Google gibi bir dış hizmet oturum açar. Uygulama hala üyelik veritabanında kullanıcısı için bir giriş oluşturur, ancak herhangi bir kimlik bilgisi depolamaz. Dış hizmetinde oturum açarak kullanıcının kimliğini doğrular.

Bu makalede yerel oturum açma senaryo arar. Hem yerel hem de sosyal oturum açma isteklerinin kimlik doğrulaması için OAuth2 Web API'sini kullanır. Ancak, yerel ve sosyal oturum açma için kimlik bilgisi akış farklıdır.

Bu makalede, ı kullanıcının oturum açma ve web API'si için kimliği doğrulanmış AJAX çağrıları Gönder sağlayan basit bir uygulama gösterir. Örnek kodu indirdikten [burada](https://github.com/MikeWasson/LocalAccountsApp). Benioku dosyasını Visual Studio'da sıfırdan örnek oluşturmayı açıklar.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Örnek uygulaması Knockout.js veri bağlama ve AJAX istekleri göndermek için jQuery kullanır. Bu makale için Knockout.js bilmeniz gerek kalmaması ı AJAX çağrılarını odaklanan.

Yol boyunca ı açıklamak:

- Uygulama istemci tarafında yaptıklarını.
- Sunucu üzerinde olanlar.
- HTTP trafiği ortasında.

İlk olarak, biz bazı OAuth2 terminolojisi tanımlamanız gerekir.

- *Kaynak*. Korunabilir veriler bazı parçası.
- *Kaynak sunucuda*. Kaynak barındıran sunucu.
- *Kaynak sahibi*. Bir kaynağa erişim izni verebilirsiniz varlık. (Genellikle kullanıcı.)
- *İstemci*: kaynağa erişim isteyen uygulama. Bu makalede, istemci bir web tarayıcısıdır.
- *Erişim belirteci*. Bir kaynağa erişim izni veren bir belirteç.
- *Taşıyıcı belirteci*. Belirli bir erişim belirteciyle herkes belirteci kullanabilirsiniz özellik türü. Diğer bir deyişle, bir istemci, bir şifreleme anahtarının veya diğer gizli bir taşıyıcı belirteç kullanmaya gerek yoktur. Bu nedenle, taşıyıcı belirteçlerini yalnızca HTTPS üzerinden kullanılmalıdır ve görece kısa kullanım süreleri sahip olmalıdır.
- *Yetkilendirme sunucusu*. Erişim belirteçleri sunan bir sunucu.

Bir uygulama, yetkilendirme sunucusu ve kaynak sunucu davranamaz. Web API projesi şablonunu bu deseni izler.

## <a name="local-login-credential-flow"></a>Yerel oturum açma kimlik bilgileri akışını

Yerel oturum açma için Web API'sini kullanan [kaynak sahibi parolası akış](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) OAuth2 tanımlanmış.

1. Kullanıcı adı ve parola istemciyi girer.
2. İstemci bu kimlik bilgileri yetkilendirme sunucusuna gönderir.
3. Yetkilendirme sunucusu kimlik bilgilerini doğrular ve bir erişim belirteci döndürür.
4. Korunan bir kaynağa erişmek için istemci erişim belirteci HTTP isteğinin yetkilendirme üst bilgi içerir.

![](individual-accounts-in-web-api/_static/image3.png)

Seçtiğinizde, **bireysel hesaplar** Web API proje şablonu proje kullanıcı kimlik bilgilerini doğrular ve belirteçleri yetkilendirme sunucusu içeriyor. Aşağıdaki diyagramda, Web API bileşenleri açısından aynı kimlik bilgisi akışı gösterilmektedir.

![](individual-accounts-in-web-api/_static/image4.png)

Bu senaryoda, Web API denetleyicilerinin kaynak sunucuları gibi davranır. Erişim belirteçleri, bir kimlik doğrulaması filtresini doğrular ve **[Authorize]** özniteliği bir kaynağı korumak için kullanılır. Denetleyici veya eylem olduğunda **[Authorize]** özniteliği, bu denetleyicideki tüm istekleri veya eylem kimlik doğrulaması gerekir. Aksi takdirde, yetkilendirme reddedildi ve Web API 401 (yetkisiz) hatası döndürür.

Yetkilendirme sunucusu ve her ikisi de çağırabilir kimlik doğrulaması filtresini bir [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) OAuth2 ayrıntılarını işleyen bileşeni. Bu öğreticinin ilerleyen bölümlerinde daha ayrıntılı tasarım açıklamak.

## <a name="sending-an-unauthorized-request"></a>Yetkisiz bir isteği gönderme

Başlamak için uygulamayı çalıştırın ve **API çağrısı** düğmesi. İstek tamamlandığında, bir hata iletisi görmelisiniz **sonuç** kutusu. İstek istek yetkisiz olacak şekilde bir erişim belirteci içermediğinden olmasıdır.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**API çağrısı** düğmesi gönderir AJAX isteği ~/api/değerleri, bir Web API denetleyici eylemi çağırır. Burada, AJAX isteği gönderir JavaScript kodu bölümünde verilmiştir. Örnek uygulamasında tüm JavaScript uygulama kodunun bulunan Scripts\app.js dosyasında.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Kullanıcı oturum açtığında kadar taşıyıcı belirteci yok ve bu nedenle hiçbir Authorization Üstbilgisi istekte yok. Bu, bir 401 hatası döndürmek istek neden olur.

HTTP isteği aşağıdadır. (Kullandım [Fiddler](http://www.telerik.com/fiddler) HTTP trafiği yakalamak için.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP yanıtı:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Yanıt için taşıyıcı ayarlamak sınama ile bir Www-Authenticate üstbilgisi içerdiğine dikkat edin. Bu, sunucunun bir taşıyıcı belirteci bekliyor gösterir.

## <a name="register-a-user"></a>Bir kullanıcı kaydı

İçinde **kaydetmek** bölüm uygulamasının bir e-posta ve parola girin ve tıklayın **kaydetmek** düğmesi.

Bu örnek için geçerli bir eposta adresi kullanmanız gerekmez, ancak gerçek bir uygulama adresini doğrulamak. (Bkz [günlüğünde, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Parola için "Parola1!" gibi bir büyük harf, küçük harf, sayı ve alfasayısal olmayan karakter kullanın. Uygulama basit tutmak için istemci tarafı doğrulama ı sol böylece parola biçiminde bir sorun varsa, 400 (Hatalı istek) hata elde.

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**Kaydetmek** düğmesini ~/api/Account/Register için bir POST isteği gönderir /. İstek gövdesinde adı ve parola tutan bir JSON nesnesidir. İsteği gönderen JavaScript kod aşağıdaki gibidir:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP isteği:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP yanıtı:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Bu istek tarafından işlenen `AccountController` sınıfı. Dahili olarak, `AccountController` üyelik veritabanını yönetmek için ASP.NET kimliğini kullanır.

Uygulama Visual Studio'dan yerel olarak çalıştırırsanız, kullanıcı hesaplarını yerel veritabanı içinde AspNetUsers tablosunda depolanır. Visual Studio'da tabloları görüntülemek için tıklatın **Görünüm** menüsünde, select **Sunucu Gezgini**, ardından genişletin **veri bağlantıları**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Şu ana kadar herhangi OAuth yapmadıysanız, ancak biz bir erişim belirteci istediğinde şimdi eylem OAuth yetkilendirme sunucusu göreceğiz. İçinde **oturum** alan örnek uygulamasının e-posta ve parola girin ve tıklatın **oturum**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**Oturum** düğmesi belirteç uç noktası için bir istek gönderir. İstek gövdesini aşağıdaki formu url kodlanmış verileri içerir:

- vermek\_türü: "parola"
- Kullanıcı adı: &lt;kullanıcının e-posta&gt;
- Parola: &lt;parola&gt;

AJAX isteği gönderir JavaScript kod aşağıdaki gibidir:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

İstek başarılı olursa, yetkilendirme sunucusu yanıt gövdesinde bir erişim belirteci döndürür. Biz API için istekleri gönderirken, daha sonra kullanmak üzere oturum depolama belirteç depolamak dikkat edin. Bazı kimlik doğrulama (örneğin, tanımlama bilgisi tabanlı kimlik doğrulaması) formlarda farklı olarak, tarayıcı otomatik olarak erişim belirteci sonraki istekler dahil edilmez. Uygulamanın bunu açıkça yapmalısınız. Olan çok iyi bir şey bu sınırladığından [CSRF güvenlik açıkları](preventing-cross-site-request-forgery-csrf-attacks.md).

HTTP isteği:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

İstek kullanıcının kimlik bilgilerini içeren görebilirsiniz. *Gerekir* Aktarım Katmanı Güvenliği sağlamak için HTTPS kullanın.

HTTP yanıtı:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Okunabilirliği kolaylaştırmak için I JSON girintili ve oldukça uzun erişim belirteci kesildi.

`access_token`, `token_type`, Ve `expires_in` özellikleri, OAuth2 belirtimi tarafından tanımlanır. Diğer özellikler (`userName`, `.issued`, ve `.expires`) yalnızca bilgilendirme amacıyla durumdadır. Bu ek özellikler ekleyen kod Bul `TokenEndpoint` /Providers/ApplicationOAuthProvider.cs dosyasındaki yöntemi.

## <a name="send-an-authenticated-request"></a>Kimliği doğrulanmış bir isteği gönder

Bir taşıyıcı belirtecine sahip olduğumuz, kimliği doğrulanmış bir isteği API'sine yapabilirsiniz. Bu istek Authorization Üstbilgisi ayarlayarak yapılır. Tıklatın **API çağrısı** yeniden görmek için düğmesi.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP isteği:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP yanıtı:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Oturumu Kapat

Tarayıcı kimlik bilgileri veya erişim belirteci önbelleğe almaması nedeniyle günlük "belirteci oturum depolama biriminden kaldırarak unutulması", sorunudur:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Bireysel hesaplar proje şablonu anlama

Seçtiğinizde, **bireysel hesaplar** ASP.NET Web uygulaması proje şablonu proje içerir:

- Bir OAuth2 yetkilendirme sunucusu.
- Kullanıcı hesaplarını yönetmek için bir Web API uç noktası
- Kullanıcı hesaplarını depolamak için bir EF modeli.

Bu özellikleri uygulamak ana uygulama sınıfları şunlardır:

- `AccountController`. Kullanıcı hesaplarını yönetmek için bir Web API uç noktası sağlar. `Register` Biz Bu öğreticide kullanılan tek bir eylemdir. Sınıfında diğer yöntemleri, parola sıfırlama, sosyal oturum açma bilgileri ve diğer işlevleri destekler.
- `ApplicationUser`, /Models/IdentityModels.cs tanımlanmış. Bu sınıf, üyelik veritabanındaki kullanıcı hesaplarının EF modelidir.
- `ApplicationUserManager`, /App içinde tanımlanan\_bu sınıfın türeyen Start/IdentityConfig.cs [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) ve kullanıcı hesaplarını, parolaları ve benzeri, doğrulama, yeni bir kullanıcı oluşturma gibi işlemleri gerçekleştirir ve otomatik olarak devam ettirir veritabanında yapılan değişiklikler.
- `ApplicationOAuthProvider`. Bu nesne OWIN ara yazılımı takılan ve ara yazılımı tarafından başlatılan olayları işler. Öğesinden türetilen [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Yetkilendirme sunucusu yapılandırma

StartupAuth.cs içinde aşağıdaki kodu OAuth2 yetkilendirme sunucusu yapılandırır.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath` Yetkilendirme sunucusu uç noktası için URL yolu bir özelliktir. URL olan taşıyıcı belirteçlerini almak için bu uygulama kullanır.

`Provider` Özelliği takılan OWIN ara yazılımı ve ara yazılım tarafından başlatılan olayları işleyen bir sağlayıcı belirtir.

Uygulama bir belirteç almak istediğinde temel akışı şöyledir:

1. Bir erişim belirteci almak için uygulama için bir istek gönderir ~ / Token.
2. OAuth ara yazılım çağrıları `GrantResourceOwnerCredentials` sağlayıcısı.
3. Sağlayıcı çağrıları `ApplicationUserManager` kimlik bilgilerini doğrulamak ve talep kimliği oluşturmak için.
4. Bu başarılı olursa, sağlayıcı belirteci üretmek için kullanılan kimlik doğrulama biletini oluşturur.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

OAuth Ara kullanıcı hesapları hakkında herhangi bir şey bilmiyor. Sağlayıcı, ASP.NET Identity ve ara yazılımı arasında iletişim kurar. Yetkilendirme sunucusu uygulama hakkında daha fazla bilgi için bkz: [OWIN OAuth 2.0 yetkilendirme sunucusu](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Web API taşıyıcı belirteçlerini kullanmak üzere yapılandırma

İçinde `WebApiConfig.Register` Web API ardışık düzeni için kimlik doğrulama yöntemi, aşağıdaki kodu ayarlar:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter** sınıfı, taşıyıcı belirteçlerini kullanarak kimlik doğrulaması sağlar.

**SuppressDefaultHostAuthentication** yöntemi istek Web API ardışık düzendeki, IIS veya OWIN ara yazılımı tarafından erişmeden önce oluşan herhangi bir kimlik doğrulaması yok saymak için Web API söyler. Böylece, biz yalnızca taşıyıcı belirteçlerini kullanarak kimlik doğrulaması için Web API kısıtlayabilirsiniz.

> [!NOTE]
> Özellikle, uygulamanızı MVC kısmı kimlik bilgileri bir tanımlama bilgisinde saklar form kimlik doğrulaması kullanabilirsiniz. Tanımlama bilgisi tabanlı kimlik doğrulaması CSRF saldırılarını önlemek için sahteciliğe karşı koruma belirteçlerini kullanılmasını gerektirir. Sahteciliğe karşı koruma belirteci istemciye göndermek için kolay bir yolu web API'si olduğundan, web API ' ları için bir sorun değildir. (Bu sorunla ilgili daha fazla arka plan için bkz: [Web API saldırılarını önleme CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).) Çağırma **SuppressDefaultHostAuthentication** Web API CSRF saldırılarına karşı savunmasız tanımlama bilgilerinde depolanan kimlik bilgileri gelen emin olunmasını sağlar.


İstemci korunan bir kaynağa istediğinde, Web API ardışık düzeninde olanlar şöyledir:

1. **HostAuthentication** filtre belirteci doğrulamak için OAuth Ara çağırır.
2. Ara yazılım belirteç talep kimliği dönüştürür.
3. Bu noktada, isteğidir *kimliği doğrulanmış* ama *yetkili*.
4. Yetkilendirme filtresini talep kimliğini inceler. Bu kaynak için kullanıcı talepleri yetkilendirmek, istek yetkili. Varsayılan olarak, **[Authorize]** özniteliği kimlik doğrulaması herhangi bir istek yetki. Ancak, diğer talepleri veya rol tarafından yetki verebilir. Daha fazla bilgi için bkz: [kimlik doğrulama ve yetkilendirme Web API](authentication-and-authorization-in-aspnet-web-api.md).
5. Önceki adımları başarılı olursa, denetleyici korumalı kaynağa döndürür. Aksi durumda, istemci bir 401 (yetkisiz) hatası alır.

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET kimliği](../../../identity/index.md)
- [Güvenlik özellikleri SPA şablonunda VS2013 RC için anlama](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). MSDN blog Sun Hongye tarafından gönderin.
- [Web API'si tek dissecting hesapları şablon – bölüm 2: Yerel hesaplar](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Blog gönderisi Dominick Baier tarafından.
- [Ana bilgisayar kimlik doğrulaması ve Web API OWIN ile](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). İyi bir açıklamasını `SuppressDefaultHostAuthentication` ve `HostAuthenticationFilter` Brock Allen tarafından.
- [ASP.NET Identity profil bilgileri VS 2013 şablonlarındaki özelleştirme](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). MSDN blog gönderisi Pranav Rastogi tarafından.
- [İstek ömür yönetimini UserManager ASP.NET Identity sınıfında başına](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). İyi bir açıklaması ile MSDN blog gönderisi Suhas Joshi tarafından `UserManager` sınıfı.
