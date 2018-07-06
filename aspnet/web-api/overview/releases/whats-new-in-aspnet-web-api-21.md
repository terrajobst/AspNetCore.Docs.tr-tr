---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-21
title: ASP.NET Web API 2.1 sürümündeki yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: b6721bba-38c8-48c4-acbf-274c1a34e817
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 7952614456b1de24e4c618b9e7ba8448b2a01741
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838409"
---
<a name="whats-new-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 sürümündeki yenilikler
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET Web API 2.1 için Yenilikler açıklanmaktadır.

- [İndir](#download)
- [Belgeler](#documentation)
- [ASP.NET Web API 2.1 sürümünde yeni özellikler](#new-features)

    - [Genel hata işleme](#global-error)
    - [Öznitelik yönlendirme geliştirmeleri](#attribute-routing)
    - [Yardım sayfası geliştirmeleri](#help-page)
    - [IgnoreRoute desteği](#ignoreroute)
    - [BSON medya türü biçimlendiricisi](#bson)
    - [Zaman uyumsuz filtreleri için daha iyi destek](#async-filters)
    - [Sorgu için istemci kitaplığı biçimlendirme ayrıştırma](#query-parsing)
- [Bilinen sorunlar ve yeni değişiklikler](#known-issues)
- [Hata düzeltmeleri](#bug-fixes)

<a id="download"></a>
## <a name="download"></a>İndir

Çalışma zamanı özellikleri, NuGet galerisindeki NuGet paketleri olarak kullanıma sunulur. Tüm çalışma zamanı paketlerini izleyin [Semantic Versioning](http://semver.org/) belirtimi. ASP.NET Web API 2.1 RTM paketin en son sürümü var: "5.1.2". Yükleme veya güncelleştirme yoluyla bu paketleri [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketleri de içerir.

NuGet Paket Yöneticisi konsolu kullanarak için kullanıma sunulan NuGet paketlerini güncelleştirin ya da yükleyin:

[!code-console[Main](whats-new-in-aspnet-web-api-21/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve ASP.NET Web API 2.1 RTM ile ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ([https://www.asp.net/web-api](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 sürümünde yeni özellikler

<a id="global-error"></a>
### <a name="global-error-handling"></a>Genel hata işleme

Merkezi bir mekanizma aracılığıyla tüm işlenmemiş özel durumlar artık kaydedilebilecek ve işlenmeyen özel durumlar için davranış özelleştirilebilir.

Framework işlenmeyen özel durum ve bu, örneğin sırada işlenmekte olan isteği gerçekleştiği bağlamı hakkında bilgi tüm, birden çok özel durum günlükçüleri destekler.

Örneğin, aşağıdaki kod, tüm işlenmemiş özel durumları günlüğe kaydetmek için System.Diagnostics.TraceSource kullanır:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample2.cs)]

Varsayılan özel durum işleyicisini de değiştirebilir, böylece işlenmeyen bir özel durum, gönderilen HTTP yanıt iletisi tam olarak özelleştirebilirsiniz gerçekleşir.

Sağladık bir [örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/Elmah/ReadMe.txt) , popüler ELMAH çerçevesi aracılığıyla tüm işlenmemiş özel durumları günlüğe kaydeder.

<a id="attribute-routing"></a>
### <a name="attribute-routing-improvements"></a>Öznitelik yönlendirme geliştirmeleri

Öznitelik şimdi yönlendirme kısıtlamaları, sürüm oluşturma ve üst bilgi tabanlı yol seçimi etkinleştirme destekler. Ayrıca, öznitelik rotaları birçok yönden artık özelleştirilebilir aracılığıyla **IDirectRouteFactory** arabirimi ve **RouteFactoryAttribute** sınıfı. Rota öneki ile Genişletilebilir **IRoutePrefix** arabirimi ve **RoutePrefixAttribute** sınıfı.

Sağladık bir [örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/RoutingConstraintsSample/ReadMe.txt) denetleyicileri bir 'api-version' HTTP üst bilgisi tarafından dinamik olarak filtrelemek için kısıtlamaları kullanan.

<a id="help-page"></a>
### <a name="help-page-improvements"></a>Yardım sayfası geliştirmeleri

Web API 2.1 aşağıdaki geliştirmeleri içerir [API Yardım sayfaları](../getting-started-with-aspnet-web-api/creating-api-help-pages.md):

- Parametre veya dönüş türleri eylemlerin özelliklerini bireysel belgeleri.
- Veri modelini ek belgeleri.

Kullanıcı Arabirimi tasarımı Yardım sayfaları, ayrıca, bu değişiklikleri uyum sağlamak için güncelleştirildi.

<a id="ignoreroute"></a>
### <a name="ignoreroute-support"></a>IgnoreRoute desteği

Web API 2.1 destekleyen Web API'si, bir dizi aracılığıyla akışında URL desenleri yoksayılıyor **IgnoreRoute** üzerinde genişletme yöntemleri **HttpRouteCollection**. Bu yöntemler Web API'si, belirtilen bir şablonu eşleşen herhangi bir URL yok saymak neden ve uygun durumlarda ek işleme uygulamak konak izin verin.

Aşağıdaki örnek ile başlayan bir URI'leri yoksayar bir &quot;içeriği&quot; segment:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample3.cs)]

<a id="bson"></a>
### <a name="bson-media-type-formatter"></a>BSON medya türü biçimlendiricisi

Web API destekler [BSON](http://bsonspec.org/) kablo biçimini, istemci ve sunucu.

Sunucu tarafında BSON etkinleştirmek için eklemeniz **BsonMediaTypeFormatter** biçimlendiricileri koleksiyonu için:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample4.cs)]

.NET istemci BSON biçimi nasıl tüketebileceği aşağıda verilmiştir:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample5.cs)]

Sağladık bir [örnek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/ReadMe.txt) istemci ve sunucu tarafı gösterir.

Daha fazla bilgi için [Web API 2.1 sürümünde BSON desteği](../formats-and-model-binding/bson-support-in-web-api-21.md)

<a id="async-filters"></a>
### <a name="better-support-for-async-filters"></a>Zaman uyumsuz filtreleri için daha iyi destek

Web API'si, zaman uyumsuz olarak yürütülen bir filtre oluşturmak için kolay bir yol artık desteklemektedir. Bu özellik yararlıdır filtrenizle veritabanı erişimi gibi bir zaman uyumsuz eylemi gerçekleştirmek gerekiyor. Filtre taban sınıfları yalnızca zaman uyumlu yöntemler açıkta olduğundan, zaman uyumsuz bir filtre oluşturmak için kendiniz filtre arabirim uygulamak vardı. Şimdi sanal kılabilirsiniz `On*Async` filtrenin yöntemleri taban sınıf.

Örneğin:

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample6.cs)]

**AuthorizationFilterAttribute**, **ActionFilterAttribute**, ve **ExceptionFilterAttribute** sınıfları tüm Web API 2.1 içinde zaman uyumsuz destekler.

<a id="query-parsing"></a>
### <a name="query-parsing-for-the-client-formatting-library"></a>Sorgu için istemci kitaplığı biçimlendirme ayrıştırma

Daha önce **Microsoft.ASPNET.webapi.Client** ayrıştırma ve sunucu tarafı kodu için URI sorguları güncelleme desteklenir, ancak eşdeğer taşınabilir kitaplık bu özelliği eksik. Web API 2.1 içinde bir istemci uygulaması artık kolayca ayrıştırma ve bir sorgu dizesi güncelleştirin.

Aşağıdaki örnekler ayrıştırma, değiştirebilir ve URI sorgu oluşturma işlemini göstermektedir. (Örnekler basit bir konsol uygulaması gösterir.)

[!code-csharp[Main](whats-new-in-aspnet-web-api-21/samples/sample7.cs)]

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, bilinen sorunlar ve ASP.NET Web API 2.1 RTM bozucu değişiklikler açıklanmaktadır.

### <a name="attribute-routing"></a>Öznitelik yönlendirme

Öznitelik yönlendirme eşleşme belirsizliklerden artık ilk eşleşme seçmek yerine bir hata bildirir.

Öznitelik rotaları kullanmaları yasaktır *{denetleyici}* parametresi ve kullanarak *{action}* rota parametresi eylemleri yerleştirilir. Bu parametreler çok büyük olasılıkla belirsizliğe neden olur.

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a>Yapı iskelesi MVC/Web API projesine 5.0 paketler için projede mevcut 5.1 paketleri sonuçları

ASP.NET Web API 2.1 RTM için NuGet paketlerini güncelleştirme, Visual Studio Araçları gibi ASP.NET iskeleti oluşturma veya ASP.NET Web uygulaması proje şablonu güncelleştirmez. ASP.NET çalışma zamanı paketlerini (5.0.0.0) önceki sürümünü kullanırlar. Sonuç olarak, zaten projelerinizde kullanılabilir yoksa ASP.NET iskeleti oluşturma önceki sürümünü (5.0.0.0) gerekli paketleri yükleyin. Ancak, en son paketlerin projelerinizde te ASP.NET iskeleti oluşturma veya güncelleştirme 1'Visual Studio 2013 RTM üzerine yazılmaz.

Web API 2.1 veya ASP.NET MVC 5.1 paketleri güncelleştirdikten sonra ASP.NET iskeleti oluşturma kullanırsanız, Web API ve MVC sürümleri tutarlı olduğundan emin olun.

### <a name="type-renames"></a>Tür yeniden adlandırma

Bazı öznitelik yönlendirme genişletilebilirliği için kullanılan türleri RC'den 2.1 RTM'ye değiştirilmiştir.

| Eski tür adı (2.1 RC) | Yeni tür adı (2.1 RTM) |
| --- | --- |
| IDirectRouteProvider | IDirectRouteFactory |
| RouteProviderAttribute | RouteFactoryAttribute |
| DirectRouteProviderContext | DirectRouteFactoryContext |

### <a name="exception-filters-do-not-unwrap-aggregate-exceptions-thrown-in-async-actions"></a>Özel durum filtreleri, zaman uyumsuz eylemleri oluşan toplam özel durum unwrap değil

Daha önce bir zaman uyumsuz eylem oluşturduysa bir **AggregateException**, özel durum filtresi özel durum sarmalamadan çıkarma ve **OnException** temel özel durum elde edersiniz. 2.1 içinde özel durum filtresi, unwrap değil ve **OnException** özgün alır **AggregateException**.

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a>Hata Düzeltmeleri

Bu sürüm ayrıca çeşitli hata düzeltmeleri içerir. Tam listesini burada bulabilirsiniz:

- [5.1.0 paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=Web%20API|Web%20API%20OData&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [5.1.1 paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.1.1%20RTM&assignedTo=All&component=Web%20API&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

5.1.2 paket IntelliSense güncelleştirme, ancak hiçbir hata düzeltmeleri içerir.
