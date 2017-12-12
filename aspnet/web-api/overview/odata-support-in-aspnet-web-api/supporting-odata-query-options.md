---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: "ASP.NET Web API 2 OData sorgu seçeneklerini destekleyen | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>ASP.NET Web API 2 OData sorgu seçeneklerini destekleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

OData OData sorgusu değiştirmek için kullanılan parametreleri tanımlar. İstemci bu parametreler isteğin URI sorgu dizesinde gönderir. Örneğin, sonuçları sıralamak için bir istemci $orderby parametresini kullanır:

`http://localhost/Products?$orderby=Name`

Bu parametreler OData belirtimi çağırır *sorgu seçenekleri*. Herhangi bir Web API denetleyicisi için OData sorgu seçeneklerini projenizi &#8212;etkinleştirebilirsiniz; Denetleyici bir OData uç nokta olması gerekmez. Bu, herhangi bir Web API uygulama sıralama ve filtreleme gibi özellikleri eklemek için kolay bir yol sağlar.

Lütfen konusunu okuyun sorgu seçeneklerini etkinleştirmeden önce [OData güvenlik rehberi](odata-security-guidance.md).

- [OData sorgu seçeneklerini etkinleştirme](#enable)
- [Örnek sorgular](#examples)
- [Sunucu tabanlı disk belleği](#server-paging)
- [Sorgu seçeneklerini sınırlama](#limiting_query_options)
- [Sorgu seçeneklerini doğrudan çağırma](#ODataQueryOptions)
- [Sorgu doğrulama](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>OData sorgu seçeneklerini etkinleştirme

Web API aşağıdaki OData sorgu seçeneklerini destekler:

| Seçenek | Açıklama |
| --- | --- |
| $expand | İlgili varlıklar satır içi genişletir. |
| $filter | Boole bir koşula göre sonuçları filtreler. |
| $inlinecount | Sunucunun yanıt olarak eşleşen varlıkları toplam sayısı dahil söyler. (Sunucu tarafı disk belleği için kullanışlıdır.) |
| $orderby | Sonuçları sıralar. |
| $select | Yanıta eklenecek hangi özelliklerin seçer. |
| $skip | İlk n sonuçları atlar. |
| $top | Yalnızca ilk n sonuçları döndürür. |

OData sorgu seçeneklerini kullanmak için bunları açıkça etkinleştirmelisiniz. Bunları tüm uygulama için genel etkinleştirmek ya da bunları belirli denetleyicileri veya belirli eylemler için etkinleştirebilirsiniz.

OData sorgu seçeneklerinin genel etkinleştirmek için arama **EnableQuerySupport** üzerinde **HttpConfiguration** başlangıçta sınıfı:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport** yöntemi genel döndürür her denetleyici eylemi için sorgu seçeneklerini etkinleştirir bir **Iqueryable** türü. Sorgu seçenekleri tüm uygulama için etkinleştirilmiş istemiyorsanız, bunları belirli denetleyici eylemleri için ekleyerek etkinleştirebilirsiniz **[Queryable]** özniteliği eylem yöntemine.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Örnek sorgular

Bu bölümde OData sorgu Seçenekleri'ni kullanarak olası sorgu türleri gösterilmiştir. OData belgelerine sorgu seçeneklerini hakkında belirli Ayrıntılar için başvurmak [www.odata.org](http://www.odata.org/).

$ Hakkında bilgi için'ni genişletin ve bkz $select [$select kullanarak $genişletin ve ASP.NET Web API OData $value](using-select-expand-and-value.md).

**İstemci tabanlı disk belleği**

Büyük varlık kümeleri için istemci sonuç sayısını sınırlamak isteyebilirsiniz. Örneğin, bir istemci, 10 girişleri sonraki sonuç sayfasını almak için "İleri" bağlantıları ile her defasında gösterebilir. Bunu yapmak için istemci $top ve $skip seçenekleri kullanır.

`http://localhost/Products?$top=10&$skip=20`

Döndürülecek giriş sayısı üst sınırı $top seçeneği sunar ve atlamak için girdi sayısı $skip seçeneği sunar. Önceki örnekte girişleri ile 30 21 getirir.

**Filtreleme**

$Filter seçeneği Boole ifadesi uygulayarak filtre sonuçları bir istemci olanak sağlar. Filtre ifadeleri oldukça güçlü; mantıksal ve aritmetik işleçler, dize işlevleri ve tarih işlevleri içerirler.

| Kategori olan tüm ürünleri "Toys" eşit döndür. | `http://localhost/Products?$filter=Category`EQ 'Toys' |
| --- | --- |
| Fiyat 10'dan küçük olan tüm ürünleri döndür. | `http://localhost/Products?$filter=Price`lt 10 |
| Mantıksal işleçler: tüm ürünleri dönüş burada fiyat > 5 ve fiyat = < = 15. | `http://localhost/Products?$filter=Price`Ge 5 ve fiyat le 15 |
| Dize işlevleri: "zz" olan tüm ürünleri adını döndürür. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Date işlevleri: 2005'ten sonra ReleaseDate olan tüm ürünleri döndürür. | `http://localhost/Products?$filter=year(ReleaseDate)`gt 2005 |

**Sıralama**

Sonuçları sıralamak için $orderby filtresi kullanın.

| Fiyat göre sıralayın. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Azalan düzende (en yüksek, düşüğe) fiyat göre sıralayın. | `http://localhost/Products?$orderby=Price desc` |
| Kategoriye göre sıralayın ve ardından fiyat kategorilerde azalan sıralamada. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Sunucu tabanlı disk belleği

Veritabanınızı milyonlarca kayıt varsa, bunları göndermek istemediğiniz tüm bir yükte. Bunu önlemek için sunucunun tek bir yanıt olarak gönderir girişleri sayısını sınırlayabilirsiniz. Sunucu disk belleği etkinleştirmek için ayarlanmış **PageSize** özelliğinde **Queryable** özniteliği. Değer döndürmek için giriş sayısı üst sınırı ' dir.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Denetleyicinizi OData biçimi döndürürse, yanıt gövdesi verilerin bir sonraki sayfasına bir bağlantı içerir:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

İstemci, bir sonraki sayfada getirmek için bu bağlantıyı kullanabilirsiniz. İstemci sonuç kümesinde girişleri toplam sayısını öğrenmek için "inlinecount" $inlinecount sorgu seçeneği değerine sahip ayarlayabilirsiniz.

`http://localhost/Products?$inlinecount=allpages`

Değer "inlinecount" toplam sayı bu sayıyı yanıta eklenecek sunucunun bildirir:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Sonraki sayfa bağlantılar ve satır içi sayı OData biçimini gerektirir. OData sayısı ve bağlantıyı tutmak için yanıt gövdesinde özel alanlar tanımlar nedenidir.


OData olmayan biçimleri için sonraki sayfa bağlantılar ve satır içi sayısı, sorgu sonuçlarında kaydırma tarafından desteklemek hala mümkündür bir **PageResult&lt;T&gt;**  nesnesi. Ancak, biraz daha fazla kod gerektirir. Aşağıda bir örnek verilmiştir:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

JSON yanıt örnek aşağıda verilmiştir:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Sorgu seçeneklerini sınırlama

Sorgu seçeneklerini istemci çok sayıda sunucu üzerinde çalışan sorgu üzerinde denetim sağlar. Bazı durumlarda, güvenlik veya performans nedenleriyle kullanılabilir seçenekler sınırlamak isteyebilirsiniz. **[Queryable]** özniteliği bazı yerleşik özelliklerinde bu. İşte bazı örnekler.

Yalnızca $skip ve $top, disk belleği ve hiçbir şey desteklemek için izin ver:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Veritabanında oluşturulmuyor özellikleri sıralama önlemek belirli özellik yalnızca tarafından sıralanmasına izin ver:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

"Eq" mantıksal işlevi ancak diğer mantıksal işlevleri izin ver:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Hiçbir aritmetik işleçler izin verme:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Oluşturarak seçenekleri genel kısıtlayabilirsiniz bir **QueryableAttribute** örneği ve kendisine geçirme **EnableQuerySupport** işlevi:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Sorgu seçeneklerini doğrudan çağırma

Kullanmak yerine **[Queryable]** özniteliğinin, sorgu seçeneklerini doğrudan denetleyicinizi çağırabilirsiniz. Bunu yapmak için add bir **ODataQueryOptions** denetleyicisi yöntemine parametre. Bu durumda, gerekmeyen **[Queryable]** özniteliği.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web API doldurur **ODataQueryOptions** URI'den sorgu dizesi. Geçişi sorgu uygulamak için bir **Iqueryable** için **ApplyTo** yöntemi. Başka bir yöntem **Iqueryable**.

Sahip değilse Gelişmiş senaryolar için bir **Iqueryable** sorgu sağlayıcısı inceleyin **ODataQueryOptions** ve başka bir forma sorgu seçeneklerini çevir. (Örneğin, RaghuRam Nadiminti'nın blog gönderisi bkz [çevirme OData sorgularını hql'e](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), ayrıca içeren bir [örnek](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Sorgu doğrulama

**[Queryable]** özniteliği yürütmeden önce sorgu doğrular. Doğrulama adımı başlığında gerçekleştirilen **QueryableAttribute.ValidateQuery** yöntemi. Doğrulama işlemini de özelleştirebilirsiniz.

Ayrıca bkz. [OData güvenlik rehberi](odata-security-guidance.md).

Geçersiz kılma sınıfları almak için diğer bir deyişle, Doğrulayıcı birini ilk olarak, tanımlanan **Web.Http.OData.Query.Validators** ad alanı. Örneğin, aşağıdaki Doğrulayıcı sınıfını $orderby seçeneği için 'desc' seçeneği devre dışı bırakır.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Bir alt **[Queryable]** geçersiz kılmak için öznitelik **ValidateQuery** yöntemi.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Ardından, özel öznitelik ya da genel olarak ayarlamak veya denetleyicisi başına:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Kullanıyorsanız **ODataQueryOptions** doğrudan Doğrulayıcı seçenekleri ayarlayın:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
