---
title: ASP.NET Core MVC görünümleri
author: ardalis
description: Uygulamanın verilerini sunumu ve kullanıcı etkileşimine karşılık olarak ASP.NET Core MVC görünümleri nasıl işleneceğini öğrenin.
ms.author: riande
ms.date: 04/03/2019
uid: mvc/views/overview
ms.openlocfilehash: 766996645bc6ef2b6be42d729baf5d57f55b6ddd
ms.sourcegitcommit: 1a7000630e55da90da19b284e1b2f2f13a393d74
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59012805"
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="6b83c-103">ASP.NET Core MVC görünümleri</span><span class="sxs-lookup"><span data-stu-id="6b83c-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="6b83c-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6b83c-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6b83c-105">Bu belgede, ASP.NET Core MVC uygulamalarında kullanılan görünümler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="6b83c-106">Razor sayfaları hakkında daha fazla bilgi için bkz: [Razor sayfaları giriş](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="6b83c-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:razor-pages/index).</span></span>

<span data-ttu-id="6b83c-107">Model-View-Controller (MVC) düzeninde *görünümü* uygulamanın verilerini sunumu ve kullanıcı etkileşimi işler.</span><span class="sxs-lookup"><span data-stu-id="6b83c-107">In the Model-View-Controller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="6b83c-108">Bir görünümü HTML şablonudur ile katıştırılmış [Razor işaretlemesi](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="6b83c-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="6b83c-109">Razor işaretlemesi istemciye gönderilen bir Web sayfası oluşturmak için bir HTML biçimlendirmesi ile etkileşime giren kodudur.</span><span class="sxs-lookup"><span data-stu-id="6b83c-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="6b83c-110">ASP.NET Core MVC görünümleri olan *.cshtml* dosyaları kullanan [C# programlama dilinin](/dotnet/csharp/) Razor biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="6b83c-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="6b83c-111">Genellikle, her uygulama için klasörleri dosyalarını görüntüle gruplanmıştır [denetleyicileri](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="6b83c-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="6b83c-112">Klasörleri depolanan bir *görünümleri* uygulamanın kök klasörü:</span><span class="sxs-lookup"><span data-stu-id="6b83c-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Visual Studio Çözüm Gezgini görünümlerini klasörü giriş klasörünün About.cshtml Contact.cshtml ve Index.cshtml dosyaları göstermek için açık açıktır](overview/_static/views_solution_explorer.png)

<span data-ttu-id="6b83c-114">*Giriş* denetleyici tarafından temsil edilen bir *giriş* klasörün içine *görünümleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="6b83c-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="6b83c-115">*Giriş* klasörde görünümleri *hakkında*, *kişi*, ve *dizin* (giriş) Web sayfaları.</span><span class="sxs-lookup"><span data-stu-id="6b83c-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="6b83c-116">Bir kullanıcı istediğinde bu üç Web sayfaları, denetleyici eylemleri birini *giriş* denetleyicisini belirleyin, üç görünüm oluşturun ve kullanıcıya bir Web sayfasını döndürmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="6b83c-117">Kullanım [düzenleri](xref:mvc/views/layout) tutarlı Web Bölümleri sağlamak ve kod tekrarından azaltın.</span><span class="sxs-lookup"><span data-stu-id="6b83c-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="6b83c-118">Düzenleri, genellikle üst bilgi, gezinti ve menü öğeleri ve alt bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="6b83c-119">Genellikle üstbilgi ve altbilgi Demirbaş biçimlendirme çok sayıda meta veri öğeleri ve betik, stil ve varlıklar bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="6b83c-120">Düzenleri, bu ortak biçimlendirme, görünümlerde önlemenize yardımcı.</span><span class="sxs-lookup"><span data-stu-id="6b83c-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="6b83c-121">[Kısmi görünümler](xref:mvc/views/partial) görünümleri yeniden kullanılabilir bölümlerini yöneterek kod yinelemesinden azaltın.</span><span class="sxs-lookup"><span data-stu-id="6b83c-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="6b83c-122">Örneğin, kısmi görünüm, birden fazla görünümlerde bir blog Web sitesinde bir yazar biyografisi yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="6b83c-123">Yazar biyografisi normal görünüm içeriği ve Web sayfasının içeriğini üretmek için yürütmek için kod gerekmez.</span><span class="sxs-lookup"><span data-stu-id="6b83c-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="6b83c-124">Yazar biyografisi içeriği görünüm tarafından tek başına, model bağlama için kullanılabilir olduğundan kısmi bir görünümü kullanarak bu içerik türü için idealdir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="6b83c-125">[Görüntüleme bileşenleri](xref:mvc/views/view-components) tekrarlanan kodları azaltabilir size izin vermesi için kısmi benzer görünümleridir, ancak bunlar Web sayfasını işlemek için sunucuda çalıştırmak için kod gerektiren görünümü içeriğini uygun.</span><span class="sxs-lookup"><span data-stu-id="6b83c-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="6b83c-126">Görünüm bileşenleri işlenmiş içeriği gerektiren alışveriş sepeti bir Web sitesi için veritabanı etkileşimi gibi olduğunda yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="6b83c-127">Görünüm bileşenleri Web sayfası çıktı oluşturmak için model bağlama için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="6b83c-128">Görünümleri kullanmanın avantajları</span><span class="sxs-lookup"><span data-stu-id="6b83c-128">Benefits of using views</span></span>

<span data-ttu-id="6b83c-129">Görünümleri yardımcı kurmak için [görev ayrımı nettir](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) uygulamanın diğer bölümlerden kullanıcı arabirimi biçimlendirme ayırarak bir MVC uygulaması içinde.</span><span class="sxs-lookup"><span data-stu-id="6b83c-129">Views help to establish [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="6b83c-130">SoC tasarım aşağıdaki uygulamanızı çeşitli avantajlar sağlayan modüler, yapar:</span><span class="sxs-lookup"><span data-stu-id="6b83c-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="6b83c-131">Uygulama, daha iyi organize çünkü korumak daha kolay olur.</span><span class="sxs-lookup"><span data-stu-id="6b83c-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="6b83c-132">Görünümler, genel olarak app özelliği tarafından gruplandırılır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="6b83c-133">Bu, bir özellik üzerinde çalışırken ilgili görünümleri bulmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="6b83c-134">Uygulama bölümleri birbirine sıkı şekilde bağlı.</span><span class="sxs-lookup"><span data-stu-id="6b83c-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="6b83c-135">Derleme ve iş mantığı ve verileri erişim bileşenleri ayrı olarak uygulamanın görünümleri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="6b83c-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="6b83c-136">Uygulama görünümlerini mutlaka uygulamanın diğer bölümlerini güncelleştirmek zorunda kalmadan değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b83c-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="6b83c-137">Uygulama kullanıcı arabirimi bölümlerini görünümleri ayrı birim olduğundan test etmek daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="6b83c-138">Daha iyi düzenleme nedeniyle yanlışlıkla kullanıcı arabirimi bölümlerini tekrarlayacaksınız daha az olasıdır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-138">Due to better organization, it's less likely that you'll accidentally repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="6b83c-139">Bir görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="6b83c-139">Creating a view</span></span>

<span data-ttu-id="6b83c-140">Bir denetleyiciye özgü görünümler oluşturulur *görünümler / [ControllerName]* klasör.</span><span class="sxs-lookup"><span data-stu-id="6b83c-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="6b83c-141">Görünümleri ve denetleyicileri arasında paylaşılan yerleştirildiğinde *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="6b83c-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="6b83c-142">Bir görünüm oluşturmak için yeni bir dosya ekleyin ve ilişkili denetleyici eylemi ile aynı adı verin *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="6b83c-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="6b83c-143">Karşılık gelen bir görünüm oluşturmak için *hakkında* eylemde *giriş* denetleyicisi oluşturma bir *About.cshtml* dosyası *görünümler/giriş*klasörü:</span><span class="sxs-lookup"><span data-stu-id="6b83c-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="6b83c-144">*Razor* biçimlendirme ile başlayan `@` simgesi.</span><span class="sxs-lookup"><span data-stu-id="6b83c-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="6b83c-145">C# yerleştirerek çalıştırma C# deyimleri kod içinde [Razor kod blokları](xref:mvc/views/razor#razor-code-blocks) küme ayraçları ayarlayın (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="6b83c-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="6b83c-146">Örneğin, "Hakkında" ataması için bkz: `ViewData["Title"]` yukarıda gösterilen.</span><span class="sxs-lookup"><span data-stu-id="6b83c-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="6b83c-147">Değeriyle başvurarak HTML'de değerleri gösterebilen `@` simgesi.</span><span class="sxs-lookup"><span data-stu-id="6b83c-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="6b83c-148">İçeriğini görmek `<h2>` ve `<h3>` yukarıdaki öğeler.</span><span class="sxs-lookup"><span data-stu-id="6b83c-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="6b83c-149">Yukarıda gösterilen içeriği görüntüleme, kullanıcı tarafından işlenen tüm Web sayfasının yalnızca bir parçası olur.</span><span class="sxs-lookup"><span data-stu-id="6b83c-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="6b83c-150">Sayfa düzeninin geri kalan ve görünüm ortak diğer yönleri diğer görünüm dosyalarında belirtilir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="6b83c-151">Daha fazla bilgi için bkz. [Düzen konu](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="6b83c-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="6b83c-152">Denetleyicileri görünümlerin nasıl belirtin</span><span class="sxs-lookup"><span data-stu-id="6b83c-152">How controllers specify views</span></span>

<span data-ttu-id="6b83c-153">Görünüm eylemlerine genellikle döndürülür bir [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), bir tür olduğu [actionresult öğesini](/dotnet/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="6b83c-153">Views are typically returned from actions as a [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="6b83c-154">Eylem yönteminizi oluşturabilir ve dönüş bir `ViewResult` doğrudan, ancak değil, yaygın olarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="6b83c-155">Çoğu denetleyicileri devralınacak beri [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller), yalnızca kullandığınız `View` döndürmek için yardımcı yöntem `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="6b83c-155">Since most controllers inherit from [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

*<span data-ttu-id="6b83c-156">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="6b83c-156">HomeController.cs</span></span>*

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="6b83c-157">Bu eylem geri döndüğünde, *About.cshtml* son bölümde gösterilen görünümü, aşağıdaki Web sayfası işlenir:</span><span class="sxs-lookup"><span data-stu-id="6b83c-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Edge tarayıcısında işlenen sayfası hakkında](overview/_static/about-page.png)

<span data-ttu-id="6b83c-159">`View` Yardımcı yöntem olan çeşitli aşırı yükler.</span><span class="sxs-lookup"><span data-stu-id="6b83c-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="6b83c-160">İsteğe bağlı olarak belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6b83c-160">You can optionally specify:</span></span>

* <span data-ttu-id="6b83c-161">Döndürülecek açık bir görünümü:</span><span class="sxs-lookup"><span data-stu-id="6b83c-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```

* <span data-ttu-id="6b83c-162">A [modeli](xref:mvc/models/model-binding) görünüme iletmek için:</span><span class="sxs-lookup"><span data-stu-id="6b83c-162">A [model](xref:mvc/models/model-binding) to pass to the view:</span></span>

  ```csharp
  return View(Orders);
  ```

* <span data-ttu-id="6b83c-163">Bir görünümü ve bir model için:</span><span class="sxs-lookup"><span data-stu-id="6b83c-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="6b83c-164">Görünüm bulma</span><span class="sxs-lookup"><span data-stu-id="6b83c-164">View discovery</span></span>

<span data-ttu-id="6b83c-165">Görünüm bir eylemi geri döndüğünde, adlandırılan bir işlem *görünümü bulma* gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="6b83c-166">Bu işlem, hangi görünüm dosyası görünüm adını temel alarak kullanıldığını belirler.</span><span class="sxs-lookup"><span data-stu-id="6b83c-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="6b83c-167">Varsayılan davranışını `View` yöntemi (`return View();`) içinden çağırıldığında eylem yöntemi olarak aynı ada sahip bir görünüm getirmektir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="6b83c-168">Örneğin, *hakkında* `ActionResult` denetleyicinin yöntemi adı adlı bir görünüm dosyayı aramak için kullanılan *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6b83c-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="6b83c-169">İlk olarak, çalışma zamanı arar *görünümler / [ControllerName]* klasör görünümü.</span><span class="sxs-lookup"><span data-stu-id="6b83c-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="6b83c-170">Eşleşen görünüm yok bulamazsa, aradığı *paylaşılan* klasör görünümü.</span><span class="sxs-lookup"><span data-stu-id="6b83c-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="6b83c-171">Dolaylı olarak dönüş yaparsa farketmez `ViewResult` ile `return View();` veya Görünüm adına açıkça geçirebilirsiniz `View` yöntemiyle `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="6b83c-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="6b83c-172">Her iki durumda da Görünüm bulma eşleşen bir görünüm dosyası şu sırayla arar:</span><span class="sxs-lookup"><span data-stu-id="6b83c-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. *<span data-ttu-id="6b83c-173">Görünümler /\[ControllerName] /\[Görünümadı] .cshtml</span><span class="sxs-lookup"><span data-stu-id="6b83c-173">Views/\[ControllerName]/\[ViewName].cshtml</span></span>*
   1. *<span data-ttu-id="6b83c-174">Görünümler/paylaşılan/\[Görünümadı] .cshtml</span><span class="sxs-lookup"><span data-stu-id="6b83c-174">Views/Shared/\[ViewName].cshtml</span></span>*

<span data-ttu-id="6b83c-175">Bir görünümü dosya yolu yerine bir görünüm adı sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="6b83c-176">Uygulama kökü başlatılıyor mutlak yol kullanıyorsanız (isteğe bağlı olarak başlayarak "/" veya "~ /"), *.cshtml* uzantı belirtilmelidir:</span><span class="sxs-lookup"><span data-stu-id="6b83c-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="6b83c-177">Görünümler farklı dizinlerde belirtmek için göreli bir yol kullanabilirsiniz *.cshtml* uzantısı.</span><span class="sxs-lookup"><span data-stu-id="6b83c-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="6b83c-178">İçinde `HomeController`, geri dönebilirsiniz *dizin* görünümünü, *Yönet* göreli yolu içeren görünümler:</span><span class="sxs-lookup"><span data-stu-id="6b83c-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="6b83c-179">Benzer şekilde, geçerli denetleyici özgü dizinle belirtebilir ". /" ön eki:</span><span class="sxs-lookup"><span data-stu-id="6b83c-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="6b83c-180">[Kısmi görünümler](xref:mvc/views/partial) ve [görüntülemek bileşenleri](xref:mvc/views/view-components) benzer (ancak aynı değildir) bulma mekanizmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="6b83c-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="6b83c-181">Nasıl görünümleri kullanarak özel bir uygulama içinde bulunan varsayılan kural özelleştirebilirsiniz [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="6b83c-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="6b83c-182">Görünüm bulma görünümü dosyaları dosya adına göre bulma üzerinde kullanır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="6b83c-183">Temeldeki dosya sistemi büyük/küçük harfe duyarlı ise, görünüm adları büyük olasılıkla büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="6b83c-184">İşletim sistemleri arasında uyumluluk için denetleyici ve eylem adları ve ilişkili görünüme klasörleri ve dosya adları arasında harf.</span><span class="sxs-lookup"><span data-stu-id="6b83c-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="6b83c-185">Bulunamayan bir görünüm dosyası büyük küçük harfe duyarlı dosya sistemi ile çalışırken bir hatayla karşılaşırsanız, büyük/küçük harf gerçek görünümü dosya adı ve istenen görünüm dosyası arasında eşleştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6b83c-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="6b83c-186">Denetleyicileri, işlemler ve Bakım ve açıklık için görünümler arasındaki ilişkiyi yansıtmak kendi görünümlerinizi dosya yapısını düzenleme en iyi izleyin.</span><span class="sxs-lookup"><span data-stu-id="6b83c-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="6b83c-187">Görünüm veri geçirme</span><span class="sxs-lookup"><span data-stu-id="6b83c-187">Passing data to views</span></span>

<span data-ttu-id="6b83c-188">Veri çeşitli yaklaşımlar kullanılarak geçer:</span><span class="sxs-lookup"><span data-stu-id="6b83c-188">Pass data to views using several approaches:</span></span>

* <span data-ttu-id="6b83c-189">Veri türü kesin: viewmodel</span><span class="sxs-lookup"><span data-stu-id="6b83c-189">Strongly typed data: viewmodel</span></span>
* <span data-ttu-id="6b83c-190">Zayıf sahipli türü belirtilmiş veri</span><span class="sxs-lookup"><span data-stu-id="6b83c-190">Weakly typed data</span></span>
  * `ViewData` <span data-ttu-id="6b83c-191">(`ViewDataAttribute`)</span><span class="sxs-lookup"><span data-stu-id="6b83c-191">(`ViewDataAttribute`)</span></span>
  * `ViewBag`

### <a name="strongly-typed-data-viewmodel"></a><span data-ttu-id="6b83c-192">Kesin türü belirtilmiş veri (viewmodel)</span><span class="sxs-lookup"><span data-stu-id="6b83c-192">Strongly typed data (viewmodel)</span></span>

<span data-ttu-id="6b83c-193">Belirtmek için en güçlü yaklaşımdır bir [modeli](xref:mvc/models/model-binding) görünümünde türü.</span><span class="sxs-lookup"><span data-stu-id="6b83c-193">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="6b83c-194">Bu model, genellikle olarak adlandırılır bir *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="6b83c-194">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="6b83c-195">Eylem görünümü viewmodel türün bir örneğini geçirin.</span><span class="sxs-lookup"><span data-stu-id="6b83c-195">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="6b83c-196">Bir görünüme veri iletmek için bir viewmodel kullanarak yararlanmak görünüm sağlayan *güçlü* tür denetimi.</span><span class="sxs-lookup"><span data-stu-id="6b83c-196">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="6b83c-197">*Güçlü yazım, yazım* (veya *kesin tür belirtilmiş*) her değişken ve sabit açıkça tanımlanmış bir tür olduğu anlamına gelir (örneğin, `string`, `int`, veya `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="6b83c-197">*Strong typing* (or *strongly typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="6b83c-198">Bir görünümde kullanılan türler geçerliliğini derleme sırasında denetlenir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-198">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="6b83c-199">[Visual Studio](https://www.visualstudio.com/vs/) ve [Visual Studio Code](https://code.visualstudio.com/) denilen bir özelliği kullanarak türü kesin belirlenmiş sınıf üyelerini listeleyin [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="6b83c-199">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="6b83c-200">Bir viewmodel özelliklerini görmek istediğinizde, bir nokta viewmodel değişken adını yazın (`.`).</span><span class="sxs-lookup"><span data-stu-id="6b83c-200">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="6b83c-201">Bu kod, daha az hatayla daha hızlı yazmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="6b83c-201">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="6b83c-202">Kullanarak bir model belirtin `@model` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="6b83c-202">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="6b83c-203">Modelinizle `@Model`:</span><span class="sxs-lookup"><span data-stu-id="6b83c-203">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="6b83c-204">Modeli görünüme sağlamak için denetleyici bu parametre olarak geçirir:</span><span class="sxs-lookup"><span data-stu-id="6b83c-204">To provide the model to the view, the controller passes it as a parameter:</span></span>

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

<span data-ttu-id="6b83c-205">Bir görünüm sağlayabilir model türleri ilgili bir kısıtlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="6b83c-205">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="6b83c-206">Düz eski CLR nesnesi (POCO) viewmodel'lar tanımlanan çok az kayıpla veya hiç davranışı ile (yöntem) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="6b83c-206">We recommend using Plain Old CLR Object (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="6b83c-207">Genellikle, viewmodel sınıfları ya da depolanan *modelleri* klasör veya ayrı bir *Viewmodel'lar* uygulamanın kök klasör.</span><span class="sxs-lookup"><span data-stu-id="6b83c-207">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="6b83c-208">*Adresi* yukarıdaki örnekte kullanılan viewmodel olan adındaki bir dosyada depolanan bir POCO viewmodel *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b83c-208">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

<span data-ttu-id="6b83c-209">Hiçbir şey viewmodel türleri ve iş modeli türleriniz için aynı sınıf kullanmasını önler.</span><span class="sxs-lookup"><span data-stu-id="6b83c-209">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="6b83c-210">Ancak, ayrı modelleri kullanarak görünümlerinizde uygulamanızın bölümlerini erişim iş mantığı ve verileri bağımsız olarak değiştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b83c-210">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="6b83c-211">Modelleri ve viewmodel'lar ayrımı sunduğu güvenlik açısından faydalı modelleri kullandığınızda [model bağlama](xref:mvc/models/model-binding) ve [doğrulama](xref:mvc/models/validation) uygulamaya kullanıcı tarafından gönderilen veriler için.</span><span class="sxs-lookup"><span data-stu-id="6b83c-211">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>

<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-viewdata-attribute-and-viewbag"></a><span data-ttu-id="6b83c-212">Veri'zayıf yazılmış (ViewData, ViewData özniteliği ve ViewBag)</span><span class="sxs-lookup"><span data-stu-id="6b83c-212">Weakly typed data (ViewData, ViewData attribute, and ViewBag)</span></span>

`ViewBag` *<span data-ttu-id="6b83c-213">Razor sayfaları içinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="6b83c-213">isn't available in Razor Pages.</span></span>*

<span data-ttu-id="6b83c-214">Kesin türü belirtilmiş görünümler yanı sıra görünümleri erişimi bir *zayıf yazılmış* (olarak da adlandırılan *gevşek yazılmış*) veri koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="6b83c-214">In addition to strongly typed views, views have access to a *weakly typed* (also called *loosely typed*) collection of data.</span></span> <span data-ttu-id="6b83c-215">Tanımlayıcı türlerinin aksine *zayıf türlerine* (veya *kaybetmiş türleri*) kullandığınız veri türünü açıkça bildirmeyin anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-215">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="6b83c-216">Küçük miktarlarda veri denetleyicilerine ve görünümleri içine ve dışına geçirmek için zayıf sahipli türü belirtilmiş veri koleksiyonunu kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6b83c-216">You can use the collection of weakly typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="6b83c-217">Arasında veri geçirme bir...</span><span class="sxs-lookup"><span data-stu-id="6b83c-217">Passing data between a ...</span></span>                        | <span data-ttu-id="6b83c-218">Örnek</span><span class="sxs-lookup"><span data-stu-id="6b83c-218">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="6b83c-219">Denetleyici ve bir görünüm</span><span class="sxs-lookup"><span data-stu-id="6b83c-219">Controller and a view</span></span>                             | <span data-ttu-id="6b83c-220">Bir açılan listedeki verilerle dolduruluyor.</span><span class="sxs-lookup"><span data-stu-id="6b83c-220">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="6b83c-221">Görünüm ve [görünümü](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="6b83c-221">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="6b83c-222">Ayarı  **\<başlığı >** düzeni görünümünde bir görünüm dosyasından öğe içeriği.</span><span class="sxs-lookup"><span data-stu-id="6b83c-222">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="6b83c-223">[Kısmi Görünüm](xref:mvc/views/partial) ve bir görünüm</span><span class="sxs-lookup"><span data-stu-id="6b83c-223">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="6b83c-224">Kullanıcının istenen Web sayfasına göre verileri görüntüleyen bir pencere öğesi.</span><span class="sxs-lookup"><span data-stu-id="6b83c-224">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="6b83c-225">Bu koleksiyonu aracılığıyla başvurulabilir `ViewData` veya `ViewBag` denetleyicileri ve görünümleri özellikleri.</span><span class="sxs-lookup"><span data-stu-id="6b83c-225">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="6b83c-226">`ViewData` Özelliktir zayıf yazılan nesnelerin bir sözlük.</span><span class="sxs-lookup"><span data-stu-id="6b83c-226">The `ViewData` property is a dictionary of weakly typed objects.</span></span> <span data-ttu-id="6b83c-227">`ViewBag` Özelliği çevresinde bir sarmalayıcı `ViewData` Dinamik özellikler için temel sağlayan `ViewData` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="6b83c-227">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span> <span data-ttu-id="6b83c-228">Not: Anahtar aramaları için hem de büyük küçük harf duyarsız `ViewData` ve `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="6b83c-228">Note: Key lookups are case-insensitive for both `ViewData` and `ViewBag`.</span></span>

`ViewData` <span data-ttu-id="6b83c-229">ve `ViewBag` çalışma zamanında dinamik olarak çözümlenir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-229">and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="6b83c-230">Derleme zamanı tür denetimini sundukları yoksa, her ikisi de genellikle daha bir viewmodel kullanmaktan hataya olduğundan.</span><span class="sxs-lookup"><span data-stu-id="6b83c-230">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="6b83c-231">Bu nedenle, en düşük düzeyde ya da hiç kullanmak bazı geliştiriciler tercih `ViewData` ve `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="6b83c-231">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>

<a name="VD"></a>

**<span data-ttu-id="6b83c-232">ViewData</span><span class="sxs-lookup"><span data-stu-id="6b83c-232">ViewData</span></span>**

`ViewData` <span data-ttu-id="6b83c-233">olan bir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) aracılığıyla erişilen nesne `string` anahtarları.</span><span class="sxs-lookup"><span data-stu-id="6b83c-233">is a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="6b83c-234">Dize verileri depolanır ve bir atama gerek kalmadan doğrudan kullanılır, ancak diğer dönüştürmelisiniz `ViewData` bunları ayıkladığınızda değerleri belirli türler için nesne.</span><span class="sxs-lookup"><span data-stu-id="6b83c-234">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="6b83c-235">Kullanabileceğiniz `ViewData` denetleyicilerinden, görünümlere ve görünümler içinde veri iletmek için [kısmi görünümler](xref:mvc/views/partial) ve [düzenleri](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="6b83c-235">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="6b83c-236">Bir karşılama ve kullanarak bir adres ilişkin değerleri ayarlayan bir örnek verilmiştir `ViewData` doğan:</span><span class="sxs-lookup"><span data-stu-id="6b83c-236">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

<span data-ttu-id="6b83c-237">Bir görünümdeki veriler ile çalışır:</span><span class="sxs-lookup"><span data-stu-id="6b83c-237">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

::: moniker range=">= aspnetcore-2.1"

**<span data-ttu-id="6b83c-238">ViewData özniteliği</span><span class="sxs-lookup"><span data-stu-id="6b83c-238">ViewData attribute</span></span>**

<span data-ttu-id="6b83c-239">Kullanan başka bir yaklaşım [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) olduğu [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="6b83c-239">Another approach that uses the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) is [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="6b83c-240">Denetleyicilerde veya modellerde Razor sayfası özelliklerini düzenlenmiş ile `[ViewData]` sözlükten yüklendi ve depolanan değerlerine sahip.</span><span class="sxs-lookup"><span data-stu-id="6b83c-240">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the dictionary.</span></span>

<span data-ttu-id="6b83c-241">Aşağıdaki örnekte giriş denetleyicisini içeren bir `Title` özelliği düzenlenmiş ile `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="6b83c-241">In the following example, the Home controller contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="6b83c-242">`About` Yöntemi hakkında görünümü başlığını ayarlar:</span><span class="sxs-lookup"><span data-stu-id="6b83c-242">The `About` method sets the title for the About view:</span></span>

```csharp
public class HomeController : Controller
{
    [ViewData]
    public string Title { get; set; }

    public IActionResult About()
    {
        Title = "About Us";
        ViewData["Message"] = "Your application description page.";

        return View();
    }
}
```

<span data-ttu-id="6b83c-243">Hakkında Görünümü'nde erişim `Title` özelliği model özelliği olarak:</span><span class="sxs-lookup"><span data-stu-id="6b83c-243">In the About view, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="6b83c-244">Düzende dışında ViewData sözlükten başlığını okuyun:</span><span class="sxs-lookup"><span data-stu-id="6b83c-244">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

**<span data-ttu-id="6b83c-245">Görünüm Paketi</span><span class="sxs-lookup"><span data-stu-id="6b83c-245">ViewBag</span></span>**

`ViewBag` *<span data-ttu-id="6b83c-246">Razor sayfaları içinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="6b83c-246">isn't available in Razor Pages.</span></span>*

`ViewBag` <span data-ttu-id="6b83c-247">olan bir [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) depolanan nesnelere dinamik erişim sağlayan nesne `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="6b83c-247">is a [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> `ViewBag` <span data-ttu-id="6b83c-248">atama gerektirmeyen bu yana birlikte çalışmak daha kullanışlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-248">can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="6b83c-249">Aşağıdaki örnek nasıl kullanılacağını gösterir `ViewBag` kullanarak aynı sonucu ile `ViewData` yukarıda:</span><span class="sxs-lookup"><span data-stu-id="6b83c-249">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**<span data-ttu-id="6b83c-250">ViewData ve ViewBag aynı anda kullanma</span><span class="sxs-lookup"><span data-stu-id="6b83c-250">Using ViewData and ViewBag simultaneously</span></span>**

`ViewBag` *<span data-ttu-id="6b83c-251">Razor sayfaları içinde kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="6b83c-251">isn't available in Razor Pages.</span></span>*

<span data-ttu-id="6b83c-252">Bu yana `ViewData` ve `ViewBag` aynı temel alınan bakın `ViewData` koleksiyonu, her ikisi de kullanabilirsiniz `ViewData` ve `ViewBag` karışımı ve bunlar arasında okurken ve yazarken değerleri aynı.</span><span class="sxs-lookup"><span data-stu-id="6b83c-252">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="6b83c-253">Başlığı kullanılarak ayarlanan `ViewBag` ve açıklaması kullanarak `ViewData` üst kısmında bir *About.cshtml* görüntüle:</span><span class="sxs-lookup"><span data-stu-id="6b83c-253">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="6b83c-254">Özelliklerini okumak istiyorum, ancak kullanımını ters `ViewData` ve `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="6b83c-254">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="6b83c-255">İçinde *_Layout.cshtml* dosya, başlığı kullanılarak elde `ViewData` ve açıklaması kullanarak elde `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="6b83c-255">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="6b83c-256">Dizeleri için bir tür dönüştürme gerektirmeyen unutmayın `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="6b83c-256">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="6b83c-257">Kullanabileceğiniz `@ViewData["Title"]` olmadan.</span><span class="sxs-lookup"><span data-stu-id="6b83c-257">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="6b83c-258">Her ikisini de kullanarak `ViewData` ve `ViewBag` adresindeki karıştırma ve okuma ve yazma özellikleri eşleştirme yoksa olarak aynı zaman çalışır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-258">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="6b83c-259">Aşağıdaki biçimlendirmede oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="6b83c-259">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**<span data-ttu-id="6b83c-260">ViewData ViewBag arasındaki farkları özeti</span><span class="sxs-lookup"><span data-stu-id="6b83c-260">Summary of the differences between ViewData and ViewBag</span></span>**

 `ViewBag` <span data-ttu-id="6b83c-261">Razor sayfaları kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="6b83c-261">isn't available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="6b83c-262">Öğesinden türetilen [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), gibi yararlı olabileceği sözlük özelliklerine sahip `ContainsKey`, `Add`, `Remove`, ve `Clear`.</span><span class="sxs-lookup"><span data-stu-id="6b83c-262">Derives from [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="6b83c-263">Sözlükteki anahtarların dizeleri olduğundan boşluğa izin.</span><span class="sxs-lookup"><span data-stu-id="6b83c-263">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="6b83c-264">Örnek:</span><span class="sxs-lookup"><span data-stu-id="6b83c-264">Example:</span></span> `ViewData["Some Key With Whitespace"]`
  * <span data-ttu-id="6b83c-265">Dışında herhangi türdeki bir `string` kullanmak için görünümünde dönüştürülmelidir `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="6b83c-265">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="6b83c-266">Öğesinden türetilen [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), böylece noktalı gösterim kullanılarak dinamik özellikler oluşturulmasını sağlar (`@ViewBag.SomeKey = <value or object>`), ve hiçbir atama gereklidir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-266">Derives from [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="6b83c-267">Söz dizimi `ViewBag` denetleyicileri ve görünümleri eklemek daha hızlı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-267">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="6b83c-268">Null değerler için basit.</span><span class="sxs-lookup"><span data-stu-id="6b83c-268">Simpler to check for null values.</span></span> <span data-ttu-id="6b83c-269">Örnek:</span><span class="sxs-lookup"><span data-stu-id="6b83c-269">Example:</span></span> `@ViewBag.Person?.Name`

**<span data-ttu-id="6b83c-270">ViewData veya ViewBag ne zaman kullanılacağını</span><span class="sxs-lookup"><span data-stu-id="6b83c-270">When to use ViewData or ViewBag</span></span>**

<span data-ttu-id="6b83c-271">Her ikisi de `ViewData` ve `ViewBag` eşit olarak küçük miktarlarda veri denetleyicilerine ve görünümleri arasında geçirmek için geçerli bir yaklaşım olan.</span><span class="sxs-lookup"><span data-stu-id="6b83c-271">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="6b83c-272">Hangisinin kullanılacağını Seçimi tercihine göre hesaplanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-272">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="6b83c-273">Karıştırın ve eşleşen `ViewData` ve `ViewBag` nesneleri, ancak kodu, okunması ve düzenlenmesi tutarlı bir şekilde kullanılan bir yaklaşım ile daha kolay.</span><span class="sxs-lookup"><span data-stu-id="6b83c-273">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="6b83c-274">Her iki yaklaşım, çalışma zamanında dinamik olarak çözümlenen ve çalışma zamanı hatalarına neden dolayısıyla saldırıya.</span><span class="sxs-lookup"><span data-stu-id="6b83c-274">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="6b83c-275">Bazı geliştirme ekipleri, bunların kaçının.</span><span class="sxs-lookup"><span data-stu-id="6b83c-275">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="6b83c-276">Dinamik görünümler</span><span class="sxs-lookup"><span data-stu-id="6b83c-276">Dynamic views</span></span>

<span data-ttu-id="6b83c-277">Bir model bildirmeyin görünümleri türü kullanarak `@model` ancak geçirilen bir model örneği varsa (örneğin, `return View(Address);`) örneğin özellikleri dinamik olarak başvurabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6b83c-277">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="6b83c-278">Bu özellik üst düzeyde esneklik sunar, ancak derleme koruma veya IntelliSense sunmaz.</span><span class="sxs-lookup"><span data-stu-id="6b83c-278">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="6b83c-279">Web sayfası oluşturma özelliği yoksa, çalışma zamanında başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="6b83c-279">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="6b83c-280">Daha fazla özelliklerini görüntüleyin</span><span class="sxs-lookup"><span data-stu-id="6b83c-280">More view features</span></span>

<span data-ttu-id="6b83c-281">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) sunucu tarafı davranışı mevcut HTML etiket eklemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-281">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="6b83c-282">Etiket Yardımcıları kullanarak özel kod veya kendi görünümlerinizi içinde Yardımcıları yazmak için gereksinimini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="6b83c-282">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="6b83c-283">Etiket Yardımcıları öznitelik olarak HTML öğeleri için uygulanır ve bunları işleyemiyor düzenleyiciler tarafından göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-283">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="6b83c-284">Bu, düzenlemek ve Araçlar çeşitli görünümü biçimlendirmesi oluşturmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6b83c-284">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="6b83c-285">Özel HTML biçimlendirme oluşturmak çok sayıda yerleşik HTML Yardımcıları ile gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-285">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="6b83c-286">Daha karmaşık kullanıcı arabirimi mantığı tarafından işlenebilir [görünüm bileşenleri](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="6b83c-286">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="6b83c-287">Görünüm bileşenleri aynı SoC bu denetleyicileri sağlayın ve görünümler sunar.</span><span class="sxs-lookup"><span data-stu-id="6b83c-287">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="6b83c-288">Bunlar, Eylemler ve ortak kullanıcı arabirim öğeleri tarafından kullanılan veri uğraşmanız görünümleri gereğini ortadan kaldırabilir.</span><span class="sxs-lookup"><span data-stu-id="6b83c-288">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="6b83c-289">Çok sayıda diğer yönleri ASP.NET Core gibi görünümleri desteği [bağımlılık ekleme](xref:fundamentals/dependency-injection), hizmetler sağlayan [görünümlere eklenmiş](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6b83c-289">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
