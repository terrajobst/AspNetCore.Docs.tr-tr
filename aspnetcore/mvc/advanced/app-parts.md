---
title: ASP.NET Core içindeki uygulama bölümleri
author: rick-anderson
description: ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümü, Razor Pages ve daha fazlasını paylaşma
ms.author: riande
ms.date: 11/7/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: ff6afa1852a3ee97fc4dbbae970dd746ec92f74c
ms.sourcegitcommit: 67116718dc33a7a01696d41af38590fdbb58e014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73799461"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="e64cc-103">ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümleri, Razor Pages ve daha fazlasını paylaşma</span><span class="sxs-lookup"><span data-stu-id="e64cc-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e64cc-104">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="e64cc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e64cc-105">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e64cc-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e64cc-106">*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="e64cc-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="e64cc-107">Uygulama bölümleri ASP.NET Core denetleyicileri bulmasına, bileşenleri, etiket yardımcılarını, Razor Pages, Razor derleme kaynaklarını ve daha fazlasını bulmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="e64cc-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="e64cc-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) bir uygulama bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="e64cc-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="e64cc-109">`AssemblyPart` bir derleme başvurusunu kapsüller ve türleri ve derleme başvurularını ortaya koyar.</span><span class="sxs-lookup"><span data-stu-id="e64cc-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="e64cc-110">*Özellik sağlayıcıları* , uygulama bölümleriyle birlikte çalışarak bir ASP.NET Core uygulamasının özelliklerini doldurur.</span><span class="sxs-lookup"><span data-stu-id="e64cc-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="e64cc-111">Uygulama bölümleri için ana kullanım örneği, bir derlemeyi, bir derlemeden ASP.NET Core özellikleri bulacak (ya da yüklemeden kaçınacak) şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="e64cc-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="e64cc-112">Örneğin, birden çok uygulama arasında ortak işlevselliği paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e64cc-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="e64cc-113">Uygulama parçalarını kullanarak, birden çok uygulamayla denetleyiciler, görünümler, Razor Pages, Razor derleme kaynakları, etiket yardımcıları ve daha fazlasını içeren bir derlemeyi (DLL) paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e64cc-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="e64cc-114">Birden çok projedeki kodu çoğaltmak için bir derlemeyi paylaşma tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="e64cc-115">ASP.NET Core uygulamalar <xref:System.Web.WebPages.ApplicationPart>özellikleri yükler.</span><span class="sxs-lookup"><span data-stu-id="e64cc-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="e64cc-116"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> sınıfı, bir derleme tarafından desteklenen bir uygulama bölümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e64cc-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="e64cc-117">ASP.NET Core özellikleri yükle</span><span class="sxs-lookup"><span data-stu-id="e64cc-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="e64cc-118">ASP.NET Core özelliklerini (denetleyiciler, görünüm bileşenleri vb.) bulup yüklemek için `ApplicationPart` ve `AssemblyPart` sınıflarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="e64cc-119"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager>, kullanılabilir uygulama parçalarını ve özellik sağlayıcılarını izler.</span><span class="sxs-lookup"><span data-stu-id="e64cc-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="e64cc-120">`ApplicationPartManager` `Startup.ConfigureServices`yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="e64cc-121">Aşağıdaki kod, `AssemblyPart`kullanarak `ApplicationPartManager` yapılandırmaya yönelik alternatif bir yaklaşım sağlar:</span><span class="sxs-lookup"><span data-stu-id="e64cc-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="e64cc-122">Önceki iki kod örneği bir derlemeden `SharedController` yükler.</span><span class="sxs-lookup"><span data-stu-id="e64cc-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="e64cc-123">`SharedController`, uygulamanın projesinde değil.</span><span class="sxs-lookup"><span data-stu-id="e64cc-123">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="e64cc-124">Bkz. [Webappparts çözüm](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) örneği indirmesi.</span><span class="sxs-lookup"><span data-stu-id="e64cc-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="e64cc-125">Görünümleri dahil et</span><span class="sxs-lookup"><span data-stu-id="e64cc-125">Include views</span></span>

<span data-ttu-id="e64cc-126">Derleme içindeki görünümleri dahil etmek için:</span><span class="sxs-lookup"><span data-stu-id="e64cc-126">To include views in the assembly:</span></span>

* <span data-ttu-id="e64cc-127">Aşağıdaki biçimlendirmeyi paylaşılan proje dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e64cc-127">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="e64cc-128"><xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e64cc-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="e64cc-129">Kaynakları yüklemeyi engelle</span><span class="sxs-lookup"><span data-stu-id="e64cc-129">Prevent loading resources</span></span>

<span data-ttu-id="e64cc-130">Uygulama bölümleri, belirli bir derleme veya konumdaki kaynakları yüklemeyi *önlemek* için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="e64cc-131">Kullanılabilir kaynakları gizlemek veya mevcut hale getirmek için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> koleksiyonun üyelerini ekleyin veya kaldırın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="e64cc-132">`ApplicationParts` koleksiyonundaki girdilerin sırası önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="e64cc-133">Kapsayıcıdaki hizmetleri yapılandırmak için kullanmadan önce `ApplicationPartManager` yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="e64cc-134">Örneğin, `AddControllersAsServices`çağırmadan önce `ApplicationPartManager` yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="e64cc-135">Bir kaynağı kaldırmak için `ApplicationParts` koleksiyonundaki `Remove` çağırın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="e64cc-136">Aşağıdaki kod uygulamadan `MyDependentLibrary` kaldırmak için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> kullanır: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="e64cc-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="e64cc-137">`ApplicationPartManager` şunlar için parçalar içerir:</span><span class="sxs-lookup"><span data-stu-id="e64cc-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="e64cc-138">Uygulamanın derlemesi ve bağımlı derlemeleri.</span><span class="sxs-lookup"><span data-stu-id="e64cc-138">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="e64cc-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="e64cc-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="e64cc-140">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="e64cc-140">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="e64cc-141">Uygulama özelliği sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="e64cc-141">Application feature providers</span></span>

<span data-ttu-id="e64cc-142">Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="e64cc-142">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="e64cc-143">Aşağıdaki ASP.NET Core özellikleri için yerleşik özellik sağlayıcıları vardır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-143">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="e64cc-144">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="e64cc-144">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="e64cc-145">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="e64cc-145">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="e64cc-146">Bileşenleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="e64cc-146">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="e64cc-147">Özellik sağlayıcıları, `T` özelliğin türü olduğu <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>devralınır.</span><span class="sxs-lookup"><span data-stu-id="e64cc-147">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="e64cc-148">Özellik sağlayıcıları, daha önce listelenen özellik türlerinden herhangi biri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-148">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="e64cc-149">`ApplicationPartManager.FeatureProviders` özellik sağlayıcılarının sırası çalışma zamanı davranışını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-149">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="e64cc-150">Daha sonra eklenen sağlayıcılar, daha önce eklenen sağlayıcıların yaptığı eylemlere yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-150">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="e64cc-151">Genel denetleyici özelliği</span><span class="sxs-lookup"><span data-stu-id="e64cc-151">Generic controller feature</span></span>

<span data-ttu-id="e64cc-152">ASP.NET Core [genel denetleyicileri](/dotnet/csharp/programming-guide/generics/generic-classes)yoksayar.</span><span class="sxs-lookup"><span data-stu-id="e64cc-152">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="e64cc-153">Genel denetleyicinin bir tür parametresi vardır (örneğin, `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="e64cc-153">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="e64cc-154">Aşağıdaki örnek, belirli bir tür listesi için genel denetleyici örnekleri ekler:</span><span class="sxs-lookup"><span data-stu-id="e64cc-154">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="e64cc-155">Türler `EntityTypes.Types`tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-155">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="e64cc-156">Özellik sağlayıcısı `Startup`eklendi:</span><span class="sxs-lookup"><span data-stu-id="e64cc-156">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="e64cc-157">Yönlendirme için kullanılan genel denetleyici adları, *pencere öğesi*yerine *' 1 [pencere öğesi] biçiminde olan genericcontroller* .</span><span class="sxs-lookup"><span data-stu-id="e64cc-157">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="e64cc-158">Aşağıdaki öznitelik, denetleyici tarafından kullanılan genel türe karşılık gelen adı değiştirir:</span><span class="sxs-lookup"><span data-stu-id="e64cc-158">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="e64cc-159">`GenericController` Sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e64cc-159">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="e64cc-160">Örneğin, `https://localhost:5001/Sprocket` bir URL 'SI istemek aşağıdaki Yanıt ile sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-160">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="e64cc-161">Kullanılabilir özellikleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="e64cc-161">Display available features</span></span>

<span data-ttu-id="e64cc-162">Bir uygulama için kullanılabilen özellikler, [bağımlılık ekleme](../../fundamentals/dependency-injection.md)yoluyla bir `ApplicationPartManager` isteyerek numaralandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="e64cc-162">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="e64cc-163">[Yükleme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) , uygulama özelliklerini göstermek için yukarıdaki kodu kullanır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-163">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

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

::: moniker-end

::: moniker range="<= aspnetcore-3.0"

<span data-ttu-id="e64cc-164">[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından</span><span class="sxs-lookup"><span data-stu-id="e64cc-164">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e64cc-165">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e64cc-165">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e64cc-166">*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="e64cc-166">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="e64cc-167">Uygulama bölümleri ASP.NET Core denetleyicileri bulmasına, bileşenleri, etiket yardımcılarını, Razor Pages, Razor derleme kaynaklarını ve daha fazlasını bulmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="e64cc-167">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="e64cc-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) bir uygulama bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="e64cc-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="e64cc-169">`AssemblyPart` bir derleme başvurusunu kapsüller ve türleri ve derleme başvurularını ortaya koyar.</span><span class="sxs-lookup"><span data-stu-id="e64cc-169">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="e64cc-170">*Özellik sağlayıcıları* , uygulama bölümleriyle birlikte çalışarak bir ASP.NET Core uygulamasının özelliklerini doldurur.</span><span class="sxs-lookup"><span data-stu-id="e64cc-170">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="e64cc-171">Uygulama bölümleri için ana kullanım örneği, bir derlemeyi, bir derlemeden ASP.NET Core özellikleri bulacak (ya da yüklemeden kaçınacak) şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="e64cc-171">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="e64cc-172">Örneğin, birden çok uygulama arasında ortak işlevselliği paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e64cc-172">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="e64cc-173">Uygulama parçalarını kullanarak, birden çok uygulamayla denetleyiciler, görünümler, Razor Pages, Razor derleme kaynakları, etiket yardımcıları ve daha fazlasını içeren bir derlemeyi (DLL) paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e64cc-173">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="e64cc-174">Birden çok projedeki kodu çoğaltmak için bir derlemeyi paylaşma tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-174">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="e64cc-175">ASP.NET Core uygulamalar <xref:System.Web.WebPages.ApplicationPart>özellikleri yükler.</span><span class="sxs-lookup"><span data-stu-id="e64cc-175">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="e64cc-176"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> sınıfı, bir derleme tarafından desteklenen bir uygulama bölümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="e64cc-176">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="e64cc-177">ASP.NET Core özellikleri yükle</span><span class="sxs-lookup"><span data-stu-id="e64cc-177">Load ASP.NET Core features</span></span>

<span data-ttu-id="e64cc-178">ASP.NET Core özelliklerini (denetleyiciler, görünüm bileşenleri vb.) bulup yüklemek için `ApplicationPart` ve `AssemblyPart` sınıflarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-178">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="e64cc-179"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager>, kullanılabilir uygulama parçalarını ve özellik sağlayıcılarını izler.</span><span class="sxs-lookup"><span data-stu-id="e64cc-179">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="e64cc-180">`ApplicationPartManager` `Startup.ConfigureServices`yapılandırılır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-180">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="e64cc-181">Aşağıdaki kod, `AssemblyPart`kullanarak `ApplicationPartManager` yapılandırmaya yönelik alternatif bir yaklaşım sağlar:</span><span class="sxs-lookup"><span data-stu-id="e64cc-181">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="e64cc-182">Önceki iki kod örneği bir derlemeden `SharedController` yükler.</span><span class="sxs-lookup"><span data-stu-id="e64cc-182">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="e64cc-183">`SharedController`, uygulamanın projesinde değil.</span><span class="sxs-lookup"><span data-stu-id="e64cc-183">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="e64cc-184">Bkz. [Webappparts çözüm](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) örneği indirmesi.</span><span class="sxs-lookup"><span data-stu-id="e64cc-184">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="e64cc-185">Görünümleri dahil et</span><span class="sxs-lookup"><span data-stu-id="e64cc-185">Include views</span></span>

<span data-ttu-id="e64cc-186">Derleme içindeki görünümleri dahil etmek için:</span><span class="sxs-lookup"><span data-stu-id="e64cc-186">To include views in the assembly:</span></span>

* <span data-ttu-id="e64cc-187">Aşağıdaki biçimlendirmeyi paylaşılan proje dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e64cc-187">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="e64cc-188"><xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e64cc-188">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="e64cc-189">Kaynakları yüklemeyi engelle</span><span class="sxs-lookup"><span data-stu-id="e64cc-189">Prevent loading resources</span></span>

<span data-ttu-id="e64cc-190">Uygulama bölümleri, belirli bir derleme veya konumdaki kaynakları yüklemeyi *önlemek* için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-190">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="e64cc-191">Kullanılabilir kaynakları gizlemek veya mevcut hale getirmek için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> koleksiyonun üyelerini ekleyin veya kaldırın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-191">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="e64cc-192">`ApplicationParts` koleksiyonundaki girdilerin sırası önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-192">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="e64cc-193">Kapsayıcıdaki hizmetleri yapılandırmak için kullanmadan önce `ApplicationPartManager` yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-193">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="e64cc-194">Örneğin, `AddControllersAsServices`çağırmadan önce `ApplicationPartManager` yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-194">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="e64cc-195">Bir kaynağı kaldırmak için `ApplicationParts` koleksiyonundaki `Remove` çağırın.</span><span class="sxs-lookup"><span data-stu-id="e64cc-195">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="e64cc-196">Aşağıdaki kod uygulamadan `MyDependentLibrary` kaldırmak için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> kullanır: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="e64cc-196">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="e64cc-197">`ApplicationPartManager` şunlar için parçalar içerir:</span><span class="sxs-lookup"><span data-stu-id="e64cc-197">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="e64cc-198">Uygulamanın derlemesi ve bağımlı derlemeleri.</span><span class="sxs-lookup"><span data-stu-id="e64cc-198">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="e64cc-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="e64cc-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="e64cc-200">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="e64cc-200">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="e64cc-201">Uygulama özelliği sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="e64cc-201">Application feature providers</span></span>

<span data-ttu-id="e64cc-202">Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="e64cc-202">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="e64cc-203">Aşağıdaki ASP.NET Core özellikleri için yerleşik özellik sağlayıcıları vardır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-203">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="e64cc-204">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="e64cc-204">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="e64cc-205">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="e64cc-205">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="e64cc-206">Bileşenleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="e64cc-206">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="e64cc-207">Özellik sağlayıcıları, `T` özelliğin türü olduğu <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>devralınır.</span><span class="sxs-lookup"><span data-stu-id="e64cc-207">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="e64cc-208">Özellik sağlayıcıları, daha önce listelenen özellik türlerinden herhangi biri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-208">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="e64cc-209">`ApplicationPartManager.FeatureProviders` özellik sağlayıcılarının sırası çalışma zamanı davranışını etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-209">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="e64cc-210">Daha sonra eklenen sağlayıcılar, daha önce eklenen sağlayıcıların yaptığı eylemlere yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="e64cc-210">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="e64cc-211">Genel denetleyici özelliği</span><span class="sxs-lookup"><span data-stu-id="e64cc-211">Generic controller feature</span></span>

<span data-ttu-id="e64cc-212">ASP.NET Core [genel denetleyicileri](/dotnet/csharp/programming-guide/generics/generic-classes)yoksayar.</span><span class="sxs-lookup"><span data-stu-id="e64cc-212">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="e64cc-213">Genel denetleyicinin bir tür parametresi vardır (örneğin, `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="e64cc-213">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="e64cc-214">Aşağıdaki örnek, belirli bir tür listesi için genel denetleyici örnekleri ekler:</span><span class="sxs-lookup"><span data-stu-id="e64cc-214">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="e64cc-215">Türler `EntityTypes.Types`tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-215">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="e64cc-216">Özellik sağlayıcısı `Startup`eklendi:</span><span class="sxs-lookup"><span data-stu-id="e64cc-216">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="e64cc-217">Yönlendirme için kullanılan genel denetleyici adları, *pencere öğesi*yerine *' 1 [pencere öğesi] biçiminde olan genericcontroller* .</span><span class="sxs-lookup"><span data-stu-id="e64cc-217">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="e64cc-218">Aşağıdaki öznitelik, denetleyici tarafından kullanılan genel türe karşılık gelen adı değiştirir:</span><span class="sxs-lookup"><span data-stu-id="e64cc-218">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="e64cc-219">`GenericController` Sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e64cc-219">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="e64cc-220">Örneğin, `https://localhost:5001/Sprocket` bir URL 'SI istemek aşağıdaki Yanıt ile sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-220">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="e64cc-221">Kullanılabilir özellikleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="e64cc-221">Display available features</span></span>

<span data-ttu-id="e64cc-222">Bir uygulama için kullanılabilen özellikler, [bağımlılık ekleme](../../fundamentals/dependency-injection.md)yoluyla bir `ApplicationPartManager` isteyerek numaralandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="e64cc-222">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="e64cc-223">[Yükleme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) , uygulama özelliklerini göstermek için yukarıdaki kodu kullanır:</span><span class="sxs-lookup"><span data-stu-id="e64cc-223">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

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

::: moniker-end