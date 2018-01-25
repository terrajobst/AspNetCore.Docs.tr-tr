---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
title: "Yönlendirme kuralları ASP.NET Web API 2 Odata | Microsoft Docs"
author: MikeWasson
description: "Bu makalede, Web API OData uç noktaları için kullandığı yönlendirme kuralları açıklanır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/31/2013
ms.topic: article
ms.assetid: adbc175a-14eb-4ab2-a441-d056ffa8266f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-routing-conventions
msc.type: authoredcontent
ms.openlocfilehash: 0ab99dd443040b90ffefd2f5b9261a63b91e9463
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="routing-conventions-in-aspnet-web-api-2-odata"></a>Yönlendirme kuralları ASP.NET Web API 2 Odata
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Bu makalede, Web API OData uç noktaları için kullandığı yönlendirme kuralları açıklanır.


Web API OData isteği aldığında, bir denetleyici adı ve bir eylem adı istek eşlenir. Eşleme HTTP yöntemi ve URI dayanır. Örneğin, `GET /odata/Products(1)` eşlendiği `ProductsController.GetProduct`.

Bölümünde bu makalede 1, ı yerleşik OData rota kuralları açıklar. Bu kuralları özellikle OData uç noktaları için tasarlanmıştır ve varsayılan Web API yönlendirme sistem değiştirin. (Çağırdığınızda değiştirme olur **MapODataRoute**.)

Bölüm 2, t özel yönlendirme kuralları ekleme gösterir. Şu anda yerleşik kuralları tüm OData aralığı URI'ler ele alınmamaktadır, ancak bunları ek durumlarında genişletebilirsiniz.

- [Yerleşik yönlendirme kuralları](#conventions)
- [Özel rota kuralları](#custom)

<a id="conventions"></a>
## <a name="built-in-routing-conventions"></a>Yerleşik yönlendirme kuralları

Web API OData rota kurallarını tanımlamak önce OData URI anlamaya yardımcı olur. Bir [OData URI](http://www.odata.org/documentation/odata-v3-documentation/url-conventions/) oluşur:

- Hizmet kök
- Kaynak yolu
- Sorgu Seçenekleri

![](odata-routing-conventions/_static/image1.png)

Yönlendirme için önemli bir bölümü kaynak yoludur. Kaynak yolu parçalara bölünür. Örneğin, `/Products(1)/Supplier` üç kesimleri vardır:

- `Products`"Ürünler" adlı bir varlık kümesine başvuruyor.
- `1`tek bir varlık kümesinden seçerek bir varlık anahtarı.
- `Supplier`ilgili varlık seçen bir gezinti özelliğidir.

Bu nedenle bu yolu ürün 1 tedarikçi seçer.

> [!NOTE]
> OData yolu kesimleri her zaman URI segmentlere karşılık gelmemelidir. Örneğin, "1" yol kesimi olarak kabul edilir.


**Denetleyici adı.** Denetleyici adı, her zaman varlık kaynak yolu kökünde kümesi türetilir. Örneğin, kaynak yolu ise `/Products(1)/Supplier`, Web API arar adlı bir denetleyici için `ProductsController`.

**Eylem adları.** Eylem adları, aşağıdaki tablolarda listelenenler gibi yol kesimleri artı varlık veri modeli (EDM) türetilir. Bazı durumlarda, eylem adı için iki seçeneğiniz vardır. Örneğin, "Get" veya &quot;GetProducts&quot;.

**Varlıkları sorgulama**

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| /EntitySet Al | / Ürünleri | GetEntitySet veya Al | GetProducts |
| /EntitySet(KEY) Al | /Products(1) | GetEntityType veya Al | GetProduct |
| /EntitySet (anahtar) alma / atama | / Ürünler (1) /Models.Book | GetEntityType veya Al | GetBook |

Daha fazla bilgi için bkz: [bir salt okunur OData uç noktası oluşturma](odata-v3/creating-an-odata-endpoint.md).

**Oluşturma, güncelleştirme ve varlıkları silme**

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| /EntitySet sonrası | / Ürünleri | PostEntityType veya sonrası | PostProduct |
| /EntitySet(KEY) YERLEŞTİRME | /Products(1) | PutEntityType veya Put | PutProduct |
| /EntitySet (anahtar) PUT / atama | / Ürünler (1) /Models.Book | PutEntityType veya Put | PutBook |
| Düzeltme eki /entityset(key) | /Products(1) | PatchEntityType ya da düzeltme eki | PatchProduct |
| /EntitySet (anahtar) düzeltme eki / atama | / Ürünler (1) /Models.Book | PatchEntityType ya da düzeltme eki | PatchBook |
| /EntitySet(KEY) Sil | /Products(1) | DeleteEntityType veya silme | DeleteProduct |
| /EntitySet (anahtar) silme / atama | / Ürünler (1) /Models.Book | DeleteEntityType veya silme | DeleteBook |

**Bir gezinme özelliği sorgulama**

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| GET /entityset (anahtar) / gezinme | / Ürünler (1) / tedarikçi | GetNavigationFromEntityType veya GetNavigation | GetSupplierFromProduct |
| /EntitySet (anahtar) / dönüştürme/Gezinti Al | / Ürünler (1) /Models.Book/Author | GetNavigationFromEntityType veya GetNavigation | GetAuthorFromBook |

Daha fazla bilgi için bkz: [ile Varlık İlişkileriyle Çalışma](odata-v3/working-with-entity-relations.md).

**Oluşturma ve bağlantıları silme**

| İstek | Örnek URI | Eylem adı |
| --- | --- | --- |
| POST /entityset (anahtar) / $links/gezinme | / Ürünler (1) / $ bağlantılar/tedarikçi | CreateLink |
| PUT /entityset (anahtar) / $links/gezinme | / Ürünler (1) / $ bağlantılar/tedarikçi | CreateLink |
| DELETE /entityset (anahtar) / $links/gezinme | / Ürünler (1) / $ bağlantılar/tedarikçi | DeleteLink |
| /EntitySet(KEY)/$Links/Navigation(relatedKey) Sil | /Products/(1)/$Links/Suppliers(1) | DeleteLink |

Daha fazla bilgi için bkz: [ile Varlık İlişkileriyle Çalışma](odata-v3/working-with-entity-relations.md).

**Özellikler**

*Web API 2 gerektirir*

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| GET /entityset (anahtar) / özelliği | / Ürünler (1) / adı | GetPropertyFromEntityType veya GetProperty | GetNameFromProduct |
| /EntitySet (anahtar) / dönüştürme/özelliği alma | / Ürünler (1) /Models.Book/Author | GetPropertyFromEntityType veya GetProperty | GetTitleFromBook |

**Eylemler**

| İstek | Örnek URI | Eylem adı | Örnek eylem |
| --- | --- | --- | --- |
| POST /entityset (anahtar) / eylem | / Ürünler (1) / oranı | ActionNameOnEntityType veya EylemAdı | RateOnProduct |
| /EntitySet (anahtar) / dönüştürme/eylem sonrası | / Ürünler (1) /Models.Book/CheckOut | ActionNameOnEntityType veya EylemAdı | CheckOutOnBook |

Daha fazla bilgi için bkz: [OData eylemlerinin](odata-v3/odata-actions.md).

**Yöntem imzaları**

Yöntem imzaları için bazı kurallar şunlardır:

- Yolun bir anahtar içeriyorsa, eylem adlı bir parametre olmalıdır *anahtar*.
- Yolun bir gezinti özelliğine bir anahtar içeriyorsa, eylem adlı bir parametre olmalıdır *relatedKey*.
- İşaretleme *anahtar* ve *relatedKey* parametrelerle **[FromODataUri]** parametresi.
- POST ve PUT isteklerini varlık türünde bir parametre alın.
- PATCH isteklerinde ele türünde bir parametre **Delta&lt;T&gt;**, burada *T* varlık türüdür.

Başvuru için burada her yerleşik OData yönlendirme kuralı yöntemi imzaları gösteren bir örnek verilmiştir.

[!code-csharp[Main](odata-routing-conventions/samples/sample1.cs)]

<a id="custom"></a>
## <a name="custom-routing-conventions"></a>Özel rota kuralları

Şu anda yerleşik kuralları tüm olası OData URI ele alınmamaktadır. Yeni kuralları uygulayarak ekleyebileceğiniz **IODataRoutingConvention** arabirimi. Bu arabirim iki yöntemi vardır:

[!code-csharp[Main](odata-routing-conventions/samples/sample2.cs)]

- **SelectController** denetleyicinin adını döndürür.
- **SelectAction** eylem adını döndürür.

Kuralı bu istek için geçerli değilse her iki yöntem için yöntemi null değerini döndürmelidir.

**ODataPath** parametresi ayrıştırılmış OData kaynak yolunu temsil eder. Bir listesini içeren  **[ODataPathSegment](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatapathsegment.aspx)**  örnekler, her segment kaynak yolu için bir tane. **ODataPathSegment** bir Özet sınıf; her bölüm türü türeyen bir sınıf tarafından temsil edilen **ODataPathSegment**.

**ODataPath.TemplatePath** özelliktir birleştirme temsil eden bir dize tüm yol kesimleri. Örneğin, URI ise `/Products(1)/Supplier`, yol şablonu &quot;~/entityset/key/navigation&quot;. Segment doğrudan URI segmentlere karşılık verme dikkat edin. Örneğin, varlık anahtarı (1) kendi olarak temsil edilir **ODataPathSegment**.

Genellikle, uygulaması **IODataRoutingConvention** şunları yapar:

1. Bu kural için geçerli istek geçerliyse görmek için yol şablonu karşılaştırın. Bu geçerli değilse null döndürür.
2. Kuralı geçerliyse, özelliklerini kullanmak **ODataPathSegment** denetleyici ve eylem adları çıkarmaya örnekleri.
3. Eylemler için eylem parametrelerini (genellikle varlık anahtarları) bağlanması için rota sözlüğünü tüm değerleri ekleyin.

Belirli bir örneğe bakalım. Yerleşik yönlendirme kuralları Gezinti koleksiyon olarak dizinleme desteklemez. Diğer bir deyişle, aşağıdaki gibi URI'ler için hiçbir kural vardır:

[!code-javascript[Main](odata-routing-conventions/samples/sample3.js)]

Aşağıda, bu tür sorgu işlemek için özel bir yönlendirme kuralı verilmiştir.

[!code-csharp[Main](odata-routing-conventions/samples/sample4.cs)]

Notlar:

1. I öğesinden türetilen **EntitySetRoutingConvention**, çünkü **SelectController** bu sınıftaki yöntemi için bu yeni yönlendirme kuralı uygundur. Anlamına yok ihtiyacım yeniden uygulamak **SelectController**.
2. Kuralı yalnızca GET isteklerini ve yalnızca yol şablonu olduğunda geçerlidir &quot;~/entityset/key/navigation/key&quot;.
3. Eylem adı &quot;{EntityType} alma&quot;, burada *{EntityType}* Gezinti koleksiyon türü. Örneğin, &quot;GetSupplier&quot;. İstediğiniz herhangi bir adlandırma kuralını &#8212;kullanabilirsiniz; yalnızca eşleşen denetleyici eylemleri emin olun.
4. Adlı iki parametre eylemde *anahtar* ve *relatedKey*. (Bazı önceden tanımlanmış parametre adları listesi için bkz: [ODataRouteConstants](https://msdn.microsoft.com/library/system.web.http.odata.routing.odatarouteconstants.aspx).)

Sonraki adım, yönlendirme kuralları listesine yeni kuralı eklemektir. Bu yapılandırması sırasında aşağıdaki kodda gösterildiği gibi olur:

[!code-csharp[Main](odata-routing-conventions/samples/sample5.cs)]

İncelemek faydalı olan bazı diğer örnek yönlendirme kuralları şunlardır:

- [CompositeKeyRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataCompositeKeySample/ODataCompositeKeySample/Extensions/CompositeKeyRoutingConvention.cs)
- [CustomNavigationRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataServiceSample/ODataService/Extensions/CustomNavigationRoutingConvention.cs)
- [NonBindableActionRoutingConvention](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataActionsSample/ODataActionsSample/NonBindableActionRoutingConvention.cs)
- [ODataVersionRouteConstraint](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/ODataVersioningSample/ODataVersioningSample/Extensions/ODataVersionRouteConstraint.cs)

Ve Elbette gördüğünüz şekilde açık kaynaklı Web API kendisini [kaynak kodu](http://aspnetwebstack.codeplex.com/) yerleşik yönlendirme kuralları için. Bunlar tanımlanan **System.Web.Http.OData.Routing.Conventions** ad alanı.
