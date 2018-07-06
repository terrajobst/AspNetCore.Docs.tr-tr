---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Kullanıcı hesapları (VB) oluşturma | Microsoft Docs
author: rick-anderson
description: Bu öğreticide yeni kullanıcı hesapları oluşturmak için üyelik framework (aracılığıyla SqlMembershipProvider) kullanarak inceleyeceksiniz. Yeni bize oluşturma göreceğiz...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: fe5e55df3fa9f65a94199c2064a785255f231537
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815349"
---
<a name="creating-user-accounts-vb"></a>Kullanıcı hesapları (VB) oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> Bu öğreticide yeni kullanıcı hesapları oluşturmak için üyelik framework (aracılığıyla SqlMembershipProvider) kullanarak inceleyeceksiniz. Program aracılığıyla ve ASP aracılığıyla yeni kullanıcı oluşturma işlemini göreceğiz. NET yerleşik CreateUserWizard denetimi.


## <a name="introduction"></a>Giriş

İçinde <a id="_msoanchor_1"> </a> [önceki öğretici](creating-the-membership-schema-in-sql-server-vb.md) eklenen tablolar, görünümler ve saklı yordamlar için gerekli bir veritabanında uygulama hizmetleri şeması yüklediğimiz `SqlMembershipProvider` ve `SqlRoleProvider`. Bu, geri kalanında bu serideki Eğitmenleri ihtiyacımız altyapı oluşturuldu. Bu öğreticide size üyelik çerçevesini kullanarak inceleyeceksiniz (aracılığıyla `SqlMembershipProvider`) yeni kullanıcı hesapları oluşturmak için. Program aracılığıyla ve ASP aracılığıyla yeni kullanıcı oluşturma işlemini göreceğiz. NET yerleşik CreateUserWizard denetimi.

Yeni kullanıcı hesapları oluşturma işlemini öğrenme yanı sıra, biz de oluşturduğumuz ilk tanıtım Web sitesine güncelleştirmeniz gerekecektir *<a id="_msoanchor_2"> </a> [form kimlik doğrulaması bir genel bakış](../introduction/an-overview-of-forms-authentication-vb.md)* Öğretici ve ardından, Gelişmiş *<a id="_msoanchor_3"> </a> [Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* öğretici. Demo web uygulamamıza sabit kodlanmış bir kullanıcı adı/parola çiftleri karşı kullanıcıların kimlik bilgilerini doğrular bir oturum açma sayfası vardır. Ayrıca, `Global.asax` özel oluşturan kodu içerir `IPrincipal` ve `IIdentity` kimliği doğrulanmış kullanıcılar için nesneleri. Üyelik framework karşı kullanıcıların kimlik bilgilerini doğrulamak ve özel asıl ve kimlik mantığı kaldırmak için oturum açma sayfasına güncelleştireceğiz.

Haydi başlayalım!

## <a name="the-forms-authentication-and-membership-checklist"></a>Form kimlik doğrulaması ve üyelik denetim listesi

Üyelik framework ile çalışmaya başlamadan önce bu noktaya ulaşması önlemlerin önemli adımlar gözden geçirmek için bir zaman ayırabiliriz. Üyelik framework ile kullanırken `SqlMembershipProvider` bir form tabanlı kimlik doğrulaması senaryosunda aşağıdaki adımları üyelik işlevselliğini web uygulamanızda uygulama önce gerçekleştirilmesi gerekir:

1. **Form tabanlı kimlik doğrulamasını etkinleştirin.** Açıkladığımız gibi  *<a id="_msoanchor_4"> </a> [form kimlik doğrulaması bir genel bakış](../introduction/an-overview-of-forms-authentication-vb.md)*, form kimlik doğrulaması etkin düzenleyerek `Web.config` ve ayarı `<authentication>` öğenin `mode` özniteliğini `Forms`. Etkin form kimlik doğrulaması ile her gelen istek için incelenir bir *forms kimlik doğrulaması bileti*, varsa tanımlayan istek sahibi.
2. **Uygulama Hizmetleri şeması için uygun veritabanı ekleyin.** Kullanırken `SqlMembershipProvider` uygulama hizmetleri şeması veritabanına yüklememiz gerekir. Genellikle bu şema, uygulamanın veri modelini tutan aynı veritabanına eklenir. *<a id="_msoanchor_5"> </a> [SQL Server'da üyelik şeması oluşturma](creating-the-membership-schema-in-sql-server-vb.md)* öğretici baktığı kullanarak `aspnet_regsql.exe` bunu gerçekleştirmek için aracı.
3. **Adım 2 ' veritabanı başvurmak için Web uygulamasının ayarlarını özelleştirin.** *SQL Server'da üyelik şeması oluşturma* öğreticide gösterilen web uygulamasını yapılandırmanın iki yolu böylece `SqlMembershipProvider` 2. adımda seçtiğiniz veritabanı kullanırsınız: değiştirerek `LocalSqlServer` bağlantı dizesi adı; veya yeni bir kayıtlı sağlayıcı üyelik framework sağlayıcılar listesine ekleyerek ve veritabanından bu yeni sağlayıcı özelleştirme 2. adım.

Bir web uygulaması oluşturma kullandığında `SqlMembershipProvider` ve form tabanlı kimlik doğrulaması kullanmadan önce bu üç adımı gerçekleştirmeniz gerekecek `Membership` sınıfı veya ASP.NET oturum açma Web denetimleri. Zaten bu adımları önceki öğreticilerdeki gerçekleştirdiğimiz olduğundan, biz üyelik framework kullanmaya başlamak hazırsınız!

## <a name="step-1-adding-new-aspnet-pages"></a>1. adım: Yeni ASP.NET sayfaları ekleme

Bu öğretici ve sonraki üç biz çeşitli üyelik ilgili işlevleri ve özellikleri İnceleme. ASP.NET sayfaları, Bu öğretici incelenirken konuları uygulamak için bir dizi ihtiyacımız. Sayfalar ve site haritası oluşturalım `(Web.sitemap)`.

Adlı projede yeni bir klasör oluşturarak başlayın `Membership`. Ardından, beş yeni ASP.NET sayfaları ekleyin `Membership` klasörü, her bir sayfa ile bağlama `Site.master` ana sayfa. Sayfa adı:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Bu noktada, projenizin Çözüm Gezgini, Şekil 1'de gösterilen ekran şuna benzemelidir.


[![Beş yeni sayfalar üyelik klasöre eklenen](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Şekil 1**: beş yeni sayfalar eklenmiştir `Membership` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image3.png))


Her sayfanın bu noktada, iki içerik denetimlerini, her bir ana sayfanın ContentPlaceHolder biri olması gerekir: `MainContent` ve `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Bu geri çağırma `LoginContent` ContentPlaceHolder'ın varsayılan biçimlendirme oturum açamayabilir veya kullanıcının kimliği doğrulanır olup olmadığına bağlı olarak site oturumunu bağlantısı görüntülenir. Varlığını `Content2` içerik denetimi, ancak ana sayfanın varsayılan biçimlendirme geçersiz kılar. Açıkladığımız gibi *<a id="_msoanchor_6"> </a> [form kimlik doğrulaması bir genel bakış](../introduction/an-overview-of-forms-authentication-vb.md)* öğretici, bu sayfaları değil istediğimiz sol sütunda oturum açmayla ilgili seçenekleri görüntülemek için yararlıdır.

Bu beş sayfaları için ancak ana sayfa için varsayılan işaretlemesini göstermek istiyoruz `LoginContent` ContentPlaceHolder. Bu nedenle, bildirim temelli biçimlendirme için kaldırma `Content2` içerik denetimi. Bunu yaptıktan sonra her beş sayfanın biçimlendirme yalnızca bir içerik denetimi içermesi gerekir.

## <a name="step-2-creating-the-site-map"></a>2. adım: Site Haritası oluşturma

En basit Web siteleri dışındaki tüm başka gezinme kullanıcı arabirimi tür uygulamanız gerekir. Gezinti kullanıcı arabirimi, bir basit bir site çeşitli bölümlerini yönelik bağlantıların listesi olabilir. Alternatif olarak, bu bağlantıları menüler ya da ağaç görünümlerini düzenlenmiş. Sayfa geliştiricileri de gezinme kullanıcı arabirimi oluşturma hikayeyi yalnızca yarısını bağlıdır. Bazı araçlar, sitenin mantıksal yapısı sürdürülebilir ve güncelleştirilebilir bir şekilde tanımlamak için de ihtiyacımız var. Yeni sayfalar eklenir veya var olan sayfaları kaldırılması gibi - site haritası - tek bir kaynağı güncelleştirmek mümkün olmasını istiyoruz ve söz konusu değişiklikler sitenin gezinme kullanıcı arabirimi yansıtılmasını.

Bu iki görevleri - site eşlemesini tanımlayan ve site haritasına dayalı olarak bir gezinti kullanıcı arabirimini uygulayan - Site Haritası framework sayesinde kolaydır ve gezinti Web eklenen ASP.NET sürüm 2.0 denetler. Site haritasını tanımlamak bir geliştirici için Site Haritası framework sağlar ve programlı bir API aracılığıyla erişim ( [ `SiteMap` sınıfı](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Gezinti Web denetimleri yerleşik dahil bir [menü denetimi](https://msdn.microsoft.com/library/bz09dy46.aspx), [TreeView denetimi](https://msdn.microsoft.com/library/3eafky27.aspx)ve [SiteMapPath denetimi](https://msdn.microsoft.com/library/3eafky27.aspx).

Üyelik ve roller çerçeveleri gibi Site Haritası framework üzerine inşa edilmiş [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Site haritası sağlayıcısı sınıfı tarafından kullanılan bellek içi yapısı oluşturmak için iş `SiteMap` sınıfından bir XML dosyasına veya bir veritabanı tablosu gibi bir kalıcı veri deposu. .NET Framework, bir XML dosyasından site haritası verileri okuyan bir varsayılan Site haritası sağlayıcısı birlikte ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), ve bu sorundan kullanacağınız Bu öğreticide sağlayıcıdır. Bazı diğer Site haritası sağlayıcısı uygulamaları için bu öğreticinin sonunda başka okumalar bölümüne bakın.

Varsayılan Site haritası sağlayıcısı adlı bir doğru biçimlendirilmiş XML dosyası bekliyor `Web.sitemap` kök dizininde bulunması. Bu varsayılan sağlayıcı kullandığımızdan, böyle bir dosya ekleyin ve uygun XML biçiminde site haritanın yapısını tanımlamak ihtiyacımız var. Dosya eklemek için Çözüm Gezgini'nde proje adının üzerine sağ tıklayın ve Yeni Öğe Ekle öğesini seçin. Site Haritası adlı türde bir dosya eklemek için iletişim kutusundan, iyileştirilmiş `Web.sitemap`.


[![Projenin kök dizinine birtakım adlı bir dosya ekleyin](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Şekil 2**: adlı bir dosya ekleme `Web.sitemap` projenin kök dizinine ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image6.png))


XML site haritası olarak hiyerarşi Web sitesinin yapısını tanımlar. Bu hiyerarşi ilişkisi aileyi aracılığıyla XML dosyasındaki modellenmiştir `<siteMapNode>` öğeleri. `Web.sitemap` İle başlamalıdır bir `<siteMap>` kesin bir üst düğümün `<siteMapNode>` alt. Bu üst düzey `<siteMapNode>` öğe hiyerarşisinin kökü temsil eder ve alt düğümleri tercihe bağlı sayıda olabilir. Her `<siteMapNode>` öğesi içermelidir bir `title` özniteliği ve isteğe bağlı olarak içerebilir `url` ve `description` öznitelikleri, diğerlerinin; boş olmayan her `url` özniteliği benzersiz olmalıdır.

Aşağıdaki XML verilerinin girin `Web.sitemap` dosyası:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

Yukarıdaki site harita biçimlendirme Şekil 3'teki hiyerarşinin tanımlar.


[![Site Haritası, hiyerarşik bir gezinti yapısını temsil eder](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Şekil 3**: Site Haritası, hiyerarşik bir gezinti yapısını temsil eder ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>3. adım: ana sayfada gezinme kullanıcı arabirimi eklemek güncelleştirme

ASP.NET Web denetimleri ile ilgili bir kullanıcı arabirimi tasarlama içerir. Bu menü, ağaç görünümünde ve SiteMapPath denetimleri içerir. Geçerli düğümün alt öğelerinden yanı sıra ziyaret gösteren bir içerik haritası SiteMapPath görüntüler ise menü ve TreeView denetimleri site haritası yapısında bir menü veya bir ağaç sırasıyla işleyin. Site haritası verileri diğer veri Web denetimleri SiteMapDataSource kullanarak bağlanabilir ve aracılığıyla programlı olarak erişilebilir `SiteMap` sınıfı.

Site Haritası framework ve gezinti denetimlerinin kapsamlı bir tartışma Bu öğretici serisinin kapsamı dışında olduğundan, bunun yerine kendi gezinme kullanıcı arabirimi şimdi kaynaklı vakit daha yerine kullanılanla ödünç my *[ ASP.NET 2.0 verilerle çalışmaya](../../data-access/index.md)* öğretici serisi, Şekil 4'te gösterildiği gibi iki derin madde işaretli liste Gezinti bağlantıları, görüntülemek için Repeater denetimiyle kullanır.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Sol sütunda bağlantılar düzey iki listesinden ekleme

Bu arabirim oluşturmak için aşağıdaki bildirim temelli işaretlemede ekleyin `Site.master` ana sayfanın sol sütunu burada metin TODO: menü gider burada... şu anda bulunduğu.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

Yukarıdaki biçimlendirme adlı bir yineleyici t:System.Windows.Forms.Binding `menu` SiteMapDataSource için tanımlanan site haritası hiyerarşisini döndürür `Web.sitemap`. SiteMapDataSource denetimin beri [ `ShowStartingNode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) başlar ana düğümünün alt ile başlayarak site haritanın hiyerarşi döndüren false ayarlayın. Yineleyici bu düğümler (şu anda yalnızca Üyelik) görüntüler bir `<li>` öğesi. Başka bir, iç Yineleyici, ardından geçerli düğümün alt öğelerinden içinde iç içe geçmiş ve sırasız bir listesini görüntüler.

Şekil 4, 2. adımda oluşturduğumuz site haritası yapıya sahip yukarıdaki biçimlendirme 's işlenen çıkışı gösterir. Yineleyici temel alınan sırasız liste biçimlendirme oluşturur; geçişli stil sayfası kuralları tanımlanan `Styles.css` aesthetically Hoş düzenini sorumludur. Yukarıdaki biçimlendirme nasıl çalıştığına ilişkin daha ayrıntılı açıklaması için başvurmak [ana sayfalar ve Site gezintisi](https://asp.net/learn/data-access/tutorial-03-vb.aspx) öğretici.


[![İşlenen kullanarak iç içe geçmiş sırasız listeler gezinme kullanıcı arabirimi olan](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Şekil 4**: Gezinme kullanıcı arabirimi olan işlenen kullanarak iç içe geçmiş sırasız listeler ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>İçerik haritalı gezinme ekleme

Sol sütunda bağlantılar listesinde ek olarak, şimdi de her sayfa görüntüleme sahip bir [içerik haritası](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Bir içerik haritası hızla kullanıcılar site hiyerarşisi içinde kendi geçerli konumu gösteren bir gezinti kullanıcı arabirimi öğesidir. SiteMapPath denetimi Site Haritası çerçeve site haritası'nda geçerli sayfanın konumunu belirlemek için kullanır ve bu bilgilere dayanarak bir içerik haritası görüntüler.

Özellikle, eklemeniz bir `<span>` ana sayfanın üst öğeye `<div>` öğesi ve yeni küme `<span>` öğenin `class` özniteliği için içerik haritası. ( `Styles.css` Sınıfı bir içerik haritası sınıfı için bir kural içerir.) Ardından, bu yeni bir SiteMapPath ekleyin `<span>` öğesi.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Şekil 5 ziyaret SiteMapPath çıktısını gösterir `~/Membership/CreatingUserAccounts.aspx`.


[![Geçerli sayfa içerik haritası görüntüler ve üst sitedeki eşleyin](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Şekil 5**: içerik haritası geçerli sayfayı ve alt öğelerinden Site haritada görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>4. adım: özel asıl ve kimlik mantığını kaldırılıyor

İçinde *<a id="_msoanchor_7"> </a> [Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* öğretici kimliği doğrulanmış kullanıcı için özel asıl ve kimlik nesneleri ilişkilendirmek nasıl gördük. Biz bu olay işleyicisinde oluşturarak yapılabilir `Global.asax` için uygulamanın `PostAuthenticateRequest` sonra tetiklenen olayı `FormsAuthenticationModule` kullanıcı kimliğini doğrulamasından. Biz bu olay işleyicisinde yerine `GenericPrincipal` ve `FormsIdentity` tarafından eklenen nesneleri `FormsAuthenticationModule` ile `CustomPrincipal` ve `CustomIdentity` Bu öğreticide oluşturduğumuz nesneleri.

Özel asıl ve kimlik nesneleri bazı senaryolarda, çoğu durumda kullanılabilir ancak `GenericPrincipal` ve `FormsIdentity` nesnelerdir yeterli. Sonuç olarak, varsayılan davranışa dönmek için düşünüyorum. Bu değişiklik ya da kaldırarak veya çıkış yorum `PostAuthenticateRequest` olay işleyicisi veya silerek `Global.asax` tamamen dosya.

> [!NOTE]
> Geçersiz kılınan veya kodda kaldırıldı sonra `Global.asax`, kodda açıklama gerekecektir `Default.aspx's` e yayınlayan kod arkası sınıfı `User.Identity` özelliğini bir `CustomIdentity` örneği.


## <a name="step-5-programmatically-creating-a-new-user"></a>5. adım: Program aracılığıyla yeni bir kullanıcı oluşturma

Üyelik framework kullanılarak yeni bir kullanıcı hesabı oluşturmak için `Membership` sınıfın [ `CreateUser` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Bu yöntem giriş parametreleri için kullanıcı adı, parola ve kullanıcı ile ilgili diğer alanlar. Çağrı üzerinde yapılandırılmış üyelik sağlayıcısı için yeni kullanıcı hesabının oluşturulmasını atar ve ardından döndürür bir [ `MembershipUser` nesne](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) yeni oluşturulan kullanıcı hesabını temsil eden.

`CreateUser` Yöntemi her farklı sayıda giriş parametrelerini kabul dört aşırı yüklemeleri vardır:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Bu dört aşırı yüklemeler, toplanan bilgi miktarına farklılık gösterir. İkinci bir kullanıcının e-posta adresi devralınmalı ilk aşırı yükleme, örneğin, yalnızca kullanıcı adı ve parola yeni kullanıcı hesabı için gerektirir.

Üyelik sağlayıcısının yapılandırma ayarlarıyla yeni bir kullanıcı hesabı oluşturmak için gereken bilgileri bağlı olduğundan bu aşırı yüklemeler var. İçinde *<a id="_msoanchor_8"> </a> [SQL Server'da üyelik şeması oluşturma](creating-the-membership-schema-in-sql-server-vb.md)* biz incelenirken belirten üyelik sağlayıcısını yapılandırma ayarlarında öğretici `Web.config`. Tablo 2 yapılandırma ayarlarının tam listesi dahil.

Bir tür üyelik sağlayıcısı yapılandırma ayarı ne etkiler `CreateUser` aşırı kullanılabilir olan `requiresQuestionAndAnswer` ayarı. Varsa `requiresQuestionAndAnswer` ayarlanır `true` (varsayılan), sonra da yeni bir kullanıcı hesabı oluştururken size bir güvenlik sorusu ve yanıtı belirtmeniz gerekir. Kullanıcı parolasını değiştirme veya sıfırlama gerekiyorsa, bu bilgiler daha sonra kullanılır. Özellikle, o anda bunlar Güvenlik sorusu gösterilir ve sıfırlama veya parolasını değiştirmek için doğru yanıtı girmeniz gerekir. Sonuç olarak, varsa `requiresQuestionAndAnswer` ayarlanır `true` ilk iki birini çağırmadan sonra `CreateUser` Güvenlik sorusu ve yanıtı içermediği için bir özel durum sonuçlarında aşırı yüklemeleri. Uygulamamız şu anda bir güvenlik sorusu ve yanıtı gerektirecek şekilde yapılandırılmış olduğundan, biz ikinci iki aşırı kullanıcının program aracılığıyla oluştururken kullanmanız gerekir.

Kullanarak göstermek için `CreateUser` yöntemi, burada size kullanıcıdan kendi adı, parola, e-posta ve önceden tanımlı Güvenlik sorusu yanıtını bir kullanıcı arabirimi oluşturalım. Açık `CreatingUserAccounts.aspx` sayfasını `Membership` klasörü ve aşağıdaki Web denetimleri içerik denetimine ekleyin:

- Adlı bir metin kutusu `Username`
- Adlı bir metin kutusu `Password`, olan `TextMode` özelliği `Password`
- Adlı bir metin kutusu `Email`
- Adlı bir etiket `SecurityQuestion` ile kendi `Text` özelliği temizlenmiş
- Adlı bir metin kutusu `SecurityAnswer`
- Adlı bir düğme `CreateAccountButton` olan `Text` özelliği, kullanıcı hesabı oluşturma için ayarlanır
- Adlı bir etiket denetimi `CreateAccountResults` ile kendi `Text` özelliği temizlenmiş

Bu noktada, ekran Şekil 6'da gösterilen ekran şuna benzemelidir.


[![Çeşitli Web denetimleri CreatingUserAccounts.aspx sayfasına ekleme](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Şekil 6**: çeşitli Web denetimlere ekleme `CreatingUserAccounts.aspx Page` ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image18.png))


`SecurityQuestion` Etiket ve `SecurityAnswer` metin kutusu, önceden tanımlı Güvenlik sorusu görüntüler ve kullanıcının yanıt toplamak için yöneliktir. Her biri kendi Güvenlik sorusu tanımlamalarına izin ver mümkündür Güvenlik sorusu ve yanıtı bir kullanıcı tarafından temelinde depolandığını unutmayın. Ancak, ı karar bir Evrensel güvenlik sorusu özelliği kullanmak Bu örnek için: en sevdiğiniz renk nedir?

Bu önceden tanımlı Güvenlik sorusu uygulamak için bir sabit adlı sayfa arka plan kod sınıfı Ekle `passwordQuestion`, Güvenlik sorusu atama. Ardından `Page_Load` olay işleyicisi, Ata bu sabit `SecurityQuestion` etiketin `Text` özelliği:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Ardından, bir olay işleyicisi oluşturun `CreateAccountButton'` s `Click` olay ve aşağıdaki kodu ekleyin:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

`Click` Olay işleyicisini başlatır adlı bir değişken tanımlayarak `createStatus` türü [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` durumunu gösteren bir numaralandırma `CreateUser` işlemi. Örneğin, kullanıcı hesabının, ortaya çıkan başarıyla oluşturulursa, `MembershipCreateStatus` örneği değerine ayarlanacak `Success;` aynı kullanıcı adına sahip bir kullanıcı zaten mevcut olduğundan işlem başarısız olursa, diğer yandan, bu değeriolarakayarlanır`DuplicateUserName`. İçinde `CreateUser` kullandığımız aşırı yükleme, ihtiyacımız geçirilecek bir `MembershipCreateStatus` yöntem örneğine. Bu parametre içinde uygun değere ayarlanır `CreateUser` yöntemi ve biz inceleyebilirsiniz değerini kullanıcı hesabının başarıyla oluşturulup oluşturulmadığını belirlemek için yöntem çağrısından sonra.

Arama sonra `CreateUser`, içinde geçen `createStatus`, `Select Case` deyimi atanan değerine bağlı olarak uygun bir ileti çıktısını almak için kullanılır `createStatus`. Şekil 7, yeni kullanıcının başarıyla oluşturulduğunda çıkış gösterir. Kullanıcı hesabı oluşturulmadığında Şekil 8 ve 9 çıktıyı gösterir. Şekil 8'de, ziyaretçi il üyelik sağlayıcısının yapılandırma ayarlarını parola gücü gereksinimlerini karşılamıyor beş harfli parola girildi. Şekil 9'da ziyaretçi var olan bir kullanıcı adı (Şekil 7'de oluşturulan bir) ile bir kullanıcı hesabı oluşturma deniyor.


[![Yeni bir kullanıcı hesabı başarıyla oluşturulmuştur](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Şekil 7**: A yeni kullanıcı hesabı, başarıyla oluşturuldu ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image21.png))


[![Sağlanan parola çok zayıf olduğu için kullanıcı hesabı oluşturulmaz.](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Şekil 8**: Sağlanan parola çok zayıf olduğu için kullanıcı hesabı oluşturulmaz ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image24.png))


[![Kullanıcı hesabı değil oluşturulan kullanıcı adı zaten kullanımda olduğundan.](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Şekil 9**: kullanıcı hesabı olduğu değil oluşturulduğu için kullanıcı adı zaten kullanımda olduğundan ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> İlk iki birini kullanırken, başarı veya başarısızlığı belirlemek nasıl merak ediyor olabilirsiniz `CreateUser` diğerinden yöntemi aşırı türünde bir parametreye sahip olan `MembershipCreateStatus`. Bu ilk iki aşırı yüklemeler throw bir [ `MembershipCreateUserException` özel durum](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) içeren bir hata karşılaşıldığında bir [ `StatusCode` özelliği](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) türü `MembershipCreateStatus`.


Birkaç kullanıcı hesabı oluşturduktan sonra hesapları içeriğini listeleyerek oluşturulmuş doğrulayın `aspnet_Users` ve `aspnet_Membership` tablolar `SecurityTutorials.mdf` veritabanı. Şekil 10 gösterildiği gibi iki kullanıcı aracılığıyla eklediğiniz `CreatingUserAccounts.aspx` sayfası: Tito ve Bruce.


[![Üyelik kullanıcı Store içinde iki kullanıcı vardır: Tito ve Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Şekil 10**: Üyelik kullanıcı Store içinde iki kullanıcı vardır: Tito ve Bruce ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image30.png))


Üyelik kullanıcı deposu artık Bruce ve Tito'nın hesap bilgilerini içerir, ancak henüz Bruce veya Tito sitesinde oturum açmaya olanak sağlayan işlevselliği uygulamak sahibiz. Şu anda `Login.aspx` kullanıcının kimlik bilgilerini doğrular, bir sabit kodlanmış kullanıcı adı/parola çiftleri kümesini - karşı çalıştığı *değil* üyelik framework karşı sağlanan kimlik bilgilerini doğrulama. Yeni kullanıcı hesapları artık görmek için `aspnet_Users` ve `aspnet_Membership` tabloları yeterli olacaktır. Sonraki öğreticide  *<a id="_msoanchor_9"> </a> [doğrulanırken kullanıcı kimlik bilgilerine karşı üyelik kullanıcı Store](validating-user-credentials-against-the-membership-user-store-vb.md)*, üyelik deposu karşı doğrulamak için oturum açma sayfasına güncelleştireceğiz.

> [!NOTE]
> Tüm kullanıcılar görmüyorsanız, `SecurityTutorials.mdf` veritabanı, web uygulamanızın varsayılan üyelik sağlayıcısını kullandığından olabilir `AspNetSqlMembershipProvider`, kullanan `ASPNETDB.mdf` veritabanı olarak kendi kullanıcı deposu. Bu sorunu olup olmadığını belirlemek için Çözüm Gezgini yenile düğmesine tıklayın. Adlı bir veritabanı `ASPNETDB.mdf` eklendi `App_Data` klasöründe sorun budur. 4. adım için iade *<a id="_msoanchor_10"> </a> [SQL Server'da üyelik şeması oluşturma](creating-the-membership-schema-in-sql-server-vb.md)* üyelik sağlayıcısının düzgün bir şekilde yapılandırma hakkında yönergeler için öğretici.


Çoğu kullanıcı hesabı senaryoları oluşturun, ziyaretçi kullanıcı adı, parola, e-posta ve bu noktada yeni bir hesap oluşturulur diğer gerekli bilgileri girmek için bazı arabirimi ile sunulur. Bu adımda biz böyle bir arabirim el ile derlemeye baktığı ve ardından nasıl kullanılacağını gördüğünüz `Membership.CreateUser` program aracılığıyla yeni kullanıcı hesabı eklemek için yöntem tabanlı kullanıcı girişleri üzerinde. Kodlarımızın, ancak yeni kullanıcı hesabının yeni oluşturduğunuz. Kullanıcının siteye yeni oluşturulan kullanıcı hesabı altında oturum veya kullanıcıya bir onay e-posta gönderme gibi eylemleri, tüm izleme gerçekleştirmedi. Şu ek adımları düğmenin ek kodda gerektirecek `Click` olay işleyicisi.

ASP.NET üyelik framework hesabı oluşturma ve gerçekleştirme sonrası hesabı için yeni kullanıcı hesapları oluşturmak için bir kullanıcı arabirimi oluşturma gelen kullanıcı hesabı oluşturma işlemi, işlemek üzere tasarlanmış CreateUserWizard denetimi ile birlikte gelir bir onay e-posta göndermek ve yeni oluşturulan kullanıcı siteye günlüğe kaydetme gibi oluşturma görevleri. CreateUserWizard denetimini kullanarak CreateUserWizard denetimi bir sayfaya Toolbox'tan sürükleyerek ve sonra birkaç özellik ayarı olarak basit bir işlemdir. Çoğu durumda, tek satırlık bir kod yazmanız gerekmez. Biz bu çok denetimi ayrıntılı adım 6'daki inceleyeceksiniz.

Yeni kullanıcı hesapları yalnızca tipik oluşturduğunuz hesabı web sayfası oluşturulursa, hiç olmadığı kadar kullanan kod yazma gerektiğini olası `CreateUser` yöntemi CreateUserWizard denetimi büyük olasılıkla gereksinimlerinizi. Ancak, `CreateUser` üst düzeyde özelleştirilmiş bir hesabı oluştur kullanıcı deneyimi gerek duyduğunuz ya da alternatif bir arabirim üzerinden yeni kullanıcı hesaplarını program aracılığıyla oluşturma gerektiğinde yöntemi senaryolarda elinizin altında. Örneğin, bir kullanıcı başka bir uygulama kullanıcı bilgilerini içeren bir XML dosyasını karşıya yüklemek bir sayfa olabilir. Sayfa içeriği karşıya yüklenen XML dosyası ve çağırarak XML içinde gösterilen her bir kullanıcı için yeni bir hesap oluşturun ayrıştırıyor olabilir `CreateUser` yöntemi.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>6. adım: yeni bir kullanıcı ile CreateUserWizard denetimi oluşturma

ASP.NET oturum açma Web denetimleri bir dizi ile birlikte gelir. Bu denetimlerin birçok yaygın kullanıcı hesabı ve oturum açmayla ilgili senaryolarda yardımcı. [CreateUserWizard denetim](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) üyelik framework için yeni bir kullanıcı hesabı eklemek için bir kullanıcı arabirimi için tasarlanmış olan bir denetim.

Birçok diğer oturum açma ile ilgili Web denetimleri gibi bir tek satır kod yazmadan CreateUserWizard kullanılabilir. Sezgisel üyelik sağlayıcısının yapılandırma ayarları ve çağrı dahili olarak bağlı bir kullanıcı arabirimi sağlar `Membership` sınıfın `CreateUser` yöntemi sonra kullanıcı, gerekli bilgileri girer ve kullanıcı Oluştur düğmesine tıklar. Son derece özelleştirilebilir CreateUserWizard denetimidir. Bir ana bilgisayar hesabını oluşturma işlemi çeşitli aşamaları boyunca tetiklenen olayların vardır. Olay işleyicileri, hesap oluşturma iş akışına özel mantığınızı eklemek, gerektiği gibi oluşturabilirsiniz. Ayrıca, CreateUserWizard'ın görünüm çok esnektir. Bir dizi varsayılan arabirimi görünümü tanımlayan özellikleri vardır; Gerekirse, denetim bir şablona dönüştürülebilir veya ek kullanıcı kayıt adımları eklenebilir.

CreateUserWizard denetimin varsayılan arabirim ve davranışını kullanarak göz başlayalım. Ardından, şunları nasıl denetimin özellikleri ve olayları aracılığıyla özelleştirme keşfedeceğiz.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>CreateUserWizard'ın varsayılan arabirim ve davranışı İnceleme

Geri dönüp `CreatingUserAccounts.aspx` sayfasını `Membership` klasörü, tasarım veya bölünmüş moduna geçin ve sonra sayfanın üst CreateUserWizard denetimi ekleyin. Toolbox'ın oturum açma denetimleri bölümü altında CreateUserWizard denetim dosyalanır. Denetimi ekledikten sonra ayarlama, `ID` özelliğini `RegisterUser`. Şekil 11 programlarını ekran görüntüsü gibi yeni kullanıcının kullanıcı adı, parola, e-posta adresi ve Güvenlik sorusu ve yanıtı için metin kutuları arabirimiyle CreateUserWizard işler.


[![CreateUserWizard denetim işleme genel kullanıcı arabirimi oluşturma](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Şekil 11**: genel bir oluşturma kullanıcı arabirimi CreateUserWizard kontrolünü icra ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image33.png))


5. adımda oluşturduğumuz arabirimi CreateUserWizard denetimi tarafından oluşturulan varsayılan kullanıcı arabirimi karşılaştırmak için bir zaman ayırabiliriz. Yeni başlayanlar için önceden tanımlı Güvenlik sorusu el ile oluşturulan arabirimimizi kullanılan ise güvenlik sorusunu ve yanıtını belirtmek ziyaretçi CreateUserWizard denetim sağlar. Henüz doğrulama arabirimimizi ait form alanlarını uygulamak vardı CreateUserWizard denetimin arabirimi ayrıca doğrulama denetimleri içerir. Ve CreateUserWizard denetim arabirimi (birlikte, metin girilen parola ve parola karşılaştırma metin kutuları eşit olduğundan emin olun CompareValidator) parolayı onayla textbox içerir.

İlgi çekici nedir CreateUserWizard denetimi, kullanıcı arabirimi işlenirken üyelik sağlayıcısının yapılandırma ayarlarını danışır olduğu. Örneğin, Güvenlik sorusu ve yanıtı metin kutuları yalnızca görüntülenir `requiresQuestionAndAnswer` True olarak ayarlayın. Benzer şekilde, CreateUserWizard RegularExpressionValidator denetim parola gücü gereksinimlerinin karşılandığını algılar ve ayarlar sağlamak için otomatik olarak ekler, `ErrorMessage` ve `ValidationExpression` özelliklerini temel alarak `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`ve `passwordStrengthRegularExpression` yapılandırma ayarları.

Adından da anlaşılacağı gibi CreateUserWizard denetim türetilmiş [Sihirbazı denetim](https://msdn.microsoft.com/library/s2etd1ek.aspx). Sihirbazı denetimleri, çok adımlı görevler tamamlamak için bir arabirim sağlamak üzere tasarlanmıştır. Rastgele bir sayıdan bir Sihirbazı denetiminiz `WizardSteps`, her biri olan HTML tanımlayan bir şablon ve Web denetimleri için bu adımı. Sihirbaz Denetimi başlangıçta ilk görüntüler `WizardStep`, sonraki bir adımda devam etmek için ya da önceki adımları döndürülecek yönetebileceklerini Gezinti denetimlerinin yanı sıra.

Bildirim temelli biçimlendirme Şekil 11'de gösterildiği gibi iki CreateUserWizard denetimin varsayılan arabirimi içeren `WizardStep` : %s

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? Yeni kullanıcı hesabı oluşturma hakkında bilgi toplamak için bir arabirim oluşturur. Şekil 11'de gösterilen adımın budur.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? hesabın başarıyla oluşturulduğunu belirten bir ileti işler.

Şablonlara dönüştürerek aşağıdaki adımlardan birini veya kendi ekleyerek CreateUserWizard'ın görünümünü ve davranışını değiştirilebilir `WizardStep` s. Biz ekleme sırasında görünür bir `WizardStep` kayıt arabirimine *ek kullanıcı bilgileri depolama* öğretici.

Eylem CreateUserWizard denetiminde görelim. Ziyaret `CreatingUserAccounts.aspx` tarayıcısından sayfası. CreateUserWizard'ın arabirimine bazı geçersiz değerler girerek başlayın. Deneyin parola gücü gereksinimlerine uymuyor. bir parola girerek veya kullanıcı ad metin kutusu boş bırakın. CreateUserWizard uygun bir hata iletisi görüntüler. Şekil 12 yeterince güçlü bir parolayla bir kullanıcı oluşturmaya çalışırken çıkış gösterir.


[![CreateUserWizard doğrulama denetimleri otomatik olarak ekler.](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Şekil 12**: CreateUserWizard otomatik olarak eklediği doğrulama denetimleri ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image36.png))


Ardından, CreateUserWizard uygun değerleri girin ve kullanıcı Oluştur düğmesine tıklayın. Gerekli alanları girilmiş ve parola'nın gücünü yeterli olduğunu varsayarsak, CreateUserWizard üyelik çerçevesi aracılığıyla yeni bir kullanıcı hesabı oluşturur ve ardından görüntülemek `CompleteWizardStep`kullanıcının arabirim (bkz. Şekil 13). Arka planda CreateUserWizard çağırır `Membership.CreateUser` adım 5'te yaptığımız gibi yöntemi.


[![Yeni bir kullanıcı hesabı başarıyla oluşturuldu sahiptir.](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Şekil 13**: A yeni kullanıcı hesabına sahipse başarıyla oluşturuldu ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> Şekil 13 gösterildiği gibi `CompleteWizardStep`ait bir devam düğmesi arabirimi içerir. Ancak, bu noktada yalnızca tıklayarak ziyaretçi aynı sayfada bırakarak bir geri gönderme gerçekleştirir. İçinde özelleştirme CreateUserWizard'ın görünümünü ve davranışını aracılığıyla kendi özellikleri bölümünde biz ziyaretçi gönderme bu düğmenin nasıl olabilir görüneceğini `Default.aspx` (veya başka bir sayfaya).


Yeni bir kullanıcı hesabı oluşturduktan sonra Visual Studio'ya geri dönün ve incelemek `aspnet_Users` ve `aspnet_Membership` hesabın başarıyla oluşturulduğunu doğrulamak için Şekil 10'da yaptığımız gibi tablolar.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>CreateUserWizard'ın davranış ve görünümünü aracılığıyla özelliklerini özelleştirme

CreateUserWizard çeşitli yollarla, özellikler arasında özelleştirilebilir `WizardStep` s ve olay işleyicileri. Bu bölümde özellikleri aracılığıyla denetimin görünümünü özelleştirme atacağız; sonraki bölümde, olay işleyicileri aracılığıyla denetimin davranışını genişletme sırasında arar.

Neredeyse tüm CreateUserWizard denetimin varsayılan kullanıcı arabiriminde görüntülenen metni özellikleri kendi deseninizi oluşturmayı özelleştirilebilir. Örneğin, metin kutuları solunda görüntülenen kullanıcı adı, parola, parolayı onayla, e-posta, Güvenlik sorusu ve güvenlik yanıtı etiketleri tarafından özelleştirilebilir [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), ve [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) Özellikler, sırasıyla. Benzer şekilde, kullanıcı oluşturma ve devam et düğme metnini belirtmek için özellik yok `CreateUserWizardStep` ve `CompleteWizardStep`, sanki de bu düğmeler düğmeler, LinkButtons veya ImageButtons işlenir.

Renkleri, kenarlık, yazı tipleri ve diğer görsel öğeler stil özellikleri ana bilgisayar üzerinden yapılandırılabilir özelliktedir. Ortak Web denetimi stil özellikleri - CreateUserWizard denetim sahip `BackColor`, `BorderStyle`, `CssClass`, `Font`, vb. - ve birkaç belirli bölümlerini görünümünü tanımlamak için stil özellikleri vardır. CreateUserWizard'ın arabirimi. [ `TextBoxStyle` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), örneğin, metin kutuları içinde stillerini tanımlar `CreateUserWizardStep`, sırada [ `TitleTextStyle` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) (kaydolma bilgisayarınızı yeni başlığı için stil tanımlar Hesabı).

Görünüm güvenlikle ilgili özellikler yanı sıra birkaç CreateUserWizard denetimin davranışını etkileyen özellikler vardır. [ `DisplayCancelButton` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), bir iptal düğmesi (varsayılan değer değeri: False) kullanıcı oluştur düğmesinin yanındaki kümesi için doğru görüntüler. İptal düğmesi görüntülerseniz, ayrıca ayarladığınızdan emin olun [ `CancelDestinationPageUrl` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), kullanıcıya gönderilir için İptal'i tıklattıktan sonra sayfayı belirtir. Önceki bölümde, devam düğmesi de belirtildiği gibi `CompleteWizardStep`'s arabirimi geri göndermeye neden olur, ancak aynı sayfada ziyaretçi bırakır. Devam düğmesine tıkladıktan sonra başka bir sayfaya ziyaretçi göndermek için URL'yi belirtmeniz yeterlidir [ `ContinueDestinationPageUrl` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Güncelleştirelim `RegisterUser` CreateUserWizard denetimi İptal düğmesini göster ve ziyaretçi göndermek için `Default.aspx` iptal veya devam düğme tıklandığında. Bunu gerçekleştirmek için ayarlanmış `DisplayCancelButton` özelliğini True ve her ikisi de `CancelDestinationPageUrl` ve `ContinueDestinationPageUrl` özelliklerine ~ / Default.aspx. Şekil 14 bir tarayıcıdan görüntülendiğinde güncelleştirilmiş CreateUserWizard gösterir.


[![İptal düğmesi CreateUserWizardStep'e içerir](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Şekil 14**: `CreateUserWizardStep` bir iptal düğmesi içerir ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image42.png))


Bir ziyaretçi bir kullanıcı adı, parola, e-posta adresi ve Güvenlik sorusu ve yanıtının girer ve kullanıcı oluşturma, yeni bir kullanıcı hesabı oluşturulur ve ziyaretçi bu yeni oluşturulan kullanıcı olarak günlüğe kaydedilir. Sayfasını ziyaret ederek kişi kendileri için yeni bir hesap oluşturuyor varsayılarak, büyük olasılıkla istenen davranışı budur. Ancak, yeni kullanıcı hesapları eklemek Yöneticiler izin vermek isteyebilirsiniz. Bunun yapılması, kullanıcı hesabının oluşturulması, ancak yönetici olarak (ve yeni oluşturulan hesaba olarak değil) yönetici olarak oturum açmış kalır. Bu davranış, Boolean değiştirilebilir [ `LoginCreatedUser` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Üyelik Framework'te kullanıcı hesapları, onaylanmış bir bayrak içerir; onaylanmamış kullanıcıların siteye oturum açamıyor. Varsayılan olarak, yeni oluşturulan hesaba siteye hemen oturum açmaya izin Onaylandı olarak işaretlenir. Ancak, onaylanmamış olarak işaretlenmiş yeni kullanıcı hesapları sağlamak için mümkündür. Belki de, yönetici yeni kullanıcılar nerelerde oturum açabileceğini önce el ile onaylamak için istediğiniz; veya belki de oturum açmasına izin veren önce kayıt sırasında girilen e-posta adresi geçerli olduğunu doğrulayın. İnovasyonunuz ne olursa olsun olabilir, onaylanmamış işaretlenmiş CreateUserWizard denetimin ayarlayarak yeni oluşturulan kullanıcı hesabı olabilir [ `DisableCreatedUser` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) true (varsayılan değeri: False).

Notun diğer davranışı güvenlikle ilgili özellikler şunlardır: `AutoGeneratePassword` ve `MailDefinition`. Varsa [ `AutoGeneratePassword` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) True olarak ayarlandığında `CreateUserWizardStep` parola ve parolayı onayla kutuları; görüntülemez bunun yerine, yeni oluşturulan kullanıcı parolasını otomatik olarak kullanılarak oluşturulan `Membership` sınıfın [ `GeneratePassword` yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). `GeneratePassword` Yöntemi belirtilen uzunluğu ve alfasayısal olmayan karakter yapılandırılan parola gücü gereksinimlerini karşılamak için yeterli sayıda ile bir parola oluşturur.

[ `MailDefinition` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) hesap oluşturma işlemi sırasında belirtilen e-posta adresine bir e-posta göndermek istiyorsanız kullanışlıdır. `MailDefinition` Alt oluşturulmuş bir e-posta iletisi hakkında bilgi tanımlamak için bir dizi özelliği içerir. Bu alt gibi seçenekleri dahil `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, ve `BodyFileName`. [ `BodyFileName` Özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) işaret eden bir metin veya e-posta iletisinin gövdesini içeren HTML dosyası. Önceden tanımlanmış iki yer tutucuyu gövdesi destekler: `<%UserName%>` ve `<%Password%>`. İçinde varsa, bu yer tutucuları `BodyFileName` dosya, yeni oluşturulan kullanıcı adı ve parola ile değiştirilecek.

> [!NOTE]
> `CreateUserWizard` Denetimin `MailDefinition` özelliği yalnızca yeni bir hesap oluşturulduğunda, gönderilen e-posta iletisi ayrıntılarını belirtir. E-posta iletisi gerçekte nasıl gönderileceğini üzerinde herhangi bir ayrıntıyı içermez (diğer bir deyişle, bir SMTP sunucusu veya posta bırakma dizini kullanılıp, tüm kimlik doğrulama bilgilerini ve benzeri). Bu alt düzey ayrıntıları tanımlanması gerekir `<system.net>` konusundaki `Web.config`. Bu yapılandırma ayarlarını ve e-posta ASP.NET 2. 0 ' genel gönderme hakkında daha fazla bilgi için [SystemNetMail.com en sık sorulan sorular](http://www.systemnetmail.com/) ve benim makale [ASP.NET 2.0 e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Olay işleyicileri kullanılarak CreateUserWizard'ın davranışını genişletme

CreateUserWizard denetimi olay sayısı, iş akışı sırasında başlatır. Örneğin, bir ziyaretçi kullanıcı adı, parola ve diğer ilgili bilgileri girer ve kullanıcı Oluştur düğmesine tıklar sonra CreateUserWizard denetimi oluşturur, [ `CreatingUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Oluşturma işlemi sırasında bir sorun varsa [ `CreateUserError` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) tetiklenir; ancak, kullanıcı başarıyla oluşturulursa, ardından [ `CreatedUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) tetiklenir. Yükseltilmiş ek CreateUserWizard denetim olayları vardır ancak bunlar üç en kodun olanlardır.

Belirli senaryolarda size uygun bir olay için bir olay işleyicisi oluşturarak yapabileceğimiz CreateUserWizard iş akışı içine dokunun isteyebilirsiniz. Bunu göstermek için şimdi geliştirmek `RegisterUser` bazı özel bir kullanıcı adı ve parola doğrulamasını içerecek şekilde CreateUserWizard denetimi. Özellikle, diyelim ki bizim CreateUserWizard kullanıcı adlarını başında veya sonunda boşluk içeremez ve Parolada herhangi bir kullanıcı adı yer alamaz geliştirin. Kısacası, birinin oluşturma "Tan" gibi bir kullanıcı adı veya bir kullanıcı adı/parola bileşimini Scott ve Scott.1234 gibi sahip önlemek istiyoruz.

İçin bir olay işleyicisi oluşturur biz bunu sağlamak için `CreatingUser` bizim ek doğrulama denetimleri gerçekleştirmek için olay. Sağlanan veriler geçerli değilse oluşturma işlemini iptal etmek gerekir. Ayrıca kullanıcı adı veya parola geçersiz olduğunu açıklayan bir ileti görüntülemek için sayfanın bir etiket Web denetimi eklemek ihtiyacımız var. Başlangıç CreateUserWizard denetim ayarı altına bir etiket denetimi ekleyerek kendi `ID` özelliğini `InvalidUserNameOrPasswordMessage` ve kendi `ForeColor` özelliğini `Red`. Temizle kendi `Text` özelliği ve kümesi kendi `EnableViewState` ve `Visible` özellikleri False.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Ardından, CreateUserWizard denetim için bir olay işleyicisi oluşturun `CreatingUser` olay. Bir olay işleyicisi oluşturmak için tasarımcıda denetimi seçin ve sonra Özellikler penceresine gidin. Buradan ışık Şimşek simgesine tıklayın ve sonra olay işleyicisi oluşturmak için uygun bir olayı çift tıklatın.

Aşağıdaki kodu ekleyin `CreatingUser` olay işleyicisi:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Kullanıcı adı ve parola CreateUserWizard denetimine girilen aracılığıyla kullanılabilir olduğuna dikkat edin, [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) ve [ `Password` özellikleri](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)sırasıyla. Bu özellikler yukarıdaki olay işleyicisini sağlanan kullanıcı adının başında veya sonunda boşluk içerip içermediğini ve kullanıcı adı parola içinde bulunup belirlemek için kullanıyoruz. Bu koşullardan biri karşılanırsa bir hata iletisi görüntülenir `InvalidUserNameOrPasswordMessage` etiket ve olay işleyicinin `e.Cancel` özelliği `True`. Varsa `e.Cancel` ayarlanır `True`, etkin kullanıcı hesabı oluşturma işlemi iptal ediliyor, iş akışının CreateUserWizard short-circuits.

Şekil 15 gösteren ekran görüntüsü `CreatingUserAccounts.aspx` öndeki boşlukları ile bir kullanıcı adı girdiğinde kullanıcı.


[![Kullanıcı adlarını baştaki veya sondaki boşluklara izin verilmez](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Şekil 15**: kullanıcı adlarını baştaki veya sondaki boşluklara izin verilmez ([tam boyutlu görüntüyü görmek için tıklatın](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> CreateUserWizard denetimin kullanma örneği göreceğiz `CreatedUser` olayında *<a id="_msoanchor_11"> </a> [ek kullanıcı bilgileri depolama](storing-additional-user-information-vb.md)* öğretici.


## <a name="summary"></a>Özet

`Membership` Sınıfın `CreateUser` yöntem, üyelik framework yeni bir kullanıcı hesabı oluşturur. Bunu yapılandırılmış üyelik sağlayıcısının çağrısı temsilci atayarak yapar. Durumunda, `SqlMembershipProvider`, `CreateUser` yöntemi için bir kayıt ekler `aspnet_Users` ve `aspnet_Membership` veritabanı tablolarını.

Program aracılığıyla (adım 5'te gördüğümüz gibi) yeni kullanıcı hesapları oluşturulabilir, ancak daha hızlı ve kolay bir yaklaşım CreateUserWizard denetim kullanmaktır. Bu denetim, üyelik framework yeni bir kullanıcı oluşturma ve kullanıcı bilgileri toplamak için bir çok adımlı kullanıcı arabirimi oluşturur. Aslında, bu denetimi aynı kullanan `Membership.CreateUser` 5. adım, ancak denetimin incelenirken olarak yöntemi doğrulama denetimleri, kullanıcı arabirimi oluşturur ve kullanıcı hesabı oluşturma hataları bir lick kod yazmak zorunda kalmadan yanıt verir.

Bu noktada işlevselliği yeni kullanıcı hesapları oluşturmadan yerinde sahibiz. Ancak, oturum açma sayfasına ikinci öğreticide belirlemiş bu sabit kodlanmış kimlik bilgilerinizi karşı doğruluyor. İçinde <a id="_msoanchor_12"> </a> [sonraki öğreticiye](validating-user-credentials-against-the-membership-user-store-vb.md) güncelleştireceğiz `Login.aspx` kullanıcıyı doğrulamak için üyelik framework karşı kimlik bilgileri sağlanan.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [`CreateUser` Teknik belgeler](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [CreateUserWizard denetimine genel bakış](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Bir dosya sistemi tabanlı Site haritası sağlayıcısı oluşturma](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [ASP.NET 2.0 Sihirbazı denetimi ile adım adım kullanıcı arabirimi oluşturma](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [ASP.NET 2.0 İnceleme kullanıcının Site gezintisi](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Ana sayfalar ve Site gezintisi](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [İçin beklemede SQL Site haritası sağlayıcısı](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Teresa Murphy oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Önceki](creating-the-membership-schema-in-sql-server-vb.md)
> [İleri](validating-user-credentials-against-the-membership-user-store-vb.md)
