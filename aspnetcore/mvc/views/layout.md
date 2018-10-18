---
title: ASP.NET core'da düzeni
author: ardalis
description: Yaygın düzenlerini kullanmayı, yönergeleri paylaşın ve işleme görünümleri önce ortak kod içinde ASP.NET Core uygulaması çalıştırma hakkında bilgi edinin.
ms.author: riande
ms.date: 10/18/2018
uid: mvc/views/layout
ms.openlocfilehash: b23fd4e0b1d91a4dd5aae548aa2b2081aa37a561
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391303"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="41868-103">ASP.NET core'da düzeni</span><span class="sxs-lookup"><span data-stu-id="41868-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="41868-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Dave Brock](https://twitter.com/daveabrock)</span><span class="sxs-lookup"><span data-stu-id="41868-104">By [Steve Smith](https://ardalis.com/) and [Dave Brock](https://twitter.com/daveabrock)</span></span>

<span data-ttu-id="41868-105">Sayfalar ve görünümler sık görsel ve programlama öğeleri paylaşın.</span><span class="sxs-lookup"><span data-stu-id="41868-105">Pages and views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="41868-106">Bu makalede gösterilmiştir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="41868-106">This article demonstrates how to:</span></span>

* <span data-ttu-id="41868-107">Ortak düzenler kullanın.</span><span class="sxs-lookup"><span data-stu-id="41868-107">Use common layouts.</span></span>
* <span data-ttu-id="41868-108">Yönergeleri paylaşın.</span><span class="sxs-lookup"><span data-stu-id="41868-108">Share directives.</span></span>
* <span data-ttu-id="41868-109">İşleme sayfaları veya görünümleri önce ortak kodu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="41868-109">Run common code before rendering pages or views.</span></span>

<span data-ttu-id="41868-110">Bu belge için ASP.NET Core MVC iki farklı yaklaşım düzenleri ele alınmaktadır: Razor sayfaları ve görünüm denetleyicileri.</span><span class="sxs-lookup"><span data-stu-id="41868-110">This document discusses layouts for the two different approaches to ASP.NET Core MVC: Razor Pages and controllers with views.</span></span> <span data-ttu-id="41868-111">Bu konu için en az bir fark vardır:</span><span class="sxs-lookup"><span data-stu-id="41868-111">For this topic, the differences are minimal:</span></span>

* <span data-ttu-id="41868-112">Razor sayfaları bulunduğunuz *sayfaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="41868-112">Razor Pages are in the *Pages* folder.</span></span>
* <span data-ttu-id="41868-113">Görünümler kullanan denetleyicileriyle bir *görünümleri* görünümleri için klasör.</span><span class="sxs-lookup"><span data-stu-id="41868-113">Controllers with views uses a *Views* folder for views.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="41868-114">Bir düzen nedir</span><span class="sxs-lookup"><span data-stu-id="41868-114">What is a Layout</span></span>

<span data-ttu-id="41868-115">Çoğu web uygulaması, sayfalar arasında gezinirken, kullanıcı ile tutarlı bir deneyim sağlayan ortak bir düzeni vardır.</span><span class="sxs-lookup"><span data-stu-id="41868-115">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="41868-116">Düzen, genellikle Uygulama Başlığı, gezinti veya menü öğeleri ve alt bilgi gibi ortak kullanıcı arabirimi öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="41868-116">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Sayfa düzeni örneği](layout/_static/page-layout.png)

<span data-ttu-id="41868-118">Betikleri ve stil sayfalarını gibi ortak HTML yapıları, bir uygulama içinde birçok sayfaları da sık sık kullanılır.</span><span class="sxs-lookup"><span data-stu-id="41868-118">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="41868-119">Tüm bu paylaşılan öğeleri içinde tanımlanabilir bir *Düzen* dosya, uygulama içinde kullanılan herhangi bir görünüm tarafından başvurulabilir.</span><span class="sxs-lookup"><span data-stu-id="41868-119">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="41868-120">Düzenleri azaltmaya yardımcı görünümlerde yinelenen kod izleyin [yoksa yineleyin kendiniz (KURU) İlkesi](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="41868-120">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="41868-121">Kural gereği, ASP.NET Core uygulaması için varsayılan düzen adlı *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="41868-121">By convention, the default layout for an ASP.NET Core app is named *_Layout.cshtml*.</span></span> <span data-ttu-id="41868-122">Düzen dosyası şablonları ile oluşturulan yeni ASP.NET Core projeleri için:</span><span class="sxs-lookup"><span data-stu-id="41868-122">The layout file for new ASP.NET Core projects created with the templates:</span></span>

* <span data-ttu-id="41868-123">Razor sayfaları: *Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="41868-123">Razor Pages: *Pages/Shared/_Layout.cshtml*</span></span>

  ![Çözüm Gezgini sayfalar klasöründe](layout/_static/rp-web-project-views.png)

* <span data-ttu-id="41868-125">Görünüm denetleyicisi: *Views/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="41868-125">Controller with views: *Views/Shared/_Layout.cshtml*</span></span>

 ![Çözüm Gezgini klasöründe görünümleri](layout/_static/mvc-web-project-views.png)

<span data-ttu-id="41868-127">Düzen görünümleri için üst düzey şablon uygulamada tanımlar.</span><span class="sxs-lookup"><span data-stu-id="41868-127">The layout defines a top level template for views in the app.</span></span> <span data-ttu-id="41868-128">Uygulamaları bir düzen gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="41868-128">Apps don't require a layout.</span></span> <span data-ttu-id="41868-129">Uygulamaları farklı görünümler alan farklı düzenler belirten birden fazla Düzen tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="41868-129">Apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="41868-130">Aşağıdaki kod projesi bir denetleyici ve görünümler ile oluşturulmuş bir şablonu Düzen dosyası gösterir:</span><span class="sxs-lookup"><span data-stu-id="41868-130">The following code shows the layout file for a template created project with a controller and views:</span></span>

[!code-html[](~/common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=44,72)]

## <a name="specifying-a-layout"></a><span data-ttu-id="41868-131">Bir düzen belirtme</span><span class="sxs-lookup"><span data-stu-id="41868-131">Specifying a Layout</span></span>

<span data-ttu-id="41868-132">Razor görünümleri olan bir `Layout` özelliği.</span><span class="sxs-lookup"><span data-stu-id="41868-132">Razor views have a `Layout` property.</span></span> <span data-ttu-id="41868-133">Tek bir görünüm bu özelliğini ayarlayarak bir düzen belirtin:</span><span class="sxs-lookup"><span data-stu-id="41868-133">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="41868-134">Belirtilen düzen bir tam yol kullanabilirsiniz (örneğin, */Pages/Shared/_Layout.cshtml* veya */Views/Shared/_Layout.cshtml*) ya da kısmi bir ad (örnek: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="41868-134">The layout specified can use a full path (for example, */Pages/Shared/_Layout.cshtml* or */Views/Shared/_Layout.cshtml*) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="41868-135">Kısmi bir adı sağlandığında, Razor görünüm altyapısını kullanarak kendi standart bulma işlemi için yerleşim dosyası arar.</span><span class="sxs-lookup"><span data-stu-id="41868-135">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="41868-136">İşleyici yöntemi (veya denetleyicisi) bulunduğu klasör, ilk olarak, arkasından aranır *paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="41868-136">The folder where the handler method (or controller) exists is searched first, followed by the *Shared* folder.</span></span> <span data-ttu-id="41868-137">Bu bulma işlemi için keşfetmek için kullanılan bir aynıdır [kısmi görünümler](partial.md).</span><span class="sxs-lookup"><span data-stu-id="41868-137">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="41868-138">Varsayılan olarak, her Düzen çağırmalıdır `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="41868-138">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="41868-139">Her yerde çağrısı `RenderBody` olan konumdaki görünüm içeriğinin işlenir.</span><span class="sxs-lookup"><span data-stu-id="41868-139">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="41868-140">Bölümler</span><span class="sxs-lookup"><span data-stu-id="41868-140">Sections</span></span>

<span data-ttu-id="41868-141">Bir düzen, isteğe bağlı olarak bir veya daha fazla başvurabilirsiniz *bölümleri*, çağırarak `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="41868-141">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="41868-142">Bölümler, belirli sayfa öğeleri nereye yerleştirileceğini düzenlemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="41868-142">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="41868-143">Her çağrı `RenderSection` bu bölümün gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="41868-143">Each call to `RenderSection` can specify whether that section is required or optional:</span></span>

```html
@section Scripts {
    @RenderSection("Scripts", required: false)
}
```

<span data-ttu-id="41868-144">Gerekli bölüm bulunamazsa, bir özel durum oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="41868-144">If a required section isn't found, an exception is thrown.</span></span> <span data-ttu-id="41868-145">Tek bir görünüm içinde bir bölümde kullanılarak oluşturulması için içeriği belirtin `@section` Razor söz dizimi.</span><span class="sxs-lookup"><span data-stu-id="41868-145">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="41868-146">Bir sayfa ya da görünümün bir bölüm tanımlar, işlenen gerekir (veya bir hata meydana gelir).</span><span class="sxs-lookup"><span data-stu-id="41868-146">If a page or view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="41868-147">Bir örnek `@section` Razor sayfaları görünüm tanımında:</span><span class="sxs-lookup"><span data-stu-id="41868-147">An example `@section` definition in Razor Pages view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
}
```

<span data-ttu-id="41868-148">Önceki kodda, *scripts/main.js* eklenir `scripts` bir sayfa ya da Görünüm bölümü.</span><span class="sxs-lookup"><span data-stu-id="41868-148">In the preceding code, *scripts/main.js* is added to the `scripts` section on a page or view.</span></span> <span data-ttu-id="41868-149">Diğer sayfaları veya görünümleri aynı uygulamada bu betik gerekli değil ve betikleri bölüm tanımlayın mıydı.</span><span class="sxs-lookup"><span data-stu-id="41868-149">Other pages or views in the same app might not require this script and wouldn't define a scripts section.</span></span>

<span data-ttu-id="41868-150">Aşağıdaki biçimlendirmede kullanan [kısmi etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) işlenecek *_ValidationScriptsPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="41868-150">The following markup uses the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) to render  *_ValidationScriptsPartial.cshtml*:</span></span>

```html
@section Scripts {
    <partial name="_ValidationScriptsPartial" />
}
```

<span data-ttu-id="41868-151">Önceki biçimlendirme tarafından oluşturulmuş [kimlik iskele kurma özelliği](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="41868-151">The preceding markup was generated by [scaffolding Identity](xref:security/authentication/scaffold-identity).</span></span>

<span data-ttu-id="41868-152">Bir sayfa veya görünümde tanımlı bölüm, yalnızca kendi anlık düzen sayfası içinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="41868-152">Sections defined in a page or view are available only in its immediate layout page.</span></span> <span data-ttu-id="41868-153">Bunlar, kısmi görünüm bileşenleri veya Görünüm sistemin diğer bölümlerini başvurulamaz.</span><span class="sxs-lookup"><span data-stu-id="41868-153">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="41868-154">Bölümler yoksayılıyor</span><span class="sxs-lookup"><span data-stu-id="41868-154">Ignoring sections</span></span>

<span data-ttu-id="41868-155">Varsayılan olarak, gövdesini ve içerik sayfasındaki tüm bölümlerin tümünü düzen sayfası tarafından oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="41868-155">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="41868-156">Razor görüntüleme motorunu gövdesi ve her bölümde oluşturulmasını isteyip izleyerek zorlar.</span><span class="sxs-lookup"><span data-stu-id="41868-156">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="41868-157">Gövde veya bölüm yok saymak için Görünüm altyapısı açmasını sağlamak için çağrı `IgnoreBody` ve `IgnoreSection` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="41868-157">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="41868-158">Gövde ve her bölümde bir Razor sayfası işlenen yoksayıldı veya gerekir.</span><span class="sxs-lookup"><span data-stu-id="41868-158">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="41868-159">Paylaşılan yönergeleri alma</span><span class="sxs-lookup"><span data-stu-id="41868-159">Importing Shared Directives</span></span>

<span data-ttu-id="41868-160">Görünümlere ve sayfalara Razor ad alanları ve kullanım içeri aktarma yönergelerini kullanabilirsiniz [bağımlılık ekleme](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="41868-160">Views and pages can use Razor directives to importing namespaces and use [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="41868-161">Yönergeleri çoğu görünümler tarafından paylaşılan ortak belirtilen *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="41868-161">Directives shared by many views may be specified in a common *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="41868-162">`_ViewImports` Dosyasını aşağıdaki yönergeleri destekler:</span><span class="sxs-lookup"><span data-stu-id="41868-162">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`
* `@removeTagHelper`
* `@tagHelperPrefix`
* `@using`
* `@model`
* `@inherits`
* `@inject`

<span data-ttu-id="41868-163">Dosya, İşlevler ve bölüm tanımları gibi diğer Razor özellikleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="41868-163">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="41868-164">Bir örnek `_ViewImports.cshtml` dosyası:</span><span class="sxs-lookup"><span data-stu-id="41868-164">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="41868-165">*_Viewımports.cshtml* bir ASP.NET Core MVC uygulaması genellikle yerleştirilir için dosya *sayfaları* (veya *görünümleri*) klasörü.</span><span class="sxs-lookup"><span data-stu-id="41868-165">The *_ViewImports.cshtml* file for an ASP.NET Core MVC app is typically placed in the *Pages* (or *Views*) folder.</span></span> <span data-ttu-id="41868-166">A *_viewımports.cshtml* herhangi bir klasör içinde dosya yerleştirilebileceğini, bu durumda, yalnızca sayfa veya görünümler bu klasöre ve alt klasörleri içinde uygulanır.</span><span class="sxs-lookup"><span data-stu-id="41868-166">A *_ViewImports.cshtml* file can be placed within any folder, in which case it will only be applied to pages or views within that folder and its subfolders.</span></span> <span data-ttu-id="41868-167">`_ViewImports` dosyaları kök düzeyinde ve ardından sayfanın konumunu öncesinde her klasör için başlangıç işlenir veya kendisini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="41868-167">`_ViewImports` files are processed starting at the root level and then for each folder leading up to the location of the page or view itself.</span></span> <span data-ttu-id="41868-168">`_ViewImports` kök düzeyindeki ayarları klasör düzeyinde geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="41868-168">`_ViewImports` settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="41868-169">Örneğin, varsayalım:</span><span class="sxs-lookup"><span data-stu-id="41868-169">For example, suppose:</span></span>

* <span data-ttu-id="41868-170">Kök düzeyinde *_viewımports.cshtml* dosyasını içeren `@model MyModel1` ve `@addTagHelper *, MyTagHelper1`.</span><span class="sxs-lookup"><span data-stu-id="41868-170">The  root level *_ViewImports.cshtml* file includes `@model MyModel1` and `@addTagHelper *, MyTagHelper1`.</span></span>
* <span data-ttu-id="41868-171">Bir alt klasör *_viewımports.cshtml* dosyasını içeren `@model MyModel2` ve `@addTagHelper *, MyTagHelper2`.</span><span class="sxs-lookup"><span data-stu-id="41868-171">A subfolder  *_ViewImports.cshtml* file includes `@model MyModel2` and `@addTagHelper *, MyTagHelper2`.</span></span>

<span data-ttu-id="41868-172">Sayfalar ve görünümler alt iki etiket Yardımcıları erişimi olacaktır ve `MyModel2` modeli.</span><span class="sxs-lookup"><span data-stu-id="41868-172">Pages and views in the subfolder will have access to both Tag Helpers and the `MyModel2` model.</span></span>

<span data-ttu-id="41868-173">Birden çok *_viewımports.cshtml* yönergeleri birleşik davranışını olan dosyaları dosya hiyerarşide bulunur:</span><span class="sxs-lookup"><span data-stu-id="41868-173">If multiple *_ViewImports.cshtml* files are found in the file hierarchy, the combined behavior of the directives are:</span></span>

* <span data-ttu-id="41868-174">`@addTagHelper`, `@removeTagHelper`: sırayla tüm çalışma</span><span class="sxs-lookup"><span data-stu-id="41868-174">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>
* <span data-ttu-id="41868-175">`@tagHelperPrefix`: en yakındakine görünümüne başka geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="41868-175">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="41868-176">`@model`: en yakındakine görünümüne başka geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="41868-176">`@model`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="41868-177">`@inherits`: en yakındakine görünümüne başka geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="41868-177">`@inherits`: the closest one to the view overrides any others</span></span>
* <span data-ttu-id="41868-178">`@using`: tüm; dahildir yinelenenler yoksayıldı</span><span class="sxs-lookup"><span data-stu-id="41868-178">`@using`: all are included; duplicates are ignored</span></span>
* <span data-ttu-id="41868-179">`@inject`: her bir özellik için en yakın bir görünüm için aynı adla başkalarıyla geçersiz kılar</span><span class="sxs-lookup"><span data-stu-id="41868-179">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="41868-180">Her görünüm önce kod çalıştırma</span><span class="sxs-lookup"><span data-stu-id="41868-180">Running Code Before Each View</span></span>

<span data-ttu-id="41868-181">Her görünüm veya sayfa önce çalıştırmak için gereken kodu yerleştirilmelidir *_ViewStart.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="41868-181">Code that needs to run before each view or page should be placed in the *_ViewStart.cshtml* file.</span></span> <span data-ttu-id="41868-182">Kural olarak, *_ViewStart.cshtml* dosyası *sayfaları* (veya *görünümleri*) klasörü.</span><span class="sxs-lookup"><span data-stu-id="41868-182">By convention, the *_ViewStart.cshtml* file is located in the *Pages* (or *Views*) folder.</span></span> <span data-ttu-id="41868-183">Listelenen deyimleri *_ViewStart.cshtml* önce her tam görünüm (değil düzenleri ve kısmi görünümler) çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="41868-183">The statements listed in *_ViewStart.cshtml* are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="41868-184">Gibi [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* hiyerarşik olduğu anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="41868-184">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), *_ViewStart.cshtml* is hierarchical.</span></span> <span data-ttu-id="41868-185">Varsa bir *_ViewStart.cshtml* dosya tanımlanır görünümü veya sayfalar klasöründe, kök dizininde tanımlananla sonra çalıştırılacak *sayfaları* (veya *görünümleri*) klasörü (varsa).</span><span class="sxs-lookup"><span data-stu-id="41868-185">If a *_ViewStart.cshtml* file is defined in the view or pages folder, it will be run after the one defined in the root of the *Pages* (or *Views*) folder (if any).</span></span>

<span data-ttu-id="41868-186">Bir örnek *_ViewStart.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="41868-186">A sample *_ViewStart.cshtml* file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="41868-187">Yukarıdaki dosyanın tüm görünümlere kullanacağını belirtir *_Layout.cshtml* düzeni.</span><span class="sxs-lookup"><span data-stu-id="41868-187">The file above specifies that all views will use the *_Layout.cshtml* layout.</span></span>

<span data-ttu-id="41868-188">*_ViewStart.cshtml* ve *_viewımports.cshtml* olan **değil** genellikle yerleştirilen */sayfaları/paylaşılan* (veya   */görünümler/paylaşılan*) klasör.</span><span class="sxs-lookup"><span data-stu-id="41868-188">*_ViewStart.cshtml* and *_ViewImports.cshtml* are **not** typically placed in the */Pages/Shared* (or */Views/Shared*) folder.</span></span> <span data-ttu-id="41868-189">Uygulama düzeyinde bu dosyaların sürümleri doğrudan yerleştirilmelidir */sayfaları* (veya */görünümler*) klasörü.</span><span class="sxs-lookup"><span data-stu-id="41868-189">The app-level versions of these files should be placed directly in the */Pages* (or */Views*) folder.</span></span>
