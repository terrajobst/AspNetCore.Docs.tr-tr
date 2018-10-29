---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: ASP.NET Web API'de yönlendirme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/29/2018
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a7bc998fc23c0453fc9cd6ac1e7b9af7bd516225
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207309"
---
<a name="routing-in-aspnet-web-api"></a>ASP.NET Web API'de yönlendirme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Bu makalede, ASP.NET Web API HTTP isteklerini denetleyicilerine nasıl yönlendirdiğini açıklanır.

> [!NOTE]
> ASP.NET MVC ile bilginiz varsa, Web API yönlendirmeye MVC yönlendirme için çok benzer. Web API eylemi seçmek için HTTP fiili, URI yolu kullanır ana farktır. MVC stili Web API'de yönlendirme de kullanabilirsiniz. Bu makalede, ASP.NET MVC, herhangi bir Bilgi Bankası varsaymaz.

## <a name="routing-tables"></a>Yönlendirme tabloları

ASP.NET Web API, bir *denetleyicisi* HTTP isteklerini işleyen sınıftır. Denetleyicinin genel yöntem de çağrıldığında *eylem yöntemleri* ya da yalnızca *eylemleri*. Web API çerçevesi, bir istek aldığında, bir eyleme isteği yönlendirir.

Çağrılacak eylemi belirlemek için framework kullanan bir *yönlendirme tablosunu*. Web API'si için Visual Studio Proje şablonu, varsayılan bir yol oluşturur:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Bu rota tanımlanan *WebApiConfig.cs* yerleştirilen dosya *uygulama\_Başlat* dizini:

![](routing-in-aspnet-web-api/_static/image1.png)

Hakkında daha fazla bilgi için `WebApiConfig` sınıfı [ASP.NET Web API'sini yapılandırma](../advanced/configuring-aspnet-web-api.md).

Web API Self barındırıyorsanız, doğrudan yönlendirme tablosu ayarlamalısınız `HttpSelfHostConfiguration` nesne. Daha fazla bilgi için [barındırılan Web API'si](../older-versions/self-host-a-web-api.md).

Yönlendirme tablosundaki her bir giriş içeren bir *rota şablonu*. Web API'si için varsayılan rota şablonudur &quot;API / {denetleyici} / {id}&quot;. Bu şablona &quot;API&quot; değişmez bir yol kesimi ve {denetleyici} ve {id} yer tutucusu değişkenlerdir.

Web API çerçevesi, bir HTTP isteği aldığında, URI yönlendirme tablosundaki rota şablonlardan birini karşı eşleştirmeyi dener. Hiçbir yol eşleşirse, istemci bir 404 hatası alır. Örneğin, aşağıdaki URI, varsayılan yolu eşleşmiyor:

- / api/kişiler
- /api/Contacts/1
- /api/Products/gizmo1

Eksik olduğundan ancak, aşağıdaki URI, eşleşmiyor &quot;API&quot; segment:

- / kişileri/1

> [!NOTE]
> "API" yolda kullanma nedeni, ASP.NET MVC yönlendirme çarpışmalardan kaçınmak için olmasıdır. Bu şekilde, elinizde &quot;/kişiler&quot; bir MVC denetleyicisi gidin ve &quot;/api/kişileri&quot; bir Web API denetleyicisi gidin. Elbette, bu yöntemi kullanmak istemiyorsanız, varsayılan rota tablosu değiştirebilirsiniz.

Eşleşen bir rota bulunduktan sonra Web API denetleyici ve eylem seçer:

- Web API denetleyicisi ekler &quot;denetleyicisi&quot; değerine *{denetleyici}* değişkeni.
- Eylem bulmak için Web API'si HTTP fiili arar ve bir eylem adı, HTTP fiili adı ile başlayan arar. Örneğin, bir eylem ön eki için bir GET isteği ile Web API görünür &quot;alma&quot;, gibi &quot;GetContact&quot; veya &quot;GetAllContacts&quot;. Bu kuralı, yalnızca GET, sonrası, PUT, DELETE, HEAD, seçenekleri ve fiilleri düzeltme eki uygular. Diğer HTTP fiilleri denetleyicinizde öznitelikleri kullanarak etkinleştirebilirsiniz. Bu örnek daha sonra göreceğiz.
- Rota şablonu diğer yer tutucu değişkenleri gibi *{id}* eylem parametrelerini eşlenir.

Bir örneğe göz atalım. Aşağıdaki denetleyicisi tanımladığınız varsayalım:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Her biri için çağrılır eylemi yanı sıra bazı olası HTTP isteklerini şunlardır:

| HTTP fiili | URI yolu | Eylem | Parametre |
| --- | --- | --- | --- |
| AL | API/ürünleri | GetAllProducts | *(hiçbiri)* |
| AL | API/ürünler/4 | GetProductById | 4 |
| DELETE | API/ürünler/4 | DeleteProduct | 4 |
| YAYINLA | API/ürünleri | *(eşleşme yok)* |  |

Dikkat *{id}* URI segmenti varsa, eşlenmiş durumda *kimliği* eylem parametresi. Bu örnekte, denetleyici, bir iki GET yöntemleri tanımlar. bir *kimliği* parametre ve parametre olmadan.

Ayrıca, denetleyici tanımlamaz çünkü POST isteği reddeder unutmayın bir &quot;Post... &quot; yöntemi.

## <a name="routing-variations"></a>Yönlendirme farklılıkları

Önceki bölümde, ASP.NET Web API'si için temel yönlendirme mekanizması açıklanmaktadır. Bu bölümde, bazı farklılıklar açıklanmaktadır.

### <a name="http-verbs"></a>HTTP fiilleri

HTTP fiilleri için adlandırma kuralı kullanmak yerine, açıkça HTTP edimi bir eylem için eylem yöntemine aşağıdaki özniteliklerden birini tasarlayarak belirtebilirsiniz:

- `[HttpGet]`
- `[HttpPut]`
- `[HttpPost]`
- `[HttpDelete]`
- `[HttpHead]`
- `[HttpOptions]`
- `[HttpPatch]`

Aşağıdaki örnekte, `FindProduct` yöntemi GET istekleri için eşleştirilen:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Bir eylem için birden çok HTTP fiillere izin verme veya GET, PUT, POST, DELETE, HEAD, SEÇENEKLERİNİ ve düzeltme eki dışındaki HTTP fiillerine izin vermek için `[AcceptVerbs]` özniteliği HTTP fiilleri listesini alır.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Eylem adına göre yönlendirme

Varsayılan yönlendirme şablonu ile bir eylem seçmek için HTTP fiili Web API'sini kullanır. Bununla birlikte, eylem adı URI'de burada dahil bir yol oluşturabilirsiniz:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

Bu yol şablonunda *{action}* parametre adları denetleyici eylem yöntemi. Yönlendirme bu stil ile öznitelikleri izin verilen HTTP fiilleri kullanın. Örneğin, aşağıdaki yöntemi denetleyicinizin olduğunu varsayın:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

Bu durumda, "API/ürünler/Ayrıntılar/1" için bir GET isteği eşlemek `Details` yöntemi. Bu stil, yönlendirme, ASP.NET MVC için benzer ve bir RPC-style API'si için uygun olabilir.

Eylem adı kullanılarak kılabilirsiniz `[ActionName]` özniteliği. Aşağıdaki örnekte, eşlenen iki bir eylem olmadığından &quot;ürünler/API/küçük/*kimliği*. Bir GET ve POST diğer destekler:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Eylemleri

Bir yöntem bir eylem olarak çağrılan önlemek için `[NonAction]` özniteliği. Aksi takdirde yönlendirme kurallarını BC olsa bile bu framework yöntemin bir eylem değil bildirir.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu konuda, yönlendirme üst düzey bir görünüm sağlanır. Daha fazla ayrıntı için [Yönlendirme ve eylem seçimi](routing-and-action-selection.md), tam olarak nasıl framework bir URI bir rota için eşleşen bir denetleyiciyi seçer ve ardından çağırmak için eylemi seçer açıklar.
