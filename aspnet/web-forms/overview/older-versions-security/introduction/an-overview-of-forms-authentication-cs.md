---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
title: Form kimlik doğrulaması (C#) genel bakış | Microsoft Docs
author: rick-anderson
description: Özel rotalar oluşturma
ms.author: riande
ms.date: 01/14/2008
ms.assetid: de2d65b9-aadc-42ba-abe1-4e87e66521a0
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 64c8ccbc82a5c80a6f1e3199bfceb62add6cde51
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752942"
---
<a name="an-overview-of-forms-authentication-c"></a>Form kimlik doğrulaması (C#) genel bakış
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_cs.pdf)

> Bu öğreticide uygulamasına yalnızca tartışma kaldıracağız; Özellikle, form kimlik doğrulaması uygulanmasına görünecektir. Üyelik ve roller için basit bir form kimlik doğrulamasını hareket ettikçe Bu öğreticide oluşturma başlangıç web uygulaması, sonraki öğreticilerde, üzerinde oluşturulacak devam eder.
> 
> Lütfen bu konuyla ilgili daha fazla bilgi için bu videoyu bakın: [ASP.NET'te temel Forms kimlik kullanarak](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](security-basics-and-asp-net-support-cs.md) ASP.NET tarafından sağlanan çeşitli kimlik doğrulama, yetkilendirme ve kullanıcı hesabı seçenekleri ele almıştık. Bu öğreticide uygulamasına yalnızca tartışma kaldıracağız; Özellikle, form kimlik doğrulaması uygulanmasına görünecektir. Üyelik ve roller için basit bir form kimlik doğrulamasını hareket ettikçe Bu öğreticide oluşturma başlangıç web uygulaması, sonraki öğreticilerde, üzerinde oluşturulacak devam eder.

Bu öğreticide, form kimlik doğrulama iş akışı, önceki öğreticide biz dokunulan üzerine bir konu ilişkin kapsamlı bir bakış ile başlar. ASP.NET Web sitesi, form kimlik doğrulaması kavramları gösteri, oluşturacağız. Ardından, site, forms kimlik doğrulaması kullanmak için bir basit bir oturum açma sayfası oluşturup, kod içinde bir kullanıcının kimliği doğrulanır ve bu durumda, kullanıcı adı ile günlüğe olup olmadığını belirlemek bkz yapılandırıyoruz.

Bir web uygulamasında etkinleştirme ve oturum açma ve kapatma sayfaları oluşturma kimlik doğrulama iş akışı, tüm önemli kullanıcı hesaplarını destekleyen ve bir web sayfası aracılığıyla kullanıcılarının kimliğini doğrulayan bir ASP.NET uygulaması oluşturma adımları olan formları anlama. Bu – nedeniyle ve bu öğreticileri birbirine - derleme olduğundan bu öğreticide tam geçmiş projelerinde form kimlik doğrulamasını yapılandırma deneyimi önceden çalıştırılmış olsa bile bir sonrakine geçmeden önce iş geçmenizi öneriyoruz.

## <a name="understanding-the-forms-authentication-workflow"></a>Form kimlik doğrulama iş akışı anlama

ASP.NET çalışma zamanı, bir ASP.NET sayfasının veya ASP.NET Web hizmeti gibi ASP.NET kaynağa ait bir isteği işlerken isteğin bir olay sayısı yaşam döngüsü sırasında başlatır. İstek olanları istek doğrulanır ve yetkili bir işlenmeyen özel durum ve benzeri söz konusu olduğunda gerçekleşen bir olay harekete geçirilen çok başlangıç ve çok sonunda harekete geçirilen olaylar vardır. Olayların tam listesi görmek için başvurmak [HttpApplication nesnenin olayları](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*HTTP modüllerinden* yönetilen sınıflar, kod, istek yaşam döngüsü içinde belirli bir olaya yanıt olarak yürütülür. ASP.NET, birkaç önemli görevleri arka planda gerçekleştirmek HTTP Modülleri ile birlikte gelir. Bizim tartışmaya yakından ilgili olan iki yerleşik HTTP modülleri şunlardır:

- **[`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** – genellikle kullanıcının tanımlama bilgilerini koleksiyona dahil edilir forms kimlik doğrulaması bileti inceleyerek kullanıcının kimliğini doğrular. Hiçbir forms kimlik doğrulaması bileti varsa, anonim bir kullanıcıdır.
- **[`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** – Geçerli kullanıcı istenen URL erişmek için yetkili olup olmadığını belirler. Bu modül, uygulamanın yapılandırma dosyalarında belirtilen yetkilendirme kuralları consulting tarafından yetkilisi belirler. ASP.NET ayrıca içerir [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) istenen dosyaları ACL'leri consulting tarafından yetkilisi belirleyen.

`FormsAuthenticationModule` Öncesinde kullanıcı kimlik doğrulama girişiminde `UrlAuthorizationModule` (ve `FileAuthorizationModule`) yürütülüyor. İsteği yapan kullanıcının istenen kaynağa erişim yetkisi yok, yetkilendirme modülü istek sonlandırır ve döndüren bir [HTTP 401 Yetkisiz](http://www.checkupdown.com/status/E401.html) durumu. Windows kimlik doğrulaması senaryolarda tarayıcıya HTTP 401 durum döndürülür. Bu durum kodu tarayıcının kullanıcıdan kimlik bilgilerini kalıcı bir iletişim kutusu aracılığıyla neden olur. FormsAuthenticationModule bu durumu algılar ve bunun yerine kullanıcı oturum açma sayfasına yeniden yönlendirmek değiştirdiği için form kimlik doğrulaması ile ancak HTTP 401 yetkilendirilmedi durum hiçbir zaman tarayıcıya gönderilen (aracılığıyla bir [HTTP 302 yeniden yönlendirme](http://www.checkupdown.com/status/E302.html) durumu).

Kullanıcının kimlik bilgilerinin geçerli olduğundan ve bu durumda, forms kimlik doğrulaması bileti oluşturmak ve kullanıcı sayfasına yeniden yönlendirmek için bunlar ziyaret etmek çalıştığınız, oturum açma sayfasının sorumluluk belirlemektir. Kimlik doğrulaması bileti Web sitesi sayfalarına sonraki istekler dahildir, `FormsAuthenticationModule` kullanıcıyı tanımlamak için kullanır.


![Form kimlik doğrulama iş akışı](an-overview-of-forms-authentication-cs/_static/image1.png)

**Şekil 1**: form kimlik doğrulama iş akışı


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Kimlik doğrulama anahtarı sayfa ziyareti hatırlama

Bunlar site gezindikçe kullanıcı olarak oturum açmış kalması açtıktan sonra forms kimlik doğrulaması bileti her istek üzerine web sunucuya geri gönderilmesi gerekir. Bu genellikle kullanıcının tanımlama bilgilerini koleksiyonda kimlik doğrulaması bileti yerleştirerek gerçekleştirilir. [Tanımlama bilgilerini](http://en.wikipedia.org/wiki/HTTP_cookie) , kullanıcının bilgisayarda bulunabilir ve her istekte tanımlama bilgisi oluşturulan Web sitesi HTTP üst bilgisindeki aktarılan küçük metin dosyalarıdır. Bu nedenle, forms kimlik doğrulaması bileti oluşturulur ve depolanır tarayıcının tanımlama bilgilerini sonra site sonraki her ziyaret edin, böylece kullanıcı olup olmadığınızı belirlemek, istekle birlikte kimlik doğrulaması bileti gönderir.

Bir tanımlama bilgisi tarih ve saate, tarayıcı tanımlama bilgisi atar, sona erme yönüdür. Forms kimlik doğrulaması tanımlama bilgisi geçerlilik süresi dolduğunda, kullanıcı artık doğrulanabilen ve bu nedenle, anonim hale. Bir kullanıcı bir genel terminalden açtıklarında kendi kimlik doğrulama anahtarı, kullanıcının tarayıcıyı kapattıktan sonra süresi dolacak şekilde istedikleri yüksektir. Evden ziyaret edildiğinde, aynı kullanıcı böylece bunlar sahip olmayan tarayıcı yeniden başlatmaları arasındaki anımsanacağını için kimlik doğrulaması bileti ancak isteyebilirsiniz yeniden oturum siteyi ziyaret ettikleri her zaman. Bu karar genellikle bir "Beni Hatırla" biçiminde bir kullanıcı tarafından oturum açma sayfasındaki onay kutusunu yapılır. Adım 3'te oturum açma sayfasını "Beni Hatırla" onay kutusu uygulamak nasıl inceleyeceğiz. Ayrıntılı kimlik doğrulaması bileti zaman aşımı ayarları aşağıdaki öğreticiye ele alır.

> [!NOTE]
> Bu Web sitesine oturum açmak için kullanılan kullanıcı aracısı tanımlama bilgilerini desteklemeyebilir mümkündür. Böyle bir durumda, ASP.NET cookieless form kimlik doğrulama biletlerini kullanabilirsiniz. Bu modda, URL'de kimlik doğrulaması bileti kodlanır. Cookieless kimlik doğrulama biletlerini kullanıldığında ve nasıl bunlar oluşturulur ve sonraki öğreticide yönetilen atacağız.


### <a name="the-scope-of-forms-authentication"></a>Form kimlik doğrulaması kapsamı

`FormsAuthenticationModule` Olan yönetilen koddan ASP.NET çalışma zamanı bir parçasıdır. Microsoft'un sürümünden önceki 7 [Internet Information Services (IIS)](https://www.iis.net/) web sunucusu IIS HTTP ardışık düzen ve ASP.NET çalışma zamanının işlem hattı arasındaki farklı bir engel oldu. Kısacası, IIS 6 ve önceki sürümlerinde, `FormsAuthenticationModule` yalnızca IIS ASP.NET çalışma zamanı için bir istek aktarıldığında yürütür. .Aspx, .asmx ve .ashx bir uzantıya sahip bir sayfa istendiğinde varsayılan olarak, IIS statik içeriğin kendisini – HTML sayfalarını ve CSS ve resim dosyalarını – ve ASP.NET çalışma zamanı için istekleri yalnızca ellerini gibi işler.

IIS 7'de, işlem hatları ancak tümleşik IIS ve ASP.NET için izin verir. IIS 7'için FormsAuthenticationModule çağırmak için birkaç yapılandırma ayarlarıyla ayarlayabilirsiniz *tüm* istekleri. Ayrıca, IIS 7 ile tüm dosya türlerini URL yetkilendirme kuralları tanımlayabilirsiniz. Daha fazla bilgi için [değişiklikler arasında IIS6 ve IIS7 güvenlik](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [uygulamanızın Web Platform güvenliği](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), ve [anlama IIS7 URL yetkilendirmesi](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Yazıyı kısa, IIS 7'dan önceki sürümlerde, yalnızca form kimlik doğrulaması ASP.NET çalışma zamanı tarafından işlenen kaynakları korumak için de kullanabilirsiniz. Benzer şekilde, URL yetkilendirme kuralları, yalnızca ASP.NET çalışma zamanı tarafından işlenen kaynaklara uygulanır. Ancak, IIS 7, IIS HTTP ardışık düzeni, böylece tüm istekler için bu işlev genişletme FormsAuthenticationModule ve UrlAuthorizationModule tümleştirin mümkündür.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>1. adım: Bu öğretici dizisinde bir ASP.NET Web sitesi oluşturma

Olabilecek en büyük hedef kitlesine ulaşmak için Microsoft'un ücretsiz Visual Studio 2008 sürümüyle oluşturmakta bu seri boyunca ASP.NET Web sitesi oluşturulacak [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Biz uygulayacak `SqlMembershipProvider` kullanıcı deposunda bir [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) veritabanı. Visual Studio 2005 ya da Visual Studio 2008 veya SQL Server'ın farklı bir sürüm kullanıyorsanız, endişelenmeyin - adımları neredeyse aynı olacaktır ve önemsiz olmayan farkları gösterilecektir.

> [!NOTE]
> Demo web uygulamasının her öğreticide kullanılan bir indirme olarak kullanılabilir. Bu indirilebilir bir uygulama, .NET Framework sürüm 3.5 için hedeflenen Visual Web Developer 2008 ile oluşturuldu. .NET 3.5 için hedeflenen uygulaması olduğundan, Web.config dosyası, 3.5 özgü ek yapılandırma öğelerini içerir. İlk olarak Web.config dosyasından 3.5 özgü biçimlendirmeye kaldırmadan .NET 3.5, ardından indirilebilir web uygulamasını bilgisayarınıza yüklemek henüz yoksa kısa yazıyı çalışmaz.


Form kimlik doğrulaması uygulamamızı yapılandırmadan önce ilk ASP.NET Web sitesi ihtiyacımız var. Yeni bir dosya sistemi tabanlı ASP.NET Web sitesi oluşturmaya başlayın. Bunu yapmak için Visual Web Developer başlatın ve dosya menüsüne gidin ve yeni Web sitesi, yeni Web sitesi iletişim kutusunda görüntüleme seçin. ASP.NET Web sitesi şablonu seçin, dosya sistem konumu aşağı açılan listesi olarak, web sitesine yerleştirmek için bir klasör seçin ve C# dilini ayarlama. Bu uygulama bir Default.aspx ASP.NET sayfası ile yeni bir web sitesi oluşturur\_veri klasörü ve Web.config dosyası.

> [!NOTE]
> Visual Studio, proje yönetimi iki modu destekler: Web sitesi projeleri ve Web Uygulama projeleri. Web sitesi projelerine proje dosyası, Web Uygulama projeleri, Visual Studio .NET 2002/2003 proje mimarisi taklit – bir proje dosyası dahil etme ve / bin klasörüne yerleştirilir tek bir derleme içine projenin kaynak kod derlenmeye ise yoksundur. Service Pack 1'web uygulaması proje modeli yeniden olsa da visual Studio 2005 başlangıçta yalnızca desteklenen Web sitesi, proje; Visual Studio 2008 her iki proje modelleri sunar. Ancak, Visual Web Developer 2005 ve 2008 sürümleri, yalnızca Web sitesi projelerini destekler. Web sitesi proje modeli kullanacaklardır. Olmayan Express edition kullanıyorsanız ve kullanmak istediğiniz [Web uygulaması proje modeli](https://msdn.microsoft.com/library/aa730880%28vs.80%29.aspx) bunun yerine, bunu yapabilir; ancak olabileceğini bazı tutarsızlıklar ekranınızın ve karşı uygulayacağınız adımlar gördükleri arasında farkında çekinmeyin gösterilen ekran görüntüleri ve bu öğreticileri, sağlanan yönergeler.


[![Yeni bir dosya sistemi tabanlı Web sitesi oluşturma](an-overview-of-forms-authentication-cs/_static/image3.png)](an-overview-of-forms-authentication-cs/_static/image2.png)

**Şekil 2**: New File System-Based Web sitesi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image4.png))


### <a name="adding-a-master-page"></a>Ana sayfa ekleme

Ardından, sitenin kök dizininde Site.master adlı yeni bir ana sayfa ekleyin. [Ana sayfalar](https://msdn.microsoft.com/library/wtxbf3hh.aspx) ASP.NET sayfaları için uygulanabilir bir site genelinde şablonlarını tanımlamak bir sayfa Geliştirici etkinleştirin. Ana sayfalar ana avantajı, böylece güncelleştirin veya sitenin Düzen ince kolaylaştırma sitenin genel görünümü tek bir konumda tanımlanabilir ' dir.


[![Ana sayfa ekleyin ve Web sitesi Site.master adlı](an-overview-of-forms-authentication-cs/_static/image6.png)](an-overview-of-forms-authentication-cs/_static/image5.png)

**Şekil 3**: bir ana sayfa adlı Site.master Web sitesine ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image7.png))


Site genelinde sayfa düzeni burada ana sayfasında tanımlayın. Tasarım görünümünü kullanın ve gereksinim duyduğunuz ne olursa olsun düzeni veya Web denetimleri ekleme ya da el ile kaynak görünümü biçimlendirme el ile ekleyebilirsiniz. Ben my ana sayfanın düzeni kullanılan Düzen taklit edecek şekilde yapılandırılmış my *[ASP.NET 2.0 verilerle çalışmaya](../../data-access/index.md)* öğretici serisinin (bkz. Şekil 4). Ana sayfa kullanan [geçişli stil sayfaları](http://www.w3schools.com/css/default.asp) konumlandırma ve stilleri (Bu, bu öğreticinin ilişkili indirme işlemine dahildir) Style.css dosyasında tanımlanan CSS ayarları için. Aşağıda gösterilen biçimlendirmeden bildiremez, ancak CSS kurallarını tanımlanan şekilde gezinti &lt;div&gt;ait içerik mutlak konumlu soldaki bölmede görünür ve 200 piksel sabit bir genişliğe sahiptir.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample1.aspx)]

Ana sayfa hem statik sayfa düzeni hem de ana sayfa kullanan ASP.NET sayfaları tarafından düzenlenebilir bir bölge tanımlar. Bu içerik düzenlenebilir bölgesi belirtilir `ContentPlaceHolder` içindeki içerik görülebilir denetimi &lt;div&gt;. Tek bir ana sayfamızı sahip `ContentPlaceHolder` (MainContent), ancak ana sayfanın birden çok ContentPlaceHolder sahip olabilir.

Yukarıda girilen biçimlendirme, Tasarım görünümüne geçiş, ana sayfanın düzenini gösterir. Bu ana sayfanın kullanan tüm ASP.NET sayfaları için biçimlendirme belirtme olanağı ile Tekdüzen bu düzen olacaktır `MainContent` bölge.


[![Ana Tasarım görünümü görüntülendiğinde sayfa](an-overview-of-forms-authentication-cs/_static/image9.png)](an-overview-of-forms-authentication-cs/_static/image8.png)

**Şekil 4**: ana sayfa, olduğunda görüntülenen aracılığıyla Tasarım görünümü ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image10.png))


### <a name="creating-content-pages"></a>İçerik sayfaları oluşturma

Default.aspx sayfasında sitemizin içinde bu noktada sahibiz ancak oluşturduğumuz ana sayfaya kullanmaz. Henüz herhangi bir içerik sayfası içermiyorsa, bir ana sayfa kullanmak için bir web sayfasının bildirim temelli biçimlendirme değiştirmek mümkün olmakla birlikte yalnızca sayfayı silin ve yeniden kullanılacak ana sayfasını belirtme projeye eklemek daha kolay olur. Bu nedenle, projeden Default.aspx silerek başlatın.

Ardından, Çözüm Gezgini'nde proje adının üzerine sağ tıklayın ve Default.aspx adlı yeni bir Web formu eklemek seçin. Bu kez, "ana sayfa seçin" onay kutusunu işaretleyin ve Site.master ana sayfayı listeden seçin.


[![Ana sayfa seçin seçerek yeni bir Default.aspx sayfa ekleme](an-overview-of-forms-authentication-cs/_static/image12.png)](an-overview-of-forms-authentication-cs/_static/image11.png)

**Şekil 5**: bir yeni Default.aspx sayfasında bir ana sayfa seçin seçme ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image13.png))


![Site.master ana sayfa kullan](an-overview-of-forms-authentication-cs/_static/image14.png)

**Şekil 6**: Site.master ana sayfa kullan


> [!NOTE]
> Yeni Öğe Ekle iletişim kutusu, Web uygulaması proje modeli kullandığınız bir "ana sayfa seçin" onay kutusu içermez. Bunun yerine, "Web içeriği formu." türünde bir öğe eklemeniz gerekir "Web içeriği formu" seçeneği ve Ekle seçeneğine tıkladıktan sonra Visual Studio aynı Seç asıl görüntüler Şekil 6 üzerinde gösterilen iletişim kutusu.


Yalnızca yeni Default.aspx sayfanın bildirim temelli biçimlendirme içeren bir @Page ana yolunu belirtmeyi yönergesi için ana sayfanın MainContent ContentPlaceHolder sayfa dosyası ve bir içerik denetimi.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample2.aspx)]

Şu an için Default.aspx boş bırakın. Biz bunu içerik eklemek için bu öğreticinin sonraki bölümlerinde döndürür.

> [!NOTE]
> Ana sayfamızı bir menü veya bazı başka gezinme arabirimi için bir bölüm içerir. Böyle bir arabirim bir sonraki öğreticide oluşturacağız.

## <a name="step-2-enabling-forms-authentication"></a>2. adım: Form kimlik doğrulamasını etkinleştirme

Oluşturulan ASP.NET ile Web sitesi, bizim sonraki görev form kimlik doğrulamasını etkinleştirmektir. Uygulamanın kimlik doğrulaması yapılandırması aracılığıyla belirtilen [ `<authentication>` öğesi](https://msdn.microsoft.com/library/532aee0e.aspx) Web.config dosyasındaki. `<authentication>` Öğe uygulama tarafından kullanılan kimlik doğrulama modeli belirten modu adlı tek bir öznitelik içeriyor. Bu öznitelik aşağıdaki dört değerden birine sahip olabilir:

- **Windows** : bir uygulamayı Windows kimlik doğrulaması kullanıyorsa, önceki öğreticide açıklandığı gibi ziyaretçi kimlik doğrulaması web sunucusunun sorumluluğundadır ve bu genellikle temel, Özet veya tümleşik Windows gerçekleştirilir kimlik doğrulaması.
- **Forms**– kullanıcılar aracılığıyla bir web sayfasında form doğrulaması.
- **Passport**– kullanıcılar Microsoft Passport ağ kullanarak doğrulaması.
- **Hiçbiri**– hiçbir kimlik doğrulama modeli kullanılır; tüm ziyaretçiler anonim olacaktır.

Varsayılan olarak, ASP.NET uygulamaları Windows kimlik doğrulaması kullanın. Form kimlik doğrulaması için kimlik doğrulama türünü değiştirmek için daha sonra değiştirmek ihtiyacımız `<authentication>` formları için öğenin mod özniteliği.

Bir Web.config dosyası projenize henüz yoksa bir artık Çözüm Gezgini'nde proje adının üzerine tıklayarak, yeni öğe Ekle seçerek ve ardından bir Web yapılandırma dosyası ekleme ekleyin.


[![Projenize henüz Web.config içermiyorsa, şimdi ekleyin](an-overview-of-forms-authentication-cs/_static/image16.png)](an-overview-of-forms-authentication-cs/_static/image15.png)

**Şekil 7**: durumunda uygulamanızın proje mu değil henüz dahil Web.config, ekleme şimdi ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image17.png))


Ardından, bulun `<authentication>` öğesi ve onu kullanmak için form kimlik doğrulamasını güncelleştirme. Bu değişiklik yapıldıktan sonra Web.config dosyanızın biçimlendirme aşağıdakine benzer görünmelidir:

[!code-xml[Main](an-overview-of-forms-authentication-cs/samples/sample3.xml)]

> [!NOTE]
> Web.config bir XML dosyası olduğundan, büyük/küçük harf önemlidir. Büyük harf ile "F" formlara, mod özniteliği ayarladığınızdan emin olun. "Form" gibi farklı bir kasa kullanıyorsanız, site tarayıcısından ziyaret edildiğinde bir yapılandırma hatası alırsınız.


`<authentication>` Öğe isteğe bağlı olarak içerebilir bir `<forms>` forms kimlik doğrulaması özgü ayarları içeren bir alt öğesi. Şimdilik varsayılan formlar kimlik doğrulaması ayarları yalnızca kullanalım. Biz inceleyeceksiniz `<forms>` adlı sıradaki öğreticide daha ayrıntılı alt öğesi.

## <a name="step-3-building-the-login-page"></a>3. adım: oturum açma sayfası oluşturma

Form kimlik doğrulamasını desteklemek için bir oturum açma sayfası sitemizin gerekir. "Anlama formları kimlik doğrulama iş akışı" bölümünde açıklandığı gibi `FormsAuthenticationModule` otomatik olarak kullanıcı oturum açma sayfasına yeniden yönlendirme olmadıkları bir sayfaya erişmeye çalışırsanız görüntülemeye yetkili. Anonim kullanıcılar için oturum açma sayfasına bir bağlantı gösterilir ASP.NET Web denetimleri vardır. Bu "Oturum açma sayfasının URL'si nedir?" sorusunu sorun.

Varsayılan olarak, forms kimlik doğrulama sistemi Login.aspx adlandırılacak şekilde oturum açma sayfasına bekliyor ve web uygulamasının kök dizine yerleştirilir. Farklı oturum açma sayfası URL'sini kullanmak istiyorsanız, Web.config dosyasında belirterek bunu yapabilirsiniz. Sonraki öğreticide bunun nasıl yapılacağını göreceğiz.

Oturum açma sayfasına üç sorumluluklara sahiptir:

1. Ziyaretçi kimlik bilgilerini sağlayan bir arabirim sağlar.
2. Gönderilen kimlik bilgileri geçerli olup olmadığını belirler.
3. "Kullanıcı forms kimlik doğrulaması bileti oluşturarak oturum".

### <a name="creating-the-login-pages-user-interface"></a>Oturum açma sayfasının kullanıcı arabirimi oluşturma

İlk görev ile başlayalım. Sitenin kök dizinine Login.aspx adlı yeni bir ASP.NET sayfası ekleyin ve Site.master ana sayfası ile ilişkilendirin.


[![Yeni bir ASP.NET sayfası Ekle Login.aspx adlı](an-overview-of-forms-authentication-cs/_static/image19.png)](an-overview-of-forms-authentication-cs/_static/image18.png)

**Şekil 8**: adlı yeni bir ASP.NET sayfasında Login.aspx ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image20.png))


İki metin kutuları: bir kullanıcının adını, parolasını – ve formunun düğme için tipik bir oturum açma sayfası arabirimini oluşur. Web siteleri önerilmesine işaretlediyseniz, sonuçta elde edilen kimlik doğrulaması bileti tarayıcı yeniden başlatmaları arasındaki devam eden bir "Beni Hatırla" bir onay kutusu içerir.

İki metin kutuları Login.aspx ve kümesi eklemek, `ID` kullanıcı adı ve parola, özellikleri sırasıyla. Parolanın ayrıca ayarlayın `TextMode` parola özelliği. Ardından, ayarı bir CheckBox denetimi ekleyin, `ID` RememberMe özelliğini ve kendi `Text` "Beni Hatırla" özelliğini. Oturum Aç düğmesini adlı bir düğme ekleyin, `Text` özelliği, "Login" için ayarlanır. Ve son olarak, bir etiket Web denetimi ekleyin ve ayarlamak kendi `ID` özelliğini InvalidCredentialsMessage, kendi `Text` özelliğini "kullanıcı adı veya parola geçersiz. Lütfen yeniden deneyin. ", kendi `ForeColor` özelliğini kırmızı ve onun `Visible` özelliğini False.

Bu noktada, ekran Şekil 9'da ekran benzer görünür ve sayfanızın bildirim temelli söz dizimi aşağıdaki gibi:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample4.aspx)]


[![Oturum açma sayfasına iki metin kutuları, bir onay kutusu, bir düğme ve bir etiket içerir.](an-overview-of-forms-authentication-cs/_static/image22.png)](an-overview-of-forms-authentication-cs/_static/image21.png)

**Şekil 9**: oturum açma sayfası içeren iki metin kutuları, bir onay kutusu, bir düğme ve bir etiket ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image23.png))


Son olarak, bir olay işleyicisi için Oturum Aç düğmesini'nın tıklayın oluşturma olayı. Tasarımcısından, yalnızca bu olay işleyicisi oluşturmak için düğme denetimini çift tıklayın.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Sağlanan kimlik bilgileri geçerli olup olmadığını belirleme

Şimdi düğmenin Click içinde 2. görev uygulamak ihtiyacımız olay işleyicisi – sağlanan kimlik bilgilerinin geçerli olup olmadığını belirleme. Bunu yapmak için sağlanan kimlik bilgileri bilinen bir kimlik bilgileri ile eşleşiyorsa belirleyebiliriz böylece tüm kullanıcıların kimlik bilgilerini tutan bir kullanıcı deposu olması gerekir.

ASP.NET 2.0 önce geliştiriciler kendi her iki kullanıcı depoları uygulamaya ve deposu ile karşılaştırarak sağlanan kimlik bilgilerini doğrulamak için kod yazma sorumlu. Çoğu geliştirici, bir veritabanında adlandırılmış kullanıcılar kullanıcı adı, parola, e-posta, LastLoginDate ve diğerleri gibi sütunlarla tablo oluşturma kullanıcı gerçek depoyu uygulayabilir. Bu tabloyu, ardından, kullanıcı hesabı başına tek bir kayıtta gerekir. Bir kullanıcının sağlanan kimlik bilgileri doğrulanıyor eşleşen bir kullanıcı adı için veritabanını sorgulama ve sonra veritabanındaki parola için sağlanan parola corresponded sağlama içerir.

ASP.NET 2.0 ile geliştiriciler üyelik sağlayıcılardan birini kullanıcı deposu yönetmek için kullanmanız gerekir. Bu öğretici serisinde biz kullanıcı deposu için bir SQL Server veritabanını kullanan SqlMembershipProvider kullanacaklardır. Tablolar, görünümler ve saklı yordamlar sağlayıcı tarafından beklenen içeren belirli bir veritabanı şeması uygulamak için ihtiyacımız SqlMembershipProvider kullanırken. Bu şemada uygulamak nasıl inceleyeceğiz ***SQL Server'da üyelik şeması oluşturma*** öğretici. Yerinde üyelik sağlayıcısı ile kullanıcının kimlik bilgilerini doğrulama çağırmak kadar basittir [üyelik sınıfı](https://msdn.microsoft.com/library/system.web.security.membership.aspx)'s [ValidateUser (*kullanıcıadı*, *parola*) yöntemi](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), belirten bir Boole değeri döndüren olup olmadığını geçerliliğini *kullanıcıadı* ve *parola* birleşimi. Henüz SqlMembershipProvider'ın kullanıcı deposu uyguladık olmayan görmekten gibi üyelik sınıfın ValidateUser yöntemi şu anda kullanamazsınız.

Yerine (SqlMembershipProvider uyguladık sonra eski olacaktır) kendi özel kullanıcı veritabanı tablosu oluşturma zamanı yerine sabit kodlu ele alalım kendisini geçerli kimlik bilgilerini içinde oturum açma sayfasında. Oturum Aç düğmesini ait tıklama olay işleyicisi, aşağıdaki kodu ekleyin:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample5.cs)]

Gördüğünüz gibi üç – Scott, Jisun ve Sam-geçerli kullanıcı hesabı vardır ve üç aynı parolayı ("parola") sahip. Kod için geçerli bir kullanıcı adı ve parola eşleşme isteyen kullanıcılar ve parolalara diziler aracılığıyla döngüde kalır. Kullanıcı adı ve parola geçerliyse, kullanıcı oturum açma ve ardından bunları uygun sayfaya yönlendirmek ihtiyacımız var. Ardından kimlik bilgileri geçersiz olduğunda InvalidCredentialsMessage etiketi gösterilir.

Kullanıcı geçerli kimlik bilgilerini girdiğinde, ardından "uygun sayfaya." yönlendirilirsiniz bahsetmiştim Uygun bir sayfayı ancak nedir? Bir kullanıcı bir sayfa görüntüleme yetkiniz yok ziyaret ettiğinde FormsAuthenticationModule otomatik olarak bunları oturum açma sayfasına yönlendirir olduğunu hatırlayın. Bunun yapılması, istenen URL'de ReturnUrl parametresi ile sorgu dizesini içerir. Diğer bir deyişle, ProtectedPage.aspx ziyaret etmek bir kullanıcı çalıştı ve bunlar Bunu yapmak için yetkileri yok, FormsAuthenticationModule bunları yeniden yönlendirme:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Başarıyla oturum açtıktan sonra kullanıcı için geri ProtectedPage.aspx yönlendirilmesi gerekir. Alternatif olarak, kullanıcılar kendi volition üzerinde oturum açma sayfasını ziyaret edebilirsiniz. Bu durumda, kullanıcı oturum sonra bunlar için kök klasörün Default.aspx sayfasında gönderilmelidir.

### <a name="logging-in-the-user"></a>Kullanıcı oturum

Sağlanan kimlik bilgilerinin geçerli olduğu varsayılırsa, forms kimlik doğrulaması bileti oluşturmak kullanıcıya siteye böylece günlüğü ihtiyacımız var. [FormsAuthentication sınıfı](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) içinde [System.Web.Security ad alanı](https://msdn.microsoft.com/library/system.web.security.aspx) kimlik doğrulama sistemi günlük giriş ve çıkış formlar üzerinden kullanıcı oturum açma için çeşitli yöntemler sağlar. FormsAuthentication sınıfta çeşitli yöntemler varken juncture en bu ilgileniriz üç şunlardır:

- [GetAuthCookie (*kullanıcıadı*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) – forms kimlik doğrulaması bileti için sağlanan adı oluşturur *username*. Ardından, bu yöntem, oluşturur ve kimlik doğrulaması bileti içeriğini tutan HttpCookie nesne döndürür. Varsa *persistCookie* true, kalıcı bir tanımlama bilgisi oluşturulur.
- [SetAuthCookie (*kullanıcıadı*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) – GetAuthCookie çağırır (*kullanıcıadı*, *persistCookie*) forms kimlik doğrulaması tanımlama bilgisi oluşturmak için yöntemi. Bu yöntem, daha sonra (form tanımlama bilgileri tabanlı kimlik doğrulaması; kullanılmıyorsa, bu yöntemin çağırdığı cookieless bilet mantığı işleyen bir iç sınıf varsayılarak) tanımlama bilgileri koleksiyonu GetAuthCookie tarafından döndürülen tanımlama bilgisi ekler.
- [RedirectFromLoginPage (*kullanıcıadı*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) – SetAuthCookie bu yöntemi çağırır (*kullanıcıadı*, *persistCookie*) ve ardından kullanıcıyı uygun sayfaya yönlendirir.

GetAuthCookie tanımlama bilgisi için tanımlama bilgileri koleksiyonu yazmadan önce kimlik doğrulaması bileti değiştirmeniz gerektiğinde kullanışlıdır. Forms kimlik doğrulaması bileti oluşturma ve tanımlama bilgileri koleksiyona eklemek istediğiniz, ancak kullanıcı uygun sayfaya yönlendirmek istemediğiniz SetAuthCookie yararlı olur. Belki de oturum açma sayfasında kalmalarını ya da diğer bazı sayfaya göndermek istersiniz.

Kullanıcının oturumunu açmak ve bunları uygun sayfaya yönlendirmek istiyoruz beri RedirectFromLoginPage kullanalım. Oturum Aç düğmesini'nın tıklatın güncelleştirme olay işleyicisi, aşağıdaki kod satırını ile iki açıklamalı TODO satırları değiştirme:

FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked);

Forms kimlik doğrulaması bileti oluştururken kullanıcıadı metin kutusunun metin özelliği forms kimlik doğrulaması bileti için kullandığımız *kullanıcıadı* parametresi ve RememberMe onay kutusunu işaretli durumu  *persistCookie* parametresi.

Oturum açma sayfasını test etmek için bir tarayıcıda ziyaret edin. "Nope" kullanıcı adı ve parola olarak "yanlış" gibi geçersiz kimlik bilgileri girerek başlayın. Oturum açma düğmesi üzerinde bir geri gönderme ortaya çıkar ve InvalidCredentialsMessage etiketi görüntülenir.


[![InvalidCredentialsMessage etikettir görüntülenen zaman girme geçersiz kimlik bilgileri](an-overview-of-forms-authentication-cs/_static/image25.png)](an-overview-of-forms-authentication-cs/_static/image24.png)

**Şekil 10**: InvalidCredentialsMessage etiket olduğu görüntülenen girme geçersiz kimlik bilgilerini ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image26.png))


Ardından, geçerli kimlik bilgilerini girin ve oturum açma düğmesine tıklayın. Bu süre, forms kimlik doğrulaması bileti geri gönderme gerçekleştiğinde oluşturulur ve geri Default.aspx için otomatik olarak yönlendirilir. Bulunmasına rağmen şu anda oturumunuzun açıldığını belirtmek için hiçbir görsel ipuçları bu noktada, Web sitesine oturum açtı. Adım 4'te program aracılığıyla bir kullanıcı olup olmadığını belirlemek nasıl göreceğiz ya da sayfasını ziyaret ederek kullanıcıyı tanımlamak nasıl yanı sıra günlüğe kaydedilir.

5. adım, bir kullanıcının Web sitesi günlüğü teknikleri inceler.

### <a name="securing-the-login-page"></a>Oturum açma sayfasına güvenliğini sağlama

Kullanıcı kendi kimlik bilgilerini girer ve oturum açma sayfası formunun gönderen – kendi parola dahil olmak üzere – kimlik bilgileri web sunucusuna Internet üzerinden iletilir *düz metin*. Ağ trafiğini algılaması herhangi bir bilgisayar korsanı, kullanıcı adı ve parola görebilirsiniz anlamına gelir. Bunu önlemek için onu kullanarak ağ trafiğini şifrelemek için önemlidir [Güvenli Yuva Katmanı (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Bu kimlik bilgilerini (aynı zamanda tüm sayfanın HTML biçimlendirmeyi) web sunucusu tarafından alınana kadar kullanıcılar tarayıcı bırakın andan şifrelenir garanti eder.

Web sitenizi duyarlı bilgi içermiyorsa, yalnızca burada kullanıcının parolasını yoksa düz metin kablo üzerinden gönderilebilir SSL oturum açma sayfasında ve diğer sayfalarında kullanmanız gerekir. Varsayılan olarak, hem şifrelenmiş ve (üzerinde oynanmasını önlemek için) dijital olarak imzalanmış olduğundan, forms kimlik doğrulaması bileti güvenliğini sağlama hakkında endişelenmeniz gerekmez. Forms kimlik doğrulaması bileti güvenlik üzerinde daha kapsamlı bir tartışma aşağıdaki öğreticide sunulur.

> [!NOTE]
> Finansal ve tıbbi birçok Web sitesi üzerinde SSL kullanmak üzere yapılandırılmış *tüm* sayfaları tarafından erişilebilen kimliği doğrulanmış kullanıcılara. Bu tür bir Web sitesi oluşturuyorsanız forms kimlik doğrulaması bileti yalnızca güvenli bir bağlantı üzerinden aktarılır, böylece form kimlik doğrulama sistemi yapılandırabilirsiniz. Çeşitli forms kimlik doğrulaması yapılandırma seçeneklerini sonraki öğreticide görüneceğini  *[Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular](forms-authentication-configuration-and-advanced-topics-cs.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>4. adım: Kimliği doğrulanmış ziyaretçiler algılama ve kimliklerini belirleme

Bu noktada biz form kimlik doğrulamasını etkinleştirdiniz ve bir ilkel oturum açma sayfası oluşturuldu, ancak ne bir kullanıcı kimliği doğrulanmış veya anonim olduğunu belirleyebiliriz incelemek henüz. Belirli senaryolarda biz farklı veri veya bilgi olup bir kimliği doğrulanmış veya anonim kullanıcı sayfasını ziyaret ederek bağlı olarak görüntülemek isteyebilirsiniz. Ayrıca, biz aktardığınızda genellikle kimliği doğrulanan kullanıcı kimliğini bilmeniz gerekir.

Şimdi bu teknikler göstermek için mevcut Default.aspx sayfasında kullanmasıdır. Default.aspx iki Panel denetimleri, bir adlandırılmış AuthenticatedMessagePanel ve başka bir adlandırılmış AnonymousMessagePanel ekleyin. İlk panelinde WelcomeBackMessage adlı bir etiket denetimi ekleyin. İkinci panelinde bir HyperLink denetimi ekleyin, "Oturum Aç" için metin özelliğini ve kendi NavigateUrl özelliğine "~ / Login.aspx". Bu noktada Default.aspx için bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample6.aspx)]

Artık büyük olasılıkla tahmin gibi buradaki AuthenticatedMessagePanel kimliği doğrulanmış ziyaretçileri ve yalnızca Anonim ziyaretçileri AnonymousMessagePanel görüntülemektir. Bunu gerçekleştirmek için bu panoları bağlı olarak kullanıcı veya oturum görünen özellikleri ayarlamak ihtiyacımız var.

[Request.IsAuthenticated özelliği](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) istek kimliğinin doğrulanıp doğrulanmadığını belirten bir Boole değeri döndürür. Sayfaya aşağıdaki kodu girin\_yük olay işleyici kodu:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample7.cs)]

Bu kod bir yerde bir tarayıcıdan Default.aspx ziyaret edin. Oturum açmak henüz varsayarak, oturum açma sayfasına bir bağlantı göreceksiniz (bkz. Şekil 11). Bu bağlantıya tıklayın ve siteye oturum açın. Adım 3'te gördüğümüz gibi kimlik bilgilerinizi girdikten sonra için Default.aspx döndürülür, ancak bu kez sayfası "Hoş Geldiniz geri!" gösterir. (bkz. Şekil 12) ileti.


![Ziyaret anonim olarak, bir günlük bağlantısını görüntülendiğinde](an-overview-of-forms-authentication-cs/_static/image27.png)

**Şekil 11**: zaman ziyaret anonim olarak, bir günlük bağlantısını görüntülenir


![Kimliği doğrulanmış kullanıcılara gösterilir](an-overview-of-forms-authentication-cs/_static/image28.png)

**Şekil 12**: kimliği doğrulanmış kullanıcılar, "yeniden Hoş Geldiniz!" gösterilir İleti


Şu anda oturum açmış kullanıcının kimliğini aracılığıyla belirleyebiliriz [HttpContext nesne](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)'s [kullanıcı özelliği](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). HttpContext nesnesi, geçerli istek hakkındaki bilgileri temsil eder ve için ortak gibi ASP.NET nesnelerin yanıt, isteğin ve oturumu, diğerlerinin yanı sıra platformdur. Kullanıcı özelliği geçerli HTTP isteği ve uyguladığı güvenlik bağlamını temsil eder [IPrincipal arabirimi](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Kullanıcı özelliği FormsAuthenticationModule tarafından ayarlanır. Özellikle, FormsAuthenticationModule gelen istekte forms kimlik doğrulaması bileti bulduğunda, yeni bir GenericPrincipal nesnesi oluşturur ve kullanıcı özelliğine atar.

Asıl nesneler (gibi GenericPrincipal), kullanıcının kimliğini ve ait oldukları roller üzerinde bilgi sağlar. IPrincipal arabirimi iki üyeleri tanımlar:

- [IPrincipal (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) – asıl belirtilen role ait olup olmadığını gösteren bir Boole değeri döndüren bir yöntem.
- [Kimlik](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) – uygulayan bir nesne döndürür bir özellik [IIdentity arabirimi](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). Üç özellik IIdentity arabirimi tanımlar: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [ısauthenticated durumunda olmasını gerektirir](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), ve [adı](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Aşağıdaki kodu kullanarak geçerli ziyaretçi adını belirleyebiliriz:

currentUsersName dize User.Identity.Name; =

Forms kimlik doğrulaması, kullanma, bir [FormsIdentity nesne](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) GenericPrincipal'ın kimlik özelliği için oluşturulur. FormsIdentity sınıfı "Form" dizesini ısauthenticated durumunda olmasını gerektirir özelliği true ve AuthenticationType özelliği için her zaman döndürür. Name özelliği, forms kimlik doğrulaması bileti oluştururken belirttiğiniz kullanıcı adını döndürür. Bu üç özelliklerine ek olarak, temel kimlik doğrulaması bileti erişimi FormsIdentity içerir, [bilet özelliği](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). Ticket özelliğine türünde bir nesne döndürür [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), sona erme, IsPersistent, IssueDate, adı ve benzeri gibi özelliklere sahiptir.

Aşağıda verilmiştir, çıkardığınız önemli olan nokta *kullanıcıadı* FormsAuthentication.GetAuthCookie içinde belirtilen parametre (*kullanıcıadı*, *persistCookie*), FormsAuthentication.SetAuthCookie (*kullanıcıadı*, *persistCookie*) ve FormsAuthentication.RedirectFromLoginPage (*kullanıcıadı*, *persistCookie*) yöntemleri User.Identity.Name tarafından döndürülen aynı değerdir. Ayrıca, bu yöntemleri ile oluşturulan kimlik doğrulaması bileti User.Identity FormsIdentity nesnesine atama ve ardından Ticket özelliğine erişen kullanılabilir:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample8.cs)]

Şimdi Default.aspx daha kişiselleştirilmiş bir ileti sağlayın. Sayfa güncelleştirmesi\_WelcomeBackMessage etiketin metin özelliği dize atanır, böylece olay işleyicisi yük "tekrar Hoş Geldiniz, *kullanıcıadı*!"

WelcomeBackMessage.Text = "Yeniden Hoş Geldiniz" + User.Identity.Name + "!";

Şekil 13 (Scott kullanıcı olarak oturum açma sırasında) Bu değişiklik etkisini gösterir.


![Hoş Geldiniz iletisi şu anda oturum açmış kullanıcının adı içerir](an-overview-of-forms-authentication-cs/_static/image29.png)

**Şekil 13**: Hoş Geldiniz iletisi şu anda oturum açmış kullanıcının adı içerir


### <a name="using-the-loginview-and-loginname-controls"></a>Bir LoginView ve LoginName denetimleri kullanma

Kimliği doğrulanmış ve anonim kullanıcılar için farklı içerik görüntüleme sık karşılaşılan bir gereksinimdir; Bu nedenle şu anda oturum açmış olan kullanıcının adını görüntülüyor. Bu nedenle, ASP.NET Şekil 13'te, ancak tek satır kod yazmanıza gerek kalmadan gösterilen aynı işlevselliği sağlayan iki Web denetimleri içerir.

[LoginView denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) farklı verileri görüntülemek için kimliği doğrulanmış ve anonim kullanıcılar kolaylaştıran şablon tabanlı bir Web denetimi. Bir LoginView iki önceden tanımlanmış şablonları içerir:

- Anonymous – bu şablona eklediğiniz herhangi bir biçimlendirme yalnızca Anonim ziyaretçilerine görüntülenir.
- LoggedInTemplate – bu şablonun biçimlendirme yalnızca kimliği doğrulanmış kullanıcılara gösterilir.

Bizim sitenin ana sayfasına Site.master LoginView denetimi ekleyelim. Yalnızca LoginView denetimi eklemek, yerine, her iki yeni ContentPlaceHolder denetim ekleyelim ve ardından bu yeni ContentPlaceHolder içinde LoginView denetimi yerleştirin. Kısa bir süre sonra bu kararı stratejinin anlaşılacaktır.

> [!NOTE]
> Anonymous ve LoggedInTemplate ek olarak, role özgü şablonları LoginView denetimi içerebilir. Role özgü şablonları biçimlendirme için belirli bir role ait kullanıcılar gösterilir. Bir sonraki öğreticide LoginView denetimi rol tabanlı özellikleri inceleyeceğiz.


Başlangıç Gezinti içinde ana sayfasına LoginContent adlı bir ContentPlaceHolder ekleyerek &lt;div&gt; öğesi. Yalnızca ContentPlaceHolder denetimi elde edilen biçimlendirme yerleştirme araç kutusundan kaynağı görünümü üzerine sürükleyebilirsiniz üzerinde doğru "TODO: menü gider burada..." metin.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample9.aspx)]

Ardından, bir LoginView denetimi LoginContent ContentPlaceHolder içinde ekleyin. Ana sayfanın ContentPlaceHolder denetimlere yerleştirilen içeriği olarak kabul edilir *varsayılan içerik* ContentPlaceHolder için. Diğer bir deyişle, bu ana sayfanın kullanan ASP.NET sayfaları için her ContentPlaceHolder kendi içeriklerini belirtebilir veya ana sayfanın varsayılan içerik kullanın.

Bir LoginView ve diğer oturum açma ile ilgili denetimler Toolbox'ın oturum açma sekmesinde yer alır.


![Araç kutusunda LoginView denetimi](an-overview-of-forms-authentication-cs/_static/image30.png)

**Şekil 14**: araç kutusunu LoginView denetimi


Ardından, iki ekleyin &lt;br /&gt; LoginView denetimi hemen sonra ancak yine de ContentPlaceHolder içinde öğeleri. Bu noktada, gezinti &lt;div&gt; öğenin biçimlendirme, aşağıdaki gibi görünmelidir:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample10.aspx)]

Tasarımcı veya bildirim temelli biçimlendirme LoginView'ın şablonları tanımlanabilir. Visual Studio Tasarımcısı'ndan bir açılan listedeki yapılandırılmış şablonları listeler LoginView'ın akıllı etiket genişletin. Metin türü ", stranger Anonymous; Hello" Ardından, bir HyperLink denetimi ekleyin ve "Oturum Aç" için metin ve NavigateUrl özelliklerini ayarlayın ve "~ / Login.aspx", sırasıyla.

Anonymous yapılandırdıktan için LoggedInTemplate geçin ve "Yeniden Hoş Geldiniz," metin girin. Ardından bir LoginName denetimi araç kutusundan hemen "Hoş Geldiniz sonra geri" metin yerleştirme LoggedInTemplate içine sürükleyin. [LoginName denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), adından da anlaşılacağı, şu anda oturum açmış olan kullanıcının adını görüntüler. Dahili olarak, LoginName denetimi User.Identity.Name özelliği yalnızca çıkarır

Bu eklemeler LoginView'ın şablonları yaptıktan sonra biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample11.aspx)]

Bu ekleme Site.master ana sayfaya her Web sayfasında kullanıcının kimliği doğrulanır olup olmadığına bağlı olarak farklı bir ileti görüntüler. Şekil 15 bir tarayıcıdan Jisun kullanıcı tarafından ziyaret edildiğinde Default.aspx sayfasında gösterilir. "Tekrar, Hoş Geldiniz Jisun" ileti iki kez yinelenir: Default.aspx'ın içinde bir kez (aracılığıyla eklediğimiz yöntemlerin LoginView denetimi) sol taraftaki ana sayfa gezinti bölümde de içerik alanının (aracılığıyla Panel denetimleri ve programlama mantığını).


![Bir LoginView denetimi görüntüler](an-overview-of-forms-authentication-cs/_static/image31.png)

**Şekil 15**: LoginView denetimi görüntüler "Hoş Geldiniz geri Jisun."


Ana sayfaya LoginView ekledik çünkü her sayfada sitemizi görünebilir. Ancak, olabilir web sayfaları bu iletiyi göstermek için istediğimiz yok. Oturum açma sayfasının bağlantısı dışında yer yok gibi görünüyor. bu yana bir sayfa oturum açma sayfasında ' dir. Biz LoginView denetimi bir ContentPlaceHolder ana sayfasına yerleştirilen olduğundan, bu varsayılan biçimlendirme içerik sayfamızı kılabilirsiniz. Bu ancak açın ve Tasarımcı'ya gidin. Biz açıkça bir içerik denetimi tanımlamadığınız beri için ana sayfasında LoginContent ContentPlaceHolder Login.aspx içinde oturum açma sayfası için bu ContentPlaceHolder ana sayfanın varsayılan biçimlendirme gösterir. Bu varsayılan biçimlendirme (LoginView denetimi) LoginContent ContentPlaceHolder gösterilmektedir Tasarımcısı – görebilirsiniz.


[![Oturum açma sayfasına varsayılan ana sayfanın LoginContent ContentPlaceHolder için içerik gösterir](an-overview-of-forms-authentication-cs/_static/image33.png)](an-overview-of-forms-authentication-cs/_static/image32.png)

**Şekil 16**: oturum açma sayfasına içerik varsayılan ana sayfanın LoginContent ContentPlaceHolder için gösterir ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image34.png))


Varsayılan biçimlendirme LoginContent ContentPlaceHolder için geçersiz kılmak için tasarımcı bölgede sağ tıklayın ve bağlam menüsünden özel içerik oluşturma seçeneğini seçin. (Visual Studio 2008 ContentPlaceHolder kullanarak içerdiğinde bir akıllı etiket, seçili olduğunda, aynı seçeneği sunar.) Bu yeni bir içerik denetimi sayfa biçimlendirmesi ve dolayısıyla ekler için bu sayfaya özel içeriği tanımlayan olanak sağlıyor. "Lütfen oturum oturum gibi", burada özel bir ileti ekleyerek ancak şimdi yalnızca bu alanı boş bırakın.

> [!NOTE]
> Visual Studio 2005'te özel içerik oluşturma oluşturur boş bir ASP.NET sayfasını denetiminde içerik. Visual Studio 2008'de, ancak, özel içerik oluşturma ana sayfanın varsayılan içerik yeni oluşturulan içerik denetimine kopyalar. Visual Studio 2008 kullanıyorsanız, daha sonra yeni içerik denetimi oluşturduktan sonra üzerinden ana sayfasından kopyalanan içeriği temizlemek emin olun.


Şekil 17 bu değişikliği yaptıktan sonra bir tarayıcısından ziyaret edildiğinde Login.aspx sayfasına gösterir. Hiçbir ", stranger Hello" olduğuna dikkat edin veya "tekrar Hoş Geldiniz, *kullanıcıadı*" sol gezinti bölmesindeki ileti &lt;div&gt; Default.aspx ziyaret olduğundan.


[![Oturum açma sayfasına varsayılan LoginContent ContentPlaceHolder'ın işaretleme gizler.](an-overview-of-forms-authentication-cs/_static/image36.png)](an-overview-of-forms-authentication-cs/_static/image35.png)

**Şekil 17**: oturum açma sayfasına varsayılan LoginContent ContentPlaceHolder'ın işaretleme gizliyor ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image37.png))


## <a name="step-5-logging-out"></a>Adım 5: Oturum kapatılıyor

Adım 3'te bir kullanıcının sitede oturum açmak için bir oturum açma sayfası oluşturmayı olan incelemiştik, ancak bir kullanıcı oturumu kapat öğrenmek henüz. Bir kullanıcı oturum yöntemlerine ek olarak, FormsAuthentication sınıfı sağlar bir [SignOut yöntemi](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). SignOut yöntemi yeterlidir, böylece kullanıcı site dışında günlüğü forms kimlik doğrulaması bileti yok eder.

ASP.NET, ortak bir özellik bir oturumu kapatma bağlantı teklifidir bir kullanıcı oturumu özel olarak tasarlanmış bir denetimi içerir. [LoginStatus denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) "Login" LinkButton ya da kullanıcının kimlik doğrulama durumuna bağlı olarak bir "Logout" LinkButton görüntüler. Kimliği doğrulanmış kullanıcılara görüntülenen "Logout" LinkButton ise "Oturum açma" LinkButton anonim kullanıcılar için işlenir. Metin "Login" ve "Logout" LinkButtons LoginStatus kişinin yapılandırılabilir LoginText ve LogoutText özellikleri.

"Login" LinkButton tıklayarak bir yeniden yönlendirme oturum açma sayfasına verildiği geri göndermenin neden olur. "Logout" LinkButton tıklayarak FormsAuthentication.SignOff yöntemini çağırmak LoginStatus denetimi neden olur ve ardından kullanıcı bir sayfasına yönlendirir. Sayfa oturum açmış kullanıcının oturumunu bağlıdır üç aşağıdaki değerlerden birine atanabilir özellikte LogoutAction yönlendirilir:

- – Varsayılan yenileme; Kullanıcı yalnızca ziyaret sayfasına yönlendirir. Ardından yalnızca ziyaret sayfasında anonim kullanıcılara izin vermediği durumlarda FormsAuthenticationModule kullanıcıyı otomatik olarak oturum açma sayfasına yönlendirir.

Neden bir yeniden yönlendirme burada gerçekleştirilir dair merak olabilir. Kullanıcı aynı sayfada kalmak isterse neden açık yeniden yönlendirme gerek? "Oturumu Kapat" LinkButton tıklandığında, kullanıcı hala forms kimlik doğrulaması bileti tanımlama bilgileri koleksiyonu içinde olduğundan nedenidir. Sonuç olarak, geri gönderme isteği kimliği doğrulanmış bir istektir. Bu denetim oturum kapatma yöntemini çağırır, ancak FormsAuthenticationModule kullanıcının kimliği doğrulandıktan sonra olur. Bu nedenle, bir açık yeniden yönlendirme, tarayıcı sayfasını yeniden istemek neden olur. Tarayıcı sayfa yeniden istekleri zamanına göre forms kimlik doğrulaması bileti kaldırıldı ve bu nedenle gelen istek anonimdir.

- Yeniden yönlendirme – kullanıcı LoginStatus'ın LogoutPageUrl özelliği tarafından belirtilen URL'ye yeniden yönlendirilir.
- RedirectToLoginPage – kullanıcı, oturum açma sayfasına yönlendirilir.

Şimdi bir LoginStatus denetimi için ana sayfaya ekleyin ve bunlar imzalanmış olduğunu onaylayan bir ileti görüntüleyen bir sayfa kullanıcı göndermek için yeniden yönlendirme seçeneği kullanacak şekilde yapılandırın. Bir sayfa Logout.aspx adlı kök dizininde oluşturarak başlayın. Bu sayfa Site.master ana sayfayla ilişkilendirilecek unutmayın. Ardından, sayfanın biçimlendirme bunlar kapattınız kullanıcıya açıklayan bir ileti girin.

Ardından, Site.master ana sayfasına dönün ve LoginContent ContentPlaceHolder ' LoginView altındaki bir LoginStatus denetimi ekleyin. Yeniden yönlendirme LoginStatus denetimin LogoutAction özelliğini ve kendi LogoutPageUrl özelliğini ayarlama "~ / Logout.aspx".

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample12.aspx)]

Bir LoginStatus LoginView denetimi dışında olduğundan, anonim ve kimliği doğrulanmış kullanıcılar için görünür, ancak LoginStatus "Login" veya "Logout" LinkButton düzgün görüntülenmesi için bu normaldir. LoginStatus denetimi ekleyerek, Anonymous "Oturum Aç" Köprü gereksiz, bu nedenle kaldırın.

Jisun ziyaret ettiğinde Şekil 18 Default.aspx gösterir. Sol sütunda "geri Jisun oturumu bağlantısını birlikte Hoş Geldiniz" iletisi görüntülenir. Oturumu kapatma LinkButton tıklayarak geri göndermeye neden olur, sistemin dışında Jisun imzalar ve her Logout.aspx için yeniden yönlendirir. Önceden imzalanmış ve bu nedenle anonimdir Logout.aspx Jisun ulaştığında zamanında şekil 19 gösterildiği gibi. Sonuç olarak, metnin sol sütununda gösterilir ", stranger ve Hoş Geldiniz" oturum açma sayfasının bağlantısı.


[![Default.aspx gösterir](an-overview-of-forms-authentication-cs/_static/image39.png)](an-overview-of-forms-authentication-cs/_static/image38.png)

**Şekil 18**: Default.aspx gösterilir "Hoş Geldiniz geri Jisun" ile birlikte bir "Logout" LinkButton ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image40.png))


[![Logout.aspx Shows](an-overview-of-forms-authentication-cs/_static/image42.png)](an-overview-of-forms-authentication-cs/_static/image41.png)

**Şekil 19**: Logout.aspx gösterilir "Hoş Geldiniz, stranger" ile birlikte bir "Oturum açma" LinkButton ([tam boyutlu görüntüyü görmek için tıklatın](an-overview-of-forms-authentication-cs/_static/image43.png))


> [!NOTE]
> (Adım 4'te Login.aspx için yaptığımız gibi) ana sayfanın LoginContent ContentPlaceHolder gizlemek için Logout.aspx sayfasını özelleştirme geçmenizi öneriyoruz. "Login" LinkButton LoginStatus denetimi tarafından işlenen neden olduğundan (altındaki bir ", stranger Hello") geçerli URL ReturnUrl querystring parametresi geçirerek oturum açma sayfası kullanıcı gönderir. Kısacası, bunlar oturum açan çıkış bir kullanıcı bu LoginStatus'ın "Login" LinkButton ve ardından günlüklerinde tıklarsa, hangi kullanıcı kolayca karıştırılabilir geri Logout.aspx için yönlendirilirsiniz.


## <a name="summary"></a>Özet

Bu öğreticide form kimlik doğrulama iş akışı bir incelenmesi çalışmaya ve ardından bir ASP.NET uygulamasında form kimlik doğrulaması uygulamak için açılır. Form kimlik doğrulaması iki sorumlulukları vardır FormsAuthenticationModule tarafından desteklenir:, forms kimlik doğrulaması bileti üzerinde temel kullanıcıları tanımlama ve yetkisiz kullanıcıların oturum açma sayfasına yeniden yönlendiriliyorsunuz.

.NET Framework'ün FormsAuthentication sınıfı oluşturma, inceleme ve form kimlik doğrulama biletlerini kaldırma yöntemleri içerir. Kullanıcı nesnesi ve Request.IsAuthenticated özelliği bir istek olup olmadığı doğrulanır ve kullanıcının kimlik bilgilerini belirlemek için ek programlama desteği sağlar. Geliştiricilere oturum açma ile ilgili birçok ortak görevleri gerçekleştirmek için hızlı ve Kodsuz bir yol sağlar. LoginView LoginStatus ve LoginName Web denetimleri vardır. Sonraki öğreticilerde bu ve diğer oturum açma ile ilgili Web denetimleri daha ayrıntılı olarak inceleyeceğiz.

Bu öğreticide, form kimlik doğrulaması gelişigüzel bir genel bakış sağlanır. Biz değil çeşitli yapılandırma seçenekleri inceleyin, nasıl cookieless forms kimlik doğrulaması bilet iş arayın veya ASP.NET formları kimlik doğrulama anahtarının içeriğini nasıl koruduğunu keşfedin. Bu konu başlıkları ve daha fazlasını ele alınacaktır [sonraki öğreticiye](forms-authentication-configuration-and-advanced-topics-cs.md).

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [IIS6 IIS7 güvenlik arasındaki değişiklikleri](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Oturum açma ASP.NET denetimleri](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Profesyonel ASP.NET 2.0 güvenlik, üyelik ve rol yönetimi](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [`<authentication>` Öğesi](https://msdn.microsoft.com/library/532aee0e.aspx)
- [`<forms>` Öğesi için `<authentication>`](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Bu öğreticide yer alan konularda eğitim videosu

- [ASP.NET’te Temel Forms Kimlik Doğrulaması Kullanma](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel performanstan...

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı Gözden Geçiren, Bu öğretici serisinin birçok yararlı Gözden Geçiren tarafından gözden geçirildi oluştu. Bu öğretici için müşteri adayı gözden geçirenler Alicja Maziarz ve John Suru Teresa Murphy içerir. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](security-basics-and-asp-net-support-cs.md)
> [İleri](forms-authentication-configuration-and-advanced-topics-cs.md)
