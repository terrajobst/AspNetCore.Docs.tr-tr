---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: ASP.NET Web API 2.2 sürümündeki yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: ef08a3bb397ff54795ca6c70cdcc35206cf122f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752577"
---
<a name="whats-new-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 sürümündeki yenilikler
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET Web API 2.2 için Yenilikler açıklanmaktadır.

- [İndir](#download)
- [Belgeler](#documentation)
- [ASP.NET Web API 2.2 sürümündeki yeni özellikler](#newf)

    - [OData v4](#OData)
    - [Öznitelik yönlendirme geliştirmeleri](#ARI)
    - [Windows Phone 8.1 için Web API İstemci Desteği](#phone)
- [Bilinen sorunlar ve yeni değişiklikler](#known-issues)
- [Hata düzeltmeleri](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>İndir

Çalışma zamanı özellikleri, NuGet galerisindeki NuGet paketleri olarak kullanıma sunulur. Tüm çalışma zamanı paketlerini izleyin [Semantic Versioning](http://semver.org/) belirtimi. ASP.NET Web API 2.2 paketin en son sürümü var: "5.2.0". Yükleme veya güncelleştirme yoluyla bu paketleri [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketleri de içerir.

NuGet Paket Yöneticisi konsolu kullanarak için kullanıma sunulan NuGet paketlerini güncelleştirin ya da yükleyin:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve diğer bilgileri ASP.NET Web API 2.2 ASP.NET web sitesinden kullanılabilir ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 sürümündeki yeni özellikler

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Bu sürüm, OData v4 protokolünü için destek ekler. Daha fazla bilgi için [Web API OData v4 belgeleri.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Temel özellikler ve değişiklikleri OData v4 bazıları şunlardır:

- [OData modelinde yumuşatma özellikleri için destek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [ComplexTypeAttribute, AssociationAttribute TimesTampAttribute ve ConcurrencyCheckAttribute ODataConventionModelBuilder içinde desteği](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Eylemler için kolay başlık sağlamanız olanağı sağlayın](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- ODL UriParser ile tümleştirin
- Destek [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [kapsama](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) ve [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Temel türler için tür dönüştürme desteği
- [OData işlevi desteği eklendi](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Parametre diğer adlarına işlev çağrıları için destek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Modelde camel case adlandırma kuralı desteği](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- $Filter içinde cast() desteği
- Açık bir karmaşık tür için destek
- Kaldırılan EntitySetController ve AsyncEntitySetController
- [Değiştirilen $link için $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Öznitelik yönlendirme desteği eklendi](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- OData çekirdek kitaplıkları 6.4.0 kullanır

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Öznitelik yönlendirme geliştirmeleri

Öznitelik şimdi yönlendirme öznitelik rotaları nasıl bulunan ve yapılandırılmış üzerinde tam denetim sağlayan IDirectRouteProvider adlı bir genişletilebilirlik noktası sağlar. Bir IDirectRouteProvider listesini eylemlerin ve denetleyicilerin birlikte tam olarak hangi yönlendirme yapılandırması için bu eylemlerin istendiğini belirtmek için ilişkili rota bilgilerini sağlamaktan sorumludur. IDirectRouteProvider uygulaması MapAttributes/MapHttpAttributeRoutes çağırırken belirtilebilir.

IDirectRouteProvider özelleştirme varsayılan kararlılığımızın DefaultDirectRouteProvider genişleterek kolay olacaktır. Bu sınıf öznitelikleri bulmak için rota girişleri oluşturup rota öneki ve alan öneki bulunması için mantığı değiştirmek için ayrı geçersiz kılınabilir sanal yöntemleri sağlar.

Bu yeni genişletilebilirlik noktası ile ne yapabilir ilişkin bazı örnekler şunlardır:

1. Rota özniteliklerinin destek devralma

    Örnek:

    Burada benzer "/ değerler/API/10" istek "Başarı: 10" başarıyla getirir

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. İstediğiniz gibi bazı kuralını takip ederek, öznitelik rotaları için varsayılan bir yol adı sağlayın. Varsayılan olarak, öznitelik yönlendirme öznitelik rotaları için adları otomatik olarak oluşturmaz.
3. Bunlar rota tablosunda düştüğünden önce öznitelik rotaları rota şablonu merkezi bir yerde değiştirin.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Windows Phone 8.1 için Web API İstemci Desteği

Windows Phone 8.1 hedeflenirken veya gelen, Web API'sini istemci mantığı uygulamak için artık Web API İstemci NuGet paketini kullanabilirsiniz bir evrensel uygulama içinde.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, bilinen sorunlar ve ASP.NET Web API 2.2 bozucu değişiklikler açıklanmaktadır.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Model Oluşturucu

Sorun: Aşırı yüklenmiş işlevler Functionımport kullanıma sunulabilir değil

2 aşırı yüklenmiş işlev vardır ve bunlar da sonra System.InvalidOperationException ~/GetAllConventionCustomers(CustomerName={customerName}) sonuçlarında isteyen aşağıda gösterildiği gibi Functionımport durumunda.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Geçici çözüm: Bu sorunun geçici çözümü her iki işlev aşırı yüklemelerinin FunctionImports eklemektir.

#### <a name="odata-routing"></a>OData yönlendirme

URL içeren bir dize değişmez değerleri, eğik çizgi (% 2F) kodlanmış ve OData kaynak yolları kullanıldığında backslash(%5C) neden bir 404 hatası.

Örneğin, dize sabit değerleri işlevlerin parametre veya anahtar değerlerini varlık kümeleri olarak OData kaynak yolları kullanılabilir.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Bu tür istekleri konakları kaldırma kaçış Hizmetleri aldığınızda, bu Web API çalışma zamanı geçirmeden önce kaçış dizileri. Bu, aşağıdaki gibi saldırılarına karşı korur:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

Bu, bir 404 hatası (bulunamadı) geri dönmek Web API OData yığın neden olur. Bu hatayı önlemek için istemcinizi ters eğik çizgi (% 255 C) ve eğik çizgi işareti (% 252F) çift kaçış dizileri kullanmanız gerekir. Bu sorgu dizelerine /Employees gibi gerçekleşmez? $filter adı eq 'Adı % 2F' =

**Kaçılmamış eğik çizgi ('/') not edin ve ters eğik çizgi (") OData kaynak yolunu dize sabit geçerli değildir. Yalnızca yol ayırıcı olarak eğik çizgi görünmelidir ve ters eğik çizgi OData kaynak yolunda hiç görünmemelidir. (Her ikisi de OData sorgu dizesi bazı kısımları içinde kullanılabilir.)**

Geçici çözüm: Gerçekten bunları ayrıştırma önce ters eğik çizgiyi dize değişmez değerleri ve eğik çizgi kaçış DefaultODataPathHandler ayrıştırma yöntemini geçersiz. Bu yaklaşımın burada bir örnek bulabilirsiniz.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Sorgulanabilir]

[Queryable] özniteliği kullanım dışı bırakılmıştır. Yeni OData v3 uygulamalar kullanması gereken **System.Web.Http.OData.EnableQueryAttribute**.

**ODataHttpConfigurationExtensions.EnableQuerySupport** genişletme yöntemi artık ekler bir **EnableQueryAttribute** genel filtre koleksiyonuna. Başka bir denetleyici varsa **[Queryable]** çağırma özniteliği `config.EnableQuerySupport()` neden olur **[Queryable]** başarısız olmasına özniteliği

Bu sorunu çözmek için önerilen yöntem, tüm örneklerini değiştirmektir **Queryableattribute'taki** ile **System.Web.Http.OData.EnableQueryAttribute**.

Alternatif bir geçici çözüm, Web API yapılandırmanızı aşağıdaki kodu kullanmaktır:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Öznitelik yönlendirme

Sorun: Model bağlama FromUri özniteliği ile donatılmış karmaşık türün özniteliği yönlendirme kullanıldığında farklı davranır.

Aşağıdaki bağlantıda, sorun izleme ve ayrıca geçici bir çözüm hakkında ayrıntılar bulunur.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Sorun: Yapı İskelesi MVC/Web API projesine 5.2.0 ile 5.1.2 paketleri sonuçlarında paketleri için projede zaten mevcut olmayan hizmetlerdir

ASP.NET MVC 5.2 için NuGet paketlerini güncelleştirme, Visual Studio Araçları gibi ASP.NET iskeleti oluşturma veya ASP.NET Web uygulaması proje şablonu güncelleştirmez. ASP.NET çalışma zamanı paketlerini (örneğin 5.1.2 güncelleştirme 2 ')'ın önceki sürümünü kullanırlar. Sonuç olarak, zaten projelerinizde kullanılabilir yoksa ASP.NET iskeleti oluşturma önceki sürümünü (örneğin 5.1.2 güncelleştirme 2) gerekli paketleri yükleyin. Ancak, en son paketlerin projelerinizde te ASP.NET iskeleti oluşturma veya güncelleştirme 1'Visual Studio 2013 RTM üzerine yazılmaz. Sonra Web API 2.2 veya ASP.NET MVC 5.2 projelerinizin paketlerin güncelleştirilmesi, ASP.NET iskeleti oluşturma kullanırsanız, Web API ve ASP.NET MVC sürümleri tutarlı olduğundan emin olun.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Hata düzeltmeleri ve alt özellik güncelleştirmeleri

Bu sürüm çeşitli hata düzeltmeleri ve küçük özelliği içerir güncelleştirmeleri. Tam listesini burada bulabilirsiniz:

- [5.2 paket](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Microsoft.AspNet.OData 5.2.1 paket NuGet bağımlılık güncelleştirme, ancak hiçbir hata düzeltmeleri içerir. Bu güncelleştirme ile 6.4.0 Microsoft.OData.Core üzerinde artık katı bir bağımlılık yoktur, ancak bir 6.4.0 7.0.0 arasındaki herhangi bir sürümüne yükseltebilirsiniz.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

Bu sürümde bir bağımlılık için değişikliği gerçekleştirdik `Json.Net 6.0.4`. Bu sürümündeki yenilikler hakkında daha fazla bilgi için `Json.NET`, bkz: [Json.NET 6.0 Release 4 - JSON birleşimi, bağımlılık ekleme](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Bu sürüm, Web API'SİNDE herhangi bir yeni özellik veya hata düzeltmesi içermez. Daha sonra Web API'ın bu yeni sürümüne bağlı olduğumuz diğer tüm bağlı paketleri güncelleştirdik.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

Yayın hakkında bilgi edinebilirsiniz [burada](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Bu sürüm yalnızca hata düzeltmeleri içerir. Kullanabileceğiniz [bu sorgu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) bu sürümde giderilen sorunlar listesinde görmek için.
