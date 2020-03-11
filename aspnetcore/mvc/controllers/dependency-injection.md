---
title: ASP.NET Core denetleyicilere bağımlılık ekleme
author: ardalis
description: ASP.NET Core MVC denetleyicilerinin bağımlılıklarını açıkça nasıl isteyeceğini, ASP.NET Core bağımlılık ekleme ile oluşturucuları aracılığıyla nasıl isteyeceğini öğrenin.
ms.author: riande
ms.date: 02/24/2019
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 202b62d4b30c5c61c407abdc8509a2a75e181cb6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660085"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="53bcd-103">ASP.NET Core denetleyicilere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="53bcd-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="53bcd-104">By [shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT)ve [Steve Smith](https://github.com/ardalis)</span><span class="sxs-lookup"><span data-stu-id="53bcd-104">By [Shadi Namrouti](https://github.com/shadinamrouti), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://github.com/ardalis)</span></span>

<span data-ttu-id="53bcd-105">ASP.NET Core MVC denetleyicileri, oluşturucular aracılığıyla bağımlılıkları doğrudan ister.</span><span class="sxs-lookup"><span data-stu-id="53bcd-105">ASP.NET Core MVC controllers request dependencies explicitly via constructors.</span></span> <span data-ttu-id="53bcd-106">ASP.NET Core, [bağımlılık ekleme (dı)](xref:fundamentals/dependency-injection)için yerleşik desteğe sahiptir.</span><span class="sxs-lookup"><span data-stu-id="53bcd-106">ASP.NET Core has built-in support for [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="53bcd-107">Dı, uygulamaların test ve bakımını daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="53bcd-107">DI makes apps easier to test and maintain.</span></span>

<span data-ttu-id="53bcd-108">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="53bcd-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="53bcd-109">Oluşturucu Ekleme</span><span class="sxs-lookup"><span data-stu-id="53bcd-109">Constructor Injection</span></span>

<span data-ttu-id="53bcd-110">Hizmetler bir oluşturucu parametresi olarak eklenir ve çalışma zamanı hizmeti hizmet kapsayıcısından çözer.</span><span class="sxs-lookup"><span data-stu-id="53bcd-110">Services are added as a constructor parameter, and the runtime resolves the service from the service container.</span></span> <span data-ttu-id="53bcd-111">Hizmetler genellikle arabirimler kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="53bcd-111">Services are typically defined using interfaces.</span></span> <span data-ttu-id="53bcd-112">Örneğin, geçerli zamanı gerektiren bir uygulamayı düşünün.</span><span class="sxs-lookup"><span data-stu-id="53bcd-112">For example, consider an app that requires the current time.</span></span> <span data-ttu-id="53bcd-113">Aşağıdaki arabirim `IDateTime` hizmetini kullanıma sunar:</span><span class="sxs-lookup"><span data-stu-id="53bcd-113">The following interface exposes the `IDateTime` service:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Interfaces/IDateTime.cs?name=snippet)]

<span data-ttu-id="53bcd-114">Aşağıdaki kod `IDateTime` arabirimini uygular:</span><span class="sxs-lookup"><span data-stu-id="53bcd-114">The following code implements the `IDateTime` interface:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Services/SystemDateTime.cs?name=snippet)]

<span data-ttu-id="53bcd-115">Hizmeti hizmet kapsayıcısına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="53bcd-115">Add the service to the service container:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup1.cs?name=snippet&highlight=3)]

<span data-ttu-id="53bcd-116"><xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>hakkında daha fazla bilgi için bkz. [dı hizmeti yaşam süreleri](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="53bcd-116">For more information on <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton*>, see [DI service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="53bcd-117">Aşağıdaki kod, günün saatine göre kullanıcıya bir karşılama görüntüler:</span><span class="sxs-lookup"><span data-stu-id="53bcd-117">The following code displays a greeting to the user based on the time of day:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="53bcd-118">Uygulamayı çalıştırın ve zamana göre bir ileti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="53bcd-118">Run the app and a message is displayed based on the time.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="53bcd-119">FromServices ile eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="53bcd-119">Action injection with FromServices</span></span>

<span data-ttu-id="53bcd-120"><xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute>, bir hizmetin Oluşturucu Ekleme kullanmadan doğrudan bir eylem yöntemine ekleme izin vermez:</span><span class="sxs-lookup"><span data-stu-id="53bcd-120">The <xref:Microsoft.AspNetCore.Mvc.FromServicesAttribute> enables injecting a service directly into an action method without using constructor injection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/HomeController.cs?name=snippet2)]

## <a name="access-settings-from-a-controller"></a><span data-ttu-id="53bcd-121">Denetleyiciden erişim ayarları</span><span class="sxs-lookup"><span data-stu-id="53bcd-121">Access settings from a controller</span></span>

<span data-ttu-id="53bcd-122">Bir denetleyici içinden uygulama veya yapılandırma ayarlarına erişmek ortak bir modeldir.</span><span class="sxs-lookup"><span data-stu-id="53bcd-122">Accessing app or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="53bcd-123"><xref:fundamentals/configuration/options> açıklanan *Seçenekler deseninin* ayarları yönetmek için tercih edilen yaklaşım vardır.</span><span class="sxs-lookup"><span data-stu-id="53bcd-123">The *options pattern* described in <xref:fundamentals/configuration/options> is the preferred approach to manage settings.</span></span> <span data-ttu-id="53bcd-124">Genellikle, bir denetleyiciye doğrudan <xref:Microsoft.Extensions.Configuration.IConfiguration> eklemeyin.</span><span class="sxs-lookup"><span data-stu-id="53bcd-124">Generally, don't directly inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into a controller.</span></span>

<span data-ttu-id="53bcd-125">Seçenekleri temsil eden bir sınıf oluşturun.</span><span class="sxs-lookup"><span data-stu-id="53bcd-125">Create a class that represents the options.</span></span> <span data-ttu-id="53bcd-126">Örnek:</span><span class="sxs-lookup"><span data-stu-id="53bcd-126">For example:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Models/SampleWebSettings.cs?name=snippet)]

<span data-ttu-id="53bcd-127">Yapılandırma sınıfını hizmetler koleksiyonuna ekleyin:</span><span class="sxs-lookup"><span data-stu-id="53bcd-127">Add the configuration class to the services collection:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Startup.cs?highlight=4&name=snippet1)]

<span data-ttu-id="53bcd-128">Bir JSON biçimli dosyadan ayarları okumak için uygulamayı yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="53bcd-128">Configure the app to read the settings from a JSON-formatted file:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Program.cs?name=snippet&range=10-15)]

<span data-ttu-id="53bcd-129">Aşağıdaki kod, hizmet kapsayıcısından `IOptions<SampleWebSettings>` ayarlarını ister ve `Index` yönteminde kullanır:</span><span class="sxs-lookup"><span data-stu-id="53bcd-129">The following code requests the `IOptions<SampleWebSettings>` settings from the service container and uses them in the `Index` method:</span></span>

[!code-csharp[](dependency-injection/sample/ControllerDI/Controllers/SettingsController.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="53bcd-130">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="53bcd-130">Additional resources</span></span>

* <span data-ttu-id="53bcd-131">Denetleyicilerde açıkça bağımlılıklar isteyerek kodu daha kolay test etme hakkında bilgi edinmek için <xref:mvc/controllers/testing> bakın.</span><span class="sxs-lookup"><span data-stu-id="53bcd-131">See <xref:mvc/controllers/testing> to learn how to make code easier to test by explicitly requesting dependencies in controllers.</span></span>

* <span data-ttu-id="53bcd-132">[Varsayılan bağımlılık ekleme kapsayıcısını üçüncü taraf bir uygulamayla değiştirin](xref:fundamentals/dependency-injection#default-service-container-replacement).</span><span class="sxs-lookup"><span data-stu-id="53bcd-132">[Replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement).</span></span>
