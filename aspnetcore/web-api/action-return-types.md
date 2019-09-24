---
title: ASP.NET Core Web API 'sindeki denetleyici eylemi dönüş türleri
author: scottaddie
description: ASP.NET Core Web API 'sindeki çeşitli denetleyici eylemi yöntemi döndürme türlerini kullanma hakkında bilgi edinin.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/09/2019
uid: web-api/action-return-types
ms.openlocfilehash: 991265810324d6339ebf346ff9aa14c479112af9
ms.sourcegitcommit: fa61d882be9d0c48bd681f2efcb97e05522051d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71205761"
---
# <a name="controller-action-return-types-in-aspnet-core-web-api"></a><span data-ttu-id="d6ca2-103">ASP.NET Core Web API 'sindeki denetleyici eylemi dönüş türleri</span><span class="sxs-lookup"><span data-stu-id="d6ca2-103">Controller action return types in ASP.NET Core web API</span></span>

<span data-ttu-id="d6ca2-104">[Scott Ade](https://github.com/scottaddie) tarafından</span><span class="sxs-lookup"><span data-stu-id="d6ca2-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="d6ca2-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d6ca2-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/action-return-types/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d6ca2-106">ASP.NET Core Web API denetleyicisi eylem dönüş türleri için aşağıdaki seçenekleri sunar:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-106">ASP.NET Core offers the following options for web API controller action return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

* [<span data-ttu-id="d6ca2-107">Belirli tür</span><span class="sxs-lookup"><span data-stu-id="d6ca2-107">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="d6ca2-108">Iactionresult</span><span class="sxs-lookup"><span data-stu-id="d6ca2-108">IActionResult</span></span>](#iactionresult-type)
* [<span data-ttu-id="d6ca2-109">ActionResult\<T ></span><span class="sxs-lookup"><span data-stu-id="d6ca2-109">ActionResult\<T></span></span>](#actionresultt-type)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* [<span data-ttu-id="d6ca2-110">Belirli tür</span><span class="sxs-lookup"><span data-stu-id="d6ca2-110">Specific type</span></span>](#specific-type)
* [<span data-ttu-id="d6ca2-111">Iactionresult</span><span class="sxs-lookup"><span data-stu-id="d6ca2-111">IActionResult</span></span>](#iactionresult-type)

::: moniker-end

<span data-ttu-id="d6ca2-112">Bu belgede, her dönüş türünü kullanmak için en uygun olan bilgiler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-112">This document explains when it's most appropriate to use each return type.</span></span>

## <a name="specific-type"></a><span data-ttu-id="d6ca2-113">Belirli tür</span><span class="sxs-lookup"><span data-stu-id="d6ca2-113">Specific type</span></span>

<span data-ttu-id="d6ca2-114">En basit eylem, basit veya karmaşık bir veri türü döndürür (örneğin, `string` veya özel nesne türü).</span><span class="sxs-lookup"><span data-stu-id="d6ca2-114">The simplest action returns a primitive or complex data type (for example, `string` or a custom object type).</span></span> <span data-ttu-id="d6ca2-115">Özel `Product` nesneler koleksiyonunu döndüren aşağıdaki eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-115">Consider the following action, which returns a collection of custom `Product` objects:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_Get)]

<span data-ttu-id="d6ca2-116">Eylem yürütme sırasında korunmak üzere bilinen koşullar olmadan, belirli bir türü döndürmek yeterli olabilir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-116">Without known conditions to safeguard against during action execution, returning a specific type could suffice.</span></span> <span data-ttu-id="d6ca2-117">Önceki eylem hiçbir parametre kabul etmez, bu nedenle parametre kısıtlamaları doğrulaması gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-117">The preceding action accepts no parameters, so parameter constraints validation isn't needed.</span></span>

<span data-ttu-id="d6ca2-118">Bir eylemde, bilinen koşulların ne zaman hesaba katılması gerektiği olduğunda, birden fazla dönüş yolu tanıtılmıştır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-118">When known conditions need to be accounted for in an action, multiple return paths are introduced.</span></span> <span data-ttu-id="d6ca2-119">Böyle bir durumda, basit veya karmaşık dönüş türü ile bir <xref:Microsoft.AspNetCore.Mvc.ActionResult> dönüş türü karıştırmak yaygındır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-119">In such a case, it's common to mix an <xref:Microsoft.AspNetCore.Mvc.ActionResult> return type with the primitive or complex return type.</span></span> <span data-ttu-id="d6ca2-120">Bu tür eyleme uyum sağlamak için [ıactionresult](#iactionresult-type) veya [\<ActionResult T >](#actionresultt-type) gereklidir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-120">Either [IActionResult](#iactionresult-type) or [ActionResult\<T>](#actionresultt-type) are necessary to accommodate this type of action.</span></span>

### <a name="return-ienumerablet-or-iasyncenumerablet"></a><span data-ttu-id="d6ca2-121">IEnumerable\<t > veya ıasyncenumerable\<t > döndürün</span><span class="sxs-lookup"><span data-stu-id="d6ca2-121">Return IEnumerable\<T> or IAsyncEnumerable\<T></span></span>

<span data-ttu-id="d6ca2-122">ASP.NET Core 2,2 ve önceki sürümlerde, bir <xref:System.Collections.Generic.IAsyncEnumerable%601> eylemden geri dönmek seri hale getirici tarafından zaman uyumlu koleksiyon yinelemesi ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-122">In ASP.NET Core 2.2 and earlier, returning <xref:System.Collections.Generic.IAsyncEnumerable%601> from an action results in synchronous collection iteration by the serializer.</span></span> <span data-ttu-id="d6ca2-123">Sonuç olarak, çağrı engelleme ve iş parçacığı havuzu için olası bir olasılık vardır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-123">The result is the blocking of calls and a potential for thread pool starvation.</span></span> <span data-ttu-id="d6ca2-124">Göstermek için, Web API 'sinin veri erişimi ihtiyaçları için Entity Framework (EF) Core kullanıldığını düşünün.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-124">To illustrate, imagine that Entity Framework (EF) Core is being used for the web API's data access needs.</span></span> <span data-ttu-id="d6ca2-125">Aşağıdaki eylemin dönüş türü serileştirme sırasında zaman uyumlu olarak numaralandırılır:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-125">The following action's return type is synchronously enumerated during serialization:</span></span>

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

<span data-ttu-id="d6ca2-126">Zaman uyumlu numaralandırmayı önlemek ve engellemeyi ASP.NET Core 2,2 ve önceki sürümlerde veritabanı üzerinde bekleyip engellemek için şunu `ToListAsync`çağırın:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-126">To avoid synchronous enumeration and blocking waits on the database in ASP.NET Core 2.2 and earlier, invoke `ToListAsync`:</span></span>

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale).ToListAsync();
```

<span data-ttu-id="d6ca2-127">ASP.NET Core 3,0 ve üzeri sürümlerde, bir `IAsyncEnumerable<T>` eylemden geri dönerek:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-127">In ASP.NET Core 3.0 and later, returning `IAsyncEnumerable<T>` from an action:</span></span>

* <span data-ttu-id="d6ca2-128">Zaman uyumlu yineleme ile sonuçlanmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-128">No longer results in synchronous iteration.</span></span>
* <span data-ttu-id="d6ca2-129">Dönerek <xref:System.Collections.Generic.IEnumerable%601>etkin hale gelir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-129">Becomes as efficient as returning <xref:System.Collections.Generic.IEnumerable%601>.</span></span>

<span data-ttu-id="d6ca2-130">ASP.NET Core 3,0 ve üzeri, serileştiriciye sağlamadan önce aşağıdaki eylemin sonucunu arabelleğe alır:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-130">ASP.NET Core 3.0 and later buffers the result of the following action before providing it to the serializer:</span></span>

```csharp
public IEnumerable<Product> GetOnSaleProducts() =>
    _context.Products.Where(p => p.IsOnSale);
```

<span data-ttu-id="d6ca2-131">Zaman uyumsuz yinelemeyi güvence altına almak `IAsyncEnumerable<T>` için eylem imzasının dönüş türünü bildirmeyi göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-131">Consider declaring the action signature's return type as `IAsyncEnumerable<T>` to guarantee the asynchronous iteration.</span></span> <span data-ttu-id="d6ca2-132">Sonuç olarak, yineleme modu döndürülmekte olan temel somut türü temel alır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-132">Ultimately, the iteration mode is based on the underlying concrete type being returned.</span></span> <span data-ttu-id="d6ca2-133">MVC, uygulayan `IAsyncEnumerable<T>`tüm somut türleri otomatik olarak arabelleğe alır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-133">MVC automatically buffers any concrete type that implements `IAsyncEnumerable<T>`.</span></span>

<span data-ttu-id="d6ca2-134">Şu şekilde `IEnumerable<Product>`satış fiyatlı ürün kayıtları döndüren aşağıdaki eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-134">Consider the following action, which returns sale-priced product records as `IEnumerable<Product>`:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProducts)]

<span data-ttu-id="d6ca2-135">Önceki `IAsyncEnumerable<Product>` eylemin eşdeğeri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-135">The `IAsyncEnumerable<Product>` equivalent of the preceding action is:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/3x/WebApiSample.Api.30/Controllers/ProductsController.cs?name=snippet_GetOnSaleProductsAsync)]

<span data-ttu-id="d6ca2-136">Yukarıdaki eylemlerin her ikisi de ASP.NET Core 3,0 itibariyle engellenmeyen bir işlem değildir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-136">Both of the preceding actions are non-blocking as of ASP.NET Core 3.0.</span></span>

## <a name="iactionresult-type"></a><span data-ttu-id="d6ca2-137">Iactionresult türü</span><span class="sxs-lookup"><span data-stu-id="d6ca2-137">IActionResult type</span></span>

<span data-ttu-id="d6ca2-138">Bir <xref:Microsoft.AspNetCore.Mvc.IActionResult> eylemde birden çok `ActionResult` dönüş türü mümkünse, dönüş türü uygun olur.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-138">The <xref:Microsoft.AspNetCore.Mvc.IActionResult> return type is appropriate when multiple `ActionResult` return types are possible in an action.</span></span> <span data-ttu-id="d6ca2-139">`ActionResult` Türler çeşitli http durum kodlarını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-139">The `ActionResult` types represent various HTTP status codes.</span></span> <span data-ttu-id="d6ca2-140">Herhangi bir soyut olmayan sınıf, geçerli `ActionResult` bir dönüş türü olarak niteleyen ' dan türetiliyor.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-140">Any non-abstract class deriving from `ActionResult` qualifies as a valid return type.</span></span> <span data-ttu-id="d6ca2-141">Bu kategorideki bazı yaygın dönüş türleri şunlardır <xref:Microsoft.AspNetCore.Mvc.BadRequestResult> (400), <xref:Microsoft.AspNetCore.Mvc.NotFoundResult> (404) ve <xref:Microsoft.AspNetCore.Mvc.OkObjectResult> (200).</span><span class="sxs-lookup"><span data-stu-id="d6ca2-141">Some common return types in this category are <xref:Microsoft.AspNetCore.Mvc.BadRequestResult> (400), <xref:Microsoft.AspNetCore.Mvc.NotFoundResult> (404), and <xref:Microsoft.AspNetCore.Mvc.OkObjectResult> (200).</span></span> <span data-ttu-id="d6ca2-142">Alternatif olarak, <xref:Microsoft.AspNetCore.Mvc.ControllerBase> sınıftaki kullanışlı yöntemler bir eylemden tür döndürmek `ActionResult` için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-142">Alternatively, convenience methods in the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class can be used to return `ActionResult` types from an action.</span></span> <span data-ttu-id="d6ca2-143">Örneğin, `return BadRequest();` öğesinin `return new BadRequestResult();`bir toplu biçimidir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-143">For example, `return BadRequest();` is a shorthand form of `return new BadRequestResult();`.</span></span>

<span data-ttu-id="d6ca2-144">Bu tür bir eylemde birden çok dönüş türü ve yolu olduğundan, [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) özniteliğinin serbest bir şekilde kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-144">Because there are multiple return types and paths in this type of action, liberal use of the [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) attribute is necessary.</span></span> <span data-ttu-id="d6ca2-145">Bu öznitelik, [Swagger](xref:tutorials/web-api-help-pages-using-swagger)gibi araçlar tarafından oluşturulan Web API Yardım sayfaları için daha açıklayıcı yanıt ayrıntıları üretir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-145">This attribute produces more descriptive response details for web API help pages generated by tools like [Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span></span> <span data-ttu-id="d6ca2-146">`[ProducesResponseType]`eylem tarafından döndürülecek bilinen türleri ve HTTP durum kodlarını gösterir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-146">`[ProducesResponseType]` indicates the known types and HTTP status codes to be returned by the action.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="d6ca2-147">Zaman uyumlu eylem</span><span class="sxs-lookup"><span data-stu-id="d6ca2-147">Synchronous action</span></span>

<span data-ttu-id="d6ca2-148">İki olası dönüş türü olan aşağıdaki zaman uyumlu eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-148">Consider the following synchronous action in which there are two possible return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdIActionResult&highlight=8,11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_GetById&highlight=8,11)]

::: moniker-end

<span data-ttu-id="d6ca2-149">Önceki eylemde:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-149">In the preceding action:</span></span>

* <span data-ttu-id="d6ca2-150">Temel alınan veri deposunda tarafından `id` temsil edilen ürün olmadığında 404 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-150">A 404 status code is returned when the product represented by `id` doesn't exist in the underlying data store.</span></span> <span data-ttu-id="d6ca2-151">Kolaylık yöntemi için `return new NotFoundResult();`toplu olarak çağrılır. <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*></span><span class="sxs-lookup"><span data-stu-id="d6ca2-151">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> convenience method is invoked as shorthand for `return new NotFoundResult();`.</span></span>
* <span data-ttu-id="d6ca2-152">Ürün mevcut olduğunda `Product` nesneyle bir 200 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-152">A 200 status code is returned with the `Product` object when the product does exist.</span></span> <span data-ttu-id="d6ca2-153">Kolaylık yöntemi için `return new OkObjectResult(product);`toplu olarak çağrılır. <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*></span><span class="sxs-lookup"><span data-stu-id="d6ca2-153">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> convenience method is invoked as shorthand for `return new OkObjectResult(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="d6ca2-154">Zaman uyumsuz eylem</span><span class="sxs-lookup"><span data-stu-id="d6ca2-154">Asynchronous action</span></span>

<span data-ttu-id="d6ca2-155">İki olası dönüş türü olan aşağıdaki zaman uyumsuz eylemi göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-155">Consider the following asynchronous action in which there are two possible return types:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncIActionResult&highlight=9,14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.Pre21/Controllers/ProductsController.cs?name=snippet_CreateAsync&highlight=9,14)]

::: moniker-end

<span data-ttu-id="d6ca2-156">Önceki eylemde:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-156">In the preceding action:</span></span>

* <span data-ttu-id="d6ca2-157">Ürün açıklaması "XYZ pencere öğesi" içerdiğinde 400 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-157">A 400 status code is returned when the product description contains "XYZ Widget".</span></span> <span data-ttu-id="d6ca2-158">Kolaylık yöntemi için `return new BadRequestResult();`toplu olarak çağrılır. <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*></span><span class="sxs-lookup"><span data-stu-id="d6ca2-158">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*> convenience method is invoked as shorthand for `return new BadRequestResult();`.</span></span>
* <span data-ttu-id="d6ca2-159">Bir ürün oluşturulduğunda 201 durum kodu <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> kolaylık yöntemi tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-159">A 201 status code is generated by the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> convenience method when a product is created.</span></span> <span data-ttu-id="d6ca2-160">`CreatedAtAction` Çağırmak`return new CreatedAtActionResult(nameof(GetById), "Products", new { id = product.Id }, product);`için bir alternatiftir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-160">An alternative to calling `CreatedAtAction` is `return new CreatedAtActionResult(nameof(GetById), "Products", new { id = product.Id }, product);`.</span></span> <span data-ttu-id="d6ca2-161">Bu kod yolunda, `Product` nesne yanıt gövdesinde sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-161">In this code path, the `Product` object is provided in the response body.</span></span> <span data-ttu-id="d6ca2-162">Yeni `Location` oluşturulan ürünün URL 'sini içeren bir yanıt üst bilgisi sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-162">A `Location` response header containing the newly created product's URL is provided.</span></span>

<span data-ttu-id="d6ca2-163">Örneğin, aşağıdaki model isteklerin `Name` ve `Description` özelliklerini içermesi gerektiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-163">For example, the following model indicates that requests must include the `Name` and `Description` properties.</span></span> <span data-ttu-id="d6ca2-164">İstek `Description` içinde sağlama `Name` hatası, model doğrulamasının başarısız olmasına neden olur.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-164">Failure to provide `Name` and `Description` in the request causes model validation to fail.</span></span>

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.DataAccess/Models/Product.cs?name=snippet_ProductClass&highlight=5-6,8-9)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d6ca2-165">ASP.NET Core 2,1 veya sonraki bir sürümde [[Apicontroller]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği uygulanmışsa, model doğrulama hataları 400 durum koduna neden olur.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-165">If the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute in ASP.NET Core 2.1 or later is applied, model validation errors result in a 400 status code.</span></span> <span data-ttu-id="d6ca2-166">Daha fazla bilgi için bkz. [OTOMATIK HTTP 400 yanıtları](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="d6ca2-166">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="actionresultt-type"></a><span data-ttu-id="d6ca2-167">ActionResult\<T > türü</span><span class="sxs-lookup"><span data-stu-id="d6ca2-167">ActionResult\<T> type</span></span>

<span data-ttu-id="d6ca2-168">ASP.NET Core 2,1, Web API denetleyicisi eylemleri için [ActionResult\<T >](xref:Microsoft.AspNetCore.Mvc.ActionResult`1) dönüş türünü sunmuştur.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-168">ASP.NET Core 2.1 introduced the [ActionResult\<T>](xref:Microsoft.AspNetCore.Mvc.ActionResult`1) return type for web API controller actions.</span></span> <span data-ttu-id="d6ca2-169"><xref:Microsoft.AspNetCore.Mvc.ActionResult> Belirli bir [türden](#specific-type)türetilen veya döndürülen bir tür döndürmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-169">It enables you to return a type deriving from <xref:Microsoft.AspNetCore.Mvc.ActionResult> or return a [specific type](#specific-type).</span></span> <span data-ttu-id="d6ca2-170">`ActionResult<T>`[ıactionresult türü](#iactionresult-type)üzerinde aşağıdaki avantajları sunar:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-170">`ActionResult<T>` offers the following benefits over the [IActionResult type](#iactionresult-type):</span></span>

* <span data-ttu-id="d6ca2-171">[[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) özniteliğinin `Type` özelliği dışarıda bırakılabilirler.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-171">The [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute) attribute's `Type` property can be excluded.</span></span> <span data-ttu-id="d6ca2-172">Örneğin, `[ProducesResponseType(200, Type = typeof(Product))]` `[ProducesResponseType(200)]`basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-172">For example, `[ProducesResponseType(200, Type = typeof(Product))]` is simplified to `[ProducesResponseType(200)]`.</span></span> <span data-ttu-id="d6ca2-173">Eylemin beklenen dönüş türü, `T` `ActionResult<T>`yerine öğesinden çıkarsanamıyor.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-173">The action's expected return type is instead inferred from the `T` in `ActionResult<T>`.</span></span>
* <span data-ttu-id="d6ca2-174">[Örtük atama işleçleri](/dotnet/csharp/language-reference/keywords/implicit) hem hem de `T` `ActionResult` için `ActionResult<T>`dönüştürmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-174">[Implicit cast operators](/dotnet/csharp/language-reference/keywords/implicit) support the conversion of both `T` and `ActionResult` to `ActionResult<T>`.</span></span> <span data-ttu-id="d6ca2-175">`T`öğesine <xref:Microsoft.AspNetCore.Mvc.ObjectResult>dönüştürür, yani `return new ObjectResult(T);` basitleştirilmiştir `return T;`.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-175">`T` converts to <xref:Microsoft.AspNetCore.Mvc.ObjectResult>, which means `return new ObjectResult(T);` is simplified to `return T;`.</span></span>

<span data-ttu-id="d6ca2-176">C#arabirimlerde örtük atama işleçlerini desteklemez.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-176">C# doesn't support implicit cast operators on interfaces.</span></span> <span data-ttu-id="d6ca2-177">Sonuç olarak, kullanmak `ActionResult<T>`için arabirimin somut bir türe dönüştürülmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-177">Consequently, conversion of the interface to a concrete type is necessary to use `ActionResult<T>`.</span></span> <span data-ttu-id="d6ca2-178">Örneğin, aşağıdaki örnekte öğesinin `IEnumerable` kullanımı işe yaramaz:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-178">For example, use of `IEnumerable` in the following example doesn't work:</span></span>

```csharp
[HttpGet]
public ActionResult<IEnumerable<Product>> Get() =>
    _repository.GetProducts();
```

<span data-ttu-id="d6ca2-179">Önceki kodu gidermeye yönelik bir seçenek, geri dönelim `_repository.GetProducts().ToList();`.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-179">One option to fix the preceding code is to return `_repository.GetProducts().ToList();`.</span></span>

<span data-ttu-id="d6ca2-180">Çoğu eylemin belirli bir dönüş türü vardır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-180">Most actions have a specific return type.</span></span> <span data-ttu-id="d6ca2-181">Eylem yürütme sırasında beklenmeyen koşullar gerçekleşebilir, bu durumda belirli tür döndürülmez.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-181">Unexpected conditions can occur during action execution, in which case the specific type isn't returned.</span></span> <span data-ttu-id="d6ca2-182">Örneğin, bir eylemin giriş parametresi, model doğrulamasının başarısız olmasına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-182">For example, an action's input parameter may fail model validation.</span></span> <span data-ttu-id="d6ca2-183">Böyle bir durumda, belirli tür yerine uygun `ActionResult` türü döndürmek yaygın bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-183">In such a case, it's common to return the appropriate `ActionResult` type instead of the specific type.</span></span>

### <a name="synchronous-action"></a><span data-ttu-id="d6ca2-184">Zaman uyumlu eylem</span><span class="sxs-lookup"><span data-stu-id="d6ca2-184">Synchronous action</span></span>

<span data-ttu-id="d6ca2-185">İki olası dönüş türü olan zaman uyumlu bir eylem düşünün:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-185">Consider a synchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_GetByIdActionResult&highlight=8,11)]

<span data-ttu-id="d6ca2-186">Önceki eylemde:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-186">In the preceding action:</span></span>

* <span data-ttu-id="d6ca2-187">Ürün veritabanında mevcut olmadığında bir 404 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-187">A 404 status code is returned when the product doesn't exist in the database.</span></span>
* <span data-ttu-id="d6ca2-188">Ürün mevcut olduğunda ilgili `Product` nesneyle bir 200 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-188">A 200 status code is returned with the corresponding `Product` object when the product does exist.</span></span> <span data-ttu-id="d6ca2-189">2,1 `return product;` ASP.NET Core önce satırın olması `return Ok(product);`gerekiyordu.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-189">Before ASP.NET Core 2.1, the `return product;` line had to be `return Ok(product);`.</span></span>

### <a name="asynchronous-action"></a><span data-ttu-id="d6ca2-190">Zaman uyumsuz eylem</span><span class="sxs-lookup"><span data-stu-id="d6ca2-190">Asynchronous action</span></span>

<span data-ttu-id="d6ca2-191">İki olası dönüş türü olan zaman uyumsuz bir eylem düşünün:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-191">Consider an asynchronous action in which there are two possible return types:</span></span>

[!code-csharp[](../web-api/action-return-types/samples/2x/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_CreateAsyncActionResult&highlight=9,14)]

<span data-ttu-id="d6ca2-192">Önceki eylemde:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-192">In the preceding action:</span></span>

* <span data-ttu-id="d6ca2-193">ASP.NET Core çalışma zamanı tarafından bir<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>400 durum kodu () döndürülür:</span><span class="sxs-lookup"><span data-stu-id="d6ca2-193">A 400 status code (<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>) is returned by the ASP.NET Core runtime when:</span></span>
  * <span data-ttu-id="d6ca2-194">[[Apicontroller]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği uygulandı ve model doğrulaması başarısız oluyor.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-194">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute has been applied and model validation fails.</span></span>
  * <span data-ttu-id="d6ca2-195">Ürün açıklaması "XYZ pencere öğesi" içerir.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-195">The product description contains "XYZ Widget".</span></span>
* <span data-ttu-id="d6ca2-196">Bir ürün oluşturulduğunda <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> yöntemi tarafından bir 201 durum kodu oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-196">A 201 status code is generated by the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method when a product is created.</span></span> <span data-ttu-id="d6ca2-197">Bu kod yolunda, `Product` nesne yanıt gövdesinde sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-197">In this code path, the `Product` object is provided in the response body.</span></span> <span data-ttu-id="d6ca2-198">Yeni `Location` oluşturulan ürünün URL 'sini içeren bir yanıt üst bilgisi sağlanır.</span><span class="sxs-lookup"><span data-stu-id="d6ca2-198">A `Location` response header containing the newly created product's URL is provided.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d6ca2-199">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d6ca2-199">Additional resources</span></span>

* <xref:mvc/controllers/actions>
* <xref:mvc/models/validation>
* <xref:tutorials/web-api-help-pages-using-swagger>
