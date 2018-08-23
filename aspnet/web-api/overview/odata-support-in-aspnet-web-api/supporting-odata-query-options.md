---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: ASP.NET Web API 2 OData sorgu seçeneklerini destekleme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 8745183125c9dd1dcc7cb0e146367a893bdb0170
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755153"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a>ASP.NET Web API 2 OData sorgu seçeneklerini destekleme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

OData OData sorgu değiştirmek için kullanılan parametreleri tanımlar. İstemci, bu parametreleri istek URI sorgu dizesinde gönderir. Örneğin, sonuçları sıralamak için bir istemci $orderby parametresini kullanır:

`http://localhost/Products?$orderby=Name`

Bu parametreleri OData belirtiminden çağırır *sorgu seçenekleri*. Projenizdeki herhangi bir Web API denetleyicisi için OData sorgu seçenekleri etkinleştirebilirsiniz &#8212; denetleyici bir OData uç nokta olması gerekmez. Bu, filtreleme ve sıralama için herhangi bir Web API'sini uygulama gibi özellikleri eklemek için kullanışlı bir yol sağlar.

Lütfen bu makaleyi okuyun, sorgu seçeneklerini etkinleştirmeden önce [OData güvenlik rehberi](odata-security-guidance.md).

- [OData sorgu seçenekleri etkinleştirme](#enable)
- [Örnek sorgular](#examples)
- [Sunucu tabanlı disk belleği](#server-paging)
- [Sorgu seçenekleri sınırlama](#limiting_query_options)
- [Sorgu seçenekleri doğrudan çağırma](#ODataQueryOptions)
- [Sorgu doğrulama](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a>OData sorgu seçenekleri etkinleştirme

Web API'si, aşağıdaki OData sorgu seçeneklerini destekler:

| Seçenek | Açıklama |
| --- | --- |
| $expand | İlgili varlıklar satır içi genişletir. |
| $filter | Sonuçları bir Boolean koşulu temel alarak filtreler. |
| $inlinecount | Sunucu yanıt olarak eşleşen varlıkların toplam sayısı dahil etmek anlatır. (Sunucu tarafı disk belleği için kullanışlıdır.) |
| $orderby | Sonuçları sıralar. |
| $select | Yanıta dahil etmek için hangi özelliklerin seçer. |
| $skip | İlk n sonuç atlar. |
| $top | Yalnızca ilk n sonuç döndürür. |

OData sorgu seçeneklerini kullanmak için bunları açıkça etkinleştirmeniz gerekir. Bunları etkinleştirmek için uygulamanın tamamını veya belirli denetleyicileri veya belirli eylemler için etkinleştirin.

OData sorgu seçeneklerini etkinleştirmek istiyorsanız arama **EnableQuerySupport** üzerinde **HttpConfiguration** başlangıçta sınıfı:

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

**EnableQuerySupport** yöntem sorgu seçenekleri için genel olarak döndüren bir denetleyici eylemi sağlar bir **Iqueryable** türü. Uygulamanın tamamı için etkin sorgu seçenekleri istemiyorsanız, bunları belirli bir denetleyici eylemleri için ekleyerek etkinleştirebilirsiniz **[Queryable]** özniteliği eylem yöntemine.

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a>Örnek sorgular

Bu bölümde, OData sorgu seçeneklerini kullanarak olası sorgu türleri gösterilmektedir. Sorgu seçenekleri hakkındaki özel ayrıntılar için OData belgelerine bakın [www.odata.org](http://www.odata.org/).

$ Hakkında bilgi için genişletin ve bkz $select, [$select kullanarak $expand ve ASP.NET Web API OData $value](using-select-expand-and-value.md).

**İstemci tabanlı disk belleği**

Büyük varlık kümeleri için istemci sonuçları sayısını sınırlamak isteyebilirsiniz. Örneğin, bir istemci, sonraki sonuç sayfasını almak için "İleri" bağlantıları ile bir kerede 10 girişleri gösterebilir. Bunu yapmak için istemci $top ve $skip seçeneklerini kullanır.

`http://localhost/Products?$top=10&$skip=20`

En fazla girdi sayısı bu değeri döndürmek için $top seçeneği sunar ve atlanacak girdi sayısı $skip seçeneği sunar. Önceki örnekte girişleri 21 ile 30 getirir.

**Filtreleme**

$Filter seçeneği bir Boole ifadesi uygulayarak, sonuçları filtrelemek için bir istemci olanak tanır. Filtre ifadeleri oldukça güçlü; Bunlar, mantıksal ve aritmetik işleçler, dize işlevleri ve tarih işlevleri içerir.

| Kategorisindeki tüm ürünler "Toys" eşit döndürür. | `http://localhost/Products?$filter=Category` EQ 'Toys' |
| --- | --- |
| Fiyat 10'dan küçük olan tüm ürünleri döndürür. | `http://localhost/Products?$filter=Price` lt 10 |
| Mantıksal işleçler: tüm ürünleri döndürmek burada fiyat > 5 ve fiyat = < = 15. | `http://localhost/Products?$filter=Price` Ge 5 ve fiyat le 15 |
| Dize işlevleri: "zz" olan tüm ürünleri adını döndürür. | `http://localhost/Products?$filter=substringof('zz',Name)` |
| Tarih işlevleri: 2005'ten sonra tüm ürünleriyle ReleaseDate döndürür. | `http://localhost/Products?$filter=year(ReleaseDate)` gt 2005 |

**Sıralama**

Sonuçları sıralamak için $orderby filtreyi kullanın.

| Fiyat göre sıralayın. | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| Fiyat (yüksekten en düşüğe) azalan düzende sırala. | `http://localhost/Products?$orderby=Price desc` |
| Kategoriye göre sıralayın ve ardından fiyat kategorilerde azalan düzende sırala. | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a>Sunucu tabanlı disk belleği

Veritabanınızı milyonlarca kayıt varsa, bunları göndermek istemiyorsanız bir yük içinde tüm. Bunu önlemek için sunucu, tek bir yanıtta gönderen giriş sayısını sınırlayabilirsiniz. Sunucu disk belleği'ni etkinleştirmek için **PageSize** özelliğinde **Queryable** özniteliği. En fazla girdi sayısı bu değeri döndürülecek değerdir.

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

Denetleyicinizi OData biçimi döndürürse, yanıt gövdesi veri sonraki sayfaya bir bağlantı içerir:

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

İstemci, bir sonraki sayfayı almak için bu bağlantıyı kullanabilirsiniz. İstemci sonuç kümesinde girişleri toplam sayısını öğrenmek $inlinecount sorgu seçeneği değerine sahip "inlinecount" ayarlayabilirsiniz.

`http://localhost/Products?$inlinecount=allpages`

Değer "inlinecount" toplam sayı bu sayıyı yanıta eklenecek sunucunun bildirir:

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> Sonraki sayfa bağlantıları hem de satır içi sayı OData biçimini gerektirir. OData sayısı ve bağlantı tutacak yanıt gövdesi içinde özel alanlar tanımlar nedenidir.


OData olmayan biçimleri için sonraki sayfa ve satır içi bağlantıları sayısı, sorgu sonuçlarında sarmalama tarafından desteklemek için tanımlanabilir bir **PageResult&lt;T&gt;**  nesne. Ancak, biraz daha fazla kod gerektirir. Aşağıda bir örnek verilmiştir:

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

Bir örnek JSON yanıtı aşağıda verilmiştir:

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a>Sorgu seçenekleri sınırlama

Sorgu seçenekleri istemci çok fazla sunucuda çalıştırılan sorguyu üzerinde denetim sağlar. Bazı durumlarda, güvenlik veya performans nedenleriyle kullanılabilir seçenekler sınırlamak isteyebilirsiniz. **[Queryable]** özniteliği bazı yerleşik özelliklerinde bu. Bazı örnekleri aşağıda verilmiştir.

Yalnızca $skip ve $top, sayfalama ve başka hiçbir şey desteklemek için izin ver:

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

Veritabanında oluşturulmuyor özellikleri sıralama önlemek belirli özellikleri yalnızca tarafından sıralanmasına izin vermek:

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

"Eq" mantıksal işlevi ancak diğer mantıksal işlevler izin ver:

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

Tüm aritmetik işleçlere izin verme:

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

Oluşturarak, genel bir seçenekler kısıtlayabilirsiniz bir **Queryableattribute'taki** örneği ve aktarması **EnableQuerySupport** işlevi:

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a>Sorgu seçenekleri doğrudan çağırma

Yerine **[Queryable]** özniteliği doğrudan denetleyicinizin sorgu seçeneklerini çağırabilirsiniz. Bunu yapmak için bir **ODataQueryOptions** denetleyici yöntemin parametre. Bu durumda, ihtiyacınız olmayan **[Queryable]** özniteliği.

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

Web API doldurur **ODataQueryOptions** URI'SİNDEN sorgu dizesi. Sorgu uygulamak için başarılı bir **Iqueryable** için **ApplyTo** yöntemi. Başka bir yöntem döndürür **Iqueryable**.

Yoksa, Gelişmiş senaryolar için bir **Iqueryable** sorgu sağlayıcısı inceleyebilirsiniz **ODataQueryOptions** ve başka bir forma sorgu seçenekleri. (Örneğin, RaghuRam Nadiminti'nın blog yayınına bakın [çevirme OData sorgularını hql'e](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), da içeren bir [örnek](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)

<a id="query-validation"></a>
## <a name="query-validation"></a>Sorgu doğrulama

**[Queryable]** özniteliği sorgu yürütülmeden önce doğrular. Doğrulama adımını içinde gerçekleştirilen **QueryableAttribute.ValidateQuery** yöntemi. Doğrulama işlemi ayrıca özelleştirebilirsiniz.

Ayrıca bkz: [OData güvenlik rehberi](odata-security-guidance.md).

Geçersiz kılma sınıfları almak için diğer bir deyişle, Doğrulayıcı birini ilk olarak tanımlanan **Web.Http.OData.Query.Validators** ad alanı. Örneğin, aşağıdaki Doğrulayıcı sınıfını $orderby seçeneği için 'desc' seçeneğini devre dışı bırakır.

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

Alt **[Queryable]** geçersiz kılmak için öznitelik **ValidateQuery** yöntemi.

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

Ardından, özel özniteliği ayarlayın ya da genel olarak veya denetleyici başına:

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

Kullanıyorsanız **ODataQueryOptions** doğrudan Doğrulayıcı seçenekleri ayarlayın:

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
