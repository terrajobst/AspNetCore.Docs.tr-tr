---
uid: identity/overview/getting-started/aspnet-identity-recommended-resources
title: ASP.NET Identity önerilen kaynaklar | Microsoft Docs
author: Rick-Anderson
description: Bu konu, ASP.NET Identity kullanma hakkında belge kaynakları için bağlantılar sağlar. Harika blogda, stackoverflow iş parçacığı veya başka bir bağlantı biliyorsanız...
ms.author: aspnetcontent
ms.date: 04/09/2015
ms.assetid: 0f78aec2-f509-46fa-b20f-d5208425d8ec
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-recommended-resources
msc.type: authoredcontent
ms.openlocfilehash: 893956f4abcb3f337879ca43c63650d62663aa91
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811921"
---
<a name="aspnet-identity-recommended-resources"></a>ASP.NET Identity önerilen kaynaklar
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu konu, ASP.NET Identity kullanma hakkında belge kaynakları için bağlantılar sağlar.
> 
> Harika bir blog gönderisi, biliyorsanız [stackoverflow](http://stackoverflow.com) iş parçacığı veya kullanışlı olabilecek diğer bir bağlantı [bize bir e-posta gönderin](mailto:aspnetue@microsoft.com?subject=Identity recommended resources) bağlantısını veya yalnızca bir ileti bu sayfanın alt kısmındaki bırakın.


- [ASP.NET Identity ile Çalışmaya Başlama](#gettingstarted)
- [Yeni öne çıkan gerekir okuma makaleler](#feat)
- [ASP.NET Identity Ara](#adv)
- [Videolar](#video)
- [Soru sorun, istek özellikleri, bir hata ve gecelik derlemeler rapor yeri](#samp)
- [Blog gönderilerine kimlik](#blog)
- [ASP.NET kimliği için özel depolama sağlayıcıları](#cust)
- [Ek kimlik kaynakları](#additional)
- [Q &amp; bir (soru/yanıt)](#qand)

<a id="gettingstarted"></a>
## <a name="getting-started-with-aspnet-identity"></a>ASP.NET Identity ile çalışmaya başlama

- [Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma ile MVC 5 uygulaması](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Bu öğreticide bir ASP.NET MVC 5 uygulaması Facebook ve Google OAuth 2 yetkilendirmesiyle yazma işlemi gösterilmektedir. Ayrıca kimlik veritabanı için ek veri ekleme işlemini de gösterir.
- [Membership, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC uygulaması azure'e](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Bu öğreticide Azure dağıtım ekler rolleri ile uygulamanızı güvenli hale getirmeyi kullanıcılar ve roller ve ek güvenlik özellikleri eklemek için üyelik API'si kullanma.
- [ASP.NET Identity’ye Giriş](introduction-to-aspnet-identity.md)
- [Oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)
- [SMS ve e-posta iki öğeli kimlik doğrulaması özellikli ASP.NET MVC 5 uygulaması](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)

<a id="feat"></a>
## <a name="new-featured-must-read-articles"></a>Yeni öne çıkan gerekir okuma makaleler

- [İzlenecek yol: Microsoft hesabı kimlik doğrulaması ile ASP.NET MVC kimlik](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/) tarafından [Benjamin gün](http://www.benday.com/about/)
- [ASP.NET Identity 2.0 genişletme kimlik modelleri ve dize yerine tamsayı anahtarları kullanarak](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
- [ASP.NET Web API 2 ve Owın kimlik belirteci kimlik doğrulaması AngularJS](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Thinktecture.IdentityManager WSAT bir ardılı olarak](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [ASP.NET Identity 2.0: Kullanıcıları ve rolleri özelleştirme](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)

<a id="adv"></a>
## <a name="intermediate-aspnet-identity"></a>ASP.NET Identity Ara

- [Hesap onaylama ve parola kurtarma ASP.NET Identity ile](../features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Identity ile SMS ve e-posta kullanılan iki öğeli kimlik doğrulaması](../features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Mevcut Bir Web Sitesini SQL Üyeliğinden ASP.NET Identity’ye Geçirme](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Boş veya Mevcut Bir Web Forms Projesine ASP.NET Identity Ekleme](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md)
- MSDN Magazine [ASP.NET Identity ile dış kimlik doğrulama](https://msdn.microsoft.com/magazine/dn745860.aspx) tarafından Dino Esposito
- MSDN Magazine[ASP.NET Identity ilk göz](https://msdn.microsoft.com/magazine/dn605872.aspx) tarafından Dino Esposito
- [ASP.NET Identity – kullanıcı kilitleme](http://tech.trailmax.info/2014/06/asp-net-identity-user-lockout/)

<a id="samp"></a>
## <a name="where-to-ask-questions-request-features-report-a-bug-and-nightly-builds"></a>Soru sorun, istek özellikleri, bir hata ve gecelik derlemeler rapor yeri

- StackOverflow için etiket kullanma [aspnet kimliği](http://stackoverflow.com/questions/tagged/asp.net-identity)
- ASP.NET forumları için postalayabilir [güvenlik Forumu](https://forums.asp.net/25.aspx) ve ekleme **ASP.NET Identity** başlığa.
- [GitHub üzerinde ASP.NET Identity](https://github.com/aspnet/AspNetIdentity) hataları gecelik derlemeleri alma, istek özelliklerini açın.

<a id="blog"></a>
## <a name="blog-posts-on-identity"></a>Blog gönderilerine kimlik

- [ASP.NET ıdentity'de bir SecurityStamp nedir?](http://stackoverflow.com/questions/19487322/what-is-asp-net-identitys-iusersecuritystampstoretuser-interface/19505060#19505060)
- Tarafından [John Atten](https://twitter.com/xivSolutions)

    - [ASP.NET Identity 2.0 genişletme kimlik modelleri ve dize yerine tamsayı anahtarları kullanarak](http://typecastexception.com/post/2014/07/13/ASPNET-Identity-20-Extending-Identity-Models-and-Using-Integer-Keys-Instead-of-Strings.aspx)
    - [ASP.NET Identity 2.0: Kullanıcıları ve rolleri özelleştirme](http://typecastexception.com/post/2014/06/22/ASPNET-Identity-20-Customizing-Users-and-Roles.aspx)
    - [ASP.NET MVC ve kimlik 2.0: temellerini anlama](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)
    - [Hesap doğrulama ve iki Faktörlü yetkilendirmeyi ayarlama](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx)
    - [DB bağlantı ve kod öncelikli geçiş için ASP.NET MVC 5 hesaplarında kimlik ve Visual Studio 2013 yapılandırma](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx)
- Tarafından [Taiseer Joudeh](http://bitoftech.net/taiseer-joudeh-blog/)

    - [Belirteç tabanlı ASP.NET Web API 2, Owın ara yazılımı ve ASP.NET Identity kullanılarak kimlik doğrulaması](http://bitoftech.net/2014/06/01/token-based-authentication-asp-net-web-api-2-owin-asp-net-identity/)
    - [ASP.NET Web API 2 ve Owın kimlik belirteci kimlik doğrulaması AngularJS](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
    - [OAuth yenileme belirteçleri AngularJS uygulamasını ASP .NET Web API 2 ve Owın – bölüm 3 kullanarak etkinleştirin.](http://bitoftech.net/2014/07/16/enable-oauth-refresh-tokens-angularjs-app-using-asp-net-web-api-2-owin/)
- Tarafından [Anders Abel](https://twitter.com/anders_abel)

    - [Owın Dış kimlik doğrulama komut zincirini anlama](http://coding.abel.nu/2014/06/understanding-the-owin-external-authentication-pipeline/)
    - [ASP.NET Identity ve Owın genel bakış](http://coding.abel.nu/2014/06/asp-net-identity-and-owin-overview/)

  Tarafından [K. Scott Allen](https://twitter.com/OdeToCode) kodu için kod üzerinde

    - [ASP.NET Core kimliği](http://odetocode.com/blogs/scott/archive/2013/11/25/asp-net-core-identity.aspx) çekirdek soyutlamalar Iuser, Iuserstore ve t dahil olmak üzere, bu blog inceler\*Store arabirimleri.
    - [Entity Framework ile ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/03/asp-net-identity-with-the-entity-framework.aspx) MVC 5, Web API ve SPA uygulamaları ve bağlantı dizeleri yöneten bağlamları içinde bireysel kullanıcı hesapları
    - [ASP.NET Identity ile özelleştirme seçenekleri](http://odetocode.com/blogs/scott/archive/2014/01/09/customization-options-with-asp-net-identity.aspx)
    - [ASP.NET Identity uygulama](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- [Benjamin gün](http://www.benday.com/about/)[izlenecek yol: Microsoft hesabı kimlik doğrulaması ile ASP.NET MVC kimlik](http://www.benday.com/2014/02/25/walkthrough-asp-net-mvc-identity-with-microsoft-account-authentication/)
- [Brock Allen](https://twitter.com/BrockLAllen)

    - [OWIN/Katana kimlik doğrulaması ara yazılımı dış oturum açma sağlayıcılarıyla (sosyal oturum açma bilgileri) hakkında bilgi](http://brockallen.com/2014/01/09/a-primer-on-external-login-providers-social-logins-with-owinkatana-authentication-middleware/)
    - [Karşınızda IdentityReboot](http://brockallen.com/2014/02/11/introducing-identityreboot/): Ben şikayet hakkında önemli eksik özellikleri uygulayan ASP.NET ıdentity'ye uzantıları kümesi.
- [Pranav Rastogi'nin](https://twitter.com/rustd)

    - [Gelen sosyal sağlayıcılar daha fazla bilgi edinin](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [@beabigrockstar](https://twitter.com/beabigrockstar) (Jerrie Pelser)

    - [2 faktörlü kimlik doğrulaması](http://www.beabigrockstar.com/blog/2-factor-authentication-with-asp-net-identity-2-0-beta-1/)
    - [ASP.NET Identity ile Google Authenticator'ı kullanma](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)
    - [ASP.NET MVC 5 kimlik doğrulama kılavuzları](http://www.beabigrockstar.com/)
- [VS 2013 proje şablonlarında kullanılan sosyal sağlayıcılardan daha fazla bilgi edinin](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [ASP.NET Identity ile basit bir ToDo uygulaması oluşturma ve kullanıcılara ToDoes ile ilişkilendirme](https://blogs.msdn.com/b/webdev/archive/2013/10/20/building-a-simple-todo-application-with-asp-net-identity-and-associating-users-with-todoes.aspx)
- [Google Openıd ASP.NET Identity ile tümleştirme sorunları](http://blog.technovert.com/2014/01/google-openid-integration-issues-asp-net-identity/) hatayı alırsanız: HTTP Hata 404.15-istek filtreleme modülü bulunamadı burada sorgu dizesi çok uzun bir isteği reddetmek üzere yapılandırıldı
- [Thinktecture.IdentityManager WSAT bir ardılı olarak](http://www.hanselman.com/blog/ThinktectureIdentityManagerAsAReplacementForTheASPNETWebSiteAdministrationTool.aspx)
- [ASP.NET Web API 2 ve Owın kimlik belirteci kimlik doğrulaması AngularJS](http://bitoftech.net/2014/06/09/angularjs-token-authentication-using-asp-net-web-api-2-owin-asp-net-identity/)
- [Basit bir Asp.net Identity çekirdek olmadan varlık çerçevesi](https://code.msdn.microsoft.com/Simple-Aspnet-Identiy-Core-7475a961)
- [MVC için ASP.NET ıdentity'de rolleriyle çalışma](http://www.dotnetfunda.com/articles/show/2898/working-with-roles-in-aspnet-identity-for-mvc) tarafından [Sheo Narayan](http://www.dotnetfunda.com/profile/sheonarayan.aspx)
- [ASP.NET üyeliğinden ASP.NET Identity'ye taşıma](http://webdojo.sharepoint.com/ajmatthews/_layouts/15/start.aspx#/Lists/Posts/Post.aspx?ID=2) Alistair Matthews tarafından

<a id="video"></a>
## <a name="videos"></a>Videolar

- Channel 9 [ASP.NET uygulamalarını ve hizmetlerini güvenli hale getirme: Modern uygulamalar için güvenlik Facelift](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B421#fbid=PhVT9E1WRtr?hashlink=fbid) Ido Flatow tarafından
- Channel 9 [ASP.NET Identity'ye giriş](https://channel9.msdn.com/Events/dotnetConf/2014/ASP-NET-Identity-Security) Pranav rastogi'nin
- Channel 9 [ASP.NET ASP.NET Identity kullanılarak kimlik doğrulaması](https://channel9.msdn.com/Shows/Web+Camps+TV/Special-Movember-Episode-ASPNET-Authentication-Provider) Corry fowler'ın tarafından
- Channel 9 [Modern Web uygulamaları oluşturma: ASP.NET Identity](https://channel9.msdn.com/Series/Building-Modern-Web-Apps/03) Jeff Koch tarafından
- Channel 9 [sitenizi ASP.NET Identity ile güvenli hale getirme](https://channel9.msdn.com/Events/TechDays/Techdays-2014-the-Netherlands/Securing-your-website-with-ASP-NET-Identity) Alex Thissen tarafından
- [ASP.NET Identity var olan bir DB-Model kullanmak](https://www.youtube.com/watch?v=elfqejow5hM) Alexander Etikan tarafından
- [Bir ASP.NET Identity](https://www.youtube.com/watch?v=w8GD-QIusKk) Ivaylo Kenov, Telerik tarafından
- [Çekçe ASP.NET Identity](https://www.youtube.com/watch?v=tVbZp5brcpY) bu ders temel kimlik doğrulaması dağıtmayı, Twitter veya Facebook gibi dış kimlik sağlayıcıları için destek ekleme ve bir kerelik parola (OTP) kullanmayı göstereceğiz. [ASP.NET Identity je nástupce üyelik rol sağlayıcısı&#367; v tedy knihovna ASP.NET pro zajišt&#283;ní autentizace uživatel&#367;. V této p&#345;ednášce sı ukážeme, jak nasad]

<a id="cust"></a>
## <a name="custom-storage-providers-for-aspnet-identity"></a>ASP.NET kimliği için özel depolama sağlayıcıları

Kendi sağlayıcınızı yazmak istiyorsanız, okuma [genel bakış, özel depolama sağlayıcıları için ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) ve [uygulama ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx) ve listelenen OSS projelerden birine kaynağı inceleyin Aşağıda.

- Öğretici: [ASP.NET kimliği için özel depolama sağlayıcıları genel bakış](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md) tarafından Tom FitzMacken
- Blog: [ASP.NET Identity uygulama](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Öğretici:[temel kimlik hesaplarını ayarlama ve bunları dış bir DB işaret eden](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Tarafından [ @xivSolutions ](https://twitter.com/xivSolutions).
- Öğretici[: özel MySQL ASP.NET Identity depolama sağlayıcısı uygulama](../extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent varlıkları](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) tarafından [SoftFluent](http://www.softfluent.com/)
- [Azure tablo depolaması](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) James Randall tarafından.
- Azure tablo depolaması: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) tarafından [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Daniel Wertheim tarafından Cloudant.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Esnek arama[h: esnek kimlik](https://github.com/bmbsqd/elastic-identity) Bombsquad AB. tarafından
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) Jonathan Sheely Jonathan Sheely tarafından.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) Antônio Milesi Bastos tarafından.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) tarafından [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) tarafından [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- EF oluşturmak için T4 şablonlarını kod için bir "ilk veritabanı" kullanıcı deposu: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)

<a id="additional"></a>
## <a name="additional-aspnet-identity-resources"></a>Ek ASP.NET Identity kaynakları

- [OWIN için Yahoo ve LinkedIn OAuth güvenlik sağlayıcıları ile tanışın](http://blog.beabigrockstar.com/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) Jerrie Pelser Yahoo ve LinkedIn yönergeleri tarafından.

<a id="qand"></a>
## <a name="qampa-questionanswer"></a>Q&amp;bir (soru/yanıt)

- Out (bunlar üzerinde bu bilgisayar/tarayıcı 2FA Git gerekmez) "Beni Hatırla" seçeneğinin etkin kullanıcılar s: kilitli değil kilitli. Neden ve nasıl, engelleyebilirim? Yanıt [burada](http://stackoverflow.com/questions/24312247/locked-out-users-can-login-if-they-have-auth-cookie).
- **Q**: nasıl miyim saklayabilir kullanıcının gerçek adı gibi özel talepler her istekte gereksiz veritabanı sorguları kaçınmak için ASP.NET kimlik tanımlama. Yanıt [burada](http://stackoverflow.com/questions/23622047/identity-cookie-loses-custom-claim-information-after-a-period-of-time).
- **S: güncelleştirme AspNetUser parola karması**: 2 projeleri sahibim. ASP.NET kimlik doğrulaması kullanan bunlardan biri, diğer yönetim tarafında Windows kimlik doğrulaması kullanır. Diğer kullanıcıları yönetebilmek için yönetici projeyi istiyorum. Parola dışında her şeyi değiştirebilirsiniz. [Burada yanıt](http://stackoverflow.com/questions/23880666/updating-aspnetuser-password-hash).
- **Q**: nasıl miyim sıfırlayabilmeniz parola diğer kullanıcılar için bir yönetici olarak? Yanıt [burada](http://stackoverflow.com/questions/23783249/identity-2-0-reset-password-by-admin/24211766#24211766).
- **Q**: ASP.NET MVC IdentityUser kullanıcı adı alanında görüntülenen adı değiştirebilir miyim? Yanıt [burada](http://stackoverflow.com/questions/23256650/can-i-change-the-displayed-name-of-the-username-field-in-asp-net-mvc-identityuse).
- **Q**: nasıl kaldırabilirim diğer kullanıcıları belirli rollere eklemek için kullanıcılara izinler gran? Yanıt [burada](http://stackoverflow.com/questions/23695373/allow-users-to-grant-permissions-to-other-users-for-their-account-in-asp-net-ide).
- **Q**: profil bilgilerini AspNetUsers tabloda AspNetUserClaims tablo ve depolama. Yanıt [burada](http://stackoverflow.com/questions/23215727/is-there-any-benefit-to-storing-user-information-in-aspnetuserclaims-with-asp-ne).
- **Q**: bana bir dış kimlik doğrulama sağlayıcısı kullanılırken unutmayın. Yanıt [burada](http://stackoverflow.com/questions/23180896/how-to-remember-the-login-in-mvc5-when-an-external-provider-is-used).
- **Q**: neden her istek gerektiriyor bir ApplicationDBContext değil, çok fazla ek yükü? Yanıt Hayır, ek yükü düşüktür.
- S: nasıl oturum açan kullanıcıların listesini elde ederim? Yanıt [burada](http://stackoverflow.com/questions/22995653/getting-a-list-of-logged-in-users-in-asp-net-identity/).
- S: bir kullanıcı Microsoft.ASPNET.Identity oturum açtığında miyim algılayabilir? Yanıt [burada](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- S: yerelleştirilmiş hata iletileri için kimlik nasıl alabilirim? Yanıt [burada](http://stackoverflow.com/questions/22835981/asp-net-identity-localization-publickeytoken/22845864#22845864).
- S: 30 dakikada bir yeni taleplerini almanın CookieMiddleware nasıl yapılandırabilirim? Yanıt [burada](http://stackoverflow.com/questions/22682663/how-to-hold-the-cookies-claims-updated-with-mcv5-owin/22796932#22796932).
- S: nasıl bunlar açtıktan sonra kullanıcı için talepleri Değiştir? Yanıt [burada](http://stackoverflow.com/questions/22768836/can-i-modify-claims-in-asp-net-identity-with-owin-after-calling-signin/22769963#22769963).
- S: nasıl güvenlik belirteçlerini geçersiz? Yanıt [burada](http://stackoverflow.com/questions/22755700/revoke-token-generated-by-usertokenprovider-in-asp-net-identity-2-0/22767286#22767286).
- S: nasıl tanımlama bilgisi Ara depolama talepleri olan? Yanıt [burada](http://stackoverflow.com/questions/22320632/storing-retrieving-user-data-without-database-when-using-owin-cookie-authenticat/22541856#22541856).
- S: bir PIN veya güvenlik MVC Uygulamam her eylem yöntemi denetlemek istiyorsanız, ancak her istek, eylem yöntemi için PIN girmesini zorunluluğunu kullanıcılar başarı depolamak istiyorsunuz. Yanıt [burada](http://stackoverflow.com/questions/22479958/security-check-an-user-to-a-access-controller-action/22486075#22486075).
- S: dağıtımım için sosyal sağlayıcıdan döndürülen e-posta adre DB'ye kaydedin, bunu nasıl yapabilirim gibi musunuz? Yanıt [burada](http://stackoverflow.com/questions/22888397/save-claims-to-db-on-login/22970969#22970969):
- S: bir kullanıcı bir "Beni Hatırla" tanımlama bilgisi iki ile/ile-out içinde açtığında miyim algılayabilir mi? Yanıt [burada](http://stackoverflow.com/questions/22956486/how-can-i-detect-when-a-user-logs-in-with-microsoft-aspnet-identity/22970698#22970698).
- ASP.NET Identity ile OWIN Taleplerde Signın çağrıldıktan sonra değiştirebilir miyim? Yanıt: Arama Signın tam olarak bir kullanıcı için talepleri değiştirmek istediğiniz ne yapılacağını kaydetmeleri beklenir ' dir. Sonraki isteklerde authenticateasync görünmesini yeni talepleri gördüğünüz neden olan tanımlama bilgisi ile serileştirilecek Claimsıdentity temel neden olur.
