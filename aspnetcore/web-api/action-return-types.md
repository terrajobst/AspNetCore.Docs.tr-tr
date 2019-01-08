---
title: ASP.NET Core Web API denetleyici eylemi dönüş türleri
author: scottaddie
description: Bir ASP.NET Core Web API'si çeşitli denetleyici eylem yönteminin dönüş türleri kullanma hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/04/2019
uid: web-api/action-return-types
ms.openlocfilehash: 98d70e0379d353cff98a6d7a13f2dd00eb4da206
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098746"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>ASP.NET Core Web API denetleyici eylemi dönüş türleri

Tarafından [Scott Addie](https://github.com/scottaddie)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

ASP.NET Core Web API denetleyici eylemi için aşağıdaki seçenekleri dönüş türleri sunar:

::: moniker range=">= aspnetcore-2.1"

* [Belirli bir türü](#specific-type)
* [IActionResult](#iactionresult-type)
* [ActionResult\<T >](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [Belirli bir türü](#specific-type)
* [IActionResult](#iactionresult-type)

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

Yukarıdaki kodda:

* Bir 400 durum kodu ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) ürün açıklaması "XYZ Widget" içerdiğinde ASP.NET Core çalışma zamanı tarafından döndürülür.
* 201 durum kodu tarafından oluşturulan [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) bir ürün oluşturulduğunda yöntemi. Bu kod yolunda `Product` nesne döndürülür.

Örneğin, şu model istekleri içermelidir gösterir `Name` ve `Description` özellikleri. Bu nedenle, sunulamamasından `Name` ve `Description` istekte model doğrulamasının başarısız olmasına neden olur.

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

Varsa [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği ASP.NET Core 2.1 veya daha sonra uygulandığında, model doğrulama hataları 400 durum koduna neden. Daha fazla bilgi için [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).

## <a name="actionresultt-type"></a>ActionResult\<T > türü

ASP.NET Core 2.1 tanıtır [actionresult öğesini\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) dönüş türü Web API denetleyici eylemleri. Türetilen bir tür dönmek olanak tanır [actionresult öğesini](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) veya döndüren bir [belirli tür](#specific-type). `ActionResult<T>` üzerinde aşağıdaki faydaları sağlar [IActionResult türü](#iactionresult-type):

* [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) özniteliğin `Type` özelliği hariç tutulamaz. Örneğin, `[ProducesResponseType(200, Type = typeof(Product))]` için Basitleştirilmiş `[ProducesResponseType(200)]`. Eylemin dönüş türü alanından çıkarılan yerine beklenen `T` içinde `ActionResult<T>`.
* [Örtük dönüştürme işleçleri](/dotnet/csharp/language-reference/keywords/implicit) hem dönüştürülmesini desteklemek `T` ve `ActionResult` için `ActionResult<T>`. `T` dönüştürür [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), yani `return new ObjectResult(T);` için Basitleştirilmiş `return T;`.

C# arabirimlerde örtük dönüştürme işleçleri desteklemez. Sonuç olarak, arabirimin somut bir türe dönüştürmeyi kullanmak için gerekli `ActionResult<T>`. Örneğin, kullanım `IEnumerable` aşağıdaki örnekte çalışmıyor:

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

Önceki kodu düzeltmek için bir seçenek, döndürmektir `_repository.GetProducts().ToList();`.

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

Yukarıdaki kodda:

* Bir 400 durum kodu ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) ASP.NET Core çalışma zamanı tarafından döndürülen zaman:
  * [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği uygulandı, model doğrulama başarısız olur.
  * Ürün açıklaması "XYZ Widget" içerir.
* 201 durum kodu tarafından oluşturulan [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) bir ürün oluşturulduğunda yöntemi. Bu kod yolunda `Product` nesne döndürülür.

> [!TIP]
> Denetleyici sınıfı ile donatılmış ASP.NET Core 2.1 itibarıyla eylem parametresi bağlama kaynağı çıkarımı etkinleştirilir `[ApiController]` özniteliği. Karmaşık tür parametreleri, istek gövdesi kullanarak otomatik olarak bağlanır. Sonuç olarak, önceki eyleme ait `product` parametresine değil açıkça ile Açıklama [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
