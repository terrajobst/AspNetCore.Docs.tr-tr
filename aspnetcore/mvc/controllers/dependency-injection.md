---
title: "Denetleyicileri içine bağımlılık ekleme"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 118f504311b58258b5a0510477280505135dd2d9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-into-controllers"></a><span data-ttu-id="f4192-102">Denetleyicileri içine bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="f4192-102">Dependency injection into controllers</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="f4192-103">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="f4192-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f4192-104">ASP.NET Core MVC denetleyicileri bağımlılıklarını açıkça kendi oluşturucular aracılığıyla istemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="f4192-104">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="f4192-105">Bazı durumlarda, tek tek denetleyici eylemleri hizmet gerektirebilir ve bu denetleyici düzeyinde istemek için anlamı olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="f4192-105">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="f4192-106">Bu durumda, bir eylem yönteminin bir parametresi olarak bir hizmet eklemesine de seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4192-106">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="f4192-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f4192-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="f4192-108">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="f4192-108">Dependency Injection</span></span>

<span data-ttu-id="f4192-109">Bağımlılık ekleme olduğunu izleyen bir teknik [bağımlılık tersine çevirme ilkesi](http://deviq.com/dependency-inversion-principle/), geniş eşleşmiş modüllerini gibi uygulamalar için izin verme.</span><span class="sxs-lookup"><span data-stu-id="f4192-109">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="f4192-110">ASP.NET Core sahip için yerleşik destek [bağımlılık ekleme](../../fundamentals/dependency-injection.md), hangi yapar uygulamaları sınama ve sürdürmek daha kolay.</span><span class="sxs-lookup"><span data-stu-id="f4192-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="f4192-111">Oluşturucu ekleme</span><span class="sxs-lookup"><span data-stu-id="f4192-111">Constructor Injection</span></span>

<span data-ttu-id="f4192-112">MVC denetleyicileri için ASP.NET Core'nın yerleşik Oluşturucusu tabanlı bağımlılık ekleme desteği genişletir.</span><span class="sxs-lookup"><span data-stu-id="f4192-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="f4192-113">Yalnızca bir hizmet türünün denetleyicinizi Oluşturucusu parametre olarak ekleyerek, ASP.NET Core yerleşik hizmet kapsayıcısında kullanarak bu türde çözümlemeye çalışır.</span><span class="sxs-lookup"><span data-stu-id="f4192-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="f4192-114">Hizmetleri genellikle, ancak her zaman, arabirimleri kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="f4192-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="f4192-115">Örneğin, geçerli zamanı bağımlı bir iş mantığı uygulamanız varsa, testlerinizi bir süre kullanmak uygulamalarında geçmesine izin saati (yerine sabit kodlama), alan bir hizmeti ekleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4192-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="f4192-116">Çalışma zamanında sistem saatini kullanır, böylece arabirimi bunun gibi uygulama kısmı oldukça kolaydır:</span><span class="sxs-lookup"><span data-stu-id="f4192-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="f4192-117">Bu yerinde biz hizmeti bizim denetleyicisi kullanabilir.</span><span class="sxs-lookup"><span data-stu-id="f4192-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="f4192-118">Bu durumda, bazı mantığı ekledik `HomeController` `Index` Tebrik Kartı kullanıcıya görüntülenecek yöntemine temel günün saati.</span><span class="sxs-lookup"><span data-stu-id="f4192-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="f4192-119">Biz uygulamayı şimdi çalıştırırsanız, biz büyük olasılıkla bir hatayla karşılaşırsınız:</span><span class="sxs-lookup"><span data-stu-id="f4192-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="f4192-120">Bir hizmet olarak yapılandırmadıysanız bu hata oluşur `ConfigureServices` yönteminde bizim `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="f4192-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="f4192-121">Yönelik isteklere belirtmek için `IDateTime` bir örneği kullanılarak çözülmelidir `SystemDateTime`, için aşağıda listesinde vurgulanan satırı ekleyin, `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="f4192-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="f4192-122">Bu belirli bir hizmet çeşitli farklı ömrü seçeneklerden biri kullanılarak uygulanan (`Transient`, `Scoped`, veya `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="f4192-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="f4192-123">Bkz: [bağımlılık ekleme](../../fundamentals/dependency-injection.md) bu kapsam seçeneklerin her biri hizmetinizi davranışını nasıl etkileyeceğini anlamak için.</span><span class="sxs-lookup"><span data-stu-id="f4192-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="f4192-124">Hizmet yapılandırıldıktan sonra uygulamayı çalıştıran ve giriş sayfasına giderek zamana dayalı iletiyi beklendiği gibi görüntülenmelidir:</span><span class="sxs-lookup"><span data-stu-id="f4192-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Sunucu selamlama](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="f4192-126">Bkz: [test denetleyicisi mantığı](testing.md) bağımlılıkları istemenin öğrenmek için [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) denetleyicileri kodu test etmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="f4192-126">See [Testing Controller Logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="f4192-127">ASP.NET Core'nın yerleşik bağımlılık ekleme sınıfları Hizmetleri istemek için yalnızca tek bir oluşturucuya sahip destekler.</span><span class="sxs-lookup"><span data-stu-id="f4192-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="f4192-128">Birden fazla Oluşturucusu varsa, belirten bir özel durum alabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="f4192-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="f4192-129">Hata iletisi durumları gibi yalnızca tek bir oluşturucuya sahip bu sorunu çözebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4192-129">As the error message states, you can correct this problem having just a single constructor.</span></span> <span data-ttu-id="f4192-130">Ayrıca [varsayılan bağımlılık ekleme desteği üçüncü taraf bir uygulama ile değiştirmek](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), birden çok oluşturucular destekleyen birçoğu.</span><span class="sxs-lookup"><span data-stu-id="f4192-130">You can also [replace the default dependency injection support with a third party implementation](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="f4192-131">FromServices ile eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="f4192-131">Action Injection with FromServices</span></span>

<span data-ttu-id="f4192-132">Bazen denetleyicinizi içinde bir hizmet için birden fazla eylem gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f4192-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="f4192-133">Bu durumda, bu eylem yönteminin bir parametresi olarak hizmet Ekle mantıklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="f4192-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="f4192-134">Bu öznitelik parametresiyle işaretleyerek yapılır `[FromServices]` aşağıda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="f4192-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="f4192-135">Bir denetleyicisinden ayarlarına erişme</span><span class="sxs-lookup"><span data-stu-id="f4192-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="f4192-136">Uygulama veya yapılandırma içinden ayarlarını bir denetleyici erişim genel bir desen olur.</span><span class="sxs-lookup"><span data-stu-id="f4192-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="f4192-137">Bu erişim açıklanan seçenekler düzeni kullanması gereken [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="f4192-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="f4192-138">Genellikle doğrudan bağımlılık ekleme kullanılarak olan, denetleyici istek ayarları döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="f4192-138">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="f4192-139">Daha iyi bir yaklaşım isteği bir `IOptions<T>` örneği burada `T` ihtiyacınız yapılandırma sınıftır.</span><span class="sxs-lookup"><span data-stu-id="f4192-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="f4192-140">Seçenekleri deseni ile çalışmak için bunun gibi seçenekleri temsil eden bir sınıf oluşturmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="f4192-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="f4192-141">Seçenekleri modeli kullanır ve Hizmetleri koleksiyonunda yapılandırma sınıf eklemek için uygulamayı yapılandırmak gereken sonra `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f4192-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="f4192-142">Yukarıdaki listede biz ayarları JSON biçimli bir dosyadan okunan uygulamaya yapılandırmış olursunuz.</span><span class="sxs-lookup"><span data-stu-id="f4192-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="f4192-143">Yukarıdaki açıklamalı kodda gösterildiği gibi tamamen kodda ayarları da yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f4192-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="f4192-144">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla yapılandırma seçenekleri için.</span><span class="sxs-lookup"><span data-stu-id="f4192-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="f4192-145">Kesin türü belirtilmiş yapılandırma nesnesi belirlediğiniz sonra (Bu durumda, `SampleWebSettings`) ve ekli Hizmetleri koleksiyonuna, onu herhangi denetleyici veya eylem yönteminden bir örneğini isteyerek talep edebilir `IOptions<T>` (Bu durumda, `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="f4192-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="f4192-146">Aşağıdaki kod bir denetleyicisinden ayarları nasıl istemek gösterir:</span><span class="sxs-lookup"><span data-stu-id="f4192-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="f4192-147">Seçenekleri Desen aşağıdaki ayarları ve yapılandırmayı birbirinden ayrılmış sağlar ve denetleyici izlemektir sağlar [sorunları ayrılması](http://deviq.com/separation-of-concerns/), nasıl ve nerede bilmek gerekli olmayan beri ayarları bulunamıyor bilgi.</span><span class="sxs-lookup"><span data-stu-id="f4192-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="f4192-148">Ayrıca denetleyicisi birim testi kolaylaştırır [test denetleyicisi mantığı](testing.md), olduğundan hiçbir [statik cling](http://deviq.com/static-cling/) veya denetleyici sınıfı içinde ayarları sınıfların doğrudan örnek oluşturma.</span><span class="sxs-lookup"><span data-stu-id="f4192-148">It also makes the controller easier to unit test [Testing Controller Logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
