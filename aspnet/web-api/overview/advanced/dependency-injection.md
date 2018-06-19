---
uid: web-api/overview/advanced/dependency-injection
title: ASP.NET Web API 2 bağımlılık ekleme | Microsoft Docs
author: MikeWasson
description: Bu öğreticide, ASP.NET Web API denetleyicinizde bağımlılıkları eklemesine gösterilmiştir. Eğitmen Web API 2 Unity uygulama bloğunda kullanılan yazılım sürümleri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036521"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="6ce9d-104">ASP.NET Web API 2 bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="6ce9d-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="6ce9d-105">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6ce9d-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="6ce9d-106">Tamamlanan projenizi indirin</span><span class="sxs-lookup"><span data-stu-id="6ce9d-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="6ce9d-107">Bu öğreticide, ASP.NET Web API denetleyicinizde bağımlılıkları eklemesine gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6ce9d-108">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="6ce9d-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6ce9d-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="6ce9d-109">Web API 2</span></span>
> - [<span data-ttu-id="6ce9d-110">Unity uygulama bloğu</span><span class="sxs-lookup"><span data-stu-id="6ce9d-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="6ce9d-111">Entity Framework 6 (sürüm 5 de çalışır)</span><span class="sxs-lookup"><span data-stu-id="6ce9d-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="6ce9d-112">Bağımlılık ekleme nedir?</span><span class="sxs-lookup"><span data-stu-id="6ce9d-112">What is Dependency Injection?</span></span>

<span data-ttu-id="6ce9d-113">A *bağımlılık* , başka bir nesnenin gerektiren herhangi bir nesne.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="6ce9d-114">Örneğin, tanımlamak için ortak bir [depo](http://martinfowler.com/eaaCatalog/repository.html) veri erişimi işler.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="6ce9d-115">Şimdi ile bir örnek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-115">Let's illustrate with an example.</span></span> <span data-ttu-id="6ce9d-116">Öncelikle, biz etki alanı modeli tanımlamanız:</span><span class="sxs-lookup"><span data-stu-id="6ce9d-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="6ce9d-117">Öğeleri Entity Framework kullanarak bir veritabanında depolayan bir basit havuz sınıf İşte.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="6ce9d-118">Şimdi şimdi GET isteklerini destekleyen bir Web API denetleyicisi tanımlamak `Product` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="6ce9d-119">(T POST ve Basitlik için diğer yöntemleri bırakmak.) İlk denemesi şöyledir:</span><span class="sxs-lookup"><span data-stu-id="6ce9d-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="6ce9d-120">Denetleyici sınıfı bağlıdır bildirimi `ProductRepository`, ve biz oluşturmak denetleyicisi izin vererek `ProductRepository` örneği.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="6ce9d-121">Ancak, çeşitli nedenlerle sabit koda bu şekilde bağımlılık hatalı bir fikir olabilir.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="6ce9d-122">Değiştirmek istiyorsanız `ProductRepository` farklı bir uygulama ile aynı zamanda denetleyici sınıfı değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="6ce9d-123">Varsa `ProductRepository` bağımlılıklara sahiptir, bu denetleyici içinde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="6ce9d-124">Birden çok denetleyicileriyle büyük projesi için yapılandırma kodunuzu projeniz dağılmış haline gelir.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="6ce9d-125">Denetleyici sorgu veritabanı sabit kodlanmış olduğundan birim testi için zordur.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="6ce9d-126">Birim testi için currect tasarım ile mümkün olmayan bir mock veya saplama deposu kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="6ce9d-127">Bu sorunları ele alabileceği *injecting* denetleyicisi depoya.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="6ce9d-128">İlk olarak, yeniden düzenlemeniz `ProductRepository` bir arabirim sınıfına:</span><span class="sxs-lookup"><span data-stu-id="6ce9d-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="6ce9d-129">Ardından sağlayın `IProductRepository` Oluşturucusu parametre olarak:</span><span class="sxs-lookup"><span data-stu-id="6ce9d-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="6ce9d-130">Bu örnekte [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="6ce9d-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="6ce9d-131">Aynı zamanda *ayarlayıcı ekleme*, bağımlılık ayarlayıcı yöntemi veya özelliği aracılığıyla ayarladığınız.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="6ce9d-132">Ancak artık bir sorun olduğundan, uygulamanızın denetleyicisi doğrudan oluşturmaz olduğundan.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="6ce9d-133">Web API denetleyici istek yönlendirir ve Web API değil bildiğiniz bir şey hakkında oluşturur `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="6ce9d-134">Web API bağımlılık çözümleyiciyi nereden geldiğini budur.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="6ce9d-135">Web API bağımlılık Çözümleyicisi</span><span class="sxs-lookup"><span data-stu-id="6ce9d-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="6ce9d-136">Web API tanımlar **Idependencyresolver** bağımlılıkları çözümleniyor için arabirim.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="6ce9d-137">Arabirim tanımı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="6ce9d-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="6ce9d-138">**IDependencyScope** arabirimi iki yöntem vardır:</span><span class="sxs-lookup"><span data-stu-id="6ce9d-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="6ce9d-139">**GetService** bir türünün bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="6ce9d-140">**GetServices** belirtilen türdeki nesneleri koleksiyonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="6ce9d-141">**Idependencyresolver** yöntemi devralır **IDependencyScope** ve ekler **BeginScope** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="6ce9d-142">Daha sonra Bu öğreticide kapsamları hakkında konuşun.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="6ce9d-143">Web API denetleyici örneği oluşturduğunda, önce çağırır **IDependencyResolver.GetService**, geçen denetleyici türü.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="6ce9d-144">Bu genişletilebilirlik kanca tüm bağımlılıkları çözümleniyor denetleyicisi oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="6ce9d-145">Varsa **GetService** null değerini döndürür, Web API denetleyicisi sınıfı parametresiz bir kurucusu arar.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="6ce9d-146">Unity kapsayıcısı ile bağımlılık çözümlemesi</span><span class="sxs-lookup"><span data-stu-id="6ce9d-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="6ce9d-147">Bir tam yazabilirsiniz rağmen **Idependencyresolver** sıfırdan arabirimi uygulaması Web API ve varolan IOC kapsayıcıları arasında köprü olarak davranmak üzere tasarlanmış gerçekten.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="6ce9d-148">Bağımlılıklar yönetmekten sorumlu bir yazılım bileşeni bir IOC kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="6ce9d-149">Kapsayıcıyla türleri kaydetmek ve nesneleri oluşturmak için kapsayıcı kullanın.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="6ce9d-150">Kapsayıcı otomatik olarak bağımlılık ilişkileri rakamlar.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="6ce9d-151">Birçok IOC kapsayıcıları nesne ömrü ve kapsam gibi denetlemenize izin.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="6ce9d-152">"IoC" anlamına gelir "tersine çevirme denetimi için", olduğu genel bir desen olduğu bir çerçeve uygulama kodu çağırır.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="6ce9d-153">IOC kapsayıcı nesnelerinizi sizin için hangi "Normal akış denetimi tersine çevirir" oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="6ce9d-154">Bu öğretici için kullanacağız [Unity](https://msdn.microsoft.com/library/ff647202.aspx) Microsoft Patterns gelen &amp; yöntemler.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="6ce9d-155">(Diğer popüler kitaplıkları dahil [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), ve [StructureMap ](http://docs.structuremap.net/).) Unity yüklemek için NuGet Paket Yöneticisi'ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="6ce9d-156">Gelen **Araçları** menü Visual Studio'da seçin **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="6ce9d-157">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="6ce9d-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="6ce9d-158">Uygulaması işte **Idependencyresolver** Unity kapsayıcı sarmalar.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="6ce9d-159">Varsa **GetService** yöntemi bir türü çözmek olamaz, döndürme zorunluluğu **null**.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="6ce9d-160">Varsa **GetServices** yöntemi bir türü çözmek olamaz, boş koleksiyon nesneyi döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="6ce9d-161">Bilinmeyen türleri için özel durumlar durum yok.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="6ce9d-162">Bağımlılık çözümleyiciyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6ce9d-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="6ce9d-163">Bağımlılık çözümleyiciyi Ayarla **DependencyResolver** genel özelliğinin **HttpConfiguration** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="6ce9d-164">Aşağıdaki kod yazmaçlar `IProductRepository` arabirim ile Unity ve ardından oluşturan bir `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="6ce9d-165">Bağımlılık kapsamı ve denetleyici yaşam süresi</span><span class="sxs-lookup"><span data-stu-id="6ce9d-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="6ce9d-166">Denetleyicileri istek başına oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-166">Controllers are created per request.</span></span> <span data-ttu-id="6ce9d-167">Nesne yaşam süresi, yönetmek için **Idependencyresolver** kavramını kullanan bir *kapsam*.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="6ce9d-168">Bağımlılık çözümleyiciyi bağlı **HttpConfiguration** nesne genel kapsama sahip.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="6ce9d-169">Web API bir denetleyici oluşturduğunda, çağıran **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="6ce9d-170">Bu yöntem bir **IDependencyScope** temsil eden bir alt kapsamı.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="6ce9d-171">Daha sonra Web API çağrılarının **GetService** denetleyicisi oluşturmak için alt kapsamında.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="6ce9d-172">İstek tamamlandıktan sonra Web API çağrılarının **Dispose** alt kapsamında.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="6ce9d-173">Kullanım **Dispose** denetleyicinin bağımlılıklarını dispose yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="6ce9d-174">Nasıl uygulayacağınıza **BeginScope** IOC kapsayıcısında bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="6ce9d-175">Unity için kapsam için bir alt kapsayıcıdaki karşılık gelir:</span><span class="sxs-lookup"><span data-stu-id="6ce9d-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="6ce9d-176">Çoğu IOC kapsayıcıları benzer eşdeğerleri.</span><span class="sxs-lookup"><span data-stu-id="6ce9d-176">Most IoC containers have similar equivalents.</span></span>
