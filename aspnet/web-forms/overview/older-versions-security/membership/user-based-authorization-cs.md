---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: "Kullanıcı tabanlı bir yetkilendirme (C#) | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide sayfalarına erişimi sınırlandırma ve sayfa düzeyinde işlevselliği teknikleri çeşitli kısıtlayarak ele alacağız."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: da03a9c3e22f5a2164534ef7896b5558beb8b6f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="user-based-authorization-c"></a>Kullanıcı tabanlı bir yetkilendirme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> Bu öğreticide sayfalarına erişimi sınırlandırma ve sayfa düzeyinde işlevselliği teknikleri çeşitli kısıtlayarak ele alacağız.


## <a name="introduction"></a>Giriş

Kullanıcı hesapları teklif çoğu web uygulamaları belirli ziyaretçileri site belirli sayfalara erişimini kısıtlamak için bölümünde bunu. Birçok çevrimiçi messageboard sitelerdeki Örneğin, tüm kullanıcılar - anonim ve kimliği doğrulanmış - messageboard's gönderileri görüntüleyebilirsiniz, ancak yalnızca kimliği doğrulanmış kullanıcıların yeni bir posta oluşturmak için web sayfasını ziyaret. Ve yalnızca belirli bir kullanıcı (veya belirli bir kullanıcı kümesini) erişilebilen yönetim sayfaları olabilir. Ayrıca, sayfa düzeyinde işlevselliği bir kullanıcı tarafından temelinde farklı olabilir. Bu arabirim anonim ziyaretçilere değildir, ancak gönderileri listesini görüntülerken, kimliği doğrulanmış kullanıcıların her post derecelendirme için bir arabirim gösterilir.

ASP.NET, kullanıcı tabanlı bir yetkilendirme kurallarını tanımlamak kolaylaştırır. Yalnızca biçimlendirme biraz ile `Web.config`, belirli web sayfalarına ya da tüm dizinleri kilitlenebilir aşağı böylece yalnızca belirtilen bir kullanıcılar alt kümesine erişilebilir. Sayfa düzeyi işlevselliği açmak veya kapatmak programlı ve bildirim temelli araçlarla şu anda oturum açmış olan kullanıcının dayanan açılabilir.

Bu öğreticide sayfalarına erişimi sınırlandırma ve sayfa düzeyinde işlevselliği teknikleri çeşitli kısıtlayarak ele alacağız. Haydi başlayalım!

## <a name="a-look-at-the-url-authorization-workflow"></a>URL yetkilendirme iş akışı bakma

' Da anlatıldığı gibi [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-cs.md) ASP.NET kaynak isteği olay sayısı, yaşam döngüsü sırasında başlatır için ASP.NET çalışma zamanı bir isteği işlerken öğretici. *HTTP modülleri* kodu isteği yaşam döngüsü belirli bir olaya yanıt yürütüldüğünde yönetilen sınıflarıdır. ASP.NET birkaç önemli görevleri arka planda gerçekleştirmek HTTP Modülleri ile birlikte gelir.

Bu tür bir HTTP modülü [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx). Önceki öğreticileri, sunucunun birincil işlevi anlatıldığı gibi `FormsAuthenticationModule` geçerli isteğin kimliğini belirlemektir. Bu, bir tanımlama bilgisinde bulunan veya URL içinde katıştırılmış forms kimlik doğrulaması bileti inceleyerek gerçekleştirilir. Bu kimliği gerçekleşir sırasında [ `AuthenticateRequest` olay](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx).

Başka bir önemli HTTP modülü [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.urlauthorizationmodule.aspx), yanıt olarak gerçekleştirilen [ `AuthorizeRequest` olay](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authorizerequest.aspx) (sonra gerçekleşir `AuthenticateRequest` olay). `UrlAuthorizationModule` Yapılandırma biçimlendirmede inceler `Web.config` geçerli kimlik belirtilen sayfasını ziyaret etmek için yetkili olup olmadığını belirlemek için. Bu işlem olarak adlandırılır *URL yetkilendirmesi*.

Biz 1. adımda, URL yetkilendirme kuralları için söz dizimi ancak daha önce şimdi inceleyeceğiz tahminde bulunduğunu görmek `UrlAuthorizationModule` istek veya yetkili bağlı olarak yapar. Varsa `UrlAuthorizationModule` istek yetki sonra hiçbir şey yapmaz ve istek kendi yaşam döngüsü devam eder belirler. Ancak, istek ise *değil* yetkili ve sonra `UrlAuthorizationModule` yaşam döngüsü durdurur ve bildirir `Response` dönmek için nesne bir [HTTP 401 Yetkisiz](http://www.checkupdown.com/status/E401.html) durumu. Forms kimlik doğrulaması kullanırken bu HTTP 401 durum hiçbir zaman istemciye çünkü döndürülür, `FormsAuthenticationModule` bir HTTP durumu 401 değiştirir kendisine algılar bir [HTTP 302 yeniden yönlendirme](http://www.checkupdown.com/status/E302.html) oturum açma sayfası.

Şekil 1'iş akışı ASP.NET ardışık düzenini gösterir `FormsAuthenticationModule`ve `UrlAuthorizationModule` yetkisiz bir istek geldiğinde. Özellikle, Şekil 1 tarafından anonim bir ziyaretçi için bir istek gösterir `ProtectedPage.aspx`, anonim kullanıcılara erişimini engellediği bir sayfa olduğu. Ziyaretçi anonimdir beri `UrlAuthorizationModule` isteğini durdurur ve bir HTTP 401 yetkilendirilmedi durum döndürür. `FormsAuthenticationModule` Oturum açma sayfasının 302 yeniden yönlendirme 401 durum dönüştürür. Kullanıcı oturum açma sayfasına doğrulandıktan sonra kendisinin yönlendirilir `ProtectedPage.aspx`. Bu süre `FormsAuthenticationModule` kullanıcı kendi kimlik doğrulama bileti tabanlı tanımlar. Ziyaretçi kimlik doğrulaması, `UrlAuthorizationModule` sayfaya erişim verir.


[![Form kimlik doğrulaması ve URL yetkilendirme iş akışı](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Şekil 1**: URL yetkilendirme iş akışını ve Forms kimlik doğrulaması ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image3.png))


Şekil 1 anonim bir ziyaretçi, anonim kullanıcılar için kullanılabilir olmayan bir kaynağa erişmeyi denediğinde oluşan etkileşimler gösterilmektedir. Böyle bir durumda anonim ziyaretçi aynen sorgu dizesinde belirtilen ziyaret denediğiniz sayfa ile oturum açma sayfasına yönlendirilir. Kullanıcı başarıyla oturum açtıktan sonra aynen otomatik olarak kendisi başlangıçta görüntülemek için çalışıyordu kaynak yeniden yönlendirilir.

Anonim kullanıcılar tarafından yetkisiz istek yapıldığında, bu iş akışı basittir ve ziyaretçi ne olduğunu anlamak kolaydır ve neden. Ancak, şunları aklınızda tutun içinde `FormsAuthenticationModule` yönlendirir *herhangi* tarafından kimliği doğrulanmış bir kullanıcı bir istekte olsa bile kullanıcı oturum açma sayfasına yetkisiz. Kimliği doğrulanmış bir kullanıcı aynen yetkilisi eksik sayfasını ziyaret edin girişiminde bulunursa bu kafa karıştırıcı bir kullanıcı deneyimi neden olabilir.

Bizim Web sitesinin yapılandırılan URL yetkilendirme kurallarını olduğunu düşünün gibi ASP.NET sayfası `OnlyTito.aspx` accessibly yalnızca Tito oluştu. Şimdi, Sam sitesini ziyaret, oturum açan ve ziyaret girişiminde düşünün `OnlyTito.aspx`. `UrlAuthorizationModule` İsteği yaşam döngüsü durdurmak ve bir HTTP 401 Yetkisiz duruma dönmesi hangi `FormsAuthenticationModule` algılar ve ardından Sam oturum açma sayfasına yeniden yönlendir. SAM zaten oturum açtıktan sonra yine de aynen oturum açma sayfasına yeniden aynen neden gönderildi merak ediyor olabilirsiniz. Aynen her oturum açma kimlik bilgileri şekilde kayıp ya da aynen geçersiz kimlik bilgileri girilen neden. Kendi kimlik bilgileri oturum açma sayfasından SAM reenters varsa aynen (yeniden) olarak oturum açmış ve olması için yeniden yönlendirilen `OnlyTito.aspx`. `UrlAuthorizationModule` Sam olamaz bu sayfasını ziyaret edin ve oturum açma sayfasına aynen döndürülür algılar.

Şekil 2 kafa karıştırıcı bu iş akışı gösterilmektedir.


[![Varsayılan iş akışı için kafa karıştırıcı bir döngü yol açabilir](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Şekil 2**: varsayılan iş akışı açabilir kafa karıştırıcı döngüsü ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image6.png))


Şekil 2'de gösterilen iş akışı hızla bile çoğu bilgisayar deneyimli ziyaretçi befuddle. 2. adım döngüsünde kafa karıştırıcı Bunu önlemenin yolu ele alacağız.

> [!NOTE]
> ASP.NET, geçerli kullanıcının belirli bir web sayfasını erişebileceğini belirlemek için iki mekanizma kullanır: URL yetkilendirme ve dosya yetkilendirme. Dosya yetkilendirmesi tarafından gerçekleştirilir [ `FileAuthorizationModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.fileauthorizationmodule.aspx), istenen dosya ACL'leri danışmanlık tarafından yetkilisi belirler. ACL'ler Windows hesaplarına uygulanan izinleri olduğundan dosya yetkilendirme Windows kimlik doğrulaması ile en yaygın olarak kullanılır. Forms kimlik doğrulaması kullanırken, tüm işletim sistemi ve dosya sistemi düzeyinde istekleri sitesini ziyaret kullanıcıya bakılmaksızın aynı Windows hesabına göre çalıştırılır. Bu öğretici seri form kimlik doğrulamasını odaklanır olduğundan, biz dosya yetkilendirme ele değil.


### <a name="the-scope-of-url-authorization"></a>URL yetkilendirme kapsamı

`UrlAuthorizationModule` Olan yönetilen ASP.NET çalışma zamanı parçası olan kod. Microsoft'un 7 sürümü önce [Internet Information Services (IIS)](https://www.iis.net/) web sunucusuna IIS HTTP ardışık düzen ve ASP.NET zamanının ardışık düzen arasında farklı bir engel oldu. Kısa içinde IIS 6 ve önceki sürümlerinde, ASP. NET'in `UrlAuthorizationModule` bir istek IIS ASP.NET çalışma yetkisi aktarıldığında yalnızca yürütür. Varsayılan olarak, IIS statik içeriğin kendisini - HTML sayfaları ve CSS, JavaScript ve görüntü dosyaları - gibi işler ve ASP.NET çalışma zamanı uzantısına sahip bir sayfa olduğunda isteklerini kapatmak yalnızca aktarır `.aspx`, `.asmx`, veya `.ashx` isteniyor.

IIS 7, ancak, tümleşik IIS ve ASP.NET için işlem hatları sağlar. Bazı yapılandırma ayarlarıyla çağırmak için IIS 7 Kurulumu `UrlAuthorizationModule` için *tüm* istekleri, URL yetkilendirme kuralları için herhangi bir türde dosyaları tanımlanabilir anlamına gelir. Ayrıca, IIS 7 kendi URL yetkilendirme altyapısı içerir. ASP.NET tümleştirme ve IIS 7'in yerel URL yetkilendirme işlevselliği hakkında daha fazla bilgi için bkz: [anlama IIS7 URL yetkilendirmesi](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Shahram Khosravi'nın defterinin bir kopya için daha kapsamlı bir görünüm ASP.NET ve IIS 7 tümleştirme sırasında çekme *Professional IIS 7 ve ASP.NET tümleşik programlama* (ISBN: 978 0470152539).

Buna koysalar IIS 7'den önceki sürümleri, URL yetkilendirme kuralları yalnızca ASP.NET çalışma zamanı tarafından işlenen kaynaklarına uygulanır. Ancak, IIS 7 ile IIS yerel URL yetkilendirme özelliğini kullanmak veya ASP tümleştirme için mümkündür. NET'in `UrlAuthorizationModule` IIS HTTP ardışık düzenine böylece tüm isteklere bu işlevselliği genişletme.

> [!NOTE]
> Bazı nasıl Zarif henüz önemli farklılıklar vardır ASP. NET'in `UrlAuthorizationModule` ve IIS 7'in URL yetkilendirme özelliği yetkilendirme kurallarını işleme. Bu öğretici IIS 7'in URL yetkilendirme işlevselliği veya nasıl karşılaştırılan yetkilendirme kuralları ayrıştırır farklar inceleyin değil `UrlAuthorizationModule`. Bu konular hakkında daha fazla bilgi için MSDN'de veya, IIS 7 belgelerine başvurun [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>1. adım: URL yetkilendirme kuralları tanımlama`Web.config`

`UrlAuthorizationModule` Uygulamanın yapılandırma dosyasındaki tanımlanan URL yetkilendirme kuralları göre belirli bir kimlik için istenen kaynak için erişim vermek veya reddetmek belirler. Yetkilendirme kuralları içinde yazıldığından [ `<authorization>` öğesi](https://msdn.microsoft.com/en-us/library/8d82143t.aspx) biçiminde `<allow>` ve `<deny>` alt öğeleri. Her `<allow>` ve `<deny>` alt öğesi belirtebilirsiniz:

- Belirli bir kullanıcı
- Kullanıcıların virgülle ayrılmış bir listesi
- Bir soru işareti (?) tarafından belirtilen tüm anonim kullanıcılar
- Tüm kullanıcılar, bir yıldız işaretiyle gösterilir (\*)

Aşağıdaki biçimlendirmede URL yetkilendirme kuralları kullanıcıların Tito ve Scott verin ve diğerlerini reddetmek için nasıl kullanılacağı gösterilmektedir:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

`<allow>` Öğesi tanımlar hangi kullanıcıların - Tito ve Scott - izin verilen sırada `<deny>` öğesi bildirir *tüm* kullanıcılar engellenir.

> [!NOTE]
> `<allow>` Ve `<deny>` öğeleri rolleri için yetkilendirme kuralları da belirtebilirsiniz. Rol tabanlı bir yetkilendirme gelecekteki öğreticideki inceleyeceğiz.


Aşağıdaki ayar (anonim ziyaretçiler dahil) Sam dışındaki bir kişiye erişim verir:

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Yalnızca kimliği doğrulanan kullanıcılar izin vermek için tüm anonim kullanıcılar için erişimini engellediği aşağıdaki yapılandırmayı kullanın:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Yetkilendirme kuralları içinde tanımlanan `<system.web>` öğesinde `Web.config` ve web uygulamasında ASP.NET kaynakların tümünü uygulanır. Bir uygulama görmemeleri, farklı yetkilendirme kuralları için farklı bölümlere sahiptir. Örneğin, bir e-ticaret sitesinde tüm ziyaretçiler ürün tekrar kullanmanıza, ürün incelemeleri bkz, katalogda arama ve benzeri. Ancak, yalnızca kimliği doğrulanan kullanıcılar checkout veya birinin sevkiyat geçmişini yönetmek için sayfaları ulaşabilir. Ayrıca, yalnızca site yöneticileri gibi select kullanıcılar tarafından erişilebilen site bölümlerini olabilir.

ASP.NET, site için farklı dosya ve klasörler için farklı yetkilendirme kurallarını tanımlamak kolaylaştırır. Kök klasör belirtilen yetkilendirme kuralları `Web.config` dosya geçerli sitedeki tüm ASP.NET kaynaklarına. Ancak, bu varsayılan yetkilendirme ayarları için belirli bir klasör ekleyerek geçersiz kılınabilir bir `Web.config` ile bir `<authorization>` bölümü.

Şimdi yalnızca kimliği doğrulanan kullanıcılar ASP.NET sayfaları ziyaret edebileceği Web sitemizi güncelleştirmek `Membership` klasör. Bunu eklemek için ihtiyacımız gerçekleştirmek için bir `Web.config` dosya `Membership` klasörü ve anonim kullanıcıların engellemek için yetkilendirme ayarlarını ayarlayın. Sağ `Membership` Çözüm Gezgininde klasör bağlam menüsünde Yeni Öğe Ekle menü seçeneğini belirleyin ve adlı yeni bir Web yapılandırma dosyası ekleme `Web.config`.


[![Üyelik klasörüne bir Web.config dosyası ekleyin](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Şekil 3**: ekleme bir `Web.config` dosya `Membership` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image9.png))


Bu noktada, projenizin iki içermelidir `Web.config` dosyaları: biri kök dizin, diğeri de `Membership` klasör.


[![Uygulamanızı şimdi iki Web.config dosyaları içermelidir](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Şekil 4**: Bilgisayarınızı uygulama gereken şimdi içeren iki `Web.config` dosyaları ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image12.png))


Yapılandırma dosyasında güncelleştirme `Membership` anonim kullanıcılara erişimi olan yasaklar şekilde klasör.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

Tüm olan İşte bu kadar!

Bu değişiklik test etmek için bir tarayıcıda giriş sayfasını ziyaret edin ve kaydedilir emin olun. Tüm ziyaretçiler izin vermek için bir ASP.NET uygulaması varsayılan davranışını olduğundan ve kök dizinin için yetkilendirme değişiklikleri siz istemediyseniz bu yana `Web.config` dosya, biz anonim bir ziyaretçi olarak kök dizindeki dosyaların ziyaret etmek kullanabilirsiniz.

Sol sütunda bulunan kullanıcı hesapları oluşturma bağlantıyı tıklatın. Bu, olanak sürecek `~/Membership/CreatingUserAccounts.aspx`. Bu yana `Web.config` dosyasını `Membership` klasörü tanımlayan anonim erişimi engellemek için yetkilendirme kuralları `UrlAuthorizationModule` isteğini durdurur ve bir HTTP 401 yetkilendirilmedi durum döndürür. `FormsAuthenticationModule` Bu oturum açma sayfasına gönderdiğiniz bir 302 yeniden yönlendirme durum değiştirir. Sayfa biz erişmeye çalıştığınız olduğunu unutmayın (`CreatingUserAccounts.aspx`) oturum açma sayfasına geçirilen `ReturnUrl` sorgu dizesi parametresi.


[![URL yetkilendirme kuralları engelle anonim erişim itibaren biz oturum açma sayfasına yeniden yönlendirilir](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Şekil 5**: URL yetkilendirme kuralları için anonim erişimi engelle olduğundan, biz oturum açma sayfasına yönlendirilirsiniz ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image15.png))


Başarıyla oturum açtıktan sonra biz yönlendirilirsiniz `CreatingUserAccounts.aspx` sayfası. Bu süre `UrlAuthorizationModule` biz artık anonim olmadığı için sayfa erişimi verir.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>URL yetkilendirme kuralları belirli bir konuma yeniden uygulama

Tanımlanan yetkilendirme ayarları `<system.web>` bölümünü `Web.config` bu dizinde ve alt dizinlerinde ASP.NET kaynakların tümünü uygulamak (Aksi halde bir başkası tarafından geçersiz kılınmış kadar `Web.config` dosyası). Bazı durumlarda, ancak biz tüm ASP.NET kaynakları belirli bir dizinde bir veya iki belirli sayfalara dışında belirli yetkilendirme yapılandırmasına sahip olmak isteyebilirsiniz. Bu ekleyerek sağlanabilir bir `<location>` öğesinde `Web.config`, yetkilendirme kuralları farklı dosyasına işaret eden ve benzersiz yetkilendirme kurallarını okuduğunuzu tanımlama.

Kullanarak göstermeye `<location>` öğesinin belirli bir kaynak için yapılandırma ayarları geçersiz kılar, şimdi yalnızca Tito ziyaret edebilirsiniz yetkilendirme ayarlarını özelleştirmek için `CreatingUserAccounts.aspx`. Bunu başarmak için add bir `<location>` öğesine `Membership` klasörün `Web.config` dosya ve onun biçimlendirmesini şuna benzeyen şekilde güncelleştirin:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

`<authorization>` Öğesinde `<system.web>` varsayılan URL yetkilendirme kuralları için ASP.NET kaynaklarında tanımlar `Membership` klasörde ve alt klasörlerinde. `<location>` Öğesi belirli bir kaynak için bu kurallarını geçersiz kılmasına izin verir. Yukarıdaki biçimlendirmede `<location>` öğesi başvuruları `CreatingUserAccounts.aspx` sayfasında ve kendi yetkilendirme kuralları Tito, izin gibi belirtir ancak Reddet diğerlerinin.

Bu yetkilendirme değişiklik sınamak için anonim kullanıcı olarak Web sitesini ziyaret ederek'ı başlatın. Herhangi bir sayfayı ziyaret edin çalışırsanız `Membership` klasörü gibi `UserBasedAuthorization.aspx`, `UrlAuthorizationModule` isteği reddeder ve oturum açma sayfasına yeniden yönlendirilir. Söyleyin, Scott olarak oturum açtıktan sonra herhangi bir sayfayı ziyaret edebilirsiniz `Membership` klasörü *dışında* için `CreatingUserAccounts.aspx`. Ziyaret çalışılıyor `CreatingUserAccounts.aspx` herkes oturum açmış ancak oturum açma sayfasına yönlendirerek Tito yetkisiz erişim girişimini sonuçlanır.

> [!NOTE]
> `<location>` Öğesi yapılandırma dışında görünmelidir `<system.web>` öğesi. Ayrı bir kullanmanıza gerek `<location>` yetkilendirme ayarlarını geçersiz kılmak istediğiniz her kaynak için öğesi.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Göz nasıl`UrlAuthorizationModule`erişim vermek veya reddetmek için yetkilendirme kuralları kullanır

`UrlAuthorizationModule` URL yetkilendirmesi çözümleyerek belirli bir URL için belirli bir kimliğe yetkilendirmek için birer birer ilk makineden başlatma ve aşağı kendi şekilde çalışan, kuralları olup olmadığını belirler. Bir eşleşme olarak kullanıcı verilen veya erişimi reddedilen, açıksa bağlı olarak eşleşmenin bir `<allow>` veya `<deny>` öğesi. **Eşleşme bulunamazsa, kullanıcıya erişim verilir.** Erişimi kısıtlamak istiyorsanız, bu nedenle, onu kullanmanız zorunludur bir `<deny>` öğesi URL yetkilendirme yapılandırma son öğesi olarak. **Atlarsanız bir ***`<deny>`*** öğesi, tüm kullanıcılara erişim verilecektir.**

Tarafından kullanılan işlem daha iyi anlamak için `UrlAuthorizationModule` yetkilisi belirlemek için şu arama sırasında bu adımda URL yetkilendirme kuralları örneği göz önünde bulundurun. İlk kural bir `<allow>` öğesi Tito ve Scott erişim sağlar. İkinci kuralları bir `<deny>` herkese erişimini engellediği öğesi. Anonim kullanıcı ziyaret, `UrlAuthorizationModule` isteyen tarafından başlatır anonim Scott veya Tito? Belli ki, yanıt yok, olduğundan, ikinci kural devam eder. Herkes kümesinde anonim mi? Bu yana yanıt Evet, işte `<deny>` kural yürürlükte koyun ve ziyaretçi oturum açma sayfasına yeniden yönlendirilir. Benzer şekilde, Jisun ziyaret, `UrlAuthorizationModule` istemekle olan Jisun başlatır Scott veya Tito? Aynen, olmadığından `UrlAuthorizationModule` olan Jisun kümesi herkes, ikinci soruyu devam eder? Aynen, çok, erişim reddedildi depoladığından, olduğundan. Son olarak, Tito ziyaret eder, ilk sorusu sorulmuş `UrlAuthorizationModule` İleticiden olumlu bir yanıt olduğundan, Tito erişimi verilir.

Bu yana `UrlAuthorizationModule` işlemleri yetkilendirme kuralları yukarıdan aşağıya doğru durdurma hiçbir eşleşme en az yayına önce gelen daha belirli kurallar sağlamak önemlidir. Diğer bir deyişle, Jisun ve anonim kullanıcılar yasaklamaz, ancak diğer tüm kimliği doğrulanmış kullanıcıların yetkilendirme kurallarını tanımlamak için en belirgin kuralla - etkileyen bir Jisun - Başlat ve daha az belirli kuralları - olanlar diğer tüm izin vererek devam Kimliği doğrulanmış kullanıcılar, ancak tüm anonim kullanıcılar engelleme. Aşağıdaki URL yetkilendirme kuralları ilk Jisun engelleme ve herhangi bir anonim kullanıcı engelleme bu ilkeyi uygular. Jisun dışındaki herhangi bir kimliği doğrulanmış kullanıcı erişimi olduğundan sağlanacak hiçbirini `<deny>` deyimleri eşleşir.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>2. adım: iş akışı yetkisiz, kimliği doğrulanmış kullanıcılar için düzeltme

Biz A bakabilir URL yetkilendirme iş akışı bölümü, bu öğreticide daha önce açıklandığı gibi dilediğiniz zaman bir yetkisiz isteği transpires, `UrlAuthorizationModule` isteğini durdurur ve bir HTTP 401 yetkilendirilmedi durum döndürür. Bu 401 durum değiştiren `FormsAuthenticationModule` kullanıcı oturum açma sayfasına yeniden gönderir durum 302 yeniden yönlendirme. Kullanıcının kimliği doğrulanır olsa bile bu iş akışı yetkisiz herhangi bir istek üzerinde oluşur.

Kimliği doğrulanmış bir kullanıcı oturum açma sayfasına döndürme sisteme zaten oturum olduğundan bunları işlemlerini birbirine karıştırmaktadır olasılığı yüksektir. Biraz iş ile Biz bu iş akışı, bunların sınırlı bir sayfaya erişmeye çalıştınız açıklayan bir sayfaya yetkisiz isteklerde kimliği doğrulanmış kullanıcıları yönlendirerek artırabilir.

Başlangıç adlı web uygulamasının kök klasöründe yeni bir ASP.NET sayfası oluşturarak `UnauthorizedAccess.aspx`; bu sayfayla ilişkilendirilecek unutmayın `Site.master` ana sayfa. Bu sayfayı oluşturduktan sonra başvuran içerik denetimi kaldırılmaya `LoginContent` ContentPlaceHolder ana sayfa varsayılan böylece içerik görüntülenir. Sonra durumu, yani açıklayan bir ileti ekleyin kullanıcı korunan bir kaynağa erişme girişiminde bulunuldu. Bu tür bir ileti ekledikten sonra `UnauthorizedAccess.aspx` sayfanın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Şimdi bir yetkisiz isteği kimliği doğrulanmış bir kullanıcı tarafından gerçekleştirilirse gönderilirler böylece iş akışını değiştirmek ihtiyacımız `UnauthorizedAccess.aspx` sayfası yerine oturum açma sayfası. Yetkisiz istekler oturum açma sayfasına yönlendirir mantığı içinde özel bir yöntem kaçınma `FormsAuthenticationModule` Biz bu davranış özelleştiremezsiniz şekilde sınıfı. Biz, ancak ne kendi mantığımızı kullanıcıya yönlendiren oturum açma sayfasına eklemektir `UnauthorizedAccess.aspx`gerekirse.

Zaman `FormsAuthenticationModule` yetkisiz bir ziyaretçi yönlendiren isteğe bağlı olarak oturum açma sayfasına adıyla querystring istenen, yetkisiz URL'sine ekler `ReturnUrl`. Örneğin, yetkisiz bir kullanıcının ziyaret çalışıldı `OnlyTito.aspx`, `FormsAuthenticationModule` bunları yeniden yönlendirme `Login.aspx?ReturnUrl=OnlyTito.aspx`. Bu nedenle, oturum açma sayfasını içeren bir sorgu dizesi sahip kimliği doğrulanmış bir kullanıcı tarafından ulaşıldığında `ReturnUrl` parametresi sonra biz biliyorsanız bu kimliği doğrulanmamış bir kullanıcı yalnızca aynen yetkilendirilmemiş görüntülemek için bir sayfasını ziyaret denediğini. Böyle bir durumda, kendisi için yeniden yönlendirme istiyoruz `UnauthorizedAccess.aspx`.

Bunu başarmak için oturum açma sayfasına ait aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Yukarıdaki kod kimliği doğrulanmış, yetkisiz kullanıcılara yönlendiren `UnauthorizedAccess.aspx` sayfası. Bu eylem mantığında görmek için anonim bir ziyaretçi olarak sitesini ziyaret edin ve sol sütunda kullanıcı hesapları oluşturma bağlantıyı tıklatın. Bu, olanak sürecek `~/Membership/CreatingUserAccounts.aspx` biz yapılandırılmış yalnızca 1. adımda erişimine izin vermek için Tito sayfasını. Anonim kullanıcılar yasaklanmış olduğundan, `FormsAuthenticationModule` bize oturum açma sayfasına yönlendirir.

Bu noktada anonim gerçekleştiriyoruz, bu nedenle `Request.IsAuthenticated` döndürür `false` ve biz yönlendirilir değil `UnauthorizedAccess.aspx`. Bunun yerine, oturum açma sayfası görüntülenir. Tito Bruce'a gibi başka bir kullanıcı olarak oturum açın. Uygun kimlik bilgilerini girdikten sonra oturum açma sayfasında üzerine bize yeniden yönlendirir `~/Membership/CreatingUserAccounts.aspx`. Ancak, bu sayfayı yalnızca Tito için erişilebilir olduğundan, biz görüntülemek için yetkisiz ve derhal oturum açma sayfasına dönersiniz. Bu süre, ancak `Request.IsAuthenticated` döndürür `true` (ve `ReturnUrl` sorgu dizesi parametresi var.), böylece biz yönlendirilirsiniz `UnauthorizedAccess.aspx` sayfası.


[![Kimlik doğrulaması, yetkisiz kullanıcılar için UnauthorizedAccess.aspx yönlendirilir](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Şekil 6**: kimlik doğrulaması, yetkisiz kullanıcıların yönlendirilirsiniz `UnauthorizedAccess.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image18.png))


Özelleştirilmiş bu iş akışı, Şekil 2'de gösterilen döngüsü kestirmeler tarafından daha duyarlı ve basit bir kullanıcı deneyimi sunar.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>3. adım: üzerinde şu anda oturum açmış olan kullanıcının tabanlı işlevsellik sınırlama

URL yetkilendirmesi, kaba yetkilendirme kurallarını belirtmek kolaylaştırır. Adım 1'de gördüğümüz gibi URL yetkilendirmesi ile biz temellerini hangi kimlikleri izin verilen ve hangilerinin belirli bir sayfa veya tüm sayfaların bir klasörde görüntüleme reddedilir durumu. Bazı senaryolarda, ancak biz tüm kullanıcıların bir sayfasını ziyaret edin, ancak bunu ziyaret kullanıcıyı temel alarak sayfanın işlevselliği sınırlamak için izin vermek isteyebilirsiniz.

Ürünlerini gözden geçirmek kimliği doğrulanmış ziyaretçileri izin veren bir e-ticaret Web sitesi durumunu göz önünde bulundurun. Adsız bir kullanıcı bir ürün sayfasını ziyaret ettiğinde, yalnızca ürün bilgileri görür ve bir gözden geçirme bırakın fırsatı verilmedi. Ancak, aynı sayfasını ziyaret kimliği doğrulanmış bir kullanıcı gözden geçirme arabirimi görür. Kimliği doğrulanmış kullanıcının henüz bu ürünün gözden değil, bunları bir gözden geçirme göndermek arabirimi sağlayacağı; Aksi takdirde, bunları önceden gönderilmiş gözden gösterir. Bu senaryo bir adım almak için daha fazla ürün sayfasını ek bilgileri göster ve e-ticaret şirket için çalışan kullanıcılar için genişletilmiş özellikler sunar. Örneğin, ürün sayfasını bir stok stoktaki listelemek ve ürünün fiyat ve bir çalışan tarafından ziyaret edildiğinde açıklama düzenlemek için seçenekleri içerir.

Bu tür ince çizgisi yetkilendirme kuralları, bildirimli olarak veya program aracılığıyla (ya da iki şunların bir birleşimi yoluyla) uygulanabilir. Sonraki bölümde ince çizgisi yetkilendirme LoginView denetimi aracılığıyla uygulama göreceğiz. Programlı teknikleri inceleyeceksiniz. Biz ince çizgisi yetkilendirme kurallarını uygulayarak en bakabilirsiniz önce ancak biz öncelikle bir sayfa, ziyaret eden kullanıcının olan işlevsellik bağlıdır oluşturmanız gerekir.

GridView içinde belirli bir dizindeki dosyaları listeleyen bir sayfa oluşturalım. Her dosyanın adı, boyutu ve diğer bilgileri listeleyen yanı sıra, GridView LinkButtons iki sütunları içerir: biri başlıklı görünümü ve bir başlıklı silme. Görünüm LinkButton tıkladıysanız, seçilen dosya içeriğini görüntülenir; Dosya Sil LinkButton tıkladıysanız silinir. Görünüm ve delete işlevselliğini tüm kullanıcılar için kullanılabilir olacak şekilde, başlangıçta bu sayfayı oluşturalım. Kullanarak, biz nasıl etkinleştirileceği veya sayfasını ziyaret kullanıcıyı temel alarak bu özellikleri devre dışı bırakılacağı görürsünüz LoginView denetimi ve program aracılığıyla sınırlama işlevselliği bölümler.

> [!NOTE]
> Yaklaşık yapı duyuyoruz ASP.NET sayfası GridView denetimi dosyaların bir listesini görüntülemek için kullanır. Form kimlik doğrulaması, yetkilendirme, kullanıcı hesapları ve rolleri serisi odaklanan Bu öğretici itibaren çalışmalar GridView denetimini ele çok fazla süre beklemesini istemiyorum. Bu öğretici bu sayfası ayarlama belirli adım adım yönergeler sağlar, ancak bunu neden belirli seçimler yapılan veya ne işlenmiş çıktı etkisi belirli özelliklere sahip ayrıntılarını içine inceleyin değil. GridView denetiminin kapsamlı incelenmesi için başvurun my  *[, ASP.NET 2.0 verilerle çalışma](../../data-access/index.md)*  öğretici serisi.


Başlangıç açarak `UserBasedAuthorization.aspx` dosyasını `Membership` klasörü ve GridView denetim adlı sayfası ekleme `FilesGrid`. GridView kullanıcının akıllı etiketten alanları iletişim kutusunu başlatmak için sütunları Düzenle bağlantısına tıklayın. Buradan, sol alt köşedeki otomatik oluşturma alanları onay kutusunun işaretini kaldırın. Ardından, üst sol Köşeden (seçin ve Sil düğmeleri CommandField türü'nün altında bulunabilir) seçme düğmesi, bir Delete düğmesi ve iki BoundFields ekleyin. Select düğmenin ayarlamak `SelectText` özelliğini görünümü ve ilk BoundField'ın `HeaderText` ve `DataField` özellikleri adı. İkinci BoundField's ayarlamak `HeaderText` bayt cinsinden boyutu özelliğine kendi `DataField` uzunluğu özelliğine kendi `DataFormatString` {0: N0} özelliğine ve onun `HtmlEncode` özelliğini false olarak ayarlayın.

GridView'ın sütunları yapılandırdıktan sonra alanları iletişim kutusunu kapatmak için Tamam'ı tıklatın. GridView ait Özellikler penceresinden ayarlamak `DataKeyNames` özelliğine `FullName`. Bu noktada GridView'ın bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Oluşturulan GridView'ın biçimlendirme ile biz belirli bir dizindeki dosyaları almak ve bunları bağlamak için GridView kod yazmaya hazırsınız. Sayfanın aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Yukarıdaki kod kullanan [ `DirectoryInfo` sınıfı](https://msdn.microsoft.com/en-us/library/system.io.directoryinfo.aspx) uygulamanın kök klasördeki dosyaların listesini elde edilir. [ `GetFiles()` Yöntemi](https://msdn.microsoft.com/en-us/library/system.io.directoryinfo.getfiles.aspx) tüm dosyaları dizinde bir dizi döndürür [ `FileInfo` nesneleri](https://msdn.microsoft.com/en-us/library/system.io.fileinfo.aspx), ardından GridView bağlı. `FileInfo` Nesneye sahip bir sınıflama özelliklerinin gibi `Name`, `Length`, ve `IsReadOnly`, diğerlerinin yanı sıra. Bildirim temelli biçimlendirmeden gördüğünüz GridView yalnızca görüntüler `Name` ve `Length` özellikleri.

> [!NOTE]
> `DirectoryInfo` Ve `FileInfo` içinde bulunan sınıflar [ `System.IO` ad alanı](https://msdn.microsoft.com/en-us/library/system.io.aspx). Bu nedenle, bu sınıf adları ad alanı adları ile yazdığınızdan veya sınıf dosyasına içeri aktarılan ad alanınız için her iki gereksinim olur (aracılığıyla `using System.IO`).


Bu sayfa bir tarayıcı aracılığıyla ziyaret etmek için bir dakikanızı ayırın. Bu uygulamanın kök dizininde bulunan dosyaların listesini görüntüler. Herhangi bir görünüm veya silme LinkButtons tıklatarak geri gönderimin neden olur, ancak biz için henüz olduğunuz için hiçbir eylem meydana gelir gerekli olay işleyicileri oluşturma.


[![GridView Web uygulamasının kök dizindeki dosyaların listeler](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Şekil 7**: GridView Web uygulamasının kök dizindeki dosyaların listeler ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image21.png))


Seçilen dosya içeriğini görüntülemek için bir yol ihtiyacımız var. Visual Studio'ya dönmek ve adlı bir metin kutusu ekleyin `FileContents` GridView üstünde. Ayarlama, `TextMode` özelliğine `MultiLine` ve kendi `Columns` ve `Rows` özellikleri % 95 ve 10, sırasıyla.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Ardından, olay işleyicisi GridView için 's oluşturmak [ `SelectedIndexChanged` olay](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Bu kodu GridView's kullanır `SelectedValue` seçilen dosyanın tam dosya adını belirlemek için özellik. Dahili olarak, `DataKeys` koleksiyonu almak için başvurulmaktadır `SelectedValue`, GridView's ayarladığınız zorunludur `DataKeyNames` özelliği daha önce bu adımda anlatıldığı gibi adı. [ `File` Sınıfı](https://msdn.microsoft.com/en-us/library/system.io.file.aspx) bir dizeye sonra atanan seçilen dosyanın içeriğini okumak için kullanılan `FileContents` TextBox'ın `Text` özelliği, dolayısıyla sayfada seçilen dosyanın içeriğini görüntüleme.


[![Seçili dosyanın içeriğini metin kutusunda görüntülenir](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Şekil 8**: seçilen dosyanın içeriğini metin kutusunda görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> HTML biçimlendirmesini içeren bir dosyanın içeriğini görüntüleyin ve görüntülemek veya dosya silme denemesi, alacak bir `HttpRequestValidationException` hata. Bu durum, web sunucusuna gönderilen geri göndermede TextBox'ın içeriği kaynaklanır. Varsayılan olarak, ASP.NET başlatır bir `HttpRequestValidationException` HTML biçimlendirmesi gibi potansiyel olarak tehlikeli olabilecek geri gönderme içerik algılandığında hata. Bu hata oluşmasını devre dışı bırakmak için sayfa için istek doğrulamayı ekleyerek devre dışı `ValidateRequest="false"` için `@Page` yönergesi. Hangi önlemleri iyi ne zaman almanız gereken olarak istek doğrulama yararları hakkında daha fazla bilgi için devre dışı bırakma okuma [istek doğrulama - komut dosyası saldırılarını önleme](https://asp.net/learn/whitepapers/request-validation/).


Son olarak, aşağıdaki kod ile olay işleyicisi GridView için 's eklemek [ `RowDeleting` olay](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Kod silmek için dosyanın tam adını görüntüler `FileContents` TextBox *olmadan* gerçekten dosyası siliniyor.


[![Sil düğmesini tıklatarak gerçekte dosyayı silmez](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Şekil 9**: Dosya Sil düğmesine gerçekte silmez'yı tıklayarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image27.png))


1. adımda biz URL yetkilendirme kuralları, anonim kullanıcılar sayfalarında görüntülemesini engellemek için yapılandırılmış `Membership` klasör. Daha iyi ince çizgisi kimlik doğrulaması göstermesi için şirketinizdeki ziyaret etmek anonim kullanıcıların `UserBasedAuthorization.aspx` sayfasında, ancak işlevselliği sınırlıdır. Tüm kullanıcılar tarafından erişilebilmesi için bu sayfayı açmak için aşağıdakileri ekleyin `<location>` öğesine `Web.config` dosyasını `Membership` klasörü:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Bunu ekledikten sonra `<location>` öğesi, site dışında açarak yeni URL yetkilendirme kuralları sınayın. Anonim kullanıcı olarak, ziyaret izni verilmesi gerekip `UserBasedAuthorization.aspx` sayfası.

Şu anda, herhangi bir doğrulanmış veya anonim kullanıcı ziyaret edebilirsiniz `UserBasedAuthorization.aspx` sayfasında ve görüntülemek veya dosyaları silin. Böylece yalnızca kimliği doğrulanmış kullanıcılar bir dosyanın içeriğini görüntüleyebilir ve yalnızca Tito bir dosyayı silebilirsiniz olalım. Bu tür ince çizgisi yetkilendirme kuralları, bildirimli olarak, program aracılığıyla veya her iki yöntemlerinin bir birleşimini aracılığıyla uygulanabilir. Bir dosyanın içeriğini görüntüleyebilecek sınırlamak için bildirim temelli bir yaklaşım kullanalım; bir dosyayı silebilirsiniz sınırı programlı yaklaşım kullanacağız.

### <a name="using-the-loginview-control"></a>LoginView denetimi kullanma

Geçmiş eğitimlerine anlatıldığı gibi LoginView denetimi kimliği doğrulanmış ve anonim kullanıcılar için farklı arabirimlerini görüntülemek için yararlıdır ve anonim kullanıcılar için erişilebilir değil işlevselliği gizlemek için kolay bir yol sunar. Anonim kullanıcılar görüntüleyemez veya dosyaları silin olduğundan, yalnızca göstermek ihtiyacımız `FileContents` sayfa kimliği doğrulanmış bir kullanıcı tarafından ziyaret edildiğinde metin kutusu. Bunu başarmak için sayfaya LoginView denetim ekleme, adlandırın `LoginViewForFileContentsTextBox`ve taşıma `FileContents` TextBox'ın bildirim temelli biçimlendirme LoginView denetimin içine `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Web denetimleri (link)'ın şablonlarındaki artık arka plan kodu sınıfından doğrudan erişilebilir. Örneğin, `FilesGrid` GridView'ın `SelectedIndexChanged` ve `RowDeleting` olay işleyicileri şu anda başvuru `FileContents` TextBox denetimi koduyla ister:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Ancak, bu kod artık geçerli değil. Taşıyarak `FileContents` metin kutusuna `LoggedInTemplate` TextBox doğrudan erişilemez. Bunun yerine, biz kullanmalısınız `FindControl("controlId")` programlı olarak denetimi başvurmak için yöntem. Güncelleştirme `FilesGrid` TextBox başvurmak için olay işleyicileri sözlüğüdür:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

TextBox (link) için 's taşıdıktan `LoggedInTemplate` ve sayfanın kodunu kullanarak metin kutusuna referansı güncelleme `FindControl("controlId")` deseni, bir anonim kullanıcı olarak sayfasını ziyaret edin. Şekil 10 gösterildiği gibi `FileContents` TextBox görüntülenmez. Ancak, görünümü LinkButton hala görüntülenir.


[![LoginView denetimi yalnızca kimliği doğrulanmış kullanıcılar için FileContents TextBox işler](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Şekil 10**: LoginView denetimi yalnızca işler `FileContents` TextBox kimliği doğrulanmış kullanıcılar için ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image30.png))


Anonim kullanıcılar için görünümü düğmesini gizlemek için bir GridView alan TemplateField'a dönüştürmek için yoludur. Bu görünüm LinkButton için bildirim temelli biçimlendirme içeren bir şablon oluşturur. Ardından TemplateField LoginView denetimi ekleyebilir ve LinkButton (link)'ın içinde yerleştirin `LoggedInTemplate`, böylece anonim ziyaretçiler görünüm düğmesinden gizleme. Bunu gerçekleştirmek için alanları iletişim kutusunu başlatmak için GridView kullanıcının akıllı etiket sütunları düzenleme bağlantısını tıklayın. Ardından, sol alt köşesinde listesinden Seç düğmesini seçin ve bu alan TemplateField bağlantısına dönüştürme'ye tıklayın. Bunun yapılması alanın bildirim temelli biçimlendirmeden değiştirir:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 Hedef: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

Bu noktada, biz TemplateField bir LoginView ekleyebilirsiniz. Aşağıdaki biçimlendirmede yalnızca kimliği doğrulanmış kullanıcılar için Görünüm LinkButton görüntüler.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Şekil 11 gösterildiği gibi sonuç görünümü LinkButtons sütun içinde gizli olsa bile oldukça görünüm olarak sütun hala görüntülendiğini değil. Biz ne tüm GridView Sütun (ve yalnızca LinkButton) gizlemek sonraki bölümde ele alacağız.


[![LoginView denetimi görünüm LinkButtons anonim ziyaretçiler için gizler.](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Şekil 11**: LoginView denetimi görünüm LinkButtons anonim ziyaretçiler için gizler ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Program aracılığıyla sınırlama işlevi

Bazı durumlarda, bildirim temelli teknikleri bir sayfaya işlevselliği sınırlamak için yeterli değil. Örneğin, belirli sayfa işlevselliği kullanılabilirliğini sayfasını ziyaret kullanıcı anonim veya kimliği doğrulanmış olup ötesinde ölçütleri bağımlı olabilir. Böyle durumlarda, çeşitli kullanıcı arabirimi öğeleri görüntülenir veya programlama yollarla gizlenir.

Program aracılığıyla işlevselliği sınırlamak için iki görevleri gerçekleştirmek ihtiyacımız var:

1. Sayfasını ziyaret kullanıcı işlevselliği erişip erişemeyeceğini belirler ve
2. Program aracılığıyla kullanıcının söz konusu işlevine erişimi olup tabanlı kullanıcı arabirimi değiştirin.

Bu iki görevler uygulama göstermek için şimdi yalnızca GridView dosyaları silmek Tito izin verir. Bizim ilk, ardından sayfasını ziyaret Tito olup olmadığını belirlemek için bir görevdir. Belirlendikten sonra biz GridView'ın sütun Sil (göstermek veya gizlemek) gerekir. GridView'ın sütunları üzerinden erişilebilir kendi `Columns` özellik; bir sütun ise yalnızca işlenen kendi `Visible` özelliği ayarlanmış `true` (varsayılan).

Aşağıdaki kodu ekleyin `Page_Load` GridView veri bağlama önce olay işleyicisi:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Biz anlatıldığı gibi [ *form kimlik doğrulaması bir genel bakış* ](../introduction/an-overview-of-forms-authentication-cs.md) öğretici, `User.Identity.Name` kimliğin adını döndürür. Bu, oturum açma denetimi girilen kullanıcı adının karşılık gelir. Sayfanın, GridView'ın ikinci sütun ziyaret Tito olup olmadığını `Visible` özelliği ayarlanmış `true`; Aksi takdirde ayarlanır `false`. Net sonucudur birisi Tito dışındaki başka bir kimliği doğrulanmış kullanıcı veya anonim bir kullanıcı, sayfayı ziyaret ettiğinde Sütun Sil (bkz. Şekil 12); işlenmez Sütun Sil Tito sayfasını ziyaret ettiğinde, ancak varsa (bkz. Şekil 13).


[![Değil çizilir zaman ziyaret tarafından birisi dışında Tito (örneğin, Bruce'a) Sil sütundur](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Şekil 12**: Sil sütundur değil çizilir zaman ziyaret tarafından birisi dışında Tito (örneğin, Bruce'a) ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image36.png))


[![Sütun Sil Tito için işlenir](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Şekil 13**: Column silmek için Tito çizilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>4. adım: Sınıflar ve yöntemler için yetkilendirme kuralları uygulama

Adım 3'te anonim kullanıcılar bir dosyanın içeriğini görüntülemesine izin verilmeyen ve tüm kullanıcılar, ancak Tito dosyaları silme yasaktır. Bu, bildirim temelli ve programlama teknikleri aracılığıyla yetkisiz ziyaretçiler için ilişkili kullanıcı arabirimi öğelerini gizleme tarafından gerçekleştirilmiştir. Basit Bizim örneğimizde, kullanıcı arabirimi öğeleri doğru gizleme açık, ancak ne oluştu daha karmaşık siteleri olabilir; burada aynı işlevleri gerçekleştirmek için birçok farklı yolu? Tüm geçerli kullanıcı arabirimi öğeleri devre dışı bırakmak veya gizlemek unutursanız yetkisiz kullanıcılara bu işlevselliği sınırlama içinde ne olur?

Bu sınıf veya yöntemin ile işaretleme işlevsellik belirli bir parçasını yetkisiz kullanıcılar tarafından erişilen emin olmak için kolay bir yoludur [ `PrincipalPermission` özniteliği](https://msdn.microsoft.com/en-us/library/system.security.permissions.principalpermissionattribute.aspx). .NET çalışma zamanı sınıf kullanır veya yöntemlerinden birini yürütür, geçerli güvenlik bağlamı sınıf kullanın veya yöntemi yürütme izni olduğundan emin olmak için denetler. `PrincipalPermission` Özniteliği üzerinden biz tanımlayabilirsiniz bu kurallar bir mekanizma sağlar.

Kullanarak gösterelim `PrincipalPermission` GridView'ın öznitelikte `SelectedIndexChanged` ve `RowDeleting` anonim kullanıcılar ve Tito, kullanıcılar tarafından yürütme sırasıyla engellemek için olay işleyicileri. Yapmamız gereken tek şey her işlev tanımı üzerinde uygun özniteliği ekleyin:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

Öznitelik için `SelectedIndexChanged` yalnızca kimliği doğrulanmış kullanıcılar olay işleyicisi belirtir, burada özniteliği olarak olay işleyicisi yürütebilir `RowDeleting` olay işleyicisi yürütme Tito için sınırlar.

Tito başka bir kullanıcı bu şekilde, yürütmeyi denediğinde, `RowDeleting` olay işleyicisi veya doğrulanmamış bir kullanıcı yürütme girişimlerini `SelectedIndexChanged` .NET çalışma zamanı olay işleyicisi yükseltmek bir `SecurityException`.


[![Güvenlik bağlamı yöntemi yürütmek için yetkili değil, bir SecurityException oluşturulur](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Şekil 14**: güvenlik bağlamını yöntemi yürütmek için yetkili değil, bir `SecurityException` atılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Birden çok güvenlik kapsamları bir sınıf veya yöntemin erişmesine izin vermek için sınıf veya yöntemin ile işaretleme bir `PrincipalPermission` özniteliği için her güvenlik bağlamı. Diğer bir deyişle, Tito ve Bruce'a yürütmek izin vermek için `RowDeleting` olay işleyicisi ekleme *iki* `PrincipalPermission` öznitelikleri:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Birçok uygulama, ASP.NET sayfaları ek olarak, iş mantığı ve verileri erişim katmanları gibi çeşitli katmanları içeren bir mimari de vardır. Bu katmanlar genellikle sınıf kitaplıkları uygulanır ve sınıfları ve iş mantığı ve verileri ilgili işlevleri gerçekleştirmek için yöntemleri sunar. `PrincipalPermission` Özniteliği bu katmanlara yetkilendirme kuralları uygulamak için yararlıdır.

Kullanma hakkında daha fazla bilgi için `PrincipalPermission` sınıflar ve yöntemler yetkilendirme kuralları tanımlamak, başvurmak için öznitelik [Scott Guthrie](https://weblogs.asp.net/scottgu/)'s blog girdisi [iş ve veri kullanan katmanlar içinyetkilendirmekurallarıekleme`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Özet

Bu öğreticide kullanıcı tabanlı yetkilendirme kuralları uygulamak ne arama. ASP göz ile başlatıldı. NET'in URL yetkilendirme çerçevesi. Her isteğin, ASP.NET altyapısı üzerinde `UrlAuthorizationModule` kimliğini istenen kaynağa erişmek için yetkili olup olmadığını belirlemek için uygulamanın yapılandırma dosyasındaki tanımlanan URL yetkilendirme kurallarını denetler. Kısacası, URL yetkilendirmesi, belirli bir dizindeki tüm sayfalar için veya belirli bir sayfa için yetkilendirme kuralları belirtmek kolaylaştırır.

URL yetkilendirme framework bir sayfa tarafından temelinde yetkilendirme kurallarını uygular. URL yetkilendirmesi ile isteyen kimliğini belirli bir kaynağa erişim yetkisi. Birçok senaryo, ancak daha fazla ince çizgisi yetkilendirme kuralları için çağırın. Kimin bir sayfa erişim izni tanımlamanın yerine, biz herkes bir sayfa erişim sağlar, ancak farklı verileri gösterir veya sayfasını ziyaret kullanıcı bağlı olarak farklı işlevler sunar gerekebilir. Sayfa düzeyinde yetkilendirme çoğunlukla, yetkisiz kullanıcıların yasaklanmış işlevselliği erişimini engellemek için belirli bir kullanıcı arabirimi öğelerini gizleme kapsar. Ayrıca, sınıflar ve belirli kullanıcılar için yöntemlerini yürütülmesi için erişimi kısıtlamak için öznitelikleri kullanmak da mümkündür.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [İş ve veri katmanlarını kullanmak için yetkilendirme kuralları ekleme`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET yetkilendirmesi](https://msdn.microsoft.com/en-us/library/wce3kxhd.aspx)
- [IIS6 ve IIS7 güvenlik arasındaki değişiklikleri](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Belirli dosya ve alt dizinleri yapılandırma](https://msdn.microsoft.com/en-us/library/6hbkh9s7.aspx)
- [Kullanıcıyı temel alarak bir sınırlama veri değişikliği işlevi](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [LoginView denetimi QuickStarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [IIS7 URL yetkilendirmesi anlama](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule`Teknik belgeler](https://msdn.microsoft.com/en-us/library/system.web.security.urlauthorizationmodule.aspx)
- [ASP.NET 2.0 verilerle çalışma](../../data-access/index.md)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Önceki](validating-user-credentials-against-the-membership-user-store-cs.md)
[sonraki](storing-additional-user-information-cs.md)
