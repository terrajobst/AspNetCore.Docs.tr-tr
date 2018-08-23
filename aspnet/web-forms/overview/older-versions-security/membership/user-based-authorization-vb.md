---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-vb
title: Kullanıcı tabanlı yetkilendirme (VB) | Microsoft Docs
author: rick-anderson
description: Bu öğreticide sayfalarına erişimi sınırlandırma ve çeşitli teknikler aracılığıyla sayfa düzeyi işlevselliği kısıtlayarak atacağız.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: bc937e9d-5c14-4fc4-aec7-440da924dd18
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 263b421cbce68cbc9a596e40a6be4ff140edc0d4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755642"
---
<a name="user-based-authorization-vb"></a>Kullanıcı tabanlı yetkilendirme (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_vb.pdf)

> Bu öğreticide sayfalarına erişimi sınırlandırma ve çeşitli teknikler aracılığıyla sayfa düzeyi işlevselliği kısıtlayarak atacağız.


## <a name="introduction"></a>Giriş

Kullanıcı hesaplarını teklif çoğu web uygulaması bölümünde belirli ziyaretçiler site belirli sayfalara erişimini kısıtlamak için bunu yapın. Birçok çevrimiçi messageboard siteler, örneğin, tüm kullanıcılar - anonim ve kimliği doğrulanmış - messageboard'ın gönderileri görüntüleyebilir, ancak yalnızca kimliği doğrulanmış kullanıcıların yeni bir gönderi oluşturmak için web sayfasını ziyaret edebilirsiniz. Ve yalnızca belirli bir kullanıcı (veya belirli bir kullanıcı kümesini) erişilebilen yönetim sayfaları olabilir. Ayrıca, sayfa düzeyi işlevi, bir kullanıcı tarafından temelinde farklı olabilir. Bu arabirim anonim ziyaretçilere değildir, ancak gönderilerinin listesini görüntülerken, kimliği doğrulanmış kullanıcılar her bir gönderi değerlendirme için bir arabirim gösterilir.

ASP.NET kullanıcı tabanlı yetkilendirme kurallarını tanımlamak kolaylaştırır. Yalnızca bir bit biçimlendirme içinde `Web.config`, belirli web sayfaları veya tüm dizinleri kilitlenebilir aşağı yalnızca belirtilen bir kullanıcı alt erişilebilir ve böylelikle. Sayfa düzeyi işlevselliği açıp programlı ve bildirim temelli bulunamazsınız şu anda oturum açmış olan kullanıcının temel alınarak kapatılabilir.

Bu öğreticide sayfalarına erişimi sınırlandırma ve çeşitli teknikler aracılığıyla sayfa düzeyi işlevselliği kısıtlayarak atacağız. Haydi başlayalım!

## <a name="a-look-at-the-url-authorization-workflow"></a>URL yetkilendirme iş akışı bakma

Bölümünde açıklandığı gibi [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-vb.md) öğretici, ASP.NET çalışma zamanı bir isteği işlerken için bir ASP.NET kaynak isteği olay sayısı sırasında yaşam döngüsü başlatır. *HTTP modüllerinden* yönetilen sınıflar, kod, istek yaşam döngüsü içinde belirli bir olaya yanıt olarak yürütülür. ASP.NET, birkaç önemli görevleri arka planda gerçekleştirmek HTTP Modülleri ile birlikte gelir.

Bir HTTP modülü [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Önceki öğreticilerde, sunucunun birincil işlevi açıklandığı gibi `FormsAuthenticationModule` geçerli isteğin kimliğini belirlemektir. Bu, bir tanımlama bilgisinde bulunan veya URL içinde gömülü forms kimlik doğrulaması bileti inceleyerek gerçekleştirilir. Bu kimliği sırasında gerçekleşir [ `AuthenticateRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Başka bir önemli HTTP modülü [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), yanıt olarak harekete [ `AuthorizeRequest` olay](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (sonra gerçekleşir `AuthenticateRequest` olay). `UrlAuthorizationModule` Yapılandırma işaretlemede inceler `Web.config` geçerli kimliğini belirtilen sayfayı ziyaret etmek için yetkili olup olmadığını belirlemek için. Bu işlem olarak adlandırılır *URL yetkilendirmesi*.

1. adımında, URL yetkilendirme kuralları için söz dizimi ancak daha önce şimdi inceleyeceğiz visualize Ara `UrlAuthorizationModule` istek veya yetkili bağlı olarak yapar. Varsa `UrlAuthorizationModule` isteğin yetkilendirilip sonra hiçbir şey yapmaz ve isteği yaşam döngüsü devam belirler. Ancak, istek *değil* yetki sonra `UrlAuthorizationModule` yaşam döngüsü durdurur ve bildirir `Response` döndürülecek nesne bir [HTTP 401 Yetkisiz](http://www.checkupdown.com/status/E401.html) durumu. Forms kimlik doğrulaması kullanırken bu HTTP 401 durum hiçbir zaman istemciye göremediğinden döndürülür, `FormsAuthenticationModule` bir HTTP durum 401 değiştirir kendisine algılar bir [HTTP 302 yeniden yönlendirme](http://www.checkupdown.com/status/E302.html) oturum açma sayfası.

Şekil 1, iş akışını ve ASP.NET ardışık düzenini gösterir `FormsAuthenticationModule`ve `UrlAuthorizationModule` yetkisiz bir istek geldiğinde. Özellikle, Şekil 1 tarafından anonim bir ziyaretçi için bir istek gösterir `ProtectedPage.aspx`, anonim kullanıcılar için erişimi engelleyen bir sayfa olduğu. Ziyaretçi anonim olduğundan `UrlAuthorizationModule` isteğini durdurur ve bir HTTP 401 yetkilendirilmedi durum döndürür. `FormsAuthenticationModule` Oturum açma sayfasının 302 yeniden yönlendirme 401 durum dönüştürür. Kullanıcı oturum açma sayfası doğrulandıktan sonra kendisi için yönlendirilir `ProtectedPage.aspx`. Bu süre `FormsAuthenticationModule` kullanıcının kendi kimlik doğrulama anahtarı üzerinde tanımlar. Ziyaretçi doğrulanır, `UrlAuthorizationModule` sayfasına erişme izni verir.


[![Form kimlik doğrulaması ve URL yetkilendirme iş akışı](user-based-authorization-vb/_static/image2.png)](user-based-authorization-vb/_static/image1.png)

**Şekil 1**: form kimlik doğrulaması ve URL yetkilendirme iş akışı ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image3.png))


Şekil 1, anonim bir ziyaretçi, anonim kullanıcılar için kullanılabilir olmayan bir kaynağa erişmeye çalıştığında oluşan etkileşimler gösterilmektedir. Böyle bir durumda, anonim ziyaretçi Filiz sorgu dizesinde belirtilen ziyaret denediğiniz sayfa ile oturum açma sayfasına yönlendirilir. Kullanıcı başarıyla oturum açtıktan sonra o otomatik olarak Filiz görüntülemek için başlangıçta çalışıyordu kaynağa geri yönlendirilirsiniz.

Anonim kullanıcılar tarafından yetkisiz istek yapıldığında, bu iş akışı oldukça basittir ve ziyaretçi ne olduğunu anlamak kolaydır ve neden. Ancak, şunları aklınızda tutun içinde `FormsAuthenticationModule` yönlendireceği *herhangi* bile tarafından kimliği doğrulanmış bir kullanıcı bir istekte kullanıcı oturum açma sayfasına yetkisiz. Kimliği doğrulanmış bir kullanıcının kendisi yetkilisi bulunmayan bir sayfayı ziyaret etmek çalışırsa bu kafa karıştırıcı bir kullanıcı deneyimi neden olabilir.

Web URL yetkilendirme kurallarını yapılandırılmış olduğunu hayal gibi ASP.NET sayfasını `OnlyTito.aspx` accessibly yalnızca Tito için oluştu. Şimdi, Sam sitesini ziyaret, oturum açan ve ziyaret dener Imagine `OnlyTito.aspx`. `UrlAuthorizationModule` Durdurma isteği yaşam döngüsünün ve bir HTTP 401 yetkilendirilmedi durum döndürür, `FormsAuthenticationModule` algılar ve ardından Sam oturum açma sayfasına yönlendir. SAM zaten oturum açtıktan sonra Filiz oturum açma sayfasına Filiz neden gönderildi merak ediyor olabilirsiniz. Filiz, her oturum açma kimlik bilgileri şekilde kayıp veya Filiz geçersiz kimlik bilgileri girilen neden. Oturum açma sayfasından kendi kimlik bilgilerini SAM çıkıldığında Filiz (yeniden) olarak oturum açmış ve yeniden yönlendirileceği `OnlyTito.aspx`. `UrlAuthorizationModule` Sam olamaz bu sayfayı ziyaret edin ve oturum açma sayfasına Filiz döndürülecek algılar.

Şekil 2, bu karmaşık iş akışı gösterilmektedir.


[![Varsayılan iş akışı için kafa karıştırıcı bir döngüye neden olabilir](user-based-authorization-vb/_static/image5.png)](user-based-authorization-vb/_static/image4.png)

**Şekil 2**: varsayılan iş akışı açabilir kafa karıştırıcı bir döngüsü ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image6.png))


Şekil 2'de gösterilen iş akışı, hızlı bir şekilde bile çoğu bilgisayar deneyimli ziyaretçi befuddle. 2. adım döngüsünde kafa karıştırıcı Bunu önlemenin yolu atacağız.

> [!NOTE]
> ASP.NET, geçerli kullanıcının belirli bir web sayfasına erişip erişemeyeceğini belirlemek için iki mekanizma kullanır: URL yetkilendirmesi ve dosya yetkilendirme. Dosya Yetkilendirme tarafından gerçekleştirilen [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), istenen dosyaları ACL'leri consulting tarafından yetkilisi belirler. ACL'ler Windows hesapları için geçerli izinleri olduğundan dosya yetkilendirmesi, Windows kimlik doğrulaması ile en yaygın olarak kullanılır. Forms kimlik doğrulaması kullanırken, tüm işletim sistemi ve dosya sistemi düzeyinde istekleri sitesinden kullanıcı bağımsız olarak aynı Windows hesabı tarafından yürütülür. Bu öğretici serisinde, form kimlik doğrulamasını odaklanır olduğundan, biz dosya yetkilendirmesi görüştükten değil.


### <a name="the-scope-of-url-authorization"></a>URL yetkilendirme kapsamı

`UrlAuthorizationModule` Olan yönetilen koddan ASP.NET çalışma zamanı bir parçasıdır. Microsoft'un sürümünden önceki 7 [Internet Information Services (IIS)](https://www.iis.net/) web sunucusu IIS HTTP ardışık düzen ve ASP.NET çalışma zamanının işlem hattı arasındaki farklı bir engel oldu. Kısacası, IIS 6 ve önceki sürümlerinde, ASP. NET `UrlAuthorizationModule` yalnızca IIS ASP.NET çalışma zamanı için bir istek aktarıldığında yürütür. Varsayılan olarak, IIS statik içeriğin kendisini - HTML sayfaları, CSS, JavaScript ve görüntü dosyaları - gibi işler ve yalnızca izin isteklerini bir uzantıya sahip bir sayfa, ASP.NET çalışma zamanı uygulamalı `.aspx`, `.asmx`, veya `.ashx` istenir.

IIS 7'de, işlem hatları ancak tümleşik IIS ve ASP.NET için izin verir. IIS 7'ı çağırmak için birkaç yapılandırma ayarlarıyla ayarlayabilirsiniz `UrlAuthorizationModule` için *tüm* istekleri, yani URL yetkilendirme kuralları için herhangi bir türde dosyalar tanımlanabilir. Ayrıca, IIS 7 kendi URL yetkilendirme altyapısı içerir. ASP.NET tümleştirmesi ve IIS 7'ın yerel URL yetkilendirme işlevselliği hakkında daha fazla bilgi için bkz. [anlama IIS7 URL yetkilendirmesi](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Shahram Khosravi'nın kitabın bir kopya için daha ayrıntılı bir görünüm, ASP.NET ve IIS 7 tümleştirme çekme *Professional IIS 7 ve ASP.NET tümleşik programlama* (ISBN: 978 0470152539).

Buna koysalar önce IIS 7 sürümlerinde, URL yetkilendirme kuralları yalnızca ASP.NET çalışma zamanı tarafından işlenen kaynaklara uygulanır. Ancak IIS 7 ile IIS yerel URL yetkilendirme özelliğini kullanmayı veya ASP tümleştirmek için mümkündür. NET `UrlAuthorizationModule` IIS HTTP ardışık düzene başvurulabilir, böylece tüm istekler için bu işlevi olur.

> [!NOTE]
> Nasıl bazı küçük ancak önemli farklar vardır ASP. NET `UrlAuthorizationModule` ve IIS 7'in URL yetkilendirme özelliği yetkilendirme kurallarını işleme. Bu öğreticide, IIS 7'in URL yetkilendirme işlevselliği ya da bunu karşılaştırıldığında yetkilendirme kuralları nasıl ayrıştırır fark incelemez `UrlAuthorizationModule`. Bu konular hakkında daha fazla bilgi için MSDN'de veya, IIS 7 belgelerine başvurun [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>1. adım: URL yetkilendirme kuralları tanımlama`Web.config`

`UrlAuthorizationModule` Uygulama yapılandırmasında tanımlı URL yetkilendirme kurallarına göre belirli bir kimlik için istenen kaynağa erişim vermek veya reddetmek belirler. Yetkilendirme kuralları, İl [ `<authorization>` öğesi](https://msdn.microsoft.com/library/8d82143t.aspx) biçiminde `<allow>` ve `<deny>` alt öğeleri. Her `<allow>` ve `<deny>` alt öğesi belirtebilirsiniz:

- Belirli bir kullanıcı
- Kullanıcıları içeren bir virgülle ayrılmış liste
- Bir soru işareti (?) tarafından belirtilen tüm anonim kullanıcılar
- Bir yıldız işaretiyle gösterilen tüm kullanıcılar (\*)

Aşağıdaki biçimlendirmede Tito ve Scott kullanıcıların ve diğerlerini reddetme URL yetkilendirme kuralları nasıl kullanıldığı gösterilmektedir:

[!code-xml[Main](user-based-authorization-vb/samples/sample1.xml)]

`<allow>` Öğe tanımlar hangi kullanıcıların - Tito ve Scott - izin verilir ancak `<deny>` öğe bildirir *tüm* kullanıcılar engellenir.

> [!NOTE]
> `<allow>` Ve `<deny>` öğeleri rolleri için yetkilendirme kuralları da belirtebilirsiniz. Rol tabanlı yetkilendirme bir sonraki öğreticide inceleyeceğiz.


Şu ayar erişim Sam (anonim ziyaretçiler dahil) dışındaki bir kişiye verir:

[!code-xml[Main](user-based-authorization-vb/samples/sample2.xml)]

Yalnızca kimliği doğrulanmış kullanıcılara izin vermek için tüm anonim kullanıcılar için erişimini engellediği aşağıdaki yapılandırmayı kullanın:

[!code-xml[Main](user-based-authorization-vb/samples/sample3.xml)]

Yetkilendirme kuralları içinde tanımlanan `<system.web>` öğesinde `Web.config` ve tüm web uygulamasındaki ASP.NET kaynakları için geçerlidir. Aktardığınızda genellikle, bir uygulamanın farklı bölümleri için farklı bir yetkilendirme kuralları vardır. Örneğin, bir e-ticaret sitesinde tüm ziyaretçiler ürünleri tekrar kullanmanıza, ürün incelemeleri görmek, katalogda arama ve benzeri. Ancak, yalnızca kimliği doğrulanmış kullanıcıların kullanıma alma veya birinin sevkiyat geçmişini yönetmek için sayfa ulaşabilir. Üstelik, yalnızca site Yöneticiler gibi belirli kullanıcılar tarafından erişilebilir olan bazı bölümleri site olabilir.

ASP.NET, sitedeki farklı dosyalar ve klasörler için farklı bir yetkilendirme kuralları tanımlamak kolaylaştırır. Kök klasör belirtilen yetkilendirme kuralları `Web.config` dosya geçerli sitedeki tüm ASP.NET kaynaklara. Ancak, bu varsayılan yetkilendirme ayarları için belirli bir klasöre ekleyerek geçersiz kılınabilir bir `Web.config` ile bir `<authorization>` bölümü.

Yalnızca kimliği doğrulanmış kullanıcılar ASP.NET sayfaları ziyaret edebilir, böylece Web sitemizi güncelleştirelim `Membership` klasör. İhtiyacımız eklemek için bunu sağlamak için bir `Web.config` dosyasını `Membership` klasörü ve anonim kullanıcılar reddetmeye yönelik yetkilendirme ayarlarını ayarlayın. Sağ `Membership` Çözüm Gezgini'nde klasörü yeni öğe Ekle menüsü bağlam menüsünden seçin ve adlı yeni bir Web yapılandırma dosyası ekleme `Web.config`.


[![Üyelik klasöre bir Web.config dosyası ekleyin](user-based-authorization-vb/_static/image8.png)](user-based-authorization-vb/_static/image7.png)

**Şekil 3**: ekleme bir `Web.config` dosyasını `Membership` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image9.png))


Bu noktada, projenizin iki içermelidir `Web.config` dosyaları: biri kök dizin, diğeri de `Membership` klasör.


[![Uygulamanızı şimdi iki Web.config dosyası içermelidir](user-based-authorization-vb/_static/image11.png)](user-based-authorization-vb/_static/image10.png)

**Şekil 4**: bilgisayarınızı uygulaması gereken artık içeren iki `Web.config` dosyaları ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image12.png))


Yapılandırma dosyasında güncelleştirme `Membership` olan anonim kullanıcılara erişimi engeller. Bu nedenle bir klasör.

[!code-xml[Main](user-based-authorization-vb/samples/sample4.xml)]

Tüm İşte bu kadar kolay!

Bu değişiklik test etmek için bir tarayıcıda giriş sayfasını ziyaret edin ve oturum emin olun. Tüm ziyaretçilerin izin vermek için bir ASP.NET uygulaması'nın varsayılan davranışını olduğundan ve biz kök dizinin herhangi bir yetkilendirme değişiklik yapmadım beri `Web.config` dosya, biz, anonim bir ziyaretçi olarak kök dizindeki dosyaların ziyaret etmek kullanabilirsiniz.

Sol sütunda bulunan kullanıcı hesapları oluşturma bağlantısına tıklayın. Bu gideceksiniz `~/Membership/CreatingUserAccounts.aspx`. Bu yana `Web.config` dosyası `Membership` klasör anonim erişimi engellemek için yetkilendirme kuralları tanımlar `UrlAuthorizationModule` isteğini durdurur ve bir HTTP 401 yetkilendirilmedi durum döndürür. `FormsAuthenticationModule` Bu oturum açma sayfasına gönderdiğiniz bir 302 yeniden yönlendirme durum değiştirir. Sayfa biz erişmeye çalıştığınız olduğunu unutmayın (`CreatingUserAccounts.aspx`) oturum açma sayfası geçirilen `ReturnUrl` sorgu dizesi parametresi.


[![URL yetkilendirme kuralları engelle anonim erişimi beri biz oturum açma sayfasına yönlendirilirsiniz](user-based-authorization-vb/_static/image14.png)](user-based-authorization-vb/_static/image13.png)

**Şekil 5**: URL yetkilendirme kuralları anonim erişimi engelle olduğundan, biz oturum açma sayfasına yönlendirilir ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image15.png))


Başarıyla oturum açtıktan sonra biz yönlendirilirsiniz `CreatingUserAccounts.aspx` sayfası. Bu süre `UrlAuthorizationModule` biz artık anonim olmadığı için sayfa erişme izni verir.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Belirli bir konuma URL yetkilendirme kuralları uygulama

Tanımlanan yetkilendirme ayarları `<system.web>` bölümünü `Web.config` Bu dizindeki ve alt dizinlerinde tüm ASP.NET kaynakları için geçerlidir (bir başkası tarafından kılınmadıkları kadar `Web.config` dosya). Bazı durumlarda, ancak biz tüm ASP.NET kaynakları belirli bir dizinde bir veya iki belirli bir sayfa dışında bir özel yetkilendirme yapılandırmasına sahip olmak isteyebilirsiniz. Bu ekleyerek gerçekleştirilebilir bir `<location>` öğesinde `Web.config`, yetkilendirme kuralları farklı bir dosyaya işaret ve sıralamadaki benzersiz yetkilendirme kurallarını tanımlama.

Kullanarak göstermek için `<location>` belirli bir kaynak için yapılandırma ayarları geçersiz kılar, yalnızca Tito ziyaret edebilir, böylece şimdi yetkilendirme ayarlarını özelleştirmek için öğe `CreatingUserAccounts.aspx`. Bunu gerçekleştirmek için ekleme bir `<location>` öğesine `Membership` klasörün `Web.config` dosya ve aşağıdaki gibi görünüyor. böylece, biçimlendirme güncelleştirin:

[!code-xml[Main](user-based-authorization-vb/samples/sample5.xml)]

`<authorization>` Öğesinde `<system.web>` ASP.NET kaynaklar için varsayılan URL yetkilendirme kuralları tanımlar; `Membership` klasör ve alt klasörleri. `<location>` Öğesi bize bu kuralları belirli bir kaynak için geçersiz kılmak sağlar. Yukarıdaki biçimlendirmede `<location>` öğesi başvuruları `CreatingUserAccounts.aspx` sayfasında ve kendi yetkilendirme gibi Tito, izin verecek kuralları belirtir ancak Reddet diğer herkes.

Bu yetkilendirme değişiklik test etmek için anonim kullanıcı olarak Web sitesini ziyaret ederek başlayın. Herhangi bir sayfayı ziyaret etmek çalışırsanız `Membership` klasörü gibi `UserBasedAuthorization.aspx`, `UrlAuthorizationModule` isteği reddeder ve oturum açma sayfasına yönlendirilir. Söyleyin, Scott, oturum açtıktan sonra herhangi bir sayfayı ziyaret edebilirsiniz `Membership` klasör *dışında* için `CreatingUserAccounts.aspx`. Ziyaret etmeye çalışırken `CreatingUserAccounts.aspx` herkes oturum açmış ancak Tito, yetkisiz erişim girişimi oturum açma sayfasına yönlendirerek sonuçlanacak.

> [!NOTE]
> `<location>` Öğesi yapılandırma dışında görünmelidir `<system.web>` öğesi. Ayrı bir kullanmanız gereken `<location>` yetkilendirme ayarlarını geçersiz kılmak istediğiniz her kaynak için öğesi.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Göz nasıl`UrlAuthorizationModule`vermek veya erişimini engellemek için yetkilendirme kuralları kullanır.

`UrlAuthorizationModule` URL yetkilendirmesi analiz ederek belirli bir kimlik için belirli bir URL yetkilendirmek için ilk projeden başlatma ve kapalı yolu çalışma teker teker kuralları olup olmadığını belirler. Bir eşleşme bulunduğu sürece kullanıcı izni veya erişimi reddedilen, açıksa bağlı olarak eşleşmenin bir `<allow>` veya `<deny>` öğesi. <strong>Eşleşme bulunursa, kullanıcıya erişim izni verilir.</strong> Erişimi kısıtlamak istiyorsanız, bu nedenle, bunu kullanmanız zorunludur bir `<deny>` son URL yetkilendirme yapılandırma öğesi olarak öğesi. <strong>Atlarsanız bir</strong><strong>`<deny>`</strong><strong>öğesi, tüm kullanıcıların erişim verilecek.</strong>

Tarafından kullanılan işlem daha iyi anlamak için `UrlAuthorizationModule` yetkilisi belirlemek için URL yetkilendirme kuralları incelemiştik Bu adımda örneği göz önünde bulundurun. İlk kuralı bir `<allow>` Tito ve Scott erişim veren öğesi. İkinci kuralları bir `<deny>` herkese erişimini engellediği öğesi. Anonim kullanıcı ziyaret ederse `UrlAuthorizationModule` isteyen ile başlar, anonim Scott veya Tito? Kuşkusuz, yanıt yok, olduğundan, ikinci kuralı geçer. Herkes kümesinde anonim mi? Bu yana yanıtı Evet, işte `<deny>` kural yürürlükte yerleştirin ve ziyaretçi oturum açma sayfasına yönlendirilir. Benzer şekilde, Jisun ziyaret, `UrlAuthorizationModule` sorarak, olan Jisun başlar Scott veya Tito? Filiz, olmadığından `UrlAuthorizationModule` herkes kümesi olan Jisun ilgili ikinci sorunun devam eder mi? Filiz, çok, erişim reddedildi Filiz, olduğundan. Son olarak, Tito ziyaret ederse, ilk sorusu sorulmuş `UrlAuthorizationModule` olduğundan bir olumlu yanıt Tito erişim izni verilir.

Bu yana `UrlAuthorizationModule` işlemleri yetkilendirme kuralları yukarıdan aşağıya doğru durdurma, herhangi bir eşleşme less yazımına özgü olanlardan önce gelen daha belirli kuralların sağlamak önemlidir. Diğer bir deyişle, Jisun ve anonim kullanıcılar yasaklar, ancak diğer tüm kimliği doğrulanmış kullanıcıların yetkilendirme kurallarını tanımlamak için en belirgin kuralı - bir etkileyen Jisun - başlatın ve ardından az belirli kurallar için - bu diğer tüm izin vererek devam edin Kimliği doğrulanmış kullanıcılar, ancak tüm anonim kullanıcılar engelleme. İlk Jisun engelleme ve sonra herhangi bir anonim kullanıcı engelleme Bu ilke aşağıdaki URL yetkilendirme kuralları uygular. Jisun dışında herhangi bir kimliği doğrulanmış kullanıcı erişimi olduğundan verilecek hiçbirini `<deny>` deyimleri eşleşir.

[!code-xml[Main](user-based-authorization-vb/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>2. adım: iş akışı yetkisiz, kimliği doğrulanmış kullanıcılar için düzeltme

Dilediğiniz zaman bir konum URL yetkilendirme iş akışı bölümü, bu öğreticinin önceki kısımlarındaki ele aldığımız gibi yetkisiz bir isteği transpires, `UrlAuthorizationModule` isteğini durdurur ve bir HTTP 401 yetkilendirilmedi durum döndürür. Bu 401 durum değiştiren `FormsAuthenticationModule` bir 302 yeniden yönlendirme kullanıcı oturum açma sayfasına gönderir durumu. Kullanıcının kimliği doğrulanır bile yetkisiz hiçbir istek üzerinde bu iş akışı gerçekleşir.

Kimliği doğrulanmış bir kullanıcı oturum açma sayfasına döndüren, sisteme zaten oturum olduğundan bunları karıştırılabilir olasıdır. Biraz iş ile bu iş akışı, kısıtlanmış bir sayfaya erişmeye çalıştınız açıklayan bir sayfaya yetkisiz isteğinde bulunan kimliği doğrulanmış kullanıcılar yönlendirerek geliştirebiliriz.

Adlı web uygulamasının kök klasöründe yeni bir ASP.NET sayfası oluşturarak başlayın `UnauthorizedAccess.aspx`; bu sayfayla ilişkilendirilecek unutmayın `Site.master` ana sayfa. Bu sayfa oluşturduktan sonra başvuran içerik denetimi kaldırın `LoginContent` ContentPlaceHolder ana sayfaya varsayılan böylece içerik görüntülenir. Ardından, duruma, yani açıklayan bir ileti ekleyin kullanıcı korunan bir kaynağa erişmeye çalıştı. Böyle bir ileti ekledikten sonra `UnauthorizedAccess.aspx` bildirim temelli biçimlendirme sayfanın aşağıdakine benzer görünmelidir:

[!code-aspx[Main](user-based-authorization-vb/samples/sample7.aspx)]

Artık bir yetkisiz isteği kimliği doğrulanmış bir kullanıcı tarafından gerçekleştirilen varsa bunlar için gönderilir, böylece iş akışını değiştirmek ihtiyacımız `UnauthorizedAccess.aspx` oturum açma sayfası yerine sayfası. Yetkisiz istekler için oturum açma sayfasına yönlendiren mantığı, özel bir yöntem içinde gömülü `FormsAuthenticationModule` sınıfı için Biz bu davranışı özelleştiremezsiniz. Biz, ancak yapabileceklerinizi kendi mantıksal oturum açma sayfası URL'sini, kullanıcı tarafından yeniden yönlendiren eklemektir `UnauthorizedAccess.aspx`, gerekirse.

Zaman `FormsAuthenticationModule` yetkisiz bir ziyaretçi yönlendiren bu ada sahip bir sorgu dizesi istenen, yetkisiz URL'sine oturum açma sayfasına ekler `ReturnUrl`. Örneğin, yetkisiz bir kullanıcı ziyaret çalışıldı `OnlyTito.aspx`, `FormsAuthenticationModule` için bunları yönlendirmek `Login.aspx?ReturnUrl=OnlyTito.aspx`. Bu nedenle, oturum açma sayfasını içeren bir sorgu dizesi ile kimliği doğrulanmış bir kullanıcı tarafından ulaşılırsa `ReturnUrl` parametresi, ardından biz biliyorsanız bu kimliği doğrulanmamış bir kullanıcı yalnızca kendisi yetkili değil görüntülemek için bir sayfayı ziyaret etmek çalıştığının. Böyle bir durumda olduğunu düşündüğü yeniden yönlendirme istiyoruz `UnauthorizedAccess.aspx`.

Bunu yapmak için oturum açma sayfasına ait aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:

[!code-vb[Main](user-based-authorization-vb/samples/sample8.vb)]

Kimliği doğrulanmış, yetkisiz kullanıcılara Yukarıdaki kod yönlendiren `UnauthorizedAccess.aspx` sayfası. Bu mantıksal iş başında görmek için anonim bir ziyaretçi olarak sitesini ziyaret edin ve sol sütunda kullanıcı hesapları oluşturma bağlantısına tıklayın. Bu gideceksiniz `~/Membership/CreatingUserAccounts.aspx` Tito için erişime izin vermek, adım 1'de biz yalnızca yapılandırılmış sayfası. Anonim kullanıcılar yasaklanmış olduğundan `FormsAuthenticationModule` bize oturum açma sayfasına yönlendirir.

Bu noktada anonim duyuyoruz, bu nedenle `Request.IsAuthenticated` döndürür `False` ve şu şekilde yönlendirilmez `UnauthorizedAccess.aspx`. Bunun yerine, oturum açma sayfası görüntülenir. Bruce gibi bir Tito başka bir kullanıcı olarak oturum açın. Oturum açma sayfasında uygun kimlik bilgilerini girdikten sonra bize geri yeniden yönlendirmeleri `~/Membership/CreatingUserAccounts.aspx`. Ancak, bu sayfa yalnızca Tito için erişilebilir olduğundan, biz görüntülemek için yetkisiz ve en kısa sürede oturum açma sayfasına dönersiniz. Bu kez, ancak `Request.IsAuthenticated` döndürür `True` (ve `ReturnUrl` querystring parametresi var), böylece biz yönlendirilirsiniz `UnauthorizedAccess.aspx` sayfası.


[![Kimliği doğrulanmış ve yetkisiz kullanıcılar için UnauthorizedAccess.aspx yönlendirilir](user-based-authorization-vb/_static/image17.png)](user-based-authorization-vb/_static/image16.png)

**Şekil 6**: kimliği doğrulanmış, yetkisiz kullanıcıların yönlendirileceği `UnauthorizedAccess.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image18.png))


Bu özelleştirilmiş iş akışı, Şekil 2'de gösterilen döngüsü kestirmeler tarafından daha mantıklı ve basit bir kullanıcı deneyimi sunar.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>3. adım: şu anda oturum açmış kullanıcıya bağlı işlevselliğini sınırlama

URL yetkilendirmesi, kaba yetkilendirme kurallarını belirtmek kolaylaştırır. 1. adımda gördüğümüz gibi URL yetkilendirmesi ile biz temellerini hangi kimlikleri izin verilir ve belirli bir sayfa ya da tüm sayfaların bir klasörde görüntüleme hangilerinin engellendi durumu. Bazı senaryolarda, ancak biz bir sayfasını ziyaret edin, ancak bunu ziyaret kullanıcıya bağlı olarak sayfanın işlevselliğini sınırlamak tüm kullanıcıların isteyebilirsiniz.

Ürünlerini gözden geçirmek kimliği doğrulanmış ziyaretçiler izin veren bir e-ticaret Web sitesinin durumu göz önünde bulundurun. Adsız bir kullanıcı bir ürün sayfasını ziyaret ettiğinde, yalnızca ürün bilgilerini görür ve İnceleme yazmalarını fırsatı verilmedi. Ancak, aynı sayfasını ziyaret ederek bir kimliği doğrulanmış kullanıcı, gözden geçirme arabirimi görür. Kimliği doğrulanmış kullanıcı henüz bu ürün gözden geçirilmedi, bunları bir gözden geçirmeyi göndermek arabirim etkinleştirmez; Aksi takdirde, bunları önceden gönderilmiş gözden geçirmelerini gösterebilir. Bu senaryo, bir aşamaya geçmeye daha fazla ürün sayfası ek bilgiler gösterilir ve e-ticaret şirketin çalışan kullanıcılar için genişletilmiş özellikler sunar. Örneğin, ürün sayfası bir stok envanterinde listelemek ve ürünün fiyatı ve bir çalışan tarafından ziyaret edildiğinde açıklamasını düzenlemel seçenekleri içerir.

Bu tür hedeflemediğinizden yetkilendirme kuralları, bildirimli olarak veya program aracılığıyla (veya ikisinin birleşimi aracılığıyla) uygulanabilir. Sonraki bölümde LoginView denetimi ile hedeflemediğinizden yetkilendirme gerçekleştirme göreceğiz. Programlama teknikleri inceleyeceksiniz. Biz hedeflemediğinizden yetkilendirme kuralları uygulamayı göz atmadan önce ancak biz öncelikle bir sayfa, ziyaret kullanıcı olan işlevselliği bağlıdır oluşturmanız gerekir.

GridView içinde belirli bir dizindeki dosyaları listeler bir sayfa oluşturalım. Her dosyanın adı, boyutu ve diğer bilgileri listelemek yanı sıra, GridView LinkButtons, iki sütun içerecektir: bir başlıklı görünümü ve bir başlıklı silme. Görünüm Linkbutton'a tıklandı Seçili dosyanın içeriğini görüntülenir; Silme Linkbutton'a tıklandı, dosya silinir. Görünüm ve delete işlevlerini tüm kullanıcılar tarafından kullanılabilir, başlangıçta bu sayfayı oluşturalım. Kullanarak, biz etkinleştirme veya sayfasını ziyaret ederek kullanıcıya dayanarak bu özellikleri devre dışı bırakma görebilir LoginView denetimi ve programlı bir şekilde sınırlama işlevselliği bölümleri.

> [!NOTE]
> İlgili yapı duyuyoruz ASP.NET sayfası bir GridView denetimi dosyaların bir listesini görüntülemek için kullanır. Seri form kimlik doğrulaması, yetkilendirme, kullanıcı hesaplarını ve rolleri üzerinde odaklanır. Bu öğreticide bu yana GridView denetiminde iç işleyişini tartışma çok fazla vakit geçirmeyi istemiyorum. Bu öğretici, bu sayfası ayarlama belirli adım adım yönergeler sağlar. ancak, bazı seçenekler neden yapıldığı veya hangi işlenmiş çıktı belirli özellikleri etkili sahip detayına anlamak için delve değil. GridView denetiminde tam incelenmesi için başvurun my *[ASP.NET 2.0 verilerle çalışmaya](../../data-access/index.md)* öğretici serisi.


Başlangıç açarak `UserBasedAuthorization.aspx` dosyası `Membership` klasör ve bir GridView denetimi adlı sayfasına ekleme `FilesGrid`. GridView'ın akıllı etiketten alanları iletişim kutusunu başlatmak için sütunları Düzenle bağlantısına tıklayın. Buradan, sol alt köşedeki otomatik oluştur alanların onay kutusunun işaretini kaldırın. Ardından, iki BoundFields seçme düğmesi ve Sil düğmesine (seçin ve Sil düğmeleri CommandField türü altında bulunabilir) sol üst köşesindeki ekleyin. Select düğmenin ayarlamak `SelectText` görüntüleyin ve ilk BoundField'ın özelliğini `HeaderText` ve `DataField` adı özellikleri. İkinci BoundField's ayarlayın `HeaderText` bayt cinsinden boyut özelliği, `DataField` uzunluk özelliğine kendi `DataFormatString` özelliğini {0:N0} ve kendi `HtmlEncode` özelliğini False olarak.

Sloupce prvku GridView yapılandırdıktan sonra alanlar iletişim kutusunu kapatmak için Tamam'a tıklayın. GridView'ın Özellikler penceresinde ayarlayın `DataKeyNames` özelliğini `FullName`. Bu noktada GridView'ın bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:

[!code-aspx[Main](user-based-authorization-vb/samples/sample9.aspx)]

Oluşturulan GridView'ın işaretleme ile belirli bir dizindeki dosyaları almak ve bunları GridView'a bağlama kod yazmaya hazırız. Aşağıdaki kodu ekleyin sayfanın `Page_Load` olay işleyicisi:

[!code-vb[Main](user-based-authorization-vb/samples/sample10.vb)]

Yukarıdaki kod [ `DirectoryInfo` sınıfı](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) uygulamanın kök klasördeki dosyaların listesi elde etmek için. [ `GetFiles()` Yöntemi](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) tüm dosyaları dizindeki bir dizi döndürür [ `FileInfo` nesneleri](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), ardından GridView'a bağlı. `FileInfo` Nesne olan özellikleri kaynaklardan gibi `Name`, `Length`, ve `IsReadOnly`, diğerlerinin yanı sıra. GridView, bildirim temelli biçimlendirmeden görebileceğiniz gibi yalnızca görüntüler `Name` ve `Length` özellikleri.

> [!NOTE]
> `DirectoryInfo` Ve `FileInfo` sınıflar bulunur [ `System.IO` ad alanı](https://msdn.microsoft.com/library/system.io.aspx). Bu nedenle, bu sınıf adlarını kendi ad alanı adları ile yazdığınızdan veya sınıf dosyasına içeri aktarılan ad alanının gerek ya da olur (aracılığıyla `Imports System.IO`).


Bir tarayıcı aracılığıyla bu sayfayı ziyaret etmek için bir dakikamızı ayıralım. Uygulamanın kök dizininde bulunan dosyaların listesini görüntüler. Herhangi bir görünüm veya silme LinkButtons tıklayarak geri göndermeye neden olur, ancak henüz için yaptığımız çünkü hiçbir eylem meydana gelir gerekli olay işleyicilerini oluşturma.


[![GridView Web uygulamasının kök dizindeki dosyaları listeler.](user-based-authorization-vb/_static/image20.png)](user-based-authorization-vb/_static/image19.png)

**Şekil 7**: GridView Web uygulamasının kök dizindeki dosyaları listeler ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image21.png))


Seçili dosya içeriğini görüntülemek için bir yol ihtiyacımız var. Visual Studio'ya geri dönün ve adlı bir metin kutusu ekleme `FileContents` yukarıda GridView. Ayarlayın, `TextMode` özelliğini `MultiLine` ve kendi `Columns` ve `Rows` % 95 ve 10, özellikleri sırasıyla.

[!code-aspx[Main](user-based-authorization-vb/samples/sample11.aspx)]

Ardından, GridView için ait bir olay işleyicisi oluşturun [ `SelectedIndexChanged` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) ve aşağıdaki kodu ekleyin:

[!code-vb[Main](user-based-authorization-vb/samples/sample12.vb)]

GridView'ın bu kodu kullanan `SelectedValue` seçili dosyasının tam dosya adını belirlemek için özellik. Dahili olarak `DataKeys` koleksiyonu almak için başvuruluyor `SelectedValue`, kesinlik temelli GridView'ın ayarlayın, bu nedenle `DataKeyNames` özelliğini bu adımda açıklandığı gibi ad. [ `File` Sınıfı](https://msdn.microsoft.com/library/system.io.file.aspx) ardından öğesine atanan bir dizeye Seçili dosyanın içeriğini okumak için kullanılan `FileContents` metin kutusunun `Text` böylece sayfasında seçilen dosyanın içeriğini görüntüleme özelliği.


[![Seçilen dosyanın içeriğinin metin kutusunda görüntülenir.](user-based-authorization-vb/_static/image23.png)](user-based-authorization-vb/_static/image22.png)

**Şekil 8**: seçilen dosyanın içeriğinin metin kutusunda görüntülenen ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image24.png))


> [!NOTE]
> HTML biçimlendirmesini içeren bir dosyanın içeriğini görüntüleyin ve sonra görüntülemek veya bir dosyayı silmek çalışırsanız, alacak bir `HttpRequestValidationException` hata. Bu durum, web sunucusuna gönderilen geri göndermede metin kutusunun içeriğini kaynaklanır. Varsayılan olarak, ASP.NET başlatan bir `HttpRequestValidationException` HTML biçimlendirmeyi gibi geri gönderme tehlikeli olabilecek içeriğe algılandığında bir hata oluştu. Bu hatanın oluşmasını devre dışı bırakmak için sayfa için istek doğrulamayı ekleyerek devre dışı `ValidateRequest="false"` için `@Page` yönergesi. İyi hangi, ne zaman önlem almalısınız olarak istek doğrulamasının avantajları hakkında daha fazla bilgi için devre dışı bırakma okuma [istek doğrulama - betik saldırılarını önleme](https://asp.net/learn/whitepapers/request-validation/).


Son olarak, bir olay işleyicisi aşağıdaki kodla GridView için ait ekleme [ `RowDeleting` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-vb[Main](user-based-authorization-vb/samples/sample13.vb)]

Kod silmek için dosyasının tam adı görüntüler `FileContents` TextBox *olmadan* gerçekten dosyası siliniyor.


[![Sil düğmesine tıklanarak gerçekten dosya silmez](user-based-authorization-vb/_static/image26.png)](user-based-authorization-vb/_static/image25.png)

**Şekil 9**: dosyayı Sil düğmesini gerçekten silinmez tıklayarak ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image27.png))


1. adımda size URL yetkilendirme kuralları, anonim kullanıcılar sayfalarında görüntülemesini engellemek için yapılandırılmış `Membership` klasör. Daha iyi hedeflemediğinizden kimlik doğrulaması göstermesi için şimdi ziyaret etmek anonim kullanıcıların `UserBasedAuthorization.aspx` sayfasında, ancak sınırlı işlevsellikle. Tüm kullanıcılar tarafından erişilebilir için bu sayfayı açın, aşağıdaki ekleyin `<location>` öğesine `Web.config` dosyası `Membership` klasörü:

[!code-xml[Main](user-based-authorization-vb/samples/sample14.xml)]

Bunu ekledikten sonra `<location>` öğesi, oturum açtıktan sonra site dışı yeni URL yetkilendirme kuralları test edin. Anonim kullanıcı olarak, ziyaret etmek için izin `UserBasedAuthorization.aspx` sayfası.

Şu anda, herhangi bir kimliği doğrulanmış veya anonim kullanıcı ziyaret edebilirsiniz `UserBasedAuthorization.aspx` sayfasında görüntüleyebilir veya dosyaları silin. Şimdi yalnızca kimliği doğrulanmış kullanıcılar bir dosyanın içeriğini görüntüleyebilir ve bir dosyayı yalnızca Tito silebilirsiniz olun. Bu tür hedeflemediğinizden yetkilendirme kuralları, bildirimli olarak, programlı olarak veya her iki yöntem de bir birleşimi yoluyla uygulanabilir. Şimdi bir dosyanın içeriğini görüntüleyebilecek kişileri sınırlandırmak için bildirim temelli bir yaklaşım kullanın; bir dosyayı silebilirsiniz sınırı programlı yaklaşımı kullanacağız.

### <a name="using-the-loginview-control"></a>Bir LoginView denetimi kullanma

Önceki öğreticilerde anlatıldığı gibi LoginView denetimi kimliği doğrulanmış ve anonim kullanıcılar için farklı arabirimler görüntülemek için yararlıdır ve anonim kullanıcılar için erişilebilir değil işlevselliği gizlemek için kolay bir yolunu sunar. Anonim kullanıcılar görüntüleyemez veya dosyaları silme yalnızca gösterilecek gerekir `FileContents` sayfa kimliği doğrulanmış bir kullanıcı tarafından ziyaret edildiğinde metin. Bunu başarmak için sayfaya bir LoginView denetimi ekleyin, adlandırın `LoginViewForFileContentsTextBox`ve taşıma `FileContents` LoginView denetimin içine bildirim temelli biçimlendirme metin kutusunun `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-vb/samples/sample15.aspx)]

Web denetimleri LoginView'ın şablonlarında artık arka plan kod sınıftan doğrudan erişilebilir. Örneğin, `FilesGrid` GridView'ın `SelectedIndexChanged` ve `RowDeleting` olay işleyicileri şu anda başvuru `FileContents` TextBox denetimi ile kod gibi:

[!code-aspx[Main](user-based-authorization-vb/samples/sample16.aspx)]

Ancak, bu kod artık geçerli değil. Taşıyarak `FileContents` TextBox'a `LoggedInTemplate` TextBox doğrudan erişilemez. Bunun yerine, biz kullanmalısınız `FindControl("controlId")` programlı olarak denetim başvurusu yapmak için yöntemi. Güncelleştirme `FilesGrid` TextBox başvurmak için olay işleyicileri şu şekilde:

[!code-vb[Main](user-based-authorization-vb/samples/sample17.vb)]

TextBox LoginView için 's taşıdıktan `LoggedInTemplate` ve sayfanın kodunu kullanarak metin kutusu başvuru güncelleştirme `FindControl("controlId")` desen, bir anonim kullanıcı olarak sayfasını ziyaret edin. Şekil 10 gösterildiği gibi `FileContents` metin kutusu görüntülenmez. Ancak, görünümü LinkButton gösterilmeye devam eder.


[![Bir LoginView denetimi yalnızca kimliği doğrulanmış kullanıcılar için FileContents TextBox işler](user-based-authorization-vb/_static/image29.png)](user-based-authorization-vb/_static/image28.png)

**Şekil 10**: LoginView denetimi yalnızca işler `FileContents` kimliği doğrulanmış kullanıcılar için metin ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image30.png))


Anonim kullanıcılar için Görüntüle düğmesi gizlemek için bir GridView alanın bir TemplateField dönüştürmek için yoludur. Bu görünüm LinkButton için bildirim temelli biçimlendirme içeren bir şablon oluşturur. Ardından bir LoginView denetimi için TemplateField ekleyebilir ve LoginView'ın içinde LinkButton yerleştirin `LoggedInTemplate`, böylece anonim ziyaretçilerden Görüntüle düğmesini gizleme. Bunu gerçekleştirmek için alanları iletişim kutusunu başlatmak için akıllı etiketinde GridView'ın sütunları Düzenle bağlantısına tıklayın. Ardından, sol alt köşesine listeden seç düğmesini seçin ve bu alan TemplateField bağlantısına Dönüştür'e tıklayın. Bunun yapılması, alanın bildirim temelli biçimlendirmeden değiştirin:

[!code-aspx[Main](user-based-authorization-vb/samples/sample18.aspx)]

 Hedef: 

[!code-aspx[Main](user-based-authorization-vb/samples/sample19.aspx)]

 Bir LoginView TemplateField için bu noktada, ekleyebiliriz. Aşağıdaki biçimlendirme yalnızca kimliği doğrulanmış kullanıcılar için Görünüm LinkButton görüntüler. 

[!code-aspx[Main](user-based-authorization-vb/samples/sample20.aspx)]

Şekil 11 gösterildiği gibi sonuç sütundaki görünümü LinkButtons gizli olsa bile oldukça görünüm olarak sütunu yine de görüntülendiğini değil. Biz tüm GridView Sütun (ve yalnızca LinkButton) gizlemek sonraki bölümde konuları ele alınacaktır.


[![Bir LoginView denetimi görünümü LinkButtons anonim ziyaretçiler için gizler.](user-based-authorization-vb/_static/image32.png)](user-based-authorization-vb/_static/image31.png)

**Şekil 11**: LoginView denetimi görünümü LinkButtons anonim ziyaretçiler için gizler. ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Program aracılığıyla işlevselliğini sınırlama

Bazı durumlarda, bildirim temelli teknikleri bir sayfaya işlevselliğini sınırlama için yetersizdir. Örneğin, belirli sayfa işlevselliği kullanılabilirliğini sayfasını ziyaret ederek kullanıcı anonim veya kimliği doğrulanmış olup ötesinde ölçütlere bağımlı olabilir. Bu gibi durumlarda, çeşitli kullanıcı arabirimi öğeleri görüntülenir veya programlı yollarla gizlenir.

Program aracılığıyla için işlevselliği sınırlamanızı için biz iki görev gerçekleştirmeniz gerekir:

1. Sayfasını ziyaret ederek kullanıcı işlevselliği erişip erişemeyeceğini belirlemek ve
2. Kullanıcı söz konusu işlevlerine erişimi olup olmamasına bağlı kullanıcı arabirimi programlı olarak değiştirin.

Uygulama bu iki görevleri göstermek için şimdi yalnızca GridView dosyaları silmek Tito izin verir. Bizim ilk, ardından sayfasını ziyaret ederek Tito olup olmadığını belirlemek için bir görevdir. Belirlendikten sonra biz GridView'ın sütun Sil (gizlemek veya göstermek) gerekir. Sloupce prvku GridView üzerinden erişilebilir, `Columns` özelliği; bir sütun ise yalnızca işlenen kendi `Visible` özelliği `True` (varsayılan).

Aşağıdaki kodu ekleyin `Page_Load` GridView'a veri bağlama öncesi olay işleyicisi:

[!code-vb[Main](user-based-authorization-vb/samples/sample21.vb)]

Açıkladığımız gibi [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-vb.md) öğreticide `User.Identity.Name` kimliğin adını döndürür. Bu oturum açma denetimine girilen kullanıcı adına karşılık gelir. Sayfa, ikinci sütun GridView'ın ziyaret Tito olup olmadığını `Visible` özelliği `True`; Aksi takdirde ayarlanmış `False`. Net sonucudur Tito dışındaki biri tarafından başka bir kimliği doğrulanmış kullanıcı veya anonim bir kullanıcı sayfasını ziyaret ettiğinde Sütun Sil (bkz: Şekil 12); işlenmez Sütun Sil Tito sayfasını ziyaret ettiğinde, ancak varsa (bkz. Şekil 13).


[![Silme değil işlenen, ziyaret edilen tarafından birisi dışında Tito (örneğin, Bruce) sütunudur](user-based-authorization-vb/_static/image35.png)](user-based-authorization-vb/_static/image34.png)

**Şekil 12**: silme Sloupec je değil işlenen, ziyaret edilen tarafından birisi dışında Tito (örneğin, Bruce) ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image36.png))


[![Sütun Sil Tito için işlenen](user-based-authorization-vb/_static/image38.png)](user-based-authorization-vb/_static/image37.png)

**Şekil 13**: Sütun Sil Tito için işlenen ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>4. adım: Sınıflar ve yöntemler için yetkilendirme kuralları uygulama

Adım 3'teki bir dosyanın içeriğini görüntüleme anonim kullanıcıların izin verilmeyen ve tüm kullanıcılar ancak Tito dosyaları silmesi yasaklanmış. Bu, bildirim temelli ve programlı teknikleri aracılığıyla yetkisiz ziyaretçiler için ilişkili kullanıcı arabirimi öğelerini gizleyerek gerçekleştirilmiştir. Basit Bizim örneğimizde, kullanıcı arabirimi öğeleri doğru gizleme açık, ancak ne hakkında olduğu siteler daha karmaşık olabilir burada aynı işlevleri gerçekleştirmek için çeşitli yollar? Tüm geçerli kullanıcı arabirimi öğeleri devre dışı bırakmak veya gizlemek unutursanız yetkisiz kullanıcılara işlevselliğini sınırlama içinde ne olur?

Bu sınıf ya da yöntemiyle donatmak için işlevsellik belirli bir parçasını yetkisiz kullanıcılar tarafından erişilmediğinden emin olmak için kolay bir yol olan [ `PrincipalPermission` özniteliği](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). .NET çalışma zamanı kullanan bir sınıf veya yöntemlerinden birini yürütür, geçerli güvenlik bağlamı sınıfı kullanın veya yöntem yürütme izni olduğundan emin olmak için kontrol eder. `PrincipalPermission` Özniteliği yoluyla tanımlarız bu kuralları bir mekanizma sağlar.

Kullanarak gösterelim `PrincipalPermission` GridView'ın özniteliği `SelectedIndexChanged` ve `RowDeleting` yürütme anonim ve Tito, dışındaki kullanıcılar tarafından sırasıyla önlemek için olay işleyicileri. Yapmamız gereken şey her işlev tanımı üzerine uygun özniteliğini ekleyin:

[!code-vb[Main](user-based-authorization-vb/samples/sample22.vb)]

Öznitelik için `SelectedIndexChanged` yalnızca kimliği doğrulanmış kullanıcılar olay işleyicisi belirtir olay işleyicisi, burada özniteliği olarak yürütebilir `RowDeleting` olay işleyicisi yürütme Tito için sınırlar.

> [!NOTE]
> Öznitelik, bir sınıf, yöntem, özellik veya olay uygulanabilir. Bir öznitelik eklerken, bu sınıfı, yöntem, özellik veya olay bildirim deyimindeki bir parçası olmalıdır. Visual Basic ifade sınırlayıcı olarak satır sonları kullandığından, öznitelikleri ya da aynı satırda bildirimi ya da doğrudan bir satır devamı karakteri (alt çizgi) ile yukarıda yer almalıdır. Yukarıdaki kod parçacığında, satır devamı karakteri, öznitelik bir satır ve başka bir yöntem bildiriminde yerleştirmek için kullanılır.


Tito başka bir kullanıcı bu şekilde, yürütme girişiminde bulunursa `RowDeleting` olay işleyicisi veya doğrulanmamış bir kullanıcı yürütme girişimlerini `SelectedIndexChanged` .NET çalışma zamanı olay işleyicisi yükseltmek bir `SecurityException`.


[![Güvenlik bağlamı metodunu yürütmek için yetkili değil, bir SecurityException oluşturulur](user-based-authorization-vb/_static/image41.png)](user-based-authorization-vb/_static/image40.png)

**Şekil 14**: metodunu yürütmek için güvenlik içeriğini Yetkilendirilmemişse bir `SecurityException` oluşturulur ([tam boyutlu görüntüyü görmek için tıklatın](user-based-authorization-vb/_static/image42.png))


> [!NOTE]
> Birden fazla güvenlik bağlamı bir sınıfta veya yöntemde erişmesine izin vermek için sınıf ya da yöntem ile donatmak bir `PrincipalPermission` her güvenlik bağlamının özniteliği. Diğer bir deyişle, Tito ve Bruce yürütmesine izin verecek şekilde `RowDeleting` olay işleyicisi ekleme *iki* `PrincipalPermission` öznitelikleri:


[!code-vb[Main](user-based-authorization-vb/samples/sample23.vb)]

ASP.NET sayfaları yanı sıra birçok uygulama ayrıca iş mantığı ve veri erişim katmanları gibi çeşitli katmanları içeren bir mimari var. Bu katman, genellikle sınıf kitaplıkları uygulanır ve sınıflar ve iş mantığı ve verileri ilgili işlevleri gerçekleştirmek için yöntemler sağlar. `PrincipalPermission` Özniteliği, bu katmanlara yetkilendirme kurallarını uygulamak için kullanışlıdır.

Kullanma hakkında daha fazla bilgi için `PrincipalPermission` başvurmak yetkilendirme kuralları sınıfları ve yöntemleri tanımlamak için öznitelik [Scott Guthrie](https://weblogs.asp.net/scottgu/)ın blog girişine [iş ve veri katmanları kullanarak Yetkilendirmekurallarıekleme`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Özet

Bu öğreticide kullanıcı tabanlı yetkilendirme kurallarını uygulamak nasıl incelemiştik. ASP göz ile Başladık. NET URL yetkilendirme çerçevesi. Her isteğin, ASP.NET altyapısı üzerinde `UrlAuthorizationModule` kimlik istenen kaynağa erişim için yetki verilip verilmediğini belirlemek için uygulama yapılandırmasında tanımlı URL yetkilendirme kurallarını denetler. Kısacası, URL yetkilendirmesi, belirli bir dizindeki tüm sayfaları veya belirli bir sayfa için yetkilendirme kuralları belirtmek kolaylaştırır.

URL yetkilendirme framework sayfa tarafından temelinde yetkilendirme kuralları geçerlidir. URL yetkilendirmesi ile isteyen kimliği veya belirli bir kaynağa erişim yetkisi. Birçok senaryo için daha fazla hedeflemediğinizden yetkilendirme kuralları ancak çağırın. Kimin bir sayfaya erişmeye izin tanımlamak yerine biz herkesin bir sayfaya erişmeye olanak tanır ancak farklı verileri görüntüleme veya sayfasını ziyaret ederek kullanıcı bağlı olarak farklı işlevsellik sunabilir gerekebilir. Sayfa düzeyinde yetkilendirme, genellikle yetkisiz kullanıcıların izin verilmeyen işlevselliği erişimini engellemek için belirli bir kullanıcı arabirimi öğeleri gizleme içerir. Ayrıca, sınıflar ve yürütülmesini metotlarını belirli kullanıcılar için erişimi kısıtlamak için öznitelikleri kullanmak da mümkündür.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İş ve veri katmanlarını kullanmak için yetkilendirme kuralları ekleme `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET yetkilendirmesi](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [IIS6 IIS7 güvenlik arasındaki değişiklikleri](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Belirli dosyaları ve alt dizinleri yapılandırma](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Kullanıcıya bağlı olarak veri değişikliği işlevselliğini sınırlama](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb.md)
- [LoginView denetimi hızlı Başlangıçlar](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [IIS7 URL yetkilendirmesi anlama](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Teknik belgeler](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [ASP.NET 2.0 verilerle çalışma](../../data-access/index.md)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](validating-user-credentials-against-the-membership-user-store-vb.md)
> [İleri](storing-additional-user-information-vb.md)
