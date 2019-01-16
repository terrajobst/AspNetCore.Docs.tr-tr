---
uid: web-api/overview/advanced/dependency-injection
title: ASP.NET Web API 2'de bağımlılık ekleme | Microsoft Docs
author: MikeWasson
description: Bu öğreticide, ASP.NET Web API denetleyicinizde bağımlılıkları ekleme işlemi gösterilmektedir. Eğitmen Web API 2 Unity uygulama bloğunda kullanılan yazılım sürümleri...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 318a2f1c587feb360212a390bb5de7bdc127513d
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341832"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="8ab81-104">ASP.NET Web API 2'de bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="8ab81-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="8ab81-105">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8ab81-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8ab81-106">Projeyi yükle</span><span class="sxs-lookup"><span data-stu-id="8ab81-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="8ab81-107">Bu öğreticide, ASP.NET Web API denetleyicinizde bağımlılıkları ekleme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8ab81-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8ab81-108">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="8ab81-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8ab81-109">Web API 2</span><span class="sxs-lookup"><span data-stu-id="8ab81-109">Web API 2</span></span>
> - [<span data-ttu-id="8ab81-110">Unity uygulama bloğu</span><span class="sxs-lookup"><span data-stu-id="8ab81-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="8ab81-111">Entity Framework 6 (sürüm 5 de kullanılabilir)</span><span class="sxs-lookup"><span data-stu-id="8ab81-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="8ab81-112">Bağımlılık ekleme nedir?</span><span class="sxs-lookup"><span data-stu-id="8ab81-112">What is Dependency Injection?</span></span>

<span data-ttu-id="8ab81-113">A *bağımlılık* olan başka bir nesneye gerektiren herhangi bir nesne.</span><span class="sxs-lookup"><span data-stu-id="8ab81-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="8ab81-114">Örneğin, tanımlamak için ortak bir [depo](http://martinfowler.com/eaaCatalog/repository.html) işleyen veri erişimi.</span><span class="sxs-lookup"><span data-stu-id="8ab81-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="8ab81-115">Şimdi içeren bir örnek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8ab81-115">Let's illustrate with an example.</span></span> <span data-ttu-id="8ab81-116">İlk olarak, bir etki alanı modeli tanımlarız:</span><span class="sxs-lookup"><span data-stu-id="8ab81-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="8ab81-117">Öğeleri Entity Framework kullanarak bir veritabanında depolar. bir basit bir depo sınıfına aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8ab81-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="8ab81-118">Artık GET isteklerini destekleyen bir Web API denetleyicisi tanımlayalım `Product` varlıklar.</span><span class="sxs-lookup"><span data-stu-id="8ab81-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="8ab81-119">(Ben POST ve kolaylık sağlaması için diğer yöntemleri bırakarak.) İlk denemesi şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="8ab81-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="8ab81-120">Denetleyici sınıfı bağımlı bildirimi `ProductRepository`, ve biz denetleyici oluşturma izni vermiş olursunuz `ProductRepository` örneği.</span><span class="sxs-lookup"><span data-stu-id="8ab81-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="8ab81-121">Ancak, çeşitli nedenlerle sabit koda bağımlılık bu şekilde, hatalı bir fikir olabilir.</span><span class="sxs-lookup"><span data-stu-id="8ab81-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="8ab81-122">Değiştirmek istiyorsanız `ProductRepository` farklı bir uygulama ile aynı zamanda denetleyici sınıfı değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="8ab81-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="8ab81-123">Varsa `ProductRepository` bağımlılıklara sahiptir, bu denetleyici içinde yapılandırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8ab81-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="8ab81-124">İçin büyük bir projenin birden fazla denetleyicileriyle yapılandırma kodunuzu projenizi arasında dağılmış olur.</span><span class="sxs-lookup"><span data-stu-id="8ab81-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="8ab81-125">Denetleyici veritabanı sorgulamak için sabit kodlanmış olduğundan birim testine zordur.</span><span class="sxs-lookup"><span data-stu-id="8ab81-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="8ab81-126">Birim testi için currect tasarım ile mümkün olmayan bir saplama sahte veya depo kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8ab81-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="8ab81-127">Bu sorunları ele *ekleme* denetleyici depoya.</span><span class="sxs-lookup"><span data-stu-id="8ab81-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="8ab81-128">İlk olarak, yeniden düzenleme `ProductRepository` bir arabirim sınıfına:</span><span class="sxs-lookup"><span data-stu-id="8ab81-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="8ab81-129">Ardından sağlayın `IProductRepository` Oluşturucu parametresi olarak:</span><span class="sxs-lookup"><span data-stu-id="8ab81-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="8ab81-130">Bu örnekte [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="8ab81-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="8ab81-131">Ayrıca *ayarlayıcı ekleme*, ayarlayıcı yöntemi veya özelliği aracılığıyla bağımlılık ayarladığınız yerdir.</span><span class="sxs-lookup"><span data-stu-id="8ab81-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="8ab81-132">Ancak artık bir sorunu, uygulamanızı denetleyici doğrudan çünkü.</span><span class="sxs-lookup"><span data-stu-id="8ab81-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="8ab81-133">Web API denetleyici oluşturur isteği yönlendirir ve Web API'si değil bilmeniz herhangi bir şey hakkında `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="8ab81-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="8ab81-134">Bu, Web API'si bağımlılık çözümleyiciyi nerede devreye girer.</span><span class="sxs-lookup"><span data-stu-id="8ab81-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="8ab81-135">Web API bağımlılık çözümleyici</span><span class="sxs-lookup"><span data-stu-id="8ab81-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="8ab81-136">Web API tanımlar **Idependencyresolver** bağımlılıkları çözümlemek için arabirim.</span><span class="sxs-lookup"><span data-stu-id="8ab81-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="8ab81-137">Arabirim tanımı aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="8ab81-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="8ab81-138">**IDependencyScope** arabirimi iki yöntem vardır:</span><span class="sxs-lookup"><span data-stu-id="8ab81-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="8ab81-139">**GetService** bir türün bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8ab81-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="8ab81-140">**GetServices** belirtilen bir türün nesnelerinin bir koleksiyonunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8ab81-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="8ab81-141">**Idependencyresolver** yöntemi devralan **IDependencyScope** ve ekler **BeginScope** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8ab81-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="8ab81-142">Kapsamlar hakkında bu öğreticinin sonraki bölümlerinde ele alacağız.</span><span class="sxs-lookup"><span data-stu-id="8ab81-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="8ab81-143">Web API denetleyici örneği oluşturduğunda, ilk çağrı **IDependencyResolver.GetService**, geçen denetleyici türü.</span><span class="sxs-lookup"><span data-stu-id="8ab81-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="8ab81-144">Bu genişletilebilirlik kanca tüm bağımlılıkları çözümleniyor denetleyicisi oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8ab81-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="8ab81-145">Varsa **GetService** null döndürür, Web API denetleyici sınıfı üzerinde parametresiz bir oluşturucu arar.</span><span class="sxs-lookup"><span data-stu-id="8ab81-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="8ab81-146">Unity kapsayıcısı ile bağımlılık çözümlemesi</span><span class="sxs-lookup"><span data-stu-id="8ab81-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="8ab81-147">Eksiksiz bir yazabilirsiniz rağmen **Idependencyresolver** sıfırdan arabirimi uygulaması Web API'si ve mevcut IOC kapsayıcılar arasında köprü olarak davranmak üzere gerçekten tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="8ab81-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="8ab81-148">IOC kapsayıcı bağımlılıkları yönetmekten sorumlu olan bir yazılım bileşenidir.</span><span class="sxs-lookup"><span data-stu-id="8ab81-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="8ab81-149">Türleri ile kapsayıcı kayıt ve sonra da kapsayıcı nesneleri oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="8ab81-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="8ab81-150">Kapsayıcı bağımlılık ilişkileri otomatik olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="8ab81-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="8ab81-151">Çok sayıda IOC kapsayıcı nesne yaşam süresi ve kapsamı gibi denetlemenize izin.</span><span class="sxs-lookup"><span data-stu-id="8ab81-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="8ab81-152">"IoC" anlamına gelir "tersine çevirme denetimi için", burada bir çerçeve koduna çağrı genel düzen olduğu.</span><span class="sxs-lookup"><span data-stu-id="8ab81-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="8ab81-153">IOC kapsayıcı nesnelerinizi sizin için "Normal denetim akışını tersine çevirir" oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8ab81-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="8ab81-154">Bu öğreticide, kullanacağız [Unity](https://msdn.microsoft.com/library/ff647202.aspx) Microsoft Patterns gelen &amp; yöntemler.</span><span class="sxs-lookup"><span data-stu-id="8ab81-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="8ab81-155">(Diğer popüler kitaplıkları içerir [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), ve [StructureMap ](http://structuremap.github.io/documentation/).) Instalovat Unity için NuGet Paket Yöneticisi'ni kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8ab81-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://structuremap.github.io/documentation/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="8ab81-156">Gelen **Araçları** Visual Studio'da seçim menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="8ab81-156">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="8ab81-157">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="8ab81-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="8ab81-158">Uygulanışı işte **Idependencyresolver** Unity kapsayıcı sonuna geldik.</span><span class="sxs-lookup"><span data-stu-id="8ab81-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="8ab81-159">Varsa **GetService** yöntemi bir türünü çözümleyemiyor, döndürme zorunluluğu **null**.</span><span class="sxs-lookup"><span data-stu-id="8ab81-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="8ab81-160">Varsa **GetServices** yöntemi, bir tür çözümleyemiyor, boş bir koleksiyon nesnesi döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="8ab81-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="8ab81-161">Bilinmeyen türleri için özel durumlar yok.</span><span class="sxs-lookup"><span data-stu-id="8ab81-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="8ab81-162">Bağımlılık çözümleyiciyi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="8ab81-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="8ab81-163">Bağımlılık çözümleyiciyi ayarlamak **DependencyResolver** genel özellik **HttpConfiguration** nesne.</span><span class="sxs-lookup"><span data-stu-id="8ab81-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="8ab81-164">Aşağıdaki kod kayıtları `IProductRepository` Unity ile arabirim ve ardından oluşturan bir `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="8ab81-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="8ab81-165">Bağımlılık kapsamı ve denetleyici ömrü</span><span class="sxs-lookup"><span data-stu-id="8ab81-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="8ab81-166">Denetleyicileri, istek başına oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8ab81-166">Controllers are created per request.</span></span> <span data-ttu-id="8ab81-167">Nesne ömrü yönetmek için **Idependencyresolver** kavramını kullanır bir *kapsam*.</span><span class="sxs-lookup"><span data-stu-id="8ab81-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="8ab81-168">Bağımlılık çözümleyiciyi bağlı **HttpConfiguration** nesnenin genel kapsam vardır.</span><span class="sxs-lookup"><span data-stu-id="8ab81-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="8ab81-169">Web API denetleyici oluşturduğunda, çağrı **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="8ab81-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="8ab81-170">Bu yöntem döndürür bir **IDependencyScope** temsil eden bir alt kapsamı.</span><span class="sxs-lookup"><span data-stu-id="8ab81-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="8ab81-171">Ardından Web API çağrılarının **GetService** denetleyicisi oluşturmak için alt kapsamda.</span><span class="sxs-lookup"><span data-stu-id="8ab81-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="8ab81-172">İstek tamamlandıktan sonra Web API çağrılarının **Dispose** alt kapsamda.</span><span class="sxs-lookup"><span data-stu-id="8ab81-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="8ab81-173">Kullanım **Dispose** denetleyicinin bağımlılıklarını atmayı yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8ab81-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="8ab81-174">Nasıl uygulayacağınıza **BeginScope** IOC kapsayıcıdaki bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="8ab81-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="8ab81-175">Unity için kapsam için bir alt kapsayıcı karşılık gelmektedir:</span><span class="sxs-lookup"><span data-stu-id="8ab81-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="8ab81-176">Çoğu IOC kapsayıcıları benzer eşdeğerleri vardır.</span><span class="sxs-lookup"><span data-stu-id="8ab81-176">Most IoC containers have similar equivalents.</span></span>
