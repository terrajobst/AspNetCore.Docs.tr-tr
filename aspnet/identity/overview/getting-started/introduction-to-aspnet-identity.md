---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: ASP.NET Identity giriş | Microsoft Docs
author: jongalloway
description: Yolları web uygulamaları typicall birçok değişiklikler olmuştur sonra ASP.NET üyelik sistemini 2005'te ve bu yana ASP.NET 2.0 sonradan sunulmuştur...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 59272f4659256e108ee99b22eb3bd3e2583a617c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-aspnet-identity"></a>ASP.NET Identity giriş
====================
tarafından [Jon Galloway](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

> Web uygulamaları genellikle kimlik doğrulama ve yetkilendirme işler yollarla birçok değişiklikler olmuştur sonra ASP.NET üyelik sistemini 2005'te ve bu yana ASP.NET 2.0 sonradan sunulmuştur. ASP.NET, web, telefon veya tablet modern uygulamalar oluştururken üyelik sistemi olması gereken bir yeni göz kimliğidir.
> 
> Bu makalede Pranav Rastogi tarafından yazılan ([@rustd](https://twitter.com/rustd)), Jon Galloway ([@jongalloway](https://twitter.com/jongalloway)), zel Dykstra ve Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>Arka planı: ASP.NET üyelik

### <a name="aspnet-membership"></a>ASP.NET üyelik

[ASP.NET üyelik](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) form kimlik doğrulaması ve SQL Server veritabanı kullanıcı adları, parolalar ve profil verileri için söz konusu 2005'te yaygın site üyeliği gereksinimlerini çözmek için tasarlanmıştır. Bugün bir çok daha geniş dizisi web uygulamaları için veri depolama seçenekleri yoktur ve çoğu Geliştirici sosyal kimlik sağlayıcıları için kimlik doğrulama ve yetkilendirme işlevselliği kullanmak sitelerinde etkinleştirmek istiyor. ASP.NET üyelik'ın tasarım sınırlamaları bu geçişi zor yapın:

- Veritabanı şeması, SQL Server için tasarlanmıştır ve onu değiştiremezsiniz. Profil bilgisi ekleyebilirsiniz, ancak ek veri erişimi dışında herhangi bir yöntem profili sağlayıcısı API aracılığıyla zorlaştırır farklı bir tablo paketlenmiştir.
- Sağlayıcı sistem yedekleme veri deposu değiştirmenizi sağlar, ancak sistem varsayımlar bir ilişkisel veritabanı için uygun çevresinde tasarlanmıştır. Azure depolama tabloları gibi bir ilişkisel olmayan depolama mekanizmasını üyelik bilgilerini depolamak için bir sağlayıcı yazabilirsiniz ancak çok fazla kod ve çok sayıda yazarak ilişkisel tasarım çalışmak zorunda sonra `System.NotImplementedException` olmayan yöntemler için özel durumlar NoSQL veritabanları için geçerlidir.
- Günlük-günlük-çıkış işlevselliği Forms kimlik doğrulamasına bağlı olduğundan, üyelik sistemi kullanamazsınız [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN ara yazılımı bileşenleri Dış kimlik sağlayıcıları (gibi Microsoft Accounts, Facebook, Google, Twitter) kullanarak oturum açma işlemleri için destek dahil olmak üzere kimlik doğrulaması içerir ve günlük bileşenler kuruluş hesaplarından kullanarak şirket içi Active Directory veya Azure Active Directory. OWIN OAuth 2.0 JWT ve CORS için destek de içerir.

### <a name="aspnet-simple-membership"></a>ASP.NET basit üyelik

[ASP.NET basit üyelik](../../../web-pages/overview/security/16-adding-security-and-membership.md) ASP.NET Web sayfaları için bir üyelik sistemi olarak geliştirilmiştir. WebMatrix ve Visual Studio 2010 SP1 ile serbest bırakıldı. Basit üyelik amacı, bir Web sayfaları uygulama üyelik işlevselliğini eklemek kolaylaştırmak için oluştu.

Basit üyelik kullanıcı profili bilgilerini özelleştirmek daha kolay yaptı, ancak hala ASP.NET üyelik diğer sorunları paylaşır ve bazı sınırlamalar vardır:

- İlişkisel olmayan deposundaki üyelik sistemi veri kalıcı hale getirmek sabit.
- OWIN ile kullanamazsınız.
- Mevcut ASP.NET üyelik sağlayıcıları ile iyi çalışmaz ve Genişletilebilir değil.

### <a name="aspnet-universal-providers"></a>ASP.NET Evrensel Sağlayıcılar

[ASP.NET Evrensel Sağlayıcılar](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) Azure SQL veritabanı ve ayrıca çalışma SQL Server Compact ile üyelik bilgilerini Microsoft kalıcı hale getirmek mümkün kılmak için geliştirilmiştir. Evrensel sağlayıcıları Entity Framework Code, Evrensel sağlayıcıları EF tarafından desteklenen tüm deposundaki verileri kalıcı hale getirmek için kullanılabileceği anlamına gelir First üzerinde oluşturulmuştur. Evrensel sağlayıcıları ile veritabanı şeması oldukça çok temizlendi.

Bunlar hala SqlMembership sağlayıcı onunla aynı sınırlamalara taşımak için Evrensel Sağlayıcılar ASP.NET üyeliği altyapısı üzerinde oluşturulmuştur. Diğer bir deyişle, ilişkisel veritabanları için tasarlanmıştır ve profil ve kullanıcı bilgileri özelleştirin zordur. Bu sağlayıcılar yine de oturum açması ve günlük genişletme işlevselliği için form kimlik doğrulaması kullanın.

## <a name="aspnet-identity"></a>ASP.NET Kimlik

Üyeliği olarak ASP.NET takım çok müşterilerden gelen geri bildirim alanından öğrenilen yıllar içinde ASP.NET yazıdaki gelişmiştir.

Kullanıcıların bir kullanıcı adı ve kendi uygulamanızı kaydettiniz parolasını girerek oturum açacak varsayımına artık geçerli değil. Web fazla sosyal haline gelmiştir. Kullanıcıların birbirleri ile gerçek zamanlı Facebook, Twitter ve diğer sosyal web siteleri gibi sosyal kanallar aracılığıyla etkileşim. Geliştiriciler, kullanıcıların bunlar zengin bir deneyim web sitelerinde böylece sosyal kimliklerini oturum oturum istiyorsunuz. Modern üyelik sistemi günlük bileşenleri yeniden yönlendirme tabanlı kimlik doğrulama sağlayıcıları Facebook, Twitter ve diğerleri gibi için etkinleştirmeniz gerekir.

Web geliştirme gelişen gibi böylece web geliştirme desenleri vermedi. Birim uygulama kodunu sınama uygulama geliştiricileri için temel sorun haline geldi. 2008'de ASP.NET kısmen birim test edilebilir ASP.NET uygulamaları geliştirmek geliştiricilere yardımcı olmak için Model-View-Controller (MVC) desenine dayalı yeni bir framework eklendi. Birimine isteyen geliştiriciler üyelik sistemi bunu yapabilmek için de isteyen kullanıcıların uygulama mantığını sınayın.

Web uygulaması geliştirme bu değişiklikler göz önünde bulundurularak, ASP.NET Identity aşağıdaki hedefleri ile geliştirilmiştir:

- **Bir ASP.NET kimlik sistemi**

    - ASP.NET kimliği, tüm ASP.NET MVC, Web Forms, Web sayfaları, Web API ve SignalR gibi ASP.NET çerçeveleri ile kullanılabilir.
    - Web, telefon, mağaza veya karma uygulamaları oluştururken ASP.NET Identity kullanılabilir.
- **Profil verileri kullanıcı hakkında takma kolaylığı**

    - Kullanıcı profili bilgileri ve şema üzerinde denetiminiz yoktur. Örneğin, bir hesap uygulamanızı kaydederken kullanıcı tarafından girilen Doğum tarihleri depolamak sistem kolayca etkinleştirebilirsiniz.

- **Kalıcılık denetimi**

    - Varsayılan olarak, ASP.NET Identity sistem tüm kullanıcı bilgilerini bir veritabanında depolar. Tüm Kalıcılık mekanizması uygulamak için ASP.NET Identity Entity Framework Code First kullanır.
    - Denetim olduğundan veritabanı şema, tablo adlarının değiştirilmesi veya değiştirme gibi genel görevleri yapmak birincil anahtarlar veri türünü basittir.
    - Throw gerek kalmadan SharePoint, Azure Storage tablo hizmeti, NoSQL veritabanları, vb. gibi farklı depolama mekanizmaları takın kolaydır `System.NotImplementedExceptions` özel durumları.
- **Birim Test Edilebilirlik**

    - ASP.NET Identity web uygulamasını daha fazla birim test edilebilir hale getirir. ASP.NET kimlik kullanmak uygulamanızın parçalarını için birim testleri yazabilirsiniz.
- **Rol sağlayıcısı**

    - Erişim için uygulamanızın parçalarını rolleri tarafından kısıtlamanıza olanak sağlayan bir rol sağlayıcısı yok. Kolayca "Yönetici" gibi rolleri oluşturun ve kullanıcıları rollere ekleyin.
- **Talep tabanlı**

    - Burada kullanıcının kimliğini talepler kümesi temsil edilir talep tabanlı kimlik doğrulaması, ASP.NET Identity destekler. Talep rolleri izin verdiğinden, bir kullanıcının kimliğini tanımlamak için çok fazla açıklayıcı olmasını geliştiricilerin olanak sağlar. Rol üyeliğini yalnızca bir Boole değeri (üye veya üye olmayan) iken, bir talep kullanıcının kimliğini ve üyelik hakkında zengin bilgi içerebilir.
- **Sosyal oturum açma sağlayıcılarıyla**

    - Kolayca Microsoft Account, Facebook, Twitter, Google ve diğerleri gibi sosyal günlük bileşenler uygulamanıza ekleyin ve uygulamanızda kullanıcıya özgü verileri depolamak.
- **Azure Active Directory**

    - Azure Active Directory'yi kullanarak oturum açma işlevini ekleyin ve kullanıcıya özgü veri depolayın. Daha fazla bilgi için bkz: [Kurumsal hesaplar](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) , Visual Studio 2013'da ASP.NET Web projeleri oluşturma
- **OWIN tümleştirme**

    - ASP.NET kimlik doğrulaması şimdi tüm OWIN tabanlı ana bilgisayarda kullanılan OWIN ara yazılımı temel alır. ASP.NET Identity System.Web üzerinde herhangi bir bağımlılığı yok. Tam uyumlu OWIN çerçevesi ve herhangi bir barındırılan OWIN uygulamada kullanılabilir.
    - ASP.NET Identity günlük-günlük-çıkış kullanıcıların web sitesindeki için OWIN kimlik doğrulaması kullanır. Bu tanımlama bilgisi oluşturmak için FormsAuthentication kullanmak yerine, uygulama OWIN CookieAuthentication Bunu yapmak için kullandığı anlamına gelir.
- **NuGet paketi**

    - ASP.NET kimliği ile Visual Studio 2013 sevk ASP.NET MVC, Web formları ve Web API şablonlarındaki yüklü bir NuGet paketi olarak dağıtılır. NuGet Galeriden bu NuGet paketini yükleyebilirsiniz.
    - Bir NuGet olarak ASP.NET Identity serbest paket yeni özellikler ve hata düzeltmeleri yinelemek ve bunlar geliştiricilere Çevik bir şekilde teslim etmek ASP.NET takım kolaylaştırır.

## <a name="getting-started-with-aspnet-identity"></a>ASP.NET kimliği ile çalışmaya başlama

ASP.NET Identity Visual Studio 2013 proje şablonlarını, ASP.NET MVC, Web Forms, Web API ve SPA için kullanılır. Bu kılavuzda, biz nasıl oturum kaydetmeyi işlevselliği eklemek için ASP.NET Identity proje şablonlarını kullanın göstermek ve bir kullanıcı oturum.

Aşağıdaki yordamı kullanarak ASP.NET Identity uygulanır. Bu makalenin amacı, ASP.NET Identity üst düzey bir genel bakış, vermektir; Bu adım adım izleyin veya yalnızca ayrıntıları okuyun. ASP.NET kimliği kullanıcıları, rolleri ve profil bilgilerini eklemek için yeni API kullanımı dahil olmak üzere, kullanarak uygulamaları oluşturma hakkında daha ayrıntılı yönergeler için bu makalenin sonunda sonraki adımlar bölümüne bakın.

1. Bir ASP.NET MVC uygulaması ile tek tek hesapları oluşturun. ASP.NET MVC, Web Forms, Web API, SignalR vb. ASP.NET Identity kullanabilirsiniz. Bu makalede biz bir ASP.NET MVC uygulamasını başlatın.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. Oluşturulan proje için ASP.NET Identity aşağıdaki üç paketleri içerir.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Bu paket, ASP.NET Identity veri ve şema SQL Server'a korunur ASP.NET Identity Entity Framework uygulamasını sahiptir.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Bu paket, ASP.NET kimliği için çekirdek arabirimleri vardır. Bu paket, hedefleri farklı Kalıcılık veritabanlarını vb. Azure Table Storage, NoSQL gibi depolayan ASP.NET kimliği için bir uygulama yazmak için kullanılabilir.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Bu paket OWIN kimlik doğrulaması, ASP.NET uygulamalarındaki ASP.NET kimliği ile bağlamak için kullanılan işlevselliği içerir. Günlük, uygulama ve bir tanımlama bilgisi oluşturmak için OWIN tanımlama bilgisi kimlik doğrulaması ara yazılımı çağrısına işlevindeki eklediğinizde, bu kullanılır.
3. Bir kullanıcı oluşturma.  
   Uygulamayı başlatın ve sonra tıklatın **kaydetmek** bir kullanıcı oluşturmak için bağlantı. Aşağıdaki resimde kullanıcı adı ve parola toplayan kayıt sayfası gösterir.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Kullanıcı tıkladığında **kaydetmek** düğmesini `Register` eylem hesabı denetleyicisinin aşağıda vurgulandığı gibi ASP.NET Identity API'sini çağırarak kullanıcı oluşturur:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Oturum aç.  
   Kullanıcı başarıyla oluşturulduysa tarafından açmışken `SignInAsync` yöntemi.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

   Yukarıdaki vurgulanmış kodu `SignInAsync` yöntem oluşturur bir [Claimsıdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET kimliği ve OWIN tanımlama bilgisi kimlik doğrulaması talep tabanlı sistem olduğundan, kullanıcı için bir Claimsıdentity oluşturmak için uygulama framework gerektirir. Claimsıdentity hangi rolleri kullanıcının ait olduğu gibi kullanıcı için tüm talepleri hakkında bilgi yer almaktadır. Bu aşamada daha fazla kullanıcı talebini de ekleyebilirsiniz.  
  
   Aşağıda vurgulanan kod `SignInAsync` yöntemi, bulunan OWIN ve arama kullanarak kullanıcı oturum açtığında `SignIn` ve Claimsıdentity geçirme.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Oturumunuzu kapatın.  
   Tıklatarak **oturumunu** bağlantı hesabı denetleyicide kapatma eylemi çağırır. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Vurgulanan gösterildiği OWIN kod `AuthenticationManager.SignOut` yöntemi. Bunun için paraleldir [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) yöntemi tarafından kullanılan [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) Web Forms modülünde.

## <a name="components-of-aspnet-identity"></a>ASP.NET Identity bileşenleri

Aşağıdaki diyagram ASP.NET Identity sisteminin bileşenleri gösterir (tıklayın [bu](introduction-to-aspnet-identity/_static/image3.png) ya da büyütmek için diyagramdaki). Yeşil paketlerinde ASP.NET Identity sistemi olun. Tüm paketler, ASP.NET uygulamalarındaki ASP.NET Identity sistemi kullanmak için gerekli bağımlılıklardır.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

Daha önce bahsedilen değil NuGet paketlerini kısa bir açıklaması verilmiştir:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Tanımlama bilgisi kullanmak bir uygulama sağlar ara yazılımı tabanlı kimlik doğrulaması, ASP benzer. NET'in form kimlik doğrulaması.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework Microsoft'un önerilen veri erişim ilişkisel veritabanları için teknolojisidir.

## <a name="migrating-from-membership-to-aspnet-identity"></a>ASP.NET kimliği için üyeliğinden geçirme

ASP.NET üyelik veya basit üyelik yeni ASP.NET Identity sisteme kullanan mevcut uygulamalarınızı geçirme Kılavuzu yakında sağlamaya yönelik umuyoruz.

## <a name="next-steps"></a>Sonraki Adımlar

- [Facebook ve Google OAuth2 ve Openıd oturum açma ile bir ASP.NET MVC 5 uygulaması oluşturma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 Öğretici ASP.NET Identity API kullanıcı veritabanı profil bilgileri ve Google ve Facebook ile kimlik doğrulaması yapmayı eklemek için kullanır.
- [Kimlik doğrulama ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Bu öğretici kimlik API kullanıcıları ve rolleri eklemek için nasıl kullanılacağını gösterir.
- [Bireysel kullanıcı hesapları](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) oluşturmada Visual Studio 2013'te ASP.NET Web projeleri
- [Kurumsal hesaplar](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) oluşturmada Visual Studio 2013'te ASP.NET Web projeleri
- [Profil bilgilerinde ASP.NET Identity VS 2013 şablonlarını özelleştirme](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [VS 2013 proje şablonlarını kullanılan sosyal sağlayıcılardan daha fazla bilgi edinin](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Temel rol ve kullanıcı desteği eklemeyi ve roller ve kullanıcı yönetimi gösteren örnek uygulama.
