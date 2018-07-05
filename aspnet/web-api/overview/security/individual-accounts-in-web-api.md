---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Bireysel hesaplar ve ASP.NET Web API 2.2 sürümünde yerel oturum açma ile bir Web API'SİNİN güvenliğini sağlama | Microsoft Docs
author: MikeWasson
description: Bu konu, bir üyelik veritabanında kimlik doğrulaması yapmak için oauth2'yi kullanarak bir web API güvenliğini sağlama işlemini gösterir. Bir öğretici Visual Studio 201 kullanılan yazılım sürümleri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: f37cb206b78fd46835bb59bede5920f8e5acd3fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399944"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Bireysel hesaplar ve ASP.NET Web API 2.2 sürümünde yerel oturum açma ile bir Web API'SİNİN güvenliğini sağlama
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Örnek uygulamasını indirin](https://github.com/MikeWasson/LocalAccountsApp)

> Bu konu, bir üyelik veritabanında kimlik doğrulaması yapmak için oauth2'yi kullanarak bir web API güvenliğini sağlama işlemini gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013 Güncelleştirme 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


Visual Studio 2013'te Web API proje şablonunu, kimlik doğrulaması için üç seçeneği sunar:

- **Bireysel hesaplar.** Uygulamayı bir üyelik veritabanı kullanır.
- **Kurumsal hesaplar.** Kullanıcılar kendi Azure Active Directory, Office 365 veya şirket içi Active Directory kimlik bilgilerinizle oturum açın.
- **Windows kimlik doğrulaması.** Bu seçenek, Intranet uygulamaları için tasarlanmıştır ve Windows kimlik doğrulaması IIS Modülü kullanır.

Bu seçenekler hakkında daha fazla ayrıntı için bkz. [Visual Studio 2013'te ASP.NET Web projeleri oluşturma](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Bireysel hesaplar oturum açmak bir kullanıcı için iki yol sağlar:

- **Yerel oturum açma**. Kullanıcı adı ve parola girme sitede kaydeder. Uygulama, parola karması üyelik veritabanında depolar. Kullanıcı oturum açtığında, ASP.NET kimlik sistemi parolayı doğrular.
- **Sosyal oturum açma**. Kullanıcı, Facebook, Microsoft veya Google gibi bir dış hizmet oturum açar. Uygulamayı hala üyelik veritabanında kullanıcı için bir giriş oluşturur, ancak herhangi bir kimlik bilgisi depolamaz. Dış hizmetine oturum açarak kullanıcının kimliğini doğrular.

Bu makalede yerel oturum açma senaryo ele alınmaktadır. Hem yerel hem de sosyal oturum açma isteklerinin kimliğini doğrulamak için OAuth2 Web API'sini kullanır. Ancak, yerel ve sosyal oturum açma için kimlik bilgisi akışlar farklıdır.

Bu makalede, kullanıcının oturum açın ve bir Web API'sine kimliği doğrulanmış AJAX çağrıları Gönder sağlayan basit bir uygulama miyim kazandırabileceğinizi göstereceğiz. Örnek kod indirebileceğiniz [burada](https://github.com/MikeWasson/LocalAccountsApp). Benioku dosyasını Visual Studio'da sıfırdan örnek oluşturmayı açıklar.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Örnek uygulama, veri bağlama ve AJAX isteklerini göndermek için jQuery Knockout.js kullanır. Bu makalede Knockout.js bilmek zorunda kalmazsınız miyim AJAX çağrıları odaklanarak.

Bu doğrultuda, ı açıklayacağız:

- Hangi uygulama istemci tarafında yapıyor.
- Sunucuda olanlar.
- HTTP trafiğini ortada.

İlk olarak biz bazı OAuth2 terminolojisi tanımlamanız gerekir.

- *Kaynak*. Korunabilir veriler parçası.
- *Kaynak sunucuda*. Bir kaynak barındıran sunucu.
- *Kaynak sahibi*. Varlık bir kaynağa erişim izni verebilirsiniz. (Genellikle kullanıcı.)
- *İstemci*: kaynağa erişim isteyen uygulama. Bu makalede, istemci, bir web tarayıcısıdır.
- *Erişim belirteci*. Bir kaynağa erişim izni veren bir belirteç.
- *Taşıyıcı belirteç*. Belirli bir erişim belirteciyle herkes belirteci kullanabilir özellik türü. Diğer bir deyişle, bir şifreleme anahtarı veya diğer gizli bir taşıyıcı belirteç kullanmak için bir istemci gerektirmez. Bu nedenle, taşıyıcı belirteçleri yalnızca HTTPS üzerinden kullanılmalıdır ve görece kısa bir zaman aşımı süresinin sahip olmalıdır.
- *Yetkilendirme sunucusu*. Erişim belirteçleri sunan bir sunucu.

Bir uygulama, yetkilendirme sunucusu ve kaynak sunucusu hareket eder. Web API proje şablonunu bu deseni izler.

## <a name="local-login-credential-flow"></a>Yerel oturum açma kimlik bilgileri akışı

Web API'si yerel oturum açma için kullanılan [kaynak sahibi parola akışı](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) OAuth2 ' tanımlanan.

1. Kullanıcı adı ve parola istemciyi girer.
2. İstemci, yetkilendirme sunucusu için bu kimlik bilgilerini gönderir.
3. Yetkilendirme sunucusu kimlik bilgilerini doğrular ve bir erişim belirteci döndürür.
4. Korunan bir kaynağa erişmek için istemci HTTP isteğinin yetkilendirme üst bilgisinde erişim belirteci içerir.

![](individual-accounts-in-web-api/_static/image3.png)

Seçtiğinizde, **bireysel hesaplar** Web API proje şablonu, proje kullanıcı kimlik bilgilerini doğrular ve belirteçleri yetkilendirme sunucusu içerir. Web API'si bileşenlerinin açısından aynı kimlik bilgileri akışı aşağıdaki diyagramda gösterilmektedir.

![](individual-accounts-in-web-api/_static/image4.png)

Bu senaryoda, Web APİ'si denetleyicilerinin kaynak sunucuları gibi davranır. Bir kimlik doğrulaması filtresini erişim belirteçlerini doğrular ve **[Authorize]** özniteliği bir kaynağı korumak için kullanılır. Bir denetleyici veya eylem olduğunda **[Authorize]** özniteliği, tüm istekleri söz konusu denetleyici veya eylem kimlik doğrulaması gerekir. Aksi takdirde, yetkilendirme reddedildi ve Web API 401 (yetkisiz) hatası döndürür.

Yetkilendirme sunucusu ve her ikisi de çağırabilir kimlik doğrulaması filtresini bir [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) OAuth2 ayrıntılarını işler bileşeni. Bu öğreticinin ilerleyen bölümlerinde daha ayrıntılı tasarım açıklayan.

## <a name="sending-an-unauthorized-request"></a>Yetkisiz bir isteği gönderiliyor

Başlamak için uygulamayı çalıştırın ve **API çağrısı** düğmesi. İsteği tamamlandığında, bir hata iletisi görürsünüz **sonucu** kutusu. İstek yetkisiz isteği, bu nedenle, bir erişim belirteci içermediğinden olmasıdır.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

**API çağrısı** düğmesi gönderir bir AJAX isteği ~/api/değerleri, bir Web API denetleyici eylemi çağırır. AJAX isteği gönderen JavaScript kod bölümünün aşağıda verilmiştir. Örnek uygulamada, tüm JavaScript uygulama kodunun bulunduğu Scripts\app.js dosyasında.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Kullanıcı oturum açtığında kadar taşıyıcı belirteç yok ve bu nedenle yetkilendirme üst bilgi isteği yok. Bu istek 401 hatası döndürülecek neden olur.

HTTP isteği aşağıda verilmiştir. (Kullandım [Fiddler](http://www.telerik.com/fiddler) HTTP trafiği yakalamak için.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP yanıtı:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Yanıt ayarlamak için taşıyıcı kimlik ile bir Www-Authenticate üstbilgisi içerdiğine dikkat edin. Bu sunucunun bir taşıyıcı belirteç bekliyor gösterir.

## <a name="register-a-user"></a>Bir kullanıcı kaydı

İçinde **kaydetme** bölümü, uygulama bir e-posta parolanızı girin ve tıklatın **kaydetme** düğmesi.

Bu örnek için geçerli bir eposta adresi kullanmanız gerekmez, ancak gerçek bir uygulamada adresini doğrulamak. (Bkz [oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Parola için "Parola1!" gibi bir büyük harf, küçük harf, sayı ve alfasayısal olmayan karakter kullanın. İstemci tarafı doğrulama, anlayabilirim uygulama basit tutmak için böylece parola biçimi ile ilgili bir sorun varsa, 400 (Hatalı istek) hatası alırsınız.

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

**Kaydetme** düğmesi için ~/api/Account/Register bir POST isteği gönderir /. İstek gövdesi adını ve parolayı tutan bir JSON nesnesidir. İsteği gönderen JavaScript kod aşağıdaki gibidir:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP isteği:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP yanıtı:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Bu istek tarafından işlenen `AccountController` sınıfı. Dahili olarak `AccountController` üyelik veritabanını yönetmek için ASP.NET kimliğini kullanır.

Uygulamayı Visual Studio'dan yerel olarak çalıştırırsanız, kullanıcı hesaplarını AspNetUsers tabloda LocalDB içinde depolanır. Visual Studio'da tabloları görüntülemek için tıklayın **görünümü** menüsünde **Sunucu Gezgini**, ardından **veri bağlantıları**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Bir erişim belirteci alma

Şu ana kadar herhangi bir OAuth tamamlamadıysanız, ancak biz bir erişim belirteci istediğinde artık, uygulamada OAuth yetkilendirme sunucusu görüyoruz. İçinde **oturum** alan örnek uygulamanın e-posta ve parola girin ve tıklatın **oturum**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

**Oturum** düğmesi belirteç uç noktasına bir istek gönderir. İstek gövdesi aşağıdaki formu url kodlanmış verileri içerir:

- verme\_türü: "password"
- Kullanıcı adı: &lt;kullanıcının e-posta&gt;
- Parola: &lt;parola&gt;

AJAX isteği gönderen JavaScript kod aşağıdaki gibidir:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

İstek başarılı olursa yetkilendirme sunucusu bir erişim belirteci yanıt gövdesinde döndürür. Biz oturumu depolama, API için istekleri gönderirken daha sonra kullanmak üzere belirteci depolamak dikkat edin. Bazı biçimlerde kimlik doğrulaması (örneğin, kimlik doğrulama tanımlama bilgisi tabanlı), tarayıcı otomatik olarak erişim belirteci sonraki istekler dahil edilmez. Uygulamayı açıkça bunu yapmalısınız. Olan çok iyi bir şey, sınırladığından [CSRF güvenlik açıklarını](preventing-cross-site-request-forgery-csrf-attacks.md).

HTTP isteği:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

İstek kullanıcının kimlik bilgilerini içerdiğini görebilirsiniz. *Gerekir* Aktarım Katmanı Güvenliği sağlamak için HTTPS kullanın.

HTTP yanıtı:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Okunabilirlik için miyim JSON girintili ve oldukça uzun bir erişim belirteci kesildi.

`access_token`, `token_type`, Ve `expires_in` özellikleri, OAuth2 belirtimi tarafından tanımlanır. Diğer özellikleri (`userName`, `.issued`, ve `.expires`) yalnızca bilgilendirici amaçlarla şunlardır. Bu ek özellikleri ekleyen kodu bulabilirsiniz `TokenEndpoint` /Providers/ApplicationOAuthProvider.cs dosyasındaki yöntemi.

## <a name="send-an-authenticated-request"></a>Kimliği doğrulanmış bir isteği gönder

Taşıyıcı belirteç sahibiz, API'sine kimliği doğrulanmış bir isteği yapabiliyoruz. Bu, istekte yetkilendirme üst bilgisi ayarlayarak yapılır. Tıklayın **API çağrısı** düğmesine tekrar bakın.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP isteği:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP yanıtı:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Oturumu Kapat

Tarayıcı kimlik bilgileri veya erişim belirteci önbelleğe almaması nedeniyle oturumu kapatmak, "belirteci oturum depolamadan kaldırarak unutarak" sorunudur:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Bireysel hesaplar proje şablonu anlama

Seçtiğinizde, **bireysel hesaplar** ASP.NET Web uygulaması proje şablonu, projeyi içerir:

- OAuth2 yetkilendirme sunucusu.
- Kullanıcı hesaplarını yönetmek için bir Web API uç noktası
- Kullanıcı hesaplarını depolamak için EF modeli.

Bu özellikleri uygulayan ana uygulama sınıfları şunlardır:

- `AccountController`. Kullanıcı hesaplarını yönetmek için bir Web API uç noktası sağlar. `Register` Bu öğreticide kullanılan tek bir eylemdir. Parola sıfırlama, sosyal oturum açma bilgileri ve diğer işlevler sınıfında diğer yöntemleri destekler.
- `ApplicationUser`, /Models/IdentityModels.cs içinde tanımlanır. Bu sınıf, üyelik veritabanındaki kullanıcı hesaplarının EF modelidir.
- `ApplicationUserManager`, /App içinde tanımlanan\_Bu sınıf türetilir Start/IdentityConfig.cs [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) ve kullanıcı hesapları, parolaları ve benzeri, doğrulama, yeni bir kullanıcı oluşturma gibi işlemler yapar ve otomatik olarak sürdürür veritabanında yapılan değişiklikler.
- `ApplicationOAuthProvider`. Bu nesne OWIN ara yazılımı yararlanmanıza imkan sağlar ve ara yazılımı tarafından başlatılan olayları işler. Öğesinden türetilen [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Yetkilendirme sunucusu yapılandırma

StartupAuth.cs aşağıdaki kod, OAuth2 yetkilendirme sunucusu yapılandırır.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

`TokenEndpointPath` Özelliği yetkilendirme sunucusu uç noktası için URL yoludur. URL olan bu uygulama, taşıyıcı belirteçlerini almak için kullanır.

`Provider` OWIN ara yazılımı yararlanmanıza imkan sağlar ve ara yazılım tarafından oluşturulan olayları işleyen bir sağlayıcı özelliği belirtir.

Uygulama bir belirteç almak istediğinde temel akışı şöyledir:

1. Uygulama erişim belirteci almak için bir istek gönderir ~ / Token.
2. OAuth Ara çağrıları `GrantResourceOwnerCredentials` sağlayıcısı.
3. Sağlayıcı çağrıları `ApplicationUserManager` kimlik doğrulamaya ve talep kimliği oluşturmak için.
4. Bu başarılı olursa sağlayıcısı, belirteç oluşturmak için kullanılan bir kimlik doğrulaması bileti oluşturur.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

OAuth ara yazılım, kullanıcı hesapları hakkında hiçbir şey bilmez. Sağlayıcı ara yazılım ve ASP.NET Identity arasında iletişim kurar. Yetkilendirme sunucusu uygulama hakkında daha fazla bilgi için bkz. [OWIN OAuth 2.0 yetkilendirme sunucusu](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Web API'si, taşıyıcı belirteçlerini kullanmak üzere yapılandırma

İçinde `WebApiConfig.Register` Web API ardışık düzeni için kimlik doğrulama yöntemi, aşağıdaki kod ayarlar:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

**HostAuthenticationFilter** sınıfı, taşıyıcı belirteçlerini kullanarak kimlik doğrulaması sağlar.

**SuppressDefaultHostAuthentication** yöntemi Web API'si, Web API ardışık düzeni, IIS veya OWIN ara yazılımı tarafından istek ulaşmadan önce gerçekleşen herhangi bir kimlik doğrulaması yok saymasını söyler. Bu şekilde, biz yalnızca taşıyıcı belirteçlerini kullanarak kimlik doğrulaması yapmak için Web API'si kısıtlayabilirsiniz.

> [!NOTE]
> Özellikle, MVC bölümüne uygulamanızın kimlik bilgileri bir tanımlama bilgisinde saklar form kimlik doğrulaması kullanabilirsiniz. Tanımlama bilgisi tabanlı kimlik doğrulaması, sahteciliğe karşı koruma belirteçleri, CSRF saldırıları önlemek için kullanılmasını gerektirir. Sahteciliğe karşı koruma belirteci istemciye gönderilecek web API'si için uygun hiçbir yolu olmadığından web API'leri için bir sorun olmasıdır. (Bu konu hakkında daha fazla bilgiler için bkz. [CSRF önleme saldırıları Web API'sindeki](preventing-cross-site-request-forgery-csrf-attacks.md).) Çağırma **SuppressDefaultHostAuthentication** Web API CSRF saldırılarına karşı savunmasız tanımlama bilgilerinde depolanan kimlik bilgileri'den değil sağlar.


İstemcinin korumalı bir kaynağın istediğinde, Web API işlem hattında ne şöyledir:

1. **HostAuthentication** filtre belirteci doğrulamak için bir OAuth ara yazılım çağırır.
2. Ara yazılım belirteci talep kimliği dönüştürür.
3. Bu noktada, isteğidir *kimliği doğrulanmış* ama *yetkili*.
4. Yetkilendirme filtresini talep kimliğini inceler. Bu kaynak için kullanıcı talepleri yetkilendirmek, istek yetkili. Varsayılan olarak, **[Authorize]** öznitelik kimliği doğrulanan herhangi bir istek yetki. Ancak, rolü veya diğer talepler yetki verebilir. Daha fazla bilgi için [kimlik doğrulama ve yetkilendirme Web API'sindeki](authentication-and-authorization-in-aspnet-web-api.md).
5. Önceki adımları başarılı olursa, denetleyici korumalı kaynağa döndürür. Aksi takdirde istemci 401 (yetkisiz) hatası alır.

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET kimlik](../../../identity/index.md)
- [VS2013 RC için güvenlik özellikleri SPA şablondaki anlama](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). MSDN blog'u Sun tarafından Hongye gönderin.
- [Web API kişi ayrıntıları hesapları şablon – bölüm 2: Yerel hesaplar](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Blog gönderisi Dominick Baier tarafından.
- [Konak kimlik doğrulaması ve Web API ile OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). İyi bir açıklama `SuppressDefaultHostAuthentication` ve `HostAuthenticationFilter` Brock Allen tarafından.
- [VS 2013 şablonlarındaki ASP.NET ıdentity'de profil bilgilerini özelleştirme](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). MSDN blog gönderisi Pranav rastogi'nin.
- [Ömür yönetimini UserManager ASP.NET Identity sınıfında istek başına](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). İyi bir açıklama ile MSDN blog gönderisine Suhas Joshi tarafından `UserManager` sınıfı.
