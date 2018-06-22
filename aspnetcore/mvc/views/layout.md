---
title: ASP.NET Core düzeni
author: ardalis
description: Ortak düzenler kullanma, yönergeleri paylaşma ve işleme görünümleri önce ortak kodun bir ASP.NET Core uygulamada çalıştırmak öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: a99b239a0aeeb14492b1eee962dc1149f056f0eb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274124"
---
# <a name="layout-in-aspnet-core"></a><span data-ttu-id="be5d9-103">ASP.NET Core düzeni</span><span class="sxs-lookup"><span data-stu-id="be5d9-103">Layout in ASP.NET Core</span></span>

<span data-ttu-id="be5d9-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="be5d9-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="be5d9-105">Görünümleri sık visual ve programlama öğeleri paylaşır.</span><span class="sxs-lookup"><span data-stu-id="be5d9-105">Views frequently share visual and programmatic elements.</span></span> <span data-ttu-id="be5d9-106">Bu makalede, ortak düzenler kullanma, yönergeleri paylaşma ve ortak kodun işleme görünümleri önce ASP.NET uygulamanızı çalıştırın öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="be5d9-106">In this article, you'll learn how to use common layouts, share directives, and run common code before rendering views in your ASP.NET app.</span></span>

## <a name="what-is-a-layout"></a><span data-ttu-id="be5d9-107">Bir düzen nedir</span><span class="sxs-lookup"><span data-stu-id="be5d9-107">What is a Layout</span></span>

<span data-ttu-id="be5d9-108">Çoğu web uygulamaları sayfadan sayfaya gidin gibi kullanıcı ile tutarlı bir deneyim sağlayan ortak bir düzen sahip.</span><span class="sxs-lookup"><span data-stu-id="be5d9-108">Most web apps have a common layout that provides the user with a consistent experience as they navigate from page to page.</span></span> <span data-ttu-id="be5d9-109">Düzen genellikle Uygulama Başlığı, gezinti veya menü öğeleri ve altbilgi gibi ortak kullanıcı arabirimi öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="be5d9-109">The layout typically includes common user interface elements such as the app header, navigation or menu elements, and footer.</span></span>

![Sayfa düzeni örneği](layout/_static/page-layout.png)

<span data-ttu-id="be5d9-111">Komut dosyalarını ve stil sayfalarını gibi ortak HTML yapıları da sıklıkla bir uygulama içinde birçok sayfalar tarafından kullanılır.</span><span class="sxs-lookup"><span data-stu-id="be5d9-111">Common HTML structures such as scripts and stylesheets are also frequently used by many pages within an app.</span></span> <span data-ttu-id="be5d9-112">Tüm bu paylaşılan öğeleri de tanımlanabilir bir *düzeni* sonra uygulama içinde kullanılan herhangi bir görünüm tarafından başvurulabilir dosya.</span><span class="sxs-lookup"><span data-stu-id="be5d9-112">All of these shared elements may be defined in a *layout* file, which can then be referenced by any view used within the app.</span></span> <span data-ttu-id="be5d9-113">Düzenleri azaltmaya yardımcı görünümlerinde yinelenen kod izleyin [yok yineleyin kendiniz (KURU) İlkesi](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="be5d9-113">Layouts reduce duplicate code in views, helping them follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="be5d9-114">Kurala göre bir ASP.NET uygulaması için varsayılan düzeni adlı `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="be5d9-114">By convention, the default layout for an ASP.NET app is named `_Layout.cshtml`.</span></span> <span data-ttu-id="be5d9-115">Visual Studio ASP.NET Core MVC proje şablonu bu düzen dosyasını içeren `Views/Shared` klasörü:</span><span class="sxs-lookup"><span data-stu-id="be5d9-115">The Visual Studio ASP.NET Core MVC project template includes this layout file in the `Views/Shared` folder:</span></span>

![Çözüm Gezgini görünümler klasöründe](layout/_static/web-project-views.png)

<span data-ttu-id="be5d9-117">Bu düzen görünümleri için bir en üst düzey şablon uygulamada tanımlar.</span><span class="sxs-lookup"><span data-stu-id="be5d9-117">This layout defines a top level template for views in the app.</span></span> <span data-ttu-id="be5d9-118">Uygulamaları bir düzen gerekmez ve uygulamaları farklı düzenler belirtme farklı görünümleri ile birden fazla yerleşim tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="be5d9-118">Apps don't require a layout, and apps can define more than one layout, with different views specifying different layouts.</span></span>

<span data-ttu-id="be5d9-119">Örnek `_Layout.cshtml`:</span><span class="sxs-lookup"><span data-stu-id="be5d9-119">An example `_Layout.cshtml`:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a><span data-ttu-id="be5d9-120">Bir düzen belirtme</span><span class="sxs-lookup"><span data-stu-id="be5d9-120">Specifying a Layout</span></span>

<span data-ttu-id="be5d9-121">Razor görünümleri olan bir `Layout` özelliği.</span><span class="sxs-lookup"><span data-stu-id="be5d9-121">Razor views have a `Layout` property.</span></span> <span data-ttu-id="be5d9-122">Tek bir görünüm, bu özelliği ayarlanarak bir düzeni belirtin:</span><span class="sxs-lookup"><span data-stu-id="be5d9-122">Individual views specify a layout by setting this property:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

<span data-ttu-id="be5d9-123">Belirtilen Düzen tam bir yol kullanabilirsiniz (örneğin: `/Views/Shared/_Layout.cshtml`) ya da kısmi bir ad (örnek: `_Layout`).</span><span class="sxs-lookup"><span data-stu-id="be5d9-123">The layout specified can use a full path (example: `/Views/Shared/_Layout.cshtml`) or a partial name (example: `_Layout`).</span></span> <span data-ttu-id="be5d9-124">Kısmi bir ad sağlandığında, Razor görüntüleme altyapısı kendi standart bulma işlemini kullanarak Düzen dosyayı arar.</span><span class="sxs-lookup"><span data-stu-id="be5d9-124">When a partial name is provided, the Razor view engine will search for the layout file using its standard discovery process.</span></span> <span data-ttu-id="be5d9-125">Denetleyici ilişkili klasörü, ilk olarak, arkasından aranır `Shared` klasör.</span><span class="sxs-lookup"><span data-stu-id="be5d9-125">The controller-associated folder is searched first, followed by the `Shared` folder.</span></span> <span data-ttu-id="be5d9-126">Bu bulma işlemi keşfetmek için kullanılan bir benzerdir [kısmi görünümler](partial.md).</span><span class="sxs-lookup"><span data-stu-id="be5d9-126">This discovery process is identical to the one used to discover [partial views](partial.md).</span></span>

<span data-ttu-id="be5d9-127">Varsayılan olarak, her düzeni çağırmalısınız `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="be5d9-127">By default, every layout must call `RenderBody`.</span></span> <span data-ttu-id="be5d9-128">Yerde çağrısı `RenderBody` olan yerleştirildiğinde, görünüm içeriğini işlenir.</span><span class="sxs-lookup"><span data-stu-id="be5d9-128">Wherever the call to `RenderBody` is placed, the contents of the view will be rendered.</span></span>

<a name="layout-sections-label"></a>

### <a name="sections"></a><span data-ttu-id="be5d9-129">Bölümler</span><span class="sxs-lookup"><span data-stu-id="be5d9-129">Sections</span></span>

<span data-ttu-id="be5d9-130">Bir düzen isteğe bağlı olarak bir veya daha fazla başvurabilir *bölümleri*, çağırarak `RenderSection`.</span><span class="sxs-lookup"><span data-stu-id="be5d9-130">A layout can optionally reference one or more *sections*, by calling `RenderSection`.</span></span> <span data-ttu-id="be5d9-131">Bölümler belirli sayfa öğelerini nereye yerleştirileceğini düzenlemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="be5d9-131">Sections provide a way to organize where certain page elements should be placed.</span></span> <span data-ttu-id="be5d9-132">Her çağrı `RenderSection` bu bölümün gerekli veya isteğe bağlı olup olmadığını belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="be5d9-132">Each call to `RenderSection` can specify whether that section is required or optional.</span></span> <span data-ttu-id="be5d9-133">Gerekli bölüm bulunamazsa, bir özel durum.</span><span class="sxs-lookup"><span data-stu-id="be5d9-133">If a required section isn't found, an exception will be thrown.</span></span> <span data-ttu-id="be5d9-134">Tek bir görünüm içinde bölüm kullanılarak oluşturulması için içeriği belirtin `@section` Razor sözdizimi.</span><span class="sxs-lookup"><span data-stu-id="be5d9-134">Individual views specify the content to be rendered within a section using the `@section` Razor syntax.</span></span> <span data-ttu-id="be5d9-135">Bir görünüm bir bölüm tanımlıyorsa oluşturulması gerekir (veya bir hata meydana gelir).</span><span class="sxs-lookup"><span data-stu-id="be5d9-135">If a view defines a section, it must be rendered (or an error will occur).</span></span>

<span data-ttu-id="be5d9-136">Örnek `@section` bir görünüm tanımında:</span><span class="sxs-lookup"><span data-stu-id="be5d9-136">An example `@section` definition in a view:</span></span>

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

<span data-ttu-id="be5d9-137">Doğrulama betikleri eklenir Yukarıdaki kod `scripts` bir form içeren bir görünümde bölümü.</span><span class="sxs-lookup"><span data-stu-id="be5d9-137">In the code above, validation scripts are added to the `scripts` section on a view that includes a form.</span></span> <span data-ttu-id="be5d9-138">Aynı uygulama diğer görünümlere ek betik gerektirmeyebilecek ve bu nedenle komut dosyaları bölüm tanımlamak gerekiyor olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="be5d9-138">Other views in the same application might not require any additional scripts, and so wouldn't need to define a scripts section.</span></span>

<span data-ttu-id="be5d9-139">Bir görünümde tanımlandığı bölümleri, kendi hemen düzen sayfası yalnızca kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="be5d9-139">Sections defined in a view are available only in its immediate layout page.</span></span> <span data-ttu-id="be5d9-140">Bunlar, kısmi, görünüm bileşenleri ya da Görünüm sisteminin diğer bölümleriyle başvurulamaz.</span><span class="sxs-lookup"><span data-stu-id="be5d9-140">They cannot be referenced from partials, view components, or other parts of the view system.</span></span>

### <a name="ignoring-sections"></a><span data-ttu-id="be5d9-141">Bölümler yoksayılıyor</span><span class="sxs-lookup"><span data-stu-id="be5d9-141">Ignoring sections</span></span>

<span data-ttu-id="be5d9-142">Varsayılan olarak, gövde ve içerik sayfasındaki tüm bölümleri tüm düzen sayfası tarafından oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="be5d9-142">By default, the body and all sections in a content page must all be rendered by the layout page.</span></span> <span data-ttu-id="be5d9-143">Razor görüntüleme altyapısı bu gövde ve her bölüm çizilir olup olmadığını izleyerek zorlar.</span><span class="sxs-lookup"><span data-stu-id="be5d9-143">The Razor view engine enforces this by tracking whether the body and each section have been rendered.</span></span>

<span data-ttu-id="be5d9-144">Gövde veya bölüm yok saymak için Görünüm altyapısı istemek üzere çağrı `IgnoreBody` ve `IgnoreSection` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="be5d9-144">To instruct the view engine to ignore the body or sections, call the `IgnoreBody` and `IgnoreSection` methods.</span></span>

<span data-ttu-id="be5d9-145">Gövde ve her bir Razor sayfasını bölümünde çizilir göz ardı ya da gerekir.</span><span class="sxs-lookup"><span data-stu-id="be5d9-145">The body and every section in a Razor page must be either rendered or ignored.</span></span>

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a><span data-ttu-id="be5d9-146">Paylaşılan yönergeleri alma</span><span class="sxs-lookup"><span data-stu-id="be5d9-146">Importing Shared Directives</span></span>

<span data-ttu-id="be5d9-147">Ad alanlarını alma veya gerçekleştirme gibi pek çok şeyi yapmak için Razor yönergesi görünümleri kullanabilir [bağımlılık ekleme](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="be5d9-147">Views can use Razor directives to do many things, such as importing namespaces or performing [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="be5d9-148">Yönergeleri birçok görünümler tarafından paylaşılan ortak bir belirtilen `_ViewImports.cshtml` dosya.</span><span class="sxs-lookup"><span data-stu-id="be5d9-148">Directives shared by many views may be specified in a common `_ViewImports.cshtml` file.</span></span> <span data-ttu-id="be5d9-149">`_ViewImports` Dosyasını aşağıdaki yönergeleri destekler:</span><span class="sxs-lookup"><span data-stu-id="be5d9-149">The `_ViewImports` file supports the following directives:</span></span>

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

<span data-ttu-id="be5d9-150">Dosya işlevleri ve bölüm tanımlarını gibi diğer Razor özellikleri desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="be5d9-150">The file doesn't support other Razor features, such as functions and section definitions.</span></span>

<span data-ttu-id="be5d9-151">Bir örnek `_ViewImports.cshtml` dosyası:</span><span class="sxs-lookup"><span data-stu-id="be5d9-151">A sample `_ViewImports.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

<span data-ttu-id="be5d9-152">`_ViewImports.cshtml` ASP.NET Core MVC uygulama genellikle yerleştirilir için dosya `Views` klasör.</span><span class="sxs-lookup"><span data-stu-id="be5d9-152">The `_ViewImports.cshtml` file for an ASP.NET Core MVC app is typically placed in the `Views` folder.</span></span> <span data-ttu-id="be5d9-153">A `_ViewImports.cshtml` , onu yalnızca uygulanacak durum görünümleri bu klasörde ve alt klasörlerinde içinde herhangi bir klasör içinde dosya yerleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="be5d9-153">A `_ViewImports.cshtml` file can be placed within any folder, in which case it will only be applied to views within that folder and its subfolders.</span></span> <span data-ttu-id="be5d9-154">`_ViewImports` dosyaları kök düzeyinde başlangıç işlenir, ve ardından kadar önde gelen her klasör için konumun görünümünün kendisinde ayarları kök düzeyinde belirtilen şekilde geçersiz klasör düzeyinde.</span><span class="sxs-lookup"><span data-stu-id="be5d9-154">`_ViewImports` files are processed starting at the root level, and then for each folder leading up to the location of the view itself, so settings specified at the root level may be overridden at the folder level.</span></span>

<span data-ttu-id="be5d9-155">Örneğin, bir kök düzeyinde varsa `_ViewImports.cshtml` dosyayı belirtir `@model` ve `@addTagHelper`ve başka bir `_ViewImports.cshtml` görünüm denetleyicisini ilişkili klasöründeki dosyasını farklı bir belirtir `@model` ve başka ekler `@addTagHelper`, görünümü Her iki etiket Yardımcıları erişebilir ve ikincisi kullanacağı `@model`.</span><span class="sxs-lookup"><span data-stu-id="be5d9-155">For example, if a root level `_ViewImports.cshtml` file specifies `@model` and `@addTagHelper`, and another `_ViewImports.cshtml` file in the controller-associated folder of the view specifies a different `@model` and adds another `@addTagHelper`, the view will have access to both tag helpers and will use the latter `@model`.</span></span>

<span data-ttu-id="be5d9-156">Birden çok `_ViewImports.cshtml` dosyaları için bir görünüm çalıştırmak, bulunan yönergeleri davranışını birleştirilmiş `ViewImports.cshtml` dosyaları şu şekilde olacaktır:</span><span class="sxs-lookup"><span data-stu-id="be5d9-156">If multiple `_ViewImports.cshtml` files are run for a view, combined behavior of the directives included in the `ViewImports.cshtml` files will be as follows:</span></span>

* <span data-ttu-id="be5d9-157">`@addTagHelper`, `@removeTagHelper`: sırayla tüm çalışma</span><span class="sxs-lookup"><span data-stu-id="be5d9-157">`@addTagHelper`, `@removeTagHelper`: all run, in order</span></span>

* <span data-ttu-id="be5d9-158">`@tagHelperPrefix`: en yakın bir görünüme herhangi diğer geçersiz kılar</span><span class="sxs-lookup"><span data-stu-id="be5d9-158">`@tagHelperPrefix`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="be5d9-159">`@model`: en yakın bir görünüme herhangi diğer geçersiz kılar</span><span class="sxs-lookup"><span data-stu-id="be5d9-159">`@model`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="be5d9-160">`@inherits`: en yakın bir görünüme herhangi diğer geçersiz kılar</span><span class="sxs-lookup"><span data-stu-id="be5d9-160">`@inherits`: the closest one to the view overrides any others</span></span>

* <span data-ttu-id="be5d9-161">`@using`: tüm dahil; Yinelenen değer yok sayılır</span><span class="sxs-lookup"><span data-stu-id="be5d9-161">`@using`: all are included; duplicates are ignored</span></span>

* <span data-ttu-id="be5d9-162">`@inject`: her bir özellik için en yakın bir görünüm için aynı adla başkalarıyla geçersiz kılar</span><span class="sxs-lookup"><span data-stu-id="be5d9-162">`@inject`: for each property, the closest one to the view overrides any others with the same property name</span></span>

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a><span data-ttu-id="be5d9-163">Her görünüm önce kodu çalıştırma</span><span class="sxs-lookup"><span data-stu-id="be5d9-163">Running Code Before Each View</span></span>

<span data-ttu-id="be5d9-164">Sahip code her görünüm önce çalıştırmanız gerekir, bu yerleştirilmelidir `_ViewStart.cshtml` dosya.</span><span class="sxs-lookup"><span data-stu-id="be5d9-164">If you have code you need to run before every view, this should be placed in the `_ViewStart.cshtml` file.</span></span> <span data-ttu-id="be5d9-165">Kural tarafından `_ViewStart.cshtml` dosyasının bulunduğu `Views` klasör.</span><span class="sxs-lookup"><span data-stu-id="be5d9-165">By convention, the `_ViewStart.cshtml` file is located in the `Views` folder.</span></span> <span data-ttu-id="be5d9-166">Listelenen deyimleri `_ViewStart.cshtml` önce her tam görünüm (değil düzenleri ve değil kısmi görünümler) çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="be5d9-166">The statements listed in `_ViewStart.cshtml` are run before every full view (not layouts, and not partial views).</span></span> <span data-ttu-id="be5d9-167">Gibi [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` hiyerarşik olduğu.</span><span class="sxs-lookup"><span data-stu-id="be5d9-167">Like [ViewImports.cshtml](xref:mvc/views/layout#viewimports), `_ViewStart.cshtml` is hierarchical.</span></span> <span data-ttu-id="be5d9-168">Varsa bir `_ViewStart.cshtml` dosya denetleyici ilişkili görünüm klasöründe tanımlanmış, kök dizininde tanımlanan bir sonra çalıştırılacak `Views` klasörü (varsa).</span><span class="sxs-lookup"><span data-stu-id="be5d9-168">If a `_ViewStart.cshtml` file is defined in the controller-associated view folder, it will be run after the one defined in the root of the `Views` folder (if any).</span></span>

<span data-ttu-id="be5d9-169">Bir örnek `_ViewStart.cshtml` dosyası:</span><span class="sxs-lookup"><span data-stu-id="be5d9-169">A sample `_ViewStart.cshtml` file:</span></span>

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

<span data-ttu-id="be5d9-170">Yukarıdaki dosyanın tüm görünümleri kullanacağını belirtir `_Layout.cshtml` düzeni.</span><span class="sxs-lookup"><span data-stu-id="be5d9-170">The file above specifies that all views will use the `_Layout.cshtml` layout.</span></span>

> [!NOTE]
> <span data-ttu-id="be5d9-171">Ne `_ViewStart.cshtml` ya da `_ViewImports.cshtml` genelde yerleştirilir `/Views/Shared` klasör.</span><span class="sxs-lookup"><span data-stu-id="be5d9-171">Neither `_ViewStart.cshtml` nor `_ViewImports.cshtml` are typically placed in the `/Views/Shared` folder.</span></span> <span data-ttu-id="be5d9-172">Bu dosyaların uygulama düzeyinde sürümleri doğrudan yerleştirilmelidir `/Views` klasör.</span><span class="sxs-lookup"><span data-stu-id="be5d9-172">The app-level versions of these files should be placed directly in the `/Views` folder.</span></span>
