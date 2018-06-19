---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: ASP.NET 4.5 ve Visual Studio 2012'de yenilikler | Microsoft Docs
author: rick-anderson
description: Bu belgede, yeni özellikler ve ASP.NET 4.5 içinde sunulan geliştirmeler açıklanmaktadır. Ayrıca, web geliştirme için yapılan geliştirmeler açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 16a2fa4b1b6617430703853543cbf29e18ba1103
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
ms.locfileid: "28886448"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>ASP.NET 4.5 ve Visual Studio 2012'de yenilikler
====================
> Bu belgede, yeni özellikler ve ASP.NET 4.5 içinde sunulan geliştirmeler açıklanmaktadır. Ayrıca, Visual Studio 2012 web geliştirme için yapılan geliştirmeler açıklanmaktadır. Bu belge, ilk olarak 29 Şubat Mayıs 2012'yayımlandı.


- [ASP.NET çekirdeği çalışma zamanı ve Framework](#_Toc318097372)

    - [HTTP istekleri ve yanıtları zaman uyumsuz olarak okumak ve yazmak](#_Toc318097373)
    - [HttpRequest işleme geliştirmeleri](#_Toc318097374)
    - [Zaman uyumsuz olarak bir yanıt temizleme](#_Toc318097375)
    - [Desteği *await* ve *görev*-tabanlı zaman uyumsuz modüller ve işleyicileri](#_Toc318097376)
    - [Zaman uyumsuz HTTP modülleri](#_Toc318097377)
    - [Zaman uyumsuz HTTP işleyicileri](#_Toc318097378)
    - [Yeni ASP.NET istek doğrulama özellikleri](#_Toc318097379)
    - [("Yavaş") istek doğrulama ertelenmiş](#_Toc318097380)
    - [Doğrulanmamış istekler için destek](#_Toc318097381)
    - [Antixss'i kitaplığı](#_Toc318097382)
    - [WebSockets Protokolü](#_Toc318097383)
    - [Paketleme ve Küçültme](#_Toc318097384)
    - [Web barındırma için performans iyileştirmeleri](#_Toc_perf)

        - [Ana performans Etkenler](#_Toc_perf_1)
        - [Yeni performans özellikleri için gereksinimler](#_Toc_perf_2)
        - [Ortak derlemeleri paylaşma](#_Toc_perf_3)
        - [Çok çekirdekli JIT derleme için hızlı başlangıç kullanma](#_Toc_perf_4)
        - [Bellek için en iyi duruma getirme ayarlama çöp toplama](#_Toc_perf_5)
        - [Web uygulamaları için prefetching](#_Toc_perf_6)
- [ASP.NET Web formları](#_Toc318097385)

    - [Kesin Türü Belirtilmiş Veri Denetimleri](#_Toc318097386)
    - [Model Bağlamaları](#_Toc318097387)

        - [Verileri seçme](#_Toc318097388)
        - [Değer sağlayıcıları](#_Toc318097389)
        - [Kontrolü değerlere göre filtreleme](#_Toc318097390)
    - [HTML olarak kodlanmış veri bağlama ifadeleri](#_Toc318097391)
    - [Örtük doğrulama](#_Toc318097392)
    - [HTML5 güncelleştirmeleri](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Visual Studio 2012 Yayın Adayı](#_Toc318097396)

    - [Proje Visual Studio 2010 ve Visual Studio 2012 Sürüm Adayı (Proje uyumluluk) arasında paylaşma](#project-compatibility)
    - [ASP.NET 4.5 Web sitesi şablonları yapılandırma değişiklikleri](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [IIS 7 ASP.NET yönlendirme için yerel destek](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML Editor](#_Toc318097397)

        - [Akıllı görevleri](#_Toc318097398)
        - [WAI ARIA desteği](#_Toc318097399)
        - [Yeni HTML5 parçacıkları](#_Toc318097400)
        - [Kullanıcı denetimine çıkar](#_Toc318097401)
        - [Kod nuggets öznitelikler için IntelliSense](#_Toc318097402)
        - [Bir açma veya kapatma etiketi yeniden adlandırdığınızda eşleşen etiketi otomatik yeniden adlandırma](#_Toc318097403)
        - [Olay işleyicisi oluşturma](#_Toc318097404)
        - [Akıllı girinti](#_Toc318097405)
        - [Deyim tamamlama otomatik azaltın](#_Toc318097406)
    - [JavaScript Düzenleyicisi](#_Toc318097407)

        - [Anahat oluşturma kodu](#_Toc318097408)
        - [Ayraç eşleştirme](#_Toc318097409)
        - [Tanıma gitme](#_Toc318097410)
        - [ECMAScript5 desteği](#_Toc318097411)
        - [DOM IntelliSense](#_Toc318097412)
        - [VSDOC imza aşırı yüklemeleri](#_Toc318097413)
        - [Örtük başvuruları](#_Toc318097414)
    - [CSS Editor](#_Toc318097415)

        - [Deyim tamamlama otomatik azaltın](#_Toc318097416)
        - [Hiyerarşik girinti.](#_Toc318097417)
        - [CSS desteği yönlendirir](#_Toc318097418)
        - [Vendor specific schemas (-moz-,-webkit)](#_Toc318097419)
        - [Yorum ve uncommenting desteği](#_Toc318097420)
        - [Renk Seçici](#_Toc318097421)
        - [Kod Parçacıkları](#_Toc318097422)
        - [Özel bölgeler](#_Toc318097423)
    - [Sayfa denetçisi](#_Toc318097424)
    - [Yayımlama](#_Toc318097425)

        - [Yayımlama profilleri](#_Toc318097426)
        - [ASP.NET ön derlemesi ve birleştirme](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Sorumluluk reddi](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET çekirdeği çalışma zamanı ve Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>HTTP istekleri ve yanıtları zaman uyumsuz olarak okumak ve yazmak

ASP.NET 4 sunulan kullanarak bir akış olarak bir HTTP istek varlığı okuma özelliği *HttpRequest.GetBufferlessInputStream* yöntemi. Bu yöntem, istek varlığı akış erişim sağlanır. Ancak, bu zaman uyumlu olarak, bir istek boyunca, bir iş parçacığını bağlı yürütüldü.

ASP.NET 4.5, bir HTTP istek varlığı zaman uyumsuz olarak akışların okuma özelliği ve zaman uyumsuz olarak temizleme olanağı destekler. ASP.NET 4.5 ayrıca çift arabellek denetleyicileri .aspx sayfa işleyicileri ve ASP.NET MVC gibi aşağı akış HTTP işleyicileri ile kolay tümleştirme sağlar bir HTTP istek varlığı yeteneği sağlar.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>HttpRequest işleme geliştirmeleri

ASP.NET 4.5'ten tarafından döndürülen akış başvuru *HttpRequest.GetBufferlessInputStream* zaman uyumlu ve zaman uyumsuz okuma yöntemlerini destekler. *Akış* döndürülen nesne *GetBufferlessInputStream* şimdi BeginRead ve EndRead yöntemlerini uygular. Zaman uyumsuz *akış* yöntemleri zaman uyumsuz okuma döngünün her yinelemesinden arasındaki geçerli iş parçacığının ASP.NET serbest parçalar, istek varlığı zaman uyumsuz olarak okumak olanak sağlar.

ASP.NET 4.5 arabelleğe alınan bir şekilde istek varlığı okumak için bir yardımcı yöntem de ekledi: *HttpRequest.GetBufferedInputStream*. Bu yeni aşırı gibi çalışır *GetBufferlessInputStream*, zaman uyumlu ve zaman uyumsuz okuma destekleme. Ancak, bunu okuyan gibi *GetBufferedInputStream* da aşağı akış modülleri ve işleyicileri hala istek varlığı erişebilmesi için varlık bayt ASP.NET iç arabellek kopyalar. Örneğin, bazı varsa upstream ardışık düzen kodu zaten kullanarak istek varlığı okuma *GetBufferedInputStream*, kullanmaya devam edebilirsiniz *HttpRequest.Form* veya *HttpRequest.Files*. Bu, zaman uyumsuz işleme (örneğin, bir veritabanına büyük dosyanın Karşıya akış), bir isteği gerçekleştiremiyor. hala çalışma .aspx sayfaları ve ASP.NET MVC denetleyicileri daha sonra sağlar.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Zaman uyumsuz olarak bir yanıt temizleme

Bir HTTP istemcisi yanıtlarını gönderme ne zaman istemci uzakta veya düşük bant genişlikli bağlantısı olan önemli ölçüde uzun sürebilir. Normalde bir uygulama tarafından oluşturulan ASP.NET yanıtı bayt sayısı arabelleğe alır. ASP.NET istek işleme çok sonunda tahakkuk eden arabellek tek gönderme işlemini gerçekleştirir.

Arabelleğe alınan yanıtı (örneğin, bir istemci için büyük bir dosya akışı) büyükse, düzenli aralıklarla çağırmalısınız *HttpResponse.Flush* arabelleğe alınan çıkış istemciye göndermek ve bellek kullanımı denetimi altında tutun. Ancak, çünkü *Temizleme* tekrarlayarak çağırma zaman uyumlu bir çağrı *Flush* hala olası uzun süredir çalışan istekleri boyunca bir iş parçacığı kullanır.

ASP.NET 4.5 kullanarak zaman uyumsuz olarak aktarır gerçekleştirmek için destek ekler *BeginFlush* ve *EndFlush* yöntemlerinin *HttpResponse* sınıfı. Bu yöntemleri kullanarak zaman uyumsuz modüller ve artımlı olarak işletim sistemi iş parçacıklarını bağlamadan veri bir istemciye göndermek zaman uyumsuz işleyicileri oluşturabilirsiniz. Arasında *BeginFlush* ve *EndFlush* çağrıları, ASP.NET geçerli iş parçacığının serbest bırakır. Bu, uzun süre çalışan HTTP indirmeleri desteklemek için gereken etkin iş parçacığı toplam sayısını önemli ölçüde azaltır.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Desteği *await* ve *görev* -tabanlı zaman uyumsuz modüller ve işleyicileri

.NET Framework 4 olarak adlandırılan bir zaman uyumsuz programlama kavram sunulan bir *görev*. Görevler tarafından gösterilen *görev* türü ve ilgili türlerinde *System.Threading.Tasks* ad alanı. .NET Framework 4.5 bu çalışmak olun derleyici geliştirmeleri derlemeler *görev* nesneleri basit. .NET Framework 4.5 derleyicileri iki yeni anahtar sözcüklerin desteği: *await* ve *zaman uyumsuz*. *Await* , paylaştırılabilen bir kod zaman uyumsuz olarak diğer bazı kod parçasına beklemesi gerektiğini belirten için söz dizimi toplu bir anahtardır. *Zaman uyumsuz* anahtar sözcüğü yöntemleri görev tabanlı zaman uyumsuz yöntemleri olarak işaretlemek için kullanabileceğiniz bir ipucu temsil eder.

Birleşimi *await*, *zaman uyumsuz*ve *görev* nesne sağlar, .NET 4.5 içinde zaman uyumsuz kod yazmak çok daha kolay. ASP.NET 4.5, zaman uyumsuz HTTP modülleri ve yeni derleyici geliştirmeleri kullanarak zaman uyumsuz HTTP işleyicileri yazmanıza olanak tanıyan yeni API'leri ile bu basitleştirme destekler.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Zaman uyumsuz HTTP modülleri

Zaman uyumsuz iş döndüren bir yöntem içinde gerçekleştirmek istediğinizi varsayalım bir *görev* nesnesi. Aşağıdaki kod örneğinde Microsoft giriş sayfasında indirmek için bir zaman uyumsuz çağrı yapan zaman uyumsuz bir yöntem tanımlar. Kullanımına dikkat edin *zaman uyumsuz* yöntem imzası anahtar sözcüğü ve *await* çağrısı *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Tüm yazmak zorunda olan — .NET Framework yükleme tamamlanması bekleniyor yanı sıra sırasında yükleme tamamlandıktan sonra otomatik olarak çağrı yığınını geri çağrı yığını geriye doğru izleme otomatik olarak işler.

Şimdi bir zaman uyumsuz ASP.NET HTTP modülü, bu zaman uyumsuz yöntem kullanmak istediğinizi varsayalım. ASP.NET 4.5 yardımcı bir yöntem içerir (*EventHandlerTaskAsyncHelper*) ve yeni bir temsilci türü (*TaskEventHandler*) görev tabanlı zaman uyumsuz yöntemleri eski ile tümleştirmek için kullanabilirsiniz ASP.NET HTTP ardışık düzen tarafından kullanıma sunulan zaman uyumsuz programlama modeli. Bu örnekte gösterilir nasıl:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Zaman uyumsuz HTTP işleyicileri

Zaman uyumsuz işleyicileri ASP.NET yazma geleneksel yaklaşım uygulamaktır *IHttpAsyncHandler* arabirimi. ASP.NET 4.5 tanıtır *HttpTaskAsyncHandler* zaman uyumsuz işleyicileri yazma çok kolay hale getiren, türetilen zaman uyumsuz temel türü.

*HttpTaskAsyncHandler* türü soyut ve geçersiz kılma gerektirir *ProcessRequestAsync* yöntemi. Dahili olarak ASP.NET dönüş imza tümleştirme mvc'deki (bir *görev* nesnesi), *ProcessRequestAsync* ASP.NET ardışık düzen tarafından kullanılan eski zaman uyumsuz programlama modeli.

Aşağıdaki örnekte nasıl kullanabileceğinizi gösterir *görev* ve *await* zaman uyumsuz bir HTTP işleyicisini uyarlamasını bir parçası olarak:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Yeni ASP.NET istek doğrulama özellikleri

Varsayılan olarak, ASP.NET istek doğrulama gerçekleştirir — biçimlendirme veya komut dosyası alanları, üst bilgiler, tanımlama bilgileri ve benzeri aramak için istekleri inceler. Herhangi bir algılandığında, ASP.NET bir özel durum oluşturur. Bu ilk bir olası siteler arası komut dosyası saldırılarına karşı savunma hattı olarak görev yapar.

ASP.NET 4.5 seçerek doğrulanmamış istek verileri okumak kolaylaştırır. ASP.NET 4.5 ayrıca önceden harici bir kitaplığı olan popüler Antixss'i kitaplığı ile tümleştirir.

Geliştiriciler, seçmeli olarak uygulamaları için istek doğrulamayı devre dışı bırakmak özelliği için sık sorulan. Örneğin, uygulamanızın forum yazılımı ise, HTML biçimli Forumu gönderileri ve yorumları gönderin, ancak hala istek doğrulama şey denetliyor emin olmak kullanıcılara izin vermek isteyebilirsiniz.

ASP.NET 4.5 seçerek doğrulanmamış girişle iş kolaylaştıran iki özellikler sunar: ("yavaş") istek doğrulamayı ve doğrulanmamış isteği verilere erişim ertelendi.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>("Yavaş") istek doğrulama ertelenmiş

ASP.NET 4.5 içinde varsayılan olarak tüm istek verileri olan istek doğrulama tabidir. Ancak, uygulama istek doğrulama isteği veri gerçekten erişim kadar erteleme yapılandırabilirsiniz. (Bu bazen için yavaş istek doğrulama, belirli veri senaryoları için geç yükleme gibi koşullarını dayalı olarak adlandırılır.) Ertelenmiş doğrulama ayarlayarak Web.config dosyasında kullanmak için uygulamayı yapılandırabilirsiniz *requestValidationMode* 4.5 içinde özniteliğini *httpRUntime* aşağıdaki örnekteki gibi öğe:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

İstek doğrulama modu 4.5 ayarladığınızda, istek doğrulama belirli istek değeri için yalnızca ve yalnızca kodunuzun bu değeri eriştiğinde tetiklenir. Örneğin, kodunuzu Request.Form["forum değerini alır,\_post"], istek doğrulama, yalnızca form koleksiyonu bu öğe için çağrılır. Diğer öğelerin hiçbiri *Form* koleksiyonu doğrulanır. Koleksiyondaki herhangi bir öğe erişildiği ASP.NET önceki sürümlerde tüm istek koleksiyonu için istek doğrulamayı tetiklendi. Yeni davranışı diğer parçaları istek doğrulamayı tetiklemeden istek verileri farklı parçalarının aramak farklı uygulama bileşenleri için kolaylaştırır.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Doğrulanmamış istekler için destek

Ertelenmiş istek doğrulama tek başına seçmeli olarak istek doğrulamayı atlayarak sorununu çözün değil. Request.Form["forum çağrısı\_post"] hala tetikleyicileri, belirli bir istek değeri için doğrulama isteği. Ancak, bu alanda biçimlendirme izin vermek istediğiniz çünkü doğrulama tetiklemeden bu alana erişmek isteyebilirsiniz.

Bu izin vermek için ASP.NET 4.5 istek verileri doğrulanmamış erişimi destekler. ASP.NET 4.5 içeren yeni bir *Unvalidated* koleksiyonu özelliğinde *HttpRequest* sınıfı. Bu koleksiyonun tüm istek verileri ortak değerlerinin erişim gibi sağlar *Form*, *QueryString*, *tanımlama bilgilerini*, ve *Url*.

Doğrulanmamış istek verileri okuyabilir için Forumu örneği kullanarak, önce yeni istek doğrulama modunu kullanacak şekilde yapılandırmanız gerekir:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Daha sonra *HttpRequest.Unvalidated* doğrulanmamış form değerini okumaya özelliği:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Güvenlik - *doğrulanmamış istek verileri dikkatli kullanın!* ASP.NET 4.5 doğrulanmamış istek özellikleri ve belirli doğrulanmamış isteği verilere erişmek kolaylaştırmak için koleksiyonları eklendi. Ancak, tehlikeli metin kullanıcılara işlenmez emin olmak için ham istek verileri üzerinde özel doğrulama gerçekleştirmelidir.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Antixss'i kitaplığı

Microsoft Antixss'i Library popülerliği nedeniyle ASP.NET 4.5 şimdi bu kitaplık 4.0 sürümünün çekirdek kodlama yordamları içerir.

Kodlama yordamları tarafından uygulanan *AntiXssEncoder* yeni türü *System.Web.Security.AntiXss* ad alanı. Kullanabileceğiniz *AntiXssEncoder* doğrudan çağırarak türünde uygulanan statik kodlama yöntemlerinden herhangi birini türü. Ancak, yeni bir anti-XSS yordamları kullanarak için kolay bir yaklaşım kullanmak için bir ASP.NET uygulaması yapılandırmaktır *AntiXssEncoder* varsayılan sınıfı. Bunu yapmak için Web.config dosyasında şu özniteliği ekleyin:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Zaman *encoderType* özniteliği kullanmak üzere ayarlanmış *AntiXssEncoder* tüm çıktı türü, ASP.NET otomatik olarak kodlaması yeni kodlama yordamları kullanır.

ASP.NET 4.5 dahil dış Antixss'i kitaplık bölümlerini şunlardır:

- *HtmlEncode*, *HtmlFormUrlEncode*, ve *HtmlAttributeEncode*
- *XmlAttributeEncode* ve *XmlEncode*
- *UrlEncode* ve *UrlPathEncode* (yeni)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>WebSockets Protokolü

WebSockets Protokolü HTTP üzerinden bir istemci ve sunucu arasında güvenli, gerçek zamanlı çift yönlü iletişim kurmak nasıl tanımlayan standartlara dayalı bir ağ protokolüdür. Microsoft, iletişim kuralı tanımlamaya yardımcı olmak için IETF ve W3C standartları gövdelerine sahip çalışmıştır. WebSockets protokolü, hem istemci hem de mobil işletim sistemleri WebSockets protokolü destekleyen önemli kaynaklara yatırım yapmadan Microsoft ile (yalnızca tarayıcıları), istemciler tarafından desteklenir.

WebSockets protokolü bir istemci ve sunucu arasındaki uzun süreli veri aktarımlarını oluşturmak çok daha kolay hale getirir. Bir istemci ve sunucu arasında doğru bir uzun süre çalışan bağlantı kurup çünkü Örneğin, sohbet uygulaması yazma çok daha kolaydır. Geçici Çözümler düzenli Yoklamasını veya HTTP uzun-yuva davranışını benzetmek için yoklama gibi çözümlemelere gerekmez.

ASP.NET 4.5 ve IIS 8 zaman uyumsuz olarak okumak ve WebSockets nesnesinde hem dize hem de ikili veri yazmak için yönetilen API'ler kullanmak üzere ASP.NET geliştiricilerinin etkinleştirme alt düzey WebSockets desteği içerir. ASP.NET 4.5 için var olan yeni bir *System.Web.WebSockets* WebSockets protokolü ile çalışmaya yönelik türler içerir ad alanı.

Bir tarayıcı istemci bir DOM oluşturarak WebSockets bağlantı kurar *WebSocket* bir URL aşağıdaki örnekteki gibi bir ASP.NET uygulamasında işaret nesnesi:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Herhangi bir modül veya işleyici türünü kullanarak ASP.NET WebSockets uç noktaları oluşturabilirsiniz. .Ashx dosyaları bir işleyici oluşturmak için hızlı bir yol olduğundan önceki örnekte, .ashx dosya, kullanıldı.

WebSockets Protokolü göre bir ASP.NET uygulaması istek WebSockets isteği için bir HTTP GET isteği yükseltilmelidir belirterek bir istemcinin WebSockets isteğini kabul eder. Örnek buradadır:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest* yöntemi ASP.NET geçerli HTTP isteği unwinds ve işlev temsilcisi denetim aktarır çünkü bir işlev temsilcisi kabul eder. Kavramsal olarak bu yaklaşım nasıl kullanmak için benzer *System.Threading.Thread*, hangi arka planda iş gerçekleştirilir başlangıç iş parçacığı temsilci burada tanımlayın.

ASP.NET ve istemci bir WebSockets anlaşması başarıyla tamamladıktan sonra ASP.NET, temsilcisini çağırır ve WebSockets uygulama çalışmaya başlar. Aşağıdaki kod örneği, ASP.NET yerleşik WebSockets desteği kullanan basit Yankı uygulamayı gösterir:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

.NET 4. 5'için desteği *await* anahtar sözcüğü ve görev tabanlı zaman uyumsuz işlemleri olduğu WebSockets uygulamaları yazmak için doğal bir sığdırma. Kod örneği, WebSockets isteği tamamen zaman uyumsuz olarak ASP.NET içinde çalıştığını gösterir. Uygulama zaman uyumsuz olarak bir istemciden çağırarak gönderilmek üzere bir ileti bekler *yuva bekler. ReceiveAsync*. Bir istemciye çağırarak zaman uyumsuz ileti benzer şekilde, gönderebilirsiniz *yuva bekler. SendAsync*.

Tarayıcıda, uygulama WebSockets iletileri üzerinden alır bir *onmessageoptions* işlevi. Bir tarayıcı üzerinden ileti göndermek için arama *Gönder* yöntemi *WebSocket* DOM türü, bu örnekte gösterildiği gibi:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

Gelecekte, biz soyut hemen bazı alt düzey kodlama diğer bir deyişle, bu sürümde WebSockets için uygulamalar gerekli olduğunu bu işlevsellik için güncelleştirmeler sürüm.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Paketleme ve küçültme

Paketleme, JavaScript ve CSS dosyaları gibi tek bir dosyayı kabul bir paket halinde birleştirmek olanak tanır. Küçültme, boşluk ve gerekli olmayan diğer karakterler kaldırarak JavaScript ve CSS dosyaları toplar. Bu özellikler, Web Forms, ASP.NET MVC ve Web sayfaları ile çalışır.

Paketleri paket sınıfı veya ScriptBundle ve StyleBundle, alt sınıflarından biri kullanılarak oluşturulur. Bir paket örneği yapılandırdıktan sonra paket bir genel BundleCollection örneğine ekleyerek gelen istekleri kullanılabilir hale getirilir. Varsayılan şablonların paket yapılandırmasını BundleConfig dosyasında gerçekleştirilir. Bu varsayılan yapılandırma, tüm çekirdek komut dosyaları ve şablonları tarafından kullanılan css dosyaları için paket oluşturur.

Paketleri gelen görünümler içinde birkaç olası Yardımcısı yöntemlerden birini kullanarak başvurulur. Sürüm modu karşılaştırması ayıklamak, bir paket için farklı bir biçimlendirme işleme desteklemek için işleme yardımcı yöntemi ScriptBundle ve StyleBundle sınıfları içerir. Hata ayıklama modunda olduğunda, işleme paketteki her bir kaynak için biçimlendirme oluşturur. Yayın modunda olduğunda, işleme tüm paketi için bir tek biçimlendirme öğesi oluşturur. Geçiş hata ayıklama ve yayın arasında modu aşağıda gösterildiği gibi web.config dosyasında derleme öğesinin hata ayıklama özniteliği değiştirerek gerçekleştirilebilir:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Ayrıca, etkinleştirme veya devre dışı bırakma en iyi duruma getirme doğrudan BundleTable.EnableOptimizations özelliği aracılığıyla ayarlanabilir.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Paketlenen dosyalar, ilk alfabetik olarak sıralanırlar (bunlar görüntülenir şekilde **Çözüm Gezgini**). Bunlar ardından kitaplıkları bilinen böylece düzenlenir ve kendi özel uzantıları (örneğin, jQuery, MooTools ve Dojo) önce yüklenir. Örneğin, yukarıda gösterildiği gibi betikler klasörüne paketleme için son siparişin olacaktır:

1. JQuery 1.6.2.js
2. JQuery ui.js
3. jquery.tools.js
4. a.js

CSS dosyaları da alfabetik ve reset.css ve normalize.css önce başka bir dosyayı gelmesini sağlamak için daha sonra yeniden düzenlenir. Yukarıda gösterilen stilleri klasörü paketleme son sıralama bu olacaktır:

1. Reset.css
2. Content.css
3. Forms.css
4. Globals.css
5. Menu.css
6. Styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Web barındırma için performans iyileştirmeleri

.NET Framework 4.5 ve Windows 8, web sunucusu iş yükleri için önemli ölçüde performans artışı elde yardımcı olabilecek özellikleri tanıtır. Bu azaltma içerir (en fazla % 35) hem de başlangıç zamanı ve ASP.NET kullanan siteleri barındırma Web bellek alanı.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Ana performans Etkenler

İdeal olarak, tüm Web siteleri etkin olmalı ve sonraki istek için hızlı yanıt güvence altına almak için bellekte her birlikte gelir. Site yanıtlama etkileyen faktörler şunlardır:

- Bir uygulama havuzu geri dönüştürüldüğünde sonra yeniden başlatmak bir site için geçen süre. Bu site derlemeleri artık bellekte olduğunda site için bir web sunucusu işlemi başlatmak için gereken süre anlamına gelmektedir. (Diğer siteler tarafından kullanıldığından platform derlemeler hala bellekte olduğundan.) Bu durum "soğuk site olarak sıcak framework başlatma" adlandırılır veya yalnızca "soğuk site başlangıç."
- Ne kadar bellek site kaplar. Bu koşulları olan "site başına bellek tüketimi" veya "çalışma kümesi paylaşımı kaldırılabilir."

Yeni performans geliştirmeleri odaklanmak faktörlerin her ikisini de.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Yeni performans özellikleri için gereksinimler

Yeni özellikleri için gereksinimler bu kategoriye ayrılabilir:

- .NET Framework 4'üzerinde çalıştırmak geliştirmeleri.
- .NET Framework 4.5 gerektirir, ancak tüm Windows sürümlerinde çalıştırabilirsiniz geliştirmeleri.
- Yalnızca .NET Framework 4.5, Windows 8'de çalıştırma ile kullanılabilen geliştirmeleri.

Performans etkinleştirmek kaydedebileceğiyle geliştirme her düzeyini artırır.

.NET Framework 4.5 geliştirmeleri bazıları, diğer senaryolar için uygulamak daha geniş performans özellikleri yararlanın.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Ortak derlemeleri paylaşma

**Gereksinim**: .NET Framework 4 ve Visual Studio 11 Developer Preview SDK'sı

Bir sunucuda farklı siteleri sık aynı yardımcı derlemeler (örneğin, bir starter kit veya örnek uygulaması derlemelerden) kullanın. Her site bu derlemeler kopyasını kendi Bin dizinine sahiptir. Nesne kodu derlemeler için aynı olsa da, her derleme soğuk site başlatma sırasında ayrı olarak okunacak nedenle fiziksel olarak ayrı derlemeler olduğunuz ve ayrı olarak bellekte depolanır.

Yeni interning işlevselliği bu Etkisizliği çözer ve RAM gereksinimlerine ve yükü azaltır zaman. Sağlar interning Windows dosya sisteminde her derleme tek bir kopyasını tutmak ve site depo klasörlerde ayrı ayrı derlemeler tek kopyalanacak sembolik bağlantılar ile değiştirilir. Tek bir site ayrı bir derleme sürümü gerekirse, sembolik bağlantıyı derlemeyi yeni sürümü tarafından değiştirilir ve yalnızca o site etkilenir.

Sembolik bağlantılar kullanarak derlemeleri paylaşma gerektirir aspnet adlı yeni bir aracı\_oluşturmak ve interned derlemeler deposu yönetmenize olanak tanır intern.exe. Visual Studio 11 Developer Preview SDK'sı bir parçası olarak sağlanır. (Ancak, yalnızca .NET Framework yüklü, en son yüklediğiniz varsayılarak 4 sahip bir sistem üzerinde çalışır [güncelleştirme](https://support.microsoft.com/kb/2468871).)

Tüm uygun derlemelerin interned emin olmak için ASP.NET çalıştıran\_intern.exe düzenli aralıklarla (örneğin, zamanlanmış bir görev olarak haftada bir kez). Tipik kullanım aşağıdaki gibidir:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Tüm seçenekleri görmek için bağımsız değişkenler olmadan aracı çalıştırın.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Çok çekirdekli JIT derleme için hızlı başlangıç kullanma

**Gereksinim**: .NET Framework 4.5

Soğuk site başlangıç için yalnızca derlemeleri diskten okunan zorunda ancak site JIT derlenmiş olmalıdır. Karmaşık bir site için bu belirgin gecikmeler ekleyebilirsiniz. .NET Framework 4.5 içinde yeni bir genel amaçlı teknik kullanılabilir işlemci çekirdeği arasında JIT derleme yayarak bu gecikmelerini azaltır. Bunu kadar yapar ve mümkün olduğunca erken sırasında toplanan bilgileri kullanarak önceki sitesini başlatır. Bu işlev tarafından uygulanan [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) yöntemi.

Bu özelliğin avantajlarından yararlanmak için bir şey yapmanız gerekmez şekilde JIT derleme-birden çok çekirdek kullanarak ASP.NET, varsayılan olarak etkindir. Bu özellik devre dışı bırakmak istiyorsanız, aşağıdaki ayarları Web.config dosyasında yapın:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Bellek için en iyi duruma getirme ayarlama çöp toplama

**Gereksinim**: .NET Framework 4.5

Bir site çalışmaya başladıktan sonra atıktoplayıcı (GC) yığın kullanımı, bellek tüketimi önemli bir etken olabilir. Tüm atık toplayıcı gibi .NET Framework GC CPU süresi (sıklığı ve koleksiyonları önemini) ve bellek kullanımı (yeni, bırakılmış veya serbest-mümkün nesneler için kullanılan ek boşluk) arasındaki dengelemeden yapar. Önceki sürümler için Kılavuzu doğru dengeyi elde etmek için GC yapılandırma konusunda sağladık (örneğin, [ASP.NET 2.0/3.5 paylaşılan barındırma yapılandırma](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

.NET Framework 4.5, birden fazla tek başına ayarı, bir iş yükü tanımlı yapılandırma ayarı yerine kullanılabilir tüm daha önce önerilen GC ayarları yanı sıra yeni ayar, her site için ek bir performans sunduğundan sağlayan çalışma kümesi.

GC belleğini etkinleştirmek için aşağıdaki ayar Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config dosyaya ekleyin:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Bu ayar eski ayarlarını değiştirir aspnet.config için önceki Kılavuzu değişikliklerin farkları bilmiyorsanız, Not — Örneğin, gcServer, gcConcurrent vb. ayarlamak için gerek yoktur. Eski ayarları kaldırmanız gerekmez.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Web uygulamaları için prefetching

**Gereksinim**: .NET Framework 4.5, Windows 8'de çalıştırma

Çeşitli yayınları için Windows olarak bilinen bir teknoloji eklemiştir [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) , uygulama başlangıcından disk okuma maliyetini azaltır. Soğuk başlangıç daha istemci uygulamaları için bir sorun olduğundan, bu teknoloji, yalnızca bir sunucuya temel bileşenlerini içeren Windows Server almamıştır. Prefetching artık burada tek tek Web sitelerinin başlatma iyileştirebilirsiniz, Windows Server'ın en son sürümünde kullanılabilir.

Windows Server için prefetcher varsayılan olarak etkin değildir. Etkinleştirmek ve yüksek yoğunluklu web barındırma için prefetcher yapılandırmak için komut satırında aşağıdaki komut kümesini çalıştırın:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Ardından, prefetcher ASP.NET uygulamaları ile tümleştirmek için Web.config dosyasında aşağıdakileri ekleyin:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Forms

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Kesin türü belirtilmiş veri denetimleri

ASP.NET 4.5 Web formları verilerle çalışmak için bazı geliştirmeler içerir. İlk geliştirme kesin türü belirtilmiş veri denetimleri ' dir. Veri bağlama değerini kullanarak önceki sürümlerinde, ASP.NET Web Forms denetimleri için görüntüleme *Eval* ve veri bağlama ifade:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

İki yönlü veri bağlaması için kullandığınız *bağlamak*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Çalışma zamanında bu çağrıları yansıma belirtilen üyenin değerini okumak ve ardından sonucu biçimlendirmede görüntülemek için kullanın. Bu yaklaşım, veri bağlama rasgele, unshaped veri karşı kolaylaştırır.

Ancak, bu gibi veri bağlama ifadeleri üye adları, gezinti (gibi Tanıma Git) ya da bu adları için denetleme zamanı için IntelliSense gibi özellikleri desteklemez.

Bu sorunu gidermek için ASP.NET 4.5 bir denetimin bağlı olduğu verilerin veri türü bildirmek için yeteneği ekler. Yeni kullanarak bunu *ItemType* özelliği. Bu özellik ayarladığınızda, iki yeni yazılan değişkenler veri bağlama ifadeleri kapsamında kullanılabilir: *öğesi* ve *BindItem*. Değişkenleri kesin türü belirtilmiş olduğundan, Visual Studio geliştirme deneyimi tüm faydalarını alın.


İki yönlü veri bağlama ifadeleri kullanma *BindItem* değişkeni:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


Veri bağlamayı destekleyen çoğu denetimlerinde ASP.NET Web Forms framework desteklemek için güncelleştirilmemiş *ItemType* özelliği.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Model Binding

Model bağlama kod odaklı veri erişimi ile çalışmak için veri bağlama ASP.NET Web Forms denetimlerinde genişletir. Gelen kavramları içerir *ObjectDataSource* denetim ve ASP.NET MVC, model bağlama.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Verileri seçme

Model bağlama verileri seçmek için kullanılacak veri denetimini yapılandırmak için denetimin ayarlayın *SelectMethod* özelliğini sayfanın kodda bir yöntemin adı. Veri denetimi sayfa yaşam döngüsündeki uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar. Açıkça çağırmak için gerek yoktur *DataBind* yöntemi.

Aşağıdaki örnekte, *GridView* denetim adlı bir yöntem kullanmak üzere yapılandırılmış *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Oluşturduğunuz *GetCategories* sayfanın kodda yöntemi. Basit bir select işlemi yöntemi parametresiz gerekir ve döndürmelidir bir *IEnumerable* veya *Iqueryable* nesnesi. Varsa yeni *ItemType* özelliği ayarlanmış (hangi etkinleştirir kesinlikle altında açıklandığı gibi veri bağlama ifadeleri yazılan [kesin türü belirtilmiş veri denetimleri](#_Toc318097386) önceki), bu arabirimleri genel sürümleri döndürülmesi gereken — *IEnumerable&lt;T&gt;*  veya *Iqueryable&lt;T&gt;*, ile *T* parametre türü eşleşen *ItemType* özelliği (örneğin, *Iqueryable&lt;kategori&gt;*).

Aşağıdaki örnek kodunu gösteren bir *GetCategories* yöntemi. Bu örnek, Northwind örnek veritabanı ile Entity Framework Code First modeli kullanır. Kod sorgu tarafından yolu, her kategori için ilgili ürünleri ayrıntılarını döndürür emin olur *INCLUDE* yöntemi. (Bu sağlar *TemplateField* işaretleme öğesinde görüntüler ürün sayısı her kategoride gerek kalmadan bir [n + 1 seçin](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Sayfa çalıştığında, *GridView* denetim çağrıları *GetCategories* yöntemi otomatik olarak ve yapılandırılmış alanlarını kullanarak döndürülen verileri işler:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Select yöntemi döndürdüğünden bir *Iqueryable* nesnesi *GridView* denetim daha fazla işlemek sorguyu çalıştırmadan önce. Örneğin, *GridView* denetim, sıralama ve döndürülen için disk belleği için sorgu ifadeleri ekleyebilirsiniz *Iqueryable* çalıştırılmadan önce böylece bu işlemler temel aldığı gerçekleştirilir nesnesi LINQ sağlayıcısı. Bu durumda, bu işlemler veritabanında gerçekleştirilen Entity Framework sağlayacaktır.

Aşağıdaki örnekte gösterildiği *GridView* denetim değiştiren sıralama ve disk belleği izin vermek için:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Şimdi sayfa çalıştığında denetimi yalnızca geçerli sayfa veri görüntülenir ve seçilen sütuna göre sıralanır emin olabilirsiniz:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Döndürülen verileri filtrelemek için select yöntemi eklenecek parametrelerine sahip. Bu parametreler, çalışma zamanında model bağlama tarafından doldurulur ve sorgu veri döndürmeden önce değiştirmek için kullanabilirsiniz.

Örneğin, sorgu dizesinde bir anahtar sözcüğünü girerek kullanıcılar filtre ürünleri izin vermek istediğinizi varsayalım. Bir parametre yöntemine ekleyin ve parametre değeri kullanmak için kodu güncelleştirme:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Bu kodu içeren bir *nerede* için bir değer sağlanmazsa ifade *anahtar sözcüğü* ve sorgu sonuçlarını döndürür.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Değer sağlayıcıları

Önceki örnekte yerleri hakkında belirli değildi değeri *anahtar sözcüğü* parametre geliyor. Bu bilgileri belirtmek için bir parametre özniteliği kullanabilirsiniz. Bu örnek için kullandığınız *QueryStringAttribute* içinde sınıf *System.Web.ModelBinding* ad alanı:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Bu model bağlama için Sorgu dizesinden bir değer bağlanmaya bildirir *anahtar sözcüğü* çalışma zamanında parametre. (Bu durumda değil ancak bu tür dönüştürme gerçekleştirme gerektirebilir.) Bir değer sağlanamaz ve türü null ise, bir özel durum oluşur.

Bu yöntemlere ait değerlerin kaynakları değer sağlayıcıları olarak adlandırılır ve kullanacağı değer sağlayıcısını belirtmek parametre öznitelikleri değer sağlayıcı öznitelik olarak adlandırılır. Web Forms değer sağlayıcıları ve tüm kullanıcı girişi tipik kaynakları için karşılık gelen öznitelikleri sorgu dizesi, tanımlama bilgileri, form değerleri, denetimleri, Görünüm durumu, oturum durumu ve profil özellikleri gibi bir Web Forms uygulamasında içerir. Özel değer sağlayıcıları de yazabilirsiniz.

Varsayılan olarak, parametre adı, değer sağlayıcı koleksiyonda bir değeri bulmak için anahtar olarak kullanılır. Örnekte, anahtar sözcüğü adlı bir sorgu dizesi değeri için kod görünecektir (örneğin, ~ / default.aspx?keyword=chef). Parametre özniteliği için bağımsız değişken olarak geçirerek bir özel anahtar belirtebilirsiniz. Örneğin, q adlı sorgu dizesi değişkeni değerini kullanmak için bunu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Bu yöntem sayfa kodunda ise, kullanıcıların sorgu dizesi kullanarak bir anahtar sözcük geçirerek sonuçları filtreleyebilirsiniz:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Model bağlama yoksa el ile kod gerekirdi birçok görevleri gerçekleştirir: değer okuma, null değerini denetimi, uygun türe dönüştürme yapma, dönüştürme başarılı olup olmadığını denetleme ve son olarak, değeri kullanarak Sorgu. Uygulamanız genelinde işlevselliği yeniden kullanma olanağı ve çok daha az kod de sonuçları bağlama model.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Kontrolü değerlere göre filtreleme

Kullanıcının aşağı açılan listeden bir filtre değeri seçmesine izin vermek için örneği genişletmek istediğinizi varsayalım. Aşağıdaki açılan listede biçimlendirme ekleyin ve başka bir yöntemi kullanarak kendi veri almak için yapılandırın *SelectMethod* özelliği:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Genellikle de eklemeniz bir *EmptyDataTemplate'i* öğesine *GridView* kontrol böylece eşleşen hiç ürün bulunursa denetimi bir ileti görüntülenir:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Sayfa kodunda, yeni açılan liste için bir yöntem seçin ekleyin:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Son olarak, güncelleştirme *GetProducts* seçin açılır listeden seçilen kategori Kimliğini içeren yeni bir parametre alan yöntemi:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Artık sayfa çalıştığında, kullanıcılar aşağı açılan listeden bir kategori seçebilir ve *GridView* denetimidir filtre uygulanmış verileri göstermek için otomatik olarak yeniden bağlama. Model bağlama select yöntemleri için parametrelerinin değerlerini izler ve herhangi bir parametre değeri sonra geri gönderimin değiştirilip değiştirilmediğini algılar olasıdır. Bu durumda, model bağlama verileri yeniden bağlamak için ilişkili veri denetimi zorlar.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>HTML olarak kodlanmış veri bağlama ifadeleri

Veri bağlama ifadeleri sonucunu şimdi HTML olarak kodlanacak olabilir. İki nokta üst üste (:) sonuna ekleyin &lt;veri bağlama ifade işaretler % # öneki:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Örtük doğrulama

Örtük JavaScript için istemci tarafı doğrulama mantığını kullanmak için yerleşik Doğrulayıcı denetimleri artık yapılandırabilirsiniz. Bu önemli ölçüde satır sayfa biçimlendirmesi içinde işlenen JavaScript miktarını azaltır ve genel sayfa boyutunu azaltır. Bu yollardan herhangi birini kullanarak örtük JavaScript doğrulama denetimleri için yapılandırabilirsiniz:

- Aşağıdaki ayarı ekleyerek genel  *&lt;appSettings&gt;*  öğesi Web.config dosyasında: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Statik ayarlayarak genel *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* özelliğine *UnobtrusiveValidationMode.WebForms* (genellikle, *uygulama \_Başlat* Global.asax dosyasındaki yöntemi).
- Yeni ayarlayarak tek tek bir sayfa için *UnobtrusiveValidationMode* özelliği *sayfa* sınıfının *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5 güncelleştirmeleri

Bazı iyileştirmeler HTML5, yeni özelliklerden yararlanmak için sunucu denetimleri Web formları için yapılmıştır:

- *Metin modu* özelliği *TextBox* denetimi gibi yeni HTML5 giriş türlerini desteklemek üzere güncelleştirilmiştir *e-posta*, *datetime*, ve benzeri.
- *Dosya yükleme* denetim artık birden çok dosya yüklemeleri bu HTML5 özelliği destekleyen tarayıcılardan destekler.
- Doğrulayıcı şimdi destek doğrulama HTML5 giriş öğeleri denetler.
- Bir URL şimdi temsil özniteliklere sahip yeni HTML5 öğeleri destek runat = "server". Sonuç olarak, URL yollarında ASP.NET kuralları gibi kullanabilirsiniz ~ uygulama kökü temsil etmek için işleci (örneğin, &lt;video runat = "server" src="~/myVideo.wmv" /&gt;).
- *UpdatePanel* denetim sabit nakil HTML5 giriş alanı desteklemek için.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta olduğu artık Visual Studio 11 Beta ile dahil. ASP.NET MVC, Model-View-Controller (MVC) deseninden yararlanarak yüksek düzeyde sınanabilir ve sürdürülebilir Web uygulamaları geliştirmek için kullanılan bir çerçevedir. ASP.NET MVC 4 mobil Web uygulamaları oluşturmak kolay hale getirir ve herhangi bir aygıtı ulaşabilir HTTP Hizmetleri oluşturmanıza yardımcı olan ASP.NET Web API içerir. Daha fazla bilgi için bkz: [ASP.NET MVC 4 sürüm notları](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web Sayfaları 2

Yeni özellikler şunları içerir:

- Yeni ve güncelleştirilmiş site şablonları.
- Sunucu tarafı ve istemci tarafı doğrulama kullanarak ekleme *doğrulama* Yardımcısı.
- Bir varlık Yöneticisi'ni kullanarak komut dosyalarını kaydetme yeteneği.
- Facebook ve OAuth ve Openıd kullanarak diğer sitelere oturum açmayı etkinleştirme.
- Ekleme eşlemeleri kullanarak *eşlemeleri* Yardımcısı.
- Web Pages uygulamaları yan yana çalışıyor.
- Mobil cihazlar için işleme sayfaları.

Bu özellikler ve tam kod örnekleri hakkında daha fazla bilgi için bkz: [Web Pages 2 Beta üst özelliklerinde](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Bu bölümde Visual Web Developer 11 Beta ve Visual Studio 2012 Yayın Adayı web geliştirme için iyileştirmeleri hakkında bilgi sağlar.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Proje Visual Studio 2010 ve Visual Studio 2012 Sürüm Adayı (Proje uyumluluk) arasında paylaşma

Visual Studio 2012 Yayın Adayı kadar daha yeni bir Visual Studio sürümünde var olan bir proje açma Dönüştürme Sihirbazı'nı başlattı. Bu içeriği (varlıklar) bir proje ve çözüm yükseltmesini geriye dönük olarak uyumlu bulunmayan yeni biçimlerine zorlandı. Bu nedenle, dönüştürmeden sonra projeyi Visual Studio eski sürümü açılamadı.

Birçok müşteri bize bu sağ yaklaşım olmadığını istiyordu. Visual Studio 11 Beta'da artık paylaşım projeler ve çözümler Visual Studio 2010 SP1 ile desteklenmektedir. Bu Visual Studio 2012 Sürüm Adayı'nda 2010 proje açarsanız, hala projeyi Visual Studio 2010 SP1'de açabilirsiniz olacağı anlamına gelir.

> [!NOTE]
> Proje birkaç türleri, Visual Studio 2010 SP1 ve Visual Studio 2012 Yayın Adayı arasında paylaşılamaz. Bunlar bazı eski projeleri (örneğin, ASP.NET MVC 2 projeleri için) veya projeleri özel amacıyla (örneğin, Kurulum projeleri) içerir.

Visual Studio 2010 SP1 Web projesini Visual Studio 11 Beta ilk kez açtığınızda, aşağıdaki özellikleri proje dosyasına eklendi:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation ve OldToolsVersion proje dosyası yükseltme işlemi tarafından kullanılır. Bunlar, Visual Studio 2010 project ile çalışma hakkında herhangi bir etkisi olacaktır.

VisualStudioVersion geçerli proje için Visual Studio sürümünü gösterir MSBuild 4.5 tarafından kullanılan yeni bir özelliktir. Bu özellik MSBuild 4.0 (Visual Studio 2010 SP1'in kullandığı MSBuild sürümü) olduğundan, bu kaydetmedi proje dosyasına bir varsayılan değer ekliyoruz.

VSToolsPath özelliği MSBuildExtensionsPath32 ayarıyla temsil yolundan almak için doğru .targets dosyasında belirlemek için kullanılır.

İçeri aktarma öğelerine ilgili bazı değişiklikler vardır. Bu değişiklikler, her iki Visual Studio sürümleri arasındaki uyumluluk desteklemek için gereklidir.

> [!NOTE]
> Bir proje Visual Studio 2010 SP1 ve Visual Studio 11 Beta arasında iki farklı bilgisayarlarda paylaşılıyor ve uygulamada yerel bir veritabanı proje içeriyorsa,\_veri klasörü, veritabanı tarafından kullanılan SQL Server sürümü olduğundan emin olmalısınız Her iki bilgisayarda yüklü.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>ASP.NET 4.5 Web sitesi şablonları yapılandırma değişiklikleri

Varsayılan olarak aşağıdaki değişiklikler yapılmıştır *Web.config* Visual Studio 2012 Sürüm Adayı'nda Web sitesi şablonları kullanılarak oluşturulan site için dosyası:

- İçinde `<httpRuntime>` öğesi, `encoderType` özniteliği şimdi ayarlanmış varsayılan olarak ASP.NET ile eklenen Antixss'i türlerini kullanın. Ayrıntılar için bkz [Antixss'i Kitaplığı](#_Toc318097382).
- Ayrıca, `<httpRuntime>` öğesi, `requestValidationMode` özniteliği "4.5" olarak ayarlanmış. Bu, varsayılan olarak, istek doğrulama ertelenmiş ("yavaş") doğrulama kullanmak için yapılandırıldığını anlamına gelir. Ayrıntılar için bkz [yeni ASP.NET istek doğrulama özellikleri](#_Toc318097379).
- `<modules>` Öğesinin `<system.webServer>` bölüm içermiyor bir `runAllManagedModulesForAllRequests` özniteliği. (Varsayılan değeri False'tur.) Başka bir deyişle, SP1'e güncelleştirilmemiş IIS 7 sürümünü kullanıyorsanız, yeni site yönlendirme sorunlar olabilir. Daha fazla bilgi için bkz: [IIS 7 ASP.NET yönlendirmesi için yerel destek](#Native_Support_In_IIS7_For_ASPNET_Routine).

Bu değişiklikler, mevcut uygulamaları etkilemez. Ancak, bunlar mevcut Web siteleri ve yeni şablonlar kullanarak ASP.NET 4.5 için oluşturduğunuz yeni Web siteleri arasında bir fark davranış gösterebilir.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>IIS 7 ASP.NET yönlendirme için yerel destek

Bunu şu şekilde bir ASP.NET geçin, ancak uygulanan SP1 güncelleştirmesi yapılmamış IIS 7 sürümü çalışıyorsanız, etkileyebilir yeni Web sitesi projeleri için şablonlar bir değişiklik değil.

ASP.NET, yönlendirme desteklemek için uygulamaları şu yapılandırma ayarı ekleyebilirsiniz:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Zaman **runAllManagedModulesForAllRequests** true, gibi bir URL olduğunu `http://mysite/myapp/home` olsa bile, ASP.NET ile gider hiçbir *.aspx*, *.mvc*, veya benzer uzantısı URL.

IIS 7'ye yapılan bir güncelleştirme yapar **runAllManagedModulesForAllRequests** gereksiz ayarı ve yerel olarak Yönlendirme ASP.NET destekler. (Microsoft Support makalesini güncelleştirme hakkında daha fazla bilgi için bkz: [etkinleştirir işlemek için IIS 7.0 veya IIS 7.5 işleyicilerinin URL'leri isteklerini belirli bir nokta ile bitmeyen kullanılabilir bir güncelleştirme olup](https://support.microsoft.com/kb/980368).)

Web sitenizi IIS 7 üzerinde çalışıyorsa ve IIS güncelleştirdiyseniz ayarlamak gerekmez **runAllManagedModulesForAllRequests** true. Gereksiz ek yükü isteği işleme eklediğinden aslında, true olarak ayarlanması önerilmez. Bu ayar true olduğunda, tüm istekleri için olanlar da dahil olmak üzere, *.htm*, *.jpg*, ve diğer statik dosyalar da ASP.NET isteği ardışık düzeni gidin.

Visual Studio 2012 RC sağlanan şablonları kullanarak yeni bir ASP.NET 4.5 Web sitesi oluşturursanız, Web sitesi için yapılandırma içermemesi **runAllManagedModulesForAllRequests** ayarı. Bu varsayılan ayarı yanlış anlamına gelir.

Ardından Web sitesi Windows 7 SP1'in yüklü çalıştırırsanız, IIS 7 gereken güncelleştirme içermez. Sonuç olarak, yönlendirme çalışmaz ve hataları görürsünüz. Burada yönlendirme çalışmıyor bir sorun varsa, ya da aşağıdakileri yapabilirsiniz:

- Windows 7 SP1'e, ama güncelleştirme IIS 7'ye ekleyecek güncelleştirin.
- Daha önce listelenen Microsoft Support makalesini içinde açıklanan güncelleştirmeyi yükleyin.
- Ayarlama **runAllManagedModulesForAllRequests** bu Web sitesinin Web.config dosyasında true. Bu istek için bazı ek ekler unutmayın.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML Düzenleyicisi

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Akıllı görevleri

Tasarım görünümünde sunucu denetimleri genellikle karmaşık Özellikleri iletişim kutusu ve bunları kolaylaştırmak için sihirbazları ilişkilendirdiniz. Örneğin, bir veri kaynağına eklemek için bir özel iletişim kutusunu kullanabilirsiniz bir *yineleyici* denetlemek veya sütun ekleme bir *GridView* denetim.

Ancak, bu tür bir kullanıcı Arabirimi Yardım karmaşık özellikleri için kaynak görünümünde kullanılabilen yedeklenmedi. Bu nedenle, Visual Studio 11 için kaynağı görünümü akıllı görevleri tanıtır. C# ve Visual Basic düzenleyicileri yaygın olarak kullanılan özellikleri için bağlam algılayan kısayolları akıllı görevlerdir.

Ekleme noktasını öğe içinde olduğunda ASP.NET Web Forms denetimleri için sunucu etiketlerini akıllı görevler küçük bir karakter görüntülenir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Karakter'a tıklayın veya CTRL tuşuna bastığınızda akıllı görev genişletir. (, yalnızca kod düzenleyicileri olduğu gibi nokta). Ardından, Tasarım görünümünde akıllı görevleri benzer kısayolları görüntüler.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Örneğin, önceki çizimde akıllı görev GridView görevleri seçeneklerini gösterir. Sütunları Düzenle seçerseniz, aşağıdaki iletişim kutusu görüntülenir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Aynı özellikleri iletişim kutusu kümelerinde doldurma Tasarım görünümünde ayarlayabilirsiniz. Tamam'ı tıklattığınızda denetimi için biçimlendirme yeni ayarlarla güncelleştirilmiştir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>WAI ARIA desteği

Erişilebilir Web siteleri yazma giderek önemli durumundadır. [WAI ARIA erişilebilirlik standart](http://www.w3.org/WAI/intro/aria) geliştiriciler erişilebilir Web siteleri nasıl yazmalısınız tanımlar. Bu standart artık Visual Studio'da tam olarak desteklenmektedir.

Örneğin, *rol* öznitelik şimdi IntelliSense sahiptir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

WAI ARIA standart ile önek öznitelikler de tanıtır *aria -* imkan sağlayan bir HTML5 belgeye semantiği ekleyin. Visual Studio tam olarak destekler bunlar *aria -* öznitelikleri:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png)![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Yeni HTML5 parçacıkları

Daha hızlı ve kolay yaygın olarak kullanılan HTML5 biçimlendirme yazmak yapmak için Visual Studio parçacıkları, birtakım içerir. Video kod parçacığını bir örnek verilmiştir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Kod parçacığını çağırmak için iki kez IntelliSense içinde öğe seçildiğinde SEKME tuşuna basın:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Bu, özelleştirebileceğiniz parçacık oluşturur.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Kullanıcı denetimine çıkar

Büyük web sayfalarında tek tek parçaları kullanıcı denetimlere taşımak için iyi bir fikir olabilir. Yeniden düzenleme, bu formu sayfa okunabilirliğini artırmaya yardımcı olabilir ve sayfa yapısı basitleştirebilirsiniz.

Web formları sayfaları kaynak görünümünde düzenlediğinizde, kolaylaştırmak için artık bir sayfasında metni seçin, sağ tıklatın ve Extract kullanıcı denetimi için seçin:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>Kod nuggets öznitelikler için IntelliSense

Visual Studio, her zaman sunucu tarafı kodu nuggets herhangi bir sayfa veya denetim için IntelliSense sağlamıştır. Şimdi Visual Studio IntelliSense kodu nuggets da HTML özniteliklerinde içerir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Bu veri bağlama ifadeleri oluşturmak kolaylaştırır:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Bir açma veya kapatma etiketi yeniden adlandırdığınızda eşleşen etiketi otomatik yeniden adlandırma

Bir HTML öğesi yeniden adlandırırsanız (örneğin, değiştirdiğiniz bir *div* olmasını etiketi bir *üstbilgi* etiketi), karşılık gelen açma veya kapatma etiketi de gerçek zamanlı olarak değişir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Bu, burada bir kapanış etiketi veya yanlış bir değiştirme unuttunuz hata önlemeye yardımcı olur.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Olay işleyicisi oluşturma

Visual Studio artık olay işleyicileri yazma ve bunları el ile bağlama yardımcı olması için kaynak görünümünde özellikler içerir. Kaynak görünümünde bir olay adı düzenliyorsanız, IntelliSense görüntüler &lt;yeni olay oluşturma&gt;, doğru imza sahiptir, olay işleyici sayfanın kodda oluşturur:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Varsayılan olarak, olay işleme yöntemin adını denetimin kimliği olay işleyicisi kullanın:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Sonuçta elde edilen olay işleyicisi (Bu durumda, C# ') şuna benzeyecektir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Akıllı girinti

Boş bir HTML öğesini sırada içinde Enter tuşuna bastığınızda, düzenleyici ekleme noktasını doğru yerde koyun:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Bu konumda Enter tuşuna basın, kapanış etiketi aşağı taşındı ve açılış etiketi eşleşecek şekilde girintili. Ekleme noktasını da girintili:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Deyim tamamlama otomatik azaltın

Böylece yalnızca uygun seçenekleri görüntüler yazdığınız üzerinde göre Visual Studio şimdi filtreler IntelliSense listesinde:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense de IntelliSense listesi ayrı sözcükleri ilk harfler büyük filtreleri göre. Örneğin, "dt" yazarsanız, dl ve asp: DataList görüntülenir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Bu özellik, bilinen öğe için ifade tamamlama almak için hızlandırır.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript Düzenleyicisi

JavaScript Düzenleyicisi'nde Visual Studio 2012 Yayın Adayı tamamen yenidir ve Visual Studio'da JavaScript ile çalışma deneyimini önemli ölçüde artırır.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Anahat oluşturma kodu

Anahat bölgeler artık geçerli odağınız ilgili olmayan dosyanın parçalarını daraltmak sağlayarak tüm işlevler için otomatik olarak oluşturulur.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Ayraç eşleştirme

Bir açma veya kapatma parantezi ekleme noktasını geçirdiğinizde, eşleşen bir düzenleyici vurgular.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Tanıma gitme

Tanımı komutunu gidin, bir işlev veya değişken kaynağı atlamak olanak tanır.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>ECMAScript5 desteği

Düzenleyici yeni sözdizimi ve API ECMAScript5, JavaScript dil açıklar standart en son sürümünü destekler.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

DOM API'leri için IntelliSense geliştirilmiştir, birçok yeni HTML5 API'leri dahil etmek için desteğiyle *querySelector*, DOM depolaması, belgeler arası ileti gönderme, ve *tuvale*. DOM IntelliSense şimdi tek bir basit JavaScript dosyası yerine yerel tür kitaplığı tanımı tarafından yönetilir. Bu, genişletme veya değiştirmek kolaylaştırır.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC imza aşırı yüklemeleri

Ayrıntılı IntelliSense açıklamaları şimdi bildirilebilir JavaScript işlevleri ayrı aşırı yüklemeleri için yeni kullanarak  *&lt;imza&gt;*  Bu örnekte gösterildiği gibi:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Örtük başvuruları

Artık, başka bir deyişle tüm verilen JavaScript dosyası ya da Engellenenler başvuruları, içeriği için IntelliSense elde edersiniz dosya listesinde örtük olarak eklenecek merkezi bir listesi için JavaScript dosyaları da ekleyebilirsiniz. Örneğin, merkezi dosyaların listesini görmek için jQuery dosyaları ekleyebilirsiniz ve açıkça başvurulan olup olmadığını IntelliSense jQuery işlevleri için hiçbir dosya, JavaScript bloğunda alırsınız (kullanılarak / / / &lt;başvuru /&gt;) veya değil.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS Editor

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Deyim tamamlama otomatik azaltın

CSS özelliklerine bağlı CSS şimdi filtreleri ve Seçili şema tarafından desteklenen değerleri için IntelliSense listesi.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense de başlık servis talebi aramaları destekler:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Hiyerarşik girinti

CSS Düzenleyicisi'ni girinti hiyerarşik kuralları görüntülemek için nasıl basamaklı kuralları mantıksal olarak düzenlenmiş bir genel bakış sağlayan kullanır. Aşağıdaki örnekte, #list bir seçici listesini basamaklı alt ve bu nedenle girintili.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Aşağıdaki örnek, daha karmaşık devralma gösterir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Bir kuralın girinti kendi üst kurallara göre belirlenir. Hiyerarşik girinti varsayılan olarak etkindir, ancak, bu seçenekler iletişim kutusu (Araçlar, menü çubuğundan seçenekleri) devre dışı bırakabilirsiniz:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS desteği yönlendirir

Gerçek dünya CSS dosyaları yüzlerce analizini CSS girişlerini yaygın ve en yaygın olarak kullanılan olanları Visual Studio artık destekliyor gösterir. Bu destek IntelliSense ve doğrulama yıldızın içerir (\*) ve alt çizgi (\_) özelliği girişlerini:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Böylece bile uygulandığında, hiyerarşik girinti korunur tipik Seçici girişlerini de desteklenir. Bir seçici ile önüne eklediğinizden hedef Internet Explorer 7 kullanılan tipik Seçici korsan olan  *\*: ilk alt + html*. Bu kuralı kullanarak hiyerarşik girinti korur:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Vendor specific schemas (-moz-, -webkit)

CSS3 farklı zamanlarda farklı tarayıcılar tarafından uygulanmış olan birçok özellik sunar. Bu daha önce belirli tarayıcılar için kod geliştiricilerin satıcıya özgü sözdizimini kullanarak zorlandı. Bu tarayıcı özgü özellikleri artık IntelliSense içinde dahil edilir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Yorum ve uncommenting desteği

Şimdi, açıklama ve kod düzenleyicisinde (Ctrl + K, açıklamaya C ve Ctrl + K, açıklamadan kaldırmasına olanak) kullandığınız aynı kısayollarını kullanarak CSS kuralları açıklamadan çıkarın.

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Renk Seçici

Visual Studio'nun önceki sürümleri renk ilgili öznitelikleri için IntelliSense adlandırılmış renk değerleri aşağı açılan listesinden içermektedir. Bu liste, tam özellikli Renk Seçici tarafından değiştirilmiştir.

Bir renk değeri girdiğinizde, Renk Seçici otomatik olarak görüntülenir ve daha önce kullanılan renkleri varsayılan renk paletini tarafından izlenen bir listesini sunar. Fare veya klavyeyi kullanarak bir renk seçebilirsiniz.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

Listeden bir tam Renk Seçici genişletilebilir. Seçici opaklık kaydırıcısını taşıdığınızda otomatik olarak herhangi bir renk RGBA dönüştürerek alfa kanal denetlemenize olanak tanır:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Kod parçacıkları

CSS Düzenleyicisi'nde parçacıkları daha kolay ve hızlı tarayıcılar arası stilleri oluşturmak için kolaylaştırır. Tarayıcı özel ayarlar için gereken birçok CSS3 özellikleri parçacıkları şimdi alındı.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS parçacıkları simgesi adresindeki yazarak (CSS3 media sorgular gibi) Gelişmiş senaryoları destekler (@), IntelliSense listesini gösterir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Seçtiğinizde, @media değer ve SEKME tuşuna basın, aşağıdaki kod parçacığında CSS Düzenleyicisi ekler:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Kod parçacıkları gibi kodu için olan kendi CSS parçacıkları oluşturabilirsiniz.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Özel bölgeler

Kod Düzenleyicisi'nde kullanılabilir olarak durumda kod bölgeler adlı CSS düzenleme için kullanıma sunulmuştur. Bu, kolayca ilgili stili bloklar grubu sağlar.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Bir bölge daraltıldığında bölge adını görüntüler:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Sayfa Denetçisi

Page Inspector, Visual Studio IDE içinde bir web sayfası (HTML, Web Forms, ASP.NET MVC veya Web sayfaları) oluşturur ve kaynak kodu ve sonuçta çıktı inceleyin olanak sağlayan bir araçtır. ASP.NET sayfaları için sayfa denetçisi hangi sunucu tarafı kodu tarayıcıda görüntülenen HTML biçimlendirmesi üretilen belirlemenize olanak sağlar.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Sayfa denetçisi hakkında daha fazla bilgi için lütfen aşağıdaki öğreticiler bakın:

- Sayfa Denetçisi'nde kullanarak [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Sayfa Denetçisi'nde kullanarak [ASP.NET Web formları](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Yayımlama

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Yayımlama profilleri

Visual Studio 2010 yayımlama bilgi Web Uygulama projeleri için sürüm denetimindeki depolanmaz ve diğer kullanıcılarla paylaşmak için tasarlanmamıştır. Visual Studio 2012 Sürüm Adayı'nda, yayımlama profili biçimi değiştirildi. Bir takım yapı yapılan ve MSBuild üzerinde temel yapılandırmalardan yararlanmak şimdi kolaydır. Yapı yapılandırma bilgilerini Yayımla iletişim kutusunda, böylelikle derleme yapılandırmaları yayımlamadan önce kolayca geçebilirsiniz.

Yayımlama profilleri PublishProfiles klasöründe depolanır. Klasör konumu bağlıdır hangi programlama diline kullanmakta olduğunuz:

- C#: Properties\PublishProfiles
- Visual Basic: My Project\PublishProfiles

Her bir profil bir MSBuild dosyasıdır. Yayımlama sırasında bu dosyayı projenin MSBuild dosyasına içeri aktarılır. Yayımlama veya paket işleme değişiklik yapmak istiyorsanız, Visual Studio 2010'da, özelleştirmelerinizi adlı bir dosyaya koymak zorunda **ProjectName**. wpp.targets. Bu hala desteklenmektedir, ancak şimdi Özelleştirmelerinizi yayımlama profili kendisi koyabilirsiniz. Böylece, özelleştirmeleri yalnızca bu profil için kullanılır.

Ayrıca Dengeleme yayımlama artık MSBuild profillerini kullanabilirsiniz. Bunu yapmak için projeyi derlerken aşağıdaki komutu kullanın:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Proje yolunu project.csproj değerdir ve ProfileName yayımlama profili adıdır. Alternatif olarak, profili adını geçirme yerine *PublishProfile* özelliği, tam yolu için yayımlama profili iletebilir.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET ön derlemesi ve birleştirme

Web Uygulama projeleri için Visual Studio 2012 Yayın Adayı önceden derlemek ve yayımladığınızda, sitenizin içeriği veya paket proje birleştirme sağlayan Paketle/Yayımla Web özellikleri sayfasında bir seçenek ekler. Bu seçenekler görmek için Çözüm Gezgini'nde projeye sağ tıklayın, Özellikler'i seçin ve Web'i Paketle/Yayımla özellik sayfasında seçin. Aşağıdaki çizimde ön derleme seçeneği yayımlamadan önce bu uygulamayı gösterir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Bu seçenek belirlendiğinde, Visual Studio yayımladığınız her uygulama veya paket web uygulaması işlemini gerçekleştirir. Site nasıl önceden derlenmiş veya derlemeler nasıl birleştirilmiş denetlemek istiyorsanız, bu seçenekleri yapılandırmak için Gelişmiş düğmesini tıklatın.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Visual Studio'da Test web projeleri için varsayılan web sunucusu IIS Express sunulmuştur. Visual Studio geliştirme sunucusu hala geliştirme sırasında yerel web sunucusu için bir seçenek olmakla birlikte IIS Express şimdi önerilen sunucu. Visual Studio 11 Beta IIS Express'i kullanma deneyimi Visual Studio 2010 SP1'de kullanmaya çok benzer.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Sorumluluk reddi

Bu ön belge ve burada açıklanan yazılım son ticari sürümünden önce önemli ölçüde değişebilir.

Bu belgede yer alan bilgileri yayımlandığı tarih itibariyle açıklanan sorunları, Microsoft Corporation'ın geçerli görünümde temsil eder. Microsoft değişen piyasa koşullarına yanıt vermesi gerektiğinden Microsoft tarafında taahhüdü olarak yorumlanmamalıdır ve Microsoft belgenin yayımlanma tarihinden sonra sunulan bilgilerin doğruluğu garanti edemez.

Bu teknik incelemede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ.

İlgili tüm telif hakkı yasalarına uymak kullanıcının sorumluluğundadır. Telif Hakkı altındaki sınırlamaksızın, bu belgenin hiçbir bölümü çoğaltılamaz, depolanan ya da bir geri alma sisteminde sunulan ya da herhangi bir yöntem (elektronik, mekanik, fotokopi, kayıt veya diğer) veya herhangi bir amaçla aktarılan, Microsoft Corporation'ın izni hızlı yazılır.

Microsoft, patentler, patent uygulamaları, ticari markalar, telif hakkı veya bu belgedeki konuyla ilgili diğer fikri mülkiyet hakları sahip olabilir. Microsoft'tan herhangi yazılı Lisans sözleşmesindeki, bu belgeyi bulundurmak, herhangi bir lisans bu patentler, ticari markaları, telif hakkı veya diğer fikri mülkiyet hakkı sağlamaz açıkça belirtilmediği sürece.

Aksi belirtilmediği sürece, örnek şirketler, kuruluşlar, ürünler, etki alanı adları, e-posta adresleri, logolar, kişiler, yerler ve olaylar burada olan kurgusal ve tüm gerçek şirket, kuruluş, ürün, etki alanı adı, e-posta ile ilişki gösterilen adresi, logo, kişi, yer veya olay amaçlanmamıştır veya çıkarılmamalıdır.

© 2012 Microsoft Corporation. Tüm hakları saklıdır.

Microsoft ve Windows kayıtlı ticari markaları ya da Microsoft Corporation'ın ABD'de ve/veya diğer ülkelerdeki ticari markalarıdır.

Burada sözü edilen gerçek şirketler ve ürünler adları, ticari markalar ilgili sahiplerinin olabilir.
