---
title: ASP.NET Core içindeki uygulama bölümleri
author: rick-anderson
description: ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümü, Razor Pages ve daha fazlasını paylaşma
ms.author: riande
ms.date: 05/14/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4b4c8c554a7045a180b56cf9998ab1a8496cde1b
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207354"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="184da-103">ASP.NET Core içindeki uygulama bölümleriyle denetleyicileri, görünümleri, Razor Pages ve daha fazlasını paylaşma</span><span class="sxs-lookup"><span data-stu-id="184da-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="184da-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="184da-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="184da-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="184da-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="184da-106">*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="184da-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="184da-107">Uygulama bölümleri ASP.NET Core denetleyicileri bulmasına, bileşenleri, etiket yardımcılarını, Razor Pages, Razor derleme kaynaklarını ve daha fazlasını bulmasına olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="184da-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="184da-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) bir uygulama bölümüdür.</span><span class="sxs-lookup"><span data-stu-id="184da-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="184da-109">`AssemblyPart`derleme başvurusunu kapsüller ve türleri ve derleme başvurularını ortaya koyar.</span><span class="sxs-lookup"><span data-stu-id="184da-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="184da-110">*Özellik sağlayıcıları* , uygulama bölümleriyle birlikte çalışarak bir ASP.NET Core uygulamasının özelliklerini doldurur.</span><span class="sxs-lookup"><span data-stu-id="184da-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="184da-111">Uygulama bölümleri için ana kullanım örneği, bir derlemeyi, bir derlemeden ASP.NET Core özellikleri bulacak (ya da yüklemeden kaçınacak) şekilde yapılandırmaktır.</span><span class="sxs-lookup"><span data-stu-id="184da-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="184da-112">Örneğin, birden çok uygulama arasında ortak işlevselliği paylaşmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="184da-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="184da-113">Uygulama parçalarını kullanarak, birden çok uygulamayla denetleyiciler, görünümler, Razor Pages, Razor derleme kaynakları, etiket yardımcıları ve daha fazlasını içeren bir derlemeyi (DLL) paylaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="184da-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="184da-114">Birden çok projedeki kodu çoğaltmak için bir derlemeyi paylaşma tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="184da-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="184da-115">ASP.NET Core uygulamalar, içindeki <xref:System.Web.WebPages.ApplicationPart>özellikleri yükler.</span><span class="sxs-lookup"><span data-stu-id="184da-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="184da-116">Sınıfı <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> , bir derleme tarafından desteklenen bir uygulama parçasını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="184da-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="184da-117">ASP.NET Core özellikleri yükle</span><span class="sxs-lookup"><span data-stu-id="184da-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="184da-118">ASP.NET Core özelliklerini ( `AssemblyPart` denetleyiciler, görünüm bileşenleri vs.) bulup yüklemek için vesınıflarınıkullanın.`ApplicationPart`</span><span class="sxs-lookup"><span data-stu-id="184da-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="184da-119">Uygulama parçalarını ve özellik sağlayıcılarını izler.<xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager></span><span class="sxs-lookup"><span data-stu-id="184da-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="184da-120">`ApplicationPartManager`Şu şekilde yapılandırılır `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="184da-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="184da-121">Aşağıdaki kod, kullanarak `ApplicationPartManager` `AssemblyPart`yapılandırma için alternatif bir yaklaşım sağlar:</span><span class="sxs-lookup"><span data-stu-id="184da-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="184da-122">Önceki iki kod örneği bir derlemeden yüklenir `SharedController` .</span><span class="sxs-lookup"><span data-stu-id="184da-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="184da-123">`SharedController` Uygulamanın projesinde değil.</span><span class="sxs-lookup"><span data-stu-id="184da-123">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="184da-124">Bkz. [Webappparts çözüm](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) örneği indirmesi.</span><span class="sxs-lookup"><span data-stu-id="184da-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="184da-125">Görünümleri dahil et</span><span class="sxs-lookup"><span data-stu-id="184da-125">Include views</span></span>

<span data-ttu-id="184da-126">Derleme içindeki görünümleri dahil etmek için:</span><span class="sxs-lookup"><span data-stu-id="184da-126">To include views in the assembly:</span></span>

* <span data-ttu-id="184da-127">Aşağıdaki biçimlendirmeyi paylaşılan proje dosyasına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="184da-127">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="184da-128"><xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> Şunu öğesine ekleyin: <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine></span><span class="sxs-lookup"><span data-stu-id="184da-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="184da-129">Kaynakları yüklemeyi engelle</span><span class="sxs-lookup"><span data-stu-id="184da-129">Prevent loading resources</span></span>

<span data-ttu-id="184da-130">Uygulama bölümleri, belirli bir derleme veya konumdaki kaynakları yüklemeyi *önlemek* için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="184da-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="184da-131">Kullanılabilir kaynakları gizlemek veya saklamak için <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> koleksiyonun üyelerini ekleyin veya kaldırın.</span><span class="sxs-lookup"><span data-stu-id="184da-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="184da-132">`ApplicationParts` Koleksiyondaki girişlerin sırası önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="184da-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="184da-133">Kapsayıcısını, `ApplicationPartManager` kapsayıcıdaki hizmetleri yapılandırmak için kullanmadan önce yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="184da-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="184da-134">Örneğin, öğesini çağırmadan `ApplicationPartManager` `AddControllersAsServices`önce öğesini yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="184da-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="184da-135">Kaynağı kaldırmak `Remove` için koleksiyondaçağrıyapın.`ApplicationParts`</span><span class="sxs-lookup"><span data-stu-id="184da-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="184da-136">Aşağıdaki kod uygulamadan kaldırmak <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> `MyDependentLibrary` için kullanır:[!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="184da-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="184da-137">, `ApplicationPartManager` Şunlar için parçalar içerir:</span><span class="sxs-lookup"><span data-stu-id="184da-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="184da-138">Uygulamanın derlemesi ve bağımlı derlemeleri.</span><span class="sxs-lookup"><span data-stu-id="184da-138">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="184da-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="184da-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="184da-140">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="184da-140">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="184da-141">Uygulama özelliği sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="184da-141">Application feature providers</span></span>

<span data-ttu-id="184da-142">Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="184da-142">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="184da-143">Aşağıdaki ASP.NET Core özellikleri için yerleşik özellik sağlayıcıları vardır:</span><span class="sxs-lookup"><span data-stu-id="184da-143">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="184da-144">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="184da-144">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="184da-145">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="184da-145">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="184da-146">Bileşenleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="184da-146">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="184da-147">Özellik sağlayıcıları öğesinden <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>devralınır, burada `T` özelliğin türüdür.</span><span class="sxs-lookup"><span data-stu-id="184da-147">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="184da-148">Özellik sağlayıcıları, daha önce listelenen özellik türlerinden herhangi biri için uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="184da-148">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="184da-149">İçindeki `ApplicationPartManager.FeatureProviders` özellik sağlayıcılarının sırası çalışma süresi davranışından etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="184da-149">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="184da-150">Daha sonra eklenen sağlayıcılar, daha önce eklenen sağlayıcıların yaptığı eylemlere yanıt verebilir.</span><span class="sxs-lookup"><span data-stu-id="184da-150">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="184da-151">Genel denetleyici özelliği</span><span class="sxs-lookup"><span data-stu-id="184da-151">Generic controller feature</span></span>

<span data-ttu-id="184da-152">ASP.NET Core [genel denetleyicileri](/dotnet/csharp/programming-guide/generics/generic-classes)yoksayar.</span><span class="sxs-lookup"><span data-stu-id="184da-152">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="184da-153">Genel denetleyicinin bir tür parametresi vardır (örneğin, `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="184da-153">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="184da-154">Aşağıdaki örnek, belirli bir tür listesi için genel denetleyici örnekleri ekler:</span><span class="sxs-lookup"><span data-stu-id="184da-154">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="184da-155">Türler içinde `EntityTypes.Types`tanımlanmıştır:</span><span class="sxs-lookup"><span data-stu-id="184da-155">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="184da-156">Özellik sağlayıcısı içine `Startup`eklenir:</span><span class="sxs-lookup"><span data-stu-id="184da-156">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="184da-157">Yönlendirme için kullanılan genel denetleyici adları, *pencere öğesi*yerine *' 1 [pencere öğesi] biçiminde olan genericcontroller* .</span><span class="sxs-lookup"><span data-stu-id="184da-157">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="184da-158">Aşağıdaki öznitelik, denetleyici tarafından kullanılan genel türe karşılık gelen adı değiştirir:</span><span class="sxs-lookup"><span data-stu-id="184da-158">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="184da-159">`GenericController` Sınıf:</span><span class="sxs-lookup"><span data-stu-id="184da-159">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="184da-160">Örneğin, aşağıdaki yanıtta `https://localhost:5001/Sprocket` sonuç URL 'si isteniyor:</span><span class="sxs-lookup"><span data-stu-id="184da-160">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="184da-161">Kullanılabilir özellikleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="184da-161">Display available features</span></span>

<span data-ttu-id="184da-162">Bir uygulama için kullanılabilen özellikler, bir `ApplicationPartManager` [bağımlılık ekleme](../../fundamentals/dependency-injection.md)isteği isteyerek numaralandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="184da-162">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="184da-163">[Yükleme örneği](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) , uygulama özelliklerini göstermek için yukarıdaki kodu kullanır:</span><span class="sxs-lookup"><span data-stu-id="184da-163">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

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
