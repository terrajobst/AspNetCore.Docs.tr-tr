---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: ASP.NET Web API 2 genel hata işleme | Microsoft Docs
author: davidmatson
description: ''
ms.author: aspnetcontent
ms.date: 02/03/2014
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: d1d1a081713da0771981229db8e8bd67a5103bf9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815873"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>ASP.NET Web API 2 genel hata işleme
====================
tarafından [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Bugün oturum veya genel olarak, hataları işlemek için Web API'SİNDE kolay bir yolu yoktur. Bazı işlenmeyen özel durumlar aracılığıyla işlenen [özel durum filtreleri](exception-handling.md), vardır, ancak bir özel durum filtreleri işleyemiyor çalışması sayısı. Örneğin:

1. Denetleyici oluşturucular oluşturulan bir özel durumlar.
2. İleti işleyicilerini oluşturulan bir özel durumlar.
3. Yönlendirme sırasında oluşturulan özel durumlar.
4. Yanıt içeriği seri hale getirme sırasında oluşturulan bir özel durumlar.

Oturum ve (mümkünse) işlemek için basit ve tutarlı bir yol sağlamak üzere istiyoruz bu özel durumlar. 

Özel durumları, burada bir hata yanıtı gönderebilmesi için duyuyoruz ve burada tüm yapabiliriz çalışması günlük özel durum işleme için iki ana durumlar vardır. İkinci durumda örneğin yanıt içeriği akış ortasında bir özel durum olduğunda; Bu durumda bu çok geç biz yalnızca bağlantı durdurmak için durum kodu, üst bilgiler ve kısmi içerik zaten kablo sahiplikten bu yana yeni bir yanıt iletisi göndermektir. Özel durum, yeni bir yanıt iletisi oluşturmak üzere işlenemez olsa da, özel durum günlüğe kaydetme yine de destekliyoruz. Burada bir hata saptanabilir durumlarda, aşağıda gösterildiği gibi biz bir uygun hata yanıtı döndürebilir:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Mevcut seçenekleri

Ek olarak [özel durum filtreleri](exception-handling.md), [ileti işleyicileri](../advanced/http-message-handlers.md) tüm 500 düzeyi yanıtları gözlemlemek için hemen kullanılabilir ancak bunlar bu orjinal hatayı hakkında bağlam bulunmadığından bu yanıtları almalarını zor. İleti işleyicileri bazı başa çıkabilir durumları ile ilgili özel durum filtreleri onunla aynı sınırlamalara sahiptir. Web API hata koşulları yakalar izleme altyapısına çalışırken izleme altyapısına tanılama amaçları içindir ve değil tasarlanmış veya üretim ortamlarında çalışan için uygun. Genel özel durum işleme ve günlüğe kaydetme, üretim sırasında çalıştırabilir ve var olan izleme çözümleriyle takılı Hizmetleri olmalıdır (örneğin, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Çözüme genel bakış

 Kullanıcı tarafından değiştirilebilen bir iki yeni hizmet, sağladığımız [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) ve IExceptionHandler, oturum ve işlenmeyen özel durumları işlemek için. Hizmetleri iki temel farklar ile çok benzer:

1. Birden çok özel durum günlükçüleri ancak yalnızca bir tek özel durum işleyicisi kaydediliyor destekliyoruz.
2. İlgili bağlantıyı durdurma duyuyoruz bile özel durum günlükçüleri her zaman çağrılmadığı. Size hangi yanıt iletisi göndermek için yine de seçebilir, özel durum işleyicileri yalnızca çağrılmadığı.

Her iki hizmet noktasından burada özel durum algılandı, ilgili bilgileri içeren bir özel durum bağlamı erişmeyi özellikle [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), özel durum ve özel durum kaynağı (Ayrıntılar aşağıda) oluşturulur.

### <a name="design-principles"></a>Tasarım ilkeleri

1. **Bozucu değişiklik** bu işlev bir alt yayın, bir çözüm etkileyen önemli kısıtlaması bozucu değişiklik, ya da sözleşme türü olması veya davranış eklendiğinden. Bu kısıtlama, 500 yanıt özel durumları kapatma mevcut catch blokları açısından yaptıktan istiyoruz biraz temizlik kullanıma çizgili. Bu ek temizleme biz için bir sonraki ana sürümüne ait düşünebilirsiniz şeydir. Bu önemli ise, lütfen adresinden oylamak [ASP.NET Web API uservoice](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Web API'si ile tutarlılığı sürdürmeye yapıları** Web API'SİNİN filtre ardışık düzen, uygulama mantığı bir özel eylem, denetleyici özel veya genel kapsamda esnekliğiyle geniş kapsamlı kritik konular işlemek için harika bir yoludur. Özel durum filtreleri de dahil olmak üzere filtreleri, eylem ve denetleyici bağlamı, genel kapsamda kaydettiğinizde bile her zaman vardır. Sözleşme filtreleri için anlamlıdır, ancak özel durum filtreleri, hatta kapsamı olanları bazı özel durumlarda, ileti işleyicileri, özel durumlar gibi hiçbir eylem veya denetleyici bağlamı nerede işleme uygun olmayan geldiğini var. Biz yine de biz özel durum işleme için filtreler tarafından gösterilen esnek kapsamı kullanmak istiyorsanız, özel durum filtreleri gerekir. Ancak bir denetleyici bağlamı dışında özel durumu işlemek ihtiyacımız olursa da ayrı bir yapısı (bir şey denetleyici bağlamını ve eylem bağlamını kısıtlamaları olmadan) tam genel hata işleme için gerekiyor.

### <a name="when-to-use"></a>Ne zaman kullanılır?

- Özel durum günlükçüleri, tüm işlenmemiş özel durum yakalandı Web API'si tarafından görmeye çözümdür.
- Özel durum işleyicileri, Web API'si tarafından yakalanan işlenmemiş özel durumların tüm olası yanıtlarını özelleştirmek için bir çözüm sağlar.
- Özel durum filtreleri, belirli bir eylem veya Denetleyici ile ilişkili alt işlenmeyen özel durumları işleme için en kolay çözümdür.

### <a name="service-details"></a>Hizmet Ayrıntıları

 Özel durum günlükçüsü ve işleyici Hizmet Arabirimleri, ilgili içerikler alma basit bir zaman uyumsuz yöntemler şunlardır: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Ayrıca temel sınıflar hem de bu arabirimler sunuyoruz. Çekirdek (eşitleme veya zaman uyumsuz) yöntemleri geçersiz kılarak, olduğundan oturum veya önerilen işlemek için gereken tüm saatler. Günlük kaydı için `ExceptionLogger` temel sınıf çekirdek günlük yöntemi yalnızca bir kez için her bir özel durum çağrıldığını sağlayabilirsiniz (daha sonra yayar bile daha fazla çağrı yığınında yukarı ve yeniden yakalanan). `ExceptionHandler` Temel sınıf yöntemi yalnızca özel durumlar iç içe geçmiş eski yoksayılıyor çağrı yığını üst kısmındaki catch blokları için işleme çekirdek çağıracaktır. (Bu temel sınıfların Basitleştirilmiş sürümleri ekte içindir.) Her ikisi de `IExceptionLogger` ve `IExceptionHandler` özel durum hakkında bilgi alabileceği bir `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Framework, bir özel durum günlükçüsü veya bir özel durum işleyicisi çağırdığında, her zaman sağlayacak bir `Exception` ve `Request`. Birim testi dışında bu da her zaman sağlayacak bir `RequestContext`. Nadiren sağlayacak bir `ControllerContext` ve `ActionContext` (yalnızca catch bloğundan için özel durum filtreleri çağırırken). Nadiren sağlayacak bir `Response`(bazı özel durumlarda yanıt yapılandırılmaya çalışılırken zaman ortasında yalnızca IIS). Bu özelliklerin bazıları olabileceğinden unutmayın `null` denetlemek için tüketici kadar olan `null` özel durum sınıfı üyeleri erişmeden önce.`CatchBlock` hangi yakalama bloğu özel durumu gördüğünüz gösteren bir dize. Catch bloğu dizeleri aşağıdaki gibidir:

- HttpServer (SendAsync yöntemi)
- HttpControllerDispatcher (SendAsync yöntemi)
- HttpBatchHandler (SendAsync yöntemi)
- IExceptionFilter (özel durum filtre ardışık ExecuteAsync işlenmesini ApiController'ın)
- OWIN ana bilgisayarı:

    - (Çıkış arabelleği için) HttpMessageHandlerAdapter.BufferResponseContentAsync
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (için çıktı akışı)
- Web ana bilgisayarı:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (for buffering output)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (için çıktı akışı)
    - HttpControllerHandler.WriteErrorResponseContentAsync (için arabelleğe alınan çıkış modu altında hata kurtarma hata)

Catch bloğu dize listesi statik salt okunur özellikler de kullanılabilir. (Çekirdek catch bloğu dize üzerinde statik ExceptionCatchBlocks; bir statik sınıftaki her OWIN ve web ana bilgisayar için kalan görünür).`IsTopLevelCatchBlock` yalnızca en üst çağrı yığınının özel durumları işlemenin önerilen Düzen izlemek için yararlıdır. 500 yanıt herhangi bir iç içe geçmiş catch bloğu gerçekleşir özel durumları açma yerine bir özel durum işleyicisi özel durumlar hakkında ana bilgisayar tarafından görülebilmesi için bunlar kabul edilene kadar yaymak izin verebilirsiniz.

Ek olarak `ExceptionContext`, tam bilgisini bir daha fazla parça günlükçüsü `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

İkinci özelliği `CanBeHandled`, işlenmeyen bir özel durum tanımlamak bir Günlükçü sağlar. Ne zaman durdurulmak bağlantı olduğu ve yeni yanıt iletisi gönderilebilir, günlükçüler çağrılır ancak işleyici olur ***değil*** çağrıldığında, ve bu özellik bu senaryoda günlükçüler tanımlayabilirsiniz.

İçinde ek `ExceptionContext`, daha fazla özelliği tam üzerinde ayarlanmış bir işleyici alır `ExceptionHandlerContext` özel durumu işlemek için:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Özel durum işleyicisi, ayarlayarak bir özel durum işlediği gösterir `Result` eylem sonucunu özelliğini (örneğin, bir [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), ya da özel bir sonuç). Varsa `Result` özelliği null, özel durum işlenmemiş ve özgün özel durum yeniden oluşturulur.

Çağrı yığınının üst özel durumlar için yanıt API arayanlar için uygun olduğundan emin olmak için fazladan bir adım attık. Özel durum barındırabilirsiniz yayılırsa çağıran Ölüm sarı ekran görür veya başka bir konak HTML genellikle olan yanıt ve genellikle uygun bir API hata yanıtı sağlanan. Bu gibi durumlarda sonuç başlatır null olmayan ve yalnızca bir özel durum işleyicisi açıkça ayarlar tekrar `null` (işlenmemiş) özel durum ana bilgisayara yayılır. Ayarı `Result` için `null` bu gibi durumlarda, iki senaryo için yararlı olabilir:

1. OWIN ara yazılımını Web API önce/dışında kayıtlı işleme özel durum ile Web API barındırılan.
2. Yerel bir tarayıcı üzerinden hata ayıklama, burada sarı ölüm gerçekten işlenmeyen bir özel durum için yararlı bir yanıt ekrandır.

Özel durum günlükçüleri ve özel durum işleyicileri için biz Günlükçü veya işleyicisi bir özel durum oluşturursa, kurtarılır hiçbir şey yapmaz. (Yayar, bu sayfanın sonundaki geri tutulacaksa özel durum dışında daha iyi bir yaklaşım vardır.) Bunlar özel durumlar kadar Arayanların yayılmasına izin vermemeniz gerekir, özel durum günlükçüleri ve işleyici sözleşmesini olur; Aksi takdirde, özel durum yalnızca, genellikle tüm HTML hata (örneğin, ASP. Bunun sonucunda konağa yayılır NET sarı ekran) (Bu genellikle JSON veya XML API arayanlar için tercih edilen seçeneği değil) istemciye gönderilen.

## <a name="examples"></a>Örnekler

### <a name="tracing-exception-logger"></a>Özel durum günlükçüsü izleme

Aşağıdaki özel durum günlükçüsü (Visual Studio'da hata ayıklama çıktı penceresine dahil), yapılandırılmış izleme kaynakları için özel durum verileri gönderin.

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Özel hata iletisi özel durum işleyicisi

Aşağıdaki istemciler için destek ile iletişim kurarak bir e-posta adresi de dahil olmak üzere, bir özel hata yanıtı oluşturur.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Özel durum filtreleri kaydediliyor

Projenizi oluşturmak için "ASP.NET MVC 4 Web uygulaması" proje şablonunu kullanıyorsanız, Web API configuration kodunuzda gezebilirsiniz put `WebApiConfig` içinde sınıf *uygulama/_başlatmayı* klasörü:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Ek: Taban sınıf ayrıntıları

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
