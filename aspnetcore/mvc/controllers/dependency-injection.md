---
title: ASP.NET core'da denetleyicilere bağımlılık ekleme
author: ardalis
description: ASP.NET Core MVC denetleyicileri bağımlılıklarını oluşturucuları bağımlılık ekleme ASP.NET Core ile açıkça aracılığıyla nasıl istek keşfedin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 9d9d0a68927da62fad8df72c868eaf4b8ada440d
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410277"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a><span data-ttu-id="29606-103">ASP.NET core'da denetleyicilere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="29606-103">Dependency injection into controllers in ASP.NET Core</span></span>

<a name="dependency-injection-controllers"></a>

<span data-ttu-id="29606-104">Tarafından [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="29606-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="29606-105">ASP.NET Core MVC denetleyicileri bağımlılıklarını oluşturucuları aracılığıyla açıkça istemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="29606-105">ASP.NET Core MVC controllers should request their dependencies explicitly via their constructors.</span></span> <span data-ttu-id="29606-106">Bazı durumlarda, bir hizmet ayrı denetleyicisinin Eylemler gerekebilir ve bu denetleyici düzeyinde istemek için anlamlı olmayabilir.</span><span class="sxs-lookup"><span data-stu-id="29606-106">In some instances, individual controller actions may require a service, and it may not make sense to request at the controller level.</span></span> <span data-ttu-id="29606-107">Bu durumda, eylem yöntemine bir parametre olarak bir hizmet ekleme de seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29606-107">In this case, you can also choose to inject a service as a parameter on the action method.</span></span>

<span data-ttu-id="29606-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="29606-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="29606-109">Bağımlılık Ekleme</span><span class="sxs-lookup"><span data-stu-id="29606-109">Dependency Injection</span></span>

<span data-ttu-id="29606-110">ASP.NET Core için yerleşik desteği vardır [bağımlılık ekleme](../../fundamentals/dependency-injection.md), getiren uygulamaları test etmek ve sürdürmek daha kolay.</span><span class="sxs-lookup"><span data-stu-id="29606-110">ASP.NET Core has built-in support for [dependency injection](../../fundamentals/dependency-injection.md), which makes applications easier to test and maintain.</span></span>

## <a name="constructor-injection"></a><span data-ttu-id="29606-111">Oluşturucu ekleme</span><span class="sxs-lookup"><span data-stu-id="29606-111">Constructor Injection</span></span>

<span data-ttu-id="29606-112">Oluşturucu tabanlı bağımlılık ekleme için ASP.NET Core'nın yerleşik destek, MVC denetleyicileri genişletir.</span><span class="sxs-lookup"><span data-stu-id="29606-112">ASP.NET Core's built-in support for constructor-based dependency injection extends to MVC controllers.</span></span> <span data-ttu-id="29606-113">Yalnızca bir hizmet türü bir oluşturucu parametresi olarak denetleyicisine ekleyerek, ASP.NET Core yerleşik hizmet kapsayıcısında kullanarak bu tür çözümlemeyi dener.</span><span class="sxs-lookup"><span data-stu-id="29606-113">By simply adding a service type to your controller as a constructor parameter, ASP.NET Core will attempt to resolve that type using its built in service container.</span></span> <span data-ttu-id="29606-114">Hizmetleri genellikle, ancak her zaman, arabirimler kullanılarak tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="29606-114">Services are typically, but not always, defined using interfaces.</span></span> <span data-ttu-id="29606-115">Örneğin, geçerli zamanı bağlıdır iş mantığı uygulamanız varsa, testlerinizi bir süre kullanan uygulamalar geçmesine izin saati (yerine sabit kodlama), alan bir hizmeti ekleyebilir.</span><span class="sxs-lookup"><span data-stu-id="29606-115">For example, if your application has business logic that depends on the current time, you can inject a service that retrieves the time (rather than hard-coding it), which would allow your tests to pass in implementations that use a set time.</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


<span data-ttu-id="29606-116">Bunun gibi arabirimi çalışma zamanında sistem saatini kullanır, böylece uygulama kısmı oldukça kolaydır:</span><span class="sxs-lookup"><span data-stu-id="29606-116">Implementing an interface like this one so that it uses the system clock at runtime is trivial:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


<span data-ttu-id="29606-117">Bu yerinde hizmet denetleyici kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="29606-117">With this in place, we can use the service in our controller.</span></span> <span data-ttu-id="29606-118">Bu durumda, bazı mantığını ekledik `HomeController` `Index` yöntemi bir karşılama kullanıcıya göstermek için günün saatini temel alan.</span><span class="sxs-lookup"><span data-stu-id="29606-118">In this case, we have added some logic to the `HomeController` `Index` method to display a greeting to the user based on the time of day.</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

<span data-ttu-id="29606-119">Biz uygulamayı şimdi çalıştırırsanız, biz büyük olasılıkla bir hatayla karşılaşırsınız:</span><span class="sxs-lookup"><span data-stu-id="29606-119">If we run the application now, we will most likely encounter an error:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

<span data-ttu-id="29606-120">Bir hizmet olarak yapılandırmadıysanız bu hata oluşur `ConfigureServices` yönteminde bizim `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="29606-120">This error occurs when we have not configured a service in the `ConfigureServices` method in our `Startup` class.</span></span> <span data-ttu-id="29606-121">Yönelik isteklere belirtmek için `IDateTime` örneği kullanılarak çözülmesi gerektiğini `SystemDateTime`, vurgulanan satırın altındaki listeye ekleyin, `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="29606-121">To specify that requests for `IDateTime` should be resolved using an instance of `SystemDateTime`, add the highlighted line in the listing below to your `ConfigureServices` method:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> <span data-ttu-id="29606-122">Bu belirli hizmet birkaç farklı ömrü seçeneklerden herhangi biri kullanılarak uygulanabilir (`Transient`, `Scoped`, veya `Singleton`).</span><span class="sxs-lookup"><span data-stu-id="29606-122">This particular service could be implemented using any of several different lifetime options (`Transient`, `Scoped`, or `Singleton`).</span></span> <span data-ttu-id="29606-123">Bkz: [bağımlılık ekleme](../../fundamentals/dependency-injection.md) bu kapsam seçeneklerin her biri, bir hizmet davranışını nasıl etkileyeceğini anlamak için.</span><span class="sxs-lookup"><span data-stu-id="29606-123">See [Dependency Injection](../../fundamentals/dependency-injection.md) to understand how each of these scope options will affect the behavior of your service.</span></span>

<span data-ttu-id="29606-124">Hizmet yapılandırıldıktan sonra uygulamayı çalıştıran ve giriş sayfasına giderek zamana bağlı ileti beklendiği gibi görüntülenmelidir:</span><span class="sxs-lookup"><span data-stu-id="29606-124">Once the service has been configured, running the application and navigating to the home page should display the time-based message as expected:</span></span>

![Sunucu karşılaması](dependency-injection/_static/server-greeting.png)

>[!TIP]
> <span data-ttu-id="29606-126">Bkz: [Test denetleyicisi mantığı](testing.md) kod test denetleyicileri bağımlılıkları açıkça isteyerek daha kolay hale getirmek hakkında bilgi edinmek için.</span><span class="sxs-lookup"><span data-stu-id="29606-126">See [Test controller logic](testing.md) to learn how to make code easier to test by explicitly requesting dependencies in controllers.</span></span>

<span data-ttu-id="29606-127">ASP.NET Core'nın yerleşik bağımlılık ekleme sınıfları Hizmetleri istemek için yalnızca tek bir oluşturucu sahip destekler.</span><span class="sxs-lookup"><span data-stu-id="29606-127">ASP.NET Core's built-in dependency injection supports having only a single constructor for classes requesting services.</span></span> <span data-ttu-id="29606-128">Birden fazla Oluşturucu varsa belirten bir durum karşılaşabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="29606-128">If you have more than one constructor, you may get an exception stating:</span></span>

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

<span data-ttu-id="29606-129">Hata iletisinde gibi tek bir kurucu kullanarak bu sorunu düzeltebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29606-129">As the error message states, you can correct this problem with the use of a single constructor.</span></span> <span data-ttu-id="29606-130">Ayrıca [varsayılan bağımlılık ekleme kapsayıcısını üçüncü taraf bir uygulama ile değiştirin](xref:fundamentals/dependency-injection#default-service-container-replacement)birden çok Oluşturucu destekleyen çoğu.</span><span class="sxs-lookup"><span data-stu-id="29606-130">You can also [replace the default dependency injection container with a third party implementation](xref:fundamentals/dependency-injection#default-service-container-replacement), many of which support multiple constructors.</span></span>

## <a name="action-injection-with-fromservices"></a><span data-ttu-id="29606-131">FromServices ile eylemi ekleme</span><span class="sxs-lookup"><span data-stu-id="29606-131">Action Injection with FromServices</span></span>

<span data-ttu-id="29606-132">Bazen denetleyicinizin içinde bir hizmet için birden fazla eylem gerekmez.</span><span class="sxs-lookup"><span data-stu-id="29606-132">Sometimes you don't need a service for more than one action within your controller.</span></span> <span data-ttu-id="29606-133">Bu durumda, bu eylem yöntemine bir parametre olarak hizmet ekleme mantıklı olabilir.</span><span class="sxs-lookup"><span data-stu-id="29606-133">In this case, it may make sense to inject the service as a parameter to the action method.</span></span> <span data-ttu-id="29606-134">Bu öznitelik ile parametre olarak işaretleyerek yapılır `[FromServices]` burada gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="29606-134">This is done by marking the parameter with the attribute `[FromServices]` as shown here:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a><span data-ttu-id="29606-135">Bir denetleyiciden ayarlarına erişme</span><span class="sxs-lookup"><span data-stu-id="29606-135">Accessing Settings from a Controller</span></span>

<span data-ttu-id="29606-136">Bir denetleyici içinde gelen uygulama veya yapılandırma ayarlarına erişme ortak bir desendir.</span><span class="sxs-lookup"><span data-stu-id="29606-136">Accessing application or configuration settings from within a controller is a common pattern.</span></span> <span data-ttu-id="29606-137">Bu erişim açıklanan seçenekler deseni kullanması gereken [yapılandırma](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="29606-137">This access should use the Options pattern described in [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="29606-138">Bağımlılık ekleme kullanılarak doğrudan denetleyicinizden ayarları genellikle istek olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="29606-138">You generally shouldn't request settings directly from your controller using dependency injection.</span></span> <span data-ttu-id="29606-139">Daha iyi bir yaklaşım isteği bir `IOptions<T>` örneği burada `T` ihtiyacınız yapılandırma sınıftır.</span><span class="sxs-lookup"><span data-stu-id="29606-139">A better approach is to request an `IOptions<T>` instance, where `T` is the configuration class you need.</span></span>

<span data-ttu-id="29606-140">Seçenekleri Desen ile çalışmak için bu gibi seçenekleri temsil eden bir sınıf oluşturmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="29606-140">To work with the options pattern, you need to create a class that represents the options, such as this one:</span></span>

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

<span data-ttu-id="29606-141">Seçenekleri modeli kullanır ve yapılandırma sınıfınıza Hizmetleri koleksiyona eklemek için uygulamayı yapılandırma ayarını yapmanız gerekir `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="29606-141">Then you need to configure the application to use the options model and add your configuration class to the services collection in `ConfigureServices`:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> <span data-ttu-id="29606-142">Yukarıdaki listenin biz JSON biçimli bir dosyadan ayarları okumak için uygulamayı yapılandırdığınızı varsayalım.</span><span class="sxs-lookup"><span data-stu-id="29606-142">In the above listing, we are configuring the application to read the settings from a JSON-formatted file.</span></span> <span data-ttu-id="29606-143">Tamamen kodunda yukarıdaki açıklamalı kodda gösterildiği gibi ayarları yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29606-143">You can also configure the settings entirely in code, as is shown in the commented code above.</span></span> <span data-ttu-id="29606-144">Bkz: [yapılandırma](xref:fundamentals/configuration/index) daha fazla yapılandırma seçeneği.</span><span class="sxs-lookup"><span data-stu-id="29606-144">See [Configuration](xref:fundamentals/configuration/index) for further configuration options.</span></span>

<span data-ttu-id="29606-145">Bir türü kesin belirlenmiş bir yapılandırma nesnesi, belirttiğiniz sonra (Bu durumda, `SampleWebSettings`) ve eklemiş Hizmetleri koleksiyonuna, onu bir denetleyici veya eylem yönteminden bir örneğini isteyerek talep edebilir `IOptions<T>` (Bu durumda, `IOptions<SampleWebSettings>`) .</span><span class="sxs-lookup"><span data-stu-id="29606-145">Once you've specified a strongly-typed configuration object (in this case, `SampleWebSettings`) and added it to the services collection, you can request it from any Controller or Action method by requesting an instance of `IOptions<T>` (in this case, `IOptions<SampleWebSettings>`).</span></span> <span data-ttu-id="29606-146">Aşağıdaki kod nasıl bir denetleyiciden ayarları istemek gösterir:</span><span class="sxs-lookup"><span data-stu-id="29606-146">The following code shows how one would request the settings from a controller:</span></span>

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

<span data-ttu-id="29606-147">Seçenekleri düzenini izleyerek birbirlerinden ölçeklendirilebilmeleri ayarlarını ve yapılandırmasını sağlar ve denetleyici takip sağlar [görev ayrımı nettir](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns), burada veya nasıl bilmeniz gerekmez bu yana ayarları bulunamıyor bilgiler.</span><span class="sxs-lookup"><span data-stu-id="29606-147">Following the Options pattern allows settings and configuration to be decoupled from one another, and ensures the controller is following [separation of concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns), since it doesn't need to know how or where to find the settings information.</span></span> <span data-ttu-id="29606-148">Ayrıca denetleyici kolaylaştırır [birim testi](testing.md), hiçbir doğrudan ayarları sınıfları için denetleyici sınıfı örneğinin olduğundan.</span><span class="sxs-lookup"><span data-stu-id="29606-148">It also makes the controller easier to [unit test](testing.md), since there's no direct instantiation of settings classes within the controller class.</span></span>
