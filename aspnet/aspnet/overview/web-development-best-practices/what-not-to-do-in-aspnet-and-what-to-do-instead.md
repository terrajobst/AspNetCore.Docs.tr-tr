---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: ASP.NET'te yapılmaması gerekenler ve bunların yerine yapılması gerekenler | Microsoft Docs
author: tfitzmac
description: Bu konuda, ASP.NET web projeleri içinde kişi olun birkaç yaygın hataları açıklanır. Bu, bu or önlemek için ne yapmanız gerektiğini yönelik öneriler sağlar...
ms.author: aspnetcontent
ms.date: 05/08/2014
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 9e9b192126995ac8a461b15bce69b60d57ca9ba1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806024"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>ASP.NET'te yapılmaması gerekenler ve bunların yerine yapılması gerekenler
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu konuda, ASP.NET web projeleri içinde kişi olun birkaç yaygın hataları açıklanır. Bu, bu yaygın hataları önlemek için ne yapmanız gerektiğini yönelik öneriler sağlar. Bağlı olduğu bir [sunu](http://vimeo.com/68390507) tarafından **Damian Edwards** Norveç Geliştiriciler Konferansı'konumunda.


## <a name="disclaimer"></a>Sorumluluk reddi

Bu konu, güvenli ve verimli uygulama olduğundan emin olmak için tam bir kılavuz olarak tasarlanmamıştır. Yine de güvenlik ve performans için bu konudaki belirtilmeyen en iyi uygulamaları izlemeniz gerekir. Yalnızca, .NET sınıfları ve işlemleri ile ilgili yaygın hataları önlemek nasıl önerir.

## <a name="overview"></a>Genel Bakış

Bu konu aşağıdaki bölümleri içermektedir:

- [Uyumluluk standartları](#standards)

    - [Denetim bağdaştırıcılarını](#adapters)
    - [Stil özellikleri denetimlerinde](#styleprop)
    - [Sayfa ve denetim geri çağırmaları](#callback)
    - [Tarayıcı özelliği algılama](#browsercap)
- [Güvenlik](#security)

    - [İsteği doğrulama](#validation)
    - [Cookieless form kimlik doğrulaması ve oturum](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Orta güven](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Güvenilirlik ve performans](#performance)

    - [PreSendRequestHeaders ve PreSendRequestContent](#presend)
    - [Web Forms ile zaman uyumsuz sayfası olayları](#asyncevents)
    - [İş Başlat ve unut](#fire)
    - [İstek Varlık gövdesi](#requestentity)
    - [Response.Redirect ve Response.End](#redirect)
    - [EnableViewState ve ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Uzun çalışan istek (> 110 saniye)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Uyumluluk standartları

<a id="adapters"></a>

### <a name="control-adapters"></a>Denetim bağdaştırıcılarını

Öneri: Uyarlamalı işleme için Denetim bağdaştırıcılarını kullanma durdurun ve CSS medya sorgular ve standartlara uygun HTML kullanın.

.NET 2. 0'ı farklı cihaz ve ortamları için özelleştirildiğinden sunu kod işlemek için denetimleri bağdaştırıcıları sunulur. Şimdi, Uyarlamalı bu işleme, CSS ve HTML ile gerçekleştirilebilir. Denetim bağdaştırıcılarını kullanmayı bırakmak ve herhangi bir mevcut bağdaştırıcıları için CSS ve HTML dönüştürme gerekir.

Daha fazla bilgi için [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) ve [nasıl yapılır: Mobil sayfalara ekleme bilgisayarınızı ASP.NET Web Forms / MVC uygulaması](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Stil özellikleri denetimlerinde

Öneri: denetim işaretlemede stil değerlerini ayarlama durdurun ve CSS stil sayfalarını biçimlendirme değerlerini ayarlayın.

Satır içi stil özellikleri ayarlamak için kullanılan özellikler onlarca Web sunucusu denetimleri içerir. Örneğin, bir denetim için metin rengi ForeColor özelliği ayarlar. CSS stil sayfaları ile daha verimli bir şekilde bu aynı etkiyi gerçekleştirebilirsiniz. Stil sayfaları, stil değer merkezileştirebilir ve uygulamanızda bu değerleri ayarlamaktan kaçının sağlar.

Aşağıdaki örnek, bir CSS sınıfı kırmızı kümeleri metni gösterir.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

Sonraki örnekte, CSS sınıfı dinamik olarak uygulanacak gösterilmektedir.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Sayfa ve denetim geri çağırmaları

Öneri: sayfa ve denetim geri çağırmaları kullanmayı ve aşağıdakilerden birini kullanın: AJAX, UpdatePanel, MVC eylem yöntemleri, Web API'sini veya SignalR.

ASP.NET önceki sürümlerinde, geri çağırma yöntemleri sayfası ve denetim web sitesine ait bir sayfanın tamamını yenilemeden güncelleştirmek etkin. Kısmi Sayfa güncelleştirmelerini aracılığıyla artık gerçekleştirebilirsiniz [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) veya [SignalR](../../../signalr/index.md). Kolay URL'lerle sorunlara neden olabilir çünkü geri çağırma yöntemleri kullanılarak ve yönlendirme durdurmanız gerekir. Varsayılan olarak, denetimleri geri çağırma yöntemleri etkinleştirmeyin, ancak denetimi içinde bu özellik etkinleştirilirse, bunu devre dışı.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Tarayıcı özelliği algılama

Öneri: statik tarayıcı özelliği algılama kullanarak durdurun ve bunun yerine dinamik özellik algılama kullanın.

ASP.NET önceki sürümlerinde desteklenen özellikler her tarayıcıda bir XML dosyasında saklanır. Statik bir arama yoluyla algılama özelliği desteği en iyi yaklaşım değildir. Şimdi, dinamik olarak bir desteklenen bir tarayıcıdan özellikleri gibi bir özellik algılama altyapısı kullanarak algılayabilir [Modernizr](http://modernizr.com/). Özellik algılama yöntemi veya özelliği kullanmak çalışıyor ve tarayıcı istenen sonucu üretilen varsa bkz denetleniyor destek belirler. Varsayılan olarak, Web uygulaması şablonları Modernizr dahildir.

<a id="security"></a>

## <a name="security"></a>Güvenlik

<a id="validation"></a>

### <a name="request-validation"></a>İsteği doğrulama

Öneri: kullanıcı girişini doğrulamak ve kullanıcıların çıkış kodlayın.

İstek doğrulamanın, her isteği inceler ve algılanan bir tehdit bulunursa istek durduran ASP.NET özelliğidir. Uygulamanızı siteler arası betik saldırılara karşı güvenli hale getirmek için istek doğrulamayı bağlı değil. Bunun yerine, kullanıcılardan gelen tüm girişini doğrulamak ve çıkış kodlayın. Sınırlı bazı durumlarda, giriş doğrulamak için normal ifadeleri kullanabilirsiniz, ancak daha karmaşık durumlarda, doğrulamalıdır değerle eşleşiyorsa belirleyen .NET sınıflarını kullanarak kullanıcı girişi izin verilen değerler.

Aşağıdaki örnek, statik bir yöntem URI sınıfında bir kullanıcı tarafından sağlanan URI geçerli olup olmadığını belirlemek için nasıl kullanılacağını gösterir.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Ancak, URI yeterince doğrulamak için Ayrıca, belirttiğinden emin olun için denetleme yapmalıdır `http` veya `https`. Aşağıdaki örnek, URI geçerli olduğunu doğrulamak için örnek yöntemler kullanır.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Kötü amaçlı kod dahil edilmez emin olmak için değerleri HTML olarak kullanıcı girişini işleme veya bir SQL sorgusunun kullanıcı girişi dahil olmak üzere önce kodlayın.

HTML için biçimlendirme değer kodlama &lt;%: %&gt; söz dizimi, aşağıda gösterildiği gibi.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Veya, HTML, Razor sözdizimini ile kodlama @, aşağıda gösterildiği gibi.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

Sonraki örnekte gösterildiği nasıl, arka plan kod içinde bir değer için HTML kodlayın.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Güvenli bir şekilde SQL komutları için bir değer kodlamak için komut parametreleri gibi kullanın [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Cookieless form kimlik doğrulaması ve oturum

Öneri: tanımlama bilgileri gerektirir.

Sorgu dizesinde kimlik doğrulama bilgilerini bu işleve geçirerek güvenli değildir. Uygulamanız kimlik doğrulaması içerdiğinde, bu nedenle, tanımlama bilgileri gerektirir. Tanımlama bilgisi duyarlı bilgi içermiyorsa, tanımlama bilgisi için SSL kullanılmasını gerekli tutmayı dikkate alın.

Aşağıdaki örnek, form kimlik doğrulaması, SSL üzerinden iletilen bir tanımlama bilgisi gerektiren Web.config dosyasında belirlemek gösterilmektedir.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Öneri: Hiçbir zaman false olarak ayarlanmış.

Varsayılan olarak, EnbableViewStateMac ayarlanır true. Uygulama görünüm durumunu kullanmıyor olsa bile, EnableViewStateMac false olarak ayarlı değil. Bu değerin false olarak ayarlanması, uygulamanızı siteler arası betik karşı savunmasız hale getirir.

ASP.NET 4.5.2 ile başlayarak, çalışma zamanı zorlar **EnableViewStateMac = true**. False olarak ayarlanmış olsa bile, çalışma zamanı bu değeri yoksayar ve true olarak değer kümesi ile devam eder. Daha fazla bilgi için [ASP.NET 4.5.2 ve EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Aşağıdaki örnek, EnableViewStateMac true olarak ayarlamak gösterilmektedir. Aslında bu değer varsayılan olarak true olduğundan true olarak ayarlamak gerekmez. Ancak, bunu false olarak herhangi bir sayfa üzerinde uygulamanızda ayarladıysanız, bu değer hemen düzeltmeniz gerekir.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Orta güven

Öneri: Orta güven (veya herhangi bir güven düzeyi) bir güvenlik sınırı olarak güvenmeyin.

Kısmi güven yeterince uygulamanızı korumaz ve kullanılmamalıdır. Bunun yerine tam güven ve ayrı uygulama havuzlarında güvenilmeyen uygulamaları ayırmak. Ayrıca, her bir uygulama havuzunun benzersiz bir kimlik altında çalıştırın. Daha fazla bilgi için [ASP.NET kısmi güven uygulama yalıtımı garantilemez](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Öneri: güvenlik ayarlarını devre dışı bırakmayın &lt;appSettings&gt; öğesi.

AppSettings öğesi, güvenlik güncelleştirmeleri için gerekli olan birçok değerlerini içerir. Değil, değiştirmek veya bu değerleri devre dışı bırakmak gerekir. Bir güncelleştirme dağıtımı sırasında bu değerleri devre dışı bırakmanız gerekir, hemen dağıtımını tamamladıktan sonra yeniden etkinleştirin.

Ayrıntılar için bkz [ASP.NET appSettings öğesi](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Öneri: Kullanın [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) yerine.

UrlPathEncode yöntemi, bir çok özel tarayıcı uyumluluk sorunu çözmek için .NET Framework eklendi. Bir URL yeterince kodlamaz ve uygulamanızı siteler arası komut dosyası oluşturmaya karşı koruma sağlamaz. Bunu uygulamanızda hiçbir zaman kullanmalısınız. Bunun yerine, [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

Aşağıdaki örnek, bir hyperlink denetimi için bir sorgu dizesi parametresi olarak kodlanmış bir URL geçirilecek gösterilmektedir.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Güvenilirlik ve performans

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders ve PreSendRequestContent

Öneri: Bu olayları ile yönetilen modülleri kullanmayın. Bunun yerine, gerekli görev gerçekleştirmek için yerel bir IIS modül yazın. Bkz: [yerel kodlu HTTP modülleri oluşturma](https://msdn.microsoft.com/library/ms693629.aspx).

Kullanabileceğiniz [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) ve [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) yerel IIS modülleri ile olayları.
> [!WARNING]
> Kullanmayın `PreSendRequestHeaders` ve `PreSendRequestContent` uygulayan yönetilen modülleri ile `IHttpModule`. Bu özellikleri ayarlama ile zaman uyumsuz istekler sorunlara neden olabilir. Uygulama istenen yönlendirme (ARR) ve websockets birleşimi w3wp kilitlenmesine neden olabilecek erişim ihlali özel durumlara neden olabilir. Örneğin, iiscore! Bir erişim ihlali özel durumu (0xC0000005) W3_CONTEXT_BASE::GetIsLastNotification + iiscore.dll, 68 oldu.

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Web Forms ile zaman uyumsuz sayfası olayları

Öneri: Sayfa yaşam döngüsü olayları için void metotları zaman uyumsuz yazma Web formlarında önlemek ve bunun yerine kullanın [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) zaman uyumsuz kod için.

Bir sayfa olay ile işaretlediğinizde **zaman uyumsuz** ve **void**, zaman uyumsuz kodun bittiği olmadığını belirleyemez. Bunun yerine, Page.RegisterAsyncTask tamamlanmasını izlemek sağlayan bir şekilde zaman uyumsuz kodu çalıştırmak için kullanın.

Aşağıdaki örnekte gösterildiği bir düğme içeren zaman uyumsuz kod işleyicisi'a tıklayın. Bu örnek bir dize değeri zaman uyumsuz olarak okuma önerilen uygulama değil de, yalnızca bir Basitleştirilmiş zaman uyumsuz bir görev örneği olarak sağlanan içerir.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Zaman uyumsuz görevler kullanıyorsanız, Http Çalışma zamanı hedef Framework'ü Web.config dosyasında 4.5 olarak ayarlayın. Hedef framework 4.5 kapatır için yeni eşitleme kapsamının üzerinde ayarı .NET 4.5 eklendi. Bu değer yeni projeler Visual Studio 2012'de varsayılan olarak ayarlanmış, ancak mevcut bir projeyi ile çalışıyorsanız ayarlanmış olması değil.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>İş Başlat ve unut

Öneri:, (böyle ThreadPool.QueueUserWorkItem yöntemi çağrılırken veya sürekli bir temsilci çağıran bir zamanlayıcı oluşturma) iş Başlat ve unut başlatma ASP.NET içinde bir isteği işlerken kaçının.

Uygulamanızın içinde ASP.NET çalışan Başlat ve unut iş varsa, uygulamanızı eşitlenmemiş alabilirsiniz. Herhangi bir zamanda uygulamanın geçerli durumu, devam eden işlemini artık eşleşmiyor olabilir yani uygulama etki alanı edilebilir.

Bu tür iş ASP.NET dışında taşımanız gerekir. Web işleri, Windows hizmeti veya çalışan rolü, Azure'da devam eden çalışmayı gerçekleştirmek için kullanın ve kodun başka bir işlemden çalıştırın.

ASP.NET içine bu iş, gerçekleştirmeniz gerekirse adlı Nuget paketi ekleyebilirsiniz [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) kodu çalıştırmak için.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>İstek Varlık gövdesi

Öneri: olay işleyicinin yürütmeden önce Request.Form veya Request.InputStream okuma kaçının.

Erken Request.Form veya Request.InputStream okumalıdır olan işleyicinin sırasında yürütme olay. Mvc'de denetleyicisi işleyici ve eylem yöntemi çalıştığında yürütme etkinliğidir. Web Forms, işleyici sayfasıdır ve Page.Init olay oluşturulduğunda yürütme etkinliğidir. Yürütme Olay'den önceki istek Varlık gövdesi okuma isteğin işlenmesi müdahale.

Yürütme olayından önce istek Varlık gövdesi okuma gerekiyorsa kullanın [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) veya [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). GetBufferlessInputStream kullandığınızda, ham akışı istekten alma ve tüm istek işlemeye sorumluluğunu. Bunlar ASP.NET tarafından doldurulduğunu değil çünkü GetBufferlessInputStream çağrıldıktan sonra Request.Form ve Request.InputStream kullanılabilir değil. GetBufferedInputStream kullandığınızda, istekten akışın bir kopyasını alın. ASP.NET dolduran başka bir kopyası olduğundan Request.Form ve Request.InputStream isteği daha sonra yine kullanılabilir durumdadır.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect ve Response.End

Öneri: iş parçacığı çağırdıktan sonra nasıl işlendiğini fark dikkat [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) yöntemi Response.End yöntemini çağırır. Bir zaman uyumlu işlemde Request.Redirect çağırma hemen iptal etmek geçerli iş parçacığının neden olur. İstek için kod yürütme devam eder ancak zaman uyumsuz bir işlemde Response.Redirect çağırma geçerli iş parçacığını iptal değil. Zaman uyumsuz bir işlemde görevi kod yürütmeyi durdurmak için yöntemden döndürmesi gerekir.

Bir MVC projesinde, Response.Redirect çağırmamanız gerekir. Bunun yerine, bir RedirectResult döndürür.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState ve ViewStateMode

Öneri: Görünüm durumu üzerinde denetimleri kullanma ayrıntılı bir denetim sağlamak için EnableViewState yerine kullanım ViewStateMode.

Yanlış sayfa yönergesinde EnableViewState ayarladığınızda, Görünüm durumu sayfa içindeki tüm denetimler için devre dışıdır ve etkinleştirilemez. Görünüm durumu sayfanızın belirli denetimler için etkinleştirmek istiyorsanız, ViewStateMode sayfa için devre dışı olarak ayarlanabilir.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Ardından, ViewStateMode görünüm durumu gerektiren denetimlere etkin olarak ayarlayın.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Yalnızca gerekli denetimleri için görünüm durumuna etkinleştirerek, web sayfaları için Görünüm durumu boyutunu küçültebilirsiniz.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Öneri: Evrensel sağlayıcıları kullanın.

Geçerli proje şablonlarında SqlMembershipProvider ile değiştirilmiştir [ASP.NET Evrensel sağlayıcıları](http://www.nuget.org/packages/Microsoft.AspNet.Providers), olduğu bir NuGet paketi olarak kullanılabilir. SqlMembershipProvider şablonları önceki bir sürümüyle oluşturulmuş bir projede kullanıyorsanız, Evrensel sağlayıcıları geçer. Evrensel sağlayıcıları, Entity Framework tarafından desteklenen tüm veritabanları ile çalışır.

Daha fazla bilgi için [Karşınızda ASP.NET Evrensel sağlayıcıları](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Uzun süren istekleri (> 110 saniye)

Öneri: Kullanın [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) veya [SignalR](../../../signalr/index.md) bağlı istemcileri ve zaman uyumsuz g/ç işlemleri kullanın.

Uzun süreli istekler web uygulamanızda öngörülemeyen sonuçlara ve performansın düşmesine neden olabilir. Bir istek için varsayılan zaman aşımı ayarını 110 saniyedir. ASP.NET oturum durumu uzun süre çalışan istekle kullanıyorsanız, 110 saniye sonra oturum nesnesi üzerindeki kilidi serbest bırakır. Ancak, uygulamanız oturum nesnesinde bir işlem ortasında kilidi serbest bırakılır ve işlem başarıyla tamamlanmayabilir olabilir. İlk istek çalışırken kullanıcı ikinci bir isteği engellenirse, ikinci isteği tutarsız bir duruma oturum nesnesinde erişebilir.

Uygulamanızın g/ç işlemleri engelleme (veya zaman uyumlu) içeriyorsa, uygulamanın yanıt veremez olacaktır.

Performansı artırmak için .NET Framework zaman uyumsuz g/ç işlemleri kullanın. Ayrıca, WebSockets veya SignalR sunucuya bağlanan istemciler için kullanın. Bu özellikler, uzun süre çalışan istekleri verimli bir şekilde işlemek üzere tasarlanmıştır.
