---
title: ASP.NET Core Web API 'sindeki denetleyici eylemi dönüş türleri
author: scottaddie
description: ASP.NET Core Web API 'sindeki çeşitli denetleyici eylemi yöntemi döndürme türlerini kullanma hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 02/03/2020
uid: web-api/action-return-types
ms.openlocfilehash: 17e290d3aba4f724fcbd1693af371017c4d3f03a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655248"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a>ASP.NET Core Web API 'sindeki denetleyici eylemi dönüş türleri

[Scott Ade](https://github.com/scottaddie) tarafından

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

ASP.NET Core Web API denetleyicisi eylem dönüş türleri için aşağıdaki seçenekleri sunar:

::: moniker range=">= aspnetcore-2.1"

* [Belirli tür](#specific-type)
* [Iactionresult](#iactionresult-type)
* [ActionResult\<T >](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [Belirli tür](#specific-type)
* [Iactionresult](#iactionresult-type)

::: moniker-end

Bu belgede, her dönüş türünü kullanmak için en uygun olan bilgiler açıklanmaktadır.

## <a name="specific-type"></a>Belirli tür

En basit eylem, basit veya karmaşık bir veri türü döndürür (örneğin, `string` veya özel nesne türü). Özel `Product` nesnelerinin bir koleksiyonunu döndüren aşağıdaki eylemi göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

Eylem yürütme sırasında korunmak üzere bilinen koşullar olmadan, belirli bir türü döndürmek yeterli olabilir. Önceki eylem hiçbir parametre kabul etmez, bu nedenle parametre kısıtlamaları doğrulaması gerekli değildir.

Bir eylemde, bilinen koşulların ne zaman hesaba katılması gerektiği olduğunda, birden fazla dönüş yolu tanıtılmıştır. Böyle bir durumda, basit veya karmaşık dönüş türüyle <xref:Microsoft.AspNetCore.Mvc.ActionResult> bir dönüş türü karıştırmak yaygındır. Bu tür eyleme uyum sağlamak için [ıactionresult](#iactionresult-type) veya [ActionResult\<t >](#actionresultt-type) gereklidir.

### <a name="return-ienumerablet-or-iasyncenumerablet"></a>IEnumerable\<T > veya ıasyncenumerable\<T > döndürün

ASP.NET Core 2,2 ve önceki sürümlerde, bir eylemden <xref:System.Collections.Generic.IEnumerable%601> döndürmek seri hale getirici tarafından zaman uyumlu koleksiyon yinelemesi ile sonuçlanır. Sonuç olarak, çağrı engelleme ve iş parçacığı havuzu için olası bir olasılık vardır. Göstermek için, Web API 'sinin veri erişimi ihtiyaçları için Entity Framework (EF) Core kullanıldığını düşünün. Aşağıdaki eylemin dönüş türü serileştirme sırasında zaman uyumlu olarak numaralandırılır:

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

Zaman uyumlu numaralandırmayı önlemek ve engellemeyi ASP.NET Core 2,2 ve önceki sürümlerde veritabanı üzerinde bekleyip, `ToListAsync`çağırın:

```csharp
public async Task<IEnumerable<Product>> GetOnSaleProducts() =>
    await _context.Products.Where(p => p.IsOnSale).ToListAsync();
```

ASP.NET Core 3,0 ve üzeri sürümlerde `IAsyncEnumerable<T>` bir eylemden geri döndürülüyor:

* Zaman uyumlu yineleme ile sonuçlanmayacaktır.
* <xref:System.Collections.Generic.IEnumerable%601>döndüren etkin hale gelir.

ASP.NET Core 3,0 ve üzeri, serileştiriciye sağlamadan önce aşağıdaki eylemin sonucunu arabelleğe alır:

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

Zaman uyumsuz yinelemeyi garantilemek için eylem imzasının dönüş türünü `IAsyncEnumerable<T>` olarak bildirmeyi düşünün. Sonuç olarak, yineleme modu döndürülmekte olan temel somut türü temel alır. MVC, `IAsyncEnumerable<T>`uygulayan herhangi bir somut türü otomatik olarak arabelleğe alır.

`IEnumerable<Product>`olarak satış fiyatlı ürün kayıtları döndüren aşağıdaki eylemi göz önünde bulundurun:

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProducts)]

Önceki eylemin `IAsyncEnumerable<Product>` eşdeğeri şunlardır:

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProductsAsync)]

Yukarıdaki eylemlerin her ikisi de ASP.NET Core 3,0 itibariyle engellenmeyen bir işlem değildir.

## <a name="iactionresult-type"></a>Iactionresult türü

<xref:Microsoft.AspNetCore.Mvc.IActionResult> dönüş türü, bir eylemde birden çok `ActionResult` dönüş türü mümkün olduğunda uygundur. `ActionResult` türleri çeşitli HTTP durum kodlarını temsil eder. `ActionResult` türetilen Özet olmayan herhangi bir sınıf, geçerli bir dönüş türü olarak nitelendirir. Bu kategorideki bazı yaygın dönüş türleri <xref:Microsoft.AspNetCore.Mvc.BadRequestResult> (400), <xref:Microsoft.AspNetCore.Mvc.NotFoundResult> (404) ve <xref:Microsoft.AspNetCore.Mvc.OkObjectResult> (200). Alternatif olarak, <xref:Microsoft.AspNetCore.Mvc.ControllerBase> sınıfındaki kullanışlı yöntemler bir eylemden `ActionResult` türlerini döndürmek için kullanılabilir. Örneğin, `return BadRequest();` `return new BadRequestResult();`toplu bir biçimidir.

Bu tür bir eylemde birden çok dönüş türü ve yolu olduğundan, [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) özniteliğin serbest kullanımı gereklidir. Bu öznitelik, [Swagger](xref:tutorials/web-api-help-pages-using-swagger)gibi araçlar tarafından oluşturulan Web API Yardım sayfaları için daha açıklayıcı yanıt ayrıntıları üretir. `[ProducesResponseType]`, eylem tarafından döndürülecek bilinen türleri ve HTTP durum kodlarını gösterir.

### <a name="synchronous-action"></a>Zaman uyumlu eylem

İki olası dönüş türü olan aşağıdaki zaman uyumlu eylemi göz önünde bulundurun:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdIActionResult&highlight=8,11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

::: moniker-end

Önceki eylemde:

* Temel alınan veri deposunda `id` tarafından temsil edilen ürün yoksa 404 durum kodu döndürülür. <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> kolaylığı yöntemi `return new NotFoundResult();`için toplu olarak çağrılır.
* Ürün mevcut olduğunda `Product` nesnesi ile bir 200 durum kodu döndürülür. <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> kolaylığı yöntemi `return new OkObjectResult(product);`için toplu olarak çağrılır.

### <a name="asynchronous-action"></a>Zaman uyumsuz eylem

İki olası dönüş türü olan aşağıdaki zaman uyumsuz eylemi göz önünde bulundurun:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncIActionResult&highlight=9,14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=9,14)]

::: moniker-end

Önceki eylemde:

* Ürün açıklaması "XYZ pencere öğesi" içerdiğinde 400 durum kodu döndürülür. <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> kolaylığı yöntemi `return new BadRequestResult();`için toplu olarak çağrılır.
* Bir ürün oluşturulduğunda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> kullanışlı yöntem tarafından bir 201 durum kodu oluşturulur. `CreatedAtAction` çağırma alternatifi `return new CreatedAtActionResult(nameof(GetById), "Products", new { id = product.Id }, product);`. Bu kod yolunda, `Product` nesnesi yanıt gövdesinde sağlanır. Yeni oluşturulan ürünün URL 'sini içeren `Location` bir yanıt üst bilgisi sağlanır.

Örneğin, aşağıdaki model isteklerin `Name` ve `Description` özelliklerini içermesi gerektiğini gösterir. `Name` sağlama hatası ve istekte `Description`, model doğrulamasının başarısız olmasına neden oluyor.

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2,1 veya sonraki bir sürümde [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği uygulanırsa, model doğrulama hataları 400 durum koduna neden olur. Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).

## <a name="actionresultt-type"></a>ActionResult\<T > türü

ASP.NET Core 2,1, Web API denetleyicisi eylemleri için [\<t >](xref:Microsoft.AspNetCore.Mvc.ActionResult`1) geri dönüş türünü sunmuştur. <xref:Microsoft.AspNetCore.Mvc.ActionResult> türettikten veya [belirli bir tür](#specific-type)döndürmenize olanak sağlar. `ActionResult<T>`, [ıactionresult türü](#iactionresult-type)üzerinde aşağıdaki avantajları sunmaktadır:

* [`[ProducesResponseType]`](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) özniteliğin `Type` özelliği dışarıda bırakılabilirler. Örneğin, `[ProducesResponseType(200, Type = typeof(Product))]` `[ProducesResponseType(200)]`basitleştirilmiştir. Eylemin beklenen dönüş türü, `ActionResult<T>``T` çıkarsandı.
* [Örtük atama işleçleri](/dotnet/csharp/language-reference/keywords/implicit) hem `T` hem de `ActionResult<T>``ActionResult` dönüştürmeyi destekler. `T` <xref:Microsoft.AspNetCore.Mvc.ObjectResult>dönüştürür, bu, `return new ObjectResult(T);` `return T;`olarak basitleşdüğü anlamına gelir.

C#arabirimlerde örtük atama işleçlerini desteklemez. Sonuç olarak, `ActionResult<T>`kullanmak için arabirimin somut bir türe dönüştürülmesi gerekir. Örneğin, aşağıdaki örnekte `IEnumerable` kullanımı çalışmaz:

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get() =>
    _repository.GetProducts();
```

Önceki kodu gidermeye yönelik bir seçenek `_repository.GetProducts().ToList();`döndürmemelidir.

Çoğu eylemin belirli bir dönüş türü vardır. Eylem yürütme sırasında beklenmeyen koşullar gerçekleşebilir, bu durumda belirli tür döndürülmez. Örneğin, bir eylemin giriş parametresi, model doğrulamasının başarısız olmasına neden olabilir. Böyle bir durumda, belirli tür yerine uygun `ActionResult` türünü döndürmek yaygın bir durumdur.

### <a name="synchronous-action"></a>Zaman uyumlu eylem

İki olası dönüş türü olan zaman uyumlu bir eylem düşünün:

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdActionResult&highlight=8,11)]

Önceki eylemde:

* Ürün veritabanında mevcut olmadığında bir 404 durum kodu döndürülür.
* Ürün mevcut olduğunda karşılık gelen `Product` nesnesiyle bir 200 durum kodu döndürülür. 2,1 ASP.NET Core önce `return product;` satırının `return Ok(product);`olması gerekiyordu.

### <a name="asynchronous-action"></a>Zaman uyumsuz eylem

İki olası dönüş türü olan zaman uyumsuz bir eylem düşünün:

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncActionResult&highlight=9,14)]

Önceki eylemde:

* Bir 400 durum kodu (<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>), şu durumlarda ASP.NET Core çalışma zamanı tarafından döndürülür:
  * [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği uygulandı ve model doğrulaması başarısız oluyor.
  * Ürün açıklaması "XYZ pencere öğesi" içerir.
* Bir ürün oluşturulduğunda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> yöntemi tarafından 201 durum kodu oluşturulur. Bu kod yolunda, `Product` nesnesi yanıt gövdesinde sağlanır. Yeni oluşturulan ürünün URL 'sini içeren `Location` bir yanıt üst bilgisi sağlanır.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
