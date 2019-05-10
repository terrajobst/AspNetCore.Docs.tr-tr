---
title: Web API kuralları kullanma
author: pranavkm
description: ASP.NET Core web API kuralları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: 671e39ded50e50bbfe6d12879895bee74367f754
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902870"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="136b8-103">Web API kuralları kullanma</span><span class="sxs-lookup"><span data-stu-id="136b8-103">Use web API conventions</span></span>

<span data-ttu-id="136b8-104">Tarafından [Pranav Krishnamoorthy](https://github.com/pranavkm) ve [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="136b8-104">By [Pranav Krishnamoorthy](https://github.com/pranavkm) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="136b8-105">ASP.NET Core 2.2 ve sonraki sürümleri içeren ortak ayıklamak için bir yol [API belgeleri](xref:tutorials/web-api-help-pages-using-swagger) ve birden fazla eylem, denetleyicileri veya derlemedeki tüm denetleyicileri uygulanır.</span><span class="sxs-lookup"><span data-stu-id="136b8-105">ASP.NET Core 2.2 and later includes a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="136b8-106">Web API kurallardır bireysel eylemleri dekorasyon yerine [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span><span class="sxs-lookup"><span data-stu-id="136b8-106">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span>

<span data-ttu-id="136b8-107">Bir kuralı sağlar:</span><span class="sxs-lookup"><span data-stu-id="136b8-107">A convention allows you to:</span></span>

* <span data-ttu-id="136b8-108">En yaygın dönüş türleri ve durum kodları belirli bir eylem türünden döndürülen tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="136b8-108">Define the most common return types and status codes returned from a specific type of action.</span></span>
* <span data-ttu-id="136b8-109">Tanımlanmış standart sapma eylemleri tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="136b8-109">Identify actions that deviate from the defined standard.</span></span>

<span data-ttu-id="136b8-110">ASP.NET Core MVC 2.2 ve sonraki sürümleri içeren bir dizi varsayılan kuralları <xref:Microsoft.AspNetCore.Mvc.DefaultApiConventions?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="136b8-110">ASP.NET Core MVC 2.2 and later includes a set of default conventions in <xref:Microsoft.AspNetCore.Mvc.DefaultApiConventions?displayProperty=fullName>.</span></span> <span data-ttu-id="136b8-111">Denetleyicide kurallarını temel alır (*ValuesController.cs*) içinde ASP.NET Core sağlanan **API** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="136b8-111">The conventions are based on the controller (*ValuesController.cs*) provided in the ASP.NET Core **API** project template.</span></span> <span data-ttu-id="136b8-112">Eylemlerinizi şablondaki desenleri izlerseniz, varsayılan kuralları kullanılarak başarılı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="136b8-112">If your actions follow the patterns in the template, you should be successful using the default conventions.</span></span> <span data-ttu-id="136b8-113">Varsayılan kuralları gereksinimlerinizi karşılamıyorsa bkz [web API'si kuralları oluşturma](#create-web-api-conventions).</span><span class="sxs-lookup"><span data-stu-id="136b8-113">If the default conventions don't meet your needs, see [Create web API conventions](#create-web-api-conventions).</span></span>

<span data-ttu-id="136b8-114">Çalışma zamanında, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> kurallarını anlar.</span><span class="sxs-lookup"><span data-stu-id="136b8-114">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="136b8-115">`ApiExplorer` ile iletişim kurmak için MVC'nin soyutlama [Openapı](https://www.openapis.org/) (diğer adıyla Swagger) belgesi üreteçleri.</span><span class="sxs-lookup"><span data-stu-id="136b8-115">`ApiExplorer` is MVC's abstraction to communicate with [OpenAPI](https://www.openapis.org/) (also known as Swagger) document generators.</span></span> <span data-ttu-id="136b8-116">Uygulanan kuralı öznitelikleri bir eylemle ilişkili ve eylemin Openapı belgelerinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="136b8-116">Attributes from the applied convention are associated with an action and are included in the action's OpenAPI documentation.</span></span> <span data-ttu-id="136b8-117">[API Çözümleyicileri](xref:web-api/advanced/analyzers) ayrıca kurallarını anlama.</span><span class="sxs-lookup"><span data-stu-id="136b8-117">[API analyzers](xref:web-api/advanced/analyzers) also understand conventions.</span></span> <span data-ttu-id="136b8-118">Eyleminizi sıra dışı ise (örneğin, belgelenen uygulanan kural gereği olmayan bir durum kodu döndürür), bir uyarı, durum kodu belgeye önerir.</span><span class="sxs-lookup"><span data-stu-id="136b8-118">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), a warning encourages you to document the status code.</span></span>

<span data-ttu-id="136b8-119">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="136b8-119">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="136b8-120">Web API kuralları uygula</span><span class="sxs-lookup"><span data-stu-id="136b8-120">Apply web API conventions</span></span>

<span data-ttu-id="136b8-121">Kuralları oluşturma yok; Her eylem, tam olarak bir kuralı ile ilişkilendirilmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="136b8-121">Conventions don't compose; each action may be associated with exactly one convention.</span></span> <span data-ttu-id="136b8-122">Daha özel kuralları üzerinde daha az belirgin kurallar önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="136b8-122">More specific conventions take precedence over less specific conventions.</span></span> <span data-ttu-id="136b8-123">İki veya daha fazla kurallar aynı öncelik için bir eylem uyguladığınızda seçim belirleyici değildir.</span><span class="sxs-lookup"><span data-stu-id="136b8-123">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="136b8-124">Bir eylem için geçerli bir kural için aşağıdaki seçenekler mevcuttur. en az belirli belirli:</span><span class="sxs-lookup"><span data-stu-id="136b8-124">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="136b8-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Bireysel eylemleri için geçerlidir ve kuralı türü ve geçerli kuralı yöntemini belirtir.</span><span class="sxs-lookup"><span data-stu-id="136b8-125">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span>

    <span data-ttu-id="136b8-126">Aşağıdaki örnekte, varsayılan kuralı türü 's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` kuralı yöntemi uygulanan `Update` eylem:</span><span class="sxs-lookup"><span data-stu-id="136b8-126">In the following example, the default convention type's `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionMethod&highlight=3)]

    <span data-ttu-id="136b8-127">`Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` Kuralı yöntemi aşağıdaki öznitelikleri eylemini uygular:</span><span class="sxs-lookup"><span data-stu-id="136b8-127">The `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` convention method applies the following attributes to the action:</span></span>

    ```csharp
    [ProducesDefaultResponseType]
    [ProducesResponseType(StatusCodes.Status204NoContent)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    ```

<span data-ttu-id="136b8-128">Daha fazla bilgi için `[ProducesDefaultResponseType]`, bkz: [varsayılan yanıt](https://swagger.io/docs/specification/describing-responses/#default).</span><span class="sxs-lookup"><span data-stu-id="136b8-128">For more information on `[ProducesDefaultResponseType]`, see [Default Response](https://swagger.io/docs/specification/describing-responses/#default).</span></span>

1. <span data-ttu-id="136b8-129">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir denetleyiciye uygulanan &mdash; belirtilen kural türü denetleyicinin tüm eylemleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="136b8-129">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the specified convention type to all actions on the controller.</span></span> <span data-ttu-id="136b8-130">Bir kural yöntem kuralı yöntemi uygulandığı eylemleri belirlemek ve ipuçları ile sunulur.</span><span class="sxs-lookup"><span data-stu-id="136b8-130">A convention method is decorated with hints that determine the actions to which the convention method applies.</span></span> <span data-ttu-id="136b8-131">İpuçları hakkında daha fazla bilgi için bkz. [web API'si kuralları oluşturma](#create-web-api-conventions)).</span><span class="sxs-lookup"><span data-stu-id="136b8-131">For more information on hints, see [Create web API conventions](#create-web-api-conventions)).</span></span>

    <span data-ttu-id="136b8-132">Aşağıdaki örnekte, tüm eylemler için varsayılan kurallar kümesini uygulanan *ContactsConventionController*:</span><span class="sxs-lookup"><span data-stu-id="136b8-132">In the following example, the default set of conventions is applied to all actions in *ContactsConventionController*:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=snippet_ApiConventionTypeAttribute&highlight=2)]

1. <span data-ttu-id="136b8-133">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir derlemeye uygulanan &mdash; belirtilen kural türü geçerli derlemedeki tüm denetleyicileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="136b8-133">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the specified convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="136b8-134">Bir öneri, derleme düzeyinde öznitelikler uygulamak *Startup.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="136b8-134">As a recommendation, apply assembly-level attributes in the *Startup.cs* file.</span></span>

    <span data-ttu-id="136b8-135">Aşağıdaki örnekte, varsayılan kuralları kümesi derlemedeki tüm denetleyicileri uygulanır:</span><span class="sxs-lookup"><span data-stu-id="136b8-135">In the following example, the default set of conventions is applied to all controllers in the assembly:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=snippet_ApiConventionTypeAttribute&highlight=1)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="136b8-136">Web API kuralları oluşturma</span><span class="sxs-lookup"><span data-stu-id="136b8-136">Create web API conventions</span></span>

<span data-ttu-id="136b8-137">Varsayılan API kurallarının gereksinimlerinizi karşılamıyorsa, kendi kuralları oluşturun.</span><span class="sxs-lookup"><span data-stu-id="136b8-137">If the default API conventions don't meet your needs, create your own conventions.</span></span> <span data-ttu-id="136b8-138">Bir kural aşağıdaki gibidir:</span><span class="sxs-lookup"><span data-stu-id="136b8-138">A convention is:</span></span>

* <span data-ttu-id="136b8-139">Yöntemleri statik türü.</span><span class="sxs-lookup"><span data-stu-id="136b8-139">A static type with methods.</span></span>
* <span data-ttu-id="136b8-140">Tanımlama yeteneği [yanıt türleri](#response-types) ve [adlandırma gereksinimlerini](#naming-requirements) eylemleri.</span><span class="sxs-lookup"><span data-stu-id="136b8-140">Capable of defining [response types](#response-types) and [naming requirements](#naming-requirements) on actions.</span></span>

### <a name="response-types"></a><span data-ttu-id="136b8-141">Yanıt türleri</span><span class="sxs-lookup"><span data-stu-id="136b8-141">Response types</span></span>

<span data-ttu-id="136b8-142">Bu yöntemleri ile açıklamalı olan `[ProducesResponseType]` veya `[ProducesDefaultResponseType]` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="136b8-142">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span> <span data-ttu-id="136b8-143">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="136b8-143">For example:</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="136b8-144">Daha özel meta veri öznitelikleri yoksa, bu kuralı bir derlemeye uygulamak, uygular:</span><span class="sxs-lookup"><span data-stu-id="136b8-144">If more specific metadata attributes are absent, applying this convention to an assembly enforces that:</span></span>

* <span data-ttu-id="136b8-145">Kuralı yöntemi adlı herhangi bir eylem için geçerli `Find`.</span><span class="sxs-lookup"><span data-stu-id="136b8-145">The convention method applies to any action named `Find`.</span></span>
* <span data-ttu-id="136b8-146">Adlı bir parametre `id` üzerinde mevcut olduğundan `Find` eylem.</span><span class="sxs-lookup"><span data-stu-id="136b8-146">A parameter named `id` is present on the `Find` action.</span></span>

### <a name="naming-requirements"></a><span data-ttu-id="136b8-147">Adlandırma gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="136b8-147">Naming requirements</span></span>

<span data-ttu-id="136b8-148">`[ApiConventionNameMatch]` Ve `[ApiConventionTypeMatch]` öznitelikler uygulandıkları eylemleri belirler kuralı yöntemi uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="136b8-148">The `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` attributes can be applied to the convention method that determines the actions to which they apply.</span></span> <span data-ttu-id="136b8-149">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="136b8-149">For example:</span></span>

```csharp
[ProducesResponseType(StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

<span data-ttu-id="136b8-150">Önceki örnekte:</span><span class="sxs-lookup"><span data-stu-id="136b8-150">In the preceding example:</span></span>

* <span data-ttu-id="136b8-151">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` Yönteminizden seçeneği, kuralı, "Bul" ile önek herhangi bir eylem eşleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="136b8-151">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method indicates that the convention matches any action prefixed with "Find".</span></span> <span data-ttu-id="136b8-152">Eşleşen eylemleri örnekler `Find`, `FindPet`, ve `FindById`.</span><span class="sxs-lookup"><span data-stu-id="136b8-152">Examples of matching actions include `Find`, `FindPet`, and `FindById`.</span></span>
* <span data-ttu-id="136b8-153">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` Uygulanan parametresi kuralı soneki tanımlayıcıda bitiş tam olarak bir parametreye sahip yöntemleri eşleştiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="136b8-153">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention matches methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="136b8-154">Örnekler parametreleri gibi `id` veya `petId`.</span><span class="sxs-lookup"><span data-stu-id="136b8-154">Examples include parameters such as `id` or `petId`.</span></span> <span data-ttu-id="136b8-155">`ApiConventionTypeMatch` parametre türü sınırlamak için türlerine benzer şekilde uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="136b8-155">`ApiConventionTypeMatch` can be similarly applied to types to constrain the parameter type.</span></span> <span data-ttu-id="136b8-156">A `params[]` bağımsız değişkenini açıkça eşlenmesi gerekmez kalan parametrelerle gösterir.</span><span class="sxs-lookup"><span data-stu-id="136b8-156">A `params[]` argument indicates remaining parameters that don't need to be explicitly matched.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="136b8-157">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="136b8-157">Additional resources</span></span>

* <xref:web-api/advanced/analyzers>
* <xref:tutorials/web-api-help-pages-using-swagger>
