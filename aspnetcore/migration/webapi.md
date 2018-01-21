---
title: "ASP.NET Web API geçirme"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: aef00b63c9889100116facc610bec99f889e4c46
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="afe86-102">ASP.NET Web API geçirme</span><span class="sxs-lookup"><span data-stu-id="afe86-102">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="afe86-103">Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="afe86-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="afe86-104">Web API'leri istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP hizmetleridir.</span><span class="sxs-lookup"><span data-stu-id="afe86-104">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="afe86-105">ASP.NET Core MVC web uygulamaları oluşturma tek, tutarlı bir yol sağlayarak Web API'ları oluşturmak için destek içerir.</span><span class="sxs-lookup"><span data-stu-id="afe86-105">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="afe86-106">Bu makalede, bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API geçirmek için gereken adımları gösterir.</span><span class="sxs-lookup"><span data-stu-id="afe86-106">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="afe86-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="afe86-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="afe86-108">Gözden geçirme ASP.NET Web API projesi</span><span class="sxs-lookup"><span data-stu-id="afe86-108">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="afe86-109">Bu makalede örnek proje kullanan *ProductsApp*, makalede oluşturulmuş [ASP.NET Web API ile çalışmaya başlama](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) , başlangıç noktası olarak.</span><span class="sxs-lookup"><span data-stu-id="afe86-109">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="afe86-110">Bu projede basit bir ASP.NET Web API projesi şu şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="afe86-110">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="afe86-111">İçinde *Global.asax.cs*, için bir çağrı yapılır `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="afe86-111">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="afe86-112">`WebApiConfig`tanımlanan *App_Start*, ve yalnızca bir statik `Register` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="afe86-112">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="afe86-113">Bu sınıf yapılandırır [özniteliği yönlendirme](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aslında projesinde kullanılan rağmen.</span><span class="sxs-lookup"><span data-stu-id="afe86-113">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="afe86-114">Ayrıca, ASP.NET Web API tarafından kullanılan yönlendirme tablosunun yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="afe86-114">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="afe86-115">Bu durumda, ASP.NET Web API biçim ile eşleşmesi için URL'leri beklediği */api/ {controller} / {id}*, ile *{id}* isteğe bağlı olması.</span><span class="sxs-lookup"><span data-stu-id="afe86-115">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="afe86-116">*ProductsApp* proje içeriyor devraldığı tek bir basit denetleyicisi `ApiController` ve iki yöntem sunar:</span><span class="sxs-lookup"><span data-stu-id="afe86-116">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="afe86-117">Son olarak, model, *ürün*, tarafından kullanılan *ProductsApp*, basit bir sınıf:</span><span class="sxs-lookup"><span data-stu-id="afe86-117">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="afe86-118">Biz basit bir proje başlayacağı sahip olduğunuza göre Biz bu Web API projesi ASP.NET Core MVC geçirmek nasıl ekleyebileceğiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="afe86-118">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="afe86-119">Hedef projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="afe86-119">Create the Destination Project</span></span>

<span data-ttu-id="afe86-120">Visual Studio kullanarak, yeni, boş bir çözüm oluşturun ve adlandırın *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="afe86-120">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="afe86-121">Varolan eklemek *ProductsApp* ona proje sonra yeni bir ASP.NET çekirdek Web uygulama projesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="afe86-121">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="afe86-122">Yeni proje adı *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="afe86-122">Name the new project *ProductsCore*.</span></span>

![Yeni Proje iletişim kutusu açmak için Web Şablonları](webapi/_static/add-web-project.png)

<span data-ttu-id="afe86-124">Ardından, Web API projesi şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="afe86-124">Next, choose the Web API project template.</span></span> <span data-ttu-id="afe86-125">Planlıyoruz *ProductsApp* bu yeni projeye içeriği.</span><span class="sxs-lookup"><span data-stu-id="afe86-125">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core şablonları listesinde seçilen Web API projesi şablonuyla yeni Web uygulaması iletişim kutusu](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="afe86-127">Silme `Project_Readme.html` yeni proje dosyasından.</span><span class="sxs-lookup"><span data-stu-id="afe86-127">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="afe86-128">Çözümünüzü gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="afe86-128">Your solution should now look like this:</span></span>

![Uygulama çözüm dosyaları ve klasörleri ProductsApp ve ProductsCore projelerin gösteren Çözüm Gezgini'nde Aç](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="afe86-130">Yapılandırmasını geçir</span><span class="sxs-lookup"><span data-stu-id="afe86-130">Migrate Configuration</span></span>

<span data-ttu-id="afe86-131">ASP.NET Core artık kullanan *Global.asax*, *web.config*, veya *App_Start* klasörler.</span><span class="sxs-lookup"><span data-stu-id="afe86-131">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="afe86-132">Bunun yerine, tüm başlangıç görevleri yapılır *haline* proje kökündeki (bkz [uygulama başlangıç](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="afe86-132">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="afe86-133">ASP.NET Core MVC'de öznitelik tabanlı yönlendirme artık varsayılan olarak dahil edilmiştir, `UseMvc()` olarak adlandırılır; ve bu Web API yolları yapılandırmak için önerilen yaklaşımdır (ve Web API başlangıç projesi yönlendirme nasıl işler).</span><span class="sxs-lookup"><span data-stu-id="afe86-133">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

<span data-ttu-id="afe86-134">Öznitelik ileride projenizde yönlendirme kullanmak istediğiniz varsayıldığında, ek bir yapılandırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="afe86-134">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="afe86-135">Yalnızca örnek gerçekleştirilir gibi denetleyicileri ve eylemleri gerektiği gibi öznitelikleri uygulanır `ValuesController` Web API starter projeye dahil sınıfı:</span><span class="sxs-lookup"><span data-stu-id="afe86-135">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that is included in the Web API starter project:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="afe86-136">Varlığını Not *[denetleyicisi]* 8 satırındaki.</span><span class="sxs-lookup"><span data-stu-id="afe86-136">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="afe86-137">Öznitelik tabanlı şimdi yönlendirmeyi destekleyen belirli belirteçleri gibi *[denetleyicisi]* ve *[eylem]*.</span><span class="sxs-lookup"><span data-stu-id="afe86-137">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="afe86-138">Bu belirteçler çalışma zamanında denetleyici veya eylemin, adı ile sırasıyla öznitelik uygulanmış değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="afe86-138">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="afe86-139">Bu proje Sihirli dizelerde sayısını azaltmak için kullanılır ve otomatik yeniden adlandırma yapan yeniden düzenlemeler uygulandığında yolların kendi ilgili denetleyicileri ve eylemleri ile eşitlenmiş tutulacak sağlar.</span><span class="sxs-lookup"><span data-stu-id="afe86-139">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="afe86-140">Ürünler API denetleyicisi geçirmek için biz öncelikle kopyalamanız gerekir *ProductsController* yeni projeye.</span><span class="sxs-lookup"><span data-stu-id="afe86-140">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="afe86-141">Ardından denetleyicisinde yol özniteliği yalnızca şunlardır:</span><span class="sxs-lookup"><span data-stu-id="afe86-141">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="afe86-142">Ayrıca eklemeniz gerekir `[HttpGet]` özniteliği için iki yöntem, her ikisi de HTTP Get çağrılmalıdır beri.</span><span class="sxs-lookup"><span data-stu-id="afe86-142">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="afe86-143">Öznitelik için bir "id" parametresi beklentisi dahil `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="afe86-143">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="afe86-144">Bu noktada, yönlendirme doğru yapılandırılmış; Ancak, henüz bunu test edemiyor.</span><span class="sxs-lookup"><span data-stu-id="afe86-144">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="afe86-145">Ek değişiklikler yapılan, önce *ProductsController* derlenir.</span><span class="sxs-lookup"><span data-stu-id="afe86-145">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="afe86-146">Modelleri ve denetleyiciler geçirme</span><span class="sxs-lookup"><span data-stu-id="afe86-146">Migrate Models and Controllers</span></span>

<span data-ttu-id="afe86-147">Bu basit Web API projesi için geçiş işlemi son adımda denetleyicileri ve kullandıkları modelleri kopyalamaktır.</span><span class="sxs-lookup"><span data-stu-id="afe86-147">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="afe86-148">Bu durumda, yalnızca kopyalama *Controllers/ProductsController.cs* yeni bir için özgün projeden.</span><span class="sxs-lookup"><span data-stu-id="afe86-148">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="afe86-149">Ardından, tüm modeller klasörü için yeni bir özgün projeden kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="afe86-149">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="afe86-150">Yeni proje adı ile eşleşmesi için ad alanları ayarlayın (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="afe86-150">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="afe86-151">Bu noktada, uygulama oluşturabilir ve derleme hatalarının sayısı bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="afe86-151">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="afe86-152">Bunlar genellikle aşağıdaki kategorilere ayrılır:</span><span class="sxs-lookup"><span data-stu-id="afe86-152">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="afe86-153">*ApiController* yok</span><span class="sxs-lookup"><span data-stu-id="afe86-153">*ApiController* does not exist</span></span>

* <span data-ttu-id="afe86-154">*System.Web.Http* ad alanı mevcut değil</span><span class="sxs-lookup"><span data-stu-id="afe86-154">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="afe86-155">*Ihttpactionresult* yok</span><span class="sxs-lookup"><span data-stu-id="afe86-155">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="afe86-156">Neyse ki, bu düzeltmesi tümü çok kolay şunlardır:</span><span class="sxs-lookup"><span data-stu-id="afe86-156">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="afe86-157">Değişiklik *ApiController* için *denetleyicisi* (eklemeniz gerekebilir *Microsoft.AspNetCore.Mvc kullanarak*)</span><span class="sxs-lookup"><span data-stu-id="afe86-157">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="afe86-158">Silmek için başvuran deyimiyle *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="afe86-158">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="afe86-159">Tüm yöntemi döndürme değiştirme *Ihttpactionresult* döndürmek için bir *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="afe86-159">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="afe86-160">Bu değişiklikler yapılmış ve kullanılmayan işlendikten sonra using deyimleri kaldırıldı, geçirilen *ProductsController* sınıfı şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="afe86-160">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="afe86-161">Şimdi geçirilen projeyi çalıştırın ve Gözat yapabiliyor olmanız gerekir */api/ürünleri*; ve 3 ürünlerinin tam listesini görmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="afe86-161">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="afe86-162">Gözat */api/products/1* ve ilk ürün görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="afe86-162">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="afe86-163">Özet</span><span class="sxs-lookup"><span data-stu-id="afe86-163">Summary</span></span>

<span data-ttu-id="afe86-164">Basit bir ASP.NET Web API projesi ASP.NET Core MVC geçirilmesi thanks ASP.NET Core MVC Web API'leri için yerleşik destek için oldukça kolay değildir.</span><span class="sxs-lookup"><span data-stu-id="afe86-164">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="afe86-165">Her ASP.NET Web API projesi geçiş yapmanız gerekecektir ana parça rotalara, denetleyicilere ve modeller, güncelleştirmeleri denetleyicileri ve eylemleri tarafından kullanılan türleri ile birlikte ' dir.</span><span class="sxs-lookup"><span data-stu-id="afe86-165">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
