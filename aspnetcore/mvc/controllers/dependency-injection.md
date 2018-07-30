---
title: ASP.NET core'da denetleyicilere bağımlılık ekleme
author: ardalis
description: ASP.NET Core MVC denetleyicileri bağımlılıklarını oluşturucuları bağımlılık ekleme ASP.NET Core ile açıkça aracılığıyla nasıl istek keşfedin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 9dec9807e8fc2883144b2da518f36a7eb8ddc871
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342139"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="9db1d-103">ASP.NET core'da denetleyicilere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="9db1d-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="9db1d-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9db1d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9db1d-105">ASP.NET Core MVC denetleyicileri bağımlılıklarını oluşturucuları aracılığıyla açıkça istemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9db1d-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="9db1d-106">Bazı durumlarda, bir hizmet ayrı denetleyicisinin Eylemler gerekebilir ve bu denetleyici düzeyinde istemek için anlamlı olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="9db1d-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="9db1d-107">Bu durumda, eylem yöntemine bir parametre olarak bir hizmet ekleme de seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9db1d-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="9db1d-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9db1d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="9db1d-109">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="9db1d-109">Dependency Injection</span></span>

<span data-ttu-id="9db1d-110">Bağımlılık ekleme, izleyen bir teknik [bağımlılık tersine çevirme ilkesine](http://deviq.com/dependency-inversion-principle/), gevşek modüllerinin oluşturulması uygulamalara izin verme.</span><span class="sxs-lookup"><span data-stu-id="9db1d-110">Dependency injection is a technique that follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), allowing for applications to be composed of loosely coupled modules.</span></span> <span data-ttu-id="9db1d-111">ASP.NET Core için yerleşik desteği vardır [bağımlılık ekleme](../../fundamentals/dependency-injection.md), getiren uygulamaları test etmek ve sürdürmek daha kolay.</span><span class="sxs-lookup"><span data-stu-id="9db1d-111">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="9db1d-112">Oluşturucu ekleme</span><span class="sxs-lookup"><span data-stu-id="9db1d-112">Constructor Injection</span></span>

<span data-ttu-id="9db1d-113">Oluşturucu tabanlı bağımlılık ekleme için ASP.NET Core'nın yerleşik destek, MVC denetleyicileri genişletir.</span><span class="sxs-lookup"><span data-stu-id="9db1d-113">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="9db1d-114">Yalnızca bir hizmet türü bir oluşturucu parametresi olarak denetleyicisine ekleyerek, ASP.NET Core yerleşik hizmet kapsayıcısında kullanarak bu tür çözümlemeyi dener.</span><span class="sxs-lookup"><span data-stu-id="9db1d-114">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="9db1d-115">Hizmetleri genellikle, ancak her zaman, arabirimler kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9db1d-115">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="9db1d-116">Örneğin, geçerli zamanı bağlıdır iş mantığı uygulamanız varsa, testlerinizi bir süre kullanan uygulamalar geçmesine izin saati (yerine sabit kodlama), alan bir hizmeti ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="9db1d-116">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="9db1d-117">Bunun gibi arabirimi çalışma zamanında sistem saatini kullanır, böylece uygulama kısmı oldukça kolaydır:</span><span class="sxs-lookup"><span data-stu-id="9db1d-117">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="9db1d-118">Bu yerinde hizmet denetleyici kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="9db1d-118">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="9db1d-119">Bu durumda, bazı mantığını ekledik `HomeController` `Index` yöntemi bir karşılama kullanıcıya göstermek için günün saatini temel alan.</span><span class="sxs-lookup"><span data-stu-id="9db1d-119">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="9db1d-120">Biz uygulamayı şimdi çalıştırırsanız, biz büyük olasılıkla bir hatayla karşılaşırsınız:</span><span class="sxs-lookup"><span data-stu-id="9db1d-120">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="9db1d-121">Bir hizmet olarak yapılandırmadıysanız bu hata oluşur `ConfigureServices` yönteminde bizim `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9db1d-121">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="9db1d-122">Yönelik isteklere belirtmek için `IDateTime` örneği kullanılarak çözülmesi gerektiğini `SystemDateTime`, vurgulanan satırın altındaki listeye ekleyin, `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9db1d-122">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="9db1d-123">Bu belirli hizmet birkaç farklı ömrü seçeneklerden herhangi biri kullanılarak uygulanabilir (`Transient`, `Scoped`, veya `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="9db1d-123">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="9db1d-124">Bkz: [bağımlılık ekleme](../../fundamentals/dependency-injection.md) bu kapsam seçeneklerin her biri, bir hizmet davranışını nasıl etkileyeceğini anlamak için.</span><span class="sxs-lookup"><span data-stu-id="9db1d-124">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="9db1d-125">Hizmet yapılandırıldıktan sonra uygulamayı çalıştıran ve giriş sayfasına giderek zamana bağlı ileti beklendiği gibi görüntülenmelidir:</span><span class="sxs-lookup"><span data-stu-id="9db1d-125">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Sunucu karşılaması](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="9db1d-127">Bkz: [Test denetleyicisi mantığı](testing.md) açık şekilde istek bağımlılıkları öğrenmek için [ http://deviq.com/explicit-dependencies-principle/ ](http://deviq.com/explicit-dependencies-principle/) denetleyicileri kodu test etmek daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="9db1d-127">See [Test controller logic](testing.md) to learn how to explicitly request dependencies [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) in controllers makes code easier to test.</span></span>

<span data-ttu-id="9db1d-128">ASP.NET Core'nın yerleşik bağımlılık ekleme sınıfları Hizmetleri istemek için yalnızca tek bir oluşturucu sahip destekler.</span><span class="sxs-lookup"><span data-stu-id="9db1d-128">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="9db1d-129">Birden fazla Oluşturucu varsa belirten bir durum karşılaşabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9db1d-129">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="9db1d-130">Hata iletisinde gibi tek bir kurucu kullanarak bu sorunu düzeltebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9db1d-130">As the error message states, you can correct this problem with the use of a single constructor.</span></span> <span data-ttu-id="9db1d-131">Ayrıca [varsayılan bağımlılık ekleme kapsayıcısını üçüncü taraf bir uygulama ile değiştirin](xref:fundamentals/dependency-injection#default-service-container-replacement)birden çok Oluşturucu destekleyen çoğu.</span><span class="sxs-lookup"><span data-stu-id="9db1d-131">You can also [replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="9db1d-132">FromServices ile eylemi ekleme</span><span class="sxs-lookup"><span data-stu-id="9db1d-132">Action Injection with FromServices</span></span>

<span data-ttu-id="9db1d-133">Bazen denetleyicinizin içinde bir hizmet için birden fazla eylem gerekmez.</span><span class="sxs-lookup"><span data-stu-id="9db1d-133">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="9db1d-134">Bu durumda, bu eylem yöntemine bir parametre olarak hizmet ekleme mantıklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="9db1d-134">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="9db1d-135">Bu öznitelik ile parametre olarak işaretleyerek yapılır `[FromServices]` burada gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="9db1d-135">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="9db1d-136">Bir denetleyiciden ayarlarına erişme</span><span class="sxs-lookup"><span data-stu-id="9db1d-136">Accessing Settings from a Controller</span></span>

<span data-ttu-id="9db1d-137">Bir denetleyici içinde gelen uygulama veya yapılandırma ayarlarına erişme ortak bir desendir.</span><span class="sxs-lookup"><span data-stu-id="9db1d-137">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="9db1d-138">Bu erişim açıklanan seçenekler deseni kullanması gereken [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9db1d-138">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="9db1d-139">Bağımlılık ekleme kullanılarak doğrudan denetleyicinizden ayarları genellikle istek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="9db1d-139">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="9db1d-140">Daha iyi bir yaklaşım isteği bir `IOptions<T>` örneği burada `T` ihtiyacınız yapılandırma sınıftır.</span><span class="sxs-lookup"><span data-stu-id="9db1d-140">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="9db1d-141">Seçenekleri Desen ile çalışmak için bu gibi seçenekleri temsil eden bir sınıf oluşturmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="9db1d-141">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="9db1d-142">Seçenekleri modeli kullanır ve yapılandırma sınıfınıza Hizmetleri koleksiyona eklemek için uygulamayı yapılandırma ayarını yapmanız gerekir `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="9db1d-142">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="9db1d-143">Yukarıdaki listenin biz JSON biçimli bir dosyadan ayarları okumak için uygulamayı yapılandırdığınızı varsayalım.</span><span class="sxs-lookup"><span data-stu-id="9db1d-143">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="9db1d-144">Tamamen kodunda yukarıdaki açıklamalı kodda gösterildiği gibi ayarları yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9db1d-144">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="9db1d-145">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla yapılandırma seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9db1d-145">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="9db1d-146">Bir türü kesin belirlenmiş bir yapılandırma nesnesi, belirttiğiniz sonra (Bu durumda, `SampleWebSettings`) ve eklemiş Hizmetleri koleksiyonuna, onu bir denetleyici veya eylem yönteminden bir örneğini isteyerek talep edebilir `IOptions<T>` (Bu durumda, `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="9db1d-146">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="9db1d-147">Aşağıdaki kod nasıl bir denetleyiciden ayarları istemek gösterir:</span><span class="sxs-lookup"><span data-stu-id="9db1d-147">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="9db1d-148">Seçenekleri düzenini izleyerek birbirlerinden ölçeklendirilebilmeleri ayarlarını ve yapılandırmasını sağlar ve denetleyici takip sağlar [görev ayrımı nettir](http://deviq.com/separation-of-concerns/), burada veya nasıl bilmeniz gerekmez bu yana ayarları bulunamıyor bilgiler.</span><span class="sxs-lookup"><span data-stu-id="9db1d-148">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](http://deviq.com/separation-of-concerns/), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="9db1d-149">Ayrıca denetleyicisi birim testi kolaylaştırır [Test denetleyicisi mantığı](testing.md), olduğundan hiçbir [statik cling](http://deviq.com/static-cling/) veya doğrudan örneğinin ayarları sınıfları için denetleyici sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9db1d-149">It also makes the controller easier to unit test [Test controller logic](testing.md), since there's no [static cling](http://deviq.com/static-cling/) or direct instantiation of settings classes within the controller class.</span></span>
