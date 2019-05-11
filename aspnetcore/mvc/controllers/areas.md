---
title: ASP.NET core'da alanları
author: rick-anderson
description: Alanlar (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba ilgili işlevleri düzenlemek için kullanılan bir ASP.NET MVC özelliği nasıl olduğunu öğrenin.
ms.author: riande
ms.date: 05/10/2019
uid: mvc/controllers/areas
ms.openlocfilehash: f3a75bc307a206e43241b421f448b09011868d08
ms.sourcegitcommit: ffe3ed7921ec6c7c70abaac1d10703ec9a43374c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65535964"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="75f7d-103">ASP.NET core'da alanları</span><span class="sxs-lookup"><span data-stu-id="75f7d-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="75f7d-104">Tarafından [Dhananjay Kumar](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="75f7d-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="75f7d-105">Alanlar (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba ilgili işlevleri düzenlemek için kullanılan, ASP.NET bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="75f7d-106">Alanlara kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla hiyerarşi oluşturur `area`, `controller` ve `action` veya bir Razor sayfası `page`.</span><span class="sxs-lookup"><span data-stu-id="75f7d-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="75f7d-107">Alanları bir ASP.NET Core Web uygulaması daha küçük işlevsel gruplar halinde bölümlere ayırmak için bir yol her biri kendi Razor sayfaları, denetleyicileri, görünümler ve modeller kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="75f7d-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="75f7d-108">Bir alan etkin bir uygulama içinde bir yapıdır.</span><span class="sxs-lookup"><span data-stu-id="75f7d-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="75f7d-109">Bir ASP.NET Core web projesinde farklı klasörlerde bulunan sayfaları, Model, denetleyici ve görünüm gibi mantıksal bileşenler tutulur.</span><span class="sxs-lookup"><span data-stu-id="75f7d-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="75f7d-110">ASP.NET Core çalışma zamanı, bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="75f7d-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="75f7d-111">Büyük bir uygulama için ayrı yüksek düzey alanlarına işlev uygulamasını bölümleme için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="75f7d-112">Örneğin, bir e-ticaret uygulamayla kullanıma alma, fatura ve arama gibi birden çok iş birimleri.</span><span class="sxs-lookup"><span data-stu-id="75f7d-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="75f7d-113">Bu birimleri her görünümleri, denetleyicileri, Razor sayfaları ve modelleri içerecek şekilde kendi alanı vardır.</span><span class="sxs-lookup"><span data-stu-id="75f7d-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="75f7d-114">Bir proje alanlarını kullanmayı olduğunda:</span><span class="sxs-lookup"><span data-stu-id="75f7d-114">Consider using Areas in a project when:</span></span>

* <span data-ttu-id="75f7d-115">Uygulama, mantıksal olarak ayrılabilen birden çok üst düzey işlevsel bileşenden.</span><span class="sxs-lookup"><span data-stu-id="75f7d-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="75f7d-116">Böylece her işlevsel alan üzerinde bağımsız olarak çalışılabilecek uygulamasını bölümleme istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="75f7d-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="75f7d-117">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="75f7d-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="75f7d-118">İndirme örnek alanları test etmek için temel bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="75f7d-118">The download sample provides a basic app for testing areas.</span></span>

<span data-ttu-id="75f7d-119">Razor sayfaları kullanıyorsanız, bkz. [Razor sayfalarıyla alanlarını](#areas-with-razor-pages) bu belgedeki.</span><span class="sxs-lookup"><span data-stu-id="75f7d-119">If you're using Razor Pages, see [Areas with Razor Pages](#areas-with-razor-pages) in this document.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="75f7d-120">Görünüm denetleyicileri için alanları</span><span class="sxs-lookup"><span data-stu-id="75f7d-120">Areas for controllers with views</span></span>

<span data-ttu-id="75f7d-121">Alanları denetleyicileri ve görünümleri tipik bir ASP.NET Core web uygulaması şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="75f7d-121">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="75f7d-122">Bir [alan klasör yapısını](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="75f7d-122">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="75f7d-123">Denetleyicileri düzenlenmiş ile [ &lbrack;alan&rbrack; ](#attribute) denetleyici alanı ile ilişkilendirilecek özniteliği:</span><span class="sxs-lookup"><span data-stu-id="75f7d-123">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area:</span></span>

  [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]

* <span data-ttu-id="75f7d-124">[Başlangıç olarak eklenen alan yolu](#add-area-route):</span><span class="sxs-lookup"><span data-stu-id="75f7d-124">The [area route added to startup](#add-area-route):</span></span>

  [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]

### <a name="area-folder-structure"></a><span data-ttu-id="75f7d-125">Alan klasör yapısı</span><span class="sxs-lookup"><span data-stu-id="75f7d-125">Area folder structure</span></span>

<span data-ttu-id="75f7d-126">İki mantıksal gruplar olan bir uygulama düşünün *ürünleri* ve *Hizmetleri*.</span><span class="sxs-lookup"><span data-stu-id="75f7d-126">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="75f7d-127">Alanlara kullanarak klasör yapısı şuna benzer olacaktır:</span><span class="sxs-lookup"><span data-stu-id="75f7d-127">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="75f7d-128">Proje adı</span><span class="sxs-lookup"><span data-stu-id="75f7d-128">Project name</span></span>
  * <span data-ttu-id="75f7d-129">Alanlar</span><span class="sxs-lookup"><span data-stu-id="75f7d-129">Areas</span></span>
    * <span data-ttu-id="75f7d-130">Ürünler</span><span class="sxs-lookup"><span data-stu-id="75f7d-130">Products</span></span>
      * <span data-ttu-id="75f7d-131">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="75f7d-131">Controllers</span></span>
        * <span data-ttu-id="75f7d-132">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="75f7d-132">HomeController.cs</span></span>
        * <span data-ttu-id="75f7d-133">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="75f7d-133">ManageController.cs</span></span>
      * <span data-ttu-id="75f7d-134">Görünümler</span><span class="sxs-lookup"><span data-stu-id="75f7d-134">Views</span></span>
        * <span data-ttu-id="75f7d-135">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="75f7d-135">Home</span></span>
          * <span data-ttu-id="75f7d-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="75f7d-136">Index.cshtml</span></span>
        * <span data-ttu-id="75f7d-137">yönetme</span><span class="sxs-lookup"><span data-stu-id="75f7d-137">Manage</span></span>
          * <span data-ttu-id="75f7d-138">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="75f7d-138">Index.cshtml</span></span>
          * <span data-ttu-id="75f7d-139">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="75f7d-139">About.cshtml</span></span>
    * <span data-ttu-id="75f7d-140">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="75f7d-140">Services</span></span>
      * <span data-ttu-id="75f7d-141">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="75f7d-141">Controllers</span></span>
        * <span data-ttu-id="75f7d-142">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="75f7d-142">HomeController.cs</span></span>
      * <span data-ttu-id="75f7d-143">Görünümler</span><span class="sxs-lookup"><span data-stu-id="75f7d-143">Views</span></span>
        * <span data-ttu-id="75f7d-144">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="75f7d-144">Home</span></span>
          * <span data-ttu-id="75f7d-145">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="75f7d-145">Index.cshtml</span></span>

<span data-ttu-id="75f7d-146">Önceki Düzen alanları kullanılırken tipik olsa da, yalnızca görünüm dosyaları bu klasör yapısı kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-146">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="75f7d-147">Görünüm bulma eşleşen bir alan görünüm dosyası şu sırayla arar:</span><span class="sxs-lookup"><span data-stu-id="75f7d-147">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="75f7d-148">Klasörleri görüntüle konumunu ister *denetleyicileri* ve *modelleri* mu **değil** önemi.</span><span class="sxs-lookup"><span data-stu-id="75f7d-148">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="75f7d-149">Örneğin, *denetleyicileri* ve *modelleri* klasörü gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-149">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="75f7d-150">İçeriği *denetleyicileri* ve *modelleri* bir .dll derlenmiş kodudur.</span><span class="sxs-lookup"><span data-stu-id="75f7d-150">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="75f7d-151">İçeriği *görünümleri* , görünüm için bir istek yayınlanana kadar derlenmiş değil.</span><span class="sxs-lookup"><span data-stu-id="75f7d-151">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="75f7d-152">Denetleyici ile bir alanı ilişkilendirin</span><span class="sxs-lookup"><span data-stu-id="75f7d-152">Associate the controller with an Area</span></span>

<span data-ttu-id="75f7d-153">Alanı denetleyicileri ile atanır [ &lbrack;alan&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="75f7d-153">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="75f7d-154">Alan yolu ekleme</span><span class="sxs-lookup"><span data-stu-id="75f7d-154">Add Area route</span></span>

<span data-ttu-id="75f7d-155">Alan yolları, genellikle geleneksel özniteliği yerine yönlendirmesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="75f7d-155">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="75f7d-156">Geleneksel yönlendirme sipariş bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="75f7d-156">Conventional routing is order-dependent.</span></span> <span data-ttu-id="75f7d-157">Genel olarak, bunlar olmadan bir alan yolları daha ayrıntılı olarak alanları rotalarla önceki rota tablosunda yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-157">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="75f7d-158">`{area:...}` URL alanı tüm alanlar arasında Tekdüzen ise, rota şablonlarındaki bir belirteci olarak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="75f7d-158">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="75f7d-159">Önceki kodda, `exists` yol alanı eşleşmelidir kısıtlama uygular.</span><span class="sxs-lookup"><span data-stu-id="75f7d-159">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="75f7d-160">Kullanarak `{area:...}` alanlarına yönlendirme eklemeye az karmaşık mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="75f7d-160">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="75f7d-161">Aşağıdaki kod <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> alan yolları adlı iki oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="75f7d-161">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="75f7d-162">Kullanırken `MapAreaRoute` bkz: ASP.NET Core 2.2 ile [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="75f7d-162">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="75f7d-163">Daha fazla bilgi için [alan yönlendirme](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="75f7d-163">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-mvc-areas"></a><span data-ttu-id="75f7d-164">MVC alanları ile bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="75f7d-164">Link generation with MVC areas</span></span>

<span data-ttu-id="75f7d-165">Aşağıdaki kodu [örnek indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) gösterir, belirtilen alanla nesil bağlantı:</span><span class="sxs-lookup"><span data-stu-id="75f7d-165">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="75f7d-166">Yukarıdaki kod ile oluşturulan herhangi bir uygulamada geçerli bağlantılardır.</span><span class="sxs-lookup"><span data-stu-id="75f7d-166">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="75f7d-167">Örnek indirme içeren bir [kısmi Görünüm](xref:mvc/views/partial) içeren önceki bağlantıları ve aynı bağlantı alanı belirtmeden.</span><span class="sxs-lookup"><span data-stu-id="75f7d-167">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="75f7d-168">Kısmi görünüm başvurulduğundan [Düzen dosyası](xref:mvc/views/layout), uygulamayı her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="75f7d-168">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="75f7d-169">Alanı belirtmeden oluşturulan bağlantıları, yalnızca bir sayfa aynı alan ve denetleyici başvurulduğunda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-169">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="75f7d-170">Alan veya denetleyici belirtilmediğinde yönlendirme bağlıdır *ortam* değerleri.</span><span class="sxs-lookup"><span data-stu-id="75f7d-170">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="75f7d-171">Geçerli isteğin geçerli rota değerleri için bağlantı oluşturma ortam değerleri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-171">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="75f7d-172">Örnek uygulama için çoğu durumda, ortam değerleri kullanarak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="75f7d-172">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="75f7d-173">Daha fazla bilgi için [denetleyici eylemlerine yönlendirme](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="75f7d-173">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="75f7d-174">Düzen _ViewStart.cshtml dosyasını kullanarak alanları için paylaşılan</span><span class="sxs-lookup"><span data-stu-id="75f7d-174">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="75f7d-175">Uygulamanın tamamında için yaygın bir düzen paylaşmak için taşıma *_ViewStart.cshtml* uygulama kök klasörüne.</span><span class="sxs-lookup"><span data-stu-id="75f7d-175">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="75f7d-176">Görünümleri depolandığı varsayılan alanı klasörü Değiştir</span><span class="sxs-lookup"><span data-stu-id="75f7d-176">Change default area folder where views are stored</span></span>

<span data-ttu-id="75f7d-177">Varsayılan alan klasöründen aşağıdaki kod değişiklikleri `"Areas"` için `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="75f7d-177">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<a name="arp"></a>

## <a name="areas-with-razor-pages"></a><span data-ttu-id="75f7d-178">Razor sayfalarıyla alanlarını</span><span class="sxs-lookup"><span data-stu-id="75f7d-178">Areas with Razor Pages</span></span>

<span data-ttu-id="75f7d-179">Razor sayfalarıyla alanlarını gerektirir ve *alanlar /&lt;alan adı&gt;/sayfaları* uygulamanın kök klasöründe.</span><span class="sxs-lookup"><span data-stu-id="75f7d-179">Areas with Razor Pages require and *Areas/&lt;area name&gt;/Pages* folder in the root of the app.</span></span> <span data-ttu-id="75f7d-180">Aşağıdaki klasör yapısına ile kullanılan [örnek indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)</span><span class="sxs-lookup"><span data-stu-id="75f7d-180">The following folder structure is used with the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples)</span></span>

* <span data-ttu-id="75f7d-181">Proje adı</span><span class="sxs-lookup"><span data-stu-id="75f7d-181">Project name</span></span>
  * <span data-ttu-id="75f7d-182">Alanlar</span><span class="sxs-lookup"><span data-stu-id="75f7d-182">Areas</span></span>
    * <span data-ttu-id="75f7d-183">Ürünler</span><span class="sxs-lookup"><span data-stu-id="75f7d-183">Products</span></span>
      * <span data-ttu-id="75f7d-184">Sayfalar</span><span class="sxs-lookup"><span data-stu-id="75f7d-184">Pages</span></span>
        * <span data-ttu-id="75f7d-185">_Viewımports</span><span class="sxs-lookup"><span data-stu-id="75f7d-185">_ViewImports</span></span>
        * <span data-ttu-id="75f7d-186">Hakkında</span><span class="sxs-lookup"><span data-stu-id="75f7d-186">About</span></span>
        * <span data-ttu-id="75f7d-187">Dizin</span><span class="sxs-lookup"><span data-stu-id="75f7d-187">Index</span></span>
    * <span data-ttu-id="75f7d-188">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="75f7d-188">Services</span></span>
      * <span data-ttu-id="75f7d-189">Sayfalar</span><span class="sxs-lookup"><span data-stu-id="75f7d-189">Pages</span></span>
        * <span data-ttu-id="75f7d-190">yönetme</span><span class="sxs-lookup"><span data-stu-id="75f7d-190">Manage</span></span>
          * <span data-ttu-id="75f7d-191">Hakkında</span><span class="sxs-lookup"><span data-stu-id="75f7d-191">About</span></span>
          * <span data-ttu-id="75f7d-192">Dizin</span><span class="sxs-lookup"><span data-stu-id="75f7d-192">Index</span></span>

### <a name="link-generation-with-razor-pages-and-areas"></a><span data-ttu-id="75f7d-193">Razor sayfaları ve alanları ile bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="75f7d-193">Link generation with Razor Pages and areas</span></span>

<span data-ttu-id="75f7d-194">Aşağıdaki kodu [örnek indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) gösterir, belirtilen alanla nesil bağlantı (örneğin, `asp-area="Products"`):</span><span class="sxs-lookup"><span data-stu-id="75f7d-194">The following code from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas) shows link generation with the area specified (for example, `asp-area="Products"`):</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="75f7d-195">Yukarıdaki kod ile oluşturulan herhangi bir uygulamada geçerli bağlantılardır.</span><span class="sxs-lookup"><span data-stu-id="75f7d-195">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="75f7d-196">Örnek indirme içeren bir [kısmi Görünüm](xref:mvc/views/partial) içeren önceki bağlantıları ve aynı bağlantı alanı belirtmeden.</span><span class="sxs-lookup"><span data-stu-id="75f7d-196">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="75f7d-197">Kısmi görünüm başvurulduğundan [Düzen dosyası](xref:mvc/views/layout), uygulamayı her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="75f7d-197">The partial view is referenced in the [layout file](xref:mvc/views/layout), so every page in the app displays the generated links.</span></span> <span data-ttu-id="75f7d-198">Alanı belirtmeden oluşturulan bağlantıları, yalnızca bir sayfa aynı alanda başvurulduğunda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-198">The links generated without specifying the area are only valid when referenced from a page in the same area.</span></span>

<span data-ttu-id="75f7d-199">Alan belirtilmediğinde yönlendirme bağlıdır *ortam* değerleri.</span><span class="sxs-lookup"><span data-stu-id="75f7d-199">When the area is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="75f7d-200">Geçerli isteğin geçerli rota değerleri için bağlantı oluşturma ortam değerleri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-200">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="75f7d-201">Örnek uygulama için çoğu durumda, ortam değerleri kullanarak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="75f7d-201">In many cases for the sample app, using the ambient values generates incorrect links.</span></span> <span data-ttu-id="75f7d-202">Örneğin, aşağıdaki koddan oluşturulan bağlantılara göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="75f7d-202">For example, consider the links generated from the following code:</span></span>

[!code-cshtml[](areas/samples/RPareas/Pages/Shared/_testLinksPartial.cshtml?name=snippet2)]

<span data-ttu-id="75f7d-203">Yukarıdaki kod için:</span><span class="sxs-lookup"><span data-stu-id="75f7d-203">For the preceding code:</span></span>

* <span data-ttu-id="75f7d-204">Üretilen bağlantı `<a asp-page="/Manage/About">` yalnızca ne son isteği sayfası için zaman doğrudur `Services` alan.</span><span class="sxs-lookup"><span data-stu-id="75f7d-204">The link generated from `<a asp-page="/Manage/About">` is correct only when the last request was for a page in `Services` area.</span></span> <span data-ttu-id="75f7d-205">Örneğin, `/Services/Manage/`, `/Services/Manage/Index`, veya `/Services/Manage/About`.</span><span class="sxs-lookup"><span data-stu-id="75f7d-205">For example, `/Services/Manage/`, `/Services/Manage/Index`, or `/Services/Manage/About`.</span></span>
* <span data-ttu-id="75f7d-206">Üretilen bağlantı `<a asp-page="/About">` yalnızca ne son isteği sayfası için zaman doğrudur `/Home`.</span><span class="sxs-lookup"><span data-stu-id="75f7d-206">The link generated from `<a asp-page="/About">` is correct only when the last request was for a page in `/Home`.</span></span>
* <span data-ttu-id="75f7d-207">Kod dandır [örnek indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span><span class="sxs-lookup"><span data-stu-id="75f7d-207">The code is from the [sample download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/areas/samples/RPareas).</span></span>

### <a name="import-namespace-and-tag-helpers-with-viewimports-file"></a><span data-ttu-id="75f7d-208">Ad alanı ve etiket Yardımcıları ile _viewımports dosyasını içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="75f7d-208">Import namespace and Tag Helpers with _ViewImports file</span></span>

<span data-ttu-id="75f7d-209">A *_viewımports.cshtml* her alanı için dosya eklenebilir *sayfaları* ad alanı ve etiket Yardımcıları klasöründeki her bir Razor sayfası içe aktarmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="75f7d-209">A *_ViewImports.cshtml* file can be added to each area *Pages* folder to import the namespace and Tag Helpers to each Razor Page in the folder.</span></span>

<span data-ttu-id="75f7d-210">Göz önünde bulundurun *Hizmetleri* alan içermiyor örnek kodu bir *_viewımports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="75f7d-210">Consider the *Services* area of the sample code, which doesn't contain a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="75f7d-211">Aşağıdaki biçimlendirme gösterildiği */Services/Manage/About* Razor sayfası:</span><span class="sxs-lookup"><span data-stu-id="75f7d-211">The following markup shows the */Services/Manage/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Services/Pages/Manage/About.cshtml)]

<span data-ttu-id="75f7d-212">Önceki biçimlendirme içinde:</span><span class="sxs-lookup"><span data-stu-id="75f7d-212">In the preceding markup:</span></span>

* <span data-ttu-id="75f7d-213">Model belirtmek için tam etki alanı adı kullanılmalıdır (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span><span class="sxs-lookup"><span data-stu-id="75f7d-213">The fully qualified domain name must be used to specify the model (`@model RPareas.Areas.Services.Pages.Manage.AboutModel`).</span></span>
* <span data-ttu-id="75f7d-214">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) etkindir `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="75f7d-214">[Tag Helpers](xref:mvc/views/tag-helpers/intro) are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="75f7d-215">Örnek indirme aşağıdaki ürünler alanı içeren *_viewımports.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="75f7d-215">In the sample download, the Products area contains the following *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/_ViewImports.cshtml)]

<span data-ttu-id="75f7d-216">Aşağıdaki biçimlendirme gösterildiği */ürünler/hakkında* Razor sayfası:</span><span class="sxs-lookup"><span data-stu-id="75f7d-216">The following markup shows the */Products/About* Razor Page:</span></span>

[!code-cshtml[](areas/samples/RPareas/Areas/Products/Pages/About.cshtml)]

<span data-ttu-id="75f7d-217">Önceki dosyasında, ad alanı ve `@addTagHelper` yönergesi tarafından dosyasına aktarılır *Areas/Products/Pages/_ViewImports.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="75f7d-217">In the preceding file, the namespace and `@addTagHelper` directive is imported to the file by the *Areas/Products/Pages/_ViewImports.cshtml* file.</span></span>

<span data-ttu-id="75f7d-218">Daha fazla bilgi için [yönetme etiketi Yardımcısı kapsam](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) ve [paylaşılan yönergeleri alma](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="75f7d-218">For more information, see [Managing Tag Helper scope](xref:mvc/views/tag-helpers/intro?view=aspnetcore-2.2#managing-tag-helper-scope) and [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives).</span></span>

### <a name="shared-layout-for-razor-pages-areas"></a><span data-ttu-id="75f7d-219">Razor sayfaları alanlar için paylaşılan düzeni</span><span class="sxs-lookup"><span data-stu-id="75f7d-219">Shared layout for Razor Pages Areas</span></span>

<span data-ttu-id="75f7d-220">Uygulamanın tamamında için yaygın bir düzen paylaşmak için taşıma *_ViewStart.cshtml* uygulama kök klasörüne.</span><span class="sxs-lookup"><span data-stu-id="75f7d-220">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

### <a name="publishing-areas"></a><span data-ttu-id="75f7d-221">Yayımlama alanları</span><span class="sxs-lookup"><span data-stu-id="75f7d-221">Publishing Areas</span></span>

<span data-ttu-id="75f7d-222">Tüm \*.cshtml dosyaları ve içinde dosyaları *wwwroot* ne zaman çıkış dizinine yayımlanır `<Project Sdk="Microsoft.NET.Sdk.Web">` \*.csproj dosyasına dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="75f7d-222">All \*.cshtml files and files within the *wwwroot* directory are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the \*.csproj file.</span></span>
