---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Nasıl yapılır: ASP.NET Web formlarınızı mobil sayfalar ekleme / MVC uygulaması | Microsoft Docs'
author: rick-anderson
description: Bu nasıl yapılır sayfaları, ASP.NET Web Forms mobil cihazlar için en iyi duruma getirilmiş için çeşitli yollar açıklanmaktadır / MVC uygulama ve mimari önerir ve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: a8358b91ca424f4f3e576057ab43d850081dda60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Nasıl yapılır: ASP.NET Web formlarınızı mobil sayfalar ekleme / MVC uygulaması
====================
> **Uygulandığı öğe:**
> 
> - ASP.NET Web Forms sürüm 4.0
> - ASP.NET MVC sürüm 3.0
> 
> **Özet**
> 
> Bu nasıl yapılır sayfaları, ASP.NET Web Forms mobil cihazlar için en iyi duruma getirilmiş için çeşitli yollar açıklanmaktadır / MVC uygulaması mimari önerir ve tasarım çok çeşitli aygıtları hedeflerken dikkate alınacak konular. Bu belgede ayrıca neden ASP.NET Mobil denetimleri ASP.NET 2.0 3.5 için artık kullanılmayan ve bazı modern alternatifleri anlatılmaktadır açıklanmaktadır.


## <a name="contents"></a>İçindekiler

- Genel Bakış
- Mimari seçenekleri
- Tarayıcı ve cihaz algılama
- ASP.NET Web formları uygulamalarını mobile özgü sayfaları nasıl sunabilir
- ASP.NET MVC uygulamaları mobile özgü sayfaları nasıl sunabilir
- Ek kaynaklar

ASP.NET Web Forms ve MVC için bu incelemeyi 's teknikleri gösteren indirilebilir kod örnekleri için bkz: [Mobile Apps & ASP.NET siteleriyle](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Genel Bakış

Mobil aygıtlar – akıllı telefonlar, özellik telefonlar ve tabletler – Web erişimi için bir yol olarak popülerliği büyümeye devam. Birçok web geliştiricileri ve web odaklı işletmeler için bu aygıtların kullanan ziyaretçilere için harika bir gözatma deneyimi sağlamak giderek daha çok önemlidir anlamına gelir.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Desteklenen ASP.NET mobil tarayıcılar nasıl önceki sürümleri

ASP.NET sürüm 2.0 için dahil 3.5 *ASP.NET Mobil denetimler*: mobil cihazlar için sunucu denetimleri kümesini *System.Web.Mobile.dll* derleme ve  *System.Web.UI.MobileControls* ad alanı. Derleme hala ASP.NET 4'te bulunur, ancak kullanım dışıdır. Geliştiriciler, bu makalede açıklanan gibi daha modern yaklaşımlar geçirmek için önerilir.

Neden ASP.NET Mobil denetimler geçersiz olarak işaretlenmiş tasarımlarını 2005 ve önceki geçici ortak cep telefonları geçici yönelik olduğunu nedenidir. Denetimler çoğunlukla WML veya cHTML biçimlendirmesi (yerine normal HTML) oluşturmak için bu dönem WAP tarayıcılar için tasarlanmıştır. Ancak HTML artık aynı şekilde mobil ve masaüstü tarayıcıları için bulunabilen biçimlendirme dili haline geldiğinden WAP, WML ve cHTML artık çoğu projeleri için uygundur.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Mobil cihaz bugün destekleyen zorlukları

Mobil tarayıcılar şimdi neredeyse Evrensel HTML destek olsa bile, birçok zorluklar harika mobil gözatma deneyimleri oluşturmak amacıyla, hala yüz:

- ***Ekran boyutu*** - mobil cihazlar farklılık önemli ölçüde formunda ve ekranlarını genellikle çok olan Masaüstü monitör küçüktür. Bu nedenle, tamamen farklı sayfa düzenleri kendileri için tasarım gerekebilir.
- ***Giriş yöntemleri*** – keypads bazı aygıtlarınız varsa, bazı styluses sahip, diğerleri dokunma kullanın. Birden çok gezinti mekanizmaları ve veri giriş yöntemi dikkate almanız gerekebilir.
- ***Standartları uyumluluğu*** – birçok mobil tarayıcılar son HTML, CSS ve JavaScript standartları desteklemez.
- ***Bant genişliği*** – hücresel veri ağ performansını değişir müthiş bir başarı ve bazı son kullanıcılar tarafından megabayt ücret tariffs yer alır.

BT'ye çözümü yoktur; Uygulamanızı görünebilir ve erişmekte cihaza göre farklı davranabilir gerekecektir. Hangi mobil destek düzeyini istediğiniz bağlı olarak, bu "tarayıcı çatışmaları" Masaüstü her zamankinden daha büyük bir sınama web geliştiricileri için olabilir.

İlk kez mobil tarayıcı desteği genellikle başlangıçta yaklaşan geliştiriciler yalnızca geliştiriciler genellikle kişisel gibi sahibi için en son ve en karmaşık akıllı telefonlar (örneğin, Windows Phone 7, iPhone veya Android), belki de desteklemek önemli olduğunu düşündüğünüz cihazlar. Ancak, ucuz telefonları hala son derece popüler ve sahiplerinin – özellikle, cep telefonları kolay almak geniş bant bağlantısı olduğu ülkede Web'e göz atmak için bunları kullanın. İşletmenizin cihazları, büyük olasılıkla müşterilerinin dikkate alarak desteklemek için hangi aralığını karar vermeniz gerekir. Lüks sistem durumu spa için çevrimiçi bir Broşürü oluşturuyorsanız, bir sinema için bir bilet kayıt sistemi oluşturuyorsanız, büyük olasılıkla daha az güçlü özelliğiyle ziyaretçiler için hesap gerek ise, yalnızca Gelişmiş akıllı telefonlar, hedeflemek için bir iş karar vermeniz telefonları.

## <a name="architectural-options"></a>Mimari seçenekleri

Biz ASP.NET Web Forms veya MVC belirli teknik ayrıntılara ulaşmadan önce web geliştiricileri genel mobil tarayıcılar desteklemek için üç ana olası seçenekler gerektiğini unutmayın:

1. ***Hiçbir şey –*** sadece, standart, Masaüstü odaklı web uygulaması oluşturun ve çalışarak işlemek için mobil tarayıcılar kullanır. 

    - **Avantajı**: uygulamak ve bakımını – ek çalışmak için ucuz seçenektir
    - **Olumsuz**: en kötü son kullanıcı deneyimi sağlar: 

        - En son Akıllı telefonlar, HTML da bir masaüstü tarayıcısı olarak duruma getirebilir, ancak kullanıcılar hala yakınlaştırma ve kaydırma yatay ve dikey olarak küçük ekranda, içeriği kullanmak için zorunlu kılınır. Bu, gölgeden uzak idealdir.
        - Eski aygıtları ve özellik telefonları tatmin edici şekilde, biçimlendirmesi oluşturmak başarısız olabilir.
        - Aygıtlarda (olan ekranlar olabilir dizüstü bilgisayar ekranları kadar büyük) bile son tablet, farklı etkileşim kuralları geçerlidir. Dokunma tabanlı giriş büyük düğmeleriyle en iyi şekilde çalışır ve forma daha fazla büyüklüğü bağlar ve fare imlecini uçarak çıkış menünün üzerine getirin bir yolu yoktur.
2. ***İstemcide sorun gidermenize* –** CSS, dikkatli kullanımıyla ve [kademeli geliştirmeyi](http://en.wikipedia.org/wiki/Progressive_enhancement) biçimlendirme, stilleri ve bunları çalışıyor ne olursa olsun tarayıcı için uyum betikleri oluşturabilirsiniz. Örneğin, [CSS 3 medya sorguları](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), aygıtlar, ekranlar seçilen eşikten daha dar bir tek sütun düzeni açar çok sütun düzeni oluşturabilirsiniz. 

    - **Avantajı**: işleme için belirli bir aygıtı kullanımda ne olursa olsun ekran ve giriş özelliklere sahip oldukları göre bile bilinmeyen gelecekteki cihazlar için en iyi duruma getirir
    - **Avantajı**: kolayca sunucu tarafı mantığı tüm cihaz türlerine – kod veya çabasını en az yineleme paylaşmanıza olanak tanır
    - **Olumsuz**: mobil cihazları gerçekten Masaüstü sayfalarından tamamen farklı olması için mobil sayfalarından farklı bilgiler gösteren istediğiniz masaüstü aygıtlardan çok farklı. Bu tür Çeşitlemeler verimsiz olabilir veya yerine elde etmek imkansız CSS ile tek başına, özellikle nasıl tutarsız bir şekilde ele eski aygıtları CSS kuralları yorumlama. Bu, özellikle CSS 3 özniteliklerini geçerlidir.
    - **Olumsuz**: değişen sunucu tarafı mantığı ve farklı aygıtlar için iş akışları için hiçbir destek sağlar. Örneğin, bir Basitleştirilmiş alışveriş sepeti checkout iş akışı mobil kullanıcılar için tek başına CSS yoluyla uygulayamaz.
    - **Olumsuz**: verimsiz bant genişliğini kullan. Sunucunuz rağmen hedef aygıt yalnızca bu bilgileri kümesini kullanır biçimlendirme ve tüm olası cihazlar için uygulama stilleri aktarmak gerekebilir.
3. ***Sunucusundaki sorunu* –** sunucunuz hangi cihaz – erişiyor biliyorsa veya ekran boyutu ve giriş yöntemi gibi bu aygıtın en az özelliklerini ve bir mobil cihaz – olup farklı mantık çalıştırılabilir ve Çıktı farklı HTML biçimlendirmesi. 

    - **Avantajı:** maksimum esneklik. Ne kadar kaydettiğinde, sunucu tarafı mantığınızı farklılık veya, biçimlendirme istenen, cihaza özgü düzen için en iyi duruma getirme sınırı yoktur.
    - **Avantajı:** verimli bant genişliğini kullan. Yalnızca biçimlendirme ve kullanmak için hedef aygıt geçiyor stil bilgileri aktarmak gerekir.
    - **Olumsuz:** bazen çaba veya kod yinelenmesinin zorlar (örn., benzer ancak biraz daha farklı kopyalarını Web formları sayfaları veya MVC görünümler oluşturmanıza hale getirme). Burada bir temel katmanı veya hizmeti, ancak bazı bölümleri UI kodu veya işaretleme yine de, ortak mantığı faktörü mümkün çoğaltılabilir ve paralel olarak tutulan gerekebilir.
    - **Olumsuz:** Aygıt algılama önemsiz değil. Bir liste veya veritabanı bilinen cihaz türleri ve (bunlar her zaman mükemmel güncel olmayabilir) özelliklerine gerektirir ve doğru şekilde her gelen istek eşleşecek şekilde garantisi yoktur. Bu belgede daha sonra bazı seçenekleri ve bunların Tuzaklar açıklanmaktadır.

En iyi sonuçları almak için çoğu Geliştirici seçenekleri (2) ve (3) birleştirmek ihtiyaç duydukları bulun. Verileri, iş akışı veya işaretleme temel farklılıkları en verimli şekilde sunucu tarafı kodda uygulanır ancak stil küçük farklılıklar CSS veya hatta JavaScript kullanılarak istemci üzerinde en iyi şekilde yerleştirilir.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Bu makale üzerinde sunucu tarafı teknikleri odaklanır

ASP.NET Web Forms ve MVC hem birincil sunucu tarafı teknolojilerin olduğundan, bu teknik incelemede farklı bir biçimlendirme ve mobil tarayıcılar için mantığı üretmenize olanak sağlayan sunucu tarafı teknikleri hakkında odaklanır. Elbette, bu da tüm istemci-tarafı teknik (örn., CSS 3 medya sorgular, aşamalı geliştirme JavaScript) birleştirebilirsiniz, ancak daha sağlasa da, web tasarımı ASP.NET geliştirme daha olmasıdır.

## <a name="browser-and-device-detection"></a>Tarayıcı ve cihaz algılama

Anahtar için mobil cihazları desteklemek için tüm sunucu tarafı teknikleri, ziyaretçi kullanarak hangi cihaz öğrenmek için önkoşuldur. Aslında, aygıt üreticisi ve modeli sayısı olan bilmek bilerek değerinden daha iyi *özelliklerini* cihaz. Özellikler içerebilir:

- Bir mobil cihaz mi?
- Giriş Yöntemi (fare/klavye, dokunma, tuş, oyun çubuğu,...)
- Ekran boyutu (fiziksel olarak ve piksel cinsinden)
- Desteklenen medya ve veri biçimleri
- VS.

Model sayıdan özelliklere göre çünkü kararlar daha iyidir sonra daha iyi gelecekteki aygıtları işlemek donanımlı.

### <a name="using-aspnets-built-in-browser-detection-support"></a>ASP kullanma. NET yerleşik tarayıcı algılama desteği

ASP.NET Web Forms ve MVC geliştiricileri hemen Bul ziyaret tarayıcının önemli özelliklerden özelliklerini inceleyerek *Request.Browser* nesnesi. Örneğin, bkz:

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...ve diğer birçok

Arka planda, ASP.NET platformu gelen eşleşen *User-Agent* tarayıcı tanım XML dosyaları kümesini normal ifadelerde karşı (UA) HTTP üstbilgisi. Varsayılan olarak platform birçok yaygın mobil cihazlar için tanımları içerir ve başkaları tarafından tanımak için istediğiniz özel tarayıcı tanım dosyalarını ekleyebilirsiniz. Daha fazla bilgi için MSDN sayfasını görmek [ASP.NET Web sunucusu denetimleri ve tarayıcı yetenekleri](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>51Degrees.mobi Foundation aracılığıyla WURFL aygıt veritabanını kullanma

While ASP. NET yerleşik tarayıcı algılama desteği birçok uygulama için yeterli olacaktır iki ana durumlar vardır yeterli olmayabilir:

- ***Son aygıtlar tanımak istediğiniz***(oluşturmadan el ile tarayıcı tanım dosyalarını kendileri için). .NET 4'ın tarayıcı tanım dosyalarını Windows Phone 7, Android telefonlar, Opera Mobile tarayıcılar veya Apple iPad tanımak için son olmadığına dikkat edin.
- ***Cihaz özellikleri hakkında daha ayrıntılı bilgi gerekir***. Bir aygıtın giriş yöntemi (örn., dokunma vs tuş takımında) hakkında bilmeniz gerekebilir veya tarayıcı hangi ses biçimleri destekler. Bu bilgiler standart tarayıcı tanım dosyalarında kullanılamaz.

[ *Kablosuz Evrensel Kaynak dosyası* (WURFL) proje](http://wurfl.sourceforge.net/) çok daha güncel ve ayrıntılı mobil cihazlar hakkında bilgi kullanımda bugün korur.

ASP .NET geliştiricileri için harika haber olur. NET'in tarayıcı algılama özelliğini genişletilebilir, olduğundan, bu sorunları gidermek için geliştirmek mümkündür. Örneğin, açık kaynak ekleyebilirsiniz [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) projenize kitaplığı. Bir ASP.NET IHttpModule veya tarayıcı yetenekleri doğrudan WURFL veri okuyan ve ASP içine kancalarını sağlayıcı (Web Forms ve MVC uygulamaları kullanılabilir) değil. NET'in yerleşik tarayıcı algılama mekanizması. Modül yükledikten sonra *Request.Browser* aniden çok daha kesin ve ayrıntılı bilgiler içerir: doğru pek çok daha önce bahsedilen cihazı tanıması ve yeteneklerini (dahil listesi ek özellikler giriş yöntemi gibi). Daha fazla ayrıntı için projenin belgelerine bakın.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Web Forms uygulamaları mobile özgü sayfaları nasıl sunabilir

Varsayılan olarak, işte nasıl yepyeni bir Web Forms uygulaması yaygın mobil cihazlarda görüntüler:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Açıkçası, ne düzeni çok mobil dostu görünüyor – bu sayfa için küçük bir dikey odaklı ekran büyük, yatay odaklı bir izleyici için tasarlanmıştır. Bu nedenle, bu konuda ne?

Bu makalede daha önce açıklandığı gibi mobil cihazlar için sayfalarınızı uyarlamak için birçok yolu vardır. Bazı teknikleri sunucu tabanlı, diğerleri istemcide çalıştırın.

### <a name="creating-a-mobile-specific-master-page"></a>Mobile özgü bir ana sayfa oluşturma

Gereksinimlerinize bağlı olarak, tüm ziyaretçiler için aynı Web formlarını kullanın, ancak iki ana sayfalar ayırmak mümkün olabilir: biri Masaüstü ziyaretçiler için mobil ziyaretçiler için başka bir. Bu, CSS stil sayfası veya mobil aygıtlar, herhangi bir sayfayı mantık çoğaltmak için zorlamadan uyacak şekilde, en üst düzey HTML biçimlendirmesini değiştirme esnekliği sağlar.

Bunu yapmak kolaydır. Örneğin, bir Web formu PreInit işleyicisi aşağıdaki gibi ekleyebilirsiniz:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Şimdi, uygulamanızın en üst düzey klasörde Mobile.Master olarak adlandırılan bir ana sayfa oluşturun ve mobil cihaz algılandığında kullanılır. Mobil ana sayfanızın mobile özgü CSS stil gerekirse başvurabilirsiniz. Masaüstü ziyaretçileri hala mobil olanı değil, varsayılan ana sayfayı görürsünüz.

### <a name="creating-independent-mobile-specific-web-forms"></a>Bağımsız mobile özgü Web formları oluşturma

Maksimum esneklik için yalnızca farklı cihaz türleri için ayrı ana sayfalar sahip daha çok daha fazla gidebilirsiniz. İki uygulayabilirsiniz *Web formları sayfaları kümesi tamamen ayrı* – bir masaüstü tarayıcıları için başka bir set kaydettiğinde için. Çok farklı bilgi ya da iş akışları mobil ziyaretçileri sunmak istiyorsanız, bu en iyi şekilde çalışır. Bu bölümün geri kalanında bu yaklaşımı ayrıntılı açıklar.

Masaüstü tarayıcıları için tasarlanmış bir Web Forms uygulamasına zaten sahip olduğu varsayılarak, devam etmek için kolay projenizi içinde "Mobile" adında bir alt klasör oluşturun ve mobil sayfalarınızı var. oluşturmak için yoludur. Bütün bir alt siteyi, kendi ana sayfalar, stil sayfaları ve sayfalar, hepsi aynı başka bir Web Forms uygulama için kullanacağınız teknikleri kullanarak oluşturabilirsiniz. Bir mobil eşdeğeri üretmek mutlaka gerekmez *her* sayfasında Masaüstü sitenizdeki; hangi işlevselliğinin alt kümesini mobil ziyaretçilere mantıklı seçebilirsiniz.

Mobil sayfalarınızı ortak statik kaynakları paylaşabilirsiniz (görüntüleri, JavaScript ya da CSS dosyaları) isterseniz, normal sayfaları ile. "Mobil" klasörünüze olacak beri *değil* işaretlenir ayrı bir uygulama (Web formları sayfaları yalnızca basit bir alt kümesidir) IIS içinde barındırıldığında, ayrıca aynı yapılandırma, oturum verilerini ve diğer altyapı paylaşır, Masaüstü sayfaları.

> [!NOTE]
> Bu yaklaşım genellikle bazı çoğaltma kod gerektirdiğinden (mobil sayfalar bazı benzerlikler Masaüstü sayfalarıyla paylaşmak büyük olasılıkla), tüm ortak iş mantığı ya da veri erişimi koda paylaşılan temel katman veya hizmet çarpanını önemlidir. Aksi takdirde, oluşturma ve uygulamanızı koruma çift çaba gerekir.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Mobil sayfalarınızı mobil ziyaretçileri yeniden yönlendirme

Genellikle yalnızca mobil sayfalar mobil ziyaretçileri yönlendirmek uygun olduğunu *ilk* istek gözatma kendi oturumunda (ve kendi oturumunda her istek üzerinde değil), çünkü:

- – Yalnızca ana sayfanızda "Masaüstü sürüme" giden bir bağlantı put istediklerinde Masaüstü sayfalarınızı erişmek mobil ziyaretçileri kolayca izin verebilirsiniz. Artık kendi oturumunda ilk istek olduğu için ziyaretçi geri bir mobil sayfasına, yeniden yönlendirilen olmaz.
- (Örn., sitenizin hem Masaüstü hem de mobil bölümleri IFRAME veya belirli Ajax işleyicileri görüntülemek ortak bir Web formu varsa), sitenizin Masaüstü ve mobil bölümleri arasında paylaşılan tüm dinamik kaynaklara yönelik isteklerin müdahale riski ortadan kaldırır

Bunu yapmak için yeniden yönlendirme mantığınızı yerleştirebilirsiniz bir **oturum\_Başlat** yöntemi. Örneğin, Global.asax.cs dosyanıza aşağıdaki yöntemi ekleyin:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Mobil sayfalarınızı cevaben form kimlik doğrulamasını yapılandırma

Form kimlik doğrulaması burada bunu ziyaretçileri sırasında ve kimlik doğrulama işlemi sonrasında yönlendirebilirsiniz hakkında bazı varsayımlarda kullandığına dikkat edin:

- Bir kullanıcı kimlik doğrulamasından geçmesi gerektiğinde, form kimlik doğrulaması bunları Masaüstü ve mobil kullanıcı olmanıza bakılmaksızın, masaüstü oturum açma sayfasına yönlendirir (yalnızca bir kavramı olduğundan *bir* oturum açma URL'si). Mobil oturum açma sayfanız farklı stil istediğiniz varsayıldığında, böylece mobil kullanıcılar ayrı mobil oturum açma sayfasına yönlendirir Masaüstü Oturum açma sayfanız geliştirmek gerekir. Örneğin, aşağıdaki kodu ekleyin, **Masaüstü** oturum açma sayfası arka plan kod: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Bir kullanıcı başarıyla oturum sonra form kimlik doğrulaması varsayılan olarak bunları Masaüstü giriş sayfanız yönlendirir (yalnızca bir kavramı olduğundan *bir* varsayılan sayfası). Böylece bir başarılı oturum açma işleminden sonra mobil giriş sayfasına yeniden mobil oturum açma sayfanız geliştirmek gerekir. Örneğin, aşağıdaki kodu ekleyin, **mobil** oturum açma sayfası arka plan kod: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Bu kod sayfanızı LoginUser, olduğu gibi varsayılan proje şablonu olarak adlandırılan bir oturum açma sunucusu denetimi sahip varsayar.

### <a name="working-with-output-caching"></a>Çıktı önbelleği ile çalışma

Çıktı önbelleği kullanıyorsanız, bir masaüstü kullanıcı (çıktısını önbelleğe alınmasına neden) belirli bir URL'yi ziyaret mümkündür varsayılan olarak, önbelleğe alınan Masaüstü çıktıyı alır bir mobil kullanıcı tarafından izlenen kaybolacağını unutmayın. Bu uyarı, yalnızca ana sayfanızın cihaz türüne göre değişen, veya tamamen ayrı Web Forms cihaz türüne göre uygulama olup olmadığını geçerlidir.

Sorunu önlemek için önbellek girişi ziyaretçi bir mobil cihaz kullanılmasına göre farklılık ASP.NET söyleyebilirsiniz. VaryByCustom parametresi sayfanızın OutputCache bildirimine aşağıdaki şekilde ekleyin:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Ardından, tanımlamak *isMobileDevice* özel önbellek olarak aşağıdaki yöntemi ekleyerek parametresi Global.asax.cs dosyanızı geçersiz kıl:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Bu, sayfa mobil ziyaretçileri çıktı önbelleğine Masaüstü ziyaretçisi tarafından daha önce yerleştirme almadığınız güvence altına alır.

### <a name="a-working-example"></a>Çalışan bir örnek

Bu eylem yöntemleri görmek için indirme [Bu incelemeyi 's kod örnekleri](https://docs.microsoft.com/aspnet/mobile/overview). Web Forms örnek uygulaması, mobil kullanıcılar otomatik olarak mobile özgü sayfalar mobil adlı bir alt kümesi için yönlendirir. Biçimlendirme ve stil bu sayfaların daha iyi duruma getirilmiştir mobil tarayıcılar için aşağıdaki ekran görüntüleri görebileceğiniz gibi:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Biçimlendirme ve CSS mobil tarayıcılar için en iyi duruma getirme hakkında daha fazla ipucu için "Stil mobil sayfalar için mobil tarayıcılar" Bu belgenin sonraki bölümlerinde bölümüne bakın.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>ASP.NET MVC uygulamaları mobile özgü sayfaları nasıl sunabilir

Model-View-Controller desen uygulama mantığını (denetleyicileri) sunu mantığında (görünümler) gelen ayrıştırır olduğundan, sunucu tarafı kodu Mobil Destek işleme için aşağıdaki yaklaşımlardan birini seçebilirsiniz:

1. ***Aynı denetleyicileri ve görünümler hem Masaüstü hem de mobil tarayıcılar için kullanır, ancak bağlı olarak cihaz türü * farklı Razor düzenleri görünümlerle işlenemiyor.** Aynı veri tüm aygıtlarda görüntülediğinizden ancak farklı CSS stil sağlayın veya kaydettiğinde için birkaç en üst düzey HTML öğeleri değiştirmek yalnızca istiyorsanız bu seçeneği en iyi şekilde çalışır.
2. ***Hem Masaüstü hem de mobil tarayıcılar için aynı denetleyicileri kullanır, ancak cihaz türüne bağlı olarak farklı görünümleri işleme***. Kabaca aynı verileri görüntüleme ve son kullanıcılar için aynı iş akışları sağlayan bu seçenek en iyi şekilde çalışır, ancak kullanılan aygıtı uyacak şekilde çok farklı HTML biçimlendirmesi oluşturmak istiyorsanız.
3. ***Bağımsız denetleyicileri ve görünümler her biri için uygulama, masaüstü ve mobil tarayıcılar için ayrı alanları oluşturmak *.** Bu seçenek, iseniz çok farklı ekranlar görüntüleme, farklı bilgiler içeren ve kullanıcının kendi cihaz türü için en iyi duruma getirilmiş farklı iş akışları aracılığıyla önde gelen en iyi çalışır. Bazı yineleme kod olduğu anlamına gelebilir, ancak, temel alınan bir katman veya hizmet ortak mantığı Finansman küçültebilirsiniz.

Almak istiyorsanız **ilk** seçeneği ve yalnızca Razor düzeni farklılık aygıt türü çok kolaydır. Yalnızca değiştirmek, \_ViewStart.cshtml dosya şu şekilde:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Adlı bir mobile özgü düzeni oluşturmak artık \_LayoutMobile.cshtml sayfa yapısına sahip ve CSS kuralları mobil aygıtlar için en iyi duruma getirilmiş.

Almak istiyorsanız **ikinci** ziyaretçi cihaz türüne göre farklı görünümleri oluşturma seçeneği için bkz: [Scott Hanselman'ın blog gönderisi](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Bu yazı kalan odaklanır **üçüncü** ayrı denetleyicileri oluşturma seçeneği – *ve* mobil aygıtları – hangi işlevselliğinin alt kümesini mobil ziyaretçiler için tam olarak sunulan denetimi için görünümleri.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Bir mobil alanı, ASP.NET MVC uygulamanızda ayarlama

Normal bir şekilde mevcut bir ASP.NET MVC uygulaması için "mobil" adlı bir alana ekleyebilirsiniz: Çözüm Gezgini'nde proje adına sağ tıklayın ve ardından Ekle à alanı seçin. Bir ASP.NET MVC uygulaması içindeki herhangi bir alan için yaptığınız gibi denetleyicilerinin ve görünümlerin daha sonra ekleyebilirsiniz. Örneğin, mobil alanınızı mobil ziyaretçiler için bir giriş sayfası davranacak şekilde HomeController adlı yeni bir denetleyici ekleyin.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>URL /Mobile sağlama ulaştığında mobil giriş sayfası

HomeController dizin eylemini mobil alanınızı içinde ulaşmak için URL /Mobile istiyorsanız, yönlendirme yapılandırmanızı iki küçük değişiklikler yapmanız gerekecektir. İlk olarak, böylece HomeController mobil bölgenizdeki varsayılan denetleyicisi MobileAreaRegistration sınıfınıza aşağıdaki kodda gösterildiği gibi güncelleştirin:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

"Home" şimdi olduğundan bu mobil giriş sayfası artık konumlandırılacağı/mobil/giriş, yerine /Mobile gelir örtük olarak mobil sayfalar için varsayılan denetleyici adı.

Ardından, uygulamanıza (yani, mobil bir, varolan bir Masaüstü ek olarak) ikinci HomeController ekleyerek, normal Masaüstü sayfanız kıran olduğunu unutmayın. Hata ile başarısız olur "*'Giriş' adlı denetleyicisi eşleşen birden çok tür bulunamadı*". Bu sorunu çözmek için belirsizlik olduğunda, Masaüstü HomeController öncelik zamanınızı belirtmek için üst düzey yönlendirme yapılandırmasında (Global.asax.cs) güncelleştirmesi:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Hata koy ve URL http:// gidecek artık<em>yoursite</em>/ Masaüstü giriş sayfası ve http:// ulaşabileceği<em>yoursite</em>/mobile/ mobil giriş sayfası ulaşmak.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Mobil alanınızı mobil ziyaretçileri yeniden yönlendirme

Vardır birçok farklı genişletilebilirlik noktaları ASP.NET MVC'de nedenle yeniden yönlendirme mantığı eklemesine birçok olası yolu yok. Masaüstünüzdeki seçeneklerden biri, aşağıdaki koşullar doğruysa, bir yeniden yönlendirme gerçekleştiren bir filtre özniteliği [RedirectMobileDevicesToMobileArea] oluşturmaktır:

1. Kullanıcının oturumu ilk istek olduğu (yani, Session.IsNewSession true eşittir)
2. Mobil bir tarayıcıdan istek gelir (yani, Request.Browser.IsMobileDevice true eşittir)
3. Kullanıcı zaten bir kaynak mobil alanında isteyen değil (yani, *yolu* URL parçası /Mobile ile başlayan değil)

Bu teknik incelemede ile dahil indirilebilir örnek bir uygulama bu mantığı içerir. (Aksi halde, bir masaüstü ziyaretçi ilk erişimleri belirli URL Masaüstü çıkış önbelleğe alınan ve ardından hizmet verilen, çıktı önbelleği kullanıyorsanız, düzgün çalışabilmesi anlamına gelir AuthorizeAttribute öğesinden türetilmiş bir yetkilendirme filtre olarak uygulanan sonraki mobil ziyaretçileri).

Bir filtre olarak ya da belirli denetleyicileri ve eylemleri uygulamak için örneğin seçebilirsiniz,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… veya, tüm denetleyicileri ve eylemleri bir MVC 3 olarak uygulayabilirsiniz *genel filtre* Global.asax.cs dosyanızda:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

İndirilebilir örnekteki ayrıca mobil alanda belirli konumlara yeniden yönlendirme alt sınıfların bu özniteliğin nasıl oluşturabileceğinizi gösterir. Bu, örneğin, aşağıdakileri yapabilirsiniz anlamına gelir:

- Varsayılan olarak mobil giriş sayfası mobil ziyaretçileri gönderdiği genel filtre olarak gösterilen yukarıdaki kaydedin.
- Ayrıca mobil sürümü istenen ne olursa olsun ürün sayfası, mobil ziyaretçileri geçen bir "görünümü ürün" eylemi için özel [RedirectMobileDevicesToMobileProductPage] filtre uygulayın.
- Filtre özel diğer alt sınıflarının belirli eylemler, mobil ziyaretçileri eşdeğer mobil sayfasına yeniden yönlendirmek için de geçerlidir

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Mobil sayfalarınızı cevaben form kimlik doğrulamasını yapılandırma

Form kimlik doğrulaması kullanıyorsanız, bir kullanıcı oturum açmak gerektiğinde otomatik olarak kullanıcı olan varsayılan olarak tek bir belirli "Oturum Aç" URL'sini için yeniden olduğunu unutmamalısınız **/Account/oturum açma**. Başka bir deyişle, mobil kullanıcılar Masaüstü "Oturum Aç" eyleminizi yönlendirilmesi.

Bu sorunu önlemek için böylece mobil "Oturum Aç" eylemi için mobil kullanıcılar yeniden yönlendiren Masaüstü "Oturum Aç" eylemi için mantığı ekleyin. ASP.NET MVC uygulama şablonu varsayılan kullanıyorsanız, AccountController'ın oturum açma eylem şu şekilde güncelleştirin:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… ve mobil bölgenizde AccountController adlı bir denetleyicisinde "Oturum Aç" uygun bir mobile özgü eylemi uygulayın.

### <a name="working-with-output-caching"></a>Çıktı önbelleği ile çalışma

[OutputCache] filtresi kullanıyorsanız, önbellek girişinin cihaz türüne göre farklılık zorlamanız gerekir. Örneğin, yazma:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Ardından, Global.asax.cs dosyanıza aşağıdaki yöntemi ekleyin:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Bu, sayfa mobil ziyaretçileri çıktı önbelleğine Masaüstü ziyaretçisi tarafından daha önce yerleştirme almadığınız güvence altına alır.

### <a name="a-working-example"></a>Çalışan bir örnek

Bu eylem yöntemleri görmek için indirme [Bu incelemeyi 's kod örnekleri ilişkili](https://docs.microsoft.com/aspnet/mobile/overview). Örnek, yukarıda açıklanan yöntemleri kullanarak mobil cihazları desteklemek için Gelişmiş bir ASP.NET MVC 3 (Sürüm Adayı) uygulama içerir.

## <a name="further-guidance-and-suggestions"></a>Daha fazla yönerge ve öneriler

Aşağıdaki tartışma hem Web formları ve bu belgede ele alınan teknikleri kullanarak MVC geliştiriciler için geçerlidir.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Yeniden yönlendirme mantığınızı 51Degrees.mobi Foundation kullanarak geliştirme

Bu belgede gösterilen yeniden yönlendirme mantığı uygulamanız için mükemmel yeterli olabilir ancak oturumları devre dışı bırakmanız gerekirse çalışmaz veya belirtilen bir istek olup olmadığını bilemezsiniz olduğundan (Bu oturumlar olamaz) tanımlama bilgilerini reddedebilirsiniz bir mobil tarayıcılar ile bulunuyor Bu ziyaretçi ilk makineden.

Zaten açık kaynak 51Degrees.mobi Foundation ASP doğruluğunu nasıl artırabilir öğrendiniz. NET'in tarayıcı algılama. Ayrıca, Web.config dosyasında yapılandırılmış belirli konumlara mobil ziyaretçileri yönlendirmek için yerleşik bir olanağı vardır. Olmadan bağlı olarak ASP.NET oturum çalışabilmek için (ve dolayısıyla tanımlama bilgileri) karmaları ziyaretçilerin HTTP üstbilgileri ve IP adreslerini geçici günlüğünü depolayarak, böylece bunu bilen her istek verilen vistor ilk makineden olup olmadığını.

Web.config dosyasını fiftyOne bölümüne eklenmesi öğesi algılanan mobil aygıttan ilk isteği sayfasına yönlendirir ~ / Mobile/Default.aspx. Tüm istekleri mobil klasörü altında sayfalara *değil* yönlendirilen, aygıt türü ne olursa olsun. Mobil aygıt için bir süre boyunca etkin olmayan 20 dakika veya daha fazla cihaz unutulursa ve yenilerini yeniden yönlendirme amacıyla sonraki isteklerde değerlendirilir.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Daha fazla ayrıntı için bkz: [51degrees.mobi Foundation belgelerine](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> *İçin* yeniden yönlendirme yapılandırmanızı düz URL'ler, Yönlendirme parametreleri açısından değil veya MVC filtreleri koyarak bakımından tanımlamak ASP.NET MVC uygulamaları, ancak kullanım 51Degrees.mobi Foundation'ın yeniden yönlendirme özelliğini gerekir Eylemler. Bunun nedeni, (yazma zamanında) filtreleri veya yönlendirme 51Degrees.mobi Foundation tanımıyor.


### <a name="disabling-transcoders-and-proxy-servers"></a>Kod dönüştürücüleri ve Proxy sunucuları devre dışı bırakma

Mobil ağ operatörleri iki geniş hedef Mobil Internet kendi yaklaşımla sahiptir:

1. Mümkün olduğunca çok ilgili içerik olarak sağlayın
2. Sınırlı radyo ağ bant genişliği paylaşabilirsiniz müşteriler sayısı en üst düzeye çıkarmak

Çoğu web sayfaları büyük Masaüstü ölçekli ekranları ve hızlı sabit hat geniş bant bağlantıları için tasarlandığından, birçok işleçleri kullanmak *kod dönüştürücüleri* veya *proxy sunucuları* web içeriği dinamik olarak değiştirmek. HTML biçimlendirmesi ya da daha küçük ekranlar ("karmaşık düzenleri işlemek için işlemci gücü eksik özellikle özelliği telefonlarını") uyacak şekilde CSS değiştirebilir ve sayfa teslim hızını artırmak için (kendi kalite önemli ölçüde azaltır) görüntülerinizi sıkıştırmak.

Ancak, sitenizin mobile için iyileştirilmiş bir sürümünü oluşturmak için çaba ayırdıktan, büyük olasılıkla ağ operatörü ile herhangi bir ek engel olmasını istemediğiniz. Aşağıdaki satırı sayfasına ekleyebilirsiniz\_herhangi bir ASP.NET Web formu yük olay:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Ya da bir ASP.NET MVC denetleyicisi için böylece bu denetleyicideki tüm eylemler için geçerli olur aşağıdaki yöntemi geçersiz kılma ekleyebilirsiniz:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Elde edilen HTTP iletisi W3C uyumlu kod dönüştürücüleri ve içerik değiştirmeyin proxy'leri bildirir. Elbette, mobil Ağ İşletmenleri bu iletiyi uyar garantisi yoktur.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Mobil sayfalar mobil tarayıcılar için stil oluşturma

Harika ayrıntılı bir şekilde tanımlamak için bu belgenin kapsamı ne tür bir HTML biçimlendirmesi iş doğru dışındadır veya kullanılabilirlik belirli cihazlarda hangi web tasarım teknikleri en üst düzeye çıkarın. Bu yukarı yeteri kadar basit bir düzen bulmak için mobil ölçekli bir ekran için güvenilir kullanmadan HTML veya püf noktaları konumlandırma CSS optimize etti. Söz, değerinde bir önemli ancak tekniktir *görünüm penceresinin meta etiketi*.

Masaüstü monitör için amacı bir çaba görüntü web sayfalarında belirli modern mobil tarayıcılar "Görünüm" olarak da adlandırılan sanal bir tuval üzerinde sayfayı oluşturmak (örneğin, sanal görünüm penceresinin iPhone üzerinde 980 pikselden ve 850 pikselden varsayılan olarak Opera Mobile adıdır) ve ardından Ekran fiziksel piksel uyacak şekilde sonucu ölçeklendirmeyi azaltın. Kullanıcı daha sonra yakınlaştırmak ve geçici bir çözüm, görünüm penceresinin kaydırmak. Bunun avantajı vardır hedeflenen düzende sayfayı görüntülemek tarayıcı sağlar, ancak ayrıca sahip kullanıcı için kullanışsız, yakınlaştırma ve kaydırma, zorlar dezavantajı. Mobile için tasarladığınız, yakınlaştırma veya yatay kaydırma gerekli olmasını sağlamak için dar bir ekran tasarımı için daha iyi olur.

Görünüm penceresinin nasıl geniş olmalıdır mobil tarayıcı bildirmek için bir standart olmayan yoludur *görünüm penceresinin* meta etiketi. Örneğin, sayfanızın HEAD bölümüne aşağıdaki eklerseniz,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… smartphone tarayıcılar destekleyen bir 480 piksel geniş sanal tuvali sayfasında yerleştirme. Bu, HTML öğeleri yüzde olarak gösteren genişliklerini tanımlarsanız, yüzdeleri bu 480 piksel genişlik, varsayılan görünüm penceresinin genişliğini göre yorumlanacak anlamına gelir. Sonuç olarak, kullanıcının yakınlaştırmasına ve kaydırmasına yatay – önemli ölçüde mobil gözatma deneyimini geliştirmek için daha az olasıdır.

Cihazın fiziksel piksel eşleşecek şekilde görünüm penceresinin genişliği isterseniz, aşağıdakileri belirtebilirsiniz:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Bu doğru çalışması, açıkça bu genişliğini aşan öğeleri zorlamanız gerekir değil (örn., kullanarak bir *genişliği* özniteliği ya da CSS özelliği), daha büyük bir görünüm penceresinin bakılmaksızın kullanmak için tarayıcı aksi zorlanır. Ayrıca bkz: [standart olmayan görünüm penceresinin etiketi hakkında daha fazla ayrıntı](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Çoğu modern akıllı telefonlar Destek *çift yönlendirmesini*: dikey veya yatay modunda tutulabilir. Bu nedenle, ekran genişliği hakkında varsayımlar piksel cinsinden yapmamak önemlidir. Ekran genişliği sabit sayfanızda çalışırken kullanıcı cihazını yeniden yönlendirebilirsiniz çünkü bile varsaymayın.

Eski Windows Mobile ve Blackberry cihazlarını ayrıca aşağıdaki meta etiketleri içerik bildiren sayfa üstbilgisinde mobile için optimize edilmiştir ve bu nedenle değil dönüştürülmesi gereken kabul edebilir.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Ek Kaynaklar

Mobil aygıt benzetmeleri ve mobil ASP.NET web uygulamanızı test etmek için kullanabileceğiniz benzeticileri listesi için sayfaya bakın [test etmek için popüler mobil cihazlar benzetimini](../mobile/device-simulators.md).

## <a name="credits"></a>KREDİLERİ

- Birincil Yazar: Steven Sanderson
- Gözden geçirenler / ek yazıcılarının içerik: Ahmet Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
