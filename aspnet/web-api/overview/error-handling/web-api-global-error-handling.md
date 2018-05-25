---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Genel hata ASP.NET Web API 2 işleme | Microsoft Docs
author: davidmatson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: c593c56ba3d0ee8ebf6dc425408d2c3b91c83f93
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Genel hata ASP.NET Web API 2 işleme
====================
tarafından [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Bugün Web API'si oturumu veya genel hataları işlemek için kolay bir yolu yoktur. Bazı işlenmeyen özel durumlar aracılığıyla işlenen [özel durum filtreleri](exception-handling.md), ancak özel durum filtreleri işleyemiyor durumlarının sayısını. Örneğin:

1. Denetleyici oluşturucular oluşturulan özel durumları.
2. İleti işleyicileri oluşturulan özel durumları.
3. Yönlendirme sırasında oluşturulan özel durumları.
4. Yanıtın içerik serileştirilmesi sırasında oluşturulan özel durumları.

Oturum ve (mümkün olduğunda) işlemek için basit ve tutarlı bir yol sağlamak üzere istiyoruz bu özel durumları. 

Özel durumları, burada size bir hata yanıtı gönderebilir ve burada tüm yapabiliriz günlük özel durumdur çalışması için iki ana durumlar vardır. İkinci durum için örnek yanıt içeriği akış ortasında bir özel durum olduğunda; Bu durumda, çok geç biz yalnızca bağlantıyı durdurma şekilde durum kodu, üstbilgi ve kısmi içerik zaten kablo ilerlemiş bu yana yeni bir yanıt iletisi göndermektir. Yeni bir yanıt iletisi oluşturmak için özel durum işlenemez olsa bile, özel durum günlüğe kaydetme hala destekler. Bir hata algılandığı durumlarda, aşağıda gösterildiği gibi size uygun hata yanıtı döndürebilirsiniz:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Var olan seçenekler

Ek olarak [özel durum filtreleri](exception-handling.md), [ileti işleyicileri](../advanced/http-message-handlers.md) bugün tüm 500 düzeyi yanıtları izlemek için kullanılabilir, ancak özgün hata hakkında bağlam bulunmadığından bu yanıtları hareket zor. İleti işleyicileri ayrıca bazı ele alabilir durumlarda ilgili özel durum filtreleri onunla aynı sınırlamalara sahiptir. Web API hata koşulları yakalar izleme altyapısına varken izleme altyapısına tanılama amaçlıdır ve değil tasarlanmış veya üretim ortamlarında çalıştırmak için uygun. Genel özel durum işleme ve günlüğe kaydetme, üretim sırasında çalıştırabilir ve var olan izleme çözümleriyle takılı Hizmetleri olmalıdır (örneğin, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Çözüme genel bakış

 İki yeni kullanıcı değiştirilebilen Hizmetleri sağladığımız [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) ve oturum ve işlenmeyen özel durumları işleme IExceptionHandler. Hizmetleri iki temel farklar ile çok benzer:

1. Birden çok özel durum günlükçüleri ancak yalnızca bir tek özel durum işleyici kaydetme destekler.
2. Yaklaşık bağlantıyı durdurma ki olsa bile özel durum günlükçüleri her zaman çağrılmadığı. Biz yine de göndermek için hangi yanıt iletiyi seçerek tüketimi özel durum işleyicileri yalnızca çağrılır.

Her iki hizmet erişebilmesi burada özel durum algılandı, noktasından ilgili bilgileri içeren bir özel durum bağlamı için özellikle [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), özel durum ve özel durum kaynak (Ayrıntılar aşağıda) oluşturulur.

### <a name="design-principles"></a>Tasarım ilkeleri

1. **Önemli değişiklikler** bu işlev bir alt yayın, bir çözüm etkileyen önemli kısıtlaması yok önemli değişiklikler, sözleşmeleri yazın ya da var olan veya davranışı eklendiğinden. Bu kısıtlamanın özel durumlar 500 yanıtları içine kapatma varolan catch bloklarını bakımından yaptığınızdan isteriz bazı temizleme çıkışı çizgili. Bu ek Temizleme için bir sonraki büyük yayın dikkate alabilirsiniz şeydir. Bu önemli değilse, lütfen adresinden oylamak [ASP.NET Web API kullanıcı sesi](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Web API ile tutarlılık koruma yapıları** Web API'nin filtre ardışık düzen olan bir özel eylem, denetleyici özel veya genel kapsamda mantığı uygulama esnekliğiyle arası kesme sorunları işlemek için kullanışlı bir yoludur. Özel durum filtreleri de dahil olmak üzere filtreleri, eylem ve denetleyici bağlamı, bile genel kapsamda kaydolurken her zaman vardır. Sözleşme filtreleri için anlamlı, ancak özel durum filtreleri, hatta genel kapsamlı olanları bazı özel durum iletisi işleyicileri özel durumlar gibi durumlarda hiçbir eylem veya denetleyici bağlamı nerede işleme için iyi bir tercihtir olmayan anlamına gelir bulunmaktadır. Şu özel durum işleme için filtreler tarafından karşılanan esnek kapsamı kullanmak istiyorsanız, biz yine özel durum filtreleri gerekir. Ancak bir denetleyici bağlamı dışında özel durumu işlemek için ihtiyacımız olmadığını biz de ayrı bir yapı (bir şey denetleyici bağlamını ve eylem bağlamı kısıtlamaları olmadan) tam genel hata işleme için gerekir.

### <a name="when-to-use"></a>Ne zaman kullanılacağı

- Özel durum günlükçüleri Web API'si tarafından yakalanan tüm işlenmeyen özel durum görmek için çözüm sağlar.
- Özel durum işleyicileri Web API'si tarafından yakalanan işlenmeyen özel durumlar için tüm olası yanıtları özelleştirme çözümdür.
- Özel durum filtreleri, özel bir eylem veya denetleyici için ilgili alt işlenmeyen özel durumları işlemek için kolay bir çözümdür.

### <a name="service-details"></a>Hizmet Ayrıntıları

 Özel durum günlükçüsü ve işleyici Hizmet Arabirimleri ilgili bağlamları alma basit zaman uyumsuz yöntemler şunlardır: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Biz de temel sınıflar hem de bu arabirimleri sağlar. Çekirdek (eşitleme veya zaman uyumsuz) yöntemleri geçersiz kılarak, olduğundan oturum veya önerilen işlemek için gereken tüm kez. Günlük kaydı için `ExceptionLogger` temel sınıf, çekirdek günlüğü yöntemi yalnızca bir kez her özel durum için çağrıldığından emin olmadığını sağlamak (daha sonra yayar olsa bile daha fazla yukarı çağrı yığını ve yeniden yakalandı). `ExceptionHandler` Temel sınıf yöntemi yalnızca iç içe geçmiş eski yoksayılıyor çağrı yığını üstündeki özel durumlar catch blokları için işleme çekirdek çağıracaktır. (Basitleştirilmiş temel sınıflar ekte sürümleridir.) Her ikisi de `IExceptionLogger` ve `IExceptionHandler` özel durum hakkında bilgi alma bir `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Framework bir özel durum günlükçüsü veya bir özel durum işleyici aradığında, her zaman sağlayacak bir `Exception` ve `Request`. Birim testi dışında ayrıca her zaman sağlayacak bir `RequestContext`. Nadiren sağlayacak bir `ControllerContext` ve `ActionContext` (yalnızca catch bloğu için özel durum filtreleri çağrılırken). Nadiren sağlayacak bir `Response`(yanıt yapılandırılmaya çalışılırken zaman ortasında yalnızca belirli durumlarda IIS). Bu özelliklerden bazıları olabileceğinden unutmayın `null` denetlemek için tüketici kadar olan `null` özel durum sınıfı üyeleri erişmeden önce.`CatchBlock` bir dize, özel durum hangi catch bloğu gördüğünüz belirten. Catch bloğu dizeleri aşağıdaki gibidir:

- HttpServer (SendAsync yöntemi)
- HttpControllerDispatcher (SendAsync yöntemi)
- HttpBatchHandler (SendAsync yöntemi)
- IExceptionFilter (ExecuteAsync özel durum filtre ardışık işlenmesini ApiController'ın)
- OWIN ana bilgisayarı:

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (for buffering output)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (for streaming output)
- Web ana bilgisayarı:

    - HttpControllerHandler.WriteBufferedResponseContentAsync (for buffering output)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (for streaming output)
    - HttpControllerHandler.WriteErrorResponseContentAsync (for failures in error recovery under buffered output mode)

Catch bloğu dizelerin listesi da statik salt okunur özellikler mevcuttur. (Çekirdek catch bloğu dize üzerinde statik ExceptionCatchBlocks; bir statik sınıf her OWIN ve web ana bilgisayar için kalan görünür).`IsTopLevelCatchBlock` çağrı yığını üstünde yalnızca özel durumları işleme önerilen desenini izlemek için yararlıdır. Özel durumlar 500 yanıtları bir iç içe geçmiş catch bloğu oluşur her yerden içine kapatma yerine bir özel durum işleyicisi özel durumlar hakkında ana bilgisayar tarafından görülebilir oldukları kadar yayılmasına izin verebilirsiniz.

Ek olarak `ExceptionContext`, tek daha fazla parça bilgi tam aracılığıyla günlükçüsü `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

İkinci özelliği `CanBeHandled`, işlenmeyen bir özel durum tanımlamak bir Günlükçü sağlar. Ne zaman hakkında durdurulacak bağlantı olduğu ve yeni bir yanıt iletisi gönderilebilir, günlükçüleri adlı ancak işleyici olacak ***değil*** çağrılabilir kullan, ve bu özellik bu senaryodan günlükçüleri tanımlayabilirsiniz.

İçinde ek `ExceptionContext`, bir daha fazla özelliği tam üzerinde ayarlanmış bir işleyici alır `ExceptionHandlerContext` özel durumu işlemek için:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Bir özel durum işleyici, ayarlayarak bir özel durum işlediği gösterir `Result` eylem sonucunu özelliğine (örneğin, bir [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), ya da özel bir sonuç). Varsa `Result` özelliği null, işlenmemiş bir işlemdir ve özgün özel durum yeniden oluşturulur.

Çağrı yığını üstündeki özel durumlar için yanıt API çağıranlar için uygun olduğundan emin olmak için fazladan bir adım sürdü. Özel durum ana kadar yayılırsa, başka bir ana bilgisayarı, genellikle HTML olan yanıt ve genellikle bir uygun API hata yanıtı sağlanan veya çağıran sarı renkli kilitlenme ekranı görürsünüz. Bu durumlarda, null olmayan ve bir özel durum işleyici formu açıkça ayarlarsa yalnızca sonuç başlatır dön `null` (işlenmemiş) özel durum ana bilgisayara yayılır. Ayarı `Result` için `null` bu gibi durumlarda iki senaryo için yararlı olabilir:

1. OWIN Web API Web API önce/dışında kayıtlı ara işleme özel durumla barındırılan.
2. Yerel bir tarayıcı yoluyla hata ayıklama, burada sarı renkli kilitlenme gerçekte işlenmeyen bir özel durum için yararlı bir yanıt ekrandır.

Özel durum günlükçüleri ve özel durum işleyicileri için Günlükçü veya işleyici kendisini bir özel durum oluşturursa kurtarmak için bir şey yapmanız yok. (Yayar, bu sayfanın sonundaki geri bildirim bırakın özel durum dışında daha iyi bir yaklaşım vardır.) Özel durum günlükçüleri ve işleyicileri için bunlar kendi arayanlar kadar yayılması özel durumlar izin vermeniz değil sözleşmedir; Aksi takdirde, özel durum yalnızca, genellikle tüm (ör. ASP HTML hata kaynaklanan ana bilgisayara yayılır NET'in sarı ekran) (genellikle JSON veya XML beklediğiniz API çağıranlar için tercih edilen seçeneği değil) geri İstemcinin gönderileceği.

## <a name="examples"></a>Örnekler

### <a name="tracing-exception-logger"></a>Özel durum günlükçüsü izleme

Aşağıdaki özel durum günlükçüsü yapılandırılmış izleme kaynakları (Visual Studio'da hata ayıklama çıktı penceresi dahil) için özel durum verileri gönderin.

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Özel hata iletisi özel durum işleyicisi

Şu değerin altında bir özel hata yanıtı destek için bir e-posta adresi de dahil olmak üzere istemcilere üretir.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Özel durum filtreleri kaydetme

Projenizi oluşturmak için "ASP.NET MVC 4 Web uygulaması" proje şablonu kullanırsanız, Web API yapılandırması kodunuzu içinde put `WebApiConfig` sınıfı, buna *uygulama/_sihirbaz Başlat* klasörü:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Ek: Taban sınıf ayrıntıları

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
