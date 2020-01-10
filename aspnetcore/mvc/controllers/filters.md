---
title: ASP.NET Core filtreler
author: Rick-Anderson
description: Filtrelerin nasıl çalıştığını ve ASP.NET Core nasıl kullanılacağını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 1/1/2020
uid: mvc/controllers/filters
ms.openlocfilehash: 759c150e7f35f3f6a52947edc5ef41448dc227fe
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828977"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="fc377-103">ASP.NET Core filtreler</span><span class="sxs-lookup"><span data-stu-id="fc377-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fc377-104">[Kirk Larkabağı](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/)ve [Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="fc377-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fc377-105">ASP.NET Core *Filtreler* , istek işleme ardışık düzeninde belirli aşamalardan önce veya sonra kod çalıştırılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="fc377-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="fc377-106">Yerleşik Filtreler şunları gibi görevleri işler:</span><span class="sxs-lookup"><span data-stu-id="fc377-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="fc377-107">Yetkilendirme (kullanıcının yetkili olmadığı kaynaklara erişimi önler).</span><span class="sxs-lookup"><span data-stu-id="fc377-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="fc377-108">Yanıt önbelleğe alma (istek ardışık düzenini önbelleğe alınmış bir yanıt döndürecek şekilde döndürür).</span><span class="sxs-lookup"><span data-stu-id="fc377-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="fc377-109">Çapraz kesme sorunlarını işlemek için özel filtreler oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="fc377-110">Çapraz kesme sorunlarına örnek olarak hata işleme, önbelleğe alma, yapılandırma, yetkilendirme ve günlüğe kaydetme dahildir.</span><span class="sxs-lookup"><span data-stu-id="fc377-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="fc377-111">Filtreler kodu çoğaltmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="fc377-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="fc377-112">Örneğin, bir hata işleme özel durum filtresi hata işlemeyi birleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="fc377-113">Bu belge, görünümler içeren Razor Pages, API denetleyicileri ve denetleyiciler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fc377-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="fc377-114">Örneği ([indirme](xref:index#how-to-download-a-sample)) [görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) .</span><span class="sxs-lookup"><span data-stu-id="fc377-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="fc377-115">Filtreler nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="fc377-115">How filters work</span></span>

<span data-ttu-id="fc377-116">Filtreler, bazen *Filtre işlem hattı*olarak da adlandırılan *ASP.NET Core eylemi çağırma işlem hattı*içinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="fc377-117">Filtre işlem hattı çalıştırılacak eylemi ASP.NET Core seçtikten sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![İstek diğer ara yazılım, yönlendirme ara yazılımı, eylem seçimi ve eylem çağırma Işlem hattı aracılığıyla işlenir.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="fc377-120">Filtre türleri</span><span class="sxs-lookup"><span data-stu-id="fc377-120">Filter types</span></span>

<span data-ttu-id="fc377-121">Her filtre türü, filtre ardışık düzeninde farklı bir aşamada yürütülür:</span><span class="sxs-lookup"><span data-stu-id="fc377-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="fc377-122">İlk olarak [Yetkilendirme filtreleri](#authorization-filters) çalışır ve kullanıcının istek için yetkilendirilip yetkilendirilmediğini tespit etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="fc377-123">Yetkilendirme filtreleri, istek yetkilendirilmezse işlem hattı kısa devre dışı olur.</span><span class="sxs-lookup"><span data-stu-id="fc377-123">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="fc377-124">[Kaynak filtreleri](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="fc377-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="fc377-125">Yetkilendirmeden sonra çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fc377-125">Run after authorization.</span></span>  
  * <span data-ttu-id="fc377-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*>, filtre ardışık düzeninin geri kalanından önce kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="fc377-127">Örneğin, `OnResourceExecuting` model bağlamalarından önce kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-127">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="fc377-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*>, ardışık düzenin geri kalanı tamamlandıktan sonra kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="fc377-129">[Eylem filtreleri](#action-filters):</span><span class="sxs-lookup"><span data-stu-id="fc377-129">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="fc377-130">Kodu bir eylem yöntemi çağrıldıktan hemen önce ve sonra çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fc377-130">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="fc377-131">Bir eyleme geçirilen bağımsız değişkenleri değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-131">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="fc377-132">Eylemden döndürülen sonucu değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-132">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="fc377-133">Razor Pages **desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="fc377-133">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="fc377-134">[Özel durum filtreleri](#exception-filters) , yanıt gövdesinin üzerine yazılmadan önce oluşan işlenmemiş özel durumlara genel ilkeler uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-134">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="fc377-135">[Sonuç filtreleri](#result-filters) , eylem sonuçlarının yürütülmesinden hemen önce ve sonra kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-135">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="fc377-136">Bunlar yalnızca eylem yöntemi başarıyla yürütüldüğünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-136">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="fc377-137">Bu değerler, görünüm veya biçimlendirici yürütmesinin yürütülmesi gereken mantık için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-137">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="fc377-138">Aşağıdaki diyagramda filtre türlerinin filtre ardışık düzeninde nasıl etkileşimde bulunduğu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fc377-138">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![İstek, Yetkilendirme filtreleri, kaynak filtreleri, model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütmesi aracılığıyla işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="fc377-141">Uygulama</span><span class="sxs-lookup"><span data-stu-id="fc377-141">Implementation</span></span>

<span data-ttu-id="fc377-142">Filtreler, farklı arabirim tanımları aracılığıyla hem zaman uyumlu hem de zaman uyumsuz uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="fc377-142">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="fc377-143">Zaman uyumlu filtreler, ardışık düzen aşamasından önce ve sonra kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-143">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="fc377-144">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> eylem yöntemi çağrılmadan önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-144">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="fc377-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*>, eylem yöntemi döndüğünde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="fc377-146">Zaman uyumsuz filtreler bir `On-Stage-ExecutionAsync` yöntemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-146">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="fc377-147">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span><span class="sxs-lookup"><span data-stu-id="fc377-147">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="fc377-148">Yukarıdaki kodda `SampleAsyncActionFilter`, eylem yöntemini yürüten bir <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fc377-148">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="fc377-149">Birden çok filtre aşaması</span><span class="sxs-lookup"><span data-stu-id="fc377-149">Multiple filter stages</span></span>

<span data-ttu-id="fc377-150">Birden çok filtre aşaması için arabirimler tek bir sınıfta uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-150">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="fc377-151">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sınıfı şunları uygular:</span><span class="sxs-lookup"><span data-stu-id="fc377-151">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="fc377-152">Zaman uyumlu: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="fc377-152">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="fc377-153">Zaman uyumsuz: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="fc377-153">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="fc377-154">Her ikisini de **değil** , bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="fc377-154">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="fc377-155">Çalışma zamanı öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-155">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="fc377-156">Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-156">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="fc377-157">Tek bir sınıfta hem zaman uyumsuz hem de zaman uyumlu arabirimler uygulanmışsa, yalnızca zaman uyumsuz yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-157">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="fc377-158"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>gibi soyut sınıflar kullanılırken, her bir filtre türü için yalnızca zaman uyumlu yöntemleri veya zaman uyumsuz yöntemi geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="fc377-158">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="fc377-159">Yerleşik filtre öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="fc377-159">Built-in filter attributes</span></span>

<span data-ttu-id="fc377-160">ASP.NET Core, alt sınıflanmış ve özelleştirilebilen yerleşik öznitelik tabanlı filtreler içerir.</span><span class="sxs-lookup"><span data-stu-id="fc377-160">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="fc377-161">Örneğin, aşağıdaki sonuç filtresi yanıta bir üst bilgi ekler:</span><span class="sxs-lookup"><span data-stu-id="fc377-161">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="fc377-162">Öznitelikler, önceki örnekte gösterildiği gibi, filtrelerin bağımsız değişkenleri kabul etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="fc377-162">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="fc377-163">`AddHeaderAttribute` bir denetleyiciye veya eylem yöntemine uygulayın ve HTTP üstbilgisinin adını ve değerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="fc377-163">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="fc377-164">Üst bilgileri incelemek için [tarayıcı geliştirici araçları](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) gibi bir araç kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc377-164">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="fc377-165">**Yanıt üst bilgileri**altında `author: Rick Anderson` görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fc377-165">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="fc377-166">Aşağıdaki kod şu şekilde bir `ActionFilterAttribute` uygular:</span><span class="sxs-lookup"><span data-stu-id="fc377-166">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="fc377-167">Yapılandırma sistemindeki başlığı ve adı okur.</span><span class="sxs-lookup"><span data-stu-id="fc377-167">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="fc377-168">Önceki örnekten farklı olarak, aşağıdaki kod koda filtre parametrelerinin eklenmesine gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="fc377-168">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="fc377-169">Başlık ve adı yanıt üstbilgisine ekler.</span><span class="sxs-lookup"><span data-stu-id="fc377-169">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="fc377-170">Yapılandırma seçenekleri, [Seçenekler deseninin](xref:fundamentals/configuration/options)kullanıldığı [yapılandırma sisteminden](xref:fundamentals/configuration/index) sağlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-170">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="fc377-171">Örneğin, *appSettings. JSON* dosyasından:</span><span class="sxs-lookup"><span data-stu-id="fc377-171">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="fc377-172">`StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fc377-172">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="fc377-173">`PositionOptions` sınıfı, `"Position"` yapılandırma alanı ile hizmet kapsayıcısına eklenir.</span><span class="sxs-lookup"><span data-stu-id="fc377-173">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="fc377-174">`MyActionFilterAttribute`, hizmet kapsayıcısına eklenir.</span><span class="sxs-lookup"><span data-stu-id="fc377-174">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="fc377-175">Aşağıdaki kod `PositionOptions` sınıfını gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-175">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="fc377-176">Aşağıdaki kod, `Index2` yöntemine `MyActionFilterAttribute` uygular:</span><span class="sxs-lookup"><span data-stu-id="fc377-176">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="fc377-177">**Yanıt üst bilgileri**, `author: Rick Anderson`ve `Editor: Joe Smith` altında `Sample/Index2` uç noktası çağrıldığında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fc377-177">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="fc377-178">Aşağıdaki kod, Razor sayfasına `MyActionFilterAttribute` ve `AddHeaderAttribute` uygular:</span><span class="sxs-lookup"><span data-stu-id="fc377-178">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="fc377-179">, Razor sayfası işleyici yöntemlerine filtre uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="fc377-179">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="fc377-180">Bu kişiler Razor sayfa modeline veya küresel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-180">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="fc377-181">Filtre arabirimlerinden birkaçı, özel uygulamalar için temel sınıflar olarak kullanılabilecek karşılık gelen özniteliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fc377-181">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="fc377-182">Filtre öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-182">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="fc377-183">Kapsamları ve yürütme sırasını filtrele</span><span class="sxs-lookup"><span data-stu-id="fc377-183">Filter scopes and order of execution</span></span>

<span data-ttu-id="fc377-184">Üç *kapsamından*birindeki işlem hattına bir filtre eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-184">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="fc377-185">Bir denetleyici eyleminde bir özniteliği kullanma.</span><span class="sxs-lookup"><span data-stu-id="fc377-185">Using an attribute on a controller action.</span></span> <span data-ttu-id="fc377-186">Filtre öznitelikleri Razor Pages işleyici yöntemlerine uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="fc377-186">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="fc377-187">Bir denetleyici veya Razor sayfasında bir özniteliği kullanma.</span><span class="sxs-lookup"><span data-stu-id="fc377-187">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="fc377-188">Genel olarak tüm denetleyiciler, Eylemler ve Razor Pages aşağıdaki kodda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="fc377-188">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="fc377-189">Varsayılan yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="fc377-189">Default order of execution</span></span>

<span data-ttu-id="fc377-190">İşlem hattının belirli bir aşamasına ilişkin birden çok filtre olduğunda, kapsam varsayılan filtre yürütme sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="fc377-190">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="fc377-191">Genel filtreler kapsayan sınıf filtreleri, bu da kapsayan Yöntem filtreleri.</span><span class="sxs-lookup"><span data-stu-id="fc377-191">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="fc377-192">Filtre iç içe geçme sonucu *olarak, filtrenin kodu,* *önceki* kodun ters sırasına göre çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-192">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="fc377-193">Filtre sırası:</span><span class="sxs-lookup"><span data-stu-id="fc377-193">The filter sequence:</span></span>

* <span data-ttu-id="fc377-194">Genel filtrelerin *önceki* kodu.</span><span class="sxs-lookup"><span data-stu-id="fc377-194">The *before* code of global filters.</span></span>
  * <span data-ttu-id="fc377-195">Denetleyicinin ve Razor sayfası filtrelerinin *öncesindeki* kodu.</span><span class="sxs-lookup"><span data-stu-id="fc377-195">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="fc377-196">Eylem yöntemi filtrelerinden *önceki* kod.</span><span class="sxs-lookup"><span data-stu-id="fc377-196">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="fc377-197">Eylem yöntemi filtrelerinden *sonraki* kod.</span><span class="sxs-lookup"><span data-stu-id="fc377-197">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="fc377-198">Denetleyicinin ve Razor sayfası filtrelerinin *sonraki* kodu.</span><span class="sxs-lookup"><span data-stu-id="fc377-198">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="fc377-199">Genel filtrelerin *sonraki* kodu.</span><span class="sxs-lookup"><span data-stu-id="fc377-199">The *after* code of global filters.</span></span>
  
<span data-ttu-id="fc377-200">Zaman uyumlu eylem filtreleri için filtre yöntemlerinin çağrıldığı sırayı gösteren aşağıdaki örnek.</span><span class="sxs-lookup"><span data-stu-id="fc377-200">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="fc377-201">Sequence</span><span class="sxs-lookup"><span data-stu-id="fc377-201">Sequence</span></span> | <span data-ttu-id="fc377-202">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="fc377-202">Filter scope</span></span> | <span data-ttu-id="fc377-203">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="fc377-203">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="fc377-204">1\.</span><span class="sxs-lookup"><span data-stu-id="fc377-204">1</span></span> | <span data-ttu-id="fc377-205">Global</span><span class="sxs-lookup"><span data-stu-id="fc377-205">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="fc377-206">2</span><span class="sxs-lookup"><span data-stu-id="fc377-206">2</span></span> | <span data-ttu-id="fc377-207">Denetleyici veya Razor sayfası</span><span class="sxs-lookup"><span data-stu-id="fc377-207">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="fc377-208">3</span><span class="sxs-lookup"><span data-stu-id="fc377-208">3</span></span> | <span data-ttu-id="fc377-209">Yöntem</span><span class="sxs-lookup"><span data-stu-id="fc377-209">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="fc377-210">4</span><span class="sxs-lookup"><span data-stu-id="fc377-210">4</span></span> | <span data-ttu-id="fc377-211">Yöntem</span><span class="sxs-lookup"><span data-stu-id="fc377-211">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="fc377-212">5</span><span class="sxs-lookup"><span data-stu-id="fc377-212">5</span></span> | <span data-ttu-id="fc377-213">Denetleyici veya Razor sayfası</span><span class="sxs-lookup"><span data-stu-id="fc377-213">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="fc377-214">6</span><span class="sxs-lookup"><span data-stu-id="fc377-214">6</span></span> | <span data-ttu-id="fc377-215">Global</span><span class="sxs-lookup"><span data-stu-id="fc377-215">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="fc377-216">Denetleyici düzeyi filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-216">Controller level filters</span></span>

<span data-ttu-id="fc377-217"><xref:Microsoft.AspNetCore.Mvc.Controller> temel sınıfından devralan her denetleyici [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)ve [controller. onactionyürütülmüş](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` yöntemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="fc377-217">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="fc377-218">Bu Yöntemler:</span><span class="sxs-lookup"><span data-stu-id="fc377-218">These methods:</span></span>

* <span data-ttu-id="fc377-219">Belirli bir eylem için çalışan filtreleri sarın.</span><span class="sxs-lookup"><span data-stu-id="fc377-219">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="fc377-220">`OnActionExecuting`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-220">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="fc377-221">`OnActionExecuted` tüm eylem filtrelerinden sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-221">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="fc377-222">`OnActionExecutionAsync`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-222">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="fc377-223">`next` sonra filtre içindeki kod eylem yönteminden sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-223">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="fc377-224">Örneğin, indirme örneğinde `MySampleActionFilter`, başlangıçta genel olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-224">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="fc377-225">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="fc377-225">The `TestController`:</span></span>

* <span data-ttu-id="fc377-226">`FilterTest2` eyleme `SampleActionFilterAttribute` (`[SampleActionFilter]`) uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-226">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="fc377-227">`OnActionExecuting` ve `OnActionExecuted`geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="fc377-227">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="fc377-228">`https://localhost:5001/Test2/FilterTest2` gitme aşağıdaki kodu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="fc377-228">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="fc377-229">Denetleyici düzeyi filtreleri [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) özelliğini `int.MinValue`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-229">Controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="fc377-230">Denetleyici düzeyi filtreleri metotlara uygulandıktan sonra çalıştırılacak şekilde ayarlanamaz.</span><span class="sxs-lookup"><span data-stu-id="fc377-230">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="fc377-231">Sıra, sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="fc377-231">Order is explained in the next section.</span></span>

<span data-ttu-id="fc377-232">Razor Pages için bkz. [filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="fc377-232">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="fc377-233">Varsayılan sırayı geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="fc377-233">Overriding the default order</span></span>

<span data-ttu-id="fc377-234">Varsayılan yürütme sırası <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>uygulayarak geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-234">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="fc377-235">`IOrderedFilter`, yürütme sırasını belirlemede kapsama göre öncelik alan <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="fc377-235">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="fc377-236">Daha düşük bir `Order` değerine sahip bir filtre:</span><span class="sxs-lookup"><span data-stu-id="fc377-236">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="fc377-237">Daha yüksek bir `Order`bir filtrenin *önüne kodundan önce* kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-237">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="fc377-238">Daha yüksek `Order` değerine sahip bir filtrenin *sonrasında koddan sonra* çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-238">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="fc377-239">`Order` özelliği bir Oluşturucu parametresiyle ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="fc377-239">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="fc377-240">Aşağıdaki denetleyicide iki eylem filtresini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="fc377-240">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="fc377-241">`StartUp.ConfigureServices`genel bir filtre eklendi:</span><span class="sxs-lookup"><span data-stu-id="fc377-241">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="fc377-242">3 filtre aşağıdaki sırayla çalışır:</span><span class="sxs-lookup"><span data-stu-id="fc377-242">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="fc377-243">`Order` özelliği filtrelerin çalıştırıldığı sırayı belirlerken kapsamı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="fc377-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="fc377-244">Filtreler sırasıyla sıraya göre sıralanır, ardından kapsam bölmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="fc377-245">Yerleşik filtrelerin tümü `IOrderedFilter` uygular ve varsayılan `Order` değerini 0 olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="fc377-246">Daha önce belirtildiği gibi, denetleyici düzeyi filtreleri [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) özelliğini yerleşik filtreler için `int.MinValue` olarak ayarlar, `Order` sıfır olmayan bir değere ayarlanmadığı sürece kapsam sıralamayı belirler.</span><span class="sxs-lookup"><span data-stu-id="fc377-246">As mentioned previously, controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="fc377-247">Yukarıdaki kodda, `MySampleActionFilter` denetleyici kapsamı bulunan `MyAction2FilterAttribute`önce çalışması için genel kapsama sahip olur.</span><span class="sxs-lookup"><span data-stu-id="fc377-247">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="fc377-248">`MyAction2FilterAttribute` önce çalıştırmak için sırayı `int.MinValue`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="fc377-248">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="fc377-249">Genel filtre `MySampleActionFilter` önce çalıştırmak için, `Order` `int.MinValue`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="fc377-249">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="fc377-250">İptal ve kısa devre dışı</span><span class="sxs-lookup"><span data-stu-id="fc377-250">Cancellation and short-circuiting</span></span>

<span data-ttu-id="fc377-251">Filtre işlem hattı, filtre yöntemine sunulan <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parametresindeki <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> özelliği ayarlanarak kısa devre dışı olabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-251">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="fc377-252">Örneğin, aşağıdaki kaynak filtresi, ardışık düzenin geri kalanının yürütülmesini engeller:</span><span class="sxs-lookup"><span data-stu-id="fc377-252">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="fc377-253">Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` hem de `AddHeader` filtresi `SomeResource` Action metodunu hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="fc377-253">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="fc377-254">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="fc377-254">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="fc377-255">Önce bir kaynak filtresi olduğundan ve `AddHeader` bir eylem filtresi olduğundan, önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-255">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="fc377-256">Kısa süreli işlem hattının geri kalanı.</span><span class="sxs-lookup"><span data-stu-id="fc377-256">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="fc377-257">Bu nedenle `AddHeader` filtresi `SomeResource` eylemi için hiçbir şekilde çalışmamayacaktır.</span><span class="sxs-lookup"><span data-stu-id="fc377-257">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="fc377-258">Her iki filtre de eylem yöntemi düzeyinde uygulanırsa, `ShortCircuitingResourceFilter` ilk kez çalıştırıldıysa bu davranış aynı olur.</span><span class="sxs-lookup"><span data-stu-id="fc377-258">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="fc377-259">`ShortCircuitingResourceFilter`, filtre türü veya `Order` özelliğinin açık kullanımı nedeniyle önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-259">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="fc377-260">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="fc377-260">Dependency injection</span></span>

<span data-ttu-id="fc377-261">Filtreler türe veya örneğe göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-261">Filters can be added by type or by instance.</span></span> <span data-ttu-id="fc377-262">Bir örnek eklenirse, bu örnek her istek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-262">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="fc377-263">Bir tür eklenirse, türü etkinleştirilmiş olur.</span><span class="sxs-lookup"><span data-stu-id="fc377-263">If a type is added, it's type-activated.</span></span> <span data-ttu-id="fc377-264">Tür etkinleştirilmiş bir filtre şu anlama gelir:</span><span class="sxs-lookup"><span data-stu-id="fc377-264">A type-activated filter means:</span></span>

* <span data-ttu-id="fc377-265">Her istek için bir örnek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fc377-265">An instance is created for each request.</span></span>
* <span data-ttu-id="fc377-266">Herhangi bir Oluşturucu bağımlılığı [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından doldurulur.</span><span class="sxs-lookup"><span data-stu-id="fc377-266">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="fc377-267">Öznitelik olarak uygulanan ve doğrudan denetleyici sınıflarına veya eylem yöntemlerine eklenen filtreler [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından sağlanmış Oluşturucu bağımlılıklarına sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="fc377-267">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="fc377-268">Oluşturucu bağımlılıkları şu nedenle şu nedenle sağlanamaz:</span><span class="sxs-lookup"><span data-stu-id="fc377-268">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="fc377-269">Özniteliklerin, uygulandıkları yerlerde, Oluşturucu parametreleri sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-269">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="fc377-270">Bu, özniteliklerin nasıl çalıştığı konusunda bir kısıtlamadır.</span><span class="sxs-lookup"><span data-stu-id="fc377-270">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="fc377-271">Aşağıdaki filtreler, dı tarafından belirtilen Oluşturucu bağımlılıklarını destekler:</span><span class="sxs-lookup"><span data-stu-id="fc377-271">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="fc377-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> özniteliğe uygulandı.</span><span class="sxs-lookup"><span data-stu-id="fc377-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="fc377-273">Önceki filtreler bir denetleyiciye veya eylem yöntemine uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-273">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="fc377-274">Günlükçüler, DI 'den alınabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-274">Loggers are available from DI.</span></span> <span data-ttu-id="fc377-275">Ancak, yalnızca günlüğe kaydetme amacıyla filtre oluşturmaktan ve kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="fc377-275">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="fc377-276">[Yerleşik çerçeve günlüğü](xref:fundamentals/logging/index) genellikle günlüğe kaydetme için gerekenleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-276">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="fc377-277">Filtrelere günlük eklendi:</span><span class="sxs-lookup"><span data-stu-id="fc377-277">Logging added to filters:</span></span>

* <span data-ttu-id="fc377-278">, İş etki alanı kaygılarını veya filtreye özgü davranışları odaklamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-278">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="fc377-279">Eylemleri veya diğer çerçeve olaylarını günlüğe **içermemelidir** .</span><span class="sxs-lookup"><span data-stu-id="fc377-279">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="fc377-280">Yerleşik filtreler günlük eylemleri ve çerçeve olayları.</span><span class="sxs-lookup"><span data-stu-id="fc377-280">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="fc377-281">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="fc377-281">ServiceFilterAttribute</span></span>

<span data-ttu-id="fc377-282">Hizmet filtresi uygulama türleri `ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-282">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="fc377-283"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, bir filtrenin bir örneğini dı öğesinden alır.</span><span class="sxs-lookup"><span data-stu-id="fc377-283">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="fc377-284">Aşağıdaki kod `AddHeaderResultServiceFilter`gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-284">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="fc377-285">Aşağıdaki kodda, `AddHeaderResultServiceFilter` dı kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="fc377-285">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="fc377-286">Aşağıdaki kodda, `ServiceFilter` özniteliği dı öğesinden `AddHeaderResultServiceFilter` filtrenin bir örneğini alır:</span><span class="sxs-lookup"><span data-stu-id="fc377-286">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="fc377-287">`ServiceFilterAttribute`kullanırken, [Servicefilterattribute. ısyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="fc377-287">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="fc377-288">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden *kullanılabilir olabileceğini gösteren* bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-288">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="fc377-289">ASP.NET Core çalışma zamanı garanti etmez:</span><span class="sxs-lookup"><span data-stu-id="fc377-289">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="fc377-290">Filtrenin tek bir örneğinin oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc377-290">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="fc377-291">Filtre, sonraki bir noktada dı kapsayıcısından yeniden istenmeyecek.</span><span class="sxs-lookup"><span data-stu-id="fc377-291">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="fc377-292">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-292">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="fc377-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="fc377-294">`IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="fc377-294">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="fc377-295">`CreateInstance`, belirtilen türü DI 'dan yükler.</span><span class="sxs-lookup"><span data-stu-id="fc377-295">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="fc377-296">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="fc377-296">TypeFilterAttribute</span></span>

<span data-ttu-id="fc377-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>benzerdir, ancak türü doğrudan dı kapsayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="fc377-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="fc377-298"><xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>kullanarak türü başlatır.</span><span class="sxs-lookup"><span data-stu-id="fc377-298">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="fc377-299">`TypeFilterAttribute` türler doğrudan dı kapsayıcısından çözümlenmediğinden:</span><span class="sxs-lookup"><span data-stu-id="fc377-299">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="fc377-300">`TypeFilterAttribute` kullanılarak başvurulan türlerin dı kapsayıcısına kaydedilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fc377-300">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="fc377-301">Bunların bağımlılıkları, dı kapsayıcısı tarafından yerine getirilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-301">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="fc377-302">`TypeFilterAttribute`, isteğe bağlı olarak tür için Oluşturucu bağımsız değişkenlerini kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-302">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="fc377-303">`TypeFilterAttribute`kullanırken [Typefilterattribute. ıyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="fc377-303">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="fc377-304">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden kullanılabilir *olabileceği* ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-304">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="fc377-305">ASP.NET Core çalışma zamanı, filtrenin tek bir örneğinin oluşturulacağı garantisi vermez.</span><span class="sxs-lookup"><span data-stu-id="fc377-305">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="fc377-306">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-306">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="fc377-307">Aşağıdaki örnek `TypeFilterAttribute`kullanarak bir türe bağımsız değişkenlerin nasıl geçirileceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-307">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="fc377-308">Yetkilendirme filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-308">Authorization filters</span></span>

<span data-ttu-id="fc377-309">Yetkilendirme filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-309">Authorization filters:</span></span>

* <span data-ttu-id="fc377-310">, Filtre ardışık düzeninde ilk filtrelerin çalıştırılmasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-310">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="fc377-311">Eylem yöntemlerine erişimi denetleyin.</span><span class="sxs-lookup"><span data-stu-id="fc377-311">Control access to action methods.</span></span>
* <span data-ttu-id="fc377-312">Bir Before yöntemi, ancak After yönteminden hiçbiri.</span><span class="sxs-lookup"><span data-stu-id="fc377-312">Have a before method, but no after method.</span></span>

<span data-ttu-id="fc377-313">Özel Yetkilendirme filtreleri özel bir yetkilendirme çerçevesi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fc377-313">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="fc377-314">Özel bir filtre yazmak için yetkilendirme ilkelerini yapılandırmayı veya özel bir yetkilendirme ilkesi yazmayı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="fc377-314">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="fc377-315">Yerleşik yetkilendirme filtresi:</span><span class="sxs-lookup"><span data-stu-id="fc377-315">The built-in authorization filter:</span></span>

* <span data-ttu-id="fc377-316">Yetkilendirme sistemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-316">Calls the authorization system.</span></span>
* <span data-ttu-id="fc377-317">İstekleri yetkilendirmez.</span><span class="sxs-lookup"><span data-stu-id="fc377-317">Does not authorize requests.</span></span>

<span data-ttu-id="fc377-318">Yetkilendirme filtreleri içinde özel **durumlar atamayın:**</span><span class="sxs-lookup"><span data-stu-id="fc377-318">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="fc377-319">Özel durum işlenmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="fc377-319">The exception will not be handled.</span></span>
* <span data-ttu-id="fc377-320">Özel durum filtreleri özel durumu işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="fc377-320">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="fc377-321">Bir yetkilendirme filtresinde özel durum oluştuğunda bir sınama vermeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="fc377-321">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="fc377-322">[Yetkilendirme](xref:security/authorization/introduction)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="fc377-322">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="fc377-323">Kaynak filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-323">Resource filters</span></span>

<span data-ttu-id="fc377-324">Kaynak filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-324">Resource filters:</span></span>

* <span data-ttu-id="fc377-325"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-325">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="fc377-326">Yürütme, filtre işlem hattının çoğunu sarmalar.</span><span class="sxs-lookup"><span data-stu-id="fc377-326">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="fc377-327">Kaynak filtrelerinden önce yalnızca [Yetkilendirme filtreleri](#authorization-filters) çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-327">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="fc377-328">Kaynak filtreleri, işlem hattının büyük bir yanındaki kısa devre için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-328">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="fc377-329">Örneğin, bir önbelleğe alma filtresi, bir önbellek isabetinden ardışık düzen geri kalanından kaçınabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-329">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="fc377-330">Kaynak filtresi örnekleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-330">Resource filter examples:</span></span>

* <span data-ttu-id="fc377-331">Daha önce gösterilen [kısa devre dışı kaynak filtresi](#short-circuiting-resource-filter) .</span><span class="sxs-lookup"><span data-stu-id="fc377-331">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="fc377-332">[Disableformvaluemodelbindingattribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="fc377-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="fc377-333">Model bağlamanın form verilerine erişimini engeller.</span><span class="sxs-lookup"><span data-stu-id="fc377-333">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="fc377-334">Form verilerinin belleğe okunmasını engellemek için büyük dosya yüklemeleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-334">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="fc377-335">Eylem filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-335">Action filters</span></span>

<span data-ttu-id="fc377-336">Eylem filtreleri Razor Pages **için uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="fc377-336">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="fc377-337">Razor Pages <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> destekler.</span><span class="sxs-lookup"><span data-stu-id="fc377-337">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="fc377-338">Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="fc377-338">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="fc377-339">Eylem filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-339">Action filters:</span></span>

* <span data-ttu-id="fc377-340"><xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-340">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="fc377-341">Yürütmesinin, eylem yöntemlerinin yürütülmesi çevreler.</span><span class="sxs-lookup"><span data-stu-id="fc377-341">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="fc377-342">Aşağıdaki kod bir örnek eylem filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-342">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="fc377-343"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="fc377-343">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="fc377-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-girişlerin bir eylem yöntemine okunmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="fc377-345"><xref:Microsoft.AspNetCore.Mvc.Controller>-denetleyici örneğinin işlenmesine izin vermez.</span><span class="sxs-lookup"><span data-stu-id="fc377-345"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="fc377-346"><xref:System.Web.Mvc.ActionExecutingContext.Result>-eylem yönteminin ve sonraki eylem filtrelerinin `Result` kısa devre dışı yürütmesi.</span><span class="sxs-lookup"><span data-stu-id="fc377-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="fc377-347">Eylem yönteminde özel durum oluşturma:</span><span class="sxs-lookup"><span data-stu-id="fc377-347">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="fc377-348">Sonraki filtrelerin çalıştırılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="fc377-348">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="fc377-349">`Result`ayarının aksine, başarılı bir sonuç yerine başarısızlık olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-349">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="fc377-350"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> `Controller` ve `Result` ek olarak aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="fc377-350">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="fc377-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-eylem yürütmesi başka bir filtre tarafından kabul edilse true.</span><span class="sxs-lookup"><span data-stu-id="fc377-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="fc377-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-eylem veya daha önce çalıştırılan eylem filtresi bir özel durum oluşturdu, null değil.</span><span class="sxs-lookup"><span data-stu-id="fc377-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="fc377-353">Bu özellik null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="fc377-353">Setting this property to null:</span></span>

  * <span data-ttu-id="fc377-354">Özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="fc377-354">Effectively handles the exception.</span></span>
  * <span data-ttu-id="fc377-355">`Result`, eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fc377-355">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="fc377-356">Bir `IAsyncActionFilter`için <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>çağrısı:</span><span class="sxs-lookup"><span data-stu-id="fc377-356">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="fc377-357">Sonraki eylem filtrelerini ve eylem yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="fc377-357">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="fc377-358">`ActionExecutedContext` döndürür.</span><span class="sxs-lookup"><span data-stu-id="fc377-358">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="fc377-359">Kısa devre dışı, bir sonuç örneğine <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> atayın ve `next` çağırmayın (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="fc377-359">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="fc377-360">Framework, alt sınıflı olabilecek bir soyut <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-360">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="fc377-361">`OnActionExecuting` eylem filtresi şu şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-361">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="fc377-362">Model durumunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-362">Validate model state.</span></span>
* <span data-ttu-id="fc377-363">Durum geçersizse bir hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="fc377-363">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="fc377-364">`OnActionExecuted` yöntemi eylem yönteminden sonra çalışır:</span><span class="sxs-lookup"><span data-stu-id="fc377-364">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="fc377-365">Ve <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> özelliği aracılığıyla eylemin sonuçlarını görebilir ve değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-365">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="fc377-366">eylem yürütmesi başka bir filtre tarafından kabul edilse, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> true olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="fc377-367">eylem veya sonraki eylem filtresi bir özel durum harekete geçirdi, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="fc377-368">`Exception` null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="fc377-368">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="fc377-369">Bir özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="fc377-369">Effectively handles an exception.</span></span>
  * <span data-ttu-id="fc377-370">`ActionExecutedContext.Result`, normal olarak eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fc377-370">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="fc377-371">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-371">Exception filters</span></span>

<span data-ttu-id="fc377-372">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-372">Exception filters:</span></span>

* <span data-ttu-id="fc377-373"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>uygulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-373">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="fc377-374">, Yaygın hata işleme ilkelerini uygulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-374">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="fc377-375">Aşağıdaki örnek özel durum filtresi, uygulama geliştirmede olduğunda oluşan özel durumlar hakkındaki ayrıntıları görüntülemek için özel bir hata görünümü kullanır:</span><span class="sxs-lookup"><span data-stu-id="fc377-375">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="fc377-376">Aşağıdaki kod özel durum filtresini sınar:</span><span class="sxs-lookup"><span data-stu-id="fc377-376">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="fc377-377">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-377">Exception filters:</span></span>

* <span data-ttu-id="fc377-378">Etkinlikden önceki ve sonraki olaylar yok.</span><span class="sxs-lookup"><span data-stu-id="fc377-378">Don't have before and after events.</span></span>
* <span data-ttu-id="fc377-379"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>uygulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-379">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="fc377-380">Razor sayfası veya denetleyici oluşturma, [model bağlama](xref:mvc/models/model-binding), eylem filtreleri veya eylem yöntemlerinde oluşan işlenmemiş özel durumları işleyin.</span><span class="sxs-lookup"><span data-stu-id="fc377-380">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="fc377-381">Kaynak filtrelerinde, sonuç filtrelerinde veya MVC sonuç yürütülürken oluşan özel **durumları yakalamayın** .</span><span class="sxs-lookup"><span data-stu-id="fc377-381">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="fc377-382">Bir özel durumu işlemek için <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> özelliğini `true` veya bir yanıt yazın.</span><span class="sxs-lookup"><span data-stu-id="fc377-382">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="fc377-383">Bu, özel durumun yayılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="fc377-383">This stops propagation of the exception.</span></span> <span data-ttu-id="fc377-384">Özel durum filtresi bir özel durumu "başarılı" olarak açamaz.</span><span class="sxs-lookup"><span data-stu-id="fc377-384">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="fc377-385">Yalnızca bir eylem filtresi bunu yapabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-385">Only an action filter can do that.</span></span>

<span data-ttu-id="fc377-386">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-386">Exception filters:</span></span>

* <span data-ttu-id="fc377-387">Eylemler içinde oluşan özel durumları yakalamaya uygundur.</span><span class="sxs-lookup"><span data-stu-id="fc377-387">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="fc377-388">, Hata işleme ara yazılımı olarak esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="fc377-388">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="fc377-389">Özel durum işleme için ara yazılımı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="fc377-389">Prefer middleware for exception handling.</span></span> <span data-ttu-id="fc377-390">Yalnızca hata işlemenin hangi eylem yöntemine göre *farklılık* gösteren özel durum filtrelerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc377-390">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="fc377-391">Örneğin, bir uygulama hem API uç noktaları hem de görünümler/HTML için eylem yöntemlerine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-391">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="fc377-392">API uç noktaları, hata bilgilerini JSON olarak döndürebilir, ancak görünüm tabanlı eylemler bir hata sayfasını HTML olarak döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-392">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="fc377-393">Sonuç filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-393">Result filters</span></span>

<span data-ttu-id="fc377-394">Sonuç filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-394">Result filters:</span></span>

* <span data-ttu-id="fc377-395">Arabirim uygulama:</span><span class="sxs-lookup"><span data-stu-id="fc377-395">Implement an interface:</span></span>
  * <span data-ttu-id="fc377-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="fc377-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="fc377-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="fc377-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="fc377-398">Yürütmesi, eylem sonuçlarının yürütülmesini çevreler.</span><span class="sxs-lookup"><span data-stu-id="fc377-398">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="fc377-399">IResultFilter ve ıasyncresultfilter</span><span class="sxs-lookup"><span data-stu-id="fc377-399">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="fc377-400">Aşağıdaki kod, bir HTTP üst bilgisi ekleyen bir sonuç filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-400">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="fc377-401">Yürütülen sonuç türü eyleme göre değişir.</span><span class="sxs-lookup"><span data-stu-id="fc377-401">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="fc377-402">Bir görünüm döndüren bir eylem, yürütülen <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir parçası olarak tüm Razor işlemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="fc377-402">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="fc377-403">Bir API yöntemi, sonucun yürütülmesinin bir parçası olarak bazı serileştirme işlemleri gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-403">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="fc377-404">[Eylem sonuçları](xref:mvc/controllers/actions)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="fc377-404">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="fc377-405">Sonuç filtreleri yalnızca bir eylem veya eylem filtresi bir eylem sonucu üretirse yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fc377-405">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="fc377-406">Şu durumlarda sonuç filtreleri yürütülmez:</span><span class="sxs-lookup"><span data-stu-id="fc377-406">Result filters are not executed when:</span></span>

* <span data-ttu-id="fc377-407">Bir yetkilendirme filtresi veya kaynak filtresi, işlem hattı için kısa süreli olarak devre dışı.</span><span class="sxs-lookup"><span data-stu-id="fc377-407">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="fc377-408">Bir özel durum filtresi, bir eylem sonucu üreterek özel durumu işler.</span><span class="sxs-lookup"><span data-stu-id="fc377-408">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="fc377-409"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> yöntemi, eylem sonucunun ve sonraki sonuç filtrelerinin `true`<xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> ayarlanarak kısa devre yürütülmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-409">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="fc377-410">Boş bir yanıt oluşturmamaya kaçınmak için kısa devre dışı bırakıldığında yanıt nesnesine yazın.</span><span class="sxs-lookup"><span data-stu-id="fc377-410">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="fc377-411">`IResultFilter.OnResultExecuting`bir özel durum üretiliyor:</span><span class="sxs-lookup"><span data-stu-id="fc377-411">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="fc377-412">Eylem sonucunun ve sonraki filtrelerin yürütülmesini önler.</span><span class="sxs-lookup"><span data-stu-id="fc377-412">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="fc377-413">Başarılı bir sonuç yerine başarısızlık olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-413">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="fc377-414"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> yöntemi çalıştırıldığında, yanıt muhtemelen istemciye zaten gönderilmiştir.</span><span class="sxs-lookup"><span data-stu-id="fc377-414">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="fc377-415">Yanıt istemciye zaten gönderildiyse, bu değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="fc377-415">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="fc377-416">`ResultExecutedContext.Canceled`, eylem sonucu yürütmesi başka bir filtre tarafından kabul edilen kısa devre ise `true` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-416">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="fc377-417">eylem sonucu veya sonraki sonuç filtresi bir özel durum harekete geçirdi, `ResultExecutedContext.Exception` null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-417">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="fc377-418">`Exception` null olarak ayarlanması, bir özel durumu etkili bir şekilde işler ve özel durumun daha sonra işlem hattının daha sonra oluşturulmasını önler.</span><span class="sxs-lookup"><span data-stu-id="fc377-418">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="fc377-419">Bir sonuç filtresinde özel durum işlenirken yanıta veri yazmanın güvenilir bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="fc377-419">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="fc377-420">Bir eylem sonucu bir özel durum oluşturduğunda üstbilgiler istemciye temizleniyorsa, hata kodu göndermek için güvenilir bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="fc377-420">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="fc377-421">Bir <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>için, <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> bir `await next` çağrısı, sonraki sonuç filtrelerini ve eylem sonucunu yürütür.</span><span class="sxs-lookup"><span data-stu-id="fc377-421">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="fc377-422">Kısa devre dışı, [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) `true` ve `ResultExecutionDelegate`çağırmayın:</span><span class="sxs-lookup"><span data-stu-id="fc377-422">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="fc377-423">Framework, alt sınıflı olabilecek bir soyut `ResultFilterAttribute` sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-423">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="fc377-424">Daha önce gösterilen [Addheaderattribute](#add-header-attribute) sınıfı bir sonuç Filtresi özniteliği örneğidir.</span><span class="sxs-lookup"><span data-stu-id="fc377-424">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="fc377-425">Ialwaysrunresultfilter ve ıasyncalwaysrunresultfilter</span><span class="sxs-lookup"><span data-stu-id="fc377-425">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="fc377-426"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> arabirimleri, tüm eylem sonuçları için çalışan bir <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> uygulamasını bildirir.</span><span class="sxs-lookup"><span data-stu-id="fc377-426">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="fc377-427">Bu, tarafından oluşturulan eylem sonuçlarını içerir:</span><span class="sxs-lookup"><span data-stu-id="fc377-427">This includes action results produced by:</span></span>

* <span data-ttu-id="fc377-428">Kısa devre olan Yetkilendirme filtreleri ve kaynak filtreleri.</span><span class="sxs-lookup"><span data-stu-id="fc377-428">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="fc377-429">Özel durum filtreleri.</span><span class="sxs-lookup"><span data-stu-id="fc377-429">Exception filters.</span></span>

<span data-ttu-id="fc377-430">Örneğin, aşağıdaki filtre her zaman çalışır ve içerik anlaşması başarısız olduğunda *422 olmayan bir varlık* durum kodu ile bir eylem sonucu (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) ayarlar:</span><span class="sxs-lookup"><span data-stu-id="fc377-430">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="fc377-431">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="fc377-431">IFilterFactory</span></span>

<span data-ttu-id="fc377-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="fc377-433">Bu nedenle, bir `IFilterFactory` örneği, filtre ardışık düzeninde herhangi bir yerde `IFilterMetadata` bir örnek olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-433">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="fc377-434">Çalışma zamanı filtreyi çağırmayı hazırlarken, bir `IFilterFactory`dönüştürmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="fc377-434">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="fc377-435">Bu atama başarılı olursa, çağrılan `IFilterMetadata` örneğini oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-435">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="fc377-436">Bu, tam filtre işlem hattının uygulama başladığında açıkça ayarlanması gerektiğinden esnek bir tasarım sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-436">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="fc377-437">`IFilterFactory`, filtre oluşturmaya yönelik başka bir yaklaşım olarak özel öznitelik uygulamaları kullanılarak uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-437">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="fc377-438">Filtre aşağıdaki kodda uygulanır:</span><span class="sxs-lookup"><span data-stu-id="fc377-438">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="fc377-439">Önceki kodu [indirme örneğini](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample)çalıştırarak test edin:</span><span class="sxs-lookup"><span data-stu-id="fc377-439">Test the preceding code by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="fc377-440">F12 geliştirici araçlarını çağırın.</span><span class="sxs-lookup"><span data-stu-id="fc377-440">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="fc377-441">[https://test-cors.org](`https://localhost:5001/Sample/HeaderWithFactory`) sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="fc377-441">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="fc377-442">F12 geliştirici araçları, örnek kod tarafından eklenen aşağıdaki yanıt üstbilgilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="fc377-442">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="fc377-443">**Yazar:** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="fc377-443">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="fc377-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="fc377-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="fc377-445">**iç:** `My header`</span><span class="sxs-lookup"><span data-stu-id="fc377-445">**internal:** `My header`</span></span>

<span data-ttu-id="fc377-446">Yukarıdaki kod, **iç:** `My header` yanıt üst bilgisini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fc377-446">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="fc377-447">Öznitelik üzerinde IFilterFactory uygulandı</span><span class="sxs-lookup"><span data-stu-id="fc377-447">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="fc377-448">`IFilterFactory` uygulayan filtreler şu filtreler için yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="fc377-448">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="fc377-449">Parametre geçirme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fc377-449">Don't require passing parameters.</span></span>
* <span data-ttu-id="fc377-450">DI tarafından doldurulması gereken Oluşturucu bağımlılıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="fc377-450">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="fc377-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="fc377-452">`IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="fc377-452">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="fc377-453">`CreateInstance`, belirtilen türü hizmetler kapsayıcısından (DI) yükler.</span><span class="sxs-lookup"><span data-stu-id="fc377-453">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="fc377-454">Aşağıdaki kodda `[SampleActionFilter]`uygulamak için üç yaklaşım gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="fc377-454">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="fc377-455">Yukarıdaki kodda, yöntemi `[SampleActionFilter]` olarak dekorasyon, `SampleActionFilter`uygulamak için tercih edilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-455">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="fc377-456">Filtre ardışık düzeninde ara yazılım kullanma</span><span class="sxs-lookup"><span data-stu-id="fc377-456">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="fc377-457">Kaynak filtreleri, işlem hattında daha sonra gelen her şeyin yürütülmesini çevreleyecek olan [Ara yazılım](xref:fundamentals/middleware/index) gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-457">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="fc377-458">Ancak filtreler, çalışma zamanının bir parçası oldukları ve bu sayede bağlam ve yapılara erişimi olan ara yazılımlar farklıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-458">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="fc377-459">Ara yazılımı bir filtre olarak kullanmak için, filtre ardışık düzenine eklenecek olan ara yazılımı belirten `Configure` yöntemi ile bir tür oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fc377-459">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="fc377-460">Aşağıdaki örnek, bir istek için geçerli kültürü oluşturmak üzere yerelleştirme ara yazılımını kullanır:</span><span class="sxs-lookup"><span data-stu-id="fc377-460">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="fc377-461">Ara yazılımı çalıştırmak için <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> kullanın:</span><span class="sxs-lookup"><span data-stu-id="fc377-461">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="fc377-462">Ara yazılım filtreleri, filtre işlem hattının aynı aşamasında, model bağlamadan önce ve işlem hattının geri kalanı ile kaynak filtreleri olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-462">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="fc377-463">Sonraki eylemler</span><span class="sxs-lookup"><span data-stu-id="fc377-463">Next actions</span></span>

* <span data-ttu-id="fc377-464">[Razor Pages Için filtre yöntemlerine](xref:razor-pages/filter)bakın.</span><span class="sxs-lookup"><span data-stu-id="fc377-464">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="fc377-465">Filtrelerle denemek için [GitHub örneğini indirin, test edin ve değiştirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span><span class="sxs-lookup"><span data-stu-id="fc377-465">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fc377-466">[Kirk Larkabağı](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/)ve [Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="fc377-466">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="fc377-467">ASP.NET Core *Filtreler* , istek işleme ardışık düzeninde belirli aşamalardan önce veya sonra kod çalıştırılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="fc377-467">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="fc377-468">Yerleşik Filtreler şunları gibi görevleri işler:</span><span class="sxs-lookup"><span data-stu-id="fc377-468">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="fc377-469">Yetkilendirme (kullanıcının yetkili olmadığı kaynaklara erişimi önler).</span><span class="sxs-lookup"><span data-stu-id="fc377-469">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="fc377-470">Yanıt önbelleğe alma (istek ardışık düzenini önbelleğe alınmış bir yanıt döndürecek şekilde döndürür).</span><span class="sxs-lookup"><span data-stu-id="fc377-470">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="fc377-471">Çapraz kesme sorunlarını işlemek için özel filtreler oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-471">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="fc377-472">Çapraz kesme sorunlarına örnek olarak hata işleme, önbelleğe alma, yapılandırma, yetkilendirme ve günlüğe kaydetme dahildir.</span><span class="sxs-lookup"><span data-stu-id="fc377-472">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="fc377-473">Filtreler kodu çoğaltmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="fc377-473">Filters avoid duplicating code.</span></span> <span data-ttu-id="fc377-474">Örneğin, bir hata işleme özel durum filtresi hata işlemeyi birleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-474">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="fc377-475">Bu belge, görünümler içeren Razor Pages, API denetleyicileri ve denetleyiciler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="fc377-475">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="fc377-476">Örneği ([indirme](xref:index#how-to-download-a-sample)) [görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) .</span><span class="sxs-lookup"><span data-stu-id="fc377-476">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="fc377-477">Filtreler nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="fc377-477">How filters work</span></span>

<span data-ttu-id="fc377-478">Filtreler, bazen *Filtre işlem hattı*olarak da adlandırılan *ASP.NET Core eylemi çağırma işlem hattı*içinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-478">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="fc377-479">Filtre işlem hattı çalıştırılacak eylemi ASP.NET Core seçtikten sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-479">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![İstek diğer ara yazılım, yönlendirme ara yazılımı, eylem seçimi ve ASP.NET Core eylemi çağırma Işlem hattı aracılığıyla işlenir.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="fc377-482">Filtre türleri</span><span class="sxs-lookup"><span data-stu-id="fc377-482">Filter types</span></span>

<span data-ttu-id="fc377-483">Her filtre türü, filtre ardışık düzeninde farklı bir aşamada yürütülür:</span><span class="sxs-lookup"><span data-stu-id="fc377-483">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="fc377-484">İlk olarak [Yetkilendirme filtreleri](#authorization-filters) çalışır ve kullanıcının istek için yetkilendirilip yetkilendirilmediğini tespit etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-484">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="fc377-485">Yetkilendirme filtreleri, istek yetkisiz ise işlem hattı kısa devre dışı.</span><span class="sxs-lookup"><span data-stu-id="fc377-485">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="fc377-486">[Kaynak filtreleri](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="fc377-486">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="fc377-487">Yetkilendirmeden sonra çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="fc377-487">Run after authorization.</span></span>  
  * <span data-ttu-id="fc377-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*>, filtre işlem hattının geri kalanının önüne kod çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="fc377-489">Örneğin `OnResourceExecuting`, model bağlamadan önce kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-489">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="fc377-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*>, ardışık düzenin geri kalanı tamamlandıktan sonra kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="fc377-491">[Eylem filtreleri](#action-filters) , tek bir eylem yöntemi çağrıldıktan hemen önce ve sonra kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-491">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="fc377-492">Bir eyleme geçirilen bağımsız değişkenleri ve eylemden döndürülen sonucu işlemek için kullanılabilirler.</span><span class="sxs-lookup"><span data-stu-id="fc377-492">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="fc377-493">Eylem filtreleri Razor Pages **desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="fc377-493">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="fc377-494">[Özel durum filtreleri](#exception-filters) , yanıt gövdesine hiçbir şey yazılmadan önce oluşan işlenmemiş özel durumlara genel ilkeler uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-494">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="fc377-495">[Sonuç filtreleri](#result-filters) , tek tek eylem sonuçlarının yürütülmesinden hemen önce ve sonra kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-495">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="fc377-496">Bunlar yalnızca eylem yöntemi başarıyla yürütüldüğünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-496">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="fc377-497">Bu değerler, görünüm veya biçimlendirici yürütmesinin yürütülmesi gereken mantık için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-497">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="fc377-498">Aşağıdaki diyagramda filtre türlerinin filtre ardışık düzeninde nasıl etkileşimde bulunduğu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fc377-498">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![İstek, Yetkilendirme filtreleri, kaynak filtreleri, model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütmesi aracılığıyla işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="fc377-501">Uygulama</span><span class="sxs-lookup"><span data-stu-id="fc377-501">Implementation</span></span>

<span data-ttu-id="fc377-502">Filtreler, farklı arabirim tanımları aracılığıyla hem zaman uyumlu hem de zaman uyumsuz uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="fc377-502">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="fc377-503">Zaman uyumlu filtreler, ardışık düzen aşamasından önce (`On-Stage-Executing`) ve sonra kodu çalıştırabilir (`On-Stage-Executed`).</span><span class="sxs-lookup"><span data-stu-id="fc377-503">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="fc377-504">Örneğin, `OnActionExecuting` eylem yöntemi çağrılmadan önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-504">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="fc377-505">`OnActionExecuted`, eylem yöntemi döndüğünde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-505">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="fc377-506">Zaman uyumsuz filtreler bir `On-Stage-ExecutionAsync` yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="fc377-506">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="fc377-507">Yukarıdaki kodda `SampleAsyncActionFilter`, eylem yöntemini yürüten bir <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fc377-507">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="fc377-508">`On-Stage-ExecutionAsync` yöntemlerinin her biri, filtrenin ardışık düzen aşamasını yürüten bir `FilterType-ExecutionDelegate` alır.</span><span class="sxs-lookup"><span data-stu-id="fc377-508">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="fc377-509">Birden çok filtre aşaması</span><span class="sxs-lookup"><span data-stu-id="fc377-509">Multiple filter stages</span></span>

<span data-ttu-id="fc377-510">Birden çok filtre aşaması için arabirimler tek bir sınıfta uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-510">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="fc377-511">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sınıfı `IActionFilter`, `IResultFilter`ve zaman uyumsuz eşdeğerlerini uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-511">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="fc377-512">Her ikisini de **değil** , bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="fc377-512">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="fc377-513">Çalışma zamanı öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-513">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="fc377-514">Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-514">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="fc377-515">Tek bir sınıfta hem zaman uyumsuz hem de zaman uyumlu arabirimler uygulanmışsa, yalnızca zaman uyumsuz yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-515">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="fc377-516"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> gibi soyut sınıflar kullanılırken, yalnızca zaman uyumlu yöntemleri veya her bir filtre türü için zaman uyumsuz yöntemi geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="fc377-516">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="fc377-517">Yerleşik filtre öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="fc377-517">Built-in filter attributes</span></span>

<span data-ttu-id="fc377-518">ASP.NET Core, alt sınıflanmış ve özelleştirilebilen yerleşik öznitelik tabanlı filtreler içerir.</span><span class="sxs-lookup"><span data-stu-id="fc377-518">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="fc377-519">Örneğin, aşağıdaki sonuç filtresi yanıta bir üst bilgi ekler:</span><span class="sxs-lookup"><span data-stu-id="fc377-519">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="fc377-520">Öznitelikler, önceki örnekte gösterildiği gibi, filtrelerin bağımsız değişkenleri kabul etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="fc377-520">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="fc377-521">`AddHeaderAttribute` bir denetleyiciye veya eylem yöntemine uygulayın ve HTTP üstbilgisinin adını ve değerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="fc377-521">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="fc377-522">Filtre arabirimlerinden birkaçı, özel uygulamalar için temel sınıflar olarak kullanılabilecek karşılık gelen özniteliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fc377-522">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="fc377-523">Filtre öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-523">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="fc377-524">Kapsamları ve yürütme sırasını filtrele</span><span class="sxs-lookup"><span data-stu-id="fc377-524">Filter scopes and order of execution</span></span>

<span data-ttu-id="fc377-525">Üç *kapsamından*birindeki işlem hattına bir filtre eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-525">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="fc377-526">Bir eylem üzerinde bir öznitelik kullanma.</span><span class="sxs-lookup"><span data-stu-id="fc377-526">Using an attribute on an action.</span></span>
* <span data-ttu-id="fc377-527">Bir denetleyicide bir özniteliği kullanma.</span><span class="sxs-lookup"><span data-stu-id="fc377-527">Using an attribute on a controller.</span></span>
* <span data-ttu-id="fc377-528">Aşağıdaki kodda gösterildiği gibi tüm denetleyiciler ve eylemler için genel olarak:</span><span class="sxs-lookup"><span data-stu-id="fc377-528">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="fc377-529">Yukarıdaki kod, [Mvcoptions. Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) koleksiyonunu kullanarak genel olarak üç filtre ekler.</span><span class="sxs-lookup"><span data-stu-id="fc377-529">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="fc377-530">Varsayılan yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="fc377-530">Default order of execution</span></span>

<span data-ttu-id="fc377-531">*Aynı türde*birden çok filtre olduğunda kapsam, filtre yürütmenin varsayılan sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="fc377-531">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="fc377-532">Genel filtreler saran sınıf filtreleri.</span><span class="sxs-lookup"><span data-stu-id="fc377-532">Global filters surround class filters.</span></span> <span data-ttu-id="fc377-533">Sınıf filtreleri surround yöntemi filtreleri.</span><span class="sxs-lookup"><span data-stu-id="fc377-533">Class filters surround method filters.</span></span>

<span data-ttu-id="fc377-534">Filtre iç içe geçme sonucu *olarak, filtrenin kodu,* *önceki* kodun ters sırasına göre çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-534">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="fc377-535">Filtre sırası:</span><span class="sxs-lookup"><span data-stu-id="fc377-535">The filter sequence:</span></span>

* <span data-ttu-id="fc377-536">Genel filtrelerin *önceki* kodu.</span><span class="sxs-lookup"><span data-stu-id="fc377-536">The *before* code of global filters.</span></span>
  * <span data-ttu-id="fc377-537">Denetleyici filtrelerinden *önceki* kod.</span><span class="sxs-lookup"><span data-stu-id="fc377-537">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="fc377-538">Eylem yöntemi filtrelerinden *önceki* kod.</span><span class="sxs-lookup"><span data-stu-id="fc377-538">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="fc377-539">Eylem yöntemi filtrelerinden *sonraki* kod.</span><span class="sxs-lookup"><span data-stu-id="fc377-539">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="fc377-540">Denetleyici filtrelerinden *sonraki* kod.</span><span class="sxs-lookup"><span data-stu-id="fc377-540">The *after* code of controller filters.</span></span>
* <span data-ttu-id="fc377-541">Genel filtrelerin *sonraki* kodu.</span><span class="sxs-lookup"><span data-stu-id="fc377-541">The *after* code of global filters.</span></span>
  
<span data-ttu-id="fc377-542">Zaman uyumlu eylem filtreleri için filtre yöntemlerinin çağrıldığı sırayı gösteren aşağıdaki örnek.</span><span class="sxs-lookup"><span data-stu-id="fc377-542">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="fc377-543">Sequence</span><span class="sxs-lookup"><span data-stu-id="fc377-543">Sequence</span></span> | <span data-ttu-id="fc377-544">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="fc377-544">Filter scope</span></span> | <span data-ttu-id="fc377-545">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="fc377-545">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="fc377-546">1\.</span><span class="sxs-lookup"><span data-stu-id="fc377-546">1</span></span> | <span data-ttu-id="fc377-547">Global</span><span class="sxs-lookup"><span data-stu-id="fc377-547">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="fc377-548">2</span><span class="sxs-lookup"><span data-stu-id="fc377-548">2</span></span> | <span data-ttu-id="fc377-549">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="fc377-549">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="fc377-550">3</span><span class="sxs-lookup"><span data-stu-id="fc377-550">3</span></span> | <span data-ttu-id="fc377-551">Yöntem</span><span class="sxs-lookup"><span data-stu-id="fc377-551">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="fc377-552">4</span><span class="sxs-lookup"><span data-stu-id="fc377-552">4</span></span> | <span data-ttu-id="fc377-553">Yöntem</span><span class="sxs-lookup"><span data-stu-id="fc377-553">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="fc377-554">5</span><span class="sxs-lookup"><span data-stu-id="fc377-554">5</span></span> | <span data-ttu-id="fc377-555">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="fc377-555">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="fc377-556">6</span><span class="sxs-lookup"><span data-stu-id="fc377-556">6</span></span> | <span data-ttu-id="fc377-557">Global</span><span class="sxs-lookup"><span data-stu-id="fc377-557">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="fc377-558">Bu sıra şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-558">This sequence shows:</span></span>

* <span data-ttu-id="fc377-559">Yöntem filtresi, denetleyici filtresi içinde iç içe geçmiş.</span><span class="sxs-lookup"><span data-stu-id="fc377-559">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="fc377-560">Denetleyici filtresi, genel filtrenin içinde iç içe geçmiş.</span><span class="sxs-lookup"><span data-stu-id="fc377-560">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="fc377-561">Denetleyici ve Razor sayfa düzeyi filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-561">Controller and Razor Page level filters</span></span>

<span data-ttu-id="fc377-562"><xref:Microsoft.AspNetCore.Mvc.Controller> temel sınıfından devralan her denetleyici [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)ve [controller. onactionyürütülmüş](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` yöntemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="fc377-562">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="fc377-563">Bu Yöntemler:</span><span class="sxs-lookup"><span data-stu-id="fc377-563">These methods:</span></span>

* <span data-ttu-id="fc377-564">Belirli bir eylem için çalışan filtreleri sarın.</span><span class="sxs-lookup"><span data-stu-id="fc377-564">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="fc377-565">`OnActionExecuting`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-565">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="fc377-566">`OnActionExecuted` tüm eylem filtrelerinden sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-566">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="fc377-567">`OnActionExecutionAsync`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-567">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="fc377-568">`next` sonra filtre içindeki kod eylem yönteminden sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-568">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="fc377-569">Örneğin, indirme örneğinde `MySampleActionFilter`, başlangıçta genel olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-569">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="fc377-570">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="fc377-570">The `TestController`:</span></span>

* <span data-ttu-id="fc377-571">`FilterTest2` eyleme `SampleActionFilterAttribute` (`[SampleActionFilter]`) uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-571">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="fc377-572">`OnActionExecuting` ve `OnActionExecuted`geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="fc377-572">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="fc377-573">`https://localhost:5001/Test/FilterTest2` gitme aşağıdaki kodu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="fc377-573">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="fc377-574">Razor Pages için bkz. [filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="fc377-574">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="fc377-575">Varsayılan sırayı geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="fc377-575">Overriding the default order</span></span>

<span data-ttu-id="fc377-576">Varsayılan yürütme sırası <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>uygulayarak geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-576">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="fc377-577">`IOrderedFilter`, yürütme sırasını belirlemede kapsama göre öncelik alan <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="fc377-577">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="fc377-578">Daha düşük bir `Order` değerine sahip bir filtre:</span><span class="sxs-lookup"><span data-stu-id="fc377-578">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="fc377-579">Daha yüksek bir `Order`bir filtrenin *önüne kodundan önce* kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-579">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="fc377-580">Daha yüksek `Order` değerine sahip bir filtrenin *sonrasında koddan sonra* çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-580">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="fc377-581">`Order` özelliği bir Oluşturucu parametresiyle ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-581">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="fc377-582">Yukarıdaki örnekte gösterilen 3 eylem filtresini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="fc377-582">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="fc377-583">Denetleyicinin ve genel filtrelerin `Order` özelliği sırasıyla 1 ve 2 ' ye ayarlandıysa, yürütme sırası tersine çevrilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-583">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="fc377-584">Sequence</span><span class="sxs-lookup"><span data-stu-id="fc377-584">Sequence</span></span> | <span data-ttu-id="fc377-585">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="fc377-585">Filter scope</span></span> | <span data-ttu-id="fc377-586">`Order` özelliği</span><span class="sxs-lookup"><span data-stu-id="fc377-586">`Order` property</span></span> | <span data-ttu-id="fc377-587">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="fc377-587">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="fc377-588">1\.</span><span class="sxs-lookup"><span data-stu-id="fc377-588">1</span></span> | <span data-ttu-id="fc377-589">Yöntem</span><span class="sxs-lookup"><span data-stu-id="fc377-589">Method</span></span> | <span data-ttu-id="fc377-590">0</span><span class="sxs-lookup"><span data-stu-id="fc377-590">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="fc377-591">2</span><span class="sxs-lookup"><span data-stu-id="fc377-591">2</span></span> | <span data-ttu-id="fc377-592">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="fc377-592">Controller</span></span> | <span data-ttu-id="fc377-593">1\.</span><span class="sxs-lookup"><span data-stu-id="fc377-593">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="fc377-594">3</span><span class="sxs-lookup"><span data-stu-id="fc377-594">3</span></span> | <span data-ttu-id="fc377-595">Global</span><span class="sxs-lookup"><span data-stu-id="fc377-595">Global</span></span> | <span data-ttu-id="fc377-596">2</span><span class="sxs-lookup"><span data-stu-id="fc377-596">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="fc377-597">4</span><span class="sxs-lookup"><span data-stu-id="fc377-597">4</span></span> | <span data-ttu-id="fc377-598">Global</span><span class="sxs-lookup"><span data-stu-id="fc377-598">Global</span></span> | <span data-ttu-id="fc377-599">2</span><span class="sxs-lookup"><span data-stu-id="fc377-599">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="fc377-600">5</span><span class="sxs-lookup"><span data-stu-id="fc377-600">5</span></span> | <span data-ttu-id="fc377-601">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="fc377-601">Controller</span></span> | <span data-ttu-id="fc377-602">1\.</span><span class="sxs-lookup"><span data-stu-id="fc377-602">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="fc377-603">6</span><span class="sxs-lookup"><span data-stu-id="fc377-603">6</span></span> | <span data-ttu-id="fc377-604">Yöntem</span><span class="sxs-lookup"><span data-stu-id="fc377-604">Method</span></span> | <span data-ttu-id="fc377-605">0</span><span class="sxs-lookup"><span data-stu-id="fc377-605">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="fc377-606">`Order` özelliği filtrelerin çalıştırıldığı sırayı belirlerken kapsamı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="fc377-606">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="fc377-607">Filtreler sırasıyla sıraya göre sıralanır, ardından kapsam bölmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-607">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="fc377-608">Yerleşik filtrelerin tümü `IOrderedFilter` uygular ve varsayılan `Order` değerini 0 olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-608">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="fc377-609">Yerleşik filtreler için, `Order` sıfır olmayan bir değere ayarlanmadığı takdirde kapsam sıralamayı belirler.</span><span class="sxs-lookup"><span data-stu-id="fc377-609">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="fc377-610">İptal ve kısa devre dışı</span><span class="sxs-lookup"><span data-stu-id="fc377-610">Cancellation and short-circuiting</span></span>

<span data-ttu-id="fc377-611">Filtre işlem hattı, filtre yöntemine sunulan <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parametresindeki <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> özelliği ayarlanarak kısa devre dışı olabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-611">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="fc377-612">Örneğin, aşağıdaki kaynak filtresi, ardışık düzenin geri kalanının yürütülmesini engeller:</span><span class="sxs-lookup"><span data-stu-id="fc377-612">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="fc377-613">Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` hem de `AddHeader` filtresi `SomeResource` Action metodunu hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="fc377-613">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="fc377-614">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="fc377-614">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="fc377-615">Önce bir kaynak filtresi olduğundan ve `AddHeader` bir eylem filtresi olduğundan, önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-615">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="fc377-616">Kısa süreli işlem hattının geri kalanı.</span><span class="sxs-lookup"><span data-stu-id="fc377-616">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="fc377-617">Bu nedenle `AddHeader` filtresi `SomeResource` eylemi için hiçbir şekilde çalışmamayacaktır.</span><span class="sxs-lookup"><span data-stu-id="fc377-617">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="fc377-618">Her iki filtre de eylem yöntemi düzeyinde uygulanırsa, `ShortCircuitingResourceFilter` ilk kez çalıştırıldıysa bu davranış aynı olur.</span><span class="sxs-lookup"><span data-stu-id="fc377-618">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="fc377-619">`ShortCircuitingResourceFilter`, filtre türü veya `Order` özelliğinin açık kullanımı nedeniyle önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-619">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="fc377-620">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="fc377-620">Dependency injection</span></span>

<span data-ttu-id="fc377-621">Filtreler türe veya örneğe göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-621">Filters can be added by type or by instance.</span></span> <span data-ttu-id="fc377-622">Bir örnek eklenirse, bu örnek her istek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-622">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="fc377-623">Bir tür eklenirse, türü etkinleştirilmiş olur.</span><span class="sxs-lookup"><span data-stu-id="fc377-623">If a type is added, it's type-activated.</span></span> <span data-ttu-id="fc377-624">Tür etkinleştirilmiş bir filtre şu anlama gelir:</span><span class="sxs-lookup"><span data-stu-id="fc377-624">A type-activated filter means:</span></span>

* <span data-ttu-id="fc377-625">Her istek için bir örnek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="fc377-625">An instance is created for each request.</span></span>
* <span data-ttu-id="fc377-626">Herhangi bir Oluşturucu bağımlılığı [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından doldurulur.</span><span class="sxs-lookup"><span data-stu-id="fc377-626">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="fc377-627">Öznitelik olarak uygulanan ve doğrudan denetleyici sınıflarına veya eylem yöntemlerine eklenen filtreler [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından sağlanmış Oluşturucu bağımlılıklarına sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="fc377-627">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="fc377-628">Oluşturucu bağımlılıkları şu nedenle şu nedenle sağlanamaz:</span><span class="sxs-lookup"><span data-stu-id="fc377-628">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="fc377-629">Özniteliklerin, uygulandıkları yerlerde, Oluşturucu parametreleri sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-629">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="fc377-630">Bu, özniteliklerin nasıl çalıştığı konusunda bir kısıtlamadır.</span><span class="sxs-lookup"><span data-stu-id="fc377-630">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="fc377-631">Aşağıdaki filtreler, dı tarafından belirtilen Oluşturucu bağımlılıklarını destekler:</span><span class="sxs-lookup"><span data-stu-id="fc377-631">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="fc377-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> özniteliğe uygulandı.</span><span class="sxs-lookup"><span data-stu-id="fc377-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="fc377-633">Önceki filtreler bir denetleyiciye veya eylem yöntemine uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-633">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="fc377-634">Günlükçüler, DI 'den alınabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-634">Loggers are available from DI.</span></span> <span data-ttu-id="fc377-635">Ancak, yalnızca günlüğe kaydetme amacıyla filtre oluşturmaktan ve kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="fc377-635">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="fc377-636">[Yerleşik çerçeve günlüğü](xref:fundamentals/logging/index) genellikle günlüğe kaydetme için gerekenleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-636">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="fc377-637">Filtrelere günlük eklendi:</span><span class="sxs-lookup"><span data-stu-id="fc377-637">Logging added to filters:</span></span>

* <span data-ttu-id="fc377-638">, İş etki alanı kaygılarını veya filtreye özgü davranışları odaklamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-638">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="fc377-639">Eylemleri veya diğer çerçeve olaylarını günlüğe **içermemelidir** .</span><span class="sxs-lookup"><span data-stu-id="fc377-639">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="fc377-640">Yerleşik filtreler günlük eylemleri ve çerçeve olayları.</span><span class="sxs-lookup"><span data-stu-id="fc377-640">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="fc377-641">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="fc377-641">ServiceFilterAttribute</span></span>

<span data-ttu-id="fc377-642">Hizmet filtresi uygulama türleri `ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-642">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="fc377-643"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, bir filtrenin bir örneğini dı öğesinden alır.</span><span class="sxs-lookup"><span data-stu-id="fc377-643">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="fc377-644">Aşağıdaki kod `AddHeaderResultServiceFilter`gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-644">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="fc377-645">Aşağıdaki kodda, `AddHeaderResultServiceFilter` dı kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="fc377-645">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="fc377-646">Aşağıdaki kodda, `ServiceFilter` özniteliği dı öğesinden `AddHeaderResultServiceFilter` filtrenin bir örneğini alır:</span><span class="sxs-lookup"><span data-stu-id="fc377-646">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="fc377-647">`ServiceFilterAttribute`kullanırken, [Servicefilterattribute. ısyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="fc377-647">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="fc377-648">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden *kullanılabilir olabileceğini gösteren* bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-648">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="fc377-649">ASP.NET Core çalışma zamanı garanti etmez:</span><span class="sxs-lookup"><span data-stu-id="fc377-649">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="fc377-650">Filtrenin tek bir örneğinin oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="fc377-650">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="fc377-651">Filtre, sonraki bir noktada dı kapsayıcısından yeniden istenmeyecek.</span><span class="sxs-lookup"><span data-stu-id="fc377-651">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="fc377-652">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-652">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="fc377-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="fc377-654">`IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="fc377-654">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="fc377-655">`CreateInstance`, belirtilen türü DI 'dan yükler.</span><span class="sxs-lookup"><span data-stu-id="fc377-655">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="fc377-656">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="fc377-656">TypeFilterAttribute</span></span>

<span data-ttu-id="fc377-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>benzerdir, ancak türü doğrudan dı kapsayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="fc377-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="fc377-658"><xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>kullanarak türü başlatır.</span><span class="sxs-lookup"><span data-stu-id="fc377-658">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="fc377-659">`TypeFilterAttribute` türler doğrudan dı kapsayıcısından çözümlenmediğinden:</span><span class="sxs-lookup"><span data-stu-id="fc377-659">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="fc377-660">`TypeFilterAttribute` kullanılarak başvurulan türlerin dı kapsayıcısına kaydedilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fc377-660">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="fc377-661">Bunların bağımlılıkları, dı kapsayıcısı tarafından yerine getirilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-661">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="fc377-662">`TypeFilterAttribute`, isteğe bağlı olarak tür için Oluşturucu bağımsız değişkenlerini kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-662">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="fc377-663">`TypeFilterAttribute`kullanırken [Typefilterattribute. ıyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="fc377-663">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="fc377-664">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden kullanılabilir *olabileceği* ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-664">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="fc377-665">ASP.NET Core çalışma zamanı, filtrenin tek bir örneğinin oluşturulacağı garantisi vermez.</span><span class="sxs-lookup"><span data-stu-id="fc377-665">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="fc377-666">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-666">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="fc377-667">Aşağıdaki örnek `TypeFilterAttribute`kullanarak bir türe bağımsız değişkenlerin nasıl geçirileceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-667">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="fc377-668">Yetkilendirme filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-668">Authorization filters</span></span>

<span data-ttu-id="fc377-669">Yetkilendirme filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-669">Authorization filters:</span></span>

* <span data-ttu-id="fc377-670">, Filtre ardışık düzeninde ilk filtrelerin çalıştırılmasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-670">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="fc377-671">Eylem yöntemlerine erişimi denetleyin.</span><span class="sxs-lookup"><span data-stu-id="fc377-671">Control access to action methods.</span></span>
* <span data-ttu-id="fc377-672">Bir Before yöntemi, ancak After yönteminden hiçbiri.</span><span class="sxs-lookup"><span data-stu-id="fc377-672">Have a before method, but no after method.</span></span>

<span data-ttu-id="fc377-673">Özel Yetkilendirme filtreleri özel bir yetkilendirme çerçevesi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="fc377-673">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="fc377-674">Özel bir filtre yazmak için yetkilendirme ilkelerini yapılandırmayı veya özel bir yetkilendirme ilkesi yazmayı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="fc377-674">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="fc377-675">Yerleşik yetkilendirme filtresi:</span><span class="sxs-lookup"><span data-stu-id="fc377-675">The built-in authorization filter:</span></span>

* <span data-ttu-id="fc377-676">Yetkilendirme sistemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="fc377-676">Calls the authorization system.</span></span>
* <span data-ttu-id="fc377-677">İstekleri yetkilendirmez.</span><span class="sxs-lookup"><span data-stu-id="fc377-677">Does not authorize requests.</span></span>

<span data-ttu-id="fc377-678">Yetkilendirme filtreleri içinde özel **durumlar atamayın:**</span><span class="sxs-lookup"><span data-stu-id="fc377-678">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="fc377-679">Özel durum işlenmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="fc377-679">The exception will not be handled.</span></span>
* <span data-ttu-id="fc377-680">Özel durum filtreleri özel durumu işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="fc377-680">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="fc377-681">Bir yetkilendirme filtresinde özel durum oluştuğunda bir sınama vermeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="fc377-681">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="fc377-682">[Yetkilendirme](xref:security/authorization/introduction)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="fc377-682">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="fc377-683">Kaynak filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-683">Resource filters</span></span>

<span data-ttu-id="fc377-684">Kaynak filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-684">Resource filters:</span></span>

* <span data-ttu-id="fc377-685"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-685">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="fc377-686">Yürütme, filtre işlem hattının çoğunu sarmalar.</span><span class="sxs-lookup"><span data-stu-id="fc377-686">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="fc377-687">Kaynak filtrelerinden önce yalnızca [Yetkilendirme filtreleri](#authorization-filters) çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-687">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="fc377-688">Kaynak filtreleri, işlem hattının büyük bir yanındaki kısa devre için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-688">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="fc377-689">Örneğin, bir önbelleğe alma filtresi, bir önbellek isabetinden ardışık düzen geri kalanından kaçınabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-689">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="fc377-690">Kaynak filtresi örnekleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-690">Resource filter examples:</span></span>

* <span data-ttu-id="fc377-691">Daha önce gösterilen [kısa devre dışı kaynak filtresi](#short-circuiting-resource-filter) .</span><span class="sxs-lookup"><span data-stu-id="fc377-691">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="fc377-692">[Disableformvaluemodelbindingattribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="fc377-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="fc377-693">Model bağlamanın form verilerine erişimini engeller.</span><span class="sxs-lookup"><span data-stu-id="fc377-693">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="fc377-694">Form verilerinin belleğe okunmasını engellemek için büyük dosya yüklemeleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-694">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="fc377-695">Eylem filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-695">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fc377-696">Eylem filtreleri Razor Pages **için uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="fc377-696">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="fc377-697">Razor Pages <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> destekler.</span><span class="sxs-lookup"><span data-stu-id="fc377-697">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="fc377-698">Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="fc377-698">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="fc377-699">Eylem filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-699">Action filters:</span></span>

* <span data-ttu-id="fc377-700"><xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-700">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="fc377-701">Yürütmesinin, eylem yöntemlerinin yürütülmesi çevreler.</span><span class="sxs-lookup"><span data-stu-id="fc377-701">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="fc377-702">Aşağıdaki kod bir örnek eylem filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-702">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="fc377-703"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="fc377-703">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="fc377-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-bir eylem yöntemine yönelik girişlerin okunmalarını sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="fc377-705"><xref:Microsoft.AspNetCore.Mvc.Controller>-denetleyici örneğinin işlenmesine izin vermez.</span><span class="sxs-lookup"><span data-stu-id="fc377-705"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="fc377-706"><xref:System.Web.Mvc.ActionExecutingContext.Result>-eylem yönteminin ve sonraki eylem filtrelerinin `Result` kısa devre dışı yürütmesi.</span><span class="sxs-lookup"><span data-stu-id="fc377-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="fc377-707">Eylem yönteminde özel durum oluşturma:</span><span class="sxs-lookup"><span data-stu-id="fc377-707">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="fc377-708">Sonraki filtrelerin çalıştırılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="fc377-708">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="fc377-709">`Result`ayarının aksine, başarılı bir sonuç yerine başarısızlık olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-709">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="fc377-710"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> `Controller` ve `Result` ek olarak aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="fc377-710">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="fc377-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-eylem yürütmesi başka bir filtre tarafından kabul edilse true.</span><span class="sxs-lookup"><span data-stu-id="fc377-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="fc377-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-eylem veya daha önce çalıştırılan eylem filtresi bir özel durum oluşturdu, null değil.</span><span class="sxs-lookup"><span data-stu-id="fc377-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="fc377-713">Bu özellik null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="fc377-713">Setting this property to null:</span></span>

  * <span data-ttu-id="fc377-714">Özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="fc377-714">Effectively handles the exception.</span></span>
  * <span data-ttu-id="fc377-715">`Result`, eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fc377-715">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="fc377-716">Bir `IAsyncActionFilter`için <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>çağrısı:</span><span class="sxs-lookup"><span data-stu-id="fc377-716">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="fc377-717">Sonraki eylem filtrelerini ve eylem yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="fc377-717">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="fc377-718">`ActionExecutedContext` döndürür.</span><span class="sxs-lookup"><span data-stu-id="fc377-718">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="fc377-719">Kısa devre dışı, bir sonuç örneğine <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> atayın ve `next` çağırmayın (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="fc377-719">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="fc377-720">Framework, alt sınıflı olabilecek bir soyut <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-720">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="fc377-721">`OnActionExecuting` eylem filtresi şu şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-721">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="fc377-722">Model durumunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-722">Validate model state.</span></span>
* <span data-ttu-id="fc377-723">Durum geçersizse bir hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="fc377-723">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="fc377-724">`OnActionExecuted` yöntemi eylem yönteminden sonra çalışır:</span><span class="sxs-lookup"><span data-stu-id="fc377-724">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="fc377-725">Ve <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> özelliği aracılığıyla eylemin sonuçlarını görebilir ve değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-725">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="fc377-726">eylem yürütmesi başka bir filtre tarafından kabul edilse, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> true olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="fc377-727">eylem veya sonraki eylem filtresi bir özel durum harekete geçirdi, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="fc377-728">`Exception` null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="fc377-728">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="fc377-729">Bir özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="fc377-729">Effectively handles an exception.</span></span>
  * <span data-ttu-id="fc377-730">`ActionExecutedContext.Result`, normal olarak eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fc377-730">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="fc377-731">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-731">Exception filters</span></span>

<span data-ttu-id="fc377-732">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-732">Exception filters:</span></span>

* <span data-ttu-id="fc377-733"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>uygulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-733">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="fc377-734">, Yaygın hata işleme ilkelerini uygulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-734">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="fc377-735">Aşağıdaki örnek özel durum filtresi, uygulama geliştirmede olduğunda oluşan özel durumlar hakkındaki ayrıntıları görüntülemek için özel bir hata görünümü kullanır:</span><span class="sxs-lookup"><span data-stu-id="fc377-735">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="fc377-736">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-736">Exception filters:</span></span>

* <span data-ttu-id="fc377-737">Etkinlikden önceki ve sonraki olaylar yok.</span><span class="sxs-lookup"><span data-stu-id="fc377-737">Don't have before and after events.</span></span>
* <span data-ttu-id="fc377-738"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>uygulayın.</span><span class="sxs-lookup"><span data-stu-id="fc377-738">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="fc377-739">Razor sayfası veya denetleyici oluşturma, [model bağlama](xref:mvc/models/model-binding), eylem filtreleri veya eylem yöntemlerinde oluşan işlenmemiş özel durumları işleyin.</span><span class="sxs-lookup"><span data-stu-id="fc377-739">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="fc377-740">Kaynak filtrelerinde, sonuç filtrelerinde veya MVC sonuç yürütülürken oluşan özel **durumları yakalamayın** .</span><span class="sxs-lookup"><span data-stu-id="fc377-740">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="fc377-741">Bir özel durumu işlemek için <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> özelliğini `true` veya bir yanıt yazın.</span><span class="sxs-lookup"><span data-stu-id="fc377-741">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="fc377-742">Bu, özel durumun yayılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="fc377-742">This stops propagation of the exception.</span></span> <span data-ttu-id="fc377-743">Özel durum filtresi bir özel durumu "başarılı" olarak açamaz.</span><span class="sxs-lookup"><span data-stu-id="fc377-743">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="fc377-744">Yalnızca bir eylem filtresi bunu yapabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-744">Only an action filter can do that.</span></span>

<span data-ttu-id="fc377-745">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-745">Exception filters:</span></span>

* <span data-ttu-id="fc377-746">Eylemler içinde oluşan özel durumları yakalamaya uygundur.</span><span class="sxs-lookup"><span data-stu-id="fc377-746">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="fc377-747">, Hata işleme ara yazılımı olarak esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="fc377-747">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="fc377-748">Özel durum işleme için ara yazılımı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="fc377-748">Prefer middleware for exception handling.</span></span> <span data-ttu-id="fc377-749">Yalnızca hata işlemenin hangi eylem yöntemine göre *farklılık* gösteren özel durum filtrelerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="fc377-749">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="fc377-750">Örneğin, bir uygulama hem API uç noktaları hem de görünümler/HTML için eylem yöntemlerine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-750">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="fc377-751">API uç noktaları, hata bilgilerini JSON olarak döndürebilir, ancak görünüm tabanlı eylemler bir hata sayfasını HTML olarak döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-751">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="fc377-752">Sonuç filtreleri</span><span class="sxs-lookup"><span data-stu-id="fc377-752">Result filters</span></span>

<span data-ttu-id="fc377-753">Sonuç filtreleri:</span><span class="sxs-lookup"><span data-stu-id="fc377-753">Result filters:</span></span>

* <span data-ttu-id="fc377-754">Arabirim uygulama:</span><span class="sxs-lookup"><span data-stu-id="fc377-754">Implement an interface:</span></span>
  * <span data-ttu-id="fc377-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="fc377-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="fc377-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="fc377-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="fc377-757">Yürütmesi, eylem sonuçlarının yürütülmesini çevreler.</span><span class="sxs-lookup"><span data-stu-id="fc377-757">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="fc377-758">IResultFilter ve ıasyncresultfilter</span><span class="sxs-lookup"><span data-stu-id="fc377-758">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="fc377-759">Aşağıdaki kod, bir HTTP üst bilgisi ekleyen bir sonuç filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="fc377-759">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="fc377-760">Yürütülen sonuç türü eyleme göre değişir.</span><span class="sxs-lookup"><span data-stu-id="fc377-760">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="fc377-761">Bir görünüm döndüren bir eylem, yürütülen <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir parçası olarak tüm Razor işlemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="fc377-761">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="fc377-762">Bir API yöntemi, sonucun yürütülmesinin bir parçası olarak bazı serileştirme işlemleri gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-762">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="fc377-763">[Eylem sonuçları](xref:mvc/controllers/actions)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="fc377-763">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="fc377-764">Sonuç filtreleri yalnızca bir eylem veya eylem filtresi bir eylem sonucu üretirse yürütülür.</span><span class="sxs-lookup"><span data-stu-id="fc377-764">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="fc377-765">Şu durumlarda sonuç filtreleri yürütülmez:</span><span class="sxs-lookup"><span data-stu-id="fc377-765">Result filters are not executed when:</span></span>

* <span data-ttu-id="fc377-766">Bir yetkilendirme filtresi veya kaynak filtresi, işlem hattı için kısa süreli olarak devre dışı.</span><span class="sxs-lookup"><span data-stu-id="fc377-766">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="fc377-767">Bir özel durum filtresi, bir eylem sonucu üreterek özel durumu işler.</span><span class="sxs-lookup"><span data-stu-id="fc377-767">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="fc377-768"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> yöntemi, eylem sonucunun ve sonraki sonuç filtrelerinin `true`<xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> ayarlanarak kısa devre yürütülmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-768">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="fc377-769">Boş bir yanıt oluşturmamaya kaçınmak için kısa devre dışı bırakıldığında yanıt nesnesine yazın.</span><span class="sxs-lookup"><span data-stu-id="fc377-769">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="fc377-770">`IResultFilter.OnResultExecuting` bir özel durum oluşturma şu şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="fc377-770">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="fc377-771">Eylem sonucunun ve sonraki filtrelerin yürütülmesini önleyin.</span><span class="sxs-lookup"><span data-stu-id="fc377-771">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="fc377-772">Başarılı bir sonuç yerine hata olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-772">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="fc377-773"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> yöntemi çalıştırıldığında, yanıt istemciye zaten gönderilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-773">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="fc377-774">Yanıt istemciye zaten gönderildiyse, daha fazla değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="fc377-774">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="fc377-775">`ResultExecutedContext.Canceled`, eylem sonucu yürütmesi başka bir filtre tarafından kabul edilen kısa devre ise `true` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-775">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="fc377-776">eylem sonucu veya sonraki sonuç filtresi bir özel durum harekete geçirdi, `ResultExecutedContext.Exception` null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="fc377-776">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="fc377-777">`Exception` null olarak ayarlanması, bir özel durumu etkili bir şekilde işler ve özel durumun, ardışık düzendeki ASP.NET Core daha sonra yeniden oluşturulmasını önler.</span><span class="sxs-lookup"><span data-stu-id="fc377-777">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="fc377-778">Bir sonuç filtresinde özel durum işlenirken yanıta veri yazmanın güvenilir bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="fc377-778">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="fc377-779">Bir eylem sonucu bir özel durum oluşturduğunda üstbilgiler istemciye temizleniyorsa, hata kodu göndermek için güvenilir bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="fc377-779">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="fc377-780">Bir <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>için, <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> bir `await next` çağrısı, sonraki sonuç filtrelerini ve eylem sonucunu yürütür.</span><span class="sxs-lookup"><span data-stu-id="fc377-780">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="fc377-781">Kısa devre dışı, [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) `true` ve `ResultExecutionDelegate`çağırmayın:</span><span class="sxs-lookup"><span data-stu-id="fc377-781">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="fc377-782">Framework, alt sınıflı olabilecek bir soyut `ResultFilterAttribute` sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-782">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="fc377-783">Daha önce gösterilen [Addheaderattribute](#add-header-attribute) sınıfı bir sonuç Filtresi özniteliği örneğidir.</span><span class="sxs-lookup"><span data-stu-id="fc377-783">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="fc377-784">Ialwaysrunresultfilter ve ıasyncalwaysrunresultfilter</span><span class="sxs-lookup"><span data-stu-id="fc377-784">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="fc377-785"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> arabirimleri, tüm eylem sonuçları için çalışan bir <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> uygulamasını bildirir.</span><span class="sxs-lookup"><span data-stu-id="fc377-785">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="fc377-786">Bu, tarafından oluşturulan eylem sonuçlarını içerir:</span><span class="sxs-lookup"><span data-stu-id="fc377-786">This includes action results produced by:</span></span>

* <span data-ttu-id="fc377-787">Kısa devre olan Yetkilendirme filtreleri ve kaynak filtreleri.</span><span class="sxs-lookup"><span data-stu-id="fc377-787">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="fc377-788">Özel durum filtreleri.</span><span class="sxs-lookup"><span data-stu-id="fc377-788">Exception filters.</span></span>

<span data-ttu-id="fc377-789">Örneğin, aşağıdaki filtre her zaman çalışır ve içerik anlaşması başarısız olduğunda *422 olmayan bir varlık* durum kodu ile bir eylem sonucu (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) ayarlar:</span><span class="sxs-lookup"><span data-stu-id="fc377-789">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="fc377-790">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="fc377-790">IFilterFactory</span></span>

<span data-ttu-id="fc377-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="fc377-792">Bu nedenle, bir `IFilterFactory` örneği, filtre ardışık düzeninde herhangi bir yerde `IFilterMetadata` bir örnek olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="fc377-792">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="fc377-793">Çalışma zamanı filtreyi çağırmayı hazırlarken, bir `IFilterFactory`dönüştürmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="fc377-793">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="fc377-794">Bu atama başarılı olursa, çağrılan `IFilterMetadata` örneğini oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="fc377-794">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="fc377-795">Bu, tam filtre işlem hattının uygulama başladığında açıkça ayarlanması gerektiğinden esnek bir tasarım sağlar.</span><span class="sxs-lookup"><span data-stu-id="fc377-795">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="fc377-796">`IFilterFactory`, filtre oluşturmaya yönelik başka bir yaklaşım olarak özel öznitelik uygulamaları kullanılarak uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-796">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="fc377-797">Önceki kod, [indirme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)çalıştırılarak test edilebilir:</span><span class="sxs-lookup"><span data-stu-id="fc377-797">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="fc377-798">F12 geliştirici araçlarını çağırın.</span><span class="sxs-lookup"><span data-stu-id="fc377-798">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="fc377-799">[https://test-cors.org](`https://localhost:5001/Sample/HeaderWithFactory`) sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="fc377-799">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="fc377-800">F12 geliştirici araçları, örnek kod tarafından eklenen aşağıdaki yanıt üstbilgilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="fc377-800">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="fc377-801">**Yazar:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="fc377-801">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="fc377-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="fc377-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="fc377-803">**iç:** `My header`</span><span class="sxs-lookup"><span data-stu-id="fc377-803">**internal:** `My header`</span></span>

<span data-ttu-id="fc377-804">Yukarıdaki kod, **iç:** `My header` yanıt üst bilgisini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="fc377-804">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="fc377-805">Öznitelik üzerinde IFilterFactory uygulandı</span><span class="sxs-lookup"><span data-stu-id="fc377-805">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="fc377-806">`IFilterFactory` uygulayan filtreler şu filtreler için yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="fc377-806">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="fc377-807">Parametre geçirme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="fc377-807">Don't require passing parameters.</span></span>
* <span data-ttu-id="fc377-808">DI tarafından doldurulması gereken Oluşturucu bağımlılıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="fc377-808">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="fc377-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular.</span><span class="sxs-lookup"><span data-stu-id="fc377-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="fc377-810">`IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="fc377-810">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="fc377-811">`CreateInstance`, belirtilen türü hizmetler kapsayıcısından (DI) yükler.</span><span class="sxs-lookup"><span data-stu-id="fc377-811">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="fc377-812">Aşağıdaki kodda `[SampleActionFilter]`uygulamak için üç yaklaşım gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="fc377-812">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="fc377-813">Yukarıdaki kodda, yöntemi `[SampleActionFilter]` olarak dekorasyon, `SampleActionFilter`uygulamak için tercih edilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="fc377-813">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="fc377-814">Filtre ardışık düzeninde ara yazılım kullanma</span><span class="sxs-lookup"><span data-stu-id="fc377-814">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="fc377-815">Kaynak filtreleri, işlem hattında daha sonra gelen her şeyin yürütülmesini çevreleyecek olan [Ara yazılım](xref:fundamentals/middleware/index) gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-815">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="fc377-816">Ancak filtreler, ASP.NET Core çalışma zamanının bir parçası olan ara yazılımlar dışında farklılık gösterir, bu da ASP.NET Core bağlamına ve yapılarına erişime sahip oldukları anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="fc377-816">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="fc377-817">Ara yazılımı bir filtre olarak kullanmak için, filtre ardışık düzenine eklenecek olan ara yazılımı belirten `Configure` yöntemi ile bir tür oluşturun.</span><span class="sxs-lookup"><span data-stu-id="fc377-817">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="fc377-818">Aşağıdaki örnek, bir istek için geçerli kültürü oluşturmak üzere yerelleştirme ara yazılımını kullanır:</span><span class="sxs-lookup"><span data-stu-id="fc377-818">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="fc377-819">Ara yazılımı çalıştırmak için <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> kullanın:</span><span class="sxs-lookup"><span data-stu-id="fc377-819">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="fc377-820">Ara yazılım filtreleri, filtre işlem hattının aynı aşamasında, model bağlamadan önce ve işlem hattının geri kalanı ile kaynak filtreleri olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="fc377-820">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="fc377-821">Sonraki eylemler</span><span class="sxs-lookup"><span data-stu-id="fc377-821">Next actions</span></span>

* <span data-ttu-id="fc377-822">[Razor Pages Için filtre yöntemlerine](xref:razor-pages/filter)bakın.</span><span class="sxs-lookup"><span data-stu-id="fc377-822">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="fc377-823">Filtrelerle denemek için [GitHub örneğini indirin, test edin ve değiştirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="fc377-823">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
