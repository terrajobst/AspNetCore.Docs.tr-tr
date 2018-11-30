---
title: Web API kuralları kullanma
author: pranavkm
description: ASP.NET Core web API kuralları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710081"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="2231e-103">Web API kuralları kullanma</span><span class="sxs-lookup"><span data-stu-id="2231e-103">Use web API conventions</span></span>

<span data-ttu-id="2231e-104">ASP.NET Core 2.2 ortak ayıklamak için bir yol sunar [API belgeleri](xref:tutorials/web-api-help-pages-using-swagger) ve birden fazla eylem, denetleyicileri veya derlemedeki tüm denetleyicileri uygulanır.</span><span class="sxs-lookup"><span data-stu-id="2231e-104">ASP.NET Core 2.2 introduces a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="2231e-105">Web API kurallardır bireysel eylemleri dekorasyon yerine [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span><span class="sxs-lookup"><span data-stu-id="2231e-105">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span> <span data-ttu-id="2231e-106">En yaygın "Geleneksel" dönüş türleri ve, eylem için eylem uygulayan kuralı yöntemi seçmek için bir yol ile iade durum kodları tanımlamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2231e-106">It allows you to define the most common "conventional" return types and status codes that you return from your action with a way to select the convention method that applies to an action.</span></span>

<span data-ttu-id="2231e-107">Varsayılan olarak, ASP.NET Core MVC 2.2 varsayılan kuralları, bir dizi ile birlikte gelen `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span><span class="sxs-lookup"><span data-stu-id="2231e-107">By default, ASP.NET Core MVC 2.2 ships with a set of default conventions, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="2231e-108">ASP.NET Core iskele oluşturulduğunu denetleyicisinde kurallarını temel alır.</span><span class="sxs-lookup"><span data-stu-id="2231e-108">The conventions are based on the controller that ASP.NET Core scaffolds.</span></span> <span data-ttu-id="2231e-109">Eylemlerinizi desenini izler, yapı iskelesi üretir, varsayılan kuralları kullanılarak başarılı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2231e-109">If your actions follow the pattern that scaffolding produces, you should be successful using the default conventions.</span></span>

<span data-ttu-id="2231e-110">Çalışma zamanında, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> kurallarını anlar.</span><span class="sxs-lookup"><span data-stu-id="2231e-110">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="2231e-111">`ApiExplorer` Açık API belge oluşturucular ile iletişim kurmak için MVC'nin soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="2231e-111">`ApiExplorer` is MVC's abstraction to communicate with Open API document generators.</span></span> <span data-ttu-id="2231e-112">Uygulanan kuralı öznitelikleri, bir eylem ile ilişkili alın ve eylemin Swagger belgelerinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="2231e-112">Attributes from the applied convention get associated with an action and are included in the action's Swagger documentation.</span></span> <span data-ttu-id="2231e-113">API Çözümleyicileri de kuralları anlayın.</span><span class="sxs-lookup"><span data-stu-id="2231e-113">API analyzers also understand conventions.</span></span> <span data-ttu-id="2231e-114">Eyleminizi sıra dışı ise (örneğin, belgelenen uygulanan kural gereği olmayan bir durum kodu döndürür), bu belge için teşvik, bir uyarı üretir.</span><span class="sxs-lookup"><span data-stu-id="2231e-114">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), it produces a warning, encouraging you to document it.</span></span>

<span data-ttu-id="2231e-115">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2231e-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="2231e-116">Web API kuralları uygula</span><span class="sxs-lookup"><span data-stu-id="2231e-116">Apply web API conventions</span></span>

<span data-ttu-id="2231e-117">Bir kuralı uygulamak için üç yolunuz vardır.</span><span class="sxs-lookup"><span data-stu-id="2231e-117">There are three ways to apply a convention.</span></span> <span data-ttu-id="2231e-118">Kuralları oluşturma yok.</span><span class="sxs-lookup"><span data-stu-id="2231e-118">Conventions don't compose.</span></span> <span data-ttu-id="2231e-119">Her eylem, tam olarak bir kuralı ile ilişkilendirilmiş olabilir.</span><span class="sxs-lookup"><span data-stu-id="2231e-119">Each action may be associated with exactly one convention.</span></span> <span data-ttu-id="2231e-120">(Aşağıda açıklanmıştır) daha belirli kuralları, daha özgül daha önceliklidir.</span><span class="sxs-lookup"><span data-stu-id="2231e-120">More specific conventions (detailed below) take precedence over less specific ones.</span></span> <span data-ttu-id="2231e-121">İki veya daha fazla kurallar aynı öncelik için bir eylem uyguladığınızda seçim belirleyici değildir.</span><span class="sxs-lookup"><span data-stu-id="2231e-121">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="2231e-122">Bir eylem için geçerli bir kural için aşağıdaki seçenekler mevcuttur. en az belirli belirli:</span><span class="sxs-lookup"><span data-stu-id="2231e-122">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="2231e-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Bireysel eylemleri için geçerlidir ve kuralı türü ve geçerli kuralı yöntemini belirtir.</span><span class="sxs-lookup"><span data-stu-id="2231e-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span> <span data-ttu-id="2231e-124">Aşağıdaki örnekte, kuralı yöntemi `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` uygulanan `Update` eylem:</span><span class="sxs-lookup"><span data-stu-id="2231e-124">In the following sample, the convention method `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. <span data-ttu-id="2231e-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir denetleyiciye uygulanan &mdash; kuralı türü denetleyicinin tüm eylemleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2231e-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the convention type to all actions on the controller.</span></span> <span data-ttu-id="2231e-126">Kuralı yöntemleri (Ayrıntılar) kuralları yazma işleminin parçası olarak uygulanacak eylemleri belirlemek ipuçları ile donatılmış.</span><span class="sxs-lookup"><span data-stu-id="2231e-126">Convention methods are decorated with hints that determine which actions it would apply to (details as part of authoring conventions).</span></span> <span data-ttu-id="2231e-127">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2231e-127">For example:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. <span data-ttu-id="2231e-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` bir derlemeye uygulanan &mdash; kuralı türü geçerli derlemedeki tüm denetleyicileri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="2231e-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="2231e-129">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2231e-129">For example:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="2231e-130">Web API kuralları oluşturma</span><span class="sxs-lookup"><span data-stu-id="2231e-130">Create web API conventions</span></span>

<span data-ttu-id="2231e-131">Yöntemleri statik bir türle bir kuraldır.</span><span class="sxs-lookup"><span data-stu-id="2231e-131">A convention is a static type with methods.</span></span> <span data-ttu-id="2231e-132">Bu yöntemleri ile açıklamalı olan `[ProducesResponseType]` veya `[ProducesDefaultResponseType]` öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="2231e-132">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span>

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

<span data-ttu-id="2231e-133">Bu kural bir derlemeye uygulama sonuçları herhangi bir işlem adıyla uygulama kuralı yönteminde `Find` ve adlı tam olarak bir parametreye sahip `id`, bunlar diğer daha özel meta veri öznitelikleri yok sürece.</span><span class="sxs-lookup"><span data-stu-id="2231e-133">Applying this convention to an assembly results in the convention method applying to any action with the name `Find` and having exactly one parameter named `id`, as long as they don't have other more specific metadata attributes.</span></span>

<span data-ttu-id="2231e-134">Ek olarak `[ProducesResponseType]` ve `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` ve `[ApiConventionTypeMatch]` uygulamak için yöntemleri belirler kuralı yöntemi uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2231e-134">In addition to `[ProducesResponseType]` and `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` can be applied to the convention method that determines the methods they apply to.</span></span> <span data-ttu-id="2231e-135">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2231e-135">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* <span data-ttu-id="2231e-136">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` Yönteme uygulanan seçeneği kuralı "İle Bul" ön ekli olduğu sürece herhangi bir eylem eşleşebilir gösterir.</span><span class="sxs-lookup"><span data-stu-id="2231e-136">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method, indicates that the convention can match any action as long as it's prefixed with "Find".</span></span> <span data-ttu-id="2231e-137">Bu gibi yöntemleri içerir `Find`, `FindPet`, veya `FindById`.</span><span class="sxs-lookup"><span data-stu-id="2231e-137">This includes methods such as `Find`, `FindPet`, or `FindById`.</span></span>
* <span data-ttu-id="2231e-138">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` Uygulanan kural yöntemleri soneki tanımlayıcıda bitiş tam olarak bir parametre ile eşleşebilir parametresi gösterir.</span><span class="sxs-lookup"><span data-stu-id="2231e-138">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention can match methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="2231e-139">Bu gibi parametreler içeren `id` veya `petId`.</span><span class="sxs-lookup"><span data-stu-id="2231e-139">This includes parameters such as `id` or `petId`.</span></span> <span data-ttu-id="2231e-140">`ApiConventionTypeMatch` benzer şekilde türleriyle sınırlamak parametresinin türü için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="2231e-140">`ApiConventionTypeMatch` can be similarly applied to types to constrain the type of the parameter.</span></span> <span data-ttu-id="2231e-141">A `params[]` bağımsız değişkeni, açıkça eşlenmesi gerekmez kalan parametreleri belirtmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2231e-141">A `params[]` argument can be used to indicate remaining parameters that don't need to be explicitly matched.</span></span>
