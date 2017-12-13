---
title: "Görünüm bileşenleri"
author: rick-anderson
description: "Görünüm bileşenleri yeniden kullanılabilir işleme mantığı sahip herhangi bir yere yöneliktir."
keywords: "ASP.NET Core, görünümü bileşenler, kısmi görünümü"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 2cf82df78c250cdfdd808d49acfc06dc2ea82f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="view-components"></a><span data-ttu-id="da8f8-104">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="da8f8-104">View components</span></span>

<span data-ttu-id="da8f8-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="da8f8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="da8f8-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="da8f8-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="da8f8-107">Görünüm bileşenleri Tanıtımı</span><span class="sxs-lookup"><span data-stu-id="da8f8-107">Introducing view components</span></span>

<span data-ttu-id="da8f8-108">Yeni ASP.NET Core MVC için Görünüm bileşenleri için kısmi görünümler benzerdir, ancak çok daha güçlü.</span><span class="sxs-lookup"><span data-stu-id="da8f8-108">New to ASP.NET Core MVC, view components are similar to partial views, but they are much more powerful.</span></span> <span data-ttu-id="da8f8-109">Görünüm bileşenleri yoksa model bağlama kullanın ve yalnızca içine çağrılırken sağladığınız verilerin bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="da8f8-109">View components don’t use model binding, and only depend on the data you provide when calling into it.</span></span> <span data-ttu-id="da8f8-110">Bir görünümü bileşen:</span><span class="sxs-lookup"><span data-stu-id="da8f8-110">A view component:</span></span>

* <span data-ttu-id="da8f8-111">Yanıtın tamamını yerine bir öbek işler</span><span class="sxs-lookup"><span data-stu-id="da8f8-111">Renders a chunk rather than a whole response</span></span>
* <span data-ttu-id="da8f8-112">Bir denetleyici ve görünüm arasında bulunan Test Edilebilirlik avantajları ve aynı ayrımı-in-ile ilgili sorunlar içerir</span><span class="sxs-lookup"><span data-stu-id="da8f8-112">Includes the same separation-of-concerns and testability benefits found between a controller and view</span></span>
* <span data-ttu-id="da8f8-113">Parametreleri ve iş mantığı olabilir</span><span class="sxs-lookup"><span data-stu-id="da8f8-113">Can have parameters and business logic</span></span>
* <span data-ttu-id="da8f8-114">Tipik bir düzen sayfasından çağrılır</span><span class="sxs-lookup"><span data-stu-id="da8f8-114">Is typically invoked from a layout page</span></span>

<span data-ttu-id="da8f8-115">Görünüm bileşenleri herhangi bir yere kısmi görünüm için çok karmaşık olduğu gibi yeniden kullanılabilir işleme mantığı sahip tasarlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="da8f8-115">View components are intended anywhere you have reusable rendering logic that is too complex for a partial view, such as:</span></span>

* <span data-ttu-id="da8f8-116">Dinamik Gezinti menüleri</span><span class="sxs-lookup"><span data-stu-id="da8f8-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="da8f8-117">(Burada veritabanını sorgular) Etiket Bulutu</span><span class="sxs-lookup"><span data-stu-id="da8f8-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="da8f8-118">Oturum açma paneli</span><span class="sxs-lookup"><span data-stu-id="da8f8-118">Login panel</span></span>
* <span data-ttu-id="da8f8-119">Alışveriş sepeti</span><span class="sxs-lookup"><span data-stu-id="da8f8-119">Shopping cart</span></span>
* <span data-ttu-id="da8f8-120">Son zamanlarda yayımlanan makaleleri</span><span class="sxs-lookup"><span data-stu-id="da8f8-120">Recently published articles</span></span>
* <span data-ttu-id="da8f8-121">Tipik bir blog kenar içerik</span><span class="sxs-lookup"><span data-stu-id="da8f8-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="da8f8-122">Her sayfada işlenir ve oturum kapatma veya bağlı durumda olan kullanıcının günlük olarak oturum açma ya da bağlantılarını göster bir oturum açma paneli</span><span class="sxs-lookup"><span data-stu-id="da8f8-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="da8f8-123">Bir görünümü bileşen iki bölümden oluşur: sınıfı (genellikle türetilmiş [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) ve isteğe bağlı olarak sonucu (genellikle bir görünüm) döndürür.</span><span class="sxs-lookup"><span data-stu-id="da8f8-123">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="da8f8-124">Denetleyicileri gibi bir POCO görünümü bileşen olabilir, ancak çoğu Geliştirici türetme tarafından kullanılabilen özellikleri ve yöntemleri yararlanmak istersiniz `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="da8f8-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="da8f8-125">Bir görünüm bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="da8f8-125">Creating a view component</span></span>

<span data-ttu-id="da8f8-126">Bu bölümde bir görünümü bileşen oluşturmak için üst düzey gereksinimleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="da8f8-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="da8f8-127">Makalenin sonraki bölümlerinde, biz her adım ayrıntılı inceleyin ve bir görünüm bileşeni oluşturma.</span><span class="sxs-lookup"><span data-stu-id="da8f8-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="da8f8-128">Görünüm bileşen sınıfı</span><span class="sxs-lookup"><span data-stu-id="da8f8-128">The view component class</span></span>

<span data-ttu-id="da8f8-129">Bir görünümü bileşen sınıfı aşağıdakilerden herhangi biri tarafından oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="da8f8-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="da8f8-130">Türetme *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="da8f8-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="da8f8-131">Bir sınıfla dekorasyon `[ViewComponent]` özniteliği ya da bir sınıf türetme `[ViewComponent]` özniteliği</span><span class="sxs-lookup"><span data-stu-id="da8f8-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="da8f8-132">Bir sınıf adı sonekiyle sona ereceği oluşturma *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="da8f8-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="da8f8-133">Denetleyicileri gibi görünüm bileşenleri genel, iç içe olmayan ve Özet olmayan sınıflar olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="da8f8-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="da8f8-134">Görünüm bileşen adı kaldırıldı "ViewComponent" soneki ile sınıfı adıdır.</span><span class="sxs-lookup"><span data-stu-id="da8f8-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="da8f8-135">Bu aynı zamanda açıkça kullanılarak belirtilebilir `ViewComponentAttribute.Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="da8f8-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="da8f8-136">Bir görünüm bileşenin sınıfı:</span><span class="sxs-lookup"><span data-stu-id="da8f8-136">A view component class:</span></span>

* <span data-ttu-id="da8f8-137">Tam olarak Oluşturucusu destekleyen [bağımlılık ekleme](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="da8f8-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="da8f8-138">Bölümü kullanamazsınız anlamına gelir denetleyicisi çevriminin almaz [filtreleri](../controllers/filters.md) bir görünüm bileşeninde</span><span class="sxs-lookup"><span data-stu-id="da8f8-138">Does not take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="da8f8-139">Görünüm bileşen yöntemleri</span><span class="sxs-lookup"><span data-stu-id="da8f8-139">View component methods</span></span>

<span data-ttu-id="da8f8-140">Bir görünüm bileşeni, mantığını tanımlayan bir `InvokeAsync` döndüren yöntemi bir `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="da8f8-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="da8f8-141">Parametreleri doğrudan çağırma görünüm bileşeninin değil, model bağlama gelmektedir.</span><span class="sxs-lookup"><span data-stu-id="da8f8-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="da8f8-142">Bir görünüm bileşeni hiçbir zaman doğrudan bir isteği işler.</span><span class="sxs-lookup"><span data-stu-id="da8f8-142">A view component never directly handles a request.</span></span> <span data-ttu-id="da8f8-143">Genellikle, bir görünüm bileşeni bir model başlatır ve çağırarak bir görünümüne geçirir `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="da8f8-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="da8f8-144">Özet olarak, bileşen yöntemlerini görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="da8f8-144">In summary, view component methods:</span></span>

* <span data-ttu-id="da8f8-145">Tanımlayan bir `InvokeAsync` döndürür yöntemi bir`IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="da8f8-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="da8f8-146">Genellikle bir model başlatır ve çağırarak bir görünümüne geçirir `ViewComponent` `View` yöntemi</span><span class="sxs-lookup"><span data-stu-id="da8f8-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="da8f8-147">Arama yöntemi, değil HTTP parametreleri gelir, model bağlama yok</span><span class="sxs-lookup"><span data-stu-id="da8f8-147">Parameters come from the calling method, not HTTP, there is no model binding</span></span>
* <span data-ttu-id="da8f8-148">Olan bir HTTP uç noktası olarak doğrudan ulaşılabilir değil, bunlar (genellikle bir görünümde) kodunuzdan çağrılan.</span><span class="sxs-lookup"><span data-stu-id="da8f8-148">Are not reachable directly as an HTTP endpoint, they are invoked from your code (usually in a view).</span></span> <span data-ttu-id="da8f8-149">Bir görünüm bileşeni hiçbir zaman bir isteği işler</span><span class="sxs-lookup"><span data-stu-id="da8f8-149">A view component never handles a request</span></span>
* <span data-ttu-id="da8f8-150">Geçerli HTTP isteği herhangi bir ayrıntıyı yerine imza aşırı</span><span class="sxs-lookup"><span data-stu-id="da8f8-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="da8f8-151">Görünüm arama yolu</span><span class="sxs-lookup"><span data-stu-id="da8f8-151">View search path</span></span>

<span data-ttu-id="da8f8-152">Aşağıdaki yolları görünümünde çalışma zamanı arar:</span><span class="sxs-lookup"><span data-stu-id="da8f8-152">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="da8f8-153">Görünümler /\<controller_name > /Components/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="da8f8-153">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="da8f8-154">Görünümler/paylaşılan/bileşenleri/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="da8f8-154">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="da8f8-155">Bir görünüm bileşeni için varsayılan görünüm adı *varsayılan*, görünüm dosyanızı başka bir deyişle, tipik olarak adlandırılacaktır *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="da8f8-155">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="da8f8-156">Görünüm bileşeni sonuç oluştururken veya çağrılırken farklı görünüm adı belirtebilirsiniz `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="da8f8-156">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="da8f8-157">Görünüm dosyası adı öneririz *Default.cshtml* ve *görünümler/paylaşılan/bileşenleri/\<view_component_name > /\<view_name >* yolu.</span><span class="sxs-lookup"><span data-stu-id="da8f8-157">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="da8f8-158">`PriorityList` Bu örnekte kullanılan görünümü bileşen *Views/Shared/Components/PriorityList/Default.cshtml* görünümünü bileşeni için.</span><span class="sxs-lookup"><span data-stu-id="da8f8-158">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="da8f8-159">Bir görünümü bileşen çağırma</span><span class="sxs-lookup"><span data-stu-id="da8f8-159">Invoking a view component</span></span>

<span data-ttu-id="da8f8-160">Görünümü bileşen kullanmak için aşağıdaki çağrı görünümü içinde:</span><span class="sxs-lookup"><span data-stu-id="da8f8-160">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="da8f8-161">Parametreleri geçirilecek `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="da8f8-161">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="da8f8-162">`PriorityList` Makalesinde geliştirilen görünümü bileşen çağrılan *Views/Todo/Index.cshtml* görünüm dosyası.</span><span class="sxs-lookup"><span data-stu-id="da8f8-162">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="da8f8-163">Aşağıdaki `InvokeAsync` yöntemi iki parametre ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="da8f8-163">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="da8f8-164">Bir görünümü bileşen etiket Yardımcısı olarak çağırma</span><span class="sxs-lookup"><span data-stu-id="da8f8-164">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="da8f8-165">ASP.NET Core 1.1 ve üzeri, bir görünüm bileşeni olarak çağırabileceği bir [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="da8f8-165">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="da8f8-166">Pascal ortası sınıfı ve yöntem parametreleri etiket Yardımcıları için çevrilen içine kendi [alt kebab durumda](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="da8f8-166">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="da8f8-167">Bir görünümü bileşen çağrılacak etiket Yardımcısı kullanan `<vc></vc>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="da8f8-167">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="da8f8-168">Görünüm bileşeni gibi belirtilir:</span><span class="sxs-lookup"><span data-stu-id="da8f8-168">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="da8f8-169">Not: bir görünüm bileşeni etiket Yardımcısı olarak kullanmak için Görünüm bileşenini kullanarak içeren derlemenin kaydetmeniz gerekir `@addTagHelper` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="da8f8-169">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="da8f8-170">Örneğin, görünüm bileşeniniz "Mywebapp şeklindedir" adlı bir derleme varsa, aşağıdaki yönergesi ekleyin. `_ViewImports.cshtml` dosyası:</span><span class="sxs-lookup"><span data-stu-id="da8f8-170">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="da8f8-171">Bir görünüm bileşeni görünümü bileşen başvuran herhangi bir dosyaya etiketi yardımcı olarak kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da8f8-171">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="da8f8-172">Bkz: [yönetme etiket Yardımcısı kapsam](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) etiket Yardımcıları kaydetme hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="da8f8-172">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="da8f8-173">`InvokeAsync` Bu öğreticide kullanılan yöntem:</span><span class="sxs-lookup"><span data-stu-id="da8f8-173">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="da8f8-174">Etiket Yardımcısı biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="da8f8-174">In Tag Helper markup:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="da8f8-175">Yukarıdaki örnekteki `PriorityList` görünümü bileşen olur `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="da8f8-175">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="da8f8-176">Görünümü bileşen parametreleri alt kebab durumda öznitelik olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="da8f8-176">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="da8f8-177">Bir görünümü bileşen denetleyicisinden doğrudan çağırma</span><span class="sxs-lookup"><span data-stu-id="da8f8-177">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="da8f8-178">Görünümü bileşenler genellikle bir görünümden çağrılır, ancak doğrudan denetleyici yönteminden çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="da8f8-178">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="da8f8-179">Görünüm bileşenleri denetleyicileri gibi uç tanımlamıyor olsa da, içeriği döndüren bir denetleyici eylemi kolayca uygulayabilirsiniz bir `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="da8f8-179">While view components do not define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="da8f8-180">Bu örnekte, görünümü bileşen doğrudan denetleyicisinden çağrılır:</span><span class="sxs-lookup"><span data-stu-id="da8f8-180">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="da8f8-181">İzlenecek yol: basit bir görünümle bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="da8f8-181">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="da8f8-182">[Karşıdan](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), yapı ve test starter kodunun.</span><span class="sxs-lookup"><span data-stu-id="da8f8-182">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="da8f8-183">Basit bir projeyle olan bir `Todo` listesini görüntüler denetleyicisi *Yapılacaklar* öğeleri.</span><span class="sxs-lookup"><span data-stu-id="da8f8-183">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![ToDos listesi](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="da8f8-185">ViewComponent sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="da8f8-185">Add a ViewComponent class</span></span>

<span data-ttu-id="da8f8-186">Oluşturma bir *ViewComponents* klasörü ve aşağıdaki ekleyin `PriorityListViewComponent` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="da8f8-186">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="da8f8-187">Kodu Notlar:</span><span class="sxs-lookup"><span data-stu-id="da8f8-187">Notes on the code:</span></span>

* <span data-ttu-id="da8f8-188">Görünümü bileşen sınıfları yer almalıdır içinde **herhangi** proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="da8f8-188">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="da8f8-189">Sınıf adı olduğundan PriorityList**ViewComponent** sonekiyle sona erer **ViewComponent**, çalışma zamanı sınıf bileşen bir görünümden başvururken dizesi "PriorityList" kullanır.</span><span class="sxs-lookup"><span data-stu-id="da8f8-189">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="da8f8-190">I, daha ayrıntılı olarak daha sonra açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="da8f8-190">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="da8f8-191">`[ViewComponent]` Öznitelik görünümü bileşen başvurmak için kullanılan adını değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="da8f8-191">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="da8f8-192">Örneğin, biz sınıfı adlandırdığınız `XYZ` ve uygulanan `ViewComponent` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="da8f8-192">For example, we could have named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="da8f8-193">`[ViewComponent]` Yukarıda öznitelik adı kullanmak için görünümü bileşen Seçicisi söyler `PriorityList` bileşeni ile ve sınıf bileşen bir görünümden başvururken dizesi "PriorityList" kullanmak için ilişkili görünümleri ararken.</span><span class="sxs-lookup"><span data-stu-id="da8f8-193">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="da8f8-194">I, daha ayrıntılı olarak daha sonra açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="da8f8-194">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="da8f8-195">Bileşen [bağımlılık ekleme](../../fundamentals/dependency-injection.md) veri bağlamı kullanılabilmesi için.</span><span class="sxs-lookup"><span data-stu-id="da8f8-195">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="da8f8-196">`InvokeAsync`çıkarır rastgele sayıda bağımsız değişken bir görünüm ve onu denilen bir yöntem alabilir.</span><span class="sxs-lookup"><span data-stu-id="da8f8-196">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="da8f8-197">`InvokeAsync` Yöntemi döndürür kümesini `ToDo` karşılamak öğeleri `isDone` ve `maxPriority` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="da8f8-197">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="da8f8-198">Görünümü bileşen Razor görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="da8f8-198">Create the view component Razor view</span></span>

* <span data-ttu-id="da8f8-199">Oluşturma *görünümler/paylaşılan/bileşenleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="da8f8-199">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="da8f8-200">Bu klasör **gerekir** adlı *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="da8f8-200">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="da8f8-201">Oluşturma *görünümler/paylaşılan/bileşenleri/PriorityList* klasör.</span><span class="sxs-lookup"><span data-stu-id="da8f8-201">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="da8f8-202">Bu klasör adı görünümü bileşen sınıfı adını ya da soneki eksi sınıfın adı eşleşmelidir (kuralı izleyen ve kullandıysanız *ViewComponent* sınıf adı soneki).</span><span class="sxs-lookup"><span data-stu-id="da8f8-202">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="da8f8-203">Kullandıysanız `ViewComponent` özniteliği, sınıf adını öznitelik ataması eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="da8f8-203">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="da8f8-204">Oluşturma bir *Views/Shared/Components/PriorityList/Default.cshtml* Razor görünümü:[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="da8f8-204">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="da8f8-205">Razor görünüm bir listesini alır `TodoItem` ve bunları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="da8f8-205">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="da8f8-206">Varsa görünümü bileşen `InvokeAsync` yöntemi (olduğu gibi bizim örnek), görünümün adını geçirmek değil *varsayılan* Görünüm adı için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="da8f8-206">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="da8f8-207">Öğreticide daha sonra t, görünümün adını geçirmek nasıl göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="da8f8-207">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="da8f8-208">Belirli bir denetleyicinin varsayılan stil geçersiz kılmak için denetleyici özel görünüm klasöre bir görünüm ekleyin (örneğin *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="da8f8-208">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="da8f8-209">Görünüm Bileşen Denetleyicisi özgü ise, denetleyici özgü klasörüne ekleyebilirsiniz (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="da8f8-209">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="da8f8-210">Ekleme bir `div` altına öncelik liste bileşeni için bir çağrı içeren *Views/Todo/index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="da8f8-210">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="da8f8-211">İşaretleme `@await Component.InvokeAsync` görünüm bileşenleri çağırma söz dizimi görülmektedir.</span><span class="sxs-lookup"><span data-stu-id="da8f8-211">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="da8f8-212">İlk bağımsız değişken çağrılamadı veya çağrı istiyoruz bileşen adıdır.</span><span class="sxs-lookup"><span data-stu-id="da8f8-212">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="da8f8-213">Sonraki parametreler bileşenine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="da8f8-213">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="da8f8-214">`InvokeAsync`rastgele sayıda bağımsız değişken alabilir.</span><span class="sxs-lookup"><span data-stu-id="da8f8-214">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="da8f8-215">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="da8f8-215">Test the app.</span></span> <span data-ttu-id="da8f8-216">Aşağıdaki resimde, yapılacaklar listesi ve öncelik öğeleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="da8f8-216">The following image shows the ToDo list and the priority items:</span></span>

![Yapılacaklar listesi ve öncelik öğeleri](view-components/_static/pi.png)

<span data-ttu-id="da8f8-218">Ayrıca, doğrudan denetleyicisinden görünümü bileşen çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="da8f8-218">You can also call the view component directly from the controller:</span></span>

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC eylemden öncelikli öğeler](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="da8f8-220">Bir görünüm adı belirtme</span><span class="sxs-lookup"><span data-stu-id="da8f8-220">Specifying a view name</span></span>

<span data-ttu-id="da8f8-221">Karmaşık görünümü bileşen, varsayılan olmayan görünüm bazı koşullar altında belirtmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="da8f8-221">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="da8f8-222">Aşağıdaki kod "PVC" görünümünden belirtme gösterir `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="da8f8-222">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="da8f8-223">Güncelleştirme `InvokeAsync` yönteminde `PriorityListViewComponent` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="da8f8-223">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="da8f8-224">Kopya *Views/Shared/Components/PriorityList/Default.cshtml* adlı bir görünümü dosyasına *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="da8f8-224">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="da8f8-225">PVC görünümü kullanılan belirtmek için bir başlık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="da8f8-225">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="da8f8-226">Güncelleştirme *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="da8f8-226">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="da8f8-227">Uygulamayı çalıştırın ve PVC görünüm doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="da8f8-227">Run the app and verify PVC view.</span></span>

![Öncelik görünümü bileşen](view-components/_static/pvc.png)

<span data-ttu-id="da8f8-229">PVC görünüm işlenmez, 4 veya daha yüksek önceliğe sahip görünümü bileşen aradığınız doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="da8f8-229">If the PVC view is not rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="da8f8-230">Görünüm yolu inceleyin</span><span class="sxs-lookup"><span data-stu-id="da8f8-230">Examine the view path</span></span>

* <span data-ttu-id="da8f8-231">Priority parametresi, üç veya daha az öncelik görünüm alınmadı şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="da8f8-231">Change the priority parameter to three or less so the priority view is not returned.</span></span>
* <span data-ttu-id="da8f8-232">Geçici olarak yeniden adlandırın *Views/Todo/Components/PriorityList/Default.cshtml* için *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="da8f8-232">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="da8f8-233">Uygulamayı test etme, aşağıdaki hatayı alırsınız:</span><span class="sxs-lookup"><span data-stu-id="da8f8-233">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="da8f8-234">Kopya *Views/Todo/Components/PriorityList/1Default.cshtml* için *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="da8f8-234">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="da8f8-235">Bazı biçimlendirmeleri eklemek *paylaşılan* görünümü belirtmek için yapılacaklar bileşen görünümünü geldiği *paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="da8f8-235">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="da8f8-236">Test **paylaşılan** bileşeni görünümü.</span><span class="sxs-lookup"><span data-stu-id="da8f8-236">Test the **Shared** component view.</span></span>

![Paylaşılan bileşeni görünümü Yapılacaklar çıkışı](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="da8f8-238">Sihirli dizeleri önleme</span><span class="sxs-lookup"><span data-stu-id="da8f8-238">Avoiding magic strings</span></span>

<span data-ttu-id="da8f8-239">Zaman güvenliği derleme istiyorsanız, sabit kodlanmış görünümü bileşen adı sınıf adıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="da8f8-239">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="da8f8-240">"ViewComponent" soneki olmayan görünümü bileşen oluşturun:</span><span class="sxs-lookup"><span data-stu-id="da8f8-240">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="da8f8-241">Ekleme bir `using` , Razor ifadesine dosya görüntülemek ve kullanmak `nameof` işleci:</span><span class="sxs-lookup"><span data-stu-id="da8f8-241">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="da8f8-242">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="da8f8-242">Additional Resources</span></span>

* [<span data-ttu-id="da8f8-243">Görünümler içinde bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="da8f8-243">Dependency injection into views</span></span>](dependency-injection.md)
