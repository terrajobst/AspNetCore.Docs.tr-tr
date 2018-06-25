---
title: ASP.NET Web API ASP.NET Core geçirme
author: ardalis
description: Bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API geçirmek öğrenin.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 4f4dc140bd60463037be0757176dcf7a619918bd
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327515"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="746ff-103">ASP.NET Web API ASP.NET Core geçirme</span><span class="sxs-lookup"><span data-stu-id="746ff-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="746ff-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="746ff-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="746ff-105">Web API'leri istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP hizmetleridir.</span><span class="sxs-lookup"><span data-stu-id="746ff-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="746ff-106">ASP.NET Core MVC web uygulamaları oluşturma tek, tutarlı bir yol sağlayarak Web API'ları oluşturmak için destek içerir.</span><span class="sxs-lookup"><span data-stu-id="746ff-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="746ff-107">Bu makalede, bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API geçirmek için gereken adımları gösterir.</span><span class="sxs-lookup"><span data-stu-id="746ff-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="746ff-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="746ff-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="746ff-109">Gözden geçirme ASP.NET Web API projesi</span><span class="sxs-lookup"><span data-stu-id="746ff-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="746ff-110">Bu makalede örnek proje kullanan *ProductsApp*, makalede oluşturulmuş [ASP.NET Web API 2 ile çalışmaya başlama](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) , başlangıç noktası olarak.</span><span class="sxs-lookup"><span data-stu-id="746ff-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="746ff-111">Bu projede basit bir ASP.NET Web API projesi şu şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="746ff-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="746ff-112">İçinde *Global.asax.cs*, için bir çağrı yapılır `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="746ff-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="746ff-113">`WebApiConfig` tanımlanan *App_Start*, ve yalnızca bir statik `Register` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="746ff-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="746ff-114">Bu sınıf yapılandırır [özniteliği yönlendirme](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aslında projesinde kullanılan rağmen.</span><span class="sxs-lookup"><span data-stu-id="746ff-114">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="746ff-115">Ayrıca, ASP.NET Web API tarafından kullanılan yönlendirme tablosu yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="746ff-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="746ff-116">Bu durumda, ASP.NET Web API biçim ile eşleşmesi için URL'leri beklediği */api/ {controller} / {id}*, ile *{id}* isteğe bağlı olması.</span><span class="sxs-lookup"><span data-stu-id="746ff-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="746ff-117">*ProductsApp* proje içeriyor devraldığı tek bir basit denetleyicisi `ApiController` ve iki yöntem sunar:</span><span class="sxs-lookup"><span data-stu-id="746ff-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="746ff-118">Son olarak, model, *ürün*, tarafından kullanılan *ProductsApp*, basit bir sınıf:</span><span class="sxs-lookup"><span data-stu-id="746ff-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="746ff-119">Biz basit bir proje başlayacağı sahip olduğunuza göre Biz bu Web API projesi ASP.NET Core MVC geçirmek nasıl ekleyebileceğiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="746ff-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="746ff-120">Hedef projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="746ff-120">Create the Destination Project</span></span>

<span data-ttu-id="746ff-121">Visual Studio kullanarak, yeni, boş bir çözüm oluşturun ve adlandırın *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="746ff-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="746ff-122">Varolan eklemek *ProductsApp* ona proje sonra yeni bir ASP.NET çekirdek Web uygulama projesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="746ff-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="746ff-123">Yeni proje adı *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="746ff-123">Name the new project *ProductsCore*.</span></span>

![Yeni Proje iletişim kutusu açmak için Web Şablonları](webapi/_static/add-web-project.png)

<span data-ttu-id="746ff-125">Ardından, Web API projesi şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="746ff-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="746ff-126">Planlıyoruz *ProductsApp* bu yeni projeye içeriği.</span><span class="sxs-lookup"><span data-stu-id="746ff-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core şablonları listesinde seçilen Web API projesi şablonuyla yeni Web uygulaması iletişim kutusu](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="746ff-128">Silme `Project_Readme.html` yeni proje dosyasından.</span><span class="sxs-lookup"><span data-stu-id="746ff-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="746ff-129">Çözümünüzü gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="746ff-129">Your solution should now look like this:</span></span>

![Uygulama çözüm dosyaları ve klasörleri ProductsApp ve ProductsCore projelerin gösteren Çözüm Gezgini'nde Aç](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="746ff-131">Yapılandırmasını geçir</span><span class="sxs-lookup"><span data-stu-id="746ff-131">Migrate Configuration</span></span>

<span data-ttu-id="746ff-132">ASP.NET Core artık kullanan *Global.asax*, *web.config*, veya *App_Start* klasörler.</span><span class="sxs-lookup"><span data-stu-id="746ff-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="746ff-133">Bunun yerine, tüm başlangıç görevleri yapılır *haline* proje kökündeki (bkz [uygulama başlangıç](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="746ff-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="746ff-134">ASP.NET Core MVC'de öznitelik tabanlı yönlendirme artık varsayılan olarak dahil edilmiştir, `UseMvc()` olarak adlandırılır; ve bu Web API yolları yapılandırmak için önerilen yaklaşımdır (ve Web API başlangıç projesi yönlendirme nasıl işler).</span><span class="sxs-lookup"><span data-stu-id="746ff-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="746ff-135">Öznitelik ileride projenizde yönlendirme kullanmak istediğiniz varsayıldığında, ek bir yapılandırma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="746ff-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="746ff-136">Yalnızca örnek gerçekleştirilir gibi denetleyicileri ve eylemleri gerektiği gibi öznitelikleri uygulanır `ValuesController` Web API starter projeye dahil sınıfı:</span><span class="sxs-lookup"><span data-stu-id="746ff-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="746ff-137">Varlığını Not *[denetleyicisi]* 8 satırındaki.</span><span class="sxs-lookup"><span data-stu-id="746ff-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="746ff-138">Öznitelik tabanlı şimdi yönlendirmeyi destekleyen belirli belirteçleri gibi *[denetleyicisi]* ve *[eylem]*.</span><span class="sxs-lookup"><span data-stu-id="746ff-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="746ff-139">Bu belirteçler çalışma zamanında denetleyici veya eylemin, adı ile sırasıyla öznitelik uygulanmış değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="746ff-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="746ff-140">Bu proje Sihirli dizelerde sayısını azaltmak için kullanılır ve otomatik yeniden adlandırma yapan yeniden düzenlemeler uygulandığında yolların kendi ilgili denetleyicileri ve eylemleri ile eşitlenmiş tutulacak sağlar.</span><span class="sxs-lookup"><span data-stu-id="746ff-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="746ff-141">Ürünler API denetleyicisi geçirmek için biz öncelikle kopyalamanız gerekir *ProductsController* yeni projeye.</span><span class="sxs-lookup"><span data-stu-id="746ff-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="746ff-142">Ardından denetleyicisinde yol özniteliği yalnızca şunlardır:</span><span class="sxs-lookup"><span data-stu-id="746ff-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="746ff-143">Ayrıca eklemeniz gerekir `[HttpGet]` özniteliği için iki yöntem, her ikisi de HTTP Get çağrılmalıdır beri.</span><span class="sxs-lookup"><span data-stu-id="746ff-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="746ff-144">Öznitelik için bir "id" parametresi beklentisi dahil `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="746ff-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="746ff-145">Bu noktada, yönlendirme doğru yapılandırılmış; Ancak, henüz bunu test edemiyor.</span><span class="sxs-lookup"><span data-stu-id="746ff-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="746ff-146">Ek değişiklikler yapılan, önce *ProductsController* derlenir.</span><span class="sxs-lookup"><span data-stu-id="746ff-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="746ff-147">Modelleri ve denetleyiciler geçirme</span><span class="sxs-lookup"><span data-stu-id="746ff-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="746ff-148">Bu basit Web API projesi için geçiş işlemi son adımda denetleyicileri ve kullandıkları modelleri kopyalamaktır.</span><span class="sxs-lookup"><span data-stu-id="746ff-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="746ff-149">Bu durumda, yalnızca kopyalama *Controllers/ProductsController.cs* yeni bir için özgün projeden.</span><span class="sxs-lookup"><span data-stu-id="746ff-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="746ff-150">Ardından, tüm modeller klasörü için yeni bir özgün projeden kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="746ff-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="746ff-151">Yeni proje adı ile eşleşmesi için ad alanları ayarlayın (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="746ff-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="746ff-152">Bu noktada, uygulama oluşturabilir ve derleme hatalarının sayısı bulacaksınız.</span><span class="sxs-lookup"><span data-stu-id="746ff-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="746ff-153">Bunlar genellikle aşağıdaki kategorilere ayrılır:</span><span class="sxs-lookup"><span data-stu-id="746ff-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="746ff-154">*ApiController* yok</span><span class="sxs-lookup"><span data-stu-id="746ff-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="746ff-155">*System.Web.Http* ad alanı mevcut değil</span><span class="sxs-lookup"><span data-stu-id="746ff-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="746ff-156">*Ihttpactionresult* yok</span><span class="sxs-lookup"><span data-stu-id="746ff-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="746ff-157">Neyse ki, bu düzeltmesi tümü çok kolay şunlardır:</span><span class="sxs-lookup"><span data-stu-id="746ff-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="746ff-158">Değişiklik *ApiController* için *denetleyicisi* (eklemeniz gerekebilir *Microsoft.AspNetCore.Mvc kullanarak*)</span><span class="sxs-lookup"><span data-stu-id="746ff-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="746ff-159">Silmek için başvuran deyimiyle *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="746ff-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="746ff-160">Tüm yöntemi döndürme değiştirme *Ihttpactionresult* döndürmek için bir *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="746ff-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="746ff-161">Bu değişiklikler yapılmış ve kullanılmayan işlendikten sonra using deyimleri kaldırıldı, geçirilen *ProductsController* sınıfı şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="746ff-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="746ff-162">Şimdi geçirilen projeyi çalıştırın ve Gözat yapabiliyor olmanız gerekir */api/ürünleri*; ve 3 ürünlerinin tam listesini görmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="746ff-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="746ff-163">Gözat */api/products/1* ve ilk ürün görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="746ff-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a><span data-ttu-id="746ff-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="746ff-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span></span>

<span data-ttu-id="746ff-165">ASP.NET Core geçirme ASP.NET Web API projeleri zaman yararlı bir araçtır [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) kitaplığı.</span><span class="sxs-lookup"><span data-stu-id="746ff-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="746ff-166">Uyumluluk dolgusu kullanılmak üzere farklı Web API 2 kuralları sayısı izin vermek için ASP.NET Core genişletir.</span><span class="sxs-lookup"><span data-stu-id="746ff-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="746ff-167">Bu belgede daha önce bağlantı noktalı uyumluluk dolgusu gerekli değildi yeterince temel örnektir.</span><span class="sxs-lookup"><span data-stu-id="746ff-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="746ff-168">Büyük projeler için uyumluluk dolgusu kullanarak geçici olarak API boşluk ASP.NET Core ve ASP.NET Web API 2 arasında köprü oluşturma için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="746ff-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="746ff-169">Web API uyumluluk dolgusu geçici bir ölçü olarak ASP.NET Core büyük geçirme Web API projeleri kolaylaştırmak için kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="746ff-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="746ff-170">ASP.NET Core desenleri uyumluluk dolgusu güvenmek yerine kullanmak için zaman içinde projeleri güncelleştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="746ff-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span> 

<span data-ttu-id="746ff-171">İçinde Microsoft.AspNetCore.Mvc.WebApiCompatShim dahil uyumluluk özellikleri içerir:</span><span class="sxs-lookup"><span data-stu-id="746ff-171">Compatibility features included in Microsoft.AspNetCore.Mvc.WebApiCompatShim include:</span></span>

* <span data-ttu-id="746ff-172">Ekler bir `ApiController` denetleyicileri taban türleri güncelleştirilecek gerekmemesi yazın.</span><span class="sxs-lookup"><span data-stu-id="746ff-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="746ff-173">Web API stili model bağlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="746ff-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="746ff-174">ASP.NET Core MVC model bağlama işlevleri varsayılan MVC 5 benzer şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="746ff-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="746ff-175">Uyumluluk dolgusu değişiklikleri Web API 2 model bağlama kurallarına daha benzer şekilde bağlama model.</span><span class="sxs-lookup"><span data-stu-id="746ff-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="746ff-176">Örneğin, karmaşık türler isteği gövdesinden otomatik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="746ff-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="746ff-177">Denetleyici eylemleri türünde parametre yararlanabilmeniz model bağlama genişletir `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="746ff-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="746ff-178">Eylemler izin verme iletisi biçimlendiricileri'türünde sonuçlar döndürecek şekilde ekler `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="746ff-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="746ff-179">Web API 2 Eylemler yanıtları sunmak için kullanılan başka yanıt yöntemleri ekler:</span><span class="sxs-lookup"><span data-stu-id="746ff-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
    * <span data-ttu-id="746ff-180">Bilgisayarın HttpResponseMessage oluşturucuları:</span><span class="sxs-lookup"><span data-stu-id="746ff-180">HttpResponseMessage generators:</span></span>
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * <span data-ttu-id="746ff-181">Eylem sonucu yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="746ff-181">Action result methods:</span></span>
        * `BadRequestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* <span data-ttu-id="746ff-182">Bir örneğini ekler `IContentNegotiator` uygulamanın dı kapsayıcısı ve yapar içerik anlaşması ile ilgili türlerinden [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="746ff-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="746ff-183">Bu gibi türlerini içerir `DefaultContentNegotiator`, `MediaTypeFormatter`vb.</span><span class="sxs-lookup"><span data-stu-id="746ff-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="746ff-184">Uyumluluk dolgusu kullanmak için aktarmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="746ff-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="746ff-185">Başvuru [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="746ff-185">Reference the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="746ff-186">Çağırarak uygulamanın dı kapsayıcı ile uyumluluk dolgusu ait Hizmetleri kaydedin `services.AddWebApiConventions()` uygulamanın içinde `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="746ff-186">Register the compatibility shim's services with the app's DI container by calling `services.AddWebApiConventions()` in the application's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="746ff-187">Web API özel yollar kullanılarak tanımlamak `MapWebApiRoute` üzerinde `IRouteBuilder` uygulamanın içinde `IApplicationBuilder.UseMvc` çağırın.</span><span class="sxs-lookup"><span data-stu-id="746ff-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the application's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="746ff-188">Özet</span><span class="sxs-lookup"><span data-stu-id="746ff-188">Summary</span></span>

<span data-ttu-id="746ff-189">Basit bir ASP.NET Web API projesi ASP.NET Core MVC geçirilmesi thanks ASP.NET Core MVC Web API'leri için yerleşik destek için oldukça kolay değildir.</span><span class="sxs-lookup"><span data-stu-id="746ff-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="746ff-190">Her ASP.NET Web API projesi geçiş yapmanız gerekecektir ana parça rotalara, denetleyicilere ve modeller, güncelleştirmeleri denetleyicileri ve eylemleri tarafından kullanılan türleri ile birlikte ' dir.</span><span class="sxs-lookup"><span data-stu-id="746ff-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
