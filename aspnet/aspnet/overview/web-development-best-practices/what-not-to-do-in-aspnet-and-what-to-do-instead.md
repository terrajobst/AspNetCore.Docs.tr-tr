---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: ASP.NET yapmanız gerekenler ve bunun yerine yapmanız gerekenler | Microsoft Docs
author: tfitzmac
description: Bu konuda kişiler ASP.NET web projeleri içinde birçok ortak hataları açıklanmaktadır. Bu commo önlemek için ne yapması gerektiğini yönelik öneriler sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 829f3a024bc15bec8b60b91193ba9bca37b78009
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034926"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>ASP.NET yapmanız gerekenler ve bunun yerine yapmanız gerekenler
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu konuda kişiler ASP.NET web projeleri içinde birçok ortak hataları açıklanmaktadır. Bu ortak hataları önlemek için ne yapması gerektiğini yönelik öneriler sağlar. Bağlı olduğu bir [sunu](http://vimeo.com/68390507) tarafından **Damian Edirneli** Norveççe geliştiriciler konferansında.


## <a name="disclaimer"></a>Sorumluluk reddi

Bu konu, uygulamanızı güvenli ve etkin olduğundan emin olmak için tam bir kılavuz tasarlanmamıştır. Hala güvenlik ve performans için bu konudaki belirtilmeyen en iyi uygulamaları izlemeniz gerekir. Yalnızca .NET sınıfları ve işlemleri ile ilgili sık karşılaşılan hatalardan kaçınmanızı nasıl olabileceğini gösteriyor.

## <a name="overview"></a>Genel Bakış

Bu konu aşağıdaki bölümleri içermektedir:

- [Standartları uyumluluğu](#standards)

    - [Denetim bağdaştırıcıları](#adapters)
    - [Stil özellikleri denetimlerinde](#styleprop)
    - [Sayfa ve denetim geri aramalar](#callback)
    - [Tarayıcı yetenek algılama](#browsercap)
- [Güvenlik](#security)

    - [İstek doğrulama](#validation)
    - [Cookieless form kimlik doğrulaması ve oturum](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Orta güven](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Güvenilirlik ve performans](#performance)

    - [PreSendRequestHeaders ve PreSendRequestContent](#presend)
    - [Web Forms ile zaman uyumsuz sayfası olayları](#asyncevents)
    - [Yangın ve unut çalışma](#fire)
    - [İstek Varlık gövdesi](#requestentity)
    - [Response.Redirect ve Response.End](#redirect)
    - [EnableViewState ve ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Uzun çalışan istekler (> 110 saniye)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Standartları uyumluluğu

<a id="adapters"></a>

### <a name="control-adapters"></a>Denetim bağdaştırıcıları

Öneri: denetim bağdaştırıcıları Uyarlamalı işleme için kullanmayı ve CSS medya sorgular ve standartlarıyla uyumlu HTML yerine kullanın.

Denetimleri bağdaştırıcıları farklı cihaz ve ortamlar için özelleştirilmiş sunu kodu oluşturmak için .NET 2.0 tanıtıldı. Şimdi, Uyarlamalı bu işleme HTML ve CSS ile gerçekleştirilebilir. Denetim bağdaştırıcıları kullanmayı ve varolan tüm bağdaştırıcıları CSS ve HTML dönüştürün.

Daha fazla bilgi için bkz: [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) ve [nasıl yapılır: Mobil sayfalara eklemek bilgisayarınızı ASP.NET Web Forms / MVC uygulaması](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Stil özellikleri denetimlerinde

Öneri: denetim biçimlendirmede stili değerleri ayarlama durdurun ve CSS stil biçimlendirme değerlerini ayarlayın.

Web sunucusu denetimleri satır içi stil özelliklerini ayarlamak için kullanılan özellikler düzinelerce içerir. Örneğin, bir denetim için metin rengini ForeColor özelliği ayarlar. CSS stil sayfaları aracılığıyla daha verimli bir şekilde bu aynı etkiyi gerçekleştirebilirsiniz. Stil sayfaları, stil değerleri merkezileştirmek ve uygulamanızı boyunca bu değerler ayarlama önlemek etkinleştirin.

Aşağıdaki örnek, bir CSS sınıfı kırmızı kümeleri metne gösterir.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

Sonraki örnek nasıl dinamik olarak CSS sınıfı uygulanacağını gösterir.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Sayfa ve denetim geri aramalar

Öneri: sayfa ve denetim geri aramalar kullanarak durdurun ve bunun yerine aşağıdakilerden herhangi birini kullanın: AJAX, UpdatePanel, MVC eylem yöntemleri, Web API veya SignalR.

ASP.NET önceki sürümlerde sayfa ve denetim geri çağırma yöntemlerini, tam bir sayfayı yenilemeyi olmadan web sayfasının parçası güncelleştirmek etkin. Kısmi Sayfa güncelleştirmelerini aracılığıyla şimdi gerçekleştirebileceğiniz [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) veya [SignalR](../../../signalr/index.md). Kolay URL'ler ile sorunlara neden olabilir çünkü geri çağırma yöntemlerini kullanarak ve yönlendirme durdurmanız gerekir. Varsayılan olarak, denetimleri geri arama yöntemleri etkinleştirmeyin, ancak bir denetim'deki bu özellik etkinleştirildiğinde, onu devre dışı bırakmanız gerekir.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Tarayıcı yetenek algılama

Öneri: statik tarayıcı yetenek algılama kullanarak durdurun ve bunun yerine dinamik özellik algılama kullanın.

ASP.NET önceki sürümlerinde desteklenen özellikler her tarayıcı için bir XML dosyasında depolanırdı. Statik bir arama yoluyla algılama özelliğini desteği en iyi yaklaşım değildir. Şimdi, dinamik olarak bir desteklenen bir tarayıcıdan özellikler gibi bir özellik algılama framework kullanarak algılayabilir [Modernizr](http://modernizr.com/). Özellik algılama yöntemi veya özelliği kullanmaya çalışıyor ve tarayıcı istenen sonucu üretilen olmadığını görmek için denetimi destek belirler. Varsayılan olarak, Web uygulaması şablonlarında Modernizr dahil edilir.

<a id="security"></a>

## <a name="security"></a>Güvenlik

<a id="validation"></a>

### <a name="request-validation"></a>İstek doğrulama

Öneri: kullanıcı girişi doğrulayın ve çıktı kullanıcılardan kodlayın.

İstek doğrulama, her isteği inceler ve algılanan tehdit bulunursa istek durdurur ASP.NET özelliğidir. Uygulamanızı siteler arası komut dosyası saldırılarına karşı güvenliğini sağlamak için istek doğrulamayı bağlı değil. Bunun yerine, kullanıcıların gelen tüm girdiyi doğrulayın ve çıktı kodlayın. Sınırlı bazı durumlarda giriş doğrulamak için normal ifadeleri kullanabilirsiniz, ancak doğrulamanız gerekir daha karmaşık durumlarda değerle eşleşiyorsa belirleyen .NET sınıfları kullanarak kullanıcı girişini izin verilen değerler.

Aşağıdaki örnek, bir statik yöntem URI sınıfında bir kullanıcı tarafından sağlanan URI geçerli olup olmadığını belirlemek için nasıl kullanılacağını gösterir.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Ancak, yeterince URI doğrulamak için ayrıca belirtir emin olmak için kontrol `http` veya `https`. Aşağıdaki örnek, URI geçerli olduğunu doğrulamak için örnek yöntemleri kullanır.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

HTML olarak kullanıcı girişini işleme veya kullanıcı girişi bir SQL sorgusuna dahil olmak üzere önce kötü amaçlı kod dahil değildir emin olmak için değerleri kodlayın.

HTML için değer ile biçimlendirmede kodlamak &lt;%: %&gt; aşağıda gösterildiği gibi sözdizimi.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Veya, Razor sözdizimini HTML ile kodlamak @, aşağıda gösterildiği gibi.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

Sonraki örnekte gösterildiği nasıl arka plan kodu değerinde HTML olarak kodlayın.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Güvenli bir şekilde SQL komutları için bir değer kodlamak için komut parametreleri gibi kullandığınız [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Cookieless form kimlik doğrulaması ve oturum

Öneri: tanımlama bilgileri gerektirir.

Kimlik doğrulama bilgilerini sorgu dizesinde geçirme güvenli değildir. Bu nedenle, uygulamanızın kimlik doğrulamasını içerdiğinde tanımlama bilgileri gerektirir. Tanımlama bilgisi gizli bilgileri depoluyorsa, tanımlama bilgisi için SSL gerektirme göz önünde bulundurun.

Aşağıdaki örnekte, form kimlik doğrulamasını SSL üzerinden aktarılan bir tanımlama bilgisi gerektirdiğini Web.config dosyasında belirtebilir gösterilmektedir.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Öneri: Hiçbir zaman false olarak ayarlanmış.

Varsayılan olarak EnbableViewStateMac ayarlanır true. Görünüm durumu, uygulamanızın kullanmıyor olsa bile EnableViewStateMac false olarak ayarlamayın. Bu değer false olarak ayarlanırsa, uygulamanızı siteler arası komut dosyası karşı savunmasız hale getirir.

ASP.NET 4.5.2 ile başlayarak, çalışma zamanı zorlar **EnableViewStateMac = true**. False olarak ayarlanmış olsa bile, çalışma zamanı bu değer yoksayar ve değer kümesiyle true olarak devam eder. Daha fazla bilgi için bkz: [ASP.NET 4.5.2 ve EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

Aşağıdaki örnek, EnableViewStateMac true olarak ayarlanması gösterilmektedir. Gerçekte bu değer varsayılan olarak true olduğundan true olarak ayarlamak gerekmez. Ancak, false olarak herhangi bir sayfasında uygulamanızda ayarlarsanız, bu değer hemen düzeltmeniz gerekir.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Orta güven

Öneri: Orta güven (veya başka bir güven düzeyinde) bir güvenlik sınırı bağlı değil.

Kısmi güven uygulamanız yeterli koruma sağlamaz ve kullanılmamalıdır. Bunun yerine, tam güven kullanın ve güvenilmeyen uygulamalarını ayrı uygulama havuzlarında yalıtma. Ayrıca, her bir uygulama havuzunun benzersiz bir kimlik altında çalıştırın. Daha fazla bilgi için bkz: [ASP.NET kısmi güven garanti etmez uygulama yalıtımı](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Öneri: güvenlik ayarlarını devre dışı bırakmayın &lt;appSettings&gt; öğesi.

AppSettings öğe güvenlik güncelleştirmeleri için gerekli olan birden fazla değer içeriyor. Değil, değiştirmek veya bu değerleri devre dışı bırakmak gerekir. Bir güncelleştirme dağıtırken bu değerleri devre dışı bırakmanız gerekir, hemen dağıtımını tamamladıktan sonra yeniden etkinleştirin.

Ayrıntılar için bkz [ASP.NET appSettings öğesi](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Öneri: Kullanmak [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) yerine.

UrlPathEncode yöntemi çok özel tarayıcı uyumluluk sorunu gidermek için .NET Framework eklendi. Yeterli bir URL kodlama değil ve uygulamanızı siteler arası komut dosyası korumaz. Hiçbir zaman uygulamanızda kullanmalısınız. Bunun yerine, kullanın [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

Aşağıdaki örnek, bir köprü denetim için bir sorgu dizesi parametresi olarak kodlanmış bir URL geçirmek gösterilmiştir.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Güvenilirlik ve performans

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders ve PreSendRequestContent

Öneri: Bu olayları ile yönetilen modüller kullanmayın. Bunun yerine, gerekli görev gerçekleştirmek için yerel bir IIS modül yazma. Bkz: [yerel kodlu HTTP modülleri oluşturma](https://msdn.microsoft.com/library/ms693629.aspx).

Kullanabileceğiniz [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) ve [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) yerel IIS modülleri olan olaylar.
> [!WARNING]
> Kullanmayın `PreSendRequestHeaders` ve `PreSendRequestContent` uygulamak yönetilen modüller ile `IHttpModule`. Bu özellikleri ayarlama ile zaman uyumsuz istekleri sorunlara neden olabilir. Uygulama istenen yönlendirme (ARR) ve websockets birleşimi w3wp çökmesine neden olabilir erişim ihlali özel durumlara neden olabilir. Örneğin, iiscore! W3_CONTEXT_BASE::GetIsLastNotification + iiscore.dll 68 bir erişim ihlali özel durumu (0xC0000005) neden.

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Web Forms ile zaman uyumsuz sayfası olayları

Sayfa yaşam döngüsü olayları için void yöntemleri zaman uyumsuz yazma Web formlarında öneri: Kaçının ve bunun yerine kullanın [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) zaman uyumsuz kodu.

Bir sayfa olayla işaretlediğinizde **zaman uyumsuz** ve **void**, zaman uyumsuz kod bittiği olmadığını belirleyemez. Bunun yerine, Page.RegisterAsyncTask zaman uyumsuz kod kendi tamamlama izlemenize olanak sağlayan bir şekilde çalıştırmak için kullanın.

Aşağıdaki örnekte gösterildiği düğmesine bir zaman uyumsuz kod içeren işleyicisi'ı tıklatın. Bu örnek bir dize değeri zaman uyumsuz olarak okumak önerilen bir uygulama olarak değil de yalnızca zaman uyumsuz bir görevi basitleştirilmiş bir örneği olarak sağlanan içerir.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Zaman uyumsuz görevleri kullanıyorsanız, Http Çalışma zamanı hedef framework 4.5 Web.config dosyasında ayarlayın. Hedef framework 4.5 kapatır yeni eşitleme bağlamda ayarı .NET 4. 5 ' eklendi. Bu değer varsayılan Visual Studio 2012'de yeni projeler olarak ayarlanmış, ancak var olan bir proje ile çalışıyorsanız ayarlanmış olmaması.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Yangın ve unut çalışma

Öneri: (Bu tür ThreadPool.QueueUserWorkItem yöntemi çağırma veya sürekli olarak bir temsilci çağıran bir zamanlayıcı oluşturma) iş yangın ve unut başlatma ASP.NET içindeki bir isteği işlerken kaçının.

Uygulamanız ASP.NET içinde çalışan yangın ve unut iş varsa, uygulamanızın eşitlenmemiş alabilirsiniz. Herhangi bir zamanda uygulama etki alanı, devam eden işlem artık uygulama geçerli durumunu eşleşmeyebilir anlamı yok edilmesi.

Bu tür iş ASP.NET dışında taşımanız gerekir. Web işleri, Windows hizmeti veya çalışan rolü, Azure'da devam eden iş gerçekleştirmek için kullanın ve başka bir işlemden bu kodu çalıştırın.

ASP.NET içine bu iş gerçekleştirmeniz gerekirse, adlı Nuget paketi ekleyebilirsiniz [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) kodu çalıştırmak için.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>İstek Varlık gövdesi

Öneri: olay işleyicinin yürütmeden önce Request.Form veya Request.InputStream okuma kaçının.

En kısa Request.Form veya Request.InputStream okumalısınız olduğu işleyicinin sırasında yürütme olay. MVC, işleyici denetleyicisidir ve eylem yöntemi çalıştığında execute etkinliğidir. Web Forms işleyici sayfasıdır ve Page.Init olay başlatıldığında execute etkinliğidir. Execute olay'den önceki istek Varlık gövdesi okuma, isteğin işlenmesi müdahale.

Execute olayından önce istek Varlık gövdesi okuma gerekiyorsa kullanın ya da [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) veya [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). GetBufferlessInputStream kullandığınızda ham akış istekten alma ve tüm istek işleme sorumluluğunu üstlenmesini. Bunlar ASP.NET tarafından doldurulduğunu değil çünkü GetBufferlessInputStream çağrıldıktan sonra Request.Form ve Request.InputStream kullanılabilir değil. GetBufferedInputStream kullandığınızda, gelen isteği akış bir kopyasını alın. ASP.NET diğer kopya doldurur çünkü Request.Form ve Request.InputStream isteği daha sonra hala kullanılabilir durumdadır.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response.Redirect ve Response.End

Öneri: iş parçacığı çağrıldıktan sonra nasıl işleneceğini farklar haberdar [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) yöntemi Response.End yöntemini çağırır. Zaman uyumlu bir işlemde hemen durdurmak geçerli iş parçacığının Request.Redirect çağırma neden olur. İstek için kod yürütme devam eder ancak, zaman uyumsuz bir işlem Response.Redirect çağırma geçerli iş parçacığının abort değil. Zaman uyumsuz bir işlem görevi kod yürütmeyi durdurmaya yönteminden döndürmelidir.

MVC projesinde Response.Redirect çağırmalısınız değil. Bunun yerine, bir RedirectResult döndürür.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState ve ViewStateMode

Öneri: Görünüm durumu üzerinde denetimleri kullanın ayrıntılı bir denetim sağlamak için EnableViewState yerine kullanım ViewStateMode.

Sayfa yönergesi false EnableViewState ayarladığınızda, Görünüm durumu sayfa dahilindeki tüm denetimler için devre dışıdır ve etkinleştirilemez. Görünüm durumu yalnızca, sayfadaki bazı denetimler için etkinleştirmek istiyorsanız, ViewStateMode sayfa için devre dışı olarak ayarlayın.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Ardından, ViewStateMode gerçekten görünüm durumu gereken denetimlere etkin olarak ayarlayın.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Yalnızca ihtiyaç denetimleri için görünüm durumuna etkinleştirerek, web sayfaları için görünüm durumuna boyutunu küçültebilirsiniz.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Öneri: Evrensel sağlayıcıları kullanır.

SqlMembershipProvider geçerli proje şablonlarını almıştır [ASP.NET Evrensel Sağlayıcılar](http://www.nuget.org/packages/Microsoft.AspNet.Providers), olduğu bir NuGet paketi olarak kullanılabilir. Şablonlar önceki bir sürümüyle oluşturulmuş bir projede SqlMembershipProvider kullanıyorsanız, Evrensel sağlayıcıları geçmelisiniz. Evrensel sağlayıcıları Entity Framework tarafından desteklenen tüm veritabanları ile çalışır.

Daha fazla bilgi için bkz: [ASP.NET Evrensel Sağlayıcılar Tanıtımı](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Uzun süre çalışan istekler (> 110 saniye)

Öneri: Kullanmak [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) veya [SignalR](../../../signalr/index.md) bağlanan istemciler ve zaman uyumsuz g/ç işlemleri kullanın.

Uzun süre çalışan istekler öngörülemeyen sonuçlara ve performansın web uygulamanızda neden olabilir. Bir istek için varsayılan zaman aşımı ayarını 110 saniyedir. ASP.NET oturum durumu uzun süre çalışan istekle kullanıyorsanız, oturum nesnesi üzerinde kilit 110 saniye sonra serbest bırakır. Ancak, uygulamanızın oturum nesnesi üzerinde bir işlemi gerçekleştiriyor kilidi serbest ve işlemi başarıyla tamamlanmayabilir olabilir. İlk istek çalışırken kullanıcıdan ikinci bir isteği engellenirse, ikinci bir istek tutarsız bir duruma Session nesnesinde erişebilir.

Uygulamanızı durdurma (veya zaman uyumlu) g/ç işlemleri içeriyorsa, uygulama yanıt vermeyen olacaktır.

Performansı artırmak için .NET Framework zaman uyumsuz g/ç işlemleri kullanın. Ayrıca, WebSockets veya SignalR sunucuya bağlanan istemciler için kullanın. Bu özellikler, uzun süredir çalışan istekleri verimli bir şekilde işlemek üzere tasarlanmıştır.
