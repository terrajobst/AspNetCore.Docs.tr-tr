---
title: ASP.NET Core bölgeler
author: rick-anderson
description: Alanların ilgili işlevleri bir grup içinde ayrı bir ad alanı (yönlendirme için) ve klasör yapısı (görünümler için) olarak düzenlemek için kullanılan bir ASP.NET MVC özelliği olduğunu öğrenin.
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 1066f4ce104e507abe63302fd3523a3a7a8dfde9
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828249"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="05315-103">ASP.NET Core bölgeler</span><span class="sxs-lookup"><span data-stu-id="05315-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="05315-104">[Dhananjay Rohan](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="05315-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="05315-105">Bölgeler, ilgili işlevselliği ayrı bir ad alanı (yönlendirme için) ve klasör yapısı (görünümler için) olarak bir grup içinde düzenlemek için kullanılan bir ASP.NET özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="05315-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="05315-106">Alanların kullanılması `area`, `controller` ve `action` ya da Razor sayfası `page`başka bir rota parametresi ekleyerek yönlendirme amacına yönelik bir hiyerarşi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="05315-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="05315-107">Alanları, her biri kendi Razor Pages, denetleyiciler, görünümler ve modeller kümesine sahip bir ASP.NET Core Web uygulamasını daha küçük işlevsel gruplar halinde bölümlemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="05315-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="05315-108">Bir alan, bir uygulamanın içindeki yapısı etkin bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="05315-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="05315-109">Bir ASP.NET Core Web projesinde, sayfalar, model, denetleyici ve görünüm gibi mantıksal bileşenler farklı klasörlerde tutulur.</span><span class="sxs-lookup"><span data-stu-id="05315-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="05315-110">ASP.NET Core çalışma zamanı, bu bileşenler arasındaki ilişkiyi oluşturmak için adlandırma kurallarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="05315-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="05315-111">Büyük bir uygulama için, uygulamayı işlevlerin ayrı üst düzey alanlarında bölümlemek avantajlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="05315-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="05315-112">Örneğin, kullanıma alma, faturalandırma ve arama gibi birden çok iş birimi içeren bir e-ticaret uygulaması.</span><span class="sxs-lookup"><span data-stu-id="05315-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="05315-113">Bu birimlerin her birinin görünümleri, denetleyicileri, Razor Pages ve modelleri içermesi için kendi alanı vardır.</span><span class="sxs-lookup"><span data-stu-id="05315-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="05315-114">Şu durumlarda bir projedeki alanı kullanmayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="05315-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="05315-115">Uygulama, mantıksal olarak ayrılabilen birden çok üst düzey işlevsel bileşenden oluşur.</span><span class="sxs-lookup"><span data-stu-id="05315-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="05315-116">Her işlevsel alanın bağımsız olarak çalışabilmesi için uygulamayı bölümlemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="05315-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="05315-117">[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="05315-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="05315-118">İndirme örneği, test bölgeleri için temel bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="05315-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="05315-119">Razor Pages kullanıyorsanız, bu belgede [Razor Pages bulunan alanlara](#areas-with-razor-pages) bakın.</span><span class="sxs-lookup"><span data-stu-id="05315-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="05315-120">Görünümlere sahip denetleyiciler için bölgeler</span><span class="sxs-lookup"><span data-stu-id="05315-120">Areas for controllers with views</span></span>

<span data-ttu-id="05315-121">Alanları, denetleyicileri ve görünümleri kullanan tipik bir ASP.NET Core Web uygulaması şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="05315-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="05315-122">Bir [alan klasörü yapısı](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="05315-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="05315-123">Denetleyiciyi alanla ilişkilendirmek için [`[Area]`](#attribute) özniteliğine sahip denetleyiciler:</span><span class="sxs-lookup"><span data-stu-id="05315-123">Controllers with the [`[Area]`](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="05315-124">[Başlangıç alanına eklenen alan yolu](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="05315-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="05315-125">Alan klasörü yapısı</span><span class="sxs-lookup"><span data-stu-id="05315-125">Area folder structure</span></span>

<span data-ttu-id="05315-126">İki mantıksal grup, *ürün* ve *hizmet*içeren bir uygulamayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="05315-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="05315-127">Alan kullanarak, klasör yapısı aşağıdakine benzer olacaktır:</span><span class="sxs-lookup"><span data-stu-id="05315-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="05315-128">Proje adı</span><span class="sxs-lookup"><span data-stu-id="05315-128">Project name</span></span>
  * <span data-ttu-id="05315-129">Alanlar</span><span class="sxs-lookup"><span data-stu-id="05315-129">Areas</span></span>
    * <span data-ttu-id="05315-130">Ürünler</span><span class="sxs-lookup"><span data-stu-id="05315-130">Products</span></span>
      * <span data-ttu-id="05315-131">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="05315-131">Controllers</span></span>
        * <span data-ttu-id="05315-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="05315-132">HomeController.cs</span></span>
        * <span data-ttu-id="05315-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="05315-133">ManageController.cs</span></span>
      * <span data-ttu-id="05315-134">Görünümler</span><span class="sxs-lookup"><span data-stu-id="05315-134">Views</span></span>
        * <span data-ttu-id="05315-135">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="05315-135">Home</span></span>
          * <span data-ttu-id="05315-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="05315-136">Index.cshtml</span></span>
        * <span data-ttu-id="05315-137">Yönet</span><span class="sxs-lookup"><span data-stu-id="05315-137">Manage</span></span>
          * <span data-ttu-id="05315-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="05315-138">Index.cshtml</span></span>
          * <span data-ttu-id="05315-139">. Cshtml hakkında</span><span class="sxs-lookup"><span data-stu-id="05315-139">About.cshtml</span></span>
    * <span data-ttu-id="05315-140">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="05315-140">Services</span></span>
      * <span data-ttu-id="05315-141">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="05315-141">Controllers</span></span>
        * <span data-ttu-id="05315-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="05315-142">HomeController.cs</span></span>
      * <span data-ttu-id="05315-143">Görünümler</span><span class="sxs-lookup"><span data-stu-id="05315-143">Views</span></span>
        * <span data-ttu-id="05315-144">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="05315-144">Home</span></span>
          * <span data-ttu-id="05315-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="05315-145">Index.cshtml</span></span>

<span data-ttu-id="05315-146">Yukarıdaki düzen, alan kullanılırken tipik olsa da, bu klasör yapısını kullanmak için yalnızca görünüm dosyaları gereklidir.</span><span class="sxs-lookup"><span data-stu-id="05315-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="05315-147">Eşleşen bir alan görünümü dosyası için bulma aramalarını aşağıdaki sırayla görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="05315-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
```

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="05315-148">Denetleyiciyi bir alanla ilişkilendir</span><span class="sxs-lookup"><span data-stu-id="05315-148">Associate the controller with an Area</span></span>

<span data-ttu-id="05315-149">Alan denetleyicileri [&lbrack;alanı&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) özniteliğiyle belirlenir:</span><span class="sxs-lookup"><span data-stu-id="05315-149">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="05315-150">Alan yolu Ekle</span><span class="sxs-lookup"><span data-stu-id="05315-150">Add Area route</span></span>

<span data-ttu-id="05315-151">Alan rotaları genellikle öznitelik yönlendirme yerine geleneksel yönlendirmeyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="05315-151">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="05315-152">Geleneksel yönlendirme sıra bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="05315-152">Conventional routing is order-dependent.</span></span> <span data-ttu-id="05315-153">Genel olarak, alanlar içeren rotalar, alan olmayan rotalardan daha belirgin olduklarından daha önce rota tablosuna yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="05315-153">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="05315-154">`{area:...}`, URL alanı tüm alanlarda Tekdüzen ise yol şablonlarında bir belirteç olarak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="05315-154">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="05315-155">Yukarıdaki kodda `exists`, yolun bir alanla eşleşmesi gereken bir kısıtlama uygular.</span><span class="sxs-lookup"><span data-stu-id="05315-155">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="05315-156">`{area:...}` kullanmak, alanlara yönlendirme eklemek için en az karmaşık mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="05315-156">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="05315-157">Aşağıdaki kod, iki adlandırılmış alan yolu oluşturmak için <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> kullanır:</span><span class="sxs-lookup"><span data-stu-id="05315-157">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="05315-158">ASP.NET Core 2,2 ile `MapAreaRoute` kullanırken, [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore/issues/7772)bakın.</span><span class="sxs-lookup"><span data-stu-id="05315-158">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="05315-159">Daha fazla bilgi için bkz. [alan yönlendirme](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="05315-159">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="05315-160">MVC alanlarıyla bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="05315-160">Link generation with MVC areas</span></span>

<span data-ttu-id="05315-161">[Örnek indirmenin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) aşağıdaki kodu, belirtilen alanla birlikte bağlantı oluşturmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="05315-161">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="05315-162">Yukarıdaki kodla oluşturulan bağlantılar, uygulamanın herhangi bir yerinde geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="05315-162">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="05315-163">Örnek indirme, önceki bağlantıları içeren [kısmi bir görünüm](xref:mvc/views/partial) ve alanı belirtmeksizin aynı bağlantıları içerir.</span><span class="sxs-lookup"><span data-stu-id="05315-163">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="05315-164">Kısmi görünüme, [Düzen dosyasında](xref:mvc/views/layout)başvurulur, bu nedenle uygulamadaki her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="05315-164">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="05315-165">Alanı belirtmeden oluşturulan bağlantılar yalnızca aynı alan ve denetleyicideki bir sayfadan başvuruluyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="05315-165">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="05315-166">Alan veya denetleyici belirtilmediğinde, yönlendirme *ortam* değerlerine göre değişir.</span><span class="sxs-lookup"><span data-stu-id="05315-166">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="05315-167">Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="05315-167">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="05315-168">Örnek uygulama için birçok durumda, ortam değerlerini kullanmak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="05315-168">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="05315-169">Daha fazla bilgi için bkz. [Denetleyici eylemlerine yönlendirme](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="05315-169">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="05315-170">_ViewStart. cshtml dosyasını kullanan alanların paylaşılan düzeni</span><span class="sxs-lookup"><span data-stu-id="05315-170">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="05315-171">Uygulamanın tamamında ortak bir düzen paylaşmak için *_ViewStart. cshtml* 'yi uygulama kök klasörüne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="05315-171">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="05315-172">_ViewImports. cshtml</span><span class="sxs-lookup"><span data-stu-id="05315-172">_ViewImports.cshtml</span></span>

<span data-ttu-id="05315-173">Standart konumunda */views/_ViewImports. cshtml* , alanlara uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="05315-173">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="05315-174">Bölgenizdeki ortak [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), `@using`veya `@inject` kullanmak için, uygun bir *_ViewImports. cshtml* dosyasının, [alan Görünümleriniz için geçerli](xref:mvc/views/layout#importing-shared-directives)olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="05315-174">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="05315-175">Tüm görünümlerinizin aynı davranışını istiyorsanız, */views/_ViewImports. cshtml* öğesini uygulama köküne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="05315-175">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="05315-176">Görünümlerin depolandığı varsayılan alan klasörünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="05315-176">Change default area folder where views are stored</span></span>

<span data-ttu-id="05315-177">Aşağıdaki kod, varsayılan alan klasörünü `"Areas"` `"MyAreas"`olarak değiştirir:</span><span class="sxs-lookup"><span data-stu-id="05315-177">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="05315-178">Razor Pages alan bölgeler</span><span class="sxs-lookup"><span data-stu-id="05315-178">Areas with Razor Pages</span></span>

<span data-ttu-id="05315-179">Razor Pages olan alanlarda, uygulamanın kökünde bir *alan/<area name>/Pages* klasörü gerekir.</span><span class="sxs-lookup"><span data-stu-id="05315-179">Areas with Razor Pages require an *Areas/<area name>/Pages* folder in the root of the app.</span></span> <span data-ttu-id="05315-180">Aşağıdaki klasör yapısı [örnek uygulamayla](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)birlikte kullanılır:</span><span class="sxs-lookup"><span data-stu-id="05315-180">The following folder structure is used with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="05315-181">Proje adı</span><span class="sxs-lookup"><span data-stu-id="05315-181">Project name</span></span>
  * <span data-ttu-id="05315-182">Alanlar</span><span class="sxs-lookup"><span data-stu-id="05315-182">Areas</span></span>
    * <span data-ttu-id="05315-183">Ürünler</span><span class="sxs-lookup"><span data-stu-id="05315-183">Products</span></span>
      * <span data-ttu-id="05315-184">Sayfalar</span><span class="sxs-lookup"><span data-stu-id="05315-184">Pages</span></span>
        * <span data-ttu-id="05315-185">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="05315-185">_ViewImports</span></span>
        * <span data-ttu-id="05315-186">hakkında</span><span class="sxs-lookup"><span data-stu-id="05315-186">About</span></span>
        * <span data-ttu-id="05315-187">Dizin</span><span class="sxs-lookup"><span data-stu-id="05315-187">Index</span></span>
    * <span data-ttu-id="05315-188">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="05315-188">Services</span></span>
      * <span data-ttu-id="05315-189">Sayfalar</span><span class="sxs-lookup"><span data-stu-id="05315-189">Pages</span></span>
        * <span data-ttu-id="05315-190">Yönet</span><span class="sxs-lookup"><span data-stu-id="05315-190">Manage</span></span>
          * <span data-ttu-id="05315-191">hakkında</span><span class="sxs-lookup"><span data-stu-id="05315-191">About</span></span>
          * <span data-ttu-id="05315-192">Dizin</span><span class="sxs-lookup"><span data-stu-id="05315-192">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="05315-193">Razor Pages ve alanlarıyla bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="05315-193">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="05315-194">[Örnek indirmenin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) aşağıdaki kodu, belirtilen alanla birlikte bağlantı oluşturmayı gösterir (örneğin, `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="05315-194">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="05315-195">Yukarıdaki kodla oluşturulan bağlantılar, uygulamanın herhangi bir yerinde geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="05315-195">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="05315-196">Örnek indirme, önceki bağlantıları içeren [kısmi bir görünüm](xref:mvc/views/partial) ve alanı belirtmeksizin aynı bağlantıları içerir.</span><span class="sxs-lookup"><span data-stu-id="05315-196">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="05315-197">Kısmi görünüme, [Düzen dosyasında](xref:mvc/views/layout)başvurulur, bu nedenle uygulamadaki her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="05315-197">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="05315-198">Alanı belirtmeden oluşturulan bağlantılar yalnızca aynı alandaki bir sayfadan başvuruluyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="05315-198">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="05315-199">Alan belirtilmediğinde, yönlendirme *ortam* değerlerine göre değişir.</span><span class="sxs-lookup"><span data-stu-id="05315-199">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="05315-200">Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="05315-200">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="05315-201">Örnek uygulama için birçok durumda, ortam değerlerini kullanmak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="05315-201">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="05315-202">Örneğin, aşağıdaki koddan oluşturulan bağlantıları göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="05315-202">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="05315-203">Önceki kod için:</span><span class="sxs-lookup"><span data-stu-id="05315-203">For the preceding code:</span></span>

* <span data-ttu-id="05315-204">`<a asp-page="/Manage/About">` oluşturulan bağlantı, yalnızca son istek `Services` alanındaki bir sayfa için olduğunda doğrudur.</span><span class="sxs-lookup"><span data-stu-id="05315-204">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="05315-205">Örneğin, `/Services/Manage/`, `/Services/Manage/Index`veya `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="05315-205">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="05315-206">`<a asp-page="/About">` oluşturulan bağlantı, yalnızca son istek `/Home`bir sayfa için olduğunda doğrudur.</span><span class="sxs-lookup"><span data-stu-id="05315-206">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="05315-207">Kod, [örnek indirden](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="05315-207">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="05315-208">Ad alanı ve etiket yardımcıları _ViewImports dosya ile içeri aktarma</span><span class="sxs-lookup"><span data-stu-id="05315-208">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="05315-209">Bir *_ViewImports. cshtml* dosyası her bir alan *sayfaları* klasörüne eklenerek, ad alanı ve etiket yardımcıları, klasördeki her bir Razor sayfasına içeri aktarabilir.</span><span class="sxs-lookup"><span data-stu-id="05315-209">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="05315-210">Örnek kodun bir *_ViewImports. cshtml* dosyası içermeyen *Hizmetler* alanını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="05315-210">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="05315-211">Aşağıdaki biçimlendirme */Services/Manage/about* Razor sayfasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="05315-211">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="05315-212">Önceki biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="05315-212">In the preceding markup:</span></span>

* <span data-ttu-id="05315-213">Modeli belirtmek için tam etki alanı adının kullanılması gerekir (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="05315-213">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="05315-214">[Etiket yardımcıları](xref:mvc/views/tag-helpers/intro) `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` tarafından etkinleştirilir</span><span class="sxs-lookup"><span data-stu-id="05315-214">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="05315-215">Örnek indirme sırasında, ürünler alanı aşağıdaki *_ViewImports. cshtml* dosyasını içerir:</span><span class="sxs-lookup"><span data-stu-id="05315-215">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="05315-216">Aşağıdaki biçimlendirme */Products/about* Razor sayfasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="05315-216">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="05315-217">Önceki dosyada, ad alanı ve `@addTagHelper` yönergesi, *alan/ürünler/Pages/_ViewImports. cshtml* dosyası tarafından dosyaya aktarılır.</span><span class="sxs-lookup"><span data-stu-id="05315-217">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="05315-218">Daha fazla bilgi için bkz. [etiket Yardımcısı kapsamını yönetme](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) ve [paylaşılan yönergeleri içeri aktarma](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="05315-218">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="05315-219">Razor Pages alanlarıyla ilgili paylaşılan düzen</span><span class="sxs-lookup"><span data-stu-id="05315-219">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="05315-220">Uygulamanın tamamında ortak bir düzen paylaşmak için *_ViewStart. cshtml* 'yi uygulama kök klasörüne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="05315-220">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="05315-221">Yayımlama alanı</span><span class="sxs-lookup"><span data-stu-id="05315-221">Publishing Areas</span></span>

<span data-ttu-id="05315-222">*Wwwroot* dizinindeki tüm \*. cshtml dosyaları ve dosyaları, `<Project Sdk="Microsoft.NET.Sdk.Web">` \*. csproj dosyasına dahil edildiğinde çıktıya yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="05315-222">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
