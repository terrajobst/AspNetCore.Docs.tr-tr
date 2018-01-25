---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: "Kullanıcı hesapları (VB) oluşturma | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide yeni kullanıcı hesapları oluşturmak için üyelik framework (aracılığıyla SqlMembershipProvider) kullanılarak inceleyeceksiniz. Yeni bize nasıl oluşturulacağını göreceğiz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 61621ffaae98ac74c16b2ff014ba9d85c2c10b3a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="creating-user-accounts-vb"></a>Kullanıcı hesapları (VB) oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> Bu öğreticide yeni kullanıcı hesapları oluşturmak için üyelik framework (aracılığıyla SqlMembershipProvider) kullanılarak inceleyeceksiniz. Program aracılığıyla ve ASP aracılığıyla yeni kullanıcılar oluşturmak nasıl göreceğiz. NET'in yerleşik CreateUserWizard denetim.


## <a name="introduction"></a>Giriş

İçinde <a id="_msoanchor_1"> </a> [önceki öğretici](creating-the-membership-schema-in-sql-server-vb.md) biz tabloları, görünümleri, eklenen ve saklı yordamlar için gerekli bir veritabanında uygulama hizmetleri şemanın yüklü `SqlMembershipProvider` ve `SqlRoleProvider`. Bu serideki öğreticileri kalanı için ihtiyacımız altyapı oluşturulur. Bu öğreticide biz üyelik framework kullanarak inceleyeceksiniz (aracılığıyla `SqlMembershipProvider`) yeni kullanıcı hesapları oluşturmak için. Program aracılığıyla ve ASP aracılığıyla yeni kullanıcılar oluşturmak nasıl göreceğiz. NET'in yerleşik CreateUserWizard denetim.

Yeni kullanıcı hesapları oluşturmak öğrenme ek olarak, biz de oluşturduğumuz ilk demo Web sitesi güncelleştirmeniz gerekecektir  *<a id="_msoanchor_2"> </a> [form kimlik doğrulaması bir genel bakış](../introduction/an-overview-of-forms-authentication-vb.md)*  öğretici, Gelişmiş ve  *<a id="_msoanchor_3"> </a> [Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*  Öğreticisi. Tanıtım web uygulamamız sabit kodlanmış kullanıcı adı/parola çiftleri karşı kullanıcıların kimlik bilgilerini doğrulayan bir oturum açma sayfası vardır. Ayrıca, `Global.asax` özel oluşturan kod içerir `IPrincipal` ve `IIdentity` nesneleri kimliği doğrulanmış kullanıcılar için. Üyelik framework karşı kullanıcıların kimlik bilgilerini doğrulamak ve özel asıl ve kimlik mantığını kaldırmak için oturum açma sayfasına güncelleştireceğiz.

Haydi başlayalım!

## <a name="the-forms-authentication-and-membership-checklist"></a>Form kimlik doğrulaması ve üyelik denetim listesi

Üyelik framework ile çalışmaya başlamadan önce şimdi Biz bu noktaya ulaşmak için yapılan önemli adımlar gözden geçirmek için bir dakikanızı ayırın. Üyelik framework ile kullanırken `SqlMembershipProvider` bir form tabanlı kimlik doğrulama senaryosunda, aşağıdaki adımları web uygulamanızda üyelik işlevselliğini uygulamadan önce gerçekleştirilmesi gerekir:

1. **Form tabanlı kimlik doğrulamasını etkinleştirin.** Biz anlatıldığı gibi  *<a id="_msoanchor_4"> </a> [form kimlik doğrulaması bir genel bakış](../introduction/an-overview-of-forms-authentication-vb.md)*, form kimlik doğrulaması etkin düzenleyerek `Web.config` ve ayarı `<authentication>` öğenin `mode` özniteliğini `Forms`. Etkin formlar kimlik doğrulaması ile her gelen istek için incelenir bir *forms kimlik doğrulaması bileti*, varsa, belirten istek sahibi.
2. **Uygulama Hizmetleri şeması uygun veritabanına ekleyin.** Kullanırken `SqlMembershipProvider` biz uygulama hizmetleri şeması veritabanına yüklemeniz gerekir. Genellikle bu şema uygulamanın veri modeli tutan aynı veritabanına eklenir. *<a id="_msoanchor_5"> </a> [SQL Server üyelik şema oluşturma](creating-the-membership-schema-in-sql-server-vb.md)*  öğretici Aranan kullanarak `aspnet_regsql.exe` bunu gerçekleştirmek için aracı.
3. **Adım 2 ' veritabanı başvurmak için Web uygulamasının ayarlarını özelleştirin.** *SQL Server üyelik şema oluşturma* öğretici gösterdi web uygulaması yapılandırmanın iki yolu böylece `SqlMembershipProvider` 2. adımda seçilen veritabanı kullanırsınız: değiştirerek `LocalSqlServer` bağlantı dizesi adı; veya yeni bir kayıtlı sağlayıcı üyelik framework sağlayıcılar listesine ekleyerek ve veritabanından kullanmak için yeni sağlayıcıyı özelleştirme 2. adım.

Bir web uygulaması oluşturma kullandığında `SqlMembershipProvider` ve form tabanlı kimlik doğrulama, kullanmadan önce bu üç adımı gerçekleştirmek gerekecek `Membership` sınıf veya ASP.NET oturum açma Web denetimleri. Biz zaten adımları önceki eğitimlerine gerçekleştirilen olduğundan, biz üyelik framework kullanmaya başlamak hazırsınız!

## <a name="step-1-adding-new-aspnet-pages"></a>1. adım: Yeni ASP.NET sayfaları ekleme

Bu öğretici ve sonraki üç biz çeşitli üyelik ilgili işlevleri ve özellikleri inceleniyor. ASP.NET sayfaları bu öğreticileri incelenmesi konuları uygulamak için bir dizi ihtiyacımız. Bu sayfa ve site haritası oluşturalım `(Web.sitemap)`.

Başlangıç adlı projeye yeni bir klasör oluşturarak `Membership`. Ardından, beş yeni ASP.NET sayfaları ekleme `Membership` klasörü, her sayfasıyla bağlama `Site.master` ana sayfa. Sayfa adı:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Bu noktada, projenizin Solution Explorer Şekil 1'de gösterilen ekran benzer görünmelidir.


[![Beş yeni sayfalar üyelik klasöre eklendi](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Şekil 1**: beş yeni sayfalar eklenmiştir `Membership` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image3.png))


Her sayfada bu noktada, iki içerik, her ana sayfanın ContentPlaceHolders için denetimleriniz gerekir: `MainContent` ve `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Sözcüğünün `LoginContent` ContentPlaceHolder'ın varsayılan biçimlendirme oturum açarken veya devre dışı olup olmadığını kullanıcının kimliği doğrulanır bağlı olarak site için bir bağlantı görüntülenir. Varlığını `Content2` içerik denetimi, ancak ana sayfanın varsayılan biçimlendirme geçersiz kılar. Biz anlatıldığı gibi  *<a id="_msoanchor_6"> </a> [form kimlik doğrulaması bir genel bakış](../introduction/an-overview-of-forms-authentication-vb.md)*  öğretici, bu sayfaları burada değil istiyoruz sol sütunda oturum açmayla ilgili seçenekleri görüntülemek için yararlıdır.

Bu beş sayfaları için ancak için ana sayfa varsayılan biçimlendirmesini göstermek istiyoruz `LoginContent` ContentPlaceHolder. Bu nedenle, kaldırmak için bildirim temelli biçimlendirme `Content2` içerik denetimi. Bunu yaptıktan sonra her beş sayfanın biçimlendirme yalnızca bir içerik denetimi içermelidir.

## <a name="step-2-creating-the-site-map"></a>2. adım: Site Haritası oluşturma

Bir gezinme kullanıcı arabirimi çeşit uygulamak için en basit Web siteleri dışındaki tüm gerekir. Gezinti kullanıcı arabirimi, basit bir site çeşitli bölümlerini bağlantılar listesi olabilir. Alternatif olarak, bu bağlantıları menüleri veya ağaç görünümlere düzenlenmiş. Sayfa geliştiricileri de gezinme kullanıcı arabirimi oluşturma Öykü yalnızca yarısını bağlıdır. Ayrıca sitenin mantıksal yapısı sürdürülebilir ve güncelleştirilebilir bir biçimde tanımlamak için bazı yöntemler ihtiyacımız var. Yeni sayfalar eklenir veya var olan sayfaları kaldırıldı gibi tek bir kaynağı - site haritası - güncelleştirebilmek istiyoruz ve bu değişiklikleri sitenin gezinme kullanıcı arabirimi yansıtılmasını.

-Site haritası tanımlama ve site haritasını kullanan bir gezinme kullanıcı arabirimini uygulayan - bu iki görevleri sayesinde Site Haritası framework yapmanın kolay ve gezinti Web eklenmiş ASP.NET 2.0 sürümünü denetler. Bir geliştirici site haritası tanımlamak Site Haritası framework sağlar ve ardından programlı bir API aracılığıyla erişmek için ( [ `SiteMap` sınıfı](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Gezinti Web denetimleri yerleşik içeren bir [menü denetimi](https://msdn.microsoft.com/library/bz09dy46.aspx), [TreeView denetimi](https://msdn.microsoft.com/library/3eafky27.aspx)ve [ASP](https://msdn.microsoft.com/library/3eafky27.aspx).

Üyelik ve roller çerçeveleri gibi Site Haritası framework üzerinde yerleşik [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Site haritası sağlayıcısı sınıfı tarafından kullanılan bellek içi yapısı oluşturmak için iş `SiteMap` bir XML dosyası veya bir veritabanı tablosu gibi bir kalıcı veri deposundan sınıfı. .NET Framework site haritası verileri bir XML dosyasından okuma varsayılan Site haritası sağlayıcısı ile birlikte ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), ve bu biz kullanacağınız Bu öğreticide sağlayıcıdır. Bazı diğer Site haritası sağlayıcısı uygulamaları için bu öğreticinin sonunda başka okumalar bölümüne bakın.

Varsayılan Site haritası sağlayıcısı adlı bir doğru biçimlendirilmiş XML dosyası bekler `Web.sitemap` kök dizininde bulunması. Bu varsayılan sağlayıcı kullanıyoruz olduğundan, biz bu tür bir dosya ekleyin ve site haritanın yapısı uygun XML biçiminde tanımlamanız gerekir. Dosya eklemek için Çözüm Gezgini'nde proje adına sağ tıklayın ve yeni öğe Ekle'yi seçin. Tür Site Haritası adlı bir dosyayı eklemek için iletişim kutusundan kabul `Web.sitemap`.


[![Projenin kök dizinine birtakım adlı bir dosya ekleme](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Şekil 2**: adlandırılmış dosya ekleme `Web.sitemap` projenin kök dizinine ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image6.png))


XML site haritası dosyası olarak hiyerarşi Web sitesi yapısını tanımlar. Bu hiyerarşi ilişkisi ilişkilerini aracılığıyla XML dosyasındaki modellenir `<siteMapNode>` öğeleri. `Web.sitemap` Başlamalı ve bir `<siteMap>` tam olarak bir üst düğümün `<siteMapNode>` alt. Bu üst düzey `<siteMapNode>` öğesi hiyerarşisinin kökü temsil eder ve rasgele sayıda alt düğüme sahip. Her `<siteMapNode>` öğesi içermelidir bir `title` özniteliği ve isteğe bağlı olarak içerebilir `url` ve `description` öznitelikler, diğerlerinin; yanı sıra her boş olmayan `url` özniteliği benzersiz olmalıdır.

Aşağıdaki XML öğesine girin `Web.sitemap` dosyası:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

Yukarıdaki site haritası biçimlendirme Şekil 3'te gösterilen hiyerarşi tanımlar.


[![Site Haritasını gezinme hiyerarşik bir yapıyı temsil eder](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Şekil 3**: Site Haritası gezinme hiyerarşik bir yapıyı temsil eder ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>3. adım: bir gezinme kullanıcı arabirimi eklemek için ana sayfa güncelleştiriliyor

ASP.NET Web denetimleri Gezinti ile ilgili bir kullanıcı arabirimi tasarlamak için bir dizi içerir. Bunlar, menü, TreeView ve SiteMapPath denetimlerini içerir. Alt öğelerinden yanı sıra ziyaret geçerli düğüm gösteren bir içerik haritası SiteMapPath görüntüler ancak Menu ve TreeView denetimleri site haritası yapısında bir menüsü veya bir ağaç sırasıyla işleyebilir. Site haritası verileri Web denetimleri SiteMapDataSource kullanarak diğer veri bağlanabilir ve aracılığıyla programlı olarak erişilebilir `SiteMap` sınıfı.

Gezinti denetimlerinin ve Site Haritası framework kapsamlı bir tartışma Bu öğretici seri kapsamında olduğundan, bunun yerine kendi gezinme kullanıcı arabirimi şimdi hazırlayın süre beklemesini daha yerine kullanılanla ödünç my  *[ ASP.NET 2.0 verilerle çalışma](../../data-access/index.md)*  öğretici serisi, Şekil 4'te gösterildiği gibi Gezinti bağlantıları iki derin madde işaretli bir listesini görüntülemek için bir yineleyici denetimi kullanır.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Sol sütunda bağlantılar iki düzeyli listesi ekleme

Bu arabirim oluşturmak için aşağıdaki bildirim temelli biçimlendirme eklemek `Site.master` ana sayfa sütun sol burada Yapılacaklar metin: menü buraya yazılacak... şu anda bulunduğu.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

Yukarıdaki biçimlendirme adında bir yineleyici denetimi bağlar `menu` bir SiteMapDataSource için tanımlanan site haritası hiyerarşisi dönen `Web.sitemap`. SiteMapDataSource denetimin itibaren [ `ShowStartingNode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) başlatır giriş düğüm alt ile başlayarak site haritanın hiyerarşi döndürme False olarak ayarlanmış. Yineleyici bu düğümler (şu anda yalnızca Üyelik) görüntüler bir `<li>` öğesi. Başka bir, iç yineleyici geçerli düğümün alt sonra içinde iç içe geçmiş sırasız bir listesini görüntüler.

Şekil 4 yukarıdaki biçimlendirme 's işlenmiş çıkış 2. adımda oluşturduğumuz site haritası yapıda gösterir. Yineleyici temel alınan sırasız liste biçimlendirme oluşturur; geçişli stil sayfası kuralları tanımlanan `Styles.css` aesthetically güzel düzenini sorumludur. Yukarıdaki biçimlendirme nasıl çalıştığını daha ayrıntılı açıklaması için bkz [ana sayfalar ve Site gezintisi](https://asp.net/learn/data-access/tutorial-03-vb.aspx) Öğreticisi.


[![Gezinme kullanıcı işlenmiş kullanarak iç içe geçmiş sırasız listeler arabirimidir](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Şekil 4**: Gezinme kullanıcı arabirimidir çizilir iç içe geçmiş sırasız listeleri kullanma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>İçerik haritası Gezinti ekleme

Sol sütunda bağlantılar listesine ek olarak, şimdi her sayfa görüntüsü de bir [içerik haritası](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Bir içerik haritası hızlı bir şekilde kullanıcıların site hiyerarşisi geçerli konumlarını gösteren bir gezinme kullanıcı arabirimi öğedir. ASP Site Haritası çerçevesi site haritası geçerli sayfanın konumunu belirlemek için kullanır ve bu bilgilere dayalı bir içerik haritası görüntüler.

Özellikle, eklemeniz bir `<span>` ana sayfanın üst öğesi `<div>` öğesi ve yeni küme `<span>` öğenin `class` özniteliği için içerik haritası. ( `Styles.css` Sınıfı, bir içerik haritası sınıf için bir kuralı içerir.) Ardından, bir SiteMapPath için bu yeni Ekle `<span>` öğesi.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Şekil 5 ziyaret eden SiteMapPath çıktısını gösterir `~/Membership/CreatingUserAccounts.aspx`.


[![Geçerli sayfa içerik haritası görüntüler ve alt öğelerinden sitedeki eşleyin](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Şekil 5**: içerik haritası geçerli sayfa ve onun üst öğelerinin Site Haritası görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>4. adım: özel asıl ve kimlik mantığını kaldırma

İçinde  *<a id="_msoanchor_7"> </a> [Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*  kimliği doğrulanmış kullanıcı için özel asıl ve kimlik nesneleri ilişkilendirmek nasıl gördüğümüz öğretici. Biz bir olay işleyicisi oluşturarak elde `Global.asax` uygulamanın için `PostAuthenticateRequest` sonra ateşlenir olay `FormsAuthenticationModule` kullanıcı kimliğini doğrulamasından. Bu olay işleyicisi biz yerini `GenericPrincipal` ve `FormsIdentity` tarafından eklenen nesneler `FormsAuthenticationModule` ile `CustomPrincipal` ve `CustomIdentity` Bu öğreticide oluşturduğumuz nesneleri.

Özel asıl ve kimlik nesneleri çoğu durumda belirli senaryolarda yararlı durumdayken `GenericPrincipal` ve `FormsIdentity` nesneleri yeterli. Sonuç olarak, varsayılan davranışa geri dönmek için faydalı düşünüyorum. Bu değişiklik kaldırarak veya çıkışı yorum `PostAuthenticateRequest` olay işleyicisi veya silerek `Global.asax` tamamen dosya.

> [!NOTE]
> Geçersiz kılınan veya kodda kaldırılan sonra `Global.asax`, kod çıkışı açıklama gerekecek `Default.aspx's` çevirir arka plandaki kod sınıfı `User.Identity` özelliğine bir `CustomIdentity` örneği.


## <a name="step-5-programmatically-creating-a-new-user"></a>5. adım: Program aracılığıyla yeni bir kullanıcı oluşturma

Üyelik framework kullanılarak yeni bir kullanıcı hesabı oluşturmak için `Membership` sınıfının [ `CreateUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Bu yöntem giriş parametreleri için kullanıcı adı, parola ve diğer kullanıcı ilgili alanları. Çağrısı, yeni kullanıcı hesabı oluşturma yapılandırılmış üyelik sağlayıcısı atar ve ardından döndürür bir [ `MembershipUser` nesne](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) yeni oluşturulan kullanıcı hesabını temsil eden.

`CreateUser` Yöntemi her farklı sayıda giriş parametreleri kabul dört aşırı sahiptir:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Bu dört aşırı toplanan bilgilerin miktarı farklılık gösterir. İkinci bir kullanıcının e-posta adresini de gerektirir ancak ilk aşırı Örneğin, yalnızca kullanıcı adı ve parola yeni kullanıcı hesabı için gerektirir.

Üyelik sağlayıcısının yapılandırma ayarlarıyla yeni bir kullanıcı hesabı oluşturmak için gereken bilgileri bağımlı olduğundan, bu aşırı mevcut. İçinde  *<a id="_msoanchor_8"> </a> [SQL Server üyelik şema oluşturma](creating-the-membership-schema-in-sql-server-vb.md)*  biz incelenmesi belirten üyelik sağlayıcısı yapılandırma ayarlarında Öğreticisi `Web.config`. Tablo 2 yapılandırma ayarlarının tam listesi dahil.

Bir tür üyelik sağlayıcısı yapılandırma ayarı ne etkiler `CreateUser` aşırı kullanılabilir olduğu `requiresQuestionAndAnswer` ayarı. Varsa `requiresQuestionAndAnswer` ayarlanır `true` (varsayılan), sonra da yeni bir kullanıcı hesabı oluştururken size bir güvenlik sorusu ve yanıtı belirtmeniz gerekir. Kullanıcının sıfırlama veya parolasını değiştirmek gerekirse bu bilgiler daha sonra kullanılır. Özellikle, o anda Güvenlik sorusu gösterilen ve sıfırlama veya parolasını değiştirmek için doğru yanıt girmeniz gerekir. Sonuç olarak, varsa `requiresQuestionAndAnswer` ayarlanır `true` ya da ilk iki çağırma sonra `CreateUser` Güvenlik sorusu ve yanıtı eksik olduğu için bir özel durum sonuçlarında overloads. Uygulamamız şu anda bir güvenlik sorusu ve yanıtı gerektirecek şekilde yapılandırılmış olduğundan, biz ikinci iki aşırı birini kullanıcının program aracılığıyla oluştururken kullanmanız gerekecektir.

Kullanarak göstermeye `CreateUser` yöntemi, bir kullanıcı arabirimi burada biz kullanıcıdan kendi adı, parola, e-posta ve önceden tanımlanmış güvenlik yanıtını oluşturalım. Açık `CreatingUserAccounts.aspx` sayfasındaki `Membership` klasörü ve aşağıdaki Web denetimlerini içerik denetimine ekleyin:

- Adlı bir metin kutusu`Username`
- Adlı bir metin kutusu `Password`, `TextMode` özelliği ayarlanmış`Password`
- Adlı bir metin kutusu`Email`
- Adlı bir etiket `SecurityQuestion` ile kendi `Text` özelliği temizlenmiş
- Adlı bir metin kutusu`SecurityAnswer`
- Adlı bir düğme `CreateAccountButton` , `Text` özelliği, kullanıcı hesabı oluşturma için ayarlanır
- Adlı bir etiket denetimi `CreateAccountResults` ile kendi `Text` özelliği temizlenmiş

Bu noktada, ekranınızın Şekil 6'gösterilen ekran benzer görünmelidir.


[![CreatingUserAccounts.aspx sayfasına çeşitli Web denetimleri ekleme](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Şekil 6**: çeşitli Web denetimleri ekleme `CreatingUserAccounts.aspx Page` ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image18.png))


`SecurityQuestion` Etiket ve `SecurityAnswer` TextBox önceden tanımlanmış Güvenlik sorusu görüntülemek ve kullanıcının yanıt toplamak için yöneliktir. Her bir kullanıcının kendi Güvenlik sorusu tanımlamak mümkündür Güvenlik sorusu ve yanıtı bir kullanıcı tarafından temelinde saklanır, böylece olduğunu unutmayın. Ancak, ı karar bir Evrensel güvenlik sorusu öğesine kullanmak bu örneğin: en sevdiğiniz renk nedir?

Bu önceden tanımlanmış Güvenlik sorusu uygulamak için sayfanın arka plan kodu sınıfa adlı bir sabit eklemek `passwordQuestion`, Güvenlik sorusu atama. Ardından `Page_Load` olay işleyicisi, Ata bu sabit `SecurityQuestion` etiketin `Text` özelliği:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Ardından, bir olay işleyicisi oluşturun `CreateAccountButton'` s `Click` olay ve aşağıdaki kodu ekleyin:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

`Click` Olay işleyicisini başlatır adlı bir değişkende tanımlayarak `createStatus` türü [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus`durumunu gösteren bir numaralandırma `CreateUser` işlemi. Örneğin, kullanıcı hesabının başarıyla, elde edilen oluşturulursa `MembershipCreateStatus` örneği değerine ayarlanacak `Success;` aynı kullanıcı adına sahip bir kullanıcı zaten mevcut olduğundan işlem başarısız olursa, diğer yandan, bu değerineayarlanır`DuplicateUserName`. İçinde `CreateUser` kullanırız aşırı ihtiyacımız geçirmek bir `MembershipCreateStatus` yöntemi örneğine. Bu parametre içinde uygun değere ayarlanır `CreateUser` yöntemi ve biz inceleyin değerini kullanıcı hesabının başarıyla oluşturulup oluşturulmadığını belirlemek için yöntem çağrısı sonra.

Çağırdıktan sonra `CreateUser`, içinde geçen `createStatus`, `Select Case` deyimi atanan değerine bağlı olarak uygun bir mesaj çıktısını almak için kullanılan `createStatus`. Şekil 7, yeni bir kullanıcı başarıyla oluşturulduğunda çıkış gösterir. Kullanıcı hesabı oluşturulmaz, Şekil 8 ve 9 çıktıyı göster. Şekil 8'de ziyaretçi üyelik sağlayıcısının yapılandırma ayarlarında yazıyla parola gücü gereksinimlerini karşılamıyor beş harfli parola girdiniz. Şekil 9'da, var olan bir kullanıcı adı (Şekil 7'de oluşturulan bir) olan bir kullanıcı hesabı oluşturmak ziyaretçi deniyor.


[![Yeni bir kullanıcı hesabı başarıyla oluşturulmuştur](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Şekil 7**: A yeni kullanıcı hesabı olan başarıyla oluşturuldu ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image21.png))


[![Sağlanan parola çok zayıf olduğu için kullanıcı hesabı oluşturulmaz.](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Şekil 8**: Sağlanan parola çok zayıf olduğu için kullanıcı hesabı oluşturulmaz ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image24.png))


[![Kullanıcı hesabı değil oluşturulan kullanıcı adı zaten kullanımda olduğundan ise](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Şekil 9**: kullanıcı hesabı ise değil oluşturulan kullanıcı adı çünkü zaten kullanılıyor ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> İlk iki birini kullanırken, başarı veya başarısızlığı belirlemek nasıl merak ediyor olabilirsiniz `CreateUser` yöntemi aşırı yüklemeleri, tipleri türünde bir parametre olan, `MembershipCreateStatus`. Bu ilk iki aşırı throw bir [ `MembershipCreateUserException` özel durum](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) bir hata karşısında içeren bir [ `StatusCode` özelliği](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) türü `MembershipCreateStatus`.


Birkaç kullanıcı hesabı oluşturduktan sonra hesapları içeriğini listeleyerek oluşturulmuş doğrulayın `aspnet_Users` ve `aspnet_Membership` tablolar `SecurityTutorials.mdf` veritabanı. Şekil 10 gösterildiği gibi iki kullanıcı aracılığıyla eklediğiniz `CreatingUserAccounts.aspx` sayfa: Tito ve Bruce'a.


[![Üyelik kullanıcı deposunda iki kullanıcı vardır: Tito ve Bruce'a](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Şekil 10**: üyeliği kullanıcı deposunda iki kullanıcı vardır: Tito ve Bruce'a ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image30.png))


Üyelik kullanıcı deposunda artık Bruce'a ve Tito'nın hesap bilgilerini içerir, ancak Bruce'a veya Tito sitesinde oturum açmaya olanak tanır işlevselliği uygulamak henüz. Şu anda `Login.aspx` kullanıcının kimlik bilgilerini doğrular bir sabit kodlanmış kullanıcı adı/parola çiftleri kümesini - karşı mevcut *değil* üyelik framework karşı sağlanan kimlik bilgilerini doğrulayın. Yeni kullanıcı hesapları artık görmek için `aspnet_Users` ve `aspnet_Membership` tabloları yeterli olacaktır. Sonraki öğreticide  *<a id="_msoanchor_9"> </a> [doğrulama kullanıcı kimlik bilgilerini karşı üyeliği kullanıcı depolamak](validating-user-credentials-against-the-membership-user-store-vb.md)*, üyelik depo doğrulamak için oturum açma sayfasına güncelleştireceğiz.

> [!NOTE]
> Tüm kullanıcılar görmüyorsanız, `SecurityTutorials.mdf` veritabanı, web uygulamanızın varsayılan üyelik sağlayıcısı kullandığından olabilir `AspNetSqlMembershipProvider`, kullanan `ASPNETDB.mdf` veritabanı kendi kullanıcı deposu olarak. Sorunun bu olup olmadığını belirlemek için Çözüm Gezgini'nde Yenile düğmesini tıklatın. Adlı bir veritabanı varsa `ASPNETDB.mdf` eklendi `App_Data` klasörü, sorunun bu olup. Döndürmek için adım 4  *<a id="_msoanchor_10"> </a> [SQL Server üyelik şema oluşturma](creating-the-membership-schema-in-sql-server-vb.md)*  üyelik sağlayıcısı düzgün şekilde yapılandırma hakkında yönergeler için Öğreticisi.


Çoğu kullanıcı hesabı senaryoları oluşturmanıza, ziyaretçi kullanıcı adı, parola, e-posta ve bu noktada yeni bir hesap oluşturulan diğer önemli bilgiler girmek için bazı arabirimiyle sunulur. Bu adımda biz böyle bir arabirim el ile oluşturmayı arama ve nasıl kullanılacağını gördünüz `Membership.CreateUser` program aracılığıyla yeni kullanıcı hesabı eklemek için yöntemine kullanıcının girişler için temel. Yeni kullanıcı hesabı oluşturduğunuz kodumuza, ancak. Kullanıcıyı siteye yeni oluşturulan kullanıcı hesabı altında oturum veya kullanıcıya bir onay e-posta gönderme gibi eylemler, tüm izleme gerçekleştirmedi. Aşağıdaki ek adımları düğmenin ek kodda gerektirecek `Click` olay işleyicisi.

ASP.NET üyelik Framework'te hesabı oluşturma ve sonrası hesap gerçekleştirmek için yeni kullanıcı hesapları oluşturmak için bir kullanıcı arabirimi oluşturma gelen kullanıcı hesabı oluşturma işlemi, işlemek üzere tasarlanmış CreateUserWizard denetimi ile birlikte gelir bir onay e-posta göndermek ve yeni oluşturulan kullanıcı siteye oturum gibi oluşturma görevleri. CreateUserWizard denetimini kullanarak bir sayfaya araç CreateUserWizard denetimini sürükleyerek ve birkaç özelliklerini ayarlama olarak basit bir işlemdir. Çoğu durumda, tek satırlık bir kod yazmaya gerek yoktur. Biz bu çok denetim ayrıntılı adım 6'daki inceleyeceksiniz.

Yeni kullanıcı hesapları normal bir hesap oluşturma web sayfası yalnızca oluşturulursa, herhangi bir zamanda kullanan kod yazmaya gerektiğine olası değil `CreateUser` yöntemi CreateUserWizard denetim olasılıkla gereksinimlerinizi. Ancak, `CreateUser` yöntemdir senaryolarda kullanışlı bir yüksek oranda özelleştirilmiş hesap oluştur kullanıcı deneyimi ihtiyaç duyacağınız veya program aracılığıyla bir alternatif arabirimi aracılığıyla yeni kullanıcı hesapları oluşturma gerektiğinde. Örneğin, bir kullanıcının başka bir uygulamanın kullanıcı bilgilerini içeren bir XML dosyasını karşıya yüklemek bir sayfa olabilir. Karşıya yüklenen XML içeriğini dosya ve çağırarak XML'de temsil edilen her kullanıcı için yeni bir hesap oluşturun ayrıştırıyor sayfa olabilir `CreateUser` yöntemi.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>6. adım: yeni bir kullanıcı ile CreateUserWizard denetimi oluşturma

ASP.NET oturum açma Web denetimleri sayısıyla birlikte gönderilir. Bu denetimler birçok ortak kullanıcı hesabı ve oturum açma ilgili senaryolarda yardımcı. [CreateUserWizard denetim](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) üyelik framework yeni bir kullanıcı hesabı eklemek için bir kullanıcı arabirimi sunmak için tasarlanmış olan bir denetim.

Birçok diğer oturum açma ile ilgili Web denetimleri gibi tek satırlık bir kod yazmak zorunda kalmadan CreateUserWizard kullanılabilir. Sezgisel üyelik sağlayıcısının yapılandırma ayarlarını ve çağrıları dahili dayalı bir kullanıcı arabirimi sağlar `Membership` sınıfının `CreateUser` kullanıcı gerekli bilgileri girer ve kullanıcı Oluştur düğmesine tıklar sonra yöntemi. Son derece özelleştirilebilir CreateUserWizard denetimdir. Bir ana bilgisayar hesabı oluşturma işlemi çeşitli aşamalarında yangın olayların vardır. Olay işleyicileri, hesap oluşturma akışına özel mantık eklemesine gerektiği gibi oluşturabilirsiniz. Ayrıca, CreateUserWizard'ın görünümü çok daha esnektir. Varsayılan arabirim görünümünü tanımlayan özellikleri vardır; Gerekirse, denetim bir şablona dönüştürülebilir veya ek kullanıcı kayıt adımları eklenebilir.

CreateUserWizard denetimin varsayılan arabirimi ve davranışını kullanarak bir bakış ile başlayalım tıklatın. Biz, ardından denetim özellikleri ve olayları aracılığıyla görünümünü özelleştirmek nasıl ele alacağız.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>CreateUserWizard'ın varsayılan arabirimi ve davranış inceleniyor

Geri dönüp `CreatingUserAccounts.aspx` sayfasındaki `Membership` klasörü, tasarım veya bölünmüş moduna geç ve daha sonra CreateUserWizard denetimini sayfanın üst kısmına ekleyin. CreateUserWizard Denetim Araç Kutusu'nın oturum açma denetimleri bölümü altında kaydedildi. Denetim ekledikten sonra ayarlamak kendi `ID` özelliğine `RegisterUser`. Şekil 11 programlarını ekran görüntüsü gibi yeni kullanıcının kullanıcı adı, parola, e-posta adresi ve Güvenlik sorusu ve yanıtı için metin kutuları arabirimiyle CreateUserWizard işler.


[![CreateUserWizard denetim işler genel kullanıcı arabirimi oluşturma](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Şekil 11**: genel bir oluşturma kullanıcı arabirimi CreateUserWizard denetimini işler ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image33.png))


Şimdi CreateUserWizard denetimini adım 5'te oluşturduğumuz arabirimi tarafından oluşturulan varsayılan kullanıcı arabirimi karşılaştırmak için bir dakikanızı ayırın. Yeni başlayanlar, önceden tanımlanmış Güvenlik sorusu el ile oluşturulan arabirimimizi kullanılan ancak CreateUserWizard denetimi Güvenlik sorusu ve yanıtı, belirtmek ziyaretçi sağlar. Biz henüz doğrulama bizim arabiriminin form alanlarını uygulamak gerekiyordu ancak CreateUserWizard denetimin arabirimi doğrulama denetimleri de içerir. Ve CreateUserWizard denetim arabirimi (birlikte metin girilen parola ve parola karşılaştırma metin kutuları eşit olduğundan emin olun CompareValidator) parolayı onayla textbox içerir.

Ne ilginç CreateUserWizard denetim kullanıcı arabirimiyle işlenirken üyelik sağlayıcısının yapılandırma ayarlarını danışır olmasıdır. Örneğin, Güvenlik sorusu ve yanıtı kutularındaki yalnızca görüntülenir `requiresQuestionAndAnswer` True olarak ayarlayın. Benzer şekilde, CreateUserWizard parola gücü gereksinimlerini karşılıyor ve ayarlar sağlamak için bir RegularExpressionValidator denetimi otomatik olarak ekler, `ErrorMessage` ve `ValidationExpression` özelliklerini temel alarak `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`ve `passwordStrengthRegularExpression` yapılandırma ayarları.

Adından da anlaşılacağı gibi CreateUserWizard denetim türetildiği [sihirbaz denetimi](https://msdn.microsoft.com/library/s2etd1ek.aspx). Sihirbazı denetimleri, çok adımlı görevleri tamamlamak için bir arabirim sağlamak üzere tasarlanmıştır. Rastgele sayıda bir sihirbaz denetiminiz `WizardSteps`, her biri HTML tanımlayan bir şablondur ve Web denetimleri için bu adımı. Sihirbaz Denetimi başlangıçta ilk görüntüler `WizardStep`, sonraki bir adımda devam ya da önceki adımlarına geri dönmek için yönetebileceklerini Gezinti denetimlerle birlikte.

Şekil 11 bildirim temelli biçimlendirmede gösterildiği gibi CreateUserWizard denetimin varsayılan arabirim iki içerir `WizardStep` : %s

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? Yeni kullanıcı hesabı oluşturma hakkında bilgi toplamak için arabirimi işler. Bu, Şekil 11'de gösterilen adımdır.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? hesabın başarıyla oluşturulduğunu belirten bir ileti oluşturur.

Bu adımları ya da şablonlara dönüştürme ya da kendi ekleyerek CreateUserWizard'ın görünümünü ve davranışını değiştirilebilir `WizardStep` s. Biz ekleme adresindeki görüneceğini bir `WizardStep` kayıt arabirimi ile *ek kullanıcı bilgilerini depolamak* Öğreticisi.

Eylem CreateUserWizard denetiminde görelim. Ziyaret `CreatingUserAccounts.aspx` bir tarayıcı aracılığıyla sayfası. CreateUserWizard'ın arabirimine bazı geçersiz değerler girerek başlayın. Deneyin parola gücünü gereksinimlerine uygun değil bir parola girme ya da kullanıcı adı metin kutusu boş bırakın. CreateUserWizard uygun bir hata iletisi görüntüler. Şekil 12 çıkış yeterince güçlü bir parola ile bir kullanıcı oluşturmaya çalışırken gösterir.


[![CreateUserWizard doğrulama denetimleri otomatik olarak yerleştirir.](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Şekil 12**: CreateUserWizard otomatik olarak yerleştirir doğrulama denetimleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image36.png))


Ardından, CreateUserWizard uygun değerleri girin ve kullanıcı Oluştur düğmesine tıklayın. Gerekli alanları girilen ve parolanın gücü yeterli olduğunu varsayarsak, CreateUserWizard üyelik çerçevesi aracılığıyla yeni bir kullanıcı hesabı oluşturur ve ardından görüntülemek `CompleteWizardStep`kullanıcının arabirim (bkz. Şekil 13). Arka planda CreateUserWizard çağırır `Membership.CreateUser` adım 5'te komutlarında yaptığımız gibi yöntemi.


[![Yeni bir kullanıcı hesabı başarıyla oluşturuldu sahip](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Şekil 13**: A yeni kullanıcı hesabına sahip başarıyla oluşturuldu ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> Şekil 13 gösterildiği gibi `CompleteWizardStep`'s arabirimi devam düğmesi içerir. Ancak, bu noktada yalnızca tıklatarak ziyaretçi aynı sayfa üzerinde bırakarak geri gönderme gerçekleştirir. Özelleştirme CreateUserWizard'ın görünümünü ve davranışını aracılığıyla ait özellikler bölümünde biz ziyaretçiye göndermek bu düğmenin nasıl olabilir adresindeki görüneceğini `Default.aspx` (veya başka bir sayfaya).


Yeni bir kullanıcı hesabı oluşturduktan sonra Visual Studio'ya geri dönün ve inceleyin `aspnet_Users` ve `aspnet_Membership` hesabın başarıyla oluşturulduğunu doğrulamak için Şekil 10'yaptığımız gibi tabloları.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>CreateUserWizard'ın davranış ve görünümünü özelliklerini aracılığıyla özelleştirme

CreateUserWizard çeşitli şekillerde özellikler üzerinden özelleştirilebilir `WizardStep` s ve olay işleyicileri. Bu bölümde özelliklerini aracılığıyla denetimin görünümünü özelleştirmek nasıl ele alacağız; sonraki bölümde, olay işleyicileri aracılığıyla denetimin davranışını genişletme sırasında arar.

Neredeyse tüm CreateUserWizard denetimin varsayılan kullanıcı arabiriminde görüntülenen metni özellikleri kendi sayısız özelleştirilebilir. Örneğin, metin kutuları solunda görüntülenen kullanıcı adı, parola, parolayı onaylayın, e-posta, Güvenlik sorusu ve güvenlik yanıtı etiketleri tarafından özelleştirilebilir [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), ve [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) Özellikler, sırasıyla. Benzer şekilde, kullanıcı oluştur ve devam et düğme metni belirtmek için özellikler yok `CreateUserWizardStep` ve `CompleteWizardStep`, bu düğmeleri düğmeler, LinkButtons veya ImageButtons da işlendiğini olur.

Renkleri, kenarlıklar, yazı tipleri ve diğer görsel öğelere stil özellikleri ana bilgisayar üzerinden yapılandırılabilir. Ortak Web denetimi stil özellikleri - CreateUserWizard denetim sahip `BackColor`, `BorderStyle`, `CssClass`, `Font`, vb. - ve stil özelliklerini belirli bölümlerini görünümünü tanımlamak için bir dizi vardır CreateUserWizard'ın arabirimi. [ `TextBoxStyle` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), örneğin kutularındaki için stiller tanımlar `CreateUserWizardStep`, sırada [ `TitleTextStyle` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) (Kaydol bilgisayarınızı yeni için başlık stilini tanımlar Hesap için).

Görünüm ilgili özellikler ek olarak bir dizi CreateUserWizard denetimin davranışını etkilemek özellikleri vardır. [ `DisplayCancelButton` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), İptal düğmesi (varsayılan değeri değeri: False) kullanıcı Oluştur düğmesine yanındaki kümesi için doğru görüntüler. İptal düğmesi görüntülerseniz de ayarladığınızdan emin olun [ `CancelDestinationPageUrl` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), kullanıcı gönderilir için İptal'i tıklattıktan sonra sayfa belirtir. Devam Et düğmesine önceki bölümünde belirtildiği gibi `CompleteWizardStep`'s arabirimi geri gönderimin neden olur, ancak aynı sayfa üzerinde ziyaretçi bırakır. Devam Et düğmesine tıkladıktan sonra başka bir sayfaya ziyaretçi göndermek için yalnızca URL'yi belirtin [ `ContinueDestinationPageUrl` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Şimdi güncelleştirme `RegisterUser` CreateUserWizard denetimi iptal düğmesi göstermek için ve ziyaretçi göndermek için `Default.aspx` iptal veya devam düğmeleri tıklandığında. Bunu gerçekleştirmek için ayarlanmış `DisplayCancelButton` özelliğini True ve her ikisi de `CancelDestinationPageUrl` ve `ContinueDestinationPageUrl` özelliklerine ~ / Default.aspx. Şekil 14 bir tarayıcıdan görüntülendiğinde güncelleştirilmiş CreateUserWizard gösterir.


[![İptal düğmesi CreateUserWizardStep'e içerir](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Şekil 14**: `CreateUserWizardStep` iptal düğmesi içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image42.png))


Bir ziyaretçi bir kullanıcı adı, parola, e-posta adresi ve Güvenlik sorusu ve yanıtı girer ve kullanıcı oluştur tıkladığında yeni bir kullanıcı hesabı oluşturulur ve ziyaretçi bu yeni oluşturulan kullanıcı olarak günlüğe kaydedilir. Sayfasını ziyaret kişi kendileri için yeni bir hesap oluşturuyor varsayılarak, büyük olasılıkla istenen davranış budur. Ancak, yeni kullanıcı hesapları eklemek Yöneticiler izin vermek isteyebilirsiniz. Bunun yapılması, kullanıcı hesabı oluşturulacak, ancak bir yönetici olarak (ve yeni oluşturulan hesaba olarak değil) yönetici olarak oturum açmış kalır. Bu davranış Boolean değiştirilebilir [ `LoginCreatedUser` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Kullanıcı hesaplarını üyelik Framework'te onaylanmış bir bayrağı içeren; onaylanmamış kullanıcılar siteye oturum açamıyor. Varsayılan olarak, yeni oluşturulan bir hesapla onaylı, siteye hemen oturum açmaya izin verme olarak işaretlenir. Ancak, yeni kullanıcı hesapları onaylanmamış olarak işaretlenmiş olması, mümkündür. Belki de, yönetici yeni kullanıcılar oturum önce el ile onaylama istediğiniz; veya, bir kullanıcının oturum açmasını sorgulamasına önce kayıt sırasında girilen e-posta adresi geçerli olduğunu doğrulamak istediğiniz olabilir. Her durumda olabilir, onaylanmamış işaretlenmiş CreateUserWizard denetimin ayarlayarak yeni oluşturulan kullanıcı hesabı olabilir [ `DisableCreatedUser` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) true (varsayılan değeri: False).

Not davranışı ilgili diğer özellikleri içerir `AutoGeneratePassword` ve `MailDefinition`. Varsa [ `AutoGeneratePassword` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) True olarak ayarlandığında `CreateUserWizardStep` parola ve Parolayı Onayla kutularındaki; görüntülemez bunun yerine, yeni oluşturulan kullanıcı parolasının otomatik olarak kullanılarak oluşturulan `Membership` sınıfının [ `GeneratePassword` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). `GeneratePassword` Yöntemi belirtilen uzunluğu ve alfasayısal olmayan karakter yapılandırılmış parola gücü gereksinimlerini karşılamak için yeterli sayıda ile bir parola oluşturur.

[ `MailDefinition` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) hesap oluşturma işlemi sırasında belirtilen e-posta adresine bir e-posta göndermek istiyorsanız kullanışlıdır. `MailDefinition` Alt oluşturulan e-posta iletisi hakkında bilgi tanımlamak için bir dizi özellik içerir. Bu alt özellikler gibi seçeneklerini kapsar `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, ve `BodyFileName`. [ `BodyFileName` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) işaret eden bir metin veya e-posta ileti gövdesini içeren HTML dosyası. Gövde iki önceden tanımlanmış yer tutucuları destekler: `<%UserName%>` ve `<%Password%>`. Varsa, bu yer tutucuları `BodyFileName` dosya, yeni oluşturulan kullanıcı adı ve parola ile değiştirilecek.

> [!NOTE]
> `CreateUserWizard` Denetimin `MailDefinition` özelliği yalnızca yeni bir hesap oluşturulduğunda gönderilen e-posta iletisi ayrıntılarını belirtir. E-posta iletisi gerçekte nasıl gönderileceğini üzerinde herhangi bir ayrıntıyı içermez (diğer bir deyişle, bir SMTP sunucusu veya posta bırakma dizini kullanılıp kullanılmadığını, hiçbir kimlik doğrulama bilgileri vb.). Bu alt düzey ayrıntıların tanımlanması gerek `<system.net>` bölümüne `Web.config`. Bu yapılandırma ayarları ve genel ASP.NET 2. 0 ' e-posta gönderme hakkında daha fazla bilgi için bkz [SystemNetMail.com en sık sorulan sorular](http://www.systemnetmail.com/) ve my makale [, ASP.NET 2.0 e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Olay işleyicilerini kullanarak CreateUserWizard'ın davranış genişletme

CreateUserWizard denetim olayların sayısı, iş akışı sırasında başlatır. Örneğin, bir ziyaretçi kullanıcı adı, parola ve diğer ilgili bilgileri girer ve kullanıcı Oluştur düğmesine tıklar sonra CreateUserWizard denetim başlatır, [ `CreatingUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Oluşturma işlemi sırasında bir sorun varsa [ `CreateUserError` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) tetiklenir; ancak, kullanıcı başarıyla oluşturulduysa, sonra [ `CreatedUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) tetiklenir. Yükseltilmiş ek CreateUserWizard denetim olaylarını vardır, ancak bunlar üç en başlığıyla ilgili olanlardır.

Bazı senaryolarda biz uygun olayı için bir olay işleyicisi oluşturarak yapabiliriz iş akışına CreateUserWizard dokunun isteyebilirsiniz. Bunu göstermek için şimdi geliştirmek `RegisterUser` CreateUserWizard denetim bazı özel bir kullanıcı adı ve parola doğrulamasını içerir. Özellikle, şimdi bizim CreateUserWizard böylece kullanıcı adları başında veya sonunda boşluk içeremez ve kullanıcı adı herhangi bir yere Parolada bulunamaz geliştirin. Kısacası, biz "Tan" gibi bir kullanıcı adı oluşturma veya Scott ve Scott.1234 gibi bir kullanıcı adı/parola bileşimi HAVING birinden önlemek isteyebilirsiniz.

İçin bir olay işleyicisi oluşturur biz bunu gerçekleştirmek için `CreatingUser` bizim ek doğrulama denetimlerini gerçekleştiren olay. Sağlanan veri geçerli değilse oluşturma işlemini iptal etmeniz gerekir. Biz de kullanıcı adı veya parola geçersiz olduğunu belirten bir ileti görüntüler sayfasına bir etiket Web denetimi eklemeniz gerekir. Etiket denetimi ayarı CreateUserWizard denetim altına eklemeye başlayın kendi `ID` özelliğine `InvalidUserNameOrPasswordMessage` ve kendi `ForeColor` özelliğine `Red`. Temizle kendi `Text` özelliği ve kümesi kendi `EnableViewState` ve `Visible` özellikleri False.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Ardından, CreateUserWizard denetim için bir olay işleyicisi oluşturun `CreatingUser` olay. Olay işleyici oluşturmak için Tasarımcısı'nda denetimi seçin ve Özellikler penceresine gidin. Buradan Şimşek Cıvata simgesine tıklayın ve olay işleyicisi oluşturmak için uygun olayı çift tıklatın.

Aşağıdaki kodu ekleyin `CreatingUser` olay işleyicisi:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Kullanıcı adı ve parola CreateUserWizard denetime girilen aracılığıyla kullanılabilir olduğuna dikkat edin, [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) ve [ `Password` özellikleri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)sırasıyla. Bu özellikler yukarıdaki olay işleyicisini sağlanan kullanıcı baştaki veya sondaki boşlukları içerip içermediğini ve kullanıcı adı parola içinde bulunup belirlemek için kullanırız. Bu koşullardan biri karşılanırsa bir hata iletisi görüntülenir `InvalidUserNameOrPasswordMessage` etiket ve olay işleyicinin `e.Cancel` özelliği ayarlanmış `True`. Varsa `e.Cancel` ayarlanır `True`, etkili bir şekilde kullanıcı hesabı oluşturma işlemi iptal ediliyor, iş akışı CreateUserWizard short-circuits.

Şekil 15 gösteren ekran görüntüsü `CreatingUserAccounts.aspx` kullanıcı öndeki boşlukları ile bir kullanıcı adı girdiğinde.


[![Başında veya sonunda boşluk ile kullanıcı adlarını izin verilmiyor](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Şekil 15**: başında veya sonunda boşluk ile kullanıcı adlarını izin verilmiyor ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> CreateUserWizard denetimin kullanma örneği göreceğiz `CreatedUser` olayında  *<a id="_msoanchor_11"> </a> [ek kullanıcı bilgilerini depolamak](storing-additional-user-information-vb.md)*  Öğreticisi.


## <a name="summary"></a>Özet

`Membership` Sınıfının `CreateUser` yöntemi, üyelik Framework'te yeni bir kullanıcı hesabı oluşturur. Bunu yapılandırılmış üyelik sağlayıcısı çağrısı için temsilci seçme yapar. Durumunda `SqlMembershipProvider`, `CreateUser` yöntemi, bir kayıt ekler `aspnet_Users` ve `aspnet_Membership` veritabanı tablolarını.

Program aracılığıyla (adım 5'te gördüğümüz gibi) yeni kullanıcı hesapları oluşturulabilir olsa da, daha hızlı ve kolay bir yaklaşım CreateUserWizard denetim kullanmaktır. Bu denetim, kullanıcı bilgilerini toplama ve üyelik Framework'te yeni bir kullanıcı oluşturmak için çok adımlı kullanıcı arabirimi işler. Bu denetimi aynı kapsar altında kullanır `Membership.CreateUser` yöntemini 5. adım, ancak denetim incelenmesi gibi kullanıcı arabirimi, doğrulama denetimleri, oluşturur ve lick kod yazmak zorunda kalmadan kullanıcı hesabı oluşturma hatası için yanıt verir.

Bu noktada işlevselliği yeni kullanıcı hesapları oluşturmak yerinde sahibiz. Ancak, oturum açma sayfasına biz ikinci öğreticide belirtilen bu sabit kodlanmış kimlik bilgilerini karşı doğruluyor. İçinde <a id="_msoanchor_12"> </a> [sonraki öğretici](validating-user-credentials-against-the-membership-user-store-vb.md) güncelleştireceğiz `Login.aspx` kullanıcıyı doğrulamak için kimlik bilgilerine karşı üyelik framework sağlanan.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [`CreateUser`Teknik belgeler](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [CreateUserWizard denetimine genel bakış](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Bir dosya sistemi tabanlı Site haritası sağlayıcısı oluşturma](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [ASP.NET 2.0 Sihirbazı denetimi ile bir adım adım kullanıcı arabirimi oluşturma](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [ASP.NET 2.0 inceleniyor kullanıcının Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Ana sayfalar ve Site gezintisi](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Beklediğinden SQL Site haritası sağlayıcısı](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Teresa Murphy oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Önceki](creating-the-membership-schema-in-sql-server-vb.md)
[sonraki](validating-user-credentials-against-the-membership-user-store-vb.md)
