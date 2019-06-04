---
title: ASP.NET Core Web API denetleyici eylemi dönüş türleri
author: scottaddie
description: Bir ASP.NET Core Web API'si çeşitli denetleyici eylem yönteminin dönüş türleri kullanma hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/04/2019
uid: web-api/action-return-types
ms.openlocfilehash: b89ead55cd46ef62a3bc28b1cfc9077d3ce9aba2
ms.sourcegitcommit: a04eb20e81243930ec829a9db5dd5de49f669450
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2019
ms.locfileid: "66470400"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="70d49-103">ASP.NET Core Web API denetleyici eylemi dönüş türleri</span><span class="sxs-lookup"><span data-stu-id="70d49-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="70d49-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="70d49-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="70d49-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="70d49-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="70d49-106">ASP.NET Core Web API denetleyici eylemi için aşağıdaki seçenekleri dönüş türleri sunar:</span><span class="sxs-lookup"><span data-stu-id="70d49-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="70d49-107">Belirli bir türü</span><span class="sxs-lookup"><span data-stu-id="70d49-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="70d49-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="70d49-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="70d49-109">ActionResult\<T ></span><span class="sxs-lookup"><span data-stu-id="70d49-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="70d49-110">Belirli bir türü</span><span class="sxs-lookup"><span data-stu-id="70d49-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="70d49-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="70d49-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="70d49-112">Bu belgede, her bir dönüş türü kullanmak en uygun olduğunda açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="70d49-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="70d49-113">Belirli bir türü</span><span class="sxs-lookup"><span data-stu-id="70d49-113">Specific type</span></span>

<span data-ttu-id="70d49-114">Bir basit veya karmaşık veri türü basit bir eylem döndürür (örneğin, `string` veya özel bir nesne).</span><span class="sxs-lookup"><span data-stu-id="70d49-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="70d49-115">Özel koleksiyonu döndüren eylem göz önünde bulundurun `Product` nesneler:</span><span class="sxs-lookup"><span data-stu-id="70d49-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="70d49-116">Eylem yürütme sırasında karşı korumak için bilinen koşul, belirli bir tür döndüren yeterli.</span><span class="sxs-lookup"><span data-stu-id="70d49-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="70d49-117">Önceki eylemi parametresi kısıtlamaları doğrulama gerekli değilse bu nedenle hiçbir parametre kabul eder.</span><span class="sxs-lookup"><span data-stu-id="70d49-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="70d49-118">Olduğunda bir uygulamada birden fazla dönüş yolları sunulan için katılması gereken koşullar bilinir.</span><span class="sxs-lookup"><span data-stu-id="70d49-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="70d49-119">Böyle bir durumda karıştırmak için ortak bir [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) dönüş türü basit veya karmaşık dönüş türüne sahip.</span><span class="sxs-lookup"><span data-stu-id="70d49-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="70d49-120">Her iki [IActionResult](#iactionresult-type) veya [actionresult öğesini\<T >](#actionresultt-type) bu tür bir eylemin uyum sağlamak gereklidir.</span><span class="sxs-lookup"><span data-stu-id="70d49-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="70d49-121">IActionResult türü</span><span class="sxs-lookup"><span data-stu-id="70d49-121">IActionResult type</span></span>

<span data-ttu-id="70d49-122">[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) dönüş türü, uygun olduğunda birden çok [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) dönüş türleri bir eylemi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="70d49-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="70d49-123">`ActionResult` Türleri çeşitli HTTP durum kodları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="70d49-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="70d49-124">Bu kategoriye dönülüyor bazı yaygın dönüş türleri [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) ve [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="70d49-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="70d49-125">Birden fazla dönüş türleri ve eylem, serbest yollarında kullanımını olduğundan [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) özniteliği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="70d49-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="70d49-126">Bu öznitelik gibi araçları tarafından oluşturulan API Yardım sayfaları daha açıklayıcı yanıt ayrıntılarını üretir [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="70d49-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="70d49-127">`[ProducesResponseType]` Eylem tarafından döndürülen HTTP durum kodları ve bilinen türleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="70d49-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="70d49-128">Zaman uyumlu işlem</span><span class="sxs-lookup"><span data-stu-id="70d49-128">Synchronous action</span></span>

<span data-ttu-id="70d49-129">Aşağıdaki iki olası dönüş türleri olduğu zaman uyumlu eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="70d49-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="70d49-130">Ürün tarafından temsil edilen zaman içinde önceki eylemi, bir 404 durum kodu döndürülür `id` temel alınan veri deposunda mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="70d49-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="70d49-131">[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) Yardımcısı metodunu çağırmak için bir kısayol olarak `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="70d49-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="70d49-132">Ürünün mevcut değilse bir `Product` yükü temsil eden bir 200 durum kodu ile döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="70d49-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="70d49-133">[Tamam](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) yardımcı yöntem toplu biçiminin çağrıldığında `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="70d49-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="70d49-134">Zaman uyumsuz eylem</span><span class="sxs-lookup"><span data-stu-id="70d49-134">Asynchronous action</span></span>

<span data-ttu-id="70d49-135">İki olası dönüş türleri olduğu aşağıdaki zaman uyumsuz eylem göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="70d49-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="70d49-136">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="70d49-136">In the preceding code:</span></span>

* <span data-ttu-id="70d49-137">Bir 400 durum kodu ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) ürün açıklaması "XYZ Widget" içerdiğinde ASP.NET Core çalışma zamanı tarafından döndürülür.</span><span class="sxs-lookup"><span data-stu-id="70d49-137">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when the product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="70d49-138">201 durum kodu tarafından oluşturulan [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) bir ürün oluşturulduğunda yöntemi.</span><span class="sxs-lookup"><span data-stu-id="70d49-138">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="70d49-139">Bu kod yolunda `Product` nesne döndürülür.</span><span class="sxs-lookup"><span data-stu-id="70d49-139">In this code path, the `Product` object is returned.</span></span>

<span data-ttu-id="70d49-140">Örneğin, şu model istekleri içermelidir gösterir `Name` ve `Description` özellikleri.</span><span class="sxs-lookup"><span data-stu-id="70d49-140">For example, the following model indicates that requests must include the `Name` and `Description` properties.</span></span> <span data-ttu-id="70d49-141">Bu nedenle, sunulamamasından `Name` ve `Description` istekte model doğrulamasının başarısız olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="70d49-141">Therefore, failure to provide `Name` and `Description` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="70d49-142">Varsa [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği ASP.NET Core 2.1 veya daha sonra uygulandığında, model doğrulama hataları 400 durum koduna neden.</span><span class="sxs-lookup"><span data-stu-id="70d49-142">If the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute in ASP.NET Core 2.1 or later  is applied, model validation errors result in a 400 status code.</span></span> <span data-ttu-id="70d49-143">Daha fazla bilgi için [otomatik HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="70d49-143">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="actionresultt-type"></a><span data-ttu-id="70d49-144">ActionResult\<T > türü</span><span class="sxs-lookup"><span data-stu-id="70d49-144">ActionResult\<T> type</span></span>

<span data-ttu-id="70d49-145">ASP.NET Core 2.1 tanıtır [actionresult öğesini\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) dönüş türü Web API denetleyici eylemleri.</span><span class="sxs-lookup"><span data-stu-id="70d49-145">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="70d49-146">Türetilen bir tür dönmek olanak tanır [actionresult öğesini](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) veya döndüren bir [belirli tür](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="70d49-146">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="70d49-147">`ActionResult<T>` üzerinde aşağıdaki faydaları sağlar [IActionResult türü](#iactionresult-type):</span><span class="sxs-lookup"><span data-stu-id="70d49-147">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="70d49-148">[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) özniteliğin `Type` özelliği hariç tutulamaz.</span><span class="sxs-lookup"><span data-stu-id="70d49-148">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="70d49-149">Örneğin, `[ProducesResponseType(200, Type = typeof(Product))]` için Basitleştirilmiş `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="70d49-149">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="70d49-150">Eylemin dönüş türü alanından çıkarılan yerine beklenen `T` içinde `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="70d49-150">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="70d49-151">[Örtük dönüştürme işleçleri](/dotnet/csharp/language-reference/keywords/implicit) hem dönüştürülmesini desteklemek `T` ve `ActionResult` için `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="70d49-151">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="70d49-152">`T` dönüştürür [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), yani `return new ObjectResult(T);` için Basitleştirilmiş `return T;`.</span><span class="sxs-lookup"><span data-stu-id="70d49-152">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="70d49-153">C# arabirimlerde örtük dönüştürme işleçleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="70d49-153">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="70d49-154">Sonuç olarak, arabirimin somut bir türe dönüştürmeyi kullanmak için gerekli `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="70d49-154">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="70d49-155">Örneğin, kullanım `IEnumerable` aşağıdaki örnekte çalışmıyor:</span><span class="sxs-lookup"><span data-stu-id="70d49-155">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="70d49-156">Önceki kodu düzeltmek için bir seçenek, döndürmektir `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="70d49-156">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="70d49-157">Çoğu Eylemler, belirli bir dönüş türüne sahip.</span><span class="sxs-lookup"><span data-stu-id="70d49-157">Most actions have a specific return type.</span></span> <span data-ttu-id="70d49-158">Beklenmeyen koşul içinde çalışması belirli tür döndürülmez eylem yürütme sırasında ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="70d49-158">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="70d49-159">Örneğin, bir eylemin giriş parametresi model doğrulama başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="70d49-159">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="70d49-160">Böyle bir durumda, uygun döndürmek için ortak `ActionResult` türü yerine belirli bir tür.</span><span class="sxs-lookup"><span data-stu-id="70d49-160">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="70d49-161">Zaman uyumlu işlem</span><span class="sxs-lookup"><span data-stu-id="70d49-161">Synchronous action</span></span>

<span data-ttu-id="70d49-162">İki olası dönüş türleri olduğu zaman uyumlu bir eylem göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="70d49-162">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=7,10)]

<span data-ttu-id="70d49-163">Önceki kodda, ürün veritabanında mevcut değilse bir 404 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="70d49-163">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="70d49-164">Ürünün mevcut, ilgili `Product` nesne döndürülür.</span><span class="sxs-lookup"><span data-stu-id="70d49-164">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="70d49-165">ASP.NET Core 2.1 önce `return product;` satır olabilirdi `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="70d49-165">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="70d49-166">Denetleyici sınıfı ile donatılmış ASP.NET Core 2.1 itibarıyla eylem parametresi bağlama kaynağı çıkarımı etkinleştirilir `[ApiController]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="70d49-166">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="70d49-167">Bir rota şablonu adı ile eşleşen bir parametre adı istek rota verilerini kullanarak otomatik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="70d49-167">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="70d49-168">Sonuç olarak, önceki eyleme ait `id` parametresine değil açıkça ile Açıklama [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="70d49-168">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="70d49-169">Zaman uyumsuz eylem</span><span class="sxs-lookup"><span data-stu-id="70d49-169">Asynchronous action</span></span>

<span data-ttu-id="70d49-170">İki olası dönüş türü olan bir zaman uyumsuz eylem göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="70d49-170">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="70d49-171">Yukarıdaki kodda:</span><span class="sxs-lookup"><span data-stu-id="70d49-171">In the preceding code:</span></span>

* <span data-ttu-id="70d49-172">Bir 400 durum kodu ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) ASP.NET Core çalışma zamanı tarafından döndürülen zaman:</span><span class="sxs-lookup"><span data-stu-id="70d49-172">A 400 status code ([BadRequest](xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*)) is returned by the ASP.NET Core runtime when:</span></span>
  * <span data-ttu-id="70d49-173">[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği uygulandı, model doğrulama başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="70d49-173">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute has been applied and model validation fails.</span></span>
  * <span data-ttu-id="70d49-174">Ürün açıklaması "XYZ Widget" içerir.</span><span class="sxs-lookup"><span data-stu-id="70d49-174">The product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="70d49-175">201 durum kodu tarafından oluşturulan [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) bir ürün oluşturulduğunda yöntemi.</span><span class="sxs-lookup"><span data-stu-id="70d49-175">A 201 status code is generated by the [CreatedAtAction](xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*) method when a product is created.</span></span> <span data-ttu-id="70d49-176">Bu kod yolunda `Product` nesne döndürülür.</span><span class="sxs-lookup"><span data-stu-id="70d49-176">In this code path, the `Product` object is returned.</span></span>

> [!TIP]
> <span data-ttu-id="70d49-177">Denetleyici sınıfı ile donatılmış ASP.NET Core 2.1 itibarıyla eylem parametresi bağlama kaynağı çıkarımı etkinleştirilir `[ApiController]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="70d49-177">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="70d49-178">Karmaşık tür parametreleri, istek gövdesi kullanarak otomatik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="70d49-178">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="70d49-179">Sonuç olarak, önceki eyleme ait `product` parametresine değil açıkça ile Açıklama [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="70d49-179">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="70d49-180">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="70d49-180">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
