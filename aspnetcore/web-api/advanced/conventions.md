---
title: Web API kuralları kullanma
author: pranavkm
description: ASP.NET Core web API kuralları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 481e3810f1e1aca40e0ee1ce3da6c67dc9d841f4
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425113"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="d231b-103">Web API kuralları kullanma</span><span class="sxs-lookup"><span data-stu-id="d231b-103">Use web API conventions</span></span>

<span data-ttu-id="d231b-104">Tarafından [Pranav Krishnamoorthy](https://github.com/pranavkm) ve [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="d231b-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="d231b-105">ASP.NET Core 2.2 ve sonraki sürümleri içeren ortak ayıklamak için bir yol [API belgeleri](xref:tutorials/web-api-help-pages-using-swagger) ve birden fazla eylem, denetleyicileri veya derlemedeki tüm denetleyicileri uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d231b-105">ASP.NET Core 2.2 and later includes a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="d231b-106">Web API kurallardır bireysel eylemleri dekorasyon yerine [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span><span class="sxs-lookup"><span data-stu-id="d231b-106">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span>

<span data-ttu-id="d231b-107">Bir kuralı sağlar:</span><span class="sxs-lookup"><span data-stu-id="d231b-107">A convention allows you to:</span></span>

* <span data-ttu-id="d231b-108">En yaygın dönüş türleri ve durum kodları belirli bir eylem türünden döndürülen tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="d231b-108">Define the most common return types and status codes returned from a specific type of action.</span></span>
* <span data-ttu-id="d231b-109">Tanımlanmış standart sapma eylemleri tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="d231b-109">Identify actions that deviate from the defined standard.</span></span>

<span data-ttu-id="d231b-110">ASP.NET Core MVC 2.2 ve sonraki sürümleri içeren bir dizi varsayılan kuralları `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span><span class="sxs-lookup"><span data-stu-id="d231b-110">ASP.NET Core MVC 2.2 and later includes a set of default conventions in `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="d231b-111">Denetleyicide kurallarını temel alır (*ValuesController.cs*) içinde ASP.NET Core sağlanan **API** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="d231b-111">The conventions are based on the controller (*ValuesController.cs*) provided in the ASP.NET Core **API** project template.</span></span> <span data-ttu-id="d231b-112">Eylemlerinizi şablondaki desenleri izlerseniz, varsayılan kuralları kullanılarak başarılı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d231b-112">If your actions follow the patterns in the template, you should be successful using the default conventions.</span></span> <span data-ttu-id="d231b-113">Varsayılan kuralları gereksinimlerinizi karşılamıyorsa bkz [web API'si kuralları oluşturma](#create-web-api-conventions).</span><span class="sxs-lookup"><span data-stu-id="d231b-113">If the default conventions don't meet your needs, see [Create web API conventions](#create-web-api-conventions).</span></span>

<span data-ttu-id="d231b-114">Çalışma zamanında, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> kurallarını anlar.</span><span class="sxs-lookup"><span data-stu-id="d231b-114">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="d231b-115">`ApiExplorer` ile iletişim kurmak için MVC'nin soyutlama [Openapı](https://www.openapis.org/) (diğer adıyla Swagger) belgesi üreteçleri.</span><span class="sxs-lookup"><span data-stu-id="d231b-115">`ApiExplorer` is MVC's abstraction to communicate with [OpenAPI](https://www.openapis.org/) (also known as Swagger) document generators.</span></span> <span data-ttu-id="d231b-116">Uygulanan kuralı öznitelikleri bir eylemle ilişkili ve eylemin Openapı belgelerinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="d231b-116">Attributes from the applied convention are associated with an action and are included in the action's OpenAPI documentation.</span></span> <span data-ttu-id="d231b-117">[API Çözümleyicileri](xref:web-api/advanced/analyzers) ayrıca kurallarını anlama.</span><span class="sxs-lookup"><span data-stu-id="d231b-117">[API analyzers](xref:web-api/advanced/analyzers) also understand conventions.</span></span> <span data-ttu-id="d231b-118">Eyleminizi sıra dışı ise (örneğin, belgelenen uygulanan kural gereği olmayan bir durum kodu döndürür), bir uyarı, durum kodu belgeye önerir.</span><span class="sxs-lookup"><span data-stu-id="d231b-118">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), a warning encourages you to document the status code.</span></span>

<span data-ttu-id="d231b-119">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d231b-119">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="d231b-120">Web API kuralları uygula</span><span class="sxs-lookup"><span data-stu-id="d231b-120">Apply web API conventions</span></span>

<span data-ttu-id="d231b-121">Kuralları oluşturma yok; Her eylem, tam olarak bir kuralı ile ilişkilendirilmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="d231b-121">Conventions don't compose; each action may be associated with exactly one convention.</span></span> <span data-ttu-id="d231b-122">Daha özel kuralları üzerinde daha az belirgin kurallar önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="d231b-122">More specific conventions take precedence over less specific conventions.</span></span> <span data-ttu-id="d231b-123">İki veya daha fazla kurallar aynı öncelik için bir eylem uyguladığınızda seçim belirleyici değildir.</span><span class="sxs-lookup"><span data-stu-id="d231b-123">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="d231b-124">Bir eylem için geçerli bir kural için aşağıdaki seçenekler mevcuttur. en az belirli belirli:</span><span class="sxs-lookup"><span data-stu-id="d231b-124">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="d231b-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Bireysel eylemleri için geçerlidir ve kuralı türü ve geçerli kuralı yöntemini belirtir.</span><span class="sxs-lookup"><span data-stu-id="d231b-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span>

    <span data-ttu-id="d231b-126">Aşağıdaki örnekte, varsayılan kuralı türü 's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` kuralı yöntemi uygulanan `Update` eylem:</span><span class="sxs-lookup"><span data-stu-id="d231b-126">In the following example, the default convention type's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    <span data-ttu-id="d231b-127">`Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` Kuralı yöntemi aşağıdaki öznitelikleri eylemini uygular:</span><span class="sxs-lookup"><span data-stu-id="d231b-127">The `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method applies the following attributes to the action:</span></span>

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(204)]
    [ProducesResponseType(404)]
    [ProducesResponseType(400)]
    ```

1. <span data-ttu-id="d231b-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir denetleyiciye uygulanan &mdash; belirtilen kural türü denetleyicinin tüm eylemleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="d231b-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the specified convention type to all actions on the controller.</span></span> <span data-ttu-id="d231b-129">Bir kural yöntem kuralı yöntemi uygulandığı eylemleri belirlemek ve ipuçları ile sunulur.</span><span class="sxs-lookup"><span data-stu-id="d231b-129">A convention method is decorated with hints that determine the actions to which the convention method applies.</span></span> <span data-ttu-id="d231b-130">İpuçları hakkında daha fazla bilgi için bkz. [web API'si kuralları oluşturma](#create-web-api-conventions)).</span><span class="sxs-lookup"><span data-stu-id="d231b-130">For more information on hints, see [Create web API conventions](#create-web-api-conventions)).</span></span>

    <span data-ttu-id="d231b-131">Aşağıdaki örnekte, tüm eylemler için varsayılan kurallar kümesini uygulanan *ContactsConventionController*:</span><span class="sxs-lookup"><span data-stu-id="d231b-131">In the following example, the default set of conventions is applied to all actions in *ContactsConventionController*:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. <span data-ttu-id="d231b-132">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir derlemeye uygulanan &mdash; belirtilen kural türü geçerli derlemedeki tüm denetleyicileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="d231b-132">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the specified convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="d231b-133">Bir öneri olarak, derleme düzeyinde öznitelikler için geçerli `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d231b-133">As a recommendation, apply assembly-level attributes to the `Startup` class.</span></span>

    <span data-ttu-id="d231b-134">Aşağıdaki örnekte, varsayılan kuralları kümesi derlemedeki tüm denetleyicileri uygulanır:</span><span class="sxs-lookup"><span data-stu-id="d231b-134">In the following example, the default set of conventions is applied to all controllers in the assembly:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="d231b-135">Web API kuralları oluşturma</span><span class="sxs-lookup"><span data-stu-id="d231b-135">Create web API conventions</span></span>

<span data-ttu-id="d231b-136">Varsayılan API kurallarının gereksinimlerinizi karşılamıyorsa, kendi kuralları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d231b-136">If the default API conventions don't meet your needs, create your own conventions.</span></span> <span data-ttu-id="d231b-137">Bir kural aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="d231b-137">A convention is:</span></span>

* <span data-ttu-id="d231b-138">Yöntemleri statik türü.</span><span class="sxs-lookup"><span data-stu-id="d231b-138">A static type with methods.</span></span>
* <span data-ttu-id="d231b-139">Tanımlama yeteneği [yanıt türleri](#response-types) ve [adlandırma gereksinimlerini](#naming-requirements) eylemleri.</span><span class="sxs-lookup"><span data-stu-id="d231b-139">Capable of defining [response types](#response-types) and [naming requirements](#naming-requirements) on actions.</span></span>

### <a name="response-types"></a><span data-ttu-id="d231b-140">Yanıt türleri</span><span class="sxs-lookup"><span data-stu-id="d231b-140">Response types</span></span>

<span data-ttu-id="d231b-141">Bu yöntemleri ile açıklamalı olan `[ProducesResponseType]` veya `[ProducesDefaultResponseType]` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="d231b-141">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span> <span data-ttu-id="d231b-142">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d231b-142">For example:</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="d231b-143">Daha özel meta veri öznitelikleri yoksa, bu kuralı bir derlemeye uygulamak, uygular:</span><span class="sxs-lookup"><span data-stu-id="d231b-143">If more specific metadata attributes are absent, applying this convention to an assembly enforces that:</span></span>

* <span data-ttu-id="d231b-144">Kuralı yöntemi adlı herhangi bir eylem için geçerli `Find`.</span><span class="sxs-lookup"><span data-stu-id="d231b-144">The convention method applies to any action named `Find`.</span></span>
* <span data-ttu-id="d231b-145">Adlı bir parametre `id` üzerinde mevcut olduğundan `Find` eylem.</span><span class="sxs-lookup"><span data-stu-id="d231b-145">A parameter named `id` is present on the `Find` action.</span></span>

### <a name="naming-requirements"></a><span data-ttu-id="d231b-146">Adlandırma gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="d231b-146">Naming requirements</span></span>

<span data-ttu-id="d231b-147">`[ApiConventionNameMatch]` Ve `[ApiConventionTypeMatch]` öznitelikler uygulandıkları eylemleri belirler kuralı yöntemi uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="d231b-147">The `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` attributes can be applied to the convention method that determines the actions to which they apply.</span></span> <span data-ttu-id="d231b-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="d231b-148">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

<span data-ttu-id="d231b-149">Önceki örnekte:</span><span class="sxs-lookup"><span data-stu-id="d231b-149">In the preceding example:</span></span>

* <span data-ttu-id="d231b-150">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` Yönteminizden seçeneği, kuralı, "Bul" ile önek herhangi bir eylem eşleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d231b-150">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method indicates that the convention matches any action prefixed with "Find".</span></span> <span data-ttu-id="d231b-151">Eşleşen eylemleri örnekler `Find`, `FindPet`, ve `FindById`.</span><span class="sxs-lookup"><span data-stu-id="d231b-151">Examples of matching actions include `Find`, `FindPet`, and `FindById`.</span></span>
* <span data-ttu-id="d231b-152">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` Uygulanan parametresi kuralı soneki tanımlayıcıda bitiş tam olarak bir parametreye sahip yöntemleri eşleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="d231b-152">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention matches methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="d231b-153">Örnekler parametreleri gibi `id` veya `petId`.</span><span class="sxs-lookup"><span data-stu-id="d231b-153">Examples include parameters such as `id` or `petId`.</span></span> <span data-ttu-id="d231b-154">`ApiConventionTypeMatch` parametre türü sınırlamak için türlerine benzer şekilde uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="d231b-154">`ApiConventionTypeMatch` can be similarly applied to types to constrain the parameter type.</span></span> <span data-ttu-id="d231b-155">A `params[]` bağımsız değişkenini açıkça eşlenmesi gerekmez kalan parametrelerle gösterir.</span><span class="sxs-lookup"><span data-stu-id="d231b-155">A `params[]` argument indicates remaining parameters that don't need to be explicitly matched.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d231b-156">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d231b-156">Additional resources</span></span>

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
