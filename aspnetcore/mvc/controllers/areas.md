---
title: ASP.NET core'da alanları
author: rick-anderson
description: Alanlar (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba ilgili işlevleri düzenlemek için kullanılan bir ASP.NET MVC özelliği nasıl olduğunu öğrenin.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: b78bb5146f1ab9039fa9ff015471654510718ed6
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312224"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="d32b5-103">ASP.NET core'da alanları</span><span class="sxs-lookup"><span data-stu-id="d32b5-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="d32b5-104">Tarafından [Dhananjay Kumar](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d32b5-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d32b5-105">Alanlar, ilgili işlevleri (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba düzenlemek için kullanılan bir ASP.NET MVC özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="d32b5-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="d32b5-106">Alanlara kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla hiyerarşi oluşturur `area`, `controller` ve `action`.</span><span class="sxs-lookup"><span data-stu-id="d32b5-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="d32b5-107">Alanları büyük bir ASP.NET Core MVC Web uygulaması işlevsel gruplamalarda daha küçük bölümlere ayırmak için bir yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="d32b5-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="d32b5-108">Bir MVC yapı bir uygulama içinde etkili bir şekilde alanıdır.</span><span class="sxs-lookup"><span data-stu-id="d32b5-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="d32b5-109">Bir MVC projesi mantıksal bileşenler modeli, denetleyici ve görünüm gibi farklı klasörlerde tutulur ve MVC bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır.</span><span class="sxs-lookup"><span data-stu-id="d32b5-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="d32b5-110">Büyük bir uygulama için ayrı yüksek düzey alanlarına işlev uygulamasını bölümleme için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d32b5-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="d32b5-111">Örneğin, bir e-ticaret uygulamayla kullanıma alma ve faturalandırma arama vb. gibi birden çok iş birimleri. Bu birimlerin her biri kendi mantıksal bileşen görünümleri, denetleyicilere ve modelleri sahip.</span><span class="sxs-lookup"><span data-stu-id="d32b5-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="d32b5-112">Bu senaryoda, fiziksel olarak aynı projede iş bileşenleri bölümlemek için alanlar kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="d32b5-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="d32b5-113">Bir alanı denetleyicileri, görünümler ve modelleri, kendi kümesi ile ASP.NET Core MVC projesinde daha küçük işlevsel birimi olarak tanımlanabilir.</span><span class="sxs-lookup"><span data-stu-id="d32b5-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="d32b5-114">Bir MVC alanlardaki ne zaman proje:</span><span class="sxs-lookup"><span data-stu-id="d32b5-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="d32b5-115">Uygulamanız, mantıksal olarak ayrılması birden çok üst düzey işlevsel bileşenden</span><span class="sxs-lookup"><span data-stu-id="d32b5-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="d32b5-116">Böylece her işlevsel alan üzerinde bağımsız olarak çalışılabilecek MVC projenize bölümlemek istediğiniz</span><span class="sxs-lookup"><span data-stu-id="d32b5-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="d32b5-117">Alan özellikleri:</span><span class="sxs-lookup"><span data-stu-id="d32b5-117">Area features:</span></span>

* <span data-ttu-id="d32b5-118">Bir ASP.NET Core MVC uygulaması herhangi bir sayıda alanları olabilir.</span><span class="sxs-lookup"><span data-stu-id="d32b5-118">An ASP.NET Core MVC app can have any number of areas.</span></span>

* <span data-ttu-id="d32b5-119">Her alan kendi denetleyicileri, modelleri ve görünümleri sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d32b5-119">Each area has its own controllers, models, and views.</span></span>

* <span data-ttu-id="d32b5-120">Alanları üzerinde bağımsız olarak çalışması birden çok üst düzey bileşenlerine büyük MVC projeleri düzenlemenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="d32b5-120">Areas allow you to organize large MVC projects into multiple high-level components that can be worked on independently.</span></span>

* <span data-ttu-id="d32b5-121">Farklı sahip oldukları sürece aynı ada sahip birden çok denetleyicileri destek alanları *alanları*.</span><span class="sxs-lookup"><span data-stu-id="d32b5-121">Areas support multiple controllers with the same name, as long as they have different *areas*.</span></span>

<span data-ttu-id="d32b5-122">Alanlarını nasıl oluşturulduğunu ve kullanılan göstermek için örnek bir göz atalım.</span><span class="sxs-lookup"><span data-stu-id="d32b5-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="d32b5-123">İki ayrı gruplandırmaları görünümleri ve denetleyicileri içeren bir mağaza uygulaması sahip düşünelim: Ürün ve Hizmetleri.</span><span class="sxs-lookup"><span data-stu-id="d32b5-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="d32b5-124">Tipik bir klasör yapısı için MVC alanlara kullanarak aşağıda benzer olduğunu:</span><span class="sxs-lookup"><span data-stu-id="d32b5-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="d32b5-125">Proje adı</span><span class="sxs-lookup"><span data-stu-id="d32b5-125">Project name</span></span>

  * <span data-ttu-id="d32b5-126">Alanları</span><span class="sxs-lookup"><span data-stu-id="d32b5-126">Areas</span></span>

    * <span data-ttu-id="d32b5-127">Ürünler</span><span class="sxs-lookup"><span data-stu-id="d32b5-127">Products</span></span>

      * <span data-ttu-id="d32b5-128">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="d32b5-128">Controllers</span></span>

        * <span data-ttu-id="d32b5-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="d32b5-129">HomeController.cs</span></span>

        * <span data-ttu-id="d32b5-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="d32b5-130">ManageController.cs</span></span>

      * <span data-ttu-id="d32b5-131">Görünümler</span><span class="sxs-lookup"><span data-stu-id="d32b5-131">Views</span></span>

        * <span data-ttu-id="d32b5-132">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="d32b5-132">Home</span></span>

          * <span data-ttu-id="d32b5-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="d32b5-133">Index.cshtml</span></span>

        * <span data-ttu-id="d32b5-134">yönetme</span><span class="sxs-lookup"><span data-stu-id="d32b5-134">Manage</span></span>

          * <span data-ttu-id="d32b5-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="d32b5-135">Index.cshtml</span></span>

    * <span data-ttu-id="d32b5-136">Hizmetler</span><span class="sxs-lookup"><span data-stu-id="d32b5-136">Services</span></span>

      * <span data-ttu-id="d32b5-137">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="d32b5-137">Controllers</span></span>

        * <span data-ttu-id="d32b5-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="d32b5-138">HomeController.cs</span></span>

      * <span data-ttu-id="d32b5-139">Görünümler</span><span class="sxs-lookup"><span data-stu-id="d32b5-139">Views</span></span>

        * <span data-ttu-id="d32b5-140">Ana Sayfası</span><span class="sxs-lookup"><span data-stu-id="d32b5-140">Home</span></span>

          * <span data-ttu-id="d32b5-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="d32b5-141">Index.cshtml</span></span>

<span data-ttu-id="d32b5-142">Varsayılan olarak bir alanda görünüm işlemek MVC çalıştığında, aşağıdaki konumlarda aramak çalışır:</span><span class="sxs-lookup"><span data-stu-id="d32b5-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="d32b5-143">Bunlar aracılığıyla değiştirilebilen varsayılan konumları `AreaViewLocationFormats` üzerinde `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="d32b5-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="d32b5-144">Örneğin, 'Alanları olarak' klasör adı yerine aşağıdaki kodu, bu 'Kategorilere' değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="d32b5-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="d32b5-145">Unutmayın olmasıdır yapısını *görünümleri* burada önemli olarak kabul edilen tek bir klasördür ve klasörleri geri kalanını içeriği gibi *denetleyicileri* ve *modelleri* mu **değil** önemi.</span><span class="sxs-lookup"><span data-stu-id="d32b5-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="d32b5-146">Örneğin, sahip bir *denetleyicileri* ve *modelleri* hiç klasör.</span><span class="sxs-lookup"><span data-stu-id="d32b5-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="d32b5-147">Bunun çalışmasının nedeni içeriğini *denetleyicileri* ve *modelleri* içeriği burada olarak derlenmiş bir .dll, yalnızca kod *görünümleri* , isteğine kadar değil Görünüm yapıldı.</span><span class="sxs-lookup"><span data-stu-id="d32b5-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="d32b5-148">Klasör hiyerarşisini tanımladınız sonra MVC denetleyicisi her bir alanı ile ilişkili olduğunu bildirmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="d32b5-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="d32b5-149">Denetleyici adı ile tasarlayarak bunu `[Area]` özniteliği.</span><span class="sxs-lookup"><span data-stu-id="d32b5-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="d32b5-150">Yeni oluşturulan alanlarınızı çalışır bir yönlendirme tanımı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="d32b5-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="d32b5-151">[Denetleyici eylemleri için rota](routing.md) öznitelik rotaları karşı geleneksel yollar kullanarak da dahil olmak üzere, yönlendirme tanımları oluşturma hakkında daha fazla ayrıntı makale gider.</span><span class="sxs-lookup"><span data-stu-id="d32b5-151">The [Route to controller actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="d32b5-152">Bu örnekte, geleneksel bir rota kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="d32b5-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="d32b5-153">Bunu yapmak için açık *Startup.cs* ekleyerek değiştirin ve dosya `areaRoute` rota tanımını aşağıdaki adlı.</span><span class="sxs-lookup"><span data-stu-id="d32b5-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="d32b5-154">Gözatmaya `http://<yourApp>/products`, `Index` eylem yöntemi `HomeController` içinde `Products` alan çağrılacak.</span><span class="sxs-lookup"><span data-stu-id="d32b5-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="d32b5-155">Bağlantı oluşturma</span><span class="sxs-lookup"><span data-stu-id="d32b5-155">Link Generation</span></span>

* <span data-ttu-id="d32b5-156">Bir alanda bir eylem bağlantıları oluşturmak aynı denetleyici içinde başka bir eylem denetleyiciye bağlı.</span><span class="sxs-lookup"><span data-stu-id="d32b5-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="d32b5-157">Geçerli isteğin yolu gibi diyelim ki `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="d32b5-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="d32b5-158">HtmlHelper sözdizimi: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="d32b5-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="d32b5-159">TagHelper sözdizimi: `<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="d32b5-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="d32b5-160">Unutmayın, biz 'alanı' ve 'controller' değerlerini belirtmeniz değil, zaten geçerli istek bağlamında kullanılabilir burada.</span><span class="sxs-lookup"><span data-stu-id="d32b5-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="d32b5-161">Bu tür değerlere çağrılır `ambient` değerleri.</span><span class="sxs-lookup"><span data-stu-id="d32b5-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="d32b5-162">Başka bir eylem farklı denetleyicisine denetleyiciye bağlı bir alanda bir eylem bağlantıları oluşturmak</span><span class="sxs-lookup"><span data-stu-id="d32b5-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="d32b5-163">Geçerli isteğin yolu gibi diyelim ki `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="d32b5-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="d32b5-164">HtmlHelper sözdizimi: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="d32b5-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="d32b5-165">TagHelper sözdizimi: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="d32b5-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="d32b5-166">Burada ortam 'alanı' değeri kullanılmıştır, ancak 'controller' değeri açıkça yukarıda belirtilen unutmayın.</span><span class="sxs-lookup"><span data-stu-id="d32b5-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="d32b5-167">Bir alanda bir eylem bağlantıları oluşturmak için başka bir eylem denetleyicisi farklı bir denetleyici ve farklı bir alana göre.</span><span class="sxs-lookup"><span data-stu-id="d32b5-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="d32b5-168">Geçerli isteğin yolu gibi diyelim ki `/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="d32b5-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="d32b5-169">HtmlHelper sözdizimi: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="d32b5-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="d32b5-170">TagHelper sözdizimi: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="d32b5-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="d32b5-171">Unutmayın burada hiçbir ortam değerler kullanılır.</span><span class="sxs-lookup"><span data-stu-id="d32b5-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="d32b5-172">İçinde bir temel alan denetleyici eylem bağlantıları farklı bir denetleyici üzerinde başka bir eylem oluşturmak ve **değil** bir alana.</span><span class="sxs-lookup"><span data-stu-id="d32b5-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="d32b5-173">HtmlHelper sözdizimi: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="d32b5-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="d32b5-174">TagHelper sözdizimi: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="d32b5-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="d32b5-175">Oluşturulacak istediğinden alanı olmayan bağlantılar denetleyici eylemi boş biz 'alanı' burada için ortam değerine göre.</span><span class="sxs-lookup"><span data-stu-id="d32b5-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="d32b5-176">Yayımlama alanları</span><span class="sxs-lookup"><span data-stu-id="d32b5-176">Publishing Areas</span></span>

<span data-ttu-id="d32b5-177">Tüm `*.cshtml` ve `wwwroot/**` dosyaları ne zaman çıkış yayımlanır `<Project Sdk="Microsoft.NET.Sdk.Web">` dahil *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="d32b5-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
