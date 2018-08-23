---
title: ASP.NET core'da düzeni
author: ardalis
description: Yaygın düzenlerini kullanmayı, yönergeleri paylaşın ve işleme görünümleri önce ortak kod içinde ASP.NET Core uygulaması çalıştırma hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: ad0b339572f387be8a636204015ffc361947acb8
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754046"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="ca6f2-103">ASP.NET core'da düzeni</span><span class="sxs-lookup"><span data-stu-id="ca6f2-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="ca6f2-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ca6f2-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ca6f2-105">Görünümler, görsel ve programlama öğeleri sık paylaşın.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="ca6f2-106">Bu makalede, yaygın düzenlerini kullanmayı, yönergeleri paylaşın ve ASP.NET Core uygulamanızı oluşturma görünümleri önce ortak kod çalıştırmak öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET Core app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="ca6f2-107">Bir düzen nedir</span><span class="sxs-lookup"><span data-stu-id="ca6f2-107">What is a Layout</span></span>

<span data-ttu-id="ca6f2-108">Çoğu web uygulaması, sayfalar arasında gezinirken, kullanıcı ile tutarlı bir deneyim sağlayan ortak bir düzeni vardır.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="ca6f2-109">Düzen, genellikle Uygulama Başlığı, gezinti veya menü öğeleri ve alt bilgi gibi ortak kullanıcı arabirimi öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Sayfa düzeni örneği](layout/_static/page-layout.png)

<span data-ttu-id="ca6f2-111">Betikleri ve stil sayfalarını gibi ortak HTML yapıları, bir uygulama içinde birçok sayfaları da sık sık kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="ca6f2-112">Tüm bu paylaşılan öğeleri içinde tanımlanabilir bir *Düzen* dosya, uygulama içinde kullanılan herhangi bir görünüm tarafından başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="ca6f2-113">Düzenleri azaltmaya yardımcı görünümlerde yinelenen kod izleyin [yoksa yineleyin kendiniz (KURU) İlkesi](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="ca6f2-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="ca6f2-114">Kural gereği, ASP.NET Core uygulaması için varsayılan düzen adlı `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-114">By convention, the default layout for an ASP.NET Core app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="ca6f2-115">Visual Studio ASP.NET Core MVC proje şablonu Düzen bu dosyada içerir `Views/Shared` klasörü:</span><span class="sxs-lookup"><span data-stu-id="ca6f2-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Çözüm Gezgini klasöründe görünümleri](layout/_static/web-project-views.png)

<span data-ttu-id="ca6f2-117">Bu düzen, bir üst düzey şablonu görünümler için uygulamada tanımlar.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="ca6f2-118">Uygulamaları bir düzen gerektirmeyen ve uygulamaları alan farklı düzenler belirterek farklı görünümleri ile birden fazla Düzen tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="ca6f2-119">Bir örnek `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="ca6f2-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="ca6f2-120">Bir düzen belirtme</span><span class="sxs-lookup"><span data-stu-id="ca6f2-120">Specifying a Layout</span></span>

<span data-ttu-id="ca6f2-121">Razor görünümleri olan bir `Layout` özelliği.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="ca6f2-122">Tek bir görünüm bu özelliğini ayarlayarak bir düzen belirtin:</span><span class="sxs-lookup"><span data-stu-id="ca6f2-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="ca6f2-123">Belirtilen düzen bir tam yol kullanabilirsiniz (örnek: `/Views/Shared/_Layout.cshtml`) ya da kısmi bir ad (örnek: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="ca6f2-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="ca6f2-124">Kısmi bir adı sağlandığında, Razor görünüm altyapısını kullanarak kendi standart bulma işlemi için yerleşim dosyası arar.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="ca6f2-125">Denetleyici ilişkili klasörü, ilk olarak, arkasından aranır `Shared` klasör.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="ca6f2-126">Bu bulma işlemi için keşfetmek için kullanılan bir aynıdır [kısmi görünümler](partial.md).</span><span class="sxs-lookup"><span data-stu-id="ca6f2-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="ca6f2-127">Varsayılan olarak, her Düzen çağırmalıdır `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="ca6f2-128">Her yerde çağrısı `RenderBody` olan konumdaki görünüm içeriğinin işlenir.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="ca6f2-129">Bölümler</span><span class="sxs-lookup"><span data-stu-id="ca6f2-129">Sections</span></span>

<span data-ttu-id="ca6f2-130">Bir düzen, isteğe bağlı olarak bir veya daha fazla başvurabilirsiniz *bölümleri*, çağırarak `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="ca6f2-131">Bölümler, belirli sayfa öğeleri nereye yerleştirileceğini düzenlemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="ca6f2-132">Her çağrı `RenderSection` bu bölümün gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="ca6f2-133">Gerekli bölüm bulunamazsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="ca6f2-134">Tek bir görünüm içinde bir bölümde kullanılarak oluşturulması için içeriği belirtin `@section` Razor söz dizimi.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="ca6f2-135">Görünüm bir bölüm tanımlar, işlenen gerekir (veya bir hata meydana gelir).</span><span class="sxs-lookup"><span data-stu-id="ca6f2-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="ca6f2-136">Bir örnek `@section` görünüm tanımında:</span><span class="sxs-lookup"><span data-stu-id="ca6f2-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="ca6f2-137">Yukarıdaki kod doğrulama betikleri için eklenen `scripts` bölümü bir form içeren bir görünümü.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="ca6f2-138">Aynı uygulamayı diğer görünümlerde herhangi ek komut dosyası gerekli değil ve bu nedenle betikleri bölümü tanımlaması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="ca6f2-139">Bir görünümde tanımlı bölüm, yalnızca kendi anlık düzen sayfası içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="ca6f2-140">Bunlar, kısmi görünüm bileşenleri veya Görünüm sistemin diğer bölümlerini başvurulamaz.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="ca6f2-141">Bölümler yoksayılıyor</span><span class="sxs-lookup"><span data-stu-id="ca6f2-141">Ignoring sections</span></span>

<span data-ttu-id="ca6f2-142">Varsayılan olarak, gövdesini ve içerik sayfasındaki tüm bölümlerin tümünü düzen sayfası tarafından oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="ca6f2-143">Razor görüntüleme motorunu gövdesi ve her bölümde oluşturulmasını isteyip izleyerek zorlar.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="ca6f2-144">Gövde veya bölüm yok saymak için Görünüm altyapısı açmasını sağlamak için çağrı `IgnoreBody` ve `IgnoreSection` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="ca6f2-145">Gövde ve her bölümde bir Razor sayfası işlenen yoksayıldı veya gerekir.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="ca6f2-146">Paylaşılan yönergeleri alma</span><span class="sxs-lookup"><span data-stu-id="ca6f2-146">Importing Shared Directives</span></span>

<span data-ttu-id="ca6f2-147">Görünümler, ad alanlarını alma veya gerçekleştirme gibi pek çok şeyi yapmak için Razor yönergeleri kullanabilir [bağımlılık ekleme](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ca6f2-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="ca6f2-148">Yönergeleri çoğu görünümler tarafından paylaşılan ortak belirtilen `_ViewImports.cshtml` dosya.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="ca6f2-149">`_ViewImports` Dosyasını aşağıdaki yönergeleri destekler:</span><span class="sxs-lookup"><span data-stu-id="ca6f2-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="ca6f2-150">Dosya, İşlevler ve bölüm tanımları gibi diğer Razor özellikleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="ca6f2-151">Bir örnek `_ViewImports.cshtml` dosyası:</span><span class="sxs-lookup"><span data-stu-id="ca6f2-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="ca6f2-152">`_ViewImports.cshtml` Bir ASP.NET Core MVC uygulaması genellikle yerleştirilir için dosya `Views` klasör.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="ca6f2-153">A `_ViewImports.cshtml` içinde çalışması, yalnızca uygulanacak görünümleri, klasör ve alt klasörleri içinde herhangi bir klasör içinde dosya yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="ca6f2-154">`_ViewImports` dosyaları kök düzeyinde başlangıç işlenir ve ardından kadar önde gelen her bir klasör için görünümünün kendisinde konumu ayarları kök düzeyinde belirtilen şekilde geçersiz klasör düzeyinde.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="ca6f2-155">Örneğin, bir kök düzeyinde `_ViewImports.cshtml` dosyasını belirtir `@model` ve `@addTagHelper`ve başka bir `_ViewImports.cshtml` farklı bir görünüm denetleyicisi ilişkili bir klasörde dosya belirtir `@model` ve başka ekler `@addTagHelper`, görünümü Her iki etiket Yardımcıları erişebilir ve ikincisi kullanacağı `@model`.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="ca6f2-156">Birden çok `_ViewImports.cshtml` dosyaları için bir görünüm çalıştırın, birlikte bulunan yönergeleri davranışını `ViewImports.cshtml` dosyaları şu şekilde olacaktır:</span><span class="sxs-lookup"><span data-stu-id="ca6f2-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="ca6f2-157">`@addTagHelper`, `@removeTagHelper`: sırayla tüm çalışma</span><span class="sxs-lookup"><span data-stu-id="ca6f2-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="ca6f2-158">`@tagHelperPrefix`: en yakındakine görünümüne başka geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="ca6f2-159">`@model`: en yakındakine görünümüne başka geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="ca6f2-160">`@inherits`: en yakındakine görünümüne başka geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="ca6f2-161">`@using`: tüm; dahildir yinelenenler yoksayıldı</span><span class="sxs-lookup"><span data-stu-id="ca6f2-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="ca6f2-162">`@inject`: her bir özellik için en yakın bir görünüm için aynı adla başkalarıyla geçersiz kılar</span><span class="sxs-lookup"><span data-stu-id="ca6f2-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="ca6f2-163">Her görünüm önce kod çalıştırma</span><span class="sxs-lookup"><span data-stu-id="ca6f2-163">Running Code Before Each View</span></span>

<span data-ttu-id="ca6f2-164">Sahip kod önce her bir görünüm çalıştırmanız gerekir, bu yerleştirilmelidir `_ViewStart.cshtml` dosya.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="ca6f2-165">Kural olarak, `_ViewStart.cshtml` dosyası `Views` klasör.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="ca6f2-166">Listelenen deyimleri `_ViewStart.cshtml` önce her tam görünüm (değil düzenleri ve kısmi görünümler) çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="ca6f2-167">Gibi [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` hiyerarşik olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="ca6f2-168">Varsa bir `_ViewStart.cshtml` dosya görünümü denetleyicisi ilişkili klasöründe tanımlanır, kök dizininde tanımlananla sonra çalıştırılacak `Views` klasörü (varsa).</span><span class="sxs-lookup"><span data-stu-id="ca6f2-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="ca6f2-169">Bir örnek `_ViewStart.cshtml` dosyası:</span><span class="sxs-lookup"><span data-stu-id="ca6f2-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="ca6f2-170">Yukarıdaki dosyanın tüm görünümlere kullanacağını belirtir `_Layout.cshtml` düzeni.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="ca6f2-171">Ne `_ViewStart.cshtml` ya da `_ViewImports.cshtml` genelde yerleştirilir `/Views/Shared` klasör.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="ca6f2-172">Uygulama düzeyinde bu dosyaların sürümleri doğrudan yerleştirilmelidir `/Views` klasör.</span><span class="sxs-lookup"><span data-stu-id="ca6f2-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
