---
title: ASP.NET Core Web API denetleyici eylemi dönüş türleri
author: scottaddie
description: Bir ASP.NET Core Web API'si çeşitli denetleyici eylem yönteminin dönüş türleri kullanma hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2018
uid: web-api/action-return-types
ms.openlocfilehash: 84300eae4271c3ee4387be022c3576dc83e144eb
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207530"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="5b7d9-103">ASP.NET Core Web API denetleyici eylemi dönüş türleri</span><span class="sxs-lookup"><span data-stu-id="5b7d9-103">Controller action return types in ASP.NET Core Web API</span></span>

<span data-ttu-id="5b7d9-104">Tarafından [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5b7d9-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="5b7d9-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5b7d9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5b7d9-106">ASP.NET Core Web API denetleyici eylemi için aşağıdaki seçenekleri dönüş türleri sunar:</span><span class="sxs-lookup"><span data-stu-id="5b7d9-106">ASP.NET Core offers the following options for Web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="5b7d9-107">Belirli bir türü</span><span class="sxs-lookup"><span data-stu-id="5b7d9-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="5b7d9-108">IActionResult</span><span class="sxs-lookup"><span data-stu-id="5b7d9-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="5b7d9-109">ActionResult\<T ></span><span class="sxs-lookup"><span data-stu-id="5b7d9-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="5b7d9-110">Belirli bir türü</span><span class="sxs-lookup"><span data-stu-id="5b7d9-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="5b7d9-111">IActionResult</span><span class="sxs-lookup"><span data-stu-id="5b7d9-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="5b7d9-112">Bu belgede, her bir dönüş türü kullanmak en uygun olduğunda açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="5b7d9-113">Belirli bir türü</span><span class="sxs-lookup"><span data-stu-id="5b7d9-113">Specific type</span></span>

<span data-ttu-id="5b7d9-114">Bir basit veya karmaşık veri türü basit bir eylem döndürür (örneğin, `string` veya özel bir nesne).</span><span class="sxs-lookup"><span data-stu-id="5b7d9-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="5b7d9-115">Özel koleksiyonu döndüren eylem göz önünde bulundurun `Product` nesneler:</span><span class="sxs-lookup"><span data-stu-id="5b7d9-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="5b7d9-116">Eylem yürütme sırasında karşı korumak için bilinen koşul, belirli bir tür döndüren yeterli.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="5b7d9-117">Önceki eylemi parametresi kısıtlamaları doğrulama gerekli değilse bu nedenle hiçbir parametre kabul eder.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="5b7d9-118">Olduğunda bir uygulamada birden fazla dönüş yolları sunulan için katılması gereken koşullar bilinir.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="5b7d9-119">Böyle bir durumda karıştırmak için ortak bir [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) dönüş türü basit veya karmaşık dönüş türüne sahip.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-119">In such a case, it's common to mix an [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return type with the primitive or complex return type.</span></span> <span data-ttu-id="5b7d9-120">Her iki [IActionResult](#iactionresult-type) veya [actionresult öğesini\<T >](#actionresultt-type) bu tür bir eylemin uyum sağlamak gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="5b7d9-121">IActionResult türü</span><span class="sxs-lookup"><span data-stu-id="5b7d9-121">IActionResult type</span></span>

<span data-ttu-id="5b7d9-122">[IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) dönüş türü, uygun olduğunda birden çok [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) dönüş türleri bir eylemi mümkündür.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-122">The [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) return type is appropriate when multiple [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) return types are possible in an action.</span></span> <span data-ttu-id="5b7d9-123">`ActionResult` Türleri çeşitli HTTP durum kodları temsil eder.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-123">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="5b7d9-124">Bu kategoriye dönülüyor bazı yaygın dönüş türleri [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400) [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404) ve [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span><span class="sxs-lookup"><span data-stu-id="5b7d9-124">Some common return types falling into this category are [BadRequestResult](/dotnet/api/microsoft.aspnetcore.mvc.badrequestresult) (400), [NotFoundResult](/dotnet/api/microsoft.aspnetcore.mvc.notfoundresult) (404), and [OkObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.okobjectresult) (200).</span></span>

<span data-ttu-id="5b7d9-125">Birden fazla dönüş türleri ve eylem, serbest yollarında kullanımını olduğundan [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) özniteliği gereklidir.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-125">Because there are multiple return types and paths in the action, liberal use of the [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute.-ctor) attribute is necessary.</span></span> <span data-ttu-id="5b7d9-126">Bu öznitelik gibi araçları tarafından oluşturulan API Yardım sayfaları daha açıklayıcı yanıt ayrıntılarını üretir [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span><span class="sxs-lookup"><span data-stu-id="5b7d9-126">This attribute produces more descriptive response details for API help pages generated by tools like [Swagger](/aspnet/core/tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="5b7d9-127">`[ProducesResponseType]` Eylem tarafından döndürülen HTTP durum kodları ve bilinen türleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-127">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="5b7d9-128">Zaman uyumlu işlem</span><span class="sxs-lookup"><span data-stu-id="5b7d9-128">Synchronous action</span></span>

<span data-ttu-id="5b7d9-129">Aşağıdaki iki olası dönüş türleri olduğu zaman uyumlu eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5b7d9-129">Consider the following synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="5b7d9-130">Ürün tarafından temsil edilen zaman içinde önceki eylemi, bir 404 durum kodu döndürülür `id` temel alınan veri deposunda mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-130">In the preceding action, a 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="5b7d9-131">[NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) Yardımcısı metodunu çağırmak için bir kısayol olarak `return new NotFoundResult();`.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-131">The [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) helper method is invoked as a shortcut to `return new NotFoundResult();`.</span></span> <span data-ttu-id="5b7d9-132">Ürünün mevcut değilse bir `Product` yükü temsil eden bir 200 durum kodu ile döndürülen nesne.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-132">If the product does exist, a `Product` object representing the payload is returned with a 200 status code.</span></span> <span data-ttu-id="5b7d9-133">[Tamam](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) yardımcı yöntem toplu biçiminin çağrıldığında `return new OkObjectResult(product);`.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-133">The [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) helper method is invoked as the shorthand form of `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="5b7d9-134">Zaman uyumsuz eylem</span><span class="sxs-lookup"><span data-stu-id="5b7d9-134">Asynchronous action</span></span>

<span data-ttu-id="5b7d9-135">İki olası dönüş türleri olduğu aşağıdaki zaman uyumsuz eylem göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5b7d9-135">Consider the following asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="5b7d9-136">Model doğrulama başarısız olduğunda önceki eylemi, bir 400 durum kodu döndürülür ve [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) yardımcı yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-136">In the preceding action, a 400 status code is returned when model validation fails and the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) helper method is invoked.</span></span> <span data-ttu-id="5b7d9-137">Örneğin, şu model istekleri sağlamalısınız gösterir `Name` özelliği ve bir değer.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-137">For example, the following model indicates that requests must provide the `Name` property and a value.</span></span> <span data-ttu-id="5b7d9-138">Bu nedenle, uygun sağlamak için hata `Name` istekte model doğrulamasının başarısız olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-138">Therefore, failure to provide a proper `Name` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6)]

<span data-ttu-id="5b7d9-139">Önceki eyleme ait diğer bilinen dönüş kodu tarafından oluşturulan bir 201 olan [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) yardımcı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-139">The preceding action's other known return code is a 201, which is generated by the [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction) helper method.</span></span> <span data-ttu-id="5b7d9-140">Bu yol içerisindeki `Product` nesne döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-140">In this path, the `Product` object is returned.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="actionresultt-type"></a><span data-ttu-id="5b7d9-141">ActionResult\<T > türü</span><span class="sxs-lookup"><span data-stu-id="5b7d9-141">ActionResult\<T> type</span></span>

<span data-ttu-id="5b7d9-142">ASP.NET Core 2.1 tanıtır [actionresult öğesini\<T >](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) dönüş türü Web API denetleyici eylemleri.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-142">ASP.NET Core 2.1 introduces the [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1) return type for Web API controller actions.</span></span> <span data-ttu-id="5b7d9-143">Türetilen bir tür dönmek olanak tanır [actionresult öğesini](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) veya döndüren bir [belirli tür](#specific-type).</span><span class="sxs-lookup"><span data-stu-id="5b7d9-143">It enables you to return a type deriving from [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult) or return a [specific type](#specific-type).</span></span> <span data-ttu-id="5b7d9-144">`ActionResult<T>` üzerinde aşağıdaki faydaları sağlar [IActionResult türü](#iactionresult-type):</span><span class="sxs-lookup"><span data-stu-id="5b7d9-144">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="5b7d9-145">[[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) özniteliğin `Type` özelliği hariç tutulamaz.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-145">The [[ProducesResponseType]](/dotnet/api/microsoft.aspnetcore.mvc.producesresponsetypeattribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="5b7d9-146">Örneğin, `[ProducesResponseType(200, Type = typeof(Product))]` için Basitleştirilmiş `[ProducesResponseType(200)]`.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-146">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="5b7d9-147">Eylemin dönüş türü alanından çıkarılan yerine beklenen `T` içinde `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-147">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="5b7d9-148">[Örtük dönüştürme işleçleri](/dotnet/csharp/language-reference/keywords/implicit) hem dönüştürülmesini desteklemek `T` ve `ActionResult` için `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-148">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="5b7d9-149">`T` dönüştürür [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), yani `return new ObjectResult(T);` için Basitleştirilmiş `return T;`.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-149">`T` converts to [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult), which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="5b7d9-150">C# arabirimlerde örtük dönüştürme işleçleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-150">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="5b7d9-151">Sonuç olarak, arabirimin somut bir türe dönüştürmeyi kullanmak için gerekli `ActionResult<T>`.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-151">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="5b7d9-152">Örneğin, kullanım `IEnumerable` aşağıdaki örnekte çalışmıyor:</span><span class="sxs-lookup"><span data-stu-id="5b7d9-152">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get()
{
    return _repository.GetProducts();
}
```

<span data-ttu-id="5b7d9-153">Önceki kodu düzeltmek için bir seçenek, döndürmektir `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-153">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="5b7d9-154">Çoğu Eylemler, belirli bir dönüş türüne sahip.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-154">Most actions have a specific return type.</span></span> <span data-ttu-id="5b7d9-155">Beklenmeyen koşul içinde çalışması belirli tür döndürülmez eylem yürütme sırasında ortaya çıkabilir.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-155">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="5b7d9-156">Örneğin, bir eylemin giriş parametresi model doğrulama başarısız olabilir.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-156">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="5b7d9-157">Böyle bir durumda, uygun döndürmek için ortak `ActionResult` türü yerine belirli bir tür.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-157">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="5b7d9-158">Zaman uyumlu işlem</span><span class="sxs-lookup"><span data-stu-id="5b7d9-158">Synchronous action</span></span>

<span data-ttu-id="5b7d9-159">İki olası dönüş türleri olduğu zaman uyumlu bir eylem göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5b7d9-159">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

<span data-ttu-id="5b7d9-160">Önceki kodda, ürün veritabanında mevcut değilse bir 404 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-160">In the preceding code, a 404 status code is returned when the product doesn't exist in the database.</span></span> <span data-ttu-id="5b7d9-161">Ürünün mevcut, ilgili `Product` nesne döndürülür.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-161">If the product does exist, the corresponding `Product` object is returned.</span></span> <span data-ttu-id="5b7d9-162">ASP.NET Core 2.1 önce `return product;` satır olabilirdi `return Ok(product);`.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-162">Before ASP.NET Core 2.1, the `return product;` line would have been `return Ok(product);`.</span></span>

> [!TIP]
> <span data-ttu-id="5b7d9-163">Denetleyici sınıfı ile donatılmış ASP.NET Core 2.1 itibarıyla eylem parametresi bağlama kaynağı çıkarımı etkinleştirilir `[ApiController]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-163">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="5b7d9-164">Bir rota şablonu adı ile eşleşen bir parametre adı istek rota verilerini kullanarak otomatik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-164">A parameter name matching a name in the route template is automatically bound using the request route data.</span></span> <span data-ttu-id="5b7d9-165">Sonuç olarak, önceki eyleme ait `id` parametresine değil açıkça ile Açıklama [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-165">Consequently, the preceding action's `id` parameter isn't explicitly annotated with the [[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute) attribute.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="5b7d9-166">Zaman uyumsuz eylem</span><span class="sxs-lookup"><span data-stu-id="5b7d9-166">Asynchronous action</span></span>

<span data-ttu-id="5b7d9-167">İki olası dönüş türü olan bir zaman uyumsuz eylem göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="5b7d9-167">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=8,13)]

<span data-ttu-id="5b7d9-168">Model doğrulama başarısız olursa, [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) yöntemi, bir 400 durum kodunu döndürmek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-168">If model validation fails, the [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest#Microsoft_AspNetCore_Mvc_ControllerBase_BadRequest_Microsoft_AspNetCore_Mvc_ModelBinding_ModelStateDictionary_) method is invoked to return a 400 status code.</span></span> <span data-ttu-id="5b7d9-169">[ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) belirli doğrulama hatalarını içeren özellik kendisine geçirilir.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-169">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property containing the specific validation errors is passed to it.</span></span> <span data-ttu-id="5b7d9-170">Model doğrulama başarılı olursa, ürün veritabanında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-170">If model validation succeeds, the product is created in the database.</span></span> <span data-ttu-id="5b7d9-171">201 durum kodunu döndürdü.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-171">A 201 status code is returned.</span></span>

> [!TIP]
> <span data-ttu-id="5b7d9-172">Denetleyici sınıfı ile donatılmış ASP.NET Core 2.1 itibarıyla eylem parametresi bağlama kaynağı çıkarımı etkinleştirilir `[ApiController]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-172">As of ASP.NET Core 2.1, action parameter binding source inference is enabled when a controller class is decorated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="5b7d9-173">Karmaşık tür parametreleri, istek gövdesi kullanarak otomatik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-173">Complex type parameters are automatically bound using the request body.</span></span> <span data-ttu-id="5b7d9-174">Sonuç olarak, önceki eyleme ait `product` parametresine değil açıkça ile Açıklama [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="5b7d9-174">Consequently, the preceding action's `product` parameter isn't explicitly annotated with the [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5b7d9-175">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5b7d9-175">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
