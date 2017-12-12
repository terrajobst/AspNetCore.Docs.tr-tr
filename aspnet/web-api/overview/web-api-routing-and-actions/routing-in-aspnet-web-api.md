---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: "ASP.NET Web API'de yönlendirme | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="routing-in-aspnet-web-api"></a>ASP.NET Web API'de yönlendirme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu makalede, nasıl ASP.NET Web API HTTP isteklerini denetleyicilerine yolları açıklanmaktadır.

> [!NOTE]
> ASP.NET MVC ile bilginiz varsa, Web API yönlendirmeye MVC yönlendirme için çok benzer. Web API eylemini seçmek için URI yolu HTTP yöntemini kullanır ana farktır. MVC stili Web API'de yönlendirme de kullanabilirsiniz. Bu makalede, ASP.NET MVC bilgisi varsaymaz.


## <a name="routing-tables"></a>Yönlendirme tabloları

ASP.NET Web API içinde bir *denetleyicisi* HTTP isteklerini işleyen sınıftır. Genel yöntemler denetleyicisinin adlı *eylem yöntemleri* ya da yalnızca *Eylemler*. Web API çerçevesi bir istek aldığında, istek için bir eylem yönlendirir.

Çağrılacak hangi eylemini belirlemek için framework kullanan bir *yönlendirme tablosu*. Web API için Visual Studio Proje şablonu, bir varsayılan yol oluşturur:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

Bu rota uygulamada yerleştirilir WebApiConfig.cs dosyasında tanımlanan\_başlangıç dizini:

![](routing-in-aspnet-web-api/_static/image1.png)

Hakkında daha fazla bilgi için **WebApiConfig** sınıfı için bkz: [yapılandırma ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).

Web API Self barındırıyorsanız, doğrudan yönlendirme tablosu ayarlamalısınız **HttpSelfHostConfiguration** nesnesi. Daha fazla bilgi için bkz: [bir Web API Self konak](../older-versions/self-host-a-web-api.md).

Yönlendirme tablosundaki her giriş içeren bir *rota şablonu*. Web API için varsayılan rota şablonudur &quot;API / {controller} / {id}&quot;. Bu şablonda &quot;API&quot; bir değişmez değer yol kesimi ve {controller} ve {id} yer tutucusu değişkenlerdir.

Web API çerçevesi bir HTTP isteği aldığında, yönlendirme tablosundaki rota şablonlarından birini karşı URI eşleştirmeye çalışır. Hiçbir yol eşleşirse, istemci bir 404 hatası alır. Örneğin, aşağıdaki URI, varsayılan yol eşleştir:

- / api/contacts
- /api/Contacts/1
- /api/Products/gizmo1

Eksik olduğundan ancak, aşağıdaki URI, eşleşmiyor &quot;API&quot; segment:

- / Kişiler/1

> [!NOTE]
> ASP.NET MVC yönlendirmesinin çakışmaları önlemek için "API" rotada kullanmanın nedeni gerekir. Böylece, elinizde &quot;/kişiler&quot; bir MVC denetleyicisi gidin ve &quot;/api/contacts&quot; Web API denetleyicisi gidin. Elbette, bu kural hoşlanmıyorsanız, varsayılan rota tablosu değiştirebilirsiniz.

Eşleşen bir rota bulunduktan sonra Web API denetleyici ve eylem seçer:

- Web API denetleyicisi bulma ekler &quot;denetleyicisi&quot; değerine *{controller}* değişkeni.
- Eylem bulmak için Web API at HTTP yöntemi arar ve adı, HTTP yöntem adı ile başlayan bir eylem arar. Örneğin, bir GET isteği ile Web API ile başlayan bir eylem arar &quot;Al... &quot;, gibi &quot;GetContact&quot; veya &quot;GetAllContacts&quot;. Bu kural, yalnızca GET, POST, koy ve Sil yöntemlerini uygular. Denetleyicinizde öznitelikleri kullanarak diğer HTTP yöntemleri etkinleştirebilirsiniz. Bu örnek daha sonra göreceğiz.
- Rota şablonunu diğer yer tutucu değişkenler gibi *{id}* eylem parametreler eşlenmedi.

Bir örneğe bakalım. Aşağıdaki denetleyicisiyle tanımladığınız varsayın:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

Her biri için çağrılır eylem birlikte bazı olası HTTP isteklerini şunlardır:

| HTTP yöntemi | URI yolu | Eylem | Parametre |
| --- | --- | --- | --- |
| AL | API/ürünleri | GetAllProducts | *(hiçbiri)* |
| AL | Ürünler/api/4 | GetProductById | 4 |
| DELETE | Ürünler/api/4 | DeleteProduct | 4 |
| YAYINLA | API/ürünleri | *(eşleşme)* |  |

Dikkat *{id}* URI segmenti varsa, eşleştirilir *kimliği* eylem parametresi. Bu örnekte, denetleyici, biriyle iki GET yöntemleri tanımlar. bir *kimliği* parametre ve hiçbir parametre.

Ayrıca, denetleyici tanımlamaz çünkü POST isteği başarısız olur unutmayın bir &quot;Post... &quot; yöntemi.

## <a name="routing-variations"></a>Yönlendirme farklılıkları

ASP.NET Web API için temel yönlendirme mekanizması önceki bölümde açıklanan. Bu bölümde bazı farklılıklar açıklanmaktadır.

### <a name="http-methods"></a>HTTP yöntemleri

Adlandırma kuralı için HTTP yöntemleri kullanmak yerine, açıkça HTTP yöntem bir eylem için eylem yöntemiyle tasarlayarak belirtebilirsiniz **HttpGet**, **HttpPut**, **HttpPost** , veya **HttpDelete** özniteliği.

Aşağıdaki örnekte, FindProduct yöntemi GET isteklerinin eşlenir:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

Bir eylem için birden çok HTTP yöntemleri izin vermek veya HTTP yöntemleri GET, PUT, POST ve DELETE dışında izin vermek için kullanın **AcceptVerbs** özniteliği HTTP yöntemlerinin listesini alır.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a>Eylem adına göre yönlendirme

Varsayılan yönlendirme şablonu ile Web API eylem seçmek için HTTP yöntemini kullanır. Bununla birlikte, eylem adı URI'de burada bulunan bir yol oluşturabilirsiniz:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

Bu yol şablonunda *{action}* parametre adları denetleyici eylem yöntemi. Yönlendirme bu stili ile öznitelikleri izin verilen HTTP yöntemleri belirtmek için kullanın. Örneğin, aşağıdaki yöntemi denetleyicinizi olduğunu varsayın:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

Bu durumda, "API/ürünler/Ayrıntılar/1" için bir GET isteği map ayrıntıları yöntemi. Yönlendirme bu stili için ASP.NET MVC benzer ve bir RPC-style API için uygun olabilir.

Eylem adı kullanılarak kılabilirsiniz **EylemAdı** özniteliği. Aşağıdaki örnekte, eşle iki eylem vardır &quot;ürünler/api/küçük/*kimliği*. Bir GET ve POST diğer destekler:

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a>Eylemler olmayan

Bir yöntem bir eylem olarak çağrılan engellemek için kullanma **NonAction** özniteliği. Aksi takdirde yönlendirme kurallarını eşleşir olsa bile bu framework yöntem bir eylem değil işaret eder.

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu konuda yönlendirme üst düzey bir görünümü sağlanır. Daha fazla ayrıntı için [Yönlendirme ve eylem seçimi](routing-and-action-selection.md), tam olarak nasıl framework eşleşen bir rota için bir URI, bir denetleyiciyi seçer ve sonra çağrılacak eylemi seçer açıklar.
