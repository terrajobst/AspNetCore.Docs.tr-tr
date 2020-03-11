---
title: ASP.NET Core filtreler
author: Rick-Anderson
description: Filtrelerin nasıl çalıştığını ve ASP.NET Core nasıl kullanılacağını öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 02/04/2020
uid: mvc/controllers/filters
ms.openlocfilehash: 03335811766ea3a1455901199863c6da0e35f7e4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662787"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="33648-103">ASP.NET Core filtreler</span><span class="sxs-lookup"><span data-stu-id="33648-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="33648-104">[Kirk Larkabağı](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/)ve [Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="33648-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="33648-105">ASP.NET Core *Filtreler* , istek işleme ardışık düzeninde belirli aşamalardan önce veya sonra kod çalıştırılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="33648-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="33648-106">Yerleşik Filtreler şunları gibi görevleri işler:</span><span class="sxs-lookup"><span data-stu-id="33648-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="33648-107">Yetkilendirme (kullanıcının yetkili olmadığı kaynaklara erişimi önler).</span><span class="sxs-lookup"><span data-stu-id="33648-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="33648-108">Yanıt önbelleğe alma (istek ardışık düzenini önbelleğe alınmış bir yanıt döndürecek şekilde döndürür).</span><span class="sxs-lookup"><span data-stu-id="33648-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="33648-109">Çapraz kesme sorunlarını işlemek için özel filtreler oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="33648-110">Çapraz kesme sorunlarına örnek olarak hata işleme, önbelleğe alma, yapılandırma, yetkilendirme ve günlüğe kaydetme dahildir.</span><span class="sxs-lookup"><span data-stu-id="33648-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="33648-111">Filtreler kodu çoğaltmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="33648-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="33648-112">Örneğin, bir hata işleme özel durum filtresi hata işlemeyi birleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="33648-113">Bu belge, görünümler içeren Razor Pages, API denetleyicileri ve denetleyiciler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="33648-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span> <span data-ttu-id="33648-114">Filtreler, [Razor bileşenleriyle](xref:blazor/components)doğrudan çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="33648-114">Filters don't work directly with [Razor components](xref:blazor/components).</span></span> <span data-ttu-id="33648-115">Filtre, şu durumlarda bir bileşeni yalnızca dolaylı olarak etkileyebilir:</span><span class="sxs-lookup"><span data-stu-id="33648-115">A filter can only indirectly affect a component when:</span></span>

* <span data-ttu-id="33648-116">Bileşen bir sayfa veya görünüme katıştırılır.</span><span class="sxs-lookup"><span data-stu-id="33648-116">The component is embedded in a page or view.</span></span>
* <span data-ttu-id="33648-117">Sayfa veya denetleyici/görünüm filtreyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="33648-117">The page or controller/view uses the filter.</span></span>

<span data-ttu-id="33648-118">Örneği ([indirme](xref:index#how-to-download-a-sample)) [görüntüleyin veya indirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) .</span><span class="sxs-lookup"><span data-stu-id="33648-118">[View or download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="33648-119">Filtreler nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="33648-119">How filters work</span></span>

<span data-ttu-id="33648-120">Filtreler, bazen *Filtre işlem hattı*olarak da adlandırılan *ASP.NET Core eylemi çağırma işlem hattı*içinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-120">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span> <span data-ttu-id="33648-121">Filtre işlem hattı çalıştırılacak eylemi ASP.NET Core seçtikten sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-121">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![İstek diğer ara yazılım, yönlendirme ara yazılımı, eylem seçimi ve eylem çağırma Işlem hattı aracılığıyla işlenir.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="33648-124">Filtre türleri</span><span class="sxs-lookup"><span data-stu-id="33648-124">Filter types</span></span>

<span data-ttu-id="33648-125">Her filtre türü, filtre ardışık düzeninde farklı bir aşamada yürütülür:</span><span class="sxs-lookup"><span data-stu-id="33648-125">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="33648-126">İlk olarak [Yetkilendirme filtreleri](#authorization-filters) çalışır ve kullanıcının istek için yetkilendirilip yetkilendirilmediğini tespit etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33648-126">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="33648-127">Yetkilendirme filtreleri, istek yetkilendirilmezse işlem hattı kısa devre dışı olur.</span><span class="sxs-lookup"><span data-stu-id="33648-127">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="33648-128">[Kaynak filtreleri](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="33648-128">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="33648-129">Yetkilendirmeden sonra çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="33648-129">Run after authorization.</span></span>  
  * <span data-ttu-id="33648-130"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*>, filtre ardışık düzeninin geri kalanından önce kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="33648-130"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="33648-131">Örneğin, `OnResourceExecuting` model bağlamalarından önce kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="33648-131">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="33648-132"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*>, ardışık düzenin geri kalanı tamamlandıktan sonra kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="33648-132"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="33648-133">[Eylem filtreleri](#action-filters):</span><span class="sxs-lookup"><span data-stu-id="33648-133">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="33648-134">Kodu bir eylem yöntemi çağrıldıktan hemen önce ve sonra çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="33648-134">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="33648-135">Bir eyleme geçirilen bağımsız değişkenleri değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-135">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="33648-136">Eylemden döndürülen sonucu değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-136">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="33648-137">Razor Pages **desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="33648-137">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="33648-138">[Özel durum filtreleri](#exception-filters) , yanıt gövdesinin üzerine yazılmadan önce oluşan işlenmemiş özel durumlara genel ilkeler uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-138">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="33648-139">[Sonuç filtreleri](#result-filters) , eylem sonuçlarının yürütülmesinden hemen önce ve sonra kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="33648-139">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="33648-140">Bunlar yalnızca eylem yöntemi başarıyla yürütüldüğünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-140">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="33648-141">Bu değerler, görünüm veya biçimlendirici yürütmesinin yürütülmesi gereken mantık için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-141">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="33648-142">Aşağıdaki diyagramda filtre türlerinin filtre ardışık düzeninde nasıl etkileşimde bulunduğu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="33648-142">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![İstek, Yetkilendirme filtreleri, kaynak filtreleri, model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütmesi aracılığıyla işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="33648-145">Uygulama</span><span class="sxs-lookup"><span data-stu-id="33648-145">Implementation</span></span>

<span data-ttu-id="33648-146">Filtreler, farklı arabirim tanımları aracılığıyla hem zaman uyumlu hem de zaman uyumsuz uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="33648-146">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="33648-147">Zaman uyumlu filtreler, ardışık düzen aşamasından önce ve sonra kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="33648-147">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="33648-148">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> eylem yöntemi çağrılmadan önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="33648-148">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="33648-149"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*>, eylem yöntemi döndüğünde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="33648-149"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="33648-150">Zaman uyumsuz filtreler bir `On-Stage-ExecutionAsync` yöntemi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="33648-150">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="33648-151">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span><span class="sxs-lookup"><span data-stu-id="33648-151">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="33648-152">Yukarıdaki kodda `SampleAsyncActionFilter`, eylem yöntemini yürüten bir <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="33648-152">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="33648-153">Birden çok filtre aşaması</span><span class="sxs-lookup"><span data-stu-id="33648-153">Multiple filter stages</span></span>

<span data-ttu-id="33648-154">Birden çok filtre aşaması için arabirimler tek bir sınıfta uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-154">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="33648-155">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sınıfı şunları uygular:</span><span class="sxs-lookup"><span data-stu-id="33648-155">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="33648-156">Zaman uyumlu: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="33648-156">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="33648-157">Zaman uyumsuz: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="33648-157">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="33648-158">Her ikisini de **değil** , bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="33648-158">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="33648-159">Çalışma zamanı öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="33648-159">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="33648-160">Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır.</span><span class="sxs-lookup"><span data-stu-id="33648-160">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="33648-161">Tek bir sınıfta hem zaman uyumsuz hem de zaman uyumlu arabirimler uygulanmışsa, yalnızca zaman uyumsuz yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="33648-161">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="33648-162"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>gibi soyut sınıflar kullanılırken, her bir filtre türü için yalnızca zaman uyumlu yöntemleri veya zaman uyumsuz yöntemi geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="33648-162">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="33648-163">Yerleşik filtre öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="33648-163">Built-in filter attributes</span></span>

<span data-ttu-id="33648-164">ASP.NET Core, alt sınıflanmış ve özelleştirilebilen yerleşik öznitelik tabanlı filtreler içerir.</span><span class="sxs-lookup"><span data-stu-id="33648-164">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="33648-165">Örneğin, aşağıdaki sonuç filtresi yanıta bir üst bilgi ekler:</span><span class="sxs-lookup"><span data-stu-id="33648-165">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="33648-166">Öznitelikler, önceki örnekte gösterildiği gibi, filtrelerin bağımsız değişkenleri kabul etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="33648-166">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="33648-167">`AddHeaderAttribute` bir denetleyiciye veya eylem yöntemine uygulayın ve HTTP üstbilgisinin adını ve değerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="33648-167">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="33648-168">Üst bilgileri incelemek için [tarayıcı geliştirici araçları](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) gibi bir araç kullanın.</span><span class="sxs-lookup"><span data-stu-id="33648-168">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="33648-169">**Yanıt üst bilgileri**altında `author: Rick Anderson` görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="33648-169">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="33648-170">Aşağıdaki kod şu şekilde bir `ActionFilterAttribute` uygular:</span><span class="sxs-lookup"><span data-stu-id="33648-170">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="33648-171">Yapılandırma sistemindeki başlığı ve adı okur.</span><span class="sxs-lookup"><span data-stu-id="33648-171">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="33648-172">Önceki örnekten farklı olarak, aşağıdaki kod koda filtre parametrelerinin eklenmesine gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="33648-172">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="33648-173">Başlık ve adı yanıt üstbilgisine ekler.</span><span class="sxs-lookup"><span data-stu-id="33648-173">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="33648-174">Yapılandırma seçenekleri, [Seçenekler deseninin](xref:fundamentals/configuration/options)kullanıldığı [yapılandırma sisteminden](xref:fundamentals/configuration/index) sağlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-174">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="33648-175">Örneğin, *appSettings. JSON* dosyasından:</span><span class="sxs-lookup"><span data-stu-id="33648-175">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="33648-176">`StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="33648-176">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="33648-177">`PositionOptions` sınıfı, `"Position"` yapılandırma alanı ile hizmet kapsayıcısına eklenir.</span><span class="sxs-lookup"><span data-stu-id="33648-177">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="33648-178">`MyActionFilterAttribute`, hizmet kapsayıcısına eklenir.</span><span class="sxs-lookup"><span data-stu-id="33648-178">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="33648-179">Aşağıdaki kod `PositionOptions` sınıfını gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-179">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="33648-180">Aşağıdaki kod, `Index2` yöntemine `MyActionFilterAttribute` uygular:</span><span class="sxs-lookup"><span data-stu-id="33648-180">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="33648-181">**Yanıt üst bilgileri**, `author: Rick Anderson`ve `Editor: Joe Smith` altında `Sample/Index2` uç noktası çağrıldığında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="33648-181">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="33648-182">Aşağıdaki kod, Razor sayfasına `MyActionFilterAttribute` ve `AddHeaderAttribute` uygular:</span><span class="sxs-lookup"><span data-stu-id="33648-182">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="33648-183">, Razor sayfası işleyici yöntemlerine filtre uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="33648-183">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="33648-184">Bu kişiler Razor sayfa modeline veya küresel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-184">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="33648-185">Filtre arabirimlerinden birkaçı, özel uygulamalar için temel sınıflar olarak kullanılabilecek karşılık gelen özniteliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="33648-185">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="33648-186">Filtre öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="33648-186">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="33648-187">Kapsamları ve yürütme sırasını filtrele</span><span class="sxs-lookup"><span data-stu-id="33648-187">Filter scopes and order of execution</span></span>

<span data-ttu-id="33648-188">Üç *kapsamından*birindeki işlem hattına bir filtre eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="33648-188">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="33648-189">Bir denetleyici eyleminde bir özniteliği kullanma.</span><span class="sxs-lookup"><span data-stu-id="33648-189">Using an attribute on a controller action.</span></span> <span data-ttu-id="33648-190">Filtre öznitelikleri Razor Pages işleyici yöntemlerine uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="33648-190">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="33648-191">Bir denetleyici veya Razor sayfasında bir özniteliği kullanma.</span><span class="sxs-lookup"><span data-stu-id="33648-191">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="33648-192">Genel olarak tüm denetleyiciler, Eylemler ve Razor Pages aşağıdaki kodda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="33648-192">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="33648-193">Varsayılan yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="33648-193">Default order of execution</span></span>

<span data-ttu-id="33648-194">İşlem hattının belirli bir aşamasına ilişkin birden çok filtre olduğunda, kapsam varsayılan filtre yürütme sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="33648-194">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="33648-195">Genel filtreler kapsayan sınıf filtreleri, bu da kapsayan Yöntem filtreleri.</span><span class="sxs-lookup"><span data-stu-id="33648-195">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="33648-196">Filtre iç içe geçme sonucu *olarak, filtrenin kodu,* *önceki* kodun ters sırasına göre çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-196">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="33648-197">Filtre sırası:</span><span class="sxs-lookup"><span data-stu-id="33648-197">The filter sequence:</span></span>

* <span data-ttu-id="33648-198">Genel filtrelerin *önceki* kodu.</span><span class="sxs-lookup"><span data-stu-id="33648-198">The *before* code of global filters.</span></span>
  * <span data-ttu-id="33648-199">Denetleyicinin ve Razor sayfası filtrelerinin *öncesindeki* kodu.</span><span class="sxs-lookup"><span data-stu-id="33648-199">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="33648-200">Eylem yöntemi filtrelerinden *önceki* kod.</span><span class="sxs-lookup"><span data-stu-id="33648-200">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="33648-201">Eylem yöntemi filtrelerinden *sonraki* kod.</span><span class="sxs-lookup"><span data-stu-id="33648-201">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="33648-202">Denetleyicinin ve Razor sayfası filtrelerinin *sonraki* kodu.</span><span class="sxs-lookup"><span data-stu-id="33648-202">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="33648-203">Genel filtrelerin *sonraki* kodu.</span><span class="sxs-lookup"><span data-stu-id="33648-203">The *after* code of global filters.</span></span>
  
<span data-ttu-id="33648-204">Zaman uyumlu eylem filtreleri için filtre yöntemlerinin çağrıldığı sırayı gösteren aşağıdaki örnek.</span><span class="sxs-lookup"><span data-stu-id="33648-204">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="33648-205">Dizisi</span><span class="sxs-lookup"><span data-stu-id="33648-205">Sequence</span></span> | <span data-ttu-id="33648-206">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="33648-206">Filter scope</span></span> | <span data-ttu-id="33648-207">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="33648-207">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="33648-208">1\.</span><span class="sxs-lookup"><span data-stu-id="33648-208">1</span></span> | <span data-ttu-id="33648-209">Genel</span><span class="sxs-lookup"><span data-stu-id="33648-209">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="33648-210">2</span><span class="sxs-lookup"><span data-stu-id="33648-210">2</span></span> | <span data-ttu-id="33648-211">Denetleyici veya Razor sayfası</span><span class="sxs-lookup"><span data-stu-id="33648-211">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="33648-212">3</span><span class="sxs-lookup"><span data-stu-id="33648-212">3</span></span> | <span data-ttu-id="33648-213">Yöntem</span><span class="sxs-lookup"><span data-stu-id="33648-213">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="33648-214">4</span><span class="sxs-lookup"><span data-stu-id="33648-214">4</span></span> | <span data-ttu-id="33648-215">Yöntem</span><span class="sxs-lookup"><span data-stu-id="33648-215">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="33648-216">5</span><span class="sxs-lookup"><span data-stu-id="33648-216">5</span></span> | <span data-ttu-id="33648-217">Denetleyici veya Razor sayfası</span><span class="sxs-lookup"><span data-stu-id="33648-217">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="33648-218">6</span><span class="sxs-lookup"><span data-stu-id="33648-218">6</span></span> | <span data-ttu-id="33648-219">Genel</span><span class="sxs-lookup"><span data-stu-id="33648-219">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="33648-220">Denetleyici düzeyi filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-220">Controller level filters</span></span>

<span data-ttu-id="33648-221"><xref:Microsoft.AspNetCore.Mvc.Controller> temel sınıfından devralan her denetleyici [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)ve [controller. onactionyürütülmüş](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` yöntemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="33648-221">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="33648-222">Bu Yöntemler:</span><span class="sxs-lookup"><span data-stu-id="33648-222">These methods:</span></span>

* <span data-ttu-id="33648-223">Belirli bir eylem için çalışan filtreleri sarın.</span><span class="sxs-lookup"><span data-stu-id="33648-223">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="33648-224">`OnActionExecuting`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="33648-224">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="33648-225">`OnActionExecuted` tüm eylem filtrelerinden sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="33648-225">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="33648-226">`OnActionExecutionAsync`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="33648-226">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="33648-227">`next` sonra filtre içindeki kod eylem yönteminden sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-227">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="33648-228">Örneğin, indirme örneğinde `MySampleActionFilter`, başlangıçta genel olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="33648-228">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="33648-229">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="33648-229">The `TestController`:</span></span>

* <span data-ttu-id="33648-230">`FilterTest2` eyleme `SampleActionFilterAttribute` (`[SampleActionFilter]`) uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-230">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="33648-231">`OnActionExecuting` ve `OnActionExecuted`geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="33648-231">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="33648-232">`https://localhost:5001/Test2/FilterTest2` gitme aşağıdaki kodu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="33648-232">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="33648-233">Denetleyici düzeyi filtreleri [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) özelliğini `int.MinValue`olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="33648-233">Controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="33648-234">Denetleyici düzeyi filtreleri metotlara uygulandıktan sonra çalıştırılacak şekilde ayarlanamaz.</span><span class="sxs-lookup"><span data-stu-id="33648-234">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="33648-235">Sıra, sonraki bölümde açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="33648-235">Order is explained in the next section.</span></span>

<span data-ttu-id="33648-236">Razor Pages için bkz. [filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="33648-236">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="33648-237">Varsayılan sırayı geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="33648-237">Overriding the default order</span></span>

<span data-ttu-id="33648-238">Varsayılan yürütme sırası <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>uygulayarak geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-238">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="33648-239">`IOrderedFilter`, yürütme sırasını belirlemede kapsama göre öncelik alan <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="33648-239">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="33648-240">Daha düşük bir `Order` değerine sahip bir filtre:</span><span class="sxs-lookup"><span data-stu-id="33648-240">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="33648-241">Daha yüksek bir `Order`bir filtrenin *önüne kodundan önce* kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="33648-241">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="33648-242">Daha yüksek `Order` değerine sahip bir filtrenin *sonrasında koddan sonra* çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-242">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="33648-243">`Order` özelliği bir Oluşturucu parametresiyle ayarlanır:</span><span class="sxs-lookup"><span data-stu-id="33648-243">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="33648-244">Aşağıdaki denetleyicide iki eylem filtresini göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="33648-244">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="33648-245">`StartUp.ConfigureServices`genel bir filtre eklendi:</span><span class="sxs-lookup"><span data-stu-id="33648-245">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="33648-246">3 filtre aşağıdaki sırayla çalışır:</span><span class="sxs-lookup"><span data-stu-id="33648-246">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="33648-247">`Order` özelliği filtrelerin çalıştırıldığı sırayı belirlerken kapsamı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="33648-247">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="33648-248">Filtreler sırasıyla sıraya göre sıralanır, ardından kapsam bölmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33648-248">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="33648-249">Yerleşik filtrelerin tümü `IOrderedFilter` uygular ve varsayılan `Order` değerini 0 olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="33648-249">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="33648-250">Daha önce belirtildiği gibi, denetleyici düzeyi filtreleri [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) özelliğini yerleşik filtreler için `int.MinValue` olarak ayarlar, `Order` sıfır olmayan bir değere ayarlanmadığı sürece kapsam sıralamayı belirler.</span><span class="sxs-lookup"><span data-stu-id="33648-250">As mentioned previously, controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="33648-251">Yukarıdaki kodda, `MySampleActionFilter` denetleyici kapsamı bulunan `MyAction2FilterAttribute`önce çalışması için genel kapsama sahip olur.</span><span class="sxs-lookup"><span data-stu-id="33648-251">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="33648-252">`MyAction2FilterAttribute` önce çalıştırmak için sırayı `int.MinValue`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="33648-252">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="33648-253">Genel filtre `MySampleActionFilter` önce çalıştırmak için, `Order` `int.MinValue`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="33648-253">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="33648-254">İptal ve kısa devre dışı</span><span class="sxs-lookup"><span data-stu-id="33648-254">Cancellation and short-circuiting</span></span>

<span data-ttu-id="33648-255">Filtre işlem hattı, filtre yöntemine sunulan <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parametresindeki <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> özelliği ayarlanarak kısa devre dışı olabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-255">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="33648-256">Örneğin, aşağıdaki kaynak filtresi, ardışık düzenin geri kalanının yürütülmesini engeller:</span><span class="sxs-lookup"><span data-stu-id="33648-256">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="33648-257">Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` hem de `AddHeader` filtresi `SomeResource` Action metodunu hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="33648-257">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="33648-258">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="33648-258">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="33648-259">Önce bir kaynak filtresi olduğundan ve `AddHeader` bir eylem filtresi olduğundan, önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-259">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="33648-260">Kısa süreli işlem hattının geri kalanı.</span><span class="sxs-lookup"><span data-stu-id="33648-260">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="33648-261">Bu nedenle `AddHeader` filtresi `SomeResource` eylemi için hiçbir şekilde çalışmamayacaktır.</span><span class="sxs-lookup"><span data-stu-id="33648-261">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="33648-262">Her iki filtre de eylem yöntemi düzeyinde uygulanırsa, `ShortCircuitingResourceFilter` ilk kez çalıştırıldıysa bu davranış aynı olur.</span><span class="sxs-lookup"><span data-stu-id="33648-262">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="33648-263">`ShortCircuitingResourceFilter`, filtre türü veya `Order` özelliğinin açık kullanımı nedeniyle önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-263">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="33648-264">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="33648-264">Dependency injection</span></span>

<span data-ttu-id="33648-265">Filtreler türe veya örneğe göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-265">Filters can be added by type or by instance.</span></span> <span data-ttu-id="33648-266">Bir örnek eklenirse, bu örnek her istek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33648-266">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="33648-267">Bir tür eklenirse, türü etkinleştirilmiş olur.</span><span class="sxs-lookup"><span data-stu-id="33648-267">If a type is added, it's type-activated.</span></span> <span data-ttu-id="33648-268">Tür etkinleştirilmiş bir filtre şu anlama gelir:</span><span class="sxs-lookup"><span data-stu-id="33648-268">A type-activated filter means:</span></span>

* <span data-ttu-id="33648-269">Her istek için bir örnek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="33648-269">An instance is created for each request.</span></span>
* <span data-ttu-id="33648-270">Herhangi bir Oluşturucu bağımlılığı [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından doldurulur.</span><span class="sxs-lookup"><span data-stu-id="33648-270">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="33648-271">Öznitelik olarak uygulanan ve doğrudan denetleyici sınıflarına veya eylem yöntemlerine eklenen filtreler [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından sağlanmış Oluşturucu bağımlılıklarına sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="33648-271">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="33648-272">Oluşturucu bağımlılıkları şu nedenle şu nedenle sağlanamaz:</span><span class="sxs-lookup"><span data-stu-id="33648-272">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="33648-273">Özniteliklerin, uygulandıkları yerlerde, Oluşturucu parametreleri sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-273">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="33648-274">Bu, özniteliklerin nasıl çalıştığı konusunda bir kısıtlamadır.</span><span class="sxs-lookup"><span data-stu-id="33648-274">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="33648-275">Aşağıdaki filtreler, dı tarafından belirtilen Oluşturucu bağımlılıklarını destekler:</span><span class="sxs-lookup"><span data-stu-id="33648-275">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="33648-276"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> özniteliğe uygulandı.</span><span class="sxs-lookup"><span data-stu-id="33648-276"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="33648-277">Önceki filtreler bir denetleyiciye veya eylem yöntemine uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="33648-277">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="33648-278">Günlükçüler, DI 'den alınabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-278">Loggers are available from DI.</span></span> <span data-ttu-id="33648-279">Ancak, yalnızca günlüğe kaydetme amacıyla filtre oluşturmaktan ve kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="33648-279">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="33648-280">[Yerleşik çerçeve günlüğü](xref:fundamentals/logging/index) genellikle günlüğe kaydetme için gerekenleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-280">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="33648-281">Filtrelere günlük eklendi:</span><span class="sxs-lookup"><span data-stu-id="33648-281">Logging added to filters:</span></span>

* <span data-ttu-id="33648-282">, İş etki alanı kaygılarını veya filtreye özgü davranışları odaklamalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-282">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="33648-283">Eylemleri veya diğer çerçeve olaylarını günlüğe **içermemelidir** .</span><span class="sxs-lookup"><span data-stu-id="33648-283">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="33648-284">Yerleşik filtreler günlük eylemleri ve çerçeve olayları.</span><span class="sxs-lookup"><span data-stu-id="33648-284">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="33648-285">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="33648-285">ServiceFilterAttribute</span></span>

<span data-ttu-id="33648-286">Hizmet filtresi uygulama türleri `ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="33648-286">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="33648-287"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, bir filtrenin bir örneğini dı öğesinden alır.</span><span class="sxs-lookup"><span data-stu-id="33648-287">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="33648-288">Aşağıdaki kod `AddHeaderResultServiceFilter`gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-288">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="33648-289">Aşağıdaki kodda, `AddHeaderResultServiceFilter` dı kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="33648-289">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="33648-290">Aşağıdaki kodda, `ServiceFilter` özniteliği dı öğesinden `AddHeaderResultServiceFilter` filtrenin bir örneğini alır:</span><span class="sxs-lookup"><span data-stu-id="33648-290">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="33648-291">`ServiceFilterAttribute`kullanırken, [Servicefilterattribute. ısyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="33648-291">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="33648-292">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden *kullanılabilir olabileceğini gösteren* bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-292">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="33648-293">ASP.NET Core çalışma zamanı garanti etmez:</span><span class="sxs-lookup"><span data-stu-id="33648-293">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="33648-294">Filtrenin tek bir örneğinin oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="33648-294">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="33648-295">Filtre, sonraki bir noktada dı kapsayıcısından yeniden istenmeyecek.</span><span class="sxs-lookup"><span data-stu-id="33648-295">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="33648-296">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-296">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="33648-297"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-297"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="33648-298">`IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="33648-298">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="33648-299">`CreateInstance`, belirtilen türü DI 'dan yükler.</span><span class="sxs-lookup"><span data-stu-id="33648-299">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="33648-300">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="33648-300">TypeFilterAttribute</span></span>

<span data-ttu-id="33648-301"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>benzerdir, ancak türü doğrudan dı kapsayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="33648-301"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="33648-302"><xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>kullanarak türü başlatır.</span><span class="sxs-lookup"><span data-stu-id="33648-302">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="33648-303">`TypeFilterAttribute` türler doğrudan dı kapsayıcısından çözümlenmediğinden:</span><span class="sxs-lookup"><span data-stu-id="33648-303">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="33648-304">`TypeFilterAttribute` kullanılarak başvurulan türlerin dı kapsayıcısına kaydedilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="33648-304">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="33648-305">Bunların bağımlılıkları, dı kapsayıcısı tarafından yerine getirilir.</span><span class="sxs-lookup"><span data-stu-id="33648-305">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="33648-306">`TypeFilterAttribute`, isteğe bağlı olarak tür için Oluşturucu bağımsız değişkenlerini kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-306">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="33648-307">`TypeFilterAttribute`kullanırken [Typefilterattribute. ıyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="33648-307">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="33648-308">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden kullanılabilir *olabileceği* ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-308">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="33648-309">ASP.NET Core çalışma zamanı, filtrenin tek bir örneğinin oluşturulacağı garantisi vermez.</span><span class="sxs-lookup"><span data-stu-id="33648-309">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="33648-310">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-310">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="33648-311">Aşağıdaki örnek `TypeFilterAttribute`kullanarak bir türe bağımsız değişkenlerin nasıl geçirileceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-311">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="33648-312">Yetkilendirme filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-312">Authorization filters</span></span>

<span data-ttu-id="33648-313">Yetkilendirme filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-313">Authorization filters:</span></span>

* <span data-ttu-id="33648-314">, Filtre ardışık düzeninde ilk filtrelerin çalıştırılmasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-314">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="33648-315">Eylem yöntemlerine erişimi denetleyin.</span><span class="sxs-lookup"><span data-stu-id="33648-315">Control access to action methods.</span></span>
* <span data-ttu-id="33648-316">Bir Before yöntemi, ancak After yönteminden hiçbiri.</span><span class="sxs-lookup"><span data-stu-id="33648-316">Have a before method, but no after method.</span></span>

<span data-ttu-id="33648-317">Özel Yetkilendirme filtreleri özel bir yetkilendirme çerçevesi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="33648-317">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="33648-318">Özel bir filtre yazmak için yetkilendirme ilkelerini yapılandırmayı veya özel bir yetkilendirme ilkesi yazmayı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="33648-318">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="33648-319">Yerleşik yetkilendirme filtresi:</span><span class="sxs-lookup"><span data-stu-id="33648-319">The built-in authorization filter:</span></span>

* <span data-ttu-id="33648-320">Yetkilendirme sistemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="33648-320">Calls the authorization system.</span></span>
* <span data-ttu-id="33648-321">İstekleri yetkilendirmez.</span><span class="sxs-lookup"><span data-stu-id="33648-321">Does not authorize requests.</span></span>

<span data-ttu-id="33648-322">Yetkilendirme filtreleri içinde özel **durumlar atamayın:**</span><span class="sxs-lookup"><span data-stu-id="33648-322">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="33648-323">Özel durum işlenmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="33648-323">The exception will not be handled.</span></span>
* <span data-ttu-id="33648-324">Özel durum filtreleri özel durumu işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="33648-324">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="33648-325">Bir yetkilendirme filtresinde özel durum oluştuğunda bir sınama vermeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="33648-325">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="33648-326">[Yetkilendirme](xref:security/authorization/introduction)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="33648-326">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="33648-327">Kaynak filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-327">Resource filters</span></span>

<span data-ttu-id="33648-328">Kaynak filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-328">Resource filters:</span></span>

* <span data-ttu-id="33648-329"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-329">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="33648-330">Yürütme, filtre işlem hattının çoğunu sarmalar.</span><span class="sxs-lookup"><span data-stu-id="33648-330">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="33648-331">Kaynak filtrelerinden önce yalnızca [Yetkilendirme filtreleri](#authorization-filters) çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-331">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="33648-332">Kaynak filtreleri, işlem hattının büyük bir yanındaki kısa devre için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-332">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="33648-333">Örneğin, bir önbelleğe alma filtresi, bir önbellek isabetinden ardışık düzen geri kalanından kaçınabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-333">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="33648-334">Kaynak filtresi örnekleri:</span><span class="sxs-lookup"><span data-stu-id="33648-334">Resource filter examples:</span></span>

* <span data-ttu-id="33648-335">Daha önce gösterilen [kısa devre dışı kaynak filtresi](#short-circuiting-resource-filter) .</span><span class="sxs-lookup"><span data-stu-id="33648-335">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="33648-336">[Disableformvaluemodelbindingattribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="33648-336">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="33648-337">Model bağlamanın form verilerine erişimini engeller.</span><span class="sxs-lookup"><span data-stu-id="33648-337">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="33648-338">Form verilerinin belleğe okunmasını engellemek için büyük dosya yüklemeleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33648-338">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="33648-339">Eylem filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-339">Action filters</span></span>

<span data-ttu-id="33648-340">Eylem filtreleri Razor Pages **için uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="33648-340">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="33648-341">Razor Pages <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> destekler.</span><span class="sxs-lookup"><span data-stu-id="33648-341">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="33648-342">Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="33648-342">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="33648-343">Eylem filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-343">Action filters:</span></span>

* <span data-ttu-id="33648-344"><xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-344">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="33648-345">Yürütmesinin, eylem yöntemlerinin yürütülmesi çevreler.</span><span class="sxs-lookup"><span data-stu-id="33648-345">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="33648-346">Aşağıdaki kod bir örnek eylem filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-346">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="33648-347"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="33648-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="33648-348"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-girişlerin bir eylem yöntemine okunmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-348"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="33648-349"><xref:Microsoft.AspNetCore.Mvc.Controller>-denetleyici örneğinin işlenmesine izin vermez.</span><span class="sxs-lookup"><span data-stu-id="33648-349"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="33648-350"><xref:System.Web.Mvc.ActionExecutingContext.Result>-eylem yönteminin ve sonraki eylem filtrelerinin `Result` kısa devre dışı yürütmesi.</span><span class="sxs-lookup"><span data-stu-id="33648-350"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="33648-351">Eylem yönteminde özel durum oluşturma:</span><span class="sxs-lookup"><span data-stu-id="33648-351">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="33648-352">Sonraki filtrelerin çalıştırılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="33648-352">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="33648-353">`Result`ayarının aksine, başarılı bir sonuç yerine başarısızlık olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="33648-353">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="33648-354"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> `Controller` ve `Result` ek olarak aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="33648-354">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="33648-355"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-eylem yürütmesi başka bir filtre tarafından kabul edilse true.</span><span class="sxs-lookup"><span data-stu-id="33648-355"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="33648-356"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-eylem veya daha önce çalıştırılan eylem filtresi bir özel durum oluşturdu, null değil.</span><span class="sxs-lookup"><span data-stu-id="33648-356"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="33648-357">Bu özellik null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="33648-357">Setting this property to null:</span></span>

  * <span data-ttu-id="33648-358">Özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="33648-358">Effectively handles the exception.</span></span>
  * <span data-ttu-id="33648-359">`Result`, eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="33648-359">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="33648-360">Bir `IAsyncActionFilter`için <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>çağrısı:</span><span class="sxs-lookup"><span data-stu-id="33648-360">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="33648-361">Sonraki eylem filtrelerini ve eylem yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="33648-361">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="33648-362">`ActionExecutedContext` döndürür.</span><span class="sxs-lookup"><span data-stu-id="33648-362">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="33648-363">Kısa devre dışı, bir sonuç örneğine <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> atayın ve `next` çağırmayın (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="33648-363">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="33648-364">Framework, alt sınıflı olabilecek bir soyut <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-364">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="33648-365">`OnActionExecuting` eylem filtresi şu şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="33648-365">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="33648-366">Model durumunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-366">Validate model state.</span></span>
* <span data-ttu-id="33648-367">Durum geçersizse bir hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="33648-367">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="33648-368">`OnActionExecuted` yöntemi eylem yönteminden sonra çalışır:</span><span class="sxs-lookup"><span data-stu-id="33648-368">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="33648-369">Ve <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> özelliği aracılığıyla eylemin sonuçlarını görebilir ve değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-369">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="33648-370">eylem yürütmesi başka bir filtre tarafından kabul edilse, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> true olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-370"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="33648-371">eylem veya sonraki eylem filtresi bir özel durum harekete geçirdi, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-371"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="33648-372">`Exception` null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="33648-372">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="33648-373">Bir özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="33648-373">Effectively handles an exception.</span></span>
  * <span data-ttu-id="33648-374">`ActionExecutedContext.Result`, normal olarak eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="33648-374">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="33648-375">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-375">Exception filters</span></span>

<span data-ttu-id="33648-376">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-376">Exception filters:</span></span>

* <span data-ttu-id="33648-377"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>uygulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-377">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="33648-378">, Yaygın hata işleme ilkelerini uygulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-378">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="33648-379">Aşağıdaki örnek özel durum filtresi, uygulama geliştirmede olduğunda oluşan özel durumlar hakkındaki ayrıntıları görüntülemek için özel bir hata görünümü kullanır:</span><span class="sxs-lookup"><span data-stu-id="33648-379">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="33648-380">Aşağıdaki kod özel durum filtresini sınar:</span><span class="sxs-lookup"><span data-stu-id="33648-380">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="33648-381">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-381">Exception filters:</span></span>

* <span data-ttu-id="33648-382">Etkinlikden önceki ve sonraki olaylar yok.</span><span class="sxs-lookup"><span data-stu-id="33648-382">Don't have before and after events.</span></span>
* <span data-ttu-id="33648-383"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>uygulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-383">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="33648-384">Razor sayfası veya denetleyici oluşturma, [model bağlama](xref:mvc/models/model-binding), eylem filtreleri veya eylem yöntemlerinde oluşan işlenmemiş özel durumları işleyin.</span><span class="sxs-lookup"><span data-stu-id="33648-384">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="33648-385">Kaynak filtrelerinde, sonuç filtrelerinde veya MVC sonuç yürütülürken oluşan özel **durumları yakalamayın** .</span><span class="sxs-lookup"><span data-stu-id="33648-385">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="33648-386">Bir özel durumu işlemek için <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> özelliğini `true` veya bir yanıt yazın.</span><span class="sxs-lookup"><span data-stu-id="33648-386">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="33648-387">Bu, özel durumun yayılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="33648-387">This stops propagation of the exception.</span></span> <span data-ttu-id="33648-388">Özel durum filtresi bir özel durumu "başarılı" olarak açamaz.</span><span class="sxs-lookup"><span data-stu-id="33648-388">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="33648-389">Yalnızca bir eylem filtresi bunu yapabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-389">Only an action filter can do that.</span></span>

<span data-ttu-id="33648-390">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-390">Exception filters:</span></span>

* <span data-ttu-id="33648-391">Eylemler içinde oluşan özel durumları yakalamaya uygundur.</span><span class="sxs-lookup"><span data-stu-id="33648-391">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="33648-392">, Hata işleme ara yazılımı olarak esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="33648-392">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="33648-393">Özel durum işleme için ara yazılımı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="33648-393">Prefer middleware for exception handling.</span></span> <span data-ttu-id="33648-394">Yalnızca hata işlemenin hangi eylem yöntemine göre *farklılık* gösteren özel durum filtrelerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="33648-394">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="33648-395">Örneğin, bir uygulama hem API uç noktaları hem de görünümler/HTML için eylem yöntemlerine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-395">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="33648-396">API uç noktaları, hata bilgilerini JSON olarak döndürebilir, ancak görünüm tabanlı eylemler bir hata sayfasını HTML olarak döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-396">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="33648-397">Sonuç filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-397">Result filters</span></span>

<span data-ttu-id="33648-398">Sonuç filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-398">Result filters:</span></span>

* <span data-ttu-id="33648-399">Arabirim uygulama:</span><span class="sxs-lookup"><span data-stu-id="33648-399">Implement an interface:</span></span>
  * <span data-ttu-id="33648-400"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="33648-400"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="33648-401"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="33648-401"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="33648-402">Yürütmesi, eylem sonuçlarının yürütülmesini çevreler.</span><span class="sxs-lookup"><span data-stu-id="33648-402">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="33648-403">IResultFilter ve ıasyncresultfilter</span><span class="sxs-lookup"><span data-stu-id="33648-403">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="33648-404">Aşağıdaki kod, bir HTTP üst bilgisi ekleyen bir sonuç filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-404">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="33648-405">Yürütülen sonuç türü eyleme göre değişir.</span><span class="sxs-lookup"><span data-stu-id="33648-405">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="33648-406">Bir görünüm döndüren bir eylem, yürütülen <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir parçası olarak tüm Razor işlemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="33648-406">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="33648-407">Bir API yöntemi, sonucun yürütülmesinin bir parçası olarak bazı serileştirme işlemleri gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-407">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="33648-408">[Eylem sonuçları](xref:mvc/controllers/actions)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="33648-408">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="33648-409">Sonuç filtreleri yalnızca bir eylem veya eylem filtresi bir eylem sonucu üretirse yürütülür.</span><span class="sxs-lookup"><span data-stu-id="33648-409">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="33648-410">Şu durumlarda sonuç filtreleri yürütülmez:</span><span class="sxs-lookup"><span data-stu-id="33648-410">Result filters are not executed when:</span></span>

* <span data-ttu-id="33648-411">Bir yetkilendirme filtresi veya kaynak filtresi, işlem hattı için kısa süreli olarak devre dışı.</span><span class="sxs-lookup"><span data-stu-id="33648-411">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="33648-412">Bir özel durum filtresi, bir eylem sonucu üreterek özel durumu işler.</span><span class="sxs-lookup"><span data-stu-id="33648-412">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="33648-413"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> yöntemi, eylem sonucunun ve sonraki sonuç filtrelerinin `true`<xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> ayarlanarak kısa devre yürütülmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-413">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="33648-414">Boş bir yanıt oluşturmamaya kaçınmak için kısa devre dışı bırakıldığında yanıt nesnesine yazın.</span><span class="sxs-lookup"><span data-stu-id="33648-414">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="33648-415">`IResultFilter.OnResultExecuting`bir özel durum üretiliyor:</span><span class="sxs-lookup"><span data-stu-id="33648-415">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="33648-416">Eylem sonucunun ve sonraki filtrelerin yürütülmesini önler.</span><span class="sxs-lookup"><span data-stu-id="33648-416">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="33648-417">Başarılı bir sonuç yerine başarısızlık olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="33648-417">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="33648-418"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> yöntemi çalıştırıldığında, yanıt muhtemelen istemciye zaten gönderilmiştir.</span><span class="sxs-lookup"><span data-stu-id="33648-418">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="33648-419">Yanıt istemciye zaten gönderildiyse, bu değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="33648-419">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="33648-420">`ResultExecutedContext.Canceled`, eylem sonucu yürütmesi başka bir filtre tarafından kabul edilen kısa devre ise `true` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-420">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="33648-421">eylem sonucu veya sonraki sonuç filtresi bir özel durum harekete geçirdi, `ResultExecutedContext.Exception` null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-421">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="33648-422">`Exception` null olarak ayarlanması, bir özel durumu etkili bir şekilde işler ve özel durumun daha sonra işlem hattının daha sonra oluşturulmasını önler.</span><span class="sxs-lookup"><span data-stu-id="33648-422">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="33648-423">Bir sonuç filtresinde özel durum işlenirken yanıta veri yazmanın güvenilir bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="33648-423">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="33648-424">Bir eylem sonucu bir özel durum oluşturduğunda üstbilgiler istemciye temizleniyorsa, hata kodu göndermek için güvenilir bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="33648-424">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="33648-425">Bir <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>için, <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> bir `await next` çağrısı, sonraki sonuç filtrelerini ve eylem sonucunu yürütür.</span><span class="sxs-lookup"><span data-stu-id="33648-425">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="33648-426">Kısa devre dışı, [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) `true` ve `ResultExecutionDelegate`çağırmayın:</span><span class="sxs-lookup"><span data-stu-id="33648-426">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="33648-427">Framework, alt sınıflı olabilecek bir soyut `ResultFilterAttribute` sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-427">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="33648-428">Daha önce gösterilen [Addheaderattribute](#add-header-attribute) sınıfı bir sonuç Filtresi özniteliği örneğidir.</span><span class="sxs-lookup"><span data-stu-id="33648-428">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="33648-429">Ialwaysrunresultfilter ve ıasyncalwaysrunresultfilter</span><span class="sxs-lookup"><span data-stu-id="33648-429">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="33648-430"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> arabirimleri, tüm eylem sonuçları için çalışan bir <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> uygulamasını bildirir.</span><span class="sxs-lookup"><span data-stu-id="33648-430">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="33648-431">Bu, tarafından oluşturulan eylem sonuçlarını içerir:</span><span class="sxs-lookup"><span data-stu-id="33648-431">This includes action results produced by:</span></span>

* <span data-ttu-id="33648-432">Kısa devre olan Yetkilendirme filtreleri ve kaynak filtreleri.</span><span class="sxs-lookup"><span data-stu-id="33648-432">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="33648-433">Özel durum filtreleri.</span><span class="sxs-lookup"><span data-stu-id="33648-433">Exception filters.</span></span>

<span data-ttu-id="33648-434">Örneğin, aşağıdaki filtre her zaman çalışır ve içerik anlaşması başarısız olduğunda *422 olmayan bir varlık* durum kodu ile bir eylem sonucu (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) ayarlar:</span><span class="sxs-lookup"><span data-stu-id="33648-434">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="33648-435">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="33648-435">IFilterFactory</span></span>

<span data-ttu-id="33648-436"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-436"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="33648-437">Bu nedenle, bir `IFilterFactory` örneği, filtre ardışık düzeninde herhangi bir yerde `IFilterMetadata` bir örnek olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-437">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="33648-438">Çalışma zamanı filtreyi çağırmayı hazırlarken, bir `IFilterFactory`dönüştürmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="33648-438">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="33648-439">Bu atama başarılı olursa, çağrılan `IFilterMetadata` örneğini oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="33648-439">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="33648-440">Bu, tam filtre işlem hattının uygulama başladığında açıkça ayarlanması gerektiğinden esnek bir tasarım sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-440">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="33648-441">`IFilterFactory`, filtre oluşturmaya yönelik başka bir yaklaşım olarak özel öznitelik uygulamaları kullanılarak uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="33648-441">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="33648-442">Filtre aşağıdaki kodda uygulanır:</span><span class="sxs-lookup"><span data-stu-id="33648-442">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="33648-443">Önceki kodu [indirme örneğini](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample)çalıştırarak test edin:</span><span class="sxs-lookup"><span data-stu-id="33648-443">Test the preceding code by running the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="33648-444">F12 geliştirici araçlarını çağırın.</span><span class="sxs-lookup"><span data-stu-id="33648-444">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="33648-445">`https://localhost:5001/Sample/HeaderWithFactory` sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="33648-445">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="33648-446">F12 geliştirici araçları, örnek kod tarafından eklenen aşağıdaki yanıt üstbilgilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="33648-446">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="33648-447">**Yazar:** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="33648-447">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="33648-448">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="33648-448">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="33648-449">**iç:** `My header`</span><span class="sxs-lookup"><span data-stu-id="33648-449">**internal:** `My header`</span></span>

<span data-ttu-id="33648-450">Yukarıdaki kod, **iç:** `My header` yanıt üst bilgisini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="33648-450">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="33648-451">Öznitelik üzerinde IFilterFactory uygulandı</span><span class="sxs-lookup"><span data-stu-id="33648-451">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="33648-452">`IFilterFactory` uygulayan filtreler şu filtreler için yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="33648-452">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="33648-453">Parametre geçirme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="33648-453">Don't require passing parameters.</span></span>
* <span data-ttu-id="33648-454">DI tarafından doldurulması gereken Oluşturucu bağımlılıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="33648-454">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="33648-455"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-455"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="33648-456">`IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="33648-456">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="33648-457">`CreateInstance`, belirtilen türü hizmetler kapsayıcısından (DI) yükler.</span><span class="sxs-lookup"><span data-stu-id="33648-457">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="33648-458">Aşağıdaki kodda `[SampleActionFilter]`uygulamak için üç yaklaşım gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="33648-458">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="33648-459">Yukarıdaki kodda, yöntemi `[SampleActionFilter]` olarak dekorasyon, `SampleActionFilter`uygulamak için tercih edilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="33648-459">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="33648-460">Filtre ardışık düzeninde ara yazılım kullanma</span><span class="sxs-lookup"><span data-stu-id="33648-460">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="33648-461">Kaynak filtreleri, işlem hattında daha sonra gelen her şeyin yürütülmesini çevreleyecek olan [Ara yazılım](xref:fundamentals/middleware/index) gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-461">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="33648-462">Ancak filtreler, çalışma zamanının bir parçası oldukları ve bu sayede bağlam ve yapılara erişimi olan ara yazılımlar farklıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-462">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="33648-463">Ara yazılımı bir filtre olarak kullanmak için, filtre ardışık düzenine eklenecek olan ara yazılımı belirten `Configure` yöntemi ile bir tür oluşturun.</span><span class="sxs-lookup"><span data-stu-id="33648-463">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="33648-464">Aşağıdaki örnek, bir istek için geçerli kültürü oluşturmak üzere yerelleştirme ara yazılımını kullanır:</span><span class="sxs-lookup"><span data-stu-id="33648-464">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="33648-465">Ara yazılımı çalıştırmak için <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> kullanın:</span><span class="sxs-lookup"><span data-stu-id="33648-465">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="33648-466">Ara yazılım filtreleri, filtre işlem hattının aynı aşamasında, model bağlamadan önce ve işlem hattının geri kalanı ile kaynak filtreleri olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-466">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="33648-467">Sonraki eylemler</span><span class="sxs-lookup"><span data-stu-id="33648-467">Next actions</span></span>

* <span data-ttu-id="33648-468">[Razor Pages Için filtre yöntemlerine](xref:razor-pages/filter)bakın.</span><span class="sxs-lookup"><span data-stu-id="33648-468">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="33648-469">Filtrelerle denemek için [GitHub örneğini indirin, test edin ve değiştirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span><span class="sxs-lookup"><span data-stu-id="33648-469">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="33648-470">[Kirk Larkabağı](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/)ve [Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="33648-470">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="33648-471">ASP.NET Core *Filtreler* , istek işleme ardışık düzeninde belirli aşamalardan önce veya sonra kod çalıştırılmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="33648-471">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="33648-472">Yerleşik Filtreler şunları gibi görevleri işler:</span><span class="sxs-lookup"><span data-stu-id="33648-472">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="33648-473">Yetkilendirme (kullanıcının yetkili olmadığı kaynaklara erişimi önler).</span><span class="sxs-lookup"><span data-stu-id="33648-473">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="33648-474">Yanıt önbelleğe alma (istek ardışık düzenini önbelleğe alınmış bir yanıt döndürecek şekilde döndürür).</span><span class="sxs-lookup"><span data-stu-id="33648-474">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="33648-475">Çapraz kesme sorunlarını işlemek için özel filtreler oluşturulabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-475">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="33648-476">Çapraz kesme sorunlarına örnek olarak hata işleme, önbelleğe alma, yapılandırma, yetkilendirme ve günlüğe kaydetme dahildir.</span><span class="sxs-lookup"><span data-stu-id="33648-476">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="33648-477">Filtreler kodu çoğaltmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="33648-477">Filters avoid duplicating code.</span></span> <span data-ttu-id="33648-478">Örneğin, bir hata işleme özel durum filtresi hata işlemeyi birleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-478">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="33648-479">Bu belge, görünümler içeren Razor Pages, API denetleyicileri ve denetleyiciler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="33648-479">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="33648-480">Örneği ([indirme](xref:index#how-to-download-a-sample)) [görüntüleyin veya indirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) .</span><span class="sxs-lookup"><span data-stu-id="33648-480">[View or download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="33648-481">Filtreler nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="33648-481">How filters work</span></span>

<span data-ttu-id="33648-482">Filtreler, bazen *Filtre işlem hattı*olarak da adlandırılan *ASP.NET Core eylemi çağırma işlem hattı*içinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-482">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="33648-483">Filtre işlem hattı çalıştırılacak eylemi ASP.NET Core seçtikten sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-483">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![İstek diğer ara yazılım, yönlendirme ara yazılımı, eylem seçimi ve ASP.NET Core eylemi çağırma Işlem hattı aracılığıyla işlenir.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="33648-486">Filtre türleri</span><span class="sxs-lookup"><span data-stu-id="33648-486">Filter types</span></span>

<span data-ttu-id="33648-487">Her filtre türü, filtre ardışık düzeninde farklı bir aşamada yürütülür:</span><span class="sxs-lookup"><span data-stu-id="33648-487">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="33648-488">İlk olarak [Yetkilendirme filtreleri](#authorization-filters) çalışır ve kullanıcının istek için yetkilendirilip yetkilendirilmediğini tespit etmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33648-488">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="33648-489">Yetkilendirme filtreleri, istek yetkisiz ise işlem hattı kısa devre dışı.</span><span class="sxs-lookup"><span data-stu-id="33648-489">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="33648-490">[Kaynak filtreleri](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="33648-490">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="33648-491">Yetkilendirmeden sonra çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="33648-491">Run after authorization.</span></span>  
  * <span data-ttu-id="33648-492"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*>, filtre işlem hattının geri kalanının önüne kod çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-492"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="33648-493">Örneğin `OnResourceExecuting`, model bağlamadan önce kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-493">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="33648-494"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*>, ardışık düzenin geri kalanı tamamlandıktan sonra kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-494"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="33648-495">[Eylem filtreleri](#action-filters) , tek bir eylem yöntemi çağrıldıktan hemen önce ve sonra kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-495">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="33648-496">Bir eyleme geçirilen bağımsız değişkenleri ve eylemden döndürülen sonucu işlemek için kullanılabilirler.</span><span class="sxs-lookup"><span data-stu-id="33648-496">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="33648-497">Eylem filtreleri Razor Pages **desteklenmez.**</span><span class="sxs-lookup"><span data-stu-id="33648-497">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="33648-498">[Özel durum filtreleri](#exception-filters) , yanıt gövdesine hiçbir şey yazılmadan önce oluşan işlenmemiş özel durumlara genel ilkeler uygulamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33648-498">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="33648-499">[Sonuç filtreleri](#result-filters) , tek tek eylem sonuçlarının yürütülmesinden hemen önce ve sonra kodu çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-499">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="33648-500">Bunlar yalnızca eylem yöntemi başarıyla yürütüldüğünde çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-500">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="33648-501">Bu değerler, görünüm veya biçimlendirici yürütmesinin yürütülmesi gereken mantık için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-501">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="33648-502">Aşağıdaki diyagramda filtre türlerinin filtre ardışık düzeninde nasıl etkileşimde bulunduğu gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="33648-502">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![İstek, Yetkilendirme filtreleri, kaynak filtreleri, model bağlama, eylem filtreleri, eylem yürütme ve eylem sonucu dönüştürme, özel durum filtreleri, sonuç filtreleri ve sonuç yürütmesi aracılığıyla işlenir.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="33648-505">Uygulama</span><span class="sxs-lookup"><span data-stu-id="33648-505">Implementation</span></span>

<span data-ttu-id="33648-506">Filtreler, farklı arabirim tanımları aracılığıyla hem zaman uyumlu hem de zaman uyumsuz uygulamaları destekler.</span><span class="sxs-lookup"><span data-stu-id="33648-506">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="33648-507">Zaman uyumlu filtreler, ardışık düzen aşamasından önce (`On-Stage-Executing`) ve sonra kodu çalıştırabilir (`On-Stage-Executed`).</span><span class="sxs-lookup"><span data-stu-id="33648-507">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="33648-508">Örneğin, `OnActionExecuting` eylem yöntemi çağrılmadan önce çağrılır.</span><span class="sxs-lookup"><span data-stu-id="33648-508">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="33648-509">`OnActionExecuted`, eylem yöntemi döndüğünde çağrılır.</span><span class="sxs-lookup"><span data-stu-id="33648-509">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="33648-510">Zaman uyumsuz filtreler bir `On-Stage-ExecutionAsync` yöntemi tanımlar:</span><span class="sxs-lookup"><span data-stu-id="33648-510">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="33648-511">Yukarıdaki kodda `SampleAsyncActionFilter`, eylem yöntemini yürüten bir <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="33648-511">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="33648-512">`On-Stage-ExecutionAsync` yöntemlerinin her biri, filtrenin ardışık düzen aşamasını yürüten bir `FilterType-ExecutionDelegate` alır.</span><span class="sxs-lookup"><span data-stu-id="33648-512">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="33648-513">Birden çok filtre aşaması</span><span class="sxs-lookup"><span data-stu-id="33648-513">Multiple filter stages</span></span>

<span data-ttu-id="33648-514">Birden çok filtre aşaması için arabirimler tek bir sınıfta uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-514">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="33648-515">Örneğin, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sınıfı `IActionFilter`, `IResultFilter`ve zaman uyumsuz eşdeğerlerini uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-515">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="33648-516">Her ikisini de **değil** , bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="33648-516">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="33648-517">Çalışma zamanı öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="33648-517">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="33648-518">Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır.</span><span class="sxs-lookup"><span data-stu-id="33648-518">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="33648-519">Tek bir sınıfta hem zaman uyumsuz hem de zaman uyumlu arabirimler uygulanmışsa, yalnızca zaman uyumsuz yöntem çağrılır.</span><span class="sxs-lookup"><span data-stu-id="33648-519">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="33648-520"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> gibi soyut sınıflar kullanılırken, yalnızca zaman uyumlu yöntemleri veya her bir filtre türü için zaman uyumsuz yöntemi geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="33648-520">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="33648-521">Yerleşik filtre öznitelikleri</span><span class="sxs-lookup"><span data-stu-id="33648-521">Built-in filter attributes</span></span>

<span data-ttu-id="33648-522">ASP.NET Core, alt sınıflanmış ve özelleştirilebilen yerleşik öznitelik tabanlı filtreler içerir.</span><span class="sxs-lookup"><span data-stu-id="33648-522">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="33648-523">Örneğin, aşağıdaki sonuç filtresi yanıta bir üst bilgi ekler:</span><span class="sxs-lookup"><span data-stu-id="33648-523">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="33648-524">Öznitelikler, önceki örnekte gösterildiği gibi, filtrelerin bağımsız değişkenleri kabul etmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="33648-524">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="33648-525">`AddHeaderAttribute` bir denetleyiciye veya eylem yöntemine uygulayın ve HTTP üstbilgisinin adını ve değerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="33648-525">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="33648-526">Filtre arabirimlerinden birkaçı, özel uygulamalar için temel sınıflar olarak kullanılabilecek karşılık gelen özniteliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="33648-526">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="33648-527">Filtre öznitelikleri:</span><span class="sxs-lookup"><span data-stu-id="33648-527">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="33648-528">Kapsamları ve yürütme sırasını filtrele</span><span class="sxs-lookup"><span data-stu-id="33648-528">Filter scopes and order of execution</span></span>

<span data-ttu-id="33648-529">Üç *kapsamından*birindeki işlem hattına bir filtre eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="33648-529">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="33648-530">Bir eylem üzerinde bir öznitelik kullanma.</span><span class="sxs-lookup"><span data-stu-id="33648-530">Using an attribute on an action.</span></span>
* <span data-ttu-id="33648-531">Bir denetleyicide bir özniteliği kullanma.</span><span class="sxs-lookup"><span data-stu-id="33648-531">Using an attribute on a controller.</span></span>
* <span data-ttu-id="33648-532">Aşağıdaki kodda gösterildiği gibi tüm denetleyiciler ve eylemler için genel olarak:</span><span class="sxs-lookup"><span data-stu-id="33648-532">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="33648-533">Yukarıdaki kod, [Mvcoptions. Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) koleksiyonunu kullanarak genel olarak üç filtre ekler.</span><span class="sxs-lookup"><span data-stu-id="33648-533">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="33648-534">Varsayılan yürütme sırası</span><span class="sxs-lookup"><span data-stu-id="33648-534">Default order of execution</span></span>

<span data-ttu-id="33648-535">*Aynı türde*birden çok filtre olduğunda kapsam, filtre yürütmenin varsayılan sırasını belirler.</span><span class="sxs-lookup"><span data-stu-id="33648-535">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="33648-536">Genel filtreler saran sınıf filtreleri.</span><span class="sxs-lookup"><span data-stu-id="33648-536">Global filters surround class filters.</span></span> <span data-ttu-id="33648-537">Sınıf filtreleri surround yöntemi filtreleri.</span><span class="sxs-lookup"><span data-stu-id="33648-537">Class filters surround method filters.</span></span>

<span data-ttu-id="33648-538">Filtre iç içe geçme sonucu *olarak, filtrenin kodu,* *önceki* kodun ters sırasına göre çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-538">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="33648-539">Filtre sırası:</span><span class="sxs-lookup"><span data-stu-id="33648-539">The filter sequence:</span></span>

* <span data-ttu-id="33648-540">Genel filtrelerin *önceki* kodu.</span><span class="sxs-lookup"><span data-stu-id="33648-540">The *before* code of global filters.</span></span>
  * <span data-ttu-id="33648-541">Denetleyici filtrelerinden *önceki* kod.</span><span class="sxs-lookup"><span data-stu-id="33648-541">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="33648-542">Eylem yöntemi filtrelerinden *önceki* kod.</span><span class="sxs-lookup"><span data-stu-id="33648-542">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="33648-543">Eylem yöntemi filtrelerinden *sonraki* kod.</span><span class="sxs-lookup"><span data-stu-id="33648-543">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="33648-544">Denetleyici filtrelerinden *sonraki* kod.</span><span class="sxs-lookup"><span data-stu-id="33648-544">The *after* code of controller filters.</span></span>
* <span data-ttu-id="33648-545">Genel filtrelerin *sonraki* kodu.</span><span class="sxs-lookup"><span data-stu-id="33648-545">The *after* code of global filters.</span></span>
  
<span data-ttu-id="33648-546">Zaman uyumlu eylem filtreleri için filtre yöntemlerinin çağrıldığı sırayı gösteren aşağıdaki örnek.</span><span class="sxs-lookup"><span data-stu-id="33648-546">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="33648-547">Dizisi</span><span class="sxs-lookup"><span data-stu-id="33648-547">Sequence</span></span> | <span data-ttu-id="33648-548">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="33648-548">Filter scope</span></span> | <span data-ttu-id="33648-549">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="33648-549">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="33648-550">1\.</span><span class="sxs-lookup"><span data-stu-id="33648-550">1</span></span> | <span data-ttu-id="33648-551">Genel</span><span class="sxs-lookup"><span data-stu-id="33648-551">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="33648-552">2</span><span class="sxs-lookup"><span data-stu-id="33648-552">2</span></span> | <span data-ttu-id="33648-553">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="33648-553">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="33648-554">3</span><span class="sxs-lookup"><span data-stu-id="33648-554">3</span></span> | <span data-ttu-id="33648-555">Yöntem</span><span class="sxs-lookup"><span data-stu-id="33648-555">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="33648-556">4</span><span class="sxs-lookup"><span data-stu-id="33648-556">4</span></span> | <span data-ttu-id="33648-557">Yöntem</span><span class="sxs-lookup"><span data-stu-id="33648-557">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="33648-558">5</span><span class="sxs-lookup"><span data-stu-id="33648-558">5</span></span> | <span data-ttu-id="33648-559">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="33648-559">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="33648-560">6</span><span class="sxs-lookup"><span data-stu-id="33648-560">6</span></span> | <span data-ttu-id="33648-561">Genel</span><span class="sxs-lookup"><span data-stu-id="33648-561">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="33648-562">Bu sıra şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-562">This sequence shows:</span></span>

* <span data-ttu-id="33648-563">Yöntem filtresi, denetleyici filtresi içinde iç içe geçmiş.</span><span class="sxs-lookup"><span data-stu-id="33648-563">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="33648-564">Denetleyici filtresi, genel filtrenin içinde iç içe geçmiş.</span><span class="sxs-lookup"><span data-stu-id="33648-564">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="33648-565">Denetleyici ve Razor sayfa düzeyi filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-565">Controller and Razor Page level filters</span></span>

<span data-ttu-id="33648-566"><xref:Microsoft.AspNetCore.Mvc.Controller> temel sınıfından devralan her denetleyici [Controller. OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller. OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*)ve [controller. onactionyürütülmüş](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` yöntemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="33648-566">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="33648-567">Bu Yöntemler:</span><span class="sxs-lookup"><span data-stu-id="33648-567">These methods:</span></span>

* <span data-ttu-id="33648-568">Belirli bir eylem için çalışan filtreleri sarın.</span><span class="sxs-lookup"><span data-stu-id="33648-568">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="33648-569">`OnActionExecuting`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="33648-569">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="33648-570">`OnActionExecuted` tüm eylem filtrelerinden sonra çağrılır.</span><span class="sxs-lookup"><span data-stu-id="33648-570">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="33648-571">`OnActionExecutionAsync`, eylemin filtrelerinden herhangi birinin önüne çağırılır.</span><span class="sxs-lookup"><span data-stu-id="33648-571">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="33648-572">`next` sonra filtre içindeki kod eylem yönteminden sonra çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-572">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="33648-573">Örneğin, indirme örneğinde `MySampleActionFilter`, başlangıçta genel olarak uygulanır.</span><span class="sxs-lookup"><span data-stu-id="33648-573">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="33648-574">`TestController`:</span><span class="sxs-lookup"><span data-stu-id="33648-574">The `TestController`:</span></span>

* <span data-ttu-id="33648-575">`FilterTest2` eyleme `SampleActionFilterAttribute` (`[SampleActionFilter]`) uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-575">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="33648-576">`OnActionExecuting` ve `OnActionExecuted`geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="33648-576">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="33648-577">`https://localhost:5001/Test/FilterTest2` gitme aşağıdaki kodu çalıştırır:</span><span class="sxs-lookup"><span data-stu-id="33648-577">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="33648-578">Razor Pages için bkz. [filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="33648-578">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="33648-579">Varsayılan sırayı geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="33648-579">Overriding the default order</span></span>

<span data-ttu-id="33648-580">Varsayılan yürütme sırası <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>uygulayarak geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-580">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="33648-581">`IOrderedFilter`, yürütme sırasını belirlemede kapsama göre öncelik alan <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> özelliğini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="33648-581">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="33648-582">Daha düşük bir `Order` değerine sahip bir filtre:</span><span class="sxs-lookup"><span data-stu-id="33648-582">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="33648-583">Daha yüksek bir `Order`bir filtrenin *önüne kodundan önce* kodu çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="33648-583">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="33648-584">Daha yüksek `Order` değerine sahip bir filtrenin *sonrasında koddan sonra* çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-584">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="33648-585">`Order` özelliği bir Oluşturucu parametresiyle ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="33648-585">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="33648-586">Yukarıdaki örnekte gösterilen 3 eylem filtresini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="33648-586">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="33648-587">Denetleyicinin ve genel filtrelerin `Order` özelliği sırasıyla 1 ve 2 ' ye ayarlandıysa, yürütme sırası tersine çevrilir.</span><span class="sxs-lookup"><span data-stu-id="33648-587">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="33648-588">Dizisi</span><span class="sxs-lookup"><span data-stu-id="33648-588">Sequence</span></span> | <span data-ttu-id="33648-589">Filtre kapsamı</span><span class="sxs-lookup"><span data-stu-id="33648-589">Filter scope</span></span> | <span data-ttu-id="33648-590">`Order` özelliği</span><span class="sxs-lookup"><span data-stu-id="33648-590">`Order` property</span></span> | <span data-ttu-id="33648-591">Filter yöntemi</span><span class="sxs-lookup"><span data-stu-id="33648-591">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="33648-592">1\.</span><span class="sxs-lookup"><span data-stu-id="33648-592">1</span></span> | <span data-ttu-id="33648-593">Yöntem</span><span class="sxs-lookup"><span data-stu-id="33648-593">Method</span></span> | <span data-ttu-id="33648-594">0</span><span class="sxs-lookup"><span data-stu-id="33648-594">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="33648-595">2</span><span class="sxs-lookup"><span data-stu-id="33648-595">2</span></span> | <span data-ttu-id="33648-596">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="33648-596">Controller</span></span> | <span data-ttu-id="33648-597">1\.</span><span class="sxs-lookup"><span data-stu-id="33648-597">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="33648-598">3</span><span class="sxs-lookup"><span data-stu-id="33648-598">3</span></span> | <span data-ttu-id="33648-599">Genel</span><span class="sxs-lookup"><span data-stu-id="33648-599">Global</span></span> | <span data-ttu-id="33648-600">2</span><span class="sxs-lookup"><span data-stu-id="33648-600">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="33648-601">4</span><span class="sxs-lookup"><span data-stu-id="33648-601">4</span></span> | <span data-ttu-id="33648-602">Genel</span><span class="sxs-lookup"><span data-stu-id="33648-602">Global</span></span> | <span data-ttu-id="33648-603">2</span><span class="sxs-lookup"><span data-stu-id="33648-603">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="33648-604">5</span><span class="sxs-lookup"><span data-stu-id="33648-604">5</span></span> | <span data-ttu-id="33648-605">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="33648-605">Controller</span></span> | <span data-ttu-id="33648-606">1\.</span><span class="sxs-lookup"><span data-stu-id="33648-606">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="33648-607">6</span><span class="sxs-lookup"><span data-stu-id="33648-607">6</span></span> | <span data-ttu-id="33648-608">Yöntem</span><span class="sxs-lookup"><span data-stu-id="33648-608">Method</span></span> | <span data-ttu-id="33648-609">0</span><span class="sxs-lookup"><span data-stu-id="33648-609">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="33648-610">`Order` özelliği filtrelerin çalıştırıldığı sırayı belirlerken kapsamı geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="33648-610">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="33648-611">Filtreler sırasıyla sıraya göre sıralanır, ardından kapsam bölmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33648-611">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="33648-612">Yerleşik filtrelerin tümü `IOrderedFilter` uygular ve varsayılan `Order` değerini 0 olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="33648-612">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="33648-613">Yerleşik filtreler için, `Order` sıfır olmayan bir değere ayarlanmadığı takdirde kapsam sıralamayı belirler.</span><span class="sxs-lookup"><span data-stu-id="33648-613">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="33648-614">İptal ve kısa devre dışı</span><span class="sxs-lookup"><span data-stu-id="33648-614">Cancellation and short-circuiting</span></span>

<span data-ttu-id="33648-615">Filtre işlem hattı, filtre yöntemine sunulan <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parametresindeki <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> özelliği ayarlanarak kısa devre dışı olabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-615">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="33648-616">Örneğin, aşağıdaki kaynak filtresi, ardışık düzenin geri kalanının yürütülmesini engeller:</span><span class="sxs-lookup"><span data-stu-id="33648-616">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="33648-617">Aşağıdaki kodda, hem `ShortCircuitingResourceFilter` hem de `AddHeader` filtresi `SomeResource` Action metodunu hedefleyin.</span><span class="sxs-lookup"><span data-stu-id="33648-617">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="33648-618">`ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="33648-618">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="33648-619">Önce bir kaynak filtresi olduğundan ve `AddHeader` bir eylem filtresi olduğundan, önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-619">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="33648-620">Kısa süreli işlem hattının geri kalanı.</span><span class="sxs-lookup"><span data-stu-id="33648-620">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="33648-621">Bu nedenle `AddHeader` filtresi `SomeResource` eylemi için hiçbir şekilde çalışmamayacaktır.</span><span class="sxs-lookup"><span data-stu-id="33648-621">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="33648-622">Her iki filtre de eylem yöntemi düzeyinde uygulanırsa, `ShortCircuitingResourceFilter` ilk kez çalıştırıldıysa bu davranış aynı olur.</span><span class="sxs-lookup"><span data-stu-id="33648-622">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="33648-623">`ShortCircuitingResourceFilter`, filtre türü veya `Order` özelliğinin açık kullanımı nedeniyle önce çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-623">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="33648-624">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="33648-624">Dependency injection</span></span>

<span data-ttu-id="33648-625">Filtreler türe veya örneğe göre eklenebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-625">Filters can be added by type or by instance.</span></span> <span data-ttu-id="33648-626">Bir örnek eklenirse, bu örnek her istek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33648-626">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="33648-627">Bir tür eklenirse, türü etkinleştirilmiş olur.</span><span class="sxs-lookup"><span data-stu-id="33648-627">If a type is added, it's type-activated.</span></span> <span data-ttu-id="33648-628">Tür etkinleştirilmiş bir filtre şu anlama gelir:</span><span class="sxs-lookup"><span data-stu-id="33648-628">A type-activated filter means:</span></span>

* <span data-ttu-id="33648-629">Her istek için bir örnek oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="33648-629">An instance is created for each request.</span></span>
* <span data-ttu-id="33648-630">Herhangi bir Oluşturucu bağımlılığı [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından doldurulur.</span><span class="sxs-lookup"><span data-stu-id="33648-630">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="33648-631">Öznitelik olarak uygulanan ve doğrudan denetleyici sınıflarına veya eylem yöntemlerine eklenen filtreler [bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından sağlanmış Oluşturucu bağımlılıklarına sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="33648-631">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="33648-632">Oluşturucu bağımlılıkları şu nedenle şu nedenle sağlanamaz:</span><span class="sxs-lookup"><span data-stu-id="33648-632">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="33648-633">Özniteliklerin, uygulandıkları yerlerde, Oluşturucu parametreleri sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-633">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="33648-634">Bu, özniteliklerin nasıl çalıştığı konusunda bir kısıtlamadır.</span><span class="sxs-lookup"><span data-stu-id="33648-634">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="33648-635">Aşağıdaki filtreler, dı tarafından belirtilen Oluşturucu bağımlılıklarını destekler:</span><span class="sxs-lookup"><span data-stu-id="33648-635">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="33648-636"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> özniteliğe uygulandı.</span><span class="sxs-lookup"><span data-stu-id="33648-636"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="33648-637">Önceki filtreler bir denetleyiciye veya eylem yöntemine uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="33648-637">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="33648-638">Günlükçüler, DI 'den alınabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-638">Loggers are available from DI.</span></span> <span data-ttu-id="33648-639">Ancak, yalnızca günlüğe kaydetme amacıyla filtre oluşturmaktan ve kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="33648-639">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="33648-640">[Yerleşik çerçeve günlüğü](xref:fundamentals/logging/index) genellikle günlüğe kaydetme için gerekenleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-640">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="33648-641">Filtrelere günlük eklendi:</span><span class="sxs-lookup"><span data-stu-id="33648-641">Logging added to filters:</span></span>

* <span data-ttu-id="33648-642">, İş etki alanı kaygılarını veya filtreye özgü davranışları odaklamalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-642">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="33648-643">Eylemleri veya diğer çerçeve olaylarını günlüğe **içermemelidir** .</span><span class="sxs-lookup"><span data-stu-id="33648-643">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="33648-644">Yerleşik filtreler günlük eylemleri ve çerçeve olayları.</span><span class="sxs-lookup"><span data-stu-id="33648-644">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="33648-645">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="33648-645">ServiceFilterAttribute</span></span>

<span data-ttu-id="33648-646">Hizmet filtresi uygulama türleri `ConfigureServices`kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="33648-646">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="33648-647"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, bir filtrenin bir örneğini dı öğesinden alır.</span><span class="sxs-lookup"><span data-stu-id="33648-647">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="33648-648">Aşağıdaki kod `AddHeaderResultServiceFilter`gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-648">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="33648-649">Aşağıdaki kodda, `AddHeaderResultServiceFilter` dı kapsayıcısına eklenir:</span><span class="sxs-lookup"><span data-stu-id="33648-649">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="33648-650">Aşağıdaki kodda, `ServiceFilter` özniteliği dı öğesinden `AddHeaderResultServiceFilter` filtrenin bir örneğini alır:</span><span class="sxs-lookup"><span data-stu-id="33648-650">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="33648-651">`ServiceFilterAttribute`kullanırken, [Servicefilterattribute. ısyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="33648-651">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="33648-652">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden *kullanılabilir olabileceğini gösteren* bir ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-652">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="33648-653">ASP.NET Core çalışma zamanı garanti etmez:</span><span class="sxs-lookup"><span data-stu-id="33648-653">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="33648-654">Filtrenin tek bir örneğinin oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="33648-654">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="33648-655">Filtre, sonraki bir noktada dı kapsayıcısından yeniden istenmeyecek.</span><span class="sxs-lookup"><span data-stu-id="33648-655">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="33648-656">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-656">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="33648-657"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-657"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="33648-658">`IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="33648-658">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="33648-659">`CreateInstance`, belirtilen türü DI 'dan yükler.</span><span class="sxs-lookup"><span data-stu-id="33648-659">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="33648-660">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="33648-660">TypeFilterAttribute</span></span>

<span data-ttu-id="33648-661"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>benzerdir, ancak türü doğrudan dı kapsayıcısından çözümlenmez.</span><span class="sxs-lookup"><span data-stu-id="33648-661"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="33648-662"><xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>kullanarak türü başlatır.</span><span class="sxs-lookup"><span data-stu-id="33648-662">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="33648-663">`TypeFilterAttribute` türler doğrudan dı kapsayıcısından çözümlenmediğinden:</span><span class="sxs-lookup"><span data-stu-id="33648-663">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="33648-664">`TypeFilterAttribute` kullanılarak başvurulan türlerin dı kapsayıcısına kaydedilmesi gerekmez.</span><span class="sxs-lookup"><span data-stu-id="33648-664">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="33648-665">Bunların bağımlılıkları, dı kapsayıcısı tarafından yerine getirilir.</span><span class="sxs-lookup"><span data-stu-id="33648-665">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="33648-666">`TypeFilterAttribute`, isteğe bağlı olarak tür için Oluşturucu bağımsız değişkenlerini kabul edebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-666">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="33648-667">`TypeFilterAttribute`kullanırken [Typefilterattribute. ıyeniden kullanılabilir](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable)olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="33648-667">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="33648-668">Filtre örneğinin, içinde oluşturulduğu istek kapsamının dışında yeniden kullanılabilir *olabileceği* ipucu sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-668">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="33648-669">ASP.NET Core çalışma zamanı, filtrenin tek bir örneğinin oluşturulacağı garantisi vermez.</span><span class="sxs-lookup"><span data-stu-id="33648-669">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="33648-670">Tek bir yaşam süresine sahip hizmetlere bağımlı olan bir filtreyle kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-670">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="33648-671">Aşağıdaki örnek `TypeFilterAttribute`kullanarak bir türe bağımsız değişkenlerin nasıl geçirileceğini gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-671">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="33648-672">Yetkilendirme filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-672">Authorization filters</span></span>

<span data-ttu-id="33648-673">Yetkilendirme filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-673">Authorization filters:</span></span>

* <span data-ttu-id="33648-674">, Filtre ardışık düzeninde ilk filtrelerin çalıştırılmasıyla sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-674">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="33648-675">Eylem yöntemlerine erişimi denetleyin.</span><span class="sxs-lookup"><span data-stu-id="33648-675">Control access to action methods.</span></span>
* <span data-ttu-id="33648-676">Bir Before yöntemi, ancak After yönteminden hiçbiri.</span><span class="sxs-lookup"><span data-stu-id="33648-676">Have a before method, but no after method.</span></span>

<span data-ttu-id="33648-677">Özel Yetkilendirme filtreleri özel bir yetkilendirme çerçevesi gerektirir.</span><span class="sxs-lookup"><span data-stu-id="33648-677">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="33648-678">Özel bir filtre yazmak için yetkilendirme ilkelerini yapılandırmayı veya özel bir yetkilendirme ilkesi yazmayı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="33648-678">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="33648-679">Yerleşik yetkilendirme filtresi:</span><span class="sxs-lookup"><span data-stu-id="33648-679">The built-in authorization filter:</span></span>

* <span data-ttu-id="33648-680">Yetkilendirme sistemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="33648-680">Calls the authorization system.</span></span>
* <span data-ttu-id="33648-681">İstekleri yetkilendirmez.</span><span class="sxs-lookup"><span data-stu-id="33648-681">Does not authorize requests.</span></span>

<span data-ttu-id="33648-682">Yetkilendirme filtreleri içinde özel **durumlar atamayın:**</span><span class="sxs-lookup"><span data-stu-id="33648-682">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="33648-683">Özel durum işlenmeyecektir.</span><span class="sxs-lookup"><span data-stu-id="33648-683">The exception will not be handled.</span></span>
* <span data-ttu-id="33648-684">Özel durum filtreleri özel durumu işleymeyecektir.</span><span class="sxs-lookup"><span data-stu-id="33648-684">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="33648-685">Bir yetkilendirme filtresinde özel durum oluştuğunda bir sınama vermeyi düşünün.</span><span class="sxs-lookup"><span data-stu-id="33648-685">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="33648-686">[Yetkilendirme](xref:security/authorization/introduction)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="33648-686">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="33648-687">Kaynak filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-687">Resource filters</span></span>

<span data-ttu-id="33648-688">Kaynak filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-688">Resource filters:</span></span>

* <span data-ttu-id="33648-689"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-689">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="33648-690">Yürütme, filtre işlem hattının çoğunu sarmalar.</span><span class="sxs-lookup"><span data-stu-id="33648-690">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="33648-691">Kaynak filtrelerinden önce yalnızca [Yetkilendirme filtreleri](#authorization-filters) çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-691">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="33648-692">Kaynak filtreleri, işlem hattının büyük bir yanındaki kısa devre için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="33648-692">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="33648-693">Örneğin, bir önbelleğe alma filtresi, bir önbellek isabetinden ardışık düzen geri kalanından kaçınabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-693">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="33648-694">Kaynak filtresi örnekleri:</span><span class="sxs-lookup"><span data-stu-id="33648-694">Resource filter examples:</span></span>

* <span data-ttu-id="33648-695">Daha önce gösterilen [kısa devre dışı kaynak filtresi](#short-circuiting-resource-filter) .</span><span class="sxs-lookup"><span data-stu-id="33648-695">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="33648-696">[Disableformvaluemodelbindingattribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="33648-696">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="33648-697">Model bağlamanın form verilerine erişimini engeller.</span><span class="sxs-lookup"><span data-stu-id="33648-697">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="33648-698">Form verilerinin belleğe okunmasını engellemek için büyük dosya yüklemeleri için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="33648-698">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="33648-699">Eylem filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-699">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33648-700">Eylem filtreleri Razor Pages **için uygulanmaz.**</span><span class="sxs-lookup"><span data-stu-id="33648-700">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="33648-701">Razor Pages <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> destekler.</span><span class="sxs-lookup"><span data-stu-id="33648-701">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="33648-702">Daha fazla bilgi için bkz. [Razor Pages Için filtre yöntemleri](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="33648-702">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="33648-703">Eylem filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-703">Action filters:</span></span>

* <span data-ttu-id="33648-704"><xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> ya da <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> arabirimini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-704">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="33648-705">Yürütmesinin, eylem yöntemlerinin yürütülmesi çevreler.</span><span class="sxs-lookup"><span data-stu-id="33648-705">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="33648-706">Aşağıdaki kod bir örnek eylem filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-706">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="33648-707"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="33648-707">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="33648-708"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>-bir eylem yöntemine yönelik girişlerin okunmalarını sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-708"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="33648-709"><xref:Microsoft.AspNetCore.Mvc.Controller>-denetleyici örneğinin işlenmesine izin vermez.</span><span class="sxs-lookup"><span data-stu-id="33648-709"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="33648-710"><xref:System.Web.Mvc.ActionExecutingContext.Result>-eylem yönteminin ve sonraki eylem filtrelerinin `Result` kısa devre dışı yürütmesi.</span><span class="sxs-lookup"><span data-stu-id="33648-710"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="33648-711">Eylem yönteminde özel durum oluşturma:</span><span class="sxs-lookup"><span data-stu-id="33648-711">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="33648-712">Sonraki filtrelerin çalıştırılmasını önler.</span><span class="sxs-lookup"><span data-stu-id="33648-712">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="33648-713">`Result`ayarının aksine, başarılı bir sonuç yerine başarısızlık olarak değerlendirilir.</span><span class="sxs-lookup"><span data-stu-id="33648-713">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="33648-714"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> `Controller` ve `Result` ek olarak aşağıdaki özellikleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="33648-714">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="33648-715"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>-eylem yürütmesi başka bir filtre tarafından kabul edilse true.</span><span class="sxs-lookup"><span data-stu-id="33648-715"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="33648-716"><xref:System.Web.Mvc.ActionExecutedContext.Exception>-eylem veya daha önce çalıştırılan eylem filtresi bir özel durum oluşturdu, null değil.</span><span class="sxs-lookup"><span data-stu-id="33648-716"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="33648-717">Bu özellik null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="33648-717">Setting this property to null:</span></span>

  * <span data-ttu-id="33648-718">Özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="33648-718">Effectively handles the exception.</span></span>
  * <span data-ttu-id="33648-719">`Result`, eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="33648-719">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="33648-720">Bir `IAsyncActionFilter`için <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>çağrısı:</span><span class="sxs-lookup"><span data-stu-id="33648-720">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="33648-721">Sonraki eylem filtrelerini ve eylem yöntemini yürütür.</span><span class="sxs-lookup"><span data-stu-id="33648-721">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="33648-722">`ActionExecutedContext` döndürür.</span><span class="sxs-lookup"><span data-stu-id="33648-722">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="33648-723">Kısa devre dışı, bir sonuç örneğine <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> atayın ve `next` çağırmayın (`ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="33648-723">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="33648-724">Framework, alt sınıflı olabilecek bir soyut <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-724">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="33648-725">`OnActionExecuting` eylem filtresi şu şekilde kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="33648-725">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="33648-726">Model durumunu doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-726">Validate model state.</span></span>
* <span data-ttu-id="33648-727">Durum geçersizse bir hata döndürür.</span><span class="sxs-lookup"><span data-stu-id="33648-727">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="33648-728">`OnActionExecuted` yöntemi eylem yönteminden sonra çalışır:</span><span class="sxs-lookup"><span data-stu-id="33648-728">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="33648-729">Ve <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> özelliği aracılığıyla eylemin sonuçlarını görebilir ve değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-729">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="33648-730">eylem yürütmesi başka bir filtre tarafından kabul edilse, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> true olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-730"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="33648-731">eylem veya sonraki eylem filtresi bir özel durum harekete geçirdi, <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-731"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="33648-732">`Exception` null olarak ayarlanıyor:</span><span class="sxs-lookup"><span data-stu-id="33648-732">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="33648-733">Bir özel durumu etkin bir şekilde işler.</span><span class="sxs-lookup"><span data-stu-id="33648-733">Effectively handles an exception.</span></span>
  * <span data-ttu-id="33648-734">`ActionExecutedContext.Result`, normal olarak eylem yönteminden döndürülmüş gibi yürütülür.</span><span class="sxs-lookup"><span data-stu-id="33648-734">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="33648-735">Özel durum filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-735">Exception filters</span></span>

<span data-ttu-id="33648-736">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-736">Exception filters:</span></span>

* <span data-ttu-id="33648-737"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>uygulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-737">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="33648-738">, Yaygın hata işleme ilkelerini uygulamak için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-738">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="33648-739">Aşağıdaki örnek özel durum filtresi, uygulama geliştirmede olduğunda oluşan özel durumlar hakkındaki ayrıntıları görüntülemek için özel bir hata görünümü kullanır:</span><span class="sxs-lookup"><span data-stu-id="33648-739">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="33648-740">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-740">Exception filters:</span></span>

* <span data-ttu-id="33648-741">Etkinlikden önceki ve sonraki olaylar yok.</span><span class="sxs-lookup"><span data-stu-id="33648-741">Don't have before and after events.</span></span>
* <span data-ttu-id="33648-742"><xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>uygulayın.</span><span class="sxs-lookup"><span data-stu-id="33648-742">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="33648-743">Razor sayfası veya denetleyici oluşturma, [model bağlama](xref:mvc/models/model-binding), eylem filtreleri veya eylem yöntemlerinde oluşan işlenmemiş özel durumları işleyin.</span><span class="sxs-lookup"><span data-stu-id="33648-743">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="33648-744">Kaynak filtrelerinde, sonuç filtrelerinde veya MVC sonuç yürütülürken oluşan özel **durumları yakalamayın** .</span><span class="sxs-lookup"><span data-stu-id="33648-744">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="33648-745">Bir özel durumu işlemek için <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> özelliğini `true` veya bir yanıt yazın.</span><span class="sxs-lookup"><span data-stu-id="33648-745">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="33648-746">Bu, özel durumun yayılmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="33648-746">This stops propagation of the exception.</span></span> <span data-ttu-id="33648-747">Özel durum filtresi bir özel durumu "başarılı" olarak açamaz.</span><span class="sxs-lookup"><span data-stu-id="33648-747">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="33648-748">Yalnızca bir eylem filtresi bunu yapabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-748">Only an action filter can do that.</span></span>

<span data-ttu-id="33648-749">Özel durum filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-749">Exception filters:</span></span>

* <span data-ttu-id="33648-750">Eylemler içinde oluşan özel durumları yakalamaya uygundur.</span><span class="sxs-lookup"><span data-stu-id="33648-750">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="33648-751">, Hata işleme ara yazılımı olarak esnek değildir.</span><span class="sxs-lookup"><span data-stu-id="33648-751">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="33648-752">Özel durum işleme için ara yazılımı tercih edin.</span><span class="sxs-lookup"><span data-stu-id="33648-752">Prefer middleware for exception handling.</span></span> <span data-ttu-id="33648-753">Yalnızca hata işlemenin hangi eylem yöntemine göre *farklılık* gösteren özel durum filtrelerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="33648-753">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="33648-754">Örneğin, bir uygulama hem API uç noktaları hem de görünümler/HTML için eylem yöntemlerine sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-754">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="33648-755">API uç noktaları, hata bilgilerini JSON olarak döndürebilir, ancak görünüm tabanlı eylemler bir hata sayfasını HTML olarak döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-755">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="33648-756">Sonuç filtreleri</span><span class="sxs-lookup"><span data-stu-id="33648-756">Result filters</span></span>

<span data-ttu-id="33648-757">Sonuç filtreleri:</span><span class="sxs-lookup"><span data-stu-id="33648-757">Result filters:</span></span>

* <span data-ttu-id="33648-758">Arabirim uygulama:</span><span class="sxs-lookup"><span data-stu-id="33648-758">Implement an interface:</span></span>
  * <span data-ttu-id="33648-759"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="33648-759"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="33648-760"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> veya <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="33648-760"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="33648-761">Yürütmesi, eylem sonuçlarının yürütülmesini çevreler.</span><span class="sxs-lookup"><span data-stu-id="33648-761">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="33648-762">IResultFilter ve ıasyncresultfilter</span><span class="sxs-lookup"><span data-stu-id="33648-762">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="33648-763">Aşağıdaki kod, bir HTTP üst bilgisi ekleyen bir sonuç filtresi gösterir:</span><span class="sxs-lookup"><span data-stu-id="33648-763">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="33648-764">Yürütülen sonuç türü eyleme göre değişir.</span><span class="sxs-lookup"><span data-stu-id="33648-764">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="33648-765">Bir görünüm döndüren bir eylem, yürütülen <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir parçası olarak tüm Razor işlemlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="33648-765">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="33648-766">Bir API yöntemi, sonucun yürütülmesinin bir parçası olarak bazı serileştirme işlemleri gerçekleştirebilir.</span><span class="sxs-lookup"><span data-stu-id="33648-766">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="33648-767">[Eylem sonuçları](xref:mvc/controllers/actions)hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="33648-767">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="33648-768">Sonuç filtreleri yalnızca bir eylem veya eylem filtresi bir eylem sonucu üretirse yürütülür.</span><span class="sxs-lookup"><span data-stu-id="33648-768">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="33648-769">Şu durumlarda sonuç filtreleri yürütülmez:</span><span class="sxs-lookup"><span data-stu-id="33648-769">Result filters are not executed when:</span></span>

* <span data-ttu-id="33648-770">Bir yetkilendirme filtresi veya kaynak filtresi, işlem hattı için kısa süreli olarak devre dışı.</span><span class="sxs-lookup"><span data-stu-id="33648-770">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="33648-771">Bir özel durum filtresi, bir eylem sonucu üreterek özel durumu işler.</span><span class="sxs-lookup"><span data-stu-id="33648-771">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="33648-772"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> yöntemi, eylem sonucunun ve sonraki sonuç filtrelerinin `true`<xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> ayarlanarak kısa devre yürütülmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-772">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="33648-773">Boş bir yanıt oluşturmamaya kaçınmak için kısa devre dışı bırakıldığında yanıt nesnesine yazın.</span><span class="sxs-lookup"><span data-stu-id="33648-773">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="33648-774">`IResultFilter.OnResultExecuting` bir özel durum oluşturma şu şekilde yapılır:</span><span class="sxs-lookup"><span data-stu-id="33648-774">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="33648-775">Eylem sonucunun ve sonraki filtrelerin yürütülmesini önleyin.</span><span class="sxs-lookup"><span data-stu-id="33648-775">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="33648-776">Başarılı bir sonuç yerine hata olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="33648-776">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="33648-777"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> yöntemi çalıştırıldığında, yanıt istemciye zaten gönderilir.</span><span class="sxs-lookup"><span data-stu-id="33648-777">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="33648-778">Yanıt istemciye zaten gönderildiyse, daha fazla değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="33648-778">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="33648-779">`ResultExecutedContext.Canceled`, eylem sonucu yürütmesi başka bir filtre tarafından kabul edilen kısa devre ise `true` olarak ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-779">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="33648-780">eylem sonucu veya sonraki sonuç filtresi bir özel durum harekete geçirdi, `ResultExecutedContext.Exception` null olmayan bir değere ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="33648-780">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="33648-781">`Exception` null olarak ayarlanması, bir özel durumu etkili bir şekilde işler ve özel durumun, ardışık düzendeki ASP.NET Core daha sonra yeniden oluşturulmasını önler.</span><span class="sxs-lookup"><span data-stu-id="33648-781">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="33648-782">Bir sonuç filtresinde özel durum işlenirken yanıta veri yazmanın güvenilir bir yolu yoktur.</span><span class="sxs-lookup"><span data-stu-id="33648-782">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="33648-783">Bir eylem sonucu bir özel durum oluşturduğunda üstbilgiler istemciye temizleniyorsa, hata kodu göndermek için güvenilir bir mekanizma yoktur.</span><span class="sxs-lookup"><span data-stu-id="33648-783">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="33648-784">Bir <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>için, <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> bir `await next` çağrısı, sonraki sonuç filtrelerini ve eylem sonucunu yürütür.</span><span class="sxs-lookup"><span data-stu-id="33648-784">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="33648-785">Kısa devre dışı, [ResultExecutingContext. Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) `true` ve `ResultExecutionDelegate`çağırmayın:</span><span class="sxs-lookup"><span data-stu-id="33648-785">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="33648-786">Framework, alt sınıflı olabilecek bir soyut `ResultFilterAttribute` sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-786">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="33648-787">Daha önce gösterilen [Addheaderattribute](#add-header-attribute) sınıfı bir sonuç Filtresi özniteliği örneğidir.</span><span class="sxs-lookup"><span data-stu-id="33648-787">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="33648-788">Ialwaysrunresultfilter ve ıasyncalwaysrunresultfilter</span><span class="sxs-lookup"><span data-stu-id="33648-788">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="33648-789"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> ve <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> arabirimleri, tüm eylem sonuçları için çalışan bir <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> uygulamasını bildirir.</span><span class="sxs-lookup"><span data-stu-id="33648-789">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="33648-790">Bu, tarafından oluşturulan eylem sonuçlarını içerir:</span><span class="sxs-lookup"><span data-stu-id="33648-790">This includes action results produced by:</span></span>

* <span data-ttu-id="33648-791">Kısa devre olan Yetkilendirme filtreleri ve kaynak filtreleri.</span><span class="sxs-lookup"><span data-stu-id="33648-791">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="33648-792">Özel durum filtreleri.</span><span class="sxs-lookup"><span data-stu-id="33648-792">Exception filters.</span></span>

<span data-ttu-id="33648-793">Örneğin, aşağıdaki filtre her zaman çalışır ve içerik anlaşması başarısız olduğunda *422 olmayan bir varlık* durum kodu ile bir eylem sonucu (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) ayarlar:</span><span class="sxs-lookup"><span data-stu-id="33648-793">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="33648-794">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="33648-794">IFilterFactory</span></span>

<span data-ttu-id="33648-795"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-795"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="33648-796">Bu nedenle, bir `IFilterFactory` örneği, filtre ardışık düzeninde herhangi bir yerde `IFilterMetadata` bir örnek olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="33648-796">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="33648-797">Çalışma zamanı filtreyi çağırmayı hazırlarken, bir `IFilterFactory`dönüştürmeyi dener.</span><span class="sxs-lookup"><span data-stu-id="33648-797">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="33648-798">Bu atama başarılı olursa, çağrılan `IFilterMetadata` örneğini oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemi çağırılır.</span><span class="sxs-lookup"><span data-stu-id="33648-798">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="33648-799">Bu, tam filtre işlem hattının uygulama başladığında açıkça ayarlanması gerektiğinden esnek bir tasarım sağlar.</span><span class="sxs-lookup"><span data-stu-id="33648-799">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="33648-800">`IFilterFactory`, filtre oluşturmaya yönelik başka bir yaklaşım olarak özel öznitelik uygulamaları kullanılarak uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="33648-800">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="33648-801">Önceki kod, [indirme örneği](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample)çalıştırılarak test edilebilir:</span><span class="sxs-lookup"><span data-stu-id="33648-801">The preceding code can be tested by running the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="33648-802">F12 geliştirici araçlarını çağırın.</span><span class="sxs-lookup"><span data-stu-id="33648-802">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="33648-803">`https://localhost:5001/Sample/HeaderWithFactory` sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="33648-803">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="33648-804">F12 geliştirici araçları, örnek kod tarafından eklenen aşağıdaki yanıt üstbilgilerini görüntüler:</span><span class="sxs-lookup"><span data-stu-id="33648-804">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="33648-805">**Yazar:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="33648-805">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="33648-806">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="33648-806">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="33648-807">**iç:** `My header`</span><span class="sxs-lookup"><span data-stu-id="33648-807">**internal:** `My header`</span></span>

<span data-ttu-id="33648-808">Yukarıdaki kod, **iç:** `My header` yanıt üst bilgisini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="33648-808">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="33648-809">Öznitelik üzerinde IFilterFactory uygulandı</span><span class="sxs-lookup"><span data-stu-id="33648-809">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="33648-810">`IFilterFactory` uygulayan filtreler şu filtreler için yararlıdır:</span><span class="sxs-lookup"><span data-stu-id="33648-810">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="33648-811">Parametre geçirme gerekmez.</span><span class="sxs-lookup"><span data-stu-id="33648-811">Don't require passing parameters.</span></span>
* <span data-ttu-id="33648-812">DI tarafından doldurulması gereken Oluşturucu bağımlılıkları vardır.</span><span class="sxs-lookup"><span data-stu-id="33648-812">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="33648-813"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>uygular.</span><span class="sxs-lookup"><span data-stu-id="33648-813"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="33648-814">`IFilterFactory`, bir <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> örneği oluşturmak için <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> yöntemini kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="33648-814">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="33648-815">`CreateInstance`, belirtilen türü hizmetler kapsayıcısından (DI) yükler.</span><span class="sxs-lookup"><span data-stu-id="33648-815">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="33648-816">Aşağıdaki kodda `[SampleActionFilter]`uygulamak için üç yaklaşım gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="33648-816">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="33648-817">Yukarıdaki kodda, yöntemi `[SampleActionFilter]` olarak dekorasyon, `SampleActionFilter`uygulamak için tercih edilen yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="33648-817">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="33648-818">Filtre ardışık düzeninde ara yazılım kullanma</span><span class="sxs-lookup"><span data-stu-id="33648-818">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="33648-819">Kaynak filtreleri, işlem hattında daha sonra gelen her şeyin yürütülmesini çevreleyecek olan [Ara yazılım](xref:fundamentals/middleware/index) gibi çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-819">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="33648-820">Ancak filtreler, ASP.NET Core çalışma zamanının bir parçası olan ara yazılımlar dışında farklılık gösterir, bu da ASP.NET Core bağlamına ve yapılarına erişime sahip oldukları anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="33648-820">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="33648-821">Ara yazılımı bir filtre olarak kullanmak için, filtre ardışık düzenine eklenecek olan ara yazılımı belirten `Configure` yöntemi ile bir tür oluşturun.</span><span class="sxs-lookup"><span data-stu-id="33648-821">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="33648-822">Aşağıdaki örnek, bir istek için geçerli kültürü oluşturmak üzere yerelleştirme ara yazılımını kullanır:</span><span class="sxs-lookup"><span data-stu-id="33648-822">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="33648-823">Ara yazılımı çalıştırmak için <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> kullanın:</span><span class="sxs-lookup"><span data-stu-id="33648-823">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="33648-824">Ara yazılım filtreleri, filtre işlem hattının aynı aşamasında, model bağlamadan önce ve işlem hattının geri kalanı ile kaynak filtreleri olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="33648-824">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="33648-825">Sonraki eylemler</span><span class="sxs-lookup"><span data-stu-id="33648-825">Next actions</span></span>

* <span data-ttu-id="33648-826">[Razor Pages Için filtre yöntemlerine](xref:razor-pages/filter)bakın.</span><span class="sxs-lookup"><span data-stu-id="33648-826">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="33648-827">Filtrelerle denemek için [GitHub örneğini indirin, test edin ve değiştirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="33648-827">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
