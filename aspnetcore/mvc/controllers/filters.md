---
title: ASP.NET core'da filtreleri
author: ardalis
description: Filtreleri nasıl çalıştığını ve ASP.NET Core nasıl kullanacağınızı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2019
uid: mvc/controllers/filters
ms.openlocfilehash: 50b199744f32ad19335080da406db69665ec1ae9
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67856156"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="44e07-103">ASP.NET core'da filtreleri</span><span class="sxs-lookup"><span data-stu-id="44e07-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="44e07-104">Tarafından [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="44e07-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="44e07-105">*Filtreler* önce veya sonra istek işleme ardışık düzeninde belirli aşamalara çalıştırmak için kod içinde ASP.NET Core izin verin.</span><span class="sxs-lookup"><span data-stu-id="44e07-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="44e07-106">Yerleşik filtreler gibi görevleri işler:</span><span class="sxs-lookup"><span data-stu-id="44e07-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="44e07-107">Yetkilendirme (bir kullanıcı için yetkili olmayan kaynaklara erişimini engelleme).</span><span class="sxs-lookup"><span data-stu-id="44e07-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="44e07-108">Yanıt önbelleğe alma (önbelleğe alınan yanıt döndürmek için istek ardışık düzenini kısa devre).</span><span class="sxs-lookup"><span data-stu-id="44e07-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="44e07-109">Özel Filtreler, geniş kapsamlı kritik konular işlemek için oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="44e07-110">Hata işleme, önbelleğe alma, yapılandırma, yetkilendirme ve günlüğe kaydetme geniş kapsamlı kritik konular örnekleridir.</span><span class="sxs-lookup"><span data-stu-id="44e07-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="44e07-111">Filtreler, kod yinelemekten kaçının.</span><span class="sxs-lookup"><span data-stu-id="44e07-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="44e07-112">Örneğin, bir hata özel durum filtresi işleme hata işleme birleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="44e07-113">Bu belge, Razor sayfaları, API denetleyicileri ve görünümleri denetleyicileriyle geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="44e07-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="44e07-114">[Görüntüleme veya indirme örnek](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="44e07-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="44e07-115">Filtreler nasıl çalışır</span><span class="sxs-lookup"><span data-stu-id="44e07-115">How filters work</span></span>

<span data-ttu-id="44e07-116">Filtre çalıştırma içinde *ASP.NET Core eylem çağrısı işlem hattı*bazen denilen *filtre ardışık düzen*.</span><span class="sxs-lookup"><span data-stu-id="44e07-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="44e07-117">ASP.NET Core yürütülecek eylemi seçtikten sonra filtre ardışık düzen çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="44e07-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![İstek, diğer bir ara yazılım, yönlendirme ara yazılım, eylem seçimi ve ASP.NET Core eylem çağrısı işlem hattı işlenir.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="44e07-120">Filtre türleri</span><span class="sxs-lookup"><span data-stu-id="44e07-120">Filter types</span></span>

<span data-ttu-id="44e07-121">Her filtre türü, farklı bir filtre ardışık düzen aşamasında yürütülen:</span><span class="sxs-lookup"><span data-stu-id="44e07-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="44e07-122">[Yetkilendirme filtreleri](#authorization-filters) ilk önce çalışır, kullanıcının istek için yetki verilip verilmediğini belirlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="44e07-123">İstek yetkisiz ise yetkilendirme filtrelerini ardışık kısa devre oluşturur.</span><span class="sxs-lookup"><span data-stu-id="44e07-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="44e07-124">[Kaynak filtreleri](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="44e07-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="44e07-125">Yetkilendirmeden sonra çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="44e07-125">Run after authorization.</span></span>  
  * <span data-ttu-id="44e07-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> Kalan filtre ardışık düzenini önce kod çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="44e07-127">Örneğin, `OnResourceExecuting` kod model bağlama önce çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44e07-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="44e07-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> Kalan ardışık düzenini tamamlandıktan sonra kodu çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44e07-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="44e07-129">[Eylem filtreleri](#action-filters) kod hemen önce ve tek bir eylem yöntemi çağrıldıktan sonra çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44e07-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="44e07-130">Bir eyleme geçirilen bağımsız değişkenleri ve döndürülen eylem sonucu işlemek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="44e07-131">Eylem filtreleri **değil** Razor sayfaları desteklenir.</span><span class="sxs-lookup"><span data-stu-id="44e07-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="44e07-132">[Özel durum filtreleri](#exception-filters) yanıt gövdesi için herhangi bir şey yazılmadan önce gerçekleşen işlenmeyen özel durumlar genel ilkeleri uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="44e07-133">[Sonuç filtreleri](#result-filters) kod hemen önce ve sonra tek tek eylem sonuçlarını yürütülmesini çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44e07-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="44e07-134">Yalnızca eylem yöntemi başarıyla yürütüldü, bunlar çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="44e07-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="44e07-135">Bunlar, görünümü veya biçimlendirici yürütme sarmanız gerekir mantığı için kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="44e07-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="44e07-136">Aşağıdaki diyagramda, filtre türleri filtre işlem hattında nasıl etkileşim kurduğu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="44e07-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![İstek, yetkilendirme filtreleri, kaynak filtreleri, Model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütme işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="44e07-139">Uygulama</span><span class="sxs-lookup"><span data-stu-id="44e07-139">Implementation</span></span>

<span data-ttu-id="44e07-140">Filtreleri farklı arabirimi tanımları aracılığıyla zaman uyumlu ve uyumsuz uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="44e07-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="44e07-141">Zaman uyumlu filtreleri önce kodu çalıştırabilirsiniz (`On-Stage-Executing`) ve sonra (`On-Stage-Executed`), ardışık düzen aşaması.</span><span class="sxs-lookup"><span data-stu-id="44e07-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="44e07-142">Örneğin, `OnActionExecuting` eylem yönteminden önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="44e07-143">`OnActionExecuted` Eylem yöntemi döndükten sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="44e07-144">Zaman uyumsuz filtre tanımlayan bir `On-Stage-ExecutionAsync` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="44e07-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="44e07-145">Önceki kodda, `SampleAsyncActionFilter` sahip bir <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`), eylem yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="44e07-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="44e07-146">Her biri `On-Stage-ExecutionAsync` yöntemleri alır bir `FilterType-ExecutionDelegate` , filtre ardışık düzen aşamasını yürütür.</span><span class="sxs-lookup"><span data-stu-id="44e07-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="44e07-147">Birden çok filtre aşamaları</span><span class="sxs-lookup"><span data-stu-id="44e07-147">Multiple filter stages</span></span>

<span data-ttu-id="44e07-148">Tek bir sınıfta birden çok filtre aşamalar için arabirimleri uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="44e07-149">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sınıfının Implements `IActionFilter`, `IResultFilter`ve zaman uyumsuz eşdeğerlerine.</span><span class="sxs-lookup"><span data-stu-id="44e07-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="44e07-150">Uygulama **ya da** zaman uyumlu veya zaman uyumsuz bir filtre arabirimi sürümü **değil** hem de.</span><span class="sxs-lookup"><span data-stu-id="44e07-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="44e07-151">Çalışma zamanı ilk filtre zaman uyumsuz arabirimini uygulayan ve çağrı yaptığı bu durumda olup olmadığını denetler.</span><span class="sxs-lookup"><span data-stu-id="44e07-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="44e07-152">Aksi durumda, zaman uyumlu arabirim yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="44e07-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="44e07-153">Bir sınıf içinde zaman uyumsuz ve zaman uyumlu arabirimleri olmasa da, yalnızca zaman uyumsuz yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="44e07-154">Soyut sınıflar gibi kullanırken <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> yalnızca zaman uyumlu metotları veya zaman uyumsuz yöntem her filtre türü için geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="44e07-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="44e07-155">Yerleşik filtre öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="44e07-155">Built-in filter attributes</span></span>

<span data-ttu-id="44e07-156">ASP.NET Core, Sınıflandırma ve özelleştirilebilir öznitelik tabanlı yerleşik filtreleri içerir.</span><span class="sxs-lookup"><span data-stu-id="44e07-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="44e07-157">Örneğin, aşağıdaki sonucu filtre üstbilgi yanıta ekler:</span><span class="sxs-lookup"><span data-stu-id="44e07-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="44e07-158">Öznitelikleri, yukarıdaki örnekte gösterildiği gibi bağımsız değişken kabul etmek filtreler sağlar.</span><span class="sxs-lookup"><span data-stu-id="44e07-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="44e07-159">Uygulama `AddHeaderAttribute` bir denetleyici veya eylem yöntemi için adı ve HTTP üstbilgisinin değerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="44e07-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="44e07-160">Filtre arabirimlerinin birkaç özel uygulamalar için temel sınıf olarak kullanılabilecek ilgili özniteliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="44e07-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="44e07-161">Filtre öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="44e07-162">Filtre kapsamları ve Yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="44e07-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="44e07-163">Bir filtre işlem hattının üç birinde eklenebilir *kapsamları*:</span><span class="sxs-lookup"><span data-stu-id="44e07-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="44e07-164">Öznitelik, bir eylem kullanarak.</span><span class="sxs-lookup"><span data-stu-id="44e07-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="44e07-165">Öznitelik, bir denetleyicisinde kullanma.</span><span class="sxs-lookup"><span data-stu-id="44e07-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="44e07-166">Genel olarak tüm denetleyicileri ve aşağıdaki kodda gösterildiği gibi eylemleri için:</span><span class="sxs-lookup"><span data-stu-id="44e07-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="44e07-167">Yukarıdaki kod genel kullanarak üç filtreler ekler [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="44e07-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="44e07-168">Varsayılan yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="44e07-168">Default order of execution</span></span>

<span data-ttu-id="44e07-169">Belirli bir ardışık düzen aşaması için birden çok filtre olduğunda, filtre yürütme varsayılan sıra kapsamı belirler.</span><span class="sxs-lookup"><span data-stu-id="44e07-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="44e07-170">Genel filtrelerin sırayla yöntemi filtreleri çevreleyen sınıf filtreleri alın.</span><span class="sxs-lookup"><span data-stu-id="44e07-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="44e07-171">Filtre iç içe geçme sonucunda *sonra* kodu filtre ters sırada çalışır *önce* kod.</span><span class="sxs-lookup"><span data-stu-id="44e07-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="44e07-172">Filtre sırası:</span><span class="sxs-lookup"><span data-stu-id="44e07-172">The filter sequence:</span></span>

* <span data-ttu-id="44e07-173">*Önce* kod genel filtre.</span><span class="sxs-lookup"><span data-stu-id="44e07-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="44e07-174">*Önce* denetleyici filtreleri kodu.</span><span class="sxs-lookup"><span data-stu-id="44e07-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="44e07-175">*Önce* kod eylem yöntemi filtreler.</span><span class="sxs-lookup"><span data-stu-id="44e07-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="44e07-176">*Sonra* kod eylem yöntemi filtreler.</span><span class="sxs-lookup"><span data-stu-id="44e07-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="44e07-177">*Sonra* denetleyici filtreleri kodu.</span><span class="sxs-lookup"><span data-stu-id="44e07-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="44e07-178">*Sonra* kod genel filtre.</span><span class="sxs-lookup"><span data-stu-id="44e07-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="44e07-179">Aşağıdaki örnek filtre yöntemleri zaman uyumlu eylem filtreleri için çağrılma sırasını göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="44e07-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="44e07-180">Sequence</span><span class="sxs-lookup"><span data-stu-id="44e07-180">Sequence</span></span> | <span data-ttu-id="44e07-181">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="44e07-181">Filter scope</span></span> | <span data-ttu-id="44e07-182">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="44e07-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="44e07-183">1\.</span><span class="sxs-lookup"><span data-stu-id="44e07-183">1</span></span> | <span data-ttu-id="44e07-184">Global</span><span class="sxs-lookup"><span data-stu-id="44e07-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="44e07-185">2</span><span class="sxs-lookup"><span data-stu-id="44e07-185">2</span></span> | <span data-ttu-id="44e07-186">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="44e07-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="44e07-187">3</span><span class="sxs-lookup"><span data-stu-id="44e07-187">3</span></span> | <span data-ttu-id="44e07-188">Yöntem</span><span class="sxs-lookup"><span data-stu-id="44e07-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="44e07-189">4</span><span class="sxs-lookup"><span data-stu-id="44e07-189">4</span></span> | <span data-ttu-id="44e07-190">Yöntem</span><span class="sxs-lookup"><span data-stu-id="44e07-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="44e07-191">5</span><span class="sxs-lookup"><span data-stu-id="44e07-191">5</span></span> | <span data-ttu-id="44e07-192">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="44e07-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="44e07-193">6</span><span class="sxs-lookup"><span data-stu-id="44e07-193">6</span></span> | <span data-ttu-id="44e07-194">Global</span><span class="sxs-lookup"><span data-stu-id="44e07-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="44e07-195">Bu dizisini gösterir:</span><span class="sxs-lookup"><span data-stu-id="44e07-195">This sequence shows:</span></span>

* <span data-ttu-id="44e07-196">Yöntem filtresi denetleyici filtresi içinde yer alıyor.</span><span class="sxs-lookup"><span data-stu-id="44e07-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="44e07-197">Denetleyici filtreyi genel filtre içinde yer alıyor.</span><span class="sxs-lookup"><span data-stu-id="44e07-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="44e07-198">Denetleyici ve Razor sayfa düzeyi filtreleri</span><span class="sxs-lookup"><span data-stu-id="44e07-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="44e07-199">Devralınan denetleyicilerin <xref:Microsoft.AspNetCore.Mvc.Controller> taban sınıfı içeren [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), ve [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*) 
 `OnActionExecuted` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="44e07-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="44e07-200">Bu yöntemler:</span><span class="sxs-lookup"><span data-stu-id="44e07-200">These methods:</span></span>

* <span data-ttu-id="44e07-201">Çalıştıran belirli bir eylem filtrelerini kaydır.</span><span class="sxs-lookup"><span data-stu-id="44e07-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="44e07-202">`OnActionExecuting` herhangi bir eylem filtreleri önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="44e07-203">`OnActionExecuted` Tüm eylem filtrelerini sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="44e07-204">`OnActionExecutionAsync` herhangi bir eylem filtreleri önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="44e07-205">Kod sonra filtre `next` sonra eylem yönteminin çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="44e07-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="44e07-206">Örneğin, yükleme örnekteki `MySampleActionFilter` başlangıç genel olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="44e07-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="44e07-207">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="44e07-207">The `TestController`:</span></span>

* <span data-ttu-id="44e07-208">Geçerli `SampleActionFilterAttribute` (`[SampleActionFilter]`) için `FilterTest2` eylem:</span><span class="sxs-lookup"><span data-stu-id="44e07-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action:</span></span>
* <span data-ttu-id="44e07-209">Geçersiz kılmalar `OnActionExecuting` ve `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="44e07-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="44e07-210">Gezinme `https://localhost:5001/Test/FilterTest2` aşağıdaki kodu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="44e07-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="44e07-211">Razor sayfaları için bkz: [Filtreleri Uygula Razor sayfası filtre yöntemi geçersiz kılarak](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="44e07-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="44e07-212">Varsayılan sıra geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="44e07-212">Overriding the default order</span></span>

<span data-ttu-id="44e07-213">Varsayılan sıralı yürütme uygulama tarafından geçersiz kılınabilir <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="44e07-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="44e07-214">`IOrderedFilter` kullanıma sunan <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> yürütme sırasını belirlemek için kapsam üzerinde önceliklidir özelliği.</span><span class="sxs-lookup"><span data-stu-id="44e07-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="44e07-215">Daha düşük bir filtre `Order` değeri:</span><span class="sxs-lookup"><span data-stu-id="44e07-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="44e07-216">Çalıştırmaları *önce* kodu daha yüksek bir değere sahip bir filtre, önce `Order`.</span><span class="sxs-lookup"><span data-stu-id="44e07-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="44e07-217">Çalıştırmaları *sonra* kodun hemen ardından, daha yüksek bir filtre `Order` değeri.</span><span class="sxs-lookup"><span data-stu-id="44e07-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="44e07-218">`Order` Özelliği ile bir oluşturucu parametresi ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="44e07-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="44e07-219">Önceki örnekte gösterilen aynı 3 eylem filtreleri göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="44e07-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="44e07-220">Varsa `Order` denetleyici ve genel filtre özelliği ayarlandığında 1 ve 2'ye sırasıyla, yürütme sıra ters çevrilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="44e07-221">Sequence</span><span class="sxs-lookup"><span data-stu-id="44e07-221">Sequence</span></span> | <span data-ttu-id="44e07-222">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="44e07-222">Filter scope</span></span> | <span data-ttu-id="44e07-223">`Order` Özelliği</span><span class="sxs-lookup"><span data-stu-id="44e07-223">`Order` property</span></span> | <span data-ttu-id="44e07-224">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="44e07-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="44e07-225">1\.</span><span class="sxs-lookup"><span data-stu-id="44e07-225">1</span></span> | <span data-ttu-id="44e07-226">Yöntem</span><span class="sxs-lookup"><span data-stu-id="44e07-226">Method</span></span> | <span data-ttu-id="44e07-227">0</span><span class="sxs-lookup"><span data-stu-id="44e07-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="44e07-228">2</span><span class="sxs-lookup"><span data-stu-id="44e07-228">2</span></span> | <span data-ttu-id="44e07-229">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="44e07-229">Controller</span></span> | <span data-ttu-id="44e07-230">1\.</span><span class="sxs-lookup"><span data-stu-id="44e07-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="44e07-231">3</span><span class="sxs-lookup"><span data-stu-id="44e07-231">3</span></span> | <span data-ttu-id="44e07-232">Global</span><span class="sxs-lookup"><span data-stu-id="44e07-232">Global</span></span> | <span data-ttu-id="44e07-233">2</span><span class="sxs-lookup"><span data-stu-id="44e07-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="44e07-234">4</span><span class="sxs-lookup"><span data-stu-id="44e07-234">4</span></span> | <span data-ttu-id="44e07-235">Global</span><span class="sxs-lookup"><span data-stu-id="44e07-235">Global</span></span> | <span data-ttu-id="44e07-236">2</span><span class="sxs-lookup"><span data-stu-id="44e07-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="44e07-237">5</span><span class="sxs-lookup"><span data-stu-id="44e07-237">5</span></span> | <span data-ttu-id="44e07-238">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="44e07-238">Controller</span></span> | <span data-ttu-id="44e07-239">1\.</span><span class="sxs-lookup"><span data-stu-id="44e07-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="44e07-240">6</span><span class="sxs-lookup"><span data-stu-id="44e07-240">6</span></span> | <span data-ttu-id="44e07-241">Yöntem</span><span class="sxs-lookup"><span data-stu-id="44e07-241">Method</span></span> | <span data-ttu-id="44e07-242">0</span><span class="sxs-lookup"><span data-stu-id="44e07-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="44e07-243">`Order` Özelliğini çalıştırma hangi filtreleri sırayla belirlerken kapsamı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="44e07-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="44e07-244">Filtreleri ilk sıraya göre sıralanır ve kapsam TIES ayırmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="44e07-245">Tüm yerleşik filtreleri uygulamak `IOrderedFilter` ve varsayılan `Order` 0 değeri.</span><span class="sxs-lookup"><span data-stu-id="44e07-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="44e07-246">Yerleşik filtreleri için sürece kapsam sırasını belirleyen `Order` sıfır olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="44e07-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="44e07-247">İptal ve kısa devre</span><span class="sxs-lookup"><span data-stu-id="44e07-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="44e07-248">Filtre ardışık düzen ayarlayarak short-circuited <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> özelliği <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> filter yöntemi için sağlanan parametre.</span><span class="sxs-lookup"><span data-stu-id="44e07-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="44e07-249">Örneğin, aşağıdaki kaynak filtre rest işlem hattının yürütülmesini engeller:</span><span class="sxs-lookup"><span data-stu-id="44e07-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="44e07-250">Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` ve `AddHeader` filtre hedef `SomeResource` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="44e07-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="44e07-251">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="44e07-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="44e07-252">Kaynak Filtresi olduğu için ilk olarak çalışır ve `AddHeader` bir eylem filtresi.</span><span class="sxs-lookup"><span data-stu-id="44e07-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="44e07-253">Kalan ardışık düzenini short-circuits.</span><span class="sxs-lookup"><span data-stu-id="44e07-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="44e07-254">Bu nedenle `AddHeader` filtresi hiç çalıştığı için `SomeResource` eylem.</span><span class="sxs-lookup"><span data-stu-id="44e07-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="44e07-255">Belirtilen eylem yöntemini düzeyinde hem de bir filtre uygulansaydı, bu davranışı aynı olacaktır `ShortCircuitingResourceFilter` ilk çalıştı.</span><span class="sxs-lookup"><span data-stu-id="44e07-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="44e07-256">`ShortCircuitingResourceFilter` Filtre türü nedeniyle ya da açık kullanılarak ilk çalıştıran `Order` özelliği.</span><span class="sxs-lookup"><span data-stu-id="44e07-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="44e07-257">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="44e07-257">Dependency injection</span></span>

<span data-ttu-id="44e07-258">Filtre türü veya örnek tarafından eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="44e07-259">Bir örneği eklenirse, bu örneği her istek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="44e07-260">Bir tür eklenirse, türü etkinleştirildi.</span><span class="sxs-lookup"><span data-stu-id="44e07-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="44e07-261">Türü tarafından etkinleştirilen bir filtre anlamına gelir:</span><span class="sxs-lookup"><span data-stu-id="44e07-261">A type-activated filter means:</span></span>

* <span data-ttu-id="44e07-262">Her istek için bir örneği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="44e07-262">An instance is created for each request.</span></span>
* <span data-ttu-id="44e07-263">Oluşturucu herhangi bir bağımlılığın tarafından doldurulur [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı).</span><span class="sxs-lookup"><span data-stu-id="44e07-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="44e07-264">Öznitelik olarak uygulanan ve doğrudan denetleyici sınıflarına veya eylem yöntemlerine eklenen filtreler tarafından sağlanan Oluşturucusu bağımlılıkları olamaz [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı).</span><span class="sxs-lookup"><span data-stu-id="44e07-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="44e07-265">Oluşturucu bağımlılıkları olduğundan DI tarafından sağlanamaz:</span><span class="sxs-lookup"><span data-stu-id="44e07-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="44e07-266">Öznitelik oluşturucu parametrelerinin nereye uygulanan sağlanan olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="44e07-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="44e07-267">Bu öznitelikler nasıl işe sınırlamasıdır.</span><span class="sxs-lookup"><span data-stu-id="44e07-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="44e07-268">Aşağıdaki filtreler DI sağlanan Oluşturucu bağımlılıkları destekler:</span><span class="sxs-lookup"><span data-stu-id="44e07-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="44e07-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> öznitelikteki uygulanır.</span><span class="sxs-lookup"><span data-stu-id="44e07-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="44e07-270">Bir denetleyici veya eylem yöntemi için yukarıdaki filtreleri uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="44e07-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="44e07-271">Günlükçüleri DI ' kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-271">Loggers are available from DI.</span></span> <span data-ttu-id="44e07-272">Bununla birlikte, oluşturma ve filtreler kullanılarak yalnızca günlüğe kaydetme amacıyla kaçının.</span><span class="sxs-lookup"><span data-stu-id="44e07-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="44e07-273">[Yerleşik framework günlük kaydını](xref:fundamentals/logging/index) günlüğü için ihtiyacınız olan şey genellikle sağlar.</span><span class="sxs-lookup"><span data-stu-id="44e07-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="44e07-274">Günlüğe kaydetme filtreleri eklendi:</span><span class="sxs-lookup"><span data-stu-id="44e07-274">Logging added to filters:</span></span>

* <span data-ttu-id="44e07-275">İşletme etki alanı sorunlarını veya filtre belirli davranış odaklanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="44e07-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="44e07-276">Gereken **değil** eylemler veya diğer framework olayları oturum.</span><span class="sxs-lookup"><span data-stu-id="44e07-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="44e07-277">Yerleşik filtreleri, eylemleri ve framework olayları günlüğe.</span><span class="sxs-lookup"><span data-stu-id="44e07-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="44e07-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="44e07-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="44e07-279">Hizmet uygulaması türlerini filtreleme kaydedilen `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="44e07-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="44e07-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> DI filtre örneğini alır.</span><span class="sxs-lookup"><span data-stu-id="44e07-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="44e07-281">Aşağıdaki kodda gösterildiği `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="44e07-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="44e07-282">Aşağıdaki kodda, `AddHeaderResultServiceFilter` DI kapsayıcıya eklenir:</span><span class="sxs-lookup"><span data-stu-id="44e07-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="44e07-283">Aşağıdaki kodda, `ServiceFilter` özniteliği bir örneğini alır `AddHeaderResultServiceFilter` filtresinden dı:</span><span class="sxs-lookup"><span data-stu-id="44e07-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="44e07-284">Kullanırken `ServiceFilterAttribute`ayarını [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="44e07-284">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="44e07-285">Bir ipucu sağlayan filtre örneğini *olabilir* içinde oluşturulduğu istek kapsamı dışında yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="44e07-286">ASP.NET Core çalışma zamanı garantisi vermez:</span><span class="sxs-lookup"><span data-stu-id="44e07-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="44e07-287">Filtre tek bir örneğini oluşturulmasına.</span><span class="sxs-lookup"><span data-stu-id="44e07-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="44e07-288">Filtre, belirli bir sonraki noktada DI kapsayıcısından yeniden istenen olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="44e07-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="44e07-289">Singleton dışında bir yaşam süresi hizmetleriyle bağımlı bir filtre ile kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="44e07-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="44e07-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> uygulayan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="44e07-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="44e07-291">`IFilterFactory` kullanıma sunan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi oluşturmak için bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği.</span><span class="sxs-lookup"><span data-stu-id="44e07-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="44e07-292">`CreateInstance` Belirtilen tür DI yükler.</span><span class="sxs-lookup"><span data-stu-id="44e07-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="44e07-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="44e07-293">TypeFilterAttribute</span></span>

<span data-ttu-id="44e07-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> benzer <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, ancak türü doğrudan DI kapsayıcısından çözülmüş değildir.</span><span class="sxs-lookup"><span data-stu-id="44e07-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="44e07-295">Kullanarak türün örneğini oluşturduğunda <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="44e07-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="44e07-296">Çünkü `TypeFilterAttribute` türleri olmayan çözümlenen doğrudan DI kapsayıcısından:</span><span class="sxs-lookup"><span data-stu-id="44e07-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="44e07-297">Kullanılarak başvurulan türleri `TypeFilterAttribute` DI kapsayıcı ile kayıtlı olması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="44e07-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="44e07-298">DI kapsayıcı tarafından yerine bunların bağımlılıklarını sahiptirler.</span><span class="sxs-lookup"><span data-stu-id="44e07-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="44e07-299">`TypeFilterAttribute` İsteğe bağlı olarak tür için oluşturucu bağımsız değişkenleri kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="44e07-300">Kullanırken `TypeFilterAttribute`ayarını [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="44e07-300">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="44e07-301">İpucu sağlayan filtre örneğini *olabilir* içinde oluşturulduğu istek kapsamı dışında yeniden kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-301">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="44e07-302">ASP.NET Core çalışma zamanı filtre tek bir örneği oluşturulur tutarlılık garantisi sağlar.</span><span class="sxs-lookup"><span data-stu-id="44e07-302">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="44e07-303">Singleton dışında bir yaşam süresi hizmetleriyle bağımlı bir filtre ile kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="44e07-303">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="44e07-304">Aşağıdaki örneği kullanarak bir tür bağımsız değişkenleri geçirmek gösterilmektedir `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="44e07-304">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="44e07-305">Yetkilendirme filtreleri</span><span class="sxs-lookup"><span data-stu-id="44e07-305">Authorization filters</span></span>

<span data-ttu-id="44e07-306">Yetkilendirme filtreleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-306">Authorization filters:</span></span>

* <span data-ttu-id="44e07-307">İlk filtreler, filtre ardışık çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-307">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="44e07-308">Eylem yöntemlerine erişimi denetler.</span><span class="sxs-lookup"><span data-stu-id="44e07-308">Control access to action methods.</span></span>
* <span data-ttu-id="44e07-309">Sahip bir yöntemi önce ancak bundan sonra yöntemi.</span><span class="sxs-lookup"><span data-stu-id="44e07-309">Have a before method, but no after method.</span></span>

<span data-ttu-id="44e07-310">Özel yetkilendirme filtrelerini özel yetkilendirme framework gerektirir.</span><span class="sxs-lookup"><span data-stu-id="44e07-310">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="44e07-311">Yetkilendirme ilkeleri yapılandırarak veya özel yetkilendirme ilkesi özel filtre yazma üzerine yazma seçeneğini tercih eder.</span><span class="sxs-lookup"><span data-stu-id="44e07-311">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="44e07-312">Yerleşik yetkilendirme Filtresi:</span><span class="sxs-lookup"><span data-stu-id="44e07-312">The built-in authorization filter:</span></span>

* <span data-ttu-id="44e07-313">Yetkilendirme sistem çağırır.</span><span class="sxs-lookup"><span data-stu-id="44e07-313">Calls the authorization system.</span></span>
* <span data-ttu-id="44e07-314">İstekleri yetkisi yok.</span><span class="sxs-lookup"><span data-stu-id="44e07-314">Does not authorize requests.</span></span>

<span data-ttu-id="44e07-315">Yapmak **değil** yetkilendirme filtrelerini içinde özel durumlar:</span><span class="sxs-lookup"><span data-stu-id="44e07-315">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="44e07-316">Özel durumun işlenip değil.</span><span class="sxs-lookup"><span data-stu-id="44e07-316">The exception will not be handled.</span></span>
* <span data-ttu-id="44e07-317">Özel durum filtreleri özel durum işleme yok.</span><span class="sxs-lookup"><span data-stu-id="44e07-317">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="44e07-318">Bir yetkilendirme filtresi bir özel durum oluştuğunda bir sınama vermeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="44e07-318">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="44e07-319">Daha fazla bilgi edinin [yetkilendirme](xref:security/authorization/introduction).</span><span class="sxs-lookup"><span data-stu-id="44e07-319">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="44e07-320">Kaynak filtreleri</span><span class="sxs-lookup"><span data-stu-id="44e07-320">Resource filters</span></span>

<span data-ttu-id="44e07-321">Kaynak filtreleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-321">Resource filters:</span></span>

* <span data-ttu-id="44e07-322">Ya da uygulama <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="44e07-322">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="44e07-323">Yürütme, filtre ardışık düzen çoğunu sarmalar.</span><span class="sxs-lookup"><span data-stu-id="44e07-323">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="44e07-324">Yalnızca [yetkilendirme filtrelerini](#authorization-filters) kaynak filtreleri önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="44e07-324">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="44e07-325">Kaynak filtreleri çoğu işlem hattı iki şekilde yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="44e07-325">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="44e07-326">Örneğin, bir önbelleğe alma filtre isabetli önbellek okuması işlem hattını geri kalanını önleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44e07-326">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="44e07-327">Kaynak Filtresi örnekleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-327">Resource filter examples:</span></span>

* <span data-ttu-id="44e07-328">[Short-circuiting kaynak filtresi](#short-circuiting-resource-filter) daha önce gösterilen.</span><span class="sxs-lookup"><span data-stu-id="44e07-328">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="44e07-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="44e07-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="44e07-330">Model bağlama form verilerini erişmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="44e07-330">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="44e07-331">Form verileri belleğe okuma önlemek için büyük dosya yüklemeleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-331">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="44e07-332">Eylem filtreleri</span><span class="sxs-lookup"><span data-stu-id="44e07-332">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44e07-333">Eylem filtreleri yapmak **değil** Razor sayfaları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="44e07-333">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="44e07-334">Razor sayfaları destekler <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span><span class="sxs-lookup"><span data-stu-id="44e07-334">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="44e07-335">Daha fazla bilgi için [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="44e07-335">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="44e07-336">Eylem filtreleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-336">Action filters:</span></span>

* <span data-ttu-id="44e07-337">Ya da uygulama <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> arabirimi.</span><span class="sxs-lookup"><span data-stu-id="44e07-337">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="44e07-338">Eylem yöntemleri yürütülmesini yürütülmesi çevreler.</span><span class="sxs-lookup"><span data-stu-id="44e07-338">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="44e07-339">Aşağıdaki kod örneği eylem filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="44e07-339">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="44e07-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> Aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="44e07-340">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="44e07-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> -bir eylem yöntemine girişleri etkinleştirir okuyun.</span><span class="sxs-lookup"><span data-stu-id="44e07-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="44e07-342"><xref:Microsoft.AspNetCore.Mvc.Controller> -denetleyici örneği düzenleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="44e07-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="44e07-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> -ayarlama `Result` sonraki eylem filtreleri eylem yöntemi ve yürütme short-circuits.</span><span class="sxs-lookup"><span data-stu-id="44e07-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="44e07-344">Bir eylem yöntemi bir özel durum:</span><span class="sxs-lookup"><span data-stu-id="44e07-344">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="44e07-345">Sonraki filtrelerin çalışmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="44e07-345">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="44e07-346">Ayar aksine `Result`, başarılı sonuç yerine bir hata olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-346">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="44e07-347"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> Sağlar `Controller` ve `Result` ayrıca aşağıdaki özellikleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="44e07-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> -Eylem yürütme başka bir filtre tarafından kısa devre yapılma gerekiyorsa true.</span><span class="sxs-lookup"><span data-stu-id="44e07-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="44e07-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> Eylem ya da daha önce çalışan eylem filtresi bir özel durum oluşturduysa null olmayan.</span><span class="sxs-lookup"><span data-stu-id="44e07-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="44e07-350">Bu özelliği null olarak ayarlama:</span><span class="sxs-lookup"><span data-stu-id="44e07-350">Setting this property to null:</span></span>

  * <span data-ttu-id="44e07-351">Etkili bir şekilde özel durumu işler.</span><span class="sxs-lookup"><span data-stu-id="44e07-351">Effectively handles the exception.</span></span>
  * <span data-ttu-id="44e07-352">`Result` Bu eylem yönteminden döndürülen yokmuş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="44e07-352">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="44e07-353">İçin bir `IAsyncActionFilter`, çağrı <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="44e07-353">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="44e07-354">Herhangi bir sonraki eylem filtrelerini ve eylem yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="44e07-354">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="44e07-355">Döndürür `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="44e07-355">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="44e07-356">Kısa devre oluşturur, Ata <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> bir sonuç örneği ve Remove() çağırmayın `next` ( `ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="44e07-356">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="44e07-357">Bir soyut bir çerçeve sağlar <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> , sınıflandırma.</span><span class="sxs-lookup"><span data-stu-id="44e07-357">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="44e07-358">`OnActionExecuting` Eylem filtresi için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="44e07-358">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="44e07-359">Model durumu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="44e07-359">Validate model state.</span></span>
* <span data-ttu-id="44e07-360">Durumu geçersiz bir hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="44e07-360">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="44e07-361">`OnActionExecuted` Yöntemi çalıştıktan sonra eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="44e07-361">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="44e07-362">Ve görebilir ve eylemin sonuçlarını işlemek <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> özelliği.</span><span class="sxs-lookup"><span data-stu-id="44e07-362">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="44e07-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> Eylem yürütme başka bir filtre tarafından kısa devre yapılma gerekiyorsa true olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="44e07-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="44e07-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> Eylem veya bir sonraki eylem filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="44e07-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="44e07-365">Ayar `Exception` null:</span><span class="sxs-lookup"><span data-stu-id="44e07-365">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="44e07-366">Bir özel durum etkili bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="44e07-366">Effectively handles an exception.</span></span>
  * <span data-ttu-id="44e07-367">`ActionExecutedContext.Result` Bu normalde eylem yönteminden döndürülen yokmuş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="44e07-367">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="44e07-368">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="44e07-368">Exception filters</span></span>

<span data-ttu-id="44e07-369">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-369">Exception filters:</span></span>

* <span data-ttu-id="44e07-370">Uygulama <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="44e07-370">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="44e07-371">Genel hata işleme ilkeleri uygulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-371">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="44e07-372">Aşağıdaki örnek özel durum filtresi, uygulama geliştirme olduğunda oluşan özel durumları hakkında ayrıntıları görüntülemek için bir özel hata görünümü kullanır:</span><span class="sxs-lookup"><span data-stu-id="44e07-372">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="44e07-373">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-373">Exception filters:</span></span>

* <span data-ttu-id="44e07-374">Önceki ve sonraki olaylar yok.</span><span class="sxs-lookup"><span data-stu-id="44e07-374">Don't have before and after events.</span></span>
* <span data-ttu-id="44e07-375">Uygulama <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="44e07-375">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="44e07-376">Razor sayfası veya denetleyici oluşturma, meydana gelen işlenmeyen özel durumları işlemek [model bağlama](xref:mvc/models/model-binding), eylem filtreleri ya da eylem yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="44e07-376">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="44e07-377">Yapmak **değil** kaynak filtreleri, sonuç filtrelerini veya MVC sonucu yürütme oluşan özel durumları yakalama.</span><span class="sxs-lookup"><span data-stu-id="44e07-377">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="44e07-378">Bir özel durumu işlemek üzere ayarlanmış <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> özelliğini `true` veya yanıt yazın.</span><span class="sxs-lookup"><span data-stu-id="44e07-378">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="44e07-379">Bu özel durumun yayılmasını durdurur.</span><span class="sxs-lookup"><span data-stu-id="44e07-379">This stops propagation of the exception.</span></span> <span data-ttu-id="44e07-380">Özel Durum Filtresi, bir özel durum "başarılı" içine kapatamazsınız.</span><span class="sxs-lookup"><span data-stu-id="44e07-380">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="44e07-381">Yalnızca bir eyleme eylem filtresi, bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="44e07-381">Only an action filter can do that.</span></span>

<span data-ttu-id="44e07-382">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-382">Exception filters:</span></span>

* <span data-ttu-id="44e07-383">Eylemler içinde gerçekleşen Yakalama özel durumlar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="44e07-383">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="44e07-384">Ara yazılım işleme hata olarak kadar esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="44e07-384">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="44e07-385">Özel durum işleme için bir ara yazılım tercih eder.</span><span class="sxs-lookup"><span data-stu-id="44e07-385">Prefer middleware for exception handling.</span></span> <span data-ttu-id="44e07-386">Kullanım özel durum filtreleri yalnızca nereye hata işleme *farklı* göre hangi eylem yöntemi çağrılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-386">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="44e07-387">Örneğin, bir uygulama, görünümler/HTML ve her iki API uç noktaları için eylem yöntemleri olabilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-387">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="44e07-388">API uç noktaları, HTML olarak bir hata sayfası görüntüleme tabanlı eylemleri döndürebilir sırasında hata bilgisi, JSON olarak döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-388">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="44e07-389">Sonuç filtreleri</span><span class="sxs-lookup"><span data-stu-id="44e07-389">Result filters</span></span>

<span data-ttu-id="44e07-390">Sonuç filtreleri:</span><span class="sxs-lookup"><span data-stu-id="44e07-390">Result filters:</span></span>

* <span data-ttu-id="44e07-391">Bir arabirim uygular:</span><span class="sxs-lookup"><span data-stu-id="44e07-391">Implement an interface:</span></span>
  * <span data-ttu-id="44e07-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="44e07-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="44e07-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="44e07-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="44e07-394">Yürütülmesi, eylem sonuçlarını yürütülmesini çevreler.</span><span class="sxs-lookup"><span data-stu-id="44e07-394">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="44e07-395">IResultFilter ve IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="44e07-395">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="44e07-396">Aşağıdaki kod, bir HTTP üstbilgisi ekler bir sonuç filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="44e07-396">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="44e07-397">Yürütülmekte olan sonuç türünü eylemini bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="44e07-397">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="44e07-398">Bir parçası olarak işleme tüm razor görünüm döndüren bir eylem verilebilir <xref:Microsoft.AspNetCore.Mvc.ViewResult> yürütülmekte.</span><span class="sxs-lookup"><span data-stu-id="44e07-398">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="44e07-399">Bir API yöntemi, bazı serileştirme yürütme sonucu bir parçası olarak gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="44e07-399">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="44e07-400">Daha fazla bilgi edinin [eylem sonuçları](xref:mvc/controllers/actions)</span><span class="sxs-lookup"><span data-stu-id="44e07-400">Learn more about [action results](xref:mvc/controllers/actions)</span></span>

<span data-ttu-id="44e07-401">Eylem veya eylem filtreleri eylem sonucu üretir, sonuç filtreleri için başarılı sonuçları - yalnızca çalıştırılır.</span><span class="sxs-lookup"><span data-stu-id="44e07-401">Result filters are only executed for successful results - when the action or action filters produce an action result.</span></span> <span data-ttu-id="44e07-402">Özel durum filtreleri, bir özel durumu işlediğinizde sonuç filtrelerini yürütülmedi.</span><span class="sxs-lookup"><span data-stu-id="44e07-402">Result filters are not executed when exception filters handle an exception.</span></span>

<span data-ttu-id="44e07-403"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> Yöntemi iki yürütme sonraki sonuç filtreleri ve eylem sonucu ayarlayarak <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> için `true`.</span><span class="sxs-lookup"><span data-stu-id="44e07-403">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="44e07-404">Yanıt nesnesi için boş bir yanıt oluşturmaktan kaçınmak için kısa devre olduğunda yazın.</span><span class="sxs-lookup"><span data-stu-id="44e07-404">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="44e07-405">Bir özel durum `IResultFilter.OnResultExecuting` olur:</span><span class="sxs-lookup"><span data-stu-id="44e07-405">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="44e07-406">Sonraki filtrelerin ve eylem sonucu yürütülmesini engeller.</span><span class="sxs-lookup"><span data-stu-id="44e07-406">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="44e07-407">Başarılı sonuç yerine bir hata olarak kabul edilecek.</span><span class="sxs-lookup"><span data-stu-id="44e07-407">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="44e07-408">Zaman <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> yöntemi çalışır:</span><span class="sxs-lookup"><span data-stu-id="44e07-408">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs:</span></span>

* <span data-ttu-id="44e07-409">Yanıtı istemciye büyük olasılıkla gönderildi ve değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="44e07-409">The response has likely been sent to the client and cannot be changed.</span></span>
* <span data-ttu-id="44e07-410">Bir özel durum oluştu, yanıt gövdesi gönderilmez.</span><span class="sxs-lookup"><span data-stu-id="44e07-410">If an exception was thrown, the response body is not sent.</span></span>

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

<span data-ttu-id="44e07-411">`ResultExecutedContext.Canceled` ayarlanır `true` yürütülen eylem sonucu başka bir filtre tarafından kısa devre yapılma durumunda.</span><span class="sxs-lookup"><span data-stu-id="44e07-411">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="44e07-412">`ResultExecutedContext.Exception` Eylem sonucu veya bir sonraki sonuç filtresi bir özel durum oluşturduysa, bir null olmayan değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="44e07-412">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="44e07-413">Ayar `Exception` için null etkili bir şekilde bir özel durum işleme ve daha sonra işlem hattı, ASP.NET Core tarafından işlenemezse gelen özel durumu önler.</span><span class="sxs-lookup"><span data-stu-id="44e07-413">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="44e07-414">Bir sonuç filtresi bir özel durum işlenirken bir yanıtı veri yazmak için güvenilir bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="44e07-414">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="44e07-415">Eylem sonucu bir özel durum oluşturduğunda üstbilgileri istemciye temizlenmiş, bir hata kodu göndermek için güvenilir bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="44e07-415">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="44e07-416">İçin bir <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, çağrı `await next` üzerinde <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> herhangi bir sonraki sonuç filtre ve eylem sonucu yürütür.</span><span class="sxs-lookup"><span data-stu-id="44e07-416">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="44e07-417">Kısa devre oluşturur, ayarlayın [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) için `true` ve Remove() çağırmayın `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="44e07-417">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="44e07-418">Bir soyut bir çerçeve sağlar `ResultFilterAttribute` , sınıflandırma.</span><span class="sxs-lookup"><span data-stu-id="44e07-418">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="44e07-419">[AddHeaderAttribute](#add-header-attribute) gösterilen sınıfı daha önce bir sonuç filtre özniteliği örneği verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="44e07-419">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="44e07-420">IAlwaysRunResultFilter ve IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="44e07-420">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="44e07-421"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> Ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> arabirimleri bildirmek bir <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> için tüm eylem sonuçlarına çalışan uygulama.</span><span class="sxs-lookup"><span data-stu-id="44e07-421">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="44e07-422">Filtre sürece tüm eylem sonuçlarına uygulanır:</span><span class="sxs-lookup"><span data-stu-id="44e07-422">The filter is applied to all action results unless:</span></span>

* <span data-ttu-id="44e07-423">Bir <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> ve yanıt short-circuits uygular.</span><span class="sxs-lookup"><span data-stu-id="44e07-423">An <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> applies and short-circuits the response.</span></span>
* <span data-ttu-id="44e07-424">Özel Durum Filtresi, eylem sonucunu üreten tarafından bir özel durumu işler.</span><span class="sxs-lookup"><span data-stu-id="44e07-424">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="44e07-425">Filtreler dışında `IExceptionFilter` ve `IAuthorizationFilter` iki yoksa `IAlwaysRunResultFilter` ve `IAsyncAlwaysRunResultFilter`.</span><span class="sxs-lookup"><span data-stu-id="44e07-425">Filters other than `IExceptionFilter` and `IAuthorizationFilter` don't short-circuit `IAlwaysRunResultFilter` and `IAsyncAlwaysRunResultFilter`.</span></span>

<span data-ttu-id="44e07-426">Örneğin, aşağıdaki filtre her zaman çalışır ve eylem sonucunu ayarlar (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) ile bir *422 işlenemeyen* içerik anlaşması başarısız olduğunda, durum kodu:</span><span class="sxs-lookup"><span data-stu-id="44e07-426">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="44e07-427">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="44e07-427">IFilterFactory</span></span>

<span data-ttu-id="44e07-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> uygulayan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="44e07-428"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="44e07-429">Bu nedenle, bir `IFilterFactory` örneği olarak kullanılabilir bir `IFilterMetadata` filtre işlem hattının herhangi bir yerindeki örneği.</span><span class="sxs-lookup"><span data-stu-id="44e07-429">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="44e07-430">Çalışma zamanı, filtre çağırmak hazırlanırken yayınlayacağınızı çalışır bir `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="44e07-430">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="44e07-431">Bu tür dönüştürme başarılı olursa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi oluşturmak için çağrılır `IFilterMetadata` çağrılan örnek.</span><span class="sxs-lookup"><span data-stu-id="44e07-431">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="44e07-432">Bu, kesin filtre ardışık düzen uygulama başlatıldığında açıkça ayarlanması gerekmez bu yana esnek bir tasarım sağlar.</span><span class="sxs-lookup"><span data-stu-id="44e07-432">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="44e07-433">`IFilterFactory` özel öznitelik uygulamaları filtreleri oluşturma başka bir yaklaşım kullanarak uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="44e07-433">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="44e07-434">Yukarıdaki kod çalıştırarak test edilebilir [örneği indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="44e07-434">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="44e07-435">F12 Geliştirici araçlarıyla çağırın.</span><span class="sxs-lookup"><span data-stu-id="44e07-435">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="44e07-436">Gidin `https://localhost:5001/Sample/HeaderWithFactory`</span><span class="sxs-lookup"><span data-stu-id="44e07-436">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`</span></span>

<span data-ttu-id="44e07-437">F12 Geliştirici araçlarıyla örnek kod tarafından eklenen aşağıdaki yanıt üstbilgilerini görüntüleme:</span><span class="sxs-lookup"><span data-stu-id="44e07-437">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="44e07-438">**Yazar:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="44e07-438">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="44e07-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="44e07-439">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="44e07-440">**İç:** `My header`</span><span class="sxs-lookup"><span data-stu-id="44e07-440">**internal:** `My header`</span></span>

<span data-ttu-id="44e07-441">Yukarıdaki kod oluşturur **iç:** `My header` yanıtı üstbilgisi.</span><span class="sxs-lookup"><span data-stu-id="44e07-441">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="44e07-442">Özniteliğin uygulandığı IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="44e07-442">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="44e07-443">Filtreler uygulayan `IFilterFactory` için yararlıdır, filtreler:</span><span class="sxs-lookup"><span data-stu-id="44e07-443">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="44e07-444">Parametreleri geçirme gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="44e07-444">Don't require passing parameters.</span></span>
* <span data-ttu-id="44e07-445">DI tarafından doldurulması gereken Oluşturucu bağımlılıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="44e07-445">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="44e07-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> uygulayan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="44e07-446"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="44e07-447">`IFilterFactory` kullanıma sunan <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi oluşturmak için bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği.</span><span class="sxs-lookup"><span data-stu-id="44e07-447">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="44e07-448">`CreateInstance` Belirtilen tür Hizmetleri kapsayıcısı (dı) yükler.</span><span class="sxs-lookup"><span data-stu-id="44e07-448">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="44e07-449">Aşağıdaki kodu uygulamak için üç koyulmaktadır `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="44e07-449">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="44e07-450">Önceki kodda yöntemiyle dekorasyon `[SampleActionFilter]` uygulamak için tercih edilen yaklaşım `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="44e07-450">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="44e07-451">Filtre işlem hattı, ara yazılımın kullanılması</span><span class="sxs-lookup"><span data-stu-id="44e07-451">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="44e07-452">İş kaynağı filtreler gibi [ara yazılım](xref:fundamentals/middleware/index) , bunlar daha sonra işlem hattı, gelen yürütme her şeyin çevreleyen.</span><span class="sxs-lookup"><span data-stu-id="44e07-452">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="44e07-453">Ancak, ASP.NET Core çalışma zamanı, ASP.NET Core bağlam ve yapılar için erişime sahip oldukları anlamına gelir parçası oldukları filtreleri ara yazılım farklı.</span><span class="sxs-lookup"><span data-stu-id="44e07-453">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="44e07-454">Ara yazılım bir filtre olarak kullanılacak bir türle oluşturma bir `Configure` filtre ardışık düzende eklemesine ara yazılım belirten yöntemi.</span><span class="sxs-lookup"><span data-stu-id="44e07-454">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="44e07-455">Aşağıdaki örnek, bir isteğin geçerli kültürünü yerleştirmeye yerelleştirme ara yazılım kullanır:</span><span class="sxs-lookup"><span data-stu-id="44e07-455">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="44e07-456">Kullanım <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> ara yazılım çalıştırmak için:</span><span class="sxs-lookup"><span data-stu-id="44e07-456">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="44e07-457">Çalışan bir ara yazılım aynı filtre ardışık düzen aşamasını kaynak olarak, model bağlama önce ve sonra kalan ardışık düzenini filtreleri.</span><span class="sxs-lookup"><span data-stu-id="44e07-457">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="44e07-458">Sonraki Eylemler</span><span class="sxs-lookup"><span data-stu-id="44e07-458">Next actions</span></span>

* <span data-ttu-id="44e07-459">Bkz: [yöntemleri Razor sayfaları için filtre](xref:razor-pages/filter)</span><span class="sxs-lookup"><span data-stu-id="44e07-459">See [Filter methods for Razor Pages](xref:razor-pages/filter)</span></span>
* <span data-ttu-id="44e07-460">Filtrelerle denemeler için [indirin, test etme ve GitHub örneği değiştirirseniz](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="44e07-460">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
