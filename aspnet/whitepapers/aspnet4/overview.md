---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 ve Visual Studio 2010 Web geliştirmeye genel bakış | Microsoft Docs
author: rick-anderson
description: Bu belge, Visual Studio 2010 ve.NET Framework 4'te dahil olan ASP.NET için yeni özelliklerin çoğu, genel bir bakış sağlar.
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 5c7aa95b18bc0a97f42cc981476c110830286fa5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829049"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 ve Visual Studio 2010 Web geliştirmeye genel bakış
====================
> Bu belge, Visual Studio 2010 ve.NET Framework 4'te dahil olan ASP.NET için yeni özelliklerin çoğu, genel bir bakış sağlar.
> 
> [Bu teknik incelemeyi indirin](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**İçindekiler**

**[Çekirdek Hizmetleri](#0.2__Toc253429238 "_Toc253429238")**  
[Web.config dosyası yeniden düzenlemeyi](#0.2__Toc253429239 "_Toc253429239")  
[Genişletilebilir çıktı önbelleği](#0.2__Toc253429240 "_Toc253429240")  
[Web uygulamalarını otomatik olarak başlatma](#0.2__Toc253429241 "_Toc253429241")  
[Bir sayfa kalıcı olarak yönlendirerek](#0.2__Toc253429242 "_Toc253429242")  
[Oturum durumu küçültme](#0.2__Toc253429243 "_Toc253429243")  
[İzin verilen URL'ler aralığının genişletilmesi](#0.2__Toc253429244 "_Toc253429244")  
[Genişletilebilir istek doğrulamayı](#0.2__Toc253429245 "_Toc253429245")  
[Önbelleğe alma nesne ve önbelleğe alma genişletilebilirlik nesne](#0.2__Toc253429246 "_Toc253429246")  
[Genişletilebilir HTML, URL ve HTTP üstbilgi kodlamasını](#0.2__Toc253429247 "_Toc253429247")  
[Bir tek bir çalışan işlemindeki tek tek uygulamalar için performansı izleme](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting'e](#0.2__Toc253429249 "_Toc253429249")

**[AJAX](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery WebForms ve MVC ile eklenen](#0.2__Toc253429251 "_Toc253429251")  
[Content Delivery Network desteği](#0.2__Toc253429252 "_Toc253429252")  
[ScriptManager açık betikleri](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Meta etiketler ve Page.MetaKeywords Page.MetaDescription özellikleri ayarlama](#0.2__Toc253429257 "_Toc253429257")  
[Tek denetimleri için Görünüm durumu etkinleştirme](#0.2__Toc253429258 "_Toc253429258")  
[Değişiklikleri tarayıcı yetenekleri](#0.2__Toc253429259 "_Toc253429259")  
[ASP.NET 4 yönlendirme](#0.2__Toc253429260 "_Toc253429260")  
[İstemci kimlikleri ayarlama](#0.2__Toc253429261 "_Toc253429261")  
[Veri denetimleri içinde kalıcı satır seçimi](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET grafiği denetimi](#0.2__Toc253429263 "_Toc253429263")  
[QueryExtender denetimi ile veri filtreleme](#0.2__Toc253429264 "_Toc253429264")  
[HTML kodlu kod ifadeleri](#0.2__Toc253429265 "_Toc253429265")  
[Proje şablonu değişiklikleri](#0.2__Toc253429266 "_Toc253429266")  
[CSS geliştirmeleri](#0.2__Toc253429267 "_Toc253429267")  
[Div öğeleri etrafında gizlenen alanları gizleme](#0.2__Toc253429268 "_Toc253429268")  
[Şablonlu denetimler için bir dış tablo oluşturma](#0.2__Toc253429269 "_Toc253429269")  
[ListView denetimi geliştirmelerini](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList ve RadioButtonList denetim geliştirmeleri](#0.2__Toc253429271 "_Toc253429271")  
[Menü denetimi geliştirmeleri](#0.2__Toc253429272 "_Toc253429272")  
[Sihirbazı ve CreateUserWizard denetimleri 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Destek alanları](#0.2__Toc253429275 "_Toc253429275")  
[Veri ek açıklama özniteliği doğrulama desteği](#0.2__Toc253429276 "_Toc253429276")  
[Şablonlu Yardımcılar](#0.2__Toc253429277 "_Toc253429277")

**[Dinamik veri](#0.2__Toc253429278 "_Toc253429278")**  
[Var olan projeleri için dinamik veri etkinleştirme](#0.2__Toc253429279 "_Toc253429279")  
[Bildirim temelli bir DynamicDataManager denetimi sözdizimi](#0.2__Toc253429280 "_Toc253429280")  
[Varlık şablonları](#0.2__Toc253429281 "_Toc253429281")  
[URL'ler ve e-posta adresleri için yeni alan şablonları](#0.2__Toc253429282 "_Toc253429282")  
[DynamicHyperLink denetimiyle bağlantılar oluşturarak](#0.2__Toc253429283 "_Toc253429283")  
[Veri modelindeki devralma desteği](#0.2__Toc253429284 "_Toc253429284")  
[Çoktan çoğa ilişkiler (yalnızca varlık çerçevesi) için destek](#0.2__Toc253429285 "_Toc253429285")  
[Denetim görüntüleme ve Destek sabit listeleri için yeni öznitelikler](#0.2__Toc253429286 "_Toc253429286")  
[Gelişmiş Filtreler için destek](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web geliştirme iyileştirmeleri](#0.2__Toc253429288 "_Toc253429288")**  
[CSS uyumluluk geliştirilmiş](#0.2__Toc253429289 "_Toc253429289")  
[HTML ve JavaScript kod parçacıkları](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript IntelliSense geliştirmeleri](#0.2__Toc253429291 "_Toc253429291")

**[Web uygulaması dağıtım Visual Studio 2010 ile](#0.2__Toc253429292 "_Toc253429292")**  
[Web paketleme](#0.2__Toc253429293 "_Toc253429293")  
[Web.config dönüşümünün](#0.2__Toc253429294 "_Toc253429294")  
[Dağıtım veritabanı](#0.2__Toc253429295 "_Toc253429295")  
[Web uygulamaları için tek tıklamayla yayımlama](#0.2__Toc253429296 "_Toc253429296")  
[Kaynakları](#0.2__Toc253429297 "_Toc253429297")

**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Çekirdek Hizmetleri

ASP.NET 4 birkaç çıkış önbelleğe alma ve oturum durumu depolama gibi temel ASP.NET Hizmetleri geliştiren özellikler sunar.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Web.config dosyasını yeniden düzenleme

`Web.config` , Ajax gibi yeni özellikler eklenmiştir gibi bir Web uygulaması .NET Framework'ün son birkaç sürümlere göre önemli ölçüde büyümüştür için yapılandırmayı içeren dosya yönlendirmesi ve IIS 7 ile tümleştirme. Bu, yapılandırma veya Visual Studio gibi bir araç olmadan yeni Web uygulamalarını başlatmak daha zor sağlamıştır. Ana yapılandırma öğeleri kullanarak .NET Framework 4, taşınmış olan `machine.config` dosya ve uygulamalar artık bu ayarları devralır. Böylece `Web.config` ASP.NET 4 uygulamaları boş olması ya da Visual Studio için hangi uygulama tarafından hedeflenen framework sürümü belirten yalnızca aşağıdaki satırları içerecek şekilde dosyasında:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Genişletilebilir çıktı önbelleği

ASP.NET 1.0 sunulan tarihten çıktı önbelleği bellekte oluşturulan sayfalar, denetimler ve HTTP yanıtlarını çıktısını depolamak geliştiricilerin etkinleştirdi. Sonraki Web istekleri, ASP.NET içeriği daha hızlı bir şekilde çıkış sıfırdan yeniden yerine bellekten oluşturulan çıktı alarak hizmet verebilir. Ancak, bu yaklaşımın bir sınırlama vardır; oluşturulan içerik her zaman sahip bellekte depolanması ve yoğun trafik yaşayan sunucularında, çıktı önbelleği tarafından kullanılan bellek bir Web uygulamasının diğer kısımlarından bellek talepleri olan rekabet edebiliyor.

ASP.NET 4, bir veya daha fazla özel çıkış önbelleği sağlayıcısı yapılandırmanıza olanak sağlayan çıktı önbelleği için bir genişletilebilirlik noktası ekler. Çıktı önbellek sağlayıcıları tüm depolama mekanizmasını HTML içeriğini kalıcı hale getirmek için kullanabilirsiniz. Bu, yerel veya uzak diskleri içerebilir, çok çeşitli Kalıcılık mekanizması, bulut depolama için özel çıkış önbelleği sağlayıcılar oluşturmalarına olanak sağlar ve önbelleği altyapıları dağıtılmış.

Yeni türetilen bir sınıf olarak özel bir çıkış önbelleği sağlayıcısı oluşturma *System.Web.Caching.OutputCacheProvider* türü. Ardından sağlayıcı yapılandırabilirsiniz `Web.config` yeni kullanarak dosya *sağlayıcıları* ayrıntılarının *outputCache* öğesi, aşağıdaki örnekte gösterildiği gibi:

[!code-xml[Main](overview/samples/sample2.xml)]

Varsayılan olarak ASP.NET 4, tüm HTTP yanıtlarını sayfaları işlenen ve denetimleri, önceki örnekte gösterildiği gibi bellek içi çıktı önbelleğinde kullanın burada *defaultProvider* özniteliği AspNetInternalProvider için ayarlanır. Farklı bir sağlayıcı adını belirterek bir Web uygulaması için kullanılan varsayılan çıkış önbelleği sağlayıcısı değiştirebilirsiniz *defaultProvider*.

Ayrıca, farklı çıkış önbelleği sağlayıcıları ve istek başına denetimi seçebilirsiniz. Bildirimli olarak bu nedenle yeni kullanarak yapmak için farklı Web kullanıcı denetimi için farklı bir çıktı önbelleği sağlayıcısı seçme en kolay yolu olan *providerName* aşağıdaki örnekte gösterildiği gibi bir denetim yönergesinde özniteliği:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Farklı bir çıkış önbelleği sağlayıcısı için bir HTTP isteği belirterek biraz daha fazla iş gerektirir. Sağlayıcı bildirimli olarak belirtmek yerine, yeni geçersiz kılmanız *GetOuputCacheProviderName* yönteminde `Global.asax` program aracılığıyla belirli bir istek için kullanmak üzere hangi sağlayıcısını belirtmek için dosya. Aşağıdaki örnek bunun nasıl yapılacağı gösterilmektedir.

[!code-csharp[Main](overview/samples/sample4.cs)]

Çıkış önbelleği sağlayıcısı genişletilebilirliği, ASP.NET 4'ın eklenmesiyle, artık çıktı önbelleği stratejileri daha agresif ve daha akıllı Web siteleri için daha sonra amacınızın. Örneğin, artık daha düşük trafik diskte alma sayfaları önbelleğe alma sırasında "İlk 10" sayfaları bellekte bir sitenin önbelleğe almak mümkündür. Alternatif olarak, her değişiklik tarafından işlenen bir sayfa için birleşim önbelleğe ancak bellek tüketimini, ön uç Web sunucularından Boşaltılan dağıtılmış önbellek kullanın.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Web uygulamalarını otomatik olarak başlatma

Bazı Web uygulamaları, büyük miktarlarda veri yükleme veya işleme ilk isteği sunmadan önce pahalı başlatma gerçekleştirmek gerekir. ASP.NET önceki sürümlerinde, bu gibi durumlar için Answering "ASP.NET uygulaması uyandır" ve ardından sırasında başlatma kodu çalıştırmak için özel bir yaklaşım gerekiyordu *uygulama\_yük* yönteminde `Global.asax` dosya.

Adlı yeni bir ölçeklenebilirlik özelliği *otomatik başlatma* adresleri bu senaryo kullanılabilir doğrudan, ASP.NET 4 üzerinde Windows Server 2008 R2'ye IIS 7.5 çalıştığı. Otomatik başlatma özelliği bir uygulama havuzu başlatılıyor, bir ASP.NET uygulamasını başlatma ve ardından HTTP isteklerini kabul etmek için bir kontrollü yaklaşım sağlar.

> [!NOTE] 
> 
> IIS 7.5 için IIS uygulama ısınması Modülü
> 
> IIS takım, IIS 7.5 için uygulama ısınması modülü ilk beta test sürümünü yayımladı. Bu, ısıtma uygulamalarınızı daha önce açıklanan çok daha kolay hale getirir. Özel kod yazmak yerine, Web uygulaması ağdan gelen istekleri kabul etmeden önce yürütülecek kaynaklarının URL'leri belirtin. IIS hizmeti başlatılırken bu Isınma gerçekleşir (IIS uygulama havuzu olarak yapılandırılmışsa *AlwaysRunning*) ve ne zaman bir IIS çalışan işlemi geri dönüştürür. Geri Dönüşüm sırasında eski IIS çalışan işlemi, yeni oluşturulan bir çalışan işlemi tam olarak warmed kadar hiçbir kesintiler veya diğer sorunlar nedeniyle unprimed önbellekler uygulamaların trafiğinde yaşanan böylece istekleri yürütmeye devam eder. Bu modül ASP.NET sürüm 2.0 ile başlayarak, herhangi bir sürümü ile çalıştığını unutmayın.
> 
> Daha fazla bilgi için [uygulama ısınması](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) IIS.NET Web sitesinde. Isınma özelliğini nasıl kullanacağınızı gösteren bir kılavuz için bkz. [IIS 7.5 uygulama ısınması modülü ile çalışmaya başlama](https://www.iis.net/learn/manage) IIS.NET Web sitesinde.


Otomatik başlatma özelliği kullanmak için aşağıdaki yapılandırmayı kullanarak otomatik olarak başlatılmasına IIS 7.5, bir uygulama havuzu IIS Yönetici ayarlar `applicationHost.config` dosyası:

[!code-xml[Main](overview/samples/sample5.xml)]

Aşağıdaki yapılandırmayı kullanarak otomatik olarak başlatılması için tek tek uygulamalar çoklu uygulamaları tek bir uygulama havuzunda içerebileceğinden, belirttiğiniz `applicationHost.config` dosyası:

[!code-xml[Main](overview/samples/sample6.xml)]

Bir IIS 7.5 sunucusu soğuk başlangıç olduğunda veya ayrı bir uygulama havuzu geri dönüştürüldüğünde, IIS 7.5 bilgileri kullanır. `applicationHost.config` otomatik olarak başlatılması için hangi Web uygulamaları gerek belirlemek için dosya. Otomatik başlatma için işaretlenmiş her uygulama için IIS7.5 bir isteği sırasında uygulama geçici olarak kabul etmediği bir durumda uygulamayı başlatmak için ASP.NET 4 için HTTP isteklerini gönderir. Bu durumda, ASP.NET tarafından tanımlanan tür başlatır *serviceAutoStartProvider* (önceki örnekte gösterildiği gibi) özniteliği ve kendi ortak giriş noktası çağırıyor.

Uygulayarak bir yönetilen otomatik başlatma türü ile gerekli giriş noktası oluşturmak *IProcessHostPreloadClient* aşağıdaki örnekte gösterildiği gibi arabirim:

[!code-csharp[Main](overview/samples/sample7.cs)]

Kod, başlatma çalışan *Preload* yöntemi ile yöntem döndürür, ASP.NET uygulama isteklerini işlemek hazır.

Otomatik olarak başlatılacak şekilde.5 IIS ve ASP.NET 4'ın eklenmesiyle, artık ilk HTTP isteği işlemeye önce pahalı uygulama başlatma gerçekleştirmek için iyi tanımlanmış bir yaklaşım vardır. Örneğin, bir uygulama başlatın ve ardından uygulamayı başlatılmış ve HTTP trafiği kabul etmeye hazır bir yük dengeleyici sinyal için yeni otomatik başlatma özelliğini kullanabilirsiniz.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Bir sayfa kalıcı yeniden yönlendirme

Bu sayfaları ve diğer içerik çevresinde birikmesi arama motorları eski bağlantılar açabilir, zaman içinde taşımak için Web uygulamalarında yaygın uygulamadır. ASP.NET'te, geliştiricilerin geleneksel eski URL'leri kullanarak işlenen istekleri *Response.Redirect* bir istek için yeni URL'yi iletmek için yöntemi. Ancak, *yeniden yönlendirme* yöntemi, kullanıcılar eski URL'leri erişmeyi denediğinde, ek bir HTTP gidiş dönüş sonuçları bir HTTP 302 bulundu (geçici yeniden yönlendirme) yanıt verir.

Yeni bir ASP.NET 4 ekler *RedirectPermanent* sorunu HTTP 301 kolaylaştırır yardımcı yöntem yanıtları, aşağıdaki örnekte olduğu gibi kalıcı olarak taşındı:

[!code-csharp[Main](overview/samples/sample8.cs)]

Arama motorları ve kalıcı yeniden yönlendirmeleri tanımak diğer kullanıcı aracıları geçici yeniden yönlendirmeleri tarayıcısı tarafından yapılan gereksiz gidiş dönüş ortadan kaldırır içerikle ilişkili yeni URL depolar.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Oturum durumu daraltma

ASP.NET oturum durumu depolamak üzere bir Web grubunda iki varsayılan seçenekleri sağlar: bir işlem dışı oturum durumu sunucu çağıran bir oturum durumu sağlayıcısı ve bir Microsoft SQL Server veritabanına veri depolayan bir oturum durumu sağlayıcısı. İki seçenek de durum bilgilerini bir Web uygulama çalışan işlemi dışında depolama içerdiğinden, uzak depoya gönderilmeden önce serileştirilecek oturum durumu vardır. Oturum durumu bir geliştirici kaydeder ne kadar bilgi bağlı olarak seri hale getirilmiş veri boyutu oldukça büyük büyüyebilir.

ASP.NET 4, her iki tür işlem dışı oturum durumu sağlayıcıları için yeni bir sıkıştırma seçeneği sunar. Zaman *compressionEnabled* yapılandırma seçeneği aşağıdaki örnekte gösterilen *true*, ASP.NET Sıkıştır (ve sıkıştırmasını) .NETFrameworkkullanarakserihalegetirilmişoturumdurumu *System.IO.Compression.GZipStream* sınıfı.

[!code-xml[Main](overview/samples/sample9.xml)]

Yeni öznitelik için basit eklenmesiyle `Web.config` dosya, Web sunucusundaki yedek CPU çevrimleri uygulamalarla serileştirilmiş oturum durumu verilerinin boyutu önemli azalmalar farkında olun.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>İzin verilen URL'leri aralığını genişletme

ASP.NET 4 uygulama URL'lerini boyutunu genişletme için yeni seçenekler sunuyor. ASP.NET'in önceki sürümlerinde, URL'yi yol uzunluğu 260 karakterden, NTFS dosya yolu sınırına göre kısıtlanmış. ASP.NET 4'te, bu sınır, uygulamalarınız için uygun şekilde (artırmak veya azaltmak için) iki yeni kullanma seçeneğiniz *httpRuntime* configuration öznitelikleri. Aşağıdaki örnek, bu yeni öznitelikler gösterir.

[!code-xml[Main](overview/samples/sample10.xml)]

Daha uzun veya kısaysa yollara (protokol, sunucu adı ve sorgu dizesi içermeyen URL'nin kısmının) izin verecek şekilde değiştirmek *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* özniteliği. Daha uzun veya kısaysa sorgu dizelerine izin vermek için değerini değiştirmek *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* özniteliği.

ASP.NET 4 URL karakter denetimi tarafından kullanılan karakterleri yapılandırmanızı sağlar. ASP.NET içinde bir URL'nin yol bölümü geçersiz bir karakter bulduğunda, isteği reddeder ve bir HTTP 400 hata verir. ASP.NET önceki sürümlerinde, URL karakter denetimleri için sabit bir karakter kümesi sınırlıydı. ASP.NET 4'te yeni kullanarak geçerli karakter kümesini özelleştirebilirsiniz *requestPathInvalidChars* özniteliği *httpRuntime* yapılandırma öğesi, aşağıdaki örnekte gösterildiği gibi:

[!code-xml[Main](overview/samples/sample11.xml)]

Varsayılan olarak, <em>requestPathInvalidChars</em> özniteliği geçersiz olarak sekiz karakter tanımlar. (Atanan dizedeki <em>requestPathInvalidChars</em> varsayılan<em>,</em>küçüktür (&lt;), büyüktür (&gt;) ve ve işareti (&amp;) karakterler çünkü, kodlanmış `Web.config` dosyasını bir XML dosyasıdır.) Geçersiz karakter kümesi, gerektiği şekilde özelleştirebilirsiniz.

> [!NOTE]
> ASP.NET 4 olanlar IETF RFC 2396 tanımlandığı gibi geçersiz bir URL karakteri olduğundan için 0x00 0x1F ASCII aralığındaki karakterler içeren bir URL yolu her zaman reddeder Not ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). IIS 6 çalıştıran Windows Server sürümlerinde veya üzeri http.sys Protokolü aygıt sürücüsü otomatik olarak URL'leri bu karakterlerle reddeder.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Genişletilebilir isteği doğrulama

ASP.NET istek doğrulama için yaygın olarak kullanılan dizelerin gelen HTTP istek verileri siteler arası betik (XSS) saldırıları arar. Olası XSS dizeleri bulunursa istek doğrulamayı şüpheli dize işaretler ve bir hata döndürür. Yerleşik istek doğrulamanın, yalnızca XSS saldırılarını içinde kullanılan en yaygın dizeleri bulduğunda bir hata döndürür. Çok sayıda hatalı pozitif sonuç daha agresif XSS doğrulama yapmak için önceki denemeler sonuçlandı. Ancak, müşteriler, belirli türde istekler veya belirli bir sayfa için daha agresif veya kasıtlı olarak XSS gevşetmek buna karşılık isteyebilirsiniz istek doğrulamayı denetler isteyebilirsiniz.

Özel istek Doğrulama mantığı kullanabilmesi için ASP.NET 4'te, istek doğrulama özelliği Genişletilebilir olarak yapıldı. İstek doğrulamayı genişletmek için yeni türetilen bir sınıf oluşturun. *System.Web.Util.RequestValidator* türü ve uygulama yapılandırma (içinde *httpRuntime* bölümünü`Web.config`dosya) özel bir tür kullanmak için. Aşağıdaki örnek, bir özel talep doğrulama sınıfını Yapılandır gösterilmektedir:

[!code-xml[Main](overview/samples/sample12.xml)]

Yeni *requestValidationType* özniteliği özel istek doğrulamayı sağlar sınıfını belirtir standart .NET Framework tür tanımlayıcı dizesi gerektirir. Her istek için her parça gelen HTTP istek verileri işlemek için özel tür ASP.NET çağırır. URI'den gelen URL ve tüm HTTP üstbilgilerin (tanımlama bilgileri ve özel üst bilgiler), aşağıdaki örnekte gösterildiği gibi bir özel talep doğrulama sınıfı tarafından İnceleme için tüm kullanılabilir:

[!code-csharp[Main](overview/samples/sample13.cs)]

Burada istemediğiniz bir parçasını gelen HTTP verileri incelemek için durumlarda, istek doğrulama sınıfı yalnızca çağrılarak ASP.NET varsayılan istek doğrulamayı bildirmek için geri dönebilir *temel. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Önbelleğe alma nesne ve önbelleğe alma genişletilebilirlik nesne

İlk sürümünden itibaren ASP.NET bir güçlü bellekteki nesne önbelleği dahil olduğunu (*System.Web.Caching.Cache*). Önbellek uygulaması olmayan Web uygulamaları kullanılmış popüler olmuştur. Ancak, bir başvuru eklemek bir Windows Formları ya da WPF uygulaması için garip `System.Web.dll` ASP.NET nesne önbelleği hemen kullanabilmek için.

Önbelleğe alma tüm uygulamalar için kullanılabilir hale getirmek için .NET Framework 4, yeni bir derleme, yeni bir ad alanı, bazı temel türler ve uygulama önbelleğe alma bir somut tanıtır. Yeni `System.Runtime.Caching.dll` derlemeyi içeren yeni bir önbellek API'SİNDE *System.Runtime.Caching* ad alanı. Ad alanı iki çekirdek sınıfları içerir:

- Herhangi bir türde özel önbellek uygulaması oluşturmak için temel sağlayan soyut türler.
- Somut bellekteki nesne önbellek uygulaması ( *System.Runtime.Caching.MemoryCache* sınıfı).

Yeni *MemoryCache* sınıfı modellenmiş yakından önbellekte ASP.NET ve ASP.NET ile iç önbellek altyapısı mantıksal çoğunu paylaşır. Ancak genel önbelleğe alma API'leri *System.Runtime.Caching* ASP.NET kullandıysanız özel önbellekler geliştirmeyi destekleyecek şekilde güncelleştirilmiştir *önbellek* nesne, tanıdık kavramları bulacaksınız yeni API'ler.

Yeni ilgili ayrıntılı bir tartışma *MemoryCache* sınıfı ve temel API'leri destekleyen tüm belgeyi gerektirir. Ancak, aşağıdaki örnekte Yeni önbellek API nasıl çalıştığı hakkında fikir verir. Örnek üzerinde herhangi bir bağımlılığı olmadan bir Windows Forms uygulaması için yazılmıştır `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Genişletilebilir HTML, URL ve kodlama HTTP üstbilgisi

ASP.NET 4'te metin kodlaması aşağıdaki ortak görevler için özel kodlama rutinleri oluşturabilirsiniz:

- HTML kodlaması.
- URL kodlaması.
- HTML öznitelik kodlaması.
- Kodlama giden HTTP üstbilgileri.

Yeni türetilerek, özel bir kodlayıcı oluşturabilirsiniz *System.Web.Util.HttpEncoder* yazın ve ardından ASP.NET yapılandırma özel türünde kullanılacak *httpRuntime* bölümünü `Web.config` dosyası olarak Aşağıdaki örnekte gösterilen:

[!code-xml[Main](overview/samples/sample15.xml)]

ASP.NET özel bir kodlayıcı yapılandırıldıktan sonra otomatik olarak özel kodlama uygulaması genel olduğunda kodlama yöntemleri çağırır. *System.Web.HttpUtility* veya *System.Web.HttpServerUtility* sınıfları çağrılır. Bu, bir Web geliştirme ekibinin bir parçası agresif karakter kodlamasını Web geliştirme takımın geri kalanı API'leri kodlama genel ASP.NET kullanmaya devam ederken uygulayan özel bir kodlayıcı oluşturma olanak tanır. Özel bir kodlayıcı merkezi olarak yapılandırarak *httpRuntime* öğesi garanti metin kodlaması API'leri kodlama genel ASP.NET çağrılarından tüm özel bir kodlayıcı yönlendirilir.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Bir tek bir çalışan işlemindeki tek tek uygulamalar için performansı izleme

Tek bir sunucu üzerinde barındırılan Web siteleri sayısını artırmak için tek çalışan işleminde birçok barındırıcıların birden çok ASP.NET uygulaması çalıştırın. Ancak, çoklu uygulamaları tek bir paylaşılan çalışan işlemi kullanıyorsanız, sorun yaşıyor tek bir uygulamayı tanımlamak sunucu yöneticileri için zor olur.

ASP.NET 4 CLR tarafından sunulan yeni kaynak izleme işlevselliği yararlanır. Bu özelliği etkinleştirmek için aşağıdaki XML yapılandırması kod parçacığını ekleyebilirsiniz `aspnet.config` yapılandırma dosyası.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Not `aspnet.config` .NET Framework yüklü olduğu dizinde dosyasıdır. Bu `Web.config` dosya.


Zaman *appDomainResourceMonitoring* özelliği etkinleştirilmişse, iki yeni performans sayaçları "ASP.NET uygulamaları" performans kategorisi vardır: *% işlemci zamanı yönetilen* ve  *Kullanılan bellek yönetilen*. Bu performans sayaçlarını her ikisi de tahmini CPU süresi ve tek tek ASP.NET uygulamalarının yönetilen bellek kullanımını izlemek için yeni CLR uygulama etki alanı kaynak yönetimi özelliğini kullanın. Sonuç olarak, ASP.NET 4 ile yöneticiler tek bir çalışan işlemde çalışan tek tek uygulamalar kaynak tüketimini daha ayrıntılı bir görünüm artık sahiptir.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Çoklu Sürüm Desteği

Belirli bir .NET Framework sürümünü hedefleyen bir uygulama oluşturabilirsiniz. ASP.NET 4, yeni bir öznitelikte *derleme* öğesinin `Web.config` dosya hedef .NET Framework 4 ve üzeri olanak tanır. .NET Framework 4 açıkça hedefliyorsanız ve isteğe bağlı öğeler içeriyorsa `Web.config` girdilerini gibi dosya *system.codedom*, bu öğelerin .NET Framework 4 için doğru olması gerekir. (.NET Framework 4 açıkça oluşturmayın, hedef Framework'ü bir girişe eksikliği gösterilir `Web.config` dosyası.)

Aşağıdaki örnek kullanımını gösterir *targetFramework* özniteliğini *derleme* öğesinin `Web.config` dosya.

[!code-xml[Main](overview/samples/sample17.xml)]

Belirli bir .NET Framework sürümünü hedefleme hakkında aşağıdakileri unutmayın:

- Bir .NET Framework 4 uygulama havuzunda ASP.NET derleme sistemi .NET Framework 4 hedef olarak varsayar `Web.config` dosya içermez *targetFramework* özniteliği veya `Web.config` dosyası eksik. (Uygulamanız .NET Framework 4 altında çalışmasını sağlamak için kodlama değişiklik gerekebilir.)
- Dahil etmezseniz *targetFramework* özniteliği ve *system.codeDom* öğe içinde tanımlanan `Web.config` dosya, bu dosya için .NET Framework 4 doğru girişleri içermelidir.
- Kullanıyorsanız *aspnet\_derleyici* (derleme ortamında olduğu gibi), uygulamanızın zaman önceden derleneceği komutu, doğru sürümünü kullanmalısınız *aspnet\_derleyici* Hedef çerçeve için komutu. Sevk derleyici, .NET Framework 3.5 ve önceki sürümleri için derlemek için .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) kullanın. Birlikte gelen derleyici ile .NET Framework 4 bu çerçeve veya sonraki sürümleri kullanılarak oluşturulan uygulamalar derlemek için kullanın.
- Çalışma zamanında derleyici bilgisayarda yüklü olan son framework derlemeleri kullanır (ve bu nedenle GAC'deki). Bir güncelleştirme framework daha sonra denenirse (örneğin, bir kuramsal sürümü 4.1 yüklü), olsa bile daha yeni bir çerçeve sürümünde özelliklerini kullanmayı mümkün olacaktır *targetFramework* özniteliği hedefleyen daha düşük bir sürüm (4.0 gibi). (Visual Studio 2010 veya kullandığınızda, tasarım zamanında ancak *aspnet\_derleyici* komutunu framework'ün yeni özelliklerini kullanarak derleyici hatalarına neden olur).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>AJAX

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery WebForms ve MVC dahil

WebForms ve MVC hem Visual Studio şablonları, açık kaynaklı jQuery kitaplığı içerir. Yeni Web sitesi ya da proje oluşturduğunuzda, aşağıdaki 3 dosyalarını içeren bir komut klasör oluşturulur:

- jQuery 1.4.1.js – insanlar tarafından okunabilen, jQuery kitaplığı unminified.
- jQuery-14.1.min.js – jQuery kitaplığı küçültülmüş sürümü.
- jQuery-1.4.1-vsdoc.js – jQuery kitaplık için IntelliSense belge dosyası.

Bir uygulama geliştirirken jQuery unminified sürümünü içerir. Üretim uygulamaları için jQuery küçültülmüş sürümünü içerir.

Örneğin, aşağıdaki Web Forms sayfası, ASP.NET TextBox denetimleri bunlar odağa sahip olduğunda sarı arka plan rengini değiştirmek için jQuery nasıl kullanabileceğinizi gösterir.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Content Delivery Network desteği

Microsoft Ajax Content Delivery Network (CDN), ASP.NET Ajax ve jQuery betikler, Web uygulamalarınıza kolayca eklemenizi sağlar. Örneğin, yalnızca ekleyerek jQuery kitaplığı kullanmaya başlayabilmeniz için bir `<script>` Ajax.microsoft.com için şöyle işaret eden sayfanıza etiketi:

[!code-html[Main](overview/samples/sample19.html)]

Microsoft Ajax CDN avantajlarından yararlanarak, Ajax uygulamalarınızın performansını önemli ölçüde artırabilir. Microsoft Ajax CDN içeriğini, tüm dünyada bulunan sunucularda önbelleğe alınır. Ayrıca, farklı etki alanlarında bulunan Web siteleri için önbelleğe alınan JavaScript dosyaları yeniden tarayıcılar Microsoft Ajax CDN sağlar.

Microsoft Ajax Content Delivery Network, Güvenli Yuva Katmanı'nı kullanarak bir web sayfası hizmet gerektiği durumlarda SSL (HTTPS) destekler.

CDN kullanılamıyorsa bir geri dönüş uygulayın. Geri dönüş test edin.

Microsoft Ajax CDN hakkında daha fazla bilgi için aşağıdaki Web sitesini ziyaret edin:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

Microsoft Ajax CDN ASP.NET ScriptManager destekler. Yalnızca ayar bir özelliği tarafından EnableCdn özelliği CDN'den tüm ASP.NET framework JavaScript dosyaları alabilir:

[!code-aspx[Main](overview/samples/sample20.aspx)]

ASP.NET framework EnableCdn özellik değeri true olarak ayarladıktan sonra doğrulama ve UpdatePanel için kullanılan tüm JavaScript dosyaları dahil olmak üzere CDN tüm ASP.NET framework JavaScript dosyaları alır. Bu bir özelliğin ayarlanması bir çarpıcı web uygulamanızın performansını etkileyebilir.

WebResource özniteliğini kullanarak kendi JavaScript dosyaları için CDN yolu ayarlayabilirsiniz. Yeni CdnPath özelliği EnableCdn özellik değeri true olarak ayarlandığında kullanılan CDN yolunu belirtir:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>ScriptManager açık betikleri

ASP.NET ScriptManger kullandıysanız geçmişte, sonra tamamı tek parça ASP.NET Ajax kitaplığı yüklemek için gerekli olmuştur. Yeni ScriptManager.AjaxFrameworkMode özelliğin avantajlarından yararlanarak, tam olarak ASP.NET Ajax Kitaplığı'nın hangi bileşenlerin yükleneceğini denetlemek ve yalnızca gereksinim duyduğunuz ASP.NET Ajax kitaplığı bileşenlerini yükleyin.

ScriptManager.AjaxFrameworkMode özelliği aşağıdaki değerlere ayarlayabilirsiniz:

- Etkin--ScriptManager denetimi otomatik olarak birleşik bir betiği her core framework betiğinin (eski davranış) MicrosoftAjax.js betik dosyasını içerdiğini belirtir.
- Devre dışı--tüm Microsoft Ajax komut dosyası özellikleri devre dışı bırakılır ve ScriptManager denetimini herhangi bir komut dosyası otomatik olarak başvurmuyor olduğunu belirtir.
- Açık--sayfanızı gerektiren tek framework core komut dosyası için komut dosyası başvuruları açıkça dahil edilir ve her komut dosyası gerektiren bağımlılıklar için başvuruları içereceğini belirtir.

Örneğin, açık değerine AjaxFrameworkMode özelliğini ayarlarsanız, gereken belirli ASP.NET Ajax bileşen komut belirtebilirsiniz:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

Web Forms, ASP.NET core özelliğinde ASP.NET 1.0 yayımlandıktan sonra geldi. ASP.NET 4, aşağıdakiler dahil olmak üzere bu alanda birçok iyileştirme olmuştur:

- Özelliğine *meta* etiketler.
- Görünüm durumu üzerinde daha fazla denetim.
- Tarayıcı özellikleri ile çalışmak için daha kolay yollar sağlar.
- ASP.NET Web Forms ile yönlendirme kullanma desteği.
- Oluşturulan kimlikleri hakkında daha fazla denetime.
- Seçilen satırları veri denetimleri içinde kalıcı yeteneği.
- İşlenmiş HTML hakkında daha fazla denetime *FormView* ve *ListView* kontrol eder.
- Veri kaynağı denetimleri filtreleme desteği.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Meta etiketlerle Page.MetaKeywords ve Page.MetaDescription özelliklerini ayarlama

ASP.NET 4 için iki özellik ekler *sayfa* sınıfı *MetaKeywords* ve *MetaDescription*. Bu iki özellik temsil eden karşılık gelen *meta* aşağıdaki örnekte gösterildiği gibi sayfanızın etiketler:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Bu iki özellik aynı çalışma biçimi sayfanın *başlık* özelliği yok. Bunlar, bu kuralları izleyin:

1. Varsa hiçbir *meta* bulunan etiketleri *baş* özellik adları eşleşen öğe (diğer bir deyişle, ad için "anahtar sözcükler" = *Page.MetaKeywords* ve ad için"description"= *Page.MetaDescription*, bu özellikleri ayarlanmamış anlamına gelir), *meta* etiketleri işlendiğinde sayfasına eklenir.
2. Varsa *meta* şu adlara sahip etiketler, bu özellikleri hareket almanıza ve ayarlamanıza yöntemleri için mevcut olan etiketlerin içeriğini gibi.

Olanak sağlayan bir veritabanı veya başka bir kaynaktan içeriği almak ve olanak sağlayan kümesi ne dinamik olarak tanımlamak için etiketleri çalışma zamanında bu özellikleri ayarlayabilirsiniz belirli bir sayfada içindir.

Ayrıca *anahtar sözcükleri* ve *açıklaması* özelliklerinde *@ sayfa* Web Forms sayfası biçimlendirme, aşağıdaki örnekte olduğu gibi üst kısmındaki yönerge:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Bu geçersiz kılar *meta* etiketi içeriği (varsa) sayfasında zaten bildirildi.

Açıklama içeriğini *meta* etiket Google önizlemelerinde listeleme arama iyileştirmek için kullanılır. (Ayrıntılar için bkz [meta açıklama yeniliği kod parçacıklarıyla geliştirmek](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) Google yayımlanması merkezi blogunda.) Google ve Windows Live Search anahtar sözcükleri içeriği için herhangi bir şey kullanmayın, ancak diğer arama motorları olabilir. Daha fazla bilgi için [Meta Anahtar sözcükleri öneriler](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) arama motoru Kılavuzu Web sitesinde.

Bu yeni özellikleri basit bir özelliğidir, ancak bunları el ile ekleme gereksiniminden veya oluşturmak için kendi kodunuzu yazma, kaydettiğiniz *meta* etiketler.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Görünüm durumu tek tek denetimler için etkinleştirme

Varsayılan olarak, uygulama için gerekli olmasa bile sayfasında her denetimin görünüm durumunu potansiyel olarak depolar sonucuyla sayfanın görünüm durumu etkin. Görünüm durumu verilerinin biçimlendirme içinde bir sayfa oluşturur ve geri gönderin ve bir sayfa istemciye göndermek için gereken süre miktarını artırır dahildir. Gerekli olandan daha fazla görünüm durumu depolamak üzere önemli performans düşüşüne neden olabilir. ASP.NET'in önceki sürümlerinde, geliştiricilerin sayfa boyutunu azaltmak için tek tek denetimler için Görünüm durumu devre dışı bırakılamadı, ancak bu nedenle açıkça için tek denetimleri yapmak zorunda. ASP.NET 4'te, Web sunucusu denetimleri içeren bir *ViewStateMode* görünüm durumu, varsayılan olarak devre dışı bırakın ve sayfanın gerektiren denetimleri için etkinleştirme olanak tanıyan özellik.

*ViewStateMode* üç değerlerine sahip bir sabit listesi özelliği alır: *etkin*, *devre dışı bırakılmış*, ve *devral*. *Etkin* görünüm durumu ayarlanmış olan tüm alt denetimler ve denetim için etkinleştirir *devral* veya hiçbir şey ayarlanmış olması gerekir. *Devre dışı bırakılmış* görünüm durumu, devre dışı bırakır ve *devral* denetim kullandığını belirtir *ViewStateMode* üst denetimini ayarlama.

Aşağıdaki örnekte gösterildiği nasıl *ViewStateMode* özelliği çalışır. İşaretleme ve kod şu sayfaya denetimleri içeren değerleri *ViewStateMode* özelliği:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Gördüğünüz gibi kod PlaceHolder1 denetimi için Görünüm durumu devre dışı bırakır. Bu özellik değeri alt label1 denetimi devralır (*devral* için varsayılan değerdir *ViewStateMode* denetimler için) ve bu nedenle hiçbir görünüm durumunu kaydeder. PlaceHolder2 denetiminde *ViewStateMode* ayarlanır *etkin*, etiket 2 Bu özellik devralır ve kayıt durumu görüntüleyin. Sayfa ilk yüklendiğinde *metin* her iki özellik *etiket* denetimleri "[DynamicValue]" dizesi olarak ayarlayın.

Bu ayarlar sayfa ilk kez yüklendiğinde, aşağıdaki çıktıyı tarayıcıda görüntülenen sahiptir:

Devre dışı `: [DynamicValue]`

Etkin:`[DynamicValue]`

Bir geri gönderme sonra ancak aşağıdaki çıktıyı görüntülenir:

Devre dışı `: [DeclaredValue]`

Etkin:`[DynamicValue]`

Label1 denetimi (olan *ViewStateMode* değeri ayarı *devre dışı bırakılmış*) kodda ayarlandığı değeri korunur değil. Ancak, etiket 2 denetleme (olan *ViewStateMode* değeri ayarı *etkin*) durumuna korunur.

Ayrıca *ViewStateMode* içinde *@page* yönergesi, aşağıdaki örnekte olduğu gibi:

[!code-aspx[Main](overview/samples/sample26.aspx)]

*Sayfa* sınıf yalnızca başka bir denetimi; sayfadaki diğer tüm denetimler için üst denetimi işlevi görür. Varsayılan değer olan *ViewStateMode* olduğu *etkin* için örneklerini *sayfa*. Denetimler için varsayılan nedeni *devral*, denetimleri devralacaktır *etkin* özellik değerini ayarlamadığınız sürece *ViewStateMode* sayfasının veya denetiminin düzeyinde.

Değerini *ViewStateMode* özellik, Görünüm durumu yalnızca korunur, belirler *EnableViewState* özelliği *true*. Varsa *EnableViewState* özelliği *false*, Görünüm durumu değil saklanabilir bile *ViewStateMode* ayarlanır *etkin*.

Bu özellik için iyi bir kullanımı olan *ContentPlaceHolder* ayarlayabildiğiniz ana sayfalar, denetimleri *ViewStateMode* için *devre dışı bırakılmış* için ana sayfa ve etkinleştirin için ayrı ayrı *ContentPlaceHolder* sırayla gerektiren denetimleri içeren denetimler durumu görüntüleyin.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Tarayıcı yetenekleri değişiklikler

ASP.NET belirler sitenizin adı verilen bir özellik kullanarak göz atmak için kullanarak bir kullanıcı tarayıcının yeteneklerini *tarayıcı yetenekleri*. Tarayıcı özellikleri tarafından temsil edilir *HttpBrowserCapabilities* nesne (tarafından kullanıma sunulan *Request.Browser* özelliği). Örneğin, kullanabileceğiniz *HttpBrowserCapabilities* türü ve sürümü geçerli tarayıcının JavaScript belirli bir sürümü destekleyip desteklemediğini belirlemek üzere nesne. Veya, kullanabileceğiniz *HttpBrowserCapabilities* isteğin bir mobil CİHAZDAN kaynaklandığı olup olmadığını belirlemek için nesne.

*HttpBrowserCapabilities* nesnesi, bir dizi tarayıcı tanımı dosyaları tarafından yönlendirilir. Bu dosyalar, belirli tarayıcılar özellikleri hakkında bilgi içerir. ASP.NET 4'te, bu tarayıcı tanım dosyalarını yakın zamanda sunulan tarayıcılar ve Google Chrome, araştırma gibi hareket BlackBerry akıllı telefonlar ve Apple iPhone cihazları hakkında bilgi içerecek şekilde güncelleştirildi.

Aşağıdaki liste, yeni tarayıcı tanım dosyalarını gösterir:

- *blackberry.browser*
- *chrome.browser*
- *Default.browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Tarayıcı yetenekleri sağlayıcıları kullanma

ASP.NET sürüm 3.5 Service Pack 1'de, aşağıdaki yollarla bir tarayıcı olan özellikleri tanımlayabilirsiniz:

- Bilgisayar düzeyinde oluştur veya güncelleştir bir `.browser` aşağıdaki klasörde bulunan XML dosyası:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Tarayıcı özelliklerine tanımladıktan sonra gelen Visual Studio komut tarayıcı yetenekleri derlemesi yeniden oluşturun ve GAC'ye eklemek için istemi aşağıdaki komutu çalıştırın:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Tek bir uygulama için oluşturduğunuz bir `.browser` uygulamanın dosyasında `App_Browsers` klasör.

XML dosyaları değiştirmek Bu yaklaşımları gerektirir ve aspnet çalıştırdıktan sonra bilgisayar düzeyinde değişiklikler için uygulamayı yeniden başlatmanız gerekir\_regbrowsers.exe işlem.

ASP.NET 4 olarak adlandırılan bir özellik içeren *tarayıcı yetenekleri sağlayıcıları*. Adından da anlaşılacağı gibi bu sayede sırayla olanak sağlayan bir sağlayıcı oluşturmak kendi kodunuzu tarayıcı yetenekleri belirlemek için kullanın.

Uygulamada, geliştiriciler genellikle özel tarayıcı yetenekleri tanımlamaz. Tarayıcı dosyaları güncelleştirmek işlem güncelleştiren oldukça karmaşıktır için sabit olan ve XML sözdizimi `.browser` dosyaları tanımlayın ve karmaşık olabilir. Ne bu işlem çok daha kolay hale getirir, ortak bir tarayıcı tanımı sözdizimleri varsa veya güncel tarayıcı tanımlarını veya hatta bir Web hizmeti gibi bir veritabanı için veritabanı bir değil. Yeni tarayıcı yetenekleri sağlayıcıları özelliği bu senaryolar için üçüncü taraf geliştiriciler mümkün ve pratik hale getirir.

Yeni ASP.NET 4 tarayıcı yetenekleri sağlayıcısı özelliğini kullanmak için iki ana yaklaşım vardır: ASP.NET tarayıcı yetenekleri tanımı işlevselliği genişletmek veya tamamen değiştirme. Aşağıdaki bölümlerde, ilk işlevselliğini nasıl değiştirileceğini ve genişletmek nasıl açıklanmaktadır.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>ASP.NET tarayıcı yetenekleri işlevselliği değiştirme

ASP.NET tarayıcı yetenekleri tanımı işlevselliğini tamamen değiştirmek için bu adımları izleyin:

1. Türetilen bir sağlayıcı sınıfı oluşturma *HttpCapabilitiesProvider* ve bu geçersiz kılmaları *GetBrowserCapabilities* yöntemi, aşağıdaki örnekte olduğu gibi: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Bu örnekteki kod yeni bir oluşturur *HttpBrowserCapabilities* nesne, yalnızca tarayıcı ve söz konusu özellik ayarlamak için MyCustomBrowser adlı özelliğini belirterek.
2. Sağlayıcı, uygulama ile kaydedin. 

    Sağlayıcı bir uygulama ile kullanmak için eklemelisiniz *sağlayıcısı* özniteliğini *browserCaps* konusundaki `Web.config` veya `Machine.config` dosyaları. (Sağlayıcı öznitelikleri de tanımlayabilirsiniz bir *konumu* uygulamasında, belirli bir mobil cihaz için bir klasör gibi belirli dizinleri için öğesi.) Aşağıdaki örnek nasıl ayarlanacağını gösterir *sağlayıcısı* özniteliği bir yapılandırma dosyası:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Yeni bir tarayıcı özelliği tanımı kaydetmek için başka bir kod, aşağıdaki örnekte gösterildiği gibi kullanmaktır:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Bu kod çalıştırması gerekir *uygulama\_Başlat* olayı `Global.asax` dosya. Değiştirmek için *BrowserCapabilitiesProvider* sınıfı önbellek çözümlenen için geçerli bir durumda kaldığından emin olmak için uygulamada herhangi bir kod yürütülmeden önce nkarakterler *HttpCapabilitiesBase* nesne.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>HttpBrowserCapabilities nesne önbelleğe alma

Yukarıdaki örnekte, kod her zaman özel bir sağlayıcı almak için çağırılır çalışır bir sorunlu *HttpBrowserCapabilities* nesne. Bu, her isteği sırasında birden çok kez gerçekleşebilir. Örnekte, sağlayıcı kodunu çok yapmaz. Özel sağlayıcınızın kodda önemli iş gerçekleştiriyorsa ancak almak için sipariş *HttpBrowserCapabilities* nesnesi, bu performansı etkileyebilir. Bunun gerçekleşmesini önlemek için önbelleğe alabilir *HttpBrowserCapabilities* nesne. Aşağıdaki adımları uygulayın:

1. Türetilen bir sınıf oluşturmanız *HttpCapabilitiesProvider*, aşağıdaki örnekte bir ister: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Örnekte, kod bir özel BuildCacheKey yöntemi çağırarak bir önbellek anahtarı oluşturur ve bir özel GetCacheTime yöntemi çağrılarak önbelleğine sürenin uzunluğunu alır. Ardından kodu çözülmüş ekler *HttpBrowserCapabilities* önbelleğe nesne. Nesneyi önbellekten alınabilir ve üzerinde yeniden olun sonraki istekleri özel sağlayıcı kullanın.
2. Sağlayıcı uygulama ile bir önceki yordamda açıklandığı şekilde kaydedin.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>ASP.NET tarayıcı yetenekleri işlevselliğini genişletme

Yeni bir önceki bölümde açıklanan *HttpBrowserCapabilities* ASP.NET 4'te bir nesne. Ayrıca, yeni tarayıcı yetenekleri tanımları zaten ASP.NET'te olanlar ekleyerek ASP.NET tarayıcı yetenekleri işlevselliği genişletebilirsiniz. XML tarayıcı tanımlarını kullanmadan bunu yapabilirsiniz. Aşağıdaki yordamda gösterildiği nasıl.

1. Türetilen bir sınıf oluşturmanız *HttpCapabilitiesEvaluator* ve bu geçersiz kılmaları *GetBrowserCapabilities* yöntemi, aşağıdaki örnekte gösterildiği gibi: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Bu kodu, tarayıcı belirlemeye ilk ASP.NET tarayıcı yetenekleri işlevselliğini kullanır. Hiçbir tarayıcı tanımladıysanız ancak istekte tanımlanan bilgilere göre (diğer bir deyişle, *tarayıcı* özelliği *HttpBrowserCapabilities* nesnesi olan "Bilinmiyor" dizesi), kod çağrıları Özel sağlayıcı (MyBrowserCapabilitiesEvaluator tarayıcıyı tanımlamak için).
2. Sağlayıcı, önceki örnekte açıklandığı gibi uygulama ile kaydedin.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Var olan yetenekleri tanımları için yeni özellikler ekleyerek tarayıcı yetenekleri işlevselliğini genişletme

Bir özel tarayıcı tanımı sağlayıcı oluşturma ek olarak ve yeni tarayıcı tanımları dinamik olarak oluşturmak için ek özelliklere sahip mevcut tarayıcı tanımlarını genişletebilirsiniz. Bu, istediğiniz yakın olan ancak yalnızca birkaç özellik eksik bir tanımı kullanmanıza olanak sağlar. Bunu yapmak için aşağıdaki adımları kullanın.

1. Türetilen bir sınıf oluşturmanız *HttpCapabilitiesEvaluator* ve bu geçersiz kılmaları *GetBrowserCapabilities* yöntemi, aşağıdaki örnekte gösterildiği gibi: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Kod örneği, mevcut ASP.NET genişletir *HttpCapabilitiesEvaluator* sınıfı ve alır *HttpBrowserCapabilities* nesnesini geçerli istek tanımını aşağıdaki kod kullanarak eşleşir :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Kodu daha sonra ekleyebilir veya bir özelliği bu tarayıcıda değiştirebilirsiniz. Yeni bir tarayıcı özellik belirtmenin iki yolu vardır:

    - Bir anahtar/değer çifti Ekle *IDictionary* tarafından sunulan nesnenin *özellikleri* özelliği *HttpCapabilitiesBase* nesne. Önceki örnekte, kod MultiTouch değeriyle adlı bir özellik ekler *true*.
    - Mevcut özellikleri ayarlayın *HttpCapabilitiesBase* nesne. Önceki örnekte, kod ayarlar *çerçeveler* özelliğini *true*. Bu özellik için yalnızca bir erişimci olduğundan *IDictionary* tarafından sunulan nesnenin *özellikleri* özelliği. 

        > [!NOTE]
        > Bu model herhangi bir özelliği için geçerli unutmayın *HttpBrowserCapabilities*, Denetim bağdaştırıcılarını dahil.
2. Sağlayıcı, önceki yordamda açıklandığı şekilde uygulama ile kaydedin.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>ASP.NET 4 yönlendirme

ASP.NET 4, Web Forms ile yönlendirme kullanma için yerleşik destek ekler. Yönlendirme sağlar kabul etmek için bir uygulama yapılandırma fiziksel dosyaları eşlemeyin URL'leri isteyin. Bunun yerine, yönlendirme, arama motoru iyileştirmesi (SEO) uygulamanıza yardımcı olabilir ve kullanıcılar için anlamlı olan URL'ler tanımlamak için kullanabilirsiniz. Örneğin, var olan bir uygulamada ürün kategorilerini görüntüleyen bir sayfa URL'si aşağıdaki örnekteki gibi görünebilir:

[!code-console[Main](overview/samples/sample36.cmd)]

Yönlendirme kullanarak, aynı bilgileri işlemek için aşağıdaki URL'yi kabul etmek için uygulamayı yapılandırabilirsiniz:

[!code-console[Main](overview/samples/sample37.cmd)]

Yönlendirme ASP.NET 3.5 SP1 ile başlayarak kullanılmaktadır. (ASP.NET 3.5 SP1'de Yönlendirme kullanma örneği için bkz: Giriş [yönlendirme WebForms ile kullanarak](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "bu giriş başlığı.") Phil Haack'ın blogunda.) Ancak, ASP.NET 4, aşağıdakiler dahil olmak üzere yönlendirme, kullanılacak kolaylaştıran bazı özellikleri içerir:

- *PageRouteHandler* yollar tanımlarken kullanan basit bir HTTP işleyicisini olan sınıf. Sınıf veri isteği yönlendirilir sayfasına geçirir.
- Yeni özellikleri *HttpRequest.RequestContext* ve *Page.RouteData* (Bu değer için bir proxy *HttpRequest.RequestContext.RouteData* nesne). Bu özellikler yolun geçirilen bilgilere erişmek için kolaylaştırır.
- İçinde tanımlanan aşağıdaki yeni ifade oluşturucularının *System.Web.Compilation.RouteUrlExpressionBuilder* ve *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, bir ASP.NET sunucu denetimi içinde bir yönlendirme URL'si için karşılık gelen URL'yi oluşturmak için basit bir yol sağlar.
- *RouteValue*, bilgileri ayıklamak için basit bir yöntem sunan *RouteContext* nesne.
- *RouteParameter* bulunan verileri geçirmek kolaylaştıran sınıfı bir *RouteContext* nesne veri kaynağı denetimi için bir sorguya (benzer şekilde [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Web formları sayfaları için yönlendirme

Aşağıdaki örnek, yeni kullanarak bir Web Forms yol tanımlamak gösterilmektedir *MapPageRoute* yöntemi *rota* sınıfı:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 tanıtır *MapPageRoute* yöntemi. Aşağıdaki örnek önceki örnekte gösterilen SearchRoute tanımı eşdeğerdir, ancak kullandığı *PageRouteHandler* sınıfı.

[!code-csharp[Main](overview/samples/sample39.cs)]

Örnek kodda, bir fiziksel sayfa için rotayı eşler (ilk rotada için `~/search.aspx`). İlk rota tanımını ayrıca searchterm adlı parametre URL'den ayıklanır ve sayfaya başarılı olduğunu belirtir.

*MapPageRoute* yöntemi aşağıdaki yöntem aşırı yüklemeleri destekler:

- *MapPageRoute (dize Routetablename, dize routeUrl, dize physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (dize Routetablename, dize routeUrl, dize physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary varsayılan)*
- *MapPageRoute (dize Routetablename, dize routeUrl, dize physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary Varsayılanları, RouteValueDictionary kısıtlamaları)*

*CheckPhysicalUrlAccess* parametresi, rota yönlendiriliyor fiziksel sayfa için güvenlik izinlerini denetleyip denetlemeyeceğini belirtir (Bu durumda, search.aspx) ve gelen URL için izinleri (Bu durumda, arama / {searchterm}). Varsa değerini *checkPhysicalUrlAccess* olduğu *false*, yalnızca gelen URL izinlerini denetlenir. Bu izinleri tanımlanan `Web.config` ayarları aşağıdaki gibi kullanarak dosya:

[!code-xml[Main](overview/samples/sample40.xml)]

Örnek yapılandırma fiziksel sayfasına erişim reddedildi `search.aspx` Yönetici rolüne sahiptirler olanlar dışındaki tüm kullanıcılar için. Zaman *checkPhysicalUrlAccess* parametrenin ayarlanmış *true* (varsayılan değer olan), yalnızca yönetici kullanıcılar izin {searchterm} URL /search/ erişim fiziksel sayfa search.aspx olduğundan Bu roldeki kullanıcılar kısıtlı. Varsa *checkPhysicalUrlAccess* ayarlanır *false* ve site, önceki örnekte gösterilen şekilde yapılandırılır, tüm kimliği doğrulanmış kullanıcılara URL /search/ {searchterm} erişmesine izin verilir.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Bir Web formları sayfasında yönlendirme bilgilerini okuma

Web Forms fiziksel sayfa kodunda yönlendirme URL'den ayıklanan bilgilere erişebilirsiniz (veya başka bir nesne eklendi diğer bilgileri *RouteData* nesnesi) iki yeni özellikleri kullanarak:  *HttpRequest.RequestContext* ve *Page.RouteData*. (*Page.RouteData* sarmalar *HttpRequest.RequestContext.RouteData*.) Aşağıdaki örnek nasıl kullanılacağını gösterir *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Kod örneği yolda daha önce tanımlanan searchterm parametresi için geçirilen değer ayıklar. Aşağıdaki istek URL'si göz önünde bulundurun:

[!code-console[Main](overview/samples/sample42.cmd)]

Ne zaman bu isteği yapıldığında, "tan" olarak işlenecek word `search.aspx` sayfası.

#### <a name="accessing-routing-information-in-markup"></a>Biçimlendirme yönlendirme bilgilerine erişme

Önceki bölümde açıklanan yöntemi, Web Forms sayfası kodunda rota verilerini alma işlemi gösterilmektedir. Biçimlendirme içinde aynı bilgilere erişmenizi ifadeleri de kullanabilirsiniz. İfade oluşturucular, bildirim temelli kod ile çalışmak için güçlü ve şık bir yoludur. (Daha fazla bilgi için bkz: Giriş [Express kendiniz ile özel ifade oluşturucular](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) Phil Haack'ın blogunda.)

ASP.NET 4 WebForms yönlendirme için iki yeni ifade derleyicileri içerir. Aşağıdaki örnek, bunların nasıl kullanılacağını gösterir.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Örnekte, *RouteUrl* ifadesi, bir rota parametresini temel alan bir URL tanımlamak için kullanılır. Bu kod biçimlendirme tam URL'ye sahip kaydeder ve URL yapısı, daha sonra bu bağlantı için herhangi bir değişiklik gerektirmeden değiştirmenize olanak tanır.

Bu işaretleme, daha önce tanımlanan yola göre aşağıdaki URL'yi oluşturur:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET, doğru yola otomatik olarak çalışır (diğer bir deyişle, doğru URL'yi oluşturur) giriş parametreleri temel alarak. Bir rota adı kullanmak için bir yol belirtmenize imkan tanıyan ifadesinde de ekleyebilirsiniz.

Aşağıdaki örnek nasıl kullanılacağını gösterir *RouteValue* ifade.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Bu denetimi içeren bir sayfa çalıştığında, değeri "tan" etikette görüntülenir.

*RouteValue* ifade kolaylaştırır rota verilerini biçimlendirmede kullanın ve daha karmaşık Page.RouteData["x ile çalışmak zorunda önler"] söz dizimi biçimlendirme.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Veri kaynağı denetim parametreleri için rota verilerini kullanma

*RouteParameter* sınıf rota verileri için sorgular veri kaynak denetimi, bir parametre değeri olarak belirtmenize olanak sağlar. Bunu [gibi çalışır çok](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) aşağıdaki örnekte gösterildiği gibi sınıfı:

[!code-aspx[Main](overview/samples/sample46.aspx)]

Bu durumda, rota parametresi searchterm değeri için kullanılacak @companyname parametresinde <em>seçin</em> deyimi.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Ayar istemci kimlikleri

Yeni *ClientIDMode* özelliği, ASP.NET'te değindiği bir sorunu giderir yani denetimleri oluşturma *kimliği* işlenen öğelere öznitelik. Bilerek *kimliği* işlenmiş öğelere öznitelik, uygulamanız bu öğelere başvuran istemci komut dosyası içeriyorsa, önemlidir.

*Kimliği* Web sunucusu denetimleri için işlenen HTML öznitelik göre oluşturulan *ClientID* denetiminin özelliği. ASP.NET 4, oluşturma algoritmasını kadar *kimliği* özniteliğini *ClientID* özellik Kimliğine sahip ve yinelenen denetimlerinde (olarak söz konusu olduğunda adlandırma kapsayıcısı (varsa) birleştirmek için oluştu bir önek ve sıralı numara eklemek için veri denetimleri). Bu her zaman sayfasındaki denetimleri kimlikleri benzersiz olduğunu garanti olsa da algoritma denetim tahmin edilebilir değildi ve bu nedenle istemci komut dosyası başvurusu zor kimliklerinin sonuçlandı.

Yeni *ClientIDMode* özelliği istemci kimliği denetimler için daha kesin bir şekilde nasıl oluşturulduğunu belirtmenize olanak sağlar. Ayarlayabileceğiniz *ClientIDMode* özellik sayfası dahil olmak üzere herhangi bir denetimi için. Olası ayarlar şunlardır:

- *AutoID* – bu oluşturma algoritmasını değerine eşdeğer olan *ClientID* ASP.NET'in önceki sürümlerinde kullanılan özellik değerleri.
- *Statik* – bu belirten *ClientID* değeri kapsayıcılarını adlandırmayla üst kimliklerini birleştirme olmadan kimliği ile aynı olacaktır. Bu, Web kullanıcı denetimlerindeki yararlı olabilir. Web kullanıcı denetimi farklı sayfalarında ve farklı bir kapsayıcı denetimleri bulunur bu nedenle, kullanan denetimler için istemci komut dosyası yazmak zor olabilir *AutoID* algoritma kimliği değerleri ne olacağını tahmin edemezsiniz nedeni .
- *Tahmin edilebilir* – bu seçenek öncelikle yinelenen şablonları kullanan veri denetimleri kullanmak için kullanılabilir. Denetimin adlandırma kapsayıcılar, ancak oluşturulan kimliği özelliklerini birleştiren *ClientID* "ctlxxx" gibi dize değerleri içermiyor. Bu ayar ile birlikte çalışır *ClientIDRowSuffix* denetiminin özelliği. Ayarladığınız *ClientIDRowSuffix* özelliğini bir veri alanı adını ve bu alanın değeri kullanılan sonek olarak oluşturulan *ClientID* değeri. Genellikle bir veri kaydı olarak birincil anahtarı kullanır *ClientIDRowSuffix* değeri.
- *Devralınan* – Bu ayar denetimler için varsayılan davranıştır; bir denetimin kimliği oluşturma, üst ile aynı olduğunu belirtir.

Ayarlayabileceğiniz *ClientIDMode* sayfa düzeyinde özelliği. Bu varsayılan tanımlar *ClientIDMode* geçerli sayfadaki tüm denetimler için değer.

Varsayılan *ClientIDMode* sayfa düzeyinde değer *AutoID*ve varsayılan *ClientIDMode* denetim düzeyi değer *devral*. Bu özellik her yerde kodunuzda değil ayarlarsanız, sonuç olarak, tüm denetimler varsayılan *AutoID* algoritması.

Sayfa düzeyi değeri kümesinde *@page* yönergesi, aşağıdaki örnekte gösterildiği gibi:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Ayrıca *ClientIDMode* yapılandırma dosyasında (makine) bilgisayar düzeyinde veya uygulama düzeyinde değeri. Bu varsayılan tanımlar *ClientIDMode* uygulamadaki tüm sayfalarındaki tüm denetimler için ayarlama. Varsayılan değer bilgisayar düzeyinde ayarlarsanız, tanımladığı *ClientIDMode* o bilgisayardaki tüm Web siteleri için ayarlama. Aşağıdaki örnekte gösterildiği *ClientIDMode* yapılandırma dosyasında ayarı:

[!code-xml[Main](overview/samples/sample48.xml)]

Değerini daha önce belirtildiği gibi *ClientID* özelliği için bir denetimin üst adlandırma kapsayıcıdan türetilmiştir. Ana sayfalar kullanırken olduğu gibi bazı senaryolarda denetimleri kimliklerle olanlar için aşağıdaki gibi HTML işlenen kalabilirsiniz:

[!code-html[Main](overview/samples/sample49.html)]

Olsa da *giriş* biçimlendirme içinde gösterilen öğesinin (gelen bir *TextBox* denetimi) yalnızca iki adlandırma kapsayıcılar olan derin sayfasındaki (iç içe *ContentPlaceholder* denetimleri), ana sayfa işlendi en nedeniyle, sonuç aşağıdaki gibi bir denetim kimliği yoludur:

[!code-console[Main](overview/samples/sample50.cmd)]

Bu kimliği sayfanın benzersiz olması garanti edilmez, ancak gereksiz yere uzun çoğu amaçları içindir. İşlenmiş kimliği kısaltın ve Kimliği'nin nasıl oluşturulacağını hakkında daha fazla denetime sahip olmasını istediğinizi düşünün. (Örneğin, "ctlxxx" ön eklerini kaldırmak istediğiniz.) Bunu yapmanın en kolay yolu ayardır *ClientIDMode* aşağıdaki örnekte gösterildiği gibi özelliği:

[!code-aspx[Main](overview/samples/sample51.aspx)]

Bu örnekte *ClientIDMode* özelliği *statik* en dıştaki için *NamingPanel* öğesi ve kümesine *tahmin edilebilir* İç için *NamingControl* öğesi. (Sayfa ve ana sayfaya geri kalanı için önceki örnekte olduğu gibi aynı olarak kabul edilir) aşağıdaki biçimlendirme içinde bu ayarları neden:

[!code-html[Main](overview/samples/sample52.html)]

*Statik* ayarının etkisini adlandırma hiyerarşi içinde en dıştaki herhangi bir denetim için sıfırlama *NamingPanel* öğesi ve ortadan *ContentPlaceHolder* ve *AnaSayfa* oluşturulan kimliği kimlikleri ( *Adı* işlenmiş öğeleri özniteliktir etkilenmeyen, normal ASP.NET işlevselliği için olayları tutulur, böylece durumu görüntülemek ve benzeri.) İşaretleme için taşıma bir yan etkisi, adlandırma hiyerarşi sıfırlama olsa bile *NamingPanel* öğeleri başka bir *ContentPlaceholder* denetimi, işlenen istemci kimliği aynı kalır.

> [!NOTE]
> İşlenen denetim kimlikleri benzersiz olduğundan emin olun size olduğuna dikkat edin. Değilse, istemci gibi bireysel HTML öğeleri için benzersiz kimliklerinin gerektiren herhangi bir işlevsellik bozabilir *document.getElementById* işlevi.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Tahmin edilebilir istemci kimlikleri içinde verilere bağlı denetimler oluşturma

*ClientID* eski algoritması tarafından veri bağlantılı liste denetiminde denetimleri olabilir, oluşturulan değerleri uzun ve gerçekten tahmin edilebilir değildir. *ClientIDMode* işlevselliği, daha fazla denetlemek nasıl bu kimliklerinin oluşturulan sahip yardımcı olabilir.

Aşağıdaki örnek biçimlendirmede içeren bir *ListView* denetimi:

[!code-aspx[Main](overview/samples/sample53.aspx)]

Önceki örnekte, *ClientIDMode* ve *RowClientIDRowSuffix* özellikleri biçimlendirme içinde ayarlanır. *ClientIDRowSuffix* özelliği yalnızca verilere bağlı denetimler kullanılabilir ve davranışını denetleyen bağlı olarak kullandığınızdan farklıdır. Farklar şunlardır:

- *GridView* denetimi; bir veya daha fazla sütun adı veri kaynağında istemci kimlikleri oluşturmak için çalışma zamanında birleştirilir belirtebilirsiniz. Örneğin, ayarlarsanız *RowClientIDRowSuffix* "ProductName, ProductID", işlenmiş öğeleri aşağıdaki gibi bir biçimde olan kimlikleri denetimi:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* denetim — istemci kimliği için eklenmiş veri kaynağındaki tek bir sütun belirtin Örneğin, ayarlarsanız *ClientIDRowSuffix* "ProductName" kimlikleri işlenmiş denetimi aşağıdaki gibi bir biçim elde edersiniz:

- [!code-console[Main](overview/samples/sample55.cmd)]

- Bu durumda sondaki 1, geçerli veri öğesi seri Numaranızı elde edilir.

- *Yineleyici* denetim — bu denetiminin desteklemediği *ClientIDRowSuffix* özelliği. İçinde bir *Repeater* denetimi, geçerli satır dizinini kullanılır. ClientIDMode kullandığınızda, "Tahmin edilebilir" ile = bir *Repeater* denetlemek, kimlikleri oluşturulan istemci, aşağıdaki biçime sahiptir:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Sondaki 0 geçerli satırın dizinidir.

*FormView* ve *DetailsView* denetimleri desteği için birden çok satır görüntülemez *ClientIDRowSuffix* özelliği.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Veri denetimleri içinde kalıcı satır seçimi

*GridView* ve *ListView* denetimler bir satırı seçin, kullanıcılara olanak sağlar. ASP.NET önceki sürümlerinde, seçimi sayfasında satır dizini temel. Örneğin, 1 sayfasında üçüncü öğeyi seçin ve sonra 2 sayfaya gidin, söz konusu sayfada bulunan üçüncü öğe seçilir.

Kalıcı seçimi, .NET Framework 3.5 SP1 içindeki dinamik veri projelerinde yalnızca başlangıçta destekleniyordu. Bu özellik etkin olduğunda, öğeye ait veri anahtar geçerli seçili öğe dayanır. Başka bir deyişle, 1 sayfasında üçüncü satırı seçin ve 2 sayfasına gitme, hiçbir şey 2 sayfasında seçilir. Üçüncü satır, sayfa 1'i yeniden taşıdığınızda, yine de seçilir. Kalıcı bir seçim için artık desteklenmektedir *GridView* ve *ListView* denetimleri kullanarak tüm projelerde *EnablePersistedSelection* gösterildiği özelliği Aşağıdaki örnekte:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>ASP.NET grafik denetimi

ASP.NET *grafik* denetimi, .NET Framework Veri görselleştirme teklifleri genişletir. Kullanarak *grafik* denetimi, karmaşık istatistik ve finansal analiz için sezgisel ve görsel açıdan ilgi çekici grafikler içeren ASP.NET sayfaları oluşturabilirsiniz. ASP.NET *grafik* denetimi eklenti olarak .NET Framework sürüm 3.5 SP1 sürümüne eklenmiştir ve .NET Framework 4 yayın bir parçasıdır.

Denetim, aşağıdaki özellikleri içerir:

- 35 farklı grafik türlerinde kullanılır.
- Grafik alanları, başlıklarını, açıklamaları ve ek açıklamalar, sınırsız sayıda.
- Tüm grafik öğeleri için Görünüm ayarlarını birçok farklı.
- 3B desteği çoğu grafik türünde.
- Veri noktası otomatik olarak sığabilen akıllı veri etiketleri.
- Şerit çizgileri, Ölçek bölme çizgisi ve Logaritmik ölçeklendirme.
- 50'den fazla finansal ve istatistiksel formülleri verileri çözümleme ve dönüştürme.
- Basit bağlama ve grafik verilerinin işlenmesini.
- Tarih, saat ve para birimi gibi ortak veri biçimleri için destek.
- Etkileşim ve istemci de dahil olmak üzere, olay temelli özelleştirme desteği Ajax kullanarak olayları tıklayın.
- Durum Yönetimi.
- İkili akış.

Aşağıdaki şekil, ASP.NET grafik denetimi tarafından üretilen finansal grafikleri örnekleri gösterir.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Şekil 2: ASP.NET grafik denetimine örnekler

Daha fazla örnek ASP.NET grafik denetiminin nasıl kullanılacağını karşıdan yükleme için örnek kod [Microsoft Grafik denetimleri için örnekleri ortamı](https://go.microsoft.com/fwlink/?LinkId=128300) sayfası MSDN Web sitesinde. Daha fazla örnekleri topluluğu, içerik bulabilirsiniz [grafik denetimi Forumu](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Grafik denetimi bir ASP.NET sayfasına ekleme

Aşağıdaki örnek nasıl ekleneceğini gösterir. bir *grafik* biçimlendirme kullanarak bir ASP.NET sayfasına denetimi. Örnekte, *grafik* denetim statik veri noktaları için bir sütun grafik oluşturur.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>3B grafikleri kullanma

*Grafik* denetimi içeren bir *ChartAreas* içerebilen koleksiyonu *ChartArea* grafik alanı özellikleri tanımlayan nesne. Örneğin, 3B grafik alanı için kullanılacak kullanın *Area3DStyle* özelliği aşağıdaki örnekteki gibi:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Bir 3B grafik dört dizi ile aşağıdaki şekilde gösterilmektedir *çubuğu* grafik türü.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Şekil 3: 3B çubuk grafik

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Ölçek bölme çizgisi ve Logaritmik ölçeklerde kullanma

Ölçek bölme çizgisi ve Logaritmik ölçeklerde Gelişmiş algoritmaların grafiğe eklemek için iki ek yöntemleridir. Bu özellikler, grafik alanında her ekseni özgüdür. Örneğin, bir grafik alanı birincil Y Ekseninde bu özellikleri kullanmak için kullanmak *AxisY.IsLogarithmic* ve *ScaleBreakStyle* özelliklerinde bir *ChartArea* nesne. Aşağıdaki kod parçacığı, birincil Y Ekseninde ölçek bölme çizgisi kullanmayı gösterir.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Aşağıdaki şekilde Y eksen ölçek bölme çizgisi etkin gösterir.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Şekil 4: Ölçek bölme çizgisi

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>QueryExtender denetimi ile verileri filtreleme

Verilere filtre uygulamak için veri odaklı Web sayfaları oluşturmak geliştiriciler için çok yaygın bir görev olduğundan. Bu geleneksel oluşturarak gerçekleştirildikten *burada* yan tümcelerinde veri kaynağı denetimleri. Bu yaklaşım, karmaşık olabilir ve bazı durumlarda *burada* söz dizimi, temel alınan veritabanı tam işlevselliğini yararlanmanızı vermez.

Yeni bir filtre daha kolay hale getirmek için *QueryExtender* denetimi, ASP.NET 4'te eklendi. Bu denetimin eklenebilir *EntityDataSource* veya *LinqDataSource* bu denetimleri tarafından döndürülen verileri filtrelemek için kontrol eder. Çünkü *QueryExtender* denetimi LINQ'ye kullanır, çok da verimli işlemlerinde sonuçları sayfası için veri gönderilmeden önce veritabanı sunucusunda filtre uygulanır.

*QueryExtender* denetimi, çeşitli filtreleme seçeneklerini destekler. Aşağıdaki bölümlerde, bu seçenekler açıklanmaktadır ve bunları nasıl kullanacağınızı örnekleri sağlar.

#### <a name="search"></a>Ara

Arama seçeneği için *QueryExtender* denetimi, belirtilen alanlara bir arama gerçekleştirir. Aşağıdaki örnekte, içeriğini arar ve TextBoxSearch denetimi, girilen metni ekleyeceğini `ProductName` ve `Supplier.CompanyName` öğesinden döndürülen bulunan veri sütunlarının *LinqDataSource* denetimi.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Aralık

Range seçeneği arama seçeneğine benzer, ancak bir değerler aralığını tanımlamak için çift belirtir. Aşağıdaki örnekte, *QueryExtender* aramaları denetlemek `UnitPrice` döndürülen veriler sütununda *LinqDataSource* denetimi. Sayfadaki TextBoxFrom ve TextBoxTo denetimlerinden aralığı okuyun.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Özellik ifadesi seçeneği bir karşılaştırma özellik değerini tanımlamanızı sağlar. İfade değerlendirme sonucu *true*, incelenmekte olan veriler döndürülür. Aşağıdaki örnekte, *QueryExtender* denetimi verilerde karşılaştırarak veri filtreler `Discontinued` sayfasında CheckBoxDiscontinued denetiminden sütun değeri.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Son olarak, ile kullanılacak özel bir ifade belirtebilirsiniz *QueryExtender* denetimi. Bu seçenek özel filtre mantığı tanımlayan sayfa içinde bir işlevi çağırmayı sağlar. Aşağıdaki örnek, özel bir ifadede bildirimli olarak belirtmek gösterilmektedir *QueryExtender* denetimi.

[!code-aspx[Main](overview/samples/sample64.aspx)]

Aşağıdaki örnek tarafından çağrılan özel işlev gösterir *QueryExtender* denetimi. Bu durumda, içeren bir veritabanı sorgusu kullanarak yerine bir *burada* yan tümcesi, kodu kullanan bir LINQ Sorgu verilerini filtrelemek için.

[!code-csharp[Main](overview/samples/sample65.cs)]

Bu örnekler yalnızca tek bir ifade içinde kullanılan *QueryExtender* denetimi birer güncelleştirir. Ancak, birden çok ifadelerinin içinde içerebilir *QueryExtender* denetimi.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>HTML kodlu kod ifadeleri

Bazı ASP.NET siteleri (özellikle ile ASP.NET MVC) yoğun kullanır `<%` =  `expression %>` sözdizimi (genellikle "nuggets code" olarak adlandırılır) metin yanıta yazılacak. Kod ifadelerini kullandığınızda, HTML kodlama-metin kullanıcıdan geliyorsa, metin girişi, sayfaları açık XSS (siteler arası komut dosyası) saldırının bırakılabilir unutmak çok kolaydır.

ASP.NET 4, aşağıdaki yeni kod sözdizimi ifadeler sözdizimi sunar:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Bu sözdizimi, varsayılan olarak, yanıtı yazarken HTML kodlaması kullanır. Bu yeni ifade etkili bir şekilde aşağıdaki ifadeyi:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Örneğin, &lt;%: İstek ["UserInput"] %&gt; değerini HTML kodlaması gerçekleştirir *["UserInput"] istek*.

Bu özellik, böylece her adımda hangisinin kullanılacağını karar vermek için zorlanmaz tüm örneklerini eski sözdizimi ile yeni söz dizimini değiştirmek mümkün kılmak için hedeftir. Ancak, bu durumda bu çift kodlama için yol açabilir çıkış olan metin HTML olacak şekilde tasarlanmış veya zaten kodlanır durumlar vardır.

Bu gibi durumlarda, yeni bir arabirimi, ASP.NET 4 tanıtır *IHtmlString*, somut bir uygulama ile birlikte *HtmlString*. Bu tür örnekleri, dönüş değeri zaten düzgün şekilde kodlanmış (veya aksi halde incelenir,) HTML olarak görüntülemek için ve bu nedenle değeri yeniden HTML ile kodlanmış gerektiğini belirten sağlar. Örneğin, aşağıdaki olmamalıdır (ve değil) HTML kodlu:

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET 4 çalıştırırken ASP.NET MVC 2 yardımcı yöntemler kodlanmış çift olmadıkları bu yeni söz dizimi ile çalışacak şekilde güncelleştirildi, ancak tek. ASP.NET 3.5 SP1'i kullanarak bir uygulamayı çalıştırdığınızda, bu yeni söz diziminin çalışmaz.

Bu XSS saldırılarına karşı koruma garantilemez aklınızda bulundurun. Örneğin, tırnak içinde olmayan bir öznitelik değerleri kullanan HTML saldırılara açıktır kullanıcı girişi içerebilir. ASP.NET denetimlerini ve ASP.NET MVC Yardımcıları çıktısı her zaman öznitelik değerleri tırnak içine, önerilen yaklaşımdır içerdiğini unutmayın.

Benzer şekilde, bu sözdizimini kullanıcı girişini temel alarak bir JavaScript dize oluştururken gibi JavaScript kodlama gerçekleştirmez.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Proje şablonu değişiklikleri

Yeni bir Web sitesi veya Web uygulaması projesi oluşturmak için Visual Studio kullandığınızda ASP.NET önceki sürümlerinde, elde edilen projeleri yalnızca bir Default.aspx sayfasında, varsayılan içeriyor. `Web.config` dosyasını ve `App_Data` aşağıda gösterildiği gibi klasörü çizim:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio ayrıca hiç, aşağıdaki resimde gösterildiği gibi dosya içermiyor bir boş Web sitesi projesi türünü destekler:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Başlangıç için olduğunu çok az Kılavuzu bir üretim Web uygulaması derleme hakkında sonucudur. Bu nedenle, ASP.NET 4 üç yeni şablonlar, bir boş Web uygulaması projesi için bir tane ve her biri için bir Web uygulaması ve Web sitesi projesi tanıtır.

#### <a name="empty-web-application-template"></a>Boş Web uygulaması şablonu

Adından da anlaşılacağı gibi boş bir Web uygulaması stripped-down bir Web uygulaması proje şablonudur. Bu proje şablonu aşağıdaki şekilde gösterildiği gibi Visual Studio yeni proje iletişim kutusundan seçin:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Tam boyutlu görüntüyü görmek için tıklatın](overview/_static/image8.png))

Boş bir ASP.NET Web uygulaması oluşturduğunuzda, Visual Studio aşağıdaki klasör düzeni oluşturur:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Boş Web sitesi düzenine benzer bir özel durum dışında ASP.NET'in daha önceki sürümlerin budur. Visual Studio 2010'da, aşağıdaki en düşük boş Web uygulaması ve boş Web sitesi projeleri içeren `Web.config` projenin hedeflediği çerçeve tanımlamak için Visual Studio tarafından kullanılan bilgileri içeren dosya:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Bu olmadan *targetFramework* özelliği, eski uygulama açılırken uyumluluğu korumak için .NET Framework 2. 0'ı hedefleyen Visual Studio varsayılanlarını.

#### <a name="web-application-and-web-site-project-templates"></a>Web uygulaması ve Web sitesi projesi şablonları

Visual Studio 2010 ile birlikte gelen diğer iki yeni proje şablonları, önemli değişiklikler içerir. Aşağıdaki şekilde, yeni bir Web uygulaması projesi oluşturduğunuzda, oluşturulan proje düzene gösterilmektedir. (Bir Web sitesi projesi düzenini neredeyse aynıdır.)

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Projenin önceki sürümlerinde oluşturulmayan dosyalarının sayısını içerir. Ayrıca, yeni bir Web uygulama projesi, yeni uygulama erişimi güvenliğini hızla başlamanıza imkan tanıyan temel üyelik işlevselliğini ile yapılandırılır. Bu ekleme nedeniyle `Web.config` için üyelik, roller ve Profiller yapılandırmak için kullanılan girişleri yeni projeyi içeren dosya. Aşağıdaki örnekte gösterildiği `Web.config` dosya için yeni bir Web uygulama projesi. (Bu durumda, *roleManager* devre dışı bırakılır.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Tam boyutlu görüntüyü görmek için tıklatın](overview/_static/image14.png))

Projeyi ikinci de içeren `Web.config` dosyası `Account` dizin. İkinci bir yapılandırma dosyası, oturum olmayan kullanıcılar ChangePassword.aspx sayfasına erişim güvenliğini sağlamak için bir yol sağlar. Aşağıdaki örnek, ikinci içeriğini gösterir `Web.config` dosya.

![](overview/_static/image15.png)

Yeni proje şablonları, varsayılan olarak oluşturulan sayfaları, daha önceki sürümlerde daha fazla içerik de içerir. Varsayılan ana sayfayı ve CSS dosyası proje içerir ve varsayılan sayfa (Default.aspx) varsayılan olarak ana sayfayı kullanmak üzere yapılandırılır. Web uygulaması veya Web sitesi ilk kez çalıştırdığınızda, varsayılan (ana) sayfası zaten işlevsel olduğunu sonucudur. Aslında, yeni bir MVC uygulaması başlattığınızda gördüğünüz varsayılan sayfasına benzer.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Tam boyutlu görüntüyü görmek için tıklatın](overview/_static/image18.png))

Proje şablonlarına yapılan bu değişiklikleri amacınıza, yeni bir Web uygulaması oluşturmaya başlamak hakkında rehberlik sağlamaktır. Anlamsal olarak doğru katı XHTML 1.0 ile uyumlu biçimlendirme ve CSS kullanarak belirtilen düzene sahip şablonları sayfaları, ASP.NET 4 Web uygulamaları oluşturmaya yönelik en iyi temsil eder. Varsayılan sayfa, bir kolayca özelleştirebilirsiniz bir iki sütunlu düzeni de vardır.

Örneğin, yeni bir Web uygulaması için bazı renkleri değiştirme ve şirket logonuzu My ASP.NET Application logosu yerine eklemek istediğiniz düşünün. Bunu yapmak için altında yeni bir dizin oluşturma `Content` logo resmi depolamak için:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Görüntü sayfasına eklemek için ardından açın `Site.Master` dosya, burada My ASP.NET Application metin tanımlanan değiştirin bulabilir ve bir *görüntü* öğesi olan *src* özniteliğini yeni logo ayarlayın Aşağıdaki örnekte olduğu gibi görüntüsü:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Tam boyutlu görüntüyü görmek için tıklatın](overview/_static/image22.png))

Ardından, Site.css dosyasına gidin ve yanı sıra başlık, sayfa arka plan rengini değiştirmek için CSS sınıf tanımlarını değiştirin.

Bu değişiklikler, çok az çabayla özelleştirilmiş giriş sayfası görüntüleyebilirsiniz sonucudur:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Tam boyutlu görüntüyü görmek için tıklatın](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>CSS geliştirmeleri

Genel ASP.NET 4'te çalışma alanlarını biri son HTML standartlarıyla uyumlu olan HTML işlemeye yardımcı olmuştur. Bu değişiklik, ASP.NET Web sunucusu denetimleri, CSS stilleri kullanımını içerir.

#### <a name="compatibility-setting-for-rendering"></a>İşleme için Uyumluluk ayarı

Bir Web uygulaması veya Web sitesi .NET Framework 4 hedefleyen, varsayılan olarak *controlRenderingCompatibilityVersion* özniteliği *sayfaları* "4.0" ayarlanır. Bu öğe, makine düzeyinde tanımlı `Web.config` dosya ve varsayılan olarak tüm ASP.NET 4 uygulamaları için geçerlidir:

[!code-xml[Main](overview/samples/sample69.xml)]

Değeri *controlRenderingCompatibility* olası yeni sürüm tanımları gelecek sürümleri izin veren bir dize. Geçerli sürümde bu özellik için aşağıdaki değerleri desteklenir:

- "3.5". Bu ayar, eski işleme ve biçimlendirmeyi gösterir. Biçimlendirme denetimleri tarafından işlenen, %100 geriye dönük olarak uyumlu ve ayarı *xhtmlConformance* özelliği dikkate alınır.
- "4.0". Özelliği bu ayar varsa, ASP.NET Web sunucusu denetimleri aşağıdakileri yapın:
- *XhtmlConformance* özelliği her zaman "Strict" kabul. Sonuç olarak, XHTML 1.0 katı biçimlendirme denetimlerini işlemeye.
- Artık olmayan giriş denetimlerini devre dışı bırakma geçersiz stilleri işler.
- *div* Gizli alanların etrafında öğeleri artık kullanıcı tarafından oluşturulmuş CSS kurallarıyla karışmaması için stili.
- Anlamsal olarak doğru ve erişilebilirlik kuralları ile uyumlu biçimlendirme menü denetimlerini işlemeye.
- Doğrulama denetimleri, satır içi stiller işlenmiyor.
- Kenarlık daha önce işlenen denetimleri = "0" (ASP.NET türetilen denetimler *tablo* denetimi ve ASP.NET *görüntü* denetimi) artık bu öznitelik oluşturma.

#### <a name="disabling-controls"></a>Denetimlerini devre dışı bırakma

ASP.NET 3.5 SP1 ve önceki sürümlerinde, çerçeve işler *devre dışı* herhangi bir alan denetim için HTML biçimlendirmeyi özniteliği *etkin* özelliğini *false*. Ancak, yalnızca HTML 4.01 belirtimine göre *giriş* öğeleri, bu öznitelik olmalıdır.

ASP.NET 4'te, ayarladığınız *controlRenderingCompatabilityVersion* özelliğini "3.5" aşağıdaki örnekte olduğu gibi:

[!code-xml[Main](overview/samples/sample70.xml)]

İşaretleme için oluşturduğunuz bir *etiket* denetimi devre dışı bırakır aşağıdaki gibi denetimi:

[!code-aspx[Main](overview/samples/sample71.aspx)]

*Etiket* denetimi aşağıdaki HTML oluşturmak:

[!code-html[Main](overview/samples/sample72.html)]

ASP.NET 4'te, ayarladığınız *controlRenderingCompatabilityVersion* "4.0". Bu durumda, bu işleme yalnızca denetimleri *giriş* öğeleri işleme bir *devre dışı* ne zaman öznitelik denetimin *etkin* özelliği *false* . HTML işlenmiyor denetimleri *giriş* öğeleri yerine oluşturmak bir *sınıfı* başvuran denetimi için devre dışı bırakılmış bir görünüm tanımlamak için kullanabileceğiniz bir CSS sınıfı özniteliği. Örneğin, *etiket* önceki örnekte gösterilen denetimi aşağıdaki biçimlendirme oluşturmak:

[!code-html[Main](overview/samples/sample73.html)]

"AspNetDisabled" Bu denetim için belirtilen sınıf için varsayılan değerdir. Ancak, statik ayarlayarak bu varsayılan değeri değiştirmek *DisabledCssClass* statik özelliği *WebControl* sınıfı. Denetim geliştiriciler için belirli bir denetim için kullanılacak davranış da kullanılarak tanımlanabilir *SupportsDisabledAttribute* özelliği.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Div öğeleri etrafında gizlenen alanları gizleme

ASP.NET 2.0 ve sonraki sürümler işleme sisteme özel gizli alanların (gibi *gizli* görünüm durumu bilgilerini depolamak için kullanılan öğe) içinde *div* XHTML standardı ile uyum için öğesi. CSS kuralını etkilediğinde ancak, bu soruna yol açabilir *div* bir sayfadaki öğeleri. Örneğin, sayfasında görünen bir piksel boyutunda bir satır sonuçlanabilir yaklaşık gizli *div* öğeleri. ASP.NET 4'te *div* ASP.NET tarafından oluşturulan gizli alanları içine öğeleri aşağıdaki örnekteki gibi CSS sınıf Başvurusu Ekle:

[!code-html[Main](overview/samples/sample74.html)]

Daha sonra yalnızca geçerli bir CSS sınıfı tanımlayabilirsiniz *gizli* aşağıdaki örnekte olduğu gibi ASP.NET tarafından oluşturulan öğeleri:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Şablonlu denetimler için bir dış tablo oluşturma

Varsayılan olarak, şablonları destekleyen aşağıdaki ASP.NET Web sunucusu denetimleri otomatik olarak satır içi stil uygulamak için kullanılan bir dış tablo sarmalanır ve:

- *FormView*
- *Oturum açma*
- *PasswordRecovery*
- *ChangePassword*
- *Sihirbazı*
- *CreateUserWizard*

Adlı yeni bir özellik *RenderOuterTable* biçimlendirmeden kaldırılması dış tablo sağlayan bu denetimler eklenmiştir. Örneğin, aşağıdaki örneği göz önünde bir *FormView* denetimi:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Bu biçimlendirme içeren bir HTML tablosu sayfasında, aşağıdaki çıktıyı oluşturur:

[!code-html[Main](overview/samples/sample77.html)]

Tablo oluşturulmasını önlemek için ayarlayabileceğiniz *FormView* denetimin *RenderOuterTable* özelliği aşağıdaki örnekteki gibi:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Önceki örnek aşağıdaki çıktıyı olmadan işler *tablo*, *tr*, ve *td* öğeleri:

> İçerik


Beklenmeyen etiket denetimi tarafından işlendiği için bu geliştirme, CSS, denetimin içeriği stili kolaylaştırabilir.

> [!NOTE]
> Not Bu değiştirme devre dışı bırakır, Visual Studio 2010 tasarımcıda otomatik biçimlendirme işlevine yönelik destek artık yoktur çünkü bir *tablo* otomatik biçimlendirme seçeneği tarafından oluşturulan bir stil öznitelikleri barındırabilir öğesi.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>ListView denetimi geliştirmeleri

*ListView* denetimi ASP.NET 4'te kullanmak daha kolay hale getirilmiştir. Bilinen bir kimliğe sahip bir sunucu denetimi içeren bir düzen şablonunu belirtin denetim önceki sürümü gerekli Aşağıdaki biçimlendirmede nasıl kullanılacağına ilişkin bir örnek gösterir *ListView* ASP.NET 3.5 denetimi.

[!code-aspx[Main](overview/samples/sample79.aspx)]

ASP.NET 4'te *ListView* denetimini bir düzen şablonunu gerektirmez. Önceki örnekte gösterilen biçimlendirme, aşağıdaki biçimlendirme değiştirilebilir:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList ve RadioButtonList denetim geliştirmeleri

ASP.NET 3.5 içinde düzeni belirtebilirsiniz *CheckBoxList* ve *RadioButtonList* aşağıdaki iki ayarı kullanarak:

- *Akış*. Kontrolünü icra *span* , içeriğe sahip öğeleri.
- *Tablo*. Denetim işleyen bir *tablo* içeriğini içerecek şekilde öğesi.

Aşağıdaki örnek her bu denetimleri için biçimlendirme gösterilmektedir.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Varsayılan olarak, aşağıdakine benzer HTML denetimlerini işlemeye:

[!code-html[Main](overview/samples/sample82.html)]

Bu denetimler anlamsal olarak doğru HTML oluşturmak için öğeleri listesi içerdiğinden HTML listesini kullanarak içerikleri işlemesi gerekir (*li*) öğeleri. Bu, yardımcı teknoloji kullanan Web sayfaları oku ve denetimleri kullanarak CSS stil kolaylaştırır kullanıcılar için kolaylaştırır.

ASP.NET 4'te *CheckBoxList* ve *RadioButtonList* denetimleri desteklemek için aşağıdaki yeni değerler *RepeatLayout* özelliği:

- *OrderedList* – içerik olarak işlenmeden *li* öğeleri içinde bir *ol* öğesi.
- *UnorderedList* – içerik olarak işlenmeden *li* öğeleri içinde bir *ul* öğesi.

Aşağıdaki örnek, bu yeni değerleri kullanma işlemini gösterir.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Önceki biçimlendirme, aşağıdaki HTML'yi oluşturur:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Ayarlarsanız Not *RepeatLayout* için *OrderedList* veya *UnorderedList*, *RepeatDirection* özelliği olur ve artık kullanılabilir özelliği, biçimlendirmeyi veya kodu içinde alındıysa çalışma zamanında bir özel durum atar. Bu denetimlerin görsel düzeni CSS kullanmayı tanımlandığından özelliği herhangi bir değer gerekir.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Menü denetimi geliştirmeleri

ASP.NET 4 için önce *menü* denetim İşlenmiş HTML tabloları bir dizi. Bu, satır içi özelliklerini ayarlama dışında CSS stilleri uygulamak daha zor hale ve ayrıca erişilebilirlik standartları ile uyumlu değil.

ASP.NET 4'te denetimi artık HTML sırasız bir listesini ve liste öğeleri oluşan anlam biçimlendirme kullanarak işler. Aşağıdaki örnek, bir ASP.NET sayfasında için biçimlendirme gösterilmektedir. *menü* denetimi.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Sayfasını işler, denetimi aşağıdaki HTML'yi oluşturur ( *onclick* kod, daha anlaşılır olması için atlanmış):

[!code-html[Main](overview/samples/sample86.html)]

İşleme iyileştirmelerinin yanı sıra, klavye ile gezinme menüsünün odak Yönetimi'ni kullanarak geliştirilmiştir. Zaman *menü* denetim odağı alır, öğeleri gezinmek için ok tuşlarını kullanın. *Menü* denetimi artık erişilebilir zengin Internet uygulamaları (ARIA) rollerini ekler ve şu öznitelikler[Kelebek](http://www.w3.org/TR/wai-aria-practices/#menu "menü ARIA yönergeleri")geliştirilmiş Erişilebilirlik.

Menü denetimi için stiller, bir stil bloğu üst sayfası yerine İşlenmiş HTML öğeleri ayarlarına uygun olarak işlenir. Denetim için stil üzerinde tam denetim göz atmak istiyorsanız, yeni ayarlayabilirsiniz *IncludeStyleBlock* özelliğini *false*, bu durumda stil bloğu değil yayılır. Bu özelliği kullanmak için bir menü görünümünü ayarlamak için Visual Studio Tasarımcısı'nda Otomatik biçimlendirme özelliğini kullanmayı yoludur. Çalıştırırsanız, sayfa kaynağı açın ve sonra işlenen stil bloğu dış bir CSS dosyasına kopyalayın. Visual Studio'da stil ve kümesi geri *IncludeStyleBlock* için *false*. Bir dış stil sayfasında stilleri kullanarak menüsü Görünüm tanımlanır sonucudur.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Sihirbazı ve CreateUserWizard denetimleri

ASP.NET *Sihirbazı* ve *CreateUserWizard* denetimleri işlenen HTML tanımlamanızı sağlayan şablonları destekler. (*CreateUserWizard* türetildiği *Sihirbazı*.) Aşağıdaki örnek, tam olarak şablonlu için biçimlendirme gösterir *CreateUserWizard* denetimi:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Denetimin HTML aşağıdakine benzer olarak işler:

[!code-html[Main](overview/samples/sample88.html)]

Şablon içeriği değiştirebilirsiniz ancak ASP.NET 3.5 SP1'de, yine de üzerinde çıkışını denetim kısıtlı *Sihirbazı* denetimi. ASP.NET 4'te oluşturduğunuz bir *LayoutTemplate* şablon ve ekleme *yer tutucu* denetimleri nasıl istediğinizi belirtmek için (ayrılmış bir adı kullanarak) *Sihirbazı denetim* işlemek için. Aşağıdaki örnek bunu gösterir:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Aşağıdaki yer tutucuları adlı örnek içerir *LayoutTemplate* öğesi:

- *headerPlaceholder* – çalışma zamanında bu içeriğine göre değiştirilir *HeaderTemplate* öğesi.
- *sideBarPlaceholder* – çalışma zamanında bu içeriğine göre değiştirilir *SideBarTemplate'i* öğesi.
- *wizardStepPlaceHolder* – çalışma zamanında bu içeriğine göre değiştirilir *WizardStepTemplate* öğesi.
- *navigationPlaceholder* – çalışma zamanında bu tanımladığınız Gezinti şablonları tarafından değiştirilir.

Yer tutucuları kullanan örnek biçimlendirmede aşağıdaki HTML'yi (olmadan gerçekten şablonlarında tanımlanan içeriği) oluşturur:

[!code-html[Main](overview/samples/sample90.html)]

Artık kullanıcı-tanımlı değil yalnızca HTML bir *span* öğesi. (Biz, gelecek sürümlerde, tahmin bile *span* öğesi değil işlenecek.) Bunu şimdi tarafından oluşturulan tüm içerik üzerinde tam denetim verir *Sihirbazı* denetimi.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC, ASP.NET 3.5 SP1'e Mart 2009'dan bir eklenti çerçevesi sunulmuştur. Yeni özellikler ve yetenekler içeren ASP.NET MVC 2, Visual Studio 2010'u içerir.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Alanları desteği

Alanları büyük bir uygulamanın diğer bölümlerden göreli ayrı bölümlere grubu denetleyicileri ve görünümleri sağlar. Her alanı, ana uygulama tarafından başvurulabilen ayrı bir ASP.NET MVC projesi olarak uygulanabilir. Bu, büyük bir uygulama oluşturduğunuzda karmaşıklığını yönetmenize yardımcı olur ve tek bir uygulama üzerinde birlikte çalışmak üzere birden çok takımı kolaylaştırır.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Veri ek açıklama özniteliği doğrulama desteği

*DataAnnotations* öznitelikleri, meta veri öznitelikleri kullanarak bir model için doğrulama mantığı eklemek olanak tanır. *DataAnnotations* öznitelikleri ASP.NET dinamik veri ASP.NET 3.5 SP1'de kullanıma sunulmuştur. Bu öznitelikler, varsayılan model bağlayıcısını entegre edilmiş ve kullanıcı girişini doğrulama için meta veri temelli bir yöntem sağlar.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Şablonlu Yardımcılar

Şablonlu yardımcılar, otomatik olarak ilişkilendirme düzenleme sağlar ve veri türleriyle şablonları görüntüler. Örneğin, bir tarih seçici kullanıcı Arabirimi öğesi için otomatik olarak işlenir belirtmek için bir şablon yardımcı kullanabilirsiniz bir *System.DateTime* değeri. ASP.NET dinamik veri alanı şablonlarında bu benzer.

*Html.EditorFor* ve *Html.DisplayFor* yardımcı yöntemler de gibi karmaşık nesnelere birden fazla özelliğe sahip standart veri işleme türleri için yerleşik desteğe sahiptir. Bunlar gibi veri ek açıklama öznitelikleri uygulamanıza imkan vererek da işleme özelleştirme *DisplayName* ve *ScaffoldColumn* için *ViewModel* nesne.

Genellikle daha da UI Yardımcıları çıkışı özelleştirip ne oluşturulan üzerinde tam denetime sahip istersiniz. *Html.EditorFor* ve *Html.DisplayFor* yardımcı yöntemler desteği bu kılabilirsiniz dış şablonları ve denetim işlenmiş çıktı tanımlamanızı sağlar bir şablon oluşturma mekanizması kullanılarak. Şablonlar, ayrı ayrı bir sınıf için işlenebilir.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dinamik veri

Dinamik veri tanıtılmıştır Orta 2008'de .NET Framework 3.5 SP1 sürümü. Bu özellik, aşağıdakiler dahil olmak üzere, veri odaklı uygulamalar oluşturmak için birçok geliştirme sağlar:

- Hızlı bir şekilde veri temelli bir Web sitesi oluşturmak için RAD deneyimi.
- Veri modelinde tanımlanan kısıtlamaları temel alan otomatik doğrulama.
- Kolayca alanlar için oluşturulmuş biçimlendirmeyi değiştirme özelliği *GridView* ve *DetailsView* dinamik veri projenizin bir parçası olan alan şablonları kullanarak denetimleri.

> [!NOTE]
> Unutmayın, daha fazla bilgi için bkz: [dinamik veri belgeleri](https://msdn.microsoft.com/library/cc488545.aspx) MSDN Kitaplığı'nda.


ASP.NET 4 için geliştiricilerin hızlı bir şekilde veri odaklı Web siteleri oluşturmak için daha fazla güç vermek için dinamik veri geliştirilmiştir.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Var olan projeleri için dinamik veri etkinleştirme

.NET Framework 3.5 SP1'de gelen dinamik veri özellikleri, aşağıdaki gibi yeni özellikler getirdi:

- Alan şablonları – bunlar veriye bağlı denetimler için veri türüne göre şablonları sağlar. Alan şablonları, her alan için şablon alanlar kullanmaktan veri denetimleri görünümünü özelleştirmek için daha basit bir yol sağlar.
- Doğrulama – dinamik veri doğrulama gerekli alanlar, aralığı denetleyerek, tür denetimi, normal ifadeler kullanarak desen gibi yaygın senaryoları ve özel doğrulama belirtmek için veri sınıflarında öznitelikleri kullanmanıza olanak sağlar. Doğrulama tarafından veri denetimleri zorlanır.

Ancak, bu özellikler, aşağıdaki gereksinimleri vardı:

- Veri erişim katmanı, Entity Framework veya LINQ to SQL temel gerekiyordu.
- Yalnızca veri kaynağı olan bu özellikleri için desteklenen denetimleri *EntityDataSource* veya *LinqDataSource* kontrol eder.
- Özelliği desteklemek için gerekli tüm dosyalar için dinamik veri veya dinamik veri varlıkları şablonları kullanılarak oluşturulmuş bir Web projesi özellikleri gereklidir.

Dinamik veri yeni işlevlerini bir ASP.NET uygulaması için ASP.NET 4'te podporu Dynamických dat ana hedefi etkinleştirmektir. Aşağıdaki örnek, biçimlendirme için dinamik veri işlevselliği varolan bir sayfa yararlanabilirsiniz denetimleri gösterir.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Povolit podporu Dynamických dat Bu denetimlerin için sayfa için kodda aşağıdaki kodu eklenmesi gerekir:

[!code-csharp[Main](overview/samples/sample92.cs)]

Zaman *GridView* kontrolü düzenleme modunda, dinamik verileri otomatik olarak doğrulama girilen verilerin doğru biçimde olduğundan. Yüklü değilse, bir hata iletisi görüntülenir.

Bu işlev, ayrıca diğer avantajlar sağlar, varsayılan belirlemek mümkün olması gibi modu değerleri ekleyin. Dinamik veri, bir alan için varsayılan bir değer uygulamak için gerekir bir olaya ekleyin, denetimi bulun (kullanarak *FindControl*) ve değerini ayarlayın. ASP.NET 4'te *EnableDynamicData* çağrı Bu örnekte gösterildiği gibi bir nesne üzerinde herhangi bir alan için varsayılan değerleri geçirmenize olanak veren ikinci bir parametresi destekler:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Bildirim temelli bir DynamicDataManager denetimi söz dizimi

*DynamicDataManager* denetim geliştirilmiştir, böylece bildirimli olarak, yapılandırabileceğiniz yalnızca içinde kod yerine çoğu denetimleri, ASP.NET ile. İşaretleme için *DynamicDataManager* denetimi aşağıdaki örnekteki gibi görünür:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Bu işaretleme başvurulduğundan GridView1'i Denetim için dinamik veri davranışını etkinleştirir *DataControls* bölümünü *DynamicDataManager* denetimi.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Varlık şablonları

Varlık şablonları, özel bir sayfa oluşturmanıza gerek kalmadan veri düzenini özelleştirmek için yeni bir yolunu sunar. Sayfa şablonları kullanın *FormView* denetimi (yerine *DetailsView* denetimi şablonların dinamik veri önceki sürümlerinde kullanılan gibi) ve *DynamicEntity* Varlık şablonları işlemek için denetimi. Bu, dinamik veri tarafından işlenen işaretleme üzerinde daha fazla denetim sağlar.

Aşağıdaki liste, varlık şablonlarını içeren yeni proje dizin düzenini gösterir:

[!code-console[Main](overview/samples/sample95.cmd)]

`EntityTemplate` Dizin veri modeli nesneleri göstermek için şablonlar içerir. Varsayılan olarak, nesneleri kullanılarak işlenir `Default.ascx` tarafından oluşturulan biçimlendirme gibi görünen biçimlendirme sağlayan şablon *DetailsView* ASP.NET 3.5 SP1'de dinamik veri tarafından kullanılan denetimi. Aşağıdaki örnek için biçimlendirme gösterir `Default.ascx` denetimi:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Varsayılan şablonlar, görünümü tüm site değiştirmek için düzenlenebilir. Görüntüleme, düzenleme ve ekleme işlemleri için şablonlar vardır. Yeni şablonlar veri nesnesinin adını göre görünüm ve yapısını yalnızca bir nesne türünü değiştirmek için eklenebilir. Örneğin, aşağıdaki şablonu ekleyebilirsiniz:

[!code-console[Main](overview/samples/sample97.cmd)]

Şablon aşağıdaki biçimlendirme içerebilir:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Yeni varlık şablonları yeni kullanarak bir sayfasında görüntülenen *DynamicEntity* denetimi. Çalışma zamanında bu denetimi varlık şablonu içeriğiyle değiştirilir. Aşağıdaki biçimlendirme gösterildiği *FormView* denetim `Detail.aspx` varlık şablonu kullanan sayfa şablonu. Bildirim *DynamicEntity* biçimlendirme öğesi.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>URL'ler ve e-posta adresleri için yeni alan şablonları

ASP.NET 4 tanıtır yeni iki yerleşik alan şablonları `EmailAddress.ascx` ve `Url.ascx`. Bu şablon olarak işaretlenmiş alanlar için kullanılan *EmailAddress* veya *Url* ile *DataType* özniteliği. İçin *EmailAddress* nesnelerini alanı kullanılarak oluşturulan bir köprü olarak gösterilir *mailto:* protokolü. Kullanıcılar bu bağlantıya tıkladığında, kullanıcının e-posta istemcisi'ni açar ve bir çatı iletisi oluşturur. Olarak belirtilmiş nesneler *Url* sıradan köprü olarak görüntülenir.

Aşağıdaki örnek nasıl alanları işaretli gösterir.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Bağlantılar DynamicHyperLink denetimi oluşturma

Dinamik veri Web sitesi erişirken son kullanıcılar URL'leri denetlemek için .NET Framework 3.5 SP1 içinde eklenen yeni yönlendirme özelliğini kullanır. Yeni *DynamicHyperLink* denetim dinamik veri sitesi sayfalara bağlantılar oluşturmanızı kolaylaştırır. Aşağıdaki örnek nasıl kullanılacağını gösterir *DynamicHyperLink* denetimi:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Bu işaretleme listesi sayfasına işaret eden bir bağlantı oluşturur `Products` tanımlanan yollar temel tablo `Global.asax` dosya. Denetim, dinamik veri sayfasını temel alan varsayılan tablo adını otomatik olarak kullanır.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Veri modelindeki devralma desteği

Entity Framework ve LINQ to SQL devralma veri modellerini destekler. Bunun bir örneği olan bir veritabanının olabilir bir `InsurancePolicy` tablo. De içerebileceğini `CarPolicy` ve `HousePolicy` aynı alanlara sahip tablolar `InsurancePolicy` ve diğer alanlar ekleyin. Dinamik veri, veri modelindeki devralınan nesneleri anlamak ve devralınan tablolar için askılamayı desteklemek için değiştirildi.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Çoktan çoğa ilişkiler (yalnızca varlık çerçevesi) için destek

Entity Framework ilişki koleksiyonu olarak gösterme tarafından uygulanan tablolar arasında çok-çok ilişkileri zengin desteği olan bir *varlık* nesne. Yeni `ManyToMany.ascx` ve `ManyToMany_Edit.ascx` alan şablonları, çoktan çoğa ilişkilerde yer veri düzenleme ve görüntüleme için destek sağlamak için eklenmiştir.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Denetim görüntüleme ve Destek sabit listeleri için yeni öznitelikler

*DisplayAttribute* alanların görüntülenme biçimini üzerinde ek denetim sağlamak için eklendi. *DisplayName* önceki sürümlerinde, bir alan için kullanılan açıklamalı alt yazı adını değiştirmek izin verilen dinamik veri özniteliği. Yeni *DisplayAttribute* sınıfı sırayla bir alanı görüntülenir ve bir alan filtre olarak kullanılan gibi bir alan görüntülemek için diğer seçenekleri belirtmenize olanak sağlar. Öznitelik etiketleri için kullanılan adı bağımsız denetim de sağlar. bir *GridView* denetim, kullanılan adı bir *DetailsView* alan için Yardım metni denetimi ve eşik kullanılır alan (alan metin girişi kabul ederse).

*EnumDataTypeAttribute* alanlarını eşleme sabit listeleri için izin verecek şekilde sınıfı eklendi. Bu öznitelik bir alan için geçerli olduğunda, bir numaralandırma türü belirtin. Dinamik veri kullanan yeni `Enumeration.ascx` görüntüleme ve sabit listesi değerlerini düzenlemek için kullanıcı Arabirimi oluşturmak üzere alan şablonu. Şablon, veritabanından alınan değerlerin sabit listesi adlarına eşler.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Filtreler için gelişmiş destek

Dinamik veri 1.0, Boole sütunları ve yabancı anahtar sütunları için yerleşik filtreleri ile birlikte gönderilir. Filtreler, görüntülenen veya bunların hangi sırayla görüntülenen belirtmenize izin vermez. Yeni *DisplayAttribute* özniteliği adreslerini hem, vererek bu sorunları denetleyen bir sütuna filtre olarak görüntülenip görüntülenmeyeceğini üzerinden ve hangi sipariş görüntülenecektir.

Bir ek re filtreleme desteği kaldırıldı geliştirmedir[yeni yazılan](#0.2__QueryExtender "_QueryExtender") Web Forms özelliğidir. Bu filtreler filtreleri ile kullanılan veri kaynak denetim bilgisini gerek kalmadan oluşturmanıza olanak sağlar. Bu uzantılar yanı sıra, filtreleri de şablon denetimlere yenilerini eklemenize olanak sağlayan kapatıldı. Son olarak, *DisplayAttribute* sınıfı daha önce bahsedilen varsayılan filtreyi, aynı kılınmasına izin verir biçimi *UIHint* geçersiz kılınacak sütun için varsayılan alan şablon sağlar.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web geliştirme iyileştirmeleri

Visual Studio 2010 Web geliştirme üretkenliğinizi HTML ve ASP.NET biçimlendirme parçacıkları ve yeni dinamik IntelliSense JavaScript aracılığıyla büyük CSS uyumluluk için geliştirilmiştir.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Geliştirilmiş CSS uyumluluğu

Visual Studio 2010 Visual Web Developer Tasarımcısı, CSS 2.1 standartları uyumluluğu artırmak için güncelleştirildi. Tasarımcı daha iyi HTML kaynağını bütünlüğünü korur ve daha sağlamdır Visual Studio'nun önceki sürümleri. Altyapı öğeleri, mimari geliştirmeler daha iyi işleme, Düzen ve Bakım yapılabilirliğini gelecekteki iyileştirmeler etkinleştirme olacak yapıldı.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML ve JavaScript kod parçacıkları

HTML düzenleyicisinde, IntelliSense otomatik olarak etiket adları tamamlar. IntelliSense kod parçacıkları özelliği otomatik olarak tüm etiketleri ve daha fazlasını tamamlar. Visual Studio 2010'da, IntelliSense kod parçacıkları, JavaScript, C# ve Visual Basic, Visual Studio'nun önceki sürümlerinde desteklenen yanı sıra için desteklenir.

Visual Studio 2010 gerekli öznitelikleri dahil olmak üzere otomatik tamamlama ortak ASP.NET ve HTML etiketlerini yardımcı 200'den fazla kod parçacıkları içerir (gibi runat = "server") ve belirli bir etikete ortak öznitelikleri (gibi *kimliği*,  *DataSourceID*, *ControlToValidate*, ve *metin*).

Ek kod parçacıkları yükleyebilir veya siz veya takımınız kullanmak için ortak görevler biçimlendirme bloklarını kapsayan kendi parçacıklarınızı yazabilirsiniz.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>JavaScript IntelliSense geliştirmeleri

JavaScript IntelliSense Visual 2010'da, daha kapsamlı bir düzenleme deneyimi sağlamak için tasarlanmıştır. IntelliSense tarafından yöntemleri gibi dinamik olarak oluşturulmuş nesneler artık tanır *registerNamespace* ile diğer JavaScript çerçevesi tarafından kullanılan benzer teknikler. Betik büyük kitaplıklarını çözümlemek ve çok az kayıpla veya hiç işleme gecikmesi IntelliSense görüntülenecek performansı İyileştirildi. Uyumluluk neredeyse tüm üçüncü taraf kitaplıklar desteklemek ve çeşitli kodlama stillerini desteklemek için önemli ölçüde artırıldı. Yazın ve hemen IntelliSense tarafından refs'nin belge yorumları artık ayrıştırılır.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Visual Studio 2010 ile Web uygulaması dağıtımı

ASP.NET geliştiricilerinin Web uygulaması dağıttığınızda, bunlar sık karşılaştıkları sorunları aşağıdaki gibi bulun:

- Paylaşılan barındırma bir siteye dağıtmak, yavaş olabilir, FTP gibi teknolojiler gerekir. Ayrıca, bir veritabanını yapılandırmak için SQL betiklerini çalıştırma gibi görevleri el ile gerçekleştirmelisiniz ve bir uygulama olarak bir sanal dizin klasörünü yapılandırma gibi IIS ayarları değiştirmeniz gerekir.
- Web uygulama dosyaları konuşlandırılıyor yanı sıra bir Kurumsal ortamda yöneticilerin sık ASP.NET yapılandırma dosyaları ve IIS ayarlarını değiştirmeniz gerekir. Veritabanı yöneticileri, bir dizi çalıştıran uygulama veritabanını almak için SQL betiklerini çalıştırmanız gerekir. İşçi yoğun yüklemeler, genellikle tamamlanması saatler süren ve dikkatli bir şekilde belgelenmelidir.

Visual Studio 2010, bu sorunları gidermek ve sorunsuz bir şekilde Web uygulamaları dağıtmanıza izin veren teknolojileri içerir. Bu teknolojiler IIS Web Dağıtım Aracı'nın (MsDeploy.exe) biridir.

Visual Studio 2010 Web dağıtım özellikleri, aşağıdaki önemli alanlar şunlardır:

- Web paketleme
- Web.config dönüşümünün
- Veritabanı dağıtımı
- Web uygulamaları için tek tıklamayla yayımlama

Aşağıdaki bölümlerde bu özellikler hakkında ayrıntılar sağlar.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Web paketleme

Visual Studio 2010 verilir uygulamanız için bir sıkıştırılmış (.zip) dosyası oluşturmak için MSDeploy aracını kullanan bir *Web paketi*. Paket dosyası, uygulamanızı ve aşağıdaki içeriği hakkındaki meta verileri içerir:

- IIS ayarları, uygulama havuzu ayarları, hata sayfası ayarlarını vb. içerir.
- Web sayfaları, kullanıcı denetimleri, statik içerik (görüntü ve HTML dosyaları) ve benzeri içeren gerçek Web içeriği.
- SQL Server veritabanı şemaları ve verileri.
- Güvenlik sertifikaları GAC, kayıt defteri ayarları ve benzeri yüklenecek bileşenleri.

Web paketi, herhangi bir sunucuya kopyalanır ve IIS Yöneticisi'ni kullanarak el ile yüklenen. Alternatif olarak, otomatik dağıtım için dağıtım API'leri kullanarak ya da komut satırı komutlarını kullanarak paket yüklenebilir.

Visual Studio 2010, MSBuild görevleri ve hedefleri Web paketleri oluşturmak için yerleşik sağlar. Daha fazla bilgi için [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) MSDN Web sitesinde ve [neden oluşturmanız gerektiğini Web paketi 10 + 20 nedeniyle](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) Vishal Joshi'nın blogunda.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web.config dönüşümünün

Web uygulaması dağıtımı için Visual Studio 2010 tanıtır [XML belge dönüştürme (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), dönüştürme olanak sağlayan bir özellik olan bir `Web.config` geliştirme ayarları dosyasına üretim ayarları. Dönüştürme ayarlarını adlı dönüştürme dosyalarında belirtilen `web.debug.config`, `web.release.config`ve benzeri. (Bu dosyaların adlarını MSBuild yapılandırmaları eşleşir.) Dönüştürme dosyası dağıtılan bir hale getirmek için gereken değişiklikleri içerir `Web.config` dosya. Değişiklikleri, basit söz dizimi kullanarak belirtin.

Aşağıdaki örnek, bir bölümünü gösterir. bir `web.release.config` , yayın yapılandırmasının dağıtım için üretilen dosyası. Dağıtım sırasında belirten Değiştir anahtar sözcüğü örnekte *connectionString* düğümünde `Web.config` dosya, örnekte listelenen değerleri ile değiştirilir.

[!code-xml[Main](overview/samples/sample102.xml)]

Daha fazla bilgi için [Web uygulama projesi dağıtımı için Web.config dönüşümü sözdizimi](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) MSDN'de <a id="0.2_a"> </a> Web sitesi ve[Web Dağıtımı: Web.Config dönüşümünün](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi'nın blogunda.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Veritabanı dağıtımı

Visual Studio 2010 dağıtım paketi, SQL Server veritabanlarını bağımlılıklarını ekleyebilirsiniz. Paket tanımı bir parçası olarak, kaynak veritabanınızın bağlantı dizesini belirtin. Web paketi oluşturduğunuzda, Visual Studio 2010 için veritabanı şeması ve veriler için isteğe bağlı olarak SQL komut dosyaları oluşturur ve bu pakete ekler. Ayrıca, özel SQL komut dosyaları sağlar ve sunucuda çalışacakları sırayı belirtebilirsiniz. Dağıtım sırasında hedef sunucu için uygun bir bağlantı dizesi sağlar; dağıtım işlemi, ardından veritabanı şemasını ve verileri eklediğiniz komut dosyalarını çalıştırmak için bu bağlantı dizesini kullanır.

Ayrıca, tek tıklama kullanarak yayımlama, dağıtım, uygulamayı uzak paylaşılan barındırma siteye yayımlandığında doğrudan veritabanınızı yayımlamak için yapılandırabilirsiniz. Daha fazla bilgi için [nasıl yapılır: bir veritabanı ile bir Web uygulaması projesi dağıtma](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) MSDN Web sitesinde ve [VS 2010 ile veritabanı dağıtımı](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) Vishal Joshi'nın blogunda.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Web uygulamaları için tek tıklamayla yayımlama

Visual Studio 2010'da, uzak bir sunucuya bir Web uygulaması yayımlamaya IIS uzaktan yönetim hizmeti kullanmanızı sağlar. Sunucuları test veya sunucuları hazırlama veya barındırma hesabınız için bir yayımlama profili oluşturabilirsiniz. Her profil uygun kimlik bilgilerini güvenli bir şekilde kaydedebilirsiniz. Daha sonra hedef birine dağıtabilirsiniz sunucuları tek tıklamayla Web kullanarak tek bir tıklamayla Yayımla araç çubuğu. Visual Studio 2010 ile MSBuild komut satırını kullanarak yayımlayabilirsiniz. Bu, yayımlama bir sürekli tümleştirme modele dahil edilecek takım derleme ortamınızı yapılandırmanıza olanak sağlar.

Daha fazla bilgi için bkz [nasıl yapılır: bir Web uygulaması projesi kullanarak tek tıklamayla yayımlama ve Web Dağıtımı'nı dağıtma](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) MSDN Web sitesinde ve [Web VS 2010 ile 1-tıklatma Yayımla](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi'nın blogunda. Visual Studio 2010'da Web uygulaması dağıtımı video sunular görüntülemek için bkz [VS 2010 Web geliştirme önizlemeleri için](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Vishal Joshi'nın blogunda.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Kaynaklar

Aşağıdaki Web siteleri, ASP.NET 4 ile Visual Studio 2010 hakkında ek bilgi sağlar.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — MSDN Web sitesinde ASP.NET 4 için resmi belgeleri.
- [https://www.asp.net/](https://www.asp.net/) — ASP.NET takımın kendi Web sitesi.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) ve [ASP.NET dinamik veri içerik haritası](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — çevrimiçi kaynaklar ASP.NET ekip sitesi ve ASP.NET dinamik veri için resmi belgeleri.
- [https://www.asp.net/ajax/](../../ajax/index.md) — ASP.NET Ajax geliştirme için ana Web kaynağı.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Visual Studio 2010'daki özellikler hakkında bilgi içerir Visual Web Developer Team blog.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — Önizleme sürümleri, ASP.NET ana Web kaynağı.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Sorumluluk reddi

Bu, Başlangıç belgesi ve burada açıklanan yazılım son ticari sürümünden önce önemli ölçüde değiştirilebilir.

Bu belgede yer alan bilgileri, Microsoft Corporation'ın geçerli görünümü tarih itibariyle doğrudur ele alınan sorunlar temsil eder. Microsoft'un değişen piyasa koşullarına yanıt vermesi gerekir çünkü Microsoft tarafında taahhüdü olarak yorumlanmamalıdır ve Microsoft yayın tarihinden sonra sunulan bilgilerin doğruluğunu garanti edemez.

Bu teknik incelemeyi yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ.

İlgili tüm telif hakkı yasalara uymak kullanıcının sorumluluğundadır. Telif Hakkı altındaki sınırlamaksızın, bu belgenin hiçbir bölümü çoğaltılamaz, depolanan veya bir sistemde saklanamaz ya da herhangi bir yolla (elektronik, mekanik, fotokopi, kaydı veya diğer) veya herhangi bir amaçla, Microsoft Corporation'ın izni hızlı yazılır.

Microsoft'un patentleri, patent başvuruları, ticari markaları, telif hakları veya diğer fikri mülkiyet hakları bu belgedeki konuyla ilgili olabilir. Microsoft'tan bir yazılı lisans anlaşmasında bu belgenin bulundurmak, herhangi bir lisans bu patentlere, ticari markaları, telif hakları veya diğer fikri mülkiyet sağlamazsa açıkça belirtilmediği sürece.

Aksi belirtilmediği sürece, örnek şirketler, kuruluşlar, ürünler, etki alanı adları, e-posta adresleri, logolar, kişiler, yerler ve olaylar burada olduğunu hayal ve tüm gerçek şirket, kuruluş, ürün, etki alanı adı, e-posta ile hiçbir ilişki adı geçen adresi, logo, kişi, yer veya olay amaçlanmamıştır veya çıkarılmamalıdır.

© 2009 Microsoft Corporation. Tüm hakları saklıdır.

Microsoft ve Windows tescilli veya Amerika Birleşik Devletleri ve/veya diğer ülkelerde Microsoft Corporation'ın ticari olan.

Burada sözü edilen gerçek şirketler ve ürünler ticari markalar kendi sahiplerinin olabilir.
