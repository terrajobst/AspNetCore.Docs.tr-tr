---
title: ASP.NET core'da denetleyicilere bağımlılık ekleme
author: ardalis
description: ASP.NET Core MVC denetleyicileri bağımlılıklarını oluşturucuları bağımlılık ekleme ASP.NET Core ile açıkça aracılığıyla nasıl istek keşfedin.
ms.author: riande
ms.date: 02/24/2019
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 6b08c321f4cae1f4efd8ea40300eaf4dfc2f63a1
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903071"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="97ee9-103">ASP.NET core'da denetleyicilere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="97ee9-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="97ee9-104">Tarafından [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Steve Smith](https://github.com/ardalis)</span><span class="sxs-lookup"><span data-stu-id="97ee9-104">By [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://github.com/ardalis)</span></span>

<span data-ttu-id="97ee9-105">ASP.NET Core MVC denetleyicileri oluşturucular açıkça aracılığıyla bağımlılıkları isteyin.</span><span class="sxs-lookup"><span data-stu-id="97ee9-105">ASP.NET Core MVC controllers request dependencies explicitly via constructors.</span></span> <span data-ttu-id="97ee9-106">ASP.NET Core için yerleşik desteği vardır [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="97ee9-106">ASP.NET Core has built-in support for [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="97ee9-107">DI uygulamaları test edin ve bakımını kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="97ee9-107">DI makes apps easier to test and maintain.</span></span>

<span data-ttu-id="97ee9-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="97ee9-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="97ee9-109">Oluşturucu ekleme</span><span class="sxs-lookup"><span data-stu-id="97ee9-109">Constructor Injection</span></span>

<span data-ttu-id="97ee9-110">Hizmet bir oluşturucu parametresi eklenir ve çalışma zamanı hizmet kapsayıcı hizmetinden giderir.</span><span class="sxs-lookup"><span data-stu-id="97ee9-110">Services are added as a constructor parameter, and the runtime resolves the service from the service container.</span></span> <span data-ttu-id="97ee9-111">Hizmetleri, genellikle arabirimleri kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="97ee9-111">Services are typically defined using interfaces.</span></span> <span data-ttu-id="97ee9-112">Örneğin, geçerli zamanı gerektiren bir uygulama düşünün.</span><span class="sxs-lookup"><span data-stu-id="97ee9-112">For example, consider an app that requires the current time.</span></span> <span data-ttu-id="97ee9-113">Aşağıdaki kullanıma sunan arabirim `IDateTime` hizmeti:</span><span class="sxs-lookup"><span data-stu-id="97ee9-113">The following interface exposes the `IDateTime` service:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Interfaces/IDateTime.cs?name=snippet)]

<span data-ttu-id="97ee9-114">Aşağıdaki kod uygulayan `IDateTime` arabirimi:</span><span class="sxs-lookup"><span data-stu-id="97ee9-114">The following code implements the `IDateTime` interface:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Services/SystemDateTime.cs?name=snippet)]

<span data-ttu-id="97ee9-115">Hizmet, hizmet kapsayıcıya ekleyin:</span><span class="sxs-lookup"><span data-stu-id="97ee9-115">Add the service to the service container:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup1.cs?name=snippet&highlight=3)]

<span data-ttu-id="97ee9-116">Daha fazla bilgi için <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, bkz: [DI hizmet yaşam süreleri](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="97ee9-116">For more information on <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, see [DI service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="97ee9-117">Aşağıdaki kod bir karşılama günün saatini temel alan kullanıcı için görüntüler:</span><span class="sxs-lookup"><span data-stu-id="97ee9-117">The following code displays a greeting to the user based on the time of day:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="97ee9-118">Uygulamayı çalıştırın ve saatini temel alan bir ileti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="97ee9-118">Run the app and a message is displayed based on the time.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="97ee9-119">FromServices ile eylemi ekleme</span><span class="sxs-lookup"><span data-stu-id="97ee9-119">Action injection with FromServices</span></span>

<span data-ttu-id="97ee9-120"><xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> Oluşturucu ekleme kullanmadan, doğrudan bir eylem yöntemi bir hizmet ekleme sağlar:</span><span class="sxs-lookup"><span data-stu-id="97ee9-120">The <xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> enables injecting a service directly into an action method without using constructor injection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet2)]

## <a name="access-settings-from-a-controller"></a><span data-ttu-id="97ee9-121">Bir denetleyiciden erişim ayarları</span><span class="sxs-lookup"><span data-stu-id="97ee9-121">Access settings from a controller</span></span>

<span data-ttu-id="97ee9-122">Bir denetleyici içinde gelen uygulama veya yapılandırma ayarlarına erişme ortak bir desendir.</span><span class="sxs-lookup"><span data-stu-id="97ee9-122">Accessing app or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="97ee9-123">*Seçenekleri deseni* açıklanan <xref:fundamentals/configuration/options> ayarlarını yönetmek için tercih edilen yaklaşım.</span><span class="sxs-lookup"><span data-stu-id="97ee9-123">The *options pattern* described in <xref:fundamentals/configuration/options> is the preferred approach to manage settings.</span></span> <span data-ttu-id="97ee9-124">Genellikle doğrudan ekleme yoksa <xref:Microsoft.Extensions.Configuration.IConfiguration> içine bir denetleyici.</span><span class="sxs-lookup"><span data-stu-id="97ee9-124">Generally, don't directly inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into a controller.</span></span>

<span data-ttu-id="97ee9-125">Seçenekleri temsil eden bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="97ee9-125">Create a class that represents the options.</span></span> <span data-ttu-id="97ee9-126">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="97ee9-126">For example:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Models/SampleWebSettings.cs?name=snippet)]

<span data-ttu-id="97ee9-127">Yapılandırma sınıfı Hizmetleri koleksiyona ekleyin:</span><span class="sxs-lookup"><span data-stu-id="97ee9-127">Add the configuration class to the services collection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup.cs?highlight=4&name=snippet1)]

<span data-ttu-id="97ee9-128">Okumasına JSON biçimli bir dosyadan ayarları yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="97ee9-128">Configure the app to read the settings from a JSON-formatted file:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Program.cs?name=snippet&range=10-15)]

<span data-ttu-id="97ee9-129">Aşağıdaki kod istekleri `IOptions<SampleWebSettings>` hizmet kapsayıcı ayarları ve bunları kullanan `Index` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="97ee9-129">The following code requests the `IOptions<SampleWebSettings>` settings from the service container and uses them in the `Index` method:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/SettingsController.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="97ee9-130">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="97ee9-130">Additional resources</span></span>

* <span data-ttu-id="97ee9-131">Bkz: <xref:mvc/controllers/testing> kod test denetleyicileri bağımlılıkları açıkça isteyerek daha kolay hale getirmek öğrenin.</span><span class="sxs-lookup"><span data-stu-id="97ee9-131">See <xref:mvc/controllers/testing> to learn how to make code easier to test by explicitly requesting dependencies in controllers.</span></span>

* <span data-ttu-id="97ee9-132">[Varsayılan bağımlılık ekleme kapsayıcısını üçüncü taraf bir uygulama ile değiştirin](xref:fundamentals/dependency-injection#default-service-container-replacement).</span><span class="sxs-lookup"><span data-stu-id="97ee9-132">[Replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement).</span></span>
