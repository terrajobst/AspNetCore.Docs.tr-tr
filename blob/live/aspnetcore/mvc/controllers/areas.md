---
title: "Alanları"
author: rick-anderson
description: "Alanları ile çalışmaya nasıl gösterir."
keywords: "ASP.NET yönlendirme, çekirdek, alanlar, görünümleri"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 5e16d5e8-5696-4cb2-8ec7-d36be305c922
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: cd0302fa1668979df9bbd6cb36f82742d325c5e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="areas"></a><span data-ttu-id="74e6a-104">Alanları</span><span class="sxs-lookup"><span data-stu-id="74e6a-104">Areas</span></span>

<span data-ttu-id="74e6a-105">Tarafından [Dhananjay Kumar](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="74e6a-105">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="74e6a-106">Alanlar, ilgili işlevleri (Yönlendirme) ayrı ad alanını ve klasör yapısı (için görünümler) olarak bir grup düzenlemek için kullanılan bir ASP.NET MVC özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="74e6a-106">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="74e6a-107">Alanları kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla bir hiyerarşi oluşturur `area`, `controller` ve `action`.</span><span class="sxs-lookup"><span data-stu-id="74e6a-107">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="74e6a-108">Alanları büyük bir ASP.NET Core MVC Web uygulaması işlevsel gruplamalarda daha küçük bölümlere ayırmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="74e6a-108">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="74e6a-109">Etkin bir uygulama içinde bir MVC yapısı alanıdır.</span><span class="sxs-lookup"><span data-stu-id="74e6a-109">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="74e6a-110">MVC projesinde, Model, denetleyici ve görünüm gibi mantıksal bileşenlerin farklı klasörlerde tutulur ve MVC bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="74e6a-110">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="74e6a-111">Büyük bir uygulama için uygulama ayrı yüksek düzey alanlarına işlevlerin bölümlemek için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="74e6a-111">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="74e6a-112">Örneğin, bir e-ticaret uygulamayla checkout, faturalama ve arama vb. gibi birden çok iş birimleri. Her bu birimleri kendi mantıksal bileşen görünümleri, denetleyicileri ve modeli vardır.</span><span class="sxs-lookup"><span data-stu-id="74e6a-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="74e6a-113">Bu senaryoda, fiziksel olarak iş bileşenleri aynı projede bölümlemek için alanları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="74e6a-113">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="74e6a-114">Bir alanı denetleyicileri, görünümler ve modelleri kendi kümesiyle ASP.NET Core MVC projesinde daha küçük işlevsel birimleri olarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="74e6a-114">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="74e6a-115">Bir MVC alanlarda kullanmayı ne zaman proje:</span><span class="sxs-lookup"><span data-stu-id="74e6a-115">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="74e6a-116">Uygulamanız, mantıksal olarak ayrılmalıdır birden çok üst düzey işlev bileşenlerden</span><span class="sxs-lookup"><span data-stu-id="74e6a-116">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="74e6a-117">Böylece her işlevsel alan üzerinde bağımsız olarak çalışılabilecek MVC projenizi bölüm istiyor</span><span class="sxs-lookup"><span data-stu-id="74e6a-117">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="74e6a-118">Alan özellikleri:</span><span class="sxs-lookup"><span data-stu-id="74e6a-118">Area features:</span></span>

* <span data-ttu-id="74e6a-119">Bir ASP.NET Core MVC uygulama herhangi bir sayıda alanları olabilir</span><span class="sxs-lookup"><span data-stu-id="74e6a-119">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="74e6a-120">Kendi denetleyicileri, modelleri ve görünümler her bir alan vardır</span><span class="sxs-lookup"><span data-stu-id="74e6a-120">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="74e6a-121">Büyük MVC projeler üzerinde bağımsız olarak çalışılabilecek birden çok üst düzey bileşenlerine düzenlemenizi sağlar</span><span class="sxs-lookup"><span data-stu-id="74e6a-121">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="74e6a-122">Farklı olduğu sürece birden çok denetleyicisi aynı adda - destekleyen *alanları*</span><span class="sxs-lookup"><span data-stu-id="74e6a-122">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="74e6a-123">Alanları nasıl oluşturulduğunu ve kullanılan göstermek için örnek bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="74e6a-123">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="74e6a-124">İki ayrı gruplandırmaları denetleyicilerinin ve görünümlerin sahip bir mağaza uygulaması sahip varsayalım: ürünler ve hizmetler.</span><span class="sxs-lookup"><span data-stu-id="74e6a-124">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="74e6a-125">Tipik bir klasör yapısı için MVC alanları kullanarak aşağıda benzer olduğunu:</span><span class="sxs-lookup"><span data-stu-id="74e6a-125">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="74e6a-126">Proje adı</span><span class="sxs-lookup"><span data-stu-id="74e6a-126">Project name</span></span>

  * <span data-ttu-id="74e6a-127">Alanları</span><span class="sxs-lookup"><span data-stu-id="74e6a-127">Areas</span></span>

    * <span data-ttu-id="74e6a-128">Ürünler</span><span class="sxs-lookup"><span data-stu-id="74e6a-128">Products</span></span>

      * <span data-ttu-id="74e6a-129">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="74e6a-129">Controllers</span></span>

        * <span data-ttu-id="74e6a-130">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="74e6a-130">HomeController.cs</span></span>

        * <span data-ttu-id="74e6a-131">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="74e6a-131">ManageController.cs</span></span>

      * <span data-ttu-id="74e6a-132">Görünümler</span><span class="sxs-lookup"><span data-stu-id="74e6a-132">Views</span></span>

        * <span data-ttu-id="74e6a-133">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="74e6a-133">Home</span></span>

          * <span data-ttu-id="74e6a-134">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="74e6a-134">Index.cshtml</span></span>

        * <span data-ttu-id="74e6a-135">Yönetme</span><span class="sxs-lookup"><span data-stu-id="74e6a-135">Manage</span></span>

          * <span data-ttu-id="74e6a-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="74e6a-136">Index.cshtml</span></span>

    * <span data-ttu-id="74e6a-137">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="74e6a-137">Services</span></span>

      * <span data-ttu-id="74e6a-138">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="74e6a-138">Controllers</span></span>

        * <span data-ttu-id="74e6a-139">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="74e6a-139">HomeController.cs</span></span>

      * <span data-ttu-id="74e6a-140">Görünümler</span><span class="sxs-lookup"><span data-stu-id="74e6a-140">Views</span></span>

        * <span data-ttu-id="74e6a-141">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="74e6a-141">Home</span></span>

          * <span data-ttu-id="74e6a-142">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="74e6a-142">Index.cshtml</span></span>

<span data-ttu-id="74e6a-143">Varsayılan olarak bir alandaki bir görünümü işlemek MVC çalıştığında, aşağıdaki konumlarda aramak çalışır:</span><span class="sxs-lookup"><span data-stu-id="74e6a-143">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="74e6a-144">Aracılığıyla değiştirilebilen varsayılan konumları bunlar `AreaViewLocationFormats` üzerinde `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="74e6a-144">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="74e6a-145">Örneğin, klasör adı 'Alanlarını' olarak sahip olmak yerine kod 'Kategorilere' değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="74e6a-145">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="74e6a-146">Not etmek için bir şey yapan, yapısını *görünümleri* klasör burada önemli olarak kabul edilen tek ve klasörleri geri kalanı içeriğini ister *denetleyicileri* ve *modelleri* mu **değil** önemli.</span><span class="sxs-lookup"><span data-stu-id="74e6a-146">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="74e6a-147">Örneğin, yüklü bir *denetleyicileri* ve *modelleri* hiç klasör.</span><span class="sxs-lookup"><span data-stu-id="74e6a-147">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="74e6a-148">Bu çalışır çünkü içeriğini *denetleyicileri* ve *modelleri* bir .dll burada içeriğini olarak derlenmiş yalnızca kodu *görünümleri* , bir istek kadar değil Görünüm yapıldı.</span><span class="sxs-lookup"><span data-stu-id="74e6a-148">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* is not until a request to that view has been made.</span></span>

<span data-ttu-id="74e6a-149">Klasör hiyerarşisi tanımladığınız sonra MVC her denetleyici bir alanı ile ilişkili olduğunu söylemek gerekir.</span><span class="sxs-lookup"><span data-stu-id="74e6a-149">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="74e6a-150">Denetleyici adı ile tasarlayarak bunu `[Area]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="74e6a-150">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="74e6a-151">Yeni oluşturulan alanlarınızı ile çalışan bir rota tanımı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="74e6a-151">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="74e6a-152">[Denetleyici eylemleri için yönlendirme](routing.md) makale gider içine öznitelik rotaları karşı geleneksel yolları kullanma dahil olmak üzere yönlendirme tanımları oluşturma hakkında ayrıntılı bilgi.</span><span class="sxs-lookup"><span data-stu-id="74e6a-152">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="74e6a-153">Bu örnekte, geleneksel bir rota kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="74e6a-153">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="74e6a-154">Bunu yapmak için açık *haline* dosya ve ekleyerek değiştirmeye `areaRoute` route tanımını aşağıdaki adlı.</span><span class="sxs-lookup"><span data-stu-id="74e6a-154">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="74e6a-155">İçin dizin taramayı `http://<yourApp>/products`, `Index` eylem yöntemi `HomeController` içinde `Products` alanı çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="74e6a-155">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="74e6a-156">Bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="74e6a-156">Link Generation</span></span>

* <span data-ttu-id="74e6a-157">Bir alanda bir eylemden bağlantıları oluşturmak aynı denetleyicisi içinde başka bir eylem denetleyiciye bağlı.</span><span class="sxs-lookup"><span data-stu-id="74e6a-157">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="74e6a-158">Geçerli isteğin yolu benzer düşünelim`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="74e6a-158">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="74e6a-159">HtmlHelper sözdizimi:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="74e6a-159">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="74e6a-160">TagHelper sözdizimi:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="74e6a-160">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="74e6a-161">Biz 'alanı' ve 'controller' değerleri sağlamanızı olmayan Not zaten geçerli istek bağlamında kullanılabilir olduğundan burada.</span><span class="sxs-lookup"><span data-stu-id="74e6a-161">Note that we need not supply the 'area' and 'controller' values here as they are already available in the context of the current request.</span></span> <span data-ttu-id="74e6a-162">Bu tür bir değerleri çağrılır `ambient` değerleri.</span><span class="sxs-lookup"><span data-stu-id="74e6a-162">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="74e6a-163">Farklı bir denetleyicideki başka bir eylem denetleyiciye bağlı bir alanda bir eylemden bağlantıları oluşturmak</span><span class="sxs-lookup"><span data-stu-id="74e6a-163">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="74e6a-164">Geçerli isteğin yolu benzer düşünelim`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="74e6a-164">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="74e6a-165">HtmlHelper sözdizimi:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="74e6a-165">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="74e6a-166">TagHelper sözdizimi:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="74e6a-166">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="74e6a-167">Burada bir 'alanı' ortam değeri kullanılır, ancak 'controller' değeri açıkça yukarıda belirtilen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="74e6a-167">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="74e6a-168">Bir alanda bir eylemden bağlantıları oluşturmak için başka bir eylem denetleyicisi farklı bir denetleyici ve farklı bir alan göre.</span><span class="sxs-lookup"><span data-stu-id="74e6a-168">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="74e6a-169">Geçerli isteğin yolu benzer düşünelim`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="74e6a-169">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="74e6a-170">HtmlHelper sözdizimi:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="74e6a-170">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="74e6a-171">TagHelper sözdizimi:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="74e6a-171">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="74e6a-172">Unutmayın burada yok ortam değerler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="74e6a-172">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="74e6a-173">Bir temel alan denetleyicisi içinde bir eylem bağlantıları farklı bir denetleyicideki başka bir eylem oluşturmak ve **değil** bir bölgede.</span><span class="sxs-lookup"><span data-stu-id="74e6a-173">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="74e6a-174">HtmlHelper sözdizimi:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="74e6a-174">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="74e6a-175">TagHelper sözdizimi:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="74e6a-175">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="74e6a-176">Biz oluşturmak istediğinizde bu yana alanına olmayan bağlantılar 'alanı' burada ortam değeri boş biz denetleyici eylemi temel.</span><span class="sxs-lookup"><span data-stu-id="74e6a-176">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="74e6a-177">Yayımlama alanları</span><span class="sxs-lookup"><span data-stu-id="74e6a-177">Publishing Areas</span></span>

<span data-ttu-id="74e6a-178">Tüm `*.cshtml` ve `wwwroot/**` dosyaları ne zaman çıkış yayımlanır `<Project Sdk="Microsoft.NET.Sdk.Web">` dahil *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="74e6a-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
