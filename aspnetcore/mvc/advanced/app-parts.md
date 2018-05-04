---
title: ASP.NET Core uygulama bölümleri
author: ardalis
description: Bir uygulama soyutlamalar kaynaklardır uygulama bölümleri bulmak veya bir derlemeye ait özelliklerin yüklenmesini önlemek için nasıl kullanılacağını öğrenin.
manager: wpickett
ms.author: riande
ms.date: 01/04/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 8f7aeadc7a1218bf203575add8c82c95faf137b4
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="95682-103">ASP.NET Core uygulama bölümleri</span><span class="sxs-lookup"><span data-stu-id="95682-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="95682-104">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="95682-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="95682-105">Bir *uygulama bölümü* MVC denetleyicileri, görünüm bileşenleri gibi özellikleri, bir uygulama kaynakları üzerinden bir soyutlamadır veya etiket Yardımcıları saptanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="95682-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="95682-106">Bir uygulama bölümü bir derleme başvurusu ve düzenlemenizi sağlayan türleri ve derleme başvurularını yalıtan bir AssemblyPart örnektir.</span><span class="sxs-lookup"><span data-stu-id="95682-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="95682-107">*Özellik sağlayıcıları* ASP.NET Core MVC uygulama özelliklerini doldurmak için uygulama bölümleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="95682-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="95682-108">Ana kullanım örneği uygulama bölümleri için Bul (veya yüklenmesini önlemek için) Uygulamanızı yapılandırmak izin vermektir bütünleştirilmiş MVC özelliklerinden.</span><span class="sxs-lookup"><span data-stu-id="95682-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="95682-109">Uygulama bölümlerini Tanıtımı</span><span class="sxs-lookup"><span data-stu-id="95682-109">Introducing Application Parts</span></span>

<span data-ttu-id="95682-110">MVC uygulamaları kendi özelliklerinden yük [uygulama bölümleri](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="95682-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="95682-111">Özellikle, [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) sınıfı, bir derlemeyi tarafından yedeklenen bir uygulama bölümü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="95682-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="95682-112">Bu sınıfların bulmak ve denetleyicileri, görünümü bileşenler, etiket yardımcıları ve razor derleme kaynakları gibi MVC özellikleri yüklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="95682-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="95682-113">[ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) MVC uygulamasını uygulama bölümleri ve özellik sağlayıcıları kullanılabilir izlemek için sorumludur.</span><span class="sxs-lookup"><span data-stu-id="95682-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="95682-114">Etkileşim kurabildikleri `ApplicationPartManager` içinde `Startup` MVC yapılandırırken:</span><span class="sxs-lookup"><span data-stu-id="95682-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

<span data-ttu-id="95682-115">Varsayılan olarak MVC bağımlılığı ağacı arayın ve denetleyicileri (hatta diğer derlemelerde) bulun.</span><span class="sxs-lookup"><span data-stu-id="95682-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="95682-116">Bir rastgele derlemeden (örneğin, derleme zamanında başvurulan değil bir eklenti) yüklemek için bir uygulama bölümü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="95682-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="95682-117">Uygulama bölümleri için kullanabileceğiniz *kaçının* belirli derleme veya konum denetleyicileri aranıyor.</span><span class="sxs-lookup"><span data-stu-id="95682-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="95682-118">Hangi bölümleri (veya derlemeler) değiştirerek uygulamaya kullanılabilir olacağını kontrol `ApplicationParts` koleksiyonu `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="95682-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="95682-119">Girdileri sırasını `ApplicationParts` koleksiyonu önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="95682-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="95682-120">Tam olarak yapılandırılması önemlidir `ApplicationPartManager` kapsayıcısında Hizmetleri'ni yapılandırmak için kullanmadan önce.</span><span class="sxs-lookup"><span data-stu-id="95682-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="95682-121">Örneğin, tam olarak yapılandırmanız gerekiyor `ApplicationPartManager` çağırmadan önce `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="95682-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="95682-122">Bunu yapmak, başarısız olan anlamına gelir uygulama bölümleri denetleyicileri sonra yöntem çağrısı etkilenmeyecek eklediğiniz (hizmet olarak kayıtlı kalmaz), uygulamanızın yanlış bevavior neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="95682-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="95682-123">Kullanılacak istemediğiniz denetleyicileri içeren bir derleme varsa kaldırmadan `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="95682-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="95682-124">Proje derleme ve bağımlı derlemeleri, ek olarak `ApplicationPartManager` bölümleri için içerecektir `Microsoft.AspNetCore.Mvc.TagHelpers` ve `Microsoft.AspNetCore.Mvc.Razor` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="95682-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="95682-125">Uygulama özellik sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="95682-125">Application Feature Providers</span></span>

<span data-ttu-id="95682-126">Uygulama özellik sağlayıcıları uygulama bölümleri inceleyin ve bu bölümleri için özellikleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="95682-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="95682-127">Aşağıdaki MVC özellikler için yerleşik özellik sağlayıcıları vardır:</span><span class="sxs-lookup"><span data-stu-id="95682-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="95682-128">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="95682-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="95682-129">Meta veri başvurusu</span><span class="sxs-lookup"><span data-stu-id="95682-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="95682-130">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="95682-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="95682-131">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="95682-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="95682-132">Özellik sağlayıcıları devral `IApplicationFeatureProvider<T>`, burada `T` özellik türüdür.</span><span class="sxs-lookup"><span data-stu-id="95682-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="95682-133">MVC'ın özellik türlerinin herhangi biriyle sağlayıcıları yukarıda listelenen kendi özellik uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="95682-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="95682-134">Özellik sağlayıcıların sırası `ApplicationPartManager.FeatureProviders` sonraki sağlayıcıları önceki sağlayıcıları tarafından gerçekleştirilen eylemler için tepki gösterebilmesi beri koleksiyonu önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="95682-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="95682-135">Örnek: Genel denetleyicisi özelliği</span><span class="sxs-lookup"><span data-stu-id="95682-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="95682-136">Varsayılan olarak, ASP.NET Core MVC genel denetleyicileri göz ardı eder (örneğin, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="95682-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="95682-137">Bu örnek sonra varsayılan Sağlayıcısı'nı çalıştıran ve belirtilen liste için genel denetleyici örnekleri türlerinin ekleyen bir denetleyici özellik sağlayıcısı kullanır (tanımlanan `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="95682-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="95682-138">Varlık türleri:</span><span class="sxs-lookup"><span data-stu-id="95682-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="95682-139">Özellik sağlayıcısı eklenir `Startup`:</span><span class="sxs-lookup"><span data-stu-id="95682-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="95682-140">Varsayılan olarak, yönlendirme için kullanılan genel denetleyicisi adları biçiminde olacaktır *GenericController'1 [pencere]* yerine *pencere öğesi*.</span><span class="sxs-lookup"><span data-stu-id="95682-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="95682-141">Aşağıdaki öznitelik adı denetleyici tarafından kullanılan genel tür karşılık gelecek şekilde değiştirmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="95682-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="95682-142">`GenericController` Sınıfı:</span><span class="sxs-lookup"><span data-stu-id="95682-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="95682-143">Eşleşen bir rota istendiğinde sonucu:</span><span class="sxs-lookup"><span data-stu-id="95682-143">The result, when a matching route is requested:</span></span>

![Örnek uygulamadan çıktı örneği okuma ve 'Hello genel Sproket denetleyicisinden.'](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="95682-145">Örnek: Görüntü kullanılabilir özellikler</span><span class="sxs-lookup"><span data-stu-id="95682-145">Sample: Display available features</span></span>

<span data-ttu-id="95682-146">İsteyerek uygulamanıza doldurulan kullanılabilen özellikleri yineleyebilirsiniz bir `ApplicationPartManager` aracılığıyla [bağımlılık ekleme](../../fundamentals/dependency-injection.md) ve uygun özelliklerin örneklerini doldurmak için kullanma:</span><span class="sxs-lookup"><span data-stu-id="95682-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="95682-147">Örnek çıktı:</span><span class="sxs-lookup"><span data-stu-id="95682-147">Example output:</span></span>

![Örnek uygulaması örnek çıkışı](app-parts/_static/available-features.png)
