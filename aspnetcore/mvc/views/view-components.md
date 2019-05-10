---
title: ASP.NET core'da görünüm bileşenleri
author: rick-anderson
description: ASP.NET Core görünümü bileşenlerin nasıl kullanıldığı ve bunları için uygulamaları nasıl ekleyeceğinizi öğrenin.
ms.author: riande
ms.date: 1/30/2019
uid: mvc/views/view-components
ms.openlocfilehash: 2bcf6411933b884c2f96d926827079dfbc25ca74
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903380"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="ce385-103">ASP.NET core'da görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="ce385-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="ce385-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ce385-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ce385-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce385-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="ce385-106">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="ce385-106">View components</span></span>

<span data-ttu-id="ce385-107">Kısmi görünüm için Görünüm bileşenleri benzerdir, ancak bunlar çok daha güçlü.</span><span class="sxs-lookup"><span data-stu-id="ce385-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="ce385-108">Görünüm bileşenleri kullanmayın model bağlama ve içine çağırırken sağlanan verileri yalnızca bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="ce385-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="ce385-109">Denetleyicileri ve görünümleri kullanarak bu makalenin yazıldığı ancak görünüm bileşenleri de Razor sayfaları kullanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="ce385-109">This article was written using controllers and views, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="ce385-110">Bir görünümü bileşeni:</span><span class="sxs-lookup"><span data-stu-id="ce385-110">A view component:</span></span>

* <span data-ttu-id="ce385-111">Bir yanıtın tamamını yerine bir öbek işler.</span><span class="sxs-lookup"><span data-stu-id="ce385-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="ce385-112">Test Edilebilirlik avantajları bir denetleyici ve görünüm arasında bulunan ve aynı ayrımı-ın-ile ilgili sorunlar içerir.</span><span class="sxs-lookup"><span data-stu-id="ce385-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="ce385-113">Parametreleri ve iş mantığına sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="ce385-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="ce385-114">Genellikle bir düzen sayfasından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ce385-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="ce385-115">Görünüm bileşenleri herhangi bir kısmi görünüm için çok karmaşık olduğu gibi yeniden kullanılabilir işleme mantığı sahip tasarlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="ce385-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="ce385-116">Dinamik Gezinti menüleri</span><span class="sxs-lookup"><span data-stu-id="ce385-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="ce385-117">Etiket bulutunu (burada veritabanını sorgular)</span><span class="sxs-lookup"><span data-stu-id="ce385-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="ce385-118">Oturum açma paneli</span><span class="sxs-lookup"><span data-stu-id="ce385-118">Login panel</span></span>
* <span data-ttu-id="ce385-119">Alışveriş sepeti</span><span class="sxs-lookup"><span data-stu-id="ce385-119">Shopping cart</span></span>
* <span data-ttu-id="ce385-120">Yakın zamanda yayımlanan makaleler</span><span class="sxs-lookup"><span data-stu-id="ce385-120">Recently published articles</span></span>
* <span data-ttu-id="ce385-121">Tipik bir blog kenar çubuğu içeriği</span><span class="sxs-lookup"><span data-stu-id="ce385-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="ce385-122">Bir oturum açma paneli her sayfada işlenir ve oturumu kapatmayın veya kullanıcının durumunu günlüğünde bağlı olarak, oturum için ya da bağlantılarını göster</span><span class="sxs-lookup"><span data-stu-id="ce385-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="ce385-123">Bileşeni görüntüle iki bölümden oluşur: sınıfı (genellikle türetilen [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) ve isteğe bağlı olarak sonucu (genellikle bir görünüm) döndürür.</span><span class="sxs-lookup"><span data-stu-id="ce385-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="ce385-124">Denetleyicileri gibi bir POCO görünümü bileşen olabilir, ancak çoğu Geliştirici türetme tarafından kullanılabilen özellikler ve yöntemler yararlanmak isteyeceksiniz `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="ce385-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="ce385-125">Bir görünümü bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="ce385-125">Creating a view component</span></span>

<span data-ttu-id="ce385-126">Bu bölümde, bir görünüm bileşeni oluşturmak için en üst düzey gereksinimleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="ce385-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="ce385-127">Makalenin sonraki bölümlerinde size her adım ayrıntılı inceleyin ve bir görünüm bileşeni oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="ce385-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="ce385-128">Görünümü bileşen sınıfı</span><span class="sxs-lookup"><span data-stu-id="ce385-128">The view component class</span></span>

<span data-ttu-id="ce385-129">Bir görünümü bileşen sınıfı aşağıdakilerden birini oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="ce385-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="ce385-130">Öğesinden türetme *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="ce385-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="ce385-131">Bir sınıf ile dekorasyon `[ViewComponent]` özniteliği veya bir sınıf türetmek `[ViewComponent]` özniteliği</span><span class="sxs-lookup"><span data-stu-id="ce385-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="ce385-132">Bir sınıf adı soneki ile sona ereceği oluşturma *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="ce385-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="ce385-133">Denetleyicileri gibi görünüm bileşenleri, genel, iç içe olmayan ve soyut olmayan sınıflar olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ce385-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="ce385-134">Görünümü bileşen adı kaldırıldı "ViewComponent" sonekine sahip sınıf adıdır.</span><span class="sxs-lookup"><span data-stu-id="ce385-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="ce385-135">Onu da açıkça kullanılarak belirtilebilir `ViewComponentAttribute.Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="ce385-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="ce385-136">Bir görünümü bileşen sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ce385-136">A view component class:</span></span>

* <span data-ttu-id="ce385-137">Oluşturucu tam olarak destekler [bağımlılık ekleme](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="ce385-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="ce385-138">Bölümü kullanamazsınız yani denetleyici yaşam döngüsünde almaz [filtreleri](../controllers/filters.md) görünümü bileşeninde</span><span class="sxs-lookup"><span data-stu-id="ce385-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="ce385-139">Görünümü bileşen yöntemleri</span><span class="sxs-lookup"><span data-stu-id="ce385-139">View component methods</span></span>

<span data-ttu-id="ce385-140">Görünümü bileşen kendi mantığı tanımlayan bir `InvokeAsync` döndüren yöntem bir `Task<IViewComponentResult>` veya bir zaman uyumlu olarak `Invoke` döndüren yöntem bir `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="ce385-140">A view component defines its logic in an `InvokeAsync` method that returns a `Task<IViewComponentResult>` or in a synchronous `Invoke` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="ce385-141">Parametreler doğrudan çağrı görünümü bileşeninin değil, model bağlama gelir.</span><span class="sxs-lookup"><span data-stu-id="ce385-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="ce385-142">Bir görünümü bileşeni hiçbir zaman doğrudan bir isteği işler.</span><span class="sxs-lookup"><span data-stu-id="ce385-142">A view component never directly handles a request.</span></span> <span data-ttu-id="ce385-143">Genellikle, bir görünüm bileşeni bir model başlatır ve çağırarak görünümüne geçirir `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ce385-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="ce385-144">Özet olarak, bileşen yöntemleri görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="ce385-144">In summary, view component methods:</span></span>

* <span data-ttu-id="ce385-145">Tanımlayan bir `InvokeAsync` döndüren yöntem bir `Task<IViewComponentResult>` veya bir zaman uyumlu `Invoke` döndüren yöntem bir `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="ce385-145">Define an `InvokeAsync` method that returns a `Task<IViewComponentResult>` or a synchronous `Invoke` method that returns an `IViewComponentResult`.</span></span>
* <span data-ttu-id="ce385-146">Genellikle bir model başlatır ve çağırarak görünümüne geçirir `ViewComponent` `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ce385-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method.</span></span>
* <span data-ttu-id="ce385-147">Parametreleri HTTP değil çağırma yönteminden gelir.</span><span class="sxs-lookup"><span data-stu-id="ce385-147">Parameters come from the calling method, not HTTP.</span></span> <span data-ttu-id="ce385-148">Hiçbir model bağlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="ce385-148">There's no model binding.</span></span>
* <span data-ttu-id="ce385-149">Doğrudan HTTP uç noktası ulaşılamıyor.</span><span class="sxs-lookup"><span data-stu-id="ce385-149">Are not reachable directly as an HTTP endpoint.</span></span> <span data-ttu-id="ce385-150">Bunlar kodunuzu (genellikle, bir görünüm) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="ce385-150">They're invoked from your code (usually in a view).</span></span> <span data-ttu-id="ce385-151">Bir görünümü bileşeni hiçbir zaman bir isteği işler.</span><span class="sxs-lookup"><span data-stu-id="ce385-151">A view component never handles a request.</span></span>
* <span data-ttu-id="ce385-152">Geçerli HTTP istek tüm ayrıntıları yerine imza aşırı yüklenmesi sebebiyledir.</span><span class="sxs-lookup"><span data-stu-id="ce385-152">Are overloaded on the signature rather than any details from the current HTTP request.</span></span>

### <a name="view-search-path"></a><span data-ttu-id="ce385-153">Görünüm arama yolu</span><span class="sxs-lookup"><span data-stu-id="ce385-153">View search path</span></span>

<span data-ttu-id="ce385-154">Çalışma zamanı aşağıdaki yollardan görünümünde arar:</span><span class="sxs-lookup"><span data-stu-id="ce385-154">The runtime searches for the view in the following paths:</span></span>

* <span data-ttu-id="ce385-155">{Denetleyici adı} /Views/ /Components/ {görünümü bileşen adı} / {View adı}</span><span class="sxs-lookup"><span data-stu-id="ce385-155">/Views/{Controller Name}/Components/{View Component Name}/{View Name}</span></span>
* <span data-ttu-id="ce385-156">/ Görünümler/paylaşılan/bileşenleri / {View bileşen adı} / {View adı}</span><span class="sxs-lookup"><span data-stu-id="ce385-156">/Views/Shared/Components/{View Component Name}/{View Name}</span></span>
* <span data-ttu-id="ce385-157">/ Sayfaları/paylaşılan/bileşenleri / {View bileşen adı} / {View adı}</span><span class="sxs-lookup"><span data-stu-id="ce385-157">/Pages/Shared/Components/{View Component Name}/{View Name}</span></span>

<span data-ttu-id="ce385-158">Arama yolu, denetleyicileri + görünümleri ve Razor sayfaları kullanan projeler için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="ce385-158">The search path applies to projects using controllers + views and Razor Pages.</span></span>

<span data-ttu-id="ce385-159">Bir görünümü bileşen için varsayılan görünüm adı *varsayılan*, yani dosyasını görüntüle genellikle adlandırılacağını *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ce385-159">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="ce385-160">Görünüm bileşen sonucu oluştururken veya çağırırken farklı görünüm adı belirtebilirsiniz `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ce385-160">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="ce385-161">Görünüm dosyası adı öneririz *Default.cshtml* ve *görünümler/paylaşılan/Components / {View bileşen adı} / {View Name}* yolu.</span><span class="sxs-lookup"><span data-stu-id="ce385-161">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/{View Component Name}/{View Name}* path.</span></span> <span data-ttu-id="ce385-162">`PriorityList` Bu örnekte kullanılan görünümü bileşen *Views/Shared/Components/PriorityList/Default.cshtml* görünümünü bileşeni için.</span><span class="sxs-lookup"><span data-stu-id="ce385-162">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="ce385-163">Bileşeni görüntüle çağırma</span><span class="sxs-lookup"><span data-stu-id="ce385-163">Invoking a view component</span></span>

<span data-ttu-id="ce385-164">Bileşeni görüntüle kullanmak için aşağıdaki çağrı görünümü içinde:</span><span class="sxs-lookup"><span data-stu-id="ce385-164">To use the view component, call the following inside a view:</span></span>

```cshtml
@await Component.InvokeAsync("Name of view component", {Anonymous Type Containing Parameters})
```

<span data-ttu-id="ce385-165">Parametreleri geçirilecek `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ce385-165">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="ce385-166">`PriorityList` Makalesinde geliştirilen görünümü bileşen ınvoked from *Views/ToDo/Index.cshtml* görünüm dosyası.</span><span class="sxs-lookup"><span data-stu-id="ce385-166">The `PriorityList` view component developed in the article is invoked from the *Views/ToDo/Index.cshtml* view file.</span></span> <span data-ttu-id="ce385-167">Aşağıdaki `InvokeAsync` yöntemi, iki parametre ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="ce385-167">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

::: moniker range=">= aspnetcore-1.1"

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="ce385-168">Bileşeni görüntüle etiket Yardımcısı çağırma</span><span class="sxs-lookup"><span data-stu-id="ce385-168">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="ce385-169">ASP.NET Core 1.1 ve sonraki bir görünüm bileşeni olarak çağırabilirsiniz bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="ce385-169">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="ce385-170">Pascal büyük küçük harfleri sınıf ve yöntem parametreleri etiket Yardımcıları için çevrilmiş halinde kullanıcıların [kebab çalışması](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="ce385-170">Pascal-cased class and method parameters for Tag Helpers are translated into their [kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="ce385-171">Bileşeni görüntüle çağırmak için etiket Yardımcısı kullanan `<vc></vc>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="ce385-171">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="ce385-172">Bileşeni görüntüle gibi belirtilir:</span><span class="sxs-lookup"><span data-stu-id="ce385-172">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="ce385-173">Bir görünümü bileşeni etiket Yardımcısı kullanılacak görünümünü kullanarak bileşeni içeren derlemenin kaydetme `@addTagHelper` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="ce385-173">To use a view component as a Tag Helper, register the assembly containing the view component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="ce385-174">Bileşeni görüntüle adlı bir derlemede ise `MyWebApp`, eklemek için aşağıdaki yönerge *_viewımports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ce385-174">If your view component is in an assembly called `MyWebApp`, add the following directive to the *_ViewImports.cshtml* file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="ce385-175">Bileşeni görüntüle görünümü bileşenine başvurduğunu herhangi bir dosyaya etiket Yardımcısı olarak kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce385-175">You can register a view component as a Tag Helper to any file that references the view component.</span></span> <span data-ttu-id="ce385-176">Bkz: [yönetme etiket Yardımcısı kapsam](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) etiket Yardımcıları kaydetme hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="ce385-176">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="ce385-177">`InvokeAsync` Bu öğreticide kullanılan yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ce385-177">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="ce385-178">Etiket Yardımcısı biçimlendirme içinde:</span><span class="sxs-lookup"><span data-stu-id="ce385-178">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="ce385-179">Yukarıdaki örnekteki `PriorityList` görünümü bileşen olur `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="ce385-179">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="ce385-180">Bileşeni görüntüle parametreleri, öznitelikleri kebab durumda olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="ce385-180">The parameters to the view component are passed as attributes in kebab case.</span></span>

::: moniker-end

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="ce385-181">Bir görünümü bileşen denetleyicisinden doğrudan çağırma</span><span class="sxs-lookup"><span data-stu-id="ce385-181">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="ce385-182">Görünüm bileşenleri, genellikle bir görünümden çağrılır, ancak doğrudan bir denetleyici yöntemi çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce385-182">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="ce385-183">Görünüm bileşenleri denetleyicileri gibi uç noktaları tanımlamak yoktur, ancak içeriği döndüren bir denetleyici eylemi kolayca uygulayabilirsiniz bir `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="ce385-183">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="ce385-184">Bu örnekte, doğrudan denetleyiciden görünüm bileşen çağrılır:</span><span class="sxs-lookup"><span data-stu-id="ce385-184">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="ce385-185">İzlenecek yol: Basit Görünüm bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="ce385-185">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="ce385-186">[İndirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample), derleme ve Başlatıcı kodu test edin.</span><span class="sxs-lookup"><span data-stu-id="ce385-186">[Download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="ce385-187">Basit bir proje olan bir `ToDo` listesini görüntüler denetleyicisi *ToDo* öğeleri.</span><span class="sxs-lookup"><span data-stu-id="ce385-187">It's a simple project with a `ToDo` controller that displays a list of *ToDo* items.</span></span>

![Açıklamada listesi](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="ce385-189">ViewComponent sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="ce385-189">Add a ViewComponent class</span></span>

<span data-ttu-id="ce385-190">Oluşturma bir *ViewComponents* klasörü ve aşağıdaki `PriorityListViewComponent` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ce385-190">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="ce385-191">Kod ile ilgili notlar:</span><span class="sxs-lookup"><span data-stu-id="ce385-191">Notes on the code:</span></span>

* <span data-ttu-id="ce385-192">Görünüm bileşen sınıfları kapsanıyorsa **herhangi** proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="ce385-192">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="ce385-193">Sınıf adı olduğundan PriorityList**ViewComponent** soneki ile sona erer **ViewComponent**, çalışma zamanı sınıf bileşen görünümden başvururken "PriorityList" dizesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="ce385-193">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="ce385-194">I, daha ayrıntılı olarak daha sonra denetleyeceğinizi açıklayacağız.</span><span class="sxs-lookup"><span data-stu-id="ce385-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="ce385-195">`[ViewComponent]` Öznitelik, bir görünüm bileşeni başvurmak için kullanılan adını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce385-195">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="ce385-196">Örneğin, biz adlı sınıfı `XYZ` ve uygulanan `ViewComponent` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="ce385-196">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="ce385-197">`[ViewComponent]` Özniteliği yukarıdaki söyler adı kullanacak şekilde görünümü Bileşen Seçici `PriorityList` bileşeni ile ve "PriorityList" dize sınıfı bileşen görünümden başvururken kullanılacak ilişkili görünümleri ararken.</span><span class="sxs-lookup"><span data-stu-id="ce385-197">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="ce385-198">I, daha ayrıntılı olarak daha sonra denetleyeceğinizi açıklayacağız.</span><span class="sxs-lookup"><span data-stu-id="ce385-198">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="ce385-199">Bileşen [bağımlılık ekleme](../../fundamentals/dependency-injection.md) veri bağlamı kullanılabilir hale getirmek için.</span><span class="sxs-lookup"><span data-stu-id="ce385-199">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="ce385-200">`InvokeAsync` kullanıma sunan bir görünümü ve onu çağrılabilen bir yöntem rastgele bir sayıda bağımsız değişken alabilir.</span><span class="sxs-lookup"><span data-stu-id="ce385-200">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="ce385-201">`InvokeAsync` Yöntemi kümesini döndürür `ToDo` karşılayan öğelerinin `isDone` ve `maxPriority` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="ce385-201">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="ce385-202">Bileşen Razor görünümünü oluşturma</span><span class="sxs-lookup"><span data-stu-id="ce385-202">Create the view component Razor view</span></span>

* <span data-ttu-id="ce385-203">Oluşturma *görünümler/paylaşılan/Components* klasör.</span><span class="sxs-lookup"><span data-stu-id="ce385-203">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="ce385-204">Bu klasör **gerekir** adı *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="ce385-204">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="ce385-205">Oluşturma *görünümler/paylaşılan/bileşenleri/PriorityList* klasör.</span><span class="sxs-lookup"><span data-stu-id="ce385-205">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="ce385-206">Bu klasör adı görünümü bileşen sınıfının adı ve soneki eksi sınıfının adı eşleşmelidir (kuralını izleyen ve kullandıysanız *ViewComponent* sınıf adı soneki).</span><span class="sxs-lookup"><span data-stu-id="ce385-206">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="ce385-207">Kullandıysanız `ViewComponent` özniteliği, sınıf adı öznitelik atamasını eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="ce385-207">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="ce385-208">Oluşturma bir *Views/Shared/Components/PriorityList/Default.cshtml* Razor Görünüm:</span><span class="sxs-lookup"><span data-stu-id="ce385-208">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view:</span></span>


  [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]

   <span data-ttu-id="ce385-209">Razor görünümü listesini alır `TodoItem` ve bunları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="ce385-209">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="ce385-210">Varsa görünümü bileşen `InvokeAsync` yöntemi (olduğu gibi örneğimizi), görünüm adını geçirin değil *varsayılan* görünümü adı için kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ce385-210">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="ce385-211">Öğreticinin sonraki bölümlerinde miyim, görünümün adının ona nasıl iletileceğini göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="ce385-211">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="ce385-212">Belirli bir denetleyicinin varsayılan stillerini geçersiz kılmak için bir görünüm denetleyicisine özgü görünüm klasöre ekleyin (örneğin *Views/ToDo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="ce385-212">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/ToDo/Components/PriorityList/Default.cshtml)*.</span></span>

    <span data-ttu-id="ce385-213">Denetleyicisine özgü görünüm bileşeni ise denetleyicisi özgü klasöre ekleyebilirsiniz (*Views/ToDo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="ce385-213">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/ToDo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="ce385-214">Ekleme bir `div` altına öncelik listesi bileşeni için bir çağrı içeren *Views/ToDo/index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ce385-214">Add a `div` containing a call to the priority list component to the bottom of the *Views/ToDo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="ce385-215">Biçimlendirme `@await Component.InvokeAsync` görünüm bileşenleri çağırma söz dizimi görülmektedir.</span><span class="sxs-lookup"><span data-stu-id="ce385-215">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="ce385-216">İlk bağımsız değişken çağırma veya çağrı istiyoruz bileşen adıdır.</span><span class="sxs-lookup"><span data-stu-id="ce385-216">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="ce385-217">Sonraki parametreler bileşenine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="ce385-217">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="ce385-218">`InvokeAsync` rastgele bir sayıda bağımsız değişken alabilir.</span><span class="sxs-lookup"><span data-stu-id="ce385-218">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="ce385-219">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="ce385-219">Test the app.</span></span> <span data-ttu-id="ce385-220">Aşağıdaki görüntüde, yapılacaklar listesi ve öncelikli öğeleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="ce385-220">The following image shows the ToDo list and the priority items:</span></span>

![Yapılacaklar listesi ve öncelikli öğeleri](view-components/_static/pi.png)

<span data-ttu-id="ce385-222">Doğrudan denetleyiciden görünüm bileşeni de çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ce385-222">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![öncelikli öğeleri IndexVC eylemi](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="ce385-224">Görünüm adı belirtme</span><span class="sxs-lookup"><span data-stu-id="ce385-224">Specifying a view name</span></span>

<span data-ttu-id="ce385-225">Karmaşık görünümü bileşen, bazı koşullar altında bir varsayılan olmayan görünüm belirtmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="ce385-225">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="ce385-226">Aşağıdaki kod "PVC" görünümünden belirteceğiniz gösterilmektedir `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ce385-226">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="ce385-227">Güncelleştirme `InvokeAsync` yönteminde `PriorityListViewComponent` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ce385-227">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="ce385-228">Kopyalama *Views/Shared/Components/PriorityList/Default.cshtml* adlı bir görünüm dosyasına *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ce385-228">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="ce385-229">PVC görünümü kullanıldığını belirtmek için bir başlık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ce385-229">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="ce385-230">Güncelleştirme *Views/ToDo/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ce385-230">Update *Views/ToDo/Index.cshtml*:</span></span>

<!-- Views/ToDo/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="ce385-231">Uygulamayı çalıştırın ve PVC görünümü doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ce385-231">Run the app and verify PVC view.</span></span>

![Öncelik bileşeni görüntüle](view-components/_static/pvc.png)

<span data-ttu-id="ce385-233">PVC görünüm işlenen değil, 4 veya daha yüksek bir önceliğe sahip görünümü bileşen aradığınız doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="ce385-233">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="ce385-234">Görünüm yolunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="ce385-234">Examine the view path</span></span>

* <span data-ttu-id="ce385-235">Öncelik parametresi, üç veya daha düşük öncelikli görünüm döndürülmez şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="ce385-235">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="ce385-236">Geçici olarak yeniden adlandırın *Views/ToDo/Components/PriorityList/Default.cshtml* için *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ce385-236">Temporarily rename the *Views/ToDo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="ce385-237">Uygulamayı test etme, aşağıdaki hatayı alırsınız:</span><span class="sxs-lookup"><span data-stu-id="ce385-237">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="ce385-238">Kopyalama *Views/ToDo/Components/PriorityList/1Default.cshtml* için *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ce385-238">Copy *Views/ToDo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="ce385-239">Bazı biçimlendirme eklemek *paylaşılan* ToDo görünümü bileşen görünümü, görünümün belirtmenizi geldiği *paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="ce385-239">Add some markup to the *Shared* ToDo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="ce385-240">Test **paylaşılan** bileşeni görünümü.</span><span class="sxs-lookup"><span data-stu-id="ce385-240">Test the **Shared** component view.</span></span>

![ToDo çıkış paylaşılan bileşen görünümü](view-components/_static/shared.png)

### <a name="avoiding-hard-coded-strings"></a><span data-ttu-id="ce385-242">Sabit kodlanmış dizeleri kaçınma</span><span class="sxs-lookup"><span data-stu-id="ce385-242">Avoiding hard-coded strings</span></span>

<span data-ttu-id="ce385-243">Zaman güvenlik derlemek isterseniz, sabit kodlanmış görünümü bileşen adı sınıf adını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ce385-243">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="ce385-244">Bileşeni görüntüle "ViewComponent" soneki olmadan oluşturun:</span><span class="sxs-lookup"><span data-stu-id="ce385-244">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="ce385-245">Ekleme bir `using` , Razor ifadesine dosyayı görüntüle ve Kullan `nameof` işleci:</span><span class="sxs-lookup"><span data-stu-id="ce385-245">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/ToDo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="perform-synchronous-work"></a><span data-ttu-id="ce385-246">Zaman uyumlu çalışma gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="ce385-246">Perform synchronous work</span></span>

<span data-ttu-id="ce385-247">Çerçeve işleme bir zaman uyumlu çağırma `Invoke` zaman uyumsuz çalışmayı gerçekleştirmek gerekmiyorsa yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ce385-247">The framework handles invoking a synchronous `Invoke` method if you don't need to perform asynchronous work.</span></span> <span data-ttu-id="ce385-248">Aşağıdaki yöntem zaman uyumlu bir oluşturur `Invoke` bileşeni görüntüle:</span><span class="sxs-lookup"><span data-stu-id="ce385-248">The following method creates a synchronous `Invoke` view component:</span></span>

```csharp
public class PriorityList : ViewComponent
{
    public IViewComponentResult Invoke(int maxPriority, bool isDone)
    {
        var items = new List<string> { $"maxPriority: {maxPriority}", $"isDone: {isDone}" };
        return View(items);
    }
}
```

<span data-ttu-id="ce385-249">Geçirilen dizeler görünümü bileşenin Razor dosyasını listeler `Invoke` yöntemi (*Views/Home/Components/PriorityList/Default.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="ce385-249">The view component's Razor file lists the strings passed to the `Invoke` method (*Views/Home/Components/PriorityList/Default.cshtml*):</span></span>

```cshtml
@model List<string>

<h3>Priority Items</h3>
<ul>
    @foreach (var item in Model)
    {
        <li>@item</li>
    }
</ul>
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="ce385-250">Bileşeni görüntüle Razor dosyasında çağrılır (örneğin, *Views/Home/Index.cshtml*) aşağıdaki yaklaşımlardan birini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="ce385-250">The view component is invoked in a Razor file (for example, *Views/Home/Index.cshtml*) using one of the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>
* [<span data-ttu-id="ce385-251">Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="ce385-251">Tag Helper</span></span>](xref:mvc/views/tag-helpers/intro)

<span data-ttu-id="ce385-252">Kullanılacak <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> yaklaşımı, çağrı `Component.InvokeAsync`:</span><span class="sxs-lookup"><span data-stu-id="ce385-252">To use the <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> approach, call `Component.InvokeAsync`:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-1.1"

<span data-ttu-id="ce385-253">Bileşeni görüntüle Razor dosyasında çağrılır (örneğin, *Views/Home/Index.cshtml*) ile <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.</span><span class="sxs-lookup"><span data-stu-id="ce385-253">The view component is invoked in a Razor file (for example, *Views/Home/Index.cshtml*) with <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.</span></span>

<span data-ttu-id="ce385-254">Çağrı `Component.InvokeAsync`:</span><span class="sxs-lookup"><span data-stu-id="ce385-254">Call `Component.InvokeAsync`:</span></span>

::: moniker-end

```cshtml
@await Component.InvokeAsync(nameof(PriorityList), new { maxPriority = 4, isDone = true })
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="ce385-255">Etiket Yardımcısı'nı kullanmak için görünümü bileşen kullanarak içeren derlemenin kaydetme `@addTagHelper` yönergesi (adlı bir derlemede görünümü bileşendir `MyWebApp`):</span><span class="sxs-lookup"><span data-stu-id="ce385-255">To use the Tag Helper, register the assembly containing the View Component using the `@addTagHelper` directive (the view component is in an assembly called `MyWebApp`):</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="ce385-256">Etiket Yardımcısı görünümü bileşen Razor biçimlendirme dosyasında kullanın:</span><span class="sxs-lookup"><span data-stu-id="ce385-256">Use the view component Tag Helper in the Razor markup file:</span></span>

```cshtml
<vc:priority-list max-priority="999" is-done="false">
</vc:priority-list>
```

::: moniker-end

<span data-ttu-id="ce385-257">Yöntem imzası `PriorityList.Invoke` zaman uyumludur, ancak Razor bulur ve içeren yöntemi çağıran `Component.InvokeAsync` biçimlendirme dosyası.</span><span class="sxs-lookup"><span data-stu-id="ce385-257">The method signature of `PriorityList.Invoke` is synchronous, but Razor finds and calls the method with `Component.InvokeAsync` in the markup file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ce385-258">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="ce385-258">Additional resources</span></span>

* [<span data-ttu-id="ce385-259">Görünümlere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="ce385-259">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
