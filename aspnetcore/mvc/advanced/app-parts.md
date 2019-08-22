---
title: ASP.NET Core içindeki uygulama bölümleri
author: ardalis
description: Bir derlemeden Özellik yüklemeyi veya bir derlemeden bir derlemeyi önlemek için bir uygulamanın kaynakları üzerinde soyutlamalar olan uygulama parçalarını nasıl kullanacağınızı öğrenin.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 4900ccf5589500db076f8cecd9da198c6a7ceea4
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886459"
---
<!-- DO NOT MAKE CHANGES BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12376 Merges -->

# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="763e0-103">ASP.NET Core içindeki uygulama bölümleri</span><span class="sxs-lookup"><span data-stu-id="763e0-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="763e0-104">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="763e0-104">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="763e0-105">*Uygulama bölümü* , bir uygulamanın kaynakları üzerinde, denetleyiciler, görüntüleme bileşenleri veya etiket YARDıMCıLARı gibi mvc özelliklerinin keşfedildiği bir soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="763e0-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="763e0-106">Bir uygulama parçasının bir örneği, derleme başvurusunu kapsülleyen ve türleri ve derleme başvurularını sunan bir AssemblyPart değildir.</span><span class="sxs-lookup"><span data-stu-id="763e0-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="763e0-107">*Özellik sağlayıcıları* ASP.NET Core MVC uygulamasının özelliklerini doldurmak için uygulama bölümleriyle birlikte çalışır.</span><span class="sxs-lookup"><span data-stu-id="763e0-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="763e0-108">Uygulama bölümleri için ana kullanım örneği, uygulamanızı bir derlemeden MVC özelliklerini bulacak (veya yüklemeden kaçınmak üzere) yapılandırmanıza izin vermaktır.</span><span class="sxs-lookup"><span data-stu-id="763e0-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="763e0-109">Uygulama bölümlerine giriş</span><span class="sxs-lookup"><span data-stu-id="763e0-109">Introducing Application Parts</span></span>

<span data-ttu-id="763e0-110">MVC uygulamaları, [uygulama parçalarından](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart)özelliklerini yükler.</span><span class="sxs-lookup"><span data-stu-id="763e0-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="763e0-111">Özellikle, [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart) sınıfı, bir derleme tarafından desteklenen bir uygulama bölümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="763e0-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="763e0-112">Denetleyiciler, görünüm bileşenleri, etiket yardımcıları ve Razor derleme kaynakları gibi MVC özelliklerini bulup yüklemek için bu sınıfları kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="763e0-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="763e0-113">[Applicationpartmanager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) , MVC uygulamasının kullanabileceği uygulama bölümlerinin ve özellik sağlayıcılarının izlenmesinden sorumludur.</span><span class="sxs-lookup"><span data-stu-id="763e0-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="763e0-114">MVC 'yi yapılandırırken `ApplicationPartManager` ' de `Startup` ile etkileşim kurabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="763e0-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

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

<span data-ttu-id="763e0-115">Varsayılan olarak, MVC bağımlılık ağacını arar ve denetleyicileri bulur (diğer derlemelerde bile).</span><span class="sxs-lookup"><span data-stu-id="763e0-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="763e0-116">Rastgele bir derlemeyi yüklemek için (örneğin, derleme zamanında başvurulmayan bir eklentiden), bir uygulama bölümü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="763e0-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="763e0-117">Belirli bir derlemede veya konumda bulunan denetleyiciler için arama *yapmaktan kaçınmak* için uygulama parçalarını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="763e0-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="763e0-118">Uygulamasının `ApplicationParts` koleksiyonunu`ApplicationPartManager`değiştirerek hangi parçaların (veya derlemelerin) kullanılabilir olduğunu kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="763e0-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="763e0-119">`ApplicationParts` Koleksiyondaki girişlerin sırası önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="763e0-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="763e0-120">Kapsayıcısını, `ApplicationPartManager` kapsayıcıda hizmetleri yapılandırmak için kullanmadan önce tam olarak yapılandırmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="763e0-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="763e0-121">Örneğin, öğesini çağırmadan `ApplicationPartManager` `AddControllersAsServices`önce tam olarak yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="763e0-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="763e0-122">Bunu yapmazsanız, uygulama bölümlerdeki denetleyicilerin bu yöntem çağrısından sonra eklenmeyeceği anlamına gelir (hizmet olarak kaydedilmez) ve bu, uygulamanızın hatalı davranışına neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="763e0-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect behavior of your application.</span></span>

<span data-ttu-id="763e0-123">Kullanılmasını istemediğiniz denetleyicileri içeren bir derlemeniz varsa, ' den `ApplicationPartManager`kaldırın:</span><span class="sxs-lookup"><span data-stu-id="763e0-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           apm.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="763e0-124">Projenizin derleme ve bağımlı derlemelerinin `ApplicationPartManager` yanı sıra, varsayılan olarak `Microsoft.AspNetCore.Mvc.TagHelpers` ve `Microsoft.AspNetCore.Mvc.Razor` parçalarını içerir.</span><span class="sxs-lookup"><span data-stu-id="763e0-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="763e0-125">Uygulama özelliği sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="763e0-125">Application Feature Providers</span></span>

<span data-ttu-id="763e0-126">Uygulama özelliği sağlayıcıları uygulama parçalarını inceler ve bu parçalar için özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="763e0-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="763e0-127">Aşağıdaki MVC özellikleri için yerleşik özellik sağlayıcıları vardır:</span><span class="sxs-lookup"><span data-stu-id="763e0-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="763e0-128">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="763e0-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="763e0-129">Meta veri başvurusu</span><span class="sxs-lookup"><span data-stu-id="763e0-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="763e0-130">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="763e0-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="763e0-131">Bileşenleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="763e0-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="763e0-132">Özellik sağlayıcıları öğesinden `IApplicationFeatureProvider<T>`devralınır, burada `T` özelliğin türüdür.</span><span class="sxs-lookup"><span data-stu-id="763e0-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="763e0-133">Yukarıda listelenen herhangi bir MVC Özellik türü için kendi özellik sağlayıcılarını uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="763e0-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="763e0-134">Daha sonraki sağlayıcılar önceki sağlayıcılar tarafından alınan `ApplicationPartManager.FeatureProviders` eylemlere yanıt verebilmesi için koleksiyondaki özellik sağlayıcılarının sırası önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="763e0-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="763e0-135">Örnekli Genel denetleyici özelliği</span><span class="sxs-lookup"><span data-stu-id="763e0-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="763e0-136">Varsayılan olarak, ASP.NET Core MVC genel denetleyicileri yoksayar (örneğin, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="763e0-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="763e0-137">Bu örnek, varsayılan sağlayıcıdan sonra çalışan bir denetleyici özellik sağlayıcısı kullanır ve belirtilen tür listesi için genel denetleyici örnekleri ekler (içinde `EntityTypes.Types`tanımlanmıştır):</span><span class="sxs-lookup"><span data-stu-id="763e0-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="763e0-138">Varlık türleri:</span><span class="sxs-lookup"><span data-stu-id="763e0-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="763e0-139">Özellik sağlayıcısı içine `Startup`eklenir:</span><span class="sxs-lookup"><span data-stu-id="763e0-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="763e0-140">Varsayılan olarak, yönlendirme için kullanılan genel denetleyici adları, *pencere öğesi*yerine *genericcontroller ' 1 [pencere öğesi]* biçiminde olacaktır.</span><span class="sxs-lookup"><span data-stu-id="763e0-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="763e0-141">Aşağıdaki öznitelik, denetleyici tarafından kullanılan genel türe karşılık gelen adı değiştirmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="763e0-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="763e0-142">`GenericController` Sınıf:</span><span class="sxs-lookup"><span data-stu-id="763e0-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="763e0-143">Sonuç, eşleşen bir yol istendiğinde:</span><span class="sxs-lookup"><span data-stu-id="763e0-143">The result, when a matching route is requested:</span></span>

![Örnek uygulama okumalarından örnek çıktı, ' bir genel Sprocket denetleyicisinden Merhaba. '](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="763e0-145">Örnekli Kullanılabilir özellikleri görüntüle</span><span class="sxs-lookup"><span data-stu-id="763e0-145">Sample: Display available features</span></span>

<span data-ttu-id="763e0-146">`ApplicationPartManager` [Bağımlılık ekleme](../../fundamentals/dependency-injection.md) ve ilgili özelliklerin örneklerini doldurmak için kullanarak uygulamanız için kullanılabilen doldurulmuş özellikler arasında yineleme yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="763e0-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="763e0-147">Örnek çıktı:</span><span class="sxs-lookup"><span data-stu-id="763e0-147">Example output:</span></span>

![Örnek uygulamadaki örnek çıkış](app-parts/_static/available-features.png)
