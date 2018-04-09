---
uid: identity/overview/getting-started/aspnet-identity-recommended-resources
title: ASP.NET Identity önerilen kaynakları | Microsoft Docs
author: Rick-Anderson
description: Bu konu, ASP.NET Identity kullanma hakkında belgeler kaynaklarına bağlantılar sağlar. Harika blog gönderisi, stackoverflow iş parçacığı veya başka bir çizgi biliyorsanız...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2015
ms.topic: article
ms.assetid: 0f78aec2-f509-46fa-b20f-d5208425d8ec
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-recommended-resources
msc.type: authoredcontent
ms.openlocfilehash: f2e1693a32fce6956ddb1e095e6f208b9f0faab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-identity-recommended-resources"></a>ASP.NET Identity kaynaklar önerilir
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu konu, ASP.NET Identity kullanma hakkında belgeler kaynaklarına bağlantılar sağlar.
> 
> Harika bir blog gönderisi, biliyorsanız [stackoverflow](http://stackoverflow.com) iş parçacığı veya yararlı olacak herhangi bir bağlantıyı [bize bir e-posta gönderin](mailto:aspnetue@microsoft.com?subject=Identity recommended resources) bağlantısı ile veya yalnızca bir ileti bu sayfanın sonundaki bırakın.


- [ASP.NET Identity ile Çalışmaya Başlama](#gettingstarted)
- [Yeni özel gerekir okuma makaleler](#feat)
- [ASP.NET Identity Ara](#adv)
- [Videolar](#video)
- [Sorulacak sorular, istek özellikleri, bir hata raporu ve her gece derlemeler](#samp)
- [Kimlik Blog gönderilerine](#blog)
- [ASP.NET kimliği için özel depolama sağlayıcıları](#cust)
- [Ek kimlik kaynakları](#additional)
- [Q &amp; bir (soru/yanıt)](#qand)

<a id="gettingstarted"></a>
## <a name="getting-started-with-aspnet-identity"></a>ASP.NET kimliği ile çalışmaya başlama

- [MVC 5 uygulamayla Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Bu öğreticide ASP.NET MVC 5 uygulamayı yetkilendirme Facebook ve Google OAuth 2 ile yazmak gösterilmektedir. Ayrıca, ek veri kimlik veritabanına eklemek nasıl gösterir.
- [Üyelik, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC uygulaması için bir Azure dağıtmak](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Bu öğretici Azure dağıtım ekler uygulamanızı rolleri ile güvenli hale getirmek nasıl kullanıcılar, roller ve ek güvenlik özellikleri eklemek için üyelik API'si kullanma.
- [ASP.NET Identity’ye Giriş](introduction-to-aspnet-identity.md)
- [Oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)
- [SMS ve e-posta iki öğeli kimlik doğrulaması özellikli ASP.NET MVC 5 uygulaması](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)

<a id="feat"></a>
## <a name="new-featured-must-read-articles"></a>Yeni özel gerekir okuma makaleler

- [İzlenecek yol: ASP.NET MVC kimliği Microsoft hesabı kimlik doğrulaması ile](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/) tarafından [Benjamin günü](http://www.benday.com/about/)
- [ASP.NET Identity 2.0 genişletme kimlik modelleri ve dizeler yerine tamsayı anahtarlarını kullanarak](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
- [AngularJS belirteci ASP.NET Web API 2, Owın ve kimlik kullanarak kimlik doğrulaması](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [WSAT için yenileme olarak Thinktecture.IdentityManager](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [ASP.NET Identity 2.0: Kullanıcıları ve rolleri özelleştirme](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)

<a id="adv"></a>
## <a name="intermediate-aspnet-identity"></a>ASP.NET Identity Ara

- [Hesap onaylamayı ve parola kurtarma ASP.NET kimliğe sahip](../features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Identity ile SMS ve e-posta kullanılan iki öğeli kimlik doğrulaması](../features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Mevcut Bir Web Sitesini SQL Üyeliğinden ASP.NET Identity’ye Geçirme](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Boş veya Mevcut Bir Web Forms Projesine ASP.NET Identity Ekleme](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)
- MSDN dergisi [ASP.NET Identity ile dış kimlik doğrulama](https://msdn.microsoft.com/magazine/dn745860.aspx) Dino Esposito tarafından
- MSDN dergisi[ASP.NET Identity ilk göz](https://msdn.microsoft.com/magazine/dn605872.aspx) Dino Esposito tarafından
- [ASP.NET Identity – kullanıcı kilitleme](http://tech.trailmax.info/2014/06/asp-net-identity-user-lockout/)

<a id="samp"></a>
## <a name="where-to-ask-questions-request-features-report-a-bug-and-nightly-builds"></a>Sorulacak sorular, istek özellikleri, bir hata raporu ve her gece derlemeler

- Etiket StackOverflow için kullanmak [aspnet kimliği](http://stackoverflow.com/questions/tagged/asp.net-identity)
- ASP.NET forumları için deftere [güvenlik Forumu](https://forums.asp.net/25.aspx) ve ekleme **ASP.NET Identity** başlığı.
- [ASP.NET Identity github'da](https://github.com/aspnet/AspNetIdentity) hatalar Get gecelik derlemeleri, istek özelliklerini açın.

<a id="blog"></a>
## <a name="blog-posts-on-identity"></a>Kimlik Blog gönderilerine

- [ASP.NET Identity içinde SecurityStamp nedir?](http://stackoverflow.com/questions/19487322/what-is-asp-net-identitys-iusersecuritystampstoretuser-interface/19505060#19505060)
- Tarafından [John Atten](https://twitter.com/xivSolutions)

    - [ASP.NET Identity 2.0 genişletme kimlik modelleri ve dizeler yerine tamsayı anahtarlarını kullanarak](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
    - [ASP.NET Identity 2.0: Kullanıcıları ve rolleri özelleştirme](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)
    - [ASP.NET MVC ve kimlik 2.0: temel kavramları anlama](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)
    - [Hesap doğrulama ve iki öğeli yetkilendirme ayarlama](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)
    - [DB bağlantısı ve ASP.NET MVC 5 kimlik hesapları için kod ilk geçiş ve Visual Studio 2013 yapılandırma](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)
- Tarafından [Taiseer Joudeh](http://bitoftech.net/taiseer-joudeh-blog/)

    - [Belirteç tabanlı ASP.NET Web API 2, Owın ara yazılımı ve ASP.NET Identity kullanarak kimlik doğrulaması](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/)
    - [AngularJS belirteci ASP.NET Web API 2, Owın ve kimlik kullanarak kimlik doğrulaması](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
    - [OAuth yenileme belirteçleri AngularJS ASP .NET Web API 2 ve Owın – bölümü 3 kullanarak uygulama etkinleştirin.](http://bitoftech.net/2014/07/16/enable-oauth-refresh-tokens-angularjs-app-using-asp-net-web-api-2-owin/)
- Tarafından [Anders Abel](https://twitter.com/anders_abel)

    - [Owın Dış kimlik doğrulaması işlem hattı anlama](http://coding.abel.nu/2014/06/understanding-the-owin-external-authentication-pipeline/)
    - [ASP.NET kimliği ve Owın genel bakış](http://coding.abel.nu/2014/06/asp-net-identity-and-owin-overview/)

  Tarafından [K. Scott Allen](https://twitter.com/OdeToCode) kodu için kod hakkında

    - [ASP.NET Core kimliği](http://odetocode.com/blogs/scott/archive/2013/11/25/asp-net-core-identity.aspx) Iuser, Iuserstore ve t dahil olmak üzere çekirdek soyutlamalar bu blog inceler\*depolamak arabirimleri.
    - [ASP.NET Identity Entity Framework](http://odetocode.com/blogs/scott/archive/2014/01/03/asp-net-identity-with-the-entity-framework.aspx) MVC 5, Web API ve SPA uygulamaları, bağlantı dizeleri ve yönetme bağlamları bireysel kullanıcı hesapları
    - [ASP.NET kimliğe sahip özelleştirme seçenekleri](http://odetocode.com/blogs/scott/archive/2014/01/09/customization-options-with-asp-net-identity.aspx)
    - [ASP.NET Identity uygulama](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- [Benjamin gün](http://www.benday.com/about/)[izlenecek yol: bir Microsoft hesabı kimlik doğrulaması ile ASP.NET MVC kimliği](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)
- [Brock Allen](https://twitter.com/BrockLAllen)

    - [OWIN/Katana kimlik doğrulaması ara yazılımı ile dış oturum açma sağlayıcılarını (sosyal oturum açma bilgileri) öncü](http://brockallen.com/2014/01/09/a-primer-on-external-login-providers-social-logins-with-owinkatana-authentication-middleware/)
    - [IdentityReboot Tanıtımı](http://brockallen.com/2014/02/11/introducing-identityreboot/): t şikayet hakkında önemli eksik özellikleri uygulayan ASP.NET Identity uzantıları kümesi.
- [Pranav Rastogi](https://twitter.com/rustd)

    - [Daha fazla bilgi sağlayıcıları gelen sosyal Al](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [@beabigrockstar](https://twitter.com/beabigrockstar) (Jerrie Pelser)

    - [2 faktörlü kimlik doğrulaması](http://www.beabigrockstar.com/blog/2-factor-authentication-with-asp-net-identity-2-0-beta-1/)
    - [Google Authenticator ile ASP.NET Identity kullanma](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)
    - [ASP.NET MVC 5 kimlik doğrulama kılavuzları](http://www.beabigrockstar.com/)
- [VS 2013 proje şablonlarını kullanılan sosyal sağlayıcılardan daha fazla bilgi edinin](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [ASP.NET kimliği ile basit bir Yapılacaklar uygulamasının oluşturma ve kullanıcıları ToDoes ile ilişkilendirme](https://blogs.msdn.com/b/webdev/archive/2013/10/20/building-a-simple-todo-application-with-asp-net-identity-and-associating-users-with-todoes.aspx)
- [Google Openıd tümleştirme sorunları ASP.NET Identity](http://blog.technovert.com/2014/01/google-openid-integration-issues-asp-net-identity/) hatayı alırsanız: HTTP Hata 404.15 – istek filtreleme modülü bulunamadı, burada sorgu dizesi çok uzun bir isteği reddedecek şekilde yapılandırıldı
- [WSAT için yenileme olarak Thinktecture.IdentityManager](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [AngularJS belirteci ASP.NET Web API 2, Owın ve kimlik kullanarak kimlik doğrulaması](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Basit Asp.net Identity Core Entity Framework olmadan](https://code.msdn.microsoft.com/Simple-Aspnet-Identiy-Core-7475a961)
- [ASP.NET MVC kimliğini rolleriyle çalışma](http://www.dotnetfunda.com/articles/show/2898/working-with-roles-in-aspnet-identity-for-mvc) tarafından [Sheo Narayan](http://www.dotnetfunda.com/profile/sheonarayan.aspx)
- [ASP.NET üyelik için ASP.NET Identity taşıma](http://webdojo.sharepoint.com/ajmatthews/_layouts/15/start.aspx#/Lists/Posts/Post.aspx?ID=2) Alistair Matthews tarafından

<a id="video"></a>
## <a name="videos"></a>Videolar

- Kanal 9 [ASP.NET uygulamaları ve Hizmetleri güvenli hale getirme: Modern uygulamalar için güvenlik Facelift](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B421#fbid=PhVT9E1WRtr?hashlink=fbid) Ido Flatow tarafından
- Kanal 9 [ASP.NET Identity giriş](https://channel9.msdn.com/Events/dotnetConf/2014/ASP-NET-Identity-Security) Pranav Rastogi tarafından
- Kanal 9 [ASP.NET ASP.NET kimliğini kullanarak kimlik doğrulaması](https://channel9.msdn.com/Shows/Web+Camps+TV/Special-Movember-Episode-ASPNET-Authentication-Provider) Cory Fowler tarafından
- Kanal 9 [Modern Web uygulamaları oluşturma: ASP.NET Identity](https://channel9.msdn.com/Series/Building-Modern-Web-Apps/03) Jeff Koch tarafından
- Kanal 9 [ASP.NET kimliği ile Web sitenizin güvenliğini](https://channel9.msdn.com/Events/TechDays/Techdays-2014-the-Netherlands/Securing-your-website-with-ASP-NET-Identity) Alex Thissen tarafından
- [ASP.NET kimliği mevcut bir DB-Model kullanmak](https://www.youtube.com/watch?v=elfqejow5hM) Alexander Etikan tarafından
- [Bir ASP.NET Identity](https://www.youtube.com/watch?v=w8GD-QIusKk) Ivaylo Kenov Telerik biri tarafından
- [Çekçe ASP.NET Identity](https://www.youtube.com/watch?v=tVbZp5brcpY) bu ders temel kimlik doğrulaması dağıtma, Twitter veya FaceBook'ta gibi dış kimlik sağlayıcıları için destek ekleme ve tek kullanımlık parola (OTP) kullanmayı göstereceğiz. [ASP.NET Identity je nástupce üyelik rol sağlayıcısı&#367; v ASP.NET, tedy knihovna pro zajišt&#283;ní autentizace uživatel&#367;. V této p&#345;ednášce si ukážeme jak nasad]

<a id="cust"></a>
## <a name="custom-storage-providers-for-aspnet-identity"></a>ASP.NET kimliği için özel depolama sağlayıcıları

Kendi sağlayıcınızı yazmak istiyorsanız, okuma [genel bakış, özel depolama sağlayıcıları için ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) ve [uygulama ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx) ve listelenen OSS projelerden biri kaynağı inceleyin Aşağıda.

- Öğretici: [ASP.NET Identity için özel depolama sağlayıcıları genel bakış](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) zel FitzMacken tarafından
- Blog: [ASP.NET Identity uygulama](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Öğretici:[temel kimlik hesapları ayarlama ve bunları dış bir DB işaret](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Tarafından [ @xivSolutions ](https://twitter.com/xivSolutions).
- Öğretici[: özel MySQL ASP.NET Identity depolama sağlayıcıyı uygulama](../extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent varlıklar](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) tarafından [SoftFluent](http://www.softfluent.com/)
- [Azure Table Storage](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) Ahmet CAN tarafından.
- Azure Table Storage: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) tarafından [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant Daniel Wertheim tarafından.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Esnek arama[y: esnek kimlik](https://github.com/bmbsqd/elastic-identity) Bombsquad AB. tarafından
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) Jonathan Sheely Jonathan Sheely tarafından.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) by Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) tarafından [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) by [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Kod için "ilk veritabanı" kullanıcı deposunu T4 EF oluşturmak için şablonları: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)

<a id="additional"></a>
## <a name="additional-aspnet-identity-resources"></a>Ek ASP.NET Identity kaynaklar

- [OWIN için Yahoo ve LinkedIn OAuth güvenlik sağlayıcıları Tanıtımı](http://blog.beabigrockstar.com/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) Jerrie Pelser Yahoo ve LinkedIn yönergeler tarafından.

<a id="qand"></a>
## <a name="qampa-questionanswer"></a>Q&amp;bir (soru/yanıt)

- S: kilitli (bunlar üzerinde o bilgisayar/tarayıcı 2FA gidin zorunda kalmamak için) "Beni anımsa" seçeneğinin etkin kullanıcılar çıkışı değil kilitlendi. Neden ve nasıl ı engelleyen? Yanıt [burada](http://stackoverflow.com/questions/24312247/locked-out-users-can-login-if-they-have-auth-cookie).
- **Q**: nasıl ı saklayabilir kullanıcının gerçek adı gibi özel talepler her istekte gereksiz veritabanı sorguları önlemek için ASP.NET Identity tanımlama bilgisi. Yanıt [burada](http://stackoverflow.com/questions/23622047/identity-cookie-loses-custom-claim-information-after-a-period-of-time).
- **S: güncelleştirme AspNetUser parola karması**: 2 projeleri sahip. ASP.NET kimlik doğrulaması bunlardan birini kullanırken, diğer yönetim yan olan Windows kimlik doğrulaması kullanır. Diğer kullanıcıları yönetebilmek için yönetici projeyi istiyorum. Parola dışında her şeyi değiştirebilirsiniz. [Burada yanıt](http://stackoverflow.com/questions/23880666/updating-aspnetuser-password-hash).
- **Q**: miyim Sıfırla nasıl parola diğer kullanıcılar için bir yönetici olarak? Yanıt [burada](http://stackoverflow.com/questions/23783249/identity-2-0-reset-password-by-admin/24211766#24211766).
- **Q**: ASP.NET MVC IdentityUser kullanıcı adı alanında görüntülenen adını değiştirebilir miyim? Yanıt [burada](http://stackoverflow.com/questions/23256650/can-i-change-the-displayed-name-of-the-username-field-in-asp-net-mvc-identityuse).
- **Q**: nasıl yapabilirim belirli rollere diğer kullanıcılar için gran kullanıcılara izinler? Yanıt [burada](http://stackoverflow.com/questions/23695373/allow-users-to-grant-permissions-to-other-users-for-their-account-in-asp-net-ide).
- **Q**: AspNetUserClaims tablo karşılaştırması AspNetUsers tablodaki profil bilgilerini depolamak. Yanıt [burada](http://stackoverflow.com/questions/23215727/is-there-any-benefit-to-storing-user-information-in-aspnetuserclaims-with-asp-ne).
- **Q**: bana bir dış kimlik doğrulama sağlayıcısı kullanırken unutmayın. Yanıt [burada](http://stackoverflow.com/questions/23180896/how-to-remember-the-login-in-mvc5-when-an-external-provider-is-used).
- **Q**: neden her istek gerektiriyor bir ApplicationDBContext değil, çok fazla ek yükü?. Yanıt Hayır, ek yükü düşüktür.
- S: oturum açan kullanıcıların listesini nasıl sağlarım? Yanıt [burada](http://stackoverflow.com/questions/22995653/getting-a-list-of-logged-in-users-in-asp-net-identity/).
- S: nasıl kullanıcı Microsoft.ASPNET.Identity oturum açtığında ı algılayabilir mi? Yanıt [burada](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- S: yerelleştirilmiş hata iletileri için kimlik nasıl sağlarım? Yanıt [burada](http://stackoverflow.com/questions/22835981/asp-net-identity-localization-publickeytoken/22845864#22845864).
- S: yeni talep 30 dakikada almak için CookieMiddleware nasıl yapılandırırım? Yanıt [burada](http://stackoverflow.com/questions/22682663/how-to-hold-the-cookies-claims-updated-with-mcv5-owin/22796932#22796932).
- S: bunlar oturum açtıktan sonra kullanıcı için talepleri nasıl değiştirileceği? Yanıt [burada](http://stackoverflow.com/questions/22768836/can-i-modify-claims-in-asp-net-identity-with-owin-after-calling-signin/22769963#22769963).
- S: güvenlik belirteçleri nasıl geçersiz? Yanıt [burada](http://stackoverflow.com/questions/22755700/revoke-token-generated-by-usertokenprovider-in-asp-net-identity-2-0/22767286#22767286).
- S: nasıl deposu talepleri içindeyse tanımlama bilgisi Ara? Yanıt [burada](http://stackoverflow.com/questions/22320632/storing-retrieving-user-data-without-database-when-using-owin-cookie-authenticat/22541856#22541856).
- S: bir PIN veya MVC Uygulamam her eylem yönteminde kontrol güvenlik istiyorum ancak her istek için eylem yönteminin PIN girmesini aktarıp kullanıcılar başarı depolamak istediğiniz. Yanıt [burada](http://stackoverflow.com/questions/22479958/security-check-an-user-to-a-access-controller-action/22486075#22486075).
- İçin bir sosyal sağlayıcıdan döndürülen e-posta adre DB'de kaydetme nasıl, yaparım gibi s: musunuz? Yanıt [burada](http://stackoverflow.com/questions/22888397/save-claims-to-db-on-login/22970969#22970969):
- S: nasıl bir kullanıcı her iki ile/ile-çıkar "Beni anımsa" bir tanımlama bilgisi açtığında ı algılayabilir mi? Yanıt [burada](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- S: ASP.NET Identity OWIN ile Taleplerde Signın çağrıldıktan sonra değiştirebilir mi? Yanıt: Arama Signın tam olarak kullanıcı için talepleri değiştirmek istediğinizde yapmanız gereken olan ' dir. Temel olarak sonraki isteklerde görünmesini yeni talep gördüğünüz neden olan tanımlama bilgisi, içine serileştirilmesi Claimsıdentity neden olur.
