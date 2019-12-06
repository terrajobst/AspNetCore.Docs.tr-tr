---
title: ASP.NET Web API 'sinden ASP.NET Core 'e geçiş
author: ardalis
description: ASP.NET 4. x Web API 'sinden bir Web API uygulamasını ASP.NET Core MVC 'ye geçirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/webapi
ms.openlocfilehash: c68cf83f427f53b110075168c6d5e4d021808782
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881142"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="823e4-103">ASP.NET Web API 'sinden ASP.NET Core 'e geçiş</span><span class="sxs-lookup"><span data-stu-id="823e4-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="823e4-104">[Scott Ade](https://twitter.com/scott_addie) ve [Steve Smith](https://ardalis.com/) tarafından</span><span class="sxs-lookup"><span data-stu-id="823e4-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="823e4-105">ASP.NET 4. x Web API 'SI, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli istemcilere ulaşan bir HTTP hizmetidir.</span><span class="sxs-lookup"><span data-stu-id="823e4-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="823e4-106">ASP.NET Core, ASP.NET 4. x ' in MVC ve Web API uygulaması modellerini ASP.NET Core MVC olarak bilinen daha basit bir programlama modeline ayırır.</span><span class="sxs-lookup"><span data-stu-id="823e4-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="823e4-107">Bu makalede, ASP.NET 4. x Web API 'sinden ASP.NET Core MVC 'ye geçiş yapmak için gereken adımlar gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="823e4-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="823e4-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="823e4-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="823e4-109">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="823e4-109">Prerequisites</span></span>

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="823e4-110">ASP.NET 4. x Web API projesini inceleyin</span><span class="sxs-lookup"><span data-stu-id="823e4-110">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="823e4-111">Başlangıç noktası olarak bu makale, [ASP.NET Web API 2 Ile çalışmaya](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)başlama bölümünde oluşturulan *productsapp* projesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="823e4-111">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="823e4-112">Bu projede, basit bir ASP.NET 4. x Web API projesi aşağıdaki şekilde yapılandırılır.</span><span class="sxs-lookup"><span data-stu-id="823e4-112">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="823e4-113">*Global.asax.cs*' de `WebApiConfig.Register`bir çağrı yapılır:</span><span class="sxs-lookup"><span data-stu-id="823e4-113">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="823e4-114">`WebApiConfig` sınıfı *App_Start* klasöründe bulunur ve statik bir `Register` yöntemine sahiptir:</span><span class="sxs-lookup"><span data-stu-id="823e4-114">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="823e4-115">Bu sınıf, proje içinde gerçekten kullanılmasa da [öznitelik yönlendirmeyi](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="823e4-115">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="823e4-116">Ayrıca, ASP.NET Web API 'SI tarafından kullanılan yönlendirme tablosunu da yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="823e4-116">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="823e4-117">Bu durumda, ASP.NET 4. x Web API 'SI, `{id}` isteğe bağlı olacak şekilde URL 'Lerin biçim `/api/{controller}/{id}`eşleşmesini bekler.</span><span class="sxs-lookup"><span data-stu-id="823e4-117">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="823e4-118">*Productsapp* projesi bir denetleyici içerir.</span><span class="sxs-lookup"><span data-stu-id="823e4-118">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="823e4-119">Denetleyici `ApiController` devralır ve iki eylem içerir:</span><span class="sxs-lookup"><span data-stu-id="823e4-119">The controller inherits from `ApiController` and contains two actions:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

<span data-ttu-id="823e4-120">`ProductsController` tarafından kullanılan `Product` modeli basit bir sınıftır:</span><span class="sxs-lookup"><span data-stu-id="823e4-120">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="823e4-121">Aşağıdaki bölümlerde, Web API projesinin ASP.NET Core MVC 'ye geçişi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="823e4-121">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="823e4-122">Hedef proje oluştur</span><span class="sxs-lookup"><span data-stu-id="823e4-122">Create destination project</span></span>

<span data-ttu-id="823e4-123">Visual Studio 'da aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="823e4-123">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="823e4-124">**Visual Studio çözümleri** > **diğer proje türlerini** > **Dosya** > **Yeni** > **projesi** ' ne gidin.</span><span class="sxs-lookup"><span data-stu-id="823e4-124">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="823e4-125">**Boş çözüm**' i seçin ve çözümü *WebAPIMigration*olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="823e4-125">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="823e4-126">**Tamam** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="823e4-126">Click the **OK** button.</span></span>
* <span data-ttu-id="823e4-127">Varolan *Productsapp* projesini çözüme ekleyin.</span><span class="sxs-lookup"><span data-stu-id="823e4-127">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="823e4-128">Çözüme yeni bir **ASP.NET Core Web uygulaması** projesi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="823e4-128">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="823e4-129">Açılan listeden **.NET Core** hedef çerçevesini seçin ve **API** proje şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="823e4-129">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="823e4-130">Projeyi *Productscore*olarak adlandırın ve **Tamam** düğmesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="823e4-130">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="823e4-131">Çözüm artık iki proje içerir.</span><span class="sxs-lookup"><span data-stu-id="823e4-131">The solution now contains two projects.</span></span> <span data-ttu-id="823e4-132">Aşağıdaki bölümlerde *Productsapp* projesinin Içeriğinin *productscore* projesine geçirilmesi açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="823e4-132">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="823e4-133">Yapılandırmayı geçir</span><span class="sxs-lookup"><span data-stu-id="823e4-133">Migrate configuration</span></span>

<span data-ttu-id="823e4-134">ASP.NET Core, *App_Start* klasörünü veya *Global. asax* dosyasını kullanmaz ve *Web. config* dosyası yayımlama zamanında eklenir.</span><span class="sxs-lookup"><span data-stu-id="823e4-134">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="823e4-135">*Startup.cs* , *Global. asax* 'in yerini alır ve proje kökünde bulunur.</span><span class="sxs-lookup"><span data-stu-id="823e4-135">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="823e4-136">`Startup` sınıfı tüm uygulama başlangıç görevlerini işler.</span><span class="sxs-lookup"><span data-stu-id="823e4-136">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="823e4-137">Daha fazla bilgi için bkz. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="823e4-137">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="823e4-138">ASP.NET Core MVC 'de, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> `Startup.Configure`çağrıldığında öznitelik yönlendirme varsayılan olarak dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="823e4-138">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="823e4-139">Aşağıdaki `UseMvc` çağrısı *Productsapp* projesinin *App_Start/webapiconfig.cs* dosyasını değiştirir:</span><span class="sxs-lookup"><span data-stu-id="823e4-139">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="823e4-140">Modelleri ve denetleyicileri geçirme</span><span class="sxs-lookup"><span data-stu-id="823e4-140">Migrate models and controllers</span></span>

<span data-ttu-id="823e4-141">*Productapp* projesinin denetleyicisi ve kullandığı model üzerine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="823e4-141">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="823e4-142">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="823e4-142">Follow these steps:</span></span>

1. <span data-ttu-id="823e4-143">*Denetleyiciyi/ProductsController. cs* öğesini özgün projeden yeni bir kopyaya kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="823e4-143">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="823e4-144">Tüm *modeller* klasörünü özgün projeden yeni bir klasöre kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="823e4-144">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="823e4-145">Kopyalanan dosyaların ad alanlarını yeni proje adıyla (*Productscore*) eşleşecek şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="823e4-145">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="823e4-146">*ProductsController.cs* `using ProductsApp.Models;` ifadesini de ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="823e4-146">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="823e4-147">Bu noktada, uygulamanın oluşturulması bir dizi derleme hatası ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="823e4-147">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="823e4-148">Aşağıdaki bileşenler ASP.NET Core mevcut olmadığından hatalar oluşur:</span><span class="sxs-lookup"><span data-stu-id="823e4-148">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="823e4-149">`ApiController` sınıfı</span><span class="sxs-lookup"><span data-stu-id="823e4-149">`ApiController` class</span></span>
* <span data-ttu-id="823e4-150">`System.Web.Http` ad alanı</span><span class="sxs-lookup"><span data-stu-id="823e4-150">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="823e4-151">`IHttpActionResult` arabirimi</span><span class="sxs-lookup"><span data-stu-id="823e4-151">`IHttpActionResult` interface</span></span>

<span data-ttu-id="823e4-152">Hataları aşağıdaki gibi düzeltir:</span><span class="sxs-lookup"><span data-stu-id="823e4-152">Fix the errors as follows:</span></span>

1. <span data-ttu-id="823e4-153">Değişiklik `ApiController` için <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="823e4-153">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="823e4-154">`ControllerBase` başvurusunu çözümlemek için `using Microsoft.AspNetCore.Mvc;` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="823e4-154">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="823e4-155">`using System.Web.Http;` klasörünü silin.</span><span class="sxs-lookup"><span data-stu-id="823e4-155">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="823e4-156">`GetProduct` eyleminin dönüş türünü `IHttpActionResult` `ActionResult<Product>`olarak değiştirin.</span><span class="sxs-lookup"><span data-stu-id="823e4-156">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

<span data-ttu-id="823e4-157">`GetProduct` eyleminin `return` ifadesini aşağıdaki şekilde kolaylaştırın:</span><span class="sxs-lookup"><span data-stu-id="823e4-157">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

```csharp
return product;
```

## <a name="configure-routing"></a><span data-ttu-id="823e4-158">Yönlendirmeyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="823e4-158">Configure routing</span></span>

<span data-ttu-id="823e4-159">Yönlendirmeyi aşağıdaki şekilde yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="823e4-159">Configure routing as follows:</span></span>

1. <span data-ttu-id="823e4-160">`ProductsController` sınıfını aşağıdaki özniteliklerle işaretleyin:</span><span class="sxs-lookup"><span data-stu-id="823e4-160">Mark the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="823e4-161">Yukarıdaki [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) özniteliği denetleyicinin öznitelik yönlendirme modelini yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="823e4-161">The preceding [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="823e4-162">[`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği, bu denetleyicideki tüm eylemler için öznitelik yönlendirmesini zorunlu kılar.</span><span class="sxs-lookup"><span data-stu-id="823e4-162">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="823e4-163">Öznitelik yönlendirme `[controller]` ve `[action]`gibi belirteçleri destekler.</span><span class="sxs-lookup"><span data-stu-id="823e4-163">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="823e4-164">Çalışma zamanında, her belirteç, sırasıyla özniteliğin uygulandığı denetleyicinin veya eylemin adı ile değiştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="823e4-164">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="823e4-165">Belirteçler, projedeki sihirli dize sayısını azaltır.</span><span class="sxs-lookup"><span data-stu-id="823e4-165">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="823e4-166">Belirteçler Ayrıca otomatik yeniden adlandırma yeniden düzenlemeler uygulandığında, yolların karşılık gelen denetleyiciler ve eylemlerle eşitlenmiş durumda kalmasını da güvence altına aldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="823e4-166">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="823e4-167">Projenin uyumluluk modunu ASP.NET Core 2,2 olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="823e4-167">Set the project's compatibility mode to ASP.NET Core 2.2:</span></span>

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    <span data-ttu-id="823e4-168">Önceki değişiklik:</span><span class="sxs-lookup"><span data-stu-id="823e4-168">The preceding change:</span></span>

    * <span data-ttu-id="823e4-169">, `[ApiController]` özniteliğini denetleyici düzeyinde kullanmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="823e4-169">Is required to use the `[ApiController]` attribute at the controller level.</span></span>
    * <span data-ttu-id="823e4-170">ASP.NET Core 2,2 ' de ortaya çıkan davranışlardan oluşan davranışa izin verebilir.</span><span class="sxs-lookup"><span data-stu-id="823e4-170">Opts in to potentially breaking behaviors introduced in ASP.NET Core 2.2.</span></span>
1. <span data-ttu-id="823e4-171">`ProductController` eylemlere HTTP Get isteklerini etkinleştirin:</span><span class="sxs-lookup"><span data-stu-id="823e4-171">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="823e4-172">`GetAllProducts` eylemine [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) özniteliğini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="823e4-172">Apply the [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="823e4-173">`GetProduct` eylemine `[HttpGet("{id}")]` özniteliğini uygulayın.</span><span class="sxs-lookup"><span data-stu-id="823e4-173">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="823e4-174">Önceki değişiklikler ve kullanılmayan `using` deyimlerinin kaldırılması sonrasında *ProductsController.cs* dosyası şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="823e4-174">After the preceding changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="823e4-175">Geçirilen projeyi çalıştırın ve `/api/products`gidin.</span><span class="sxs-lookup"><span data-stu-id="823e4-175">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="823e4-176">Üç ürünün tam bir listesi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="823e4-176">A full list of three products appears.</span></span> <span data-ttu-id="823e4-177">konumuna gözatın `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="823e4-177">Browse to `/api/products/1`.</span></span> <span data-ttu-id="823e4-178">İlk ürün görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="823e4-178">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="823e4-179">Uyumluluk dolgusu</span><span class="sxs-lookup"><span data-stu-id="823e4-179">Compatibility shim</span></span>

<span data-ttu-id="823e4-180">[Microsoft. AspNetCore. Mvc. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) kitaplığı, ASP.NET 4. x Web apı projelerini ASP.NET Core 'ye taşımak için bir uyumluluk dolgusu sağlar.</span><span class="sxs-lookup"><span data-stu-id="823e4-180">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="823e4-181">Uyumluluk dolgusu, ASP.NET 4. x Web API 2 ' den çok sayıda kuralı desteklemek için ASP.NET Core genişletir.</span><span class="sxs-lookup"><span data-stu-id="823e4-181">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="823e4-182">Bu belgede daha önce yer alan örnek, uyumluluk dolgunun gereksiz olduğu kadar temel bir örnektir.</span><span class="sxs-lookup"><span data-stu-id="823e4-182">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="823e4-183">Daha büyük projeler için, uyumluluk dolgunun kullanılması, ASP.NET Core ile ASP.NET 4. x Web API 2 arasındaki API boşluğunu geçici olarak köprülemesi için yararlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="823e4-183">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="823e4-184">Web API 'SI uyumluluk dolgusu, büyük ASP.NET 4. x Web API projelerini ASP.NET Core geçirmeyi desteklemek için geçici bir ölçü olarak kullanılmak üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="823e4-184">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="823e4-185">Zaman içinde, projelerin uyumluluk dolgusunda güvenmek yerine ASP.NET Core desenler kullanması için güncelleştirilmeleri gerekir.</span><span class="sxs-lookup"><span data-stu-id="823e4-185">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="823e4-186">`Microsoft.AspNetCore.Mvc.WebApiCompatShim` eklenen uyumluluk özellikleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="823e4-186">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="823e4-187">Denetleyicilerin temel türlerinin güncelleştirilmesine gerek kalmaması için `ApiController` bir tür ekler.</span><span class="sxs-lookup"><span data-stu-id="823e4-187">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="823e4-188">Web API stili model bağlamayı etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="823e4-188">Enables Web API-style model binding.</span></span> <span data-ttu-id="823e4-189">ASP.NET Core MVC modeli bağlama işlevleri, varsayılan olarak ASP.NET 4. x MVC 5 ' in benzer şekilde.</span><span class="sxs-lookup"><span data-stu-id="823e4-189">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="823e4-190">Uyumluluk dolgusu model bağlamasını ASP.NET 4. x Web API 2 model bağlama kurallarına benzer olacak şekilde değiştirir.</span><span class="sxs-lookup"><span data-stu-id="823e4-190">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="823e4-191">Örneğin, karmaşık türler, istek gövdesinden otomatik olarak bağlanır.</span><span class="sxs-lookup"><span data-stu-id="823e4-191">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="823e4-192">Denetleyici eylemlerinin `HttpRequestMessage`türünde parametreler alması için model bağlamayı genişletir.</span><span class="sxs-lookup"><span data-stu-id="823e4-192">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="823e4-193">Eylemlerin `HttpResponseMessage`sonuçları döndürmesini sağlayan ileti biçimleri ekler.</span><span class="sxs-lookup"><span data-stu-id="823e4-193">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="823e4-194">Web API 2 eylemlerinin yanıtları karşılamak için kullanmış olabileceği ek yanıt yöntemleri ekler:</span><span class="sxs-lookup"><span data-stu-id="823e4-194">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="823e4-195">`HttpResponseMessage` üreteçleri:</span><span class="sxs-lookup"><span data-stu-id="823e4-195">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="823e4-196">Eylem sonucu yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="823e4-196">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="823e4-197">Uygulamanın bağımlılık ekleme (dı) kapsayıcısına bir `IContentNegotiator` örneği ekler ve [Microsoft. Aspnet. WebApi. Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)öğesinden içerik anlaşmasına ilişkin türleri kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="823e4-197">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="823e4-198">Bu tür türlere örnek olarak `DefaultContentNegotiator` ve `MediaTypeFormatter`verilebilir.</span><span class="sxs-lookup"><span data-stu-id="823e4-198">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="823e4-199">Uyumluluk Shim 'yi kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="823e4-199">To use the compatibility shim:</span></span>

1. <span data-ttu-id="823e4-200">[Microsoft. AspNetCore. Mvc. WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="823e4-200">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="823e4-201">`Startup.ConfigureServices``services.AddMvc().AddWebApiConventions()` çağırarak, uyumluluk dolgunun hizmetlerini uygulamanın dı kapsayıcısına kaydedin.</span><span class="sxs-lookup"><span data-stu-id="823e4-201">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="823e4-202">Uygulamanın `IApplicationBuilder.UseMvc` çağrısında `IRouteBuilder` `MapWebApiRoute` kullanarak Web API 'sine özgü yollar tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="823e4-202">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="823e4-203">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="823e4-203">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
