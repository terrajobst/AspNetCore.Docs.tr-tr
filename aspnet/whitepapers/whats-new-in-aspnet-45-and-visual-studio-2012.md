---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: ASP.NET 4.5 ve Visual Studio 2012'deki yenilikler | Microsoft Docs
author: rick-anderson
description: Bu belgede, yeni özellikler ve ASP.NET 4.5 içinde sunulan geliştirmeler açıklanmaktadır. Ayrıca, web geliştirme için yapılan geliştirmeleri açıklar...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 6bbfb4aa7f29e4c189da4dfdca6f2113c7550b68
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753147"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>ASP.NET 4.5 ve Visual Studio 2012'deki yenilikler
====================
> Bu belgede, yeni özellikler ve ASP.NET 4.5 içinde sunulan geliştirmeler açıklanmaktadır. Ayrıca, Visual Studio 2012 web geliştirme için yapılan geliştirmeleri açıklar. Bu belge, ilk 29 Şubat 2012'de yayımlanmıştır.


- [ASP.NET Core çalışma zamanı ve Framework](#_Toc318097372)

    - [HTTP isteklerini ve yanıtlarını zaman uyumsuz olarak okuma ve yazma](#_Toc318097373)
    - [HttpRequest işleme geliştirmeleri](#_Toc318097374)
    - [Zaman uyumsuz olarak bir yanıt temizleme](#_Toc318097375)
    - [Destek *await* ve *görev*-tabanlı zaman uyumsuz modüller ve işleyiciler](#_Toc318097376)
    - [Zaman uyumsuz HTTP modülleri](#_Toc318097377)
    - [Zaman uyumsuz HTTP işleyicileri](#_Toc318097378)
    - [Yeni ASP.NET isteği doğrulama özellikleri](#_Toc318097379)
    - [("Geç") istek doğrulamayı ertelendi](#_Toc318097380)
    - [Doğrulanmamış istekleri için destek](#_Toc318097381)
    - [AntiXSS kitaplığından](#_Toc318097382)
    - [WebSockets protokolü desteği](#_Toc318097383)
    - [Paketleme ve Küçültme](#_Toc318097384)
    - [Web barındırma için performans geliştirmeleri](#_Toc_perf)

        - [Anahtar performans etmenleri](#_Toc_perf_1)
        - [Yeni performans özellikleri için gereksinimler](#_Toc_perf_2)
        - [Ortak derlemeleri paylaşma](#_Toc_perf_3)
        - [Hızlı Başlangıç için çok çekirdekli JIT kullanma](#_Toc_perf_4)
        - [Çöp toplama, bellek için en iyi duruma getirmeyi ayarlama](#_Toc_perf_5)
        - [Web uygulamaları için önceden getiriliyor](#_Toc_perf_6)
- [ASP.NET Web formları](#_Toc318097385)

    - [Kesin Türü Belirtilmiş Veri Denetimleri](#_Toc318097386)
    - [Model Bağlamaları](#_Toc318097387)

        - [Verileri seçme](#_Toc318097388)
        - [Değer sağlayıcıları](#_Toc318097389)
        - [Bir denetimden gelen değerlere göre filtreleme](#_Toc318097390)
    - [Kodlamalı HTML veri bağlama ifadeleri](#_Toc318097391)
    - [Örtük doğrulama](#_Toc318097392)
    - [HTML5 güncelleştirmeleri](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web sayfaları 2](#_Toc318097395)
- [Visual Studio 2012 Sürüm Adayı](#_Toc318097396)

    - [Proje Visual Studio 2010 ve Visual Studio 2012 Sürüm Adayı (Proje uyumluluğu) arasında paylaşma](#project-compatibility)
    - [ASP.NET 4.5 Web sitesi şablonlarında yapılandırma değişiklikleri](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [IIS 7 ASP.NET yönlendirmesi için yerel destek](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [HTML düzenleyicisi](#_Toc318097397)

        - [Akıllı görevleri](#_Toc318097398)
        - [WAI ARIA desteği](#_Toc318097399)
        - [Yeni HTML5 kod parçacıkları](#_Toc318097400)
        - [Kullanıcı denetimine çıkar](#_Toc318097401)
        - [Kod nuggets öznitelikler için IntelliSense](#_Toc318097402)
        - [Etiketi eşleşen bir açılış veya kapanış etiketi yeniden adlandırdığınızda otomatik yeniden adlandırma](#_Toc318097403)
        - [Olay işleyicisi oluşturma](#_Toc318097404)
        - [Akıllı girintileme](#_Toc318097405)
        - [Deyim tamamlama otomatik olarak azaltma](#_Toc318097406)
    - [JavaScript Düzenleyicisi](#_Toc318097407)

        - [Anahat oluşturma kodu](#_Toc318097408)
        - [Ayraç eşleştirme](#_Toc318097409)
        - [Tanıma Git](#_Toc318097410)
        - [ECMAScript5 desteği](#_Toc318097411)
        - [DOM IntelliSense](#_Toc318097412)
        - [VSDOC imza aşırı yüklemeleri](#_Toc318097413)
        - [Örtük başvuruları](#_Toc318097414)
    - [CSS Düzenleyicisi](#_Toc318097415)

        - [Deyim tamamlama otomatik olarak azaltma](#_Toc318097416)
        - [Hiyerarşik girintileme.](#_Toc318097417)
        - [CSS desteği yönlendirir.](#_Toc318097418)
        - [Satıcıya özgü şemaları (- moz-, - webkit)](#_Toc318097419)
        - [Yorum ve uncommenting desteği](#_Toc318097420)
        - [Renk Seçici](#_Toc318097421)
        - [Kod Parçacıkları](#_Toc318097422)
        - [Özel bölgeler](#_Toc318097423)
    - [Sayfa denetçisi](#_Toc318097424)
    - [Yayımlama](#_Toc318097425)

        - [Yayımlama profilleri](#_Toc318097426)
        - [ASP.NET ön derleme ve birleştirme](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Sorumluluk reddi](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>ASP.NET Core çalışma zamanı ve Framework

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>HTTP isteklerini ve yanıtlarını zaman uyumsuz olarak okuma ve yazma

ASP.NET 4 kullanan bir akış olarak bir HTTP istek varlığı okuma yeteneği sunulan *HttpRequest.GetBufferlessInputStream* yöntemi. Bu yöntem, istek varlığı akış erişim sağlanır. Ancak, bir isteğin süresi boyunca, bir iş parçacığını bağlı eş yürütülen.

ASP.NET 4.5, bir HTTP istek varlığı zaman uyumsuz olarak akışların okuma yeteneği ve zaman uyumsuz olarak flush özelliğini destekler. ASP.NET 4.5, çift-arabellek denetleyicileri aşağı akış HTTP işleyicileri gibi .aspx sayfası işleyicileri ve ASP.NET MVC ile kolay tümleştirme sağlar bir HTTP istek varlığı olanağı sağlar.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>HttpRequest işleme geliştirmeleri

ASP.NET 4.5'ten tarafından döndürülen Stream başvuru *HttpRequest.GetBufferlessInputStream* zaman uyumlu ve zaman uyumsuz okuma yöntemleri destekler. *Stream* döndürülen nesne *GetBufferlessInputStream* artık BeginRead hem EndRead yöntemlerini uygular. Zaman uyumsuz *Stream* yöntemleri, geçerli iş parçacığı her zaman uyumsuz bir okuma döngü yinelemesi arasında ASP.NET serbest öbekler halinde, istek varlığı zaman uyumsuz olarak okumak olanak tanır.

ASP.NET 4.5 arabelleğe alınan bir biçimde istek varlığı okumak için bir yardımcı yöntem ayrıca ekledi: *HttpRequest.GetBufferedInputStream*. Bu yeni aşırı yükleme gibi çalışır *GetBufferlessInputStream*, zaman uyumlu ve zaman uyumsuz okuma destekleme. Ancak, okuduğu şekilde *GetBufferedInputStream* ayrıca varlık bayt ASP.NET iç arabellek kopyalar, böylece aşağı akış modüller ve işleyiciler istek varlığı erişmeye devam edebilirsiniz. Örneğin, bazı, Yukarı Akış işlem hattının kodu zaten istek kullanarak varlık okuma *GetBufferedInputStream*, kullanmaya devam edebilirsiniz *HttpRequest.Form* veya *HttpRequest.Files*. Bu, bir istek (örneğin bir veritabanına büyük dosyayı karşıya yükleme akış,), zaman uyumsuz işleme gerçekleştirmek hala çalıştırma .aspx sayfaları ve ASP.NET MVC denetleyicileri daha sonra sağlar.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Zaman uyumsuz olarak bir yanıt temizleme

Bir HTTP istemci yanıtlar göndermek ne zaman istemci uzakta olduğu veya düşük bant genişlikli bağlantısı olan önemli ölçüde uzun sürebilir. Normal bir uygulama tarafından oluşturulan yanıtı bayt sayısı ASP.NET arabelleğe alır. ASP.NET tahakkuk edilen arabellek isteğin işlenmesinin en sonunda bir tek gönderme işlemi gerçekleştirir.

Arabelleğe alınan yanıtı (örneğin, bir istemci için büyük bir dosya akışı) büyük ise, düzenli aralıklarla çağırmalısınız *HttpResponse.Flush* arabelleğe alınan çıkış istemciye göndermek ve bellek kullanımı denetim altında tutun. Ancak, çünkü *Temizleme* çalıştırmalarınızı çağrılırken bir zaman uyumlu çağrı *Temizleme* hala muhtemelen uzun süren istekleri süresi boyunca bir iş parçacığı kullanır.

ASP.NET 4.5 kullanarak zaman uyumsuz olarak aktarır gerçekleştirmek için destek ekler *BeginFlush* ve *EndFlush* yöntemlerinin *HttpResponse* sınıfı. Bu yöntemleri kullanarak zaman uyumsuz modüller ve artımlı olarak veri işletim sistemi iş parçacıklarını bağlamadan bir istemciye göndermek zaman uyumsuz işleyiciler oluşturabilirsiniz. Arasında *BeginFlush* ve *EndFlush* çağrıları, ASP.NET, geçerli iş parçacığını serbest bırakır. Bu, uzun süre çalışan HTTP yüklemeleri desteklemek için gerekli olan etkin iş parçacığı toplam sayısını önemli ölçüde azaltır.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Destek *await* ve *görev* -tabanlı zaman uyumsuz modüller ve işleyiciler

.NET Framework 4 olarak adlandırılan bir zaman uyumsuz programlama konsepti sunulan bir *görev*. Görevler tarafından temsil edilir *görev* türü ve ilgili türü *System.Threading.Tasks* ad alanı. Bu derlemeler .NET Framework 4.5 çalışmak olun derleyici geliştirmeler *görev* nesneleri basit. .NET Framework 4.5, iki yeni anahtar sözcüklerle derleyiciler desteği: *await* ve *zaman uyumsuz*. *Await* anahtar sözcüğü, kod parçasını diğer bazı kod parçasına zaman uyumsuz olarak beklemesi belirten için söz dizimi toplu özellik. *Zaman uyumsuz* anahtar sözcüğü, görev tabanlı zaman uyumsuz yöntemler olarak yöntemlerini işaretlemek için kullanabileceğiniz bir ipucu temsil eder.

Birleşimi *await*, *zaman uyumsuz*ve *görev* nesnesi haline getirir, .NET 4.5 içinde zaman uyumsuz kod yazmayı sizin için çok daha kolay. ASP.NET 4.5, zaman uyumsuz HTTP modülleri ve yeni derleyici iyileştirmeleri kullanan zaman uyumsuz HTTP işleyicileri yazmanıza izin veren yeni API'ler ile bu basitleştirme destekler.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Zaman uyumsuz HTTP modülleri

Zaman uyumsuz iş döndüren bir yöntem içinde gerçekleştirmek istediğiniz varsayalım bir *görev* nesne. Aşağıdaki kod örneği, Microsoft giriş sayfasında indirmek için zaman uyumsuz bir çağrı yapan zaman uyumsuz bir yöntem tanımlar. Kullanımına dikkat edin *zaman uyumsuz* anahtar sözcüğü yöntem imzası ve *await* çağrısı *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Tüm yazmanız — .NET Framework yükleme tamamlanması bekleniyor yanı sıra sırasında çağrı yığınını indirme tamamlandıktan sonra otomatik olarak geri çağrı yığınını geriye doğru izleme otomatik olarak işler.

Şimdi bu zaman uyumsuz yöntem zaman uyumsuz bir ASP.NET HTTP modülü, kullanmak istediğiniz varsayalım. ASP.NET 4.5 bir yardımcı yöntem içerir (*EventHandlerTaskAsyncHelper*) ve yeni bir temsilci türü (*TaskEventHandler*), görev tabanlı zaman uyumsuz yöntemler eski ile tümleştirmek için kullanabileceğiniz ASP.NET HTTP ardışık düzen tarafından kullanıma sunulan zaman uyumsuz programlama modeli. Bu örnek gösterir nasıl:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Zaman uyumsuz HTTP işleyicileri

ASP.NET ile zaman uyumsuz işleyiciler yazma geleneksel yaklaşım uygulamaktır *IHttpAsyncHandler* arabirimi. ASP.NET 4.5 tanıtır *HttpTaskAsyncHandler* zaman uyumsuz işleyiciler yazmak çok daha kolay hale getiren, türetilen zaman uyumsuz temel türü.

*HttpTaskAsyncHandler* türü soyuttur ve geçersiz kılma gerektirir *ProcessRequestAsync* yöntemi. Dahili olarak ASP.NET dönüş imza tümleştirme üstlenir (bir *görev* nesne), *ProcessRequestAsync* ASP.NET işlem hattı tarafından kullanılan eski zaman uyumsuz programlama modeli ile.

Aşağıdaki örnek nasıl kullanılacağını göstermektedir *görev* ve *await* zaman uyumsuz bir HTTP işleyicisini uygulamasının bir parçası olarak:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Yeni ASP.NET isteği doğrulama özellikleri

Varsayılan olarak, ASP.NET istek doğrulamayı gerçekleştirir; biçimlendirme veya komut dosyası alanlar, üst bilgiler, tanımlama bilgilerini ve benzeri aramak için istekleri inceler. Tüm algılanırsa, ASP.NET bir özel durum oluşturur. Bu, bir ilk olası siteler arası betik saldırıları karşı savunma hattı olarak görev yapar.

ASP.NET 4.5 seçerek doğrulanmamış istek verileri okumak kolaylaştırır. ASP.NET 4.5, ayrıca önceden harici bir kitaplık olan popüler AntiXSS kitaplığından tümleştirir.

Geliştiriciler, seçmeli olarak uygulamalarına ait istek doğrulamayı devre dışı bırakma özelliği için sık sorulan. Örneğin, uygulamanız, forum yazılımı ise, kullanıcıların HTML biçimli forum gönderilerinden ve açıklamaları Gönder, ancak yine de istek doğrulamayı diğer her şey denetliyor emin olmak isteyebilirsiniz.

ASP.NET 4.5 seçerek doğrulanmamış giriş ile çalışmak kolaylaştıran iki özellik sunar: ("geç") istek doğrulamayı ve doğrulanmamış isteği verilere erişim ertelendi.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>("Geç") istek doğrulamayı ertelendi

ASP.NET 4.5 içinde varsayılan olarak tüm istek verileri olan istek doğrulama işlemine tabi. Ancak, uygulama istek verilerini gerçekten erişim kadar istek doğrulamayı erteleneceği yapılandırabilirsiniz. (Bu bazen için yavaş istek doğrulamayı, koşulları yavaş yükleme gibi belirli veri senaryoları için temel olarak adlandırılır.) Ertelenmiş doğrulama ayarlayarak Web.config dosyasında kullanmak için uygulamayı yapılandırabilirsiniz *requestValidationMode* 4.5 içinde özniteliği *httpRUntime* öğesi, aşağıdaki örnekte olduğu gibi:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

İstek doğrulama modu 4.5 olarak ayarlandığında, istek doğrulamanın yalnızca belirli bir talep değeri için ve yalnızca kodunuzun bu değeri eriştiğinde tetiklenir. Kodunuzu Request.Form["forum değerini alır. Örneğin,\_sonrası"], istek doğrulama yalnızca bir form koleksiyonu bu öğe için çağrılır. Diğer öğelerin hiçbiri *Form* koleksiyon doğrulanır. Koleksiyondaki herhangi bir öğe erişildiğinde ASP.NET önceki sürümlerinde, tüm istek koleksiyon için istek doğrulamayı tetiklendi. Yeni davranış diğer parçaları istek doğrulamayı tetiklemeden istek verileri farklı parçalarını aramak farklı uygulama bileşenleri için kolaylaştırır.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Doğrulanmamış istekleri için destek

Tek başına ertelenmiş istek doğrulamayı seçmeli olarak istek doğrulamayı atlayarak sorununu çözün değil. Request.Form["forum çağrısı\_sonrası"] hala tetikleyicileri, belirli bir talep değeri için doğrulama isteği. Ancak, bu alan biçimlendirme o alanda izin vermek istediğiniz olduğundan doğrulama tetiklemeden erişim isteyebilirsiniz.

Bu izin vermek için ASP.NET 4.5 istek verileri doğrulanmamış erişimi destekler. ASP.NET 4.5 içeren yeni bir *Unvalidated* koleksiyon özelliğinde *HttpRequest* sınıfı. Bu koleksiyonu gibi tüm yaygın değerleri istek verisi, erişim sağlar *Form*, *QueryString*, *tanımlama bilgilerini*, ve *Url*.

Doğrulanmamış istek verileri okumak için forum örneği kullanarak, önce yeni istek doğrulama modunu kullanmak için uygulamayı yapılandırmanız gerekir:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Ardından *HttpRequest.Unvalidated* özelliği doğrulanmamış form değeri okunamıyor:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Güvenlik - *doğrulanmamış istek verileri dikkatli kullanın!* ASP.NET 4.5 belirli doğrulanmamış isteği verilere erişmek kolaylaştırmak için koleksiyonları ve doğrulanmamış istek özellikleri eklendi. Ancak, tehlikeli metin kullanıcılara işlenmez emin olmak için ham isteği verilere özel doğrulama gerçekleştirmelisiniz.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>AntiXSS kitaplığından

ASP.NET 4.5, Microsoft AntiXSS Library popülaritesi nedeniyle, temel kodlama rutinleri konusu kitaplığın sürüm 4.0 artık içerir.

Kodlama rutinleri tarafından uygulanan *AntiXssEncoder* yeni türü *System.Web.Security.AntiXss* ad alanı. Kullanabileceğiniz *AntiXssEncoder* doğrudan çağırarak herhangi bir tür içinde uygulanan kodlama yöntemleri statik türü. Ancak, bir ASP.NET uygulamasını kullanmak için yeni bir anti-XSS yordamları kullanmak için en kolay yaklaşım olan *AntiXssEncoder* varsayılan sınıfı. Bunu yapmak için Web.config dosyasına aşağıdaki özniteliği ekleyin:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Zaman *encoderType* kullanılacak özniteliği ayarlanmış *AntiXssEncoder* tüm çıkış türü, ASP.NET'te otomatik olarak kodlama kullanan yeni kodlama rutinleri.

ASP.NET 4.5 dahil dış AntiXSS kitaplığından bölümleri şunlardır:

- *HtmlEncode*, *HtmlFormUrlEncode*, ve *HtmlAttributeEncode*
- *XmlAttributeEncode* ve *XmlEncode*
- *UrlEncode* ve *UrlPathEncode* (yeni)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>WebSockets protokolü desteği

WebSockets Protokolü HTTP üzerinden bir istemci ve sunucu arasında güvenli, gerçek zamanlı çift yönlü iletişimler kurmak nasıl tanımlayan standartlara dayalı bir ağ protokolüdür. Microsoft, iletişim kuralı tanımlamaya yardımcı olmak için IETF ve W3C standart kuruluşlarıyla çalışmıştır. WebSockets protokolü, hem istemci hem de mobil işletim sistemleri WebSockets protokolü destekleyen önemli kaynaklara yatırım Microsoft ile herhangi bir istemci (yalnızca tarayıcıları) tarafından desteklenir.

WebSockets protokolü bir istemci ve sunucu arasındaki uzun süreli veri aktarımlarını oluşturmak çok daha kolay hale getirir. Bir istemci ve sunucu arasında gerçek uzun süreli bağlantı kurabilmesi için örneğin, bir sohbet uygulaması yazma çok daha kolay olmasıdır. Geçici Çözümler gibi düzenli Yoklamasını veya HTTP uzun bir yuva davranışını benzetmekte yoklaması başvurmadan gerekmez.

ASP.NET 4.5 ve IIS 8 ASP.NET geliştiricilerin zaman uyumsuz olarak bir WebSockets nesnesinde hem dize ve ikili veri yazma ve okuma için yönetilen API'leri kullanmak alt düzey WebSockets desteği içerir. ASP.NET 4.5 için var olan yeni bir *System.Web.WebSockets* WebSockets protokolü ile çalışmaya yönelik türler içeren ad alanı.

Bir tarayıcı istemci bir DOM oluşturarak WebSockets bağlantı kurar ve *WebSocket* bir ASP.NET uygulamasında aşağıdaki örnekteki gibi bir URL işaret eden bir nesne:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Modül veya işleyici herhangi bir türden kullanarak ASP.NET WebSockets uç noktaları oluşturabilirsiniz. Bir işleyici oluşturmak için hızlı bir şekilde .ashx dosyaları olduğu için önceki örnekte, .ashx dosya, kullanıldı.

WebSockets Protokolü göre bir ASP.NET uygulaması bir HTTP GET isteği WebSockets isteğine yükseltilmelidir isteği belirterek bir istemcinin WebSockets isteğini kabul eder. Örnek buradadır:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

*AcceptWebSocketRequest* yöntemi kabul eden bir işlev temsilcisi olduğundan, ASP.NET, geçerli HTTP isteği geriye doğru izler ve işlev temsilcisi için Denetim aktarır. Bu yaklaşım kavramsal olarak benzer nasıl kullanacağınız için *System.Threading.Thread*, burada hangi arka planda çalışma gerçekleştirilir bir iş parçacığı başlangıç temsilci tanımlarsınız.

ASP.NET ve istemci bir WebSockets anlaşması başarıyla tamamladıktan sonra ASP.NET, temsilcisini çağırır ve WebSockets uygulama çalışmaya başlar. Aşağıdaki kod örneği, ASP.NET yerleşik WebSockets desteği kullanan basit echo Uygulama gösterilmektedir:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

.NET 4.5 için desteği *await* anahtar sözcüğü ve görev tabanlı zaman uyumsuz işlemler olduğundan WebSockets uygulamaları yazmak için son derece uygundur. Kod örneği, WebSockets isteği tamamen zaman uyumsuz olarak ASP.NET içinde çalıştığını gösterir. Uygulama, bir istemciden çağırarak gönderilmek üzere bir ileti için zaman uyumsuz olarak bekler *yuva bekler. ReceiveAsync*. Bir istemciye çağırarak bir zaman uyumsuz ileti benzer şekilde, gönderebilir *yuva bekler. SendAsync*.

Tarayıcıda, uygulama üzerinden WebSockets iletileri alır. bir *onmessageoptions* işlevi. Çağrı tarayıcısı aracılığıyla ileti göndermek için *Gönder* yöntemi *WebSocket* DOM türü, bu örnekte gösterildiği gibi:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

Gelecekte, biz soyut hemen bazı alt düzey kodlama diğer bir deyişle bu sürümde WebSockets için gerekli uygulamaları, bu işlevsellik güncelleştirmeleri yayın yapabilir.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Paketleme ve küçültme

Paketleme, JavaScript ve CSS dosyaları tek tek bir dosya gibi işlem gören bir paket birleşti olanak tanır. Küçültme, boşluk ve gerekli olmayan diğer karakterleri kaldırarak, JavaScript ve CSS dosyaları toplar. Bu özellikler, Web Forms, ASP.NET MVC ve Web sayfaları ile çalışır.

Paketler, paket sınıfı veya ScriptBundle ve StyleBundle, alt sınıflarından birini kullanarak oluşturulur. Bir paket örneğini yapılandırdıktan sonra paket bir genel BundleCollection örneğine ekleyerek gelen istekler için kullanılabilir hale getirilir. Varsayılan şablonların, paket yapılandırmasını BundleConfig dosyasında gerçekleştirilir. Bu varsayılan yapılandırma, tüm çekirdek ve css dosyalarındaki şablonu tarafından kullanılan paketleri oluşturur.

Paketleri gelen görünümler içinde birkaç olası yardımcı yöntemler kullanılarak başvurulur. Sürüm modu karşılaştırması ayıklamak, bir paket için farklı biçimlendirme işlemeye desteği için işleme yardımcı yöntemi, ScriptBundle ve StyleBundle sınıfları olması. Hata ayıklama modunda olduğunda, işleme paketteki her bir kaynak için biçimlendirme oluşturur. Yayın modunda olduğunda, işleme tüm paketi için bir tek biçimlendirme öğesi oluşturur. Geçiş hata ayıklama ve yayın arasında modu aşağıda gösterildiği gibi hata ayıklama özniteliği web.config içindeki derleme öğesinin değiştirerek gerçekleştirilebilir:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Ayrıca, en iyi duruma getirme devre dışı bırakma veya etkinleştirme doğrudan BundleTable.EnableOptimizations özelliği aracılığıyla ayarlanabilir.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Paketlenen dosyalar, bunlar ilk alfabetik olarak sıralanır (bunlar görüntülenir şekilde **Çözüm Gezgini**). Özel uzantılarını (örneğin, jQuery, MooTools ve Dojo) ilk kez yükleniyorken ve bunlar ardından kitaplıkları bilinen olacak şekilde düzenlenir. Örneğin, yukarıda gösterildiği gibi Scripts klasörü paketleme için son sıralaması olacaktır:

1. JQuery 1.6.2.js
2. JQuery ui.js
3. JQuery.Tools.js
4. a.js

CSS dosyaları da alfabetik olarak sıralanır ve reset.css ve normalize.css herhangi bir dosya önce gelmesini sağlamak için daha sonra yeniden düzenledik. Yukarıda gösterilen stilleri klasörü paketleme son sıralama bu olacaktır:

1. Reset.css
2. Content.css
3. Forms.css
4. Globals.css
5. Menu.css
6. Styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Web barındırma için performans geliştirmeleri

.NET Framework 4.5 ve Windows 8, web-server iş yükleri için önemli performans artışının elde etmenize yardımcı olan özellikler sunar. Bu bir indirim içerir (en fazla %35) hem başlangıç süresi ve bellek Ayak izi, web siteleri, ASP.NET kullanmak barındırma.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Anahtar performans etmenleri

İdeal olarak, tüm Web sitelerinin etkin olmalıdır ve bellekte, hızlı yanıt olarak bir sonraki istekte güvence altına almak için her birlikte gelir. Site yanıt hızını etkileyen faktörler aşağıda verilmiştir:

- Bir uygulama havuzunu geri dönüştüren sonra yeniden başlatmak bir site için geçen süre. Site derlemeleri artık bellekte olduğunda site için bir web sunucusu işlemi başlatmak için gereken süreyi budur. (Diğer siteler tarafından kullanıldığından platformu derlemeleri bellekte hala vardır.) Bu durum "soğuk site olarak sıcak framework başlatma" adlandırılır veya yalnızca "soğuk başlangıç."
- Ne kadar bellek site kaplar. "Site başına bellek tüketimi" veya "çalışma kümesi paylaşımı kaldırılabilir." terimler bu

Yeni performans geliştirmeleri odaklanır. bu faktörlerin her ikisini de.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Yeni performans özellikleri için gereksinimler

Yeni özellikleri için gereksinimler, bu kategoriye ayrılabilir:

- .NET Framework 4'te çalışan geliştirmeleri.
- .NET Framework 4.5 gerektirir, ancak herhangi bir Windows sürümünde çalıştırabilirsiniz geliştirmeleri.
- Yalnızca Windows 8'de çalışan .NET Framework 4.5 ile kullanılabilen geliştirmeleri.

Performansı, BT'nin geliştirme her düzeyini artırır.

.NET Framework 4.5 geliştirmelerin bazılarını diğer senaryolar için geçerli olan daha geniş performans özelliklerini yararlanın.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Ortak derlemeleri paylaşma

**Gereksinim**: .NET Framework 4 ve Visual Studio 11 Developer Preview SDK'sı

Bir sunucuda farklı siteleri sık aynı yardımcı derlemeler (örneğin, bir başlangıç Seti ya da örnek uygulamasının derlemelerden) kullanın. Her site kendi Bin dizininde yer alan bu derlemeleri kendi kopyasına sahip olur. Derlemeler için nesne kodu aynı olsa bile her derleme soğuk site başlatma sırasında ayrı olarak okunması gereken şekilde fiziksel olarak ayrı derlemeler, yaptığınız ve ayrı olarak bellekte tutulur.

Yeni interning işlevselliği bu Etkisizliği çözer ve RAM gereksinimlerine hem yük azaltır zaman. Yineleniyorsa sağlar, Windows dosya sistemindeki her derleme tek bir kopyasını tutmak ve tek kopyası sembolik bağlantılar ile site Bin klasörlerde ayrı ayrı derlemeler değiştirilir. Tek bir site derlemenin farklı bir sürümünü gerekiyorsa, sembolik bağlantıyı yeni derleme sürümü tarafından değiştirilir ve yalnızca o siteye etkilenir.

Sembolik bağlantılar kullanarak derlemeleri paylaşma gerektirir aspnet adlı yeni bir aracı\_oluşturup deponun interned derlemelerin yönetmenize imkan tanıyan intern.exe. Bu, Visual Studio 11 Developer Preview SDK bir parçası olarak sağlanır. (Ancak, yalnızca .NET Framework, en son yüklediğiniz varsayılarak 4'ün yüklü olduğu bir sistemde çalışır [güncelleştirme](https://support.microsoft.com/kb/2468871).)

ASP.NET çalıştırdığınız uygun derlemelerin interned emin olmak için\_intern.exe düzenli aralıklarla (örneğin, zamanlanmış bir görev olarak haftada bir kez). Tipik bir kullanımı aşağıdaki gibidir:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Tüm seçenekleri görmek için aracı bağımsız değişken olmadan çalıştırın.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Hızlı Başlangıç için çok çekirdekli JIT kullanma

**Gereksinim**: .NET Framework 4.5

Bir site soğuk başlangıç için yalnızca diskten okunan derlemelere sahip ancak site JIT olarak derlenmiş olmalıdır. Karmaşık bir site için bu önemli gecikmelere ekleyebilirsiniz. .NET Framework 4.5 içinde yeni bir genel anlamda kullanılabilir işlemci çekirdeği JIT derlemesi yayarak bu gecikmelerini azaltır. Bunu kadar yapar ve mümkün olduğunca erken sırasında toplanan bilgileri kullanarak önceki sitesini başlatır. Bu işlev tarafından uygulanan [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) yöntemi.

Bu özelliğin avantajlarından yararlanmak için herhangi bir şey yapmanız gerekmez için JIT derleme-birden çok çekirdek kullanarak ASP.NET, varsayılan olarak etkindir. Bu özellik devre dışı bırakmak isterseniz, aşağıdaki ayarları Web.config dosyasına olun:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Çöp toplama, bellek için en iyi duruma getirmeyi ayarlama

**Gereksinim**: .NET Framework 4.5

Bir site çalışır duruma geçtikten sonra atık toplayıcı (GC) yığın kullanımı, bellek tüketimi bir etken olabilir. Herhangi bir atık toplayıcısı gibi .NET Framework GC CPU süresi (sıklığı ve koleksiyonlarının önem) ve bellek tüketimi (yeni, serbest bırakılan veya ücretsiz-mümkün nesneler için kullanılır fazladan boşluk) bir denge sağlar. Önceki sürümler için kılavuz GC doğru dengeyi elde etmek için yapılandırma konusunda sağladık (örneğin, [ASP.NET 2.0/3,5 paylaşılan barındırma Yapılandırması](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

.NET Framework 4.5, birden fazla tek başına ayarlar, bir iş yükü tarafından tanımlanan yapılandırma ayarı yerine kullanılabilir tüm daha önce önerilen GC ayarları yanı sıra yeni ayar, her site için ek performans sunan sağlayan çalışma kümesi.

GC belleğini etkinleştirmek için aşağıdaki ayar Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config dosyaya ekleyin:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Aspnet.config için değişiklikler için önceki yönergeleri ile biliyorsanız, bu ayar eski ayarları değiştirir; Örneğin, gcServer, gcConcurrent vb. ayarlamak için gerek yoktur. Eski ayarları kaldırmak erişiminiz yok.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Web uygulamaları için önceden getiriliyor

**Gereksinim**: .NET Framework 4.5, Windows 8'de çalıştırma

Çeşitli sürümler için Windows olarak bilinen bir teknoloji eklemiştir [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) , uygulama başlangıcından disk okuma maliyetini azaltır. Soğuk başlangıç istemci uygulamaları için genellikle bir sorun olduğundan, bu teknoloji, yalnızca bir sunucu için gerekli olan bileşenleri içeren Windows Server almamıştır. Önceden getiriliyor artık burada tek tek Web sitelerinin başlatma iyileştirebilir, Windows Server'ın en son sürümünde kullanılabilir.

Windows Server'de prefetcher varsayılan olarak etkin değildir. Etkinleştirme ve yüksek yoğunluklu web barındırma için prefetcher yapılandırmak için aşağıdaki komutları kümesini komut satırında çalıştırın:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Ardından, prefetcher ASP.NET uygulamalarıyla tümleştirmek için Web.config dosyasına aşağıdakileri ekleyin:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>ASP.NET Web Forms

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Kesin türü belirtilmiş veri denetimleri

ASP.NET 4.5 içinde Web Forms verilerle çalışmaya yönelik bazı geliştirmeler içerir. İlk geliştirme kesin türü belirtilmiş veri denetimleri ' dir. Önceki sürümlerinde ASP.NET Web Forms denetimleri için veri bağlama değerini kullanan bir görüntü *Eval* ve veri bağlama ifadesi:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

İki yönlü veri bağlama için kullandığınız *bağlama*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Bu çağrılar, çalışma zamanında belirtilen üyenin değerini okumak ve ardından işaretlemede sonucu görüntülemek için yansıma kullanın. Bu yaklaşım, isteğe bağlı, unshaped verilerine yönelik veri bağlama kolaylaştırır.

Ancak, bu gibi veri bağlama ifadeleri, IntelliSense gibi özellikler üye adları, gezinti (Tanıma Git gibi) veya derleme zamanı için bu adlar denetleniyor desteklemez.

Bu sorunu gidermek için bir denetimin bağlı olduğu veri veri türü bildirmek için özelliği ASP.NET 4.5 ekler. Yeni kullanarak bunu *Itemtype* özelliği. İki yeni türü belirlenmiş değişkenler bu özelliğini ayarladığınızda, veri bağlama ifadeleri kapsamında kullanılabilir: *öğesi* ve *BindItem*. Değişkenlerin kesinlikle yazıldığından, Visual Studio geliştirme deneyiminizi tam avantajlarından yararlanın.


İki yönlü veri bağlama ifadeleri kullanma *BindItem* değişkeni:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


Veri bağlamayı destekleyen çoğu denetimleri ASP.NET Web formları Framework desteklemek için güncelleştirilmiş *Itemtype* özelliği.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Model bağlama

Model bağlama, kod odaklı veri erişimi ile çalışmak için ASP.NET Web Forms denetimleriyle veri bağlama genişletir. Gelen kavramları kapsayan *ObjectDataSource* denetimi ve ASP.NET MVC, model bağlama.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Verileri seçme

Model bağlama verileri seçmek için kullanılacak veri denetimi yapılandırmak için denetimin ayarlayın. *SelectMethod* özelliğini sayfanın kodda bir yöntemin adı. Veri denetimi, sayfa yaşam döngüsü içinde uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar. Açıkça çağırmaya gerek yoktur *DataBind* yöntemi.

Aşağıdaki örnekte, *GridView* denetim adlı bir yöntem kullanacak şekilde yapılandırıldığını *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Oluşturduğunuz *GetCategories* sayfanın kodda yöntemi. Basit bir select işlemi için yöntem parametre gerekir ve döndürmelidir bir *IEnumerable* veya *Iqueryable* nesne. Varsa yeni *Itemtype* özelliği (olan veri bağlama ifadeleri altında açıklandığı gibi kesin olarak [kesin türü belirtilmiş veri denetimleri](#_Toc318097386) daha önce), bu arabirimler genel sürümleri döndürülmesi gereken — *IEnumerable&lt;T&gt;*  veya *Iqueryable&lt;T&gt;*, ile *T* parametre türü eşleşen *Itemtype* özelliği (örneğin, *Iqueryable&lt;kategori&gt;*).

Aşağıdaki örnek kodunu gösteren bir *GetCategories* yöntemi. Bu örnek Northwind örnek veritabanıyla Entity Framework Code First modeli kullanır. Sorgu sunar her kategori için ilgili ürün ayrıntılarını döndürür kodu xenapp'i *INCLUDE* yöntemi. (Bu sağlar *TemplateField* biçimlendirme öğesinde görüntüler ürün sayısı her kategoride gerek kalmadan bir [n + 1 seçin](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Sayfa çalıştığında, *GridView* denetim çağrıları *GetCategories* yöntemi otomatik olarak ve yapılandırılmış alanlarını kullanarak döndürülen veri işler:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Select yönteminin döndürdüğü için bir *Iqueryable* nesnesi *GridView* denetimi daha fazla işlemek sorgu yürütülmeden önce. Örneğin, *GridView* denetimi döndürülen sayfalama ve sıralama için sorgu ifadeleri ekleyebilirsiniz *Iqueryable* çalıştırılmadan önce bu işlemler temel olarak gerçekleştirilir böylece nesnesi LINQ sağlayıcısı. Bu durumda, Entity Framework, bu işlemleri veritabanına gerçekleştirilir sağlayacaktır.

Aşağıdaki örnekte gösterildiği *GridView* denetim değiştiren sayfalama ve sıralama izin vermek için:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Şimdi sayfa çalıştığında denetimi yalnızca verilerin geçerli sayfa görüntülenir ve seçili sütunu bulunduğundan emin hale getirebilirsiniz:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Döndürülen verileri filtrelemek için select yöntemi eklenecek parametrelere sahip. Bu parametreler, çalışma zamanında model bağlama tarafından doldurulur ve sorgu verileri döndürmeden önce değiştirmek için kullanabilirsiniz.

Örneğin, sorgu dizesinde bir anahtar sözcüğünü girerek, kullanıcıların ürünleri filtrelemek izin istediğini varsayın. Yönteme bir parametre ekleyin ve parametre değeri kullanmak için kodu güncelleştirin:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Bu kod içeren bir *burada* için bir değer sağlanmazsa ifade *anahtar sözcüğü* ve sorgu sonuçlarını döndürür.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Değer sağlayıcıları

Önceki örnekte yeri belirli değildi değeri *anahtar sözcüğü* parametre yakında. Bu bilgileri belirtmek için bir parametre özniteliği kullanabilirsiniz. Bu örnekte, kullandığınız *QueryStringAttribute* içinde sınıf *System.Web.ModelBinding* ad alanı:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Bu model bağlama için Sorgu dizesinden bir değer bağlanmaya için bildirir *anahtar sözcüğü* çalışma zamanında parametresi. (Bu durumda değil olsa da bu tür dönüştürme gerçekleştirme gerektirebilir.) Bir değer sağlanamaz ve türü atanamayan ise, bir özel durum oluşturulur.

Bu yöntemlere ait değerlerin kaynakları değer sağlayıcıları adlandırılır ve kullanmak için hangi değer sağlayıcı gösterir parametre öznitelikleri için değer sağlayıcı öznitelik olarak adlandırılır. Web Forms, sorgu dizesi, tanımlama bilgileri, form değerleri, denetimler, Görünüm durumu, oturum durumu ve profil özellikleri gibi bir Web Forms uygulaması, değer sağlayıcıları ve tüm kullanıcı girişi tipik kaynakları için karşılık gelen öznitelikleri içerecektir. Özel değer sağlayıcıları da yazabilirsiniz.

Varsayılan olarak, parametre adı, değer sağlayıcısının koleksiyona bir değeri bulmak için anahtar olarak kullanılır. Örnekte, kod anahtar sözcüğü adlı bir sorgu dizesi değerini görünecektir (örneğin, ~ / default.aspx?keyword=chef). Parametre özniteliği için bağımsız değişken olarak ileterek, özel anahtar belirtebilirsiniz. Örneğin, soru adlı sorgu-dize değişkeninin değerini kullanmak için bunu:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Bu yöntem sayfa kodunda, kullanıcıların sorgu dizesi kullanarak bir anahtar sözcük geçirerek sonuçları filtreleyebilirsiniz:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Model bağlama, aksi takdirde el ile kod gerekirdi birçok görevi gerçekleştirir: değer okunurken, null değerini denetlemek, uygun türe dönüştürme yapma, dönüştürme başarılı olup olmadığını denetleme ve son olarak, değeri kullanarak Sorgu. Sonuçlar daha az kod ve uygulamanızda işlevi yeniden kullanabilme bağlama model.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Bir denetimden gelen değerlere göre filtreleme

Aşağı açılan listeden bir filtre değeri seçmek izin vermek için örneği genişletmek istediğinizi varsayalım. Biçimlendirme için aşağıdaki açılan listeye ekleyin ve başka bir yöntemi kullanarak kendi verilerini almak için yapılandırma *SelectMethod* özelliği:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Aynı zamanda genellikle eklediğiniz bir *EmptyDataTemplate'i* öğesine *GridView* böylece eşleşen ürün yok bulunursa denetim bir ileti görüntülenir:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Sayfa kodunda, yeni aşağı açılan liste için bir yöntem Seç ekleyin:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Son olarak, güncelleştirme *GetProducts* aşağı açılan listeden seçilen kategori Kimliğini içeren yeni bir parametre alması gereken yöntemini seçin:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Şimdi sayfa çalıştığında kullanıcılar bir kategori aşağı açılan listeden seçim yapabilirsiniz ve *GridView* denetimidir filtrelenmiş verileri otomatik olarak yeniden ilişkili. Model bağlama select yöntemleri için parametre değerlerini izler ve herhangi bir parametre değeri bir geri gönderme sonra değiştirilip değiştirilmediğini algılar olduğundan, bu mümkündür. Bu durumda, model bağlama verileri yeniden bağlamak için ilişkili veri denetimi zorlar.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Kodlamalı HTML veri bağlama ifadeleri

Veri bağlama ifadeleri sonucu artık HTML kodlama yapabilirsiniz. Sonuna iki nokta üst üste (:) ekleyin &lt;veri bağlama ifadesi işaretleyen % # öneki:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Örtük doğrulama

Örtük JavaScript için istemci tarafı doğrulama mantığını kullanılacak yerleşik doğrulama denetimleri artık yapılandırabilirsiniz. Bu önemli ölçüde satır sayfa biçimlendirmesi içinde oluşturulan JavaScript azaltır ve genel sayfa boyutunu azaltır. Örtük JavaScript doğrulama denetimleri için aşağıdaki yöntemlerden birini yapılandırabilirsiniz:

- Aşağıdaki ayarı ekleyerek genel *&lt;appSettings&gt;* Web.config dosyasında öğe: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Statik ayarlayarak genel *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* özelliğini *UnobtrusiveValidationMode.WebForms* (normalde *uygulama \_Başlat* Global.asax dosyasındaki yöntemi).
- Yeni ayarlayarak ayrı ayrı bir sayfa için *UnobtrusiveValidationMode* özelliği *sayfa* sınıfının *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>HTML5 güncelleştirmeleri

Bazı iyileştirmeler, HTML5, yeni özelliklerden yararlanmak için sunucu denetimleri Web formları için yapılmıştır:

- *Metin modu* özelliği *TextBox* denetimi gibi yeni HTML5 giriş türlerini desteklemek üzere güncelleştirilmiştir *e-posta*, *datetime*, ve benzeri.
- *FileUpload* denetim artık birden çok dosyayı yükler ve bu HTML5 özelliği destekleyen tarayıcılardan destekler.
- Doğrulayıcı artık destek doğrulama HTML5 giriş öğeleri denetler.
- Artık URL'yi temsil eden öznitelikleri olan Yeni HTML5 öğeler destek runat = "server". Sonuç olarak, URL yollarında ASP.NET kuralları gibi kullanabilirsiniz ~ işleci uygulama kökünü temsil etmek için (örneğin, &lt;video runat = "server" src="~/myVideo.wmv" /&gt;).
- *UpdatePanel* denetimi, posta HTML5 giriş alanlarını desteklemek için düzeltilmiştir.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta, artık Visual Studio 11 Beta dahil. ASP.NET MVC, Model-View-Controller (MVC) deseninden yararlanarak yüksek düzeyde sınanabilir ve sürdürülebilir Web uygulamaları geliştirmeye yönelik bir çerçevedir. ASP.NET MVC 4 mobil Web uygulamaları oluşturmayı kolaylaştırır ve tüm cihazlara ulaşan HTTP Hizmetleri oluşturmanıza yardımcı olan ASP.NET Web API içerir. Daha fazla bilgi için [ASP.NET MVC 4 sürüm notları](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web Sayfaları 2

Yeni özellikler şunları içerir:

- Yeni ve güncelleştirilmiş site şablonları.
- Sunucu tarafı ve istemci tarafı doğrulama kullanarak ekleme *doğrulama* Yardımcısı.
- Bir varlık Yöneticisi'ni kullanarak komut dosyalarını kaydetme olanağı.
- Facebook ve OAuth ve Openıd kullanarak diğer sitelere oturumu açma etkinleştiriliyor.
- Ekleme haritaları kullanarak *eşler* Yardımcısı.
- Web sayfaları uygulamalarını yan yana çalışıyor.
- Mobil cihazlar için sayfa oluşturma.

Bu özellikler ve tam kod örnekleri hakkında daha fazla bilgi için bkz: [Web sayfaları 2 Beta üst özellikleri](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Bu bölümde Visual Web Developer 11 Beta ve Visual Studio 2012 Sürüm Adayı web geliştirme için geliştirmeler hakkında bilgi sağlar.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Proje Visual Studio 2010 ve Visual Studio 2012 Sürüm Adayı (Proje uyumluluğu) arasında paylaşma

Visual Studio 2012 Sürüm Adayı kadar Visual Studio'nun daha yeni bir sürümü mevcut bir projeyi açmayı Dönüştürme Sihirbazı'nı başlattı. Bu içeriği (varlıklar) bir proje ve çözüm yükseltmesini geriye dönük olarak uyumlu bulunmayan yeni biçimleri zorlandı. Bu nedenle, dönüştürmeden sonra projeyi Visual Studio'nun eski sürümde açılamadı.

Birçok müşteri bu aldığı doğru yaklaşım olmadığını olmamızı. Visual Studio 11 Beta'da artık paylaşım projeler ve çözümler Visual Studio 2010 SP1 ile desteklenmektedir. Bu, Visual Studio 2012 Sürüm Adayı'nda 2010 projesini açarsanız, yine de Visual Studio 2010 SP1 ile projeyi açabilirsiniz anlamına gelir.

> [!NOTE]
> Farklı türlerde projeler, Visual Studio 2010 SP1 ve Visual Studio 2012 Sürüm Adayı arasında paylaşılamaz. Bunlar bazı eski projeleri (örneğin, ASP.NET MVC 2 projeler için) veya projeleri özel amaçlı (örneğin, Kurulum projeleri) içerir.

İlk kez Visual Studio 11 Beta'da Visual Studio 2010 SP1 Web projesi açtığınızda, proje dosyasına aşağıdaki özellikler eklenmiştir:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags UpgradeBackupLocation ve OldToolsVersion proje dosyasını yükseltme işlemi tarafından kullanılır. Visual Studio 2010'projesinde çalışan üzerinde hiçbir etkiye sahiptirler.

VisualStudioVersion gösteren Visual Studio sürümü geçerli proje için MSBuild 4.5 tarafından kullanılan yeni bir özelliktir. Bu özellik, MSBuild 4.0 (Visual Studio 2010 SP1'i kullanan MSBuild sürümü) bulunamadı, çünkü biz bir varsayılan değer proje dosyasına yerleştirir.

VSToolsPath özellik MSBuildExtensionsPath32 ayarı tarafından temsil edilen yolunu almak için doğru .targets dosyasını belirlemek için kullanılır.

İçeri aktarma öğelerini ilgili bazı değişiklikler vardır. Bu değişiklikler, hem Visual Studio sürümleri arasındaki uyumluluk desteklemek için gereklidir.

> [!NOTE]
> Bir projeyi Visual Studio 2010 SP1 ve Visual Studio 11 Beta arasında iki farklı bilgisayarlarda paylaşılıyor ve uygulamada yerel bir veritabanı projesi içeriyorsa\_veri klasörü, veritabanı tarafından kullanılan SQL Server sürümü olduğundan emin olmalısınız Her iki bilgisayarda yüklü.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>ASP.NET 4.5 Web sitesi şablonlarında yapılandırma değişiklikleri

Varsayılan olarak aşağıdaki değişiklikler yapılmıştır *Web.config* dosyası Visual Studio 2012 Sürüm Adayı'nda Web sitesi şablonları kullanılarak oluşturulan bir site için:

- İçinde `<httpRuntime>` öğesi `encoderType` öznitelik şimdi varsayılan olarak ayarlandığı için ASP.NET eklenen AntiXSS türlerini kullanmak için. Ayrıntılar için bkz [AntiXSS kitaplığından](#_Toc318097382).
- Ayrıca `<httpRuntime>` öğesi `requestValidationMode` özniteliğini "4.5" olarak ayarlayın. Bu, varsayılan olarak, istek doğrulamayı ertelenmiş ("geç") doğrulama kullanmak için yapılandırıldığı anlamına gelir. Ayrıntılar için bkz [yeni ASP.NET isteği doğrulama özellikleri](#_Toc318097379).
- `<modules>` Öğesinin `<system.webServer>` bölümü içermiyor bir `runAllManagedModulesForAllRequests` özniteliği. (Varsayılan değer false'tur.) Bu, IIS 7, SP1 için güncelleştirilmiş bir sürümünü kullanıyorsanız, yeni site yönlendirmesinde sorunlar gerekebileceği anlamına gelir. Daha fazla bilgi için [IIS 7 ASP.NET yönlendirmesi için yerel destek](#Native_Support_In_IIS7_For_ASPNET_Routine).

Bu değişiklikler, mevcut uygulamaları etkilemez. Ancak, bir davranışı fark var olan Web siteleri ile ASP.NET 4.5 için yeni şablonlar kullanarak oluşturduğunuz yeni Web siteleri arasındaki temsil edebilir.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>IIS 7 ASP.NET yönlendirmesi için yerel destek

Bu, şu şekilde bir değişiklik ASP.NET'e ancak şablonları SP1 Güncelleştirmesi uygulanmamış yapılmamış bir IIS 7 sürümü çalışıyorsanız etkileyebilecek yeni Web sitesi projeleri için bir değişiklik değil.

ASP.NET'te, yönlendirme desteklemek için uygulamaları şu yapılandırma ayarı ekleyebilirsiniz:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Zaman **runAllManagedModulesForAllRequests** true, gibi bir URL olduğundan `http://mysite/myapp/home` olsa bile, ASP.NET gider hiçbir *.aspx*, *.mvc*, ya da benzer uzantısı URL.

IIS 7'ye yapılan bir güncelleştirme yapar **runAllManagedModulesForAllRequests** gereksiz ayarı ve yerel olarak Yönlendirme ASP.NET destekler. (Güncelleştirme hakkında daha fazla bilgi için bkz. Microsoft Support makalesini [etkinleştirir işlemek için IIS 7.0 veya IIS 7.5 işleyicilerinin URL'leri istekleri belirli bir nokta ile bitmeyen bir güncelleştirme kullanılabilir](https://support.microsoft.com/kb/980368).)

Web IIS 7'de çalışıyorsa ve IIS güncelleştirildiyse ayarlayın gerekmez **runAllManagedModulesForAllRequests** true. Gereksiz yükü isteği işleme eklediğinden aslında, doğru olarak ayarlanması önerilmez. Bu ayar true olduğunda, için dahil olmak üzere tüm istekleri *.htm*, *.jpg*, ve diğer statik dosyalar da ASP.NET isteği ardışık düzeni geçin.

Visual Studio 2012 RC sürümünde sağlanan şablonları kullanarak yeni bir ASP.NET 4.5 Web sitesi oluşturursanız, Web sitesi için yapılandırma içermemesi **runAllManagedModulesForAllRequests** ayarı. Bu ayar varsayılan olarak false anlamına gelir.

Ardından Web sitesi üzerinde Windows 7 SP1 yüklü çalıştırırsanız, IIS 7 gerekli güncelleştirme içermez. Sonuç olarak Yönlendirme çalışmaz ve hatalar görürsünüz. Burada yönlendirme çalışmaz bir sorun varsa, bunu ya da aşağıdakileri yapabilirsiniz:

- Windows 7 SP1, IIS 7'ye güncelleştirme ekleyeceksiniz güncelleştirin.
- Yukarıda listelenen Microsoft Support makalesini içinde açıklanan güncelleştirmeyi yükleyin.
- Ayarlama **runAllManagedModulesForAllRequests** bu Web sitesinin Web.config dosyasında true. Bu istek için bazı ek yükler ekler unutmayın.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>HTML Düzenleyicisi

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Akıllı görevleri

Tasarım Görünümü'nde, sunucu denetimlerinin genellikle karmaşık özellikler iletişim kutuları ve bunları ayarlamak kolay hale getirmek için sihirbaz ilişkilendirdiniz. Örneğin, bir veri kaynağına eklemek için bir özel iletişim kutusunu kullanabilirsiniz bir *Repeater* denetlemek veya sütunlara Ekle bir *GridView* denetimi.

Ancak, bu tür bir karmaşık özellikler için kullanıcı Arabirimi Yardımı Kaynak Görünümü'nde kullanılabilen ayarlanmamış. Bu nedenle, Visual Studio 11 için kaynak görünümü akıllı görevleri tanıtır. C# ve Visual Basic düzenleyicilerde yaygın olarak kullanılan özellikleri için bağlama duyarlı kısayolları akıllı görevlerdir.

Ekleme noktasını öğe içinde olduğunda küçük bir simge olarak ASP.NET Web Forms denetimleri için akıllı görevleri sunucu etiketler görünür:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

Glif tıklayın ya da CTRL + basın akıllı görev genişletir. (, yalnızca kod düzenleyicilerinden olduğu gibi nokta). Ardından, akıllı görevleri Tasarım görünümünde benzer kısayollarını görüntüler.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Örneğin, akıllı bir görev önceki çizimdeki GridView görevleri seçenekleri gösterir. Sütunları Düzenle seçeneğini belirlerseniz, aşağıdaki iletişim kutusu görüntülenir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Aynı özellikleri iletişim kutusu kümelerinde doldurma Tasarım görünümünde ayarlayabilirsiniz. Tamam'a tıkladığınızda, denetim için biçimlendirme yeni ayarlarla güncelleştirilmiştir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>WAI ARIA desteği

Erişilebilir Web siteleri yazma önemi gittikçe artan gelmektedir. [WAI ARIA erişilebilirlik standart](http://www.w3.org/WAI/intro/aria) geliştiriciler erişilebilir Web siteleri nasıl yazmalısınız tanımlar. Bu standart, artık Visual Studio'da tam olarak desteklenir.

Örneğin, *rol* öznitelik şimdi tam IntelliSense sahiptir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

WAI ARIA standart ön eki öznitelikleri da tanıtılmaktadır *aria -* izin veren bir HTML5 belgeye semantiği ekleyin. Visual Studio da tam olarak destekler bu *aria -* öznitelikleri:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png)![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Yeni HTML5 kod parçacıkları

Daha hızlı ve yaygın olarak kullanılan HTML5 biçimlendirme yazmak kolay hale getirmek için Visual Studio kod parçacıkları, birtakım içerir. Ekran kod parçacığını bir örnek verilmiştir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Kod parçacığı çağırmak için iki kez Intellisense'te öğe seçildiğinde SEKME tuşuna basın:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Bu, özelleştirebileceğiniz bir kod parçacığı oluşturur.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Kullanıcı denetimine çıkar

Büyük web sayfaları'nda, tek tek parça kullanıcı denetimleri taşımak için iyi bir fikir olabilir. Bu formu yeniden düzenleme, sayfa yapısı basitleştirebilir ve sayfa okunabilirliğini artırmak yardımcı olabilir.

Kaynak Görünümü Web Forms sayfalarında düzenlerken, kolaylaştırmak için artık bir sayfasında metni seçin, sağ tıklayın ve kullanıcı denetimi ayıklayın seçin:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>Kod nuggets öznitelikler için IntelliSense

Visual Studio, her zaman sunucu tarafı kodu nuggets herhangi bir sayfa veya denetim için IntelliSense sağlamıştır. Artık Visual Studio kod nuggets HTML özniteliklerindeki için IntelliSense'i içerir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Bu, veri bağlama ifadeleri oluşturmak kolaylaştırır:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Etiketi eşleşen bir açılış veya kapanış etiketi yeniden adlandırdığınızda otomatik yeniden adlandırma

HTML öğesi yeniden adlandırırsanız (örneğin, bir *div* olmasını etiketi bir *üstbilgi* etiketi), karşılık gelen açma veya kapatma etiketi de gerçek zamanlı olarak değişir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Bu durum, burada bir kapatma etiketi veya yanlış bir değiştirme unuttunuz hata önlemeye yardımcı olur.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Olay işleyicisi oluşturma

Visual Studio artık olay işleyicileri yazmak ve bunları el ile bağlama yardımcı olması için kaynak görünümünde özelliklerini içeriyor. Kaynak Görünümü'nde bir olay adı düzenliyorsanız, IntelliSense görüntüler &lt;yeni olay oluşturma&gt;, doğru imzaya sahip olduğu bir olay işleyicisi sayfanın kodda oluşturur:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Varsayılan olarak, olay işleme yöntemin adı için denetimin kimliği olay işleyicisi kullanın:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

Sonuçta elde edilen olay işleyicisi (Bu durumda, C# ') şöyle görünür:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Akıllı girintileme

İç ederken boş bir HTML öğesi Enter tuşuna bastığınızda, düzenleyici ekleme noktasını doğru yere yerleştirin:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Bu konumda Enter tuşuna basın, kapanış etiketi taşınır ve açılış etiketinde eşleşecek şekilde girintili. Ekleme noktasını da girintili hale getirilir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Deyim tamamlama otomatik olarak azaltma

Böylece yalnızca ilgili seçenekleri görüntüler yazdığınız üzerinde temel Visual Studio şimdi filtrelerinde IntelliSense listesi:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

Ayrıca başlık kelimeler IntelliSense listesinde durumunun filtreleri temel IntelliSense. Örneğin, "dl" yazarsanız, dl hem asp: DataList görüntülenir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Bu özellik, daha hızlı bilinen öğeler için deyim tamamlama almak için kılar.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>JavaScript Düzenleyicisi

Visual Studio 2012 Sürüm Adayı içindeki JavaScript Düzenleyicisi tamamen yenidir ve büyük ölçüde Visual Studio'da JavaScript ile çalışmanın deneyimini geliştirir.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Anahat oluşturma kodu

Anahat oluşturma bölgeleri artık dosya geçerli odağınızı ilgili olmayan bölümleri Daralt olanak tanıyan, tüm işlevler için otomatik olarak oluşturulur.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Ayraç eşleştirme

Ekleme noktasını bir açılış veya kapanış ayracı yerleştirdiğinizde, düzenleyici eşleşen bir vurgular.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Tanıma Git

Git komutu tanımı, bir işlev veya değişkeni kaynağına bağlantı sağlar.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>ECMAScript5 desteği

Düzenleyici ECMAScript5, JavaScript dilinin açıklayan standart'ın en son sürümünü yeni söz dizimini ve API'leri destekler.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>DOM IntelliSense

DOM API'leri için IntelliSense geliştirildi, birçok yeni HTML5 API'leri dahil etmek için desteğiyle *querySelector*, DOM depolama, belgeler arası Mesajlaşma ve *tuval*. DOM IntelliSense, artık tek, basit bir JavaScript dosyası yerine yerel bir tür kitaplığı tanımı tarafından yönetilir. Bu, genişletme veya değiştirmek kolaylaştırır.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>VSDOC imza aşırı yüklemeleri

Ayrıntılı IntelliSense yorumlar artık bildirilebilir JavaScript işlevleri ayrı aşırı yüklemeleri için yeni kullanarak *&lt;imza&gt;* öğesi, bu örnekte gösterildiği gibi:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Örtük başvuruları

Artık, başka bir deyişle herhangi belirli JavaScript dosyası ya da blok başvuruları içerikleri için IntelliSense alırsınız dosyaları listesinde örtük olarak eklenecek merkezi bir listesi için JavaScript dosyaları da ekleyebilirsiniz. Örneğin, jQuery dosyaları dosyaları merkezi listesine ekleyebilirsiniz ve açıkça başvurulan olmadığını dosyası, herhangi bir JavaScript bloğu içinde IntelliSense için jQuery işlevleri alırsınız (kullanılarak / / / &lt;başvuru /&gt;) veya değil.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>CSS Düzenleyicisi

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Deyim tamamlama otomatik olarak azaltma

CSS özelliklerini dayalı CSS şimdi filtreleri ve Seçili şema tarafından desteklenen değerleri için IntelliSense listesi.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense, başlık büyük/küçük aramalar da destekler:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Hiyerarşik girintileme

CSS Düzenleyicisi girinti hiyerarşik kurallarını görüntülemek için geçişli kuralları mantıksal olarak nasıl düzenlendiği genel bir bakış sunan kullanır. Aşağıdaki örnekte, #list bir seçici listesini basamaklı bir alt öğesidir ve bu nedenle girintili hale getirilir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

Aşağıdaki örnek, daha karmaşık devralma gösterir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Bir kuralın girinti üst kuralları tarafından belirlenir. Hiyerarşik girintileme varsayılan olarak etkindir, ancak, bu seçenekler iletişim kutusu (Araçlar, Seçenekler menü çubuğundan) devre dışı bırakabilirsiniz:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS desteği yönlendirir.

Analiz, gerçek hayatta kullanılan CSS dosyaları yüzlerce CSS atölyelere çok ortak olan ve artık Visual Studio, en yaygın olarak kullanılan olanları destekler gösterir. Bu destek, IntelliSense ve doğrulama yıldız içerir (\*) ve alt çizgi (\_) özelliği atölyelere:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Hiyerarşik girintileme ne zaman uygulanacağını bile korunur, böylece tipik Seçici atölyelere de desteklenir. Internet Explorer 7 hedefi için kullanılan bir tipik Seçici hack Seçici ile önüne eklediğinizden olan  *\*: ilk alt + html*. Bu kural kullanarak, hiyerarşik girintileme korur:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Satıcıya özgü şemaları (- moz - webkit-)

CSS3, farklı zamanlarda farklı tarayıcılar tarafından uygulanmış olan birçok özellik sunar. Bu daha önce geliştiricileri için kod belirli tarayıcılar için satıcıya özgü sözdizimi kullanılarak zorlandı. Bu tarayıcı özgü özellikleri artık IntelliSense'de dahil edilir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Yorum ve uncommenting desteği

Şimdi, açıklama ve Kod Düzenleyicisi'nde (Ctrl + K, açıklamaya C ve Ctrl + K, açıklamadan kaldırmasına olanak) kullandığınız aynı kısayolları kullanarak CSS kurallarını açıklamasını kaldırın.

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Renk Seçici

Visual Studio'nun önceki sürümlerinde, aşağı açılan listesini adlandırılmış renk değerlerinin renk ilgili öznitelikler için IntelliSense oluşmuştur. Bu liste, bir tam özellikli bir renk seçici tarafından değiştirilmiştir.

Bir renk değeri girdiğinizde, Renk Seçici otomatik olarak görüntülenir ve daha önce kullanılan renkler varsayılan renk paleti tarafından izlenen bir listesini sunar. Klavye veya fare kullanarak bir renk seçebilirsiniz.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

Listeden bir tam Renk Seçici genişletilebilir. Seçici opaklık kaydırıcıyı olduğunda otomatik olarak herhangi bir renk RGBA dönüştürerek alfa kanalı denetlemenize olanak tanır:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Kod parçacıkları

CSS Düzenleyicisi kod parçacıkları daha kolay ve tarayıcılar arası stiller oluşturmak için hızlı kolaylaştırır. Tarayıcı özel ayarları için gereken birçok CSS3 özellikleri kod parçacıkları artık sağlık.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

CSS parçacıkları sembol adresindeki yazarak (CSS3 media sorgular için gibi) Gelişmiş senaryoları destekler (@), IntelliSense listesini gösterir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Seçtiğinizde, @media değeri ve SEKME tuşuna basın, aşağıdaki kod parçacığı CSS Düzenleyicisi ekler:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Kod parçacıkları gibi kod ile CSS kendi parçacıklarınızı oluşturabilirsiniz.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Özel bölgeler

Zaten Kod Düzenleyicisi'nde kullanılabilir olan kod bölgeleri adlı CSS düzenleme için kullanıma sunulmuştur. Bu, kolayca ilgili stil bloğu grubu sağlar.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Bir bölge daraltıldığında bölge adını görüntüler:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Sayfa Denetçisi

Page Inspector, Visual Studio IDE içinde bir web sayfası (HTML, Web Forms, ASP.NET MVC veya Web sayfaları) oluşturur ve kaynak kodunu hem de elde edilen çıkış incelemenize olanak sağlar. bir araçtır. ASP.NET sayfaları için sayfa denetçisi hangi sunucu tarafı kodu tarayıcıda görüntülenen HTML biçimlendirmeyi üretmiştir belirlemenize olanak tanır.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Sayfa denetçisi hakkında daha fazla bilgi için lütfen aşağıdaki öğreticilere bakın:

- İçinde sayfa denetçisini kullanma [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- İçinde sayfa denetçisini kullanma [ASP.NET Web formları](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Yayımlama

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Yayımlama profilleri

Visual Studio 2010, Web Uygulama projeleri için yayımlama bilgileri, sürüm denetiminde depolanmaz ve başkalarıyla paylaşmak için tasarlanmamıştır. Visual Studio 2012 Sürüm Adayı'nda, yayımlama profilini biçimi değiştirildi. Bir takım yapıt yapılan ve MSBuild üzerinde temel yapılandırmalardan yararlanmak artık kolaydır. Derleme yapılandırmaları yayımlamadan önce kolayca geçiş yapabilirsiniz yapı yapılandırma bilgilerini Yayımla iletişim kutusunda ' dir.

Yayımlama profilleri PublishProfiles klasöründe depolanır. Klasörün konumu bağlıdır hangi programlama dili kullanıyorsunuz:

- C#: Properties\PublishProfiles
- Visual Basic: Benim Project\PublishProfiles

Her profil bir MSBuild dosyasıdır. Yayımlama sırasında bu dosya projenin MSBuild dosyasına aktarılır. Yayımlama veya paket işleme, değişiklik yapmak istiyorsanız, Visual Studio 2010'da, özelleştirmelerinizi adlı bir dosyaya koymak zorunda **ProjectName**. wpp.targets. Bu hala desteklenmektedir, ancak artık yayımlama profili kendi özelleştirmelerinizi koyabilirsiniz. Bu şekilde, özelleştirmeler, yalnızca bu profil için kullanılır.

Ayrıca yararlanarak yayımlama profillerini MSBuild artık. Bunu yapmak için projeyi oluşturduğunuzda aşağıdaki komutu kullanın:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

Proje yolu project.csproj değerdir ve ProfileName yayımlama profili adıdır. Alternatif olarak, profili adını geçirerek yerine *PublishProfile* özelliği, yolun tamamı için yayımlama profili iletebilir.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET ön derleme ve birleştirme

Web Uygulama projeleri için Visual Studio 2012 Sürüm Adayı ön derleme ve yayımladığınızda, sitenizin içeriğini veya paket proje birleştirmenize olanak tanır paket/yayımlama Web özellikleri sayfasında bir seçenek ekler. Bu seçenekleri görmek için Çözüm Gezgini'nde projeye sağ tıklayın, Özellikler'i seçin ve ardından Web'i Paketle/Yayımla özellik sayfasını seçin. Aşağıdaki çizim ön derleme seçeneği yayımlamadan önce bu uygulamayı gösterir.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Bu seçenek belirlendiğinde, yayımladığınız her uygulama veya paketle web uygulamasını Visual Studio işlemini gerçekleştirir. Nasıl site önceden derlenmiş veya derlemeleri nasıl birleştirilmiş denetlemek istiyorsanız, bu seçenekleri yapılandırmak için Gelişmiş düğmesine tıklayın.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

Visual Studio'da Test web projeleri için varsayılan web sunucusu IIS Express sunulmuştur. Visual Studio geliştirme sunucusu hala geliştirme sırasında yerel web sunucusu için bir seçenek, ancak IIS Express artık önerilen sunucu. IIS Express kullanarak Visual Studio 11 Beta'da deneyimini, Visual Studio 2010 SP1'de kullanmaya çok benzer.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Sorumluluk reddi

Bu, Başlangıç belgesi ve burada açıklanan yazılım son ticari sürümünden önce önemli ölçüde değiştirilebilir.

Bu belgede yer alan bilgileri, Microsoft Corporation'ın geçerli görünümü tarih itibariyle doğrudur ele alınan sorunlar temsil eder. Microsoft'un değişen piyasa koşullarına yanıt vermesi gerekir çünkü Microsoft tarafında taahhüdü olarak yorumlanmamalıdır ve Microsoft yayın tarihinden sonra sunulan bilgilerin doğruluğunu garanti edemez.

Bu teknik incelemeyi yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ.

İlgili tüm telif hakkı yasalara uymak kullanıcının sorumluluğundadır. Telif Hakkı altındaki sınırlamaksızın, bu belgenin hiçbir bölümü çoğaltılamaz, depolanan veya bir sistemde saklanamaz ya da herhangi bir yolla (elektronik, mekanik, fotokopi, kaydı veya diğer) veya herhangi bir amaçla, Microsoft Corporation'ın izni hızlı yazılır.

Microsoft'un patentleri, patent başvuruları, ticari markaları, telif hakları veya diğer fikri mülkiyet hakları bu belgedeki konuyla ilgili olabilir. Microsoft'tan bir yazılı lisans anlaşmasında bu belgenin bulundurmak, herhangi bir lisans bu patentlere, ticari markaları, telif hakları veya diğer fikri mülkiyet sağlamazsa açıkça belirtilmediği sürece.

Aksi belirtilmediği sürece, örnek şirketler, kuruluşlar, ürünler, etki alanı adları, e-posta adresleri, logolar, kişiler, yerler ve olaylar burada olduğunu hayal ve tüm gerçek şirket, kuruluş, ürün, etki alanı adı, e-posta ile hiçbir ilişki adı geçen adresi, logo, kişi, yer veya olay amaçlanmamıştır veya çıkarılmamalıdır.

© 2012 Microsoft Corporation. Tüm hakları saklıdır.

Microsoft ve Windows tescilli veya Amerika Birleşik Devletleri ve/veya diğer ülkelerde Microsoft Corporation'ın ticari olan.

Burada sözü edilen gerçek şirketler ve ürünler ticari markalar kendi sahiplerinin olabilir.
