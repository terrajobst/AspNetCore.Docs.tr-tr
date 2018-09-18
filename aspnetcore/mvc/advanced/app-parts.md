---
title: ASP.NET core'da uygulama bölümleri
author: ardalis
description: Uygulama kaynaklarını özetlerdir, uygulama bölümleri bulmak veya bir derlemeye ait özelliklerin yüklenmesini önlemek için kullanmayı öğrenin.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 41ae3fd4059844698ded4551dcedc8933ab8cff6
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011319"
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="215db-103">ASP.NET core'da uygulama bölümleri</span><span class="sxs-lookup"><span data-stu-id="215db-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="215db-104">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="215db-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="215db-105">Bir *uygulama bölümü* etiket Yardımcıları bulunabileceğini veya MVC denetleyicileri, görünüm bileşenleri gibi özellikleri, bir uygulamanın kaynaklar üzerinde bir soyutlamadır.</span><span class="sxs-lookup"><span data-stu-id="215db-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="215db-106">Bir uygulama bölümü bir bütünleştirilmiş kod başvurusu ve düzenlemenizi sağlayan türler ve derleme başvurularını kapsülleyen bir AssemblyPart örneğidir.</span><span class="sxs-lookup"><span data-stu-id="215db-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="215db-107">*Özellik sağlayıcıları* bir ASP.NET Core MVC uygulaması özelliklerini doldurmak için uygulama bölümleri ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="215db-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="215db-108">Bul (veya yüklenmesini önlemek için) yapılandırmanız, izin vermek için uygulama bölümleri için ana kullanım örneği olan bir derlemeden MVC özellikleri.</span><span class="sxs-lookup"><span data-stu-id="215db-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="215db-109">Uygulama Bölümleri ile tanışın</span><span class="sxs-lookup"><span data-stu-id="215db-109">Introducing Application Parts</span></span>

<span data-ttu-id="215db-110">MVC uygulamaları yüklemek, özelliklerinden [uygulama bölümleri](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="215db-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="215db-111">Özellikle, [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) sınıfı, bir derleme tarafından desteklenen bir uygulama bölümü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="215db-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="215db-112">Bu sınıfların bulmak ve MVC denetleyicileri, görünüm bileşenleri, etiket yardımcıları ve razor derleme kaynakları gibi özellikleri yüklemek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="215db-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="215db-113">[ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) uygulama bölümleri ve özellik sağlayıcıları kullanılabilir MVC uygulaması için izleme sorumludur.</span><span class="sxs-lookup"><span data-stu-id="215db-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="215db-114">Etkileşim kurabileceğiniz `ApplicationPartManager` içinde `Startup` MVC yapılandırırken:</span><span class="sxs-lookup"><span data-stu-id="215db-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

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

<span data-ttu-id="215db-115">Varsayılan olarak MVC Bağımlılık ağacı aramak ve denetleyicileri (hatta diğer derlemelerde) bulun.</span><span class="sxs-lookup"><span data-stu-id="215db-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="215db-116">(Örneğin, derleme zamanında başvuru değil bir eklenti) gelen rasgele bir derlemeyi yüklemek için bir uygulama bölümünü kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="215db-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="215db-117">Uygulama bölümleri için kullanabileceğiniz *önlemek* belirli bir derleme veya konum denetleyicileri aranıyor.</span><span class="sxs-lookup"><span data-stu-id="215db-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="215db-118">Hangi bölümlerini (veya derlemeleri) değiştirerek uygulaması tarafından denetleyebilirsiniz `ApplicationParts` koleksiyonunu `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="215db-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="215db-119">Giriş sırası `ApplicationParts` koleksiyon önemli değildir.</span><span class="sxs-lookup"><span data-stu-id="215db-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="215db-120">Tam olarak yapılandırılması önemlidir `ApplicationPartManager` kapsayıcıda Hizmetleri'ni yapılandırmak için kullanmadan önce.</span><span class="sxs-lookup"><span data-stu-id="215db-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="215db-121">Örneğin, tam olarak yapılandırmanız gereken `ApplicationPartManager` çağırmadan önce `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="215db-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="215db-122">Bunu yapmak başarısız olan anlamına gelir uygulama bölümleri denetleyicileri sonra yöntem çağrısının etkilenmez eklediğiniz (hizmet olarak kayıtlı gerekmez), uygulamanızın içinde yanlış davranışlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="215db-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect behavior of your application.</span></span>

<span data-ttu-id="215db-123">Kullanılacak istemediğiniz denetleyicileri içeren bir bütünleştirilmiş kod varsa, oradan kaldırın `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="215db-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

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

<span data-ttu-id="215db-124">Proje derleme ve bunların bağımlı derlemeleri yanı sıra `ApplicationPartManager` bölümlerini içerecek `Microsoft.AspNetCore.Mvc.TagHelpers` ve `Microsoft.AspNetCore.Mvc.Razor` varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="215db-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="215db-125">Uygulama özellik sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="215db-125">Application Feature Providers</span></span>

<span data-ttu-id="215db-126">Uygulama özellik sağlayıcıları, uygulama bölümleri inceleyin ve bu parçaları için özellikler sağlar.</span><span class="sxs-lookup"><span data-stu-id="215db-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="215db-127">Aşağıdaki MVC özellikler için yerleşik özellik sağlayıcıları vardır:</span><span class="sxs-lookup"><span data-stu-id="215db-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="215db-128">Denetleyiciler</span><span class="sxs-lookup"><span data-stu-id="215db-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="215db-129">Meta veri başvurusu</span><span class="sxs-lookup"><span data-stu-id="215db-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="215db-130">Etiket Yardımcıları</span><span class="sxs-lookup"><span data-stu-id="215db-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="215db-131">Görünüm bileşenleri</span><span class="sxs-lookup"><span data-stu-id="215db-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="215db-132">Özellik sağlayıcıları devralmanız `IApplicationFeatureProvider<T>`burada `T` özelliği türüdür.</span><span class="sxs-lookup"><span data-stu-id="215db-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="215db-133">Kendi özellik MVC'nin özellik türlerinden herhangi birini sağlayıcıları, yukarıda listelenen uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="215db-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="215db-134">Özellik sağlayıcıları sırasını `ApplicationPartManager.FeatureProviders` sonraki sağlayıcıları önceki sağlayıcıları tarafından gerçekleştirilen eylemler için tepki verebilir olduğundan koleksiyon önemli olabilir.</span><span class="sxs-lookup"><span data-stu-id="215db-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="215db-135">Örnek: Genel denetleyicisi özelliği</span><span class="sxs-lookup"><span data-stu-id="215db-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="215db-136">Varsayılan olarak, ASP.NET Core MVC denetleyicileri genel göz ardı eder (örneğin, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="215db-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="215db-137">Bu örnek sonra varsayılan Sağlayıcısı'nı çalıştıran ve belirtilen bir liste türleri genel denetleyicisi örnekleri ekleyen bir denetleyici özellik sağlayıcısı kullanıyor (tanımlanan `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="215db-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="215db-138">Varlık türleri:</span><span class="sxs-lookup"><span data-stu-id="215db-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="215db-139">Özellik sağlayıcısı eklenir `Startup`:</span><span class="sxs-lookup"><span data-stu-id="215db-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="215db-140">Varsayılan olarak, yönlendirme için kullanılan genel denetleyicisi adları biçiminde olacaktır *GenericController'1 [pencere öğesi]* yerine *pencere öğesi*.</span><span class="sxs-lookup"><span data-stu-id="215db-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="215db-141">Aşağıdaki öznitelik adı denetleyici tarafından kullanılan genel tür karşılık gelecek şekilde değiştirmek için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="215db-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="215db-142">`GenericController` Sınıfı:</span><span class="sxs-lookup"><span data-stu-id="215db-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="215db-143">Eşleşen bir rota istendiğinde sonucu:</span><span class="sxs-lookup"><span data-stu-id="215db-143">The result, when a matching route is requested:</span></span>

![Örnek uygulamadan çıktı örneği okur, 'Hello genel Sproket denetleyicisinden.'](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="215db-145">Örnek: Görüntü kullanılabilir özellikler</span><span class="sxs-lookup"><span data-stu-id="215db-145">Sample: Display available features</span></span>

<span data-ttu-id="215db-146">İsteyerek doldurulan özellikleri uygulamanıza yineleyebilirsiniz bir `ApplicationPartManager` aracılığıyla [bağımlılık ekleme](../../fundamentals/dependency-injection.md) ve uygun özellikler örneğini doldurmak için kullanarak:</span><span class="sxs-lookup"><span data-stu-id="215db-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="215db-147">Örnek çıktı:</span><span class="sxs-lookup"><span data-stu-id="215db-147">Example output:</span></span>

![Örnek uygulamayı örnek çıktısı](app-parts/_static/available-features.png)
