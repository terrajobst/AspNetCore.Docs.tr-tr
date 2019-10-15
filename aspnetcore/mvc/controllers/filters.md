---
title: ASP.NET Core filtreler
author: ardalis
description: Filtrelerin nasıl çalıştığını ve ASP.NET Core nasıl kullanılacağını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 0c3597f24e02af40517e12a86127b140ed4fb550
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333927"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="13b30-103">ASP.NET Core filtreler</span><span class="sxs-lookup"><span data-stu-id="13b30-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="13b30-104">[Kirk Larkabağı](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/)ve [Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="13b30-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="13b30-105">ASP.NET Core *Filtreler* , istek işleme ardışık düzeninde belirli aşamalardan önce veya sonra kod çalıştırılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="13b30-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="13b30-106">Yerleşik Filtreler şunları gibi görevleri işler:</span><span class="sxs-lookup"><span data-stu-id="13b30-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="13b30-107">Yetkilendirme (kullanıcının yetkili olmadığı kaynaklara erişimi önler).</span><span class="sxs-lookup"><span data-stu-id="13b30-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="13b30-108">Yanıt önbelleğe alma (istek ardışık düzenini önbelleğe alınmış bir yanıt döndürecek şekilde döndürür).</span><span class="sxs-lookup"><span data-stu-id="13b30-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="13b30-109">Çapraz kesme sorunlarını işlemek için özel filtreler oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="13b30-110">Çapraz kesme sorunlarına örnek olarak hata işleme, önbelleğe alma, yapılandırma, yetkilendirme ve günlüğe kaydetme dahildir.</span><span class="sxs-lookup"><span data-stu-id="13b30-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="13b30-111">Filtreler kodu çoğaltmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="13b30-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="13b30-112">Örneğin, bir hata işleme özel durum filtresi hata işlemeyi birleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="13b30-113">Bu belge, görünümler içeren Razor Pages, API denetleyicileri ve denetleyiciler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="13b30-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="13b30-114">Örneği ([indirme](xref:index#how-to-download-a-sample)) [görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) .</span><span class="sxs-lookup"><span data-stu-id="13b30-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="13b30-115">Filtreler nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="13b30-115">How filters work</span></span>

<span data-ttu-id="13b30-116">Filtreler, bazen *Filtre işlem hattı*olarak da adlandırılan *ASP.NET Core eylemi çağırma işlem hattı*içinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="13b30-117">Filtre işlem hattı çalıştırılacak eylemi ASP.NET Core seçtikten sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![İstek diğer ara yazılım, yönlendirme ara yazılımı, eylem seçimi ve ASP.NET Core eylemi çağırma Işlem hattı aracılığıyla işlenir.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="13b30-120">Filtre türleri</span><span class="sxs-lookup"><span data-stu-id="13b30-120">Filter types</span></span>

<span data-ttu-id="13b30-121">Her filtre türü, filtre ardışık düzeninde farklı bir aşamada yürütülür:</span><span class="sxs-lookup"><span data-stu-id="13b30-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="13b30-122">İlk olarak [Yetkilendirme filtreleri](#authorization-filters) çalışır ve kullanıcının istek için yetkilendirilip yetkilendirilmediğini tespit etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="13b30-123">Yetkilendirme filtreleri, istek yetkisiz ise işlem hattı kısa devre dışı.</span><span class="sxs-lookup"><span data-stu-id="13b30-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="13b30-124">[Kaynak filtreleri](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="13b30-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="13b30-125">Yetkilendirmeden sonra çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="13b30-125">Run after authorization.</span></span>  
  * <span data-ttu-id="13b30-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*>, filtre işlem hattının geri kalanının önüne kod çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="13b30-127">Örneğin `OnResourceExecuting`, model bağlamadan önce kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="13b30-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*>, ardışık düzenin geri kalanı tamamlandıktan sonra kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="13b30-129">[Eylem filtreleri](#action-filters) , tek bir eylem yöntemi çağrıldıktan hemen önce ve sonra kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="13b30-130">Bir eyleme geçirilen bağımsız değişkenleri ve eylemden döndürülen sonucu işlemek için kullanılabilirler.</span><span class="sxs-lookup"><span data-stu-id="13b30-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="13b30-131">Eylem filtreleri Razor Pages **desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="13b30-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="13b30-132">[Özel durum filtreleri](#exception-filters) , yanıt gövdesine hiçbir şey yazılmadan önce oluşan işlenmemiş özel durumlara genel ilkeler uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="13b30-133">[Sonuç filtreleri](#result-filters) , tek tek eylem sonuçlarının yürütülmesinden hemen önce ve sonra kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="13b30-134">Bunlar yalnızca eylem yöntemi başarıyla yürütüldüğünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="13b30-135">Bu değerler, görünüm veya biçimlendirici yürütmesinin yürütülmesi gereken mantık için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="13b30-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="13b30-136">Aşağıdaki diyagramda filtre türlerinin filtre ardışık düzeninde nasıl etkileşimde bulunduğu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="13b30-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![İstek, Yetkilendirme filtreleri, kaynak filtreleri, model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütmesi aracılığıyla işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="13b30-139">Uygulama</span><span class="sxs-lookup"><span data-stu-id="13b30-139">Implementation</span></span>

<span data-ttu-id="13b30-140">Filtreler, farklı arabirim tanımları aracılığıyla hem zaman uyumlu hem de zaman uyumsuz uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="13b30-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="13b30-141">Zaman uyumlu filtreler, bir kodu (`On-Stage-Executing`) ve sonra (`On-Stage-Executed`) ardışık düzen aşamasına çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="13b30-142">Örneğin, `OnActionExecuting`, eylem yöntemi çağrılmadan önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="13b30-143">`OnActionExecuted`, eylem yöntemi döndüğünde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="13b30-144">Zaman uyumsuz filtreler `On-Stage-ExecutionAsync` yöntemini tanımlar:</span><span class="sxs-lookup"><span data-stu-id="13b30-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="13b30-145">Yukarıdaki kodda `SampleAsyncActionFilter` ' a, eylem yöntemini yürüten bir <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) vardır.</span><span class="sxs-lookup"><span data-stu-id="13b30-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="13b30-146">@No__t-0 yöntemlerinin her biri, filtrenin ardışık düzen aşamasını yürüten bir `FilterType-ExecutionDelegate` alır.</span><span class="sxs-lookup"><span data-stu-id="13b30-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="13b30-147">Birden çok filtre aşaması</span><span class="sxs-lookup"><span data-stu-id="13b30-147">Multiple filter stages</span></span>

<span data-ttu-id="13b30-148">Birden çok filtre aşaması için arabirimler tek bir sınıfta uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="13b30-149">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sınıfı `IActionFilter`, `IResultFilter` ve zaman uyumsuz eşdeğerlerini uygular.</span><span class="sxs-lookup"><span data-stu-id="13b30-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="13b30-150">Her ikisini de **değil** , bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="13b30-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="13b30-151">Çalışma zamanı öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="13b30-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="13b30-152">Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır.</span><span class="sxs-lookup"><span data-stu-id="13b30-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="13b30-153">Tek bir sınıfta hem zaman uyumsuz hem de zaman uyumlu arabirimler uygulanmışsa, yalnızca zaman uyumsuz yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="13b30-154">@No__t gibi soyut sınıflar kullanılırken, yalnızca zaman uyumlu yöntemleri veya her bir filtre türü için zaman uyumsuz yöntemi geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="13b30-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="13b30-155">Yerleşik filtre öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="13b30-155">Built-in filter attributes</span></span>

<span data-ttu-id="13b30-156">ASP.NET Core, alt sınıflanmış ve özelleştirilebilen yerleşik öznitelik tabanlı filtreler içerir.</span><span class="sxs-lookup"><span data-stu-id="13b30-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="13b30-157">Örneğin, aşağıdaki sonuç filtresi yanıta bir üst bilgi ekler:</span><span class="sxs-lookup"><span data-stu-id="13b30-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="13b30-158">Öznitelikler, önceki örnekte gösterildiği gibi, filtrelerin bağımsız değişkenleri kabul etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="13b30-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="13b30-159">@No__t-0 ' i bir denetleyiciye veya eylem yöntemine uygulayın ve HTTP üstbilgisinin adını ve değerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="13b30-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="13b30-160">Filtre arabirimlerinden birkaçı, özel uygulamalar için temel sınıflar olarak kullanılabilecek karşılık gelen özniteliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="13b30-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="13b30-161">Filtre öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="13b30-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="13b30-162">Kapsamları ve yürütme sırasını filtrele</span><span class="sxs-lookup"><span data-stu-id="13b30-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="13b30-163">Üç *kapsamından*birindeki işlem hattına bir filtre eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="13b30-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="13b30-164">Bir eylem üzerinde bir öznitelik kullanma.</span><span class="sxs-lookup"><span data-stu-id="13b30-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="13b30-165">Bir denetleyicide bir özniteliği kullanma.</span><span class="sxs-lookup"><span data-stu-id="13b30-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="13b30-166">Aşağıdaki kodda gösterildiği gibi tüm denetleyiciler ve eylemler için genel olarak:</span><span class="sxs-lookup"><span data-stu-id="13b30-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="13b30-167">Yukarıdaki kod, [Mvcoptions. Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) koleksiyonunu kullanarak genel olarak üç filtre ekler.</span><span class="sxs-lookup"><span data-stu-id="13b30-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="13b30-168">Varsayılan yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="13b30-168">Default order of execution</span></span>

<span data-ttu-id="13b30-169">İşlem hattının belirli bir aşamasına ilişkin birden çok filtre olduğunda, kapsam varsayılan filtre yürütme sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="13b30-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="13b30-170">Genel filtreler kapsayan sınıf filtreleri, bu da kapsayan Yöntem filtreleri.</span><span class="sxs-lookup"><span data-stu-id="13b30-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="13b30-171">Filtre iç içe geçme sonucu *olarak, filtrenin kodu,* *önceki* kodun ters sırasına göre çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="13b30-172">Filtre sırası:</span><span class="sxs-lookup"><span data-stu-id="13b30-172">The filter sequence:</span></span>

* <span data-ttu-id="13b30-173">Genel filtrelerin *önceki* kodu.</span><span class="sxs-lookup"><span data-stu-id="13b30-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="13b30-174">Denetleyici filtrelerinden *önceki* kod.</span><span class="sxs-lookup"><span data-stu-id="13b30-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="13b30-175">Eylem yöntemi filtrelerinden *önceki* kod.</span><span class="sxs-lookup"><span data-stu-id="13b30-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="13b30-176">Eylem yöntemi filtrelerinden *sonraki* kod.</span><span class="sxs-lookup"><span data-stu-id="13b30-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="13b30-177">Denetleyici filtrelerinden *sonraki* kod.</span><span class="sxs-lookup"><span data-stu-id="13b30-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="13b30-178">Genel filtrelerin *sonraki* kodu.</span><span class="sxs-lookup"><span data-stu-id="13b30-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="13b30-179">Zaman uyumlu eylem filtreleri için filtre yöntemlerinin çağrıldığı sırayı gösteren aşağıdaki örnek.</span><span class="sxs-lookup"><span data-stu-id="13b30-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="13b30-180">Sequence</span><span class="sxs-lookup"><span data-stu-id="13b30-180">Sequence</span></span> | <span data-ttu-id="13b30-181">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="13b30-181">Filter scope</span></span> | <span data-ttu-id="13b30-182">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="13b30-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="13b30-183">1\.</span><span class="sxs-lookup"><span data-stu-id="13b30-183">1</span></span> | <span data-ttu-id="13b30-184">Global</span><span class="sxs-lookup"><span data-stu-id="13b30-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="13b30-185">2</span><span class="sxs-lookup"><span data-stu-id="13b30-185">2</span></span> | <span data-ttu-id="13b30-186">Kumandasını</span><span class="sxs-lookup"><span data-stu-id="13b30-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="13b30-187">3</span><span class="sxs-lookup"><span data-stu-id="13b30-187">3</span></span> | <span data-ttu-id="13b30-188">Yöntem</span><span class="sxs-lookup"><span data-stu-id="13b30-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="13b30-189">4</span><span class="sxs-lookup"><span data-stu-id="13b30-189">4</span></span> | <span data-ttu-id="13b30-190">Yöntem</span><span class="sxs-lookup"><span data-stu-id="13b30-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="13b30-191">5</span><span class="sxs-lookup"><span data-stu-id="13b30-191">5</span></span> | <span data-ttu-id="13b30-192">Kumandasını</span><span class="sxs-lookup"><span data-stu-id="13b30-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="13b30-193">6</span><span class="sxs-lookup"><span data-stu-id="13b30-193">6</span></span> | <span data-ttu-id="13b30-194">Global</span><span class="sxs-lookup"><span data-stu-id="13b30-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="13b30-195">Bu sıra şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="13b30-195">This sequence shows:</span></span>

* <span data-ttu-id="13b30-196">Yöntem filtresi, denetleyici filtresi içinde iç içe geçmiş.</span><span class="sxs-lookup"><span data-stu-id="13b30-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="13b30-197">Denetleyici filtresi, genel filtrenin içinde iç içe geçmiş.</span><span class="sxs-lookup"><span data-stu-id="13b30-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="13b30-198">Denetleyici ve Razor sayfa düzeyi filtreleri</span><span class="sxs-lookup"><span data-stu-id="13b30-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="13b30-199">@No__t-0 taban sınıfından devralan her denetleyici [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)ve [controller. onactionyürütülmüş](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
 @ no__t-5 yöntemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="13b30-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="13b30-200">Bu Yöntemler:</span><span class="sxs-lookup"><span data-stu-id="13b30-200">These methods:</span></span>

* <span data-ttu-id="13b30-201">Belirli bir eylem için çalışan filtreleri sarın.</span><span class="sxs-lookup"><span data-stu-id="13b30-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="13b30-202">`OnActionExecuting`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="13b30-203">`OnActionExecuted`, tüm eylem filtrelerinden sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="13b30-204">`OnActionExecutionAsync`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="13b30-205">@No__t-0 ' dan sonra filtre içindeki kod eylem yönteminden sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="13b30-206">Örneğin, indirme örneğinde, `MySampleActionFilter`, başlangıçta genel olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="13b30-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="13b30-207">@No__t-0:</span><span class="sxs-lookup"><span data-stu-id="13b30-207">The `TestController`:</span></span>

* <span data-ttu-id="13b30-208">@No__t-0 (`[SampleActionFilter]`) `FilterTest2` eylemine uygular.</span><span class="sxs-lookup"><span data-stu-id="13b30-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="13b30-209">@No__t-0 ve `OnActionExecuted` geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="13b30-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="13b30-210">@No__t-0 ' a gitme aşağıdaki kodu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="13b30-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="13b30-211">Razor Pages için bkz. [filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="13b30-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="13b30-212">Varsayılan sırayı geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="13b30-212">Overriding the default order</span></span>

<span data-ttu-id="13b30-213">Varsayılan yürütme sırası <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter> uygulayarak geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="13b30-214">`IOrderedFilter`, yürütme sırasını belirlemede kapsama göre öncelik alan <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="13b30-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="13b30-215">Küçük bir @no__t 0 değeri olan bir filtre:</span><span class="sxs-lookup"><span data-stu-id="13b30-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="13b30-216">Daha yüksek `Order` değerine sahip bir filtreden *önce kodu çalıştırır* .</span><span class="sxs-lookup"><span data-stu-id="13b30-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="13b30-217">Daha yüksek `Order` değerine sahip bir filtrenin *sonrasında koddan sonra* çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="13b30-218">@No__t-0 özelliği bir Oluşturucu parametresiyle ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="13b30-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="13b30-219">Yukarıdaki örnekte gösterilen 3 eylem filtresini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="13b30-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="13b30-220">Denetleyicinin ve genel filtrelerin `Order` özelliği sırasıyla 1 ve 2 ' ye ayarlandıysa, yürütme sırası tersine çevrilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="13b30-221">Sequence</span><span class="sxs-lookup"><span data-stu-id="13b30-221">Sequence</span></span> | <span data-ttu-id="13b30-222">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="13b30-222">Filter scope</span></span> | <span data-ttu-id="13b30-223">`Order` özelliği</span><span class="sxs-lookup"><span data-stu-id="13b30-223">`Order` property</span></span> | <span data-ttu-id="13b30-224">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="13b30-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="13b30-225">1\.</span><span class="sxs-lookup"><span data-stu-id="13b30-225">1</span></span> | <span data-ttu-id="13b30-226">Yöntem</span><span class="sxs-lookup"><span data-stu-id="13b30-226">Method</span></span> | <span data-ttu-id="13b30-227">0</span><span class="sxs-lookup"><span data-stu-id="13b30-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="13b30-228">2</span><span class="sxs-lookup"><span data-stu-id="13b30-228">2</span></span> | <span data-ttu-id="13b30-229">Kumandasını</span><span class="sxs-lookup"><span data-stu-id="13b30-229">Controller</span></span> | <span data-ttu-id="13b30-230">1\.</span><span class="sxs-lookup"><span data-stu-id="13b30-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="13b30-231">3</span><span class="sxs-lookup"><span data-stu-id="13b30-231">3</span></span> | <span data-ttu-id="13b30-232">Global</span><span class="sxs-lookup"><span data-stu-id="13b30-232">Global</span></span> | <span data-ttu-id="13b30-233">2</span><span class="sxs-lookup"><span data-stu-id="13b30-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="13b30-234">4</span><span class="sxs-lookup"><span data-stu-id="13b30-234">4</span></span> | <span data-ttu-id="13b30-235">Global</span><span class="sxs-lookup"><span data-stu-id="13b30-235">Global</span></span> | <span data-ttu-id="13b30-236">2</span><span class="sxs-lookup"><span data-stu-id="13b30-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="13b30-237">5</span><span class="sxs-lookup"><span data-stu-id="13b30-237">5</span></span> | <span data-ttu-id="13b30-238">Kumandasını</span><span class="sxs-lookup"><span data-stu-id="13b30-238">Controller</span></span> | <span data-ttu-id="13b30-239">1\.</span><span class="sxs-lookup"><span data-stu-id="13b30-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="13b30-240">6</span><span class="sxs-lookup"><span data-stu-id="13b30-240">6</span></span> | <span data-ttu-id="13b30-241">Yöntem</span><span class="sxs-lookup"><span data-stu-id="13b30-241">Method</span></span> | <span data-ttu-id="13b30-242">0</span><span class="sxs-lookup"><span data-stu-id="13b30-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="13b30-243">@No__t-0 özelliği filtrelerin çalıştırıldığı sırayı belirlerken kapsamı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="13b30-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="13b30-244">Filtreler sırasıyla sıraya göre sıralanır, ardından kapsam bölmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="13b30-245">Yerleşik filtrelerin tümü `IOrderedFilter` uygular ve varsayılan `Order` değerini 0 olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="13b30-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="13b30-246">Yerleşik filtreler için, `Order` sıfır olmayan bir değere ayarlanmadığı takdirde kapsam sıralamayı belirler.</span><span class="sxs-lookup"><span data-stu-id="13b30-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="13b30-247">İptal ve kısa devre dışı</span><span class="sxs-lookup"><span data-stu-id="13b30-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="13b30-248">Filtre işlem hattı, filtre yöntemine sunulan <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parametresinde <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> özelliği ayarlanarak kısa devre dışı olabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="13b30-249">Örneğin, aşağıdaki kaynak filtresi, ardışık düzenin geri kalanının yürütülmesini engeller:</span><span class="sxs-lookup"><span data-stu-id="13b30-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="13b30-250">Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` hem de `AddHeader` filtresi `SomeResource` eylem metodunu hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="13b30-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="13b30-251">@No__t-0:</span><span class="sxs-lookup"><span data-stu-id="13b30-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="13b30-252">İlk olarak bir kaynak filtresi olduğundan ve `AddHeader` bir eylem filtresi olduğundan, önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="13b30-253">Kısa süreli işlem hattının geri kalanı.</span><span class="sxs-lookup"><span data-stu-id="13b30-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="13b30-254">Bu nedenle `AddHeader` filtresi hiçbir şekilde `SomeResource` eylemi için çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="13b30-255">Bu davranış, işlem yöntemi düzeyinde her iki filtre de uygulanmışsa aynı olur, çünkü `ShortCircuitingResourceFilter` önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="13b30-256">@No__t-0, filtre türü veya `Order` özelliğinin açık kullanımı nedeniyle önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="13b30-257">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="13b30-257">Dependency injection</span></span>

<span data-ttu-id="13b30-258">Filtreler türe veya örneğe göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="13b30-259">Bir örnek eklenirse, bu örnek her istek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="13b30-260">Bir tür eklenirse, türü etkinleştirilmiş olur.</span><span class="sxs-lookup"><span data-stu-id="13b30-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="13b30-261">Tür etkinleştirilmiş bir filtre şu anlama gelir:</span><span class="sxs-lookup"><span data-stu-id="13b30-261">A type-activated filter means:</span></span>

* <span data-ttu-id="13b30-262">Her istek için bir örnek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="13b30-262">An instance is created for each request.</span></span>
* <span data-ttu-id="13b30-263">Herhangi bir Oluşturucu bağımlılığı [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından doldurulur.</span><span class="sxs-lookup"><span data-stu-id="13b30-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="13b30-264">Öznitelik olarak uygulanan ve doğrudan denetleyici sınıflarına veya eylem yöntemlerine eklenen filtreler [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından sağlanmış Oluşturucu bağımlılıklarına sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="13b30-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="13b30-265">Oluşturucu bağımlılıkları şu nedenle şu nedenle sağlanamaz:</span><span class="sxs-lookup"><span data-stu-id="13b30-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="13b30-266">Özniteliklerin, uygulandıkları yerlerde, Oluşturucu parametreleri sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="13b30-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="13b30-267">Bu, özniteliklerin nasıl çalıştığı konusunda bir kısıtlamadır.</span><span class="sxs-lookup"><span data-stu-id="13b30-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="13b30-268">Aşağıdaki filtreler, dı tarafından belirtilen Oluşturucu bağımlılıklarını destekler:</span><span class="sxs-lookup"><span data-stu-id="13b30-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="13b30-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> özniteliğinde uygulandı.</span><span class="sxs-lookup"><span data-stu-id="13b30-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="13b30-270">Önceki filtreler bir denetleyiciye veya eylem yöntemine uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="13b30-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="13b30-271">Günlükçüler, DI 'den alınabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-271">Loggers are available from DI.</span></span> <span data-ttu-id="13b30-272">Ancak, yalnızca günlüğe kaydetme amacıyla filtre oluşturmaktan ve kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="13b30-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="13b30-273">[Yerleşik çerçeve günlüğü](xref:fundamentals/logging/index) genellikle günlüğe kaydetme için gerekenleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="13b30-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="13b30-274">Filtrelere günlük eklendi:</span><span class="sxs-lookup"><span data-stu-id="13b30-274">Logging added to filters:</span></span>

* <span data-ttu-id="13b30-275">, İş etki alanı kaygılarını veya filtreye özgü davranışları odaklamalıdır.</span><span class="sxs-lookup"><span data-stu-id="13b30-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="13b30-276">Eylemleri veya diğer çerçeve olaylarını günlüğe **içermemelidir** .</span><span class="sxs-lookup"><span data-stu-id="13b30-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="13b30-277">Yerleşik filtreler günlük eylemleri ve çerçeve olayları.</span><span class="sxs-lookup"><span data-stu-id="13b30-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="13b30-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="13b30-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="13b30-279">Hizmet filtresi uygulama türleri `ConfigureServices` ' a kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="13b30-280">@No__t-0 ' dan bir filtrenin örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="13b30-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="13b30-281">Aşağıdaki kod @no__t gösterir-0:</span><span class="sxs-lookup"><span data-stu-id="13b30-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="13b30-282">Aşağıdaki kodda, `AddHeaderResultServiceFilter`, dı kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="13b30-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="13b30-283">Aşağıdaki kodda `ServiceFilter` özniteliği, ' dan `AddHeaderResultServiceFilter` filtresinin bir örneğini alır:</span><span class="sxs-lookup"><span data-stu-id="13b30-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="13b30-284">@No__t-0 kullanırken, [Servicefilterattribute. ıyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="13b30-284">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="13b30-285">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden *kullanılabilir olabileceğini gösteren* bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="13b30-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="13b30-286">ASP.NET Core çalışma zamanı garanti etmez:</span><span class="sxs-lookup"><span data-stu-id="13b30-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="13b30-287">Filtrenin tek bir örneğinin oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="13b30-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="13b30-288">Filtre, sonraki bir noktada dı kapsayıcısından yeniden istenmeyecek.</span><span class="sxs-lookup"><span data-stu-id="13b30-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="13b30-289">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="13b30-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="13b30-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> @no__t uygular-1.</span><span class="sxs-lookup"><span data-stu-id="13b30-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="13b30-291">`IFilterFactory`, <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13b30-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="13b30-292">`CreateInstance`, belirtilen türü DI öğesinden yükler.</span><span class="sxs-lookup"><span data-stu-id="13b30-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="13b30-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="13b30-293">TypeFilterAttribute</span></span>

<span data-ttu-id="13b30-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>, <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> ile benzerdir, ancak türü doğrudan dı kapsayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="13b30-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="13b30-295">@No__t-0 kullanarak türü başlatır.</span><span class="sxs-lookup"><span data-stu-id="13b30-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="13b30-296">@No__t-0 türleri doğrudan dı kapsayıcısından çözümlenmediği için:</span><span class="sxs-lookup"><span data-stu-id="13b30-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="13b30-297">@No__t-0 kullanılarak başvurulan türlerin, dı kapsayıcısına kayıtlı olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="13b30-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="13b30-298">Bunların bağımlılıkları, dı kapsayıcısı tarafından yerine getirilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="13b30-299">`TypeFilterAttribute` isteğe bağlı olarak tür için Oluşturucu bağımsız değişkenlerini kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="13b30-300">@No__t-0 kullanılırken, [Typefilterattribute. ıyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="13b30-300">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="13b30-301">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden kullanılabilir *olabileceği* ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="13b30-301">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="13b30-302">ASP.NET Core çalışma zamanı, filtrenin tek bir örneğinin oluşturulacağı garantisi vermez.</span><span class="sxs-lookup"><span data-stu-id="13b30-302">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="13b30-303">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="13b30-303">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="13b30-304">Aşağıdaki örnek `TypeFilterAttribute` kullanarak bir türe bağımsız değişkenlerin nasıl geçirileceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="13b30-304">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="13b30-305">Yetkilendirme filtreleri</span><span class="sxs-lookup"><span data-stu-id="13b30-305">Authorization filters</span></span>

<span data-ttu-id="13b30-306">Yetkilendirme filtreleri:</span><span class="sxs-lookup"><span data-stu-id="13b30-306">Authorization filters:</span></span>

* <span data-ttu-id="13b30-307">, Filtre ardışık düzeninde ilk filtrelerin çalıştırılmasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="13b30-307">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="13b30-308">Eylem yöntemlerine erişimi denetleyin.</span><span class="sxs-lookup"><span data-stu-id="13b30-308">Control access to action methods.</span></span>
* <span data-ttu-id="13b30-309">Bir Before yöntemi, ancak After yönteminden hiçbiri.</span><span class="sxs-lookup"><span data-stu-id="13b30-309">Have a before method, but no after method.</span></span>

<span data-ttu-id="13b30-310">Özel Yetkilendirme filtreleri özel bir yetkilendirme çerçevesi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="13b30-310">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="13b30-311">Özel bir filtre yazmak için yetkilendirme ilkelerini yapılandırmayı veya özel bir yetkilendirme ilkesi yazmayı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="13b30-311">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="13b30-312">Yerleşik yetkilendirme filtresi:</span><span class="sxs-lookup"><span data-stu-id="13b30-312">The built-in authorization filter:</span></span>

* <span data-ttu-id="13b30-313">Yetkilendirme sistemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="13b30-313">Calls the authorization system.</span></span>
* <span data-ttu-id="13b30-314">İstekleri yetkilendirmez.</span><span class="sxs-lookup"><span data-stu-id="13b30-314">Does not authorize requests.</span></span>

<span data-ttu-id="13b30-315">Yetkilendirme filtreleri içinde özel **durumlar atamayın:**</span><span class="sxs-lookup"><span data-stu-id="13b30-315">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="13b30-316">Özel durum işlenmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="13b30-316">The exception will not be handled.</span></span>
* <span data-ttu-id="13b30-317">Özel durum filtreleri özel durumu işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="13b30-317">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="13b30-318">Bir yetkilendirme filtresinde özel durum oluştuğunda bir sınama vermeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="13b30-318">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="13b30-319">[Yetkilendirme](xref:security/authorization/introduction)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="13b30-319">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="13b30-320">Kaynak filtreleri</span><span class="sxs-lookup"><span data-stu-id="13b30-320">Resource filters</span></span>

<span data-ttu-id="13b30-321">Kaynak filtreleri:</span><span class="sxs-lookup"><span data-stu-id="13b30-321">Resource filters:</span></span>

* <span data-ttu-id="13b30-322">@No__t-0 ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="13b30-322">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="13b30-323">Yürütme, filtre işlem hattının çoğunu sarmalar.</span><span class="sxs-lookup"><span data-stu-id="13b30-323">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="13b30-324">Kaynak filtrelerinden önce yalnızca [Yetkilendirme filtreleri](#authorization-filters) çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-324">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="13b30-325">Kaynak filtreleri, işlem hattının büyük bir yanındaki kısa devre için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="13b30-325">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="13b30-326">Örneğin, bir önbelleğe alma filtresi, bir önbellek isabetinden ardışık düzen geri kalanından kaçınabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-326">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="13b30-327">Kaynak filtresi örnekleri:</span><span class="sxs-lookup"><span data-stu-id="13b30-327">Resource filter examples:</span></span>

* <span data-ttu-id="13b30-328">Daha önce gösterilen [kısa devre dışı kaynak filtresi](#short-circuiting-resource-filter) .</span><span class="sxs-lookup"><span data-stu-id="13b30-328">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="13b30-329">[Disableformvaluemodelbindingattribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="13b30-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="13b30-330">Model bağlamanın form verilerine erişimini engeller.</span><span class="sxs-lookup"><span data-stu-id="13b30-330">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="13b30-331">Form verilerinin belleğe okunmasını engellemek için büyük dosya yüklemeleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-331">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="13b30-332">Eylem filtreleri</span><span class="sxs-lookup"><span data-stu-id="13b30-332">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="13b30-333">Eylem filtreleri Razor Pages **için uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="13b30-333">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="13b30-334">Razor Pages <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> ' i destekler.</span><span class="sxs-lookup"><span data-stu-id="13b30-334">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="13b30-335">Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="13b30-335">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="13b30-336">Eylem filtreleri:</span><span class="sxs-lookup"><span data-stu-id="13b30-336">Action filters:</span></span>

* <span data-ttu-id="13b30-337">@No__t-0 ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="13b30-337">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="13b30-338">Yürütmesinin, eylem yöntemlerinin yürütülmesi çevreler.</span><span class="sxs-lookup"><span data-stu-id="13b30-338">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="13b30-339">Aşağıdaki kod bir örnek eylem filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="13b30-339">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="13b30-340">@No__t-0 aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="13b30-340">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="13b30-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-bir eylem yöntemine yönelik girişlerin okunmalarını sağlar.</span><span class="sxs-lookup"><span data-stu-id="13b30-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="13b30-342"><xref:Microsoft.AspNetCore.Mvc.Controller>-denetleyici örneğinin işlenmesine izin vermez.</span><span class="sxs-lookup"><span data-stu-id="13b30-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="13b30-343"><xref:System.Web.Mvc.ActionExecutingContext.Result>-eylem yönteminin ve sonraki eylem filtrelerinin `Result` kısa devre dışı yürütmesi.</span><span class="sxs-lookup"><span data-stu-id="13b30-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="13b30-344">Eylem yönteminde özel durum oluşturma:</span><span class="sxs-lookup"><span data-stu-id="13b30-344">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="13b30-345">Sonraki filtrelerin çalıştırılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="13b30-345">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="13b30-346">@No__t-0 ayarından farklı olarak, başarılı bir sonuç yerine başarısızlık olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-346">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="13b30-347">@No__t-0 `Controller` ve `Result` ile aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="13b30-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="13b30-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-eylem yürütmesi başka bir filtre tarafından kabul edilse true.</span><span class="sxs-lookup"><span data-stu-id="13b30-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="13b30-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-eylem veya daha önce çalıştırılan bir eylem filtresi özel durum oluşturdu ise null olmayan.</span><span class="sxs-lookup"><span data-stu-id="13b30-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="13b30-350">Bu özellik null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="13b30-350">Setting this property to null:</span></span>

  * <span data-ttu-id="13b30-351">Özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="13b30-351">Effectively handles the exception.</span></span>
  * <span data-ttu-id="13b30-352">`Result`, eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="13b30-352">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="13b30-353">@No__t-0 için, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> ' e bir çağrı:</span><span class="sxs-lookup"><span data-stu-id="13b30-353">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="13b30-354">Sonraki eylem filtrelerini ve eylem yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="13b30-354">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="13b30-355">@No__t-0 döndürür.</span><span class="sxs-lookup"><span data-stu-id="13b30-355">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="13b30-356">Kısa devre 'a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> ' ı bir sonuç örneğine atayın ve `next` (`ActionExecutionDelegate`) çağırmayın.</span><span class="sxs-lookup"><span data-stu-id="13b30-356">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="13b30-357">Framework, alt sınıflı olabilecek bir soyut @no__t (0) sağlar.</span><span class="sxs-lookup"><span data-stu-id="13b30-357">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="13b30-358">@No__t-0 eylem filtresi şu şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="13b30-358">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="13b30-359">Model durumunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="13b30-359">Validate model state.</span></span>
* <span data-ttu-id="13b30-360">Durum geçersizse bir hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="13b30-360">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="13b30-361">@No__t-0 yöntemi eylem yönteminden sonra çalışır:</span><span class="sxs-lookup"><span data-stu-id="13b30-361">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="13b30-362">Ve <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> özelliği aracılığıyla eylemin sonuçlarını görebilir ve değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-362">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="13b30-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled>, eylem yürütmesi başka bir filtre tarafından kabul edilse true olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="13b30-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="13b30-364">eylem veya sonraki eylem filtresi bir özel durum harekete geçirdi, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="13b30-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="13b30-365">@No__t-0 değeri null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="13b30-365">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="13b30-366">Bir özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="13b30-366">Effectively handles an exception.</span></span>
  * <span data-ttu-id="13b30-367">`ActionExecutedContext.Result`, normal olarak eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="13b30-367">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="13b30-368">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="13b30-368">Exception filters</span></span>

<span data-ttu-id="13b30-369">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="13b30-369">Exception filters:</span></span>

* <span data-ttu-id="13b30-370">@No__t-0 veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter> uygulayın.</span><span class="sxs-lookup"><span data-stu-id="13b30-370">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="13b30-371">, Yaygın hata işleme ilkelerini uygulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-371">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="13b30-372">Aşağıdaki örnek özel durum filtresi, uygulama geliştirmede olduğunda oluşan özel durumlar hakkındaki ayrıntıları görüntülemek için özel bir hata görünümü kullanır:</span><span class="sxs-lookup"><span data-stu-id="13b30-372">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="13b30-373">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="13b30-373">Exception filters:</span></span>

* <span data-ttu-id="13b30-374">Etkinlikden önceki ve sonraki olaylar yok.</span><span class="sxs-lookup"><span data-stu-id="13b30-374">Don't have before and after events.</span></span>
* <span data-ttu-id="13b30-375">@No__t-0 veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*> uygulayın.</span><span class="sxs-lookup"><span data-stu-id="13b30-375">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="13b30-376">Razor sayfası veya denetleyici oluşturma, [model bağlama](xref:mvc/models/model-binding), eylem filtreleri veya eylem yöntemlerinde oluşan işlenmemiş özel durumları işleyin.</span><span class="sxs-lookup"><span data-stu-id="13b30-376">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="13b30-377">Kaynak filtrelerinde, sonuç filtrelerinde veya MVC sonuç yürütülürken oluşan özel **durumları yakalamayın** .</span><span class="sxs-lookup"><span data-stu-id="13b30-377">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="13b30-378">Bir özel durumu işlemek için <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> özelliğini `true` olarak ayarlayın veya bir yanıt yazın.</span><span class="sxs-lookup"><span data-stu-id="13b30-378">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="13b30-379">Bu, özel durumun yayılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="13b30-379">This stops propagation of the exception.</span></span> <span data-ttu-id="13b30-380">Özel durum filtresi bir özel durumu "başarılı" olarak açamaz.</span><span class="sxs-lookup"><span data-stu-id="13b30-380">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="13b30-381">Yalnızca bir eylem filtresi bunu yapabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-381">Only an action filter can do that.</span></span>

<span data-ttu-id="13b30-382">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="13b30-382">Exception filters:</span></span>

* <span data-ttu-id="13b30-383">Eylemler içinde oluşan özel durumları yakalamaya uygundur.</span><span class="sxs-lookup"><span data-stu-id="13b30-383">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="13b30-384">, Hata işleme ara yazılımı olarak esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="13b30-384">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="13b30-385">Özel durum işleme için ara yazılımı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="13b30-385">Prefer middleware for exception handling.</span></span> <span data-ttu-id="13b30-386">Yalnızca hata işlemenin hangi eylem yöntemine göre *farklılık* gösteren özel durum filtrelerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="13b30-386">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="13b30-387">Örneğin, bir uygulama hem API uç noktaları hem de görünümler/HTML için eylem yöntemlerine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-387">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="13b30-388">API uç noktaları, hata bilgilerini JSON olarak döndürebilir, ancak görünüm tabanlı eylemler bir hata sayfasını HTML olarak döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-388">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="13b30-389">Sonuç filtreleri</span><span class="sxs-lookup"><span data-stu-id="13b30-389">Result filters</span></span>

<span data-ttu-id="13b30-390">Sonuç filtreleri:</span><span class="sxs-lookup"><span data-stu-id="13b30-390">Result filters:</span></span>

* <span data-ttu-id="13b30-391">Arabirim uygulama:</span><span class="sxs-lookup"><span data-stu-id="13b30-391">Implement an interface:</span></span>
  * <span data-ttu-id="13b30-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="13b30-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="13b30-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="13b30-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="13b30-394">Yürütmesi, eylem sonuçlarının yürütülmesini çevreler.</span><span class="sxs-lookup"><span data-stu-id="13b30-394">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="13b30-395">IResultFilter ve ıasyncresultfilter</span><span class="sxs-lookup"><span data-stu-id="13b30-395">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="13b30-396">Aşağıdaki kod, bir HTTP üst bilgisi ekleyen bir sonuç filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="13b30-396">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="13b30-397">Yürütülen sonuç türü eyleme göre değişir.</span><span class="sxs-lookup"><span data-stu-id="13b30-397">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="13b30-398">Bir görünüm döndüren bir eylem, yürütülmekte olan <xref:Microsoft.AspNetCore.Mvc.ViewResult> ' ın bir parçası olarak tüm Razor işlemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="13b30-398">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="13b30-399">Bir API yöntemi, sonucun yürütülmesinin bir parçası olarak bazı serileştirme işlemleri gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-399">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="13b30-400">[Eylem sonuçları](xref:mvc/controllers/actions)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="13b30-400">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="13b30-401">Sonuç filtreleri yalnızca bir eylem veya eylem filtresi bir eylem sonucu üretirse yürütülür.</span><span class="sxs-lookup"><span data-stu-id="13b30-401">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="13b30-402">Şu durumlarda sonuç filtreleri yürütülmez:</span><span class="sxs-lookup"><span data-stu-id="13b30-402">Result filters are not executed when:</span></span>

* <span data-ttu-id="13b30-403">Bir yetkilendirme filtresi veya kaynak filtresi, işlem hattı için kısa süreli olarak devre dışı.</span><span class="sxs-lookup"><span data-stu-id="13b30-403">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="13b30-404">Bir özel durum filtresi, bir eylem sonucu üreterek özel durumu işler.</span><span class="sxs-lookup"><span data-stu-id="13b30-404">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="13b30-405">@No__t-0 yöntemi, <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> ' i `true` ' ye ayarlayarak eylem sonucunun ve sonraki sonuç filtrelerinin kısa devre yürütülmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-405">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="13b30-406">Boş bir yanıt oluşturmamaya kaçınmak için kısa devre dışı bırakıldığında yanıt nesnesine yazın.</span><span class="sxs-lookup"><span data-stu-id="13b30-406">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="13b30-407">@No__t-0 ' da bir özel durum üretiliyor:</span><span class="sxs-lookup"><span data-stu-id="13b30-407">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="13b30-408">Eylem sonucunun ve sonraki filtrelerin yürütülmesini önleyin.</span><span class="sxs-lookup"><span data-stu-id="13b30-408">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="13b30-409">Başarılı bir sonuç yerine hata olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-409">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="13b30-410">@No__t-0 yöntemi çalıştırıldığında, yanıt istemciye zaten gönderilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-410">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="13b30-411">Yanıt istemciye zaten gönderildiyse, daha fazla değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="13b30-411">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="13b30-412">`ResultExecutedContext.Canceled`, eylem sonucu yürütmesi başka bir filtre tarafından kabul edilen kısa devre olduysa, `true` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="13b30-412">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="13b30-413">`ResultExecutedContext.Exception`, eylem sonucu veya sonraki sonuç filtresi bir özel durum harekete geçirdi null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="13b30-413">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="13b30-414">@No__t-0 olarak null değeri etkin bir şekilde oluşturulur ve özel durumun, ardışık düzendeki ASP.NET Core daha sonra yeniden oluşturulmasını önler.</span><span class="sxs-lookup"><span data-stu-id="13b30-414">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="13b30-415">Bir sonuç filtresinde özel durum işlenirken yanıta veri yazmanın güvenilir bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="13b30-415">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="13b30-416">Bir eylem sonucu bir özel durum oluşturduğunda üstbilgiler istemciye temizleniyorsa, hata kodu göndermek için güvenilir bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="13b30-416">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="13b30-417">@No__t-0 için, <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> üzerindeki `await next` çağrısı, sonraki sonuç filtrelerini ve eylem sonucunu yürütür.</span><span class="sxs-lookup"><span data-stu-id="13b30-417">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="13b30-418">Kısa devre dışı bırakmak için [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) `true` olarak ayarlayın ve `ResultExecutionDelegate` ' yi çağırmayın:</span><span class="sxs-lookup"><span data-stu-id="13b30-418">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="13b30-419">Framework, alt sınıflı olabilecek bir soyut @no__t (0) sağlar.</span><span class="sxs-lookup"><span data-stu-id="13b30-419">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="13b30-420">Daha önce gösterilen [Addheaderattribute](#add-header-attribute) sınıfı bir sonuç Filtresi özniteliği örneğidir.</span><span class="sxs-lookup"><span data-stu-id="13b30-420">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="13b30-421">Ialwaysrunresultfilter ve ıasyncalwaysrunresultfilter</span><span class="sxs-lookup"><span data-stu-id="13b30-421">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="13b30-422">@No__t-0 ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> arabirimleri tüm eylem sonuçları için çalışan bir @no__t 2 uygulamasını bildirir.</span><span class="sxs-lookup"><span data-stu-id="13b30-422">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="13b30-423">Bu, tarafından oluşturulan eylem sonuçlarını içerir:</span><span class="sxs-lookup"><span data-stu-id="13b30-423">This includes action results produced by:</span></span>

* <span data-ttu-id="13b30-424">Kısa devre olan Yetkilendirme filtreleri ve kaynak filtreleri.</span><span class="sxs-lookup"><span data-stu-id="13b30-424">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="13b30-425">Özel durum filtreleri.</span><span class="sxs-lookup"><span data-stu-id="13b30-425">Exception filters.</span></span>

<span data-ttu-id="13b30-426">Örneğin, aşağıdaki filtre her zaman çalışır ve içerik anlaşması başarısız olduğunda *422 olmayan bir varlık* durum kodu ile bir eylem sonucu (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) ayarlar:</span><span class="sxs-lookup"><span data-stu-id="13b30-426">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="13b30-427">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="13b30-427">IFilterFactory</span></span>

<span data-ttu-id="13b30-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> @no__t uygular-1.</span><span class="sxs-lookup"><span data-stu-id="13b30-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="13b30-429">Bu nedenle, bir `IFilterFactory` örneği, filtre ardışık düzeninde herhangi bir yerde `IFilterMetadata` örneği olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="13b30-429">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="13b30-430">Çalışma zamanı filtreyi çağırmayı hazırlarken, `IFilterFactory` ' a dönüştürmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="13b30-430">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="13b30-431">Bu atama başarılı olursa, çağrılan `IFilterMetadata` örneğini oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="13b30-431">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="13b30-432">Bu, tam filtre işlem hattının uygulama başladığında açıkça ayarlanması gerektiğinden esnek bir tasarım sağlar.</span><span class="sxs-lookup"><span data-stu-id="13b30-432">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="13b30-433">`IFilterFactory`, özel öznitelik uygulamaları kullanılarak filtre oluşturmaya yönelik başka bir yaklaşım olarak uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="13b30-433">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="13b30-434">Önceki kod, [indirme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)çalıştırılarak test edilebilir:</span><span class="sxs-lookup"><span data-stu-id="13b30-434">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="13b30-435">F12 geliştirici araçlarını çağırın.</span><span class="sxs-lookup"><span data-stu-id="13b30-435">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="13b30-436">@No__t-0 ' a gidin.</span><span class="sxs-lookup"><span data-stu-id="13b30-436">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="13b30-437">F12 geliştirici araçları, örnek kod tarafından eklenen aşağıdaki yanıt üstbilgilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="13b30-437">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="13b30-438">**Yazar:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="13b30-438">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="13b30-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="13b30-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="13b30-440">**Dahili:** `My header`</span><span class="sxs-lookup"><span data-stu-id="13b30-440">**internal:** `My header`</span></span>

<span data-ttu-id="13b30-441">Yukarıdaki kod, **iç:** `My header` yanıt üst bilgisini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="13b30-441">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="13b30-442">Öznitelik üzerinde IFilterFactory uygulandı</span><span class="sxs-lookup"><span data-stu-id="13b30-442">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="13b30-443">@No__t-0 uygulayan filtreler şu filtreler için yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="13b30-443">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="13b30-444">Parametre geçirme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="13b30-444">Don't require passing parameters.</span></span>
* <span data-ttu-id="13b30-445">DI tarafından doldurulması gereken Oluşturucu bağımlılıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="13b30-445">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="13b30-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> @no__t uygular-1.</span><span class="sxs-lookup"><span data-stu-id="13b30-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="13b30-447">`IFilterFactory`, <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini gösterir.</span><span class="sxs-lookup"><span data-stu-id="13b30-447">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="13b30-448">`CreateInstance`, belirtilen türü hizmetler kapsayıcısından (dı) yükler.</span><span class="sxs-lookup"><span data-stu-id="13b30-448">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="13b30-449">Aşağıdaki kod @no__t uygulamak için üç yaklaşım gösterir:</span><span class="sxs-lookup"><span data-stu-id="13b30-449">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="13b30-450">Yukarıdaki kodda, yöntemi `[SampleActionFilter]` ile dekorasyon, `SampleActionFilter` ' i uygulamak için tercih edilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="13b30-450">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="13b30-451">Filtre ardışık düzeninde ara yazılım kullanma</span><span class="sxs-lookup"><span data-stu-id="13b30-451">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="13b30-452">Kaynak filtreleri, işlem hattında daha sonra gelen her şeyin yürütülmesini çevreleyecek olan [Ara yazılım](xref:fundamentals/middleware/index) gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-452">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="13b30-453">Ancak filtreler, ASP.NET Core çalışma zamanının bir parçası olan ara yazılımlar dışında farklılık gösterir, bu da ASP.NET Core bağlamına ve yapılarına erişime sahip oldukları anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="13b30-453">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="13b30-454">Ara yazılımı bir filtre olarak kullanmak için, filtre ardışık düzenine eklenecek olan ara yazılımı belirten `Configure` yöntemiyle bir tür oluşturun.</span><span class="sxs-lookup"><span data-stu-id="13b30-454">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="13b30-455">Aşağıdaki örnek, bir istek için geçerli kültürü oluşturmak üzere yerelleştirme ara yazılımını kullanır:</span><span class="sxs-lookup"><span data-stu-id="13b30-455">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="13b30-456">Ara yazılımı çalıştırmak için <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> kullanın:</span><span class="sxs-lookup"><span data-stu-id="13b30-456">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="13b30-457">Ara yazılım filtreleri, filtre işlem hattının aynı aşamasında, model bağlamadan önce ve işlem hattının geri kalanı ile kaynak filtreleri olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="13b30-457">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="13b30-458">Sonraki eylemler</span><span class="sxs-lookup"><span data-stu-id="13b30-458">Next actions</span></span>

* <span data-ttu-id="13b30-459">[Razor Pages Için filtre yöntemlerine](xref:razor-pages/filter)bakın.</span><span class="sxs-lookup"><span data-stu-id="13b30-459">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="13b30-460">Filtrelerle denemek için [GitHub örneğini indirin, test edin ve değiştirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="13b30-460">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
