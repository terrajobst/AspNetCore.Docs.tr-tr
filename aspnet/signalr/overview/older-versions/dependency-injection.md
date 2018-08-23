---
uid: signalr/overview/older-versions/dependency-injection
title: Signalr'da bağımlılık ekleme 1.x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: d59ca85f1005b08ff52ded61d94323dabdb40d0a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752159"
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="d48f6-102">Signalr'da bağımlılık ekleme 1.x</span><span class="sxs-lookup"><span data-stu-id="d48f6-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="d48f6-103">tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d48f6-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="d48f6-104">Bağımlılık ekleme (sahte nesneler kullanılarak) test etmek için ya da bir nesnenin bağımlılıkları değiştirin veya çalışma zamanı davranışını değiştirmek için daha kolay hale getirme nesneleri arasındaki sabit kodlu bağımlılıkları kaldırmak için bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="d48f6-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="d48f6-105">Bu öğreticide SignalR hub'larında bağımlılık ekleme yapmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="d48f6-106">Ayrıca SignalR ile IOC kapsayıcıları kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="d48f6-107">IOC kapsayıcı için bağımlılık ekleme genel bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="d48f6-108">Bağımlılık ekleme nedir?</span><span class="sxs-lookup"><span data-stu-id="d48f6-108">What is Dependency Injection?</span></span>

<span data-ttu-id="d48f6-109">Zaten bağımlılık ekleme hakkında bilginiz varsa, bu bölümü atlayın.</span><span class="sxs-lookup"><span data-stu-id="d48f6-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="d48f6-110">*Bağımlılık ekleme* (dı) olan bir desen burada nesneler kendi bağımlılıklar oluşturmaktan sorumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="d48f6-111">DI becerisiyle basit bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="d48f6-112">İletilerini günlüğe kaydetmek için gereken bir nesne olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="d48f6-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="d48f6-113">Günlüğe kaydetme arabirimi tanımlayabilir:</span><span class="sxs-lookup"><span data-stu-id="d48f6-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="d48f6-114">Nesnesi içinde oluşturduğunuz bir `ILogger` iletilerini günlüğe kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="d48f6-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="d48f6-115">Bu çalışır, ancak en iyi tasarım değil.</span><span class="sxs-lookup"><span data-stu-id="d48f6-115">This works, but it's not the best design.</span></span> <span data-ttu-id="d48f6-116">Değiştirmek istiyorsanız `FileLogger` diğeriyle `ILogger` uygulama, değiştirme gerekir `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="d48f6-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="d48f6-117">Diğer birçok kullanım nesnelerini kabul ederek şehirler `FileLogger`, bunların tümünün değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="d48f6-118">Veya yapmaya karar verirseniz `FileLogger` tek Ayrıca uygulama boyunca değişiklik yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="d48f6-119">Daha iyi bir yaklaşım "" bulunmaktır bir `ILogger` nesnesine — Örneğin, bir oluşturucu bağımsız değişkenini kullanarak tarafından:</span><span class="sxs-lookup"><span data-stu-id="d48f6-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="d48f6-120">Nesne seçmek için sorumlu değildir. Şimdi `ILogger` kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="d48f6-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="d48f6-121">Swich yapabilecekleriniz `ILogger` bağımlı nesneleri değiştirmeden uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="d48f6-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="d48f6-122">Bu düzeni olarak adlandırılır [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="d48f6-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="d48f6-123">Başka bir ayarlayıcı eklemesi, ayarlayıcı yöntemi veya özelliği aracılığıyla bağımlılık ayarladığınız modelidir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="d48f6-124">Signalr'da basit bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="d48f6-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="d48f6-125">Sohbet uygulaması Öğreticisi göz önünde bulundurun [SignalR ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="d48f6-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="d48f6-126">Bu uygulama hub sınıfı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="d48f6-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="d48f6-127">Sohbet iletileri göndermeden önce sunucuda depolamak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="d48f6-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="d48f6-128">Bu işlev soyutlayan bir arabirim tanımlayın ve arabirimine eklemesine DI kullanmak `ChatHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d48f6-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="d48f6-129">Bir SignalR uygulama doğrudan hub'ları oluşturmaz yalnızca sorunudur; SignalR bunları sizin için oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d48f6-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="d48f6-130">Varsayılan olarak, bir hub sınıfı, parametresiz bir oluşturucu sağlamak için SignalR bekliyor.</span><span class="sxs-lookup"><span data-stu-id="d48f6-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="d48f6-131">Ancak, kolayca hub örnekleri oluşturmak için bir işlev kaydetmek ve DI gerçekleştirmek için bu işlevi kullanın.</span><span class="sxs-lookup"><span data-stu-id="d48f6-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="d48f6-132">İşlevini çağırarak kaydetmek **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="d48f6-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="d48f6-133">SignalR oluşturmanız gerektiğinde bu anonim işlev çağıracağı artık bir `ChatHub` örneği.</span><span class="sxs-lookup"><span data-stu-id="d48f6-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="d48f6-134">IOC kapsayıcıları</span><span class="sxs-lookup"><span data-stu-id="d48f6-134">IoC Containers</span></span>

<span data-ttu-id="d48f6-135">Önceki kod, basit durumlar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="d48f6-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="d48f6-136">Ancak yine de bu yazmanız gerekirdi:</span><span class="sxs-lookup"><span data-stu-id="d48f6-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="d48f6-137">Birçok bağımlılıkları olan karmaşık bir uygulamada bu "bağlantı" kod yazmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="d48f6-138">Bu kod, özellikle bağımlılıkları iç içe geçmişse sağlamak zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="d48f6-139">Ayrıca, birim testi de zordur.</span><span class="sxs-lookup"><span data-stu-id="d48f6-139">It is also hard to unit test.</span></span>

<span data-ttu-id="d48f6-140">Tek bir çözüm IOC kapsayıcı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="d48f6-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="d48f6-141">IOC kapsayıcı bağımlılıkları yönetmekten sorumlu olan bir yazılım bileşenidir. Türleri ile kapsayıcı kayıt ve sonra da kapsayıcı nesneleri oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="d48f6-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="d48f6-142">Kapsayıcı bağımlılık ilişkileri otomatik olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="d48f6-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="d48f6-143">Çok sayıda IOC kapsayıcı nesne yaşam süresi ve kapsamı gibi denetlemenize izin.</span><span class="sxs-lookup"><span data-stu-id="d48f6-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="d48f6-144">"IoC" anlamına gelir "tersine çevirme denetimi için", burada bir çerçeve koduna çağrı genel düzen olduğu.</span><span class="sxs-lookup"><span data-stu-id="d48f6-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="d48f6-145">IOC kapsayıcı nesnelerinizi sizin için "Normal denetim akışını tersine çevirir" oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d48f6-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="d48f6-146">SignalR öğesinde IOC kapsayıcıları kullanma</span><span class="sxs-lookup"><span data-stu-id="d48f6-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="d48f6-147">Sohbet uygulaması büyük olasılıkla bir IOC kapsayıcısından yararlanmak basit bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="d48f6-148">Bunun yerine, göz atalım [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) örnek.</span><span class="sxs-lookup"><span data-stu-id="d48f6-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="d48f6-149">StockTicker örnek iki ana sınıf tanımlar:</span><span class="sxs-lookup"><span data-stu-id="d48f6-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="d48f6-150">`StockTickerHub`: İstemci bağlantıları yönetir hub sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d48f6-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="d48f6-151">`StockTicker`: Hisse senedi fiyatlarına tutar ve bunları düzenli aralıklarla güncelleştiren bir tekli.</span><span class="sxs-lookup"><span data-stu-id="d48f6-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="d48f6-152">`StockTickerHub` bir başvuru tutan `StockTicker` tekil, ancak `StockTicker` bir başvuru tutan **IHubConnectionContext** için `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="d48f6-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="d48f6-153">Bu arabirim ile iletişim kurmak için kullandığı `StockTickerHub` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="d48f6-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="d48f6-154">(Daha fazla bilgi için [ASP.NET SignalR ile sunucu yayını](index.md).)</span><span class="sxs-lookup"><span data-stu-id="d48f6-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="d48f6-155">Bu bağımlılıklar biraz ayrıştırmaya IOC kapsayıcı kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="d48f6-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="d48f6-156">İlk olarak, şimdi basitleştirmek `StockTickerHub` ve `StockTicker` sınıfları.</span><span class="sxs-lookup"><span data-stu-id="d48f6-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="d48f6-157">Aşağıdaki kodda, ihtiyaç duymayacağımız olduğunu miyim bölümlerini açıklamalı.</span><span class="sxs-lookup"><span data-stu-id="d48f6-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="d48f6-158">Parametresiz oluşturucusu öğesinden kaldırmak `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="d48f6-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="d48f6-159">Bunun yerine, her zaman DI hub'ı oluşturmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="d48f6-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="d48f6-160">StockTicker için tekil örneğini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="d48f6-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="d48f6-161">Daha sonra IOC StockTicker ömrünü denetlemek için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="d48f6-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="d48f6-162">Ayrıca, ortak oluşturucu olun.</span><span class="sxs-lookup"><span data-stu-id="d48f6-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="d48f6-163">Ardından, biz kodu için bir arabirim oluşturarak yeniden düzenleyebilirsiniz `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="d48f6-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="d48f6-164">Bu arabirim ayrıştırmak için kullanacağız `StockTickerHub` gelen `StockTicker` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d48f6-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="d48f6-165">Visual Studio bu kolay tür yeniden düzenleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="d48f6-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="d48f6-166">StockTicker.cs dosyasını açın, sağ `StockTicker` sınıf bildiriminin ve seçin **yeniden düzenleme** ... **Ayıklama arabirimi**.</span><span class="sxs-lookup"><span data-stu-id="d48f6-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="d48f6-167">İçinde **Arabirimi Ayıkla** iletişim kutusunda, tıklayın **Tümünü Seç**.</span><span class="sxs-lookup"><span data-stu-id="d48f6-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="d48f6-168">Diğer varsayılan değerleri bırakın.</span><span class="sxs-lookup"><span data-stu-id="d48f6-168">Leave the other defaults.</span></span> <span data-ttu-id="d48f6-169">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="d48f6-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="d48f6-170">Visual Studio adlı yeni bir arabirim oluşturur `IStockTicker`ve ayrıca değiştirir `StockTicker` türetmek için `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="d48f6-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="d48f6-171">IStockTicker.cs dosyasını açın ve değiştirmek için arabirimi **genel**.</span><span class="sxs-lookup"><span data-stu-id="d48f6-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="d48f6-172">İçinde `StockTickerHub` sınıfı, iki örneğinden birini değiştirmek `StockTicker` için `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="d48f6-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="d48f6-173">Oluşturma bir `IStockTicker` arabirimi kesinlikle gerekli değildir, ancak DI uygulamanızın bileşenleri arasında eşleştirme yapmaktan azaltmak için nasıl yardımcı olabileceğini gösteren istedi.</span><span class="sxs-lookup"><span data-stu-id="d48f6-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="d48f6-174">Ninject kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="d48f6-174">Add the Ninject Library</span></span>

<span data-ttu-id="d48f6-175">.NET için çok sayıda açık kaynaklı IOC kapsayıcı vardır.</span><span class="sxs-lookup"><span data-stu-id="d48f6-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="d48f6-176">Bu öğretici için kullanacağım [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="d48f6-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="d48f6-177">(Diğer popüler kitaplıkları içerir [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), ve [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="d48f6-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="d48f6-178">Yüklemek için NuGet Paket Yöneticisi'ni kullanın [Ninject Kitaplığı](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="d48f6-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="d48f6-179">Visual Studio'da gelen **Araçları** menüsünü seçin **kitaplık Paket Yöneticisi** | **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="d48f6-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="d48f6-180">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="d48f6-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="d48f6-181">SignalR bağımlılık çözümleyiciyi değiştirin</span><span class="sxs-lookup"><span data-stu-id="d48f6-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="d48f6-182">SignalR içinde Ninject kullanmak için türetilen bir sınıf oluşturma **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="d48f6-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="d48f6-183">Bu sınıf geçersiz kılmalar **GetService** ve **GetServices** yöntemlerinin **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="d48f6-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="d48f6-184">SignalR hub örneklerinin yanı sıra, SignalR tarafından dahili olarak kullanılan çeşitli Hizmetleri dahil olmak üzere çalışma zamanında, çeşitli nesneleri oluşturmak için bu yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="d48f6-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="d48f6-185">**GetService** yöntemi, bir türün tek bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d48f6-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="d48f6-186">Bu yöntemin Ninject çekirdek 's çağırmaya **TryGet** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="d48f6-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="d48f6-187">Bu yöntem null değeri döndürür, varsayılan çözümleyici için geri döner.</span><span class="sxs-lookup"><span data-stu-id="d48f6-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="d48f6-188">**GetServices** yöntemi, belirtilen bir türün nesnelerinin bir koleksiyonunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d48f6-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="d48f6-189">Ninject sonuçlardan varsayılan çözümleyici sonuçları ile birleştirmek için bu yöntemi yok sayın.</span><span class="sxs-lookup"><span data-stu-id="d48f6-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="d48f6-190">Ninject bağlamaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="d48f6-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="d48f6-191">Artık türü bağlamaları bildirmek için Ninject kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="d48f6-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="d48f6-192">RegisterHubs.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="d48f6-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="d48f6-193">İçinde `RegisterHubs.Start` yöntemi çağıran Ninject Ninject kapsayıcı oluşturma *çekirdek*.</span><span class="sxs-lookup"><span data-stu-id="d48f6-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="d48f6-194">Bizim özel bağımlılık Çözümleyicisi örneği oluşturun:</span><span class="sxs-lookup"><span data-stu-id="d48f6-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="d48f6-195">İçin bir bağlama oluşturun `IStockTicker` gibi:</span><span class="sxs-lookup"><span data-stu-id="d48f6-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="d48f6-196">Bu kod, iki şey diyor.</span><span class="sxs-lookup"><span data-stu-id="d48f6-196">This code is saying two things.</span></span> <span data-ttu-id="d48f6-197">İlki, uygulamayı gerektiğinde bir `IStockTicker`, çekirdek örneğini oluşturmalısınız `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="d48f6-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="d48f6-198">İkinci olarak, `StockTicker` sınıfı tekil nesnesi olarak oluşturulmuş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d48f6-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="d48f6-199">Ninject nesnesinin bir kopyasını oluşturmak ve her istek için aynı örnek döndürülecektir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="d48f6-200">İçin bir bağlama oluşturun **IHubConnectionContext** gibi:</span><span class="sxs-lookup"><span data-stu-id="d48f6-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="d48f6-201">Bu kod creatres döndüren bir anonim işlev bir **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="d48f6-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="d48f6-202">**WhenInjectedInto** yöntemi yalnızca oluştururken bu işlevi kullanmak için Ninject söyler `IStockTicker` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="d48f6-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="d48f6-203">SignalR oluşturduğu nedeni **IHubConnectionContext** dahili olarak, örnekler ve SignalR bunları nasıl oluşturduğunu geçersiz kılmak istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="d48f6-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="d48f6-204">Bu işlevi yalnızca uygular bizim `StockTicker` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="d48f6-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="d48f6-205">Bağımlılık çözümleyiciyi içine geçirmek **MapHubs** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="d48f6-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="d48f6-206">SignalR belirtilen çözümleyici kullanacağı artık **MapHubs**, yerine varsayılan çözümleyici.</span><span class="sxs-lookup"><span data-stu-id="d48f6-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="d48f6-207">İçin tam kodu işte `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="d48f6-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="d48f6-208">Visual Studio'da StockTicker uygulamayı çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="d48f6-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="d48f6-209">Tarayıcı penceresinde gidin `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="d48f6-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="d48f6-210">Uygulamayı önce olarak tam olarak aynı işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="d48f6-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="d48f6-211">(Bir açıklaması için bkz. [ASP.NET SignalR ile sunucu yayını](index.md).) Davranışı değiştirdik henüz; yalnızca kod sınamak için korumak ve geliştirmek daha kolay.</span><span class="sxs-lookup"><span data-stu-id="d48f6-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
