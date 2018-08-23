---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: Yönlendirme kuralları ASP.NET Web API 2 Odata | Microsoft Docs
author: MikeWasson
description: Bu makalede, Web API OData uç noktaları için kullandığı yönlendirme kuralları açıklanır.
ms.author: riande
ms.date: 07/31/2013
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0199ccde100c8c3ab70db912b8ab984e0617f9dd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753784"
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Yönlendirme kuralları ASP.NET Web API 2 Odata
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Bu makalede, Web API OData uç noktaları için kullandığı yönlendirme kuralları açıklanır.


Web API OData isteği aldığında, denetleyici adı ve eylem adı için istek eşler. Eşleme, HTTP yöntemi ve URI temel alır. Örneğin, `GET /odata/Products(1)` eşlendiği `ProductsController.GetProduct`.

Bölümünde, bu makalenin 1 miyim yerleşik OData rota kurallarını açıklar. Bu kuralları özellikle OData uç noktaları için tasarlanmıştır ve bunlar varsayılan Web API yönlendirme sistemi değiştirir. (Çağırdığınızda değişiklik olur **MapODataRoute**.)

Bölüm 2'de, ben nasıl özel yönlendirme kuralları ekleneceğini göstermektedir. Şu anda yerleşik kuralları, tüm OData aralığı URI'ler kapsamaz, ancak ek durumlarında genişletebilirsiniz.

- [Yerleşik yönlendirme kuralları](#conventions)
- [Özel rota kuralları](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Yerleşik yönlendirme kuralları

Ben Web API OData rota kurallarını tanımlamak önce OData URI anlamak yararlıdır. Bir [OData URI'SİNİN](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) oluşur:

- Hizmet kök
- Kaynak yolu
- Sorgu Seçenekleri

![](odata-routing-conventions/_static/image1.png)

Yönlendirme için önemli bir parçası ise kaynak yoludur. Kaynak yolu parçalara bölünmüştür. Örneğin, `/Products(1)/Supplier` üç segmentler içerir:

- `Products` "Ürünler" adlı bir varlık kümesini ifade eder.
- `1` kümesinden tek bir varlık seçerek bir varlık anahtarı.
- `Supplier` ilgili varlık seçen bir gezinti özelliği var.

Bu nedenle bu yolu 1 ürün sağlayıcısıyla seçer.

> [!NOTE]
> OData yolu kesimleri her zaman URI segmentlere karşılık gelmez. Örneğin, "1", bir yol kesimi olarak kabul edilir.


**Denetleyici adı.** Denetleyici adı her zaman varlık kümesi kaynak yolu kökünde türetilir. Örneğin, kaynak yolu ise `/Products(1)/Supplier`, Web API görünen adlı bir denetleyici için `ProductsController`.

**Eylem adları.** Eylem adları, aşağıdaki tablolarda listelenenler gibi yol kesimleri artı varlık veri modeli (EDM) türetilmiştir. Bazı durumlarda, eylem adı için iki seçeneğiniz vardır. Örneğin, "Get" veya &quot;GetProducts&quot;.

**Varlıkları sorgulama**

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| /EntitySet Al | / Ürünleri | GetEntitySet veya Get | GetProducts |
| /EntitySet(KEY) Al | /Products(1) | GetEntityType veya Get | GetProduct |
| /EntitySet (anahtar) GET / atama | / Ürünleri (1) /Models.Book | GetEntityType veya Get | GetBook |

Daha fazla bilgi için [salt okunur OData uç noktası oluşturma](odata-v3/creating-an-odata-endpoint.md).

**Oluşturma, güncelleştirme ve varlıkları silme**

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| /EntitySet gönderin | / Ürünleri | PostEntityType veya sonrası | PostProduct |
| /EntitySet(KEY) yerleştirin | /Products(1) | PutEntityType veya Put | PutProduct |
| /EntitySet (anahtar) PUT / atama | / Ürünleri (1) /Models.Book | PutEntityType veya Put | PutBook |
| Düzeltme eki /entityset(key) | /Products(1) | PatchEntityType veya düzeltme eki | PatchProduct |
| Düzeltme eki /EntitySet (anahtar) / atama | / Ürünleri (1) /Models.Book | PatchEntityType veya düzeltme eki | PatchBook |
| /EntitySet(KEY) Sil | /Products(1) | DeleteEntityType ya da Delete | DeleteProduct |
| /EntitySet (anahtar) silme / atama | / Ürünleri (1) /Models.Book | DeleteEntityType ya da Delete | DeleteBook |

**Bir gezinme özelliği sorgulama**

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| GET /entityset (anahtar) / gezinti | / Ürünleri (1) / sağlayıcı | GetNavigationFromEntityType veya GetNavigation | GetSupplierFromProduct |
| /EntitySet (anahtar) / dönüştürme/Gezinti Al | / Ürünleri (1) /Models.Book/Author | GetNavigationFromEntityType veya GetNavigation | GetAuthorFromBook |

Daha fazla bilgi için [varlık ilişkilerinde çalışma](odata-v3/working-with-entity-relations.md).

**Oluşturma ve bağlantıları silme**

| İstek | Örnek URI | Eylem adı |
| --- | --- | --- |
| POST /entityset (anahtar) / $links/Gezinti | / Bağlantı/tedarikçi ürünleri (1) / $ | CreateLink |
| PUT /entityset (anahtar) / $links/Gezinti | / Bağlantı/tedarikçi ürünleri (1) / $ | CreateLink |
| DELETE /entityset (anahtar) / $links/Gezinti | / Bağlantı/tedarikçi ürünleri (1) / $ | DeleteLink |
| /EntitySet(KEY)/$Links/Navigation(relatedKey) Sil | /Products/(1)/$Links/Suppliers(1) | DeleteLink |

Daha fazla bilgi için [varlık ilişkilerinde çalışma](odata-v3/working-with-entity-relations.md).

**Özellikler**

*Web API 2 gerektirir*

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| GET /entityset (anahtar) / özelliği | / Ürünleri (1) / ad | GetPropertyFromEntityType veya GetProperty | GetNameFromProduct |
| /EntitySet (anahtar) / dönüştürme/özellik Al | / Ürünleri (1) /Models.Book/Author | GetPropertyFromEntityType veya GetProperty | GetTitleFromBook |

**Eylemler**

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| POST /entityset (anahtar) / işlem | / Ürünleri (1) / oranı | ActionNameOnEntityType veya ActionName | RateOnProduct |
| /EntitySet (anahtar) / dönüştürme/eylem | / Ürünleri (1) /Models.Book/CheckOut | ActionNameOnEntityType veya ActionName | CheckOutOnBook |

Daha fazla bilgi için [OData eylemleri](odata-v3/odata-actions.md).

**Yöntem imzaları**

Yöntem imzaları için bazı kurallar şunlardır:

- Yolun bir anahtar içeriyorsa, eylem adlı bir parametresi olmalıdır *anahtar*.
- Yolun bir gezinti özelliğine bir anahtar içeriyorsa, eylem adlı bir parametresi olmalıdır *relatedKey*.
- Süslemek *anahtarı* ve *relatedKey* parametrelerle **[FromODataUri]** parametresi.
- Varlık türünde bir parametre, POST ve PUT isteklerini yararlanın.
- PATCH isteklerinde ele türünde bir parametre **Delta&lt;T&gt;** burada *T* varlık türü olduğu.

Başvuru için her yerleşik OData rota kuralları örneği için yöntem imzaları gösteren bir örnek aşağıda verilmiştir.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Özel rota kuralları

Şu anda yerleşik kuralları, tüm olası OData URI kapsamaz. Yeni kuralları uygulayarak ekleyebileceğiniz **IODataRoutingConvention** arabirimi. Bu arabirim, iki yöntem vardır:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** denetleyicinin adını döndürür.
- **SelectAction** eylem adını döndürür.

Kural bu istek için geçerli değilse her iki yöntem için metot null döndürmelidir.

**ODataPath** parametresini ayrıştırılmış OData kaynak yolunu temsil eder. Bir listesini içeren **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)** örnekler, bir kaynak yolu, her bir kesim. **ODataPathSegment** bir Özet sınıf; her bir kesim türü türetilen bir sınıf tarafından temsil edilen **ODataPathSegment**.

**ODataPath.TemplatePath** birleştirme temsil eden bir dize özelliği olan tüm yol kesimleri. Örneğin, URI ise `/Products(1)/Supplier`, yol şablonu &quot;~/entityset/key/navigation&quot;. Segmentlerini doğrudan URI segmentlere karşılık gelmeyen dikkat edin. Örneğin, varlık anahtarı (1) kendi olarak temsil edilen **ODataPathSegment**.

Genellikle, uygulaması **IODataRoutingConvention** şunları yapar:

1. Bu kural için geçerli istek geçerliyse görmek için yol şablonu karşılaştırın. Bu geçerli değilse null döndür.
2. Kuralı geçerliyse, özelliklerini kullanmak **ODataPathSegment** denetleyici ve eylem adları türetmek için örnekleri.
3. Eylemler için eylem parametrelerini (genellikle varlık anahtarları) bağlanması için rota sözlüğünü herhangi bir değer ekleyin.

Belirli bir örneğe bakalım. Yerleşik yönlendirme kuralları, bir gezinti koleksiyon olarak dizinleme desteklemez. Diğer bir deyişle, bir URI'leri için hiçbir kural aşağıdaki gibi vardır:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Bu tür bir sorgu işleme özel rota kuralları örneği aşağıda verilmiştir.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notlar:

1. Ben öğesinden türetilen **EntitySetRoutingConvention**, çünkü **SelectController** yöntemi bu sınıfta bu yeni yönlendirme kuralı için uygun. Anlamı yoktur ihtiyacım yeniden uygulamak **SelectController**.
2. Kuralını yalnızca GET istekleri için ve yalnızca yol şablonu olduğunda geçerlidir &quot;~/entityset/key/navigation/key&quot;.
3. Eylem adı &quot;{EntityType} alma&quot;burada *{EntityType}* Gezinti koleksiyon türü. Örneğin, &quot;GetSupplier&quot;. İstediğiniz herhangi bir adlandırma kuralını kullanabilirsiniz &#8212; eşleşen denetleyici eylemleri emin olmanız yeterlidir.
4. Adlı iki parametre eylemde *anahtarı* ve *relatedKey*. (Bazı önceden tanımlanmış bir parametre adları listesi için bkz. [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Sonraki adım, yeni kural yönlendirme kuralları listesine eklemektir. Aşağıdaki kodda gösterildiği gibi bu yapılandırma sırasında gerçekleşir:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

İncelemek yararlı olan bazı diğer örnek yönlendirme kuralları şunlardır:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Ve Elbette gördüğünüz Web API'nin açık kaynaklı [kaynak kodu](http://aspnetwebstack.codeplex.com/) için yerleşik bir rota kuralları. Bu tanımlı **System.Web.Http.OData.Routing.Conventions** ad alanı.
