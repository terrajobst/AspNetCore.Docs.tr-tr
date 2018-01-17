---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "Öznitelik ASP.NET Web API 2 yönlendirme | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 7c563f566b8456b63ffe0a3c4876432c60a19e89
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 özniteliği yönlendirme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

*Yönlendirme* bir eylem için bir URI Web API'sini nasıl eşleştirir. Web API 2 destekleyen yeni bir tür yönlendirme biri olarak adlandırılan *özniteliği yönlendirme*. Adından da anlaşılacağı gibi özniteliği yönlendirme rotaları tanımlamak için özniteliklerini kullanır. Öznitelik yönlendirme, web API URI'ler üzerinde daha fazla denetim sağlar. Örneğin, kaynak hiyerarşileri tanımlayan URI kolayca oluşturabilirsiniz.

Kurala dayalı olarak adlandırılan yönlendirme, önceki stili yönlendirme, hala tam olarak desteklenmektedir. Aslında, her iki tekniği aynı projede de birleştirebilirsiniz.

Bu konu, öznitelik yönlendirme etkinleştirme gösterir ve öznitelik yönlendirme çeşitli seçenekler açıklanmaktadır. Öznitelik yönlendirmesi kullanan bir uçtan uca öğretici için bkz [özniteliği yönlendirme Web API 2'deki bir REST API'si oluşturma](create-a-rest-api-with-attribute-routing.md).


## <a name="prerequisites"></a>Önkoşullar

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional veya Enterprise Edition

Alternatif olarak, gerekli paketleri yüklemek için NuGet paket yöneticisini kullanın. Gelen **Araçları** menü Visual Studio'da seçin **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsol penceresinde aşağıdaki komutu girin:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Neden yönlendirme özniteliği?

Web API kullanılan ilk sürümünü *kurala dayalı* yönlendirme. Yönlendirme bu tür, temel daha fazla yol şablon dizeleri parametreli veya bir tanımlar. Çerçeve bir istek aldığında, rota şablonu karşı URI eşleşir. (Kurala dayalı yönlendirme hakkında daha fazla bilgi için bkz: [ASP.NET Web API'de yönlendirme](routing-in-aspnet-web-api.md).

Kurala dayalı yönlendirme bir avantajı, şablonları tek bir yerde tanımlanır ve yönlendirme kurallarını tüm denetleyicilerinde tutarlı bir şekilde uygulandığından olmasıdır. Ne yazık ki, kurala dayalı yönlendirme sabit RESTful API'leri ortak olan belirli URI desenleri desteklemesini sağlar. Örneğin, kaynak alt kaynakları genellikle içerir: müşterilerin sahip siparişleri, filmler sahip aktörler, books yazarlarının vardır ve benzeri. Bu ilişkileri yansıtacak URI oluşturmak için doğal şöyledir:

`/customers/1/orders`

Bu tür bir URI, kurala dayalı yönlendirme kullanarak oluşturduğunuz zordur. Yapılabilir rağmen iyi birçok denetleyicileri veya kaynak türleri varsa sonuçları ölçeklendirmeyin.

Öznitelik yönlendirme ile bir rota için bu URI tanımlamak için kısmı oldukça kolaydır. Yalnızca denetleyici eylemi bir özniteliği ekleyin:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Burada, yönlendirme yapar kolay özniteliği bazı bir desenleri bulunmaktadır.

**API sürümü oluşturma**

Bu örnekte, "/ v1/api/ürünleri" olacaktır farklı bir denetleyici yönlendirilmiş "/ v2/api/ürünleri".

`/api/v1/products`  
`/api/v2/products`

**Aşırı yüklenmiş URI parçaları**

Bu örnekte, "1" bir sıra numarasıdır, ancak "bekliyor" bir koleksiyona eşler.

`/orders/1`  
`/orders/pending`

**Birden çok parametre türleri**

Bu örnekte, "1" bir sıra numarasıdır, ancak "06/2013/16" bir tarihi belirtir.

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Öznitelik yönlendirmeyi etkinleştirme

Öznitelik yönlendirmeyi etkinleştirmek için arama **MapHttpAttributeRoutes** yapılandırma sırasında. Bu uzantı metodu tanımlanan **System.Web.Http.HttpConfigurationExtensions** sınıfı.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

İle öznitelik yönlendirme birleştirilebilir [kurala dayalı](routing-in-aspnet-web-api.md) yönlendirme. Kurala dayalı rotaları tanımlamak için arama **MapHttpRoute** yöntemi.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Web API yapılandırma hakkında daha fazla bilgi için bkz: [yapılandırma ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Not: Web API 1 ' geçiş

Web API 2 önce Web API projesi şablonları aşağıdakine benzer bir kod oluşturulur:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Öznitelik yönlendirme etkinse, bu kod bir özel durum oluşturur. Öznitelik yönlendirmeyi kullanmak için mevcut bir Web API projesini yükseltirseniz, bu yapılandırma kodunu aşağıdaki güncelleştirdiğinizden emin olun:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Daha fazla bilgi için bkz: [ASP.NET barındırma ile Web API yapılandırma](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Rota öznitelikleri ekleme

Bir öznitelik kullanılarak tanımlanmış bir yol bir örneği burada verilmiştir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Dize &quot;müşteriler / {CustomerID} / siparişleri&quot; rota için URI şablonudur. Web API, istek URI'si şablon eşleştirmeyi dener. Bu örnekte, "müşteriler" ve "Siparişler" değişmez değer kesimleri olan ve "{CustomerID}" değişken bir parametredir. Bu şablon aşağıdaki URI'ler eşleşir:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Kullanarak eşleşen kısıtlayabilirsiniz [kısıtlamaları](#constraints), bu konunun ilerleyen bölümlerinde açıklanmıştır.

Dikkat &quot;{CustomerID}&quot; rota şablonu parametresinde eşleşen adını *CustomerID* yöntemi parametresi. Web API denetleyici eylemi çalıştırdığında, rota parametrelerinin bağlamaya çalışır. Örneğin, URI ise `http://example.com/customers/1/orders`, "1" değeri bağlamak Web API dener *CustomerID* eylem parametresi.

Bir URI şablonunu birkaç parametre sahip olabilir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Bir rota özniteliğine sahip olmayan herhangi bir denetleyici yöntemin kurala dayalı yönlendirme kullanın. Bu şekilde, her iki tür aynı projede yönlendirme birleştirebilirsiniz.

## <a name="http-methods"></a>HTTP yöntemleri

Web API (GET, POST, vb.) İstek HTTP yöntemine dayalı eylemleri de seçer. Varsayılan olarak, Web API denetleyicisi yöntem adı başlangıcı küçük harf olarak eşleşen arar. Örneğin, adlandırılmış bir denetleyici yöntemi `PutCustomers` bir HTTP PUT İsteği eşleşir.

Aşağıdaki öznitelikler herhangi bir yöntemle tasarlayarak bu kuralı kılabilirsiniz:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Aşağıdaki örnek CreateBook yöntemini HTTP POST isteklerini eşler.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Standart olmayan yöntemleri de dahil olmak üzere tüm diğer HTTP yöntemleri kullanmak için **AcceptVerbs** özniteliği HTTP yöntemlerinin listesini alır.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Rota önekleri

Genellikle, yollar tüm aynı önekiyle başlayan bir denetleyici. Örneğin:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Tüm bir denetleyici için ortak bir önek kullanarak ayarlayabilirsiniz **[routeprefix öğesi]** özniteliği:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Bir tilde (~) yöntemi öznitelikte rota öneki geçersiz kılmak için kullanın:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Rota öneki parametreleri içerebilir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Rota kısıtlamaları

Rota kısıtlamalarını, rota şablonu parametrelerinde nasıl eşleştirilir kısıtlamanıza olanak sağlar. Genel sözdizimi &quot;{parametresi: kısıtlaması}&quot;. Örneğin:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Burada, ilk yol yalnızca durumunda seçilecektir &quot;kimliği&quot; URI parçası olan bir tamsayı. Aksi takdirde, ikinci yol seçilir.

Aşağıdaki tabloda, desteklenen kısıtlamaları listeler.

| Kısıtlama | Açıklama | Örnek |
| --- | --- | --- |
| Alfa | Eşleşme büyük veya küçük harf Latin alfabesi karakterler (a-z, A-Z) | {x:alpha} |
| bool | Bir Boole değeri ile eşleşir. | {x:bool} |
| datetime | Eşleşen bir **DateTime** değeri. | {x:datetime} |
| decimal | Ondalık bir değeri ile eşleşir. | {x:decimal} |
| çift | 64-bit kayan nokta değeri eşleşir. | {x:double} |
| float | 32 bit kayan nokta değeri eşleşir. | {x:float} |
| GUID | GUID değeri eşleşir. | {x: GUID} |
| int | 32 bit tamsayı değeri eşleşir. | {x:int} |
| length | Belirtilen uzunluğa sahip veya uzunlukları belirtilen aralığı içinde bir dizeyle eşleşir. | {x:length(6)} {x:length(1,20)} |
| long | 64 bit tamsayı değeri eşleşir. | {x:long} |
| max | Tamsayı bir maksimum değer ile eşleşir. | {x:max(10)} |
| MaxLength | En fazla bir dizeyle eşleşir. | {x:maxlength(10)} |
| min | Tamsayı en az bir değerle eşleşir. | {x:min(10)} |
| MinLength | Minimum uzunluk bir dizeyle eşleşir. | {x:minlength(10)} |
| aralık | Tamsayı değerleri aralığı içinde eşleşir. | {x:range(10,50)} |
| Regex | Normal ifadeyle eşleşir. | {x:regex(^\d{3}-\d{3}-\d{4}$)} |

Bildirimi, bazı kısıtlamaları gibi &quot;min&quot;, bağımsız değişkenleri parantez içine alır. Virgülle ayrılmış bir parametre birden çok kısıtlama uygulayabilirsiniz.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Özel rota kısıtlamaları

Özel rota kısıtlamaları uygulayarak oluşturabileceğiniz **IHttpRouteConstraint** arabirimi. Örneğin, aşağıdaki kısıtlama parametre sıfır tamsayı değerine kısıtlar.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Aşağıdaki kod kısıtlaması nasıl gösterir:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Şimdi yollarınızı kısıtlaması uygulayabilirsiniz:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Ayrıca tüm değiştirin **DefaultInlineConstraintResolver** uygulayarak sınıfı **IInlineConstraintResolver** arabirimi. Bunun yapılması yerini alacak tüm yerleşik kısıtlamalar sürece uygulamanıza **IInlineConstraintResolver** özellikle bunları ekler.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>İsteğe bağlı URI parametreleri ve varsayılan değerler

Bir URI parametre isteğe bağlı bir soru işareti rota parametresini ekleyerek yapabilirsiniz. Bir rota parametresini isteğe bağlı ise, yöntem parametresi için varsayılan bir değer tanımlamanız gerekir.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

Bu örnekte, `/api/books/locale/1033` ve `/api/books/locale` aynı kaynak döndürür.

Alternatif olarak, varsayılan değer rota şablonu içinde aşağıdaki gibi belirtebilirsiniz:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Bu neredeyse önceki örnekle aynıdır, ancak varsayılan değer uygulandığında küçük bir fark davranış olduğu.

- Parametresi bu tam değerine sahip şekilde ilk örnekte ("{LCID?}") 1033 varsayılan değerini doğrudan yöntem parametresi için atanır.
- İkinci örnekte ("{LCID 1033 =}"), "1033" varsayılan değerini model bağlama süreci devam ettiği. Varsayılan model bağlayıcısını "1033" 1033 sayısal değerine dönüştürür. Ancak, farklı bir şey yapabilecek özel model bağlayıcı içinde takın.

(Özel model bağlayıcıları, ardışık düzeninde yoksa çoğu durumda, iki biçim eşdeğer olacaktır.)

<a id="route-names"></a>
## <a name="route-names"></a>Yol adları

Web API'de her rotanın bir adı vardır. Böylece bir HTTP yanıt olarak bir bağlantı ekleyebilirsiniz rota adları bağlantıları oluşturmak için faydalıdır.

Rota adını belirtmek için ayarlayın **adı** öznitelik özelliği. Aşağıdaki örnek, rota adı ayarlama ve ayrıca bağlantı oluştururken rota adı kullanmayı gösterir.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Rota sırası

URI'sı bir yol ile eşleşecek şekilde framework çalıştığında, belirli bir sırada yollar değerlendirir. Sıra belirtmek üzere ayarlayın **RouteOrder** rota özniteliğinin özelliği. Düşük değerler önce değerlendirilir. Varsayılan sıra değeri sıfır olur.

İşte toplam sıralama nasıl belirlenir:

1. Karşılaştırma **RouteOrder** rota özniteliğinin özelliği.
2. Rota şablonu her URI kesimdeki bakın. Her segment için aşağıdaki gibi sipariş: 

    1. Değişmez değer kesimi.
    2. Rota parametrelerine kısıtlamalarına sahip.
    3. Rota parametrelerine kısıtlamaları olmadan.
    4. Joker karakter parametresi kesimleri kısıtlamalarına sahip.
    5. Joker karakter parametresi kesimleri kısıtlamaları olmadan.
3. Bağ durumunda yollar büyük küçük harf duyarsız sıralı dize karşılaştırma tarafından sıralanır ([Ordinalıgnorecase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) rota şablonu.

Aşağıda bir örnek vardır. Aşağıdaki denetleyicisiyle tanımladığınız varsayın:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Bu yolları şu şekilde sıralanır.

1. Sipariş/Ayrıntıları
2. siparişleri / {id}
3. siparişleri / {customerName}
4. siparişleri / {\*tarih}
5. siparişleri / beklemede

"Ayrıntılar" değişmez değer kesimi ise ve "{id}" önce görünür ancak "bekliyor" son görünür, çünkü bildirimi **RouteOrder** 1 bir özelliktir. (Bu örnekte "Ayrıntılar" adlı hiçbir müşterinin var. varsayılır veya "bekliyor". Belirsiz yollar önlemek genel olarak, deneyin. Bu örnekte, daha iyi bir yol şablonu için `GetByCustomer` olan "müşteriler / {customerName}")
