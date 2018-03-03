---
title: "Görünüm bileşenleri"
author: rick-anderson
description: "Görünüm bileşenleri yeniden kullanılabilir işleme mantığı sahip herhangi bir yere yöneliktir."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-components
ms.openlocfilehash: 2db6c22c27bad5a242771a6e44ef5e0fa8f77395
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="view-components"></a><span data-ttu-id="3be89-103">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="3be89-103">View components</span></span>

<span data-ttu-id="3be89-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3be89-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3be89-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3be89-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introducing-view-components"></a><span data-ttu-id="3be89-106">Görünüm bileşenleri Tanıtımı</span><span class="sxs-lookup"><span data-stu-id="3be89-106">Introducing view components</span></span>

<span data-ttu-id="3be89-107">Yeni ASP.NET Core MVC için Görünüm bileşenleri için kısmi görünümler benzerdir, ancak çok daha güçlü.</span><span class="sxs-lookup"><span data-stu-id="3be89-107">New to ASP.NET Core MVC, view components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="3be89-108">Görünüm bileşenleri yoksa model bağlama kullanın ve yalnızca içine çağrılırken sağlanan verileri bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="3be89-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="3be89-109">Bir görünümü bileşen:</span><span class="sxs-lookup"><span data-stu-id="3be89-109">A view component:</span></span>

* <span data-ttu-id="3be89-110">Yanıtın tamamını yerine bir öbek işler.</span><span class="sxs-lookup"><span data-stu-id="3be89-110">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="3be89-111">Bir denetleyici ve görünüm arasında bulunan Test Edilebilirlik avantajları ve aynı ayrımı-in-ile ilgili sorunlar içerir.</span><span class="sxs-lookup"><span data-stu-id="3be89-111">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="3be89-112">Parametreleri ve iş mantığı sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="3be89-112">Can have parameters and business logic.</span></span>
* <span data-ttu-id="3be89-113">Tipik bir düzen sayfasından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="3be89-113">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="3be89-114">Görünüm bileşenleri herhangi bir yere kısmi görünüm için çok karmaşık olduğu gibi yeniden kullanılabilir işleme mantığı sahip tasarlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="3be89-114">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="3be89-115">Dinamik Gezinti menüleri</span><span class="sxs-lookup"><span data-stu-id="3be89-115">Dynamic navigation menus</span></span>
* <span data-ttu-id="3be89-116">(Burada veritabanını sorgular) Etiket Bulutu</span><span class="sxs-lookup"><span data-stu-id="3be89-116">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="3be89-117">Oturum açma paneli</span><span class="sxs-lookup"><span data-stu-id="3be89-117">Login panel</span></span>
* <span data-ttu-id="3be89-118">Alışveriş sepeti</span><span class="sxs-lookup"><span data-stu-id="3be89-118">Shopping cart</span></span>
* <span data-ttu-id="3be89-119">Son zamanlarda yayımlanan makaleleri</span><span class="sxs-lookup"><span data-stu-id="3be89-119">Recently published articles</span></span>
* <span data-ttu-id="3be89-120">Tipik bir blog kenar içerik</span><span class="sxs-lookup"><span data-stu-id="3be89-120">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="3be89-121">Her sayfada işlenir ve oturum kapatma veya bağlı durumda olan kullanıcının günlük olarak oturum açma ya da bağlantılarını göster bir oturum açma paneli</span><span class="sxs-lookup"><span data-stu-id="3be89-121">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="3be89-122">Bir görünümü bileşen iki bölümden oluşur: sınıfı (genellikle türetilmiş [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) ve isteğe bağlı olarak sonucu (genellikle bir görünüm) döndürür.</span><span class="sxs-lookup"><span data-stu-id="3be89-122">A view component consists of two parts: the class (typically derived from [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="3be89-123">Denetleyicileri gibi bir POCO görünümü bileşen olabilir, ancak çoğu Geliştirici türetme tarafından kullanılabilen özellikleri ve yöntemleri yararlanmak istersiniz `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="3be89-123">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="3be89-124">Bir görünüm bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="3be89-124">Creating a view component</span></span>

<span data-ttu-id="3be89-125">Bu bölümde bir görünümü bileşen oluşturmak için üst düzey gereksinimleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="3be89-125">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="3be89-126">Makalenin sonraki bölümlerinde, biz her adım ayrıntılı inceleyin ve bir görünüm bileşeni oluşturma.</span><span class="sxs-lookup"><span data-stu-id="3be89-126">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="3be89-127">Görünüm bileşen sınıfı</span><span class="sxs-lookup"><span data-stu-id="3be89-127">The view component class</span></span>

<span data-ttu-id="3be89-128">Bir görünümü bileşen sınıfı aşağıdakilerden herhangi biri tarafından oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="3be89-128">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="3be89-129">Türetme *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="3be89-129">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="3be89-130">Bir sınıfla dekorasyon `[ViewComponent]` özniteliği ya da bir sınıf türetme `[ViewComponent]` özniteliği</span><span class="sxs-lookup"><span data-stu-id="3be89-130">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="3be89-131">Bir sınıf adı sonekiyle sona ereceği oluşturma *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="3be89-131">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="3be89-132">Denetleyicileri gibi görünüm bileşenleri genel, iç içe olmayan ve Özet olmayan sınıflar olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3be89-132">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="3be89-133">Görünüm bileşen adı kaldırıldı "ViewComponent" soneki ile sınıfı adıdır.</span><span class="sxs-lookup"><span data-stu-id="3be89-133">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="3be89-134">Bu aynı zamanda açıkça kullanılarak belirtilebilir `ViewComponentAttribute.Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="3be89-134">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="3be89-135">Bir görünüm bileşenin sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3be89-135">A view component class:</span></span>

* <span data-ttu-id="3be89-136">Tam olarak Oluşturucusu destekleyen [bağımlılık ekleme](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="3be89-136">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="3be89-137">Bölümü kullanamazsınız anlamına gelir denetleyicisi çevriminin almaz [filtreleri](../controllers/filters.md) bir görünüm bileşeninde</span><span class="sxs-lookup"><span data-stu-id="3be89-137">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="3be89-138">Görünüm bileşen yöntemleri</span><span class="sxs-lookup"><span data-stu-id="3be89-138">View component methods</span></span>

<span data-ttu-id="3be89-139">Bir görünüm bileşeni, mantığını tanımlayan bir `InvokeAsync` döndüren yöntemi bir `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="3be89-139">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="3be89-140">Parametreleri doğrudan çağırma görünüm bileşeninin değil, model bağlama gelmektedir.</span><span class="sxs-lookup"><span data-stu-id="3be89-140">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="3be89-141">Bir görünüm bileşeni hiçbir zaman doğrudan bir isteği işler.</span><span class="sxs-lookup"><span data-stu-id="3be89-141">A view component never directly handles a request.</span></span> <span data-ttu-id="3be89-142">Genellikle, bir görünüm bileşeni bir model başlatır ve çağırarak bir görünümüne geçirir `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3be89-142">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="3be89-143">Özet olarak, bileşen yöntemlerini görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="3be89-143">In summary, view component methods:</span></span>

* <span data-ttu-id="3be89-144">Tanımlayan bir `InvokeAsync` döndürür yöntemi bir `IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="3be89-144">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="3be89-145">Genellikle bir model başlatır ve çağırarak bir görünümüne geçirir `ViewComponent` `View` yöntemi</span><span class="sxs-lookup"><span data-stu-id="3be89-145">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="3be89-146">Arama yöntemi, değil HTTP parametreleri gelir, model bağlama yok</span><span class="sxs-lookup"><span data-stu-id="3be89-146">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="3be89-147">Olan bir HTTP uç noktası olarak doğrudan ulaşılabilir değil, bunlar (genellikle bir görünümde) kodunuzdan çağrılan.</span><span class="sxs-lookup"><span data-stu-id="3be89-147">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="3be89-148">Bir görünüm bileşeni hiçbir zaman bir isteği işler</span><span class="sxs-lookup"><span data-stu-id="3be89-148">A view component never handles a request</span></span>
* <span data-ttu-id="3be89-149">Geçerli HTTP isteği herhangi bir ayrıntıyı yerine imza aşırı</span><span class="sxs-lookup"><span data-stu-id="3be89-149">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="3be89-150">Görünüm arama yolu</span><span class="sxs-lookup"><span data-stu-id="3be89-150">View search path</span></span>

<span data-ttu-id="3be89-151">Aşağıdaki yolları görünümünde çalışma zamanı arar:</span><span class="sxs-lookup"><span data-stu-id="3be89-151">The runtime searches for the view in the following paths:</span></span>

   * <span data-ttu-id="3be89-152">Görünümler /\<controller_name > /Components/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="3be89-152">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
   * <span data-ttu-id="3be89-153">Görünümler/paylaşılan/bileşenleri/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="3be89-153">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="3be89-154">Bir görünüm bileşeni için varsayılan görünüm adı *varsayılan*, görünüm dosyanızı başka bir deyişle, tipik olarak adlandırılacaktır *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3be89-154">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="3be89-155">Görünüm bileşeni sonuç oluştururken veya çağrılırken farklı görünüm adı belirtebilirsiniz `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3be89-155">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="3be89-156">Görünüm dosyası adı öneririz *Default.cshtml* ve *görünümler/paylaşılan/bileşenleri/\<view_component_name > /\<view_name >* yolu.</span><span class="sxs-lookup"><span data-stu-id="3be89-156">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="3be89-157">`PriorityList` Bu örnekte kullanılan görünümü bileşen *Views/Shared/Components/PriorityList/Default.cshtml* görünümünü bileşeni için.</span><span class="sxs-lookup"><span data-stu-id="3be89-157">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="3be89-158">Bir görünümü bileşen çağırma</span><span class="sxs-lookup"><span data-stu-id="3be89-158">Invoking a view component</span></span>

<span data-ttu-id="3be89-159">Görünümü bileşen kullanmak için aşağıdaki çağrı görünümü içinde:</span><span class="sxs-lookup"><span data-stu-id="3be89-159">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="3be89-160">Parametreleri geçirilecek `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3be89-160">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="3be89-161">`PriorityList` Makalesinde geliştirilen görünümü bileşen çağrılan *Views/Todo/Index.cshtml* görünüm dosyası.</span><span class="sxs-lookup"><span data-stu-id="3be89-161">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="3be89-162">Aşağıdaki `InvokeAsync` yöntemi iki parametre ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="3be89-162">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="3be89-163">Bir görünümü bileşen etiket Yardımcısı olarak çağırma</span><span class="sxs-lookup"><span data-stu-id="3be89-163">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="3be89-164">ASP.NET Core 1.1 ve üzeri, bir görünüm bileşeni olarak çağırabileceği bir [etiket Yardımcısı](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="3be89-164">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="3be89-165">Pascal ortası sınıfı ve yöntem parametreleri etiket Yardımcıları için çevrilen içine kendi [alt kebab durumda](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="3be89-165">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="3be89-166">Bir görünümü bileşen çağrılacak etiket Yardımcısı kullanan `<vc></vc>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="3be89-166">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="3be89-167">Görünüm bileşeni gibi belirtilir:</span><span class="sxs-lookup"><span data-stu-id="3be89-167">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="3be89-168">Not: bir görünüm bileşeni etiket Yardımcısı olarak kullanmak için Görünüm bileşenini kullanarak içeren derlemenin kaydetmeniz gerekir `@addTagHelper` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="3be89-168">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="3be89-169">Örneğin, görünüm bileşeniniz "Mywebapp şeklindedir" adlı bir derleme varsa, aşağıdaki yönergesi ekleyin. `_ViewImports.cshtml` dosyası:</span><span class="sxs-lookup"><span data-stu-id="3be89-169">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="3be89-170">Bir görünüm bileşeni görünümü bileşen başvuran herhangi bir dosyaya etiketi yardımcı olarak kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3be89-170">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="3be89-171">Bkz: [yönetme etiket Yardımcısı kapsam](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) etiket Yardımcıları kaydetme hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="3be89-171">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="3be89-172">`InvokeAsync` Bu öğreticide kullanılan yöntem:</span><span class="sxs-lookup"><span data-stu-id="3be89-172">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="3be89-173">Etiket Yardımcısı biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="3be89-173">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="3be89-174">Yukarıdaki örnekteki `PriorityList` görünümü bileşen olur `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="3be89-174">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="3be89-175">Görünümü bileşen parametreleri alt kebab durumda öznitelik olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="3be89-175">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="3be89-176">Bir görünümü bileşen denetleyicisinden doğrudan çağırma</span><span class="sxs-lookup"><span data-stu-id="3be89-176">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="3be89-177">Görünümü bileşenler genellikle bir görünümden çağrılır, ancak doğrudan denetleyici yönteminden çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3be89-177">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="3be89-178">Görünüm bileşenleri denetleyicileri gibi uç noktaları tanımlamak yoktur, ancak içeriği döndüren bir denetleyici eylemi kolayca uygulayabilirsiniz bir `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="3be89-178">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="3be89-179">Bu örnekte, görünümü bileşen doğrudan denetleyicisinden çağrılır:</span><span class="sxs-lookup"><span data-stu-id="3be89-179">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="3be89-180">İzlenecek yol: basit bir görünümle bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="3be89-180">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="3be89-181">[Karşıdan](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), yapı ve test starter kodunun.</span><span class="sxs-lookup"><span data-stu-id="3be89-181">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="3be89-182">Basit bir projeyle olan bir `Todo` listesini görüntüler denetleyicisi *Yapılacaklar* öğeleri.</span><span class="sxs-lookup"><span data-stu-id="3be89-182">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![ToDos listesi](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="3be89-184">ViewComponent sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="3be89-184">Add a ViewComponent class</span></span>

<span data-ttu-id="3be89-185">Oluşturma bir *ViewComponents* klasörü ve aşağıdaki ekleyin `PriorityListViewComponent` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3be89-185">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="3be89-186">Kodu Notlar:</span><span class="sxs-lookup"><span data-stu-id="3be89-186">Notes on the code:</span></span>

* <span data-ttu-id="3be89-187">Görünümü bileşen sınıfları yer almalıdır içinde **herhangi** proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="3be89-187">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="3be89-188">Sınıf adı olduğundan PriorityList**ViewComponent** sonekiyle sona erer **ViewComponent**, çalışma zamanı sınıf bileşen bir görünümden başvururken dizesi "PriorityList" kullanır.</span><span class="sxs-lookup"><span data-stu-id="3be89-188">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="3be89-189">I, daha ayrıntılı olarak daha sonra açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3be89-189">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="3be89-190">`[ViewComponent]` Öznitelik görünümü bileşen başvurmak için kullanılan adını değiştirebilir.</span><span class="sxs-lookup"><span data-stu-id="3be89-190">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="3be89-191">Örneğin, biz sınıfı adlı `XYZ` ve uygulanan `ViewComponent` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="3be89-191">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="3be89-192">`[ViewComponent]` Yukarıda öznitelik adı kullanmak için görünümü bileşen Seçicisi söyler `PriorityList` bileşeni ile ve sınıf bileşen bir görünümden başvururken dizesi "PriorityList" kullanmak için ilişkili görünümleri ararken.</span><span class="sxs-lookup"><span data-stu-id="3be89-192">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="3be89-193">I, daha ayrıntılı olarak daha sonra açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3be89-193">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="3be89-194">Bileşen [bağımlılık ekleme](../../fundamentals/dependency-injection.md) veri bağlamı kullanılabilmesi için.</span><span class="sxs-lookup"><span data-stu-id="3be89-194">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="3be89-195">`InvokeAsync` çıkarır rastgele sayıda bağımsız değişken bir görünüm ve onu denilen bir yöntem alabilir.</span><span class="sxs-lookup"><span data-stu-id="3be89-195">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="3be89-196">`InvokeAsync` Yöntemi döndürür kümesini `ToDo` karşılamak öğeleri `isDone` ve `maxPriority` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="3be89-196">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="3be89-197">Görünümü bileşen Razor görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="3be89-197">Create the view component Razor view</span></span>

* <span data-ttu-id="3be89-198">Oluşturma *görünümler/paylaşılan/bileşenleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="3be89-198">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="3be89-199">Bu klasör **gerekir** adlı *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="3be89-199">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="3be89-200">Oluşturma *görünümler/paylaşılan/bileşenleri/PriorityList* klasör.</span><span class="sxs-lookup"><span data-stu-id="3be89-200">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="3be89-201">Bu klasör adı görünümü bileşen sınıfı adını ya da soneki eksi sınıfın adı eşleşmelidir (kuralı izleyen ve kullandıysanız *ViewComponent* sınıf adı soneki).</span><span class="sxs-lookup"><span data-stu-id="3be89-201">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="3be89-202">Kullandıysanız `ViewComponent` özniteliği, sınıf adını öznitelik ataması eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="3be89-202">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="3be89-203">Oluşturma bir *Views/Shared/Components/PriorityList/Default.cshtml* Razor görünümü: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="3be89-203">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="3be89-204">Razor görünüm bir listesini alır `TodoItem` ve bunları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="3be89-204">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="3be89-205">Varsa görünümü bileşen `InvokeAsync` yöntemi (olduğu gibi bizim örnek), görünümün adını geçirmek değil *varsayılan* Görünüm adı için kural tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3be89-205">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="3be89-206">Öğreticide daha sonra t, görünümün adını geçirmek nasıl göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="3be89-206">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="3be89-207">Belirli bir denetleyicinin varsayılan stil geçersiz kılmak için denetleyici özel görünüm klasöre bir görünüm ekleyin (örneğin *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="3be89-207">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="3be89-208">Görünüm Bileşen Denetleyicisi özgü ise, denetleyici özgü klasörüne ekleyebilirsiniz (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="3be89-208">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="3be89-209">Ekleme bir `div` altına öncelik liste bileşeni için bir çağrı içeren *Views/Todo/index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="3be89-209">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="3be89-210">İşaretleme `@await Component.InvokeAsync` görünüm bileşenleri çağırma söz dizimi görülmektedir.</span><span class="sxs-lookup"><span data-stu-id="3be89-210">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="3be89-211">İlk bağımsız değişken çağrılamadı veya çağrı istiyoruz bileşen adıdır.</span><span class="sxs-lookup"><span data-stu-id="3be89-211">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="3be89-212">Sonraki parametreler bileşenine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="3be89-212">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="3be89-213">`InvokeAsync` rastgele sayıda bağımsız değişken alabilir.</span><span class="sxs-lookup"><span data-stu-id="3be89-213">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="3be89-214">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="3be89-214">Test the app.</span></span> <span data-ttu-id="3be89-215">Aşağıdaki resimde, yapılacaklar listesi ve öncelik öğeleri gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="3be89-215">The following image shows the ToDo list and the priority items:</span></span>

![Yapılacaklar listesi ve öncelik öğeleri](view-components/_static/pi.png)

<span data-ttu-id="3be89-217">Ayrıca, doğrudan denetleyicisinden görünümü bileşen çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="3be89-217">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![IndexVC eylemden öncelikli öğeler](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="3be89-219">Bir görünüm adı belirtme</span><span class="sxs-lookup"><span data-stu-id="3be89-219">Specifying a view name</span></span>

<span data-ttu-id="3be89-220">Karmaşık görünümü bileşen, varsayılan olmayan görünüm bazı koşullar altında belirtmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3be89-220">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="3be89-221">Aşağıdaki kod "PVC" görünümünden belirtme gösterir `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3be89-221">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="3be89-222">Güncelleştirme `InvokeAsync` yönteminde `PriorityListViewComponent` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3be89-222">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="3be89-223">Kopya *Views/Shared/Components/PriorityList/Default.cshtml* adlı bir görünümü dosyasına *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3be89-223">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="3be89-224">PVC görünümü kullanılan belirtmek için bir başlık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3be89-224">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="3be89-225">Güncelleştirme *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3be89-225">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="3be89-226">Uygulamayı çalıştırın ve PVC görünüm doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="3be89-226">Run the app and verify PVC view.</span></span>

![Öncelik görünümü bileşen](view-components/_static/pvc.png)

<span data-ttu-id="3be89-228">PVC görünüm işlenen değil, 4 veya daha yüksek önceliğe sahip görünümü bileşen aradığınız doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="3be89-228">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="3be89-229">Görünüm yolu inceleyin</span><span class="sxs-lookup"><span data-stu-id="3be89-229">Examine the view path</span></span>

* <span data-ttu-id="3be89-230">Priority parametresi, üç veya daha az öncelik görünüm döndürülmezse şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3be89-230">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="3be89-231">Geçici olarak yeniden adlandırın *Views/Todo/Components/PriorityList/Default.cshtml* için *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3be89-231">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="3be89-232">Uygulamayı test etme, aşağıdaki hatayı alırsınız:</span><span class="sxs-lookup"><span data-stu-id="3be89-232">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="3be89-233">Kopya *Views/Todo/Components/PriorityList/1Default.cshtml* için *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3be89-233">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="3be89-234">Bazı biçimlendirmeleri eklemek *paylaşılan* görünümü belirtmek için yapılacaklar bileşen görünümünü geldiği *paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="3be89-234">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="3be89-235">Test **paylaşılan** bileşeni görünümü.</span><span class="sxs-lookup"><span data-stu-id="3be89-235">Test the **Shared** component view.</span></span>

![Paylaşılan bileşeni görünümü Yapılacaklar çıkışı](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="3be89-237">Sihirli dizeleri önleme</span><span class="sxs-lookup"><span data-stu-id="3be89-237">Avoiding magic strings</span></span>

<span data-ttu-id="3be89-238">Zaman güvenliği derleme istiyorsanız, sabit kodlanmış görünümü bileşen adı sınıf adıyla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="3be89-238">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="3be89-239">"ViewComponent" soneki olmayan görünümü bileşen oluşturun:</span><span class="sxs-lookup"><span data-stu-id="3be89-239">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="3be89-240">Ekleme bir `using` , Razor ifadesine dosya görüntülemek ve kullanmak `nameof` işleci:</span><span class="sxs-lookup"><span data-stu-id="3be89-240">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a><span data-ttu-id="3be89-241">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3be89-241">Additional resources</span></span>

* [<span data-ttu-id="3be89-242">Görünümlere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="3be89-242">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
