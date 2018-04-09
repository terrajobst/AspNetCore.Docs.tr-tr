---
title: ASP.NET Web API ASP.NET Core geçirme
author: ardalis
description: Bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API geçirmek öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 2b9d6ac41266e0e6085153e1302d84a34ee85257
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="a9e04-103">ASP.NET Web API ASP.NET Core geçirme</span><span class="sxs-lookup"><span data-stu-id="a9e04-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="a9e04-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="a9e04-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="a9e04-105">Web API'leri istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP hizmetleridir.</span><span class="sxs-lookup"><span data-stu-id="a9e04-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="a9e04-106">ASP.NET Core MVC web uygulamaları oluşturma tek, tutarlı bir yol sağlayarak Web API'ları oluşturmak için destek içerir.</span><span class="sxs-lookup"><span data-stu-id="a9e04-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="a9e04-107">Bu makalede, bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API geçirmek için gereken adımları gösterir.</span><span class="sxs-lookup"><span data-stu-id="a9e04-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="a9e04-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a9e04-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="a9e04-109">Gözden geçirme ASP.NET Web API projesi</span><span class="sxs-lookup"><span data-stu-id="a9e04-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="a9e04-110">Bu makalede örnek proje kullanan *ProductsApp*, makalede oluşturulmuş [ASP.NET Web API 2 ile çalışmaya başlama](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) , başlangıç noktası olarak.</span><span class="sxs-lookup"><span data-stu-id="a9e04-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="a9e04-111">Bu projede basit bir ASP.NET Web API projesi şu şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="a9e04-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="a9e04-112">İçinde *Global.asax.cs*, için bir çağrı yapılır `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="a9e04-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="a9e04-113">`WebApiConfig` tanımlanan *App_Start*, ve yalnızca bir statik `Register` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a9e04-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="a9e04-114">Bu sınıf yapılandırır [özniteliği yönlendirme](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aslında projesinde kullanılan rağmen.</span><span class="sxs-lookup"><span data-stu-id="a9e04-114">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="a9e04-115">Ayrıca, ASP.NET Web API tarafından kullanılan yönlendirme tablosunun yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="a9e04-115">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="a9e04-116">Bu durumda, ASP.NET Web API biçim ile eşleşmesi için URL'leri beklediği */api/ {controller} / {id}*, ile *{id}* isteğe bağlı olması.</span><span class="sxs-lookup"><span data-stu-id="a9e04-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="a9e04-117">*ProductsApp* proje içeriyor devraldığı tek bir basit denetleyicisi `ApiController` ve iki yöntem sunar:</span><span class="sxs-lookup"><span data-stu-id="a9e04-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="a9e04-118">Son olarak, model, *ürün*, tarafından kullanılan *ProductsApp*, basit bir sınıf:</span><span class="sxs-lookup"><span data-stu-id="a9e04-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="a9e04-119">Biz basit bir proje başlayacağı sahip olduğunuza göre Biz bu Web API projesi ASP.NET Core MVC geçirmek nasıl ekleyebileceğiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a9e04-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="a9e04-120">Hedef projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a9e04-120">Create the Destination Project</span></span>

<span data-ttu-id="a9e04-121">Visual Studio kullanarak, yeni, boş bir çözüm oluşturun ve adlandırın *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="a9e04-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="a9e04-122">Varolan eklemek *ProductsApp* ona proje sonra yeni bir ASP.NET çekirdek Web uygulama projesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a9e04-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="a9e04-123">Yeni proje adı *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="a9e04-123">Name the new project *ProductsCore*.</span></span>

![Yeni Proje iletişim kutusu açmak için Web Şablonları](webapi/_static/add-web-project.png)

<span data-ttu-id="a9e04-125">Ardından, Web API projesi şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="a9e04-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="a9e04-126">Planlıyoruz *ProductsApp* bu yeni projeye içeriği.</span><span class="sxs-lookup"><span data-stu-id="a9e04-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core şablonları listesinde seçilen Web API projesi şablonuyla yeni Web uygulaması iletişim kutusu](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="a9e04-128">Silme `Project_Readme.html` yeni proje dosyasından.</span><span class="sxs-lookup"><span data-stu-id="a9e04-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="a9e04-129">Çözümünüzü gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="a9e04-129">Your solution should now look like this:</span></span>

![Uygulama çözüm dosyaları ve klasörleri ProductsApp ve ProductsCore projelerin gösteren Çözüm Gezgini'nde Aç](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="a9e04-131">Yapılandırmasını geçir</span><span class="sxs-lookup"><span data-stu-id="a9e04-131">Migrate Configuration</span></span>

<span data-ttu-id="a9e04-132">ASP.NET Core artık kullanan *Global.asax*, *web.config*, veya *App_Start* klasörler.</span><span class="sxs-lookup"><span data-stu-id="a9e04-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="a9e04-133">Bunun yerine, tüm başlangıç görevleri yapılır *haline* proje kökündeki (bkz [uygulama başlangıç](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="a9e04-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="a9e04-134">ASP.NET Core MVC'de öznitelik tabanlı yönlendirme artık varsayılan olarak dahil edilmiştir, `UseMvc()` olarak adlandırılır; ve bu Web API yolları yapılandırmak için önerilen yaklaşımdır (ve Web API başlangıç projesi yönlendirme nasıl işler).</span><span class="sxs-lookup"><span data-stu-id="a9e04-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-none[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

<span data-ttu-id="a9e04-135">Öznitelik ileride projenizde yönlendirme kullanmak istediğiniz varsayıldığında, ek bir yapılandırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a9e04-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="a9e04-136">Yalnızca örnek gerçekleştirilir gibi denetleyicileri ve eylemleri gerektiği gibi öznitelikleri uygulanır `ValuesController` Web API starter projeye dahil sınıfı:</span><span class="sxs-lookup"><span data-stu-id="a9e04-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="a9e04-137">Varlığını Not *[denetleyicisi]* 8 satırındaki.</span><span class="sxs-lookup"><span data-stu-id="a9e04-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="a9e04-138">Öznitelik tabanlı şimdi yönlendirmeyi destekleyen belirli belirteçleri gibi *[denetleyicisi]* ve *[eylem]*.</span><span class="sxs-lookup"><span data-stu-id="a9e04-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="a9e04-139">Bu belirteçler çalışma zamanında denetleyici veya eylemin, adı ile sırasıyla öznitelik uygulanmış değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="a9e04-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="a9e04-140">Bu proje Sihirli dizelerde sayısını azaltmak için kullanılır ve otomatik yeniden adlandırma yapan yeniden düzenlemeler uygulandığında yolların kendi ilgili denetleyicileri ve eylemleri ile eşitlenmiş tutulacak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a9e04-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="a9e04-141">Ürünler API denetleyicisi geçirmek için biz öncelikle kopyalamanız gerekir *ProductsController* yeni projeye.</span><span class="sxs-lookup"><span data-stu-id="a9e04-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="a9e04-142">Ardından denetleyicisinde yol özniteliği yalnızca şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a9e04-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="a9e04-143">Ayrıca eklemeniz gerekir `[HttpGet]` özniteliği için iki yöntem, her ikisi de HTTP Get çağrılmalıdır beri.</span><span class="sxs-lookup"><span data-stu-id="a9e04-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="a9e04-144">Öznitelik için bir "id" parametresi beklentisi dahil `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="a9e04-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="a9e04-145">Bu noktada, yönlendirme doğru yapılandırılmış; Ancak, henüz bunu test edemiyor.</span><span class="sxs-lookup"><span data-stu-id="a9e04-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="a9e04-146">Ek değişiklikler yapılan, önce *ProductsController* derlenir.</span><span class="sxs-lookup"><span data-stu-id="a9e04-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="a9e04-147">Modelleri ve denetleyiciler geçirme</span><span class="sxs-lookup"><span data-stu-id="a9e04-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="a9e04-148">Bu basit Web API projesi için geçiş işlemi son adımda denetleyicileri ve kullandıkları modelleri kopyalamaktır.</span><span class="sxs-lookup"><span data-stu-id="a9e04-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="a9e04-149">Bu durumda, yalnızca kopyalama *Controllers/ProductsController.cs* yeni bir için özgün projeden.</span><span class="sxs-lookup"><span data-stu-id="a9e04-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="a9e04-150">Ardından, tüm modeller klasörü için yeni bir özgün projeden kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="a9e04-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="a9e04-151">Yeni proje adı ile eşleşmesi için ad alanları ayarlayın (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="a9e04-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="a9e04-152">Bu noktada, uygulama oluşturabilir ve derleme hatalarının sayısı bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="a9e04-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="a9e04-153">Bunlar genellikle aşağıdaki kategorilere ayrılır:</span><span class="sxs-lookup"><span data-stu-id="a9e04-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="a9e04-154">*ApiController* yok</span><span class="sxs-lookup"><span data-stu-id="a9e04-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="a9e04-155">*System.Web.Http* ad alanı mevcut değil</span><span class="sxs-lookup"><span data-stu-id="a9e04-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="a9e04-156">*Ihttpactionresult* yok</span><span class="sxs-lookup"><span data-stu-id="a9e04-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="a9e04-157">Neyse ki, bu düzeltmesi tümü çok kolay şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a9e04-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="a9e04-158">Değişiklik *ApiController* için *denetleyicisi* (eklemeniz gerekebilir *Microsoft.AspNetCore.Mvc kullanarak*)</span><span class="sxs-lookup"><span data-stu-id="a9e04-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="a9e04-159">Silmek için başvuran deyimiyle *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="a9e04-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="a9e04-160">Tüm yöntemi döndürme değiştirme *Ihttpactionresult* döndürmek için bir *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="a9e04-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="a9e04-161">Bu değişiklikler yapılmış ve kullanılmayan işlendikten sonra using deyimleri kaldırıldı, geçirilen *ProductsController* sınıfı şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="a9e04-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="a9e04-162">Şimdi geçirilen projeyi çalıştırın ve Gözat yapabiliyor olmanız gerekir */api/ürünleri*; ve 3 ürünlerinin tam listesini görmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="a9e04-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="a9e04-163">Gözat */api/products/1* ve ilk ürün görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a9e04-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="a9e04-164">Özet</span><span class="sxs-lookup"><span data-stu-id="a9e04-164">Summary</span></span>

<span data-ttu-id="a9e04-165">Basit bir ASP.NET Web API projesi ASP.NET Core MVC geçirilmesi thanks ASP.NET Core MVC Web API'leri için yerleşik destek için oldukça kolay değildir.</span><span class="sxs-lookup"><span data-stu-id="a9e04-165">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="a9e04-166">Her ASP.NET Web API projesi geçiş yapmanız gerekecektir ana parça rotalara, denetleyicilere ve modeller, güncelleştirmeleri denetleyicileri ve eylemleri tarafından kullanılan türleri ile birlikte ' dir.</span><span class="sxs-lookup"><span data-stu-id="a9e04-166">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
