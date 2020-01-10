---
title: ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümleri, Razor Pages ve daha fazlasını paylaşma
author: rick-anderson
description: ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümü, Razor Pages ve daha fazlasını paylaşma
ms.author: riande
ms.date: 11/11/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: a95c344410db0651b9f8f1c1eb7551029f084c25
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829081"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts"></a><span data-ttu-id="172e0-103">Uygulama parçalarıyla denetleyiciler, görünümler, Razor Pages ve daha fazlasını paylaşma</span><span class="sxs-lookup"><span data-stu-id="172e0-103">Share controllers, views, Razor Pages and more with Application Parts</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="172e0-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="172e0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="172e0-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="172e0-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="172e0-106">*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="172e0-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="172e0-107">Uygulama bölümleri ASP.NET Core denetleyicileri bulmasına, bileşenleri, etiket yardımcılarını, Razor Pages, Razor derleme kaynaklarını ve daha fazlasını bulmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="172e0-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="172e0-108"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> bir uygulama bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="172e0-108"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> is an Application part.</span></span> <span data-ttu-id="172e0-109">`AssemblyPart` bir derleme başvurusunu kapsüller ve türleri ve derleme başvurularını ortaya koyar.</span><span class="sxs-lookup"><span data-stu-id="172e0-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="172e0-110">[Özellik sağlayıcıları](#fp) , uygulama bölümleriyle birlikte çalışarak bir ASP.NET Core uygulamasının özelliklerini doldurur.</span><span class="sxs-lookup"><span data-stu-id="172e0-110">[Feature providers](#fp) work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="172e0-111">Uygulama bölümleri için ana kullanım örneği, bir derlemeyi, bir derlemeden ASP.NET Core özellikleri bulacak (ya da yüklemeden kaçınacak) şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="172e0-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="172e0-112">Örneğin, birden çok uygulama arasında ortak işlevselliği paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="172e0-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="172e0-113">Uygulama parçalarını kullanarak, birden çok uygulamayla denetleyiciler, görünümler, Razor Pages, Razor derleme kaynakları, etiket yardımcıları ve daha fazlasını içeren bir derlemeyi (DLL) paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="172e0-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="172e0-114">Birden çok projedeki kodu çoğaltmak için bir derlemeyi paylaşma tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="172e0-115">ASP.NET Core uygulamalar <xref:System.Web.WebPages.ApplicationPart>özellikleri yükler.</span><span class="sxs-lookup"><span data-stu-id="172e0-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="172e0-116"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> sınıfı, bir derleme tarafından desteklenen bir uygulama bölümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="172e0-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="172e0-117">ASP.NET Core özellikleri yükle</span><span class="sxs-lookup"><span data-stu-id="172e0-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="172e0-118">ASP.NET Core özelliklerini (denetleyiciler, görünüm bileşenleri vb.) bulup yüklemek için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> ve <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> sınıflarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="172e0-118">Use the <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> and <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="172e0-119"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager>, kullanılabilir uygulama parçalarını ve özellik sağlayıcılarını izler.</span><span class="sxs-lookup"><span data-stu-id="172e0-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="172e0-120">`ApplicationPartManager` `Startup.ConfigureServices`yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="172e0-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/3.0sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="172e0-121">Aşağıdaki kod, `AssemblyPart`kullanarak `ApplicationPartManager` yapılandırmaya yönelik alternatif bir yaklaşım sağlar:</span><span class="sxs-lookup"><span data-stu-id="172e0-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/3.0sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="172e0-122">Önceki iki kod örneği bir derlemeden `SharedController` yükler.</span><span class="sxs-lookup"><span data-stu-id="172e0-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="172e0-123">`SharedController`, uygulamanın projesinde değil.</span><span class="sxs-lookup"><span data-stu-id="172e0-123">The `SharedController` is not in the app's project.</span></span> <span data-ttu-id="172e0-124">Bkz. [Webappparts çözüm](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/3.0sample1/WebAppParts) örneği indirmesi.</span><span class="sxs-lookup"><span data-stu-id="172e0-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/3.0sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="172e0-125">Görünümleri dahil et</span><span class="sxs-lookup"><span data-stu-id="172e0-125">Include views</span></span>

<span data-ttu-id="172e0-126">Derlemeye görünümler eklemek için [Razor sınıf kitaplığı](xref:razor-pages/ui-class) kullanın.</span><span class="sxs-lookup"><span data-stu-id="172e0-126">Use a [Razor class library](xref:razor-pages/ui-class) to include views in the assembly.</span></span>

### <a name="prevent-loading-resources"></a><span data-ttu-id="172e0-127">Kaynakları yüklemeyi engelle</span><span class="sxs-lookup"><span data-stu-id="172e0-127">Prevent loading resources</span></span>

<span data-ttu-id="172e0-128">Uygulama bölümleri, belirli bir derleme veya konumdaki kaynakları yüklemeyi *önlemek* için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-128">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="172e0-129">Kullanılabilir kaynakları gizlemek veya mevcut hale getirmek için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> koleksiyonun üyelerini ekleyin veya kaldırın.</span><span class="sxs-lookup"><span data-stu-id="172e0-129">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="172e0-130">`ApplicationParts` koleksiyonundaki girdilerin sırası önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="172e0-130">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="172e0-131">Kapsayıcıdaki hizmetleri yapılandırmak için kullanmadan önce `ApplicationPartManager` yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="172e0-131">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="172e0-132">Örneğin, `AddControllersAsServices`çağırmadan önce `ApplicationPartManager` yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="172e0-132">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="172e0-133">Bir kaynağı kaldırmak için `ApplicationParts` koleksiyonundaki `Remove` çağırın.</span><span class="sxs-lookup"><span data-stu-id="172e0-133">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="172e0-134">`ApplicationPartManager` şunlar için parçalar içerir:</span><span class="sxs-lookup"><span data-stu-id="172e0-134">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="172e0-135">Uygulamanın derlemesi ve bağımlı derlemeleri.</span><span class="sxs-lookup"><span data-stu-id="172e0-135">The app's assembly and dependent assemblies.</span></span>
* `Microsoft.AspNetCore.Mvc.ApplicationParts.CompiledRazorAssemblyPart`
* `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`
* <span data-ttu-id="172e0-136">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="172e0-136">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="172e0-137">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="172e0-137">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

<a name="fp"></a>

## <a name="feature-providers"></a><span data-ttu-id="172e0-138">Özellik sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="172e0-138">Feature providers</span></span>

<span data-ttu-id="172e0-139">Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="172e0-139">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="172e0-140">Aşağıdaki ASP.NET Core özellikleri için yerleşik özellik sağlayıcıları vardır:</span><span class="sxs-lookup"><span data-stu-id="172e0-140">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Controllers.ControllerFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.TagHelpers.TagHelperFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.Compilation.MetadataReferenceFeatureProvider>
* <xref:Microsoft.AspNetCore.Mvc.Razor.Compilation.ViewsFeatureProvider>
* <span data-ttu-id="172e0-141">`internal class` [RazorCompiledItemFeatureProvider](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Razor/src/ApplicationParts/RazorCompiledItemFeatureProvider.cs#L14)</span><span class="sxs-lookup"><span data-stu-id="172e0-141">`internal class` [RazorCompiledItemFeatureProvider](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Razor/src/ApplicationParts/RazorCompiledItemFeatureProvider.cs#L14)</span></span>

<span data-ttu-id="172e0-142">Özellik sağlayıcıları, `T` özelliğin türü olduğu <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>devralınır.</span><span class="sxs-lookup"><span data-stu-id="172e0-142">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="172e0-143">Özellik sağlayıcıları, daha önce listelenen özellik türlerinden herhangi biri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-143">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="172e0-144">`ApplicationPartManager.FeatureProviders` özellik sağlayıcılarının sırası çalışma zamanı davranışını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-144">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="172e0-145">Daha sonra eklenen sağlayıcılar, daha önce eklenen sağlayıcıların yaptığı eylemlere yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-145">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="display-available-features"></a><span data-ttu-id="172e0-146">Kullanılabilir özellikleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="172e0-146">Display available features</span></span>

<span data-ttu-id="172e0-147">Bir uygulama için kullanılabilen özellikler, [bağımlılık ekleme](../../fundamentals/dependency-injection.md)yoluyla bir `ApplicationPartManager` isteyerek numaralandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="172e0-147">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="172e0-148">[Yükleme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) , uygulama özelliklerini göstermek için yukarıdaki kodu kullanır:</span><span class="sxs-lookup"><span data-stu-id="172e0-148">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

## <a name="discovery-in-application-parts"></a><span data-ttu-id="172e0-149">Uygulama bölümlerinde bulma</span><span class="sxs-lookup"><span data-stu-id="172e0-149">Discovery in application parts</span></span>

<span data-ttu-id="172e0-150">HTTP 404 hataları, uygulama parçalarıyla geliştirme sırasında sık karşılaşılan bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="172e0-150">HTTP 404 errors are not uncommon when developing with application parts.</span></span> <span data-ttu-id="172e0-151">Bu hatalar genellikle uygulamaların bölümlerinin nasıl keşfedildiği konusunda önemli bir gereksinimin eksik olmasından kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="172e0-151">These errors are typically caused by missing an essential requirement for how applications parts are discovered.</span></span> <span data-ttu-id="172e0-152">Uygulamanız bir HTTP 404 hatası döndürürse, aşağıdaki gereksinimlerin karşılandığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="172e0-152">If your app returns an HTTP 404 error, verify the following requirements have been met:</span></span>

* <span data-ttu-id="172e0-153">`applicationName` ayarının, bulma için kullanılan kök derlemeye ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="172e0-153">The `applicationName` setting needs to be set to the root assembly used for discovery.</span></span> <span data-ttu-id="172e0-154">Bulma için kullanılan kök derleme normalde giriş noktası derlemesidir.</span><span class="sxs-lookup"><span data-stu-id="172e0-154">The root assembly used for discovery is normally the entry point assembly.</span></span>
* <span data-ttu-id="172e0-155">Kök derlemenin, bulma için kullanılan bölümlere bir başvuruya sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="172e0-155">The root assembly needs to have a reference to the parts used for discovery.</span></span> <span data-ttu-id="172e0-156">Başvuru doğrudan veya geçişli olabilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-156">The reference can be direct or transitive.</span></span>
* <span data-ttu-id="172e0-157">Kök derlemenin Web SDK 'sına başvurması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="172e0-157">The root assembly needs to reference the Web SDK.</span></span> <span data-ttu-id="172e0-158">Çerçevede, öznitelikleri bulma için kullanılan kök derlemeye damgalayan mantık vardır.</span><span class="sxs-lookup"><span data-stu-id="172e0-158">The framework has logic that stamps attributes into the root assembly that are used for discovery.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="172e0-159">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="172e0-159">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="172e0-160">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="172e0-160">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="172e0-161">*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="172e0-161">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="172e0-162">Uygulama bölümleri ASP.NET Core denetleyicileri bulmasına, bileşenleri, etiket yardımcılarını, Razor Pages, Razor derleme kaynaklarını ve daha fazlasını bulmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="172e0-162">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="172e0-163">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) bir uygulama bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="172e0-163">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="172e0-164">`AssemblyPart` bir derleme başvurusunu kapsüller ve türleri ve derleme başvurularını ortaya koyar.</span><span class="sxs-lookup"><span data-stu-id="172e0-164">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="172e0-165">*Özellik sağlayıcıları* , uygulama bölümleriyle birlikte çalışarak bir ASP.NET Core uygulamasının özelliklerini doldurur.</span><span class="sxs-lookup"><span data-stu-id="172e0-165">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="172e0-166">Uygulama bölümleri için ana kullanım örneği, bir derlemeyi, bir derlemeden ASP.NET Core özellikleri bulacak (ya da yüklemeden kaçınacak) şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="172e0-166">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="172e0-167">Örneğin, birden çok uygulama arasında ortak işlevselliği paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="172e0-167">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="172e0-168">Uygulama parçalarını kullanarak, birden çok uygulamayla denetleyiciler, görünümler, Razor Pages, Razor derleme kaynakları, etiket yardımcıları ve daha fazlasını içeren bir derlemeyi (DLL) paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="172e0-168">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="172e0-169">Birden çok projedeki kodu çoğaltmak için bir derlemeyi paylaşma tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-169">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="172e0-170">ASP.NET Core uygulamalar <xref:System.Web.WebPages.ApplicationPart>özellikleri yükler.</span><span class="sxs-lookup"><span data-stu-id="172e0-170">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="172e0-171"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> sınıfı, bir derleme tarafından desteklenen bir uygulama bölümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="172e0-171">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="172e0-172">ASP.NET Core özellikleri yükle</span><span class="sxs-lookup"><span data-stu-id="172e0-172">Load ASP.NET Core features</span></span>

<span data-ttu-id="172e0-173">ASP.NET Core özelliklerini (denetleyiciler, görünüm bileşenleri vb.) bulup yüklemek için `ApplicationPart` ve `AssemblyPart` sınıflarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="172e0-173">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="172e0-174"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager>, kullanılabilir uygulama parçalarını ve özellik sağlayıcılarını izler.</span><span class="sxs-lookup"><span data-stu-id="172e0-174">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="172e0-175">`ApplicationPartManager` `Startup.ConfigureServices`yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="172e0-175">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="172e0-176">Aşağıdaki kod, `AssemblyPart`kullanarak `ApplicationPartManager` yapılandırmaya yönelik alternatif bir yaklaşım sağlar:</span><span class="sxs-lookup"><span data-stu-id="172e0-176">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="172e0-177">Önceki iki kod örneği bir derlemeden `SharedController` yükler.</span><span class="sxs-lookup"><span data-stu-id="172e0-177">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="172e0-178">`SharedController`, uygulamanın projesinde değil.</span><span class="sxs-lookup"><span data-stu-id="172e0-178">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="172e0-179">Bkz. [Webappparts çözüm](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) örneği indirmesi.</span><span class="sxs-lookup"><span data-stu-id="172e0-179">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="172e0-180">Görünümleri dahil et</span><span class="sxs-lookup"><span data-stu-id="172e0-180">Include views</span></span>

<span data-ttu-id="172e0-181">Derlemeye görünümler eklemek için [Razor sınıf kitaplığı](xref:razor-pages/ui-class) kullanın.</span><span class="sxs-lookup"><span data-stu-id="172e0-181">Use a [Razor class library](xref:razor-pages/ui-class) to include views in the assembly.</span></span>

### <a name="prevent-loading-resources"></a><span data-ttu-id="172e0-182">Kaynakları yüklemeyi engelle</span><span class="sxs-lookup"><span data-stu-id="172e0-182">Prevent loading resources</span></span>

<span data-ttu-id="172e0-183">Uygulama bölümleri, belirli bir derleme veya konumdaki kaynakları yüklemeyi *önlemek* için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-183">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="172e0-184">Kullanılabilir kaynakları gizlemek veya mevcut hale getirmek için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> koleksiyonun üyelerini ekleyin veya kaldırın.</span><span class="sxs-lookup"><span data-stu-id="172e0-184">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="172e0-185">`ApplicationParts` koleksiyonundaki girdilerin sırası önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="172e0-185">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="172e0-186">Kapsayıcıdaki hizmetleri yapılandırmak için kullanmadan önce `ApplicationPartManager` yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="172e0-186">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="172e0-187">Örneğin, `AddControllersAsServices`çağırmadan önce `ApplicationPartManager` yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="172e0-187">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="172e0-188">Bir kaynağı kaldırmak için `ApplicationParts` koleksiyonundaki `Remove` çağırın.</span><span class="sxs-lookup"><span data-stu-id="172e0-188">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="172e0-189">Aşağıdaki kod uygulamadan `MyDependentLibrary` kaldırmak için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> kullanır: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="172e0-189">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="172e0-190">`ApplicationPartManager` şunlar için parçalar içerir:</span><span class="sxs-lookup"><span data-stu-id="172e0-190">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="172e0-191">Uygulamanın derlemesi ve bağımlı derlemeleri.</span><span class="sxs-lookup"><span data-stu-id="172e0-191">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="172e0-192">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="172e0-192">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="172e0-193">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="172e0-193">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="172e0-194">Uygulama özelliği sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="172e0-194">Application feature providers</span></span>

<span data-ttu-id="172e0-195">Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="172e0-195">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="172e0-196">Aşağıdaki ASP.NET Core özellikleri için yerleşik özellik sağlayıcıları vardır:</span><span class="sxs-lookup"><span data-stu-id="172e0-196">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="172e0-197">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="172e0-197">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="172e0-198">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="172e0-198">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="172e0-199">Bileşenleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="172e0-199">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="172e0-200">Özellik sağlayıcıları, `T` özelliğin türü olduğu <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>devralınır.</span><span class="sxs-lookup"><span data-stu-id="172e0-200">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="172e0-201">Özellik sağlayıcıları, daha önce listelenen özellik türlerinden herhangi biri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-201">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="172e0-202">`ApplicationPartManager.FeatureProviders` özellik sağlayıcılarının sırası çalışma zamanı davranışını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-202">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="172e0-203">Daha sonra eklenen sağlayıcılar, daha önce eklenen sağlayıcıların yaptığı eylemlere yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-203">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="display-available-features"></a><span data-ttu-id="172e0-204">Kullanılabilir özellikleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="172e0-204">Display available features</span></span>

<span data-ttu-id="172e0-205">Bir uygulama için kullanılabilen özellikler, [bağımlılık ekleme](../../fundamentals/dependency-injection.md)yoluyla bir `ApplicationPartManager` isteyerek numaralandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="172e0-205">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="172e0-206">[Yükleme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) , uygulama özelliklerini göstermek için yukarıdaki kodu kullanır:</span><span class="sxs-lookup"><span data-stu-id="172e0-206">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

## <a name="discovery-in-application-parts"></a><span data-ttu-id="172e0-207">Uygulama bölümlerinde bulma</span><span class="sxs-lookup"><span data-stu-id="172e0-207">Discovery in application parts</span></span>

<span data-ttu-id="172e0-208">HTTP 404 hataları, uygulama parçalarıyla geliştirme sırasında sık karşılaşılan bir durumdur.</span><span class="sxs-lookup"><span data-stu-id="172e0-208">HTTP 404 errors are not uncommon when developing with application parts.</span></span> <span data-ttu-id="172e0-209">Bu hatalar genellikle uygulamaların bölümlerinin nasıl keşfedildiği konusunda önemli bir gereksinimin eksik olmasından kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="172e0-209">These errors are typically caused by missing an essential requirement for how applications parts are discovered.</span></span> <span data-ttu-id="172e0-210">Uygulamanız bir HTTP 404 hatası döndürürse, aşağıdaki gereksinimlerin karşılandığından emin olun:</span><span class="sxs-lookup"><span data-stu-id="172e0-210">If your app returns an HTTP 404 error, verify the following requirements have been met:</span></span>

* <span data-ttu-id="172e0-211">`applicationName` ayarının, bulma için kullanılan kök derlemeye ayarlanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="172e0-211">The `applicationName` setting needs to be set to the root assembly used for discovery.</span></span> <span data-ttu-id="172e0-212">Bulma için kullanılan kök derleme normalde giriş noktası derlemesidir.</span><span class="sxs-lookup"><span data-stu-id="172e0-212">The root assembly used for discovery is normally the entry point assembly.</span></span>
* <span data-ttu-id="172e0-213">Kök derlemenin, bulma için kullanılan bölümlere bir başvuruya sahip olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="172e0-213">The root assembly needs to have a reference to the parts used for discovery.</span></span> <span data-ttu-id="172e0-214">Başvuru doğrudan veya geçişli olabilir.</span><span class="sxs-lookup"><span data-stu-id="172e0-214">The reference can be direct or transitive.</span></span>
* <span data-ttu-id="172e0-215">Kök derlemenin Web SDK 'sına başvurması gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="172e0-215">The root assembly needs to reference the Web SDK.</span></span>
  * <span data-ttu-id="172e0-216">ASP.NET Core Framework 'te, bulma için kullanılan kök derlemeye öznitelikleri damgalayan özel yapı mantığı vardır.</span><span class="sxs-lookup"><span data-stu-id="172e0-216">The ASP.NET Core framework has custom build logic that stamps attributes into the root assembly that are used for discovery.</span></span>

::: moniker-end
