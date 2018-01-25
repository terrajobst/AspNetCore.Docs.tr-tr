---
uid: whitepapers/aspnet4/overview
title: "ASP.NET 4 ve Visual Studio 2010 Web geliştirme genel bakış | Microsoft Docs"
author: rick-anderson
description: "Bu belgede, Visual Studio 2010 ve.NET Framework 4'te yer alan ASP.NET için yeni özelliklerin bir genel bakış sağlanır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: f0224bcd2badc423ba5146feacccc44b8f33a608
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 ve Visual Studio 2010 Web geliştirme genel bakış
====================
> Bu belgede, Visual Studio 2010 ve.NET Framework 4'te yer alan ASP.NET için yeni özelliklerin bir genel bakış sağlanır.
> 
> [Bu teknik indirin](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**İçindekiler**

**[Çekirdek Hizmetleri](#0.2__Toc253429238 "_Toc253429238")**  
[Web.config dosyasına düzenlemesi](#0.2__Toc253429239 "_Toc253429239")  
[Genişletilebilir çıktı önbelleği](#0.2__Toc253429240 "_Toc253429240")  
[Web uygulamaları otomatik olarak başlatma](#0.2__Toc253429241 "_Toc253429241")  
[Kalıcı bir sayfasına yeniden yönlendirmeye](#0.2__Toc253429242 "_Toc253429242")  
[Oturum durumu küçültme](#0.2__Toc253429243 "_Toc253429243")  
[İzin verilen URL'ler aralığını genişletme](#0.2__Toc253429244 "_Toc253429244")  
[Genişletilebilir istek doğrulama](#0.2__Toc253429245 "_Toc253429245")  
[Önbelleğe alma nesne ve önbelleğe alma genişletilebilirlik nesne](#0.2__Toc253429246 "_Toc253429246")  
[Genişletilebilir HTML, URL ve HTTP Kodlama üstbilgisi](#0.2__Toc253429247 "_Toc253429247")  
[Tek çalışan işlemindeki tek tek uygulamalar için performans izleme](#0.2__Toc253429248 "_Toc253429248")  
[Çoklu sürüm desteği](#0.2__Toc253429249 "_Toc253429249")

**[AJAX](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery eklenen Web Forms ve MVC](#0.2__Toc253429251 "_Toc253429251")  
[İçerik teslim ağı Destek](#0.2__Toc253429252 "_Toc253429252")  
[ScriptManager açık betikleri](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Meta etiketleri Page.MetaKeywords ve Page.MetaDescription özelliklerini ayarlama](#0.2__Toc253429257 "_Toc253429257")  
[Görünüm durumu için ayrı ayrı denetimler etkinleştirme](#0.2__Toc253429258 "_Toc253429258")  
[Değişiklikleri tarayıcı yetenekleri](#0.2__Toc253429259 "_Toc253429259")  
[ASP.NET 4 yönlendirme](#0.2__Toc253429260 "_Toc253429260")  
[İstemci kimlikleri ayarı](#0.2__Toc253429261 "_Toc253429261")  
[Veri denetimleri kalıcı satır seçimi](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET grafik denetiminin](#0.2__Toc253429263 "_Toc253429263")  
[QueryExtender denetimi ile verileri filtreleme](#0.2__Toc253429264 "_Toc253429264")  
[HTML kod ifadeleri kodlanmış](#0.2__Toc253429265 "_Toc253429265")  
[Proje şablonu değişiklikleri](#0.2__Toc253429266 "_Toc253429266")  
[CSS geliştirmeleri](#0.2__Toc253429267 "_Toc253429267")  
[Div öğeleri geçici gizli alanları gizleme](#0.2__Toc253429268 "_Toc253429268")  
[Bir dış tablo için şablonlu denetimlerini işlemeye](#0.2__Toc253429269 "_Toc253429269")  
[ListView denetimi geliştirmelerini](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList ve RadioButtonList denetim geliştirmeleri](#0.2__Toc253429271 "_Toc253429271")  
[Menü denetim geliştirmeleri](#0.2__Toc253429272 "_Toc253429272")  
[Sihirbaz ve CreateUserWizard denetimleri 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Alanlarda Destek](#0.2__Toc253429275 "_Toc253429275")  
[Veri ek açıklamasını özniteliği doğrulama desteği](#0.2__Toc253429276 "_Toc253429276")  
[Şablonlu Yardımcılar](#0.2__Toc253429277 "_Toc253429277")

**[Dinamik veri](#0.2__Toc253429278 "_Toc253429278")**  
[Dinamik veri için mevcut projeleri etkinleştirme](#0.2__Toc253429279 "_Toc253429279")  
[Bildirim temelli DynamicDataManager denetimi sözdizimi](#0.2__Toc253429280 "_Toc253429280")  
[Varlık şablonları](#0.2__Toc253429281 "_Toc253429281")  
[URL'ler ve e-posta adresleri için yeni alan şablonlar](#0.2__Toc253429282 "_Toc253429282")  
[DynamicHyperLink denetimiyle bağlantılar oluşturma](#0.2__Toc253429283 "_Toc253429283")  
[Veri modelindeki devralma desteği](#0.2__Toc253429284 "_Toc253429284")  
[Çok-çok ilişkileri (yalnızca Entity Framework) için destek](#0.2__Toc253429285 "_Toc253429285")  
[Denetim görüntüleme ve Destek numaralandırma için yeni öznitelikler](#0.2__Toc253429286 "_Toc253429286")  
[Filtreler için gelişmiş destek](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web geliştirme geliştirmeleri](#0.2__Toc253429288 "_Toc253429288")**  
[CSS uyumluluk geliştirilmiş](#0.2__Toc253429289 "_Toc253429289")  
[HTML ve JavaScript parçacıkları](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript IntelliSense geliştirmeleri](#0.2__Toc253429291 "_Toc253429291")

**[Web uygulama dağıtımı Visual Studio 2010 ile](#0.2__Toc253429292 "_Toc253429292")**  
[Web paketleme](#0.2__Toc253429293 "_Toc253429293")  
[Web.config dönüştürmesini](#0.2__Toc253429294 "_Toc253429294")  
[Veritabanı dağıtımı](#0.2__Toc253429295 "_Toc253429295")  
[Tek tıklamayla yayımlama Web uygulamaları için](#0.2__Toc253429296 "_Toc253429296")  
[Kaynakları](#0.2__Toc253429297 "_Toc253429297")

**[Vazgeçme](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Çekirdek Hizmetleri

ASP.NET 4 çekirdek ASP.NET Hizmetleri çıktı önbelleği ve oturum durumu depolama gibi artırmak özelliklerini tanıtır.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Web.config dosyasını yeniden düzenleme

`Web.config` Ajax gibi yeni özellikler eklenmiştir gibi bir Web uygulaması .NET Framework'ün birkaç geçmiş sunumlar önemli ölçüde büyümüştür için yapılandırmayı içeren dosya Yönlendirme ve IIS 7 ile tümleştirme. Bu, yapılandırma veya Visual Studio gibi bir araç olmadan yeni Web uygulamalarını başlatmak daha zor sağlamıştır. Önemli yapılandırma öğelerini kullanarak NET Framework 4'de, taşınmış olan `machine.config` dosya ve uygulamaların artık bu ayarları devralır. Böylece `Web.config` ASP.NET 4 uygulamaları boş veya Visual Studio için uygulama hedefleme framework'ün hangi sürümünün belirten yalnızca aşağıdaki satırları içeren dosyasında:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Genişletilebilir çıktı önbelleği

ASP.NET 1.0 yayımlanan zamanından bu yana, çıktı önbelleği bellekte oluşturulan sayfalar, denetimleri ve HTTP yanıtlarını çıktısını depolamak geliştiricilere etkinleştirdi. Sonraki Web isteklerinde ASP.NET içeriği daha hızlı bir şekilde çıkış baştan yeniden yerine bellekten oluşturulan çıktı alarak hizmet verebilir. Ancak, bu yaklaşımın bir sınırlama vardır — oluşturulan içeriği her zaman sahip bellekte depolanması ve yoğun trafik yaşıyor sunucularında, bir Web uygulaması diğer kısımlarından bellek taleplerini çıktı önbelleği tarafından tüketilen bellek rekabet.

ASP.NET 4 bir veya daha fazla özel çıkış önbelleği sağlayıcısı yapılandırmanıza olanak sağlayan önbelleğe alma çıkarmak için genişletilebilirlik noktanız ekler. Çıktı önbelleği sağlayıcılarını herhangi bir depolama mekanizma HTML içeriği kalıcı hale getirmek için kullanabilirsiniz. Bu depolama yerel veya uzak diskleri içerebilir, farklı Kalıcılık mekanizmaları bulut özel çıkış önbelleği sağlayıcıları oluşturmak mümkün hale getirir ve önbellek motorları dağıtılmış.

Yeni türeyen bir sınıf olarak özel bir çıkış önbelleği sağlayıcısı oluşturmanız *System.Web.Caching.OutputCacheProvider* türü. Daha sonra sağlayıcısında yapılandırabilirsiniz `Web.config` yeni kullanarak dosya *sağlayıcıları* , alt bölüm *outputCache* aşağıdaki örnekte gösterildiği gibi:

[!code-xml[Main](overview/samples/sample2.xml)]

Varsayılan olarak ASP.NET 4, tüm HTTP yanıtları sayfaları işlenmiş ve denetimleri kullanın bellek içi çıktı önbelleği önceki örnekte gösterildiği gibi nerede *defaultProvider* özniteliği AspNetInternalProvider için ayarlanmış. İçin farklı bir sağlayıcı adı belirterek bir Web uygulaması için kullanılan varsayılan çıkış önbelleği sağlayıcısı değiştirebileceğiniz *defaultProvider*.

Ayrıca, farklı çıkış önbelleği sağlayıcıları Denetim başına ve istek başına seçebilirsiniz. Böylece bildirimli olarak yeni kullanarak yapmak için farklı Web kullanıcı denetimleri için farklı bir çıkış önbelleği sağlayıcısı seçmek için en kolay yolu olan *providerName* aşağıdaki örnekte gösterildiği gibi bir denetim yönergesinde özniteliği:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Bir HTTP isteği için farklı bir çıkış önbelleği sağlayıcısı belirtme biraz daha fazla iş gerektirir. Sağlayıcı bildirimli olarak belirtmek yerine, yeni geçersiz kılma *GetOuputCacheProviderName* yönteminde `Global.asax` program aracılığıyla belirli bir istek için kullanacağı sağlayıcısını belirtmek için dosya. Aşağıdaki örnek bunun nasıl yapılacağı gösterilmektedir.

[!code-csharp[Main](overview/samples/sample4.cs)]

Çıkış önbelleği sağlayıcısı genişletilebilirlik ASP.NET 4 eklenmesiyle, artık daha agresif ve daha akıllı çıkış önbelleğe alma stratejileri Web siteleriniz için bir işin peşine düşmek. Örneğin, artık alt trafik diskte alma sayfaları önbelleğe alma sırasında "İlk 10" sayfaları bir sitenin bellekte önbelleğe almak mümkündür. Alternatif olarak, her farklılık tarafından işlenen bir sayfa için birleşimi önbelleğe ancak dağıtılmış önbellek bellek tüketimi ön uç Web sunucularından Boşaltılan sağlamak için kullanın.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Otomatik başlatma Web uygulamaları

Büyük miktarda veriyi yüklemek veya işleme ilk istek sunmadan önce pahalı başlatma gerçekleştirmek bazı Web uygulamaları gerekir. ASP.NET önceki sürümlerinde, bu durumlarda, "ASP.NET uygulaması uyandırmak" ve sırasında başlatma kodu çalıştırmak için özel yaklaşımlar insanlara gerekiyordu *uygulama\_yük* yönteminde `Global.asax` dosya.

Adlı yeni bir ölçeklenebilirlik özelliği *otomatik başlatma* adresleri bu senaryo kullanılabilir doğrudan ASP.NET 4 IIS 7.5 Windows Server 2008 R2 üzerinde çalıştığında. Otomatik başlatma özellik, bir uygulama havuzunu başlatma, bir ASP.NET uygulamasını başlatma ve HTTP isteklerini kabul etmek için bir kontrollü yaklaşım sağlar.

> [!NOTE] 
> 
> IIS 7.5 için IIS uygulama ısınması Modülü
> 
> IIS takım IIS 7.5 için uygulama ısınması modülü ilk beta test sürümünü yayımladı. Bu, ısıtma uygulamalarınızı daha önce açıklanan çok daha kolay hale getirir. Özel kod yazmak yerine, Web uygulaması ağ gelen istekleri kabul etmeden önce yürütülecek kaynakları URL'lerini belirtin. IIS hizmeti başlatma sırasında bu Isınma oluşur (IIS uygulama havuzu olarak yapılandırılmışsa *AlwaysRunning*) ve ne zaman bir IIS çalışan işlemi geri dönüştürüldüğünde. Geri Dönüşüm sırasında yeni oluşturulan çalışan işlem, tam olarak warmed kadar böylece hiçbir kesintiler ve diğer sorunlar unprimed önbellekleri nedeniyle uygulamaları deneyimi isteklerin yürütülebilmesi eski IIS çalışan işlemi devam eder. Bu modül ASP.NET 2.0 sürümü ile başlayarak, herhangi bir sürümüyle çalıştığını unutmayın.
> 
> Daha fazla bilgi için bkz: [uygulama ısınması](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) IIS.NET Web sitesinde. Isınma özelliğinin nasıl kullanılacağı gösterilmektedir bir anlatım için bkz: [IIS 7.5 uygulama ısınması modülü ile çalışmaya başlama](https://www.iis.net/learn/manage) IIS.NET Web sitesinde.


Otomatik başlatma özelliğini kullanmak için IIS Yöneticisi bir uygulama havuzu aşağıdaki yapılandırmayı kullanarak otomatik olarak başlatılması için IIS 7.5 ayarlar `applicationHost.config` dosyası:

[!code-xml[Main](overview/samples/sample5.xml)]

Tek bir uygulama havuzunda birden çok uygulama içerdiğinden aşağıdaki yapılandırmayı kullanarak otomatik olarak başlatılması için ayrı ayrı uygulamaların belirtin `applicationHost.config` dosyası:

[!code-xml[Main](overview/samples/sample6.xml)]

Bir IIS 7.5 sunucu soğuk başlatılan olduğunda veya ayrı bir uygulama havuzu geri dönüştürüldüğünde, IIS 7.5 bilgileri kullanır `applicationHost.config` otomatik olarak başlatılması için hangi Web uygulamaları gerek belirlemek için dosya. Otomatik başlatma için işaretlendiğinden her uygulama için IIS7.5 bir isteği sırasında uygulama geçici olarak kabul etmediği bir durumda uygulamayı başlatmak için ASP.NET 4 HTTP istekleri gönderir. Bu durumda olduğunda, ASP.NET tarafından tanımlanan türü başlatır *serviceAutoStartProvider* (önceki örnekte gösterildiği gibi) özniteliği ve kendi ortak giriş noktası çağrılar.

Uygulayarak bir yönetilen otomatik başlangıç türü ile gerekli giriş noktası oluşturmanız *IProcessHostPreloadClient* arabirimi, aşağıdaki örnekte gösterildiği gibi:

[!code-csharp[Main](overview/samples/sample7.cs)]

Sonra başlatma kodu çalışan *önyükleme* yöntemi ve yöntemi döndürür, ASP.NET uygulama isteklerini işlemek hazır.

Otomatik olarak başlatılacak şekilde.5 IIS ve ASP.NET 4 eklenmesiyle ilk HTTP istek işleme önce pahalı uygulama başlatma gerçekleştirmek için iyi tanımlanmış bir yaklaşım vardır. Örneğin, bir uygulamayı başlatın ve sonra uygulamayı başlatılmış ve HTTP trafiği kabul etmeye hazır bir yük dengeleyici sinyal için yeni otomatik başlatma özelliğini kullanabilirsiniz.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Kalıcı olarak bir sayfa yeniden yönlendirme

Bu arama motorları eski bağlantılar toplamı açabilir zaman içinde sayfaları ve diğer içeriği geçici taşımak için Web uygulamalarında yaygın uygulamadır. ASP.NET, geliştiricilerin geleneksel eski URL'leri kullanarak işlenen istekleri *Response.Redirect* yeni URL için bir istek iletmek için yöntem. Ancak, *yeniden yönlendirme* yöntemi kullanıcıların eski URL'leri erişmeyi denediğinde, ek bir HTTP gidiş dönüş sonuçları bir HTTP 302 bulundu (geçici yeniden yönlendirme) yanıt verir.

Yeni bir ASP.NET 4 ekler *RedirectPermanent* sorunu HTTP 301 kolaylaştırır yardımcı yöntem taşınmış kalıcı olarak aşağıdaki örnekteki gibi yanıtları:

[!code-csharp[Main](overview/samples/sample8.cs)]

Arama motorları ve kalıcı yeniden yönlendirmeleri tanıması diğer kullanıcı aracıları geçici yeniden yönlendirmeleri tarayıcısı tarafından yapılan gereksiz gidiş dönüş ortadan kaldırır içerik ile ilişkili yeni URL depolar.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Oturum durumu küçültme

ASP.NET Web grubu oturum durumunu depolamak için iki varsayılan seçenekleri sağlar: bir işlem dışı oturum durumu sunucusu çağıran bir oturum durumu sağlayıcısı ve Microsoft SQL Server veritabanında veri depolayan bir oturum durumu sağlayıcısı. Her iki seçeneği bir Web uygulamasının çalışan işlemin dışında durumu bilgilerini depolamak içerdiğinden, uzaktaki depolama birimine gönderilmeden önce serileştirilmesi oturum durumu vardır. Bir geliştirici kavramak kaydeder ne kadar bilgi bağlı olarak serileştirilmiş verilerin boyutu oldukça büyük büyüyebilir.

ASP.NET 4 işlem dışı oturum durumu sağlayıcılarının her iki tür için yeni bir sıkıştırma seçeneği sunar. Zaman *compressionEnabled* aşağıdaki örnekte gösterilen yapılandırma seçeneği ayarlanmış *doğru*, ASP.NET sıkıştırmak (ve genişletmek) .NETFrameworkkullanılarakserileştirilmişoturumdurumu *System.IO.Compression.GZipStream* sınıfı.

[!code-xml[Main](overview/samples/sample9.xml)]

Yeni öznitelik basit ekiyle `Web.config` dosyası, Web sunucularındaki yedek CPU döngülerini uygulamalarla serileştirilmiş oturum durumu verilerinin boyutu önemli azaltması farkında olun.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>İzin verilen URL'ler aralığını genişletme

ASP.NET 4 uygulama URL'leri boyutunu genişletmek için yeni seçenekler sunar. ASP.NET önceki sürümlerini URL yolu uzunlukları 260 karakter, NTFS dosya yolu sınırını tabanlı kısıtlanmış. ASP.NET 4'te, bu sınır, uygulamalarınız için uygun şekilde (artırmak veya azaltmak için) iki yeni kullanma seçeneğiniz *httpRuntime* configuration öznitelikleri. Aşağıdaki örnek, bu yeni öznitelikler gösterir.

[!code-xml[Main](overview/samples/sample10.xml)]

Daha uzun veya kısaysa yollara (protokolü, sunucu adını ve sorgu dizesi dahil değildir URL bölümü) izin verecek şekilde değiştirmek  *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)*  özniteliği. Daha uzun veya kısaysa sorgu dizelerine izin vermek için değeri değiştirmek  *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)*  özniteliği.

ASP.NET 4 URL karakter denetimi tarafından kullanılan karakter yapılandırmanızı sağlar. ASP.NET bir URL yolu bölümünde geçersiz bir karakter bulduğunda, isteği reddeder ve bir HTTP 400 hatası verir. ASP.NET önceki sürümlerinde, sabit bir karakter kümesi için sınırlı URL karakter denetler. ASP.NET 4'te yeni kullanarak geçerli karakter kümesi özelleştirebilirsiniz *requestPathInvalidChars* özniteliği *httpRuntime* yapılandırma öğesi, aşağıdaki örnekte gösterildiği gibi:

[!code-xml[Main](overview/samples/sample11.xml)]

Varsayılan olarak, *requestPathInvalidChars* özniteliği geçersiz olarak sekiz karakter tanımlar. (Atandığı dizesindeki *requestPathInvalidChars* varsayılan*,*değerinden (&lt;), büyüktür (&gt;) ve ve işareti (&amp;) karakterler çünkü kodlanmış `Web.config` dosyasını bir XML dosyasıdır.) Geçersiz karakter kümesi gerektiği gibi özelleştirebilirsiniz.

> [!NOTE]
> ASP.NET 4 olanlar IETF RFC 2396 tanımlanan geçersiz URL karakterler olduğundan için 0x00 0x1F, ASCII aralığında karakterden URL yolu her zaman reddeder Not ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). IIS 6 çalıştıran Windows Server sürümlerinde veya üzeri, http.sys Protokolü aygıt sürücüsü otomatik olarak URL'ler bu karakterlerle reddeder.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Genişletilebilir istek doğrulama

ASP.NET istek doğrulama siteler arası komut dosyası (XSS) saldırıları gelen HTTP istek verileri yaygın olarak kullanılan dizeleri arar. Olası XSS dize bulunursa, istek doğrulama şüpheli dize bayraklar ve bir hata döndürür. Yalnızca XSS saldırılarını kullanılan en yaygın dizeleri bulduğunda yerleşik istek doğrulamayı bir hata döndürür. Çok sayıda hatalı pozitif sonuç XSS doğrulama daha agresif yapmak için önceki denemeleriniz sonuçlandı. Ancak, müşterilere belirli türde istekler veya özel sayfalar için daha katı veya tam tersi bilerek XSS hafifletin isteyebilirsiniz istek doğrulama denetler isteyebilirsiniz.

Özel istek doğrulama mantığını kullanabilmesi için ASP.NET 4'te istek doğrulama özelliği Genişletilebilir olarak yapıldı. İstek doğrulamayı genişletmek için yeni türeyen bir sınıf oluşturun. *System.Web.Util.RequestValidator* türü ve uygulama yapılandırma (içinde *httpRuntime* bölümünü`Web.config`dosya) özel türünü kullanmak için. Aşağıdaki örnek, bir özel talep doğrulama sınıfını Yapılandır gösterilmektedir:

[!code-xml[Main](overview/samples/sample12.xml)]

Yeni *requestValidationType* öznitelik özel istek doğrulama sağlayan sınıf belirten standart .NET Framework türü tanımlayıcısı dizesi gerektirir. Her istek için ASP.NET her parçasının gelen HTTP istek verileri işlemek için özel tür çağırır. Gelen URL, tüm HTTP üstbilgilerin (tanımlama bilgileri ve özel üstbilgi) ve varlık gövdesini, aşağıdaki örnekte gösterildiği gibi bir özel istek doğrulama sınıf tarafından İnceleme için tüm kullanılabilir:

[!code-csharp[Main](overview/samples/sample13.cs)]

Burada istemediğiniz gelen HTTP veri parçası incelemek için durumlarda, istek doğrulama sınıfı ASP.NET varsayılan istek doğrulama çağrısı yaparak çalışması izin vermek için geri dönebilir *temel. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Önbelleğe alma nesne ve önbelleğe alma genişletilebilirlik nesne

İlk sürümünden itibaren güçlü bellekteki nesne önbelleği ASP.NET eklemiştir (*System.Web.Caching.Cache*). Önbellek uygulaması Web olmayan uygulamalarda kullanılmış bu nedenle popüler olmuştur. Ancak, bir başvuru eklemek bir Windows Forms veya WPF uygulaması için alışılmadık `System.Web.dll` ASP.NET nesne önbelleği yalnızca kullanabilmek için.

Önbelleğe alma tüm uygulamalar için kullanılabilir hale getirmek için .NET Framework 4 yeni bir derleme, yeni bir ad alanı, bazı temel türleri ve uygulama önbelleğe alma somut tanıtır. Yeni `System.Runtime.Caching.dll` derleme içeren yeni bir önbellek API *System.Runtime.Caching* ad alanı. Ad alanı iki çekirdek sınıfları içerir:

- Herhangi bir türde özel önbellek uygulaması oluşturmak için temel sağlayan soyut türler.
- Somut bellekteki nesne önbelleği uygulaması ( *System.Runtime.Caching.MemoryCache* sınıfı).

Yeni *MemoryCache* sınıfı Modellenen yakından ASP.NET önbellekte ve ASP.NET ile iç önbellek altyapısı mantığı çoğunu paylaşır. Ancak ortak önbelleğe alma API'lerinde *System.Runtime.Caching* ASP.NET kullandıysanız, geliştirme özel önbelleklerinin desteklemek üzere güncelleştirilmiştir *önbellek* nesnesi, bilinen kavramlarını bulacaksınız yeni API'ler.

Yeni kapsamlı bir açıklama *MemoryCache* sınıf ve taban API'lerini destekleyen tüm belgeyi duyar. Ancak, aşağıdaki örnek yeni önbellek API nasıl çalıştığı hakkında bir fikir verir. Örnek üzerinde hiçbir bağımlılık olmadan bir Windows Forms uygulaması için yazılmıştır `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Genişletilebilir HTML, URL ve HTTP Kodlama üstbilgisi

ASP.NET 4'te, metin kodlama aşağıdaki ortak görevler için özel kodlama yordamları oluşturabilirsiniz:

- HTML kodlaması.
- URL kodlaması.
- HTML öznitelik kodlaması.
- Giden HTTP üstbilgileri kodlama.

Özel bir kodlayıcı yeni türetme oluşturabileceğiniz *System.Web.Util.HttpEncoder* özel türünde kullanılacak türü ve ASP.NET yapılandırma *httpRuntime* bölümünü `Web.config` dosyası olarak Aşağıdaki örnekte gösterilen:

[!code-xml[Main](overview/samples/sample15.xml)]

Özel bir kodlayıcı yapılandırıldıktan sonra ASP.NET özel kodlama uygulama ortak her yöntemlerini kodlama otomatik olarak çağırır. *System.Web.HttpUtility* veya *System.Web.HttpServerUtility* sınıfları çağrılır. Bu, bir Web geliştirme ekibi bir parçası ve Web geliştirme ekibi rest API'leri kodlama ortak ASP.NET kullanmak devam ederken agresif karakter kodlama uygulayan özel bir kodlayıcı oluşturma sağlar. Bir özel Kodlayıcısı merkezi olarak yapılandırarak *httpRuntime* öğesi garanti API'leri kodlama ortak ASP.NET gelen tüm metin kodlaması çağrıları özel Encoder'da yönlendirilir.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Tek çalışan işlemindeki tek tek uygulamalar için performans izleme

Tek bir sunucu üzerinde barındırılan Web siteleri sayısını artırmak için bir tek çalışan işlemde birden çok ASP.NET uygulaması birçok barındırıcıların çalıştırın. Ancak, birden çok uygulama tek paylaşılan çalışan işlemi kullanırsanız, sorunlar yaşıyorsanız tek bir uygulamayı tanımlamak sunucu yöneticileri için zor olabilir.

ASP.NET 4 CLR tarafından sunulan yeni kaynak izleme işlevselliği yararlanır. Bu işlevselliği etkinleştirmek için aşağıdaki XML yapılandırma parçacığını ekleyebilirsiniz `aspnet.config` yapılandırma dosyası.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Not `aspnet.config` .NET Framework yüklü olduğu dizinde bir dosyadır. Değil `Web.config` dosya.


Zaman *appDomainResourceMonitoring* özelliği etkin, iki yeni performans sayaçları "ASP.NET uygulamaları" performans kategorisinde kullanılabilir: *% işlemci zamanı yönetilen* ve  *Kullanılan bellek yönetilen*. Bu performans sayaçlarını her ikisi de tahmini CPU süresi ve tek tek ASP.NET uygulamaları yönetilen bellek kullanımını izlemek için yeni CLR uygulama etki alanı kaynak yönetimi özelliğini kullanın. Sonuç olarak, ASP.NET 4 ile yöneticiler artık tek çalışan işleminde çalışan ayrı ayrı uygulamaların kaynak tüketimini daha ayrıntılı bir görünüme var.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Çoklu Sürüm Desteği

Belirli bir .NET Framework sürümünü hedefleyen bir uygulama oluşturabilirsiniz. ASP.NET 4, yeni bir öznitelik *derleme* öğesinin `Web.config` dosyası .NET Framework 4 ve üzeri hedefleyin imkan tanır. .NET Framework 4 açıkça hedefliyorsanız ve isteğe bağlı öğeleri dahil ederseniz `Web.config` dosyası girişlerinde gibi *system.codedom*, bu öğeler için .NET Framework 4 doğru olması gerekir. (.NET Framework 4 açıkça oluşturmayın, hedef Framework'ü bir girişe eksikliği gelen algılanır `Web.config` dosyası.)

Aşağıdaki örnek kullanımı gösterilmiştir *targetFramework* özniteliğini *derleme* öğesinin `Web.config` dosya.

[!code-xml[Main](overview/samples/sample17.xml)]

Belirli bir .NET Framework sürümünü hedefleme hakkında aşağıdakileri unutmayın:

- Bir .NET Framework 4 uygulama havuzunda ASP.NET yapılandırma sistemi .NET Framework 4 hedef olarak varsayar `Web.config` dosya içermez *targetFramework* özniteliği veya `Web.config` dosyası eksik. (.NET Framework 4 altında çalışmasını sağlamak için uygulamanızın kodlama değişiklik gerekebilir.)
- Eklerseniz *targetFramework* özniteliği ve *system.codeDom* öğesi tanımlanmış `Web.config` dosyası, bu dosya için .NET Framework 4 doğru girişleri içermelidir.
- Kullanıyorsanız *aspnet\_derleyici* (yapı ortamı olduğu gibi), uygulamanızın derleneceği komutu, doğru sürümünü kullanmanız gerekir *aspnet\_derleyici* Hedef Framework'ü komutu. Gönderilen derleyici (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) .NET Framework 2.0 ile .NET Framework 3.5 ve önceki sürümler için derlemek için kullanın. Bu framework veya sonraki sürümleri kullanılarak oluşturulan uygulamaların derlemek için .NET Framework 4 ile birlikte gelen derleyici kullanın.
- Çalışma zamanında derleyici bilgisayarda yüklü olan son framework derlemeleri kullanır (ve bu nedenle GAC içinde). Bir güncelleştirmeyi daha sonra framework yaptıysanız (örneğin, bir kuramsal sürümü 4.1 yüklü), rağmen daha yeni sürümüne framework özelliklerini kullanabilmek için siz *targetFramework* özniteliği hedefleyen daha düşük bir sürümü (4.0 gibi). (Ancak, tasarım zamanında Visual Studio 2010 veya kullandığınızda *aspnet\_derleyici* komutunu framework'ün daha yeni özellikleri kullanma derleyici hataları neden olur).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>AJAX

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery Web Forms ve MVC dahil

Web Forms ve MVC için Visual Studio şablonları açık kaynak jQuery kitaplığı içerir. Yeni Web sitesi veya proje oluşturduğunuzda, aşağıdaki 3 dosyalarını içeren bir komut dosyaları klasörü oluşturulur:

- jQuery 1.4.1.js – okunabilir, jQuery kitaplığı sürümü unminified.
- jQuery-14.1.min.js – jQuery kitaplığının küçültülmüş sürümü.
- jQuery-1.4.1-vsdoc.js – jQuery kitaplığı için IntelliSense belge dosyası.

Bir uygulama geliştirirken jQuery unminified sürümünü içerir. Üretim uygulamaları için jQuery küçültülmüş sürümünü içerir.

Örneğin, aşağıdaki Web Forms sayfası ASP.NET TextBox denetimi odağa sahip olduğunda sarı arka plan rengini değiştirmek için jQuery nasıl kullanabileceğiniz gösterilmektedir.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>İçerik teslim ağı desteği

Microsoft Ajax içerik teslim ağı (CDN), ASP.NET Ajax ve jQuery betikleri, Web uygulamalarınızı kolayca eklemenize olanak tanır. Örneğin, jQuery kitaplığı basitçe ekleyerek kullanmaya başlayabilmeniz için bir `<script>` Ajax.microsoft.com için şöyle işaret sayfanıza etiketi:

[!code-html[Main](overview/samples/sample19.html)]

Microsoft Ajax CDN yararlanarak, Ajax uygulamalarınızın performansını önemli ölçüde artırabilir. Microsoft Ajax CDN içeriğini tüm dünyada bulunan sunucuları önbelleğe alınır. Ayrıca, Microsoft Ajax CDN farklı etki alanlarında bulunan Web siteleri için önbelleğe alınmış JavaScript dosyalarını yeniden tarayıcılar sağlar.

Güvenli Yuva Katmanı'ni kullanarak bir web sayfası hizmet gerekebileceği Microsoft Ajax içerik teslim ağı SSL (HTTPS) destekler.

Microsoft Ajax CDN hakkında daha fazla bilgi için aşağıdaki Web sitesini ziyaret edin:

[https://www.ASP.NET/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ASP.NET ScriptManager Microsoft Ajax CDN destekler. Yalnızca ayarı bir özelliği tarafından EnableCdn, CDN tüm ASP.NET framework JavaScript dosyaları alabilirsiniz:

[!code-aspx[Main](overview/samples/sample20.aspx)]

EnableCdn özellik değeri true olarak ayarladıktan sonra ASP.NET çerçevesini doğrulama ve UpdatePanel için kullanılan tüm JavaScript dosyaları dahil olmak üzere CDN tüm ASP.NET framework JavaScript dosyaları alır. Bir bu özelliği ayarlamak, web uygulamanızın performansı üzerinde önemli bir etkisi olabilir.

Web kaynağı özniteliğini kullanarak kendi JavaScript dosyaları için CDN yolunu ayarlayabilirsiniz. Yeni CdnPath özellik değeri true olarak EnableCdn özelliğini ayarlarken kullanılan CDN yolunu belirtir:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>ScriptManager açık komut dosyaları

ASP.NET ScriptManger kullandıysanız geçmişte, ardından, tüm tek yapılı ASP.NET Ajax kitaplığı yüklemek için gerekli olmuştur. Yeni ScriptManager.AjaxFrameworkMode özelliğinin yararlanarak, tam olarak hangi bileşenler ASP.NET Ajax kitaplığı yükleneceğini denetlemek ve yalnızca gereksinim duyduğunuz ASP.NET Ajax kitaplığı bileşenlerini yükleyin.

ScriptManager.AjaxFrameworkMode özelliği aşağıdaki değerlere ayarlanabilir:

- Etkin--ScriptManager denetimi otomatik olarak her çekirdek framework komut (eski davranış) birleşik komut dosyasıdır MicrosoftAjax.js komut dosyasını içerdiğini belirtir.
- Devre dışı--tüm Microsoft Ajax komut dosyası özellikleri devre dışı bırakılır ve ScriptManager denetimi herhangi bir komut dosyası otomatik olarak başvurmuyor olduğunu belirtir.
- Açık--sayfanızı gerektiren tek tek framework çekirdek komut dosyası komut dosyası başvuruları açıkça dahil edilir ve her komut dosyası gerektirir bağımlılıkları başvurular içereceğini belirtir.

Örneğin, AjaxFrameworkMode özelliğini Explicit değerine ayarlarsanız, gereken belirli ASP.NET Ajax bileşen komut dosyalarını belirtebilirsiniz:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

Web Forms, ASP.NET core özelliğinde ASP.NET 1.0 sürümünden bu yana bırakıldı. Birçok iyileştirme, ASP.NET 4 aşağıdakiler de dahil olmak üzere, bu alanda bırakıldı:

- Ayarlamanıza olanak *meta* etiketler.
- Görünüm durumu üzerinde daha fazla denetim.
- Tarayıcı yetenekleri ile çalışmak için daha kolay yolları.
- ASP.NET Web Forms yönlendirme kullanma desteği.
- Oluşturulan kimlikleri üzerinde daha fazla denetim.
- Veri denetimleri seçilen satır kalıcı yeteneği.
- Oluşturulan HTML'de üzerinde daha fazla denetim *FormView* ve *ListView* kontrol eder.
- Veri kaynağı denetimleri filtreleme desteği.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Meta etiketleri Page.MetaKeywords ve Page.MetaDescription özelliklerini ayarlama

ASP.NET 4 ekler için iki özellik *sayfa* sınıfı, *MetaKeywords* ve *MetaDescription*. Bu iki özellik temsil karşılık gelen *meta* aşağıdaki örnekte gösterildiği gibi sayfasında etiketler:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Bu iki özellik aynı çalışma biçimi sayfanın *başlık* özelliği yapar. Bunlar, bu kurallar izleyin:

1. Varsa hiçbir *meta* bulunan etiketleri *head* özellik adları eşleşen öğe (diğer bir deyişle, ad = "anahtar sözcükler" için *Page.MetaKeywords* ve ad = "Açıklama" için *Page.MetaDescription*, yani bu özellikleri ayarlanmamış), *meta* etiketleri işlendiğinde sayfasına eklenir.
2. Varsa *meta* bu adı taşıyan etiketler, bu özellikleri hareket almak ve ayarlamak için varolan etiketleri içeriğini yöntemleri olarak.

Bu özellikleri olanak sağlayan, bir veritabanı veya başka bir kaynağından içerik almak ve olanak sağlayan dinamik olarak ne tanımlamak için etiketleri ayarlamanıza çalışma zamanında ayarlayabilirsiniz belirli bir sayfa içindir.

Seçeneğini de ayarlayabilirsiniz *anahtar sözcükleri* ve *açıklama* özelliklerinde *@page* Web Forms sayfası biçimlendirme, aşağıdaki örnekte olduğu gibi üstündeki yönerge:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Bu geçersiz kılar *meta* etiketi sayfada zaten bildirilen içeriği (varsa).

Açıklama içeriğini *meta* etiketi, Google önizlemelerde listeleme aramasını geliştirme için kullanılır. (Ayrıntılar için bkz [meta açıklama yeniliği ile parçacıkları artırmak](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) Google yayımlanması merkezi blogunda.) Google ve Windows Live arama anahtar sözcükleri içeriğini için herhangi bir şey kullanmaz, ancak diğer arama motorları olabilir. Daha fazla bilgi için bkz: [Meta Anahtar sözcükler öneriler](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) arama motoru Kılavuzu Web sitesinde.

Bu yeni özellikler basit bir özellik olan, ancak bunlar el ile ekleme gereksiniminden veya oluşturmak için kendi kodunuzu yazma bunlar kaydettiğiniz *meta* etiketler.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Görünüm durumu için ayrı ayrı denetimler etkinleştirme

Varsayılan olarak, bu uygulama için gerekli olsa bile sayfasında her denetim görünüm durumu potansiyel olarak depolar sonucunda sayfa için Görünüm durumunun etkin. Görünüm durumu verilerini biçimlendirme içinde bir sayfa oluşturur ve bir sayfa istemciye göndermek ve geri post için geçen süreyi artırır dahil edilir. Gerekli olandan daha fazla görünüm durumu depolamak üzere önemli performans düşüşüne neden olabilir. ASP.NET önceki sürümlerinde, geliştiricilerin sayfa boyutunu azaltmak için ayrı ayrı denetimler için görünüm durumuna devre dışı bırakılamadı, ancak bu nedenle açıkça bireysel denetimleri için yapmak zorunda. ASP.NET 4'te Web sunucusu denetimleri içeren bir *ViewStateMode* görünüm durumu varsayılan olarak devre dışı bırakma ve sayfanın gerektiren denetimleri için etkinleştirme olanak tanıyan özellik.

*ViewStateMode* özelliği üç değerlerine sahip bir numaralandırmasını alır: *etkin*, *devre dışı*, ve *devral*. *Etkin* görünüm durumunu ayarlamak için tüm alt denetimleri ve bu denetim için etkinleştirir *devral* veya hiçbir şey ayarlanmış olması gerekir. *Devre dışı* görünüm durumu, devre dışı bırakır ve *devral* denetim kullandığını belirtir *ViewStateMode* üst denetimini ayarlama.

Aşağıdaki örnekte gösterildiği nasıl *ViewStateMode* özelliği çalışır. Biçimlendirme ve kodun denetimleri için aşağıdaki sayfayı içerir değerlerini *ViewStateMode* özelliği:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Gördüğünüz gibi PlaceHolder1 denetimi için görünüm durumuna kod devre dışı bırakır. Bu özellik değeri alt label1 denetim devralır (*devral* için varsayılan değer *ViewStateMode* denetimler için) ve bu nedenle hiçbir görünüm durumunu kaydeder. PlaceHolder2 denetiminde *ViewStateMode* ayarlanır *etkin*, böylece label2 bu özellik devralır ve kaydeder durumunu görüntüleyin. Sayfa ilk kez yüklendiğinde *metin* özelliği hem *etiket* denetimleri "[DynamicValue]" dizeye ayarlayın.

Bu ayarları etkisini sayfa ilk kez yüklendiğinde, aşağıdaki çıkış tarayıcıda görüntülenen şöyledir:

Devre dışı`: [DynamicValue]`

Etkin:`[DynamicValue]`

Bir geri gönderme sonra ancak, aşağıdaki çıkış görüntülenir:

Devre dışı`: [DeclaredValue]`

Etkin:`[DynamicValue]`

Label1 denetimi (olan *ViewStateMode* değeri ayarı *devre dışı*) kodda ayarlandığı değeri muhafaza edilmesini değil. Ancak, label2 denetlemek (olan *ViewStateMode* değeri ayarı *etkin*) durumu korunur.

Ayrıca ayarlayabilirsiniz *ViewStateMode* içinde *@page* yönerge aşağıdaki örnekteki gibi:

[!code-aspx[Main](overview/samples/sample26.aspx)]

*Sayfa* sınıfın yalnızca başka bir denetim; diğer denetimleri için sayfanın üst denetim gibi davranır. Varsayılan değer olan *ViewStateMode* olan *etkin* için örneklerini *sayfa*. Denetimler için varsayılan çünkü *devral*, denetimler devralır *etkin* özellik değerini ayarladığınız sürece *ViewStateMode* sayfasının veya denetiminin düzeyinde.

Değeri *ViewStateMode* özelliği belirler görünüm durumu yalnızca korunur, *EnableViewState* özelliği ayarlanmış *doğru*. Varsa *EnableViewState* özelliği ayarlanmış *false*, Görünüm durumu tutulmaz olsa bile *ViewStateMode* ayarlanır *etkin*.

Bu özellik için iyi bir kullanımını olduğu *ContentPlaceHolder* ayarlayabileceğiniz ana sayfalar denetimlerinde *ViewStateMode* için *devre dışı* Yöneticisi için sayfa ve etkinleştirin tek tek için *ContentPlaceHolder* sırayla gerektiren denetimleri içeren denetimler durumunu görüntüleyin.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Tarayıcı yetenekleri yapılan değişiklikler

ASP.NET belirleyen bir kullanıcı olarak adlandırılan bir özellik kullanarak sitenize göz atmak için kullanarak tarayıcının yeteneklerini *tarayıcı yetenekleri*. Tarayıcı yetenekleri temsil ettiği *HttpBrowserCapabilities* nesne (tarafından sunulan *Request.Browser* özelliği). Örneğin, kullanabileceğiniz *HttpBrowserCapabilities* nesne türü ve geçerli tarayıcı sürümü JavaScript belirli bir sürümü destekleyip desteklemediğini belirlemek için. Veya, kullanabileceğiniz *HttpBrowserCapabilities* isteği bir mobil aygıttan geldiğini olup olmadığını belirlemek için nesne.

*HttpBrowserCapabilities* nesne tarayıcı tanım dosyalarını bir dizi tarafından yönlendirilen. Bu dosyalar belirli tarayıcılar özellikleri hakkında bilgi içerir. ASP.NET 4'te bu tarayıcı tanım dosyalarını yakın zamanda sunulan tarayıcılar ve hareket BlackBerry akıllı telefonlar ve Apple iPhone Google Chrome, araştırma gibi cihazlar hakkında bilgi içerecek şekilde güncelleştirildi.

Aşağıdaki liste, yeni tarayıcı tanım dosyalarını gösterir:

- *BlackBerry.browser*
- *Chrome.browser*
- *Default.browser*
- *Firefox.browser*
- *Gateway.browser*
- *Generic.browser*
- *IE.browser*
- *iemobile.browser*
- *iPhone.browser*
- *Opera.browser*
- *Safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Tarayıcı yetenekleri sağlayıcıları kullanma

ASP.NET sürüm 3.5 Service Pack 1, aşağıdaki yollarla bir tarayıcısı olan özellikleri tanımlayabilirsiniz:

- Bilgisayar düzeyinde oluştur veya güncelleştir bir `.browser` aşağıdaki klasörde bulunan XML dosyası:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Tarayıcı yeteneğini tanımladıktan sonra gelen Visual Studio komut tarayıcı yetenekleri derlemesi yeniden oluşturun ve GAC'ye eklemek için istemi aşağıdaki komutu çalıştırın:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Tek bir uygulama için oluşturduğunuz bir `.browser` uygulamanın dosyasında `App_Browsers` klasör.

Bu yaklaşım, XML dosyalarını değiştirmek gerektirir ve bilgisayar düzeyinde değişikliklerinde aspnet çalıştırdıktan sonra uygulamayı yeniden başlatmalısınız\_regbrowsers.exe işlemi.

ASP.NET 4 olarak adlandırılan bir özellik içerir *tarayıcı yetenekleri sağlayıcıları*. Adı da anlaşılacağı gibi sırayla olanak sağlayan bir sağlayıcı yapı bu sağlar kendi kodunuzu tarayıcı yetenekleri belirlemek için kullanın.

Uygulamada, geliştiriciler genellikle özel tarayıcı yetenekleri tanımlamayın. Tarayıcı dosyaları güncelleştirmek, işlem oldukça karmaşık güncelleştiren için sabit olan ve XML sözdizimi `.browser` dosyaları kullanın ve tanımlamak için karmaşık olabilir. Ne bu işlem çok daha kolay hale getirir, bir ortak tarayıcı tanımı sözdizimi olsaydı veya güncel tarayıcı tanımları veya bir Web hizmeti gibi bir veritabanı için bile bulunan veritabanı bir değil. Yeni tarayıcı yetenekleri sağlayıcıları özelliği bu senaryoları üçüncü taraf geliştiriciler için olası ve pratik hale getirir.

Yeni ASP.NET 4 tarayıcı yetenekleri sağlayıcısı özelliğini kullanmak için iki ana yaklaşım vardır: ASP.NET tarayıcı yetenekleri tanımı işlevselliği genişletmek veya tamamen değiştirme. Aşağıdaki bölümlerde ilk işlevselliği değiştirme ve genişletmek nasıl açıklanmaktadır.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>ASP.NET tarayıcı yetenekleri işlevselliğini değiştirme

ASP.NET tarayıcı yetenekleri tanımı işlevselliğini tamamen değiştirmek için aşağıdaki adımları izleyin:

1. Türetilen bir sağlayıcı sınıfı oluşturmak *HttpCapabilitiesProvider* ve, geçersiz kılmalar *GetBrowserCapabilities* aşağıdaki örnekteki gibi yöntemi: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Bu örnekteki kod yeni bir oluşturur *HttpBrowserCapabilities* nesne, yalnızca tarayıcı ile MyCustomBrowser için bu özelliği ayarlama adlı yetenek belirtme.
2. Sağlayıcıyı uygulama ile kaydedin. 

    Sağlayıcı bir uygulama ile kullanmak için eklemelisiniz *sağlayıcı* özniteliğini *browserCaps* bölümüne `Web.config` veya `Machine.config` dosyaları. (Sağlayıcı öznitelikleri de tanımlayabilirsiniz bir *konumu* öğesi uygulamada, belirli bir mobil aygıt için bir klasör gibi belirli dizinler için.) Aşağıdaki örnekte nasıl ayarlanacağını gösterir *sağlayıcı* özniteliği bir yapılandırma dosyası:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Yeni bir tarayıcı özelliği tanımı kaydetmek için başka bir yolu kod, aşağıdaki örnekte gösterildiği gibi kullanmaktır:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Bu kod çalıştırması gerekir *uygulama\_Başlat* olayı `Global.asax` dosya. Değiştirmek için *BrowserCapabilitiesProvider* sınıfı gerekir ortaya önbellek çözümlenmiş için geçerli bir durumda kaldığından emin olmak için tüm uygulama kodunda yürütülmeden önce *HttpCapabilitiesBase* nesnesi.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>HttpBrowserCapabilities nesne önbelleğe alma

Yukarıdaki örnekteki kod özel sağlayıcı çağrılır alabilmek için her zaman çalışır olduğundan bir sorun olduğu *HttpBrowserCapabilities* nesnesi. Bu, her isteği sırasında birden çok kez meydana gelebilir. Örnekte, sağlayıcı için kod çok yapmaz. Özel sağlayıcınızın kodunda önemli iş gerçekleştirirse ancak almak için sipariş *HttpBrowserCapabilities* nesnesi, bu performansı etkileyebilir. Bunun olmaması için önbelleğe alabilir *HttpBrowserCapabilities* nesnesi. Aşağıdaki adımları uygulayın:

1. Türeyen bir sınıf oluşturun *HttpCapabilitiesProvider*, aşağıdaki örnekte bir ister: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Örnekte, bir özel BuildCacheKey yöntemini çağırarak bir önbellek anahtarını kod oluşturur ve özel bir GetCacheTime yöntemini çağırarak süre önbelleğe alır. Kod sonra çözümlenen ekler *HttpBrowserCapabilities* önbelleğe nesne. Nesneyi önbellekten ve üzerinde yeniden olun sonraki istekleri özel sağlayıcı kullanın.
2. Sağlayıcıyı uygulama ile önceki yordamda açıklandığı şekilde kaydedin.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>ASP.NET tarayıcı yetenekleri işlevini genişletme

Yeni bir oluşturma önceki bölümde açıklanan *HttpBrowserCapabilities* ASP.NET 4'te nesnesi. Yeni tarayıcı yetenekleri tanımları zaten ASP.NET olanlar ekleyerek, ASP.NET tarayıcı yetenekleri işlevselliği de genişletebilirsiniz. XML tarayıcı tanımları kullanmadan bunu yapabilirsiniz. Aşağıdaki yordamda gösterildiği nasıl.

1. Türeyen bir sınıf oluşturun *HttpCapabilitiesEvaluator* ve, geçersiz kılmalar *GetBrowserCapabilities* yöntemi, aşağıdaki örnekte gösterildiği gibi: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Bu kod, ASP.NET tarayıcı yetenekleri işlevi ilk tarayıcı belirlemeye kullanır. Hiçbir tarayıcı belirlediyseniz, ancak istekte tanımlanan bilgilere dayanarak (varsa, diğer bir deyişle, *tarayıcı* özelliği *HttpBrowserCapabilities* nesnesidir "Bilinmiyor" dizesi), kod çağrıları Özel sağlayıcı tarayıcıyı tanımlamak için (MyBrowserCapabilitiesEvaluator).
2. Sağlayıcı, önceki örnekte açıklandığı gibi uygulama ile kaydedin.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Var olan özellikleri tanımları için yeni özellikler ekleyerek tarayıcı yetenekleri işlevini genişletme

Özel tarayıcı tanımı sağlayıcısı oluşturma yanı sıra ve dinamik olarak yeni tarayıcı tanımları oluşturmak için varolan tarayıcı tanımları ek özellikler ile genişletebilirsiniz. Bu, istediğiniz yakın olan ancak yalnızca birkaç özellik eksik bir tanımı kullanmanıza olanak sağlar. Bunu yapmak için aşağıdaki adımları kullanın.

1. Türeyen bir sınıf oluşturun *HttpCapabilitiesEvaluator* ve, geçersiz kılmalar *GetBrowserCapabilities* yöntemi, aşağıdaki örnekte gösterildiği gibi: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Kod örneği mevcut ASP.NET genişletir *HttpCapabilitiesEvaluator* sınıfı ve alır *HttpBrowserCapabilities* aşağıdaki kodu kullanarak geçerli istek tanımının eşleşen nesnesi :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Kod eklemek veya bu tarayıcı için bir özelliği değiştirmek için. Yeni bir tarayıcı özelliği belirtmek için iki yolu vardır:

    - Bir anahtar/değer çifti Ekle *IDictionary* tarafından sunulan nesnenin *yetenekleri* özelliği *HttpCapabilitiesBase* nesnesi. Önceki örnekte, kod MultiTouch değeri olan adlı bir özellik ekler *doğru*.
    - Mevcut özellikleri ayarlayın *HttpCapabilitiesBase* nesnesi. Önceki örnekte, kod ayarlar *çerçeveler* özelliğine *doğru*. Bu özellik yalnızca için bir erişimci olan *IDictionary* tarafından sunulan nesnenin *yetenekleri* özelliği. 

        > [!NOTE]
        > Bu model, herhangi bir özelliği için geçerli unutmayın *HttpBrowserCapabilities*, denetim bağdaştırıcıları dahil olmak üzere.
2. Sağlayıcıyı uygulama ile önceki yordamda açıklandığı şekilde kaydedin.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>ASP.NET 4 yönlendirme

ASP.NET 4 Web Forms yönlendirme kullanarak için yerleşik destek ekler. Kabul etmek için bir uygulama yapılandırmanızı Yönlendirme sağlar fiziksel dosyalarına eşleme değil URL'leri isteyin. Bunun yerine, kullanıcılara anlamlı ve uygulamanız için arama motoru iyileştirme (SEO) yardımcı olabilir URL'leri tanımlamak için yönlendirme kullanabilirsiniz. Örneğin, var olan bir uygulamada ürün kategorileri görüntüleyen sayfanın URL'sini aşağıdaki gibi görünebilir:

[!code-console[Main](overview/samples/sample36.cmd)]

Yönlendirme kullanarak, aynı bilgileri işlemek için şu URL'yi kabul etmek için uygulama yapılandırabilirsiniz:

[!code-console[Main](overview/samples/sample37.cmd)]

Yönlendirme ASP.NET 3.5 SP1'den itibaren edinilebilir. (Giriş ASP.NET 3.5 SP1'de Yönlendirme kullanma örneği için bkz: [kullanarak yönlendirme ile WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "bu girişi başlığı.") Phil Haack'ın blogunda.) Ancak, ASP.NET 4, aşağıdakiler de dahil olmak üzere daha kolay yönlendirmeyi, kullanmak için bazı özellikleri içerir:

- *PageRouteHandler* yollar tanımlarken kullanan basit bir HTTP işleyicisi sınıfı. Sınıf verileri isteği yönlendirilir sayfasına geçirir.
- Yeni özellikleri *HttpRequest.RequestContext* ve *Page.RouteData* (bir proxy olduğu *HttpRequest.RequestContext.RouteData* nesnesi). Bu özellikleri rotadaki geçirilen bilgilere erişmek için kolaylaştırır.
- İçinde tanımlanan aşağıdaki yeni ifade oluşturucularının *System.Web.Compilation.RouteUrlExpressionBuilder* ve *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, bir ASP.NET sunucu denetimi içinde bir rota URL'ye karşılık gelen bir URL oluşturmak için basit bir yol sağlar.
- *RouteValue*, bilgileri ayıklamak için basit bir yol sağlayan *RouteContext* nesnesi.
- *RouteParameter* bulunan verileri geçirmek kolaylaştırır sınıfı bir *RouteContext* bir sorgu için veri kaynağı denetimi nesnesine (benzer şekilde [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Web formları sayfaları için yönlendirme

Aşağıdaki örnek yeni kullanarak bir Web Forms yol tanımlanacağı gösterilmektedir *MapPageRoute* yöntemi *rota* sınıfı:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 tanıtır *MapPageRoute* yöntemi. Aşağıdaki örnek, önceki örnekte gösterilen SearchRoute tanımı eşdeğerdir ancak kullanan *PageRouteHandler* sınıfı.

[!code-csharp[Main](overview/samples/sample39.cs)]

Örnek kodda bir fiziksel sayfasına rotayı eşler (ilk rotadaki için `~/search.aspx`). İlk yol tanımını da searchterm adlı parametre URL'den ayıklanan ve sayfasına geçirilen belirtir.

*MapPageRoute* yöntemi aşağıdaki yöntemi aşırı yüklemeleri destekler:

- *MapPageRoute (dize Routetablename, dize routeUrl, dize physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (dize Routetablename, dize routeUrl, dize physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary varsayılanlar)*
- *MapPageRoute (dize Routetablename, dize routeUrl, dize physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary Varsayılanları, RouteValueDictionary kısıtlamaları)*

*CheckPhysicalUrlAccess* parametresi, rota yönlendiriliyor fiziksel sayfa güvenlik izinlerini denetleyip denetlemeyeceğini belirtir (Bu durumda, search.aspx) ve gelen URL izinlerini (Bu durumda, arama / {searchterm}). Varsa değerini *checkPhysicalUrlAccess* olan *yanlış*, yalnızca gelen URL izinleri denetlenir. Bu izinleri tanımlanan `Web.config` aşağıdaki gibi ayarları kullanarak dosya:

[!code-xml[Main](overview/samples/sample40.xml)]

Örnek yapılandırma fiziksel sayfasına erişim reddedildi `search.aspx` yönetici roldeki olanlar dışındaki tüm kullanıcılar için. Zaman *checkPhysicalUrlAccess* parametrenin ayarlanmış *true* (varsayılan değeri olduğu), yalnızca yönetici kullanıcılar URL /search/ {searchterm} erişmesine izin verilir fiziksel sayfa search.aspx olduğundan Bu roldeki kullanıcılar için kısıtlı. Varsa *checkPhysicalUrlAccess* ayarlanır *false* ve site önceki örnekte gösterildiği gibi yapılandırılmışsa, tüm kimliği doğrulanmış kullanıcılara URL /search/ {searchterm} erişmesine izin verilir.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Bir Web Forms sayfasında yönlendirme bilgilerini okuma

Web Forms fiziksel sayfa kodunda yönlendirme URL'den ayıklanan bilgilere erişebilirsiniz (veya başka bir nesne eklenmiş diğer bilgi *RouteData* nesnesi) iki yeni özellikleri kullanarak:  *HttpRequest.RequestContext* ve *Page.RouteData*. (*Page.RouteData* sarmalar *HttpRequest.RequestContext.RouteData*.) Aşağıdaki örnekte nasıl kullanılacağını gösterir *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Kod örnek rotadaki daha önce tanımlanan searchterm parametresi için geçirilen değer ayıklar. Aşağıdaki istek URL'si göz önünde bulundurun:

[!code-console[Main](overview/samples/sample42.cmd)]

Ne zaman bu istek yapıldığında, "tan" çizilir, word `search.aspx` sayfası.

#### <a name="accessing-routing-information-in-markup"></a>Biçimlendirmede yönlendirme bilgilerine erişme

Önceki bölümde açıklanan yöntemde, Web Forms sayfası kodda rota verilerini almak gösterilmiştir. Aynı bilgilere erişmenizi biçimlendirme ifadelerde de kullanabilirsiniz. İfade oluşturuculara tanımlayıcı kod ile çalışmak için güçlü ve zarif bir yoludur. (Daha fazla bilgi için bkz: Giriş [Express kendiniz ile özel ifade oluşturuculara](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) Phil Haack'ın blogunda.)

ASP.NET 4 Web Forms yönlendirme için iki yeni ifade oluşturuculara içerir. Aşağıdaki örnek, bunların nasıl kullanılacağını gösterir.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Örnekte, *RouteUrl* ifadesi, bir rota parametresi temelinde bir URL tanımlamak için kullanılır. Bu kod sabit işaretleme içine tam URL gereğini ortadan kaldırır ve URL yapısı, daha sonra bu bağlantı için herhangi bir değişiklik gerektirmeden değiştirmenize olanak tanır.

Daha önce tanımlanan yola göre bu biçimlendirme aşağıdaki URL'yi oluşturur:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET doğru yol otomatik olarak çalışır (diğer bir deyişle, doğru URL'yi oluşturur) giriş parametrelerine göre. Bir rota adı kullanmak için bir yol belirtmenize imkan tanıyan ifadesinde de içerebilir.

Aşağıdaki örnekte nasıl kullanılacağını gösterir *RouteValue* ifade.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Bu denetim içerir sayfa çalıştığında değeri "tan" etiketi görüntülenir.

*RouteValue* ifadesi, kolaylaştırır rota verileri biçimlendirme kullanın ve daha karmaşık Page.RouteData["x ile çalışmak zorunda önler"] söz dizimi biçimlendirme.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Rota verileri için veri kaynağı denetim parametrelerini kullanarak

*RouteParameter* sınıfı bir veri kaynağı denetimi sorguları için parametre değeri olarak rota verileri belirtmenize olanak sağlar. Bu [works çok benzer şekilde](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) , aşağıdaki örnekte gösterildiği gibi sınıfı:

[!code-aspx[Main](overview/samples/sample46.aspx)]

Bu durumda, rota parametresi searchterm değeri için kullanılacak olan @companyname parametresinde *seçin* deyimi.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>İstemci kimlikleri ayarlama

Yeni *ClientIDMode* özelliği ASP.NET artırmanın uzun süredir bilinen bir sorunu giderir öğesine denetimleri nasıl oluşturacağınızı *kimliği* işlenen öğelerin özniteliği. Bilerek *kimliği* işlenmiş öğelerin özniteliktir uygulamanız bu öğeleri başvuran istemci komut dosyası içeriyorsa, önemli.

*Kimliği* Web sunucusu denetimleri için işlenen HTML özniteliğinde göre oluşturulur *ClientID* denetiminin özelliği. ASP.NET 4, oluşturmak için algoritma kadar *kimliği* özniteliğini *ClientID* özelliği adlandırma kapsayıcısı (varsa) Kimliğine sahip ve yinelenen denetimlerinde (olarak söz konusu olduğunda birleştirmek için açıldı bir önek ve sıralı numara eklemek için veri denetimleri). Bu her zaman sayfasındaki denetimleri kimliklerinin benzersiz garanti olsa da, Denetim tahmin edilebilir değil ve bu nedenle istemci komut dosyası başvurusunda zor kimlikleri algoritmayı sonuçlandı.

Yeni *ClientIDMode* özelliği daha kesin olarak istemci kimliği denetimler için nasıl oluşturulur belirtmenize olanak sağlar. Ayarlayabileceğiniz *ClientIDMode* sayfa için dahil olmak üzere herhangi bir denetimi özelliği. Olası ayarlar şunlardır:

- *AutoID* – bu oluşturmak için algoritma eşdeğerdir *ClientID* ASP.NET önceki sürümlerinde kullanılan özellik değerleri.
- *Statik* – bu belirleyen *ClientID* değerler kapsayıcı adlandırma üst kimliklerini birleştirme olmadan kimliği ile aynı olur. Bu, Web kullanıcı denetimlerinde yararlı olabilir. Bir Web kullanıcı denetimi farklı sayfalarında ve farklı kapsayıcı denetimleri bulunan olabileceğinden, istemci komut dosyası kullanan denetimler için yazmak zor olabilir *AutoID* algoritma kimliği değerleri ne olacağını tahmin edilemez çünkü .
- *Tahmin edilebilir* – yinelenen şablonlarını kullanma veri denetimleri kullanmak için öncelikle bu seçenektir. Oluşturulan ancak denetimin adlandırma kapsayıcıları kimliği özelliklerini art arda ekler *ClientID* değerleri dizeler "ctlxxx" gibi içermez. Bu ayarı ile birlikte çalışır *ClientIDRowSuffix* denetiminin özelliği. Ayarladığınız *ClientIDRowSuffix* özelliği veri alanının adı ve bu alanın değeri için kullanılır son eki olarak oluşturulan için *ClientID* değeri. Genellikle bir veri kaydı olarak birincil anahtarı kullanır *ClientIDRowSuffix* değeri.
- *Devralma* – Bu ayar denetimleri için varsayılan davranış; bir denetimin kimliği oluşturma, üst ile aynı olduğunu belirtir.

Ayarlayabileceğiniz *ClientIDMode* sayfa düzeyinde özelliği. Bu varsayılan tanımlar *ClientIDMode* geçerli sayfadaki tüm denetimler için değer.

Varsayılan *ClientIDMode* değer sayfa düzeyinde *AutoID*ve varsayılan *ClientIDMode* denetim düzeyi değerdir *devral*. Bu özellik herhangi bir yere kodunuzda değil ayarlarsanız, sonuç olarak, tüm denetimler varsayılan *AutoID* algoritması.

Sayfa düzeyi değeri ayarlayın *@page* aşağıdaki örnekte gösterildiği gibi yönergesi:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Ayrıca ayarlayabilirsiniz *ClientIDMode* yapılandırma dosyasında (makine) bilgisayar düzeyinde veya uygulama düzeyinde değeri. Bu varsayılan tanımlar *ClientIDMode* tüm denetimleri uygulamadaki tüm sayfalar için ayarlama. Bilgisayar düzeyinde değeri ayarlarsanız, varsayılan tanımlar *ClientIDMode* o bilgisayarda tüm Web siteleri için ayarlama. Aşağıdaki örnekte gösterildiği *ClientIDMode* yapılandırma dosyasında ayarı:

[!code-xml[Main](overview/samples/sample48.xml)]

Değeri, daha önce belirtildiği gibi *ClientID* özelliği için bir denetimin üst adlandırma kapsayıcıdan elde edilmiştir. Ana sayfalar kullanırken olduğu gibi bazı senaryolarda denetimleri kimliklerle olanlar için aşağıdaki gibi HTML çizilir düşebilir:

[!code-html[Main](overview/samples/sample49.html)]

Olsa bile *giriş* biçimlendirmede gösterilen öğesinin (gelen bir *TextBox* denetimi) yalnızca iki adlandırma kapsayıcılar olan derin sayfasındaki (iç içe *ContentPlaceholder* denetimleri), ana sayfa işlendi nedeniyle, sonuç aşağıdaki gibi bir denetim kimliği yoldur:

[!code-console[Main](overview/samples/sample50.cmd)]

Bu kimliği sayfasında benzersiz olması sağlanır, ancak gereksiz yere uzun çoğu amaçlıdır. İşlenmiş kimliği kısaltın ve kimliği nasıl oluşturulacağını üzerinde daha fazla denetime sahip olmasını istediğiniz düşünün. (Örneğin, "ctlxxx" öneklerini kaldırmak istediğiniz.) Bunu elde etmenin en kolay yolu ayarlanmasıdır *ClientIDMode* aşağıdaki örnekte gösterildiği gibi özelliği:

[!code-aspx[Main](overview/samples/sample51.aspx)]

Bu örnekte *ClientIDMode* özelliği ayarlanmış *statik* en dıştaki için *NamingPanel* öğesi ve kümesine *tahmin edilebilir* İç için *NamingControl* öğesi. Aşağıdaki biçimlendirmede (sayfa ve ana sayfaya geri kalanı için önceki örnekte olduğu gibi aynı olduğu varsayılır) bu ayarları neden:

[!code-html[Main](overview/samples/sample52.html)]

*Statik* ayarının etkisi en dıştaki içinde herhangi bir denetim için adlandırma hiyerarşisi sıfırlamanın *NamingPanel* öğesi ve ortadan *ContentPlaceHolder* ve *AnaSayfa* oluşturulan kimliği kimlikleri ( *Adı* özniteliği işlenmiş öğelerinin etkilenmez, normal ASP.NET işlevselliği için olayları tutulur şekilde durumunu görüntülemek ve benzeri.) İşaretleme için taşıma adlandırma hiyerarşi sıfırlamanın bir yan etkisi olsa bile *NamingPanel* öğelerine farklı bir *ContentPlaceholder* denetim, işlenen istemci kimliklerinin aynı kalır.

> [!NOTE]
> İşlenen denetim kimlikleri benzersiz olduğundan emin olun size olduğuna dikkat edin. Değilse, istemci gibi tek tek HTML öğeleri için benzersiz kimlikler gerektiren herhangi bir işlevsellik bölünebilir *document.getElementById* işlevi.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Tahmin edilebilir istemci kimlikleri verilere bağlı denetimler oluşturma

*ClientID* eski algoritması veriye bağlı bir liste denetiminde denetimleri olabilir için oluşturulan değerleri uzun ve gerçekten tahmin edilebilir değil. *ClientIDMode* işlevselliği daha nasıl bu kimlikleri oluşturulduğunu kontrol size yardımcı olabilir.

Aşağıdaki örnekte biçimlendirme içeren bir *ListView* denetimi:

[!code-aspx[Main](overview/samples/sample53.aspx)]

Önceki örnekte, *ClientIDMode* ve *RowClientIDRowSuffix* özellikleri biçimlendirmede ayarlayın. *ClientIDRowSuffix* özelliği yalnızca verilere bağlı denetimler kullanılabilir ve davranışını kullandığınız denetleyen bağlı olarak farklılık gösterir. Farklılıklar şunlardır:

- *GridView* denetim —, bir veya daha fazla sütun adı veri kaynağında istemci kimlikleri oluşturmak için çalışma zamanında birleştirilen belirtebilirsiniz. Örneğin, ayarlarsanız *RowClientIDRowSuffix* "ProductName, ProductID", işlenen öğelerini aşağıdaki gibi bir biçim için kimlikleri denetlemek:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* denetim — istemci kimliğine eklenen veri kaynağındaki tek bir sütun belirtebilirsiniz Örneğin, ayarlarsanız *ClientIDRowSuffix* "ProductName" işlenmiş kimlikleri aşağıdaki gibi bir biçim denetiminiz olur:

- [!code-console[Main](overview/samples/sample55.cmd)]

- Bu durumda sonunda 1, geçerli veri öğesinin ürün Kimliğinden türetilir.

- *Yineleyici* denetim — bu denetim desteklemediği *ClientIDRowSuffix* özelliği. İçinde bir *yineleyici* denetimi, geçerli satırın dizini kullanılır. ClientIDMode kullandığınızda, "Tahmin edilebilir" ile = bir *yineleyici* denetlemek, kimlikleri üretilen istemci, aşağıdaki biçime sahiptir:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Sondaki 0 geçerli satır dizinidir.

*FormView* ve *DetailsView* denetimleri uygulamalarının desteklemediği için birden çok satır görüntülemez *ClientIDRowSuffix* özelliği.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Veri denetimleri kalıcı satır seçimi

*GridView* ve *ListView* denetimler kullanıcıları bir satır Seç olanak sağlar. ASP.NET önceki sürümlerde Seçimi sayfasında satır dizini temel. Örneğin, 1 sayfasında üçüncü öğeyi seçin ve ardından 2 sayfasına gitmek, o sayfadaki üçüncü öğe seçilir.

Kalıcı seçimi, .NET Framework 3.5 SP1'deki dinamik veri projelerinde yalnızca başlangıçta destekleniyordu. Bu özellik etkinleştirildiğinde, geçerli seçili öğe öğesi için veri anahtarı temel alır. Başka bir deyişle, 1 sayfasındaki üçüncü satır seçin ve sayfa 2 taşırsanız hiçbir şey 2 sayfasında seçilidir. Üçüncü satır, 1 sayfasına geri dönün, hala seçilir. Kalıcı seçimi için desteklenen şimdi *GridView* ve *ListView* denetimleri kullanarak tüm projelerde *EnablePersistedSelection* gösterildiği gibi özelliği Aşağıdaki örnek:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>ASP.NET grafik denetimi

ASP.NET *grafik* denetim .NET Framework Veri görselleştirme teklifleri genişletir. Kullanarak *grafik* denetimi, karmaşık bir istatistik veya finansal analiz sezgisel ve görsel olarak ilgi çekici grafiklerde sahip ASP.NET sayfaları oluşturabilirsiniz. ASP.NET *grafik* denetim eklenti olarak .NET Framework sürüm 3.5 SP1 sürümü kullanıma sunuldu ve .NET Framework 4 yayın bir parçasıdır.

Denetimi aşağıdaki özellikleri içerir:

- 35 ayrı grafik türleri.
- Grafik alanı, başlıklar, Göstergeler ve ek açıklamaları sınırsız sayıda.
- Tüm grafik öğeleri için Görünüm ayarlarını birçok farklı.
- Çoğu grafik türünde 3-b desteği.
- Veri noktalarını otomatik olarak sığabilecek akıllı veri etiketlerini.
- Şerit çizgisi, Ölçek bölme çizgisi ve Logaritmik ölçeklendirme.
- 50'den fazla finansal ve istatistiksel formüller veri çözümleme ve dönüştürme için.
- Basit bağlama ve düzenleme grafik veri.
- Tarih, saat ve para birimi gibi ortak veri biçimleri için destek.
- Etkileşim ve istemci de dahil olmak üzere, olay denetimli özelleştirme desteği Ajax kullanarak olayları'ı tıklatın.
- Durum Yönetimi.
- İkili akış.

Aşağıdaki şekil ASP.NET grafik denetim tarafından üretilen finansal grafikleri örnekleri gösterir.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Şekil 2: ASP.NET grafik denetimine örnekler

ASP.NET grafik denetiminin nasıl kullanıldığını daha fazla örnekleri indirmek için örnek kod [örnekleri ortamı Microsoft Grafik denetimleri için](https://go.microsoft.com/fwlink/?LinkId=128300) MSDN Web sitesinde sayfası. Daha fazla örnekleri Topluluğu'nun en içerik bulabilirsiniz [grafik denetim Forumu](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Grafik denetimi için bir ASP.NET sayfası ekleme

Aşağıdaki örnekte nasıl ekleneceğini gösterir bir *grafik* biçimlendirme kullanarak bir ASP.NET sayfası denetimi. Örnekte, *grafik* denetim statik veri noktaları için bir sütun grafik üretir.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>3B grafikleri kullanma

*Grafik* denetimi içeren bir *ChartAreas* içerebilir koleksiyonu *ChartArea* grafik alanları özelliklerini tanımlayan nesneleri. Örneğin, 3B grafik alanı için kullanmayı *Area3DStyle* özelliği aşağıdaki örnekteki gibi:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Aşağıdaki şekilde dört dizi içeren bir 3B grafik gösterir *çubuğu* grafik türü.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Şekil 3: 3-b çubuk grafiği

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Ölçek bölme çizgisi ve Logaritmik ölçekler kullanma

Ölçek bölme çizgisi ve Logaritmik ölçekler açıdan çok yönlülük grafiğe eklemek için iki ek yöntemler verilmiştir. Bu özellikler, grafik alanında her eksen özgüdür. Örneğin, bu özellikler bir grafik alanı birincil Y ekseninde, kullanmayı *AxisY.IsLogarithmic* ve *ScaleBreakStyle* özelliklerinde bir *ChartArea* nesnesi. Aşağıdaki kod parçacığında birincil Y Ekseninde ölçek bölme çizgisi kullanmayı gösterir.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Aşağıdaki şekilde Y ekseni ölçek bölme çizgisi etkin gösterilmektedir.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Şekil 4: Ölçek bölme çizgisi

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>QueryExtender denetimi ile verileri filtreleme

Verilerini filtrelemek için veri tabanlı Web sayfaları oluşturan geliştiriciler için yaygın bir görev olur. Bu geleneksel oluşturarak gerçekleştirildikten *burada* veri yan tümcelerinde kaynak kontrol eder. Bu yaklaşım karmaşık, olabilir ve bazı durumlarda *burada* sözdizimi, temel alınan veritabanı tam işlevselliğini yararlanmak vermez.

Daha kolay, yeni bir filtreleme yapmak için *QueryExtender* Denetim ASP.NET 4'te eklendi. Bu denetim eklenebilir *EntityDataSource* veya *LinqDataSource* bu denetimleri tarafından döndürülen verileri filtrelemek için kontrol eder. Çünkü *QueryExtender* denetim LINQ üzerinde kullanır, verileri çok verimli işlemlerinde sonuçları sayfasına gönderilmeden önce veritabanı sunucusunda filtre uygulanır.

*QueryExtender* denetimi, çeşitli filtre seçenekleri destekler. Aşağıdaki bölümlerde bu seçenekleri açıklar ve bunları nasıl kullanacağınızı örnekleri sağlayın.

#### <a name="search"></a>Ara

Arama seçeneği için *QueryExtender* denetimi belirli alanlarında arama gerçekleştirir. Aşağıdaki örnekte, içeriğini arar ve TextBoxSearch denetim girilen metin ekleyeceğini `ProductName` ve `Supplier.CompanyName` sunucudan döndürülen veri sütunlarında *LinqDataSource* Denetim.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Aralık

Aralık seçeneği arama seçeneğine benzer, ancak bir değer çifti aralığı tanımlamak için belirtir. Aşağıdaki örnekte, *QueryExtender* kontrol aramaları `UnitPrice` döndürülen veriler sütununda *LinqDataSource* denetim. Sayfadaki TextBoxFrom ve TextBoxTo denetimlerinden aralığı okuyun.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Özellik deyim seçeneği, bir özellik değeri bir karşılaştırma tanımlamanıza olanak sağlar. İfade değerlendirilirse *doğru*, incelenir veriler döndürülür. Aşağıdaki örnekte, *QueryExtender* denetimi verilerde karşılaştırarak veri filtreleri `Discontinued` sayfasında CheckBoxDiscontinued denetiminden değerine sütun.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Son olarak, ile kullanmak için özel bir ifade belirtebilirsiniz *QueryExtender* denetim. Bu seçenek, özel filtre mantığını tanımlayan sayfasında bir işlevi çağırmak sağlar. Aşağıdaki örnek, özel bir ifadede bildirimli olarak belirtmek gösterilmiştir *QueryExtender* denetim.

[!code-aspx[Main](overview/samples/sample64.aspx)]

Aşağıdaki örnek tarafından çağrılan özel işlevi gösterir *QueryExtender* denetim. Bu durumda, içeren bir veritabanı sorgusu kullanmak yerine bir *burada* yan tümcesi, kod LINQ sorgusu verilere filtre için kullanır.

[!code-csharp[Main](overview/samples/sample65.cs)]

Bu örnekler yalnızca tek bir ifade kullanılmasını *QueryExtender* birer birer denetim. Ancak, birden çok ifadelerinin içinde içerebilir *QueryExtender* denetim.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>HTML kod ifadeleri kodlanmış

Bazı ASP.NET siteler (özellikle ile ASP.NET MVC) yoğun kullanır `<%` =  `expression %>` (genellikle "nuggets code" olarak adlandırılır) sözdizimi bazı metinleri yanıta yazılacak. Kod ifadeleri kullandığınızda, HTML olarak metin kullanıcıdan geliyorsa metin girişi, sayfalar açık XSS (siteler arası komut dosyası) saldırının bırakılabilir kodlanacak unutmanız kolaydır.

ASP.NET 4 kod ifadeler için aşağıdaki yeni sözdizimini sunar:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Varsayılan olarak, yanıt yazılırken HTML kodlaması bu söz dizimini kullanır. Bu yeni ifade etkili bir şekilde aşağıdaki ifadeyi:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Örneğin, &lt;%: İstek ["UserInput"] %&gt; değerine göre HTML kodlaması gerçekleştirme *["UserInput"] isteği*.

Bu özelliğin amacı, böylece her aşamada hangisinin kullanılacağını karar vermek için zorlanmaz eski sözdizimi tüm örneklerini yeni sözdizimi ile değiştirmek mümkün olmasını sağlamaktır. Ancak, bu durumda bu çift kodlama için yol açabilir çıkış olan metin HTML olma amacını veya zaten kodlanır durumlar vardır.

Bu durumlarda, yeni bir arabirimi, ASP.NET 4 tanıtır *IHtmlString*, somut bir uygulama ile birlikte *HtmlString*. Bu tür örnekleri, dönüş değeri zaten düzgün şekilde kodlanmış (veya aksi halde incelenmesi olduğunu) HTML olarak görüntülemek için ve bu nedenle değer yeniden HTML ile kodlanmış olmamalıdır olduğunu belirtmek olanak tanır. Örneğin, aşağıdaki olmamalıdır (ve değil) kodlanmış HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET 4 çalıştırırken ASP.NET MVC 2 yardımcı yöntemler kodlanmış çift olmadıkları böylece bu yeni sözdizimi ile çalışmak için güncelleştirilmiş, ancak yalnızca olmuştur. ASP.NET 3.5 SP1'i kullanarak bir uygulamayı çalıştırdığınızda, bu yeni söz diziminin çalışmaz.

Bu XSS saldırılarına karşı koruma garanti etmez olduğunu aklınızda bulundurun. Örneğin, tırnak işaretleri içine olmayan öznitelik değerleri kullanır HTML getiriyor kullanıcı girişi içerebilir. ASP.NET denetimleri ve ASP.NET MVC Yardımcıları çıktısı her zaman öznitelik değerlerine tırnak işaretleri içindeki önerilen yaklaşım olduğu içerdiğini unutmayın.

Benzer şekilde, bu söz dizimini kullanıcı girişini temel alarak bir JavaScript dize oluşturduğunuzda gibi JavaScript kodlama gerçekleştirmez.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Proje şablonu değişiklikleri

Yeni bir Web sitesi veya Web uygulaması projesi oluşturmak için Visual Studio kullandığınızda ASP.NET önceki sürümlerinde, sonuçta elde edilen projeleri yalnızca bir Default.aspx sayfasında, varsayılan içeriyor. `Web.config` dosyası ve `App_Data` aşağıda gösterildiği gibi klasörü çizim:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio hiç, aşağıdaki çizimde gösterildiği gibi dosya içermiyor bir boş Web sitesi proje türü de destekler:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Sonuç başlangıç için çok az Kılavuzu üretim Web uygulaması derleme hakkında olmasıdır. Bu nedenle, üç yeni şablonlar, biri boş bir Web uygulaması projesi için ve her biri bir Web uygulaması ve Web sitesi proje için ASP.NET 4 tanıtır.

#### <a name="empty-web-application-template"></a>Boş Web uygulaması şablonu

Adı da anlaşılacağı gibi boş bir Web uygulaması stripped-down bir Web uygulaması proje şablonudur. Bu proje şablonu aşağıdaki resimde gösterildiği gibi Visual Studio yeni proje iletişim kutusundan seçin:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](overview/_static/image8.png))

Boş bir ASP.NET Web uygulaması oluşturduğunuzda, Visual Studio aşağıdaki klasör düzeni oluşturur:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Bu boş Web sitesi düzene önceki sürümlerinden ASP.NET, tek bir istisna benzer. Visual Studio 2010'da, aşağıdaki en düşük boş bir Web uygulaması ve boş Web sitesi projeleri içeren `Web.config` projenin hedeflediği çerçeve tanımlamak için Visual Studio tarafından kullanılan bilgileri içeren dosyanın:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Bu olmadan *targetFramework* özelliği, eski uygulamaları açarken uyumluluğu korumak için .NET Framework 2.0 hedefleme için Visual Studio Varsayılanları.

#### <a name="web-application-and-web-site-project-templates"></a>Web uygulaması ve Web sitesi proje şablonları

Visual Studio 2010 ile birlikte sağlanan diğer iki yeni proje şablonları önemli değişiklikler içeriyor. Aşağıdaki şekilde yeni bir Web uygulaması projesi oluşturduğunuzda, oluşturduğunuz proje düzeni gösterilmektedir. (Bir Web sitesi projesini düzenini neredeyse aynıdır.)

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Projeyi önceki sürümlerde oluşturulmayan dosyalarının sayısını içerir. Ayrıca, yeni Web uygulaması projesi, yeni uygulama erişimi güvenliğini hızla başlamanıza sağlayan temel üyelik işlevselliğini ile yapılandırılır. Bu ekleme nedeniyle `Web.config` dosya yeni proje üyelik, roller ve profillerini yapılandırmak için kullanılan girişleri içerir. Aşağıdaki örnekte gösterildiği `Web.config` dosyası için yeni bir Web uygulaması projesi. (Bu durumda, *roleManager* devre dışı bırakılır.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](overview/_static/image14.png))

İkinci bir projeyi de içeren `Web.config` dosyasını `Account` dizin. İkinci yapılandırma dosyası olmayan-oturum açmış olan kullanıcılar için ChangePassword.aspx sayfasına erişim güvenliğini sağlamak için bir yol sağlar. Aşağıdaki örnek, ikinci içeriğini gösterir `Web.config` dosya.

![](overview/_static/image15.png)

Varsayılan olarak yeni proje şablonları tarafından oluşturulan sayfaları'den daha fazla içerik önceki sürümlerinde de içerir. Varsayılan ana sayfa ve CSS dosyası projeyi içeren ve varsayılan sayfa (Default.aspx) varsayılan olarak ana sayfayı kullanmak üzere yapılandırılmıştır. Web uygulaması veya Web sitesi ilk kez çalıştırdığınızda, varsayılan (ana) sayfası zaten işlevsel olduğunu sonucudur. Aslında, yeni bir MVC uygulaması başlattığınızda gördüğünüz varsayılan sayfa benzerlik gösterir.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](overview/_static/image18.png))

Proje şablonları yapılan bu değişiklikleri amacı, yeni bir Web uygulaması oluşturmaya başlayın nasıl Kılavuzu sağlamaktır. Anlam olarak doğru katı XHTML 1.0 uyumlu biçimlendirme ve CSS kullanarak belirtilen düzeni, şablonları sayfalarında ASP.NET 4 Web uygulamaları oluşturmak için en iyi yöntemler temsil eder. Varsayılan sayfa kolayca özelleştirebilirsiniz iki sütun düzeni de vardır.

Örneğin, yeni bir Web uygulaması için bazı renkleri değiştirmek ve ASP.NET uygulamam logosu yerine şirket logonuzu eklemek istediğinizi düşünelim. Bunu yapmak için altında yeni bir dizin oluşturun. `Content` logo görüntüsü saklamak için:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Resim eklemek için ardından açın `Site.Master` dosya, burada My ASP.NET Application metin tanımlanır ve bunların yerine Bul bir *görüntü* öğesi, *src* özniteliği için yeni logo ayarlayın Aşağıdaki örnekte olduğu gibi resmi:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](overview/_static/image22.png))

Ardından, Site.css dosyasına gidin ve CSS sınıf tanımları yanı sıra, üstbilgi sayfa arka plan rengini değiştirmek için değiştirin.

Bu değişikliklerin sonucu olarak, özelleştirilmiş giriş sayfası çok az çaba ile görüntüleyebilirsiniz şöyledir:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Tam boyutlu görüntüyü görüntülemek için tıklatın](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>CSS geliştirmeleri

Önde gelen ASP.NET 4'te çalışma alanları biri son HTML standartlarla uyumlu HTML işleme yardımcı olmak için bırakıldı. Bu, ASP.NET Web sunucusu denetimleri CSS stilleri kullanma değişiklikleri içerir.

#### <a name="compatibility-setting-for-rendering"></a>İşleme için Uyumluluk ayarı

Varsayılan olarak bir Web uygulaması veya Web sitesi .NET Framework 4 hedeflediğinde, *controlRenderingCompatibilityVersion* özniteliği *sayfaları* öğesi "4.0" olarak ayarlanmış. Bu öğe içinde makine düzeyinde tanımlanan `Web.config` dosya ve varsayılan olarak tüm ASP.NET 4 uygulamaları için geçerlidir:

[!code-xml[Main](overview/samples/sample69.xml)]

Değeri *controlRenderingCompatibility* olası yeni sürüm tanımları gelecekteki Sunumlarda sağlayan bir dize. Geçerli sürümde, bu özellik için aşağıdaki değerleri desteklenir:

- "3.5". Bu ayar, eski işleme ve biçimlendirme gösterir. Biçimlendirme denetimleri tarafından işlenen, %100 geriye dönük olarak uyumlu ve ayarını *xhtmlConformance* özelliği dikkate alınır.
- "4.0". Bu ayar özelliğine sahipse, ASP.NET Web sunucusu denetimleri aşağıdakileri yapın:
- *XhtmlConformance* özelliği her zaman "Strict" değerlendirilir. Sonuç olarak, denetimleri XHTML 1.0 Strict biçimlendirmesi oluşturmak.
- Artık olmayan giriş denetimlerini devre dışı bırakma geçersiz stiller işler.
- *div* Gizli alanların etrafında öğeleri artık kullanıcı tarafından oluşturulan CSS kuralları ile karışmaması için stili.
- Menü denetimleri anlamsal olarak doğru ve erişilebilirlik yönergelerini ile uyumlu biçimlendirme işleyebilir.
- Doğrulama denetimleri, satır içi stiller işlenmeyebilir.
- Denetimleri kenarlık daha önce işlenen = "0" (ASP.NET tarafından türetilen denetimler *tablo* denetimi ve ASP.NET *görüntü* denetimi) artık bu öznitelik işleme.

#### <a name="disabling-controls"></a>Denetimleri devre dışı bırakma

ASP.NET 3.5 SP1 ve önceki sürümlerinde framework işler *devre dışı* herhangi bir özelliği denetim için HTML biçimlendirmesini özniteliği *etkin* özelliğini *false*. Ancak, yalnızca HTML 4.01 belirtimine göre *giriş* öğeleri, bu öznitelik olmalıdır.

ASP.NET 4'te ayarladığınız *controlRenderingCompatabilityVersion* "3.5" aşağıdaki örnekte olduğu gibi özelliğine:

[!code-xml[Main](overview/samples/sample70.xml)]

Biçimlendirme için oluşturacağınız bir *etiket* denetim denetimini devre dışı bırakır aşağıdaki gibi:

[!code-aspx[Main](overview/samples/sample71.aspx)]

*Etiket* denetimi aşağıdaki HTML oluşturmak:

[!code-html[Main](overview/samples/sample72.html)]

ASP.NET 4'te ayarladığınız *controlRenderingCompatabilityVersion* "4.0". Bu durumda, bu işleme yalnızca denetimleri *giriş* öğeleri sokacak bir *devre dışı* ne zaman özniteliği denetimin *etkin* özelliği ayarlanmış *false* . HTML işlenmeyebilir denetimleri *giriş* öğeleri oluşturmak yerine bir *sınıfı* başvuran denetiminin devre dışı bırakılmış bir görünüm tanımlamak için kullanabileceğiniz bir CSS sınıfı özniteliği. Örneğin, *etiket* önceki örnekte gösterilen denetimi aşağıdaki biçimlendirme oluşturmak:

[!code-html[Main](overview/samples/sample73.html)]

Bu denetim için belirtilen sınıfı için varsayılan değer "aspNetDisabled" dir. Ancak, bu varsayılan değeri statik ayarlayarak değiştirebileceğiniz *DisabledCssClass* statik özelliği *WebControl* sınıfı. Denetim geliştiriciler için belirli bir denetim için kullanılacak davranış da kullanılarak tanımlanabilir *SupportsDisabledAttribute* özelliği.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Div öğeleri geçici gizli alanları gizleme

ASP.NET 2.0 ve sonraki sürümler işlemek sisteme özgü gizli alanlar (gibi *gizli* görünüm durumu bilgilerini depolamak için kullanılan öğe) içinde *div* XHTML standart uymak için öğesi. Bir CSS kuralına etkilediğinde ancak, bu soruna yol açabilir *div* bir sayfadaki öğeler. Örneğin, bu sayfada görünen tek pikselli satırında sonuçlanabilir yaklaşık gizli *div* öğeleri. ASP.NET 4'te *div* ASP.NET tarafından oluşturulan gizli alanları içine öğelerini aşağıdaki örnekteki gibi CSS sınıf başvurusu ekleyin:

[!code-html[Main](overview/samples/sample74.html)]

Daha sonra yalnızca için geçerli bir CSS sınıfı tanımlayabilirsiniz *gizli* aşağıdaki örnekte olduğu gibi ASP.NET tarafından oluşturulan öğeleri:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Bir dış tablo şablonlu denetimler için oluşturma

Varsayılan olarak, şablonları destekleyen aşağıdaki ASP.NET Web sunucusu denetimleri otomatik olarak satır içi stiller uygulamak için kullanılan bir dış tablo Sarmalanan:

- *FormView*
- *Oturum açma*
- *PasswordRecovery*
- *Parola değiştirme*
- *Sihirbazı*
- *CreateUserWizard*

Adlı yeni bir özellik *RenderOuterTable* biçimlendirmeden kaldırılacak dış tablo sağlayan bu denetimleri eklenmiştir. Örneğin, aşağıdaki örneği göz önünde bulundurun bir *FormView* denetimi:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Bu biçimlendirme içeren bir HTML tablosu sayfasında, aşağıdaki çıkışı oluşturur:

[!code-html[Main](overview/samples/sample77.html)]

Tablo oluşturulmasını önlemek için ayarlayabileceğiniz *FormView* denetimin *RenderOuterTable* özelliği, aşağıdaki örnekteki gibi:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Önceki örnekte şu çıktıyı, olmadan işler *tablo*, *tr*, ve *td* öğeleri:

> İçerik


Beklenmeyen etiket denetimi tarafından işlenen çünkü bu geliştirme, CSS, denetimiyle içeriğini stiline kolaylaştırabilir.

> [!NOTE]
> Bu değişiklik devre dışı bırakır, Visual Studio 2010 tasarımcıda Otomatik Biçimlendir işlevi desteği artık yoktur çünkü Not bir *tablo* otomatik biçimlendirme seçeneği tarafından oluşturulan stil öznitelikleri barındırabilir öğesi.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>ListView denetimi geliştirmeleri

*ListView* Denetim ASP.NET 4'te kullanmak daha kolay hale getirilmiştir. Sunucu denetimi bilinen bir kimliği bulunan bir düzen şablonu belirtin denetimi önceki sürümü gerekli Aşağıdaki biçimlendirmede nasıl kullanılacağına ilişkin bir örnek gösterilir *ListView* ASP.NET 3.5 denetimi.

[!code-aspx[Main](overview/samples/sample79.aspx)]

ASP.NET 4'te *ListView* denetim düzen şablonunu gerektirmez. Önceki örnekte gösterilen biçimlendirmesi olan aşağıdaki biçimlendirme değiştirilebilir:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList ve RadioButtonList denetim geliştirmeleri

ASP.NET 3. 5'için düzeni belirtebilirsiniz *CheckBoxList* ve *RadioButtonList* aşağıdaki iki ayarı kullanarak:

- *Akış*. Denetimini işler *span* içeriği içerecek şekilde öğeleri.
- *Tablo*. Denetim işleyen bir *tablo* içeriğini içeren öğe.

Aşağıdaki örnekte bu denetimleri için işaretleme gösterilir.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Varsayılan olarak, aşağıdakine benzer HTML denetimlerini işlemeye:

[!code-html[Main](overview/samples/sample82.html)]

Bu denetimler anlamsal olarak doğru HTML oluşturmak için öğeleri listesini içerdiğinden HTML liste kullanarak içeriklerini oluşturması gerektiğini (*li*) öğeleri. Bu yardımcı teknoloji kullanarak Web sayfaları oku ve denetimleri kullanarak CSS stil kolaylaştırır kullanıcılar için kolaylaştırır.

ASP.NET 4'te *CheckBoxList* ve *RadioButtonList* denetimleri desteklemek için aşağıdaki yeni değerleri *RepeatLayout* özelliği:

- *OrderedList* – içerik olarak işlenmeden *li* içinde öğelerin bir *ol* öğesi.
- *UnorderedList* – içerik olarak işlenmeden *li* içinde öğelerin bir *ul* öğesi.

Aşağıdaki örnek, bu yeni değerlerin nasıl kullanılacağını gösterir.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Aşağıdaki HTML önceki biçimlendirme oluşturur:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Ayarlanmış olmadığını unutmayın *RepeatLayout* için *OrderedList* veya *UnorderedList*, *RepeatDirection* özelliği olur ve artık kullanılabilir İşaretleme veya kod içinde özellik ayarlanmışsa çalışma zamanında bir özel durum. Bu denetimlerin görsel düzen CSS kullanmayı tanımlandığından özelliği herhangi bir değer gerekir.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Menü denetim geliştirmeleri

ASP.NET 4 önce *menü* denetim HTML tabloları bir dizi çizilir. Bu satır içi özelliklerini ayarlama dışında CSS stilleri uygulamak daha zor hale ve ayrıca erişilebilirlik standartları ile uyumlu değil.

ASP.NET 4'te denetimi şimdi sırasız bir listesini ve liste öğeleri oluşur anlamsal biçimlendirme kullanarak HTML işler. Aşağıdaki örnek için bir ASP.NET sayfasından biçimlendirmeyi gösterir *menü* denetim.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Sayfayı işleyen, aşağıdaki HTML denetimi üretir ( *onclick* kod, daha anlaşılır olması için atlanmış):

[!code-html[Main](overview/samples/sample86.html)]

İşleme geliştirmeleri yanı sıra odak Yönetimi'ni kullanarak klavye gezinti menüsünün geliştirilmiştir. Zaman *menü* denetim odağı alır, öğeleri gezinmek için ok tuşlarını kullanabilirsiniz. *Menü* denetimi de artık erişilebilir zengin Internet uygulamaları (ARIA) rolleri ekler ve aşağıda öznitelikleri[ki](http://www.w3.org/TR/wai-aria-practices/#menu "menü ARIA yönergeleri")geliştirilmiş için Erişilebilirlik.

Menü denetimi için stiller, stil bloğu işlenen HTML öğeleri uygun olarak değil, sayfanın en üstünde içinde işlenir. Denetimi için stil üzerinde tam denetim almak istiyorsanız, yeni ayarlayabileceğiniz *IncludeStyleBlock* özelliğine *yanlış*, stil bloğu değil; Bu durumda yayınlanır. Bu özelliği kullanmak için bir menü görünümünü ayarlamak için Visual Studio Tasarımcısı'nda Otomatik biçimlendirme özelliğini kullanmayı yoludur. Sayfayı çalıştırın sayfası kaynağı açın ve sonra işlenen stil bloğu harici bir CSS dosyaya kopyalayın. Visual Studio'da, stil ve kümesi geri *IncludeStyleBlock* için *false*. Menü görünüm stilleri bir harici stil sayfası kullanılarak tanımlanır sonucudur.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Sihirbaz ve CreateUserWizard denetimleri

ASP.NET *Sihirbazı* ve *CreateUserWizard* denetimleri işlenen HTML tanımlamanızı sağlayan şablonları destekler. (*CreateUserWizard* türetilen *Sihirbazı*.) Aşağıdaki örnek, tam olarak şablonlu için biçimlendirme gösterir *CreateUserWizard* denetimi:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Denetimin HTML aşağıdakine benzer işler:

[!code-html[Main](overview/samples/sample88.html)]

Şablon içeriği değiştirebilirsiniz, ancak ASP.NET 3.5 SP1'de, hala çıktısını denetime sınırlı *Sihirbazı* denetim. ASP.NET 4'te oluşturduğunuz bir *LayoutTemplate* şablonu ve INSERT *yer tutucu* denetler (ayrılmış bir adı kullanarak) nasıl istediğinizi belirtmek için *Sihirbazı'nı Denetim* işlenecek. Aşağıdaki örnekte bu gösterilmektedir:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Aşağıdaki yer tutucuları adlandırılmış örnek içeren *LayoutTemplate* öğe:

- *headerPlaceholder* – çalışma zamanında bu içeriğine göre değiştirilir *HeaderTemplate* öğesi.
- *sideBarPlaceholder* – çalışma zamanında bu içeriğine göre değiştirilir *SideBarTemplate'i* öğesi.
- *wizardStepPlaceHolder* – çalışma zamanında bu içeriğine göre değiştirilir *WizardStepTemplate* öğesi.
- *navigationPlaceholder* – çalışma zamanında bu tanımladığınız Gezinti şablonlardan tarafından değiştirilir.

Yer tutucuları kullanan örnek biçimlendirmede aşağıdaki HTML (olmadan gerçekte şablonlarında tanımlanmış içerik) işler:

[!code-html[Main](overview/samples/sample90.html)]

Artık kullanıcı-tanımlı değil yalnızca HTML bir *span* öğesi. (Biz bu gelecek sürümler, düşündüğünüz bile *span* öğesi değil çizilir.) Bu şimdi tarafından oluşturulan tüm içerik üzerinde tam denetim verir *Sihirbazı* denetim.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC, ASP.NET 3.5 SP1'e Mart 2009 bir eklenti çerçeve sunulmuştur. Visual Studio 2010 yeni özellikleri ve yetenekleri içerir ASP.NET MVC 2 içerir.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Alanları desteği

Alanları diğer bölümlerden göreli yalıtım bulunan büyük bir uygulamanın bölümlere Grup denetleyicileri ve görünümleri sağlar. Her bir alan sonra ana uygulama tarafından başvurulan ayrı bir ASP.NET MVC proje olarak uygulanabilir. Bu, büyük bir uygulama oluşturduğunuzda karmaşıklığını yönetmenize yardımcı olur ve tek bir uygulama üzerinde birlikte çalışmak üzere birden çok ekibin kolaylaştırır.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Veri ek açıklamasını özniteliği doğrulama desteği

*DataAnnotations* öznitelikleri meta veri öznitelikleri kullanarak bir model için doğrulama mantığını ekleme olanak sağlar. *DataAnnotations* öznitelikleri ASP.NET dinamik veri ASP.NET 3.5 SP1'de sunulmuştur. Bu öznitelikler varsayılan model bağlayıcısını tümleştirilmiştir ve kullanıcı girişi doğrulamak için meta veri güdümlü bir yol sağlar.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Şablonlu Yardımcılar

Şablonlu Yardımcılar veri türleriyle şablonlarını görüntülemek ve otomatik olarak ilişkilendirmek düzenleme sağlar. Örneğin, tarih seçici UI öğesi için otomatik olarak işlenir belirtmek için bir şablon yardımcı kullanabilirsiniz bir *System.DateTime* değeri. ASP.NET dinamik veri alan şablonlarında bu benzer.

*Html.EditorFor* ve *Html.DisplayFor* yardımcı yöntemler standart veri işleme birden fazla özelliğe sahip de gibi karmaşık nesne türleri için yerleşik destek vardır. Bunlar gibi veri ek açıklamasını özniteliklere uygulamanıza izin vererek ayrıca işleme özelleştirme *DisplayName* ve *ScaffoldColumn* için *ViewModel* nesnesi.

UI Yardımcıları daha çıktısını özelleştirebilir ve ne oluşturulan üzerinde tam denetime sahip istediğiniz sıklığı. *Html.EditorFor* ve *Html.DisplayFor* yardımcı yöntemler destek bunu geçersiz kılabilirsiniz dış şablonları ve denetim çizilir çıkış tanımlamanıza olanak sağlayan bir şablon mekanizmasını kullanarak. Şablonları tek tek bir sınıf için oluşturulabilir.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dinamik veri

Dinamik veri sunulmuştur Orta 2008'de .NET Framework 3.5 SP1 sürümünde. Bu özellik, aşağıdakiler de dahil olmak üzere, veri güdümlü uygulamaları oluşturmak için birçok iyileştirme sağlar:

- Hızlı veri kullanan bir Web sitesi oluşturmak için RAD bir deneyim.
- Veri modelinde tanımlı kısıtlamaları dayalı otomatik doğrulama.
- Kolayca alanları için oluşturulmuş biçimlendirmeyi değiştirme yeteneğini *GridView* ve *DetailsView* dinamik veri projenizi parçası olan alan şablonları kullanarak kontrol eder.

> [!NOTE]
> Not daha fazla bilgi için bkz: [dinamik veri belgelerine](https://msdn.microsoft.com/library/cc488545.aspx) MSDN Kitaplığı'nda.


ASP.NET 4 için dinamik veri geliştiricilerin hızla veri güdümlü Web siteleri oluşturmak için daha fazla güç vermek için geliştirilmiştir.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Dinamik veri için mevcut projeleri etkinleştirme

.NET Framework 3.5 SP1'de sevk dinamik veri özellikleri, aşağıdakiler gibi yeni özellikler duruma:

- Alan şablonları – bu verilere bağlı denetimler için veri türü tabanlı şablonlar sağlar. Alan şablonları, her bir alan için şablon alanları kullanmaktan veri denetimleri görünümünü özelleştirmek için daha basit bir yol sağlar.
- Doğrulama – dinamik veri doğrulama gerekli alanlar, aralığı denetleyerek, tür denetlemesi, normal ifadeler kullanarak eşleşen kalıbı gibi yaygın senaryolar için ve özel doğrulama belirtmek için veri sınıflarında öznitelikleri kullanmanıza olanak sağlar. Doğrulama veri denetimleri tarafından zorlanır.

Ancak, bu özellikler aşağıdaki gereksinimlere sahiptir:

- Veri erişim katmanı Entity Framework veya LINQ-SQL temel gerekiyordu.
- Yalnızca veri kaynağı bu özellik için desteklenen denetimleri *EntityDataSource* veya *LinqDataSource* kontrol eder.
- Özellikler özelliğini desteklemek için gereken tüm dosyaları olması için dinamik veri ya da dinamik veri varlıkları şablonları kullanılarak oluşturulmuş bir Web projesi gereklidir.

Dinamik veri desteği ASP.NET 4'te ana amacı, dinamik veri herhangi bir ASP.NET uygulaması için yeni işlevsellik etkinleştirmektir. Aşağıdaki örnek, dinamik veri işlevselliği varolan bir sayfa yararlanabilir denetimleri için işaretleme gösterir.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Bu denetimler için dinamik veri desteğini etkinleştirmek için sayfanın kodunu aşağıdaki kodu eklenmesi gerekir:

[!code-csharp[Main](overview/samples/sample92.cs)]

Zaman *GridView* denetim düzenleme modunda, dinamik veri otomatik olarak doğrular girilen verilerin doğru biçimde değil. Değilse, hata iletisi görüntülenir.

Bu işlevi başka avantajları da sağlar, varsayılan belirlemek mümkün olması gibi modu için değerler Ekle. Dinamik veri bir alan için bir varsayılan değer uygulamak için gerekir olaya ekleme, denetimini bulun (kullanarak *FindControl*) ve değerini ayarlayın. ASP.NET 4'te *EnableDynamicData* çağrısı Bu örnekte gösterildiği gibi herhangi bir alan için varsayılan değerleri nesnesinde geçirmenize olanak veren ikinci bir parametresi destekler:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Bildirim temelli DynamicDataManager denetimi sözdizimi

*DynamicDataManager* denetim geliştirilmiştir, böylelikle bildirimli olarak, yapılandırabilirsiniz ASP.NET, çoğu denetimlerinde yalnızca içinde kod yerine olduğu gibi. İşaretleme için *DynamicDataManager* denetimi aşağıdaki gibi görünür:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Bu biçimlendirme başvuru GridView1'i denetimi için dinamik veri davranışı etkinleştirir *DataControls* bölümünü *DynamicDataManager* denetim.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Varlık şablonları

Varlık şablonları, özel bir sayfa oluşturmanıza gerek kalmadan veri düzenini özelleştirmek için yeni bir yolunu sunar. Sayfa şablonları kullanma *FormView* denetimi (yerine *DetailsView* denetlemek, şablonların dinamik veri önceki sürümlerinde kullanılan gibi) ve *DynamicEntity* Varlık şablonları oluşturmak için denetim. Bu dinamik veri tarafından işlenen biçimlendirme üzerinde daha fazla denetim sağlar.

Aşağıdaki liste, varlık şablonlarını içeren yeni proje dizin düzenini gösterir:

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate` Dizini veri modeli nesneleri görüntülemek nasıl için şablonlar içerir. Varsayılan olarak, kullanarak nesneleri işlendiğini `Default.ascx` tarafından oluşturulan işaretleme benzer biçimlendirme sağlar şablonu *DetailsView* ASP.NET 3.5 SP1'deki dinamik veri tarafından kullanılan denetim. Aşağıdaki örnek için biçimlendirme gösterir `Default.ascx` denetimi:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Varsayılan şablonlar, görünüm tüm sitenin değiştirmek için düzenlenebilir. Görüntüleme, düzenleme ve ekleme işlemleri için şablonlar vardır. Yeni şablonlar veri nesnesinin adını göre tek bir nesne türünü Görünüm ve yapısını değiştirmek için eklenebilir. Örneğin, aşağıdaki şablonu ekleyebilirsiniz:

[!code-console[Main](overview/samples/sample97.cmd)]

Şablon aşağıdaki işaretleme içerebilir:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Yeni varlık şablonları yeni kullanarak bir sayfasında görüntülenen *DynamicEntity* denetim. Çalışma zamanında, bu denetim varlık şablonu içeriği ile değiştirilir. Aşağıdaki biçimlendirme gösterildiği *FormView* denetim `Detail.aspx` varlık şablonu kullanan sayfası şablonu. Bildirim *DynamicEntity* işaretleme öğesinde.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-e-mail-addresses"></a>URL'ler ve e-posta adresleri için yeni alan şablonlar

ASP.NET 4 tanıtan iki yeni yerleşik alan şablonlar, `EmailAddress.ascx` ve `Url.ascx`. Bu şablon olarak işaretlenmiş alanlar için kullanılan *EmailAddress* veya *Url* ile *DataType* özniteliği. İçin *EmailAddress* nesneleri alanı kullanılarak oluşturulan bir köprü olarak görüntülenir *mailto:* protokolü. Kullanıcılar bağlantıyı tıklattığında, kullanıcının e-posta istemcisi'ni açar ve bir iskelet iletisi oluşturur. Olarak yazılan nesnelerini *Url* sıradan köprü olarak görüntülenir.

Aşağıdaki örnekte nasıl alanları işaretlenmiş gösterir.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Bağlantılar ile DynamicHyperLink denetimi oluşturma

Dinamik veri Web sitesi eriştiklerinde, son kullanıcıların gördüğü URL'leri denetlemek için .NET Framework 3.5 SP1 içinde eklenen yeni yönlendirme özelliği kullanır. Yeni *DynamicHyperLink* denetim dinamik veri sitesi sayfalarına bağlantılar oluşturmak kolaylaştırır. Aşağıdaki örnekte nasıl kullanılacağını gösterir *DynamicHyperLink* denetimi:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Bu biçimlendirme için liste sayfaya götüren bir bağlantı oluşturur ve `Products` tanımlanan yollar temel tablo `Global.asax` dosya. Denetim dinamik veri sayfası dayanır varsayılan tablo adı otomatik olarak kullanır.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Veri modelindeki devralma desteği

Entity Framework ve LINQ-SQL devralma veri modellerini destekler. Bunun bir örneği olan bir veritabanında olabilir bir `InsurancePolicy` tablo. De içerebileceğini `CarPolicy` ve `HousePolicy` aynı alanlara sahip tablolar `InsurancePolicy` ve daha fazla alan ekleyin. Dinamik veri için veri modelindeki devralınan nesneleri anlamak ve devralınan tablolar için askılamayı desteklemek için değiştirildi.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Çok-çok ilişkileri (yalnızca Entity Framework) desteği

Entity Framework üzerinde bir ilişki bir koleksiyon olarak göstererek uygulanan tablolar arasında çok-çok ilişkileri zengin desteği olan bir *varlık* nesnesi. Yeni `ManyToMany.ascx` ve `ManyToMany_Edit.ascx` alan şablonları, görüntüleme ve çok-çok ilişkilerde yer verileri düzenleme desteği sağlamak için eklenmiştir.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Denetim görüntüleme ve Destek numaralandırma için yeni öznitelikler

*DisplayAttribute* alanları görüntülenme üzerinde ek denetim vermek için eklendi. *DisplayName* önceki sürümlerinde, bir alan için bir resim yazısı olarak kullanılan adını değiştirmek izin verilen dinamik veri özniteliği. Yeni *DisplayAttribute* sınıfı sırayla bir alan görüntülenir ve bir alanın filtre olarak kullanılan gibi bir alanı görüntülemek için daha fazla seçenek belirtmenize olanak sağlar. Öznitelik etiketler için kullanılan ad bağımsız denetim de sağlar. bir *GridView* denetim, kullanılan adı bir *DetailsView* , Yardım metni alan için kontrol eder ve filigran kullanılır alan (alanın metin girişi kabul ederse).

*EnumDataTypeAttribute* sınıfı, numaralandırmalar için alanlarını eşleme izin verecek şekilde eklendi. Bu öznitelik bir alan için geçerli olduğunda, bir numaralandırma türü belirtin. Dinamik veri kullanan yeni `Enumeration.ascx` görüntüleme ve numaralandırma değerlerini düzenlemek için kullanıcı Arabirimi oluşturmak üzere alan şablonu. Şablon numaralandırması adlarında veritabanından değerlere eşler.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Filtreler için gelişmiş destek

Dinamik veri 1.0 Boolean sütunları ve yabancı anahtar sütunları için yerleşik filtreleri ile birlikte gelir. Filtreler görüntülenen her ya da bunların hangi sırayla görüntülenen belirtin izin vermedi. Yeni *DisplayAttribute* özniteliği adreslerini hem, vererek bu sorunları kontrol bir sütun filtre olarak görüntülenip görüntülenmeyeceğini üzerinden ve ne sipariş görüntülenecektir.

Bir ek destek filtreleme re olduğunu yeniliktir[yeni kullanmak için yazılmış](#0.2__QueryExtender "_QueryExtender") Web Forms özelliğidir. Bu bilgi filtreleri ile kullanılan veri kaynağı denetiminin gerek kalmadan filtreler oluşturmanızı sağlar. Bu uzantılar yanı sıra, filtreler de şablon denetimlere yenilerini eklemenize olanak sağlayan kapatıldı. Son olarak, *DisplayAttribute* daha önce bahsedilen sınıfı verir, aynı geçersiz kılınacak varsayılan filtre biçimi *UIHint* geçersiz kılınacak bir sütun için varsayılan alan şablon sağlar.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web geliştirme geliştirmeleri

Visual Studio 2010 Web geliştirme büyük CSS uyumluluk, HTML ve ASP.NET biçimlendirme parçacıkları ve yeni dinamik IntelliSense JavaScript aracılığıyla verimliliği için geliştirilmiştir.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Geliştirilmiş CSS uyumluluk

Visual Studio 2010 Visual Web Developer tasarımcısında CSS 2.1 standartları uyumluluğu artırmak için güncelleştirilmiştir. Tasarımcı daha iyi HTML kaynağını bütünlüğünü korur ve daha sağlamdır Visual Studio'nun önceki sürümleri. Başlık altında mimari geliştirmeler daha iyi işleme, Düzen ve Bakım yapılabilirliğini gelecekteki iyileştirmeleri etkinleştirin, yapılmıştır.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML ve JavaScript kod parçacıkları

HTML Düzenleyicisi'nde, IntelliSense otomatik-etiket adları tamamlar. IntelliSense kod parçacıkları özelliği otomatik-tüm etiketleri ve daha fazlasını tamamlar. Visual Studio 2010'da, C# ve Visual Basic, Visual Studio'nun önceki sürümlerde desteklenen yanında JavaScript IntelliSense kod parçacıkları desteklenir.

Visual Studio 2010'u içerir gerekli öznitelikler de dahil olmak üzere otomatik tamamlama ortak ASP.NET ve HTML etiketleri, Yardım 200 parçacıkları (gibi runat = "server") ve ortak öznitelikleri için bir etiket belirli (gibi *kimliği*,  *DataSourceID*, *ControlToValidate*, ve *metin*).

Ek parçacıkları yükleyebilir veya sizin veya ekibiniz kullanmak için ortak görevler biçimlendirme bloklarını kapsülleyen kendi parçacıkları yazabilirsiniz.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>JavaScript IntelliSense geliştirmeleri

Visual 2010'da, JavaScript IntelliSense daha zengin bir düzenleme deneyimi sağlamak için tasarlanmıştır. IntelliSense şimdi yöntemlerle gibi oluşturulmuş dinamik olarak nesneleri tanıdığı *registerNamespace* ve diğer JavaScript çerçeveleri tarafından kullanılan benzer teknikleri tarafından. Performans, çok az kayıpla veya hiç işleme gecikmesi IntelliSense görüntülenecek ve komut dosyası büyük kitaplıklarının çözümlemek için geliştirilmiştir. Uyumluluk önemli ölçüde farklı kodlama stillerini desteklemek için ve hemen hemen tüm üçüncü taraf kitaplıklar desteklemek üzere çıkarılmıştır. Belge açıklamaları yazın ve IntelliSense tarafından hemen yararlanılabilir şimdi ayrıştırılır.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Visual Studio 2010 ile Web uygulaması dağıtımı

ASP.NET geliştiricilerinin Web uygulaması dağıttığınızda, bunlar genellikle aşağıdaki gibi sorunlar karşılaştıkları bulun:

- Paylaşılan bir barındırma siteye dağıtımı yavaş olabilir FTP gibi teknolojileri gerektirir. Ayrıca, el ile bir veritabanını yapılandırmak için SQL komut dosyaları çalıştırmak gibi görevleri gerçekleştirmeniz gerekir ve bir uygulama olarak bir sanal dizin klasörü yapılandırma gibi IIS ayarlarını değiştirmeniz gerekir.
- Web uygulama dosyaları dağıtılmasına ek olarak bir kuruluş ortamında, yöneticilerin sık sık ASP.NET yapılandırma dosyalarını ve IIS ayarlarını değiştirmeniz gerekir. Veritabanı yöneticileri, bir dizi çalıştıran uygulama veritabanını almak için SQL betikleri çalıştırmanız gerekir. Bu tür yüklemelerin işgücü yoğun, genellikle tamamlamak için saat sürebilir ve dikkatlice belgelenmelidir.

Visual Studio 2010, bu sorunları gidermek ve sorunsuz bir şekilde Web uygulamaları dağıtmanıza imkan sağlayan teknolojileri içerir. Bu teknolojiler IIS Web Dağıtım Aracı'nın (MsDeploy.exe) biridir.

Visual Studio 2010 Web dağıtım özellikleri aşağıdaki önemli alanlar şunlardır:

- Web paketleme
- Web.config dönüştürmesini
- Veritabanı dağıtımı
- Web uygulamaları için tek tıklamayla yayımlama

Aşağıdaki bölümlerde bu özellikler hakkında ayrıntılar sağlanmaktadır.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Web paketleme

Visual Studio 2010 olarak adlandırılır, uygulamanız için bir sıkıştırılmış (.zip) dosyası oluşturmak için MSDeploy Aracı'nı kullanan bir *Web paketi*. Paket dosyası uygulamanızı artı aşağıdaki içerik hakkındaki meta verileri içerir:

- IIS ayarları, uygulama havuzu ayarları, hata sayfası ayarlarını vb. içerir.
- Web sayfaları, kullanıcı denetimleri, statik içerik (görüntü ve HTML dosyaları) vb. içeren gerçek Web içeriği.
- SQL Server veritabanı şemaları ve verileri.
- Güvenlik sertifikaları, GAC, kayıt defteri ayarları ve benzeri yüklenecek bileşenleri.

Bir Web paketi herhangi bir sunucuya kopyalanır ve IIS Yöneticisi'ni kullanarak el ile yüklenmiş. Alternatif olarak, otomatik dağıtım için komut satırı komutlarını kullanarak veya dağıtım API'leri kullanarak paket yüklenebilir.

Visual Studio 2010 sağlayan yerleşik MSBuild görevleri ve Web paketleri oluşturmak için hedefler. Daha fazla bilgi için bkz: [ASP.NET Web uygulaması projesi dağıtımına genel bakış](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) MSDN Web sitesindeki ve [neden oluşturduğunuz Web paketi 10 + 20 nedeniyle](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) Vishal Joshi'nın blogunda.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web.config dönüştürmesini

Web uygulama dağıtımı için Visual Studio 2010 tanıtır [XML belge dönüştürme (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), dönüştürme olanak sağlayan bir özellik olan bir `Web.config` geliştirme ayarları dosyasına üretim ayarlar. Dönüştürme ayarlarını adlı dönüştürme dosyaları içinde belirtilen `web.debug.config`, `web.release.config`ve benzeri. (Bu dosyaların adlarını MSBuild yapılandırmaları eşleşir.) Dönüşüm dosyası yalnızca dağıtılmış için yapmanız gereken değişiklikleri içerir `Web.config` dosya. Basit sözdizimi kullanılarak değişiklikleri belirtin.

Aşağıdaki örnek, bir kısmı gösterir bir `web.release.config` yayın yapılandırmasının dağıtım için üretilen dosyası. Dağıtım sırasında belirtir Değiştir anahtar sözcüğü örnekte *connectionString* düğümünde `Web.config` dosya örnekte listelenen değerleri ile değiştirilecek.

[!code-xml[Main](overview/samples/sample102.xml)]

Daha fazla bilgi için bkz: [Web uygulama projesi dağıtımı için Web.config dönüşümü sözdizimi](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) MSDN'de <a id="0.2_a"> </a> Web sitesi ve[Web Dağıtımı: Web.Config dönüştürmesini](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi'nın blogunda.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Veritabanı dağıtımı

Visual Studio 2010 dağıtım paketi, SQL Server veritabanlarını bağımlılıkları içerebilir. Paket tanımının bir parçası olarak, kaynak veritabanı için bağlantı dizesi girin. Web paketi oluşturduğunuzda, Visual Studio 2010 için veritabanı şeması ve veriler için isteğe bağlı olarak SQL komut dosyaları oluşturur ve bu pakete ekler. Ayrıca, özel SQL komut dosyaları sağlar ve sunucuda çalışacakları sırayı belirtin. Dağıtım sırasında hedef sunucu için uygun bir bağlantı dizesi girin; dağıtım işlemi, bu bağlantı dizesi sonra veritabanı şeması oluşturmak ve veri ekleyen komut dosyalarını çalıştırmak için kullanır.

Ayrıca, tek tıklatmayla kullanarak yayımlamak, uygulama uzak paylaşılan barındırma siteye yayımlandığında, veritabanınızı doğrudan yayımlamak için dağıtım yapılandırabilirsiniz. Daha fazla bilgi için bkz: [nasıl yapılır: bir veritabanı ile bir Web uygulaması projesi dağıtma](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) MSDN Web sitesindeki ve [VS 2010 ile veritabanı dağıtımı](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) Vishal Joshi'nın blogunda.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Web uygulamaları için tek tıklamayla yayımlama

Visual Studio 2010'u da bir Web uygulaması uzak bir sunucuya yayımlamak için IIS Uzaktan Yönetim hizmetinin kullanmanıza olanak sağlar. Sunucuları test veya sunucuları hazırlama veya barındırma hesabınız için bir yayımlama profili oluşturabilirsiniz. Her profil uygun kimlik bilgilerini güvenli bir şekilde kaydedebilirsiniz. Ardından herhangi bir hedef ağa dağıtabilirsiniz sunucuları tek tıklatmayla Web kullanarak tek bir tıklatmayla Yayımla araç çubuğu. Visual Studio 2010 ile MSBuild komut satırını kullanarak yayımlayabilirsiniz. Bu bir sürekli tümleştirme modele yayımlama dahil edilecek takım yapı ortamınızı yapılandırmanıza olanak sağlar.

Daha fazla bilgi için bkz: [nasıl yapılır: bir Web uygulaması projesi kullanarak tek tıklamayla yayımlama ve Web dağıtımı dağıtma](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) MSDN Web sitesindeki ve [Web 1-tıklatma Yayımla VS 2010 ile](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi'nın blogunda. Visual Studio 2010'da Web uygulaması dağıtımını video sunular görüntülemek için bkz [Web Developer Preview VS 2010](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Vishal Joshi'nın blogunda.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Kaynaklar

Aşağıdaki Web sitelerini ASP.NET 4 ve Visual Studio 2010 hakkında ek bilgi sağlar.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — MSDN Web sitesinde ASP.NET 4 için resmi belge.
- [https://www.ASP.NET/](https://www.asp.net/) — ASP.NET ekibin kendi Web sitesi.
- [https://www.ASP.NET/DynamicData/](https://msdn.microsoft.com/library/cc488545.aspx) ve [ASP.NET dinamik veri içerik haritası](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — çevrimiçi kaynaklar ASP.NET ekip sitesi ve ASP.NET dinamik veri için resmi belge.
- [https://www.ASP.NET/AJAX/](../../ajax/index.md) — ASP.NET Ajax geliştirme için ana Web kaynağı.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Visual Studio 2010'da özellikleri hakkında bilgi içeren Visual Web Developer ekip blogu.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — ASP.NET Önizleme sürümleri için ana Web kaynağı.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Sorumluluk reddi

Bu ön belge ve burada açıklanan yazılım son ticari sürümünden önce önemli ölçüde değişebilir.

Bu belgede yer alan bilgileri yayımlandığı tarih itibariyle açıklanan sorunları, Microsoft Corporation'ın geçerli görünümde temsil eder. Microsoft değişen piyasa koşullarına yanıt vermesi gerektiğinden Microsoft tarafında taahhüdü olarak yorumlanmamalıdır ve Microsoft belgenin yayımlanma tarihinden sonra sunulan bilgilerin doğruluğu garanti edemez.

Bu teknik incelemede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ.

İlgili tüm telif hakkı yasalarına uymak kullanıcının sorumluluğundadır. Telif Hakkı altındaki sınırlamaksızın, bu belgenin hiçbir bölümü çoğaltılamaz, depolanan ya da bir geri alma sisteminde sunulan ya da herhangi bir yöntem (elektronik, mekanik, fotokopi, kayıt veya diğer) veya herhangi bir amaçla aktarılan, Microsoft Corporation'ın izni hızlı yazılır.

Microsoft, patentler, patent uygulamaları, ticari markalar, telif hakkı veya bu belgedeki konuyla ilgili diğer fikri mülkiyet hakları sahip olabilir. Microsoft'tan herhangi yazılı Lisans sözleşmesindeki, bu belgeyi bulundurmak, herhangi bir lisans bu patentler, ticari markaları, telif hakkı veya diğer fikri mülkiyet hakkı sağlamaz açıkça belirtilmediği sürece.

Aksi belirtilmediği sürece, örnek şirketler, kuruluşlar, ürünler, etki alanı adları, e-posta adresleri, logolar, kişiler, yerler ve olaylar burada olan kurgusal ve tüm gerçek şirket, kuruluş, ürün, etki alanı adı, e-posta ile ilişki gösterilen adresi, logo, kişi, yer veya olay amaçlanmamıştır veya çıkarılmamalıdır.

© 2009 Microsoft Corporation. Tüm hakları saklıdır.

Microsoft ve Windows kayıtlı ticari markaları ya da Microsoft Corporation'ın ABD'de ve/veya diğer ülkelerdeki ticari markalarıdır.

Burada sözü edilen gerçek şirketler ve ürünler adları, ticari markalar ilgili sahiplerinin olabilir.
