---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Yönlendirme ve eylem seçimi ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/27/2012
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 2421d3dd134e9188faf6933b076fb9d22a119617
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832836"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Yönlendirme ve eylem seçimi ASP.NET Web API
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Bu makalede, bir HTTP isteği ASP.NET Web API denetleyicisine belirli bir eylemi için nasıl yönlendirdiğini açıklanır.

> [!NOTE]
> Yönlendirme ilişkin üst düzey genel bakış için bkz: [ASP.NET Web API'de yönlendirme](routing-in-aspnet-web-api.md).


Bu makalede, yönlendirme işleminin ayrıntılarını da ele alınmaktadır. Umarım bu makalede bir Web API projesi oluşturursanız ve beklediğiniz gibi bazı istekler elde etmezsiniz Bul yönlendirilmesini yardımcı olur.

Yönlendirme üç ana aşama içerir:

1. Bir rota şablonu için URI eşleşen.
2. Bir denetleyici seçin.
3. Bir eylem seçebilirsiniz.

İşlemin bazı bölümleri, kendi özel davranışlarınızı ile değiştirebilirsiniz. Bu makalede, ben varsayılan davranışını açıklar. Sonunda, ben burada davranışını özelleştirebilirsiniz yerleri unutmayın.

## <a name="route-templates"></a>Rota şablonlarının

Bir rota şablonu için bir URI yolu gibi görünür, ancak küme ayracı ile gösterilen, yer tutucu değerlerini sahip olabilir:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Bir rota oluşturduğunuzda, bazılarını veya tümünü yer tutucular için varsayılan değerler sağlayabilirsiniz:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Ayrıca, bir URI segmenti yer tutucu nasıl eşleşebilir kısıtlama kısıtlamaları, sağlayabilirsiniz:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Framework, şablon URI'si yoldaki kesimlerin eşleştirmeyi dener. Şablondaki değişmez değerler tam olarak eşleşmelidir. Bir yer tutucu kısıtlamaları belirtmediğiniz sürece herhangi bir değer ile eşleşir. Framework URI, ana bilgisayar adı veya sorgu parametreleri gibi diğer bölümleri eşleşmiyor. Framework ilk yönlendirme URI'si ile eşleşen rota tablosunda seçer.

İki özel yer tutucuları vardır: "{denetleyici}" ve "{action}".

- "{denetleyici}" denetleyicinin adı sağlar.
- "{action}" eyleminin adını sağlar. Web API'de normal "{action}" atlamak için kuralıdır.

### <a name="defaults"></a>Varsayılanları

Rota Varsayılanları sağlarsanız, bu kesimleri eksik bir URI eşleşir. Örneğin:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

URI "`http://localhost/api/products`" Bu rotayla eşleşir. "{Category}" segment "tüm" varsayılan değer atanır.

### <a name="route-dictionary"></a>Rota sözlüğünü

Framework bir URI için bir eşleşme bulduğunda, için her bir yer tutucu değeri içeren bir sözlük oluşturur. Anahtarlar ve küme ayraçlarının dahil değil yer tutucu adlarıdır. Değerler, URI yolu veya Varsayılanları alınır. Sözlüğüne depolanan **IHttpRouteData** nesne.

Bu rota için eşleşen aşamasında, özel "{denetleyici}" ve "{action}" yer tutucuları yalnızca diğer yer tutucuları gibi kabul edilir. Bunlar yalnızca diğer değerlerle sözlükte depolanır.

Özel değeri, bir varsayılan olabilir **RouteParameter.Optional**. Bu değer bir yer tutucu atandığını, değer için rota sözlüğünü eklenmez. Örneğin:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

URI yolu "API/ürünleri" rota sözlüğünü içerir:

- Denetleyici: "Ürünler"
- Kategori: "tüm"

"API/ürünler/toys/123 için", ancak rota sözlüğünü içerir:

- Denetleyici: "Ürünler"
- Kategori: "toys"
- Kimliği: "123"

Varsayılan değerleri, rota şablonu herhangi bir yerde görünmez bir değer de içerebilir. Rotanın eşleşmesi durumunda bu değer sözlükte depolanır. Örneğin:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

URI yolu "kök/API/8" ise, sözlük iki değerlerini içerir:

- Denetleyici: "Müşteri"
- Kimliği: "8"

## <a name="selecting-a-controller"></a>Bir denetleyici seçme

Denetleyici seçimi tarafından işlenir **IHttpControllerSelector.SelectController** yöntemi. Bu yöntem bir **HttpRequestMessage** örneği ve döndürür bir **HttpControllerDescriptor**. Varsayılan uygulama tarafından sağlanan **DefaultHttpControllerSelector** sınıfı. Bu sınıf, bir basit algoritması kullanır:

1. "Controller" anahtarı için rota sözlüğünü bakın.
2. Bu anahtar için değeri alabilir ve denetleyici türü adını almak için "Controller" dizesini.
3. Bu tür adı ile bir Web API denetleyicisi arayın.

Denetleyici türü "ProductsController" ise, "Ürünler" Örneğin, rota sözlüğünü "controller" anahtar-değer çifti içeriyorsa =. Eşleşen bir türü veya birden çok eşleşme varsa, framework istemciye bir hata döndürür.

Adım 3 ' ü **DefaultHttpControllerSelector** kullanan **IHttpControllerTypeResolver** Web API denetleyicisi türleri listesini almak için arabirim. Varsayılan uygulaması **IHttpControllerTypeResolver** tüm ortak sınıfları döndürür (a) uygulayan **IHttpController**, (b) olan değil soyut ve (c) "Denetleyicisi" ile biten bir adı vardır.

## <a name="action-selection"></a>Eylem Seçimi

Denetleyici seçtikten sonra framework eylemi çağırarak seçer **IHttpActionSelector.SelectAction** yöntemi. Bu yöntem bir **HttpControllerContext** ve döndüren bir **HttpActionDescriptor**.

Varsayılan uygulama tarafından sağlanan **ApiControllerActionSelector** sınıfı. Bir eylem seçmek için aşağıdaki konumda görünür:

- İsteğin HTTP yöntemi.
- Varsa rota şablonu içinde "{action}" yer tutucu.
- Denetleyici eylemleri parametreleri.

Seçimi algoritması aramadan önce denetleyici eylemleri hakkında bazı şeyleri anlamak ihtiyacımız var.

**Hangi yöntemlerin denetleyicisinde "Eylemler" olarak kabul edilir?** Bir eylem seçerken framework denetleyicisinde yalnızca ortak örnek yöntem ele arar. Ayrıca, dışlar ["özel adı"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) yöntemleri (Oluşturucular, olaylar, İşleç aşırı yüklemeleri ve diğerleri) ve devralınan yöntemleri **ApiController** sınıfı.

**HTTP yöntemleri.** Framework, aşağıdaki gibi belirlenen isteğin HTTP yöntemi ile eşleşen eylemleri yalnızca seçer:

1. HTTP yöntemi ile bir öznitelik belirtebilirsiniz: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, veya **HttpPut**.
2. Denetleyici yöntemin adı "Get", "Post", "Put", "Delete", "Ana", "Seçenekler" veya "Düzeltme" ile başlıyorsa, aksi takdirde, sonra Kural gereği eylemi, HTTP yöntemini destekler.
3. Yukarıdakilerin hiçbiri, POST yöntemi destekler.

**Parametre bağlama.** Parametre bağlaması, Web API'si, bir parametre için bir değer nasıl oluşturur? ' dir. Parametre bağlaması için varsayılan kuralı şu şekildedir:

- Basit türler URI'SİNDEN alınır.
- Karmaşık türler, istek gövdesinden alınır.

Basit türler dahil tüm [.NET Framework ilkel türleri](https://msdn.microsoft.com/library/system.type.isprimitive), artı **DateTime**, **ondalık**, **GUID**, **dize** , ve **TimeSpan**. En fazla bir parametre, her eylem için istek gövdesi okuyabilirsiniz.

> [!NOTE]
> Varsayılan bağlama kurallarını geçersiz kılmak mümkündür. Bkz: [başlık altında Webapı parametre bağlaması](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Bu bilgileri, eylem seçimi algoritma aşağıda verilmiştir.

1. HTTP istek yöntemi eşleşen denetleyicisinde tüm eylemlerin bir listesini oluşturun.
2. Rota sözlüğünü "action" girişi varsa, eylem adı, bu değeri eşleşmiyor kaldırın.
3. Eylem parametreleri için URI şu şekilde eşleşecek şekilde deneyin: 

    1. Her eylem için burada bağlama parametresi URI'SİNDEN alır bir basit türü parametreler listesini alın. İsteğe bağlı parametreler hariç tutun.
    2. Rota sözlüğünü veya URI sorgu dizesinde her parametre adı için bir eşleşme bulmak bu listeden deneyin. Eşleşme büyük/küçük harfe duyarsızdır ve parametre sırasını bağımlı olmadan.
    3. Her parametre listesinde URI'de bir eşleşme bulunduğu bir eylem seçin.
    4. Daha fazla, bir eylem aşağıdaki ölçütleri karşılıyorsa, çoğu parametresi eşleşme ile seçin.
4. Eylemler Yoksay **[NonAction]** özniteliği.

#3. adım büyük olasılıkla en karmaşıktır. Bir parametre değeri ya da urı'sinden, istek gövdesinden veya özel bir bağlama alabilirsiniz temel olur. URİ'den gelen parametre URI gerçekten yolu (aracılığıyla rota sözlüğünü) veya sorgu dizesinde, parametre için bir değer içerdiğinden emin olmak istiyoruz.

Örneğin, aşağıdaki eylemi göz önünde bulundurun:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*Kimliği* URI parametreyi bağlar. Bu nedenle, bu eylem yalnızca "id", rota sözlüğünü veya sorgu dizesi için bir değer içeren bir URI ile eşleşebilir.

İsteğe bağlı oldukları için isteğe bağlı bir özel durum parametrelerdir. Bağlama değeri URI'SİNDEN alınamıyor, isteğe bağlı bir parametre için Tamam olduğunu.

Karmaşık türler, farklı bir neden için bir özel durumdur. Karmaşık bir tür, yalnızca üzerinden özel bağlama için URI bağlayabilirsiniz. Ancak bu durumda, framework önceden parametresi için belirli bir URI bağlayın olup olmadığını bilemezsiniz. Öğrenmek için bu bağlama çağırmasına gerekir. Seçimi algoritması amacı herhangi bir bağlama çağırmadan önce statik açıklamasından eylem seçmektir. Bu nedenle, eşleştirme algoritmasını kullanarak karmaşık türler hariç tutulur.

Tüm parametre bağlamaları eylem seçildikten sonra çağrılır.

Özet:

- Eylem, isteğin HTTP yöntemi ile eşleşmesi gerekir.
- Eylem adı için rota sözlüğünü "action" girişi varsa eşleşmelidir.
- Parametresi URI'den alınmışsa eylemin her parametre için sonra parametre adı için rota sözlüğünü veya URI sorgu dizesinde bulunması gerekir. (İsteğe bağlı parametreler ve karmaşık türler parametrelerle bırakılır.)
- Parametre sayısı en çok eşleşen deneyin. En iyi eşleşmeyi parametresiz bir yöntem olabilir.

## <a name="extended-example"></a>Genişletilmiş örneği

Yollar:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Denetleyici:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP isteği:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Eşleşen yol

URI "DefaultApi" adlı rota eşleşir. Rota sözlüğünü aşağıdaki girdileri içerir:

- Denetleyici: "Ürünler"
- Kimliği: "1"

Sorgu dizesi parametreleri, "Sürüm" ve "details" rota sözlüğünü içermiyor, ancak bunlar yine de sırasında eylem seçimi kabul edilir.

### <a name="controller-selection"></a>Denetleyici seçimi

Rota sözlüğünde "controller" girdisinden denetleyicisi türüdür `ProductsController`.

### <a name="action-selection"></a>Eylem Seçimi

Bir GET isteği HTTP isteğidir. GET işlemini denetleyici eylemleri `GetAll`, `GetById`, ve `FindProductsByName`. Biz eylem adıyla eşleşmesi gerekmez rota sözlüğünü "action" için bir giriş içermiyor.

Ardından, biz eylemleri için parametre adlarını eşleştirmek yalnızca GET eylemlerini bakarak deneyin.

| Eylem | Eşleştirme parametreleri |
| --- | --- |
| `GetAll` | yok |
| `GetById` | "id" |
| `FindProductsByName` | "name" |

Dikkat *sürüm* parametresinin `GetById` isteğe bağlı parametresi olduğundan, olarak kabul edilmez.

`GetAll` Yöntemi artık önemsiz olarak eşleşir. `GetById` Yöntemi ayrıca eşleşen, çünkü "id" rota sözlüğünü içeriyor. `FindProductsByName` Yöntemi eşleşmiyor.

`GetById` Yöntemi WINS parametresi yerine bir parametre eşleştiğinden `GetAll`. Yöntemi, aşağıdaki parametre değerleriniz ile çağrılır:

- *Kimliği* = 1
- *Sürüm* 1.5 =

Rağmen dikkat *sürüm* kullanılmadığı seçimi algoritması URI sorgu dizesi parametresinin değeri gelir.

## <a name="extension-points"></a>Uzantı noktaları

Web API'si, bazı bölümlerini yönlendirme işlemi için uzantı noktaları sağlar.

| Arabirim | Açıklama |
| --- | --- |
| **IHttpControllerSelector** | Denetleyiciyi seçer. |
| **IHttpControllerTypeResolver** | Denetleyici türleri listesini alır. **DefaultHttpControllerSelector** denetleyici türü bu listeden seçer. |
| **IAssembliesResolver** | Proje derleme listesini alır. **IHttpControllerTypeResolver** arabirimi denetleyici türlerini bulmak için bu listeyi kullanır. |
| **IHttpControllerActivator** | Yeni denetleyici örneği oluşturur. |
| **IHttpActionSelector** | Eylemi seçer. |
| **IHttpActionInvoker** | Eylemi çağırır. |

Şu arabirimlerin herhangi birinde için kendi uygulamasını sağlamak için kullanın **Hizmetleri** koleksiyonunda **HttpConfiguration** nesnesi:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
