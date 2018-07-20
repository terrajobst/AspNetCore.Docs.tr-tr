---
title: ASP.NET Core Web API denetleyici eylemi dönüş türleri
author: scottaddie
description: Bir ASP.NET Core Web API'si çeşitli denetleyici eylem yönteminin dönüş türleri kullanma hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/21/2018
uid: web-api/action-return-types
ms.openlocfilehash: 422db97da222fb5e742e1d8e6ae410edc90dbc18
ms.sourcegitcommit: ee2b26c7d08b38c908c668522554b52ab8efa221
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "36273562"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>ASP.NET Core Web API denetleyici eylemi dönüş türleri

Tarafından [Scott Addie](https://github.com/scottaddie)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

ASP.NET Core Web API denetleyici eylemi için aşağıdaki seçenekleri dönüş türleri sunar:

::: moniker range="<= aspnetcore-2.0"
* [Belirli bir türü](#specific-type)
* [IActionResult](#iactionresult-type)
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
* [Belirli bir türü](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T >](#actionresultt-type)
::: moniker-end

Bu belgede, her bir dönüş türü kullanmak en uygun olduğunda açıklanmaktadır.

## <a name="specific-type"></a>Belirli bir türü

Bir basit veya karmaşık veri türü basit bir eylem döndürür (örneğin, `string` veya özel bir nesne). Özel koleksiyonu döndüren eylem göz önünde bulundurun `Product` nesneler:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Eylem yürütme sırasında karşı korumak için bilinen koşul, belirli bir tür döndüren yeterli. Önceki eylemi parametresi kısıtlamaları doğrulama gerekli değilse bu nedenle hiçbir parametre kabul eder.

Olduğunda bir uygulamada birden fazla dönüş yolları sunulan için katılması gereken koşullar bilinir. Böyle bir durumda karıştırmak için ortak bir [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) dönüş türü basit veya karmaşık dönüş türüne sahip. Her iki [IActionResult](#iactionresult-type) veya [actionresult öğesini\<T >](#actionresultt-type) bu tür bir eylemin uyum sağlamak gereklidir.

## <a name="iactionresult-type"></a>IActionResult türü

[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) dönüş türü, uygun olduğunda birden çok [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) dönüş türleri bir eylemi mümkündür. `ActionResult` Türleri çeşitli HTTP durum kodları temsil eder. Bu kategoriye dönülüyor bazı yaygın dönüş türleri [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) ve [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).

Birden fazla dönüş türleri ve eylem, serbest yollarında kullanımını olduğundan [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) özniteliği gereklidir. Bu öznitelik gibi araçları tarafından oluşturulan API Yardım sayfaları daha açıklayıcı yanıt ayrıntılarını üretir [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger). `[ProducesResponseType]` Eylem tarafından döndürülen HTTP durum kodları ve bilinen türleri gösterir.

### <a name="synchronous-action"></a>Zaman uyumlu işlem

Aşağıdaki iki olası dönüş türleri olduğu zaman uyumlu eylemi göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

Ürün tarafından temsil edilen zaman içinde önceki eylemi, bir 404 durum kodu döndürülür `id` temel alınan veri deposunda mevcut değil. [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) Yardımcısı metodunu çağırmak için bir kısayol olarak `return new NotFoundResult();`. Ürünün mevcut değilse bir `Product` yükü temsil eden bir 200 durum kodu ile döndürülen nesne. [Tamam](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) yardımcı yöntem toplu biçiminin çağrıldığında `return new OkObjectResult(product);`.

### <a name="asynchronous-action"></a>Zaman uyumsuz eylem

İki olası dönüş türleri olduğu aşağıdaki zaman uyumsuz eylem göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Model doğrulama başarısız olduğunda önceki eylemi, bir 400 durum kodu döndürülür ve [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) yardımcı yöntemi çağrılır. Örneğin, şu model istekleri sağlamalısınız gösterir `Name` özelliği ve bir değer. Bu nedenle, uygun sağlamak için hata `Name` istekte model doğrulamasının başarısız olmasına neden olur.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

Önceki eyleme ait diğer bilinen dönüş kodu tarafından oluşturulan bir 201 olan [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) yardımcı yöntemi. Bu yol içerisindeki `Product` nesne döndürülür.

::: moniker range=">= aspnetcore-2.1"
## <a name="actionresultt-type"></a>ActionResult\<T > türü

ASP.NET Core 2.1 tanıtır [actionresult öğesini\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) dönüş türü Web API denetleyici eylemleri. Türetilen bir tür dönmek olanak tanır [actionresult öğesini](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) veya döndüren bir [belirli tür](#specific-type). `ActionResult<T>` üzerinde aşağıdaki faydaları sağlar [IActionResult türü](#iactionresult-type):

* [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) özniteliğin `Type` özelliği hariç tutulamaz.
* [Örtük dönüştürme işleçleri](/dotnet/csharp/language-reference/keywords/implicit) hem dönüştürülmesini desteklemek `T` ve `ActionResult` için `ActionResult<T>`. `T` dönüştürür [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), yani `return new ObjectResult(T);` için Basitleştirilmiş `return T;`.

Çoğu Eylemler, belirli bir dönüş türüne sahip. Beklenmeyen koşul içinde çalışması belirli tür döndürülmez eylem yürütme sırasında ortaya çıkabilir. Örneğin, bir eylemin giriş parametresi model doğrulama başarısız olabilir. Böyle bir durumda, uygun döndürmek için ortak `ActionResult` türü yerine belirli bir tür.

### <a name="synchronous-action"></a>Zaman uyumlu işlem

İki olası dönüş türleri olduğu zaman uyumlu bir eylem göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

Önceki kodda, ürün veritabanında mevcut değilse bir 404 durum kodu döndürülür. Ürünün mevcut, ilgili `Product` nesne döndürülür. ASP.NET Core 2.1 önce `return product;` satır olabilirdi `return Ok(product);`.

> [!TIP]
> Denetleyici sınıfı ile donatılmış ASP.NET Core 2.1 itibarıyla eylem parametresi bağlama kaynağı çıkarımı etkinleştirilir `[ApiController]` özniteliği. Bir rota şablonu adı ile eşleşen bir parametre adı istek rota verilerini kullanarak otomatik olarak bağlanır. Sonuç olarak, önceki eyleme ait `id` parametresine değil açıkça ile Açıklama [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) özniteliği.

### <a name="asynchronous-action"></a>Zaman uyumsuz eylem

İki olası dönüş türü olan bir zaman uyumsuz eylem göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

Model doğrulama başarısız olursa, [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) yöntemi, bir 400 durum kodunu döndürmek için çağrılır. [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) belirli doğrulama hatalarını içeren özellik kendisine geçirilir. Model doğrulama başarılı olursa, ürün veritabanında oluşturulur. 201 durum kodunu döndürdü.

> [!TIP]
> Denetleyici sınıfı ile donatılmış ASP.NET Core 2.1 itibarıyla eylem parametresi bağlama kaynağı çıkarımı etkinleştirilir `[ApiController]` özniteliği. Karmaşık tür parametreleri, istek gövdesi kullanarak otomatik olarak bağlanır. Sonuç olarak, önceki eyleme ait `product` parametresine değil açıkça ile Açıklama [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği.
::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Denetleyici eylemleri](xref:mvc/controllers/actions)
* [Model doğrulaması](xref:mvc/models/validation)
* [Swagger kullanan web API Yardım sayfaları](xref:tutorials/web-api-help-pages-using-swagger)
