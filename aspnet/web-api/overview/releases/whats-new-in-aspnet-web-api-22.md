---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: ASP.NET Web API 2.2 yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 400329dd852ca3c527387ee45e3e902b725e771b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566826"
---
<a name="whats-new-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 yenilikler nelerdir?
====================
tarafından [Microsoft](https://github.com/microsoft)

Bu konuda, ASP.NET Web API 2.2 yenilikler açıklanmaktadır.

- [İndir](#download)
- [Belgeler](#documentation)
- [ASP.NET Web API 2.2 yeni özellikler](#newf)

    - [OData v4](#OData)
    - [Yönlendirme geliştirmeleri özniteliği](#ARI)
    - [Windows Phone 8.1 için Web API İstemci Desteği](#phone)
- [Bilinen sorunlar ve yeni değişiklikler](#known-issues)
- [Hata düzeltmeleri](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>İndir

Çalışma zamanı özellikleri NuGet galerisinde NuGet paketlerini olarak yayımlanmıştır. Tüm çalışma zamanı paketleri izleyin [anlamsal sürüm oluşturma](http://semver.org/) belirtimi. En son ASP.NET Web API 2.2 paketini aşağıdaki sürümü vardır: "5.2.0". Yükleme veya güncelleştirme bu paketleri yoluyla [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Sürüm, karşılık gelen yerelleştirilmiş NuGet paketlerini de içerir.

Yükleme veya NuGet Paket Yöneticisi konsolu kullanılarak yayımlanan NuGet paketlerini güncelleştirin:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Belgeler

Öğreticiler ve ASP.NET Web API 2.2 ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>ASP.NET Web API 2.2 yeni özellikler

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Bu sürüm OData v4 protokolü için desteği ekler. Daha fazla bilgi için bkz: [Web API OData v4 belgeleri.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Anahtar özellikleri ve değişiklikleri OData v4 için bazıları şunlardır:

- [OData modelinde yumuşatma özellikleri için destek](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute ve ODataConventionModelBuilder ConcurrencyCheckAttribute desteği](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Eylemler için kolay başlık sağlamak için olanağı sunar.](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- ODL UriParser ile tümleştirme
- Desteği [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [kapsama](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) ve [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- İlkel türler için cast desteği
- [OData işlevi desteği eklendi](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [İşlev çağrıları için destek parametre diğer adlar](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Ortası büyük küçük harf adlandırma kuralını modelde desteği](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- $Filter öğesinde cast() desteği
- Açık karmaşık tür için destek
- Kaldırılan EntitySetController ve AsyncEntitySetController
- [Değiştirilen $link için $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Öznitelik yönlendirme desteği eklendi](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- OData çekirdek kitaplıkları 6.4.0 kullanır

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Yönlendirme geliştirmeleri özniteliği

Öznitelik şimdi yönlendirme öznitelik rotaları nasıl bulunan ve yapılandırılmış üzerinde tam denetim verir IDirectRouteProvider adlı bir genişletilebilirlik noktası sağlar. Bir IDirectRouteProvider eylemlerin ve denetleyicilerin tam olarak hangi yönlendirme yapılandırması için bu eylemleri istendiğini belirtmek için ilişkili rota bilgilerle birlikte listesini sağlamaktan sorumludur. IDirectRouteProvider uygulaması MapAttributes/MapHttpAttributeRoutes çağrılırken belirtilebilir.

IDirectRouteProvider özelleştirme DefaultDirectRouteProvider bizim varsayılan uygulaması genişleterek kolay olacaktır. Bu sınıf öznitelikleri bulma, rota girişleri oluşturmak ve rota öneki ve alan öneki keşfetmek için mantığı değiştirmek için ayrı geçersiz kılınabilir sanal yöntemleri sağlar.

Bu yeni genişletilebilirlik noktası ile ne yapabilir ilişkin bazı örnekler şunlardır:

1. Rota özniteliklerin destek devralma

    Örnek:

    Bir istek LIKE "/ değerleri/api/10" "Başarı: 10" başarıyla burada döndürür

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. İstediğiniz gibi bazı kuralı izleyerek, öznitelik rotaları için varsayılan bir rota adı sağlayın. Varsayılan olarak, öznitelik yönlendirme öznitelik rotaları için adları otomatik olarak oluşturmaz.
3. Bunlar rota tablosunda şunun önce öznitelik rotaları rota şablonu merkezi bir yerde değiştirin.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Windows Phone 8.1 için Web API İstemci Desteği

Windows Phone 8.1 hedeflerken veya gelen Web API İstemci mantığınızı uygulamak için Web API İstemci NuGet paketi şimdi kullanabileceğiniz bir evrensel uygulama içinde.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

Bu bölümde, bilinen sorunlar ve ASP.NET Web API 2.2 önemli değişiklikler açıklanmaktadır.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Model Oluşturucu

Sorun: Aşırı yüklenmiş işlevlerin Functionımport olarak açığa çıkabileceği değil

2 aşırı yüklenmiş işlevlerin vardır ve bunlar da System.InvalidOperationException ~/GetAllConventionCustomers(CustomerName={customerName}) sonuçlarında isteyen aşağıda gösterildiği gibi Functionımport durumunda.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Geçici çözüm: Bu sorun için geçici çözüm hem de, bir işlev aşırı yüklemelerinin FunctionImports eklemektir.

#### <a name="odata-routing"></a>OData yönlendirme

Eğik çizgi (% 2F) arasında URL dize değişmez değerleri kodlanmış ve OData kaynak yollarında kullanıldığında backslash(%5C) neden 404 hatası.

Örneğin, dize değişmez değerleri OData kaynak yollarında işlevlerin parametreleri ya da varlık kümelerinin anahtar değerleri kullanılır.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Bu tür istekleri konakları kaydını kaçış Hizmetleri aldığınızda, bu Web API çalışma zamanı için geçirmeden önce kaçış dizilerine. Bu, aşağıdaki gibi saldırılarına karşı korur:  
  
 http://www.contoso.com/..%2f..%2f/Windows/System32/cmd.exe?/c+dir+c:

Bu, 404 hatası (bulunamadı) dönmek Web API OData yığını neden olur. Bu hatayı önlemek için istemci ters eğik çizgi (% 255 C) ve çift kaçış sıralarına eğik çizgi işareti (% 252F) kullanmanız gerekir. Bu sorgu dizelerine /Employees gibi gerçekleşmez? $filter adı eq 'Adı % 2F' =

**Atlanmamış eğik çizgi ('/') not edin ve ters eğik çizgi (") içinde OData kaynak yolu dize değişmez değerleri geçerli değildir. Eğik çizgi yalnızca yolu ayırıcısı olarak görüntülenmelidir ve ters eğik çizgi OData kaynak yolu hiç görünmemesi gerekir. (Her ikisi de bir OData sorgu dizesi bazı kısımları kullanılabilir.)**

Geçici çözüm: Gerçekte bunları ayrıştırmadan önce eğik çizgi ve dize değişmez değerleri ters eğik çizgiyi kaçınmak için DefaultODataPathHandler ayrıştırma yöntemi geçersiz. Bu yaklaşımın burada bir örnek bulabilirsiniz.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Sorgulanabilir]

[Queryable] özniteliği kullanım dışı bırakılmıştır. Yeni OData v3 uygulamaları kullanması gereken **System.Web.Http.OData.EnableQueryAttribute**.

**ODataHttpConfigurationExtensions.EnableQuerySupport** genişletme yöntemi şimdi ekler bir **EnableQueryAttribute** genel filtre koleksiyonuna. Tüm denetleyiciniz varsa **[Queryable]** özniteliği, çağırma `config.EnableQuerySupport()` neden olacak **[Queryable]** başarısız özniteliği

Bu sorunu çözmek için önerilen yöntem, tüm örneklerini değiştirmektir **QueryableAttribute** ile **System.Web.Http.OData.EnableQueryAttribute**.

Aşağıdaki kod, Web API yapılandırmada kullanmak için alternatif bir geçici çözüm şöyledir:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Öznitelik yönlendirme

Sorun: FromUri özniteliği ile donatılmış karmaşık türünün Model bağlama özniteliği yönlendirme kullanırken farklı şekilde davranır.

Aşağıdaki bağlantı sorun izleme ve geçici bir çözüm ayrıntılarını da sahiptir.  
[http://aspnetwebstack.Codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Sorun: Yapı İskelesi MVC/Web API projesine 5.2.0 ile 5.1.2 paketleri sonuçlarında paketler için olanları projede zaten mevcut değil

ASP.NET MVC 5.2 için NuGet paketlerini güncelleştirme ASP.NET yapı iskelesi gibi Visual Studio araçlarını veya ASP.NET Web uygulaması proje şablonu güncelleştirmez. ASP.NET çalışma zamanı paketleri (örneğin 5.1.2 güncelleştirme 2 ')'ın önceki sürümünü kullanırlar. Sonuç olarak, bunlar zaten projelerinizde yoksa ASP.NET yapı iskelesi gerekli paketleri, önceki sürümünü (örneğin güncelleştirme 2'deki 5.1.2) yükleyin. Ancak, Visual Studio 2013 RTM veya güncelleştirme 1 ASP.NET iskele projelerinizi son paketlerinde üzerine yazmaz. Web API 2.2 veya ASP.NET MVC 5.2 projelerinizi paketleri güncelleştirdikten sonra ASP.NET yapı iskelesi kullanırsanız, Web API ve ASP.NET MVC sürümleri tutarlı olduğundan emin olun.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Hata düzeltmeleri ve küçük özellik güncelleştirmeleri

Bu sürüm çeşitli hata düzeltmeleri ve küçük özellik içerir güncelleştirmeler. Tam listesini burada bulabilirsiniz:

- [5.2 paketi](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Microsoft.AspNet.OData 5.2.1 paketi NuGet bağımlılık güncelleştirmeler ancak hiçbir hata düzeltmeleri içerir. Bu güncelleştirme ile 6.4.0 Microsoft.OData.Core üzerinde artık katı bir bağımlılık yoktur, ancak bir 6.4.0 7.0.0 arasındaki herhangi bir sürümüne yükseltebilirsiniz.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

Bu sürümde biz değiştirmek bağımlılık yapmış olduğunuz `Json.Net 6.0.4`. Bu sürümündeki yenilikler hakkında daha fazla bilgi için `Json.NET`, bkz: [Json.NET 6.0 sürüm 4 - JSON birleştirme, bağımlılık ekleme](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Bu sürümdeki diğer yeni özellikleri ve hata düzeltmeleri Web API'si sahip değil. Biz, biz Web API'sini yeni bu sürümüne bağlıdır kendine ait diğer tüm bağımlı paketler sonradan güncelleştirdiniz.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

Yayın hakkında bilgi edinebilirsiniz [burada](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Bu sürüm yalnızca hata düzeltmeleri içerir. Kullanabileceğiniz [bu sorguyu](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) bu sürümde giderilen sorunlar listesini görmek için.
