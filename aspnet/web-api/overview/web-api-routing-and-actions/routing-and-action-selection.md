---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Yönlendirme ve eylem seçimi ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043424"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Yönlendirme ve eylem seçimi ASP.NET Web API
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu makalede, nasıl ASP.NET Web API HTTP isteğinden denetleyicisinde belirli bir eylem yönlendirir açıklanmaktadır.

> [!NOTE]
> Yönlendirme ilişkin üst düzey genel bakış için bkz: [ASP.NET Web API'de yönlendirme](routing-in-aspnet-web-api.md).


Bu makalede aşağıdaki adresten yönlendirme işlemi ayrıntıları arar. Bir Web API projesi oluşturursanız ve beklediğiniz gibi bazı istekleri almadım Bul yönlendirilen umarız bu makalede yardımcı olur.

Yönlendirme üç ana aşamadan oluşur:

1. Eşleşen bir rota şablonu için URI.
2. Bir denetleyici seçme.
3. Bir eylem seçin.

İşlem bazı bölümleri, kendi özel davranışlar ile değiştirebilirsiniz. Bu makalede, t varsayılan davranışı açıklanmaktadır. Sonunda, ı burada davranışını özelleştirebilirsiniz yerler unutmayın.

## <a name="route-templates"></a>Rota şablonlarının

Bir rota şablonu için bir URI yolu benzer, ancak yer tutucu değerlerini, süslü ayraçlar ile belirtilen olabilir:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Bir rota oluşturduğunuzda bazılarını veya tümünü yer tutucuları için varsayılan değerler sağlayabilirsiniz:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Bir URI segmenti bir yer tutucu nasıl eşleştirebilirsiniz kısıtlamak kısıtlamaları de sağlayabilirsiniz:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Framework şablona URI'si yoldaki kesimlerin eşleştirmeyi dener. Değişmez değerler şablondaki tam olarak eşleşmelidir. Bir yer tutucu kısıtlamaları belirtmediğiniz sürece herhangi bir değer ile eşleşir. Framework'te diğer ana bilgisayar adı veya sorgu parametreleri gibi URI'SİNİN bölümlerini eşleşmiyor. Framework URI eşleşen rota tablosunda ilk yolu seçer.

İki özel yer tutucuları vardır: "{controller}" ve "{action}".

- "{controller}" denetleyicinin adı sağlar.
- "{action}" eylemin adı sağlar. Web API'si "{action}" atlayın normal kuralıdır.

### <a name="defaults"></a>Varsayılanları

Rota Varsayılanları sağlarsanız, bu kesimleri eksik bir URI eşleşir. Örneğin:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

URI "`http://localhost/api/products`" Bu yolla eşleşir. "{Kategori}" kesimine, varsayılan değer "tümü" atanır.

### <a name="route-dictionary"></a>Rota sözlüğünü

Çerçeve bir URI yönelik bir eşleşme bulursa, için her bir yer tutucu değerini içeren bir sözlük oluşturur. Anahtarlar süslü ayraçlar hariç yer tutucu adlarıdır. Değerler URI yolu veya Varsayılanları alınır. Sözlük depolanan **IHttpRouteData** nesnesi.

Bu rota eşleşen aşamasında, yalnızca diğer yer tutucuları gibi özel "{controller}" ve "{action}" yer tutucuları kabul edilir. Bunlar yalnızca sözlük diğer değerler ile depolanır.

Varsayılan özel değerine sahip **RouteParameter.Optional**. Bir yer tutucu bu değer atanmış, değer rota sözlüğünü eklenmez. Örneğin:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

URI yolu "API/ürünleri" rota sözlüğünü içerir:

- controller: "products"
- Kategori: "tümü"

"API/ürünler/toys/123 için", ancak rota sözlüğünü içerir:

- controller: "products"
- Kategori: "toys"
- id: "123"

Varsayılan rota şablonu herhangi bir yerde görünmez bir değer dahil edebilirsiniz. Rota eşleşiyorsa, bu değeri sözlükte depolanır. Örneğin:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

URI yolu "kök/api/8" ise, sözlük iki değerlerini içerir:

- Denetleyici: "Müşteri"
- id: "8"

## <a name="selecting-a-controller"></a>Bir denetleyici seçme

Denetleyici seçimi tarafından işlenir **IHttpControllerSelector.SelectController** yöntemi. Bu yöntem alır bir **HttpRequestMessage** örneği ve döndürür bir **HttpControllerDescriptor**. Varsayılan uygulama tarafından sağlanan **DefaultHttpControllerSelector** sınıfı. Bu sınıf, bir basit algoritması kullanır:

1. "Denetleyici" anahtarı için rota sözlüğünü bakın.
2. Bu anahtar değeri alın ve denetleyici türü adını almak için "denetleyici" dizesi ekleyin.
3. Bu tür adı ile Web API denetleyicisi arayın.

Örneğin, rota sözlüğünü "denetleyici" anahtar-değer çifti içeriyorsa denetleyici türü "ProductsController" sonra "Ürünler" =. Eşleşen bir türü veya birden çok eşleşme varsa framework istemciye bir hata döndürür.

Adım 3 ' için **DefaultHttpControllerSelector** kullanan **IHttpControllerTypeResolver** Web API denetleyicisi türleri listesini almak için arabirim. Varsayılan uygulaması **IHttpControllerTypeResolver** tüm ortak sınıflar (a) uygulayan döndürür **IHttpController**, (b) değil soyut ve (c) içinde "denetleyici" ile biten bir ada sahip olduğundan.

## <a name="action-selection"></a>Eylem Seçimi

Denetleyici seçtikten sonra framework eylemi çağırarak seçer **IHttpActionSelector.SelectAction** yöntemi. Bu yöntem alır bir **HttpControllerContext** ve döndüren bir **HttpActionDescriptor**.

Varsayılan uygulama tarafından sağlanan **ApiControllerActionSelector** sınıfı. Bir eylem seçmek için şu görünür:

- İsteğin HTTP yöntemi.
- "{Action}" yer tutucu rota şablonuyla varsa.
- Denetleyici eylemleri parametreleri.

Seçimi algoritması aramadan önce biz denetleyici eylemleri hakkında bazı şeyleri anlamanız gerekir.

**Hangi yöntemlerin denetleyicisinde "Eylemler" olarak kabul edilir?** Bir eylem seçerken, framework ortak örnek yöntemleri denetleyicisinde yalnızca arar. Ayrıca, dışlar ["özel adı"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (Oluşturucular, olaylar, işlecin ve benzeri) ve devralınan yöntemleri **ApiController** sınıfı.

**HTTP yöntemleri.** Framework yalnızca aşağıdaki gibi belirlenen isteğin HTTP yöntemi ile eşleşen Eylemler seçti:

1. HTTP yöntemi ile bir öznitelik belirtebilirsiniz: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, veya **HttpPut**.
2. Denetleyici yönteminin adı "Get", "Post", "Put", "Delete", "Head", "Seçenekleri" veya "Patch" ile başlıyorsa, aksi takdirde, sonra kurala göre eylemi, HTTP yöntemini destekler.
3. Yukarıdakilerin hiçbiri, POST yöntemini destekler.

**Parametre bağlamaları.** Parametre bağlaması, bir parametre için bir değer Web API'sini nasıl oluşturduğunu ' dir. Parametre bağlama için varsayılan kuralı şöyledir:

- Basit türler URI'den alınır.
- Karmaşık türler isteği gövdesinden alınır.

Basit türler tüm [.NET Framework ilkel türler](https://msdn.microsoft.com/library/system.type.isprimitive), artı **DateTime**, **ondalık**, **GUID**, **dize** , ve **TimeSpan**. Her bir eylem için en fazla bir parametre istek gövdesi okuyabilir.

> [!NOTE]
> Varsayılan bağlama kurallarını geçersiz kılmasına mümkündür. Bkz: [Webapı parametre bağlaması başlık altında](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Bu arka plan, eylem seçimi algoritma aşağıda verilmiştir.

1. HTTP istek yöntemi eşleşen denetleyicisinde tüm eylemlerin bir listesini oluşturun.
2. Rota sözlüğünü bir "eylem" giriş varsa, adı bu değerle eşleşmiyor Eylemler kaldırın.
3. URI için eylem parametrelerini aşağıdaki gibi eşleşecek şekilde deneyin: 

    1. Her bir eylem için burada bağlama parametresi URI'den alır basit bir tür parametreler listesini alın. İsteğe bağlı parametreler hariç tutun.
    2. Bir eşleşme her parametre adı için rota sözlüğünü veya URI sorgu dizesinde bulmak için bu listeden deneyin. Eşleşme büyük küçük harfe duyarlı ve parametre sırasını güvenmeyin.
    3. Listedeki her parametre URI'de bir eşleşme bulunduğu bir eylem seçin.
    4. Daha o eylemi Bu ölçütleri karşılıyorsa, çoğu parametre eşleşmeleri ile seçin.
4. Eylemler ile Yoksay **[NonAction]** özniteliği.

#3. adım büyük olasılıkla en karmaşıktır. Bir parametre değeri ya da URI, istek gövdesinden veya özel bağlama alabilirsiniz temel olur. URI'den gelen parametreleri için URI gerçekte yolun (aracılığıyla rota sözlüğünü) veya bir sorgu dizesi Bu parametre için bir değer içerdiğinden emin olmak istiyoruz.

Örneğin, aşağıdaki eylemi göz önünde bulundurun:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*Kimliği* parametreyi bağlar URI'si. Bu nedenle, bu eylem yalnızca "id", rota sözlüğünü veya sorgu dizesi için bir değer içeren bir URI eşleştirebilirsiniz.

İsteğe bağlı oldukları için isteğe bağlı parametreler bir özel durumdur. Bağlama URI'den değeri alınamıyor isteğe bağlı bir parametre için Tamam iyisidir.

Karmaşık türler için farklı bir neden bir özel durumdur. Karmaşık bir tür, yalnızca URI üzerinden özel bağlama bağlayabilirsiniz. Ancak bu durumda, framework önceden parametresi için belirli bir URI bağlayın olup olmadığını bilemezsiniz. Öğrenmek için bir bağlama çağrılacak gerekir. Hiçbir bağlama çağırmadan önce statik açıklamasından bir eylem seçmek için seçim algoritması belirtilir. Bu nedenle, karmaşık türler eşleşen algoritmadan hariç tutulur.

Tüm parametre bağlamaları eylem seçildikten sonra çağrılır.

Özeti:

- Eylem isteğin HTTP yöntemi ile eşleşmesi gerekir.
- Eylem adı, varsa rota sözlüğünü "eylem" girdisinde eşleşmesi gerekir.
- Parametre URI'den alınmışsa her eylem parametresi için sonra parametre adı için rota sözlüğünü veya URI sorgu dizesinde bulunması gerekir. (İsteğe bağlı parametreler ve karmaşık türler parametrelerle hariç tutulur.)
- En çok parametrelerden biri eşleşecek şekilde deneyin. En iyi eşleşen hiçbir parametre bir yöntem olabilir.

## <a name="extended-example"></a>Genişletilmiş örnek

Yollar:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Denetleyici:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP isteği:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Eşleşen yol

URI "DefaultApi" adlı rota eşleşir. Rota sözlüğünü aşağıdaki girdileri içerir:

- controller: "products"
- id: "1"

Rota sözlüğünü sorgu dizesi parametreleri, "Sürüm" ve "Ayrıntılar" içermiyor, ancak bunlar hala eylem seçimi sırasında olarak kabul edilir.

### <a name="controller-selection"></a>Denetleyici seçimi

Rota sözlükte "denetleyici" girdisinden denetleyicisi türüdür `ProductsController`.

### <a name="action-selection"></a>Eylem Seçimi

HTTP GET isteği isteğidir. GET destek denetleyici eylemleri `GetAll`, `GetById`, ve `FindProductsByName`. Biz eylem adı ile eşleşmesi gerek kalmaması için rota sözlüğünü "eylem" için bir giriş içermiyor.

Ardından, biz eylemleri için parametre adlarını eşleştirmek yalnızca GET eylemlerini bakmayı deneyin.

| Eylem | Eşleştirilecek parametreleri |
| --- | --- |
| `GetAll` | yok |
| `GetById` | "id" |
| `FindProductsByName` | "name" |

Dikkat *sürüm* parametresinin `GetById` isteğe bağlı bir parametre olduğundan, olarak kabul edilmez.

`GetAll` Yöntemi trivially ile eşleşir. `GetById` Yöntemi ayrıca eşleşir, rota sözlüğünü "id" içerdiğinden. `FindProductsByName` Yöntemi eşleşmiyor.

`GetById` Yöntemi WINS için hiçbir parametre karşı bir parametre eşleştiğinden `GetAll`. Yöntemi, aşağıdaki parametre değerleri ile çağrılır:

- *id* = 1
- *Sürüm* 1.5 =

Rağmen dikkat *sürüm* kullanılmadığı seçimi algoritması URI sorgu dizesi parametresinin değeri gelir.

## <a name="extension-points"></a>Uzantı noktaları

Web API yönlendirme işlemi bazı bölümleri için uzantı noktaları sağlar.

| Arabirim | Açıklama |
| --- | --- |
| **IHttpControllerSelector** | Denetleyiciyi seçer. |
| **IHttpControllerTypeResolver** | Denetleyici türlerini listesini alır. **DefaultHttpControllerSelector** denetleyici türü bu listeden seçer. |
| **IAssembliesResolver** | Proje derleme listesini alır. **IHttpControllerTypeResolver** arabirimi denetleyici türlerini bulmak için bu listeyi kullanır. |
| **IHttpControllerActivator** | Yeni denetleyici örneği oluşturur. |
| **IHttpActionSelector** | Eylemi seçer. |
| **IHttpActionInvoker** | Eylemi çağırır. |

Herhangi bir bu arabirimleri için kendi uygulama sunmak amacıyla kullanmak **Hizmetleri** koleksiyonunda **HttpConfiguration** nesnesi:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
