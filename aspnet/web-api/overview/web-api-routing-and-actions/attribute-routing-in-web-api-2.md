---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Öznitelik ASP.NET Web API 2 yönlendirme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: d16dcc618bf6c60714179601db14f4dd2a9e41ce
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912158"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2'de öznitelik yönlendirme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

*Yönlendirme* Web API'sini bir eylem için bir URI nasıl eşleştirir. Web API 2 destekleyen yeni bir tür adı yönlendirme *öznitelik yönlendirme*. Adından da anlaşılacağı gibi öznitelik yönlendirme yollarını tanımlamak için öznitelikleri kullanır. Öznitelik yönlendirme, web API'nize bir URI'leri üzerinde daha fazla denetim sağlar. Örneğin, kaynak hiyerarşileri açıklayan bir URI'leri kolayca oluşturabilirsiniz.

Adlı kural tabanlı yönlendirme, önceki stil yönlendirme, yine de tam olarak desteklenir. Aslında, aynı projede iki tekniği birleştirebilirsiniz.

Bu konu, öznitelik yönlendirme etkinleştirme gösterir ve öznitelik yönlendirme için çeşitli seçenekler açıklanmaktadır. Öznitelik yönlendirme kullanan bir uçtan uca öğretici için bkz [Web API 2'de öznitelik yönlendirme ile REST API oluşturma](create-a-rest-api-with-attribute-routing.md).

## <a name="prerequisites"></a>Önkoşullar

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional veya Enterprise edition

Alternatif olarak, gerekli paketleri yüklemek için NuGet paket yöneticisini kullanın. Gelen **Araçları** Visual Studio'da seçim menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Neden yönlendirme özniteliği?

Web API kullanılan ilk sürümü *kural tabanlı* yönlendirme. Yönlendirme, bu tür bir tane tanımlayacaksınız veya temel olarak daha fazla rota şablonlarının dizeleri parametreli. Framework, bir istek aldığında, rota şablonu karşı URI ile eşleşir. (Kural tabanlı yönlendirme hakkında daha fazla bilgi için bkz. [ASP.NET Web API'de yönlendirme](routing-in-aspnet-web-api.md).

Kural tabanlı yönlendirme bir avantajı, tek bir yerde tanımlanmış şablonları ve yönlendirme kurallarını tüm denetleyicileri arasında tutarlı bir şekilde uygulandığından olmasıdır. Ne yazık ki, kural tabanlı yönlendirme RESTful API'leri ortak olan belirli bir URI düzenleri desteği zorlaştırır. Genellikle alt kaynakları gibi kaynakları içeren: imajlarını siparişler, filmler aktörler sahip, books Yazar sahip ve VS. Bu ilişkileri yansıtan bir URI'leri oluşturmak için doğal bir:

`/customers/1/orders`

Bu tür bir URI, kural tabanlı yönlendirme kullanarak oluşturmak zordur. Bu yapılabilir olsa da, sonuçları da birçok denetleyicileri veya kaynak türleri varsa ölçeklendirilemediği.

Öznitelik yönlendirme ile basit bir iş bir rota için bu URI tanımlar. Denetleyici eylemi için yalnızca bir özniteliği ekleyin:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Öznitelik yönlendirme yapar kolay bazı diğer desenleri aşağıda verilmiştir.

**API sürümü oluşturma**

Bu örnekte, "/ v1/API/ürünleri" olacaktır farklı bir denetleyicisine yönlendirilen "/ v2/API/ürünleri".

`/api/v1/products`
`/api/v2/products`

**Aşırı yüklenmiş URI segmentleri**

Bu örnekte, "1" sipariş numarası olduğu halde "bekliyor" bir koleksiyona eşler.

`/orders/1`
`/orders/pending`

**Birden çok parametre türleri**

Bu örnekte, "1" sipariş numarası olduğu halde "06/2013/16" bir tarihi belirtir.

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Öznitelik yönlendirmeyi etkinleştirme

Öznitelik yönlendirmeyi etkinleştirmek için çağrı **MapHttpAttributeRoutes** yapılandırma sırasında. Bu genişletme yöntemi tanımlanan **System.Web.Http.HttpConfigurationExtensions** sınıfı.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

İle öznitelik yönlendirme birleştirilebilir [kural tabanlı](routing-in-aspnet-web-api.md) yönlendirme. Kural tabanlı rotaları tanımlamak için çağrı **MapHttpRoute** yöntemi.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Web API'sini yapılandırma hakkında daha fazla bilgi için bkz. [ASP.NET Web API 2 yapılandırma](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Not: Web API 1 ' geçiş

Web API 2 önce Web API proje şablonları aşağıdakine benzer bir kod oluşturuldu:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Bu kod, öznitelik yönlendirme etkinse, bir özel durum oluşturur. Öznitelik yönlendirmeyi kullanmak için mevcut bir Web API projesini yükseltirseniz, bu yapılandırma kodu aşağıdakine güncelleştirmeyi unutmayın:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Daha fazla bilgi için [barındırma ASP.NET ile Web API'sini yapılandırma](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Rota özniteliklerini ekleme

Bir rotanın bir özniteliği kullanılarak tanımlanan bir örnek aşağıda verilmiştir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Dize &quot;müşteriler / {CustomerID} / orders&quot; rota için URI şablonudur. Web API'si, ' % s'istek URI'SİNDEN şablon eşleştirmeyi dener. Bu örnekte, "Müşteri" ve "Siparişler" değişmez değer segmentler ve "{CustomerID}" bir değişken bir parametredir. Bu şablon aşağıdaki bir URI'leri eşleşir:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Kullanarak eşleşen kısıtlayabilirsiniz [kısıtlamaları](#constraints), bu konunun ilerleyen bölümlerinde açıklanmıştır.

Dikkat &quot;{CustomerID}&quot; rota şablonu parametre adı ile eşleşen *CustomerID* yöntem parametresi. Web API denetleyici eylemini çalıştırdığında, rota parametrelerine bağlamak çalışır. Örneğin, URI ise `http://example.com/customers/1/orders`, Web API'sini çalışır değerini "1" bağlamak *CustomerID* eylem parametresi.

Bir URI şablonu, birkaç parametre sahip olabilir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Kural tabanlı yönlendirme, yol özniteliğine sahip olmayan herhangi bir denetleyici yöntemin kullanın. Böylece, her iki tür aynı projede yönlendirme birleştirebilirsiniz.

## <a name="http-methods"></a>HTTP yöntemleri

Web API'si işlemleri (GET, POST, vb.) isteğin HTTP yöntemine dayalı seçilmesini sağlar. Varsayılan olarak, Web API denetleyicisi yöntem adı başlangıcını harf olarak eşleşen arar. Örneğin, adında bir denetleyici yöntemi `PutCustomers` eşleşen bir HTTP PUT İsteği.

Aşağıdaki özniteliklerden herhangi bir yöntemle tasarlayarak bu kuralı kılabilirsiniz:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Aşağıdaki örnek, HTTP POST istekleri CreateBook yöntemi eşleştirir.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Standart olmayan yöntemler de dahil olmak üzere tüm diğer HTTP yöntemleri kullanmak için **AcceptVerbs** özniteliği HTTP yöntemlerinin listesini alır.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Yol ön ekleri

Genellikle, tümü aynı önekiyle başlayan bir denetleyici yolları. Örneğin:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Tüm bir denetleyici için ortak bir öneki kullanarak ayarlayabilirsiniz **[routeprefix öğesi]** özniteliği:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Bir tilde (~) üzerinde method özniteliği rota öneki geçersiz kılmak için kullanın:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Rota öneki parametreleri şunları içerebilir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Rota kısıtlamaları

Rota kısıtlamalarını nasıl yol şablonunda parametreler olarak eşleştirilir kısıtlamanıza olanak tanır. Genel söz dizimi &quot;{parametresi: kısıtlaması}&quot;. Örneğin:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Burada, ilk yolun yalnızca, seçili olur &quot;kimliği&quot; URI parçası olan bir tamsayı. Aksi takdirde, ikinci rota seçilir.

Aşağıdaki tablo desteklenen kısıtlamaları listeler.

| Kısıtlama | Açıklama | Örnek |
| --- | --- | --- |
| alfa | Eşleşme büyük veya küçük harf Latin alfabesi karakterler (a-z, A-Z) | {alpha: x} |
| bool | Bir Boolean değeri eşler. | {x: bool} |
| datetime | Eşleşen bir **DateTime** değeri. | {x:datetime} |
| decimal | Ondalık bir değeri eşler. | {x:decimal} |
| çift | Bir 64-bit kayan nokta değeri eşler. | {x:double} |
| float | Bir 32-bit kayan nokta değeri eşler. | x: kayan {} |
| GUID | Bir GUID değeri eşler. | {x: GUID} |
| int | Bir 32-bit tamsayı değeri eşler. | {x:int} |
| length | Belirtilen uzunlukta veya uzunluklarının belirtilen bir aralık içindeki bir dizeyle eşleşir. | {x: length(6)} {x: length(1,20)} |
| long | Bir 64-bit tamsayı değeri eşler. | {x: uzun} |
| max | Bir tamsayı en yüksek bir değerle eşleşir. | {x:max(10)} |
| MaxLength | En fazla bir dizeyle eşleşir. | {x:maxlength(10)} |
| min | Bir tamsayı en az bir değerle eşleşir. | {x: min(10)} |
| MinLength | Minimum uzunluk bir dizeyle eşleşir. | {x: minlength(10)} |
| aralık | Tamsayı değerleri aralığı içinde eşleşir. | {x: range(10,50)} |
| Normal ifade | Normal bir ifadeyle eşleşiyor. | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

Bildirimi, bazı kısıtlamaları gibi &quot;min&quot;, parantez içine bağımsız değişken almaz. Virgül ile ayrılmış bir parametre, birden çok kısıtlaması uygulayabilirsiniz.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Özel rota kısıtlamaları

Özel rota kısıtlamalarını uygulayarak oluşturabilirsiniz **IHttpRouteConstraint** arabirimi. Örneğin, aşağıdaki kısıtlaması bir parametre sıfır olmayan bir tamsayı değerine kısıtlar.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Aşağıdaki kod, kısıtlama kaydettirmek gösterilmektedir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Artık yollarınızı kısıtlaması uygulayabilirsiniz:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Ayrıca tüm değiştirebilirsiniz **DefaultInlineConstraintResolver** uygulayarak sınıfı **IInlineConstraintResolver** arabirimi. Bunun yapılması yerini alacak tüm yerleşik kısıtlamalar sürece uygulamanıza **IInlineConstraintResolver** özellikle ekler.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>İsteğe bağlı URI parametreleri ve varsayılan değerler

Bir soru işareti için bir rota parametresini ekleyerek bir URI parametresinin isteğe bağlı yapabilirsiniz. Bir rota parametresini isteğe bağlı ise, yöntem parametresi için varsayılan bir değer tanımlamanız gerekir.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

Bu örnekte, `/api/books/locale/1033` ve `/api/books/locale` aynı kaynak döndürür.

Alternatif olarak, rota şablonu içinde bir varsayılan değer şu şekilde belirtebilirsiniz:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Bu hemen önceki örnekle aynıdır, ancak varsayılan değer uygulandığında izinlerde davranış olduğu.

- Parametresi bu değere sahiptir; bu nedenle ilk örnekte ("{LCID?}"), doğrudan yöntem parametresi için 1033 varsayılan değeri atanır.
- İkinci örnekte ("{lcid = 1033}"), "1033" varsayılan değeri model bağlama işlemi boyunca geçer. Varsayılan model bağlayıcısını "1033" 1033 sayısal değerine dönüştürür. Ancak, farklı bir şey yapabilir ve özel bir model bağlayıcı eklenebilecek.

(Özel model bağlayıcıları, işlem hattınızda olmadığı sürece çoğu durumda, iki eşdeğer olacaktır.)

<a id="route-names"></a>
## <a name="route-names"></a>Rota adları

Web API'de her yol bir adı vardır. Bir HTTP yanıtında bir bağlantı ekleyebilirsiniz, böylece rota adları, bağlantı oluşturmak için kullanışlıdır.

Rota adını belirtmek için ayarlayın **adı** öznitelik özelliği. Aşağıdaki örnek nasıl ayarlanacağı rota adı ve rota adını, bağlantı oluşturulurken kullanılacak nasıl gösterir.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Rota sırası

Framework bir URI bir rotayla eşleşen çalıştığında, belirli bir sırada yolları değerlendirir. Sıra belirtmek için ayarlanmış **RouteOrder** rota özniteliğinin özelliği. Düşük değerler, ilk olarak değerlendirilir. Varsayılan sıra değeri sıfırdır.

Toplam sıralama nasıl belirlendiğini aşağıda verilmiştir:

1. Karşılaştırma **RouteOrder** rota özniteliğinin bir özelliğidir.
2. Rota şablonu içinde her bir URI segmenti bakın. Her bir kesim için şu şekilde sıralayın:

    1. Değişmez değer kesimi.
    2. Rota parametrelerinin kısıtlamaları.
    3. Rota parametrelerinin kısıtlamaları olmadan.
    4. Joker karakter parametresi kesimleri kısıtlamalarına sahip.
    5. Joker karakter parametresi kesimleri kısıtlamaları olmadan.
3. Bağ olması durumunda, yolların bir büyük küçük harf duyarsız sıralı dize karşılaştırmasına göre sıralanır ([Ordinalıgnorecase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) rota şablonu.

Aşağıda bir örnek vardır. Aşağıdaki denetleyicisi tanımladığınız varsayalım:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Bu yolları şu şekilde sıralanır.

1. Sipariş/Ayrıntıları
2. Siparişler / {id}
3. Siparişler / {customerName}
4. Siparişler / {\*tarih}
5. Siparişler / beklemede

"Details" bir değişmez değer kesimi ve önce "{id}" görünür ancak "bekliyor" son görünür, çünkü bildirimi **RouteOrder** özelliği 1'dir. (Bu örnekte "details" adlı hiçbir müşterinin var. varsayılır veya "bekliyor". Genel olarak, belirsiz yollar kaçınmaya çalışın. Bu örnekte, daha iyi bir rota şablonu için `GetByCustomer` olan "müşteriler / {customerName}")
