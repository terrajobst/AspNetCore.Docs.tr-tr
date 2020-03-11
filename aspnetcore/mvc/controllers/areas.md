---
title: ASP.NET Core bölgeler
author: rick-anderson
description: Alanların ilgili işlevleri bir grup içinde ayrı bir ad alanı (yönlendirme için) ve klasör yapısı (görünümler için) olarak düzenlemek için kullanılan bir ASP.NET MVC özelliği olduğunu öğrenin.
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/areas
ms.openlocfilehash: 41f7bdd6dbb3e33f843cb2a765dd30f98c81ce21
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665405"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="8b648-103">ASP.NET Core bölgeler</span><span class="sxs-lookup"><span data-stu-id="8b648-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="8b648-104">[Dhananjay Rohan](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8b648-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8b648-105">Bölgeler, ilgili işlevselliği ayrı olarak bir grup içinde düzenlemek için kullanılan bir ASP.NET özelliğidir:</span><span class="sxs-lookup"><span data-stu-id="8b648-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate:</span></span>

* <span data-ttu-id="8b648-106">Yönlendirme için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="8b648-106">Namespace for routing.</span></span>
* <span data-ttu-id="8b648-107">Görünümler ve Razor Pages için klasör yapısı.</span><span class="sxs-lookup"><span data-stu-id="8b648-107">Folder structure for views and Razor Pages.</span></span>

<span data-ttu-id="8b648-108">Alanların kullanılması `area`, `controller` ve `action` ya da Razor sayfası `page`başka bir rota parametresi ekleyerek yönlendirme amacına yönelik bir hiyerarşi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8b648-108">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="8b648-109">Alanları, her biri kendi Razor Pages, denetleyiciler, görünümler ve modeller kümesine sahip bir ASP.NET Core Web uygulamasını daha küçük işlevsel gruplar halinde bölümlemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="8b648-109">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="8b648-110">Bir alan, bir uygulamanın içindeki yapısı etkin bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="8b648-110">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="8b648-111">Bir ASP.NET Core Web projesinde, sayfalar, model, denetleyici ve görünüm gibi mantıksal bileşenler farklı klasörlerde tutulur.</span><span class="sxs-lookup"><span data-stu-id="8b648-111">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="8b648-112">ASP.NET Core çalışma zamanı, bu bileşenler arasındaki ilişkiyi oluşturmak için adlandırma kurallarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b648-112">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="8b648-113">Büyük bir uygulama için, uygulamayı işlevlerin ayrı üst düzey alanlarında bölümlemek avantajlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="8b648-113">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="8b648-114">Örneğin, kullanıma alma, faturalandırma ve arama gibi birden çok iş birimi içeren bir e-ticaret uygulaması.</span><span class="sxs-lookup"><span data-stu-id="8b648-114">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="8b648-115">Bu birimlerin her birinin görünümleri, denetleyicileri, Razor Pages ve modelleri içermesi için kendi alanı vardır.</span><span class="sxs-lookup"><span data-stu-id="8b648-115">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="8b648-116">Şu durumlarda bir projedeki alanı kullanmayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8b648-116">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="8b648-117">Uygulama, mantıksal olarak ayrılabilen birden çok üst düzey işlevsel bileşenden oluşur.</span><span class="sxs-lookup"><span data-stu-id="8b648-117">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="8b648-118">Her işlevsel alanın bağımsız olarak çalışabilmesi için uygulamayı bölümlemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="8b648-118">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="8b648-119">[Örnek kodu görüntüleyin veya indirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8b648-119">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="8b648-120">İndirme örneği, test bölgeleri için temel bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="8b648-120">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="8b648-121">Razor Pages kullanıyorsanız, bu belgede [Razor Pages bulunan alanlara](#areas-with-razor-pages) bakın.</span><span class="sxs-lookup"><span data-stu-id="8b648-121">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="8b648-122">Görünümlere sahip denetleyiciler için bölgeler</span><span class="sxs-lookup"><span data-stu-id="8b648-122">Areas for controllers with views</span></span>

<span data-ttu-id="8b648-123">Alanları, denetleyicileri ve görünümleri kullanan tipik bir ASP.NET Core Web uygulaması şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="8b648-123">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="8b648-124">Bir [alan klasörü yapısı](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="8b648-124">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="8b648-125">Denetleyiciyi alanla ilişkilendirmek için [`[Area]`](#attribute) özniteliğine sahip denetleyiciler:</span><span class="sxs-lookup"><span data-stu-id="8b648-125">Controllers with the [`[Area]`](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/31samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="8b648-126">[Başlangıç alanına eklenen alan yolu](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="8b648-126">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/31samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="8b648-127">Alan klasörü yapısı</span><span class="sxs-lookup"><span data-stu-id="8b648-127">Area folder structure</span></span>

<span data-ttu-id="8b648-128">İki mantıksal grup, *ürün* ve *hizmet*içeren bir uygulamayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="8b648-128">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="8b648-129">Alan kullanarak, klasör yapısı aşağıdakine benzer olacaktır:</span><span class="sxs-lookup"><span data-stu-id="8b648-129">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="8b648-130">Proje adı</span><span class="sxs-lookup"><span data-stu-id="8b648-130">Project name</span></span>
  * <span data-ttu-id="8b648-131">Alanlar</span><span class="sxs-lookup"><span data-stu-id="8b648-131">Areas</span></span>
    * <span data-ttu-id="8b648-132">Ürünler</span><span class="sxs-lookup"><span data-stu-id="8b648-132">Products</span></span>
      * <span data-ttu-id="8b648-133">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="8b648-133">Controllers</span></span>
        * <span data-ttu-id="8b648-134">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="8b648-134">HomeController.cs</span></span>
        * <span data-ttu-id="8b648-135">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="8b648-135">ManageController.cs</span></span>
      * <span data-ttu-id="8b648-136">Görünümler</span><span class="sxs-lookup"><span data-stu-id="8b648-136">Views</span></span>
        * <span data-ttu-id="8b648-137">Ev</span><span class="sxs-lookup"><span data-stu-id="8b648-137">Home</span></span>
          * <span data-ttu-id="8b648-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="8b648-138">Index.cshtml</span></span>
        * <span data-ttu-id="8b648-139">Yönetme</span><span class="sxs-lookup"><span data-stu-id="8b648-139">Manage</span></span>
          * <span data-ttu-id="8b648-140">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="8b648-140">Index.cshtml</span></span>
          * <span data-ttu-id="8b648-141">. Cshtml hakkında</span><span class="sxs-lookup"><span data-stu-id="8b648-141">About.cshtml</span></span>
    * <span data-ttu-id="8b648-142">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="8b648-142">Services</span></span>
      * <span data-ttu-id="8b648-143">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="8b648-143">Controllers</span></span>
        * <span data-ttu-id="8b648-144">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="8b648-144">HomeController.cs</span></span>
      * <span data-ttu-id="8b648-145">Görünümler</span><span class="sxs-lookup"><span data-stu-id="8b648-145">Views</span></span>
        * <span data-ttu-id="8b648-146">Ev</span><span class="sxs-lookup"><span data-stu-id="8b648-146">Home</span></span>
          * <span data-ttu-id="8b648-147">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="8b648-147">Index.cshtml</span></span>

<span data-ttu-id="8b648-148">Yukarıdaki düzen, alan kullanılırken tipik olsa da, bu klasör yapısını kullanmak için yalnızca görünüm dosyaları gereklidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-148">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="8b648-149">Eşleşen bir alan görünümü dosyası için bulma aramalarını aşağıdaki sırayla görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="8b648-149">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
```

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="8b648-150">Denetleyiciyi bir alanla ilişkilendir</span><span class="sxs-lookup"><span data-stu-id="8b648-150">Associate the controller with an Area</span></span>

<span data-ttu-id="8b648-151">Alan denetleyicileri [&lbrack;alanı&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) özniteliğiyle belirlenir:</span><span class="sxs-lookup"><span data-stu-id="8b648-151">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/31samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="8b648-152">Alan yolu Ekle</span><span class="sxs-lookup"><span data-stu-id="8b648-152">Add Area route</span></span>

<span data-ttu-id="8b648-153">Alan rotaları genellikle [öznitelik yönlendirme](xref:mvc/controllers/routing#ar)yerine [geleneksel yönlendirmeyi](xref:mvc/controllers/routing#cr) kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b648-153">Area routes typically use  [conventional routing](xref:mvc/controllers/routing#cr) rather than [attribute routing](xref:mvc/controllers/routing#ar).</span></span> <span data-ttu-id="8b648-154">Geleneksel yönlendirme sıra bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="8b648-154">Conventional routing is order-dependent.</span></span> <span data-ttu-id="8b648-155">Genel olarak, alanlar içeren rotalar, alan olmayan rotalardan daha belirgin olduklarından daha önce rota tablosuna yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-155">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="8b648-156">`{area:...}`, URL alanı tüm alanlarda Tekdüzen ise yol şablonlarında bir belirteç olarak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="8b648-156">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/31samples/MVCareas/Startup.cs?name=snippet&highlight=21-23)]

<span data-ttu-id="8b648-157">Yukarıdaki kodda `exists`, yolun bir alanla eşleşmesi gereken bir kısıtlama uygular.</span><span class="sxs-lookup"><span data-stu-id="8b648-157">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="8b648-158">`MapControllerRoute`ile `{area:...}` kullanma:</span><span class="sxs-lookup"><span data-stu-id="8b648-158">Using `{area:...}` with `MapControllerRoute`:</span></span>

* <span data-ttu-id="8b648-159">Alanlara yönlendirme eklemek için en az karmaşık mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="8b648-159">Is the least complicated mechanism to adding routing to areas.</span></span>
* <span data-ttu-id="8b648-160">`[Area("Area name")]` özniteliğiyle tüm denetleyicilerle eşleşir.</span><span class="sxs-lookup"><span data-stu-id="8b648-160">Matches all controllers with the `[Area("Area name")]` attribute.</span></span>

<span data-ttu-id="8b648-161">Aşağıdaki kod, iki adlandırılmış alan yolu oluşturmak için <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> kullanır:</span><span class="sxs-lookup"><span data-stu-id="8b648-161">The following code uses <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/31samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=21-29)]

<span data-ttu-id="8b648-162">Daha fazla bilgi için bkz. [alan yönlendirme](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="8b648-162">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="8b648-163">MVC alanlarıyla bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b648-163">Link generation with MVC areas</span></span>

<span data-ttu-id="8b648-164">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples) aşağıdaki kodu, belirtilen alanla birlikte bağlantı oluşturmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="8b648-164">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/31samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="8b648-165">Örnek indirme, aşağıdakileri içeren [kısmi bir görünüm](xref:mvc/views/partial) içerir:</span><span class="sxs-lookup"><span data-stu-id="8b648-165">The sample download includes a [partial view](xref:mvc/views/partial) that contains:</span></span>

* <span data-ttu-id="8b648-166">Önceki bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="8b648-166">The preceding links.</span></span>
* <span data-ttu-id="8b648-167">Yukarıdaki `area` hariç olan bağlantılar belirtilmedi.</span><span class="sxs-lookup"><span data-stu-id="8b648-167">Links similar to the preceding except `area` is not specified.</span></span>

<span data-ttu-id="8b648-168">Kısmi görünüme, [Düzen dosyasında](xref:mvc/views/layout)başvurulur, bu nedenle uygulamadaki her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="8b648-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="8b648-169">Alanı belirtmeden oluşturulan bağlantılar yalnızca aynı alan ve denetleyicideki bir sayfadan başvuruluyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="8b648-170">Alan veya denetleyici belirtilmediğinde, yönlendirme [ortam](xref:mvc/controllers/routing#ambient) değerlerine göre değişir.</span><span class="sxs-lookup"><span data-stu-id="8b648-170">When the area or controller is not specified, routing depends on the [ambient](xref:mvc/controllers/routing#ambient) values.</span></span> <span data-ttu-id="8b648-171">Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="8b648-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="8b648-172">Örnek uygulama için birçok durumda, ortam değerlerini kullanmak, alanı belirtmeyen biçimlendirme ile yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8b648-172">In many cases for the sample app, using the ambient values generates incorrect links with the markup that doesn't specify the area.</span></span>

<span data-ttu-id="8b648-173">Daha fazla bilgi için bkz. [Denetleyici eylemlerine yönlendirme](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="8b648-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="8b648-174">_ViewStart. cshtml dosyasını kullanan alanların paylaşılan düzeni</span><span class="sxs-lookup"><span data-stu-id="8b648-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="8b648-175">Uygulamanın tamamı için ortak bir düzen paylaşmak üzere, [uygulama kök klasöründe](#arf) *_ViewStart. cshtml* 'yi saklayın.</span><span class="sxs-lookup"><span data-stu-id="8b648-175">To share a common layout for the entire app, keep the *_ViewStart.cshtml* in the [application root folder](#arf).</span></span> <span data-ttu-id="8b648-176">Daha fazla bilgi için bkz. <xref:mvc/views/layout></span><span class="sxs-lookup"><span data-stu-id="8b648-176">For more information, see <xref:mvc/views/layout></span></span>

<a name="arf"></a>

### <a name="application-root-folder"></a><span data-ttu-id="8b648-177">Uygulama kök klasörü</span><span class="sxs-lookup"><span data-stu-id="8b648-177">Application root folder</span></span>

<span data-ttu-id="8b648-178">Uygulama kök klasörü, ASP.NET Core şablonları ile oluşturulan Web uygulamasındaki *Startup.cs* içeren klasördür.</span><span class="sxs-lookup"><span data-stu-id="8b648-178">The application root folder is the folder containing *Startup.cs* in web app created with the ASP.NET Core templates.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="8b648-179">_ViewImports. cshtml</span><span class="sxs-lookup"><span data-stu-id="8b648-179">_ViewImports.cshtml</span></span>

 <span data-ttu-id="8b648-180">*/Views/_ViewImports. cshtml*, MVC için */pages/_ViewImports. cshtml* for Razor Pages, alanlardaki görünümlere aktarılmaz.</span><span class="sxs-lookup"><span data-stu-id="8b648-180">*/Views/_ViewImports.cshtml*, for MVC, and */Pages/_ViewImports.cshtml* for Razor Pages, is not imported to views in areas.</span></span> <span data-ttu-id="8b648-181">Tüm görünümlere görünüm içeri aktarmaları sağlamak için aşağıdaki yaklaşımlardan birini kullanın:</span><span class="sxs-lookup"><span data-stu-id="8b648-181">Use one of the following approaches to provide view imports to all views:</span></span>

* <span data-ttu-id="8b648-182">[Uygulama kök klasörüne](#arf) *_ViewImports. cshtml* ekleyin.</span><span class="sxs-lookup"><span data-stu-id="8b648-182">Add *_ViewImports.cshtml* to the [application root folder](#arf).</span></span> <span data-ttu-id="8b648-183">Uygulama kök klasöründeki bir *_ViewImports. cshtml* uygulamadaki tüm görünümlere uygulanır.</span><span class="sxs-lookup"><span data-stu-id="8b648-183">A *_ViewImports.cshtml* in the application root folder will apply to all views in the app.</span></span>
* <span data-ttu-id="8b648-184">*_ViewImports. cshtml* dosyasını, alanlarda uygun görünüm klasörüne kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="8b648-184">Copy the *_ViewImports.cshtml* file to the appropriate view folder under areas.</span></span>

<span data-ttu-id="8b648-185">*_ViewImports. cshtml* dosyası genellikle [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) içeri aktarmaları, `@using`ve `@inject` deyimlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="8b648-185">The *_ViewImports.cshtml* file typically contains [Tag Helpers](xref:mvc/views/tag-helpers/intro) imports, `@using`, and `@inject` statements.</span></span> <span data-ttu-id="8b648-186">Daha fazla bilgi için bkz. [paylaşılan yönergeleri Içeri aktarma](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="8b648-186">For more information, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="8b648-187">Görünümlerin depolandığı varsayılan alan klasörünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="8b648-187">Change default area folder where views are stored</span></span>

<span data-ttu-id="8b648-188">Aşağıdaki kod, varsayılan alan klasörünü `"Areas"` `"MyAreas"`olarak değiştirir:</span><span class="sxs-lookup"><span data-stu-id="8b648-188">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/31samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="8b648-189">Razor Pages alan bölgeler</span><span class="sxs-lookup"><span data-stu-id="8b648-189">Areas with Razor Pages</span></span>

<span data-ttu-id="8b648-190">Razor Pages olan alanların, uygulamanın kökünde bir `Areas/<area name>/Pages` klasör olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b648-190">Areas with Razor Pages require an `Areas/<area name>/Pages` folder in the root of the app.</span></span> <span data-ttu-id="8b648-191">Aşağıdaki klasör yapısı [örnek uygulamayla](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples)birlikte kullanılır:</span><span class="sxs-lookup"><span data-stu-id="8b648-191">The following folder structure is used with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples):</span></span>

* <span data-ttu-id="8b648-192">Proje adı</span><span class="sxs-lookup"><span data-stu-id="8b648-192">Project name</span></span>
  * <span data-ttu-id="8b648-193">Alanlar</span><span class="sxs-lookup"><span data-stu-id="8b648-193">Areas</span></span>
    * <span data-ttu-id="8b648-194">Ürünler</span><span class="sxs-lookup"><span data-stu-id="8b648-194">Products</span></span>
      * <span data-ttu-id="8b648-195">Sayfaları</span><span class="sxs-lookup"><span data-stu-id="8b648-195">Pages</span></span>
        * <span data-ttu-id="8b648-196">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="8b648-196">_ViewImports</span></span>
        * <span data-ttu-id="8b648-197">Hakkında</span><span class="sxs-lookup"><span data-stu-id="8b648-197">About</span></span>
        * <span data-ttu-id="8b648-198">Dizin oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b648-198">Index</span></span>
    * <span data-ttu-id="8b648-199">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="8b648-199">Services</span></span>
      * <span data-ttu-id="8b648-200">Sayfaları</span><span class="sxs-lookup"><span data-stu-id="8b648-200">Pages</span></span>
        * <span data-ttu-id="8b648-201">Yönetme</span><span class="sxs-lookup"><span data-stu-id="8b648-201">Manage</span></span>
          * <span data-ttu-id="8b648-202">Hakkında</span><span class="sxs-lookup"><span data-stu-id="8b648-202">About</span></span>
          * <span data-ttu-id="8b648-203">Dizin oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b648-203">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="8b648-204">Razor Pages ve alanlarıyla bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b648-204">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="8b648-205">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) aşağıdaki kodu, belirtilen alanla birlikte bağlantı oluşturmayı gösterir (örneğin, `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="8b648-205">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/31samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="8b648-206">Örnek indirme, önceki bağlantıları içeren [kısmi bir görünüm](xref:mvc/views/partial) ve alanı belirtmeksizin aynı bağlantıları içerir.</span><span class="sxs-lookup"><span data-stu-id="8b648-206">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="8b648-207">Kısmi görünüme, [Düzen dosyasında](xref:mvc/views/layout)başvurulur, bu nedenle uygulamadaki her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="8b648-207">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="8b648-208">Alanı belirtmeden oluşturulan bağlantılar yalnızca aynı alandaki bir sayfadan başvuruluyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-208">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="8b648-209">Alan belirtilmediğinde, yönlendirme *ortam* değerlerine göre değişir.</span><span class="sxs-lookup"><span data-stu-id="8b648-209">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="8b648-210">Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="8b648-210">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="8b648-211">Örnek uygulama için birçok durumda, ortam değerlerini kullanmak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8b648-211">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="8b648-212">Örneğin, aşağıdaki koddan oluşturulan bağlantıları göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8b648-212">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/31samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="8b648-213">Önceki kod için:</span><span class="sxs-lookup"><span data-stu-id="8b648-213">For the preceding code:</span></span>

* <span data-ttu-id="8b648-214">`<a asp-page="/Manage/About">` oluşturulan bağlantı, yalnızca son istek `Services` alanındaki bir sayfa için olduğunda doğrudur.</span><span class="sxs-lookup"><span data-stu-id="8b648-214">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="8b648-215">Örneğin, `/Services/Manage/`, `/Services/Manage/Index`veya `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="8b648-215">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="8b648-216">`<a asp-page="/About">` oluşturulan bağlantı, yalnızca son istek `/Home`bir sayfa için olduğunda doğrudur.</span><span class="sxs-lookup"><span data-stu-id="8b648-216">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="8b648-217">Kod, [örnek indirden](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="8b648-217">The code is from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/31samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="8b648-218">Ad alanı ve etiket yardımcıları _ViewImports dosya ile içeri aktarma</span><span class="sxs-lookup"><span data-stu-id="8b648-218">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="8b648-219">Bir *_ViewImports. cshtml* dosyası her bir alan *sayfaları* klasörüne eklenerek, ad alanı ve etiket yardımcıları, klasördeki her bir Razor sayfasına içeri aktarabilir.</span><span class="sxs-lookup"><span data-stu-id="8b648-219">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="8b648-220">Örnek kodun bir *_ViewImports. cshtml* dosyası içermeyen *Hizmetler* alanını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="8b648-220">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="8b648-221">Aşağıdaki biçimlendirme */Services/Manage/about* Razor sayfasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="8b648-221">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/31samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="8b648-222">Önceki biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="8b648-222">In the preceding markup:</span></span>

* <span data-ttu-id="8b648-223">Modeli belirtmek için tam etki alanı adının kullanılması gerekir (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="8b648-223">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="8b648-224">[Etiket yardımcıları](xref:mvc/views/tag-helpers/intro) `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` tarafından etkinleştirilir</span><span class="sxs-lookup"><span data-stu-id="8b648-224">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="8b648-225">Örnek indirme sırasında, ürünler alanı aşağıdaki *_ViewImports. cshtml* dosyasını içerir:</span><span class="sxs-lookup"><span data-stu-id="8b648-225">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/31samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="8b648-226">Aşağıdaki biçimlendirme */Products/about* Razor sayfasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="8b648-226">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/31samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="8b648-227">Önceki dosyada, ad alanı ve `@addTagHelper` yönergesi, *alan/ürünler/Pages/_ViewImports. cshtml* dosyası tarafından dosyaya aktarılır.</span><span class="sxs-lookup"><span data-stu-id="8b648-227">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="8b648-228">Daha fazla bilgi için bkz. [etiket Yardımcısı kapsamını yönetme](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) ve [paylaşılan yönergeleri içeri aktarma](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="8b648-228">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="8b648-229">Razor Pages alanlarıyla ilgili paylaşılan düzen</span><span class="sxs-lookup"><span data-stu-id="8b648-229">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="8b648-230">Uygulamanın tamamında ortak bir düzen paylaşmak için *_ViewStart. cshtml* 'yi uygulama kök klasörüne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="8b648-230">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="8b648-231">Yayımlama alanı</span><span class="sxs-lookup"><span data-stu-id="8b648-231">Publishing Areas</span></span>

<span data-ttu-id="8b648-232">*Wwwroot* dizinindeki tüm \*. cshtml dosyaları ve dosyaları, `<Project Sdk="Microsoft.NET.Sdk.Web">` \*. csproj dosyasına dahil edildiğinde çıktıya yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="8b648-232">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8b648-233">Bölgeler, ilgili işlevselliği ayrı bir ad alanı (yönlendirme için) ve klasör yapısı (görünümler için) olarak bir grup içinde düzenlemek için kullanılan bir ASP.NET özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-233">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="8b648-234">Alanların kullanılması `area`, `controller` ve `action` ya da Razor sayfası `page`başka bir rota parametresi ekleyerek yönlendirme amacına yönelik bir hiyerarşi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8b648-234">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="8b648-235">Alanları, her biri kendi Razor Pages, denetleyiciler, görünümler ve modeller kümesine sahip bir ASP.NET Core Web uygulamasını daha küçük işlevsel gruplar halinde bölümlemek için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="8b648-235">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="8b648-236">Bir alan, bir uygulamanın içindeki yapısı etkin bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="8b648-236">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="8b648-237">Bir ASP.NET Core Web projesinde, sayfalar, model, denetleyici ve görünüm gibi mantıksal bileşenler farklı klasörlerde tutulur.</span><span class="sxs-lookup"><span data-stu-id="8b648-237">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="8b648-238">ASP.NET Core çalışma zamanı, bu bileşenler arasındaki ilişkiyi oluşturmak için adlandırma kurallarını kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b648-238">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="8b648-239">Büyük bir uygulama için, uygulamayı işlevlerin ayrı üst düzey alanlarında bölümlemek avantajlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="8b648-239">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="8b648-240">Örneğin, kullanıma alma, faturalandırma ve arama gibi birden çok iş birimi içeren bir e-ticaret uygulaması.</span><span class="sxs-lookup"><span data-stu-id="8b648-240">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="8b648-241">Bu birimlerin her birinin görünümleri, denetleyicileri, Razor Pages ve modelleri içermesi için kendi alanı vardır.</span><span class="sxs-lookup"><span data-stu-id="8b648-241">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="8b648-242">Şu durumlarda bir projedeki alanı kullanmayı göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8b648-242">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="8b648-243">Uygulama, mantıksal olarak ayrılabilen birden çok üst düzey işlevsel bileşenden oluşur.</span><span class="sxs-lookup"><span data-stu-id="8b648-243">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="8b648-244">Her işlevsel alanın bağımsız olarak çalışabilmesi için uygulamayı bölümlemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="8b648-244">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="8b648-245">[Örnek kodu görüntüleyin veya indirin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([nasıl indirilir](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8b648-245">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="8b648-246">İndirme örneği, test bölgeleri için temel bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="8b648-246">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="8b648-247">Razor Pages kullanıyorsanız, bu belgede [Razor Pages bulunan alanlara](#areas-with-razor-pages) bakın.</span><span class="sxs-lookup"><span data-stu-id="8b648-247">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="8b648-248">Görünümlere sahip denetleyiciler için bölgeler</span><span class="sxs-lookup"><span data-stu-id="8b648-248">Areas for controllers with views</span></span>

<span data-ttu-id="8b648-249">Alanları, denetleyicileri ve görünümleri kullanan tipik bir ASP.NET Core Web uygulaması şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="8b648-249">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="8b648-250">Bir [alan klasörü yapısı](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="8b648-250">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="8b648-251">Denetleyiciyi alanla ilişkilendirmek için [`[Area]`](#attribute) özniteliğine sahip denetleyiciler:</span><span class="sxs-lookup"><span data-stu-id="8b648-251">Controllers with the [`[Area]`](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="8b648-252">[Başlangıç alanına eklenen alan yolu](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="8b648-252">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="8b648-253">Alan klasörü yapısı</span><span class="sxs-lookup"><span data-stu-id="8b648-253">Area folder structure</span></span>

<span data-ttu-id="8b648-254">İki mantıksal grup, *ürün* ve *hizmet*içeren bir uygulamayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="8b648-254">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="8b648-255">Alan kullanarak, klasör yapısı aşağıdakine benzer olacaktır:</span><span class="sxs-lookup"><span data-stu-id="8b648-255">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="8b648-256">Proje adı</span><span class="sxs-lookup"><span data-stu-id="8b648-256">Project name</span></span>
  * <span data-ttu-id="8b648-257">Alanlar</span><span class="sxs-lookup"><span data-stu-id="8b648-257">Areas</span></span>
    * <span data-ttu-id="8b648-258">Ürünler</span><span class="sxs-lookup"><span data-stu-id="8b648-258">Products</span></span>
      * <span data-ttu-id="8b648-259">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="8b648-259">Controllers</span></span>
        * <span data-ttu-id="8b648-260">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="8b648-260">HomeController.cs</span></span>
        * <span data-ttu-id="8b648-261">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="8b648-261">ManageController.cs</span></span>
      * <span data-ttu-id="8b648-262">Görünümler</span><span class="sxs-lookup"><span data-stu-id="8b648-262">Views</span></span>
        * <span data-ttu-id="8b648-263">Ev</span><span class="sxs-lookup"><span data-stu-id="8b648-263">Home</span></span>
          * <span data-ttu-id="8b648-264">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="8b648-264">Index.cshtml</span></span>
        * <span data-ttu-id="8b648-265">Yönetme</span><span class="sxs-lookup"><span data-stu-id="8b648-265">Manage</span></span>
          * <span data-ttu-id="8b648-266">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="8b648-266">Index.cshtml</span></span>
          * <span data-ttu-id="8b648-267">. Cshtml hakkında</span><span class="sxs-lookup"><span data-stu-id="8b648-267">About.cshtml</span></span>
    * <span data-ttu-id="8b648-268">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="8b648-268">Services</span></span>
      * <span data-ttu-id="8b648-269">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="8b648-269">Controllers</span></span>
        * <span data-ttu-id="8b648-270">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="8b648-270">HomeController.cs</span></span>
      * <span data-ttu-id="8b648-271">Görünümler</span><span class="sxs-lookup"><span data-stu-id="8b648-271">Views</span></span>
        * <span data-ttu-id="8b648-272">Ev</span><span class="sxs-lookup"><span data-stu-id="8b648-272">Home</span></span>
          * <span data-ttu-id="8b648-273">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="8b648-273">Index.cshtml</span></span>

<span data-ttu-id="8b648-274">Yukarıdaki düzen, alan kullanılırken tipik olsa da, bu klasör yapısını kullanmak için yalnızca görünüm dosyaları gereklidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-274">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="8b648-275">Eşleşen bir alan görünümü dosyası için bulma aramalarını aşağıdaki sırayla görüntüleyin:</span><span class="sxs-lookup"><span data-stu-id="8b648-275">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
```

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="8b648-276">Denetleyiciyi bir alanla ilişkilendir</span><span class="sxs-lookup"><span data-stu-id="8b648-276">Associate the controller with an Area</span></span>

<span data-ttu-id="8b648-277">Alan denetleyicileri [&lbrack;alanı&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) özniteliğiyle belirlenir:</span><span class="sxs-lookup"><span data-stu-id="8b648-277">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="8b648-278">Alan yolu Ekle</span><span class="sxs-lookup"><span data-stu-id="8b648-278">Add Area route</span></span>

<span data-ttu-id="8b648-279">Alan rotaları genellikle öznitelik yönlendirme yerine geleneksel yönlendirmeyi kullanır.</span><span class="sxs-lookup"><span data-stu-id="8b648-279">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="8b648-280">Geleneksel yönlendirme sıra bağımlıdır.</span><span class="sxs-lookup"><span data-stu-id="8b648-280">Conventional routing is order-dependent.</span></span> <span data-ttu-id="8b648-281">Genel olarak, alanlar içeren rotalar, alan olmayan rotalardan daha belirgin olduklarından daha önce rota tablosuna yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-281">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="8b648-282">`{area:...}`, URL alanı tüm alanlarda Tekdüzen ise yol şablonlarında bir belirteç olarak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="8b648-282">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="8b648-283">Yukarıdaki kodda `exists`, yolun bir alanla eşleşmesi gereken bir kısıtlama uygular.</span><span class="sxs-lookup"><span data-stu-id="8b648-283">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="8b648-284">`{area:...}` kullanmak, alanlara yönlendirme eklemek için en az karmaşık mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="8b648-284">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="8b648-285">Aşağıdaki kod, iki adlandırılmış alan yolu oluşturmak için <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> kullanır:</span><span class="sxs-lookup"><span data-stu-id="8b648-285">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="8b648-286">ASP.NET Core 2,2 ile `MapAreaRoute` kullanırken, [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore/issues/7772)bakın.</span><span class="sxs-lookup"><span data-stu-id="8b648-286">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/dotnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="8b648-287">Daha fazla bilgi için bkz. [alan yönlendirme](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="8b648-287">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="8b648-288">MVC alanlarıyla bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b648-288">Link generation with MVC areas</span></span>

<span data-ttu-id="8b648-289">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) aşağıdaki kodu, belirtilen alanla birlikte bağlantı oluşturmayı gösterir:</span><span class="sxs-lookup"><span data-stu-id="8b648-289">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="8b648-290">Yukarıdaki kodla oluşturulan bağlantılar, uygulamanın herhangi bir yerinde geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-290">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="8b648-291">Örnek indirme, önceki bağlantıları içeren [kısmi bir görünüm](xref:mvc/views/partial) ve alanı belirtmeksizin aynı bağlantıları içerir.</span><span class="sxs-lookup"><span data-stu-id="8b648-291">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="8b648-292">Kısmi görünüme, [Düzen dosyasında](xref:mvc/views/layout)başvurulur, bu nedenle uygulamadaki her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="8b648-292">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="8b648-293">Alanı belirtmeden oluşturulan bağlantılar yalnızca aynı alan ve denetleyicideki bir sayfadan başvuruluyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-293">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="8b648-294">Alan veya denetleyici belirtilmediğinde, yönlendirme *ortam* değerlerine göre değişir.</span><span class="sxs-lookup"><span data-stu-id="8b648-294">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="8b648-295">Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="8b648-295">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="8b648-296">Örnek uygulama için birçok durumda, ortam değerlerini kullanmak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8b648-296">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="8b648-297">Daha fazla bilgi için bkz. [Denetleyici eylemlerine yönlendirme](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="8b648-297">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-_viewstartcshtml-file"></a><span data-ttu-id="8b648-298">_ViewStart. cshtml dosyasını kullanan alanların paylaşılan düzeni</span><span class="sxs-lookup"><span data-stu-id="8b648-298">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="8b648-299">Uygulamanın tamamında ortak bir düzen paylaşmak için *_ViewStart. cshtml* 'yi uygulama kök klasörüne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="8b648-299">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="_viewimportscshtml"></a><span data-ttu-id="8b648-300">_ViewImports. cshtml</span><span class="sxs-lookup"><span data-stu-id="8b648-300">_ViewImports.cshtml</span></span>

<span data-ttu-id="8b648-301">Standart konumunda */views/_ViewImports. cshtml* , alanlara uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="8b648-301">In its standard location, */Views/_ViewImports.cshtml* doesn't apply to areas.</span></span> <span data-ttu-id="8b648-302">Bölgenizdeki ortak [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), `@using`veya `@inject` kullanmak için, uygun bir *_ViewImports. cshtml* dosyasının, [alan Görünümleriniz için geçerli](xref:mvc/views/layout#importing-shared-directives)olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="8b648-302">To use common [Tag Helpers](xref:mvc/views/tag-helpers/intro), `@using`, or `@inject` in your area, ensure a proper *_ViewImports.cshtml* file [applies to your area views](xref:mvc/views/layout#importing-shared-directives).</span></span> <span data-ttu-id="8b648-303">Tüm görünümlerinizin aynı davranışını istiyorsanız, */views/_ViewImports. cshtml* öğesini uygulama köküne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="8b648-303">If you want the same behavior in all your views, move */Views/_ViewImports.cshtml* to the application root.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="8b648-304">Görünümlerin depolandığı varsayılan alan klasörünü değiştirme</span><span class="sxs-lookup"><span data-stu-id="8b648-304">Change default area folder where views are stored</span></span>

<span data-ttu-id="8b648-305">Aşağıdaki kod, varsayılan alan klasörünü `"Areas"` `"MyAreas"`olarak değiştirir:</span><span class="sxs-lookup"><span data-stu-id="8b648-305">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="8b648-306">Razor Pages alan bölgeler</span><span class="sxs-lookup"><span data-stu-id="8b648-306">Areas with Razor Pages</span></span>

<span data-ttu-id="8b648-307">Razor Pages olan alanların, uygulamanın kökünde bir `Areas/<area name>/Pages` klasör olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b648-307">Areas with Razor Pages require an `Areas/<area name>/Pages` folder in the root of the app.</span></span> <span data-ttu-id="8b648-308">Aşağıdaki klasör yapısı [örnek uygulamayla](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)birlikte kullanılır:</span><span class="sxs-lookup"><span data-stu-id="8b648-308">The following folder structure is used with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples):</span></span>

* <span data-ttu-id="8b648-309">Proje adı</span><span class="sxs-lookup"><span data-stu-id="8b648-309">Project name</span></span>
  * <span data-ttu-id="8b648-310">Alanlar</span><span class="sxs-lookup"><span data-stu-id="8b648-310">Areas</span></span>
    * <span data-ttu-id="8b648-311">Ürünler</span><span class="sxs-lookup"><span data-stu-id="8b648-311">Products</span></span>
      * <span data-ttu-id="8b648-312">Sayfaları</span><span class="sxs-lookup"><span data-stu-id="8b648-312">Pages</span></span>
        * <span data-ttu-id="8b648-313">_ViewImports</span><span class="sxs-lookup"><span data-stu-id="8b648-313">_ViewImports</span></span>
        * <span data-ttu-id="8b648-314">Hakkında</span><span class="sxs-lookup"><span data-stu-id="8b648-314">About</span></span>
        * <span data-ttu-id="8b648-315">Dizin oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b648-315">Index</span></span>
    * <span data-ttu-id="8b648-316">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="8b648-316">Services</span></span>
      * <span data-ttu-id="8b648-317">Sayfaları</span><span class="sxs-lookup"><span data-stu-id="8b648-317">Pages</span></span>
        * <span data-ttu-id="8b648-318">Yönetme</span><span class="sxs-lookup"><span data-stu-id="8b648-318">Manage</span></span>
          * <span data-ttu-id="8b648-319">Hakkında</span><span class="sxs-lookup"><span data-stu-id="8b648-319">About</span></span>
          * <span data-ttu-id="8b648-320">Dizin oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b648-320">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="8b648-321">Razor Pages ve alanlarıyla bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b648-321">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="8b648-322">[Örnek indirmenin](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) aşağıdaki kodu, belirtilen alanla birlikte bağlantı oluşturmayı gösterir (örneğin, `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="8b648-322">The following code from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="8b648-323">Yukarıdaki kodla oluşturulan bağlantılar, uygulamanın herhangi bir yerinde geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-323">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="8b648-324">Örnek indirme, önceki bağlantıları içeren [kısmi bir görünüm](xref:mvc/views/partial) ve alanı belirtmeksizin aynı bağlantıları içerir.</span><span class="sxs-lookup"><span data-stu-id="8b648-324">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="8b648-325">Kısmi görünüme, [Düzen dosyasında](xref:mvc/views/layout)başvurulur, bu nedenle uygulamadaki her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="8b648-325">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="8b648-326">Alanı belirtmeden oluşturulan bağlantılar yalnızca aynı alandaki bir sayfadan başvuruluyorsa geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8b648-326">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="8b648-327">Alan belirtilmediğinde, yönlendirme *ortam* değerlerine göre değişir.</span><span class="sxs-lookup"><span data-stu-id="8b648-327">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="8b648-328">Geçerli isteğin geçerli yol değerleri, bağlantı oluşturma için çevresel değerler olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="8b648-328">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="8b648-329">Örnek uygulama için birçok durumda, ortam değerlerini kullanmak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8b648-329">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="8b648-330">Örneğin, aşağıdaki koddan oluşturulan bağlantıları göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="8b648-330">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="8b648-331">Önceki kod için:</span><span class="sxs-lookup"><span data-stu-id="8b648-331">For the preceding code:</span></span>

* <span data-ttu-id="8b648-332">`<a asp-page="/Manage/About">` oluşturulan bağlantı, yalnızca son istek `Services` alanındaki bir sayfa için olduğunda doğrudur.</span><span class="sxs-lookup"><span data-stu-id="8b648-332">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="8b648-333">Örneğin, `/Services/Manage/`, `/Services/Manage/Index`veya `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="8b648-333">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="8b648-334">`<a asp-page="/About">` oluşturulan bağlantı, yalnızca son istek `/Home`bir sayfa için olduğunda doğrudur.</span><span class="sxs-lookup"><span data-stu-id="8b648-334">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="8b648-335">Kod, [örnek indirden](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="8b648-335">The code is from the [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-_viewimports-file"></a><span data-ttu-id="8b648-336">Ad alanı ve etiket yardımcıları _ViewImports dosya ile içeri aktarma</span><span class="sxs-lookup"><span data-stu-id="8b648-336">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="8b648-337">Bir *_ViewImports. cshtml* dosyası her bir alan *sayfaları* klasörüne eklenerek, ad alanı ve etiket yardımcıları, klasördeki her bir Razor sayfasına içeri aktarabilir.</span><span class="sxs-lookup"><span data-stu-id="8b648-337">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="8b648-338">Örnek kodun bir *_ViewImports. cshtml* dosyası içermeyen *Hizmetler* alanını göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="8b648-338">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="8b648-339">Aşağıdaki biçimlendirme */Services/Manage/about* Razor sayfasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="8b648-339">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="8b648-340">Önceki biçimlendirmede:</span><span class="sxs-lookup"><span data-stu-id="8b648-340">In the preceding markup:</span></span>

* <span data-ttu-id="8b648-341">Modeli belirtmek için tam etki alanı adının kullanılması gerekir (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="8b648-341">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="8b648-342">[Etiket yardımcıları](xref:mvc/views/tag-helpers/intro) `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` tarafından etkinleştirilir</span><span class="sxs-lookup"><span data-stu-id="8b648-342">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="8b648-343">Örnek indirme sırasında, ürünler alanı aşağıdaki *_ViewImports. cshtml* dosyasını içerir:</span><span class="sxs-lookup"><span data-stu-id="8b648-343">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="8b648-344">Aşağıdaki biçimlendirme */Products/about* Razor sayfasını göstermektedir:</span><span class="sxs-lookup"><span data-stu-id="8b648-344">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="8b648-345">Önceki dosyada, ad alanı ve `@addTagHelper` yönergesi, *alan/ürünler/Pages/_ViewImports. cshtml* dosyası tarafından dosyaya aktarılır.</span><span class="sxs-lookup"><span data-stu-id="8b648-345">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="8b648-346">Daha fazla bilgi için bkz. [etiket Yardımcısı kapsamını yönetme](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) ve [paylaşılan yönergeleri içeri aktarma](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="8b648-346">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="8b648-347">Razor Pages alanlarıyla ilgili paylaşılan düzen</span><span class="sxs-lookup"><span data-stu-id="8b648-347">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="8b648-348">Uygulamanın tamamında ortak bir düzen paylaşmak için *_ViewStart. cshtml* 'yi uygulama kök klasörüne taşıyın.</span><span class="sxs-lookup"><span data-stu-id="8b648-348">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="8b648-349">Yayımlama alanı</span><span class="sxs-lookup"><span data-stu-id="8b648-349">Publishing Areas</span></span>

<span data-ttu-id="8b648-350">*Wwwroot* dizinindeki tüm \*. cshtml dosyaları ve dosyaları, `<Project Sdk="Microsoft.NET.Sdk.Web">` \*. csproj dosyasına dahil edildiğinde çıktıya yayımlanır.</span><span class="sxs-lookup"><span data-stu-id="8b648-350">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
::: moniker-end
