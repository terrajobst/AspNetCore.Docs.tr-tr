---
title: ASP.NET Core bölgeler
author: rick-anderson
description: Alanların ilgili işlevleri bir grup içinde ayrı bir ad alanı (yönlendirme için) ve klasör yapısı (görünümler için) olarak düzenlemek için kullanılan bir ASP.NET MVC özelliği olduğunu öğrenin.
ms.author: riande
ms.date: 08/16/2019
uid: mvc/controllers/areas
ms.openlocfilehash: d0af3092776ee09469c879fffd3047c50b1a59b4
ms.sourcegitcommit: 4cb0c7e74355f2e87c60e2a196f842b937247a99
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69545797"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="99fc0-103">ASP.NET Core bölgeler</span><span class="sxs-lookup"><span data-stu-id="99fc0-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="99fc0-104">[Dhananjay Rohan](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="99fc0-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="99fc0-105">Bölgeler, ilgili işlevselliği ayrı bir ad alanı (yönlendirme için) ve klasör yapısı (görünümler için) olarak bir grup içinde düzenlemek için kullanılan bir ASP.NET özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="99fc0-106">Alanların kullanılması, başka `area`bir yol parametresi, `controller` , `action` veya ya da Razor sayfası `page`ekleyerek yönlendirmenin amacı için bir hiyerarşi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="99fc0-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="99fc0-107">Alanları, her biri kendi Razor Pages, denetleyiciler, görünümler ve modeller kümesine sahip bir ASP.NET Core Web uygulamasını daha küçük işlevsel gruplar halinde bölümlemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="99fc0-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="99fc0-108">Bir alan, bir uygulamanın içindeki yapısı etkin bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="99fc0-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="99fc0-109">Bir ASP.NET Core Web projesinde, sayfalar, model, denetleyici ve görünüm gibi mantıksal bileşenler farklı klasörlerde tutulur.</span><span class="sxs-lookup"><span data-stu-id="99fc0-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="99fc0-110">ASP.NET Core çalışma zamanı, bu bileşenler arasındaki ilişkiyi oluşturmak için adlandırma kurallarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="99fc0-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="99fc0-111">Büyük bir uygulama için, uygulamayı işlevlerin ayrı üst düzey alanlarında bölümlemek avantajlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="99fc0-112">Örneğin, kullanıma alma, faturalandırma ve arama gibi birden çok iş birimi içeren bir e-ticaret uygulaması.</span><span class="sxs-lookup"><span data-stu-id="99fc0-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="99fc0-113">Bu birimlerin her birinin görünümleri, denetleyicileri, Razor Pages ve modelleri içermesi için kendi alanı vardır.</span><span class="sxs-lookup"><span data-stu-id="99fc0-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="99fc0-114">Şu durumlarda bir projedeki alanı kullanmayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="99fc0-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="99fc0-115">Uygulama, mantıksal olarak ayrılabilen birden çok üst düzey işlevsel bileşenden oluşur.</span><span class="sxs-lookup"><span data-stu-id="99fc0-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="99fc0-116">Her işlevsel alanın bağımsız olarak çalışabilmesi için uygulamayı bölümlemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="99fc0-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="99fc0-117">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([indirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="99fc0-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="99fc0-118">İndirme örneği, test bölgeleri için temel bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="99fc0-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="99fc0-119">Razor Pages kullanıyorsanız, bu belgede [Razor Pages bulunan alanlara](#areas-with-razor-pages) bakın.</span><span class="sxs-lookup"><span data-stu-id="99fc0-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="99fc0-120">Görünümlere sahip denetleyiciler için bölgeler</span><span class="sxs-lookup"><span data-stu-id="99fc0-120">Areas for controllers with views</span></span>

<span data-ttu-id="99fc0-121">Alanları, denetleyicileri ve görünümleri kullanan tipik bir ASP.NET Core Web uygulaması şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="99fc0-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="99fc0-122">Bir [alan klasörü yapısı](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="99fc0-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="99fc0-123">Denetleyiciyi alanla ilişkilendirmek için [ &lbrack;alan&rbrack; ](#attribute) özniteliğiyle donatılmış denetleyiciler:</span><span class="sxs-lookup"><span data-stu-id="99fc0-123">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="99fc0-124">[Başlangıç alanına eklenen alan yolu](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="99fc0-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="99fc0-125">Alan klasörü yapısı</span><span class="sxs-lookup"><span data-stu-id="99fc0-125">Area folder structure</span></span>

<span data-ttu-id="99fc0-126">İki mantıksal grup, *ürün* ve *hizmet*içeren bir uygulamayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="99fc0-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="99fc0-127">Alan kullanarak, klasör yapısı aşağıdakine benzer olacaktır:</span><span class="sxs-lookup"><span data-stu-id="99fc0-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="99fc0-128">Proje adı</span><span class="sxs-lookup"><span data-stu-id="99fc0-128">Project name</span></span>
  * <span data-ttu-id="99fc0-129">Alanlar</span><span class="sxs-lookup"><span data-stu-id="99fc0-129">Areas</span></span>
    * <span data-ttu-id="99fc0-130">Ürünler</span><span class="sxs-lookup"><span data-stu-id="99fc0-130">Products</span></span>
      * <span data-ttu-id="99fc0-131">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="99fc0-131">Controllers</span></span>
        * <span data-ttu-id="99fc0-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="99fc0-132">HomeController.cs</span></span>
        * <span data-ttu-id="99fc0-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="99fc0-133">ManageController.cs</span></span>
      * <span data-ttu-id="99fc0-134">Görünümler</span><span class="sxs-lookup"><span data-stu-id="99fc0-134">Views</span></span>
        * <span data-ttu-id="99fc0-135">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="99fc0-135">Home</span></span>
          * <span data-ttu-id="99fc0-136">Index. cshtml</span><span class="sxs-lookup"><span data-stu-id="99fc0-136">Index.cshtml</span></span>
        * <span data-ttu-id="99fc0-137">Yönetebilmeniz</span><span class="sxs-lookup"><span data-stu-id="99fc0-137">Manage</span></span>
          * <span data-ttu-id="99fc0-138">Index. cshtml</span><span class="sxs-lookup"><span data-stu-id="99fc0-138">Index.cshtml</span></span>
          * <span data-ttu-id="99fc0-139">. Cshtml hakkında</span><span class="sxs-lookup"><span data-stu-id="99fc0-139">About.cshtml</span></span>
    * <span data-ttu-id="99fc0-140">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="99fc0-140">Services</span></span>
      * <span data-ttu-id="99fc0-141">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="99fc0-141">Controllers</span></span>
        * <span data-ttu-id="99fc0-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="99fc0-142">HomeController.cs</span></span>
      * <span data-ttu-id="99fc0-143">Görünümler</span><span class="sxs-lookup"><span data-stu-id="99fc0-143">Views</span></span>
        * <span data-ttu-id="99fc0-144">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="99fc0-144">Home</span></span>
          * <span data-ttu-id="99fc0-145">Index. cshtml</span><span class="sxs-lookup"><span data-stu-id="99fc0-145">Index.cshtml</span></span>

<span data-ttu-id="99fc0-146">Yukarıdaki düzen, alan kullanılırken tipik olsa da, bu klasör yapısını kullanmak için yalnızca görünüm dosyaları gereklidir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="99fc0-147">Eşleşen bir alan görünümü dosyası için bulma aramalarını aşağıdaki sırayla görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="99fc0-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="99fc0-148">*Denetleyiciler* ve *modeller* gibi görünüm olmayan klasörlerin konumu bunun önemi **yoktur** .</span><span class="sxs-lookup"><span data-stu-id="99fc0-148">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="99fc0-149">Örneğin, *denetleyiciler* ve *modeller* klasörü gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-149">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="99fc0-150">*Denetleyicilerin* ve *modellerin* içeriği bir. dll ' ye derlenen koddur.</span><span class="sxs-lookup"><span data-stu-id="99fc0-150">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="99fc0-151">*Görünümlerin* içeriği, bu görünüme bir istek getirilene kadar derlenmez.</span><span class="sxs-lookup"><span data-stu-id="99fc0-151">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="99fc0-152">Denetleyiciyi bir alanla ilişkilendir</span><span class="sxs-lookup"><span data-stu-id="99fc0-152">Associate the controller with an Area</span></span>

<span data-ttu-id="99fc0-153">Alan denetleyicileri, [ &lbrack;alan&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) özniteliğiyle belirlenir:</span><span class="sxs-lookup"><span data-stu-id="99fc0-153">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="99fc0-154">Alan yolu Ekle</span><span class="sxs-lookup"><span data-stu-id="99fc0-154">Add Area route</span></span>

<span data-ttu-id="99fc0-155">Alan rotaları genellikle öznitelik yönlendirme yerine geleneksel yönlendirmeyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="99fc0-155">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="99fc0-156">Geleneksel yönlendirme sıra bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="99fc0-156">Conventional routing is order-dependent.</span></span> <span data-ttu-id="99fc0-157">Genel olarak, alanlar içeren rotalar, alan olmayan rotalardan daha belirgin olduklarından daha önce rota tablosuna yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-157">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="99fc0-158">`{area:...}`, URL alanı tüm alanlarda Tekdüzen ise yol şablonlarında bir belirteç olarak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="99fc0-158">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="99fc0-159">Önceki kodda, `exists` yolun bir alanla eşleşmesi gereken bir kısıtlama uygular.</span><span class="sxs-lookup"><span data-stu-id="99fc0-159">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="99fc0-160">Kullanmak `{area:...}` , alanlara yönlendirme eklemek için en az karmaşık mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="99fc0-160">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="99fc0-161">Aşağıdaki kod, iki <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> adlandırılmış alan yolları oluşturmak için kullanır:</span><span class="sxs-lookup"><span data-stu-id="99fc0-161">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="99fc0-162">ASP.NET Core 2,2 `MapAreaRoute` ile kullanırken, [Bu GitHub sorununa](https://github.com/aspnet/AspNetCore/issues/7772)bakın.</span><span class="sxs-lookup"><span data-stu-id="99fc0-162">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="99fc0-163">Daha fazla bilgi için bkz. [alan yönlendirme](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="99fc0-163">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="99fc0-164">MVC alanlarıyla bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="99fc0-164">Link generation with MVC areas</span></span>

<span data-ttu-id="99fc0-165">[Örnek indirmenin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) aşağıdaki kodu, belirtilen alanla birlikte bağlantı oluşturmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="99fc0-165">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="99fc0-166">Yukarıdaki kodla oluşturulan bağlantılar, uygulamanın herhangi bir yerinde geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-166">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="99fc0-167">Örnek indirme, önceki bağlantıları içeren [kısmi bir görünüm](xref:mvc/views/partial) ve alanı belirtmeksizin aynı bağlantıları içerir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-167">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="99fc0-168">Kısmi görünüme, [Düzen dosyasında](xref:mvc/views/layout)başvurulur, bu nedenle uygulamadaki her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="99fc0-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="99fc0-169">Alanı belirtmeden oluşturulan bağlantılar yalnızca aynı alan ve denetleyicideki bir sayfadan başvuruluyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="99fc0-170">Alan veya denetleyici belirtilmediğinde, yönlendirme *ortam* değerlerine göre değişir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-170">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="99fc0-171">Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="99fc0-172">Örnek uygulama için birçok durumda, ortam değerlerini kullanmak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="99fc0-172">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="99fc0-173">Daha fazla bilgi için bkz. [Denetleyici eylemlerine yönlendirme](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="99fc0-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="99fc0-174">_ViewStart. cshtml dosyasını kullanan alanların paylaşılan düzeni</span><span class="sxs-lookup"><span data-stu-id="99fc0-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="99fc0-175">Uygulamanın tamamında ortak bir düzen paylaşmak için *_Viewstart. cshtml* öğesini uygulama kök klasörüne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="99fc0-175">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="99fc0-176">_Viewwimports. cshtml</span><span class="sxs-lookup"><span data-stu-id="99fc0-176">_ViewImports.cshtml</span></span>

<span data-ttu-id="99fc0-177">Standart konumunda */views/_viewwimports.cshtml* , alanlara uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="99fc0-177">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="99fc0-178">Ortak [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), `@using`veya `@inject` bölgenizde kullanmak için, [alan Görünümleriniz için](xref:mvc/views/layout#importing-shared-directives)uygun bir *_viewwimports. cshtml* dosyasının geçerli olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="99fc0-178">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="99fc0-179">Tüm görünümlerinizin aynı davranışını istiyorsanız, */views/_viewwimports.cshtml* öğesini uygulama köküne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="99fc0-179">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="99fc0-180">Görünümlerin depolandığı varsayılan alan klasörünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="99fc0-180">Change default area folder where views are stored</span></span>

<span data-ttu-id="99fc0-181">Aşağıdaki kod varsayılan alan klasörünü ' den `"Areas"` `"MyAreas"`' a değiştirir:</span><span class="sxs-lookup"><span data-stu-id="99fc0-181">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="99fc0-182">Razor Pages alan bölgeler</span><span class="sxs-lookup"><span data-stu-id="99fc0-182">Areas with Razor Pages</span></span>

<span data-ttu-id="99fc0-183">Razor Pages olan alanlarda, uygulamanın kökünde bir *Areas/<area name>/Pages* klasörü gerekir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-183">Areas with Razor Pages require an *Areas/<area name>/Pages* folder in the root of the app.</span></span> <span data-ttu-id="99fc0-184">Aşağıdaki klasör yapısı [örnek uygulamayla](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)birlikte kullanılır:</span><span class="sxs-lookup"><span data-stu-id="99fc0-184">The following folder structure is used with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="99fc0-185">Proje adı</span><span class="sxs-lookup"><span data-stu-id="99fc0-185">Project name</span></span>
  * <span data-ttu-id="99fc0-186">Alanlar</span><span class="sxs-lookup"><span data-stu-id="99fc0-186">Areas</span></span>
    * <span data-ttu-id="99fc0-187">Ürünler</span><span class="sxs-lookup"><span data-stu-id="99fc0-187">Products</span></span>
      * <span data-ttu-id="99fc0-188">Sayfalar</span><span class="sxs-lookup"><span data-stu-id="99fc0-188">Pages</span></span>
        * <span data-ttu-id="99fc0-189">_Viewıtems 'Lar</span><span class="sxs-lookup"><span data-stu-id="99fc0-189">_ViewImports</span></span>
        * <span data-ttu-id="99fc0-190">Hakkında</span><span class="sxs-lookup"><span data-stu-id="99fc0-190">About</span></span>
        * <span data-ttu-id="99fc0-191">Dizin</span><span class="sxs-lookup"><span data-stu-id="99fc0-191">Index</span></span>
    * <span data-ttu-id="99fc0-192">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="99fc0-192">Services</span></span>
      * <span data-ttu-id="99fc0-193">Sayfalar</span><span class="sxs-lookup"><span data-stu-id="99fc0-193">Pages</span></span>
        * <span data-ttu-id="99fc0-194">Yönetebilmeniz</span><span class="sxs-lookup"><span data-stu-id="99fc0-194">Manage</span></span>
          * <span data-ttu-id="99fc0-195">Hakkında</span><span class="sxs-lookup"><span data-stu-id="99fc0-195">About</span></span>
          * <span data-ttu-id="99fc0-196">Dizin</span><span class="sxs-lookup"><span data-stu-id="99fc0-196">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="99fc0-197">Razor Pages ve alanlarıyla bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="99fc0-197">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="99fc0-198">[Örnek indirmenin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) aşağıdaki kodu, belirtilen alanla birlikte bağlantı oluşturmayı gösterir (örneğin, `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="99fc0-198">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="99fc0-199">Yukarıdaki kodla oluşturulan bağlantılar, uygulamanın herhangi bir yerinde geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-199">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="99fc0-200">Örnek indirme, önceki bağlantıları içeren [kısmi bir görünüm](xref:mvc/views/partial) ve alanı belirtmeksizin aynı bağlantıları içerir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-200">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="99fc0-201">Kısmi görünüme, [Düzen dosyasında](xref:mvc/views/layout)başvurulur, bu nedenle uygulamadaki her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="99fc0-201">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="99fc0-202">Alanı belirtmeden oluşturulan bağlantılar yalnızca aynı alandaki bir sayfadan başvuruluyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-202">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="99fc0-203">Alan belirtilmediğinde, yönlendirme *ortam* değerlerine göre değişir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-203">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="99fc0-204">Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="99fc0-204">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="99fc0-205">Örnek uygulama için birçok durumda, ortam değerlerini kullanmak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="99fc0-205">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="99fc0-206">Örneğin, aşağıdaki koddan oluşturulan bağlantıları göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="99fc0-206">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="99fc0-207">Önceki kod için:</span><span class="sxs-lookup"><span data-stu-id="99fc0-207">For the preceding code:</span></span>

* <span data-ttu-id="99fc0-208">Öğesinden `<a asp-page="/Manage/About">` oluşturulan bağlantı, yalnızca son istek alanındaki bir `Services` sayfa için olduğunda doğrudur.</span><span class="sxs-lookup"><span data-stu-id="99fc0-208">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="99fc0-209">Örneğin `/Services/Manage/` `/Services/Manage/About`, ,veya.`/Services/Manage/Index`</span><span class="sxs-lookup"><span data-stu-id="99fc0-209">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="99fc0-210">Öğesinden `<a asp-page="/About">` oluşturulan bağlantı yalnızca son istek içindeki `/Home`bir sayfa için olduğunda doğrudur.</span><span class="sxs-lookup"><span data-stu-id="99fc0-210">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="99fc0-211">Kod, [örnek indirden](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="99fc0-211">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="99fc0-212">_Viewwimports dosyası ile ad alanı ve etiket yardımcıları içeri aktar</span><span class="sxs-lookup"><span data-stu-id="99fc0-212">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="99fc0-213">Bir *_Viewwimports. cshtml* dosyası her bir alan *sayfaları* klasörüne eklenerek, her bir Razor sayfasına ad alanı ve etiket yardımcıları içeri aktarabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="99fc0-213">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="99fc0-214">Örnek kodun bir *_Viewwimports. cshtml* dosyası içermeyen *Hizmetler* alanını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="99fc0-214">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="99fc0-215">Aşağıdaki biçimlendirme */Services/Manage/about* Razor sayfasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="99fc0-215">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="99fc0-216">Önceki biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="99fc0-216">In the preceding markup:</span></span>

* <span data-ttu-id="99fc0-217">Modeli belirtmek için tam etki alanı adının kullanılması gerekir (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="99fc0-217">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="99fc0-218">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) tarafından etkinleştirilir`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="99fc0-218">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="99fc0-219">Örnek indirme sırasında, ürünler alanı aşağıdaki *_Viewimports. cshtml* dosyasını içerir:</span><span class="sxs-lookup"><span data-stu-id="99fc0-219">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="99fc0-220">Aşağıdaki biçimlendirme */Products/about* Razor sayfasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="99fc0-220">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="99fc0-221">Önceki dosyada, ad alanı ve `@addTagHelper` yönerge, *alan/ürünler/sayfalar/_viewwimports. cshtml* dosyası tarafından dosyasına aktarılır.</span><span class="sxs-lookup"><span data-stu-id="99fc0-221">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="99fc0-222">Daha fazla bilgi için bkz. [etiket Yardımcısı kapsamını yönetme](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) ve [paylaşılan yönergeleri içeri aktarma](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="99fc0-222">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="99fc0-223">Razor Pages alanlarıyla ilgili paylaşılan düzen</span><span class="sxs-lookup"><span data-stu-id="99fc0-223">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="99fc0-224">Uygulamanın tamamında ortak bir düzen paylaşmak için *_Viewstart. cshtml* öğesini uygulama kök klasörüne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="99fc0-224">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="99fc0-225">Yayımlama alanı</span><span class="sxs-lookup"><span data-stu-id="99fc0-225">Publishing Areas</span></span>

<span data-ttu-id="99fc0-226">*Wwwroot* dizinindeki tüm \*. cshtml dosyaları ve dosyaları, \*. csproj dosyasına dahil edildiğinde `<Project Sdk="Microsoft.NET.Sdk.Web">` çıkışa yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="99fc0-226">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
