---
title: ASP.NET Web API ASP.NET Core için geçirme
author: ardalis
description: Bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API'si geçirmeyi öğrenin.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894198"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="11a12-103">ASP.NET Web API ASP.NET Core için geçirme</span><span class="sxs-lookup"><span data-stu-id="11a12-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="11a12-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="11a12-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="11a12-105">Web istemcileri, tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaşan HTTP Hizmetleri apı'lerdir.</span><span class="sxs-lookup"><span data-stu-id="11a12-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="11a12-106">ASP.NET Core MVC web uygulamaları oluşturmanın tek, tutarlı bir yol sağlayan Web API'leri oluşturmaya yönelik destek içerir.</span><span class="sxs-lookup"><span data-stu-id="11a12-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="11a12-107">Bu makalede, bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API'si geçirmek için gereken adımları size gösterir.</span><span class="sxs-lookup"><span data-stu-id="11a12-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="11a12-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="11a12-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="11a12-109">ASP.NET Web API projesi gözden geçirme</span><span class="sxs-lookup"><span data-stu-id="11a12-109">Review ASP.NET Web API project</span></span>

<span data-ttu-id="11a12-110">Bu makalede örnek proje kullanan *ProductsApp*, makaledeki oluşturulmuş [ASP.NET Web API 2 ile çalışmaya başlama](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) , başlangıç noktası olarak.</span><span class="sxs-lookup"><span data-stu-id="11a12-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="11a12-111">Projede, basit bir ASP.NET Web API projesi şu şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="11a12-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="11a12-112">İçinde *Global.asax.cs*, için bir çağrı yapılır `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="11a12-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="11a12-113">`WebApiConfig` tanımlanan *App_Start*, ve yalnızca bir statik `Register` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="11a12-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

<span data-ttu-id="11a12-114">Bu sınıf yapılandırır [öznitelik yönlendirme](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), ancak aslında projede kullanılıyor.</span><span class="sxs-lookup"><span data-stu-id="11a12-114">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="11a12-115">Ayrıca, ASP.NET Web API'si tarafından kullanılan yönlendirme tablosunun yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="11a12-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="11a12-116">Bu durumda, ASP.NET Web API biçim ile eşleşmesi için URL'leri beklediği */api/ {denetleyici} / {id}*, ile *{id}* isteğe bağlı olan.</span><span class="sxs-lookup"><span data-stu-id="11a12-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="11a12-117">*ProductsApp* Proje öğesinden devralan tek bir basit denetleyicisi içeren `ApiController` ve iki yöntem sunar:</span><span class="sxs-lookup"><span data-stu-id="11a12-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="11a12-118">Son olarak, model *ürün*tarafından kullanılan *ProductsApp*, basit bir sınıf:</span><span class="sxs-lookup"><span data-stu-id="11a12-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="11a12-119">Basit bir proje başlayacağı sahibiz, biz bu Web API projesi için ASP.NET Core MVC geçirme gösterebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11a12-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="11a12-120">Hedef proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="11a12-120">Create the Destination Project</span></span>

<span data-ttu-id="11a12-121">Visual Studio kullanarak, yeni, boş bir çözüm oluşturun ve adlandırın *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="11a12-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="11a12-122">Mevcut yapıtı Ekle *ProductsApp* proje kendisine ve ardından, yeni bir ASP.NET Core Web uygulaması projesi çözüme ekleyin.</span><span class="sxs-lookup"><span data-stu-id="11a12-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="11a12-123">Yeni proje adını *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="11a12-123">Name the new project *ProductsCore*.</span></span>

![Yeni Proje iletişim kutusu açmak için Web Şablonları](webapi/_static/add-web-project.png)

<span data-ttu-id="11a12-125">Ardından, Web API proje şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="11a12-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="11a12-126">Taşımayı planlıyoruz *ProductsApp* içerikleri bu yeni proje için.</span><span class="sxs-lookup"><span data-stu-id="11a12-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core şablonları listesinde seçilen Web API proje şablonuyla yeni Web uygulaması iletişim kutusu](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="11a12-128">Silme `Project_Readme.html` dosyasından yeni bir proje.</span><span class="sxs-lookup"><span data-stu-id="11a12-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="11a12-129">Çözümünüz şöyle görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="11a12-129">Your solution should now look like this:</span></span>

![Dosya ve klasörleri ProductsApp ve ProductsCore projelerin gösteren Çözüm Gezgini içinde uygulama çözümü açın](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="11a12-131">Yapılandırma geçişi</span><span class="sxs-lookup"><span data-stu-id="11a12-131">Migrate Configuration</span></span>

<span data-ttu-id="11a12-132">ASP.NET Core bundan böyle *Global.asax*, *web.config*, veya *App_Start* klasörleri.</span><span class="sxs-lookup"><span data-stu-id="11a12-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="11a12-133">Bunun yerine, tüm başlangıç görevleri gerçekleştirilir *Startup.cs* proje kökündeki (bkz [uygulama başlatma](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="11a12-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="11a12-134">ASP.NET Core MVC, öznitelik tabanlı yönlendirme artık varsayılan olarak dahil edilir olduğunda `UseMvc()` çağrılır; ve bu Web API yolları yapılandırmak için önerilen bir yaklaşım (ve üretim Web API'si başlangıç projesini nasıl işleyeceğini).</span><span class="sxs-lookup"><span data-stu-id="11a12-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="11a12-135">İleriye dönük projenizde öznitelik yönlendirme kullanmak istediğiniz varsayılarak, ek bir yapılandırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="11a12-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="11a12-136">Aşağıdaki örnekte olduğu gibi denetleyicileri ve eylemleri gerektiği gibi öznitelikleri yalnızca uygulanır `ValuesController` Web API'si başlangıç projesine dahil sınıfı:</span><span class="sxs-lookup"><span data-stu-id="11a12-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="11a12-137">Varlığını Not *[controller]* 8. satırda.</span><span class="sxs-lookup"><span data-stu-id="11a12-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="11a12-138">Öznitelik tabanlı artık yönlendirmeyi destekleyen bazı belirteçler gibi *[controller]* ve *[action]*.</span><span class="sxs-lookup"><span data-stu-id="11a12-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="11a12-139">Bu belirteçler çalışma zamanında denetleyici veya eylemin, adıyla sırasıyla özniteliği uygulanmış değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="11a12-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="11a12-140">Bu projedeki Sihirli dize sayısını azaltmak için kullanılır ve otomatik yeniden adlandır yeniden düzenlemeler uygulandığında yolların kendi ilgili denetleyicileri ve eylemleri ile eşitlenmiş tutulacak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="11a12-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="11a12-141">Ürünleri API denetleyicisi geçirmek için biz öncelikle kopyalamalısınız *ProductsController* yeni projeye.</span><span class="sxs-lookup"><span data-stu-id="11a12-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="11a12-142">Ardından yalnızca denetleyici üzerinde rota özniteliğinin şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="11a12-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="11a12-143">Ayrıca eklemenize gerek `[HttpGet]` her ikisi de HTTP Get çağrılmalıdır beri iki yöntem için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="11a12-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="11a12-144">Özniteliği için bir "ID" parametresi beklentisi dahil `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="11a12-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="11a12-145">Bu noktada, yönlendirme düzgün yapılandırılıp yapılandırılmadığını; Ancak, henüz bunu test edilemez.</span><span class="sxs-lookup"><span data-stu-id="11a12-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="11a12-146">Ek değişiklik yaptıysanız, önce *ProductsController* derlenir.</span><span class="sxs-lookup"><span data-stu-id="11a12-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="11a12-147">Modelleri ve denetleyicileri geçirme</span><span class="sxs-lookup"><span data-stu-id="11a12-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="11a12-148">Bu basit bir Web API projesi için geçiş işlemi son adımda denetleyicileri ve kullandıkları herhangi bir model kopyalamaktır.</span><span class="sxs-lookup"><span data-stu-id="11a12-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="11a12-149">Bu durumda, sadece kopyalayın *Controllers/ProductsController.cs* özgün proje için yeni bir tane.</span><span class="sxs-lookup"><span data-stu-id="11a12-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="11a12-150">Ardından, tüm modeller klasörü özgün projeden yeni bir tane kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="11a12-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="11a12-151">Yeni Proje adıyla eşleşecek şekilde ad alanlarını Ayarla (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="11a12-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="11a12-152">Bu noktada, uygulama oluşturabilir ve derleme hatalarının sayısını görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="11a12-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="11a12-153">Bunlar, genellikle aşağıdaki kategorilere ayrılır:</span><span class="sxs-lookup"><span data-stu-id="11a12-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="11a12-154">*ApiController* yok</span><span class="sxs-lookup"><span data-stu-id="11a12-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="11a12-155">*System.Web.Http* ad alanı mevcut değil</span><span class="sxs-lookup"><span data-stu-id="11a12-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="11a12-156">*Ihttpactionresult* yok</span><span class="sxs-lookup"><span data-stu-id="11a12-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="11a12-157">Neyse ki, bunları düzeltmek tüm çok kolaydır:</span><span class="sxs-lookup"><span data-stu-id="11a12-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="11a12-158">Değişiklik *ApiController* için *denetleyicisi* (eklemeniz gerekebilir *Microsoft.AspNetCore.Mvc kullanarak*)</span><span class="sxs-lookup"><span data-stu-id="11a12-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="11a12-159">Silmek başvuran deyimini *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="11a12-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="11a12-160">Döndüren bir yöntemi değiştirme *Ihttpactionresult* döndürülecek bir *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="11a12-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="11a12-161">Bu değişiklikler yapılmış ve kullanılmayan ulaştıktan sonra using deyimlerini kaldırıldıysa, geçirilen *ProductsController* sınıfı aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="11a12-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="11a12-162">Artık geçirilen projeyi çalıştırın ve göz atın olmalıdır */api/ürünleri*; ve 3 ürünlerin tam listesini görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="11a12-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="11a12-163">Gözat */api/products/1* ve ilk ürün görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="11a12-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a><span data-ttu-id="11a12-164">ASP.NET 4.x Web API 2 uyumluluk dolgusu</span><span class="sxs-lookup"><span data-stu-id="11a12-164">ASP.NET 4.x Web API 2 compatibility shim</span></span>

<span data-ttu-id="11a12-165">Geçirme ASP.NET Web API ASP.NET Core projelerinde, kullanışlı bir araçtır [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="11a12-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="11a12-166">ASP.NET Core, kullanılacak farklı bir Web API 2 kuralları sayısı izin vermek için Uyumluluk dolgu genişletir.</span><span class="sxs-lookup"><span data-stu-id="11a12-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="11a12-167">Bu belgede daha önce unity'nin örnek uyumluluk dolgu gerekli olmadığını yeterince temel üyeliktir.</span><span class="sxs-lookup"><span data-stu-id="11a12-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="11a12-168">Daha büyük projeler için Uyumluluk dolgu kullanarak geçici olarak ASP.NET Web API 2 ile ASP.NET Core arasındaki API açığını köprüleme yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="11a12-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="11a12-169">Web API Uyumluluk dolgu geçirme büyük Web API projelerini ASP.NET core'a kolaylaştırmak için geçici bir önlem kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="11a12-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="11a12-170">Zaman içinde projeler üzerinde uyumluluğu dolgu güvenmek yerine, ASP.NET Core düzenlerinin kullanılacağı güncelleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="11a12-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="11a12-171">Uyumluluk özellikleri dahil `Microsoft.AspNetCore.Mvc.WebApiCompatShim` içerir:</span><span class="sxs-lookup"><span data-stu-id="11a12-171">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="11a12-172">Ekler bir `ApiController` denetleyicileri temel türleri güncelleştirilmesi gerekmeyen yazın.</span><span class="sxs-lookup"><span data-stu-id="11a12-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="11a12-173">Web API stili model bağlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="11a12-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="11a12-174">ASP.NET Core MVC model bağlama işlevleri varsayılan olarak, MVC 5 benzer şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="11a12-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="11a12-175">Uyumluluk dolgu değişiklikleri model daha Web API 2 model bağlama kurallarına benzer şekilde bağlama.</span><span class="sxs-lookup"><span data-stu-id="11a12-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="11a12-176">Örneğin, karmaşık türler gövdeden otomatik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="11a12-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="11a12-177">Denetleyici eylemleri türünde parametre yararlanabilmeniz model bağlama genişletir `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="11a12-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="11a12-178">İleti biçimlendiriciler eylemleri izin verme türü sonuçları döndürmek için ekler `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="11a12-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="11a12-179">Web API 2 eylemleri yanıtlar verecek kullanmış olabilirsiniz ek yanıt yöntemleri ekler:</span><span class="sxs-lookup"><span data-stu-id="11a12-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="11a12-180">HttpResponseMessage oluşturucuları:</span><span class="sxs-lookup"><span data-stu-id="11a12-180">HttpResponseMessage generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="11a12-181">Eylem sonucu yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="11a12-181">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="11a12-182">Bir örneğini ekler `IContentNegotiator` uygulamanın DI kapsayıcıya ve içerik anlaşması ile ilgili türlerinden yapar [System.NET.http.Formatting](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="11a12-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="11a12-183">Bu gibi türlerini içerir `DefaultContentNegotiator`, `MediaTypeFormatter`vb..</span><span class="sxs-lookup"><span data-stu-id="11a12-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="11a12-184">Uyumluluk dolgu kullanmak üzere için gerekir:</span><span class="sxs-lookup"><span data-stu-id="11a12-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="11a12-185">Yükleme [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="11a12-185">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="11a12-186">Çağırarak uygulamanın DI kapsayıcı ile uyumluluk dolgu ait Hizmetleri kaydedin `services.AddMvc().AddWebApiConventions()` uygulamasının `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="11a12-186">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="11a12-187">Web API özel yollar kullanarak tanımlama `MapWebApiRoute` üzerinde `IRouteBuilder` uygulamasının `IApplicationBuilder.UseMvc` çağırın.</span><span class="sxs-lookup"><span data-stu-id="11a12-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="11a12-188">Özet</span><span class="sxs-lookup"><span data-stu-id="11a12-188">Summary</span></span>

<span data-ttu-id="11a12-189">ASP.NET Core MVC için basit bir ASP.NET Web API projesi geçirme yararlanabilirsiniz ASP.NET Core MVC Web API'leri için yerleşik destek oldukça basittir.</span><span class="sxs-lookup"><span data-stu-id="11a12-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="11a12-190">Her bir ASP.NET Web API projesi geçiş yapmanız gerekecektir ana parçaları rotalara, denetleyicilere ve modeller, denetleyicilere ve eylemlere tarafından kullanılan türleri güncelleştirmeleri ile birlikte ' dir.</span><span class="sxs-lookup"><span data-stu-id="11a12-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
