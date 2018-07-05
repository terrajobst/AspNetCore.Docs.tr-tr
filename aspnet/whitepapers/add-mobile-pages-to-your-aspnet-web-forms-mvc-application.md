---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Nasıl yapılır: ASP.NET Web formlarınızı mobil sayfalar ekleme / MVC uygulaması | Microsoft Docs'
author: rick-anderson
description: Bu nasıl yapılır sayfaları, ASP.NET Web Forms mobil cihazlar için en iyi duruma getirilmiş sunmak için çeşitli şekillerde açıklayan / MVC uygulaması ve mimari önerir ve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: ''
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 075329087cb5e07d85bba0c546538e7cc55ac463
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366763"
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Nasıl yapılır: ASP.NET Web formlarınızı mobil sayfalar ekleme / MVC uygulaması
====================
> **İçin geçerlidir**
> 
> - ASP.NET Web Forms sürüm 4.0
> - ASP.NET MVC sürüm 3.0
> 
> **Özet**
> 
> Bu nasıl yapılır sayfaları, ASP.NET Web Forms mobil cihazlar için en iyi duruma getirilmiş sunmak için çeşitli yolları açıklar / MVC uygulaması, çok çeşitli cihazları hedefleyen yaparken dikkate alınması gereken sorunları tasarım ve mimari önerir. Bu belge de neden 3.5 için ASP.NET Mobil denetimleri ASP.NET 2.0 artık kullanım dışıdır ve modern bazı alternatifleri anlatılmaktadır açıklar.


## <a name="contents"></a>İçindekiler

- Genel Bakış
- Mimari seçenekleri
- Tarayıcı ve cihaz algılama
- ASP.NET Web formları uygulamalarını mobile özgü sayfaları nasıl sunabilirsiniz
- ASP.NET MVC uygulamaları mobile özgü sayfaları nasıl sunabilirsiniz
- Ek kaynaklar

ASP.NET Web Forms ve MVC için bu teknik incelemeyi 's teknikleri gösteren indirilebilir kod örnekleri için bkz. [Mobile Apps & siteleri ile ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Genel Bakış

Mobil cihazları – akıllı telefonlar, özellik telefonlar ve tabletler – araç Web erişimi için bir yol olarak popülerliği artmaya devam eder. Birçok web geliştiricileri ve web tabanlı işletmelerin bu ziyaretçiler için bu cihazları kullanarak harika bir gözatma deneyimi sağlamak giderek daha çok önemli olduğu anlamına gelir.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Desteklenen ASP.NET mobil tarayıcılar nasıl önceki sürümleri

ASP.NET sürüm 2.0 için dahil edilen 3.5 *ASP.NET Mobil denetimleri*: sunucu denetimleri mobil cihazlar için bir dizi *System.Web.Mobile.dll* derleme ve  *System.Web.UI.MobileControls* ad alanı. Derleme, ASP.NET 4'te yine de dahildir, ancak kullanım dışıdır. Geliştiricilerin bu belgede açıklananlar gibi daha modern yaklaşımlar geçirmek için önerilir.

Neden ASP.NET Mobil denetimleri artık kullanılmıyor olarak işaretlenmiş tasarımlarına'nın 2005 ve önceki etrafında ortak cep telefonları etrafında yönelik nedenidir. Denetimler çoğunlukla WML veya cHTML biçimlendirmesi (yerine normal HTML) oluşturmak için bu dönem WAP tarayıcılar için tasarlanmıştır. Ancak HTML artık mobil ve Masaüstü tarayıcılarda benzer için bulunabilen biçimlendirme dili haline geldiğinden WAP, WML ve cHTML artık çoğu proje için uygundur.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Bugün, mobil cihazları destekleyen sorunlarından

Mobil tarayıcılar artık neredeyse tüm dünyada HTML destekler olsa da, harika mobil gözatma deneyimler oluşturmak amacıyla, birçok güçlükle de hala karşılaşır:

- ***Ekran boyutu*** - mobil cihazları önemli ölçüde farklılık biçiminde ve bunların genellikle çok ekranlardır Masaüstü monitörler daha küçüktür. Bu nedenle, bunlar için tamamen farklı sayfa düzenleri tasarlamak gerekebilir.
- ***Giriş yöntemleri*** – tuş bazı cihazlara sahip, dokunma diğerlerinde styluses vardır. Birden çok gezinti mekanizmalar ve veri giriş yöntemlerini dikkate almanız gerekebilir.
- ***Standartlara uyum*** – birçok mobil tarayıcılar, HTML, CSS ve JavaScript geliştirmeye desteklemez.
- ***Bant genişliği*** : hücresel veri ağı performans çok farklılık gösterir ve bazı son kullanıcılar tarafından megabayt ücret tariffs yer alır.

BT'ye çözümü yoktur; uygulamanızı bulun ve ona erişen cihaz göre farklı davranır gerekecektir. Mobil destek düzeyini istediğiniz bağlı olarak, bu "tarayıcı wars" Masaüstü her zamankinden daha büyük bir sınama web geliştiricileri için olabilir.

Geliştiriciler genellikle başlangıçta ilk kez mobil tarayıcı desteği yaklaşan gibi geliştiriciler genellikle kişisel olarak sahip olduğundan (örneğin, Windows Phone 7, iPhone ve Android), en son ve en ileri Teknolojili Smartphone belki de desteklemek yalnızca önemlidir düşünün. cihazlar. Ancak, ucuz telefonlar hala son derece popüler olduğunu ve sahiplerinin – özellikle cep telefonları kolay almak geniş bant bağlantısı bulunduğu ülkede Web'de gezinmek için bunları kullanın. İşletmeniz, cihazların büyük olasılıkla müşterilerine dikkate alarak desteklemek için hangi aralığı karar vermeniz gerekir. Çevrimiçi bir lüks sistem durumu spa broşürlerde oluşturuyorsanız, bilet kayıt sistemi için bir sinema oluşturuyorsanız, büyük olasılıkla daha güçlü bir özellik ile ziyaretçiler için hesap gerekir ancak yalnızca Gelişmiş akıllı telefonlar, hedeflemek için bir iş karar telefonlar.

## <a name="architectural-options"></a>Mimari seçenekleri

Biz ASP.NET Web Forms veya MVC belirli teknik ayrıntılara geçmeden önce web geliştiricileri genel mobil tarayıcılar desteklemek için üç ana olası seçeneklerin sahip dikkat edin:

1. ***Hiçbir şey –*** yeterlidir, standart, masaüstü tabanlı bir web uygulaması oluşturun ve mobil tarayıcı üzerinde çalışarak işlemek için kullanır. 

    - **Avantajı**: uygulamak ve sürdürmek – herhangi bir ek çalışma için ucuz seçenektir
    - **Olumsuz**: kötü son kullanıcı deneyimi sağlar: 

        - En son Akıllı telefonlar, HTML ekleyebiliyorsa bir masaüstü tarayıcısı olarak kalabilir, ancak kullanıcılar hala yatay kaydırma ve yakınlaştırma ve dikey olarak küçük ekranlarda, içeriği kullanmak için zorlanır. En iyi gölgeden uzak budur.
        - Eski cihazları ve özellik telefonlar, biçimlendirmesi tatmin edici bir şekilde oluşturmak başarısız olabilir.
        - Cihazlarda (olan ekranlar olabilir dizüstü bilgisayar ekranları büyük) bile son tablet, farklı etkileşim kuralları geçerlidir. Dokunmatik tabanlı giriş büyük düğmeleriyle en iyi şekilde çalışır ve yayılma daha uzaklıkta bağlar ve bir fare imlecini bir çıkış menüye getirin bir yolu yoktur.
2. ***İstemci üzerinde sorunu* –** CSS dikkatli kullanımı ile ve [kademeli geliştirmeyi](http://en.wikipedia.org/wiki/Progressive_enhancement) biçimlendirme, stillerini ve bunları çalışıyor ne olursa olsun tarayıcı için uyum betikleri oluşturabilirsiniz. Örneğin, [CSS 3 medya sorguları](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), tek sütunlu bir düzende cihazlarda seçilen eşikten daha dar olan ekranlar dönüştüren çok sütunlu düzeni oluşturabilir. 

    - **Avantajı**: işleme Kullanımdaki her ekranı ve giriş özelliklerine sahip oldukları göre bile bilinmeyen gelecekteki cihazlar için söz konusu cihaz için en iyi duruma getirir
    - **Avantajı**: kolayca sunucu tarafı mantık tüm cihaz türlerine – en az çoğaltma kod veya çaba paylaşmanıza olanak tanır
    - **Olumsuz**: mobil cihazlar, Masaüstü cihazları, gerçekten mobil sayfalarınızı Masaüstü sayfalarından tamamen farklı farklı bilgiler gösteren isteyebilirsiniz, böylece farklı. Böyle çeşitlemeleri verimsiz olabilir veya olanaksız yerine elde etmek CSS ile tek başına, özellikle nasıl ayırdığını dikkate eski cihazları CSS kurallarını yorumlama. Bu, özellikle CSS 3 özniteliklerini geçerlidir.
    - **Olumsuz**: değişen sunucu tarafı mantık ve iş akışları için farklı cihazlar için destek sağlar. Örneğin, bir Basitleştirilmiş alışveriş sepeti ödeme iş akışı mobil kullanıcılar için CSS yoluyla tek başına uygulayamaz.
    - **Olumsuz**: verimsiz bant genişliğini kullanma. Sunucunuz rağmen hedef cihaza yalnızca bu bilgileri kümesini kullanır biçimlendirme ve tüm olası cihazlar için uygulama stilleri aktarmaya sahip olabilir.
3. ***Sunucu üzerinde sorunu* –** sunucunuzun hangi cihaz – erişen biliyorsa veya ekran boyutunu ve giriş yöntemi gibi bu cihazı az özelliklerini ve bir mobil cihaz – olup farklı mantıksal çalıştırabilirsiniz ve Çıkış farklı HTML biçimlendirmesi. 

    - **Avantajı:** maksimum esneklik. Ne kadar kaydettiğinde, sunucu tarafı mantığı farklılık veya, biçimlendirme istenen, cihaza özel düzen için en iyi duruma getirme sınırı yoktur.
    - **Avantajı:** verimli bant genişliğini kullanma. Biçimlendirme ve hedef cihaza kullanmayı düşünüyor stil bilgileri iletmek yeterlidir.
    - **Olumsuz:** bazen zorlar yineleme çaba veya kod (örneğin, benzer ancak biraz farklı kopyalarını Web formları sayfaları veya MVC görünümleri oluşturmanıza yapmak için). Burada temel alınan bir katmanı veya hizmet, ancak bazı bölümleri UI kod veya biçimlendirme yine de sık kullanılan mantıksal faktörü olası çoğaltılabilir ve ardından paralel olarak tutulan gerekebilir.
    - **Olumsuz:** cihaz algılama Önemsiz değildir. Bir liste veya veritabanı bilinen cihaz türleri ve (her zaman tamamen güncel olmayabilir) kendi özellikleri gerektirir ve her gelen istek doğru şekilde eşleşecek şekilde garanti yoktur. Bu belgede daha sonra bazı seçenekleri ve bunların Tuzaklar açıklanmaktadır.

En iyi sonuçları almak için seçenekleri (2) ve (3) birleştirmek ihtiyaç duydukları Çoğu geliştirici bulun. Verileri, iş akışı veya biçimlendirme önemli farklılıkları en etkili bir şekilde sunucu tarafı kodda uygulanır ancak stil küçük farklılıklar CSS veya hatta JavaScript kullanarak istemci üzerinde en iyi şekilde yerleştirilir.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Bu yazıda sunucu tarafı teknikleri üzerinde odaklanır.

ASP.NET Web Forms ve MVC öncelikle sunucu tarafı hem teknolojiler olduğundan, bu teknik incelemeyi farklı biçimlendirme ve mobil tarayıcılar için mantığı üretmek olanak tanıyan bir sunucu tarafı teknikleri üzerinde odaklanır. Elbette, bu ayrıca tüm istemci tarafı teknik (örneğin, CSS 3 medya sorgular, aşamalı geliştirme JavaScript) ile birleştirebilirsiniz, ancak web tasarım birkaç ASP.NET geliştirme daha fazla olmasıdır.

## <a name="browser-and-device-detection"></a>Tarayıcı ve cihaz algılama

Anahtar mobil cihazları desteklemek için tüm sunucu tarafı teknikleri, ziyaretçi kullanarak hangi cihaz öğrenmek için önkoşuldur. Aslında, daha iyi olan aygıt üreticisi ve modeli sayısını bilmek bilmek daha *özellikleri* cihaz. Özelliklere şunlar olabilir:

- Bu, bir mobil cihaz mı?
- Giriş Yöntemi (fare/klavye, dokunma, tuş, oyun kumandası,...)
- Ekran boyutu (fiziksel olarak ve piksel cinsinden)
- Desteklenen medya ve veri biçimleri
- VS.

Model sayıdan özelliklere göre çünkü kararlar daha iyidir, ardından daha iyi gelecekteki cihazları işlemek donanımlı olacaktır.

### <a name="using-aspnets-built-in-browser-detection-support"></a>ASP kullanıyor. NET yerleşik tarayıcı algılama desteği

ASP.NET Web Forms ve MVC geliştiriciler hemen bulabilir ziyaret tarayıcı önemli özelliklerini özelliklerini inceleyerek *Request.Browser* nesne. Örneğin, bkz:

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...ve diğer birçok

Arka planda, ASP.NET platformunda gelen eşleşen *User-Agent* karşı normal ifadeler bir dizi tarayıcı tanımı XML dosyası (UA) HTTP üstbilgisi. Varsayılan platform birçok yaygın mobil cihazlar için tanımları içerir ve başkaları için tanımak istediğiniz özel tarayıcı tanım dosyalarını ekleyebilirsiniz. Daha fazla ayrıntı için bkz MSDN sayfasını [ASP.NET Web sunucusu denetimleri ve tarayıcı yetenekleri](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>51Degrees.mobi Foundation aracılığıyla WURFL cihaz veritabanını kullanma

While ASP. NET yerleşik tarayıcı algılama desteği birçok uygulama için yeterli olacaktır iki ana durumlar vardır yeterli olmayabilir:

- ***En son cihazlar tanımak istediğiniz***(olmadan tarayıcı tanımlama dosyaları için bunları el ile oluşturma). .NET 4'ün tarayıcı tanım dosyalarını Windows Phone 7, Android telefonlar, Opera Mobile tarayıcılar veya Apple iPad tanımak için son olmadığına dikkat edin.
- ***Cihaz özellikleri hakkında daha ayrıntılı bilgi ihtiyacınız***. Bir cihazın giriş yöntemi (örneğin, dokunma vs klavyede) hakkında bilmesi gerekebilir veya tarayıcı hangi ses biçimleri destekler. Bu bilgileri standart tarayıcı tanım dosyalarında kullanılamaz.

[ *Kablosuz Evrensel Kaynak dosyası* (WURFL) proje](http://wurfl.sourceforge.net/) çok hakkında daha fazla güncel ve ayrıntılı bilgi mobil cihazları bugün tutar.

ASP .NET geliştiricileri için harika bir haberimiz var olur. NET tarayıcı algılama özelliğini Genişletilebilir olduğundan, bu sorunları çözmek için geliştirmek mümkündür. Örneğin, açık kaynak ekleyebilirsiniz [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) kitaplığı projenize. Bu, bir ASP.NET IHttpModule ya da tarayıcı yetenekleri doğrudan WURFL verileri okur ve ASP kancaları sağlayıcısı (Web Forms hem MVC uygulamaları kullanılabilir), yöneliktir. NET yerleşik tarayıcı algılama mekanizması. Modülü yükledikten sonra *Request.Browser* aniden çok daha doğru ve ayrıntılı bilgiler içerir: Bu doğru pek çok daha önce bahsedilen cihazı tanıması ve yeteneklerini (dahil olmak üzere liste gibi ek özellikler giriş yöntemi). Daha fazla ayrıntı için projenin belgelerine bakın.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Web formları uygulamalarını mobile özgü sayfaları nasıl sunabilirsiniz

Varsayılan olarak, işte yepyeni bir Web Forms uygulaması yaygın mobil cihazlarda nasıl görüntüler:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Açıkçası, ne düzeni çok mobil kullanımı kolay görünüyor: Bu sayfa, dikey odaklı küçük bir ekran için büyük, yatay odaklı bir izleyici için tasarlanmıştır. Bu nedenle bu konuda yapabilecekleriniz?

Bu yazının önceki bölümlerinde açıklandığı gibi mobil cihazlar için sayfalarınızı uyarlamak için birçok yolu vardır. Sunucu tabanlı bazı teknikleri, diğer istemci üzerinde çalıştırın.

### <a name="creating-a-mobile-specific-master-page"></a>Mobile özgü bir ana sayfa oluşturma

Gereksinimlerinize bağlı olarak, tüm ziyaretçiler için aynı Web Forms kullanın, ancak iki ana sayfalar ayrı sahip olabilir: biri Masaüstü ziyaretçiler, başka bir mobil ziyaretçiler için. Bu, CSS stil sayfası veya herhangi bir sayfa mantık oluşturmaya zorlamadan mobil cihazlara uyacak şekilde, en üst düzey bir HTML biçimlendirmesi değiştirme esnekliği sağlar.

Bunu yapmak kolay bir işlemdir. Örneğin, bir Web formu için PreInit işleyicisi aşağıdaki gibi ekleyebilirsiniz:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Şimdi, uygulamanızın en üst düzey klasöre Mobile.Master adlı bir ana sayfa oluşturma ve bir mobil cihaz algılandığında kullanılacaktır. Gerekirse, mobil ana sayfanıza mobile özgü CSS stil sayfası başvurabilirsiniz. Masaüstü ziyaretçiler hala mobil olanı değil, varsayılan ana sayfayı görürsünüz.

### <a name="creating-independent-mobile-specific-web-forms"></a>Bağımsız mobile özgü Web formları oluşturma

Maksimum esneklik için yalnızca farklı cihaz türleri için ayrı bir ana sayfalar sahip daha çok daha gidebilirsiniz. İki uygulayabilirsiniz *Web formları sayfaları kümesi tamamen ayrı* – bir masaüstü tarayıcıları için başka bir set kaydettiğinde için. Çok farklı bilgiler veya iş akışları için mobil ziyaretçiler sunmak istiyorsanız bu en iyi şekilde çalışır. Bu bölümün geri kalanında bu yaklaşım ayrıntılı açıklar.

Masaüstü tarayıcıları için tasarlanmış bir Web Forms uygulaması zaten olduğunu varsayarsak, devam etmek için en kolay yolu projeniz "Mobile" adlı bir alt klasör oluşturun ve mobil sayfalar var. derleme sağlamaktır. Kendi ana sayfalar, stil sayfaları ve sayfalar ile bütün bir alt siteyi, aynı herhangi bir Web Forms uygulaması için kullanacağınız teknikleri kullanarak oluşturabilirsiniz. Mutlaka üretmek için bir mobil eşdeğer gerekmez *her* sayfasında Masaüstü sitenizdeki; mobil ziyaretçiler için hangi işlevselliğin alt mantıklı seçebilirsiniz.

Mobil sayfalarınızı ortak statik kaynakları paylaşabilirsiniz (görüntüler, JavaScript ve CSS gibi dosyaları) istiyorsanız normal sayfalarınızı ile. "Mobile" klasörünüzü olacak beri *değil* işaretlenir ayrı bir uygulama (Web formları sayfaları yalnızca basit bir alt kümesidir) IIS içinde barındırıldığında, ayrıca aynı yapılandırma, oturum verilerini ve diğer altyapı olarak paylaşır, Masaüstü sayfaları.

> [!NOTE]
> Bu yaklaşım genellikle kod bazı çoğaltılmasıyla gerektirdiğinden (mobil sayfalar Masaüstü sayfalarıyla bazı benzerliğe olasılığı), tüm genel iş mantığı ya da veri erişim kodu paylaşılan, temel katmana ya da hizmet çarpanını önemlidir. Aksi takdirde, oluşturulması ve bakımının yapılması uygulamanızın çift çaba gerekir.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Mobil ziyaretçiler, mobil sayfalar için yeniden yönlendirme

Genellikle yalnızca mobil sayfalar için mobil ziyaretçiler yönlendirmek uygun olan *ilk* istek kendi gözatma oturumunda (ve her istekte oturumuna göre değil), çünkü:

- Mobil ziyaretçiler, bunlar – yalnızca ana sayfanızda "Masaüstü sürümünü" giden bir bağlantı koymak istiyorsanız Masaüstü sayfalarınızı erişmesine kolayca izin verebilirsiniz. Artık kendi oturumunda ilk istek olmadığı için mobil sayfasına geri bir ziyaretçi yönlendirilmesi olmaz.
- Herhangi bir dinamik (örneğin, hem Masaüstü hem de mobil bölümlerini sitenizin bir IFRAME veya belirli Ajax işleyicileri görüntülemek ortak bir Web formu varsa) sitenizin Masaüstü ve mobil bölümleri arasında paylaşılan kaynaklar için istekleri ile engellemesini riskini önler

Bunu yapmak için yeniden yönlendirme mantığınızda yerleştirebilirsiniz bir **oturumu\_Başlat** yöntemi. Örneğin, Global.asax.cs dosyanıza aşağıdaki yöntemi ekleyin:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Mobil sayfalarınızı cevaben Forms kimlik doğrulamasını yapılandırma

Form kimlik doğrulaması hakkında burada, ziyaretçi sırasında ve sonrasında kimlik doğrulama işlemi yönlendirebilirsiniz bazı varsayımlarda kullandığına dikkat edin:

- Bir kullanıcı kimlik doğrulaması gerektiğinde, form kimlik doğrulaması bunları Masaüstü veya mobil kullanıcı olmanıza bakılmaksızın Masaüstü Oturum açma sayfasına yönlendirir (yalnızca bir kavramı bulunduğundan *bir* oturum açma URL'si). Farklı mobil oturum açma sayfanız stilini belirlemek istediğiniz varsayılarak, mobil kullanıcıların ayrı mobil oturum açma sayfasına yeniden yönlendirir, böylece Masaüstü Oturum açma sayfanız geliştirmek gerekir. Örneğin, aşağıdaki kodu ekleyin, **Masaüstü** oturum açma sayfası arka plan kod: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Kullanıcı başarıyla oturum açtığında sonra form kimlik doğrulaması varsayılan olarak bunları Masaüstü giriş sayfasına yönlendirir (yalnızca bir kavramı bulunduğundan *bir* varsayılan sayfası). Bir başarılı oturum açma işleminden sonra mobil giriş sayfasına yönlendirir, böylece mobil oturum açma sayfanız geliştirmek için ihtiyacınız. Örneğin, aşağıdaki kodu ekleyin, **mobil** oturum açma sayfası arka plan kod: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Bu kod, sayfanız LoginUser, olduğu gibi varsayılan proje şablonu adlı bir oturum açma sunucusu denetimi sahip varsayar.

### <a name="working-with-output-caching"></a>Çıktı önbelleği ile çalışma

Çıktı önbelleği kullanıyorsanız, bir masaüstü kullanıcı (çıktısını önbelleğe alınmasına neden) belirli bir URL'yi ziyaret mümkündür varsayılan olarak, ardından önbelleğe alınan Masaüstü çıktıyı alan bir mobil kullanıcı tarafından izlenen dikkatli olun. Bu uyarı, yalnızca ana sayfanıza cihaz türüne göre değişen ediyorsanız, cihaz türüne göre tamamen ayrı bir Web Forms uygulama uygulanır.

Sorunu önlemek için önbellek girdisi ziyaretçi bir mobil cihaz kullanmanıza göre değişen ASP.NET bildirebilirsiniz. VaryByCustom parametre sayfanızın OutputCache bildirimine aşağıdaki şekilde ekleyin:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Ardından, tanımlama *isMobileDevice* özel bir önbellek olarak aşağıdaki yöntemi ekleyerek parametresi Global.asax.cs dosyanıza geçersiz kıl:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Bu sayfaya mobil ziyaretçiler çıkış daha önce bir masaüstü ziyaretçisi tarafından önbelleğe koyulur almadığınız garanti eder.

### <a name="a-working-example"></a>Çalışan bir örnek

Bu teknikler iş başında görmek için indirme [Bu incelemeyi ait kod örnekleri](https://docs.microsoft.com/aspnet/mobile/overview). Web Forms örnek uygulaması, mobil kullanıcılar otomatik olarak mobile özgü sayfalar mobil adlı bir alt kümesi için yönlendirir. Biçimlendirme ve stil bu sayfaların daha iyi optimize mobil tarayıcılar için aşağıdaki ekran görüntüleri görebileceğiniz gibi:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Biçimlendirme ve CSS mobil tarayıcılar için en iyi duruma getirme hakkında daha fazla ipucu için "Stil mobil sayfalar mobil tarayıcılar için" Bu belgenin sonraki bölümlerinde bölümüne bakın.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>ASP.NET MVC uygulamaları mobile özgü sayfaları nasıl sunabilirsiniz

Model-View-Controller deseni uygulama mantığını (denetleyiciler) sunum mantığı (görünümlerde) öğesinden ayırır olduğundan, sunucu tarafı kodu mobil desteği işlemek için aşağıdaki yaklaşımlardan birini seçebilirsiniz:

1. ***Masaüstü ve mobil tarayıcılar için aynı denetleyicileri ve görünümleri kullanın, ancak görünümleri bağlı olarak cihaz türü * farklı Razor düzeni ile işleyin.** Tüm cihazlarda aynı veri görüntülediğinizden, ancak yalnızca farklı CSS stil sağlayın veya kaydettiğinde için birkaç en üst düzey HTML öğeleri değiştirmek istiyorsanız bu seçenek en iyi şekilde çalışır.
2. ***Masaüstü ve mobil tarayıcılar için aynı denetleyicileri kullanır, ancak cihaz türüne bağlı olarak farklı görünümler oluşturma***. Bu seçenek, yaklaşık aynı verileri görüntüleme ve son kullanıcılar için aynı iş akışları sağlayan en iyi şekilde çalışır ancak kullanılan cihaz uyacak şekilde çok farklı HTML biçimlendirmesi oluşturmak isteyebilirsiniz.
3. ***Masaüstü ve mobil tarayıcılar, bağımsız denetleyicileri ve görünümlerinin her uygulama için ayrı alanlar oluşturun *.** Bu seçenek, çok farklı ekranları görüntüleme, farklı bilgi içeren ve kullanıcı, cihaz türü için en iyi duruma getirilmiş farklı iş akışları aracılığıyla önde gelen en iyi şekilde çalışır. Bazı kod tekrarını gelebilir, ancak, temel katman veya hizmet ortak mantığı katacak küçültebilirsiniz.

Göz atmak istiyorsanız **ilk** seçenek ve Razor Düzen farklı cihaz türü çok kolay. Yalnızca değiştirmek, \_ViewStart.cshtml dosya gibi:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Adlı bir mobile özgü düzeni oluşturabilir artık \_LayoutMobile.cshtml sayfa yapısı ve CSS kurallarını mobil cihazlar için en iyi duruma getirilmiş.

Göz atmak istiyorsanız **ikinci** ziyaretçi cihaz türüne göre farklı görünümleri oluşturma seçeneği için bkz: [Scott Hanselman'ın blog gönderisi](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Bu yazıda geri kalanını odaklanır **üçüncü** ayrı denetleyicileri oluşturma seçeneği – *ve* işlevsellik hangi alt mobil ziyaretçiler için sunulan tam olarak denetleyebilirsiniz. böylece mobil cihazları – görünümleri.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>ASP.NET MVC uygulamanızda mobil bir alanı ayarlama

Normal bir şekilde mevcut bir ASP.NET MVC uygulaması için "Mobile" adlı bir alan ekleyebilirsiniz: Çözüm Gezgini'nde proje adına sağ tıklayın ve ardından Ekle à alanı seçin. Bir ASP.NET MVC uygulaması içindeki herhangi bir alan için yaptığınız gibi ardından denetleyicileri ve görünümleri ekleyebilirsiniz. Örneğin, mobil bölgenizde mobil ziyaretçiler için giriş sayfası olarak davranacak şekilde HomeController adlı yeni bir denetleyici ekleyeceksiniz.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>URL /Mobile sağlama ulaştığında mobil giriş sayfası

HomeController mobil bölgenizde içinde dizin eylem ulaşmak için URL /Mobile istiyorsanız yönlendirme yapılandırmanızı iki küçük değişiklikler yapmanız gerekir. İlk olarak, böylece HomeController mobil bölgenizdeki varsayılan denetleyicisi MobileAreaRegistration sınıfınıza aşağıdaki kodda gösterildiği gibi güncelleştirin:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

"Home" şimdi olduğundan mobil giriş sayfası artık konumlandırılacağı/Mobile/giriş, yerine /Mobile yani örtük olarak mobil sayfalar için varsayılan denetleyici adı.

Ardından, uygulamanıza (yani, mobil bir tane var olan bir Masaüstü ek olarak) ikinci HomeController ekleyerek, normal Masaüstü sayfanızı kıran olduğunu unutmayın. Şu hata ile başarısız olur "*'Giriş' adlı denetleyicisi eşleşen birden çok tür bulundu*". Bu sorunu çözmek için Masaüstü HomeController belirsizlik olduğunda öncelik zamanınızı belirtmek için üst düzey yönlendirme yapılandırmasında (Global.asax.cs) güncelleştirin:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Hata geçilir koy ve URL http:// artık<em>yoursite</em>/ Masaüstü giriş sayfası ve http:// ulaşacak<em>yoursite</em>/mobile/ ulaşmak mobil giriş sayfası.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Mobil ziyaretçiler, mobil alanına yönlendirme

Var. birçok farklı genişletilebilirlik noktaları ASP.NET MVC'de şekilde yeniden yönlendirme mantığınızı eklemek birçok olası yolları Masaüstünüzdeki seçeneklerden biri, aşağıdaki koşullar karşılandığında bir yeniden yönlendirme gerçekleştiren bir filtre özniteliği [RedirectMobileDevicesToMobileArea] oluşturmaktır:

1. İlk istek kullanıcının oturumunu olduğu (yani, Session.IsNewSession true eşittir)
2. İstek bir mobil tarayıcıda gelir (yani, Request.Browser.IsMobileDevice true eşittir)
3. Kullanıcı zaten bir kaynak mobil alanında isteyen değil (yani, *yolu* URL'SİNİN bir parçası /Mobile başlamak değil)

Bu mantık uygulaması ile bu teknik incelemeyi dahil karşıdan yüklenebilir örnek içerir. (Aksi takdirde, bir masaüstü ziyaretçi ilk erişimleri belirli URL Masaüstü çıkış gizlenebilir ve sonra için sunulan, çıktı önbelleği kullanıyorsanız, düzgün çalışabilmesi yani AuthorizeAttribute öğesinden türetilen bir yetkilendirme filtresi olarak uygulanır sonraki mobil ziyaretçiler).

Bir filtre olduğu gibi belirli denetleyicileri ve eylemleri, uygulanacak ya da örneğin seçebilirsiniz,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… veya, tüm denetleyicilere ve eylemlere bir MVC 3'olarak uygulayabilirsiniz *genel filtre* Global.asax.cs dosyanızda:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

İndirilebilir örnek ayrıca, mobil alanda belirli konumlara yönlendirme alt sınıflar bu özniteliğin nasıl oluşturabileceğinizi gösterir. Bu, örneğin, şunları yapabilirsiniz anlamına gelir:

- Mobil ziyaretçiler mobil giriş sayfasının varsayılan olarak gönderdiği genel filtre olarak gösterilen yukarıdaki kaydedin.
- Ayrıca mobil ziyaretçiler istenen her ürün sayfası mobil sürümüne gerçekleştireceği bir "görünümü product" eylemi için özel [RedirectMobileDevicesToMobileProductPage] filtre uygulayın.
- Filtrenin özel diğer alt sınıflar için mobil ziyaretçiler eşdeğer mobil sayfasına yönlendirme, belirli eylemleri de geçerlidir

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Mobil sayfalarınızı cevaben Forms kimlik doğrulamasını yapılandırma

Form kimlik doğrulaması kullanıyorsanız, bir kullanıcı, oturum açmak gerektiğinde, bunu otomatik olarak kullanıcı olan varsayılan olarak bir tek belirli "Oturum Aç" için yeniden yönlendirildiği URL, unutmamalısınız **/hesabı/oturum açma**. Bu, mobil kullanıcılar, Masaüstü "Oturum Aç" eylemini yönlendirilebilirsiniz anlamına gelir.

Bu sorunu önlemek için mobil bir "Oturum Aç" eylem mobil kullanıcıların yeniden yönlendirir, böylece Masaüstü "Oturum Aç" eylemi için mantık ekleyin. AccountController'ın oturum açma eylemi, ' % s'varsayılan ASP.NET MVC uygulama şablonu kullanıyorsanız, şu şekilde güncelleştirin:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… ve ardından mobil bölgenizde AccountController adlı denetleyicisine uygun bir mobile özgü "Oturum Aç" eylem uygulayın.

### <a name="working-with-output-caching"></a>Çıktı önbelleği ile çalışma

[OutputCache] filtresi kullanıyorsanız, cihaz türüne göre değişen önbellek girdisi zorlaması gerekir. Örneğin, şunu yazın:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Ardından, Global.asax.cs dosyanıza aşağıdaki yöntemi ekleyin:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Bu sayfaya mobil ziyaretçiler çıkış daha önce bir masaüstü ziyaretçisi tarafından önbelleğe koyulur almadığınız garanti eder.

### <a name="a-working-example"></a>Çalışan bir örnek

Bu teknikler iş başında görmek için indirme [Bu incelemeyi ait kod örnekleri ilişkili](https://docs.microsoft.com/aspnet/mobile/overview). Örnek, yukarıda açıklanan yöntemi kullanarak mobil cihazları desteklemek için Gelişmiş bir ASP.NET MVC 3 (Sürüm Adayı) uygulaması içerir.

## <a name="further-guidance-and-suggestions"></a>Ek yönergeler ve öneriler

Aşağıdaki açıklama, hem Web Forms ve bu belgede ele alınan teknikleri kullanarak MVC geliştiriciler için geçerlidir.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Yeniden yönlendirme mantığınızı 51Degrees.mobi Foundation'ı kullanarak geliştirme

Bu belgede gösterilen yeniden yönlendirme mantıksal uygulamanız için mükemmel bir şekilde yeterli olabilir, ancak oturumlarını devre dışı bırakmanız gerekirse çalışmaz veya belirli bir istek olup olmadığını bilemezsiniz içerdiği (Bu oturumları olamaz) tanımlama bilgilerini reddedebilirsiniz mobil tarayıcılar ile olan İlki, ziyaretçi öğesinden.

Zaten açık kaynak 51Degrees.mobi Foundation ASP doğruluğunu nasıl geliştireceğiniz öğrendiniz. NET tarayıcı algılama. Ayrıca, mobil Ziyaretçiler Web.config dosyasında yapılandırılan belirli konumlara yönlendirmek için yerleşik bir olanağı da vardır. Olmadan ASP.NET oturumları bağlı olarak çalışabilmek için (ve dolayısıyla tanımlama bilgileri) karmaları ziyaretçilerinizin HTTP üst bilgilerini ve IP adreslerini geçici günlüğünü depolayarak, bu nedenle giden her isteğin birinci verilen vistor olup olmadığını.

Web.config dosyasının fiftyOne bölümüne eklenen aşağıdaki öğe, algılanan bir mobil CİHAZDAN ilk isteği sayfasına yönlendirir ~ / Mobile/Default.aspx. Sayfaları mobil klasörü altında yapılan tüm isteklere olacak *değil* yönlendirilir, cihaz türü ne olursa olsun. Mobil cihaz bir süre boyunca etkin olmaması durumunda 20 dakika veya daha fazla cihaz Unutulan ve yenilerini yeniden yönlendirme amacıyla sonraki istekleri kabul edilir.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Daha fazla ayrıntı için [51degrees.mobi Foundation belgeleri](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> *Olabilir* Yönlendirme parametreleri açısından değil veya MVC filtreleri koyarak düz URL'leri açısından, yeniden yönlendirme yapılandırmasını tanımlamak ASP.NET MVC uygulamaları, ancak kullanım 51Degrees.mobi Foundation'ın yeniden yönlendirme özelliği gerekir Eylemler. Bunun nedeni, (yazma sırasında) filtreleri ya da yönlendirme 51Degrees.mobi Foundation tanımıyor.


### <a name="disabling-transcoders-and-proxy-servers"></a>Biçim dönüştürücüleri ve Proxy sunucuları devre dışı bırakma

Mobil Ağ İşletmenleri iki geniş hedefe yaklaşımlarını mobil İnternet'e sahip:

1. Mümkün olduğunca çok ilgili içerik sağlayın
2. Radyo sınırlı ağ bant genişliğini paylaşabilirsiniz müşterilerin sayısını en üst düzeye çıkarın

Çoğu web sayfaları Masaüstü boyutlu büyük ekranlar ve hızlı sabit satır içi geniş ağ bağlantıları için tasarlandığından, birçok işleçleri kullanan *biçim dönüştürücüleri* veya *proxy sunucuları* web içeriği dinamik olarak değiştirin. HTML biçimlendirmesi ya da CSS daha küçük ekranlar ("olan işleme gücüne karmaşık düzenler işlemek için eksik özellikle özellik telefonlar için") uyacak şekilde değiştirebilir ve sayfa teslim hızını artırmak için (bunların kalitesinin önemli ölçüde azaltarak) görüntülerinizi sıkıştırmak.

Ancak büyük olasılıkla sitenizin mobil için iyileştirilmiş bir sürümünü oluşturmak için gereken çabayı zamandaki, ağ operatörünüze kendisiyle herhangi ek engel olmasını istemiyorsanız. Aşağıdaki satırı sayfasına ekleyebilirsiniz\_herhangi bir ASP.NET Web formu yük olayı:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Veya, bir ASP.NET MVC denetleyicisi için denetleyicideki tüm eylemler için geçerli olacak şekilde aşağıdaki yöntemi geçersiz kılma ekleyebilirsiniz:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Elde edilen HTTP iletisi W3C uyumlu kod dönüştürücüleri ve içerik değiştirmeyin proxy'leri bildirir. Elbette, mobil Ağ İşletmenleri bu iletiyi uyar garantisi yoktur.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Mobil tarayıcılar için stil mobil sayfalar

Harika ayrıntılı tanımlamak için bu belgenin kapsamı ne tür HTML biçimlendirme çalışmanın düzgün dışındadır veya hangi web tasarım teknikleri belirli cihazlarda kullanılabilirliği en üst düzeye çıkarın. Bu yukarı yeteri kadar basit bir düzen bulmak için mobil boyutlu bir ekran için güvenilir kullanmadan HTML veya püf noktaları konumlandırma CSS en iyi hale getirdi. Değer bahseden, bir önemli ancak tekniktir *Görünüm penceresi meta etiketi*.

Belirli modern mobil tarayıcılar, Masaüstü izleyiciler için gereken bir çaba görünen web sayfalarında sayfayı "Görünüm" olarak da bilinir, sanal bir tuval üzerinde oluşturmak (örneğin, sanal görünüm penceresinin İphone'da 980 piksel ve varsayılan olarak Opera Mobile geniş 850 piksel adıdır) ve ardından sonucu ekranın fiziksel piksel sığdırmak için ölçeği azaltın. Kullanıcı daha sonra yakınlaştırmak ve o görünüm penceresinin kaydır. Bu avantajı vardır hedeflenen yerleşimi etkinleştirilirken sayfada tarayıcı sağlar, ancak aynı zamanda kullanıcı için kullanışsız, yakınlaştırma ve kaydırma, zorlar dezavantajı vardır. Mobil için tasarlıyorsanız, yakınlaştırma veya yatay kaydırma gerekli olması daha dar bir ekran için tasarım iyidir.

Görünüm penceresinin nasıl geniş olmalıdır taşınabilir tarayıcıya için bir standart dışı yoludur *Görünüm penceresi* meta etiketi. Örneğin, aşağıdaki sayfanızın baş bölümüne eklerseniz,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… smartphone tarayıcılar'ı destekleyen sayfasında 480 piksel geniş sanal tuval düzenleme. Bu, HTML öğeleri yüzde genişliklerini tanımlarsanız, yüzde varsayılan görünüm penceresinin genişliğini bu 480 piksel genişliği göre yorumlanacaktır anlamına gelir. Sonuç olarak, kullanıcının yakınlaştırmasına ve kaydırmasına yatay – önemli ölçüde mobil gözatma deneyimini geliştirmek için daha az olasıdır.

Cihazın fiziksel piksel eşleştirmek için Görünüm penceresi genişliğini istiyorsanız, aşağıdakileri belirtebilirsiniz:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Bunun düzgün çalışması için açıkça bu genişliği aşmayı öğeleri zorlamalısınız değil (örn, kullanarak bir *genişliği* özniteliği ya da CSS özelliği), aksi takdirde tarayıcı daha büyük bir Görünüm penceresi bakılmaksızın kullanmaya zorlanır. Ayrıca bkz: [kullanıldı Görünüm penceresi etiketi hakkında daha fazla ayrıntı](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Çoğu modern akıllı telefonlar Destek *çift yönlendirme*: dikey veya yatay modda tutulabilir. Bu nedenle, ekran genişliği ile ilgili varsayım piksel cinsinden yapmamak önemlidir. Ekran genişliği sabit sayfanızda sırasında kullanıcı cihazını yeniden yönlendirebilirsiniz çünkü bile varsaymayın.

Eski Windows Mobile ve Blackberry cihazları da içerik onlara neyi bildirmek için sayfa üst bilgisindeki aşağıdaki meta etiketler mobil için iyileştirilmiş ve bu nedenle değil dönüştürülmesi gereken kabul etmesi olabilir.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Ek Kaynaklar

Sayfayı mobil aygıt benzetmeleri ve simülatörler mobil ASP.NET web uygulamanızı test etmek için kullanabileceğiniz bir listesi için bkz. [test için popüler Mobil cihazların benzetimini yapma](../mobile/device-simulators.md).

## <a name="credits"></a>Krediler

- Birincil Yazar: Steven Sanderson
- Gözden geçirenler / ek yazarların içerik: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
