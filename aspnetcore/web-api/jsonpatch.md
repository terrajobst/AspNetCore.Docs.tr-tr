---
title: ASP.NET Core Web API JsonPatch
author: tdykstra
description: Bir ASP.NET Core web API'si, JSON Patch isteklerinin nasıl işleneceğini öğrenin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/24/2019
uid: web-api/jsonpatch
ms.openlocfilehash: c5330f778ef202d366db5cdff5345d0db43a5c1b
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59013216"
---
# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="54a46-103">ASP.NET Core Web API JsonPatch</span><span class="sxs-lookup"><span data-stu-id="54a46-103">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="54a46-104">tarafından [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="54a46-104">By [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="54a46-105">Bu makalede, bir ASP.NET Core web API'si, JSON Patch isteklerinin nasıl işleneceğini açıklar.</span><span class="sxs-lookup"><span data-stu-id="54a46-105">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="54a46-106">PATCH HTTP istek yöntemi</span><span class="sxs-lookup"><span data-stu-id="54a46-106">PATCH HTTP request method</span></span>

<span data-ttu-id="54a46-107">KOY ve [düzeltme eki](https://tools.ietf.org/html/rfc5789) yöntemleri, mevcut bir kaynağı güncelleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="54a46-107">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="54a46-108">Aralarındaki fark, düzeltme eki yalnızca değişiklikleri belirtirken PUT tüm kaynak değiştirir ' dir.</span><span class="sxs-lookup"><span data-stu-id="54a46-108">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="54a46-109">JSON yaması</span><span class="sxs-lookup"><span data-stu-id="54a46-109">JSON Patch</span></span>

<span data-ttu-id="54a46-110">[JSON yaması](https://tools.ietf.org/html/rfc6902) bir kaynağa uygulanacak güncelleştirmeleri belirtmek için bir biçimidir.</span><span class="sxs-lookup"><span data-stu-id="54a46-110">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="54a46-111">Bir dizi JSON yama belgesi sahip *operations*.</span><span class="sxs-lookup"><span data-stu-id="54a46-111">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="54a46-112">Her bir işlemin belirli bir değişiklik türünü tanımlar, gibi bir dizi öğesine eklemek veya bir özellik değeri değiştirin.</span><span class="sxs-lookup"><span data-stu-id="54a46-112">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="54a46-113">Örneğin, aşağıdaki JSON belgelerini bir kaynak, kaynak ve patch işlemleri sonucu için JSON patch belgenin gösterir.</span><span class="sxs-lookup"><span data-stu-id="54a46-113">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="54a46-114">Kaynak örneği</span><span class="sxs-lookup"><span data-stu-id="54a46-114">Resource example</span></span>

[!code-csharp[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="54a46-115">JSON patch örneği</span><span class="sxs-lookup"><span data-stu-id="54a46-115">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="54a46-116">Yukarıdaki JSON:</span><span class="sxs-lookup"><span data-stu-id="54a46-116">In the preceding JSON:</span></span>

* <span data-ttu-id="54a46-117">`op` Özelliği işlem türünü belirtir.</span><span class="sxs-lookup"><span data-stu-id="54a46-117">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="54a46-118">`path` Güncelleştirilecek öğe özelliği belirtir.</span><span class="sxs-lookup"><span data-stu-id="54a46-118">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="54a46-119">`value` Özelliğinin yeni değeri sağlar.</span><span class="sxs-lookup"><span data-stu-id="54a46-119">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="54a46-120">Düzeltme eki sonra kaynak</span><span class="sxs-lookup"><span data-stu-id="54a46-120">Resource after patch</span></span>

<span data-ttu-id="54a46-121">Yukarıdaki JSON yama belgesi uyguladıktan sonra kaynak şöyledir:</span><span class="sxs-lookup"><span data-stu-id="54a46-121">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="54a46-122">Kaynak JSON yama belgesi uygulayarak yapılan değişiklikler atomiktir: listedeki herhangi bir işlem başarısız olursa, listedeki işlem uygulanır.</span><span class="sxs-lookup"><span data-stu-id="54a46-122">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="54a46-123">Yolu sözdizimi</span><span class="sxs-lookup"><span data-stu-id="54a46-123">Path syntax</span></span>

<span data-ttu-id="54a46-124">[Yolu](http://tools.ietf.org/html/rfc6901) işlem nesnesi özelliğine düzeyleri arasında eğik çizgi sahiptir.</span><span class="sxs-lookup"><span data-stu-id="54a46-124">The [path](http://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="54a46-125">Örneğin: `"/address/zipCode"`</span><span class="sxs-lookup"><span data-stu-id="54a46-125">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="54a46-126">Sıfır tabanlı dizin, dizi öğeleri belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="54a46-126">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="54a46-127">İlk öğesi `addresses` dizi konumunda olacak `/addresses/0`.</span><span class="sxs-lookup"><span data-stu-id="54a46-127">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="54a46-128">İçin `add` bir dizinin sonuna bir tire (-) yerine bir dizin numarasını kullanın: `/addresses/-`.</span><span class="sxs-lookup"><span data-stu-id="54a46-128">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="54a46-129">İşlemler</span><span class="sxs-lookup"><span data-stu-id="54a46-129">Operations</span></span>

<span data-ttu-id="54a46-130">Aşağıdaki tablo desteklenen gösterir işlemleri sınıfında tanımlandığı gibi [JSON Patch belirtimi](https://tools.ietf.org/html/rfc6902):</span><span class="sxs-lookup"><span data-stu-id="54a46-130">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="54a46-131">Çalışma</span><span class="sxs-lookup"><span data-stu-id="54a46-131">Operation</span></span>  | <span data-ttu-id="54a46-132">Notlar</span><span class="sxs-lookup"><span data-stu-id="54a46-132">Notes</span></span> |
|-----------|--------------------------------|
| `add`     | <span data-ttu-id="54a46-133">Bir özellik ya da dizi öğesini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="54a46-133">Add a property or array element.</span></span> <span data-ttu-id="54a46-134">Var olan bir özellik için: değeri ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="54a46-134">For existing property: set value.</span></span>|
| `remove`  | <span data-ttu-id="54a46-135">Bir özellik ya da dizi öğesini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="54a46-135">Remove a property or array element.</span></span> |
| `replace` | <span data-ttu-id="54a46-136">Aynı `remove` ardından `add` aynı konumda.</span><span class="sxs-lookup"><span data-stu-id="54a46-136">Same as `remove` followed by `add` at same location.</span></span> |
| `move`    | <span data-ttu-id="54a46-137">Aynı `remove` ardından kaynağından `add` değerini kullanarak bir kaynaktan bir hedefe.</span><span class="sxs-lookup"><span data-stu-id="54a46-137">Same as `remove` from source followed by `add` to destination using value from source.</span></span> |
| `copy`    | <span data-ttu-id="54a46-138">Aynı `add` değerini kullanarak bir kaynaktan bir hedefe.</span><span class="sxs-lookup"><span data-stu-id="54a46-138">Same as `add` to destination using value from source.</span></span> |
| `test`    | <span data-ttu-id="54a46-139">Başarılı durum kodu döndürür değerini `path` sağlanan = `value`.</span><span class="sxs-lookup"><span data-stu-id="54a46-139">Return success status code if value at `path` = provided `value`.</span></span>|

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="54a46-140">ASP.NET core'da JsonPatch</span><span class="sxs-lookup"><span data-stu-id="54a46-140">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="54a46-141">JSON yaması ASP.NET Core uygulaması sağlanan [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="54a46-141">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="54a46-142">Paket dahil [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span><span class="sxs-lookup"><span data-stu-id="54a46-142">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="54a46-143">Eylem yöntemi kodu</span><span class="sxs-lookup"><span data-stu-id="54a46-143">Action method code</span></span>

<span data-ttu-id="54a46-144">API denetleyicisi içinde bir eylem yöntemi için JSON Patch:</span><span class="sxs-lookup"><span data-stu-id="54a46-144">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="54a46-145">İle açıklanıyor `HttpPatch` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="54a46-145">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="54a46-146">Kabul eden bir `JsonPatchDocument<T>`, genellikle [FromBody] ile.</span><span class="sxs-lookup"><span data-stu-id="54a46-146">Accepts a `JsonPatchDocument<T>`, typically with [FromBody].</span></span>
* <span data-ttu-id="54a46-147">Çağrıları `ApplyTo` üzerindeki değişiklikleri uygulamak için yama belgesi.</span><span class="sxs-lookup"><span data-stu-id="54a46-147">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="54a46-148">Örnek buradadır:</span><span class="sxs-lookup"><span data-stu-id="54a46-148">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="54a46-149">Bu örnek uygulama kodundan aşağıdakilerle çalışır `Customer` modeli.</span><span class="sxs-lookup"><span data-stu-id="54a46-149">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="54a46-150">Örnek eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="54a46-150">The sample action method:</span></span>

* <span data-ttu-id="54a46-151">Oluşturur bir `Customer`.</span><span class="sxs-lookup"><span data-stu-id="54a46-151">Constructs a `Customer`.</span></span>
* <span data-ttu-id="54a46-152">Düzeltme eki uygular.</span><span class="sxs-lookup"><span data-stu-id="54a46-152">Applies the patch.</span></span>
* <span data-ttu-id="54a46-153">Yanıt gövdesinde sonucunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="54a46-153">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="54a46-154">Gerçek bir uygulamada kod bir veritabanı gibi bir depolama alanından verileri almak ve düzeltme eki uygulandıktan sonra veritabanı güncelleştirmesi.</span><span class="sxs-lookup"><span data-stu-id="54a46-154">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="54a46-155">Model durumu</span><span class="sxs-lookup"><span data-stu-id="54a46-155">Model state</span></span>

<span data-ttu-id="54a46-156">Önceki eylem yöntemi örneği bir aşırı yüklemesini çağırır `ApplyTo` , parametrelerinden biri olarak model durumunu alır.</span><span class="sxs-lookup"><span data-stu-id="54a46-156">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="54a46-157">Bu seçenek belirtilmişse, hata iletileri yanıtlarını alabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="54a46-157">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="54a46-158">Aşağıdaki örnek, bir 400 Hatalı istek yanıt gövdesinin gösterir. bir `test` işlemi:</span><span class="sxs-lookup"><span data-stu-id="54a46-158">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="54a46-159">Dinamik nesneler</span><span class="sxs-lookup"><span data-stu-id="54a46-159">Dynamic objects</span></span>

<span data-ttu-id="54a46-160">Eylem yöntemi aşağıda dinamik bir nesne için bir düzeltme eki uygulama işlemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="54a46-160">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="54a46-161">Ekleme işlemi</span><span class="sxs-lookup"><span data-stu-id="54a46-161">The add operation</span></span>

* <span data-ttu-id="54a46-162">Varsa `path` bir dizi öğesine işaret eder: tarafından belirtilen bir önce yeni bir öğe ekler `path`.</span><span class="sxs-lookup"><span data-stu-id="54a46-162">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="54a46-163">Varsa `path` gösteren bir özelliğe: özellik değeri ayarlar.</span><span class="sxs-lookup"><span data-stu-id="54a46-163">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="54a46-164">Varsa `path` varolmayan bir konuma işaret eder:</span><span class="sxs-lookup"><span data-stu-id="54a46-164">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="54a46-165">Düzeltme eki kaynağa dinamik bir nesne ise: bir özellik ekler.</span><span class="sxs-lookup"><span data-stu-id="54a46-165">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="54a46-166">Kaynak düzeltme eki için statik bir nesneyse: istek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="54a46-166">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="54a46-167">Aşağıdaki örnek yama belgesi değerini ayarlar `CustomerName` ve ekler bir `Order` nesnesinin sonuna `Orders` dizisi.</span><span class="sxs-lookup"><span data-stu-id="54a46-167">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="54a46-168">Kaldırma işlemi</span><span class="sxs-lookup"><span data-stu-id="54a46-168">The remove operation</span></span>

* <span data-ttu-id="54a46-169">Varsa `path` bir dizi öğesine işaret eder: öğeyi kaldırır.</span><span class="sxs-lookup"><span data-stu-id="54a46-169">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="54a46-170">Varsa `path` işaret eden bir özellik için:</span><span class="sxs-lookup"><span data-stu-id="54a46-170">If `path` points to a property:</span></span>
  * <span data-ttu-id="54a46-171">Kaynak düzeltme eki için dinamik bir nesne ise: özelliği kaldırır.</span><span class="sxs-lookup"><span data-stu-id="54a46-171">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="54a46-172">Kaynak düzeltme eki için statik bir nesne ise:</span><span class="sxs-lookup"><span data-stu-id="54a46-172">If resource to patch is a static object:</span></span> 
    * <span data-ttu-id="54a46-173">Özellik boş değer atanabilir ise: null olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="54a46-173">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="54a46-174">NULL olmayan bir özelliğidir, bu ayarlar `default<T>`.</span><span class="sxs-lookup"><span data-stu-id="54a46-174">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="54a46-175">Aşağıdaki örnek, düzeltme eki belge kümeleri `CustomerName` null ve silmeleri `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="54a46-175">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="54a46-176">Değiştirme işlemi</span><span class="sxs-lookup"><span data-stu-id="54a46-176">The replace operation</span></span>

<span data-ttu-id="54a46-177">Bu işlem işlevsel olarak aynıdır bir `remove` arkasından bir `add`.</span><span class="sxs-lookup"><span data-stu-id="54a46-177">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="54a46-178">Aşağıdaki örnek yama belgesi değerini ayarlar `CustomerName` ve değiştirir `Orders[0]`yeni bir `Order` nesne.</span><span class="sxs-lookup"><span data-stu-id="54a46-178">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="54a46-179">Taşıma işlemi</span><span class="sxs-lookup"><span data-stu-id="54a46-179">The move operation</span></span>

* <span data-ttu-id="54a46-180">Varsa `path` bir dizi öğesine işaret: kopyalar `from` konumunu öğesine `path` öğesi, daha sonra çalışan bir `remove` işlemi `from` öğesi.</span><span class="sxs-lookup"><span data-stu-id="54a46-180">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="54a46-181">Varsa `path` gösteren bir özelliğe: kopyalar değerini `from` özelliğini `path` özelliği, daha sonra çalışan bir `remove` işlemi `from` özelliği.</span><span class="sxs-lookup"><span data-stu-id="54a46-181">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="54a46-182">Varsa `path` işaret varolmayan bir özellik için:</span><span class="sxs-lookup"><span data-stu-id="54a46-182">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="54a46-183">Kaynak düzeltme eki için statik bir nesneyse: istek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="54a46-183">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="54a46-184">Yama kaynak dinamik bir nesne ise: kopyaları `from` özelliği tarafından belirtilen konuma `path`, sonra çalışan bir `remove` işlemi `from` özelliği.</span><span class="sxs-lookup"><span data-stu-id="54a46-184">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="54a46-185">Aşağıdaki örnek yama belgesi:</span><span class="sxs-lookup"><span data-stu-id="54a46-185">The following sample patch document:</span></span>

* <span data-ttu-id="54a46-186">Değerini kopyalar `Orders[0].OrderName` için `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="54a46-186">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="54a46-187">Kümeleri `Orders[0].OrderName` null.</span><span class="sxs-lookup"><span data-stu-id="54a46-187">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="54a46-188">Taşır `Orders[1]` için önce `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="54a46-188">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="54a46-189">Kopyalama işlemi</span><span class="sxs-lookup"><span data-stu-id="54a46-189">The copy operation</span></span>

<span data-ttu-id="54a46-190">Bu işlem işlevsel olarak aynıdır bir `move` son olmadan işlemi `remove` adım.</span><span class="sxs-lookup"><span data-stu-id="54a46-190">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="54a46-191">Aşağıdaki örnek yama belgesi:</span><span class="sxs-lookup"><span data-stu-id="54a46-191">The following sample patch document:</span></span>

* <span data-ttu-id="54a46-192">Değerini kopyalar `Orders[0].OrderName` için `CustomerName`.</span><span class="sxs-lookup"><span data-stu-id="54a46-192">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="54a46-193">Bir kopyasını ekler `Orders[1]` önce `Orders[0]`.</span><span class="sxs-lookup"><span data-stu-id="54a46-193">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="54a46-194">Test işlemi</span><span class="sxs-lookup"><span data-stu-id="54a46-194">The test operation</span></span>

<span data-ttu-id="54a46-195">Bu konumdaki değeri tarafından belirtilmişse `path` sağlanan değerinden farklı `value`, istek başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="54a46-195">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="54a46-196">Bu durumda bile yama belgesindeki tüm diğer işlemler, aksi takdirde başarılı olabilir tüm PATCH isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="54a46-196">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="54a46-197">`test` İşlemi bir eşzamanlılık çakışması olduğunda, bir güncelleştirme önlemek için yaygın olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="54a46-197">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="54a46-198">Aşağıdaki örnek yama belgesi etkisizdir başlangıç değeri oluşan `CustomerName` "John", test başarısız çünkü:</span><span class="sxs-lookup"><span data-stu-id="54a46-198">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="54a46-199">Kodu alma</span><span class="sxs-lookup"><span data-stu-id="54a46-199">Get the code</span></span>

<span data-ttu-id="54a46-200">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span><span class="sxs-lookup"><span data-stu-id="54a46-200">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="54a46-201">([Nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="54a46-201">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="54a46-202">Örnek test etmek için uygulamayı çalıştırın ve aşağıdaki ayarlarla HTTP istekleri göndermek için:</span><span class="sxs-lookup"><span data-stu-id="54a46-202">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="54a46-203">URL:</span><span class="sxs-lookup"><span data-stu-id="54a46-203">URL:</span></span> `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`
* <span data-ttu-id="54a46-204">HTTP yöntemi:</span><span class="sxs-lookup"><span data-stu-id="54a46-204">HTTP method:</span></span> `PATCH`
* <span data-ttu-id="54a46-205">Üst bilgi:</span><span class="sxs-lookup"><span data-stu-id="54a46-205">Header:</span></span> `Content-Type: application/json-patch+json`
* <span data-ttu-id="54a46-206">Gövdesi: JSON patch belge örneklerinden birini kopyalayıp *JSON* proje klasörü.</span><span class="sxs-lookup"><span data-stu-id="54a46-206">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="54a46-207">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="54a46-207">Additional resources</span></span>

* [<span data-ttu-id="54a46-208">IETF RFC 5789 düzeltme eki yöntemi belirtimi</span><span class="sxs-lookup"><span data-stu-id="54a46-208">IETF RFC 5789 PATCH method specification</span></span>](https://tools.ietf.org/html/rfc5789)
* [<span data-ttu-id="54a46-209">IETF RFC 6902 JSON Patch belirtimi</span><span class="sxs-lookup"><span data-stu-id="54a46-209">IETF RFC 6902 JSON Patch specification</span></span>](https://tools.ietf.org/html/rfc6902)
* [<span data-ttu-id="54a46-210">IETF RFC 6901 JSON Patch yolu biçim belirtimi</span><span class="sxs-lookup"><span data-stu-id="54a46-210">IETF RFC 6901 JSON Patch path format spec</span></span>](http://tools.ietf.org/html/rfc6901)
* <span data-ttu-id="54a46-211">[JSON yaması belgeleri](http://jsonpatch.com/).</span><span class="sxs-lookup"><span data-stu-id="54a46-211">[JSON Patch documentation](http://jsonpatch.com/).</span></span> <span data-ttu-id="54a46-212">JSON yaması belgeleri oluşturmak için kaynakların bağlantılarını içerir.</span><span class="sxs-lookup"><span data-stu-id="54a46-212">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="54a46-213">ASP.NET Core JSON Patch kaynak kodu</span><span class="sxs-lookup"><span data-stu-id="54a46-213">ASP.NET Core JSON Patch source code</span></span>](https://github.com/aspnet/AspNetCore/tree/master/src/Features/JsonPatch/src)
