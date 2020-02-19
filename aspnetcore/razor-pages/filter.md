---
title: ASP.NET Core Razor Pages için filtre yöntemleri
author: Rick-Anderson
description: ASP.NET Core Razor Pages için filtre yöntemleri oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 2/18/2020
uid: razor-pages/filter
ms.openlocfilehash: a60b17685c6f836de7c0afcc5b89a9894fb8b28f
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447237"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="1b195-103">ASP.NET Core Razor Pages için filtre yöntemleri</span><span class="sxs-lookup"><span data-stu-id="1b195-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1b195-104">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1b195-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1b195-105">Razor sayfa filtreleri [ıpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) ve [ıasyncpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Razor Pages, bir Razor sayfa işleyicisi çalıştırılmadan önce ve sonra kodu çalıştırmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="1b195-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="1b195-106">Razor sayfası filtreleri, tek sayfa işleyicisi yöntemlerine uygulanamadığından, [ASP.NET Core MVC eylem filtrelerine](xref:mvc/controllers/filters#action-filters)benzerdir.</span><span class="sxs-lookup"><span data-stu-id="1b195-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span>

<span data-ttu-id="1b195-107">Razor sayfası filtreleri:</span><span class="sxs-lookup"><span data-stu-id="1b195-107">Razor Page filters:</span></span>

* <span data-ttu-id="1b195-108">Bir işleyici yöntemi seçildikten sonra, ancak model bağlama gerçekleşmeden önce kodu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1b195-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="1b195-109">Model bağlama işlemi tamamlandıktan sonra işleyici metodu yürütülmeden önce kodu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1b195-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="1b195-110">İşleyici yöntemi yürütüldükten sonra kodu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1b195-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="1b195-111">, Bir sayfada veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="1b195-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="1b195-112">Belirli sayfa işleyici yöntemlerine uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="1b195-112">Cannot be applied to specific page handler methods.</span></span>
* <span data-ttu-id="1b195-113">, [Bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından doldurulmuş Oluşturucu bağımlılıkları olabilir.</span><span class="sxs-lookup"><span data-stu-id="1b195-113">Can have constructor dependencies populated by [Dependency Injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="1b195-114">Daha fazla bilgi için bkz. [Servicefilterattribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) ve [typefilterattribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).</span><span class="sxs-lookup"><span data-stu-id="1b195-114">For more information, see [ServiceFilterAttribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) and [TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).</span></span>

<span data-ttu-id="1b195-115">Sayfa oluşturucular ve ara yazılım, bir işleyici yöntemi yürütmeden önce özel kod yürütmeyi etkinleştirirken, yalnızca Razor sayfası filtreleri <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> ve sayfaya erişimi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="1b195-115">While page constructors and middleware enable executing custom code before a handler method executes, only Razor Page filters enable access to <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> and the page.</span></span> <span data-ttu-id="1b195-116">Ara yazılım `HttpContext`erişimine sahiptir ancak "sayfa bağlamına" erişemez.</span><span class="sxs-lookup"><span data-stu-id="1b195-116">Middleware has access to the `HttpContext`, but not to the "page context".</span></span> <span data-ttu-id="1b195-117">Filtreler, `HttpContext`erişim sağlayan <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> türetilmiş bir parametreye sahiptir.</span><span class="sxs-lookup"><span data-stu-id="1b195-117">Filters have a <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="1b195-118">Örneğin, [bir filtre uygula özniteliği](#ifa) örneği yanıta, oluşturucular veya ara yazılım ile yapılamadığını belirten bir üst bilgi ekler.</span><span class="sxs-lookup"><span data-stu-id="1b195-118">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="1b195-119">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1b195-119">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1b195-120">Razor sayfası filtreleri, genel olarak veya sayfa düzeyinde uygulanabilecek aşağıdaki yöntemleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="1b195-120">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="1b195-121">Zaman uyumlu Yöntemler:</span><span class="sxs-lookup"><span data-stu-id="1b195-121">Synchronous methods:</span></span>

  * <span data-ttu-id="1b195-122">[Onpagehandlerselected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : bir işleyici yöntemi seçildikten sonra, ancak model bağlama gerçekleşmeden önce çağırılır.</span><span class="sxs-lookup"><span data-stu-id="1b195-122">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="1b195-123">[Onpagehandlerexecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Işleyici Yöntemi yürütülmeden önce çağırılır, model bağlama işlemi tamamlandıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="1b195-123">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
  * <span data-ttu-id="1b195-124">[Onpagehandleryürütüldü](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : işleyici yöntemi yürütüldükten sonra, eylem sonucundan önce çağırılır.</span><span class="sxs-lookup"><span data-stu-id="1b195-124">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="1b195-125">Zaman uyumsuz yöntemler:</span><span class="sxs-lookup"><span data-stu-id="1b195-125">Asynchronous methods:</span></span>

  * <span data-ttu-id="1b195-126">[Onpagehandlerselectionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Handler yöntemi seçildikten sonra zaman uyumsuz olarak çağırılır, ancak model bağlama gerçekleşmeden önce.</span><span class="sxs-lookup"><span data-stu-id="1b195-126">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="1b195-127">[Onpagehandlerexecutionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Handler yöntemi çağrılmadan önce zaman uyumsuz olarak çağrıldı, model bağlama işlemi tamamlandıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="1b195-127">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

<span data-ttu-id="1b195-128">Her ikisini de **değil** , bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="1b195-128">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="1b195-129">Çerçeve öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="1b195-129">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="1b195-130">Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır.</span><span class="sxs-lookup"><span data-stu-id="1b195-130">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="1b195-131">Her iki arabirim de uygulanmışsa yalnızca zaman uyumsuz yöntemler çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1b195-131">If both interfaces are implemented, only the async methods are called.</span></span> <span data-ttu-id="1b195-132">Aynı kural sayfalardaki geçersiz kılmalara uygulanır, her ikisine de değil, geçersiz kılmanın zaman uyumlu veya zaman uyumsuz sürümünü uygular.</span><span class="sxs-lookup"><span data-stu-id="1b195-132">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="1b195-133">Razor sayfası filtrelerini küresel olarak uygulama</span><span class="sxs-lookup"><span data-stu-id="1b195-133">Implement Razor Page filters globally</span></span>

<span data-ttu-id="1b195-134">Aşağıdaki kod `IAsyncPageFilter`uygular:</span><span class="sxs-lookup"><span data-stu-id="1b195-134">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="1b195-135">Yukarıdaki kodda `ProcessUserAgent.Write`, Kullanıcı aracı dizesiyle birlikte çalışarak Kullanıcı tarafından sağlanan koddur.</span><span class="sxs-lookup"><span data-stu-id="1b195-135">In the preceding code, `ProcessUserAgent.Write` is user supplied code that works with the user agent string.</span></span>

<span data-ttu-id="1b195-136">Aşağıdaki kod `Startup` sınıfındaki `SampleAsyncPageFilter` sunar:</span><span class="sxs-lookup"><span data-stu-id="1b195-136">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup.cs?name=snippet2)]

<span data-ttu-id="1b195-137">Aşağıdaki kod, `SampleAsyncPageFilter` yalnızca */filmlerde*sayfalara uygulamak için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> çağırır:</span><span class="sxs-lookup"><span data-stu-id="1b195-137">The following code calls <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to apply the `SampleAsyncPageFilter` to only pages in */Movies*:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="1b195-138">Aşağıdaki kod, zaman uyumlu `IPageFilter`uygular:</span><span class="sxs-lookup"><span data-stu-id="1b195-138">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="1b195-139">Aşağıdaki kod `SamplePageFilter`etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="1b195-139">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/StartupSync.cs?name=snippet2)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="1b195-140">Filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama</span><span class="sxs-lookup"><span data-stu-id="1b195-140">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="1b195-141">Aşağıdaki kod, zaman uyumsuz Razor sayfası filtrelerini geçersiz kılar:</span><span class="sxs-lookup"><span data-stu-id="1b195-141">The following code overrides the asynchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Index.cshtml.cs?name=snippet)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a><span data-ttu-id="1b195-142">Filtre özniteliği uygulama</span><span class="sxs-lookup"><span data-stu-id="1b195-142">Implement a filter attribute</span></span>

<span data-ttu-id="1b195-143">Yerleşik öznitelik tabanlı filtre <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> filtresi, alt sınıflı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1b195-143">The built-in attribute-based filter <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> filter can be subclassed.</span></span> <span data-ttu-id="1b195-144">Aşağıdaki filtre yanıta bir üst bilgi ekler:</span><span class="sxs-lookup"><span data-stu-id="1b195-144">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="1b195-145">Aşağıdaki kod `AddHeader` özniteliğini uygular:</span><span class="sxs-lookup"><span data-stu-id="1b195-145">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Movies/Test.cshtml.cs)]

<span data-ttu-id="1b195-146">Üst bilgileri incelemek için tarayıcı geliştirici araçları gibi bir araç kullanın.</span><span class="sxs-lookup"><span data-stu-id="1b195-146">Use a tool such as the browser developer tools to examine the headers.</span></span> <span data-ttu-id="1b195-147">**Yanıt üst bilgileri**altında `author: Rick` görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="1b195-147">Under **Response Headers**, `author: Rick` is displayed.</span></span>

<span data-ttu-id="1b195-148">Sıralamayı geçersiz kılma yönergeleri için bkz. [varsayılan sırayı geçersiz kılma](xref:mvc/controllers/filters#overriding-the-default-order) .</span><span class="sxs-lookup"><span data-stu-id="1b195-148">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="1b195-149">Filtre işlem hattının bir filtreden kısa devre dışı olması için bkz. [iptal ve kısa](xref:mvc/controllers/filters#cancellation-and-short-circuiting) devre.</span><span class="sxs-lookup"><span data-stu-id="1b195-149">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span>

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a><span data-ttu-id="1b195-150">Yetkilendir filtre özniteliği</span><span class="sxs-lookup"><span data-stu-id="1b195-150">Authorize filter attribute</span></span>

<span data-ttu-id="1b195-151">[Yetkilendir](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) özniteliği bir `PageModel`uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="1b195-151">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1b195-152">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1b195-152">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1b195-153">Razor sayfa filtreleri [ıpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) ve [ıasyncpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Razor Pages, bir Razor sayfa işleyicisi çalıştırılmadan önce ve sonra kodu çalıştırmasına izin verir.</span><span class="sxs-lookup"><span data-stu-id="1b195-153">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="1b195-154">Razor sayfası filtreleri, tek sayfa işleyicisi yöntemlerine uygulanamadığından, [ASP.NET Core MVC eylem filtrelerine](xref:mvc/controllers/filters#action-filters)benzerdir.</span><span class="sxs-lookup"><span data-stu-id="1b195-154">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span>

<span data-ttu-id="1b195-155">Razor sayfası filtreleri:</span><span class="sxs-lookup"><span data-stu-id="1b195-155">Razor Page filters:</span></span>

* <span data-ttu-id="1b195-156">Bir işleyici yöntemi seçildikten sonra, ancak model bağlama gerçekleşmeden önce kodu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1b195-156">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="1b195-157">Model bağlama işlemi tamamlandıktan sonra işleyici metodu yürütülmeden önce kodu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1b195-157">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="1b195-158">İşleyici yöntemi yürütüldükten sonra kodu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1b195-158">Run code after the handler method executes.</span></span>
* <span data-ttu-id="1b195-159">, Bir sayfada veya genel olarak uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="1b195-159">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="1b195-160">Belirli sayfa işleyici yöntemlerine uygulanamaz.</span><span class="sxs-lookup"><span data-stu-id="1b195-160">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="1b195-161">Bir işleyici yöntemi sayfa Oluşturucusu veya ara yazılım kullanılarak yürütülmeden önce kod çalıştırılabilir, ancak yalnızca Razor sayfası filtrelerinin [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext)'e erişimi vardır.</span><span class="sxs-lookup"><span data-stu-id="1b195-161">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="1b195-162">Filtrelerin `HttpContext`erişim sağlayan bir [Filtercontext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) türetilmiş parametresi vardır.</span><span class="sxs-lookup"><span data-stu-id="1b195-162">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="1b195-163">Örneğin, [bir filtre uygula özniteliği](#ifa) örneği yanıta, oluşturucular veya ara yazılım ile yapılamadığını belirten bir üst bilgi ekler.</span><span class="sxs-lookup"><span data-stu-id="1b195-163">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="1b195-164">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1b195-164">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1b195-165">Razor sayfası filtreleri, genel olarak veya sayfa düzeyinde uygulanabilecek aşağıdaki yöntemleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="1b195-165">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="1b195-166">Zaman uyumlu Yöntemler:</span><span class="sxs-lookup"><span data-stu-id="1b195-166">Synchronous methods:</span></span>

  * <span data-ttu-id="1b195-167">[Onpagehandlerselected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : bir işleyici yöntemi seçildikten sonra, ancak model bağlama gerçekleşmeden önce çağırılır.</span><span class="sxs-lookup"><span data-stu-id="1b195-167">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="1b195-168">[Onpagehandlerexecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Işleyici Yöntemi yürütülmeden önce çağırılır, model bağlama işlemi tamamlandıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="1b195-168">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
  * <span data-ttu-id="1b195-169">[Onpagehandleryürütüldü](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : işleyici yöntemi yürütüldükten sonra, eylem sonucundan önce çağırılır.</span><span class="sxs-lookup"><span data-stu-id="1b195-169">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="1b195-170">Zaman uyumsuz yöntemler:</span><span class="sxs-lookup"><span data-stu-id="1b195-170">Asynchronous methods:</span></span>

  * <span data-ttu-id="1b195-171">[Onpagehandlerselectionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Handler yöntemi seçildikten sonra zaman uyumsuz olarak çağırılır, ancak model bağlama gerçekleşmeden önce.</span><span class="sxs-lookup"><span data-stu-id="1b195-171">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="1b195-172">[Onpagehandlerexecutionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Handler yöntemi çağrılmadan önce zaman uyumsuz olarak çağrıldı, model bağlama işlemi tamamlandıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="1b195-172">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="1b195-173">Her ikisini de değil, bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.**</span><span class="sxs-lookup"><span data-stu-id="1b195-173">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="1b195-174">Çerçeve öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır.</span><span class="sxs-lookup"><span data-stu-id="1b195-174">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="1b195-175">Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır.</span><span class="sxs-lookup"><span data-stu-id="1b195-175">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="1b195-176">Her iki arabirim de uygulanmışsa yalnızca zaman uyumsuz yöntemler çağrılır.</span><span class="sxs-lookup"><span data-stu-id="1b195-176">If both interfaces are implemented, only the async methods are called.</span></span> <span data-ttu-id="1b195-177">Aynı kural sayfalardaki geçersiz kılmalara uygulanır, her ikisine de değil, geçersiz kılmanın zaman uyumlu veya zaman uyumsuz sürümünü uygular.</span><span class="sxs-lookup"><span data-stu-id="1b195-177">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="1b195-178">Razor sayfası filtrelerini küresel olarak uygulama</span><span class="sxs-lookup"><span data-stu-id="1b195-178">Implement Razor Page filters globally</span></span>

<span data-ttu-id="1b195-179">Aşağıdaki kod `IAsyncPageFilter`uygular:</span><span class="sxs-lookup"><span data-stu-id="1b195-179">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="1b195-180">Yukarıdaki kodda, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="1b195-180">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="1b195-181">Uygulama için izleme bilgilerini sağlamak üzere örnekte kullanılır.</span><span class="sxs-lookup"><span data-stu-id="1b195-181">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="1b195-182">Aşağıdaki kod `Startup` sınıfındaki `SampleAsyncPageFilter` sunar:</span><span class="sxs-lookup"><span data-stu-id="1b195-182">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="1b195-183">Aşağıdaki kod, tüm `Startup` sınıfını gösterir:</span><span class="sxs-lookup"><span data-stu-id="1b195-183">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="1b195-184">Aşağıdaki kod, `SampleAsyncPageFilter` yalnızca */alt klasöründeki*sayfalara uygulamak için `AddFolderApplicationModelConvention` çağırır:</span><span class="sxs-lookup"><span data-stu-id="1b195-184">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="1b195-185">Aşağıdaki kod, zaman uyumlu `IPageFilter`uygular:</span><span class="sxs-lookup"><span data-stu-id="1b195-185">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="1b195-186">Aşağıdaki kod `SamplePageFilter`etkinleştirilir:</span><span class="sxs-lookup"><span data-stu-id="1b195-186">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="1b195-187">Filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama</span><span class="sxs-lookup"><span data-stu-id="1b195-187">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="1b195-188">Aşağıdaki kod, zaman uyumlu Razor sayfası filtrelerini geçersiz kılar:</span><span class="sxs-lookup"><span data-stu-id="1b195-188">The following code overrides the synchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a><span data-ttu-id="1b195-189">Filtre özniteliği uygulama</span><span class="sxs-lookup"><span data-stu-id="1b195-189">Implement a filter attribute</span></span>

<span data-ttu-id="1b195-190">Yerleşik öznitelik tabanlı filtre [Onresultexecutionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filtresi, alt sınıflı olabilir.</span><span class="sxs-lookup"><span data-stu-id="1b195-190">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="1b195-191">Aşağıdaki filtre yanıta bir üst bilgi ekler:</span><span class="sxs-lookup"><span data-stu-id="1b195-191">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="1b195-192">Aşağıdaki kod `AddHeader` özniteliğini uygular:</span><span class="sxs-lookup"><span data-stu-id="1b195-192">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="1b195-193">Sıralamayı geçersiz kılma yönergeleri için bkz. [varsayılan sırayı geçersiz kılma](xref:mvc/controllers/filters#overriding-the-default-order) .</span><span class="sxs-lookup"><span data-stu-id="1b195-193">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="1b195-194">Filtre işlem hattının bir filtreden kısa devre dışı olması için bkz. [iptal ve kısa](xref:mvc/controllers/filters#cancellation-and-short-circuiting) devre.</span><span class="sxs-lookup"><span data-stu-id="1b195-194">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a><span data-ttu-id="1b195-195">Yetkilendir filtre özniteliği</span><span class="sxs-lookup"><span data-stu-id="1b195-195">Authorize filter attribute</span></span>

<span data-ttu-id="1b195-196">[Yetkilendir](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) özniteliği bir `PageModel`uygulanabilir:</span><span class="sxs-lookup"><span data-stu-id="1b195-196">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end
