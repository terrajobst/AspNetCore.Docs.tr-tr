---
title: ASP.NET Core Web API, denetleyici eylem dönüş türleri
author: scottaddie
description: Bir ASP.NET çekirdek Web API'si çeşitli denetleyici eylem yönteminin dönüş türleri kullanma hakkında bilgi edinin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/21/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: web-api/action-return-types
ms.openlocfilehash: 88c8e61960982f405275c429087c7dd0c61abb8d
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>ASP.NET Core Web API, denetleyici eylem dönüş türleri

Tarafından [Scott Addie](https://github.com/scottaddie)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

ASP.NET Core dönüş türleri Web API denetleyici eylemi için aşağıdaki seçenekleri sağlar:

::: moniker range="<= aspnetcore-2.0"
* [Belirli tür](#specific-type)
* [IActionResult](#iactionresult-type)
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
* [Belirli tür](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T >](#actionresultt-type)
::: moniker-end

Bu belge her dönüş türünü kullanmak en uygun olduğunda açıklar.

## <a name="specific-type"></a>Belirli tür

Basit veya karmaşık veri türü basit eylem döndürür (örneğin, `string` veya bir özel nesne türü). Özel bir koleksiyonunu döndürür aşağıdaki eylemi göz önünde bulundurun `Product` nesneler:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Eylem yürütme sırasında karşı korumak için bilinen koşullar, belirli bir tür döndüren yeterli. Parametre kısıtlamaları doğrulama gerekli değil şekilde önceki eylem hiçbir parametre kabul eder.

Zaman bir eylemi birden çok dönüş yolları sunulan için dikkate alınması gereken koşulları bilinen. Böyle bir durumda karıştırmak için ortak bir [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) dönüş türü basit veya karmaşık dönüş türüne sahip. Her iki [IActionResult](#iactionresult-type) veya [ActionResult\<T >](#actionresultt-type) bu tür eylem uyum sağlamak gereklidir.

## <a name="iactionresult-type"></a>IActionResult türü

[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) dönüş türü, uygun olduğunda birden çok [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) türleri bir eylemi olası döndürür. `ActionResult` Türleri çeşitli HTTP durum kodları temsil eder. Bu kategoride dönmeden bazı ortak dönüş türleri [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) ve [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Olduğundan birden çok dönüş türü ve eylem, serbest yollarında kullanımını [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) özniteliği gereklidir. Bu öznitelik gibi araçları tarafından oluşturulan API yardım sayfalarına daha açıklayıcı yanıt ayrıntılarını üreten [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` bilinen türler ve HTTP durum kodları eylem tarafından döndürülecek gösterir.

### <a name="synchronous-action"></a>Zaman uyumlu işlem

İki olası dönüş türü olan aşağıdaki zaman uyumlu eylemi göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

Ürün tarafından temsil edilen önceki eylemde 404 durum kodu döndürülür `id` temel alınan veri deposunda yok. [NotFound](/dotnet/api/system.web.http.apicontroller.notfound) yardımcı yöntemi, bir kısayol olarak çağrılır `return new NotFoundResult();`. Ürünün mevcut değilse, bir `Product` yükünü temsil eden bir 200 durum koduyla döndürülen nesne. [Tamam](/dotnet/api/system.web.http.apicontroller.ok) yardımcı yöntemi toplu biçiminde çağrılan `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Zaman uyumsuz eylem

İki olası dönüş türü olan aşağıdaki zaman uyumsuz eylem göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Model doğrulama başarısız olduğunda önceki eyleminde 400 durum kodu döndürülür ve [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) yardımcı yöntemi çağrılır. Örneğin, aşağıdaki model istekleri sağlamalısınız belirtir `Name` özellik ve bir değer. Bu nedenle, uygun sağlamak için hata `Name` istekte model doğrulamasının başarısız olmasına neden olur.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

Önceki eyleme ait diğer bilinen dönüş kodu bir 201 tarafından oluşturulan olan [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) yardımcı yöntemi. Bu yolunda `Product` nesne döndürülür.

::: moniker range=">= aspnetcore-2.1"
## <a name="actionresultt-type"></a>ActionResult\<T > türü

ASP.NET Core 2.1 tanıtır `ActionResult<T>` dönüş türü Web API denetleyici eylemleri. Dönüş türetme bir türü sağlar [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) veya döndüren bir [belirli tür](#specific-type). `ActionResult<T>` üzerinde aşağıdaki avantajları sunar [IActionResult türü](#iactionresult-type):

* [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) özniteliğin `Type` özelliği hariç tutulamaz.
* [Örtük atama işleçleri](/dotnet/csharp/language-reference/keywords/implicit) her ikisi de dönüşümünü destekleyen `T` ve `ActionResult` için `ActionResult<T>`. `T` dönüştürür [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), anlamına `return new ObjectResult(T);` için Basitleştirilmiş `return T;`.

Çoğu eylemi belirli bir dönüş türüne sahip. Beklenmeyen koşulları durumu belirli türü döndürülen değil eylem yürütme sırasında ortaya çıkabilir. Örneğin, bir eylem giriş parametresi model doğrulama başarısız olabilir. Böyle bir durumda uygun döndürmek için ortak olan `ActionResult` belirli türü yerine türü.

### <a name="synchronous-action"></a>Zaman uyumlu işlem

İki olası dönüş türleri olduğu zaman uyumlu bir eylem göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

Önceki kod içinde ürünün veritabanında yok 404 durum kodu döndürülür. Ürün yoksa, karşılık gelen `Product` nesne döndürülür. ASP.NET Core 2.1 önce `return product;` satır olabilirdi `return Ok(product);`.

> [!TIP]
> Denetleyici sınıfı ile donatılmış ASP.NET Core 2.1 itibariyle eylem parametresi bağlama kaynak çıkarım etkinleştirilir `[ApiController]` özniteliği. Bir rota şablonu adı ile eşleşen bir parametre adı isteği rota verilerini kullanarak otomatik olarak bağlanır. Sonuç olarak, önceki eyleme ait `id` parametresine değil açıkça ile Açıklama [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) özniteliği.

### <a name="asynchronous-action"></a>Zaman uyumsuz eylem

İki olası dönüş türü olan bir zaman uyumsuz eylem göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Model doğrulama başarısız olursa, [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) yöntemi, dönüş 400 durum kodu için çağrılır. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) belirli doğrulama hatalarını içeren özellik kendisine geçirilir. Model doğrulama başarılı olursa, ürün veritabanında oluşturulur. 201 durum kodu döndürülür.

> [!TIP]
> Denetleyici sınıfı ile donatılmış ASP.NET Core 2.1 itibariyle eylem parametresi bağlama kaynak çıkarım etkinleştirilir `[ApiController]` özniteliği. Karmaşık tür parametreleri, istek gövdesini kullanarak otomatik olarak bağlanır. Sonuç olarak, önceki eyleme ait `product` parametresine değil açıkça ile Açıklama [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği.
::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Denetleyici eylemleri](xref:mvc/controllers/actions)
* [Model doğrulaması](xref:mvc/models/validation)
* [Web API Yardım sayfalarını kullanma Swagger](xref:tutorials/web-api-help-pages-using-swagger)
