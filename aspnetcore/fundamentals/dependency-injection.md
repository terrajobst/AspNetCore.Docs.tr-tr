---
title: ASP.NET Core bağımlılık ekleme
author: ardalis
description: ASP.NET Core bağımlılık ekleme nasıl uyguladığını ve nasıl kullanılacağını öğrenin.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 8a105f835dddfcd0e9f32059e644f60dc1fdbbe1
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="dependency-injection-in-aspnet-core"></a><span data-ttu-id="2094f-103">ASP.NET Core bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="2094f-103">Dependency injection in ASP.NET Core</span></span>

<a name="fundamentals-dependency-injection"></a>

<span data-ttu-id="2094f-104">Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="2094f-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="2094f-105">ASP.NET Core sıfırdan yukarı desteklemek ve bağımlılık ekleme yararlanmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2094f-105">ASP.NET Core is designed from the ground up to support and leverage dependency injection.</span></span> <span data-ttu-id="2094f-106">ASP.NET Core uygulamaları başlangıç sınıfı yöntemleri içine eklenen sağlayarak yerleşik bir çerçeve Hizmetleri yararlanabilir ve uygulama hizmetleri de ekleme işlemi için yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="2094f-106">ASP.NET Core applications can leverage built-in framework services by having them injected into methods in the Startup class, and application services can be configured for injection as well.</span></span> <span data-ttu-id="2094f-107">En az bir özellik ayarlama ve diğer kapsayıcılar değiştirmeye yönelik değil ASP.NET Core tarafından sağlanan varsayılan Hizmetleri kapsayıcı sağlar.</span><span class="sxs-lookup"><span data-stu-id="2094f-107">The default services container provided by ASP.NET Core provides a minimal feature set and isn't intended to replace other containers.</span></span>

<span data-ttu-id="2094f-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2094f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="2094f-109">Bağımlılık ekleme nedir?</span><span class="sxs-lookup"><span data-stu-id="2094f-109">What is dependency injection?</span></span>

<span data-ttu-id="2094f-110">Bağımlılık ekleme (dı), nesneleri ve ortak çalışanlar veya bağımlılıkları arasındaki bu sıkı bağ elde etmek için kullanılan bir tekniktir.</span><span class="sxs-lookup"><span data-stu-id="2094f-110">Dependency injection (DI) is a technique for achieving loose coupling between objects and their collaborators, or dependencies.</span></span> <span data-ttu-id="2094f-111">Ortak Çalışanlar doğrudan başlatmasını veya statik başvuruları kullanma yerine eylemlerini gerçekleştirmek için bir sınıf gereken nesneleri şekilde sınıfında sağlanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="2094f-111">Rather than directly instantiating collaborators, or using static references, the objects a class needs in order to perform its actions are provided to the class in some fashion.</span></span> <span data-ttu-id="2094f-112">Sınıfları bağımlılıklarını izlemek vermeden kendi Oluşturucusu aracılığıyla çoğunlukla tanımlayacağınız [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="2094f-112">Most often, classes will declare their dependencies via their constructor, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="2094f-113">Bu yaklaşım "Oluşturucu ekleme" bilinir.</span><span class="sxs-lookup"><span data-stu-id="2094f-113">This approach is known as "constructor injection".</span></span>

<span data-ttu-id="2094f-114">Sınıflar ile DI göz önünde tasarlanmıştır, kendi ortak çalışanlar üzerinde doğrudan, sabit kodlu bağımlılıklar olmadığından bunlar daha gevşek.</span><span class="sxs-lookup"><span data-stu-id="2094f-114">When classes are designed with DI in mind, they're more loosely coupled because they don't have direct, hard-coded dependencies on their collaborators.</span></span> <span data-ttu-id="2094f-115">Bunu izleyen [bağımlılık ters çevirmeyi ilkesine](http://deviq.com/dependency-inversion-principle/), hangi durumları *"yüksek düzey modüller düşük düzey modülleri bağımlı döndürmemelidir; her ikisi de soyutlamalar üzerinde bağımlı olmalıdır."*</span><span class="sxs-lookup"><span data-stu-id="2094f-115">This follows the [Dependency Inversion Principle](http://deviq.com/dependency-inversion-principle/), which states that *"high level modules shouldn't depend on low level modules; both should depend on abstractions."*</span></span> <span data-ttu-id="2094f-116">Belirli uygulamaları başvuran yerine sınıfları isteği soyutlamalar (genellikle `interfaces`) sağlanan onlara sınıfı oluşturulduğunda.</span><span class="sxs-lookup"><span data-stu-id="2094f-116">Instead of referencing specific implementations, classes request abstractions (typically `interfaces`) which are provided to them when the class is constructed.</span></span> <span data-ttu-id="2094f-117">Bağımlılıklar arabirimleri içine ayıklanması ve parametre olarak bu arabirimleri uygulamalarını sağlayarak örneğidir de [stratejisi tasarım deseni](http://deviq.com/strategy-design-pattern/).</span><span class="sxs-lookup"><span data-stu-id="2094f-117">Extracting dependencies into interfaces and providing implementations of these interfaces as parameters is also an example of the [Strategy design pattern](http://deviq.com/strategy-design-pattern/).</span></span>

<span data-ttu-id="2094f-118">Bir sistem dı kullanmak üzere tasarlanmış, bağımlılıklarını kendi Oluşturucusu (veya Özellikler) aracılığıyla isteyen çok sayıda sınıflarla, bunların ilişkili bağımlılıkları olan bu sınıfları oluşturmak için adanmış bir sınıf yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="2094f-118">When a system is designed to use DI, with many classes requesting their dependencies via their constructor (or properties), it's helpful to have a class dedicated to creating these classes with their associated dependencies.</span></span> <span data-ttu-id="2094f-119">Bu sınıfların denir *kapsayıcıları*, ya da daha açık belirtmek gerekirse [denetimi tersine çevirme (IOC)](http://deviq.com/inversion-of-control/) kapsayıcıları veya bağımlılık ekleme (dı) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="2094f-119">These classes are referred to as *containers*, or more specifically, [Inversion of Control (IoC)](http://deviq.com/inversion-of-control/) containers or Dependency Injection (DI) containers.</span></span> <span data-ttu-id="2094f-120">Buradan istenen türlerin örnekleri sağlamaktan sorumludur Fabrika aslında bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="2094f-120">A container is essentially a factory that's responsible for providing instances of types that are requested from it.</span></span> <span data-ttu-id="2094f-121">Belirli bir türde bağımlılıklara sahiptir ve kapsayıcı bağımlılık türleri sağlayacak şekilde yapılandırılmış belirtmiş, bağımlılıklar istenen örneği oluşturma bir parçası olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2094f-121">If a given type has declared that it has dependencies, and the container has been configured to provide the dependency types, it will create the dependencies as part of creating the requested instance.</span></span> <span data-ttu-id="2094f-122">Bu şekilde, tüm sabit kodlanmış nesne oluşturması gerek kalmadan sınıflarına karmaşık bağımlılık grafikleri sağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="2094f-122">In this way, complex dependency graphs can be provided to classes without the need for any hard-coded object construction.</span></span> <span data-ttu-id="2094f-123">Kendi bağımlılıkları olan nesneler oluşturmaya ek olarak, kapsayıcı nesne yaşam süresi uygulama içinde genellikle yönetin.</span><span class="sxs-lookup"><span data-stu-id="2094f-123">In addition to creating objects with their dependencies, containers typically manage object lifetimes within the application.</span></span>

<span data-ttu-id="2094f-124">ASP.NET Core içeren basit bir yerleşik kapsayıcı (tarafından temsil edilen `IServiceProvider` arabirimi) varsayılan oluşturucu ekleme destekleyen ve ASP.NET belirli hizmetleri dı aracılığıyla kullanılabilir yapar.</span><span class="sxs-lookup"><span data-stu-id="2094f-124">ASP.NET Core includes a simple built-in container (represented by the `IServiceProvider` interface) that supports constructor injection by default, and ASP.NET makes certain services available through DI.</span></span> <span data-ttu-id="2094f-125">ASP. NET'in kapsayıcı yönetir olarak türleri başvurduğu *Hizmetleri*.</span><span class="sxs-lookup"><span data-stu-id="2094f-125">ASP.NET's container refers to the types it manages as *services*.</span></span> <span data-ttu-id="2094f-126">Bu makalenin kalanında *Hizmetleri* ASP.NET Core'nın IOC kapsayıcı tarafından yönetilen türlerine başvurur.</span><span class="sxs-lookup"><span data-stu-id="2094f-126">Throughout the rest of this article, *services* will refer to types that are managed by ASP.NET Core's IoC container.</span></span> <span data-ttu-id="2094f-127">Yerleşik kapsayıcının Hizmetleri'nde yapılandırma `ConfigureServices` uygulamanızın yönteminde `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2094f-127">You configure the built-in container's services in the `ConfigureServices` method in your application's `Startup` class.</span></span>

> [!NOTE]
> <span data-ttu-id="2094f-128">Martin Fowler yazma kapsamlı bir makale üzerinde [denetimi tersine çevirme kapsayıcıları ve bağımlılık ekleme düzeni](https://www.martinfowler.com/articles/injection.html).</span><span class="sxs-lookup"><span data-stu-id="2094f-128">Martin Fowler has written an extensive article on [Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html).</span></span> <span data-ttu-id="2094f-129">Microsoft Patterns and Practices Ayrıca, harika bir açıklamaya sahip [bağımlılık ekleme](https://msdn.microsoft.com/library/hh323705.aspx).</span><span class="sxs-lookup"><span data-stu-id="2094f-129">Microsoft Patterns and Practices also has a great description of [Dependency Injection](https://msdn.microsoft.com/library/hh323705.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="2094f-130">Bu makalede, tüm ASP.NET uygulamaları için geçerli olduğundan bağımlılık ekleme yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="2094f-130">This article covers Dependency Injection as it applies to all ASP.NET applications.</span></span> <span data-ttu-id="2094f-131">Bağımlılık ekleme MVC denetleyicileri içinde kapsanan [bağımlılık ekleme ve denetleyiciler](../mvc/controllers/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="2094f-131">Dependency Injection within MVC controllers is covered in [Dependency Injection and Controllers](../mvc/controllers/dependency-injection.md).</span></span>

### <a name="constructor-injection-behavior"></a><span data-ttu-id="2094f-132">Oluşturucusu ekleme işlemi davranışı</span><span class="sxs-lookup"><span data-stu-id="2094f-132">Constructor injection behavior</span></span>

<span data-ttu-id="2094f-133">Oluşturucu ekleme, söz konusu Oluşturucusu olmasını gerektirir *ortak*.</span><span class="sxs-lookup"><span data-stu-id="2094f-133">Constructor injection requires that the constructor in question be *public*.</span></span> <span data-ttu-id="2094f-134">Aksi takdirde, uygulamanızı özel durum oluşturacak bir `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="2094f-134">Otherwise, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="2094f-135">'YourType' türü için uygun bir oluşturucu bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="2094f-135">A suitable constructor for type 'YourType' couldn't be located.</span></span> <span data-ttu-id="2094f-136">Türü somut ve Hizmetleri bir public oluşturucuya tüm parametreler için kayıtlı emin olun.</span><span class="sxs-lookup"><span data-stu-id="2094f-136">Ensure the type is concrete and services are registered for all parameters of a public constructor.</span></span>

<span data-ttu-id="2094f-137">Bu yalnızca bir geçerli Oluşturucusu mevcut Oluşturucu ekleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2094f-137">Constructor injection requires that only one applicable constructor exist.</span></span> <span data-ttu-id="2094f-138">Oluşturucu aşırı desteklenir, ancak yalnızca bir aşırı yüklemesi olan bağımsız değişkenler tüm tarafından bağımlılık ekleme yerine getirmenin bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="2094f-138">Constructor overloads are supported, but only one overload can exist whose arguments can all be fulfilled by dependency injection.</span></span> <span data-ttu-id="2094f-139">Birden fazla varsa, uygulamanızı özel durum oluşturacak bir `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="2094f-139">If more than one exists, your app will throw an `InvalidOperationException`:</span></span>

> <span data-ttu-id="2094f-140">Tüm belirtilen bağımsız değişken türleri kabul birden çok oluşturucular türü 'YourType' bulundu.</span><span class="sxs-lookup"><span data-stu-id="2094f-140">Multiple constructors accepting all given argument types have been found in type 'YourType'.</span></span> <span data-ttu-id="2094f-141">Yalnızca geçerli bir oluşturucusu olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="2094f-141">There should only be one applicable constructor.</span></span>

<span data-ttu-id="2094f-142">Oluşturucular bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bunlar varsayılan değerlerini desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2094f-142">Constructors can accept arguments that are not provided by dependency injection, but these must support default values.</span></span> <span data-ttu-id="2094f-143">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2094f-143">For example:</span></span>

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a><span data-ttu-id="2094f-144">Framework tarafından sağlanan hizmetleri kullanma</span><span class="sxs-lookup"><span data-stu-id="2094f-144">Using framework-provided services</span></span>

<span data-ttu-id="2094f-145">`ConfigureServices` Yönteminde `Startup` sınıftır uygulama kullanır, Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri dahil olmak üzere hizmetleri tanımlamak için sorumlu.</span><span class="sxs-lookup"><span data-stu-id="2094f-145">The `ConfigureServices` method in the `Startup` class is responsible for defining the services the application will use, including platform features like Entity Framework Core and ASP.NET Core MVC.</span></span> <span data-ttu-id="2094f-146">Başlangıçta, `IServiceCollection` için sağlanan `ConfigureServices` tanımlı aşağıdaki hizmetleri sahiptir (bağlı olarak [konak nasıl yapılandırılan](xref:fundamentals/hosting)):</span><span class="sxs-lookup"><span data-stu-id="2094f-146">Initially, the `IServiceCollection` provided to `ConfigureServices` has the following services defined (depending on [how the host was configured](xref:fundamentals/hosting)):</span></span>

| <span data-ttu-id="2094f-147">Hizmet Türü</span><span class="sxs-lookup"><span data-stu-id="2094f-147">Service Type</span></span> | <span data-ttu-id="2094f-148">Ömür</span><span class="sxs-lookup"><span data-stu-id="2094f-148">Lifetime</span></span> |
| ----- | ------- |
| [<span data-ttu-id="2094f-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="2094f-149">Microsoft.AspNetCore.Hosting.IHostingEnvironment</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | <span data-ttu-id="2094f-150">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-150">Singleton</span></span> |
| [<span data-ttu-id="2094f-151">Microsoft.Extensions.Logging.ILoggerFactory</span><span class="sxs-lookup"><span data-stu-id="2094f-151">Microsoft.Extensions.Logging.ILoggerFactory</span></span>](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | <span data-ttu-id="2094f-152">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-152">Singleton</span></span> |
| [<span data-ttu-id="2094f-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="2094f-153">Microsoft.Extensions.Logging.ILogger&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.logging.ilogger) | <span data-ttu-id="2094f-154">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-154">Singleton</span></span> |
| [<span data-ttu-id="2094f-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span><span class="sxs-lookup"><span data-stu-id="2094f-155">Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | <span data-ttu-id="2094f-156">Geçici</span><span class="sxs-lookup"><span data-stu-id="2094f-156">Transient</span></span> |
| [<span data-ttu-id="2094f-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span><span class="sxs-lookup"><span data-stu-id="2094f-157">Microsoft.AspNetCore.Http.IHttpContextFactory</span></span>](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | <span data-ttu-id="2094f-158">Geçici</span><span class="sxs-lookup"><span data-stu-id="2094f-158">Transient</span></span> |
| [<span data-ttu-id="2094f-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="2094f-159">Microsoft.Extensions.Options.IOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.ioptions-1) | <span data-ttu-id="2094f-160">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-160">Singleton</span></span> |
| [<span data-ttu-id="2094f-161">System.Diagnostics.DiagnosticSource</span><span class="sxs-lookup"><span data-stu-id="2094f-161">System.Diagnostics.DiagnosticSource</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | <span data-ttu-id="2094f-162">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-162">Singleton</span></span> |
| [<span data-ttu-id="2094f-163">System.Diagnostics.DiagnosticListener</span><span class="sxs-lookup"><span data-stu-id="2094f-163">System.Diagnostics.DiagnosticListener</span></span>](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | <span data-ttu-id="2094f-164">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-164">Singleton</span></span> |
| [<span data-ttu-id="2094f-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span><span class="sxs-lookup"><span data-stu-id="2094f-165">Microsoft.AspNetCore.Hosting.IStartupFilter</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | <span data-ttu-id="2094f-166">Geçici</span><span class="sxs-lookup"><span data-stu-id="2094f-166">Transient</span></span> |
| [<span data-ttu-id="2094f-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span><span class="sxs-lookup"><span data-stu-id="2094f-167">Microsoft.Extensions.ObjectPool.ObjectPoolProvider</span></span>](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | <span data-ttu-id="2094f-168">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-168">Singleton</span></span> |
| [<span data-ttu-id="2094f-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span><span class="sxs-lookup"><span data-stu-id="2094f-169">Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;</span></span>](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | <span data-ttu-id="2094f-170">Geçici</span><span class="sxs-lookup"><span data-stu-id="2094f-170">Transient</span></span> |
| [<span data-ttu-id="2094f-171">Microsoft.AspNetCore.Hosting.Server.IServer</span><span class="sxs-lookup"><span data-stu-id="2094f-171">Microsoft.AspNetCore.Hosting.Server.IServer</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | <span data-ttu-id="2094f-172">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-172">Singleton</span></span> |
| [<span data-ttu-id="2094f-173">Microsoft.AspNetCore.Hosting.IStartup</span><span class="sxs-lookup"><span data-stu-id="2094f-173">Microsoft.AspNetCore.Hosting.IStartup</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | <span data-ttu-id="2094f-174">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-174">Singleton</span></span> |
| [<span data-ttu-id="2094f-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="2094f-175">Microsoft.AspNetCore.Hosting.IApplicationLifetime</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | <span data-ttu-id="2094f-176">singleton</span><span class="sxs-lookup"><span data-stu-id="2094f-176">Singleton</span></span> |

<span data-ttu-id="2094f-177">Genişletme yöntemleri gibi çok sayıda kullanarak kapsayıcı ek hizmetler eklemek nasıl bir örneği aşağıdadır `AddDbContext`, `AddIdentity`, ve `AddMvc`.</span><span class="sxs-lookup"><span data-stu-id="2094f-177">Below is an example of how to add additional services to the container using a number of extension methods like `AddDbContext`, `AddIdentity`, and `AddMvc`.</span></span>

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

<span data-ttu-id="2094f-178">ASP.NET MVC gibi tarafından sağlanan ara yazılımı ve özellikler tek Ekle kullanmanın bir kuralı izleyin*ServiceName* tüm bu özellik tarafından gerekli hizmetleri kaydetmek için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2094f-178">The features and middleware provided by ASP.NET, such as MVC, follow a convention of using a single Add*ServiceName* extension method to register all of the services required by that feature.</span></span>

> [!TIP]
> <span data-ttu-id="2094f-179">Belirli framework tarafından sağlanan hizmetlerin içindeki istek `Startup` parametresi listelerine - yöntemlerle bkz [uygulama başlangıç](startup.md) daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="2094f-179">You can request certain framework-provided services within `Startup` methods through their parameter lists - see [Application Startup](startup.md) for more details.</span></span>

## <a name="registering-services"></a><span data-ttu-id="2094f-180">Hizmetleri kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="2094f-180">Registering services</span></span>

<span data-ttu-id="2094f-181">Kendi uygulama hizmetleri şu şekilde kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2094f-181">You can register your own application services as follows.</span></span> <span data-ttu-id="2094f-182">İlk genel tür kapsayıcıdan istenen türü (genellikle bir arabirim) temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2094f-182">The first generic type represents the type (typically an interface) that will be requested from the container.</span></span> <span data-ttu-id="2094f-183">İkinci genel türü tarafından kapsayıcı örneği ve bu tür isteklerini yerine getirmek için kullanılan somut türünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2094f-183">The second generic type represents the concrete type that will be instantiated by the container and used to fulfill such requests.</span></span>

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> <span data-ttu-id="2094f-184">Her `services.Add<ServiceName>` genişletme yöntemi ekler (ve büyük olasılıkla yapılandırır) Hizmetleri.</span><span class="sxs-lookup"><span data-stu-id="2094f-184">Each `services.Add<ServiceName>` extension method adds (and potentially configures) services.</span></span> <span data-ttu-id="2094f-185">Örneğin, `services.AddMvc()` MVC gerektirir Hizmetleri ekler.</span><span class="sxs-lookup"><span data-stu-id="2094f-185">For example, `services.AddMvc()` adds the services MVC requires.</span></span> <span data-ttu-id="2094f-186">Genişletme yöntemleri yerleştirme bu kuralı izlemeniz önerilir `Microsoft.Extensions.DependencyInjection` hizmet kayıtlar grupları kapsüllemek için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="2094f-186">It's recommended that you follow this convention, placing extension methods in the `Microsoft.Extensions.DependencyInjection` namespace, to encapsulate groups of service registrations.</span></span>

<span data-ttu-id="2094f-187">`AddTransient` Yöntemi soyut türler bunu gerektiren her nesne için ayrı ayrı örneği somut Hizmetleri eşlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2094f-187">The `AddTransient` method is used to map abstract types to concrete services that are instantiated separately for every object that requires it.</span></span> <span data-ttu-id="2094f-188">Bu hizmetin bilinir *ömrü*, ve ek ömrü seçenekler aşağıda açıklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2094f-188">This is known as the service's *lifetime*, and additional lifetime options are described below.</span></span> <span data-ttu-id="2094f-189">Kaydettiğiniz hizmetlerinin her biri için uygun bir yaşam süresi seçmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="2094f-189">It's important to choose an appropriate lifetime for each of the services you register.</span></span> <span data-ttu-id="2094f-190">Hizmetinin yeni bir örneğini istediği her sınıf sağlanmalıdır?</span><span class="sxs-lookup"><span data-stu-id="2094f-190">Should a new instance of the service be provided to each class that requests it?</span></span> <span data-ttu-id="2094f-191">Bir örnek verilen web isteği kullanılsın mı?</span><span class="sxs-lookup"><span data-stu-id="2094f-191">Should one instance be used throughout a given web request?</span></span> <span data-ttu-id="2094f-192">Veya tek bir örnek uygulama ömrü boyunca kullanılmalıdır?</span><span class="sxs-lookup"><span data-stu-id="2094f-192">Or should a single instance be used for the lifetime of the application?</span></span>

<span data-ttu-id="2094f-193">Bu makalede örnek adlı karakter adları görüntüleyen basit bir denetleyici yok `CharactersController`.</span><span class="sxs-lookup"><span data-stu-id="2094f-193">In the sample for this article, there's a simple controller that displays character names, called `CharactersController`.</span></span> <span data-ttu-id="2094f-194">Kendi `Index` yöntemi uygulamada depolanan karakterler geçerli listesini görüntüler ve hiçbiri yoksa koleksiyon sayıda karakter ile başlatır.</span><span class="sxs-lookup"><span data-stu-id="2094f-194">Its `Index` method displays the current list of characters that have been stored in the application, and initializes the collection with a handful of characters if none exist.</span></span> <span data-ttu-id="2094f-195">Bu uygulama Entity Framework Çekirdek kullansa unutmayın ve `ApplicationDbContext` sınıfı için kendi Kalıcılık, hiçbiri denetleyicide görünür olan.</span><span class="sxs-lookup"><span data-stu-id="2094f-195">Note that although this application uses Entity Framework Core and the `ApplicationDbContext` class for its persistence, none of that's apparent in the controller.</span></span> <span data-ttu-id="2094f-196">Bunun yerine, belirli veri erişim mekanizması bir arabirim soyutlanır `ICharacterRepository`, hangi aşağıdaki [havuz deseni](http://deviq.com/repository-pattern/).</span><span class="sxs-lookup"><span data-stu-id="2094f-196">Instead, the specific data access mechanism has been abstracted behind an interface, `ICharacterRepository`, which follows the [repository pattern](http://deviq.com/repository-pattern/).</span></span> <span data-ttu-id="2094f-197">Örneği `ICharacterRepository` oluşturucu aracılığıyla istenir ve ardından gerekirse karakterleri erişmek için kullanılan bir özel alan atanmış.</span><span class="sxs-lookup"><span data-stu-id="2094f-197">An instance of `ICharacterRepository` is requested via the constructor and assigned to a private field, which is then used to access characters as necessary.</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

<span data-ttu-id="2094f-198">`ICharacterRepository` Denetleyicisinin gereksinim duyduğu çalışmak için iki yöntem tanımlar `Character` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="2094f-198">The `ICharacterRepository` defines the two methods the controller needs to work with `Character` instances.</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

<span data-ttu-id="2094f-199">Bu arabirim somut bir türde tarafından sırayla uygulanır `CharacterRepository`, çalışma zamanında kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2094f-199">This interface is in turn implemented by a concrete type, `CharacterRepository`, that's used at runtime.</span></span>

> [!NOTE]
> <span data-ttu-id="2094f-200">DI yolu ile kullanılan `CharacterRepository` tüm "depoları" ya da veri erişimi sınıfları, yalnızca uygulama hizmetlerinizi izleyin genel bir model bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="2094f-200">The way DI is used with the `CharacterRepository` class is a general model you can follow for all of your application services, not just in "repositories" or data access classes.</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

<span data-ttu-id="2094f-201">Unutmayın `CharacterRepository` isteklerini bir `ApplicationDbContext` kendi oluşturucusuna.</span><span class="sxs-lookup"><span data-stu-id="2094f-201">Note that `CharacterRepository` requests an `ApplicationDbContext` in its constructor.</span></span> <span data-ttu-id="2094f-202">Bunun gibi zincirleme bir şekilde sırayla kendi bağımlılıkları isteyen her istenen bağımlılık ile kullanılacak bağımlılık ekleme ait değil.</span><span class="sxs-lookup"><span data-stu-id="2094f-202">It's not unusual for dependency injection to be used in a chained fashion like this, with each requested dependency in turn requesting its own dependencies.</span></span> <span data-ttu-id="2094f-203">Kapsayıcı, tüm bağımlılıklarıyla grafikte çözme ve tam olarak çözümlenmiş hizmeti döndürüyor sorumludur.</span><span class="sxs-lookup"><span data-stu-id="2094f-203">The container is responsible for resolving all of the dependencies in the graph and returning the fully resolved service.</span></span>

> [!NOTE]
> <span data-ttu-id="2094f-204">İstenen nesne ve tüm gerektirdiği nesneleri ve tüm bu gerektiren nesneleri oluşturma bazen olarak adlandırılır bir *Nesne grafiği*.</span><span class="sxs-lookup"><span data-stu-id="2094f-204">Creating the requested object, and all of the objects it requires, and all of the objects those require, is sometimes referred to as an *object graph*.</span></span> <span data-ttu-id="2094f-205">Benzer şekilde, çözümlenmelidir bağımlılıkları toplu kümesini tipik olarak adlandırılır bir *bağımlılığı ağacı* veya *bağımlılık grafiğinin*.</span><span class="sxs-lookup"><span data-stu-id="2094f-205">Likewise, the collective set of dependencies that must be resolved is typically referred to as a *dependency tree* or *dependency graph*.</span></span>

<span data-ttu-id="2094f-206">Bu durumda, her ikisi de `ICharacterRepository` ve dolayısıyla `ApplicationDbContext` hizmetler kapsayıcısının ile kayıtlı olması gerekir `ConfigureServices` içinde `Startup`.</span><span class="sxs-lookup"><span data-stu-id="2094f-206">In this case, both `ICharacterRepository` and in turn `ApplicationDbContext` must be registered with the services container in `ConfigureServices` in `Startup`.</span></span> <span data-ttu-id="2094f-207">`ApplicationDbContext` genişletme yöntemi çağrısı ile yapılandırılmış `AddDbContext<T>`.</span><span class="sxs-lookup"><span data-stu-id="2094f-207">`ApplicationDbContext` is configured with the call to the extension method `AddDbContext<T>`.</span></span> <span data-ttu-id="2094f-208">Aşağıdaki kod kaydını gösterir `CharacterRepository` türü.</span><span class="sxs-lookup"><span data-stu-id="2094f-208">The following code shows the registration of the `CharacterRepository` type.</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

<span data-ttu-id="2094f-209">Entity Framework bağlamları Hizmetleri kullanarak kapsayıcı eklenmesi `Scoped` yaşam süresi.</span><span class="sxs-lookup"><span data-stu-id="2094f-209">Entity Framework contexts should be added to the services container using the `Scoped` lifetime.</span></span> <span data-ttu-id="2094f-210">Bu otomatik olarak yukarıda gösterildiği gibi yardımcı yöntemler kullanırsanız dikkate.</span><span class="sxs-lookup"><span data-stu-id="2094f-210">This is taken care of automatically if you use the helper methods as shown above.</span></span> <span data-ttu-id="2094f-211">Entity Framework'ü kullanın yapacak depoları aynı yaşam süresini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2094f-211">Repositories that will make use of Entity Framework should use the same lifetime.</span></span>

> [!WARNING]
> <span data-ttu-id="2094f-212">Çok dikkatli olmak ana tehlike çözümlenirken bir `Scoped` tek gelen hizmet.</span><span class="sxs-lookup"><span data-stu-id="2094f-212">The main danger to be wary of is resolving a `Scoped` service from a singleton.</span></span> <span data-ttu-id="2094f-213">Hizmet hatalı durum istekler işlenirken gerekir çalışması olasıdır.</span><span class="sxs-lookup"><span data-stu-id="2094f-213">It's likely in such a case that the service will have incorrect state when processing subsequent requests.</span></span>

<span data-ttu-id="2094f-214">Bağımlılıkları olan hizmetleri bunları kapsayıcısında kaydetmeniz.</span><span class="sxs-lookup"><span data-stu-id="2094f-214">Services that have dependencies should register them in the container.</span></span> <span data-ttu-id="2094f-215">Bir hizmetin yapıcı, ilkel gibi gerektiriyorsa bir `string`, bunu kullanarak yerleştirilebilir [yapılandırma](xref:fundamentals/configuration/index) ve [seçenekleri düzeni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="2094f-215">If a service's constructor requires a primitive, such as a `string`, this can be injected by using [configuration](xref:fundamentals/configuration/index) and the [options pattern](xref:fundamentals/configuration/options).</span></span>

## <a name="service-lifetimes-and-registration-options"></a><span data-ttu-id="2094f-216">Hizmet yaşam süresi ve kayıt seçenekleri</span><span class="sxs-lookup"><span data-stu-id="2094f-216">Service lifetimes and registration options</span></span>

<span data-ttu-id="2094f-217">ASP.NET Hizmetleri ile aşağıdaki ömürleri yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="2094f-217">ASP.NET services can be configured with the following lifetimes:</span></span>

<span data-ttu-id="2094f-218">**Geçici**</span><span class="sxs-lookup"><span data-stu-id="2094f-218">**Transient**</span></span>

<span data-ttu-id="2094f-219">Geçici ömrü Hizmetleri istedikleri her zaman oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2094f-219">Transient lifetime services are created each time they're requested.</span></span> <span data-ttu-id="2094f-220">Bu ömrü basit, durum bilgisi olmayan hizmetler için en iyi şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="2094f-220">This lifetime works best for lightweight, stateless services.</span></span>

<span data-ttu-id="2094f-221">**Kapsamlı**</span><span class="sxs-lookup"><span data-stu-id="2094f-221">**Scoped**</span></span>

<span data-ttu-id="2094f-222">Kapsamlı ömrü Hizmetleri istek başına bir kez oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2094f-222">Scoped lifetime services are created once per request.</span></span>

> [!WARNING]
> <span data-ttu-id="2094f-223">Bir ara yazılımında kapsamlı bir hizmeti kullanıyorsanız hizmete Ekle `Invoke` veya `InvokeAsync` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2094f-223">If you're using a scoped service in a middleware, inject the service into the `Invoke` or `InvokeAsync` method.</span></span> <span data-ttu-id="2094f-224">Singleton gibi davranır hizmete zorladığından Oluşturucu ekleme Ekle yok.</span><span class="sxs-lookup"><span data-stu-id="2094f-224">Don't inject via constructor injection because it forces the service to behave like a singleton.</span></span>

<span data-ttu-id="2094f-225">**singleton**</span><span class="sxs-lookup"><span data-stu-id="2094f-225">**Singleton**</span></span>

<span data-ttu-id="2094f-226">Singleton ömrü Hizmetleri istenen ilk kez oluşturulur (veya ne zaman `ConfigureServices` bir örneği var. belirtirseniz çalıştırın) ve ardından sonraki her istek aynı örneğini kullanır.</span><span class="sxs-lookup"><span data-stu-id="2094f-226">Singleton lifetime services are created the first time they're requested (or when `ConfigureServices` is run if you specify an instance there) and then every subsequent request will use the same instance.</span></span> <span data-ttu-id="2094f-227">Uygulamanızı singleton davranışı gerektiriyorsa, hizmetin ömrünü yönetmek hizmet kapsayıcı izin vererek singleton tasarım deseni uygulama ve sınıf, nesnenin yaşam süresi kendiniz yönetmek yerine önerilir.</span><span class="sxs-lookup"><span data-stu-id="2094f-227">If your application requires singleton behavior, allowing the services container to manage the service's lifetime is recommended instead of implementing the singleton design pattern and managing your object's lifetime in the class yourself.</span></span>

<span data-ttu-id="2094f-228">Hizmetleri, çeşitli şekillerde kapsayıcısı ile kaydedilebilir.</span><span class="sxs-lookup"><span data-stu-id="2094f-228">Services can be registered with the container in several ways.</span></span> <span data-ttu-id="2094f-229">Biz, zaten bir hizmet uygulaması ile belirli bir türde kullanmak için somut türde belirterek nasıl gördünüz.</span><span class="sxs-lookup"><span data-stu-id="2094f-229">We have already seen how to register a service implementation with a given type by specifying the concrete type to use.</span></span> <span data-ttu-id="2094f-230">Ayrıca, bir Fabrika, ardından isteğe bağlı olarak örnek oluşturmak için kullanılacak olan belirtilebilir.</span><span class="sxs-lookup"><span data-stu-id="2094f-230">In addition, a factory can be specified, which will then be used to create the instance on demand.</span></span> <span data-ttu-id="2094f-231">Üçüncü yaklaşım, kapsayıcı durumu hiçbir zaman bir örnek oluşturmaya çalışacak (ya da örneği dispose) kullanmak için türün örneğini doğrudan belirtmektir.</span><span class="sxs-lookup"><span data-stu-id="2094f-231">The third approach is to directly specify the instance of the type to use, in which case the container will never attempt to create an instance (nor will it dispose of the instance).</span></span>

<span data-ttu-id="2094f-232">Bu yaşam süresi ve kaydını seçenekleri arasındaki farkı göstermek için bir veya daha çok görev olarak temsil eden basit bir arabirim göz önünde bulundurun. bir *işlemi* benzersiz bir tanımlayıcı `OperationId`.</span><span class="sxs-lookup"><span data-stu-id="2094f-232">To demonstrate the difference between these lifetime and registration options, consider a simple interface that represents one or more tasks as an *operation* with a unique identifier, `OperationId`.</span></span> <span data-ttu-id="2094f-233">Nasıl Biz bu hizmet için kullanım ömrünü nasıl yapılandırdığınıza bağlı olarak, kapsayıcı ya da aynı veya farklı örneklerini isteyen sınıfına hizmet sağlar.</span><span class="sxs-lookup"><span data-stu-id="2094f-233">Depending on how we configure the lifetime for this service, the container will provide either the same or different instances of the service to the requesting class.</span></span> <span data-ttu-id="2094f-234">Hangi ömrü istenen temizleyin yapmak için bir türü ömür seçeneği başına oluşturacağız:</span><span class="sxs-lookup"><span data-stu-id="2094f-234">To make it clear which lifetime is being requested, we will create one type per lifetime option:</span></span>

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

<span data-ttu-id="2094f-235">Biz bu arabirimleri kullanarak tek bir sınıf uygulama `Operation`, kabul eden bir `Guid` kendi Oluşturucusu ya da yeni bir kullanır `Guid` hiçbiri sağlanmazsa.</span><span class="sxs-lookup"><span data-stu-id="2094f-235">We implement these interfaces using a single class, `Operation`, that accepts a `Guid` in its constructor, or uses a new `Guid` if none is provided.</span></span>

<span data-ttu-id="2094f-236">Ardından `ConfigureServices`, her tür adlandırılmış yaşam göre kapsayıcı eklenir:</span><span class="sxs-lookup"><span data-stu-id="2094f-236">Next, in `ConfigureServices`, each type is added to the container according to its named lifetime:</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

<span data-ttu-id="2094f-237">Unutmayın `IOperationSingletonInstance` hizmet bilinen bir kimliği ile belirli bir örneği kullanarak `Guid.Empty` bu türü (GUID'sine tümüyle sıfırlardan olacaktır) kullanımda olduğunda Temizle olur.</span><span class="sxs-lookup"><span data-stu-id="2094f-237">Note that the `IOperationSingletonInstance` service is using a specific instance with a known ID of `Guid.Empty` so it will be clear when this type is in use (its Guid will be all zeroes).</span></span> <span data-ttu-id="2094f-238">Biz de kayıtlı bir `OperationService` her, diğer bağımlıdır `Operation` türleri temizleyin isteği içinde bu hizmet denetleyicisi ya da her işlem türü için yeni bir tane olarak aynı örneği olup olmadığını alma olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="2094f-238">We have also registered an `OperationService` that depends on each of the other `Operation` types, so that it will be clear within a request whether this service is getting the same instance as the controller, or a new one, for each operation type.</span></span> <span data-ttu-id="2094f-239">Bu hizmeti yapar şey görünümünde görüntülenen şekilde bağımlılıklarını özellikleri olarak kullanıma sunar.</span><span class="sxs-lookup"><span data-stu-id="2094f-239">All this service does is expose its dependencies as properties, so they can be displayed in the view.</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

<span data-ttu-id="2094f-240">Nesne yaşam süresi içinde ve uygulama için ayrı ayrı istekler arasında göstermek için örnek içeren bir `OperationsController` her tür isteklerine `IOperation` türü yanı sıra bir `OperationService`.</span><span class="sxs-lookup"><span data-stu-id="2094f-240">To demonstrate the object lifetimes within and between separate individual requests to the application, the sample includes an `OperationsController` that requests each kind of `IOperation` type as well as an `OperationService`.</span></span> <span data-ttu-id="2094f-241">`Index` Sonra eylemi görüntüler tüm denetleyicinin ve hizmetin `OperationId` değerleri.</span><span class="sxs-lookup"><span data-stu-id="2094f-241">The `Index` action then displays all of the controller's and service's `OperationId` values.</span></span>

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

<span data-ttu-id="2094f-242">Şimdi iki ayrı Bu denetleyici eylemi için yapılan isteklerinin:</span><span class="sxs-lookup"><span data-stu-id="2094f-242">Now two separate requests are made to this controller action:</span></span>

![Microsoft Edge geçici, kapsamındaki, tek ve örnek denetleyicisi ve işlem hizmeti işlemleri için ilk istek işlemi kimliği değerleri (GUID'ın) gösteren çalışan bağımlılık ekleme örnek web uygulaması işlemleri görünümü.](dependency-injection/_static/lifetimes_request1.png)

![İkinci bir istek için işlem kimliği değerleri gösteren işlemlerini görüntüleyin.](dependency-injection/_static/lifetimes_request2.png)

<span data-ttu-id="2094f-245">Hangi gözlemlemek `OperationId` değerleri, bir istek içinde ve istekler arasında değişir.</span><span class="sxs-lookup"><span data-stu-id="2094f-245">Observe which of the `OperationId` values vary within a request, and between requests.</span></span>

* <span data-ttu-id="2094f-246">*Geçici* nesneleri farklı her zaman; her denetleyici ve her hizmet için yeni bir örnek sağlanır.</span><span class="sxs-lookup"><span data-stu-id="2094f-246">*Transient* objects are always different; a new instance is provided to every controller and every service.</span></span>

* <span data-ttu-id="2094f-247">*Kapsamlı* nesneleri aynıdır ancak farklı istekler arasında farklı bir istek içinde</span><span class="sxs-lookup"><span data-stu-id="2094f-247">*Scoped* objects are the same within a request, but different across different requests</span></span>

* <span data-ttu-id="2094f-248">*Singleton* nesneleridir aynı her nesne ve her istek için (örneği içinde olup olmadığını sağlanan bağımsız olarak `ConfigureServices`)</span><span class="sxs-lookup"><span data-stu-id="2094f-248">*Singleton* objects are the same for every object and every request (regardless of whether an instance is provided in `ConfigureServices`)</span></span>

## <a name="resolve-a-scoped-service-within-the-application-scope"></a><span data-ttu-id="2094f-249">Uygulama kapsamındaki kapsamlı hizmetinin çözümleyin</span><span class="sxs-lookup"><span data-stu-id="2094f-249">Resolve a scoped service within the application scope</span></span>

<span data-ttu-id="2094f-250">Oluşturma bir [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) ile [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) uygulamanın kapsam içinde kapsamlı bir hizmeti çözümlemek için.</span><span class="sxs-lookup"><span data-stu-id="2094f-250">Create an [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) with [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to resolve a scoped service within the app's scope.</span></span> <span data-ttu-id="2094f-251">Bu yaklaşım başlatma görevleri çalıştırmak için başlangıçta kapsamlı bir hizmete erişmek kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="2094f-251">This approach is useful to access a scoped service at startup to run initialization tasks.</span></span> <span data-ttu-id="2094f-252">Aşağıdaki örnekte bir bağlam için edinmeyi gösteren `MyScopedService` içinde `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="2094f-252">The following example shows how to obtain a context for the `MyScopedService` in `Program.Main`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a><span data-ttu-id="2094f-253">Kapsam doğrulama</span><span class="sxs-lookup"><span data-stu-id="2094f-253">Scope validation</span></span>

<span data-ttu-id="2094f-254">Uygulama geliştirme ortamında ASP.NET Core 2.0 veya sonraki sürümlerde çalıştırırken, varsayılan hizmet sağlayıcısı doğrulamak üzere denetler:</span><span class="sxs-lookup"><span data-stu-id="2094f-254">When the app is running in the Development environment on ASP.NET Core 2.0 or later, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="2094f-255">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak kök servis sağlayıcısı'ndan çözülmüş değil.</span><span class="sxs-lookup"><span data-stu-id="2094f-255">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="2094f-256">Kapsamlı Hizmetleri doğrudan veya dolaylı olarak teklileri eklenen değil.</span><span class="sxs-lookup"><span data-stu-id="2094f-256">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="2094f-257">Kök hizmet sağlayıcısı oluşturulur [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2094f-257">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="2094f-258">Kök hizmet sağlayıcısının ömrü zaman sağlayıcı uygulamayla başlatır ve uygulamayı kapatıldığında atıldı uygulama/sunucusunun ömrü karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="2094f-258">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="2094f-259">Kapsamlı Hizmetleri oluşturuldukları kapsayıcı tarafından elden.</span><span class="sxs-lookup"><span data-stu-id="2094f-259">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="2094f-260">Kapsamlı bir hizmet kök kapsayıcısında oluşturduysanız, uygulama/sunucu kapatıldığında yalnızca kök kapsayıcı tarafından atıldı çünkü hizmetin ömrü tekliye etkili bir şekilde yükseltildi.</span><span class="sxs-lookup"><span data-stu-id="2094f-260">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="2094f-261">Hizmet kapsamları doğrulama yakalar bu durumlarda, `BuildServiceProvider` olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2094f-261">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="2094f-262">Daha fazla bilgi için bkz: [kapsam doğrulama barındırma konusunda](xref:fundamentals/hosting#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="2094f-262">For more information, see [Scope validation in the Hosting topic](xref:fundamentals/hosting#scope-validation).</span></span>

## <a name="request-services"></a><span data-ttu-id="2094f-263">İstek Hizmetleri</span><span class="sxs-lookup"><span data-stu-id="2094f-263">Request Services</span></span>

<span data-ttu-id="2094f-264">Bir ASP.NET içinde kullanılabilir hizmetler isteği `HttpContext` aracılığıyla sunulan `RequestServices` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="2094f-264">The services available within an ASP.NET request from `HttpContext` are exposed through the `RequestServices` collection.</span></span>

![İstek Hizmetleri alır veya ayarlar isteğin hizmet kapsayıcısı erişim sağlayan IServiceProvider belirten HttpContext isteği Hizmetleri IntelliSense bağlamsal iletişim kutusu.](dependency-injection/_static/request-services.png)

<span data-ttu-id="2094f-266">İstek hizmetleri yapılandırmak ve uygulamanızı bir parçası olarak istek Hizmetleri temsil eder.</span><span class="sxs-lookup"><span data-stu-id="2094f-266">Request Services represent the services you configure and request as part of your application.</span></span> <span data-ttu-id="2094f-267">Nesnelerinizi bağımlılıklarını belirttiğinizde, bunlar bulunan tür tarafından karşılanır `RequestServices`değil `ApplicationServices`.</span><span class="sxs-lookup"><span data-stu-id="2094f-267">When your objects specify dependencies, these are satisfied by the types found in `RequestServices`, not `ApplicationServices`.</span></span>

<span data-ttu-id="2094f-268">Genellikle, bunun yerine sınıfınızın oluşturucu aracılığıyla gerektiren sınıflarınızı istek türleri tercih ederek ve bu bağımlılıklar Ekle framework izin vererek, doğrudan bu özellikleri kullanmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="2094f-268">Generally, you shouldn't use these properties directly, preferring instead to request the types your classes you require via your class's constructor, and letting the framework inject these dependencies.</span></span> <span data-ttu-id="2094f-269">Bu test etmek daha kolay olan sınıfları verir (bkz [Test ve hata ayıklama](../testing/index.md)) ve daha geniş bağlı değildir.</span><span class="sxs-lookup"><span data-stu-id="2094f-269">This yields classes that are easier to test (see [Test and debug](../testing/index.md)) and are more loosely coupled.</span></span>

> [!NOTE]
> <span data-ttu-id="2094f-270">Erişim için Oluşturucusu parametre olarak bağımlılıkları isteyen tercih `RequestServices` koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="2094f-270">Prefer requesting dependencies as constructor parameters to accessing the `RequestServices` collection.</span></span>

## <a name="designing-services-for-dependency-injection"></a><span data-ttu-id="2094f-271">Bağımlılık ekleme Hizmetleri Tasarlama</span><span class="sxs-lookup"><span data-stu-id="2094f-271">Designing services for dependency injection</span></span>

<span data-ttu-id="2094f-272">Bağımlılık ekleme kendi ortak çalışanlar almak için kullanılacak hizmetlerinizi tasarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2094f-272">You should design your services to use dependency injection to get their collaborators.</span></span> <span data-ttu-id="2094f-273">Bu durum bilgisi olan statik yöntem çağrılarını kullanımını önleme anlamına gelir (hangi neden olarak bilinen bir kod kokusu [statik cling](http://deviq.com/static-cling/)) ve hizmetlerinizi içinde bağımlı sınıfların doğrudan örnek oluşturma.</span><span class="sxs-lookup"><span data-stu-id="2094f-273">This means avoiding the use of stateful static method calls (which result in a code smell known as [static cling](http://deviq.com/static-cling/)) and the direct instantiation of dependent classes within your services.</span></span> <span data-ttu-id="2094f-274">Tümcecik anımsaması yardımcı olabilecek [Birleştirici yenilikleri](https://ardalis.com/new-is-glue), bir türü örneği mi, yoksa bağımlılık ekleme istemek için seçerken.</span><span class="sxs-lookup"><span data-stu-id="2094f-274">It may help to remember the phrase, [New is Glue](https://ardalis.com/new-is-glue), when choosing whether to instantiate a type or to request it via dependency injection.</span></span> <span data-ttu-id="2094f-275">İzleyerek [DÜZ ilkeler, nesne yönelimli tasarımı](http://deviq.com/solid/), sınıflarınızı doğal olarak küçük, iyi faktörlere göre ve kolayca sınanan olma eğilimindedir.</span><span class="sxs-lookup"><span data-stu-id="2094f-275">By following the [SOLID Principles of Object Oriented Design](http://deviq.com/solid/), your classes will naturally tend to be small, well-factored, and easily tested.</span></span>

<span data-ttu-id="2094f-276">Ne sınıflarınızı şekilde eklenmiş olan çok sayıda bağımlılıklara sahip olma eğilimi gösterir bulduğunuz?</span><span class="sxs-lookup"><span data-stu-id="2094f-276">What if you find that your classes tend to have way too many dependencies being injected?</span></span> <span data-ttu-id="2094f-277">Bu, genellikle sınıfınız çok işlemini yapmaya çalışan ve büyük olasılıkla SRP - ihlal eden bir oturum [tek sorumluluk ilkesine](http://deviq.com/single-responsibility-principle/).</span><span class="sxs-lookup"><span data-stu-id="2094f-277">This is generally a sign that your class is trying to do too much, and is probably violating SRP - the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/).</span></span> <span data-ttu-id="2094f-278">Yeni bir sınıf bazı sorumlulukları taşıyarak sınıfı yeniden varsa bkz.</span><span class="sxs-lookup"><span data-stu-id="2094f-278">See if you can refactor the class by moving some of its responsibilities into a new class.</span></span> <span data-ttu-id="2094f-279">Unutmayın, `Controller` sınıfları odaklanmış, UI sorunlarını iş kuralları ve veri erişim uygulama ayrıntılarını bunları uygun sınıflardaki tutulması gerektiği şekilde [ayrı sorunları](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="2094f-279">Keep in mind that your `Controller` classes should be focused on UI concerns, so business rules and data access implementation details should be kept in classes appropriate to these [separate concerns](http://deviq.com/separation-of-concerns/).</span></span>

<span data-ttu-id="2094f-280">Veri erişimi göre özellikle ekleyemezsiniz `DbContext` denetleyicilerinizi içine (varsayılarak hizmetler kapsayıcısının EF ekledik `ConfigureServices`).</span><span class="sxs-lookup"><span data-stu-id="2094f-280">With regards to data access specifically, you can inject the `DbContext` into your controllers (assuming you've added EF to the services container in `ConfigureServices`).</span></span> <span data-ttu-id="2094f-281">Bazı geliştiriciler veritabanına bir depo arabirimi kullanmayı tercih ederseniz injecting yerine `DbContext` doğrudan.</span><span class="sxs-lookup"><span data-stu-id="2094f-281">Some developers prefer to use a repository interface to the database rather than injecting the `DbContext` directly.</span></span> <span data-ttu-id="2094f-282">Veri kapsülleyen bir arabirimini kullanarak tek bir yerde erişim mantığı veritabanınızı değiştiğinde değiştirmeniz gerekecektir basamak sayısını en aza indirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2094f-282">Using an interface to encapsulate the data access logic in one place can minimize how many places you will have to change when your database changes.</span></span>

### <a name="disposing-of-services"></a><span data-ttu-id="2094f-283">Hizmetlerin atma</span><span class="sxs-lookup"><span data-stu-id="2094f-283">Disposing of services</span></span>

<span data-ttu-id="2094f-284">Kapsayıcı çağıracak `Dispose` için `IDisposable` türleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2094f-284">The container will call `Dispose` for `IDisposable` types it creates.</span></span> <span data-ttu-id="2094f-285">Bir örneği kapsayıcıya kendiniz eklerseniz, ancak bunu silinip.</span><span class="sxs-lookup"><span data-stu-id="2094f-285">However, if you add an instance to the container yourself, it will not be disposed.</span></span>

<span data-ttu-id="2094f-286">Örnek:</span><span class="sxs-lookup"><span data-stu-id="2094f-286">Example:</span></span>

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> <span data-ttu-id="2094f-287">Sürüm 1.0, kapsayıcı adlı dispose üzerinde *tüm* `IDisposable` kaydetmedi oluşturma dahil olmak üzere nesneleri.</span><span class="sxs-lookup"><span data-stu-id="2094f-287">In version 1.0, the container called dispose on *all* `IDisposable` objects, including those it didn't create.</span></span>

## <a name="replacing-the-default-services-container"></a><span data-ttu-id="2094f-288">Varsayılan hizmet kapsayıcı değiştirme</span><span class="sxs-lookup"><span data-stu-id="2094f-288">Replacing the default services container</span></span>

<span data-ttu-id="2094f-289">Yerleşik hizmet kapsayıcı framework üzerinde oluşturulan çoğu tüketici uygulamalar ve temel gereksinimlerini karşılamak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2094f-289">The built-in services container is meant to serve the basic needs of the framework and most consumer applications built on it.</span></span> <span data-ttu-id="2094f-290">Ancak, geliştiricilerin kendi tercih edilen kapsayıcı ile yerleşik kapsayıcı değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2094f-290">However, developers can replace the built-in container with their preferred container.</span></span> <span data-ttu-id="2094f-291">`ConfigureServices` Yöntemi genellikle döndürür `void`, ancak imzası döndürülecek değiştirilirse `IServiceProvider`, farklı bir kapsayıcı yapılandırılmış ve döndürdü.</span><span class="sxs-lookup"><span data-stu-id="2094f-291">The `ConfigureServices` method typically returns `void`, but if its signature is changed to return `IServiceProvider`, a different container can be configured and returned.</span></span> <span data-ttu-id="2094f-292">.NET için birçok IOC kapsayıcıları vardır.</span><span class="sxs-lookup"><span data-stu-id="2094f-292">There are many IOC containers available for .NET.</span></span> <span data-ttu-id="2094f-293">Bu örnekte, [Autofac](https://autofac.org/) paketi kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2094f-293">In this example, the [Autofac](https://autofac.org/) package is used.</span></span>

<span data-ttu-id="2094f-294">Önce uygun bir kapsayıcı paketler yükleyin:</span><span class="sxs-lookup"><span data-stu-id="2094f-294">First, install the appropriate container package(s):</span></span>

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

<span data-ttu-id="2094f-295">Ardından, kapsayıcısında yapılandırın `ConfigureServices` ve dönüş bir `IServiceProvider`:</span><span class="sxs-lookup"><span data-stu-id="2094f-295">Next, configure the container in `ConfigureServices` and return an `IServiceProvider`:</span></span>

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add other framework services

    // Add Autofac
    var containerBuilder = new ContainerBuilder();
    containerBuilder.RegisterModule<DefaultModule>();
    containerBuilder.Populate(services);
    var container = containerBuilder.Build();
    return new AutofacServiceProvider(container);
}
```

> [!NOTE]
> <span data-ttu-id="2094f-296">Bir üçüncü taraf dı kapsayıcı kullanırken değiştirmelisiniz `ConfigureServices` döndürdüğü böylece `IServiceProvider` yerine `void`.</span><span class="sxs-lookup"><span data-stu-id="2094f-296">When using a third-party DI container, you must change `ConfigureServices` so that it returns `IServiceProvider` instead of `void`.</span></span>

<span data-ttu-id="2094f-297">Son olarak, normal olarak Autofac yapılandırma `DefaultModule`:</span><span class="sxs-lookup"><span data-stu-id="2094f-297">Finally, configure Autofac as normal in `DefaultModule`:</span></span>

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

<span data-ttu-id="2094f-298">Çalışma zamanında Autofac türleri çözümlemek ve bağımlılıkları ekleme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2094f-298">At runtime, Autofac will be used to resolve types and inject dependencies.</span></span> <span data-ttu-id="2094f-299">[Autofac ve ASP.NET Core kullanma hakkında daha fazla bilgi](http://docs.autofac.org/en/latest/integration/aspnetcore.html).</span><span class="sxs-lookup"><span data-stu-id="2094f-299">[Learn more about using Autofac and ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).</span></span>

### <a name="thread-safety"></a><span data-ttu-id="2094f-300">İş parçacığı güvenliği</span><span class="sxs-lookup"><span data-stu-id="2094f-300">Thread safety</span></span>

<span data-ttu-id="2094f-301">Singleton Hizmetleri iş parçacığı içinde korumalı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2094f-301">Singleton services need to be thread safe.</span></span> <span data-ttu-id="2094f-302">Tek bir hizmet üzerinde geçici bir hizmet bağımlılık varsa, geçici hizmet iş parçacığı tarafından singleton nasıl kullanıldığı bağlı olarak güvenli olacak şekilde de gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="2094f-302">If a singleton service has a dependency on a transient service, the transient service may also need to be thread safe depending how it's used by the singleton.</span></span>

## <a name="recommendations"></a><span data-ttu-id="2094f-303">Önerileri</span><span class="sxs-lookup"><span data-stu-id="2094f-303">Recommendations</span></span>

<span data-ttu-id="2094f-304">Bağımlılık ekleme ile çalışırken, aşağıdaki önerileri göz önünde bulundurun:</span><span class="sxs-lookup"><span data-stu-id="2094f-304">When working with dependency injection, keep the following recommendations in mind:</span></span>

* <span data-ttu-id="2094f-305">DI, karmaşık bağımlılıklara sahip nesneleri için durumdadır.</span><span class="sxs-lookup"><span data-stu-id="2094f-305">DI is for objects that have complex dependencies.</span></span> <span data-ttu-id="2094f-306">Denetleyicileri, hizmetleri, bağdaştırıcılar ve depoları için dı eklenebilecek nesneler için tüm örnektir.</span><span class="sxs-lookup"><span data-stu-id="2094f-306">Controllers, services, adapters, and repositories are all examples of objects that might be added to DI.</span></span>

* <span data-ttu-id="2094f-307">Verileri ve yapılandırma doğrudan dı içinde saklamaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="2094f-307">Avoid storing data and configuration directly in DI.</span></span> <span data-ttu-id="2094f-308">Örneğin, bir kullanıcının alışveriş sepeti Hizmetleri kapsayıcıya genellikle eklenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2094f-308">For example, a user's shopping cart shouldn't typically be added to the services container.</span></span> <span data-ttu-id="2094f-309">Yapılandırma kullanması gereken [seçenekleri düzeni](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="2094f-309">Configuration should use the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="2094f-310">Benzer şekilde, yalnızca başka bir nesne erişmesine izin vermek için mevcut "veri sahibi" nesneleri kaçının.</span><span class="sxs-lookup"><span data-stu-id="2094f-310">Similarly, avoid "data holder" objects that only exist to allow access to some other object.</span></span> <span data-ttu-id="2094f-311">İstek dı mümkünse gereken gerçek öğesi daha iyidir.</span><span class="sxs-lookup"><span data-stu-id="2094f-311">It's better to request the actual item needed via DI, if possible.</span></span>

* <span data-ttu-id="2094f-312">Statik hizmetlerine erişim kaçının.</span><span class="sxs-lookup"><span data-stu-id="2094f-312">Avoid static access to services.</span></span>

* <span data-ttu-id="2094f-313">Hizmet konumu, uygulama kodunuzda kaçının.</span><span class="sxs-lookup"><span data-stu-id="2094f-313">Avoid service location in your application code.</span></span>

* <span data-ttu-id="2094f-314">Statik erişimi önlemek `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="2094f-314">Avoid static access to `HttpContext`.</span></span>

> [!NOTE]
> <span data-ttu-id="2094f-315">Gibi tüm ayarlar önerilerin bir yoksayılıyor gerekli olduğu durumlar karşılaşabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2094f-315">Like all sets of recommendations, you may encounter situations where ignoring one is required.</span></span> <span data-ttu-id="2094f-316">Nadir--istisnaları framework çoğunlukla çok özel durumlarını bulduk.</span><span class="sxs-lookup"><span data-stu-id="2094f-316">We have found exceptions to be rare -- mostly very special cases within the framework itself.</span></span>

<span data-ttu-id="2094f-317">Bağımlılık ekleme olduğunu unutmayın bir *alternatif* statik/genel nesne erişim desenler için.</span><span class="sxs-lookup"><span data-stu-id="2094f-317">Remember, dependency injection is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="2094f-318">Statik nesne erişimi ile karıştırmak istiyorsanız dı faydaları hayata geçirmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="2094f-318">You won't be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2094f-319">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2094f-319">Additional resources</span></span>

* [<span data-ttu-id="2094f-320">Uygulama Başlatma</span><span class="sxs-lookup"><span data-stu-id="2094f-320">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="2094f-321">Test ve hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="2094f-321">Test and debug</span></span>](xref:testing/index)
* [<span data-ttu-id="2094f-322">Ara yazılımı Fabrika tabanlı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="2094f-322">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="2094f-323">Bağımlılık ekleme (MSDN) ile ASP.NET Core temiz kod yazma</span><span class="sxs-lookup"><span data-stu-id="2094f-323">Writing Clean Code in ASP.NET Core with Dependency Injection (MSDN)</span></span>](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [<span data-ttu-id="2094f-324">Kapsayıcı yönetilen uygulama tasarımı, Prelude: Burada kapsayıcı ait mu?</span><span class="sxs-lookup"><span data-stu-id="2094f-324">Container-Managed Application Design, Prelude: Where does the Container Belong?</span></span>](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [<span data-ttu-id="2094f-325">Açık bağımlılıkları İlkesi</span><span class="sxs-lookup"><span data-stu-id="2094f-325">Explicit Dependencies Principle</span></span>](http://deviq.com/explicit-dependencies-principle/)
* <span data-ttu-id="2094f-326">[Denetimi kapsayıcıları ve bağımlılık ekleme düzeni ters çevirmeyi](https://www.martinfowler.com/articles/injection.html) (Fowler)</span><span class="sxs-lookup"><span data-stu-id="2094f-326">[Inversion of Control Containers and the Dependency Injection Pattern](https://www.martinfowler.com/articles/injection.html) (Fowler)</span></span>
