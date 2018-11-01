---
title: ASP.NET core'da görünüm bileşenleri
author: rick-anderson
description: ASP.NET Core görünümü bileşenlerin nasıl kullanıldığı ve bunları için uygulamaları nasıl ekleyeceğinizi öğrenin.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/view-components
ms.openlocfilehash: 91399acafb36f1f8759ed1783e70e59b631e3bf0
ms.sourcegitcommit: 4a6bbe84db24c2f3dd2de065de418fde952c8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50253139"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="7c2d3-103">ASP.NET core'da görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="7c2d3-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="7c2d3-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c2d3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7c2d3-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7c2d3-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="7c2d3-106">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="7c2d3-106">View components</span></span>

<span data-ttu-id="7c2d3-107">Kısmi görünüm için Görünüm bileşenleri benzerdir, ancak bunlar çok daha güçlü.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="7c2d3-108">Görünüm bileşenleri kullanmayın model bağlama ve içine çağırırken sağlanan verileri yalnızca bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="7c2d3-109">ASP.NET Core MVC kullanarak bu makalenin yazıldığı ancak görünüm bileşenleri de Razor sayfaları kullanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-109">This article was written using ASP.NET Core MVC, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="7c2d3-110">Bir görünümü bileşeni:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-110">A view component:</span></span>

* <span data-ttu-id="7c2d3-111">Bir yanıtın tamamını yerine bir öbek işler.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="7c2d3-112">Test Edilebilirlik avantajları bir denetleyici ve görünüm arasında bulunan ve aynı ayrımı-ın-ile ilgili sorunlar içerir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="7c2d3-113">Parametreleri ve iş mantığına sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="7c2d3-114">Genellikle bir düzen sayfasından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="7c2d3-115">Görünüm bileşenleri herhangi bir kısmi görünüm için çok karmaşık olduğu gibi yeniden kullanılabilir işleme mantığı sahip tasarlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="7c2d3-116">Dinamik Gezinti menüleri</span><span class="sxs-lookup"><span data-stu-id="7c2d3-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="7c2d3-117">Etiket bulutunu (burada veritabanını sorgular)</span><span class="sxs-lookup"><span data-stu-id="7c2d3-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="7c2d3-118">Oturum açma paneli</span><span class="sxs-lookup"><span data-stu-id="7c2d3-118">Login panel</span></span>
* <span data-ttu-id="7c2d3-119">Alışveriş sepeti</span><span class="sxs-lookup"><span data-stu-id="7c2d3-119">Shopping cart</span></span>
* <span data-ttu-id="7c2d3-120">Yakın zamanda yayımlanan makaleler</span><span class="sxs-lookup"><span data-stu-id="7c2d3-120">Recently published articles</span></span>
* <span data-ttu-id="7c2d3-121">Tipik bir blog kenar çubuğu içeriği</span><span class="sxs-lookup"><span data-stu-id="7c2d3-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="7c2d3-122">Bir oturum açma paneli her sayfada işlenir ve oturumu kapatmayın veya kullanıcının durumunu günlüğünde bağlı olarak, oturum için ya da bağlantılarını göster</span><span class="sxs-lookup"><span data-stu-id="7c2d3-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="7c2d3-123">Bileşeni görüntüle iki bölümden oluşur: sınıfı (genellikle türetilen [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) ve isteğe bağlı olarak sonucu (genellikle bir görünüm) döndürür.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="7c2d3-124">Denetleyicileri gibi bir POCO görünümü bileşen olabilir, ancak çoğu Geliştirici türetme tarafından kullanılabilen özellikler ve yöntemler yararlanmak isteyeceksiniz `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="7c2d3-125">Bir görünümü bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="7c2d3-125">Creating a view component</span></span>

<span data-ttu-id="7c2d3-126">Bu bölümde, bir görünüm bileşeni oluşturmak için en üst düzey gereksinimleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="7c2d3-127">Makalenin sonraki bölümlerinde size her adım ayrıntılı inceleyin ve bir görünüm bileşeni oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="7c2d3-128">Görünümü bileşen sınıfı</span><span class="sxs-lookup"><span data-stu-id="7c2d3-128">The view component class</span></span>

<span data-ttu-id="7c2d3-129">Bir görünümü bileşen sınıfı aşağıdakilerden birini oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="7c2d3-130">Öğesinden türetme *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="7c2d3-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="7c2d3-131">Bir sınıf ile dekorasyon `[ViewComponent]` özniteliği veya bir sınıf türetmek `[ViewComponent]` özniteliği</span><span class="sxs-lookup"><span data-stu-id="7c2d3-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="7c2d3-132">Bir sınıf adı soneki ile sona ereceği oluşturma *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="7c2d3-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="7c2d3-133">Denetleyicileri gibi görünüm bileşenleri, genel, iç içe olmayan ve soyut olmayan sınıflar olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="7c2d3-134">Görünümü bileşen adı kaldırıldı "ViewComponent" sonekine sahip sınıf adıdır.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="7c2d3-135">Onu da açıkça kullanılarak belirtilebilir `ViewComponentAttribute.Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="7c2d3-136">Bir görünümü bileşen sınıfı:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-136">A view component class:</span></span>

* <span data-ttu-id="7c2d3-137">Oluşturucu tam olarak destekler [bağımlılık ekleme](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="7c2d3-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="7c2d3-138">Bölümü kullanamazsınız yani denetleyici yaşam döngüsünde almaz [filtreleri](../controllers/filters.md) görünümü bileşeninde</span><span class="sxs-lookup"><span data-stu-id="7c2d3-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="7c2d3-139">Görünümü bileşen yöntemleri</span><span class="sxs-lookup"><span data-stu-id="7c2d3-139">View component methods</span></span>

<span data-ttu-id="7c2d3-140">Görünümü bileşen kendi mantığı tanımlayan bir `InvokeAsync` döndüren yöntem bir `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="7c2d3-141">Parametreler doğrudan çağrı görünümü bileşeninin değil, model bağlama gelir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="7c2d3-142">Bir görünümü bileşeni hiçbir zaman doğrudan bir isteği işler.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-142">A view component never directly handles a request.</span></span> <span data-ttu-id="7c2d3-143">Genellikle, bir görünüm bileşeni bir model başlatır ve çağırarak görünümüne geçirir `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="7c2d3-144">Özet olarak, bileşen yöntemleri görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-144">In summary, view component methods:</span></span>

* <span data-ttu-id="7c2d3-145">Tanımlayan bir `InvokeAsync` döndüren yöntem bir `IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="7c2d3-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="7c2d3-146">Genellikle bir model başlatır ve çağırarak görünümüne geçirir `ViewComponent` `View` yöntemi</span><span class="sxs-lookup"><span data-stu-id="7c2d3-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="7c2d3-147">Parametreleri HTTP değil çağırma yönteminden gelir, model bağlama yok</span><span class="sxs-lookup"><span data-stu-id="7c2d3-147">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="7c2d3-148">Olan bir HTTP uç noktası olarak doğrudan ulaşılabilir değil, bunlar kodunuzu (genellikle, bir görünüm) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-148">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="7c2d3-149">Bir görünümü bileşeni hiçbir zaman bir isteği işler</span><span class="sxs-lookup"><span data-stu-id="7c2d3-149">A view component never handles a request</span></span>
* <span data-ttu-id="7c2d3-150">Geçerli HTTP istek tüm ayrıntıları yerine imza aşırı</span><span class="sxs-lookup"><span data-stu-id="7c2d3-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="7c2d3-151">Görünüm arama yolu</span><span class="sxs-lookup"><span data-stu-id="7c2d3-151">View search path</span></span>

<span data-ttu-id="7c2d3-152">Çalışma zamanı aşağıdaki yollardan görünümünde arar:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-152">The runtime searches for the view in the following paths:</span></span>

* <span data-ttu-id="7c2d3-153">/Pages/bileşenleri / {View bileşen adı} / {View adı}</span><span class="sxs-lookup"><span data-stu-id="7c2d3-153">/Pages/Components/{View Component Name}/{View Name}</span></span>
* <span data-ttu-id="7c2d3-154">{Denetleyici adı} /Views/ /Components/ {görünümü bileşen adı} / {View adı}</span><span class="sxs-lookup"><span data-stu-id="7c2d3-154">/Views/{Controller Name}/Components/{View Component Name}/{View Name}</span></span>
* <span data-ttu-id="7c2d3-155">/ Görünümler/paylaşılan/bileşenleri / {View bileşen adı} / {View adı}</span><span class="sxs-lookup"><span data-stu-id="7c2d3-155">/Views/Shared/Components/{View Component Name}/{View Name}</span></span>

<span data-ttu-id="7c2d3-156">Bir görünümü bileşen için varsayılan görünüm adı *varsayılan*, yani dosyasını görüntüle genellikle adlandırılacağını *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-156">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="7c2d3-157">Görünüm bileşen sonucu oluştururken veya çağırırken farklı görünüm adı belirtebilirsiniz `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-157">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="7c2d3-158">Görünüm dosyası adı öneririz *Default.cshtml* ve *görünümler/paylaşılan/Components / {View bileşen adı} / {View Name}* yolu.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-158">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/{View Component Name}/{View Name}* path.</span></span> <span data-ttu-id="7c2d3-159">`PriorityList` Bu örnekte kullanılan görünümü bileşen *Views/Shared/Components/PriorityList/Default.cshtml* görünümünü bileşeni için.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-159">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="7c2d3-160">Bileşeni görüntüle çağırma</span><span class="sxs-lookup"><span data-stu-id="7c2d3-160">Invoking a view component</span></span>

<span data-ttu-id="7c2d3-161">Bileşeni görüntüle kullanmak için aşağıdaki çağrı görünümü içinde:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-161">To use the view component, call the following inside a view:</span></span>

```cshtml
@await Component.InvokeAsync("Name of view component", {Anonymous Type Containing Parameters})
```

<span data-ttu-id="7c2d3-162">Parametreleri geçirilecek `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-162">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="7c2d3-163">`PriorityList` Makalesinde geliştirilen görünümü bileşen ınvoked from *Views/Todo/Index.cshtml* görünüm dosyası.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-163">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="7c2d3-164">Aşağıdaki `InvokeAsync` yöntemi, iki parametre ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-164">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

::: moniker range=">= aspnetcore-1.1"

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="7c2d3-165">Bileşeni görüntüle etiket Yardımcısı çağırma</span><span class="sxs-lookup"><span data-stu-id="7c2d3-165">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="7c2d3-166">ASP.NET Core 1.1 ve sonraki bir görünüm bileşeni olarak çağırabilirsiniz bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="7c2d3-166">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="7c2d3-167">Pascal büyük küçük harfleri sınıf ve yöntem parametreleri etiket Yardımcıları için çevrilmiş içine kendi [alt kebab çalışması](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="7c2d3-167">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="7c2d3-168">Bileşeni görüntüle çağırmak için etiket Yardımcısı kullanan `<vc></vc>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-168">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="7c2d3-169">Bileşeni görüntüle gibi belirtilir:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-169">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="7c2d3-170">Bir görünümü bileşeni etiket Yardımcısı kullanılacak görünümünü kullanarak bileşeni içeren derlemenin kaydetme `@addTagHelper` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-170">To use a view component as a Tag Helper, register the assembly containing the view component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="7c2d3-171">Bileşeni görüntüle adlı bir derlemede ise `MyWebApp`, eklemek için aşağıdaki yönerge *_viewımports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-171">If your view component is in an assembly called `MyWebApp`, add the following directive to the *_ViewImports.cshtml* file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="7c2d3-172">Bileşeni görüntüle görünümü bileşenine başvurduğunu herhangi bir dosyaya etiket Yardımcısı olarak kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-172">You can register a view component as a Tag Helper to any file that references the view component.</span></span> <span data-ttu-id="7c2d3-173">Bkz: [yönetme etiket Yardımcısı kapsam](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) etiket Yardımcıları kaydetme hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-173">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="7c2d3-174">`InvokeAsync` Bu öğreticide kullanılan yöntemi:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-174">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="7c2d3-175">Etiket Yardımcısı biçimlendirme içinde:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-175">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="7c2d3-176">Yukarıdaki örnekteki `PriorityList` görünümü bileşen olur `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-176">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="7c2d3-177">Bileşeni görüntüle parametreleri alt kebab durumda öznitelik olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-177">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

::: moniker-end

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="7c2d3-178">Bir görünümü bileşen denetleyicisinden doğrudan çağırma</span><span class="sxs-lookup"><span data-stu-id="7c2d3-178">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="7c2d3-179">Görünüm bileşenleri, genellikle bir görünümden çağrılır, ancak doğrudan bir denetleyici yöntemi çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-179">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="7c2d3-180">Görünüm bileşenleri denetleyicileri gibi uç noktaları tanımlamak yoktur, ancak içeriği döndüren bir denetleyici eylemi kolayca uygulayabilirsiniz bir `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-180">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="7c2d3-181">Bu örnekte, doğrudan denetleyiciden görünüm bileşen çağrılır:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-181">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="7c2d3-182">İzlenecek yol: Basit Görünüm bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="7c2d3-182">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="7c2d3-183">[İndirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), derleme ve Başlatıcı kodu test edin.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-183">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="7c2d3-184">Basit bir proje olan bir `Todo` listesini görüntüler denetleyicisi *Todo* öğeleri.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-184">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Açıklamada listesi](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="7c2d3-186">ViewComponent sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="7c2d3-186">Add a ViewComponent class</span></span>

<span data-ttu-id="7c2d3-187">Oluşturma bir *ViewComponents* klasörü ve aşağıdaki `PriorityListViewComponent` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-187">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="7c2d3-188">Kod ile ilgili notlar:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-188">Notes on the code:</span></span>

* <span data-ttu-id="7c2d3-189">Görünüm bileşen sınıfları kapsanıyorsa **herhangi** proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-189">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="7c2d3-190">Sınıf adı olduğundan PriorityList**ViewComponent** soneki ile sona erer **ViewComponent**, çalışma zamanı sınıf bileşen görünümden başvururken "PriorityList" dizesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-190">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="7c2d3-191">I, daha ayrıntılı olarak daha sonra denetleyeceğinizi açıklayacağız.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-191">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="7c2d3-192">`[ViewComponent]` Öznitelik, bir görünüm bileşeni başvurmak için kullanılan adını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-192">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="7c2d3-193">Örneğin, biz adlı sınıfı `XYZ` ve uygulanan `ViewComponent` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-193">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="7c2d3-194">`[ViewComponent]` Özniteliği yukarıdaki söyler adı kullanacak şekilde görünümü Bileşen Seçici `PriorityList` bileşeni ile ve "PriorityList" dize sınıfı bileşen görünümden başvururken kullanılacak ilişkili görünümleri ararken.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-194">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="7c2d3-195">I, daha ayrıntılı olarak daha sonra denetleyeceğinizi açıklayacağız.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-195">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="7c2d3-196">Bileşen [bağımlılık ekleme](../../fundamentals/dependency-injection.md) veri bağlamı kullanılabilir hale getirmek için.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-196">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="7c2d3-197">`InvokeAsync` kullanıma sunan bir görünümü ve onu çağrılabilen bir yöntem rastgele bir sayıda bağımsız değişken alabilir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-197">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="7c2d3-198">`InvokeAsync` Yöntemi kümesini döndürür `ToDo` karşılayan öğelerinin `isDone` ve `maxPriority` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-198">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="7c2d3-199">Bileşen Razor görünümünü oluşturma</span><span class="sxs-lookup"><span data-stu-id="7c2d3-199">Create the view component Razor view</span></span>

* <span data-ttu-id="7c2d3-200">Oluşturma *görünümler/paylaşılan/Components* klasör.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-200">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="7c2d3-201">Bu klasör **gerekir** adı *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-201">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="7c2d3-202">Oluşturma *görünümler/paylaşılan/bileşenleri/PriorityList* klasör.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-202">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="7c2d3-203">Bu klasör adı görünümü bileşen sınıfının adı ve soneki eksi sınıfının adı eşleşmelidir (kuralını izleyen ve kullandıysanız *ViewComponent* sınıf adı soneki).</span><span class="sxs-lookup"><span data-stu-id="7c2d3-203">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="7c2d3-204">Kullandıysanız `ViewComponent` özniteliği, sınıf adı öznitelik atamasını eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-204">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="7c2d3-205">Oluşturma bir *Views/Shared/Components/PriorityList/Default.cshtml* Razor Görünüm: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="7c2d3-205">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="7c2d3-206">Razor görünümü listesini alır `TodoItem` ve bunları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-206">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="7c2d3-207">Varsa görünümü bileşen `InvokeAsync` yöntemi (olduğu gibi örneğimizi), görünüm adını geçirin değil *varsayılan* görünümü adı için kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-207">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="7c2d3-208">Öğreticinin sonraki bölümlerinde miyim, görünümün adının ona nasıl iletileceğini göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-208">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="7c2d3-209">Belirli bir denetleyicinin varsayılan stillerini geçersiz kılmak için bir görünüm denetleyicisine özgü görünüm klasöre ekleyin (örneğin *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-209">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="7c2d3-210">Denetleyicisine özgü görünüm bileşeni ise denetleyicisi özgü klasöre ekleyebilirsiniz (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="7c2d3-210">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="7c2d3-211">Ekleme bir `div` altına öncelik listesi bileşeni için bir çağrı içeren *Views/Todo/index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-211">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="7c2d3-212">Biçimlendirme `@await Component.InvokeAsync` görünüm bileşenleri çağırma söz dizimi görülmektedir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-212">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="7c2d3-213">İlk bağımsız değişken çağırma veya çağrı istiyoruz bileşen adıdır.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-213">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="7c2d3-214">Sonraki parametreler bileşenine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-214">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="7c2d3-215">`InvokeAsync` rastgele bir sayıda bağımsız değişken alabilir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-215">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="7c2d3-216">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-216">Test the app.</span></span> <span data-ttu-id="7c2d3-217">Aşağıdaki görüntüde, yapılacaklar listesi ve öncelikli öğeleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-217">The following image shows the ToDo list and the priority items:</span></span>

![Yapılacaklar listesi ve öncelikli öğeleri](view-components/_static/pi.png)

<span data-ttu-id="7c2d3-219">Doğrudan denetleyiciden görünüm bileşeni de çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-219">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![öncelikli öğeleri IndexVC eylemi](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="7c2d3-221">Görünüm adı belirtme</span><span class="sxs-lookup"><span data-stu-id="7c2d3-221">Specifying a view name</span></span>

<span data-ttu-id="7c2d3-222">Karmaşık görünümü bileşen, bazı koşullar altında bir varsayılan olmayan görünüm belirtmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-222">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="7c2d3-223">Aşağıdaki kod "PVC" görünümünden belirteceğiniz gösterilmektedir `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-223">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="7c2d3-224">Güncelleştirme `InvokeAsync` yönteminde `PriorityListViewComponent` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-224">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="7c2d3-225">Kopyalama *Views/Shared/Components/PriorityList/Default.cshtml* adlı bir görünüm dosyasına *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-225">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="7c2d3-226">PVC görünümü kullanıldığını belirtmek için bir başlık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-226">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="7c2d3-227">Güncelleştirme *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-227">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="7c2d3-228">Uygulamayı çalıştırın ve PVC görünümü doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-228">Run the app and verify PVC view.</span></span>

![Öncelik bileşeni görüntüle](view-components/_static/pvc.png)

<span data-ttu-id="7c2d3-230">PVC görünüm işlenen değil, 4 veya daha yüksek bir önceliğe sahip görünümü bileşen aradığınız doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-230">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="7c2d3-231">Görünüm yolunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="7c2d3-231">Examine the view path</span></span>

* <span data-ttu-id="7c2d3-232">Öncelik parametresi, üç veya daha düşük öncelikli görünüm döndürülmez şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-232">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="7c2d3-233">Geçici olarak yeniden adlandırın *Views/Todo/Components/PriorityList/Default.cshtml* için *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-233">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="7c2d3-234">Uygulamayı test etme, aşağıdaki hatayı alırsınız:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-234">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="7c2d3-235">Kopyalama *Views/Todo/Components/PriorityList/1Default.cshtml* için *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-235">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="7c2d3-236">Bazı biçimlendirme eklemek *paylaşılan* Todo görünümü bileşen görünümü, görünümün belirtmenizi geldiği *paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-236">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="7c2d3-237">Test **paylaşılan** bileşeni görünümü.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-237">Test the **Shared** component view.</span></span>

![ToDo çıkış paylaşılan bileşen görünümü](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="7c2d3-239">Sihirli dize kaçınma</span><span class="sxs-lookup"><span data-stu-id="7c2d3-239">Avoiding magic strings</span></span>

<span data-ttu-id="7c2d3-240">Zaman güvenlik derlemek isterseniz, sabit kodlanmış görünümü bileşen adı sınıf adını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-240">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="7c2d3-241">Bileşeni görüntüle "ViewComponent" soneki olmadan oluşturun:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-241">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="7c2d3-242">Ekleme bir `using` , Razor ifadesine dosyayı görüntüle ve Kullan `nameof` işleci:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-242">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="perform-synchronous-work"></a><span data-ttu-id="7c2d3-243">Zaman uyumlu çalışma gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="7c2d3-243">Perform synchronous work</span></span>

<span data-ttu-id="7c2d3-244">Çerçeve işleme bir zaman uyumlu çağırma `Invoke` zaman uyumsuz çalışmayı gerçekleştirmek gerekmiyorsa yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-244">The framework handles invoking a synchronous `Invoke` method if you don't need to perform asynchronous work.</span></span> <span data-ttu-id="7c2d3-245">Aşağıdaki yöntem zaman uyumlu bir oluşturur `Invoke` bileşeni görüntüle:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-245">The following method creates a synchronous `Invoke` view component:</span></span>

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

<span data-ttu-id="7c2d3-246">Geçirilen dizeler görünümü bileşenin Razor dosyasını listeler `Invoke` yöntemi (*Views/Home/Components/PriorityList/Default.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="7c2d3-246">The view component's Razor file lists the strings passed to the `Invoke` method (*Views/Home/Components/PriorityList/Default.cshtml*):</span></span>

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

<span data-ttu-id="7c2d3-247">Bileşeni görüntüle Razor dosyasında çağrılır (örneğin, *Views/Home/Index.cshtml*) aşağıdaki yaklaşımlardan birini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-247">The view component is invoked in a Razor file (for example, *Views/Home/Index.cshtml*) using one of the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>
* [<span data-ttu-id="7c2d3-248">Etiketi Yardımcısı</span><span class="sxs-lookup"><span data-stu-id="7c2d3-248">Tag Helper</span></span>](xref:mvc/views/tag-helpers/intro)

<span data-ttu-id="7c2d3-249">Kullanılacak <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> yaklaşımı, çağrı `Component.InvokeAsync`:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-249">To use the <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper> approach, call `Component.InvokeAsync`:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-1.1"

<span data-ttu-id="7c2d3-250">Bileşeni görüntüle Razor dosyasında çağrılır (örneğin, *Views/Home/Index.cshtml*) ile <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-250">The view component is invoked in a Razor file (for example, *Views/Home/Index.cshtml*) with <xref:Microsoft.AspNetCore.Mvc.IViewComponentHelper>.</span></span>

<span data-ttu-id="7c2d3-251">Çağrı `Component.InvokeAsync`:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-251">Call `Component.InvokeAsync`:</span></span>

::: moniker-end

```cshtml
@await Component.InvokeAsync(nameof(PriorityList), new { maxPriority = 4, isDone = true })
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="7c2d3-252">Etiket Yardımcısı'nı kullanmak için görünümü bileşen kullanarak içeren derlemenin kaydetme `@addTagHelper` yönergesi (adlı bir derlemede görünümü bileşendir `MyWebApp`):</span><span class="sxs-lookup"><span data-stu-id="7c2d3-252">To use the Tag Helper, register the assembly containing the View Component using the `@addTagHelper` directive (the view component is in an assembly called `MyWebApp`):</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="7c2d3-253">Etiket Yardımcısı görünümü bileşen Razor biçimlendirme dosyasında kullanın:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-253">Use the view component Tag Helper in the Razor markup file:</span></span>

```cshtml
<vc:priority-list max-priority="999" is-done="false">
</vc:priority-list>
```
::: moniker-end

<span data-ttu-id="7c2d3-254">Yöntem imzası `PriorityList.Invoke` zaman uyumludur, ancak Razor bulur ve içeren yöntemi çağıran `Component.InvokeAsync` biçimlendirme dosyası.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-254">The method signature of `PriorityList.Invoke` is synchronous, but Razor finds and calls the method with `Component.InvokeAsync` in the markup file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c2d3-255">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7c2d3-255">Additional resources</span></span>

* [<span data-ttu-id="7c2d3-256">Görünümlere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="7c2d3-256">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
