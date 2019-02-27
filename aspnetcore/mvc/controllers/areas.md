---
title: ASP.NET core'da alanları
author: rick-anderson
description: Alanlar (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba ilgili işlevleri düzenlemek için kullanılan bir ASP.NET MVC özelliği nasıl olduğunu öğrenin.
ms.author: riande
ms.date: 02/14/2019
uid: mvc/controllers/areas
ms.openlocfilehash: c21eed04ea68512515da262b6b6895dc1a821039
ms.sourcegitcommit: 2c7ffe349eabdccf2ed748dd303ffd0ba6e1cfe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56833533"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="89f8b-103">ASP.NET core'da alanları</span><span class="sxs-lookup"><span data-stu-id="89f8b-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="89f8b-104">Tarafından [Dhananjay Kumar](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="89f8b-104">By [Dhananjay Kumar](https://twitter.com/debug_mode) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="89f8b-105">Alanlar (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba ilgili işlevleri düzenlemek için kullanılan, ASP.NET bir özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="89f8b-105">Areas are an ASP.NET feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="89f8b-106">Alanlara kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla hiyerarşi oluşturur `area`, `controller` ve `action` veya bir Razor sayfası `page`.</span><span class="sxs-lookup"><span data-stu-id="89f8b-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action` or a Razor Page `page`.</span></span>

<span data-ttu-id="89f8b-107">Alanları bir ASP.NET Core Web uygulaması daha küçük işlevsel gruplar halinde bölümlere ayırmak için bir yol her biri kendi Razor sayfaları, denetleyicileri, görünümler ve modeller kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="89f8b-107">Areas provide a way to partition an ASP.NET Core Web app into smaller functional groups, each  with its own set of Razor Pages, controllers, views, and models.</span></span> <span data-ttu-id="89f8b-108">Bir alan etkin bir uygulama içinde bir yapıdır.</span><span class="sxs-lookup"><span data-stu-id="89f8b-108">An area is effectively a structure inside an app.</span></span> <span data-ttu-id="89f8b-109">Bir ASP.NET Core web projesinde farklı klasörlerde bulunan sayfaları, Model, denetleyici ve görünüm gibi mantıksal bileşenler tutulur.</span><span class="sxs-lookup"><span data-stu-id="89f8b-109">In an ASP.NET Core web project, logical components like Pages, Model, Controller, and View are kept in different folders.</span></span> <span data-ttu-id="89f8b-110">ASP.NET Core çalışma zamanı, bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="89f8b-110">The ASP.NET Core runtime uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="89f8b-111">Büyük bir uygulama için ayrı yüksek düzey alanlarına işlev uygulamasını bölümleme için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="89f8b-111">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="89f8b-112">Örneğin, bir e-ticaret uygulamayla kullanıma alma, fatura ve arama gibi birden çok iş birimleri.</span><span class="sxs-lookup"><span data-stu-id="89f8b-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search.</span></span> <span data-ttu-id="89f8b-113">Bu birimleri her görünümleri, denetleyicileri, Razor sayfaları ve modelleri içerecek şekilde kendi alanı vardır.</span><span class="sxs-lookup"><span data-stu-id="89f8b-113">Each of these units have their own area to contain views, controllers, Razor Pages, and models.</span></span>

<span data-ttu-id="89f8b-114">Alanları projesinde kullanmayı olduğunda:</span><span class="sxs-lookup"><span data-stu-id="89f8b-114">Consider using Areas in an project when:</span></span>

* <span data-ttu-id="89f8b-115">Uygulama, mantıksal olarak ayrılabilen birden çok üst düzey işlevsel bileşenden.</span><span class="sxs-lookup"><span data-stu-id="89f8b-115">The app is made of multiple high-level functional components that can be logically separated.</span></span>
* <span data-ttu-id="89f8b-116">Böylece her işlevsel alan üzerinde bağımsız olarak çalışılabilecek uygulamasını bölümleme istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="89f8b-116">You want to partition the app so that each functional area can be worked on independently.</span></span>

<span data-ttu-id="89f8b-117">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="89f8b-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="89f8b-118">İndirme örnek alanları test etmek için temel bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="89f8b-118">The download sample provides a basic app for testing areas.</span></span>

## <a name="areas-for-controllers-with-views"></a><span data-ttu-id="89f8b-119">Görünüm denetleyicileri için alanları</span><span class="sxs-lookup"><span data-stu-id="89f8b-119">Areas for controllers with views</span></span>

<span data-ttu-id="89f8b-120">Alanları denetleyicileri ve görünümleri tipik bir ASP.NET Core web uygulaması şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="89f8b-120">A typical ASP.NET Core web app using areas, controllers, and views contains the following:</span></span>

* <span data-ttu-id="89f8b-121">Bir [alan klasör yapısını](#area-folder-structure).</span><span class="sxs-lookup"><span data-stu-id="89f8b-121">An [Area folder structure](#area-folder-structure).</span></span>
* <span data-ttu-id="89f8b-122">Denetleyicileri düzenlenmiş ile [ &lbrack;alan&rbrack; ](#attribute) denetleyici alanı ile ilişkilendirilecek özniteliği: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="89f8b-122">Controllers decorated with the [&lbrack;Area&rbrack;](#attribute) attribute to associate the controller with the area: [!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?name=snippet2)]</span></span>
* <span data-ttu-id="89f8b-123">[Başlangıç olarak eklenen alan yolu](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="89f8b-123">The [area route added to startup](#add-area-route): [!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet2&highlight=3-6)]</span></span>

## <a name="area-folder-structure"></a><span data-ttu-id="89f8b-124">Alan klasör yapısı</span><span class="sxs-lookup"><span data-stu-id="89f8b-124">Area folder structure</span></span>
<span data-ttu-id="89f8b-125">İki mantıksal gruplar olan bir uygulama düşünün *ürünleri* ve *Hizmetleri*.</span><span class="sxs-lookup"><span data-stu-id="89f8b-125">Consider an app that has two logical groups, *Products* and *Services*.</span></span> <span data-ttu-id="89f8b-126">Alanlara kullanarak klasör yapısı şuna benzer olacaktır:</span><span class="sxs-lookup"><span data-stu-id="89f8b-126">Using areas, the folder structure would be similar to the following:</span></span>

* <span data-ttu-id="89f8b-127">Proje adı</span><span class="sxs-lookup"><span data-stu-id="89f8b-127">Project name</span></span>
  * <span data-ttu-id="89f8b-128">Alanlar</span><span class="sxs-lookup"><span data-stu-id="89f8b-128">Areas</span></span>
    * <span data-ttu-id="89f8b-129">Ürünler</span><span class="sxs-lookup"><span data-stu-id="89f8b-129">Products</span></span>
      * <span data-ttu-id="89f8b-130">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="89f8b-130">Controllers</span></span>
        * <span data-ttu-id="89f8b-131">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="89f8b-131">HomeController.cs</span></span>
        * <span data-ttu-id="89f8b-132">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="89f8b-132">ManageController.cs</span></span>
      * <span data-ttu-id="89f8b-133">Görünümler</span><span class="sxs-lookup"><span data-stu-id="89f8b-133">Views</span></span>
        * <span data-ttu-id="89f8b-134">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="89f8b-134">Home</span></span>
          * <span data-ttu-id="89f8b-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="89f8b-135">Index.cshtml</span></span>
        * <span data-ttu-id="89f8b-136">yönetme</span><span class="sxs-lookup"><span data-stu-id="89f8b-136">Manage</span></span>
          * <span data-ttu-id="89f8b-137">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="89f8b-137">Index.cshtml</span></span>
          * <span data-ttu-id="89f8b-138">About.cshtml</span><span class="sxs-lookup"><span data-stu-id="89f8b-138">About.cshtml</span></span>
    * <span data-ttu-id="89f8b-139">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="89f8b-139">Services</span></span>
      * <span data-ttu-id="89f8b-140">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="89f8b-140">Controllers</span></span>
        * <span data-ttu-id="89f8b-141">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="89f8b-141">HomeController.cs</span></span>
      * <span data-ttu-id="89f8b-142">Görünümler</span><span class="sxs-lookup"><span data-stu-id="89f8b-142">Views</span></span>
        * <span data-ttu-id="89f8b-143">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="89f8b-143">Home</span></span>
          * <span data-ttu-id="89f8b-144">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="89f8b-144">Index.cshtml</span></span>

<span data-ttu-id="89f8b-145">Önceki Düzen alanları kullanılırken tipik olsa da, yalnızca görünüm dosyaları bu klasör yapısı kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="89f8b-145">While the preceding layout is typical when using Areas, only the view files are required to use this folder structure.</span></span> <span data-ttu-id="89f8b-146">Görünüm bulma eşleşen bir alan görünüm dosyası şu sırayla arar:</span><span class="sxs-lookup"><span data-stu-id="89f8b-146">View discovery searches for a matching area view file in the following order:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
/Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
/Views/Shared/<Action-Name>.cshtml
/Pages/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="89f8b-147">Klasörleri görüntüle konumunu ister *denetleyicileri* ve *modelleri* mu **değil** önemi.</span><span class="sxs-lookup"><span data-stu-id="89f8b-147">The location of non-view folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="89f8b-148">Örneğin, *denetleyicileri* ve *modelleri* klasörü gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="89f8b-148">For example, the *Controllers* and *Models* folder are not required.</span></span> <span data-ttu-id="89f8b-149">İçeriği *denetleyicileri* ve *modelleri* bir .dll derlenmiş kodudur.</span><span class="sxs-lookup"><span data-stu-id="89f8b-149">The content of *Controllers* and *Models* is code which gets compiled into a .dll.</span></span> <span data-ttu-id="89f8b-150">İçeriği *görünümleri* , görünüm için bir istek yayınlanana kadar derlenmiş değil.</span><span class="sxs-lookup"><span data-stu-id="89f8b-150">The content of the *Views* isn't compiled until a request to that view has been made.</span></span>

<!-- TODO review:
The content of the *Views* isn't compiled until a request to that view has been made.

What about precompiled views? 
 -->
<a name="attribute"></a>

### <a name="associate-the-controller-with-an-area"></a><span data-ttu-id="89f8b-151">Denetleyici ile bir alanı ilişkilendirin</span><span class="sxs-lookup"><span data-stu-id="89f8b-151">Associate the controller with an Area</span></span>

<span data-ttu-id="89f8b-152">Alanı denetleyicileri ile atanır [ &lbrack;alan&rbrack; ](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="89f8b-152">Area controllers are designated with the [&lbrack;Area&rbrack;](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute:</span></span>

[!code-csharp[](areas/samples/MVCareas/Areas/Products/Controllers/ManageController.cs?highlight=5&name=snippet)]

### <a name="add-area-route"></a><span data-ttu-id="89f8b-153">Alan yolu ekleme</span><span class="sxs-lookup"><span data-stu-id="89f8b-153">Add Area route</span></span>

<span data-ttu-id="89f8b-154">Alan yolları, genellikle geleneksel özniteliği yerine yönlendirmesi kullanır.</span><span class="sxs-lookup"><span data-stu-id="89f8b-154">Area routes typically use conventional routing rather than attribute routing.</span></span> <span data-ttu-id="89f8b-155">Geleneksel yönlendirme sipariş bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="89f8b-155">Conventional routing is order-dependent.</span></span> <span data-ttu-id="89f8b-156">Genel olarak, bunlar olmadan bir alan yolları daha ayrıntılı olarak alanları rotalarla önceki rota tablosunda yerleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="89f8b-156">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="89f8b-157">`{area:...}` URL alanı tüm alanlar arasında Tekdüzen ise, rota şablonlarındaki bir belirteci olarak kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="89f8b-157">`{area:...}` can be used as a token in route templates if url space is uniform across all areas:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup.cs?name=snippet&highlight=18-21)]

<span data-ttu-id="89f8b-158">Önceki kodda, `exists` yol alanı eşleşmelidir kısıtlama uygular.</span><span class="sxs-lookup"><span data-stu-id="89f8b-158">In the preceding code, `exists` applies a constraint that the route must match an area.</span></span> <span data-ttu-id="89f8b-159">Kullanarak `{area:...}` alanlarına yönlendirme eklemeye az karmaşık mekanizmadır.</span><span class="sxs-lookup"><span data-stu-id="89f8b-159">Using `{area:...}` is the least complicated mechanism to adding routing to areas.</span></span>

<span data-ttu-id="89f8b-160">Aşağıdaki kod <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> alan yolları adlı iki oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="89f8b-160">The following code uses <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute*> to create two named area routes:</span></span>

[!code-csharp[](areas/samples/MVCareas/StartupMapAreaRoute.cs?name=snippet&highlight=18-27)]

<span data-ttu-id="89f8b-161">Kullanırken `MapAreaRoute` bkz: ASP.NET Core 2.2 ile [bu GitHub sorunu](https://github.com/aspnet/AspNetCore/issues/7772).</span><span class="sxs-lookup"><span data-stu-id="89f8b-161">When using `MapAreaRoute` with ASP.NET Core 2.2, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/7772).</span></span>

<span data-ttu-id="89f8b-162">Daha fazla bilgi için [alan yönlendirme](xref:mvc/controllers/routing#areas).</span><span class="sxs-lookup"><span data-stu-id="89f8b-162">For more information, see [Area routing](xref:mvc/controllers/routing#areas).</span></span>

### <a name="link-generation-with-areas"></a><span data-ttu-id="89f8b-163">Alanları ile bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="89f8b-163">Link Generation with Areas</span></span>

<span data-ttu-id="89f8b-164">Aşağıdaki kodu [örnek indirme](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) gösterir, belirtilen alanla nesil bağlantı:</span><span class="sxs-lookup"><span data-stu-id="89f8b-164">The following code from the [sample download](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/areas/samples) shows link generation with the area specified:</span></span>

[!code-cshtml[](areas/samples/MVCareas/Views/Shared/_testLinksPartial.cshtml?name=snippet)]

<span data-ttu-id="89f8b-165">Yukarıdaki kod ile oluşturulan herhangi bir uygulamada geçerli bağlantılardır.</span><span class="sxs-lookup"><span data-stu-id="89f8b-165">The links generated with the preceding code are valid anywhere in the app.</span></span>

<span data-ttu-id="89f8b-166">Örnek indirme içeren bir [kısmi Görünüm](xref:mvc/views/partial) içeren önceki bağlantıları ve aynı bağlantı alanı belirtmeden.</span><span class="sxs-lookup"><span data-stu-id="89f8b-166">The sample download includes a [partial view](xref:mvc/views/partial) that contains the preceding links and the same links without specifying the area.</span></span> <span data-ttu-id="89f8b-167">Kısmi görünüm başvurulduğundan [Düzen dosyası](), uygulamayı her sayfa oluşturulan bağlantıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="89f8b-167">The partial view is referenced in the [layout file](), so every page in the app displays the generated links.</span></span> <span data-ttu-id="89f8b-168">Alanı belirtmeden oluşturulan bağlantıları, yalnızca bir sayfa aynı alan ve denetleyici başvurulduğunda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="89f8b-168">The links generated without specifying the area are only valid when referenced from a page in the same area and controller.</span></span>

<span data-ttu-id="89f8b-169">Alan veya denetleyici belirtilmediğinde yönlendirme bağlıdır *ortam* değerleri.</span><span class="sxs-lookup"><span data-stu-id="89f8b-169">When the area or controller is not specified, routing depends on the *ambient* values.</span></span> <span data-ttu-id="89f8b-170">Geçerli isteğin geçerli rota değerleri için bağlantı oluşturma ortam değerleri olarak kabul edilir.</span><span class="sxs-lookup"><span data-stu-id="89f8b-170">The current route values of the current request are considered ambient values for link generation.</span></span> <span data-ttu-id="89f8b-171">Örnek uygulama için çoğu durumda, ortam değerleri kullanarak yanlış bağlantılar oluşturur.</span><span class="sxs-lookup"><span data-stu-id="89f8b-171">In many cases for the sample app, using the ambient values generates incorrect links.</span></span>

<span data-ttu-id="89f8b-172">Daha fazla bilgi için [denetleyici eylemlerine yönlendirme](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="89f8b-172">For more information, see [Routing to controller actions](xref:mvc/controllers/routing).</span></span>

### <a name="shared-layout-for-areas-using-the-viewstartcshtml-file"></a><span data-ttu-id="89f8b-173">Düzen _ViewStart.cshtml dosyasını kullanarak alanları için paylaşılan</span><span class="sxs-lookup"><span data-stu-id="89f8b-173">Shared layout for Areas using the _ViewStart.cshtml file</span></span>

<span data-ttu-id="89f8b-174">Uygulamanın tamamında için yaygın bir düzen paylaşmak için taşıma *_ViewStart.cshtml* uygulama kök klasörüne.</span><span class="sxs-lookup"><span data-stu-id="89f8b-174">To share a common layout for the entire app, move the *_ViewStart.cshtml* to the application root folder.</span></span>

<!-- This section will be completed after https://github.com/aspnet/Docs/pull/10978 is merged.
<a name="arp"></a>

## Areas for Razor Pages
-->
<a name="rename"></a>

### <a name="change-default-area-folder-where-views-are-stored"></a><span data-ttu-id="89f8b-175">Görünümleri depolandığı varsayılan alanı klasörü Değiştir</span><span class="sxs-lookup"><span data-stu-id="89f8b-175">Change default area folder where views are stored</span></span>

<span data-ttu-id="89f8b-176">Varsayılan alan klasöründen aşağıdaki kod değişiklikleri `"Areas"` için `"MyAreas"`:</span><span class="sxs-lookup"><span data-stu-id="89f8b-176">The following code changes the default area folder from `"Areas"` to `"MyAreas"`:</span></span>

[!code-csharp[](areas/samples/MVCareas/Startup2.cs?name=snippet)]

<!-- TODO review - can we delete this. Areas doesn't change publishing - right? -->
### <a name="publishing-areas"></a><span data-ttu-id="89f8b-177">Yayımlama alanları</span><span class="sxs-lookup"><span data-stu-id="89f8b-177">Publishing Areas</span></span>

<span data-ttu-id="89f8b-178">Tüm `*.cshtml` ve `wwwroot/**` dosyaları ne zaman çıkış yayımlanır `<Project Sdk="Microsoft.NET.Sdk.Web">` dahil *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="89f8b-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
