---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "ASP.NET MVC 4 bağımlılık ekleme | Microsoft Docs"
author: rick-anderson
description: "Not: Bu uygulamalı Laboratuvar ASP.NET MVC ve ASP.NET MVC 4 filtreleri temel bilgiye sahip olduğunu varsayar. ASP.NET MVC 4 filtrelerden önce biz rec kullanmadıysanız..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 48a7d7fdb670aebb72450fc4eb12a364ef595c53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="27af7-104">ASP.NET MVC 4 bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="27af7-104">ASP.NET MVC 4 Dependency Injection</span></span>
====================
<span data-ttu-id="27af7-105">tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="27af7-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> [!NOTE]
> <span data-ttu-id="27af7-106">Bu uygulamalı Laboratuvar temel bilgiye sahip varsayar **ASP.NET MVC** ve **ASP.NET MVC 4 filtreler**.</span><span class="sxs-lookup"><span data-stu-id="27af7-106">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="27af7-107">Değil kullandıysanız **ASP.NET MVC 4 filtreler** gitmenizi öneririz önce **ASP.NET MVC özel eylem filtrelerini** uygulamalı Laboratuvar.</span><span class="sxs-lookup"><span data-stu-id="27af7-107">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>
> 
> <span data-ttu-id="27af7-108">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="27af7-108">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<span data-ttu-id="27af7-109">İçinde **nesne odaklı programlama** standardı, nesneleri çalışma birlikte işbirliği modelinde Katkıda Bulunanlar ve tüketicilerin olduğu.</span><span class="sxs-lookup"><span data-stu-id="27af7-109">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="27af7-110">Doğal olarak, bu iletişim modelini nesneleri ve karmaşıklığını artırır yönetmek zor hale bileşenler arasındaki bağımlılıkları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27af7-110">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="27af7-111">![Sınıf bağımlılıkları ve model karmaşıklık](aspnet-mvc-4-dependency-injection/_static/image1.png "sınıf bağımlılıkları ve model karmaşıklık")</span><span class="sxs-lookup"><span data-stu-id="27af7-111">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="27af7-112">*Sınıf bağımlılıklar ve model karmaşıklık*</span><span class="sxs-lookup"><span data-stu-id="27af7-112">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="27af7-113">Büyük olasılıkla hakkında duymuş **Fabrika düzeni** ve arabirim ve Hizmetleri, istemci nesneleri genellikle hizmet konumu için sorumlu olduğu kullanarak uygulama arasında ayrım.</span><span class="sxs-lookup"><span data-stu-id="27af7-113">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="27af7-114">Bağımlılık ekleme düzeni ters çevirmeyi denetiminin belirli bir uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="27af7-114">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="27af7-115">**Tersine çevirme (IOC) denetiminin** nesneler üzerinde bunlar kullanan işlerini yapmak için diğer nesneleri oluşturmayın anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="27af7-115">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="27af7-116">Bunun yerine, bir dış kaynaktan (örneğin, bir xml yapılandırma dosyası) ihtiyaç duydukları nesneleri alın.</span><span class="sxs-lookup"><span data-stu-id="27af7-116">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="27af7-117">**Bağımlılık ekleme (dı)** bu nesne müdahalesi olmadan genellikle Oluşturucusu parametrelerini geçirir framework bileşeni tarafından yapılır ve özelliklerini ayarlama anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="27af7-117">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="27af7-118">Bağımlılık ekleme (dı) tasarım deseni</span><span class="sxs-lookup"><span data-stu-id="27af7-118">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="27af7-119">Yüksek bir düzeyde bağımlılık ekleme amacı olan istemci sınıfı (örneğin *golfçü*) bir arabirim karşılayan bir şey gerekir (örneğin *IClub*).</span><span class="sxs-lookup"><span data-stu-id="27af7-119">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="27af7-120">Somut türde nedir önemli değildir (örneğin *WoodClub, IronClub, WedgeClub* veya *PutterClub*), işleyen başka birinin istediği (iyi bir örneğin *caddy*).</span><span class="sxs-lookup"><span data-stu-id="27af7-120">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="27af7-121">ASP.NET mvc'de bağımlılık çözümleyiciyi bağımlılık mantığınızı başka bir yere kaydetmenizi izin verebilir (örn. bir kapsayıcı veya bir *Sinek paketi*).</span><span class="sxs-lookup"><span data-stu-id="27af7-121">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="27af7-122">![Bağımlılık ekleme işlemi diyagramı](aspnet-mvc-4-dependency-injection/_static/image2.png "bağımlılık ekleme çizim")</span><span class="sxs-lookup"><span data-stu-id="27af7-122">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="27af7-123">*Bağımlılık ekleme - Golf benzerleme*</span><span class="sxs-lookup"><span data-stu-id="27af7-123">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="27af7-124">Bağımlılık ekleme düzeni ve denetim ters çevirmeyi kullanmanın avantajları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="27af7-124">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="27af7-125">Sınıf azaltır</span><span class="sxs-lookup"><span data-stu-id="27af7-125">Reduces class coupling</span></span>
- <span data-ttu-id="27af7-126">Kodu yeniden kullanma artırır</span><span class="sxs-lookup"><span data-stu-id="27af7-126">Increases code reusing</span></span>
- <span data-ttu-id="27af7-127">Kod bakımı artırır</span><span class="sxs-lookup"><span data-stu-id="27af7-127">Improves code maintainability</span></span>
- <span data-ttu-id="27af7-128">Uygulamayı test artırır</span><span class="sxs-lookup"><span data-stu-id="27af7-128">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="27af7-129">Bağımlılık ekleme bazen soyut Fabrika tasarım deseni ile karşılaştırıldığında, ancak her iki yaklaşımın arasında küçük bir fark yoktur.</span><span class="sxs-lookup"><span data-stu-id="27af7-129">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="27af7-130">DI oluşturucular ve kaydedilen Hizmetleri çağırarak bağımlılıkları çözmek için arkasında çalışma bir çerçeve vardır.</span><span class="sxs-lookup"><span data-stu-id="27af7-130">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="27af7-131">Bağımlılık ekleme düzeni anladığınıza göre ASP.NET MVC 4'te uygulama bu Laboratuvar öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-131">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="27af7-132">Bağımlılık ekleme kullanılarak başlayacak **denetleyicileri** Veritabanı Erişim hizmeti eklenecek.</span><span class="sxs-lookup"><span data-stu-id="27af7-132">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="27af7-133">Ardından, bağımlılık ekleme için geçerli olacaktır **görünümleri** bir hizmeti kullanmak ve bilgileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="27af7-133">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="27af7-134">Son olarak, çözümü özel eylem filtresinde injecting ASP.NET MVC 4 filtreleri için dı uzatır.</span><span class="sxs-lookup"><span data-stu-id="27af7-134">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="27af7-135">Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="27af7-135">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="27af7-136">ASP.NET MVC 4 ile Unity NuGet paketlerini kullanarak bağımlılık ekleme için tümleştirme</span><span class="sxs-lookup"><span data-stu-id="27af7-136">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="27af7-137">ASP.NET MVC denetleyicisinin içinde bağımlılık ekleme kullanın</span><span class="sxs-lookup"><span data-stu-id="27af7-137">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="27af7-138">ASP.NET MVC görünümü içinde bağımlılık ekleme kullanın</span><span class="sxs-lookup"><span data-stu-id="27af7-138">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="27af7-139">Bir ASP.NET MVC eylem filtresi içinde bağımlılık ekleme kullanın</span><span class="sxs-lookup"><span data-stu-id="27af7-139">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="27af7-140">Bu Laboratuvar Unity.Mvc3 NuGet paketi için bağımlılık çözümlemesi kullanıyor, ancak ASP.NET MVC 4 ile çalışmak için hiçbir bağımlılık ekleme Framework uyum mümkündür.</span><span class="sxs-lookup"><span data-stu-id="27af7-140">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="27af7-141">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="27af7-141">Prerequisites</span></span>

<span data-ttu-id="27af7-142">Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="27af7-142">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="27af7-143">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="27af7-143">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="27af7-144">Kurulum</span><span class="sxs-lookup"><span data-stu-id="27af7-144">Setup</span></span>

<span data-ttu-id="27af7-145">**Kod parçacıkları yükleme**</span><span class="sxs-lookup"><span data-stu-id="27af7-145">**Installing Code Snippets**</span></span>

<span data-ttu-id="27af7-146">Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="27af7-146">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="27af7-147">Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="27af7-147">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="27af7-148">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek B: kullanarak kod parçacıkları](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="27af7-148">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="27af7-149">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="27af7-149">Exercises</span></span>

<span data-ttu-id="27af7-150">Bu uygulamalı Laboratuvar tarafından aşağıdaki alıştırmaları oluşmaktadır:</span><span class="sxs-lookup"><span data-stu-id="27af7-150">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="27af7-151">Alıştırma 1: bir denetleyici Injecting</span><span class="sxs-lookup"><span data-stu-id="27af7-151">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="27af7-152">Alıştırma 2: bir görünüm Injecting</span><span class="sxs-lookup"><span data-stu-id="27af7-152">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="27af7-153">Alıştırma 3: Filtreleri Injecting</span><span class="sxs-lookup"><span data-stu-id="27af7-153">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="27af7-154">Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="27af7-154">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="27af7-155">Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-155">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="27af7-156">Bu laboratuvarı tamamlamak için süre tahmini: **30 dakika**.</span><span class="sxs-lookup"><span data-stu-id="27af7-156">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="27af7-157">Alıştırma 1: bir denetleyici Injecting</span><span class="sxs-lookup"><span data-stu-id="27af7-157">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="27af7-158">Bu alıştırmada, bağımlılık ekleme ASP.NET MVC denetleyicileri bir NuGet paketi kullanarak Unity tümleştirerek kullanmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-158">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="27af7-159">Bu nedenle, veri erişim mantığı ayırmak için MvcMusicStore denetleyicilerinizi içine Hizmetleri dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="27af7-159">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="27af7-160">Hizmetleri yardımıyla bağımlılık ekleme kullanılarak çözümlenir denetleyici Oluşturucu, yeni bir bağımlılık oluşturacak **Unity**.</span><span class="sxs-lookup"><span data-stu-id="27af7-160">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="27af7-161">Bu yaklaşım daha esnek ve daha kolay korumak ve test etmek daha az bağlı uygulamaları oluşturmak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="27af7-161">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="27af7-162">Ayrıca ASP.NET MVC, Unity ile tümleştirmek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-162">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="27af7-163">StoreManager hizmeti hakkında</span><span class="sxs-lookup"><span data-stu-id="27af7-163">About StoreManager Service</span></span>

<span data-ttu-id="27af7-164">Adlı depolama denetleyicisi verileri yöneten bir hizmet başlangıç çözümde şimdi sağlanan MVC müzik deposu içerir **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="27af7-164">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="27af7-165">Aşağıda depolama hizmeti uygulaması bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-165">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="27af7-166">Tüm yöntemleri modeli varlıkları döndürme unutmayın.</span><span class="sxs-lookup"><span data-stu-id="27af7-166">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="27af7-167">**StoreController** begin işleminden çözüm şimdi tüketir **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="27af7-167">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="27af7-168">Tüm veri başvuruları sıradan kaldırıldığını **StoreController**ve geçerli veri erişimi sağlayıcı tüketir herhangi bir yöntemini değiştirmeden artık mümkün **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="27af7-168">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="27af7-169">Bunun altında bulabilirsiniz **StoreController** uygulama ile bir bağımlılığa sahip **StoreService** sınıfı oluşturucusu içinde.</span><span class="sxs-lookup"><span data-stu-id="27af7-169">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="27af7-170">Bu alıştırmada sunulan bağımlılık ilgili **ters çevirmeyi denetim** (IOC).</span><span class="sxs-lookup"><span data-stu-id="27af7-170">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="27af7-171">**StoreController** sınıfı oluşturucusu alan bir **IStoreService** sınıfı içinde hizmet çağrılarından gerçekleştirmek için gerekli olan tür parametresi.</span><span class="sxs-lookup"><span data-stu-id="27af7-171">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="27af7-172">Ancak, **StoreController** herhangi bir denetleyicisi ASP.NET MVC ile çalışmak zorunda varsayılan Oluşturucusu (parametresiz) uygulamıyor.</span><span class="sxs-lookup"><span data-stu-id="27af7-172">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="27af7-173">Bağımlılık çözmek için denetleyici soyut fabrikası (belirtilen türdeki herhangi bir nesne döndürür sınıfı) tarafından oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="27af7-173">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="27af7-174">Sınıf bildirilen parametresiz bir kurucusu olduğundan StoreController hizmet nesnesi göndermeden oluşturmaya çalıştığında bir hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="27af7-174">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="27af7-175">Görev 1 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="27af7-175">Task 1 - Running the Application</span></span>

<span data-ttu-id="27af7-176">Bu görevde, veri erişim uygulama mantığından ayıran depolama denetleyicisi içine hizmet içeren başlangıç uygulama çalışır.</span><span class="sxs-lookup"><span data-stu-id="27af7-176">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="27af7-177">Denetleyici Hizmeti varsayılan olarak bir parametre olarak geçirilen değil gibi uygulama çalışırken bir özel durum alırsınız:</span><span class="sxs-lookup"><span data-stu-id="27af7-177">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="27af7-178">Açık **başlamak** çözüm bulunan **Source\Ex01 Injecting Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="27af7-178">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

    1. <span data-ttu-id="27af7-179">Bazı eksik NuGet paketlerini indirmek gerekir devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="27af7-179">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="27af7-180">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="27af7-180">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="27af7-181">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="27af7-181">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="27af7-182">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="27af7-182">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="27af7-183">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="27af7-183">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="27af7-184">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="27af7-184">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="27af7-185">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="27af7-185">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="27af7-186">Tuşuna **Ctrl + F5** hata ayıklama olmadan uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="27af7-186">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="27af7-187">Hata iletisi alırsınız &quot; **bu nesne için tanımlanan parametresiz bir kurucusu**&quot;:</span><span class="sxs-lookup"><span data-stu-id="27af7-187">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="27af7-188">![ASP.NET MVC başlamak uygulaması çalıştırılırken hata](aspnet-mvc-4-dependency-injection/_static/image3.png "başlamak ASP.NET MVC uygulaması çalıştırılırken hata")</span><span class="sxs-lookup"><span data-stu-id="27af7-188">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="27af7-189">*ASP.NET MVC başlamak uygulaması çalıştırılırken hata*</span><span class="sxs-lookup"><span data-stu-id="27af7-189">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="27af7-190">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="27af7-190">Close the browser.</span></span>

<span data-ttu-id="27af7-191">Aşağıdaki adımlarda bu denetleyicisinin gereksinim duyduğu bağımlılık eklemesine müzik deposu çözüm üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="27af7-191">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="27af7-192">Görev 2 - dahil olmak üzere Unity MvcMusicStore çözüm</span><span class="sxs-lookup"><span data-stu-id="27af7-192">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="27af7-193">Bu görevde, içerecektir **Unity.Mvc3** çözüm için NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="27af7-193">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="27af7-194">Unity.Mvc3 paketi ASP.NET MVC 3 için tasarlanmıştır ancak ASP.NET MVC 4 ile tam uyumludur.</span><span class="sxs-lookup"><span data-stu-id="27af7-194">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="27af7-195">Unity bir hafif, Genişletilebilir bağımlılık ekleme kapsayıcısını isteğe bağlı destek ile örneği için olan ve kişiler tarafından ele yazın.</span><span class="sxs-lookup"><span data-stu-id="27af7-195">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="27af7-196">Her tür .NET uygulama kullanmak için genel amaçlı bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="27af7-196">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="27af7-197">İçinde bağımlılık ekleme mekanizmaları dahil olmak üzere bulunan tüm genel özellikleri sağlar: nesne oluşturma, Bileşen Yapılandırması kapsayıcısı ertelemeyi tarafından çalışma zamanı ve esneklik, bağımlılıkları belirterek gereksinimlerinin Özet.</span><span class="sxs-lookup"><span data-stu-id="27af7-197">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="27af7-198">Yükleme **Unity.Mvc3** NuGet paketi **MvcMusicStore** projesi.</span><span class="sxs-lookup"><span data-stu-id="27af7-198">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="27af7-199">Bunu yapmak için açın **Paket Yöneticisi Konsolu** gelen **Görünüm** | **diğer pencereler**.</span><span class="sxs-lookup"><span data-stu-id="27af7-199">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="27af7-200">Şu komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="27af7-200">Run the following command.</span></span>

    <span data-ttu-id="27af7-201">PMC</span><span class="sxs-lookup"><span data-stu-id="27af7-201">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="27af7-202">![Unity.Mvc3 NuGet paketi Yükleniyor](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet paketi yükleniyor")</span><span class="sxs-lookup"><span data-stu-id="27af7-202">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="27af7-203">*Unity.Mvc3 NuGet paketi yükleniyor*</span><span class="sxs-lookup"><span data-stu-id="27af7-203">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="27af7-204">Bir kez **Unity.Mvc3** paketi yüklendiğinde, dosya ve klasörleri otomatik olarak ekler Unity yapılandırmasını basitleştirmek için keşfedin.</span><span class="sxs-lookup"><span data-stu-id="27af7-204">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="27af7-205">![Yüklü Unity.Mvc3 paket](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 paketi yüklü")</span><span class="sxs-lookup"><span data-stu-id="27af7-205">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="27af7-206">*Yüklü Unity.Mvc3 paketi*</span><span class="sxs-lookup"><span data-stu-id="27af7-206">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="27af7-207">Görev 3 - kaydetme Unity Global.asax.cs uygulamada\_Başlat</span><span class="sxs-lookup"><span data-stu-id="27af7-207">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="27af7-208">Bu görevde, güncelleştirecektir **uygulama\_Başlat** yöntemi bulunan **Global.asax.cs** Unity önyükleyici Başlatıcı arayın ve ardından, önyükleyici dosyası kaydediliyor güncelleştirmek için Denetleyici ve hizmet bağımlılık ekleme işlemi için kullanır.</span><span class="sxs-lookup"><span data-stu-id="27af7-208">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="27af7-209">Şimdi, Unity kapsayıcı başlatır dosyası olan önyükleyici ve bağımlılık çözümleyiciyi kanca.</span><span class="sxs-lookup"><span data-stu-id="27af7-209">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="27af7-210">Bunu yapmak için açın **Global.asax.cs** ve içinde aşağıdaki vurgulanmış kodu ekleyin **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="27af7-210">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="27af7-211">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - başlatmak Unity*)</span><span class="sxs-lookup"><span data-stu-id="27af7-211">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="27af7-212">Açık **Bootstrapper.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="27af7-212">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="27af7-213">Şu ad alanlarından içerir: **MvcMusicStore.Services** ve **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="27af7-213">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="27af7-214">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - önyükleyici ad alanlarını ekleme*)</span><span class="sxs-lookup"><span data-stu-id="27af7-214">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="27af7-215">Değiştir **BuildUnityContainer** yöntemi depolama denetleyicisi ve depolama hizmeti kaydeder aşağıdaki kod ile içerik.</span><span class="sxs-lookup"><span data-stu-id="27af7-215">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="27af7-216">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - kayıt depolama denetleyicisi ve hizmet*)</span><span class="sxs-lookup"><span data-stu-id="27af7-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="27af7-217">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="27af7-217">Task 4 - Running the Application</span></span>

<span data-ttu-id="27af7-218">Bu görevde, bunu şimdi Unity dahil olmak üzere sonra yüklenebilir olduğunu doğrulamak için uygulamanın çalıştırılacağı.</span><span class="sxs-lookup"><span data-stu-id="27af7-218">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="27af7-219">Tuşuna **F5** uygulamayı çalıştırmak için uygulama artık herhangi bir hata iletisi göstermeden yüklenecektir.</span><span class="sxs-lookup"><span data-stu-id="27af7-219">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="27af7-220">![Bağımlılık ekleme ile uygulama çalıştıran](aspnet-mvc-4-dependency-injection/_static/image6.png "uygulama bağımlılık ekleme ile çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="27af7-220">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="27af7-221">*Bağımlılık ekleme ile çalışan uygulama*</span><span class="sxs-lookup"><span data-stu-id="27af7-221">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="27af7-222">Gözat **/deposu**.</span><span class="sxs-lookup"><span data-stu-id="27af7-222">Browse to **/Store**.</span></span> <span data-ttu-id="27af7-223">Bu çağıracağı **StoreController**, şimdi oluşturulduğu kullanarak **Unity**.</span><span class="sxs-lookup"><span data-stu-id="27af7-223">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="27af7-224">![MVC müzik deposu](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC müzik deposu")</span><span class="sxs-lookup"><span data-stu-id="27af7-224">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="27af7-225">*MVC müzik deposu*</span><span class="sxs-lookup"><span data-stu-id="27af7-225">*MVC Music Store*</span></span>
3. <span data-ttu-id="27af7-226">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="27af7-226">Close the browser.</span></span>

<span data-ttu-id="27af7-227">Aşağıdaki alıştırmalarda ASP.NET MVC görünümleri ve eylem filtrelerini içinde kullanılacak bağımlılık ekleme kapsamı genişletmek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-227">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="27af7-228">Alıştırma 2: bir görünüm Injecting</span><span class="sxs-lookup"><span data-stu-id="27af7-228">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="27af7-229">Bu alıştırmada, bağımlılık ekleme ASP.NET MVC 4'ün yeni özelliklerle görünümünde Unity tümleştirme için nasıl kullanılacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-229">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="27af7-230">Bunu yapmak için özel bir hizmet deposu Gözat bir ileti ve bir görüntü gösterecektir görünümü içinde çağırır.</span><span class="sxs-lookup"><span data-stu-id="27af7-230">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="27af7-231">Ardından, projeyi Unity ile tümleştirmek ve bağımlılıkları eklemesine özel bağımlılık çözümleyici oluşturun.</span><span class="sxs-lookup"><span data-stu-id="27af7-231">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="27af7-232">Görev 1 - bir hizmeti kullanan bir görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="27af7-232">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="27af7-233">Bu görevde, yeni bir bağımlılık oluşturmak için bir hizmet çağrı yapan bir görünüm oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27af7-233">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="27af7-234">Bu çözümdeki basit bir Mesajlaşma hizmetinde servis oluşur.</span><span class="sxs-lookup"><span data-stu-id="27af7-234">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="27af7-235">Açık **başlamak** çözüm bulunan **Source\Ex02 Injecting View\Begin** klasör.</span><span class="sxs-lookup"><span data-stu-id="27af7-235">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="27af7-236">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="27af7-236">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="27af7-237">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="27af7-237">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="27af7-238">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="27af7-238">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="27af7-239">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="27af7-239">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="27af7-240">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="27af7-240">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="27af7-241">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="27af7-241">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="27af7-242">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="27af7-242">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="27af7-243">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="27af7-243">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="27af7-244">Bu makalede daha fazla bilgi için bkz: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="27af7-244">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="27af7-245">Dahil **MessageService.cs** ve **IMessageService.cs** sınıfları bulunan **\Assets kaynak** klasöründe **/Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="27af7-245">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="27af7-246">Bunu yapmak için sağ **Hizmetleri** klasörü ve select **varolan öğeyi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="27af7-246">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="27af7-247">Dosya konumuna göz atın ve bunları ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-247">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="27af7-248">![İleti hizmeti ve hizmet arabirimi ekleme](aspnet-mvc-4-dependency-injection/_static/image8.png "ileti hizmeti ve hizmet arabirimi ekleme")</span><span class="sxs-lookup"><span data-stu-id="27af7-248">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="27af7-249">*Ekleme ileti hizmeti ve hizmet arabirimi*</span><span class="sxs-lookup"><span data-stu-id="27af7-249">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="27af7-250">**IMessageService** arabirimi tanımlar tarafından uygulanan iki Özellikler **MessageService** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="27af7-250">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="27af7-251">Bu özellikleri -**ileti** ve **ImageUrl**-ileti ve görüntünün URL'si görüntülenecek depolar.</span><span class="sxs-lookup"><span data-stu-id="27af7-251">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="27af7-252">Bir klasör oluşturun **/sayfaları** projenin kök klasörünü ve ardından mevcut bir sınıf ekleyin **MyBasePage.cs** gelen **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="27af7-252">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="27af7-253">Öğesinden devralacak taban sayfası aşağıdaki yapısına sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="27af7-253">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="27af7-254">![Sayfaları klasöründeki](aspnet-mvc-4-dependency-injection/_static/image9.png "sayfaları klasörü")</span><span class="sxs-lookup"><span data-stu-id="27af7-254">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="27af7-255">Açık **Browse.cshtml** gelen görüntülemek **/görünümler/deposu** klasörünü ve devralınmalıdır hale **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="27af7-255">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="27af7-256">İçinde **Gözat** görüntülemek için bir çağrı ekleyin **MessageService** görüntü ve hizmet tarafından alınan ileti görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="27af7-256">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
<span data-ttu-id="27af7-257">(C#)</span><span class="sxs-lookup"><span data-stu-id="27af7-257">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="27af7-258">Görev 2 - özel bağımlılık çözümleyici ve özel görünüm sayfa Etkinleştiricisini dahil</span><span class="sxs-lookup"><span data-stu-id="27af7-258">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="27af7-259">Önceki görevde bir hizmeti çağrısı içindeki gerçekleştirmek için bir görünüm içinde yeni bir bağımlılık eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="27af7-259">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="27af7-260">ASP.NET MVC bağımlılık ekleme arabirimleri uygulayarak bu bağımlılığı şimdi çözümleyecek **IViewPageActivator** ve **Idependencyresolver**.</span><span class="sxs-lookup"><span data-stu-id="27af7-260">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="27af7-261">Çözümde uygulaması içerecektir **Idependencyresolver** , Dağıt hizmet alma ile Unity kullanarak.</span><span class="sxs-lookup"><span data-stu-id="27af7-261">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="27af7-262">Ardından, başka bir özel uyarlamasını içerecektir **IViewPageActivator** görünümleri oluşturma çözmek için arabirim.</span><span class="sxs-lookup"><span data-stu-id="27af7-262">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="27af7-263">ASP.NET MVC 3 itibaren bağımlılık ekleme uygulamasını Hizmetleri kaydettirmek için arabirimler Basitleştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="27af7-263">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="27af7-264">**Idependencyresolver** ve **IViewPageActivator** bağımlılık ekleme işlemi için ASP.NET MVC 3 özellikleri parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="27af7-264">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="27af7-265">**-Idependencyresolver** arabirimi önceki IMvcServiceLocator yerini alır.</span><span class="sxs-lookup"><span data-stu-id="27af7-265">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="27af7-266">Idependencyresolver Implementers hizmet veya hizmet koleksiyonu örneğini döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="27af7-266">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="27af7-267">**-IViewPageActivator** arabirimi görünüm sayfalarının bağımlılık ekleme nasıl oluşturulur üzerinde daha ayrıntılı denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="27af7-267">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="27af7-268">Uygulayan sınıflar **IViewPageActivator** arabirimi bağlamını kullanarak görünüm örnekleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-268">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="27af7-269">Oluşturma /**oluşturucuları** projenin kök klasöründe.</span><span class="sxs-lookup"><span data-stu-id="27af7-269">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="27af7-270">Dahil **CustomViewPageActivator.cs** çözümünüzden için **/kaynakları/varlıklar/** için **oluşturucuları** klasör.</span><span class="sxs-lookup"><span data-stu-id="27af7-270">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="27af7-271">Bunu yapmak için sağ **/Factories** klasöründe seçin **Ekle | Varolan öğeyi** ve ardından **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="27af7-271">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="27af7-272">Bu sınıf uygulayan **IViewPageActivator** Unity kapsayıcı tutmak için arabirim.</span><span class="sxs-lookup"><span data-stu-id="27af7-272">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="27af7-273">**CustomViewPageActivator** Unity kapsayıcısını kullanarak bir görünüm oluşturma yönetilmesinden sorumludur.</span><span class="sxs-lookup"><span data-stu-id="27af7-273">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="27af7-274">Dahil **UnityDependencyResolver.cs** dosya **/kaynakları/varlıklar** için **/Factories** klasör.</span><span class="sxs-lookup"><span data-stu-id="27af7-274">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="27af7-275">Bunu yapmak için sağ **/Factories** klasöründe seçin **Ekle | Varolan öğeyi** ve ardından **UnityDependencyResolver.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="27af7-275">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="27af7-276">**UnityDependencyResolver** Unity için özel bir DependencyResolver bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="27af7-276">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="27af7-277">Bir hizmet Unity kapsayıcısı içindeki bulunamadığında temel çözümleyici invocated.</span><span class="sxs-lookup"><span data-stu-id="27af7-277">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="27af7-278">Aşağıdaki görev, hizmetleri ve görünümleri yerini modeli izin vermek için her iki uygulamaları kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="27af7-278">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="27af7-279">Görev 3 - Unity kapsayıcıda bağımlılık ekleme işlemi için kaydetme</span><span class="sxs-lookup"><span data-stu-id="27af7-279">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="27af7-280">Bu görevde, birlikte çalışma bağımlılık ekleme yapmak için önceki bir şey sokar.</span><span class="sxs-lookup"><span data-stu-id="27af7-280">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="27af7-281">Şimdiye kadar çözümünüzü aşağıdaki öğeleri içerir:</span><span class="sxs-lookup"><span data-stu-id="27af7-281">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="27af7-282">A **Gözat** devraldığı Görünüm **MyBaseClass** ve **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="27af7-282">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="27af7-283">Bir ara sınıf -**MyBaseClass**-için hizmet arabirimi bildirilen bağımlılık ekleme sahiptir.</span><span class="sxs-lookup"><span data-stu-id="27af7-283">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="27af7-284">Hizmeti - **MessageService** - ve onun arabirim **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="27af7-284">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="27af7-285">Unity için-özel bağımlılık çözümleyiciyi **UnityDependencyResolver** -hizmet alma ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="27af7-285">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="27af7-286">Bir görünüm sayfası Etkinleştirici - **CustomViewPageActivator** -sayfası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="27af7-286">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="27af7-287">Eklemesine **Gözat** görünümü şimdi kaydettiğiniz özel bağımlılık çözümleyiciyi Unity kapsayıcısında.</span><span class="sxs-lookup"><span data-stu-id="27af7-287">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="27af7-288">Açık **Bootstrapper.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="27af7-288">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="27af7-289">Örneğini kaydetmek **MessageService** hizmeti başlatmak için Unity kapsayıcı içine:</span><span class="sxs-lookup"><span data-stu-id="27af7-289">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="27af7-290">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - kayıt iletisi hizmeti*)</span><span class="sxs-lookup"><span data-stu-id="27af7-290">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="27af7-291">Bir başvuru ekleyin **MvcMusicStore.Factories** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="27af7-291">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="27af7-292">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - oluşturucuları Namespace*)</span><span class="sxs-lookup"><span data-stu-id="27af7-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="27af7-293">Kayıt **CustomViewPageActivator** bir görünüm sayfası Etkinleştirici Unity kapsayıcı içine olarak:</span><span class="sxs-lookup"><span data-stu-id="27af7-293">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="27af7-294">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - kayıt CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="27af7-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="27af7-295">ASP.NET MVC 4 varsayılan bağımlılık Çözümleyicisi örneği ile Değiştir **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="27af7-295">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="27af7-296">Bunu yapmak için yerini **Initialise** yöntemini aşağıdaki kodla içerik:</span><span class="sxs-lookup"><span data-stu-id="27af7-296">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="27af7-297">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - güncelleştirme bağımlılık çözümleyiciyi*)</span><span class="sxs-lookup"><span data-stu-id="27af7-297">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="27af7-298">ASP.NET MVC varsayılan bağımlılık çözümleyici sınıfı sağlar.</span><span class="sxs-lookup"><span data-stu-id="27af7-298">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="27af7-299">Unity için oluşturduk biri olarak özel bağımlılık çözümleyiciler ile çalışmak için bu çözümleyici değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="27af7-299">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="27af7-300">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="27af7-300">Task 4 - Running the Application</span></span>

<span data-ttu-id="27af7-301">Bu görevde deposu Tarayıcı hizmeti kullanır ve görüntü ve alınan ileti gösterilir doğrulamak için uygulamanın çalıştırılacağı:</span><span class="sxs-lookup"><span data-stu-id="27af7-301">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="27af7-302">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="27af7-302">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="27af7-303">' I tıklatın **Rock** bakın ve türler menüden içinde nasıl **MessageService** görünümüne eklendi ve Hoş Geldiniz iletisi ve görüntünün yüklendi.</span><span class="sxs-lookup"><span data-stu-id="27af7-303">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="27af7-304">Bu örnekte, biz için giriyorsunuz &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="27af7-304">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="27af7-305">![MVC müzik deposu - görünüm ekleme](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC müzik deposu - görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="27af7-305">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="27af7-306">*MVC müzik deposu - görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="27af7-306">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="27af7-307">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="27af7-307">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="27af7-308">Alıştırma 3: Eylem filtrelerini Injecting</span><span class="sxs-lookup"><span data-stu-id="27af7-308">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="27af7-309">Önceki uygulamalı laboratuarda **özel eylem filtrelerini** filtreleri özelleştirme ve ekleme çalıştı.</span><span class="sxs-lookup"><span data-stu-id="27af7-309">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="27af7-310">Bu alıştırmada, bağımlılık ekleme ile Unity kapsayıcısını kullanarak filtreleri eklemesine öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-310">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="27af7-311">Bunu yapmak için site etkinliğini izleme özel eylemi filtre müzik deposu çözüme ekleyecek.</span><span class="sxs-lookup"><span data-stu-id="27af7-311">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="27af7-312">Görev 1 - çözümde izleme filtresi dahil olmak üzere</span><span class="sxs-lookup"><span data-stu-id="27af7-312">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="27af7-313">Bu görevde, müzik mağazası'nda bir özel eylem filtresi olayları izlemek dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="27af7-313">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="27af7-314">Özel eylem filtre olarak kavramları zaten önceki laboratuar ortamında davranılır &quot;özel eylem filtrelerini&quot;, yalnızca bu Laboratuvar varlıklar klasöründen filtre sınıfının içerir ve Unity için filtre sağlayıcı oluşturma:</span><span class="sxs-lookup"><span data-stu-id="27af7-314">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="27af7-315">Açık **başlamak** çözüm bulunan **Source\Ex03 - Injecting eylem Filter\Begin** klasör.</span><span class="sxs-lookup"><span data-stu-id="27af7-315">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="27af7-316">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="27af7-316">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="27af7-317">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="27af7-317">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="27af7-318">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="27af7-318">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="27af7-319">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="27af7-319">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="27af7-320">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="27af7-320">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="27af7-321">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="27af7-321">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="27af7-322">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="27af7-322">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="27af7-323">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="27af7-323">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="27af7-324">Bu makalede daha fazla bilgi için bkz: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="27af7-324">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="27af7-325">Dahil **TraceActionFilter.cs** dosya **/kaynakları/varlıklar** için **/filtreler** klasör.</span><span class="sxs-lookup"><span data-stu-id="27af7-325">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="27af7-326">Bu özel eylem filtresi ASP.NET izleme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="27af7-326">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="27af7-327">Kontrol edebilirsiniz &quot;ASP.NET MVC 4 yerel ve dinamik eylem filtrelerini&quot; Laboratuvar için daha fazla başvuru.</span><span class="sxs-lookup"><span data-stu-id="27af7-327">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="27af7-328">Boş sınıfı ekleme **FilterProvider.cs** klasörü'nde projeye   **/filtreler.**</span><span class="sxs-lookup"><span data-stu-id="27af7-328">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="27af7-329">Ekleme **System.Web.Mvc** ve **Microsoft.Practices.Unity** ad alanları **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="27af7-329">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="27af7-330">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcı ad alanlarını ekleme*)</span><span class="sxs-lookup"><span data-stu-id="27af7-330">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="27af7-331">Devralınan sınıfı olun **IFilterProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="27af7-331">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="27af7-332">Ekleme bir **IUnityContainer** özelliğinde **FilterProvider** sınıfı ve kapsayıcı atamak için bir sınıf oluşturucu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="27af7-332">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="27af7-333">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcı Oluşturucusu*)</span><span class="sxs-lookup"><span data-stu-id="27af7-333">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="27af7-334">Filtre sağlayıcı sınıfı oluşturucusu değil oluşturma bir **yeni** içinde nesne.</span><span class="sxs-lookup"><span data-stu-id="27af7-334">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="27af7-335">Kapsayıcı parametre olarak geçirilir ve bağımlılık Unity tarafından çözüldü.</span><span class="sxs-lookup"><span data-stu-id="27af7-335">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="27af7-336">İçinde **FilterProvider** sınıfı, yöntemi uygulaması **GetFilters** gelen **IFilterProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="27af7-336">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="27af7-337">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcı GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="27af7-337">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="27af7-338">Görev 2 - kaydetme ve filtre etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="27af7-338">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="27af7-339">Bu görevde, site izlemeyi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="27af7-339">In this task, you will enable site tracking.</span></span> <span data-ttu-id="27af7-340">Bunu yapmak için filtreye kaydedeceksiniz **Bootstrapper.cs BuildUnityContainer** yöntemi izlemeyi başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="27af7-340">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="27af7-341">Açık **Web.config** proje kök ve etkinleştir izleme izleme System.Web Grup adresindeki bulunur.</span><span class="sxs-lookup"><span data-stu-id="27af7-341">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="27af7-342">Açık **Bootstrapper.cs** proje kökündeki.</span><span class="sxs-lookup"><span data-stu-id="27af7-342">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="27af7-343">Bir başvuru ekleyin **MvcMusicStore.Filters** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="27af7-343">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="27af7-344">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - önyükleyici ad alanlarını ekleme*)</span><span class="sxs-lookup"><span data-stu-id="27af7-344">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="27af7-345">Seçin **BuildUnityContainer** yöntemi ve kaydetme Unity kapsayıcısında filtre.</span><span class="sxs-lookup"><span data-stu-id="27af7-345">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="27af7-346">Eylem filtresi yanı sıra filtresi sağlayıcısını kaydetmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="27af7-346">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="27af7-347">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - kayıt FilterProvider ve ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="27af7-347">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="27af7-348">Görev 3 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="27af7-348">Task 3 - Running the Application</span></span>

<span data-ttu-id="27af7-349">Bu görevde uygulamayı çalıştırın ve özel eylem filtresi Etkinlik izleme olduğunu sınayın:</span><span class="sxs-lookup"><span data-stu-id="27af7-349">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="27af7-350">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="27af7-350">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="27af7-351">Tıklatın **Rock** türler menü içinde.</span><span class="sxs-lookup"><span data-stu-id="27af7-351">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="27af7-352">İsterseniz daha fazla türler göz atabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-352">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="27af7-353">![Müzik deposu](aspnet-mvc-4-dependency-injection/_static/image11.png "müzik deposu")</span><span class="sxs-lookup"><span data-stu-id="27af7-353">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="27af7-354">*Müzik Deposu*</span><span class="sxs-lookup"><span data-stu-id="27af7-354">*Music Store*</span></span>
3. <span data-ttu-id="27af7-355">Gözat **/Trace.axd** uygulama sayfasında ve ardından izleme görmek için **ayrıntıları görüntüle**.</span><span class="sxs-lookup"><span data-stu-id="27af7-355">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="27af7-356">![Uygulama izleme günlüğü](aspnet-mvc-4-dependency-injection/_static/image12.png "uygulama izleme günlüğü")</span><span class="sxs-lookup"><span data-stu-id="27af7-356">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="27af7-357">*Uygulama izleme günlüğü*</span><span class="sxs-lookup"><span data-stu-id="27af7-357">*Application Trace Log*</span></span>

    <span data-ttu-id="27af7-358">![Uygulama izleme - istek ayrıntıları](aspnet-mvc-4-dependency-injection/_static/image13.png "uygulama izleme - istek ayrıntıları")</span><span class="sxs-lookup"><span data-stu-id="27af7-358">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="27af7-359">*Uygulama izleme - istek ayrıntıları*</span><span class="sxs-lookup"><span data-stu-id="27af7-359">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="27af7-360">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="27af7-360">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="27af7-361">Özet</span><span class="sxs-lookup"><span data-stu-id="27af7-361">Summary</span></span>

<span data-ttu-id="27af7-362">Bu uygulamalı Laboratuvar tamamlayarak bağımlılık ekleme ASP.NET MVC 4'te bir NuGet paketi kullanarak Unity tümleştirerek kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="27af7-362">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="27af7-363">Elde etmek için bağımlılık ekleme denetleyicileri, görünümler ve eylem filtrelerini içinde kullandınız.</span><span class="sxs-lookup"><span data-stu-id="27af7-363">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="27af7-364">Aşağıdaki kavramlar kapsanan:</span><span class="sxs-lookup"><span data-stu-id="27af7-364">The following concepts were covered:</span></span>

- <span data-ttu-id="27af7-365">ASP.NET MVC 4 bağımlılık ekleme özellikleri</span><span class="sxs-lookup"><span data-stu-id="27af7-365">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="27af7-366">Unity.Mvc3 NuGet paketi kullanarak unity tümleştirme</span><span class="sxs-lookup"><span data-stu-id="27af7-366">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="27af7-367">Denetleyicileriyle bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="27af7-367">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="27af7-368">Görünümlerde bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="27af7-368">Dependency Injection in Views</span></span>
- <span data-ttu-id="27af7-369">Eylem filtreleri bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="27af7-369">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="27af7-370">Ek A: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="27af7-370">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="27af7-371">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="27af7-371">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="27af7-372">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="27af7-372">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="27af7-373">Git [ [https://go.microsoft.com/? LinkID 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="27af7-373">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="27af7-374">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.</span><span class="sxs-lookup"><span data-stu-id="27af7-374">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="27af7-375">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="27af7-375">Click on **Install Now**.</span></span> <span data-ttu-id="27af7-376">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="27af7-376">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="27af7-377">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="27af7-377">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="27af7-378">![Visual Studio Express yükleme](aspnet-mvc-4-dependency-injection/_static/image14.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="27af7-378">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="27af7-379">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="27af7-379">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="27af7-380">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="27af7-380">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="27af7-382">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="27af7-382">*Accepting the license terms*</span></span>
5. <span data-ttu-id="27af7-383">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="27af7-383">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="27af7-385">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="27af7-385">*Installation progress*</span></span>
6. <span data-ttu-id="27af7-386">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="27af7-386">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="27af7-388">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="27af7-388">*Installation completed*</span></span>
7. <span data-ttu-id="27af7-389">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="27af7-389">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="27af7-390">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="27af7-390">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="27af7-392">*VS Express Web döşemeye*</span><span class="sxs-lookup"><span data-stu-id="27af7-392">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="27af7-393">Ek B: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="27af7-393">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="27af7-394">Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip.</span><span class="sxs-lookup"><span data-stu-id="27af7-394">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="27af7-395">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="27af7-395">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="27af7-396">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-dependency-injection/_static/image19.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="27af7-396">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="27af7-397">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="27af7-397">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="27af7-398">***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="27af7-398">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="27af7-399">İmleci, burada kod eklemek istediğiniz yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="27af7-399">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="27af7-400">(Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="27af7-400">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="27af7-401">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="27af7-401">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="27af7-402">Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="27af7-402">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="27af7-403">İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="27af7-403">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="27af7-404">![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-dependency-injection/_static/image20.png "parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="27af7-404">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="27af7-405">*Kod parçacığında adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="27af7-405">*Start typing the snippet name*</span></span>

<span data-ttu-id="27af7-406">![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-dependency-injection/_static/image21.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="27af7-406">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="27af7-407">*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="27af7-407">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="27af7-408">![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-dependency-injection/_static/image22.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")</span><span class="sxs-lookup"><span data-stu-id="27af7-408">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="27af7-409">*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*</span><span class="sxs-lookup"><span data-stu-id="27af7-409">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="27af7-410">***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="27af7-410">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="27af7-411">Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="27af7-411">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="27af7-412">Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.</span><span class="sxs-lookup"><span data-stu-id="27af7-412">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="27af7-413">Tıklayarak ilgili kod parçacığında listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="27af7-413">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="27af7-414">![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-dependency-injection/_static/image23.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")</span><span class="sxs-lookup"><span data-stu-id="27af7-414">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="27af7-415">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*</span><span class="sxs-lookup"><span data-stu-id="27af7-415">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="27af7-416">![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-dependency-injection/_static/image24.png "tıklayarak ilgili kod parçacığında listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="27af7-416">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="27af7-417">*Tıklayarak ilgili kod parçacığında listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="27af7-417">*Pick the relevant snippet from the list, by clicking on it*</span></span>
