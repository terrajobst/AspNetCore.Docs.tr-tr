---
title: ASP.NET Core MVC görünümlerde
author: ardalis
description: Görünümler, uygulamanın veri sunumu ve ASP.NET Core MVC kullanıcı etkileşimini nasıl işleneceğini öğrenin.
ms.author: riande
ms.date: 12/12/2017
uid: mvc/views/overview
ms.openlocfilehash: 4d5cb6288711cdef145ebb0b52e4e645c535bdf2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278355"
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="87b06-103">ASP.NET Core MVC görünümlerde</span><span class="sxs-lookup"><span data-stu-id="87b06-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="87b06-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="87b06-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="87b06-105">Bu belgede ASP.NET Core MVC uygulamalarında kullanılan görünümler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="87b06-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="87b06-106">Razor sayfalarında daha fazla bilgi için bkz: [Razor sayfalarının giriş](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="87b06-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:razor-pages/index).</span></span>

<span data-ttu-id="87b06-107">Model-View-Controller (MVC) düzeninde *Görünüm* uygulamanın veri sunu ve kullanıcı etkileşimini işler.</span><span class="sxs-lookup"><span data-stu-id="87b06-107">In the Model-View-Controller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="87b06-108">Bir HTML şablonu görünümdür ile katıştırılmış [Razor biçimlendirme](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="87b06-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="87b06-109">Razor biçimlendirme istemciye gönderilen bir Web sayfası oluşturmak için HTML biçimlendirmesi etkileşimde kodudur.</span><span class="sxs-lookup"><span data-stu-id="87b06-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="87b06-110">ASP.NET Core MVC'de görünümlerdir *.cshtml* dosyaları kullanan [C# programlama dili](/dotnet/csharp/) Razor biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="87b06-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="87b06-111">Genellikle, her uygulamanın adlı klasörlere dosyaları görüntüle gruplandırılmış [denetleyicileri](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="87b06-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="87b06-112">Klasörleri depolanmış bir *görünümleri* uygulama kökünde klasör:</span><span class="sxs-lookup"><span data-stu-id="87b06-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Visual Studio Çözüm Gezgini görünümler klasöründe giriş klasörü About.cshtml, Contact.cshtml ve Index.cshtml dosyaları göstermek için açık açık](overview/_static/views_solution_explorer.png)

<span data-ttu-id="87b06-114">*Giriş* denetleyicisi ile temsil edilir bir *giriş* klasöre *görünümleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="87b06-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="87b06-115">*Giriş* görünümleri içeren klasör *hakkında*, *kişi*, ve *dizin* (giriş sayfası) Web sayfaları.</span><span class="sxs-lookup"><span data-stu-id="87b06-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="87b06-116">Bir kullanıcı isteğinde bulunduğunda bu üç Web sayfaları, denetleyici eylemleri birini *giriş* denetleyicisi belirlemek, üç görünüm oluşturmak ve bir Web sayfası kullanıcıya döndürmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="87b06-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="87b06-117">Kullanım [düzenleri](xref:mvc/views/layout) tutarlı Web sayfası bölümler sağlar ve kod yineleme azaltmak için.</span><span class="sxs-lookup"><span data-stu-id="87b06-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="87b06-118">Düzenleri genellikle üstbilgi, gezinme ve menü öğeleri ve alt bilgi içerir.</span><span class="sxs-lookup"><span data-stu-id="87b06-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="87b06-119">Üstbilgi ve altbilgi genellikle Demirbaş biçimlendirme çok sayıda meta veri öğeleri ve komut dosyası ve stil varlıklar bağlantılar içerir.</span><span class="sxs-lookup"><span data-stu-id="87b06-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="87b06-120">Düzenleri görünümlerinizi bu Demirbaş biçimlendirme önlemenize yardımcı.</span><span class="sxs-lookup"><span data-stu-id="87b06-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="87b06-121">[Kısmi görünümler](xref:mvc/views/partial) görünümleri yeniden kullanılabilir bölümlerini yöneterek kod yinelemesinden azaltın.</span><span class="sxs-lookup"><span data-stu-id="87b06-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="87b06-122">Örneğin, kısmi görünüm, birkaç görünümlerde bir blog Web sitesinde Yazar biyografisi yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="87b06-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="87b06-123">Yazar biyografisi sıradan görünüm içeriğin ve Web sayfasının içeriği üretmek için yürütmek için kodu gerektirmez.</span><span class="sxs-lookup"><span data-stu-id="87b06-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="87b06-124">Bu içerik türü için kısmi görünüm kullanılması idealdir şekilde yazar biyografisi içerik görünüm tek başına, model bağlama tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="87b06-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="87b06-125">[Görüntüleme bileşenleri](xref:mvc/views/view-components) yineleyen kod azaltmanızı izin vermesi benzeyen kısmi görünümler, ancak bu Web sayfası oluşturmak için sunucuda çalıştırmak için kod gerektiren görünümü içerik için uygun.</span><span class="sxs-lookup"><span data-stu-id="87b06-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="87b06-126">İşlenmiş içeriği gerektirdiğinde alışveriş sepeti bir Web sitesi için veritabanı etkileşimini gibi görünüm bileşenleri yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="87b06-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="87b06-127">Görünüm bileşenleri Web sayfası bir çıktı oluşturmak için bağlama modellemek için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="87b06-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="87b06-128">Görünümleri kullanmanın yararları</span><span class="sxs-lookup"><span data-stu-id="87b06-128">Benefits of using views</span></span>

<span data-ttu-id="87b06-129">Görünümleri Yardım kurmak için bir [sorunları ayırma (SoC) tasarım](http://deviq.com/separation-of-concerns/) kullanıcı arabirimi biçimlendirme uygulama diğer bölümlerden ayıran bir MVC uygulaması içindeki.</span><span class="sxs-lookup"><span data-stu-id="87b06-129">Views help to establish a [Separation of Concerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="87b06-130">SoC tasarım aşağıdaki uygulamanızı birkaç avantaj sağlayan modüler yapar:</span><span class="sxs-lookup"><span data-stu-id="87b06-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="87b06-131">Uygulama, daha iyi organize çünkü korumak kolaydır.</span><span class="sxs-lookup"><span data-stu-id="87b06-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="87b06-132">Görünümleri genellikle uygulama özelliği tarafından gruplandırılır.</span><span class="sxs-lookup"><span data-stu-id="87b06-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="87b06-133">Bu, bir özellik çalışırken ilgili görünümleri bulmayı kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="87b06-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="87b06-134">Uygulama bölümlerini gevşek.</span><span class="sxs-lookup"><span data-stu-id="87b06-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="87b06-135">Derleme ve uygulamanın görünümleri ayrı ayrı iş mantığı ve verileri erişim bileşenleri güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87b06-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="87b06-136">Uygulamanın diğer bölümleri güncelleştirmek mutlaka zorunda kalmadan uygulama görünümlerini değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87b06-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="87b06-137">Uygulama kullanıcı arabirimi bölümlerini görünümleri ayrı birimi olduğundan test daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="87b06-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="87b06-138">Daha iyi düzenleme nedeniyle kullanıcı arabiriminin yanlışlıkla yineleme bölümleri gerekir daha az olasıdır.</span><span class="sxs-lookup"><span data-stu-id="87b06-138">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="87b06-139">Bir görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="87b06-139">Creating a view</span></span>

<span data-ttu-id="87b06-140">Bir denetleyiciye özgü görünümler oluşturulan *görünümler / [controllername öğesi]* klasör.</span><span class="sxs-lookup"><span data-stu-id="87b06-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="87b06-141">Denetleyicileri arasında paylaşılan görünümleri yerleştirilir *görünümler/paylaşılan* klasör.</span><span class="sxs-lookup"><span data-stu-id="87b06-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="87b06-142">Bir görünüm oluşturmak için yeni bir dosya ekleyin ve onun ilişkili denetleyici eylemi ile aynı adı vermek *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="87b06-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="87b06-143">Karşılık gelen bir görünüm oluşturmak için *hakkında* eylemde *giriş* denetleyicisi oluşturma bir *About.cshtml* dosyasını *görünümler/giriş*klasörü:</span><span class="sxs-lookup"><span data-stu-id="87b06-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="87b06-144">*Razor* biçimlendirme ile başlayan `@` simgesi.</span><span class="sxs-lookup"><span data-stu-id="87b06-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="87b06-145">C# yerleştirerek çalıştırma C# deyimleri kod içinde [Razor kod blokları](xref:mvc/views/razor#razor-code-blocks) küme ayraçları ayarlayın (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="87b06-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="87b06-146">Örneğin, "Hakkında" atanması için bkz: `ViewData["Title"]` yukarıda gösterilen.</span><span class="sxs-lookup"><span data-stu-id="87b06-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="87b06-147">HTML içindeki değerleri değeriyle başvurarak görüntüleyebilirsiniz `@` simgesi.</span><span class="sxs-lookup"><span data-stu-id="87b06-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="87b06-148">İçeriğini görmek `<h2>` ve `<h3>` yukarıdaki öğeler.</span><span class="sxs-lookup"><span data-stu-id="87b06-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="87b06-149">Yukarıda gösterilen görünüm içeriği kullanıcı için işlenen tüm Web sayfası yalnızca bir parçası olur.</span><span class="sxs-lookup"><span data-stu-id="87b06-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="87b06-150">Sayfanın düzenini geri kalanı ve diğer genel görünümü yönlerini diğer görünüm dosyalarında belirtilmiş.</span><span class="sxs-lookup"><span data-stu-id="87b06-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="87b06-151">Daha fazla bilgi için bkz: [düzeni konu](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="87b06-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="87b06-152">Denetleyicileri görünümleri nasıl belirtin</span><span class="sxs-lookup"><span data-stu-id="87b06-152">How controllers specify views</span></span>

<span data-ttu-id="87b06-153">Görünümleri eylemlerine genellikle döndürülür bir [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), bir tür olduğu [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="87b06-153">Views are typically returned from actions as a [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/dotnet/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="87b06-154">Eylem yöntemi oluşturun ve dönüş bir `ViewResult` doğrudan, ancak değil, yaygın olarak yapılır.</span><span class="sxs-lookup"><span data-stu-id="87b06-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="87b06-155">Çoğu denetleyicileri devralınmalıdır beri [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller), yalnızca kullandığınız `View` dönmek için yardımcı yöntem `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="87b06-155">Since most controllers inherit from [Controller](/dotnet/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="87b06-156">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="87b06-156">*HomeController.cs*</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="87b06-157">Bu eylem geri döndüğünde, *About.cshtml* son bölümünde gösterilen görünüm aşağıdaki Web sayfası işlenen:</span><span class="sxs-lookup"><span data-stu-id="87b06-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Edge tarayıcısında çizilir sayfası hakkında](overview/_static/about-page.png)

<span data-ttu-id="87b06-159">`View` Yardımcı yöntem birçok aşırı yüklemeye sahip.</span><span class="sxs-lookup"><span data-stu-id="87b06-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="87b06-160">İsteğe bağlı olarak belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="87b06-160">You can optionally specify:</span></span>

* <span data-ttu-id="87b06-161">Döndürülecek açık bir görünümü:</span><span class="sxs-lookup"><span data-stu-id="87b06-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="87b06-162">A [modeli](xref:mvc/models/model-binding) görünüme iletmek için:</span><span class="sxs-lookup"><span data-stu-id="87b06-162">A [model](xref:mvc/models/model-binding) to pass to the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="87b06-163">Bir görünüm ve model için:</span><span class="sxs-lookup"><span data-stu-id="87b06-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="87b06-164">Görünüm bulma</span><span class="sxs-lookup"><span data-stu-id="87b06-164">View discovery</span></span>

<span data-ttu-id="87b06-165">Bir eylem bir görünüm döndürdüğünde adlı bir işlem *görünüm bulma* gerçekleşmesi.</span><span class="sxs-lookup"><span data-stu-id="87b06-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="87b06-166">Bu işlem, hangi görünüm dosyası görünüm adını temel alarak kullanıldığını belirler.</span><span class="sxs-lookup"><span data-stu-id="87b06-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="87b06-167">Varsayılan davranışını `View` yöntemi (`return View();`) içinden çağırıldığında eylem yöntemi aynı ada sahip bir görünüm getirmektir.</span><span class="sxs-lookup"><span data-stu-id="87b06-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="87b06-168">Örneğin, *hakkında* `ActionResult` denetleyicinin yöntemi adı adlı bir görünüm dosyayı aramak için kullanılan *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="87b06-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="87b06-169">İlk olarak, çalışma zamanı görünür *görünümler / [controllername öğesi]* görünümü için klasör.</span><span class="sxs-lookup"><span data-stu-id="87b06-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="87b06-170">Eşleşen bir görünüm vardır bulamazsa arama *paylaşılan* görünümü için klasör.</span><span class="sxs-lookup"><span data-stu-id="87b06-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="87b06-171">Örtük olarak döndürürse önemli değildir `ViewResult` ile `return View();` veya Görünüm adı açıkça geçirmek `View` yöntemiyle `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="87b06-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="87b06-172">Her iki durumda da, bu sırada eşleşen bir görünüm dosyası görünüm bulma arar:</span><span class="sxs-lookup"><span data-stu-id="87b06-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="87b06-173">*Görünümler /\[controllername öğesi] /\[Görünümadı] .cshtml*</span><span class="sxs-lookup"><span data-stu-id="87b06-173">*Views/\[ControllerName]/\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="87b06-174">*Görünümler/paylaşılan/\[Görünümadı] .cshtml*</span><span class="sxs-lookup"><span data-stu-id="87b06-174">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="87b06-175">Bir görünüm adı yerine bir görünüm dosya yolu sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="87b06-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="87b06-176">Uygulama kök dizininde başlayan mutlak bir yol kullanıyorsanız (isteğe bağlı olarak başlayarak "/" veya "~ /"), *.cshtml* uzantısı belirtilmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="87b06-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="87b06-177">Görünümler farklı dizinlerde belirtmek için göreli bir yol kullanabilirsiniz *.cshtml* uzantısı.</span><span class="sxs-lookup"><span data-stu-id="87b06-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="87b06-178">İçinde `HomeController`, geri dönebilirsiniz *dizin* görünümünü, *Yönet* göreli bir yol görünümlerle:</span><span class="sxs-lookup"><span data-stu-id="87b06-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="87b06-179">Benzer şekilde, geçerli denetleyici özgü dizinle belirtebilir ". /" öneki:</span><span class="sxs-lookup"><span data-stu-id="87b06-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="87b06-180">[Kısmi görünümler](xref:mvc/views/partial) ve [görüntülemek bileşenleri](xref:mvc/views/view-components) benzer (ancak aynı) bulma mekanizmasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="87b06-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="87b06-181">Nasıl görünümleri özel kullanarak uygulama içinde bulunur ve varsayılan kuralını özelleştirebilirsiniz [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="87b06-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="87b06-182">Görünüm bulma görünümü dosyaları dosya adına göre bulma kullanır.</span><span class="sxs-lookup"><span data-stu-id="87b06-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="87b06-183">Temeldeki dosya sistemi büyük küçük harfe duyarlı ise, görünüm adları büyük olasılıkla büyük küçük harf duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="87b06-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="87b06-184">İşletim sistemleri arasında uyumluluk için denetleyici ve eylem adları ve ilişkili görünüm klasörleri ve dosya adları arasında büyük küçük harf duyarlı.</span><span class="sxs-lookup"><span data-stu-id="87b06-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="87b06-185">Bulunamayan bir görünüm dosyası büyük küçük harfe duyarlı dosya sistemi ile çalışırken bir hatayla karşılaşırsanız, büyük küçük harf istenen görünüm dosyası ve gerçek görünümü dosya adı arasında eşleştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="87b06-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="87b06-186">Denetleyicileri, işlemler ve Bakım ve daha anlaşılır olması için görünümler arasındaki ilişkiyi yansıtmak kendi görünümlerinizi dosya yapısı düzenleme en iyi uygulama olarak izleyin.</span><span class="sxs-lookup"><span data-stu-id="87b06-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="87b06-187">Veri görünümleri geçirme</span><span class="sxs-lookup"><span data-stu-id="87b06-187">Passing data to views</span></span>

<span data-ttu-id="87b06-188">Veri çeşitli yaklaşımlar kullanılarak geçer:</span><span class="sxs-lookup"><span data-stu-id="87b06-188">Pass data to views using several approaches:</span></span>

* <span data-ttu-id="87b06-189">Kesin türü belirtilmiş veri: viewmodel</span><span class="sxs-lookup"><span data-stu-id="87b06-189">Strongly-typed data: viewmodel</span></span>
* <span data-ttu-id="87b06-190">Zayıf yazılmış veri</span><span class="sxs-lookup"><span data-stu-id="87b06-190">Weakly-typed data</span></span>
  - <span data-ttu-id="87b06-191">`ViewData` (`ViewDataAttribute`)</span><span class="sxs-lookup"><span data-stu-id="87b06-191">`ViewData` (`ViewDataAttribute`)</span></span>
  - `ViewBag`

### <a name="strongly-typed-data-viewmodel"></a><span data-ttu-id="87b06-192">Kesin türü belirtilmiş veri (viewmodel)</span><span class="sxs-lookup"><span data-stu-id="87b06-192">Strongly-typed data (viewmodel)</span></span>

<span data-ttu-id="87b06-193">En güçlü belirtmek için bir yaklaşımdır bir [modeli](xref:mvc/models/model-binding) görünümünde türü.</span><span class="sxs-lookup"><span data-stu-id="87b06-193">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="87b06-194">Bu model, genellikle olarak adlandırılır bir *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="87b06-194">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="87b06-195">Viewmodel türünün bir örneği eylemden görünümüne geçin.</span><span class="sxs-lookup"><span data-stu-id="87b06-195">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="87b06-196">Bir görünüme veri iletmek için bir viewmodel kullanarak sağlar yararlanmak için görünümü *güçlü* türü denetleniyor.</span><span class="sxs-lookup"><span data-stu-id="87b06-196">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="87b06-197">*Güçlü yazarak* (veya *kesin türü belirtilmiş*) her değişken ve sabiti açıkça tanımlanmış bir türü olduğu anlamına gelir (örneğin, `string`, `int`, veya `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="87b06-197">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="87b06-198">Bir görünümde kullanılan türleri geçerliliğini derleme zamanında denetlenir.</span><span class="sxs-lookup"><span data-stu-id="87b06-198">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="87b06-199">[Visual Studio](https://www.visualstudio.com/vs/) ve [Visual Studio Code](https://code.visualstudio.com/) denilen bir özelliği kullanarak türü kesin belirlenmiş sınıf üyeleri liste [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="87b06-199">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="87b06-200">Bir viewmodel özelliklerini görmek istediğinizde, ardından bir noktayla viewmodel değişken adını yazın (`.`).</span><span class="sxs-lookup"><span data-stu-id="87b06-200">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="87b06-201">Bu kodu daha az hatalarla daha hızlı yazmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="87b06-201">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="87b06-202">Kullanarak bir model belirtin `@model` yönergesi.</span><span class="sxs-lookup"><span data-stu-id="87b06-202">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="87b06-203">Model ile kullanmak `@Model`:</span><span class="sxs-lookup"><span data-stu-id="87b06-203">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="87b06-204">Modeli görünüme sağlamak için denetleyici, bir parametre olarak geçirir:</span><span class="sxs-lookup"><span data-stu-id="87b06-204">To provide the model to the view, the controller passes it as a parameter:</span></span>

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

<span data-ttu-id="87b06-205">Bir görünüme sağlayabilir modeli türlerinde bir kısıtlama yoktur.</span><span class="sxs-lookup"><span data-stu-id="87b06-205">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="87b06-206">Düz eski CLR nesnesi (POCO) viewmodels tanımlanan çok az kayıpla veya hiç davranışları ile (yöntemleri) kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="87b06-206">We recommend using Plain Old CLR Object (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="87b06-207">Genellikle, viewmodel sınıflar ya da depolanmış *modelleri* klasör veya ayrı bir *ViewModels* uygulama kökünde klasör.</span><span class="sxs-lookup"><span data-stu-id="87b06-207">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="87b06-208">*Adresi* yukarıdaki örnekte kullanılan viewmodel olan adındaki bir dosyada depolanan bir POCO viewmodel *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="87b06-208">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

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

<span data-ttu-id="87b06-209">Hiçbir şey viewmodel türleri ve iş modeli türleri için aynı sınıfları kullanmalarını engeller.</span><span class="sxs-lookup"><span data-stu-id="87b06-209">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="87b06-210">Ancak, ayrı modelleri kullanarak iş mantığı ve verileri, uygulamanızın erişim bölümleri bağımsız olarak değiştirmek kendi görünümlerinizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="87b06-210">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="87b06-211">Modelleri ve viewmodels ayrılması sunduğu güvenlik avantajlarından modelleri kullandığınızda [model bağlama](xref:mvc/models/model-binding) ve [doğrulama](xref:mvc/models/validation) uygulama için kullanıcı tarafından gönderilen veriler için.</span><span class="sxs-lookup"><span data-stu-id="87b06-211">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>

<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-viewdata-attribute-and-viewbag"></a><span data-ttu-id="87b06-212">Zayıf yazılmış veri (ViewData, ViewData özniteliği ve Görünüm Paketi)</span><span class="sxs-lookup"><span data-stu-id="87b06-212">Weakly-typed data (ViewData, ViewData attribute, and ViewBag)</span></span>

<span data-ttu-id="87b06-213">`ViewBag` *Razor sayfalarında kullanılamaz.*</span><span class="sxs-lookup"><span data-stu-id="87b06-213">`ViewBag` *isn't available in Razor Pages.*</span></span>

<span data-ttu-id="87b06-214">Kesin türü belirtilmiş görünümleri yanı sıra, görünümleri erişimi bir *zayıf yazılmış* (olarak da bilinir *geniş yazılmış*) veri koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="87b06-214">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="87b06-215">Güçlü türlerinin aksine *zayıf türleri* (veya *kaybetmiş türleri*) kullanmakta olduğunuz veri türünü açıkça bildirmeyin anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="87b06-215">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="87b06-216">Zayıf yazılmış veri koleksiyonunu küçük miktarda denetleyicileri ve görünümler ve dışındaki veri geçirmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87b06-216">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="87b06-217">Arasında veri geçirme bir...</span><span class="sxs-lookup"><span data-stu-id="87b06-217">Passing data between a ...</span></span>                        | <span data-ttu-id="87b06-218">Örnek</span><span class="sxs-lookup"><span data-stu-id="87b06-218">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="87b06-219">Denetleyici ve bir görünüm</span><span class="sxs-lookup"><span data-stu-id="87b06-219">Controller and a view</span></span>                             | <span data-ttu-id="87b06-220">Bir açılır liste verilerle doldurma.</span><span class="sxs-lookup"><span data-stu-id="87b06-220">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="87b06-221">Görünüm ve [görünümü](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="87b06-221">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="87b06-222">Ayarı  **\<title >** bir görünüm dosyasından düzeni görünümünde öğe içeriği.</span><span class="sxs-lookup"><span data-stu-id="87b06-222">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="87b06-223">[Kısmi Görünüm](xref:mvc/views/partial) ve bir görünüm</span><span class="sxs-lookup"><span data-stu-id="87b06-223">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="87b06-224">Kullanıcının istenen Web sayfasındaki göre verileri görüntüleyen bir pencere öğesi.</span><span class="sxs-lookup"><span data-stu-id="87b06-224">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="87b06-225">Bu koleksiyon yoluyla başvurulabilir `ViewData` veya `ViewBag` denetleyicileri ve görünümlere özellikleri.</span><span class="sxs-lookup"><span data-stu-id="87b06-225">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="87b06-226">`ViewData` Zayıf yazılmış nesneleri sözlüğü bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="87b06-226">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="87b06-227">`ViewBag` Özelliktir çevresinde bir sarmalayıcı `ViewData` arka plandaki için dinamik özellikler sağlayan `ViewData` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="87b06-227">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="87b06-228">`ViewData` ve `ViewBag` çalışma zamanında dinamik olarak çözümlendiği.</span><span class="sxs-lookup"><span data-stu-id="87b06-228">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="87b06-229">Derleme zamanı tür denetimi sunmuyoruz olduğundan, her ikisi de genellikle daha hata-bir viewmodel kullanmaktan daha fazladır.</span><span class="sxs-lookup"><span data-stu-id="87b06-229">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="87b06-230">Bu nedenle, bazı geliştiriciler en düşük düzeyde ya da hiç kullanmayı tercih ederseniz `ViewData` ve `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="87b06-230">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>

<a name="VD"></a>

<span data-ttu-id="87b06-231">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="87b06-231">**ViewData**</span></span>

<span data-ttu-id="87b06-232">`ViewData` olan bir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) üzerinden erişilen nesne `string` anahtarları.</span><span class="sxs-lookup"><span data-stu-id="87b06-232">`ViewData` is a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="87b06-233">Dize verilerini depolanır ve bir cast gerek kalmadan doğrudan kullanılır, ancak diğer atamalısınız `ViewData` bunları ayıkladığınızda belirli türleri için değer nesnesi.</span><span class="sxs-lookup"><span data-stu-id="87b06-233">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="87b06-234">Kullanabileceğiniz `ViewData` görünümlere ve görünümler dahil olmak üzere içinde denetleyicilerinden veri iletmek için [kısmi görünümler](xref:mvc/views/partial) ve [düzenleri](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="87b06-234">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="87b06-235">Aşağıdaki tebrik kartı ve bir adresi kullanarak ilişkin değerleri ayarlayan bir örnektir `ViewData` bir eylem:</span><span class="sxs-lookup"><span data-stu-id="87b06-235">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

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

<span data-ttu-id="87b06-236">Bir görünümdeki verilerle çalışma:</span><span class="sxs-lookup"><span data-stu-id="87b06-236">Work with the data in a view:</span></span>

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
<span data-ttu-id="87b06-237">**ViewData özniteliği**</span><span class="sxs-lookup"><span data-stu-id="87b06-237">**ViewData attribute**</span></span>

<span data-ttu-id="87b06-238">Kullanan başka bir yaklaşım [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) olan [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="87b06-238">Another approach that uses the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) is [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="87b06-239">Denetleyicileri veya Razor sayfasını modelleri özelliklerini donatılmış ile `[ViewData]` depolanır ve sözlükten yüklenen değerlerine sahip.</span><span class="sxs-lookup"><span data-stu-id="87b06-239">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the dictionary.</span></span>

<span data-ttu-id="87b06-240">Aşağıdaki örnekte, giriş denetleyicisi içeren bir `Title` özelliği donatılmış ile `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="87b06-240">In the following example, the Home controller contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="87b06-241">`About` Yöntemi hakkında görünümü için başlık ayarlar:</span><span class="sxs-lookup"><span data-stu-id="87b06-241">The `About` method sets the title for the About view:</span></span>

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

<span data-ttu-id="87b06-242">Hakkında Görünümü'nde erişim `Title` özelliği model özelliği olarak:</span><span class="sxs-lookup"><span data-stu-id="87b06-242">In the About view, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="87b06-243">Düzende başlığı ViewData sözlükten okunur:</span><span class="sxs-lookup"><span data-stu-id="87b06-243">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

<span data-ttu-id="87b06-244">**Görünüm Paketi**</span><span class="sxs-lookup"><span data-stu-id="87b06-244">**ViewBag**</span></span>

<span data-ttu-id="87b06-245">`ViewBag` *Razor sayfalarında kullanılamaz.*</span><span class="sxs-lookup"><span data-stu-id="87b06-245">`ViewBag` *isn't available in Razor Pages.*</span></span>

<span data-ttu-id="87b06-246">`ViewBag` olan bir [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) depolanan nesnelere dinamik erişim sağlayan nesne `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="87b06-246">`ViewBag` is a [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="87b06-247">`ViewBag` atama gerektirmez, çalışmaya daha kullanışlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="87b06-247">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="87b06-248">Aşağıdaki örnekte nasıl kullanılacağını gösterir `ViewBag` kullanarak aynı sonucu ile `ViewData` yukarıda:</span><span class="sxs-lookup"><span data-stu-id="87b06-248">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

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

<span data-ttu-id="87b06-249">**ViewData ve Görünüm Paketi aynı anda kullanma**</span><span class="sxs-lookup"><span data-stu-id="87b06-249">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="87b06-250">`ViewBag` *Razor sayfalarında kullanılamaz.*</span><span class="sxs-lookup"><span data-stu-id="87b06-250">`ViewBag` *isn't available in Razor Pages.*</span></span>

<span data-ttu-id="87b06-251">Bu yana `ViewData` ve `ViewBag` aynı temel bakın `ViewData` koleksiyonu, her ikisi de kullanabilirsiniz `ViewData` ve `ViewBag` karıştırmak ve aralarındaki okurken ve yazarken değerleri aynı.</span><span class="sxs-lookup"><span data-stu-id="87b06-251">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="87b06-252">Başlık kullanılarak ayarlanan `ViewBag` ve açıklaması kullanarak `ViewData` en üstündeki bir *About.cshtml* görünümü:</span><span class="sxs-lookup"><span data-stu-id="87b06-252">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="87b06-253">Özellikleri okunmaya ancak kullanımını ters `ViewData` ve `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="87b06-253">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="87b06-254">İçinde *_Layout.cshtml* dosya, başlık kullanarak elde `ViewData` ve açıklaması kullanarak elde `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="87b06-254">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="87b06-255">Dizeler için bir cast gerektirmeyen unutmayın `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="87b06-255">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="87b06-256">Kullanabileceğiniz `@ViewData["Title"]` atama olmadan.</span><span class="sxs-lookup"><span data-stu-id="87b06-256">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="87b06-257">Her ikisini de kullanarak `ViewData` ve `ViewBag` adresindeki karıştırma ve okuma ve yazma özellikleri eşleştirme yoksa olarak aynı zaman çalışır.</span><span class="sxs-lookup"><span data-stu-id="87b06-257">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="87b06-258">Aşağıdaki biçimlendirmede oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="87b06-258">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="87b06-259">**Görünüm Paketi ViewData arasındaki farkları özeti**</span><span class="sxs-lookup"><span data-stu-id="87b06-259">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="87b06-260">`ViewBag` Razor sayfalarında kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="87b06-260">`ViewBag` isn't available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="87b06-261">Türetilen [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)gibi yararlı olabilecek sözlük özelliklere sahip nedenle `ContainsKey`, `Add`, `Remove`, ve `Clear`.</span><span class="sxs-lookup"><span data-stu-id="87b06-261">Derives from [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="87b06-262">Boşluk izin sözlükteki anahtarları, dizelerdir.</span><span class="sxs-lookup"><span data-stu-id="87b06-262">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="87b06-263">Örnek: `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="87b06-263">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="87b06-264">Herhangi türdeki dışındaki bir `string` kullanmak için görünümünde dönüştürülmelidir `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="87b06-264">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="87b06-265">Türetilen [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), böylece noktalı gösterim kullanılarak dinamik özellikleri oluşturulmasını sağlar (`@ViewBag.SomeKey = <value or object>`), ve hiçbir atama gereklidir.</span><span class="sxs-lookup"><span data-stu-id="87b06-265">Derives from [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="87b06-266">Söz dizimi `ViewBag` denetleyicileri ve görünümleri eklemek daha hızlı hale getirir.</span><span class="sxs-lookup"><span data-stu-id="87b06-266">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="87b06-267">Null değerler için denetlemek daha basit.</span><span class="sxs-lookup"><span data-stu-id="87b06-267">Simpler to check for null values.</span></span> <span data-ttu-id="87b06-268">Örnek: `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="87b06-268">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="87b06-269">**Zaman ViewData veya Görünüm paketini kullanmak için**</span><span class="sxs-lookup"><span data-stu-id="87b06-269">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="87b06-270">Her ikisi de `ViewData` ve `ViewBag` eşit olan küçük miktarda denetleyicileri ve görünüm arasında veri geçirme için geçerli yaklaşımlar.</span><span class="sxs-lookup"><span data-stu-id="87b06-270">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="87b06-271">Hangisinin kullanılacağını Seçimi tercihine göre dayanır.</span><span class="sxs-lookup"><span data-stu-id="87b06-271">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="87b06-272">Karışık ve eşleşen `ViewData` ve `ViewBag` nesneleri, Bununla birlikte, kod okuyun ve sürekli olarak kullanılan bir yaklaşım ile korumak daha kolay.</span><span class="sxs-lookup"><span data-stu-id="87b06-272">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="87b06-273">Her iki yaklaşımın çalışma zamanında dinamik olarak çözümlenmiş ve çalışma zamanı hataları neden dolayısıyla saldırıya.</span><span class="sxs-lookup"><span data-stu-id="87b06-273">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="87b06-274">Bazı geliştirme ekiplerinin bunları kaçının.</span><span class="sxs-lookup"><span data-stu-id="87b06-274">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="87b06-275">Dinamik Görünüm</span><span class="sxs-lookup"><span data-stu-id="87b06-275">Dynamic views</span></span>

<span data-ttu-id="87b06-276">Bir model bildirmeyin görünümleri yazın kullanarak `@model` ancak onlara geçirilen bir model örneği sahip (örneğin, `return View(Address);`) örneğin özellikleri dinamik olarak başvurabilir:</span><span class="sxs-lookup"><span data-stu-id="87b06-276">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="87b06-277">Bu özellik esneklik sunar ancak derleme koruma veya IntelliSense sunmaz.</span><span class="sxs-lookup"><span data-stu-id="87b06-277">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="87b06-278">Web sayfası oluşturma özelliği yoksa, çalışma zamanında başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="87b06-278">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="87b06-279">Daha fazla görünümü özellikleri</span><span class="sxs-lookup"><span data-stu-id="87b06-279">More view features</span></span>

<span data-ttu-id="87b06-280">[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) varolan HTML etiketleri için sunucu tarafı davranışı eklemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="87b06-280">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="87b06-281">Etiket Yardımcıları kullanarak özel kod veya kendi görünümlerinizi içinde Yardımcıları yazmak için gereken önler.</span><span class="sxs-lookup"><span data-stu-id="87b06-281">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="87b06-282">Etiket Yardımcıları olarak öznitelikleri HTML öğelerine uygulanır ve bunları işleyemiyor düzenleyiciler tarafından göz ardı edilir.</span><span class="sxs-lookup"><span data-stu-id="87b06-282">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="87b06-283">Bu, çeşitli araçları görünüm biçimlendirmesinde oluşturmak ve düzenlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="87b06-283">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="87b06-284">Özel HTML biçimlendirme oluşturmak çok sayıda yerleşik HTML Yardımcıları ile gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="87b06-284">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="87b06-285">Daha karmaşık kullanıcı arabirimi mantığı tarafından işlenebilir [görünüm bileşenleri](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="87b06-285">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="87b06-286">Görünüm bileşenleri aynı SoC o denetleyicileri sağlar ve görünümler sunar.</span><span class="sxs-lookup"><span data-stu-id="87b06-286">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="87b06-287">Bunlar, eylemleri ve ortak kullanıcı arabirimi öğeleri tarafından kullanılan verileri uğraşmanız görünümleri gereksinimini ortadan kaldırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="87b06-287">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="87b06-288">Birçok diğer yönlerini ASP.NET Core gibi görünümleri Destek [bağımlılık ekleme](xref:fundamentals/dependency-injection), olmasını hizmetlerine izin [görünümlere eklenen](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="87b06-288">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
