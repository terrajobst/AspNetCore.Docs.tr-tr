---
title: ASP.NET core'da görünüm bileşenleri
author: rick-anderson
description: ASP.NET Core görünümü bileşenlerin nasıl kullanıldığı ve bunları için uygulamaları nasıl ekleyeceğinizi öğrenin.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/view-components
ms.openlocfilehash: c4e4de6e4ffb634a636bccdb2a929a524baebecf
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756286"
---
# <a name="view-components-in-aspnet-core"></a><span data-ttu-id="5f8ce-103">ASP.NET core'da görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="5f8ce-103">View components in ASP.NET Core</span></span>

<span data-ttu-id="5f8ce-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5f8ce-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5f8ce-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5f8ce-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="view-components"></a><span data-ttu-id="5f8ce-106">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="5f8ce-106">View components</span></span>

<span data-ttu-id="5f8ce-107">Kısmi görünüm için Görünüm bileşenleri benzerdir, ancak bunlar çok daha güçlü.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-107">View components are similar to partial views, but they're much more powerful.</span></span> <span data-ttu-id="5f8ce-108">Görünüm bileşenleri kullanmayın model bağlama ve içine çağırırken sağlanan verileri yalnızca bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-108">View components don't use model binding, and only depend on the data provided when calling into it.</span></span> <span data-ttu-id="5f8ce-109">ASP.NET Core MVC kullanarak bu makalenin yazıldığı ancak görünüm bileşenleri de Razor sayfaları kullanmaya çalışır.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-109">This article was written using ASP.NET Core MVC, but view components also work with Razor Pages.</span></span>

<span data-ttu-id="5f8ce-110">Bir görünümü bileşeni:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-110">A view component:</span></span>

* <span data-ttu-id="5f8ce-111">Bir yanıtın tamamını yerine bir öbek işler.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-111">Renders a chunk rather than a whole response.</span></span>
* <span data-ttu-id="5f8ce-112">Test Edilebilirlik avantajları bir denetleyici ve görünüm arasında bulunan ve aynı ayrımı-ın-ile ilgili sorunlar içerir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-112">Includes the same separation-of-concerns and testability benefits found between a controller and view.</span></span>
* <span data-ttu-id="5f8ce-113">Parametreleri ve iş mantığına sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-113">Can have parameters and business logic.</span></span>
* <span data-ttu-id="5f8ce-114">Genellikle bir düzen sayfasından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-114">Is typically invoked from a layout page.</span></span>

<span data-ttu-id="5f8ce-115">Görünüm bileşenleri herhangi bir kısmi görünüm için çok karmaşık olduğu gibi yeniden kullanılabilir işleme mantığı sahip tasarlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-115">View components are intended anywhere you have reusable rendering logic that's too complex for a partial view, such as:</span></span>

* <span data-ttu-id="5f8ce-116">Dinamik Gezinti menüleri</span><span class="sxs-lookup"><span data-stu-id="5f8ce-116">Dynamic navigation menus</span></span>
* <span data-ttu-id="5f8ce-117">Etiket bulutunu (burada veritabanını sorgular)</span><span class="sxs-lookup"><span data-stu-id="5f8ce-117">Tag cloud (where it queries the database)</span></span>
* <span data-ttu-id="5f8ce-118">Oturum açma paneli</span><span class="sxs-lookup"><span data-stu-id="5f8ce-118">Login panel</span></span>
* <span data-ttu-id="5f8ce-119">Alışveriş sepeti</span><span class="sxs-lookup"><span data-stu-id="5f8ce-119">Shopping cart</span></span>
* <span data-ttu-id="5f8ce-120">Yakın zamanda yayımlanan makaleler</span><span class="sxs-lookup"><span data-stu-id="5f8ce-120">Recently published articles</span></span>
* <span data-ttu-id="5f8ce-121">Tipik bir blog kenar çubuğu içeriği</span><span class="sxs-lookup"><span data-stu-id="5f8ce-121">Sidebar content on a typical blog</span></span>
* <span data-ttu-id="5f8ce-122">Bir oturum açma paneli her sayfada işlenir ve oturumu kapatmayın veya kullanıcının durumunu günlüğünde bağlı olarak, oturum için ya da bağlantılarını göster</span><span class="sxs-lookup"><span data-stu-id="5f8ce-122">A login panel that would be rendered on every page and show either the links to log out or log in, depending on the log in state of the user</span></span>

<span data-ttu-id="5f8ce-123">Bileşeni görüntüle iki bölümden oluşur: sınıfı (genellikle türetilen [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) ve isteğe bağlı olarak sonucu (genellikle bir görünüm) döndürür.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-123">A view component consists of two parts: the class (typically derived from [ViewComponent](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponent)) and the result it returns (typically a view).</span></span> <span data-ttu-id="5f8ce-124">Denetleyicileri gibi bir POCO görünümü bileşen olabilir, ancak çoğu Geliştirici türetme tarafından kullanılabilen özellikler ve yöntemler yararlanmak isteyeceksiniz `ViewComponent`.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-124">Like controllers, a view component can be a POCO, but most developers will want to take advantage of the methods and properties available by deriving from `ViewComponent`.</span></span>

## <a name="creating-a-view-component"></a><span data-ttu-id="5f8ce-125">Bir görünümü bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="5f8ce-125">Creating a view component</span></span>

<span data-ttu-id="5f8ce-126">Bu bölümde, bir görünüm bileşeni oluşturmak için en üst düzey gereksinimleri bulunur.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-126">This section contains the high-level requirements to create a view component.</span></span> <span data-ttu-id="5f8ce-127">Makalenin sonraki bölümlerinde size her adım ayrıntılı inceleyin ve bir görünüm bileşeni oluşturmak.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-127">Later in the article, we'll examine each step in detail and create a view component.</span></span>

### <a name="the-view-component-class"></a><span data-ttu-id="5f8ce-128">Görünümü bileşen sınıfı</span><span class="sxs-lookup"><span data-stu-id="5f8ce-128">The view component class</span></span>

<span data-ttu-id="5f8ce-129">Bir görünümü bileşen sınıfı aşağıdakilerden birini oluşturulabilir:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-129">A view component class can be created by any of the following:</span></span>

* <span data-ttu-id="5f8ce-130">Öğesinden türetme *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="5f8ce-130">Deriving from *ViewComponent*</span></span>
* <span data-ttu-id="5f8ce-131">Bir sınıf ile dekorasyon `[ViewComponent]` özniteliği veya bir sınıf türetmek `[ViewComponent]` özniteliği</span><span class="sxs-lookup"><span data-stu-id="5f8ce-131">Decorating a class with the `[ViewComponent]` attribute, or deriving from a class with the `[ViewComponent]` attribute</span></span>
* <span data-ttu-id="5f8ce-132">Bir sınıf adı soneki ile sona ereceği oluşturma *ViewComponent*</span><span class="sxs-lookup"><span data-stu-id="5f8ce-132">Creating a class where the name ends with the suffix *ViewComponent*</span></span>

<span data-ttu-id="5f8ce-133">Denetleyicileri gibi görünüm bileşenleri, genel, iç içe olmayan ve soyut olmayan sınıflar olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-133">Like controllers, view components must be public, non-nested, and non-abstract classes.</span></span> <span data-ttu-id="5f8ce-134">Görünümü bileşen adı kaldırıldı "ViewComponent" sonekine sahip sınıf adıdır.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-134">The view component name is the class name with the "ViewComponent" suffix removed.</span></span> <span data-ttu-id="5f8ce-135">Onu da açıkça kullanılarak belirtilebilir `ViewComponentAttribute.Name` özelliği.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-135">It can also be explicitly specified using the `ViewComponentAttribute.Name` property.</span></span>

<span data-ttu-id="5f8ce-136">Bir görünümü bileşen sınıfı:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-136">A view component class:</span></span>

* <span data-ttu-id="5f8ce-137">Oluşturucu tam olarak destekler [bağımlılık ekleme](../../fundamentals/dependency-injection.md)</span><span class="sxs-lookup"><span data-stu-id="5f8ce-137">Fully supports constructor [dependency injection](../../fundamentals/dependency-injection.md)</span></span>

* <span data-ttu-id="5f8ce-138">Bölümü kullanamazsınız yani denetleyici yaşam döngüsünde almaz [filtreleri](../controllers/filters.md) görünümü bileşeninde</span><span class="sxs-lookup"><span data-stu-id="5f8ce-138">Doesn't take part in the controller lifecycle, which means you can't use [filters](../controllers/filters.md) in a view component</span></span>

### <a name="view-component-methods"></a><span data-ttu-id="5f8ce-139">Görünümü bileşen yöntemleri</span><span class="sxs-lookup"><span data-stu-id="5f8ce-139">View component methods</span></span>

<span data-ttu-id="5f8ce-140">Görünümü bileşen kendi mantığı tanımlayan bir `InvokeAsync` döndüren yöntem bir `IViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-140">A view component defines its logic in an `InvokeAsync` method that returns an `IViewComponentResult`.</span></span> <span data-ttu-id="5f8ce-141">Parametreler doğrudan çağrı görünümü bileşeninin değil, model bağlama gelir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-141">Parameters come directly from invocation of the view component, not from model binding.</span></span> <span data-ttu-id="5f8ce-142">Bir görünümü bileşeni hiçbir zaman doğrudan bir isteği işler.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-142">A view component never directly handles a request.</span></span> <span data-ttu-id="5f8ce-143">Genellikle, bir görünüm bileşeni bir model başlatır ve çağırarak görünümüne geçirir `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-143">Typically, a view component initializes a model and passes it to a view by calling the `View` method.</span></span> <span data-ttu-id="5f8ce-144">Özet olarak, bileşen yöntemleri görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-144">In summary, view component methods:</span></span>

* <span data-ttu-id="5f8ce-145">Tanımlayan bir `InvokeAsync` döndüren yöntem bir `IViewComponentResult`</span><span class="sxs-lookup"><span data-stu-id="5f8ce-145">Define an `InvokeAsync` method that returns an `IViewComponentResult`</span></span>
* <span data-ttu-id="5f8ce-146">Genellikle bir model başlatır ve çağırarak görünümüne geçirir `ViewComponent` `View` yöntemi</span><span class="sxs-lookup"><span data-stu-id="5f8ce-146">Typically initializes a model and passes it to a view by calling the `ViewComponent` `View` method</span></span>
* <span data-ttu-id="5f8ce-147">Parametreleri HTTP değil çağırma yönteminden gelir, model bağlama yok</span><span class="sxs-lookup"><span data-stu-id="5f8ce-147">Parameters come from the calling method, not HTTP, there's no model binding</span></span>
* <span data-ttu-id="5f8ce-148">Olan bir HTTP uç noktası olarak doğrudan ulaşılabilir değil, bunlar kodunuzu (genellikle, bir görünüm) çağrılır.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-148">Are not reachable directly as an HTTP endpoint, they're invoked from your code (usually in a view).</span></span> <span data-ttu-id="5f8ce-149">Bir görünümü bileşeni hiçbir zaman bir isteği işler</span><span class="sxs-lookup"><span data-stu-id="5f8ce-149">A view component never handles a request</span></span>
* <span data-ttu-id="5f8ce-150">Geçerli HTTP istek tüm ayrıntıları yerine imza aşırı</span><span class="sxs-lookup"><span data-stu-id="5f8ce-150">Are overloaded on the signature rather than any details from the current HTTP request</span></span>

### <a name="view-search-path"></a><span data-ttu-id="5f8ce-151">Görünüm arama yolu</span><span class="sxs-lookup"><span data-stu-id="5f8ce-151">View search path</span></span>

<span data-ttu-id="5f8ce-152">Çalışma zamanı aşağıdaki yollardan görünümünde arar:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-152">The runtime searches for the view in the following paths:</span></span>

* <span data-ttu-id="5f8ce-153">/Pages/bileşenleri/<component name>/\<view_name ></span><span class="sxs-lookup"><span data-stu-id="5f8ce-153">/Pages/Components/<component name>/\<view_name></span></span>
* <span data-ttu-id="5f8ce-154">Görünümler /\<controller_name > /Components/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="5f8ce-154">Views/\<controller_name>/Components/\<view_component_name>/\<view_name></span></span>
* <span data-ttu-id="5f8ce-155">Görünümler/paylaşılan/Components/\<view_component_name > /\<view_name ></span><span class="sxs-lookup"><span data-stu-id="5f8ce-155">Views/Shared/Components/\<view_component_name>/\<view_name></span></span>

<span data-ttu-id="5f8ce-156">Bir görünümü bileşen için varsayılan görünüm adı *varsayılan*, yani dosyasını görüntüle genellikle adlandırılacağını *Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-156">The default view name for a view component is *Default*, which means your view file will typically be named *Default.cshtml*.</span></span> <span data-ttu-id="5f8ce-157">Görünüm bileşen sonucu oluştururken veya çağırırken farklı görünüm adı belirtebilirsiniz `View` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-157">You can specify a different view name when creating the view component result or when calling the `View` method.</span></span>

<span data-ttu-id="5f8ce-158">Görünüm dosyası adı öneririz *Default.cshtml* ve *görünümler/paylaşılan/Components/\<view_component_name > /\<view_name >* yolu.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-158">We recommend you name the view file *Default.cshtml* and use the *Views/Shared/Components/\<view_component_name>/\<view_name>* path.</span></span> <span data-ttu-id="5f8ce-159">`PriorityList` Bu örnekte kullanılan görünümü bileşen *Views/Shared/Components/PriorityList/Default.cshtml* görünümünü bileşeni için.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-159">The `PriorityList` view component used in this sample uses *Views/Shared/Components/PriorityList/Default.cshtml* for the view component view.</span></span>

## <a name="invoking-a-view-component"></a><span data-ttu-id="5f8ce-160">Bileşeni görüntüle çağırma</span><span class="sxs-lookup"><span data-stu-id="5f8ce-160">Invoking a view component</span></span>

<span data-ttu-id="5f8ce-161">Bileşeni görüntüle kullanmak için aşağıdaki çağrı görünümü içinde:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-161">To use the view component, call the following inside a view:</span></span>

```cshtml
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

<span data-ttu-id="5f8ce-162">Parametreleri geçirilecek `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-162">The parameters will be passed to the `InvokeAsync` method.</span></span> <span data-ttu-id="5f8ce-163">`PriorityList` Makalesinde geliştirilen görünümü bileşen ınvoked from *Views/Todo/Index.cshtml* görünüm dosyası.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-163">The `PriorityList` view component developed in the article is invoked from the *Views/Todo/Index.cshtml* view file.</span></span> <span data-ttu-id="5f8ce-164">Aşağıdaki `InvokeAsync` yöntemi, iki parametre ile çağrılır:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-164">In the following, the `InvokeAsync` method is called with two parameters:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a><span data-ttu-id="5f8ce-165">Bileşeni görüntüle etiket Yardımcısı çağırma</span><span class="sxs-lookup"><span data-stu-id="5f8ce-165">Invoking a view component as a Tag Helper</span></span>

<span data-ttu-id="5f8ce-166">ASP.NET Core 1.1 ve sonraki bir görünüm bileşeni olarak çağırabilirsiniz bir [etiketi Yardımcısı](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="5f8ce-166">For ASP.NET Core 1.1 and higher, you can invoke a view component as a [Tag Helper](xref:mvc/views/tag-helpers/intro):</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="5f8ce-167">Pascal büyük küçük harfleri sınıf ve yöntem parametreleri etiket Yardımcıları için çevrilmiş içine kendi [alt kebab çalışması](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="5f8ce-167">Pascal-cased class and method parameters for Tag Helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="5f8ce-168">Bileşeni görüntüle çağırmak için etiket Yardımcısı kullanan `<vc></vc>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-168">The Tag Helper to invoke a view component uses the `<vc></vc>` element.</span></span> <span data-ttu-id="5f8ce-169">Bileşeni görüntüle gibi belirtilir:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-169">The view component is specified as follows:</span></span>

```cshtml
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

<span data-ttu-id="5f8ce-170">Not: bir görünüm bileşeni etiket Yardımcısı kullanmak için görünümü bileşen kullanarak içeren derlemenin kaydetmeniz gerekir `@addTagHelper` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-170">Note: In order to use a View Component as a Tag Helper, you must register the assembly containing the View Component using the `@addTagHelper` directive.</span></span> <span data-ttu-id="5f8ce-171">Örneğin, "Mywebapp şeklindedir" adlı bir derlemede görünümü Bileşeniniz varsa, aşağıdaki yönerge için ekleyin. `_ViewImports.cshtml` dosyası:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-171">For example, if your View Component is in an assembly called "MyWebApp", add the following directive to the `_ViewImports.cshtml` file:</span></span>

```cshtml
@addTagHelper *, MyWebApp
```

<span data-ttu-id="5f8ce-172">Bileşeni görüntüle görünümü bileşenine başvurduğunu herhangi bir dosyaya etiket Yardımcısı olarak kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-172">You can register a View Component as a Tag Helper to any file that references the View Component.</span></span> <span data-ttu-id="5f8ce-173">Bkz: [yönetme etiket Yardımcısı kapsam](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) etiket Yardımcıları kaydetme hakkında daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-173">See [Managing Tag Helper Scope](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) for more information on how to register Tag Helpers.</span></span>

<span data-ttu-id="5f8ce-174">`InvokeAsync` Bu öğreticide kullanılan yöntemi:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-174">The `InvokeAsync` method used in this tutorial:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="5f8ce-175">Etiket Yardımcısı biçimlendirme içinde:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-175">In Tag Helper markup:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

<span data-ttu-id="5f8ce-176">Yukarıdaki örnekteki `PriorityList` görünümü bileşen olur `priority-list`.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-176">In the sample above, the `PriorityList` view component becomes `priority-list`.</span></span> <span data-ttu-id="5f8ce-177">Bileşeni görüntüle parametreleri alt kebab durumda öznitelik olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-177">The parameters to the view component are passed as attributes in lower kebab case.</span></span>

### <a name="invoking-a-view-component-directly-from-a-controller"></a><span data-ttu-id="5f8ce-178">Bir görünümü bileşen denetleyicisinden doğrudan çağırma</span><span class="sxs-lookup"><span data-stu-id="5f8ce-178">Invoking a view component directly from a controller</span></span>

<span data-ttu-id="5f8ce-179">Görünüm bileşenleri, genellikle bir görünümden çağrılır, ancak doğrudan bir denetleyici yöntemi çağırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-179">View components are typically invoked from a view, but you can invoke them directly from a controller method.</span></span> <span data-ttu-id="5f8ce-180">Görünüm bileşenleri denetleyicileri gibi uç noktaları tanımlamak yoktur, ancak içeriği döndüren bir denetleyici eylemi kolayca uygulayabilirsiniz bir `ViewComponentResult`.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-180">While view components don't define endpoints like controllers, you can easily implement a controller action that returns the content of a `ViewComponentResult`.</span></span>

<span data-ttu-id="5f8ce-181">Bu örnekte, doğrudan denetleyiciden görünüm bileşen çağrılır:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-181">In this example, the view component is called directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a><span data-ttu-id="5f8ce-182">İzlenecek yol: Basit Görünüm bileşeni oluşturma</span><span class="sxs-lookup"><span data-stu-id="5f8ce-182">Walkthrough: Creating a simple view component</span></span>

<span data-ttu-id="5f8ce-183">[İndirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), derleme ve Başlatıcı kodu test edin.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-183">[Download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), build and test the starter code.</span></span> <span data-ttu-id="5f8ce-184">Basit bir proje olan bir `Todo` listesini görüntüler denetleyicisi *Todo* öğeleri.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-184">It's a simple project with a `Todo` controller that displays a list of *Todo* items.</span></span>

![Açıklamada listesi](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a><span data-ttu-id="5f8ce-186">ViewComponent sınıfı Ekle</span><span class="sxs-lookup"><span data-stu-id="5f8ce-186">Add a ViewComponent class</span></span>

<span data-ttu-id="5f8ce-187">Oluşturma bir *ViewComponents* klasörü ve aşağıdaki `PriorityListViewComponent` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-187">Create a *ViewComponents* folder and add the following `PriorityListViewComponent` class:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

<span data-ttu-id="5f8ce-188">Kod ile ilgili notlar:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-188">Notes on the code:</span></span>

* <span data-ttu-id="5f8ce-189">Görünüm bileşen sınıfları kapsanıyorsa **herhangi** proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-189">View component classes can be contained in **any** folder in the project.</span></span>
* <span data-ttu-id="5f8ce-190">Sınıf adı olduğundan PriorityList**ViewComponent** soneki ile sona erer **ViewComponent**, çalışma zamanı sınıf bileşen görünümden başvururken "PriorityList" dizesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-190">Because the class name PriorityList**ViewComponent** ends with the suffix **ViewComponent**, the runtime will use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="5f8ce-191">I, daha ayrıntılı olarak daha sonra denetleyeceğinizi açıklayacağız.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-191">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="5f8ce-192">`[ViewComponent]` Öznitelik, bir görünüm bileşeni başvurmak için kullanılan adını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-192">The `[ViewComponent]` attribute can change the name used to reference a view component.</span></span> <span data-ttu-id="5f8ce-193">Örneğin, biz adlı sınıfı `XYZ` ve uygulanan `ViewComponent` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-193">For example, we could've named the class `XYZ` and applied the `ViewComponent` attribute:</span></span>

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* <span data-ttu-id="5f8ce-194">`[ViewComponent]` Özniteliği yukarıdaki söyler adı kullanacak şekilde görünümü Bileşen Seçici `PriorityList` bileşeni ile ve "PriorityList" dize sınıfı bileşen görünümden başvururken kullanılacak ilişkili görünümleri ararken.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-194">The `[ViewComponent]` attribute above tells the view component selector to use the name `PriorityList` when looking for the views associated with the component, and to use the string "PriorityList" when referencing the class component from a view.</span></span> <span data-ttu-id="5f8ce-195">I, daha ayrıntılı olarak daha sonra denetleyeceğinizi açıklayacağız.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-195">I'll explain that in more detail later.</span></span>
* <span data-ttu-id="5f8ce-196">Bileşen [bağımlılık ekleme](../../fundamentals/dependency-injection.md) veri bağlamı kullanılabilir hale getirmek için.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-196">The component uses [dependency injection](../../fundamentals/dependency-injection.md) to make the data context available.</span></span>
* <span data-ttu-id="5f8ce-197">`InvokeAsync` kullanıma sunan bir görünümü ve onu çağrılabilen bir yöntem rastgele bir sayıda bağımsız değişken alabilir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-197">`InvokeAsync` exposes a method which can be called from a view, and it can take an arbitrary number of arguments.</span></span>
* <span data-ttu-id="5f8ce-198">`InvokeAsync` Yöntemi kümesini döndürür `ToDo` karşılayan öğelerinin `isDone` ve `maxPriority` parametreleri.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-198">The `InvokeAsync` method returns the set of `ToDo` items that satisfy the `isDone` and `maxPriority` parameters.</span></span>

### <a name="create-the-view-component-razor-view"></a><span data-ttu-id="5f8ce-199">Bileşen Razor görünümünü oluşturma</span><span class="sxs-lookup"><span data-stu-id="5f8ce-199">Create the view component Razor view</span></span>

* <span data-ttu-id="5f8ce-200">Oluşturma *görünümler/paylaşılan/Components* klasör.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-200">Create the *Views/Shared/Components* folder.</span></span> <span data-ttu-id="5f8ce-201">Bu klasör **gerekir** adı *bileşenleri*.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-201">This folder **must** be named *Components*.</span></span>

* <span data-ttu-id="5f8ce-202">Oluşturma *görünümler/paylaşılan/bileşenleri/PriorityList* klasör.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-202">Create the *Views/Shared/Components/PriorityList* folder.</span></span> <span data-ttu-id="5f8ce-203">Bu klasör adı görünümü bileşen sınıfının adı ve soneki eksi sınıfının adı eşleşmelidir (kuralını izleyen ve kullandıysanız *ViewComponent* sınıf adı soneki).</span><span class="sxs-lookup"><span data-stu-id="5f8ce-203">This folder name must match the name of the view component class, or the name of the class minus the suffix (if we followed convention and used the *ViewComponent* suffix in the class name).</span></span> <span data-ttu-id="5f8ce-204">Kullandıysanız `ViewComponent` özniteliği, sınıf adı öznitelik atamasını eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-204">If you used the `ViewComponent` attribute, the class name would need to match the attribute designation.</span></span>

* <span data-ttu-id="5f8ce-205">Oluşturma bir *Views/Shared/Components/PriorityList/Default.cshtml* Razor Görünüm: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="5f8ce-205">Create a *Views/Shared/Components/PriorityList/Default.cshtml* Razor view: [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]</span></span>
    
   <span data-ttu-id="5f8ce-206">Razor görünümü listesini alır `TodoItem` ve bunları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-206">The Razor view takes a list of `TodoItem` and displays them.</span></span> <span data-ttu-id="5f8ce-207">Varsa görünümü bileşen `InvokeAsync` yöntemi (olduğu gibi örneğimizi), görünüm adını geçirin değil *varsayılan* görünümü adı için kural olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-207">If the view component `InvokeAsync` method doesn't pass the name of the view (as in our sample), *Default* is used for the view name by convention.</span></span> <span data-ttu-id="5f8ce-208">Öğreticinin sonraki bölümlerinde miyim, görünümün adının ona nasıl iletileceğini göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-208">Later in the tutorial, I'll show you how to pass the name of the view.</span></span> <span data-ttu-id="5f8ce-209">Belirli bir denetleyicinin varsayılan stillerini geçersiz kılmak için bir görünüm denetleyicisine özgü görünüm klasöre ekleyin (örneğin *Views/Todo/Components/PriorityList/Default.cshtml)*.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-209">To override the default styling for a specific controller, add a view to the controller-specific view folder (for example *Views/Todo/Components/PriorityList/Default.cshtml)*.</span></span>
    
    <span data-ttu-id="5f8ce-210">Denetleyicisine özgü görünüm bileşeni ise denetleyicisi özgü klasöre ekleyebilirsiniz (*Views/Todo/Components/PriorityList/Default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="5f8ce-210">If the view component is controller-specific, you can add it to the controller-specific folder (*Views/Todo/Components/PriorityList/Default.cshtml*).</span></span>

* <span data-ttu-id="5f8ce-211">Ekleme bir `div` altına öncelik listesi bileşeni için bir çağrı içeren *Views/Todo/index.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-211">Add a `div` containing a call to the priority list component to the bottom of the *Views/Todo/index.cshtml* file:</span></span>

    [!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

<span data-ttu-id="5f8ce-212">Biçimlendirme `@await Component.InvokeAsync` görünüm bileşenleri çağırma söz dizimi görülmektedir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-212">The markup `@await Component.InvokeAsync` shows the syntax for calling view components.</span></span> <span data-ttu-id="5f8ce-213">İlk bağımsız değişken çağırma veya çağrı istiyoruz bileşen adıdır.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-213">The first argument is the name of the component we want to invoke or call.</span></span> <span data-ttu-id="5f8ce-214">Sonraki parametreler bileşenine aktarılır.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-214">Subsequent parameters are passed to the component.</span></span> <span data-ttu-id="5f8ce-215">`InvokeAsync` rastgele bir sayıda bağımsız değişken alabilir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-215">`InvokeAsync` can take an arbitrary number of arguments.</span></span>

<span data-ttu-id="5f8ce-216">Uygulamayı test etme.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-216">Test the app.</span></span> <span data-ttu-id="5f8ce-217">Aşağıdaki görüntüde, yapılacaklar listesi ve öncelikli öğeleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-217">The following image shows the ToDo list and the priority items:</span></span>

![Yapılacaklar listesi ve öncelikli öğeleri](view-components/_static/pi.png)

<span data-ttu-id="5f8ce-219">Doğrudan denetleyiciden görünüm bileşeni de çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-219">You can also call the view component directly from the controller:</span></span>

[!code-csharp[](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![öncelikli öğeleri IndexVC eylemi](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a><span data-ttu-id="5f8ce-221">Görünüm adı belirtme</span><span class="sxs-lookup"><span data-stu-id="5f8ce-221">Specifying a view name</span></span>

<span data-ttu-id="5f8ce-222">Karmaşık görünümü bileşen, bazı koşullar altında bir varsayılan olmayan görünüm belirtmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-222">A complex view component might need to specify a non-default view under some conditions.</span></span> <span data-ttu-id="5f8ce-223">Aşağıdaki kod "PVC" görünümünden belirteceğiniz gösterilmektedir `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-223">The following code shows how to specify the "PVC" view  from the `InvokeAsync` method.</span></span> <span data-ttu-id="5f8ce-224">Güncelleştirme `InvokeAsync` yönteminde `PriorityListViewComponent` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-224">Update the `InvokeAsync` method in the `PriorityListViewComponent` class.</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

<span data-ttu-id="5f8ce-225">Kopyalama *Views/Shared/Components/PriorityList/Default.cshtml* adlı bir görünüm dosyasına *Views/Shared/Components/PriorityList/PVC.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-225">Copy the *Views/Shared/Components/PriorityList/Default.cshtml* file to a view named *Views/Shared/Components/PriorityList/PVC.cshtml*.</span></span> <span data-ttu-id="5f8ce-226">PVC görünümü kullanıldığını belirtmek için bir başlık ekleyin.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-226">Add a heading to indicate the PVC view is being used.</span></span>

[!code-cshtml[](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

<span data-ttu-id="5f8ce-227">Güncelleştirme *Views/TodoList/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-227">Update *Views/TodoList/Index.cshtml*:</span></span>

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

<span data-ttu-id="5f8ce-228">Uygulamayı çalıştırın ve PVC görünümü doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-228">Run the app and verify PVC view.</span></span>

![Öncelik bileşeni görüntüle](view-components/_static/pvc.png)

<span data-ttu-id="5f8ce-230">PVC görünüm işlenen değil, 4 veya daha yüksek bir önceliğe sahip görünümü bileşen aradığınız doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-230">If the PVC view isn't rendered, verify you are calling the view component with a priority of 4 or higher.</span></span>

### <a name="examine-the-view-path"></a><span data-ttu-id="5f8ce-231">Görünüm yolunu inceleyin</span><span class="sxs-lookup"><span data-stu-id="5f8ce-231">Examine the view path</span></span>

* <span data-ttu-id="5f8ce-232">Öncelik parametresi, üç veya daha düşük öncelikli görünüm döndürülmez şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-232">Change the priority parameter to three or less so the priority view isn't returned.</span></span>
* <span data-ttu-id="5f8ce-233">Geçici olarak yeniden adlandırın *Views/Todo/Components/PriorityList/Default.cshtml* için *1Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-233">Temporarily rename the *Views/Todo/Components/PriorityList/Default.cshtml* to *1Default.cshtml*.</span></span>
* <span data-ttu-id="5f8ce-234">Uygulamayı test etme, aşağıdaki hatayı alırsınız:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-234">Test the app, you'll get the following error:</span></span>

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' wasn't found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* <span data-ttu-id="5f8ce-235">Kopyalama *Views/Todo/Components/PriorityList/1Default.cshtml* için *Views/Shared/Components/PriorityList/Default.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-235">Copy *Views/Todo/Components/PriorityList/1Default.cshtml* to *Views/Shared/Components/PriorityList/Default.cshtml*.</span></span>
* <span data-ttu-id="5f8ce-236">Bazı biçimlendirme eklemek *paylaşılan* Todo görünümü bileşen görünümü, görünümün belirtmenizi geldiği *paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-236">Add some markup to the *Shared* Todo view component view to indicate the view is from the *Shared* folder.</span></span>
* <span data-ttu-id="5f8ce-237">Test **paylaşılan** bileşeni görünümü.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-237">Test the **Shared** component view.</span></span>

![ToDo çıkış paylaşılan bileşen görünümü](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a><span data-ttu-id="5f8ce-239">Sihirli dize kaçınma</span><span class="sxs-lookup"><span data-stu-id="5f8ce-239">Avoiding magic strings</span></span>

<span data-ttu-id="5f8ce-240">Zaman güvenlik derlemek isterseniz, sabit kodlanmış görünümü bileşen adı sınıf adını değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="5f8ce-240">If you want compile time safety, you can replace the hard-coded view component name with the class name.</span></span> <span data-ttu-id="5f8ce-241">Bileşeni görüntüle "ViewComponent" soneki olmadan oluşturun:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-241">Create the view component without the "ViewComponent" suffix:</span></span>

[!code-csharp[](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

<span data-ttu-id="5f8ce-242">Ekleme bir `using` , Razor ifadesine dosyayı görüntüle ve Kullan `nameof` işleci:</span><span class="sxs-lookup"><span data-stu-id="5f8ce-242">Add a `using` statement to your Razor view file, and use the `nameof` operator:</span></span>

[!code-cshtml[](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,35-)]

## <a name="additional-resources"></a><span data-ttu-id="5f8ce-243">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="5f8ce-243">Additional resources</span></span>

* [<span data-ttu-id="5f8ce-244">Görünümlere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="5f8ce-244">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
