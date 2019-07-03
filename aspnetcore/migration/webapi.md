---
title: ASP.NET Web API ASP.NET Core için geçirme
author: ardalis
description: Bir web API uygulaması için ASP.NET Core MVC ASP.NET 4.x Web API'si geçirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/10/2018
uid: migration/webapi
ms.openlocfilehash: 74c7730a667ebc979241489733cdace149cacdf2
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555747"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="8ba51-103">ASP.NET Web API ASP.NET Core için geçirme</span><span class="sxs-lookup"><span data-stu-id="8ba51-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="8ba51-104">Tarafından [Scott Addie](https://twitter.com/scott_addie) ve [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8ba51-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8ba51-105">ASP.NET 4.x Web API'si, istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaştığında bir HTTP hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="8ba51-106">ASP.NET Core, ASP.NET 4.x'ın MVC birleştirir ve ASP.NET Core MVC bilinen basit bir programlama modeli içinde Web API uygulaması modeller.</span><span class="sxs-lookup"><span data-stu-id="8ba51-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="8ba51-107">Bu makalede, ASP.NET 4.x Web API'si için ASP.NET Core MVC geçirmek için gereken adımları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="8ba51-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8ba51-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8ba51-109">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="8ba51-109">Prerequisites</span></span>

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="8ba51-110">ASP.NET 4.x Web API projesi gözden geçirin</span><span class="sxs-lookup"><span data-stu-id="8ba51-110">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="8ba51-111">Bu makalede bir başlangıç noktası olarak kullandığı *ProductsApp* oluşturulan proje [ASP.NET Web API 2 ile çalışmaya başlama](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="8ba51-111">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="8ba51-112">Projede, basit bir ASP.NET 4.x Web API projesi şu şekilde yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="8ba51-112">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="8ba51-113">İçinde *Global.asax.cs*, için bir çağrı yapılır `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="8ba51-113">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="8ba51-114">`WebApiConfig` Sınıfı içinde bulunan *App_Start* klasör ve bir statik `Register` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8ba51-114">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="8ba51-115">Bu sınıf yapılandırır [öznitelik yönlendirme](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), ancak aslında projede kullanılıyor.</span><span class="sxs-lookup"><span data-stu-id="8ba51-115">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="8ba51-116">Ayrıca, ASP.NET Web API'si tarafından kullanılan yönlendirme tablosunun yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8ba51-116">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="8ba51-117">Bu durumda, biçim ile eşleşmesi için URL ASP.NET 4.x Web API bekliyor `/api/{controller}/{id}`, ile `{id}` isteğe bağlı olan.</span><span class="sxs-lookup"><span data-stu-id="8ba51-117">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="8ba51-118">*ProductsApp* proje bir denetleyici içerir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-118">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="8ba51-119">Denetleyici devraldığı `ApiController` ve iki eylemleri içerir:</span><span class="sxs-lookup"><span data-stu-id="8ba51-119">The controller inherits from `ApiController` and contains two actions:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

<span data-ttu-id="8ba51-120">`Product` Modeli tarafından kullanılan `ProductsController` basit sınıfı:</span><span class="sxs-lookup"><span data-stu-id="8ba51-120">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="8ba51-121">Aşağıdaki bölümlerde ASP.NET Core MVC Web API projesi geçişini gösterir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-121">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="8ba51-122">Hedef proje oluşturma</span><span class="sxs-lookup"><span data-stu-id="8ba51-122">Create destination project</span></span>

<span data-ttu-id="8ba51-123">Visual Studio'da aşağıdaki adımları tamamlayın:</span><span class="sxs-lookup"><span data-stu-id="8ba51-123">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="8ba51-124">Git **dosya** > **yeni** > **proje** > **diğer proje türleri**  >  **Visual Studio çözümleri**.</span><span class="sxs-lookup"><span data-stu-id="8ba51-124">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="8ba51-125">Seçin **boş çözüm**ve çözüm adı *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="8ba51-125">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="8ba51-126">Tıklayın **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="8ba51-126">Click the **OK** button.</span></span>
* <span data-ttu-id="8ba51-127">Mevcut yapıtı Ekle *ProductsApp* çözüme bir proje.</span><span class="sxs-lookup"><span data-stu-id="8ba51-127">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="8ba51-128">Yeni bir **ASP.NET Core Web uygulaması** çözüme bir proje.</span><span class="sxs-lookup"><span data-stu-id="8ba51-128">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="8ba51-129">Seçin **.NET Core** hedef framework'açılır ve seçin **API** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="8ba51-129">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="8ba51-130">Projeyi adlandırın *ProductsCore*, tıklatıp **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="8ba51-130">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="8ba51-131">Çözüm, artık iki proje içermektedir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-131">The solution now contains two projects.</span></span> <span data-ttu-id="8ba51-132">Aşağıdaki bölümlerde, geçiş açıklanmaktadır *ProductsApp* projenin içeriğini *ProductsCore* proje.</span><span class="sxs-lookup"><span data-stu-id="8ba51-132">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="8ba51-133">Yapılandırma geçişi</span><span class="sxs-lookup"><span data-stu-id="8ba51-133">Migrate configuration</span></span>

<span data-ttu-id="8ba51-134">ASP.NET Core kullanmaz *App_Start* klasör veya *Global.asax* dosyasını ve *web.config* dosya eklendiğinde yayımlama zamanı.</span><span class="sxs-lookup"><span data-stu-id="8ba51-134">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="8ba51-135">*Startup.cs* ardılı olan *Global.asax* ve proje kök dizininde bulunur.</span><span class="sxs-lookup"><span data-stu-id="8ba51-135">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="8ba51-136">`Startup` Sınıfı, tüm uygulama başlatma görevlerini işler.</span><span class="sxs-lookup"><span data-stu-id="8ba51-136">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="8ba51-137">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="8ba51-137">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="8ba51-138">ASP.NET Core MVC öznitelik yönlendirme varsayılan olarak dahil edilir olduğunda <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> çağrılma yeri `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="8ba51-138">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="8ba51-139">Aşağıdaki `UseMvc` çağrı değiştirir *ProductsApp* projenin *App_Start/WebApiConfig.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8ba51-139">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="8ba51-140">Modelleri ve denetleyicileri geçirme</span><span class="sxs-lookup"><span data-stu-id="8ba51-140">Migrate models and controllers</span></span>

<span data-ttu-id="8ba51-141">Kopyalayabilirsiniz *ProductApp* projenin denetleyici ve modeli kullanır.</span><span class="sxs-lookup"><span data-stu-id="8ba51-141">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="8ba51-142">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="8ba51-142">Follow these steps:</span></span>

1. <span data-ttu-id="8ba51-143">Kopyalama *Controllers/ProductsController.cs* özgün proje için yeni bir tane.</span><span class="sxs-lookup"><span data-stu-id="8ba51-143">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="8ba51-144">Tamamını *modelleri* özgün proje klasörüne yeni bir tane.</span><span class="sxs-lookup"><span data-stu-id="8ba51-144">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="8ba51-145">Yeni Proje adıyla eşleşmesi için kopyalanan dosyalar ad alanlarını değiştirmek (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="8ba51-145">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="8ba51-146">Ayarlama `using ProductsApp.Models;` deyiminde *ProductsController.cs* çok.</span><span class="sxs-lookup"><span data-stu-id="8ba51-146">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="8ba51-147">Bu noktada, derleme hataları çeşitli uygulama sonuçları oluşturma.</span><span class="sxs-lookup"><span data-stu-id="8ba51-147">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="8ba51-148">Aşağıdaki bileşenler de ASP.NET Core mevcut olmaması nedeniyle hatalar oluşur:</span><span class="sxs-lookup"><span data-stu-id="8ba51-148">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="8ba51-149">`ApiController` Sınıfı</span><span class="sxs-lookup"><span data-stu-id="8ba51-149">`ApiController` class</span></span>
* <span data-ttu-id="8ba51-150">`System.Web.Http` Namespace</span><span class="sxs-lookup"><span data-stu-id="8ba51-150">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="8ba51-151">`IHttpActionResult` Arabirimi</span><span class="sxs-lookup"><span data-stu-id="8ba51-151">`IHttpActionResult` interface</span></span>

<span data-ttu-id="8ba51-152">Şu şekilde hataları düzeltin:</span><span class="sxs-lookup"><span data-stu-id="8ba51-152">Fix the errors as follows:</span></span>

1. <span data-ttu-id="8ba51-153">Değişiklik `ApiController` için <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="8ba51-153">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="8ba51-154">Ekleme `using Microsoft.AspNetCore.Mvc;` çözümlenecek `ControllerBase` başvuru.</span><span class="sxs-lookup"><span data-stu-id="8ba51-154">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="8ba51-155">`using System.Web.Http;` klasörünü silin.</span><span class="sxs-lookup"><span data-stu-id="8ba51-155">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="8ba51-156">Değişiklik `GetProduct` eylemin dönüş türünden `IHttpActionResult` için `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="8ba51-156">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

<span data-ttu-id="8ba51-157">Basitleştirmek `GetProduct` eylemin `return` aşağıdaki deyimi:</span><span class="sxs-lookup"><span data-stu-id="8ba51-157">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

```csharp
return product;
```

## <a name="configure-routing"></a><span data-ttu-id="8ba51-158">Yönlendirmeyi Yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8ba51-158">Configure routing</span></span>

<span data-ttu-id="8ba51-159">Yönlendirmeyi şu şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="8ba51-159">Configure routing as follows:</span></span>

1. <span data-ttu-id="8ba51-160">Süslemek `ProductsController` aşağıdaki özniteliklerle sınıfı:</span><span class="sxs-lookup"><span data-stu-id="8ba51-160">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="8ba51-161">Önceki [[yol]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) özniteliği, denetleyicinin öznitelik yönlendirme deseni yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="8ba51-161">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="8ba51-162">[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği öznitelik bu denetleyicide tüm eylemler için bir gereksinim yönlendirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="8ba51-162">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="8ba51-163">Öznitelik yönlendirme belirteçleri destekler, gibi `[controller]` ve `[action]`.</span><span class="sxs-lookup"><span data-stu-id="8ba51-163">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="8ba51-164">Çalışma zamanında her belirteç denetleyici veya eylemin, adıyla sırasıyla özniteliği uygulanmış değiştirilir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-164">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="8ba51-165">Belirteçler, projedeki Sihirli dize sayısını azaltın.</span><span class="sxs-lookup"><span data-stu-id="8ba51-165">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="8ba51-166">Belirteçleri de yollar ilgili denetleyicileri ile eşitlenmiş olarak kalır ve otomatik yeniden adlandırdığınızda yeniden düzenlemeler eylemleri uygulanan emin olun.</span><span class="sxs-lookup"><span data-stu-id="8ba51-166">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="8ba51-167">Projenin uyumluluk modu için ASP.NET Core 2.2 ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="8ba51-167">Set the project's compatibility mode to ASP.NET Core 2.2:</span></span>

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    <span data-ttu-id="8ba51-168">Yukarıdaki değişikliği:</span><span class="sxs-lookup"><span data-stu-id="8ba51-168">The preceding change:</span></span>

    * <span data-ttu-id="8ba51-169">Kullanmak için gerekli `[ApiController]` denetleyici düzeyinde özniteliği.</span><span class="sxs-lookup"><span data-stu-id="8ba51-169">Is required to use the `[ApiController]` attribute at the controller level.</span></span>
    * <span data-ttu-id="8ba51-170">ASP.NET Core 2.2 içinde tanıtılan davranışları bozucu olabilecek için kabul eder.</span><span class="sxs-lookup"><span data-stu-id="8ba51-170">Opts in to potentially breaking behaviors introduced in ASP.NET Core 2.2.</span></span>
1. <span data-ttu-id="8ba51-171">HTTP Get isteklerini etkinleştirme `ProductController` eylemler:</span><span class="sxs-lookup"><span data-stu-id="8ba51-171">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="8ba51-172">Uygulama [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) özniteliğini `GetAllProducts` eylem.</span><span class="sxs-lookup"><span data-stu-id="8ba51-172">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="8ba51-173">Uygulama `[HttpGet("{id}")]` özniteliğini `GetProduct` eylem.</span><span class="sxs-lookup"><span data-stu-id="8ba51-173">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="8ba51-174">Önceki değişiklikler ve kullanılmayan kaldırılmasını sonra `using` deyimleri *ProductsController.cs* dosya şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="8ba51-174">After the preceding changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="8ba51-175">Geçirilen proje çalıştırın ve göz atın `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="8ba51-175">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="8ba51-176">Üç ürün tam bir listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-176">A full list of three products appears.</span></span> <span data-ttu-id="8ba51-177">konumuna gözatın `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="8ba51-177">Browse to `/api/products/1`.</span></span> <span data-ttu-id="8ba51-178">İlk ürün görünür.</span><span class="sxs-lookup"><span data-stu-id="8ba51-178">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="8ba51-179">Uyumluluk dolgusu</span><span class="sxs-lookup"><span data-stu-id="8ba51-179">Compatibility shim</span></span>

<span data-ttu-id="8ba51-180">[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) kitaplığı, ASP.NET 4.x Web API projelerini ASP.NET Core taşımak için Uyumluluk dolgu sağlar.</span><span class="sxs-lookup"><span data-stu-id="8ba51-180">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="8ba51-181">Uyumluluk dolgu, ASP.NET Core, ASP.NET 4.x Web API 2 kurallarından sayısını destekleyecek şekilde genişletir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-181">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="8ba51-182">Bu belgede daha önce unity'nin örnek uyumluluk dolgu gereksiz yeterince temel üyeliktir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-182">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="8ba51-183">Daha büyük projeler için Uyumluluk dolgu kullanarak ASP.NET Core ve ASP.NET 4.x Web API 2 arasındaki API açığını geçici olarak köprüleme yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-183">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="8ba51-184">Web API Uyumluluk dolgu geçirme büyük ASP.NET 4.x Web API projelerini ASP.NET core'a desteklemek için geçici bir önlem kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8ba51-184">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="8ba51-185">Zaman içinde projeler üzerinde uyumluluğu dolgu güvenmek yerine, ASP.NET Core düzenlerinin kullanılacağı güncelleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="8ba51-185">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="8ba51-186">Uyumluluk özellikleri dahil `Microsoft.AspNetCore.Mvc.WebApiCompatShim` içerir:</span><span class="sxs-lookup"><span data-stu-id="8ba51-186">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="8ba51-187">Ekler bir `ApiController` denetleyicileri temel türleri güncelleştirilmesi gerekmeyen yazın.</span><span class="sxs-lookup"><span data-stu-id="8ba51-187">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="8ba51-188">Web API stili model bağlama sağlar.</span><span class="sxs-lookup"><span data-stu-id="8ba51-188">Enables Web API-style model binding.</span></span> <span data-ttu-id="8ba51-189">ASP.NET Core MVC, model bağlama işlevlerine benzer şekilde, ASP.NET 4.x MVC 5, varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="8ba51-189">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="8ba51-190">Uyumluluk dolgu değişiklikleri model daha ASP.NET 4.x Web API 2 model bağlama kurallarına benzer şekilde bağlama.</span><span class="sxs-lookup"><span data-stu-id="8ba51-190">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="8ba51-191">Örneğin, karmaşık türler gövdeden otomatik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="8ba51-191">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="8ba51-192">Denetleyici eylemleri türünde parametre yararlanabilmeniz model bağlama genişletir `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="8ba51-192">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="8ba51-193">İleti biçimlendiriciler eylemleri izin verme türü sonuçları döndürmek için ekler `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="8ba51-193">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="8ba51-194">Web API 2 eylemleri yanıtlar verecek kullanmış olabilirsiniz ek yanıt yöntemleri ekler:</span><span class="sxs-lookup"><span data-stu-id="8ba51-194">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="8ba51-195">`HttpResponseMessage` oluşturucuları:</span><span class="sxs-lookup"><span data-stu-id="8ba51-195">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="8ba51-196">Eylem sonucu yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="8ba51-196">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="8ba51-197">Bir örneğini ekler `IContentNegotiator` uygulamaya bağımlılık ekleme (dı) kapsayıcı kullanıcının ve içerik anlaşması ile ilgili türlerinden kullanılabilmesini [System.NET.http.Formatting](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="8ba51-197">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="8ba51-198">Bu tür örnekleri, `DefaultContentNegotiator` ve `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8ba51-198">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="8ba51-199">Uyumluluk dolgu kullanmak üzere:</span><span class="sxs-lookup"><span data-stu-id="8ba51-199">To use the compatibility shim:</span></span>

1. <span data-ttu-id="8ba51-200">Yükleme [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="8ba51-200">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="8ba51-201">Çağırarak uygulamanın DI kapsayıcı ile uyumluluk dolgu ait Hizmetleri kaydedin `services.AddMvc().AddWebApiConventions()` içinde `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8ba51-201">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="8ba51-202">Web API özel yolları kullanarak tanımlama `MapWebApiRoute` üzerinde `IRouteBuilder` uygulamasının `IApplicationBuilder.UseMvc` çağırın.</span><span class="sxs-lookup"><span data-stu-id="8ba51-202">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ba51-203">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="8ba51-203">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
