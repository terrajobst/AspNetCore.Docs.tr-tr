---
title: ASP.NET Core ile depo deseni
author: ardalis
description: ASP.NET Core uygulamanızı depo uygulama tasarım deseni uygulamayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2018
uid: fundamentals/repository-pattern
ms.openlocfilehash: d64c3d090f62c580ebc05a6bbc2744a4508bb9bc
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342770"
---
# <a name="repository-pattern-with-aspnet-core"></a><span data-ttu-id="82a6f-103">ASP.NET Core ile depo deseni</span><span class="sxs-lookup"><span data-stu-id="82a6f-103">Repository pattern with ASP.NET Core</span></span>

<span data-ttu-id="82a6f-104">Tarafından [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), ve [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="82a6f-104">By [Steve Smith](https://ardalis.com/), [Scott Addie](https://scottaddie.com), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="82a6f-105">*Depo deseni* veri erişimi soyutlama arabirimi arkasında yalıtan bir tasarım örüntüsüdür.</span><span class="sxs-lookup"><span data-stu-id="82a6f-105">The *repository pattern* is a design pattern that isolates data access behind interface abstractions.</span></span> <span data-ttu-id="82a6f-106">Veritabanına bağlanma ve veri depolama nesnelerini düzenleme, arabirim uygulama tarafından sağlanan yöntemleri aracılığıyla gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="82a6f-106">Connecting to the database and manipulating data storage objects is performed through methods provided by the interface's implementation.</span></span> <span data-ttu-id="82a6f-107">Sonuç olarak, bağlantılar, komutları ve okuyucular gibi veritabanı konuları uğraşmanız kodu çağırmak için gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="82a6f-107">Consequently, there's no need for calling code to deal with database concerns, such as connections, commands, and readers.</span></span>

<span data-ttu-id="82a6f-108">ASP.NET Core ile depo düzeninin uygulaması aşağıdaki faydaları sağlar:</span><span class="sxs-lookup"><span data-stu-id="82a6f-108">Implementation of the repository pattern with ASP.NET Core has the following benefits:</span></span>

* <span data-ttu-id="82a6f-109">Uygulamanın daha az karmaşık iş ve veri erişim katmanları arasında doğrudan hiçbir karşılıklı bağımlılığı olan kuruluştur.</span><span class="sxs-lookup"><span data-stu-id="82a6f-109">Organization of the app is less complex with no direct interdependency between the business and data access layers.</span></span>
* <span data-ttu-id="82a6f-110">Kod merkezi olarak bir veya daha fazla depo tarafından yönetildiğinden veritabanı erişim kodu yeniden kullanmak daha kolaydır.</span><span class="sxs-lookup"><span data-stu-id="82a6f-110">It's easier to reuse database access code because the code is centrally managed by one or more repositories.</span></span>
* <span data-ttu-id="82a6f-111">İş etki alanı bağımsız olarak veritabanı katmanından test birimi olabilir.</span><span class="sxs-lookup"><span data-stu-id="82a6f-111">The business domain can be independently unit tested from the database layer.</span></span>

<span data-ttu-id="82a6f-112">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="82a6f-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="82a6f-113">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) başlatıp film karakter adlarının bir listesini görüntülemek için havuz deseni kullanır.</span><span class="sxs-lookup"><span data-stu-id="82a6f-113">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/repository-pattern/samples) uses the repository pattern to initialize and display a list of movie character names.</span></span> <span data-ttu-id="82a6f-114">Uygulamanın kullandığı [Entity Framework Core](/ef/core/) ve `ApplicationDbContext` verilere nasıl erişildiği burada sınıfı, veri kalıcılığı, ancak veritabanı altyapısı için görünür değildir.</span><span class="sxs-lookup"><span data-stu-id="82a6f-114">The app uses [Entity Framework Core](/ef/core/) and an `ApplicationDbContext` class for its data persistence, but the database infrastructure isn't apparent where the data is accessed.</span></span> <span data-ttu-id="82a6f-115">Veri erişimi ve veritabanı nesneleri soyutlanır arkasındaki bir [depo](https://martinfowler.com/eaaCatalog/repository.html).</span><span class="sxs-lookup"><span data-stu-id="82a6f-115">Data access and database objects are abstracted behind a [repository](https://martinfowler.com/eaaCatalog/repository.html).</span></span>

## <a name="repository-interface"></a><span data-ttu-id="82a6f-116">Depo Arabirimi</span><span class="sxs-lookup"><span data-stu-id="82a6f-116">Repository interface</span></span>

<span data-ttu-id="82a6f-117">Özellikleri ve yöntemleri uygulaması için bir depo arabirimi tanımlar.</span><span class="sxs-lookup"><span data-stu-id="82a6f-117">A repository interface defines the properties and methods for implementation.</span></span> <span data-ttu-id="82a6f-118">Örnek uygulamada, film karakter veri deposu arabirimidir `ICharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="82a6f-118">In the sample app, the repository interface for movie character data is `ICharacterRepository`.</span></span> <span data-ttu-id="82a6f-119">`ICharacterRepository` tanımlar `ListAll` ve `Add` ile çalışması için gereken yöntemleri `Character` uygulama örnekleri:</span><span class="sxs-lookup"><span data-stu-id="82a6f-119">`ICharacterRepository` defines the `ListAll` and `Add` methods required to work with `Character` instances in the app:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Interfaces/ICharacterRepository.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="82a6f-120">`Character` olarak tanımlanır:</span><span class="sxs-lookup"><span data-stu-id="82a6f-120">`Character` is defined as:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/Character.cs?name=snippet1)]

::: moniker-end

## <a name="repository-concrete-type"></a><span data-ttu-id="82a6f-121">Depo somut türü</span><span class="sxs-lookup"><span data-stu-id="82a6f-121">Repository concrete type</span></span>

<span data-ttu-id="82a6f-122">Arabirim somut bir tür tarafından uygulanır.</span><span class="sxs-lookup"><span data-stu-id="82a6f-122">The interface is implemented by a concrete type.</span></span> <span data-ttu-id="82a6f-123">Örnek uygulamada `CharacterRepository` veritabanı bağlamı yönetir ve uygulayan `ListAll` ve `Add` yöntemleri:</span><span class="sxs-lookup"><span data-stu-id="82a6f-123">In the sample app, `CharacterRepository` manages the database context and implements the `ListAll` and `Add` methods:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Models/CharacterRepository.cs?name=snippet1)]

::: moniker-end

## <a name="register-the-repository-service"></a><span data-ttu-id="82a6f-124">Depo Hizmeti kaydedin</span><span class="sxs-lookup"><span data-stu-id="82a6f-124">Register the repository service</span></span>

<span data-ttu-id="82a6f-125">Havuz ve veritabanı bağlamı service kapsayıcısında ile kayıtlı `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="82a6f-125">The repository and database context are registered with the service container in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="82a6f-126">Örnek uygulamada `ApplicationDbContext` uzantı yöntemine yapılan çağrı ile yapılandırılmış [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span><span class="sxs-lookup"><span data-stu-id="82a6f-126">In the sample app, `ApplicationDbContext` is configured with the call to the extension method [AddDbContext](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext).</span></span> <span data-ttu-id="82a6f-127">`ICharacterRepository` kapsamlı bir hizmet olarak kayıtlı:</span><span class="sxs-lookup"><span data-stu-id="82a6f-127">`ICharacterRepository` is registered as a scoped service:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,18)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Startup.cs?name=snippet1&highlight=4-6,12)]

::: moniker-end

## <a name="inject-an-instance-of-the-repository"></a><span data-ttu-id="82a6f-128">Depo bir örneğini ekleme</span><span class="sxs-lookup"><span data-stu-id="82a6f-128">Inject an instance of the repository</span></span>

<span data-ttu-id="82a6f-129">Veritabanı erişimi gerekli olduğu bir sınıfta bir örnek depo yapıcı istenen ve sınıfı yöntemleri tarafından kullanmak için özel bir alan atandı.</span><span class="sxs-lookup"><span data-stu-id="82a6f-129">In a class where database access is required, an instance of the repository is requested via the constructor and assigned to a private field for use by class methods.</span></span> <span data-ttu-id="82a6f-130">Örnek uygulamada `ICharacterRepository` için kullanılır:</span><span class="sxs-lookup"><span data-stu-id="82a6f-130">In the sample app, `ICharacterRepository` is used to:</span></span>

* <span data-ttu-id="82a6f-131">Hiçbir karakteri yoksa veritabanını doldurmak.</span><span class="sxs-lookup"><span data-stu-id="82a6f-131">Populate the database if no characters exist.</span></span>
* <span data-ttu-id="82a6f-132">Görüntü karakterlerin listesini edinin.</span><span class="sxs-lookup"><span data-stu-id="82a6f-132">Obtain a list of the characters for display.</span></span>

<span data-ttu-id="82a6f-133">Çağıran kod arabirimin uygulamasıyla yalnızca nasıl etkileştiğini fark `CharacterRepository`.</span><span class="sxs-lookup"><span data-stu-id="82a6f-133">Notice how the calling code only interacts with the interface's implementation, `CharacterRepository`.</span></span> <span data-ttu-id="82a6f-134">Kodu çağırma kullanmaz `ApplicationDbContext` doğrudan:</span><span class="sxs-lookup"><span data-stu-id="82a6f-134">Calling code doesn't use the `ApplicationDbContext` directly:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](repository-pattern/samples/2.x/RepositoryPatternSample/Pages/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](repository-pattern/samples/1.x/RepositoryPatternSample/Controllers/HomeController.cs?name=snippet1)]

::: moniker-end

## <a name="generic-repository-interface"></a><span data-ttu-id="82a6f-135">Genel depo arabirimi</span><span class="sxs-lookup"><span data-stu-id="82a6f-135">Generic repository interface</span></span>

<span data-ttu-id="82a6f-136">Bu konu ve örnek uygulama, bir depo için her bir iş nesnesi oluşturulduğu, depo düzeninin basit uygulaması gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="82a6f-136">This topic and its sample app demonstrate the simplest implementation of the repository pattern, where one repository is created for each business object.</span></span> <span data-ttu-id="82a6f-137">Uygulamayı birkaç nesneleri artarsa bir *genel depo arabirimi* depo deseni uygulamak için gereken kod miktarını düşürebilir.</span><span class="sxs-lookup"><span data-stu-id="82a6f-137">If the app grows beyond a few objects, a *generic repository interface* may reduce the amount of code required to implement the repository pattern.</span></span> <span data-ttu-id="82a6f-138">Daha fazla bilgi için [DevIQ: depo deseni: Genel depo arabirimi](http://deviq.com/repository-pattern/).</span><span class="sxs-lookup"><span data-stu-id="82a6f-138">For more information, see [DevIQ: Repository Pattern: Generic Repository Interface](http://deviq.com/repository-pattern/).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="82a6f-139">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="82a6f-139">Additional resources</span></span>

* [<span data-ttu-id="82a6f-140">DevIQ: Depo deseni</span><span class="sxs-lookup"><span data-stu-id="82a6f-140">DevIQ: Repository Pattern</span></span>](https://deviq.com/repository-pattern/)
* [<span data-ttu-id="82a6f-141">Bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="82a6f-141">Dependency injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="82a6f-142">Görünümlere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="82a6f-142">Dependency injection into views</span></span>](xref:mvc/views/dependency-injection)
* [<span data-ttu-id="82a6f-143">Denetleyicilere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="82a6f-143">Dependency injection into controllers</span></span>](xref:mvc/controllers/dependency-injection)
* [<span data-ttu-id="82a6f-144">Gereksinim işleyicilerine bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="82a6f-144">Dependency injection in requirement handlers</span></span>](xref:security/authorization/dependencyinjection)
* [<span data-ttu-id="82a6f-145">Tersine çevirme denetimi kapsayıcıları ve bağımlılık ekleme deseni</span><span class="sxs-lookup"><span data-stu-id="82a6f-145">Inversion of Control Containers and the Dependency Injection Pattern</span></span>](https://www.martinfowler.com/articles/injection.html)
