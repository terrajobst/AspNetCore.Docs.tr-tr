---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: ASP.NET MVC 4 bağımlılık ekleme | Microsoft Docs
author: rick-anderson
description: 'Not: Bu uygulamalı Laboratuvar, ASP.NET MVC ve ASP.NET MVC 4 filtreleri temel bilgiye sahip varsayar. ASP.NET MVC 4 filtrelerden önce biz rec kullanmadıysanız...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3f9222c7b485f552da91f4875c882db7e03cdd0a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755349"
---
# <a name="aspnet-mvc-4-dependency-injection"></a><span data-ttu-id="e050f-104">ASP.NET MVC 4 bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-104">ASP.NET MVC 4 Dependency Injection</span></span>

<span data-ttu-id="e050f-105">Tarafından [Team Web Kampları](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e050f-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e050f-106">Eğitim Seti Web Kampları indirin</span><span class="sxs-lookup"><span data-stu-id="e050f-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="e050f-107">Bu uygulamalı laboratuvarı temel bilgiye sahip olduğunuz varsayılır **ASP.NET MVC** ve **ASP.NET MVC 4 filtreler**.</span><span class="sxs-lookup"><span data-stu-id="e050f-107">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC** and **ASP.NET MVC 4 filters**.</span></span> <span data-ttu-id="e050f-108">Kullanmadıysanız, **ASP.NET MVC 4 filtreler** gitmenizi öneririz önce **ASP.NET MVC özel eylem filtreleri** Hands-on-Lab.</span><span class="sxs-lookup"><span data-stu-id="e050f-108">If you have not used **ASP.NET MVC 4 filters** before, we recommend you to go over **ASP.NET MVC Custom Action Filters** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="e050f-109">Web Kampları eğitim Seti'nde bulunan, tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e050f-109">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="e050f-110">Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 bağımlılık ekleme](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="e050f-110">The project specific to this lab is available at [ASP.NET MVC 4 Dependency Injection](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).</span></span>

<span data-ttu-id="e050f-111">İçinde **nesne yönelimli programlama** paradigm, nesneleri birlikte çalışır bir işbirliği modelinde katkıda bulunanların ve tüketiciler olduğu.</span><span class="sxs-lookup"><span data-stu-id="e050f-111">In **Object Oriented Programming** paradigm, objects work together in a collaboration model where there are contributors and consumers.</span></span> <span data-ttu-id="e050f-112">Doğal olarak, bu iletişim modelini nesneleri ve karmaşıklık artar yönetmek zor hale bileşenleri arasındaki bağımlılıkları oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e050f-112">Naturally, this communication model generates dependencies between objects and components, becoming difficult to manage when complexity increases.</span></span>

<span data-ttu-id="e050f-113">![Sınıf bağımlılıkları ve model karmaşıklığı](aspnet-mvc-4-dependency-injection/_static/image1.png "sınıfı bağımlılıkları ve model karmaşıklığı")</span><span class="sxs-lookup"><span data-stu-id="e050f-113">![Class dependencies and model complexity](aspnet-mvc-4-dependency-injection/_static/image1.png "Class dependencies and model complexity")</span></span>

<span data-ttu-id="e050f-114">*Sınıf bağımlılıkları ve model karmaşıklığı*</span><span class="sxs-lookup"><span data-stu-id="e050f-114">*Class dependencies and model complexity*</span></span>

<span data-ttu-id="e050f-115">Büyük olasılıkla hakkında duymuş **Fabrika deseni** ve arabirim ve istemci nesneler genellikle hizmet konumu için sorumlu olduğu hizmetleri kullanılarak uygulama arasındaki ayrımı.</span><span class="sxs-lookup"><span data-stu-id="e050f-115">You have probably heard about the **Factory Pattern** and the separation between the interface and the implementation using services, where the client objects are often responsible for service location.</span></span>

<span data-ttu-id="e050f-116">Bağımlılık ekleme, ters çevirmeyi denetimin belirli bir uygulama modelidir.</span><span class="sxs-lookup"><span data-stu-id="e050f-116">The Dependency Injection pattern is a particular implementation of Inversion of Control.</span></span> <span data-ttu-id="e050f-117">**Tersine çevirme (IOC) denetiminin** nesneleri bağlı oldukları işlerini yapmak için diğer nesnelerin oluşturmayın anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e050f-117">**Inversion of Control (IoC)** means that objects do not create other objects on which they rely to do their work.</span></span> <span data-ttu-id="e050f-118">Bunun yerine bir dış kaynaktan (örneğin, bir xml yapılandırma dosyası) ihtiyaç duydukları nesneleri alın.</span><span class="sxs-lookup"><span data-stu-id="e050f-118">Instead, they get the objects that they need from an outside source (for example, an xml configuration file).</span></span>

<span data-ttu-id="e050f-119">**Bağımlılık ekleme (dı)** bu nesne müdahalesi olmadan genellikle Oluşturucusu parametrelerini geçirir framework bileşeni tarafından yapılır ve özelliklerini ayarlama, anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e050f-119">**Dependency Injection (DI)** means that this is done without the object intervention, usually by a framework component that passes constructor parameters and set properties.</span></span>

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a><span data-ttu-id="e050f-120">Bağımlılık ekleme (dı) tasarım deseni</span><span class="sxs-lookup"><span data-stu-id="e050f-120">The Dependency Injection (DI) Design Pattern</span></span>

<span data-ttu-id="e050f-121">Yüksek bir düzeyde bağımlılık ekleme amacı olan bir istemci sınıfı (örn *golfçü*) arabirim karşılayan bir şey gerekir (örn *IClub*).</span><span class="sxs-lookup"><span data-stu-id="e050f-121">At a high level, the goal of Dependency Injection is that a client class (e.g. *the golfer*) needs something that satisfies an interface (e.g. *IClub*).</span></span> <span data-ttu-id="e050f-122">Somut türünü nedir önemli değildir (örn *WoodClub, IronClub, WedgeClub* veya *PutterClub*), işleyen başka birisi istediği (örneğin iyi *caddy*).</span><span class="sxs-lookup"><span data-stu-id="e050f-122">It doesn't care what the concrete type is (e.g. *WoodClub, IronClub, WedgeClub* or *PutterClub*), it wants someone else to handle that (e.g. a good *caddy*).</span></span> <span data-ttu-id="e050f-123">ASP.NET mvc'de bağımlılık çözümleyiciyi bağımlılık mantığınızı başka bir yerde kaydetmenizi izin verebilirsiniz (örneğin bir kapsayıcı veya bir *kulüpleri paketi*).</span><span class="sxs-lookup"><span data-stu-id="e050f-123">The Dependency Resolver in ASP.NET MVC can allow you to register your dependency logic somewhere else (e.g. a container or a *bag of clubs*).</span></span>

<span data-ttu-id="e050f-124">![Bağımlılık ekleme diyagramı](aspnet-mvc-4-dependency-injection/_static/image2.png "bağımlılık ekleme çizim")</span><span class="sxs-lookup"><span data-stu-id="e050f-124">![Dependency Injection diagram](aspnet-mvc-4-dependency-injection/_static/image2.png "Dependency Injection illustration")</span></span>

<span data-ttu-id="e050f-125">*Bağımlılık ekleme - Golf benzerleme*</span><span class="sxs-lookup"><span data-stu-id="e050f-125">*Dependency Injection - Golf analogy*</span></span>

<span data-ttu-id="e050f-126">Bağımlılık ekleme desenini ve tersine çevirme denetim kullanmanın avantajları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e050f-126">The advantages of using Dependency Injection pattern and Inversion of Control are the following:</span></span>

- <span data-ttu-id="e050f-127">Sınıf bağlantısı azaltır</span><span class="sxs-lookup"><span data-stu-id="e050f-127">Reduces class coupling</span></span>
- <span data-ttu-id="e050f-128">Kodu yeniden kullanma artırır</span><span class="sxs-lookup"><span data-stu-id="e050f-128">Increases code reusing</span></span>
- <span data-ttu-id="e050f-129">Kod bakımı artırır</span><span class="sxs-lookup"><span data-stu-id="e050f-129">Improves code maintainability</span></span>
- <span data-ttu-id="e050f-130">Uygulamayı test artırır</span><span class="sxs-lookup"><span data-stu-id="e050f-130">Improves application testing</span></span>

> [!NOTE]
> <span data-ttu-id="e050f-131">Bağımlılık ekleme bazen soyut Fabrika tasarım deseni ile karşılaştırıldığında, ancak her iki yaklaşım arasında küçük bir fark yoktur.</span><span class="sxs-lookup"><span data-stu-id="e050f-131">Dependency Injection is sometimes compared with Abstract Factory Design Pattern, but there is a slight difference between both approaches.</span></span> <span data-ttu-id="e050f-132">DI fabrikaları ve kaydedilen Hizmetleri çağırarak bağımlılıkları çözmeye arkasında çalışma bir çerçeve vardır.</span><span class="sxs-lookup"><span data-stu-id="e050f-132">DI has a Framework working behind to solve dependencies by calling the factories and the registered services.</span></span>


<span data-ttu-id="e050f-133">Bağımlılık ekleme desenini anladığınıza göre ASP.NET MVC 4'te uygulama bu Laboratuvar öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-133">Now that you understand the Dependency Injection Pattern, you will learn throughout this lab how to apply it in ASP.NET MVC 4.</span></span> <span data-ttu-id="e050f-134">Bağımlılık ekleme kullanılarak başlar **denetleyicileri** Veritabanı Erişim hizmeti eklemek için.</span><span class="sxs-lookup"><span data-stu-id="e050f-134">You will start using Dependency Injection in the **Controllers** to include a database access service.</span></span> <span data-ttu-id="e050f-135">Ardından, bağımlılık ekleme için uygulayacağınız **görünümleri** bir hizmeti kullanmak ve bilgi göstermek için.</span><span class="sxs-lookup"><span data-stu-id="e050f-135">Next, you will apply Dependency Injection to the **Views** to consume a service and show information.</span></span> <span data-ttu-id="e050f-136">Son olarak, bir özel eylem filtresi çözümdeki ekleme DI ASP.NET MVC 4 filtreleri için genişletilir.</span><span class="sxs-lookup"><span data-stu-id="e050f-136">Finally, you will extend the DI to ASP.NET MVC 4 Filters, injecting a custom action filter in the solution.</span></span>

<span data-ttu-id="e050f-137">Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="e050f-137">In this Hands-on Lab, you will learn how to:</span></span>

- <span data-ttu-id="e050f-138">ASP.NET MVC 4 bağımlılık ekleme için NuGet paketlerini kullanarak Unity ile tümleştirin</span><span class="sxs-lookup"><span data-stu-id="e050f-138">Integrate ASP.NET MVC 4 with Unity for Dependency Injection using NuGet Packages</span></span>
- <span data-ttu-id="e050f-139">ASP.NET MVC denetleyicisi içinde kullanım bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-139">Use Dependency Injection inside an ASP.NET MVC Controller</span></span>
- <span data-ttu-id="e050f-140">Bir ASP.NET MVC görünümü içinde kullanım bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-140">Use Dependency Injection inside an ASP.NET MVC View</span></span>
- <span data-ttu-id="e050f-141">Bir ASP.NET MVC eylem filtresi içinde kullanım bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-141">Use Dependency Injection inside an ASP.NET MVC Action Filter</span></span>

> [!NOTE]
> <span data-ttu-id="e050f-142">Bu Laboratuvar için bağımlılık çözümlemesi Unity.Mvc3 NuGet paketi kullanıyor, ancak ASP.NET MVC 4 ile çalışmak için bir bağımlılık ekleme Framework uyum sağlamak mümkündür.</span><span class="sxs-lookup"><span data-stu-id="e050f-142">This Lab is using Unity.Mvc3 NuGet Package for dependency resolution, but it is possible to adapt any Dependency Injection Framework to work with ASP.NET MVC 4.</span></span>


<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e050f-143">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e050f-143">Prerequisites</span></span>

<span data-ttu-id="e050f-144">Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="e050f-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="e050f-145">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="e050f-145">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e050f-146">Kurulum</span><span class="sxs-lookup"><span data-stu-id="e050f-146">Setup</span></span>

<span data-ttu-id="e050f-147">**Kod parçacıkları yükleniyor**</span><span class="sxs-lookup"><span data-stu-id="e050f-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="e050f-148">Kolaylık olması için bu Laboratuvar yöneteceğiniz kodun çoğu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e050f-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="e050f-149">Kod parçacıklarını çalıştırmak yüklenecek **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="e050f-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="e050f-150">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek B: kullanarak kod parçacıklarına](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="e050f-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e050f-151">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="e050f-151">Exercises</span></span>

<span data-ttu-id="e050f-152">Bu uygulamalı Laboratuvar tarafından aşağıdaki çalışmaları oluşur:</span><span class="sxs-lookup"><span data-stu-id="e050f-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="e050f-153">Alıştırma 1: bir denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-153">Exercise 1: Injecting a Controller</span></span>](#Exercise1)
2. [<span data-ttu-id="e050f-154">Alıştırma 2: bir görünüm ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-154">Exercise 2: Injecting a View</span></span>](#Exercise2)
3. [<span data-ttu-id="e050f-155">Alıştırma 3: Filtre ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-155">Exercise 3: Injecting Filters</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="e050f-156">Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="e050f-156">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="e050f-157">Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-157">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="e050f-158">Bu laboratuvarı tamamlamak için tahmini süre: **30 dakika**.</span><span class="sxs-lookup"><span data-stu-id="e050f-158">Estimated time to complete this lab: **30 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a><span data-ttu-id="e050f-159">Alıştırma 1: bir denetleyici ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-159">Exercise 1: Injecting a Controller</span></span>

<span data-ttu-id="e050f-160">Bu alıştırmada, bir NuGet paketi kullanarak Unity tümleştirerek ASP.NET MVC denetleyicileri bağımlılık ekleme kullanmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-160">In this exercise, you will learn how to use Dependency Injection in ASP.NET MVC Controllers by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="e050f-161">Bu nedenle, veri erişim mantığı ayırmak için MvcMusicStore denetleyicilere Hizmetleri dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="e050f-161">For that reason, you will include services into your MvcMusicStore controllers to separate the logic from the data access.</span></span> <span data-ttu-id="e050f-162">Hizmetleri yardımıyla bağımlılık ekleme kullanılarak çözümlenir denetleyici Oluşturucu, yeni bir bağımlılık oluşturacak **Unity**.</span><span class="sxs-lookup"><span data-stu-id="e050f-162">The services will create a new dependency in the controller constructor, which will be resolved using Dependency Injection with the help of **Unity**.</span></span>

<span data-ttu-id="e050f-163">Bu yaklaşım, daha esnek ve bakımı ve test etmek daha kolay olan daha az bağlı uygulamalar, oluşturma adımları gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e050f-163">This approach will show you how to generate less coupled applications, which are more flexible and easier to maintain and test.</span></span> <span data-ttu-id="e050f-164">Aynı zamanda ASP.NET MVC, Unity ile tümleştirmeyi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-164">You will also learn how to integrate ASP.NET MVC with Unity.</span></span>

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a><span data-ttu-id="e050f-165">StoreManager hizmeti hakkında</span><span class="sxs-lookup"><span data-stu-id="e050f-165">About StoreManager Service</span></span>

<span data-ttu-id="e050f-166">MVC müzik başlangıç çözümde şimdi sağlanan Store adlı Store denetleyicisi verileri yöneten bir hizmet içerir **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="e050f-166">The MVC Music Store provided in the begin solution now includes a service that manages the Store Controller data named **StoreService**.</span></span> <span data-ttu-id="e050f-167">Aşağıda Store hizmet uygulaması bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-167">Below you will find the Store Service implementation.</span></span> <span data-ttu-id="e050f-168">Tüm yöntemleri modeli varlıklarından dönüş unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e050f-168">Note that all the methods return Model entities.</span></span>

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

<span data-ttu-id="e050f-169">**StoreController** begin işlevinden çözümü şimdi tüketir **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="e050f-169">**StoreController** from the begin solution now consumes **StoreService**.</span></span> <span data-ttu-id="e050f-170">Tüm veri başvurularını sıradan kaldırıldığını **StoreController**ve geçerli veri erişimi sağlayıcısı kullanan herhangi bir yöntemi değiştirmeden artık mümkün **StoreService**.</span><span class="sxs-lookup"><span data-stu-id="e050f-170">All the data references were removed from **StoreController**, and now possible to modify the current data access provider without changing any method that consumes **StoreService**.</span></span>

<span data-ttu-id="e050f-171">Bunun altında bulabilirsiniz **StoreController** uygulaması ile bir bağımlılık içeriyor **StoreService** sınıf oluşturucusu içinde.</span><span class="sxs-lookup"><span data-stu-id="e050f-171">You will find below that the **StoreController** implementation has a dependency with **StoreService** inside the class constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="e050f-172">Bu alıştırmada, sunulan bağımlılık ilgili **denetim tersine çevirme** (IOC).</span><span class="sxs-lookup"><span data-stu-id="e050f-172">The dependency introduced in this exercise is related to **Inversion of Control** (IoC).</span></span>
> 
> <span data-ttu-id="e050f-173">**StoreController** sınıf oluşturucusunu alır bir **IStoreService** sınıf içinde hizmet çağrılarından gerçekleştirmek için gerekli olan tür parametresi.</span><span class="sxs-lookup"><span data-stu-id="e050f-173">The **StoreController** class constructor receives an **IStoreService** type parameter, which is essential to perform service calls from inside the class.</span></span> <span data-ttu-id="e050f-174">Ancak, **StoreController** herhangi bir denetleyici, ASP.NET MVC ile çalışmak için gereken varsayılan oluşturucu (parametresiz) uygulamıyor.</span><span class="sxs-lookup"><span data-stu-id="e050f-174">However, **StoreController** does not implement the default constructor (with no parameters) that any controller must have to work with ASP.NET MVC.</span></span>
> 
> <span data-ttu-id="e050f-175">Bağımlılık çözmek için denetleyici soyut bir Fabrika (belirtilen türde herhangi bir nesne döndürür sınıfı) tarafından oluşturulması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e050f-175">To resolve the dependency, the controller has to be created by an abstract factory (a class that returns any object of the specified type).</span></span>


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="e050f-176">Hizmet nesnesi göndermeden StoreController bildirilen parametresiz bir oluşturucusu olmadığından oluşturmak sınıf çalıştığında bir hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="e050f-176">You will get an error when the class tries to create the StoreController without sending the service object, as there is no parameterless constructor declared.</span></span>


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a><span data-ttu-id="e050f-177">Görev 1 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e050f-177">Task 1 - Running the Application</span></span>

<span data-ttu-id="e050f-178">Bu görevde, veri erişimi uygulama mantığından ayıran Store denetleyici hizmeti içerir başlangıç uygulama çalışır.</span><span class="sxs-lookup"><span data-stu-id="e050f-178">In this task, you will run the Begin application, which includes the service into the Store Controller that separates the data access from the application logic.</span></span>

<span data-ttu-id="e050f-179">Denetleyici Hizmeti varsayılan olarak bir parametre olarak geçirilir şekilde uygulama çalışırken, bir özel durum alırsınız:</span><span class="sxs-lookup"><span data-stu-id="e050f-179">When running the application, you will receive an exception, as the controller service is not passed as a parameter by default:</span></span>

1. <span data-ttu-id="e050f-180">Açık **başlamak** çözüm bulunan **Source\Ex01 ekleme Controller\Begin**.</span><span class="sxs-lookup"><span data-stu-id="e050f-180">Open the **Begin** solution located in **Source\Ex01-Injecting Controller\Begin**.</span></span>

   1. <span data-ttu-id="e050f-181">Bazı eksik NuGet paketleri indirmeniz gerekecek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="e050f-181">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e050f-182">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="e050f-182">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e050f-183">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="e050f-183">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e050f-184">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="e050f-184">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e050f-185">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="e050f-185">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e050f-186">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="e050f-186">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e050f-187">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="e050f-187">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="e050f-188">Tuşuna **Ctrl + F5 tuşlarına basarak** uygulamayı hata ayıklama olmadan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e050f-188">Press **Ctrl + F5** to run the application without debugging.</span></span> <span data-ttu-id="e050f-189">Hata iletisi alırsınız &quot; **bu nesne için tanımlanmış parametresiz bir oluşturucusu**&quot;:</span><span class="sxs-lookup"><span data-stu-id="e050f-189">You will get the error message &quot;**No parameterless constructor defined for this object**&quot;:</span></span>

    <span data-ttu-id="e050f-190">![ASP.NET MVC uygulaması başlamak çalıştırılırken hata](aspnet-mvc-4-dependency-injection/_static/image3.png "başlamak ASP.NET MVC uygulaması çalıştırılırken hata")</span><span class="sxs-lookup"><span data-stu-id="e050f-190">![Error while running ASP.NET MVC Begin Application](aspnet-mvc-4-dependency-injection/_static/image3.png "Error while running ASP.NET MVC Begin Application")</span></span>

    <span data-ttu-id="e050f-191">*ASP.NET MVC uygulaması başlamak çalıştırılırken hata*</span><span class="sxs-lookup"><span data-stu-id="e050f-191">*Error while running ASP.NET MVC Begin Application*</span></span>
3. <span data-ttu-id="e050f-192">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="e050f-192">Close the browser.</span></span>

<span data-ttu-id="e050f-193">Aşağıdaki adımlarda bu denetleyicisi bağımlılık ekleme müzik Store çözüm üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="e050f-193">In the following steps you will work on the Music Store Solution to inject the dependency this controller needs.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a><span data-ttu-id="e050f-194">Görev 2 - MvcMusicStore çözümünde dahil olmak üzere Unity</span><span class="sxs-lookup"><span data-stu-id="e050f-194">Task 2 - Including Unity into MvcMusicStore Solution</span></span>

<span data-ttu-id="e050f-195">Bu görevde, içerecektir **Unity.Mvc3** çözüm için NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="e050f-195">In this task, you will include **Unity.Mvc3** NuGet Package to the solution.</span></span>

> [!NOTE]
> <span data-ttu-id="e050f-196">Unity.Mvc3 paketi ASP.NET MVC 3 için tasarlanmıştır ancak ASP.NET MVC 4 ile tamamen uyumludur.</span><span class="sxs-lookup"><span data-stu-id="e050f-196">Unity.Mvc3 package was designed for ASP.NET MVC 3, but it is fully compatible with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="e050f-197">Unity örneği için bir hafif, Genişletilebilir bağımlılık ekleme kapsayıcısını isteğe bağlı destek olduğu ve durdurma yazın.</span><span class="sxs-lookup"><span data-stu-id="e050f-197">Unity is a lightweight, extensible dependency injection container with optional support for instance and type interception.</span></span> <span data-ttu-id="e050f-198">Her tür .NET uygulama kullanmak için genel amaçlı bir kapsayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="e050f-198">It is a general-purpose container for use in any type of .NET application.</span></span> <span data-ttu-id="e050f-199">Bağımlılık ekleme mekanizmalar da dahil olmak üzere tüm ortak özellikleri sağlar: nesne oluşturma, kapsayıcıya Bileşen Yapılandırması erteleyerek çalışma zamanı ve esneklik, bağımlılıkları belirterek gereksinimleri soyutlamasıdır.</span><span class="sxs-lookup"><span data-stu-id="e050f-199">It provides all the common features found in dependency injection mechanisms including: object creation, abstraction of requirements by specifying dependencies at runtime and flexibility, by deferring the component configuration to the container.</span></span>


1. <span data-ttu-id="e050f-200">Yükleme **Unity.Mvc3** NuGet paketini **MvcMusicStore** proje.</span><span class="sxs-lookup"><span data-stu-id="e050f-200">Install **Unity.Mvc3** NuGet Package in the **MvcMusicStore** project.</span></span> <span data-ttu-id="e050f-201">Bunu yapmak için açık **Paket Yöneticisi Konsolu** gelen **görünümü** | **diğer Windows**.</span><span class="sxs-lookup"><span data-stu-id="e050f-201">To do this, open the **Package Manager Console** from **View** | **Other Windows**.</span></span>
2. <span data-ttu-id="e050f-202">Şu komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e050f-202">Run the following command.</span></span>

    <span data-ttu-id="e050f-203">PMC</span><span class="sxs-lookup"><span data-stu-id="e050f-203">PMC</span></span>

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    <span data-ttu-id="e050f-204">![Unity.Mvc3 NuGet paketini yükleyerek](aspnet-mvc-4-dependency-injection/_static/image4.png "Unity.Mvc3 NuGet paketi yükleniyor")</span><span class="sxs-lookup"><span data-stu-id="e050f-204">![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")</span></span>

    <span data-ttu-id="e050f-205">*Unity.Mvc3 NuGet paketi yükleniyor*</span><span class="sxs-lookup"><span data-stu-id="e050f-205">*Installing Unity.Mvc3 NuGet Package*</span></span>
3. <span data-ttu-id="e050f-206">Bir kez **Unity.Mvc3** paketinin yüklendiğinden, otomatik olarak ekler Unity yapılandırmasını basitleştirmek için klasör ve dosya keşfedin.</span><span class="sxs-lookup"><span data-stu-id="e050f-206">Once the **Unity.Mvc3** package is installed, explore the files and folders it automatically adds in order to simplify Unity configuration.</span></span>

    <span data-ttu-id="e050f-207">![Yüklü Unity.Mvc3 paket](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 paket yüklü")</span><span class="sxs-lookup"><span data-stu-id="e050f-207">![Unity.Mvc3 package installed](aspnet-mvc-4-dependency-injection/_static/image5.png "Unity.Mvc3 package installed")</span></span>

    <span data-ttu-id="e050f-208">*Yüklü Unity.Mvc3 paketi*</span><span class="sxs-lookup"><span data-stu-id="e050f-208">*Unity.Mvc3 package installed*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a><span data-ttu-id="e050f-209">Görev 3 - kaydetme Unity Global.asax.cs uygulamada\_Başlat</span><span class="sxs-lookup"><span data-stu-id="e050f-209">Task 3 - Registering Unity in Global.asax.cs Application\_Start</span></span>

<span data-ttu-id="e050f-210">Bu görevde, güncelleştirecektir **uygulama\_Başlat** yöntemi bulunan **Global.asax.cs** Unity önyükleyici Başlatıcı arayın ve sonra önyükleyici dosya kaydetme güncelleştirmek için Denetleyici ve hizmet bağımlılık ekleme için kullanır.</span><span class="sxs-lookup"><span data-stu-id="e050f-210">In this task, you will update the **Application\_Start** method located in **Global.asax.cs** to call the Unity Bootstrapper initializer and then, update the Bootstrapper file registering the Service and Controller you will use for Dependency Injection.</span></span>

1. <span data-ttu-id="e050f-211">Şimdi, Unity kapsayıcı başlatır dosyası olan önyükleyici ve bağımlılık çözümleyiciyi yedekleme kanca.</span><span class="sxs-lookup"><span data-stu-id="e050f-211">Now, you will hook up the Bootstrapper which is the file that initializes the Unity container and Dependency Resolver.</span></span> <span data-ttu-id="e050f-212">Bunu yapmak için açık **Global.asax.cs** ve içinde aşağıdaki vurgulanmış kodu ekleyin **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e050f-212">To do this, open **Global.asax.cs** and add the following highlighted code within the **Application\_Start** method.</span></span>

    <span data-ttu-id="e050f-213">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - Unity başlatmak*)</span><span class="sxs-lookup"><span data-stu-id="e050f-213">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Initialize Unity*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. <span data-ttu-id="e050f-214">Açık **Bootstrapper.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="e050f-214">Open **Bootstrapper.cs** file.</span></span>
3. <span data-ttu-id="e050f-215">Aşağıdaki ad alanlarını içerir: **MvcMusicStore.Services** ve **MusicStore.Controllers**.</span><span class="sxs-lookup"><span data-stu-id="e050f-215">Include the following namespaces: **MvcMusicStore.Services** and **MusicStore.Controllers**.</span></span>

    <span data-ttu-id="e050f-216">(Kod parçacığını - *ASP.NET bağımlılık ekleme - Ex01 - Laboratuvar önyükleyici ad alanlarını ekleme*)</span><span class="sxs-lookup"><span data-stu-id="e050f-216">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. <span data-ttu-id="e050f-217">Değiştirin **BuildUnityContainer** yöntemini aşağıdaki kodla Store denetleyici ve Store hizmet kaydeden içerik.</span><span class="sxs-lookup"><span data-stu-id="e050f-217">Replace **BuildUnityContainer** method's content with the following code that registers Store Controller and Store Service.</span></span>

    <span data-ttu-id="e050f-218">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex01 - kayıt Store denetleyici ve hizmet*)</span><span class="sxs-lookup"><span data-stu-id="e050f-218">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex01 - Register Store Controller and Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="e050f-219">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e050f-219">Task 4 - Running the Application</span></span>

<span data-ttu-id="e050f-220">Bu görevde, bunu şimdi Unity eklendikten sonra yüklenebileceğini doğrulamak amacıyla uygulamanın çalışır.</span><span class="sxs-lookup"><span data-stu-id="e050f-220">In this task, you will run the application to verify that it can now be loaded after including Unity.</span></span>

1. <span data-ttu-id="e050f-221">Tuşuna **F5** uygulamayı çalıştırmak için uygulama artık herhangi bir hata iletisi göstermeden yüklenecektir.</span><span class="sxs-lookup"><span data-stu-id="e050f-221">Press **F5** to run the application, the application should now load without showing any error message.</span></span>

    <span data-ttu-id="e050f-222">![Uygulama bağımlılık ekleme ile çalışan](aspnet-mvc-4-dependency-injection/_static/image6.png "bağımlılık ekleme ile uygulama çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="e050f-222">![Running Application with Dependency Injection](aspnet-mvc-4-dependency-injection/_static/image6.png "Running Application with Dependency Injection")</span></span>

    <span data-ttu-id="e050f-223">*Bağımlılık ekleme ile çalışan uygulama*</span><span class="sxs-lookup"><span data-stu-id="e050f-223">*Running Application with Dependency Injection*</span></span>
2. <span data-ttu-id="e050f-224">Gözat **/Store**.</span><span class="sxs-lookup"><span data-stu-id="e050f-224">Browse to **/Store**.</span></span> <span data-ttu-id="e050f-225">Bu çağırma **StoreController**, artık oluşturulduğu kullanarak **Unity**.</span><span class="sxs-lookup"><span data-stu-id="e050f-225">This will invoke **StoreController**, which is now created using **Unity**.</span></span>

    <span data-ttu-id="e050f-226">![MVC müzik Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC müzik Store")</span><span class="sxs-lookup"><span data-stu-id="e050f-226">![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")</span></span>

    <span data-ttu-id="e050f-227">*MVC müzik Store*</span><span class="sxs-lookup"><span data-stu-id="e050f-227">*MVC Music Store*</span></span>
3. <span data-ttu-id="e050f-228">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="e050f-228">Close the browser.</span></span>

<span data-ttu-id="e050f-229">Aşağıdaki alıştırmalarda ASP.NET MVC görünümleri ve eylem filtrelerini içinde kullanmak için bağımlılık ekleme kapsamını genişletmek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-229">In the following exercises you will learn how to extend the Dependency Injection scope to use it inside ASP.NET MVC Views and Action Filters.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a><span data-ttu-id="e050f-230">Alıştırma 2: bir görünüm ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-230">Exercise 2: Injecting a View</span></span>

<span data-ttu-id="e050f-231">Bu alıştırmada, bağımlılık ekleme ile ASP.NET MVC 4'ün yeni özelliklerini görünümünde Unity tümleştirme için nasıl kullanılacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-231">In this exercise, you will learn how to use Dependency Injection in a view with the new features of ASP.NET MVC 4 for Unity integration.</span></span> <span data-ttu-id="e050f-232">Bunu yapabilmek için Store Gözat bir ileti ve aşağıdaki görüntü gösterilir görünümü içinde özel bir hizmeti çağırır.</span><span class="sxs-lookup"><span data-stu-id="e050f-232">In order to do that, you will call a custom service inside the Store Browse View, which will show a message and an image below.</span></span>

<span data-ttu-id="e050f-233">Daha sonra projeyi Unity ile tümleştirin ve bağımlılıkları eklenmek üzere bir özel bağımlılık çözümleyiciyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e050f-233">Then, you will integrate the project with Unity and create a custom dependency resolver to inject the dependencies.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a><span data-ttu-id="e050f-234">Görev 1 - bir hizmeti bir görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="e050f-234">Task 1 - Creating a View that Consumes a Service</span></span>

<span data-ttu-id="e050f-235">Bu görevde, yeni bir bağımlılık oluşturmak için bir hizmet çağrısı gerçekleştiren bir görünümü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e050f-235">In this task, you will create a view that performs a service call to generate a new dependency.</span></span> <span data-ttu-id="e050f-236">Bu çözümde bulunan basit bir Mesajlaşma hizmetinde hizmet oluşur.</span><span class="sxs-lookup"><span data-stu-id="e050f-236">The service consists in a simple messaging service included in this solution.</span></span>

1. <span data-ttu-id="e050f-237">Açık **başlamak** çözüm bulunan **Source\Ex02 ekleme View\Begin** klasör.</span><span class="sxs-lookup"><span data-stu-id="e050f-237">Open the **Begin** solution located in the **Source\Ex02-Injecting View\Begin** folder.</span></span> <span data-ttu-id="e050f-238">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="e050f-238">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e050f-239">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="e050f-239">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e050f-240">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="e050f-240">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e050f-241">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="e050f-241">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e050f-242">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="e050f-242">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e050f-243">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="e050f-243">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e050f-244">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="e050f-244">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e050f-245">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="e050f-245">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="e050f-246">Bu makalede daha fazla bilgi için bkz: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e050f-246">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e050f-247">Dahil **MessageService.cs** ve **IMessageService.cs** sınıfları bulunan **\Assets kaynak** klasöründe **/Hizmetleri**.</span><span class="sxs-lookup"><span data-stu-id="e050f-247">Include the **MessageService.cs** and the **IMessageService.cs** classes located in the **Source \Assets** folder in **/Services**.</span></span> <span data-ttu-id="e050f-248">Bunu yapmak için sağ **Hizmetleri** klasörü ve select **varolan öğeyi Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e050f-248">To do this, right-click **Services** folder and select **Add Existing Item**.</span></span> <span data-ttu-id="e050f-249">Dosyaların konumuna göz atın ve bunları ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-249">Browse to the files' location and include them.</span></span>

    <span data-ttu-id="e050f-250">![İleti hizmeti ve hizmet arabirimi ekleme](aspnet-mvc-4-dependency-injection/_static/image8.png "ileti hizmeti ve hizmet arabirimi ekleme")</span><span class="sxs-lookup"><span data-stu-id="e050f-250">![Adding Message Service and Service Interface](aspnet-mvc-4-dependency-injection/_static/image8.png "Adding Message Service and Service Interface")</span></span>

    <span data-ttu-id="e050f-251">*Ekleme ileti hizmeti ve hizmet arabirimi*</span><span class="sxs-lookup"><span data-stu-id="e050f-251">*Adding Message Service and Service Interface*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e050f-252">**IMessageService** arabirimi tarafından uygulanan iki özellikler tanımlar **MessageService** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e050f-252">The **IMessageService** interface defines two properties implemented by the **MessageService** class.</span></span> <span data-ttu-id="e050f-253">Bu özellikleri -**ileti** ve **ImageUrl**-görüntülenecek iletiyi ve görüntünün URL'sini depolayın.</span><span class="sxs-lookup"><span data-stu-id="e050f-253">These properties -**Message** and **ImageUrl**- store the message and the URL of the image to be displayed.</span></span>
3. <span data-ttu-id="e050f-254">Klasör Oluştur **/sayfaları** projenin kök klasörünü ve ardından varolan sınıf Ekle **MyBasePage.cs** gelen **Source\Assets**.</span><span class="sxs-lookup"><span data-stu-id="e050f-254">Create the folder **/Pages** in the project's root folder, and then add the existing class **MyBasePage.cs** from **Source\Assets**.</span></span> <span data-ttu-id="e050f-255">Devralınacağı temel sayfa aşağıdaki yapıya sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e050f-255">The base page you will inherit from has the following structure.</span></span>

    <span data-ttu-id="e050f-256">![Sayfalar klasöründe](aspnet-mvc-4-dependency-injection/_static/image9.png "sayfaları klasörü")</span><span class="sxs-lookup"><span data-stu-id="e050f-256">![Pages folder](aspnet-mvc-4-dependency-injection/_static/image9.png "Pages folder")</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. <span data-ttu-id="e050f-257">Açık **Browse.cshtml** görünümüne **/Views/Store** klasöründe ve devralınan hale **MyBasePage.cs**.</span><span class="sxs-lookup"><span data-stu-id="e050f-257">Open **Browse.cshtml** view from **/Views/Store** folder, and make it inherit from **MyBasePage.cs**.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. <span data-ttu-id="e050f-258">İçinde **Gözat** görüntülemek için bir çağrı ekleyin **MessageService** görüntü ve hizmet tarafından alınan bir ileti görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="e050f-258">In the **Browse** view, add a call to **MessageService** to display an image and a message retrieved by the service.</span></span>
   <span data-ttu-id="e050f-259">(C#)</span><span class="sxs-lookup"><span data-stu-id="e050f-259">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a><span data-ttu-id="e050f-260">Görev 2 - özel bağımlılık çözümleyiciyi ve özel görünüm sayfa Etkinleştiricisini dahil</span><span class="sxs-lookup"><span data-stu-id="e050f-260">Task 2 - Including a Custom Dependency Resolver and a Custom View Page Activator</span></span>

<span data-ttu-id="e050f-261">Önceki görevde bir hizmet çağrısı içindeki gerçekleştirmek için bir görünüm içindeki yeni bir bağımlılık eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="e050f-261">In the previous task, you injected a new dependency inside a view to perform a service call inside it.</span></span> <span data-ttu-id="e050f-262">ASP.NET MVC bağımlılık ekleme arabirimlerini uygulayarak, bağımlılık şimdi çözülecektir **IViewPageActivator** ve **Idependencyresolver**.</span><span class="sxs-lookup"><span data-stu-id="e050f-262">Now, you will resolve that dependency by implementing the ASP.NET MVC Dependency Injection interfaces **IViewPageActivator** and **IDependencyResolver**.</span></span> <span data-ttu-id="e050f-263">Çözümde uygulaması içerecektir **Idependencyresolver** , baş hizmet alma ile Unity kullanarak.</span><span class="sxs-lookup"><span data-stu-id="e050f-263">You will include in the solution an implementation of **IDependencyResolver** that will deal with the service retrieval by using Unity.</span></span> <span data-ttu-id="e050f-264">Ardından, başka bir özel uygulanışı içerecektir **IViewPageActivator** oluşturulması görünüm çözmek için arabirim.</span><span class="sxs-lookup"><span data-stu-id="e050f-264">Then, you will include another custom implementation of **IViewPageActivator** interface that will solve the creation of the views.</span></span>

> [!NOTE]
> <span data-ttu-id="e050f-265">ASP.NET MVC 3'ten sonraki bağımlılık ekleme için uygulama Hizmetleri'ne kaydetme arabirimler Basitleştirilmiş.</span><span class="sxs-lookup"><span data-stu-id="e050f-265">Since ASP.NET MVC 3, the implementation for Dependency Injection had simplified the interfaces to register services.</span></span> <span data-ttu-id="e050f-266">**Idependencyresolver** ve **IViewPageActivator** bağımlılık ekleme için ASP.NET MVC 3 özellikleri parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="e050f-266">**IDependencyResolver** and **IViewPageActivator** are part of ASP.NET MVC 3 features for Dependency Injection.</span></span>
> 
> <span data-ttu-id="e050f-267">**-Idependencyresolver** arabirimi önceki IMvcServiceLocator değiştirir.</span><span class="sxs-lookup"><span data-stu-id="e050f-267">**- IDependencyResolver** interface replaces the previous IMvcServiceLocator.</span></span> <span data-ttu-id="e050f-268">Uygulayıcılar Idependencyresolver, hizmetin veya hizmet koleksiyonu örneği döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="e050f-268">Implementers of IDependencyResolver must return an instance of the service or a service collection.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> <span data-ttu-id="e050f-269">**-IViewPageActivator** arabirimi görünüm sayfalarının bağımlılık ekleme nasıl örneği oluşturulur üzerinde daha ayrıntılı denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="e050f-269">**- IViewPageActivator** interface provides more fine-grained control over how view pages are instantiated via dependency injection.</span></span> <span data-ttu-id="e050f-270">Uygulayan sınıflar **IViewPageActivator** arabirimi örnekleri görüntüle bağlam bilgilerini kullanarak oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-270">The classes that implement **IViewPageActivator** interface can create view instances using context information.</span></span>
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. <span data-ttu-id="e050f-271">Oluşturma /**fabrikaları** projenin kök klasöründe.</span><span class="sxs-lookup"><span data-stu-id="e050f-271">Create the /**Factories** folder in the project's root folder.</span></span>
2. <span data-ttu-id="e050f-272">Dahil **CustomViewPageActivator.cs** çözümünüzden için **/kaynakları/varlıklar/** için **fabrikaları** klasör.</span><span class="sxs-lookup"><span data-stu-id="e050f-272">Include **CustomViewPageActivator.cs** to your solution from **/Sources/Assets/** to **Factories** folder.</span></span> <span data-ttu-id="e050f-273">Bunu yapmak için sağ **/Factories** klasörüne **Ekle | Var olan öğe** seçip **CustomViewPageActivator.cs**.</span><span class="sxs-lookup"><span data-stu-id="e050f-273">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **CustomViewPageActivator.cs**.</span></span> <span data-ttu-id="e050f-274">Bu sınıfın uyguladığı **IViewPageActivator** Unity kapsayıcı tutmak için arabirim.</span><span class="sxs-lookup"><span data-stu-id="e050f-274">This class implements the **IViewPageActivator** interface to hold the Unity Container.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > <span data-ttu-id="e050f-275">**CustomViewPageActivator** Unity kapsayıcısını kullanarak bir görünüm oluşturma yönetmekten sorumludur.</span><span class="sxs-lookup"><span data-stu-id="e050f-275">**CustomViewPageActivator** is responsible for managing the creation of a view by using a Unity container.</span></span>
3. <span data-ttu-id="e050f-276">Dahil **UnityDependencyResolver.cs** dosya **/kaynakları/varlıklar** için **/Factories** klasör.</span><span class="sxs-lookup"><span data-stu-id="e050f-276">Include **UnityDependencyResolver.cs** file from **/Sources/Assets** to **/Factories** folder.</span></span> <span data-ttu-id="e050f-277">Bunu yapmak için sağ **/Factories** klasörüne **Ekle | Var olan öğe** seçip **UnityDependencyResolver.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="e050f-277">To do that, right-click the **/Factories** folder, select **Add | Existing Item** and then select **UnityDependencyResolver.cs** file.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="e050f-278">**UnityDependencyResolver** Unity için özel bir DependencyResolver sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e050f-278">**UnityDependencyResolver** class is a custom DependencyResolver for Unity.</span></span> <span data-ttu-id="e050f-279">Bir hizmet içinde Unity kapsayıcı bulunamıyor, temel çözümleyici invocated.</span><span class="sxs-lookup"><span data-stu-id="e050f-279">When a service cannot be found inside the Unity container, the base resolver is invocated.</span></span>

<span data-ttu-id="e050f-280">Aşağıdaki görev hem de uygulamaları, hizmetleri ve görünümleri konumunu bilmeniz modeli izin vermek için kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="e050f-280">In the following task both implementations will be registered to let the model know the location of the services and the views.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a><span data-ttu-id="e050f-281">Görev 3 - için bağımlılık ekleme kapsayıcısına Unity kaydediliyor</span><span class="sxs-lookup"><span data-stu-id="e050f-281">Task 3 - Registering for Dependency Injection within Unity container</span></span>

<span data-ttu-id="e050f-282">Bu görevde, birlikte çalışma bağımlılık ekleme yapmak için önceki her şeyi koyacaktır.</span><span class="sxs-lookup"><span data-stu-id="e050f-282">In this task, you will put all the previous things together to make Dependency Injection work.</span></span>

<span data-ttu-id="e050f-283">Şimdiye kadar çözümünüzün aşağıdaki öğelere sahiptir:</span><span class="sxs-lookup"><span data-stu-id="e050f-283">Up to now your solution has the following elements:</span></span>

- <span data-ttu-id="e050f-284">A **Gözat** devralınan görünümü **MyBaseClass** ve **MessageService**.</span><span class="sxs-lookup"><span data-stu-id="e050f-284">A **Browse** View that inherits from **MyBaseClass** and consumes **MessageService**.</span></span>
- <span data-ttu-id="e050f-285">Bir ara sınıfı -**MyBaseClass**-bağımlılık ekleme için hizmet arabirimi bildirilen sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e050f-285">An intermediate class -**MyBaseClass**- that has dependency injection declared for the service interface.</span></span>
- <span data-ttu-id="e050f-286">Hizmeti - **MessageService** - ve arabirimiyle **IMessageService**.</span><span class="sxs-lookup"><span data-stu-id="e050f-286">A service - **MessageService** - and its interface **IMessageService**.</span></span>
- <span data-ttu-id="e050f-287">Unity için-özel bağımlılık çözümleyici **UnityDependencyResolver** -hizmet alma ile ilgilidir.</span><span class="sxs-lookup"><span data-stu-id="e050f-287">A custom dependency resolver for Unity - **UnityDependencyResolver** - that deals with service retrieval.</span></span>
- <span data-ttu-id="e050f-288">Bir görünüm sayfa etkinleştiricisini - **CustomViewPageActivator** -sayfası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e050f-288">A View Page activator - **CustomViewPageActivator** - that creates the page.</span></span>

<span data-ttu-id="e050f-289">Eklemesine **Gözat** görünümü artık kayıt özel bağımlılık çözümleyiciyi Unity kapsayıcısında.</span><span class="sxs-lookup"><span data-stu-id="e050f-289">To inject **Browse** View, you will now register the custom dependency resolver in the Unity container.</span></span>

1. <span data-ttu-id="e050f-290">Açık **Bootstrapper.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="e050f-290">Open **Bootstrapper.cs** file.</span></span>
2. <span data-ttu-id="e050f-291">Örneğini kaydetmek **MessageService** hizmeti başlatmak için Unity kapsayıcıya alın:</span><span class="sxs-lookup"><span data-stu-id="e050f-291">Register an instance of **MessageService** into the Unity container to initialize the service:</span></span>

    <span data-ttu-id="e050f-292">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - kayıt iletisi hizmeti*)</span><span class="sxs-lookup"><span data-stu-id="e050f-292">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register Message Service*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. <span data-ttu-id="e050f-293">Bir başvuru ekleyin **MvcMusicStore.Factories** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="e050f-293">Add a reference to **MvcMusicStore.Factories** namespace.</span></span>

    <span data-ttu-id="e050f-294">(Kod parçacığını - *ASP.NET bağımlılık ekleme - Ex02 - Laboratuvar fabrikaları Namespace*)</span><span class="sxs-lookup"><span data-stu-id="e050f-294">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Factories Namespace*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. <span data-ttu-id="e050f-295">Kayıt **CustomViewPageActivator** Unity kapsayıcı içine bir görünüm sayfa etkinleştiricisini olarak:</span><span class="sxs-lookup"><span data-stu-id="e050f-295">Register **CustomViewPageActivator** as a View Page activator into the Unity container:</span></span>

    <span data-ttu-id="e050f-296">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - kayıt CustomViewPageActivator*)</span><span class="sxs-lookup"><span data-stu-id="e050f-296">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Register CustomViewPageActivator*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. <span data-ttu-id="e050f-297">ASP.NET MVC 4 varsayılan bağımlılık çözümleyici örneği ile değiştirin **UnityDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="e050f-297">Replace ASP.NET MVC 4 default dependency resolver with an instance of **UnityDependencyResolver**.</span></span> <span data-ttu-id="e050f-298">Bunu yapmak için değiştirin **Initialise** yöntemini aşağıdaki kodla içerik:</span><span class="sxs-lookup"><span data-stu-id="e050f-298">To do this, replace **Initialise** method content with the following code:</span></span>

    <span data-ttu-id="e050f-299">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex02 - güncelleştirme bağımlılık çözümleyiciyi*)</span><span class="sxs-lookup"><span data-stu-id="e050f-299">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex02 - Update Dependency Resolver*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > <span data-ttu-id="e050f-300">ASP.NET MVC varsayılan bağımlılık çözümleyici sınıfı sağlar.</span><span class="sxs-lookup"><span data-stu-id="e050f-300">ASP.NET MVC provides a default dependency resolver class.</span></span> <span data-ttu-id="e050f-301">Unity için oluşturduk biri olarak özel bağımlılık çözümleyiciler ile çalışmak için bu çözümleyici değiştirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e050f-301">To work with custom dependency resolvers as the one we have created for unity, this resolver has to be replaced.</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="e050f-302">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e050f-302">Task 4 - Running the Application</span></span>

<span data-ttu-id="e050f-303">Bu görevde, Store Tarayıcı hizmeti ve görüntü ve alınan ileti gösterilir doğrulamak amacıyla uygulamanın çalışır:</span><span class="sxs-lookup"><span data-stu-id="e050f-303">In this task, you will run the application to verify that the Store Browser consumes the service and shows the image and the message retrieved:</span></span>

1. <span data-ttu-id="e050f-304">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e050f-304">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e050f-305">Tıklayın **Rock** görmek türler menü ve içinde nasıl **MessageService** Hoş Geldiniz iletisi ve görüntü yüklendi ve görünüm eklendi.</span><span class="sxs-lookup"><span data-stu-id="e050f-305">Click **Rock** within the Genres Menu and see how the **MessageService** was injected to the view and loaded the welcome message and the image.</span></span> <span data-ttu-id="e050f-306">Bu örnekte biz için giriyorsunuz &quot; **Rock**&quot;:</span><span class="sxs-lookup"><span data-stu-id="e050f-306">In this example, we are entering to &quot;**Rock**&quot;:</span></span>

    <span data-ttu-id="e050f-307">![MVC müzik Store - görünümü ekleme](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC müzik Store - görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="e050f-307">![MVC Music Store - View Injection](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store - View Injection")</span></span>

    <span data-ttu-id="e050f-308">*MVC müzik Store - görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="e050f-308">*MVC Music Store - View Injection*</span></span>
3. <span data-ttu-id="e050f-309">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="e050f-309">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a><span data-ttu-id="e050f-310">Alıştırma 3: Eylem filtrelerini ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-310">Exercise 3: Injecting Action Filters</span></span>

<span data-ttu-id="e050f-311">Önceki uygulamalı laboratuvarda **özel eylem filtreleri** filtreleri özelleştirme ve ekleme çalıştı.</span><span class="sxs-lookup"><span data-stu-id="e050f-311">In the previous Hands-On lab **Custom Action Filters** you have worked with filters customization and injection.</span></span> <span data-ttu-id="e050f-312">Bu alıştırmada, Unity kapsayıcısını kullanarak bağımlılık ekleme ile filtre eklemesine öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-312">In this exercise, you will learn how to inject filters with Dependency Injection by using the Unity container.</span></span> <span data-ttu-id="e050f-313">Bunu yapmak için site etkinliklerini izler bir özel eylem filtresi müzik Store çözüme ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-313">To do that, you will add to the Music Store solution a custom action filter that will trace the activity of the site.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a><span data-ttu-id="e050f-314">Görev 1 - izleme filtresi dahil</span><span class="sxs-lookup"><span data-stu-id="e050f-314">Task 1 - Including the Tracking Filter in the Solution</span></span>

<span data-ttu-id="e050f-315">Bu görevde, bir özel eylem filtresi olayları izlemek müzik Store içerecektir.</span><span class="sxs-lookup"><span data-stu-id="e050f-315">In this task, you will include in the Music Store a custom action filter to trace events.</span></span> <span data-ttu-id="e050f-316">Özel eylem filtresi olarak kavramları zaten önceki laboratuvarda değerlendirilir &quot;özel eylem filtreleri&quot;, yalnızca bu Laboratuvar varlıklarını klasöründen filtre sınıfının içerir ve Unity için filtre sağlayıcı oluşturup:</span><span class="sxs-lookup"><span data-stu-id="e050f-316">As custom action filter concepts are already treated in the previous Lab &quot;Custom Action Filters&quot;, you will just include the filter class from the Assets folder of this lab, and then create a Filter Provider for Unity:</span></span>

1. <span data-ttu-id="e050f-317">Açık **başlamak** çözüm bulunan **Source\Ex03 - ekleme eylemi Filter\Begin** klasör.</span><span class="sxs-lookup"><span data-stu-id="e050f-317">Open the **Begin** solution located in the **Source\Ex03 - Injecting Action Filter\Begin** folder.</span></span> <span data-ttu-id="e050f-318">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="e050f-318">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="e050f-319">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="e050f-319">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="e050f-320">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="e050f-320">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="e050f-321">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="e050f-321">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="e050f-322">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="e050f-322">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="e050f-323">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="e050f-323">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="e050f-324">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="e050f-324">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="e050f-325">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="e050f-325">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="e050f-326">Bu makalede daha fazla bilgi için bkz: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="e050f-326">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="e050f-327">Dahil **TraceActionFilter.cs** dosya **/kaynakları/varlıklar** için **/filtreler** klasör.</span><span class="sxs-lookup"><span data-stu-id="e050f-327">Include **TraceActionFilter.cs** file from **/Sources/Assets** to **/Filters** folder.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > <span data-ttu-id="e050f-328">Bu özel eylem filtresi ASP.NET izleme gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="e050f-328">This custom action filter performs ASP.NET tracing.</span></span> <span data-ttu-id="e050f-329">Denetleyebilirsiniz &quot;ASP.NET MVC 4 yerel ve dinamik eylem filtrelerini&quot; Laboratuvar için daha fazla başvuru.</span><span class="sxs-lookup"><span data-stu-id="e050f-329">You can check &quot;ASP.NET MVC 4 local and Dynamic Action Filters&quot; Lab for more reference.</span></span>
3. <span data-ttu-id="e050f-330">Boş sınıf ekleme **FilterProvider.cs** klasöründe projeye   **/filtreler.**</span><span class="sxs-lookup"><span data-stu-id="e050f-330">Add the empty class **FilterProvider.cs** to the project in the folder **/Filters.**</span></span>
4. <span data-ttu-id="e050f-331">Ekleme **System.Web.Mvc** ve **Microsoft.Practices.Unity** ad alanlarında **FilterProvider.cs**.</span><span class="sxs-lookup"><span data-stu-id="e050f-331">Add the **System.Web.Mvc** and **Microsoft.Practices.Unity** namespaces in **FilterProvider.cs**.</span></span>

    <span data-ttu-id="e050f-332">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcı ad alanları ekleme*)</span><span class="sxs-lookup"><span data-stu-id="e050f-332">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. <span data-ttu-id="e050f-333">Devralınan sınıf olun **IFilterProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="e050f-333">Make the class inherit from **IFilterProvider** Interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. <span data-ttu-id="e050f-334">Ekleme bir **IUnityContainer** özelliğinde **FilterProvider** sınıfı ve kapsayıcı atamak için bir sınıf oluşturucusuna oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e050f-334">Add a **IUnityContainer** property in the **FilterProvider** class, and then create a class constructor to assign the container.</span></span>

    <span data-ttu-id="e050f-335">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcısı Oluşturucusu*)</span><span class="sxs-lookup"><span data-stu-id="e050f-335">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider Constructor*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="e050f-336">Filtre sağlayıcısı sınıf oluşturucusu olmayan oluşturma bir **yeni** iç nesne.</span><span class="sxs-lookup"><span data-stu-id="e050f-336">The filter provider class constructor is not creating a **new** object inside.</span></span> <span data-ttu-id="e050f-337">Kapsayıcı, bir parametre olarak geçirilir ve bağımlılık Unity tarafından çözülür.</span><span class="sxs-lookup"><span data-stu-id="e050f-337">The container is passed as a parameter, and the dependency is solved by Unity.</span></span>
7. <span data-ttu-id="e050f-338">İçinde **FilterProvider** yöntemi uygulamak, sınıf **GetFilters** gelen **IFilterProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="e050f-338">In the **FilterProvider** class, implement the method **GetFilters** from **IFilterProvider** interface.</span></span>

    <span data-ttu-id="e050f-339">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - filtre sağlayıcısı GetFilters*)</span><span class="sxs-lookup"><span data-stu-id="e050f-339">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Filter Provider GetFilters*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a><span data-ttu-id="e050f-340">Görev 2 - kaydetme ve filtre etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="e050f-340">Task 2 - Registering and Enabling the Filter</span></span>

<span data-ttu-id="e050f-341">Bu görevde, site izlemeyi etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="e050f-341">In this task, you will enable site tracking.</span></span> <span data-ttu-id="e050f-342">Bunu yapmak için filtreye kaydolacak **Bootstrapper.cs BuildUnityContainer** yöntemi izlemeyi başlatmak için:</span><span class="sxs-lookup"><span data-stu-id="e050f-342">To do that, you will register the filter in **Bootstrapper.cs BuildUnityContainer** method to start tracing:</span></span>

1. <span data-ttu-id="e050f-343">Açık **Web.config** etkinleştir izleme izleme System.Web grubu ve proje kök bulunur.</span><span class="sxs-lookup"><span data-stu-id="e050f-343">Open **Web.config** located in the project root and enable trace tracking at System.Web group.</span></span>

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. <span data-ttu-id="e050f-344">Açık **Bootstrapper.cs** proje kökündeki.</span><span class="sxs-lookup"><span data-stu-id="e050f-344">Open **Bootstrapper.cs** at project root.</span></span>
3. <span data-ttu-id="e050f-345">Bir başvuru ekleyin **MvcMusicStore.Filters** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="e050f-345">Add a reference to the **MvcMusicStore.Filters** namespace.</span></span>

    <span data-ttu-id="e050f-346">(Kod parçacığını - *ASP.NET bağımlılık ekleme - Ex03 - Laboratuvar önyükleyici ad alanlarını ekleme*)</span><span class="sxs-lookup"><span data-stu-id="e050f-346">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Bootstrapper Adding Namespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. <span data-ttu-id="e050f-347">Seçin **BuildUnityContainer** yöntemi ve kayıt Unity kapsayıcısında filtre.</span><span class="sxs-lookup"><span data-stu-id="e050f-347">Select the **BuildUnityContainer** method and register the filter in the Unity Container.</span></span> <span data-ttu-id="e050f-348">Eylem filtresi yanı sıra filtresi sağlayıcıyı kaydetmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="e050f-348">You will have to register the filter provider as well as the action filter.</span></span>

    <span data-ttu-id="e050f-349">(Kod parçacığını - *ASP.NET bağımlılık ekleme Laboratuvar - Ex03 - kayıt FilterProvider ve ActionFilter*)</span><span class="sxs-lookup"><span data-stu-id="e050f-349">(Code Snippet - *ASP.NET Dependency Injection Lab - Ex03 - Register FilterProvider and ActionFilter*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="e050f-350">Görev 3 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e050f-350">Task 3 - Running the Application</span></span>

<span data-ttu-id="e050f-351">Bu görevde, uygulamayı çalıştırmak ve özel eylem filtresi Etkinlik izleme, test edin:</span><span class="sxs-lookup"><span data-stu-id="e050f-351">In this task, you will run the application and test that the custom action filter is tracing the activity:</span></span>

1. <span data-ttu-id="e050f-352">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e050f-352">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="e050f-353">Tıklayın **Rock** içindeki türleri.</span><span class="sxs-lookup"><span data-stu-id="e050f-353">Click **Rock** within the Genres Menu.</span></span> <span data-ttu-id="e050f-354">İsterseniz daha fazla oyun türü için göz atabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-354">You can browse to more genres if you want to.</span></span>

    <span data-ttu-id="e050f-355">![Müzik Store](aspnet-mvc-4-dependency-injection/_static/image11.png "müzik Store")</span><span class="sxs-lookup"><span data-stu-id="e050f-355">![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")</span></span>

    <span data-ttu-id="e050f-356">*Müzik Deposu*</span><span class="sxs-lookup"><span data-stu-id="e050f-356">*Music Store*</span></span>
3. <span data-ttu-id="e050f-357">Gözat **/Trace.axd** uygulama sayfasını ve ardından izlemek için **ayrıntıları**.</span><span class="sxs-lookup"><span data-stu-id="e050f-357">Browse to **/Trace.axd** to see the Application Trace page, and then click **View Details**.</span></span>

    <span data-ttu-id="e050f-358">![Uygulama izleme günlüğü](aspnet-mvc-4-dependency-injection/_static/image12.png "uygulama izleme günlüğü")</span><span class="sxs-lookup"><span data-stu-id="e050f-358">![Application Trace Log](aspnet-mvc-4-dependency-injection/_static/image12.png "Application Trace Log")</span></span>

    <span data-ttu-id="e050f-359">*Uygulama izleme günlüğü*</span><span class="sxs-lookup"><span data-stu-id="e050f-359">*Application Trace Log*</span></span>

    <span data-ttu-id="e050f-360">![Uygulama izleme - istek ayrıntılarını](aspnet-mvc-4-dependency-injection/_static/image13.png "uygulama izleme - istek ayrıntıları")</span><span class="sxs-lookup"><span data-stu-id="e050f-360">![Application Trace - Request Details](aspnet-mvc-4-dependency-injection/_static/image13.png "Application Trace - Request Details")</span></span>

    <span data-ttu-id="e050f-361">*Uygulama izleme - istek ayrıntıları*</span><span class="sxs-lookup"><span data-stu-id="e050f-361">*Application Trace - Request Details*</span></span>
4. <span data-ttu-id="e050f-362">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="e050f-362">Close the browser.</span></span>

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e050f-363">Özet</span><span class="sxs-lookup"><span data-stu-id="e050f-363">Summary</span></span>

<span data-ttu-id="e050f-364">Bu uygulamalı laboratuvarı tamamlayarak bağımlılık ekleme ASP.NET MVC 4'te bir NuGet paketi kullanarak Unity tümleştirerek kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-364">By completing this Hands-On Lab you have learned how to use Dependency Injection in ASP.NET MVC 4 by integrating Unity using a NuGet Package.</span></span> <span data-ttu-id="e050f-365">Bunu başarmak için bağımlılık ekleme denetleyicileri, görünümler ve eylem filtrelerini içinde kullandınız.</span><span class="sxs-lookup"><span data-stu-id="e050f-365">To achieve that, you have used Dependency Injection inside controllers, views and action filters.</span></span>

<span data-ttu-id="e050f-366">Aşağıdaki kavramlar kapsanan:</span><span class="sxs-lookup"><span data-stu-id="e050f-366">The following concepts were covered:</span></span>

- <span data-ttu-id="e050f-367">ASP.NET MVC 4 bağımlılık ekleme özellikleri</span><span class="sxs-lookup"><span data-stu-id="e050f-367">ASP.NET MVC 4 Dependency Injection features</span></span>
- <span data-ttu-id="e050f-368">Unity.Mvc3 NuGet paketi kullanarak unity tümleştirmesi</span><span class="sxs-lookup"><span data-stu-id="e050f-368">Unity integration using Unity.Mvc3 NuGet Package</span></span>
- <span data-ttu-id="e050f-369">Denetleyicileri bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-369">Dependency Injection in Controllers</span></span>
- <span data-ttu-id="e050f-370">Görünümlere bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-370">Dependency Injection in Views</span></span>
- <span data-ttu-id="e050f-371">Eylem filtreleri bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="e050f-371">Dependency injection of Action Filters</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="e050f-372">Ek A: Visual Studio'yu Web için Express 2012</span><span class="sxs-lookup"><span data-stu-id="e050f-372">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="e050f-373">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="e050f-373">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="e050f-374">Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="e050f-374">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="e050f-375">Git [ [ https://go.microsoft.com/? LinkId 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="e050f-375">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="e050f-376">Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="e050f-376">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="e050f-377">Tıklayarak **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="e050f-377">Click on **Install Now**.</span></span> <span data-ttu-id="e050f-378">Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e050f-378">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="e050f-379">Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="e050f-379">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="e050f-380">![Visual Studio Express yükleyin](aspnet-mvc-4-dependency-injection/_static/image14.png "Visual Studio Express'i yükle")</span><span class="sxs-lookup"><span data-stu-id="e050f-380">![Install Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="e050f-381">*Visual Studio Express yükleyin*</span><span class="sxs-lookup"><span data-stu-id="e050f-381">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="e050f-382">Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="e050f-382">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-dependency-injection/_static/image15.png)

    <span data-ttu-id="e050f-384">*Lisans koşullarını kabul etme*</span><span class="sxs-lookup"><span data-stu-id="e050f-384">*Accepting the license terms*</span></span>
5. <span data-ttu-id="e050f-385">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="e050f-385">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-dependency-injection/_static/image16.png)

    <span data-ttu-id="e050f-387">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="e050f-387">*Installation progress*</span></span>
6. <span data-ttu-id="e050f-388">Yükleme tamamlandığında, tıklayın **son**.</span><span class="sxs-lookup"><span data-stu-id="e050f-388">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-dependency-injection/_static/image17.png)

    <span data-ttu-id="e050f-390">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="e050f-390">*Installation completed*</span></span>
7. <span data-ttu-id="e050f-391">Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="e050f-391">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="e050f-392">Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.</span><span class="sxs-lookup"><span data-stu-id="e050f-392">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Web kutucuğu için VS Express](aspnet-mvc-4-dependency-injection/_static/image18.png)

    <span data-ttu-id="e050f-394">*Web kutucuğu için VS Express*</span><span class="sxs-lookup"><span data-stu-id="e050f-394">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="e050f-395">Ek B: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="e050f-395">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="e050f-396">Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır.</span><span class="sxs-lookup"><span data-stu-id="e050f-396">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="e050f-397">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="e050f-397">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="e050f-398">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-dependency-injection/_static/image19.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="e050f-398">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-dependency-injection/_static/image19.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="e050f-399">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="e050f-399">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="e050f-400">***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="e050f-400">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="e050f-401">Kod eklemesini istediğiniz imleci yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="e050f-401">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="e050f-402">(Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="e050f-402">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="e050f-403">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="e050f-403">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="e050f-404">Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="e050f-404">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="e050f-405">İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="e050f-405">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="e050f-406">![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-dependency-injection/_static/image20.png "kod parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="e050f-406">![Start typing the snippet name](aspnet-mvc-4-dependency-injection/_static/image20.png "Start typing the snippet name")</span></span>

<span data-ttu-id="e050f-407">*Kod parçacığı adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="e050f-407">*Start typing the snippet name*</span></span>

<span data-ttu-id="e050f-408">![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-dependency-injection/_static/image21.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="e050f-408">![Press Tab to select the highlighted snippet](aspnet-mvc-4-dependency-injection/_static/image21.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="e050f-409">*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="e050f-409">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="e050f-410">![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-dependency-injection/_static/image22.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")</span><span class="sxs-lookup"><span data-stu-id="e050f-410">![Press Tab again and the snippet will expand](aspnet-mvc-4-dependency-injection/_static/image22.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="e050f-411">*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*</span><span class="sxs-lookup"><span data-stu-id="e050f-411">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="e050f-412">***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="e050f-412">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="e050f-413">Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e050f-413">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="e050f-414">Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.</span><span class="sxs-lookup"><span data-stu-id="e050f-414">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="e050f-415">Tıklayarak ilgili kod parçacığı listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="e050f-415">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="e050f-416">![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-dependency-injection/_static/image23.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")</span><span class="sxs-lookup"><span data-stu-id="e050f-416">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-dependency-injection/_static/image23.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="e050f-417">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*</span><span class="sxs-lookup"><span data-stu-id="e050f-417">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="e050f-418">![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-dependency-injection/_static/image24.png "tıklayarak ilgili kod parçacığı listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="e050f-418">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-dependency-injection/_static/image24.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="e050f-419">*Tıklayarak ilgili kod parçacığı listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="e050f-419">*Pick the relevant snippet from the list, by clicking on it*</span></span>
