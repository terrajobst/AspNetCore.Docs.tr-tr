---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: ASP.NET Web API 2.1 yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: cc5dc111d88cc7dae6a4a93203317fa0769d5427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 yenilikler nelerdir?
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET Web API 2. 1 yenilikler açıklanmaktadır.

- [İndir](#download)
- [Belgeler](#documentation)
- [ASP.NET Web API 2.1 yeni özellikler](#new-features)

    - [Genel hata işleme](#global-error)
    - [Yönlendirme geliştirmeleri özniteliği](#attribute-routing)
    - [Yardım sayfası geliştirmeleri](#help-page)
    - [IgnoreRoute desteği](#ignoreroute)
    - [BSON medya türü biçimlendiricisi](#bson)
    - [Zaman uyumsuz filtreleri için daha iyi destek](#async-filters)
    - [Sorgu kitaplığı biçimlendirme istemci için ayrıştırma](#query-parsing)
- [Bilinen sorunlar ve yeni değişiklikler](#known-issues)
- [Hata düzeltmeleri](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>İndir

Çalışma zamanı özellikleri NuGet galerisinde NuGet paketlerini olarak yayımlanmıştır. Tüm çalışma zamanı paketleri izleyin [anlamsal sürüm oluşturma](http://semver.org/) belirtimi. En son ASP.NET Web API 2.1 RTM paketini aşağıdaki sürümü vardır: "5.1.2". Yükleme veya güncelleştirme bu paketleri yoluyla [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketlerini de içerir.

Yükleme veya NuGet Paket Yöneticisi konsolu kullanılarak yayımlanan NuGet paketlerini güncelleştirin:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve ASP.NET Web API 2.1 RTM ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 yeni özellikler

<a id="global-error"></a>
### <a name="global-error-handling"></a>Genel hata işleme

Tüm işlenmeyen özel durumlar şimdi merkezi bir mekanizma aracılığıyla kaydedilebilir ve işlenmeyen özel durumlar için davranış özelleştirilebilir.

Bu, aynı anda işlenmekte olan istek gibi gerçekleştiği bağlamı hakkında bilgi ve işlenmeyen özel durum bkz tüm, birden çok özel durum günlükçüleri framework destekler.

Örneğin, aşağıdaki kod, tüm işlenmeyen özel durumları günlüğe kaydetmek için System.Diagnostics.TraceSource kullanır:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Varsayılan özel durum işleyici da değiştirebilir, böylece işlenmeyen bir özel durum, gönderilen HTTP yanıt iletisi tam olarak özelleştirebilirsiniz oluşur.

Sağladık bir [örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , popüler ELMAH framework aracılığıyla tüm işlenmeyen özel durumları günlüğe kaydeder.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Yönlendirme geliştirmeleri özniteliği

Öznitelik şimdi yönlendirme kısıtlamaları, sürüm ve üstbilgi tabanlı yol seçimi etkinleştirme destekler. Ayrıca, öznitelik rotaları pek çok görünüşünün şimdi aracılığıyla özelleştirilebilir **IDirectRouteFactory** arabirimi ve **RouteFactoryAttribute** sınıfı. Rota öneki artık yoluyla Genişletilebilir **IRoutePrefix** arabirimi ve **RoutePrefixAttribute** sınıfı.

Sağladık bir [örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) denetleyicileri 'api-version' HTTP üstbilgisinin tarafından dinamik olarak filtrelemek için kısıtlamaları kullanan.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Yardım sayfası geliştirmeleri

Web API 2.1 aşağıdaki geliştirmeleri içerir [API Yardım sayfaları](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Parametreleri veya dönüş türleri eylemlerin ayrı ayrı özellikler belgeleri.
- Veri modeli ek açıklamaları belgeleri.

Yardım sayfalarına UI tasarımını Ayrıca, bu değişiklikleri uyum sağlayacak şekilde güncelleştirildi.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>IgnoreRoute desteği

Web API 2.1 destekleyen bir dizi ile Web API, yönlendirme, URL desenlerini yoksayılıyor **IgnoreRoute** genişletme yöntemleri **HttpRouteCollection**. Bu yöntemleri belirtilen şablonla eşleşen herhangi bir URL yoksaymak Web API neden ve uygunsa, ek işleme uygulamak konak izin verme.

Aşağıdaki örnek ile başlayan URI yoksayar bir &quot;içerik&quot; segment:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON medya türü biçimlendiricisi

Web API destekler [BSON](http://bsonspec.org/) kablo biçiminde, istemci ve sunucu.

Sunucu tarafında BSON etkinleştirmek için add **BsonMediaTypeFormatter** biçimlendiricileri koleksiyonu için:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

.NET istemci BSON biçimi nasıl tüketebileceği aşağıda verilmiştir:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Sağladık bir [örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) istemci ve sunucu tarafı gösterir.

Daha fazla bilgi için bkz: [Web API 2.1 BSON desteği](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Zaman uyumsuz filtreleri için daha iyi destek

Web API artık zaman uyumsuz yürütme filtreleri oluşturmak için kolay bir yol destekler. Bu özellik yararlıdır veritabanı erişimi gibi bir zaman uyumsuz eylemi gerçekleştirmek, filtre gerekiyor. Filtre temel sınıfları, yalnızca zaman uyumlu yöntemleri eline olduğundan daha önce bir zaman uyumsuz filtre oluşturmak için filtre arabirimi kendiniz uygulamak gerekiyordu. Sanal kılabilirsiniz artık `On*Async` filtre yöntemlerinin temel sınıfı.

Örneğin:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**, **ActionFilterAttribute**, ve **ExceptionFilterAttribute** sınıfları tüm Web API 2.1 içinde zaman uyumsuz destekler.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Sorgu kitaplığı biçimlendirme istemci için ayrıştırma

Daha önce **Microsoft.ASPNET.webapi.Client** ayrıştırma ve sunucu tarafı kodu için URI sorguları güncelleştirme desteklenir, ancak eşdeğer taşınabilir kitaplığı bu özelliği eksik. Web API 2.1 istemci uygulaması artık kolayca ayrıştırma ve bir sorgu dizesi güncelleştirin.

Aşağıdaki örnekler ayrıştırma, değiştirmek ve URI sorguları oluşturmak nasıl gösterir. (Örnekler basit bir konsol uygulaması gösterir.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, bilinen sorunlar ve ASP.NET Web API 2.1 RTM, önemli değişiklikler açıklanmaktadır.

### <a name="attribute-routing"></a>Öznitelik yönlendirme

Öznitelik yönlendirme eşleşmeleri belirsizlikleri şimdi ilk eşleşmeye seçmek yerine bir hata bildirir.

Öznitelik rotaları yasaklanmış kullanarak *{controller}* parametresini kullanarak gelen ve giden *{action}* rota parametresi yerleştirilen eylemleri. Bu parametreler, büyük olasılıkla belirsizliğe neden olur.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Yapı iskelesi MVC/Web API projesine projede zaten mevcut olanları 5.0 paketleri 5.1 paketleri sonuçlarında ile

ASP.NET Web API 2.1 RTM için NuGet paketlerini güncelleştirme ASP.NET yapı iskelesi gibi Visual Studio Araçları veya ASP.NET Web uygulaması proje şablonu güncelleştirmez. ASP.NET çalışma zamanı paketleri (5.0.0.0)'ın önceki sürümünü kullanırlar. Sonuç olarak, bunlar zaten projelerinizde yoksa ASP.NET yapı iskelesi gerekli paketleri, önceki sürümü (5.0.0.0) yükleyin. Ancak, Visual Studio 2013 RTM veya güncelleştirme 1 ASP.NET iskele projelerinizi son paketlerinde üzerine yazmaz.

Web API 2.1 veya ASP.NET MVC 5.1 paketleri güncelleştirdikten sonra ASP.NET yapı iskelesi kullanırsanız, Web API ve MVC sürümleri tutarlı olduğundan emin olun.

### <a name="type-renames"></a>Türü yeniden adlandırma

Bazı öznitelik yönlendirme genişletilebilirliği için kullanılan türleri RC sürümünden 2.1 RTM adlandırıldı.

| Eski tür adı (2.1 RC) | Yeni tür adı (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Özel durum filtreleri zaman uyumsuz eylemleri oluşturulan toplam özel durumları kaydırma değil

Daha önce bir zaman uyumsuz eylem oluşturduysa bir **AggregateException**, bir özel durum filtresi özel durum kaydırma ve **OnException** temel özel durum almak. 2.1, özel durum filtresi, kaydırma değil ve **OnException** özgün alır **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Hata Düzeltmeleri

Bu sürüm çeşitli hata düzeltmeleri de içerir. Tam listesini burada bulabilirsiniz:

- [5.1.0 Paketi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 paketi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 paket IntelliSense güncelleştirmeler ancak hiçbir hata düzeltmeleri içerir.
