---
uid: signalr/overview/older-versions/dependency-injection
title: "SignalR öğesindeki bağımlılık ekleme 1.x | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr-1x"></a><span data-ttu-id="bae94-102">SignalR öğesindeki bağımlılık ekleme 1.x</span><span class="sxs-lookup"><span data-stu-id="bae94-102">Dependency Injection in SignalR 1.x</span></span>
====================
<span data-ttu-id="bae94-103">tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="bae94-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

<span data-ttu-id="bae94-104">Bağımlılık ekleme, nesneler, nesnenin bağımlılıkları, (sahte nesneler kullanılarak) sınama ya da değiştirmek için ya da çalışma zamanı davranışını değiştirmek için daha kolay hale arasındaki sabit kodlu bağımlılıklar kaldırmak için bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="bae94-104">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="bae94-105">Bu öğretici SignalR hub'larında bağımlılık ekleme gerçekleştirme gösterir.</span><span class="sxs-lookup"><span data-stu-id="bae94-105">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="bae94-106">Ayrıca SignalR ile IOC kapsayıcıları kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="bae94-106">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="bae94-107">IOC kapsayıcı bağımlılık ekleme için genel bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="bae94-107">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="bae94-108">Bağımlılık ekleme nedir?</span><span class="sxs-lookup"><span data-stu-id="bae94-108">What is Dependency Injection?</span></span>

<span data-ttu-id="bae94-109">Bağımlılık ekleme ile bilginiz varsa bu bölüm atlayın.</span><span class="sxs-lookup"><span data-stu-id="bae94-109">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="bae94-110">*Bağımlılık ekleme* (dı) olan bir desen burada nesneler kendi bağımlılıkları oluşturmaktan sorumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="bae94-110">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="bae94-111">DI becerisiyle basit bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bae94-111">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="bae94-112">İletilerini günlüğe kaydetmek için gereken bir nesne olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="bae94-112">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="bae94-113">Günlük arabirim tanımlayabilir:</span><span class="sxs-lookup"><span data-stu-id="bae94-113">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="bae94-114">Nesnenizin içinde oluşturduğunuz bir `ILogger` iletilerini günlüğe kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="bae94-114">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="bae94-115">Bu çalışır, ancak en iyi tasarım değil.</span><span class="sxs-lookup"><span data-stu-id="bae94-115">This works, but it's not the best design.</span></span> <span data-ttu-id="bae94-116">Değiştirmek istiyorsanız `FileLogger` diğeriyle `ILogger` uygulaması, olacaktır değiştirmek `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="bae94-116">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="bae94-117">Diğer birçok kullanım nesneleri kabul `FileLogger`, bunların tümünün değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="bae94-117">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="bae94-118">Veya yapmaya karar verirseniz `FileLogger` bir singleton Ayrıca uygulama boyunca değişiklik sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bae94-118">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="bae94-119">Daha iyi bir yaklaşım "eklemesine" olan bir `ILogger` nesnesine — oluşturucu bağımsız değişkeni kullanarak örneğin:</span><span class="sxs-lookup"><span data-stu-id="bae94-119">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="bae94-120">Nesne seçmek için sorumlu değildir artık `ILogger` kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="bae94-120">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="bae94-121">Swich yapabilecekleriniz `ILogger` bağımlı nesneleri değiştirmeden uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="bae94-121">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="bae94-122">Bu desen adlı [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="bae94-122">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="bae94-123">Başka bir düzen ayarlayıcı yöntemi veya özelliği aracılığıyla bağımlılık ayarladığınız ayarlayıcı ekleme yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="bae94-123">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="bae94-124">SignalR öğesindeki basit bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="bae94-124">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="bae94-125">Öğretici sohbet uygulamadan göz önünde bulundurun [SignalR ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="bae94-125">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="bae94-126">Bu uygulama hub sınıfından şöyledir:</span><span class="sxs-lookup"><span data-stu-id="bae94-126">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="bae94-127">Sohbet iletileri göndermeden önce sunucuda saklamak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="bae94-127">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="bae94-128">Bu işlev soyutlar bir arabirim tanımlayın ve arabirimine eklemesine dı kullanmak `ChatHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bae94-128">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="bae94-129">Bir SignalR uygulaması doğrudan hub'ları oluşturmaz yalnızca sorunudur; SignalR bunları sizin için oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bae94-129">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="bae94-130">Varsayılan olarak, bir parametresiz oluşturucuya sahip olması için bir hub sınıf SignalR bekliyor.</span><span class="sxs-lookup"><span data-stu-id="bae94-130">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="bae94-131">Ancak, kolayca hub örnekleri oluşturmak için bir işlev kaydetmek ve DI gerçekleştirmek için bu işlevi kullanın.</span><span class="sxs-lookup"><span data-stu-id="bae94-131">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="bae94-132">İşlevini çağırarak kaydetmek **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="bae94-132">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="bae94-133">Oluşturması için gereken her SignalR bu anonim işlevi çağırma artık bir `ChatHub` örneği.</span><span class="sxs-lookup"><span data-stu-id="bae94-133">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="bae94-134">IOC kapsayıcıları</span><span class="sxs-lookup"><span data-stu-id="bae94-134">IoC Containers</span></span>

<span data-ttu-id="bae94-135">Önceki kod basit durumlar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="bae94-135">The previous code is fine for simple cases.</span></span> <span data-ttu-id="bae94-136">Ancak hala bu yazmak zorunda kaldı:</span><span class="sxs-lookup"><span data-stu-id="bae94-136">But you still had to write this:</span></span>

[!code-css[Main](dependency-injection/samples/sample8.css)]

<span data-ttu-id="bae94-137">Birçok bağımlılıkları olan karmaşık bir uygulamada bu "kablolama" kod çok yazma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="bae94-137">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="bae94-138">Bu kod, özellikle bağımlılıkları iç içe geçmişse korumak zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="bae94-138">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="bae94-139">Ayrıca, birim testi için de zordur.</span><span class="sxs-lookup"><span data-stu-id="bae94-139">It is also hard to unit test.</span></span>

<span data-ttu-id="bae94-140">Bir çözüm IOC kapsayıcı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="bae94-140">One solution is to use an IoC container.</span></span> <span data-ttu-id="bae94-141">Bağımlılıklar yönetmekten sorumlu bir yazılım bileşeni bir IOC kapsayıcıdır. Kapsayıcıyla türleri kaydetmek ve nesneleri oluşturmak için kapsayıcı kullanın.</span><span class="sxs-lookup"><span data-stu-id="bae94-141">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="bae94-142">Kapsayıcı otomatik olarak bağımlılık ilişkileri rakamlar.</span><span class="sxs-lookup"><span data-stu-id="bae94-142">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="bae94-143">Birçok IOC kapsayıcıları nesne ömrü ve kapsam gibi denetlemenize izin.</span><span class="sxs-lookup"><span data-stu-id="bae94-143">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="bae94-144">"IoC" anlamına gelir "tersine çevirme denetimi için", olduğu genel bir desen olduğu bir çerçeve uygulama kodu çağırır.</span><span class="sxs-lookup"><span data-stu-id="bae94-144">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="bae94-145">IOC kapsayıcı nesnelerinizi sizin için hangi "Normal akış denetimi tersine çevirir" oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bae94-145">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="bae94-146">SignalR öğesinde IOC kapsayıcıları kullanma</span><span class="sxs-lookup"><span data-stu-id="bae94-146">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="bae94-147">Sohbet uygulaması büyük olasılıkla bir IOC kapsayıcıdan yararlanmak basit bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="bae94-147">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="bae94-148">Bunun yerine, bakalım [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) örnek.</span><span class="sxs-lookup"><span data-stu-id="bae94-148">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="bae94-149">StockTicker örnek iki ana sınıf tanımlar:</span><span class="sxs-lookup"><span data-stu-id="bae94-149">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="bae94-150">`StockTickerHub`: İstemci bağlantıları yönetir hub sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bae94-150">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="bae94-151">`StockTicker`: Hisse senedi fiyatları tutar ve bunları düzenli aralıklarla güncelleştirir bir singleton.</span><span class="sxs-lookup"><span data-stu-id="bae94-151">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="bae94-152">`StockTickerHub`bir başvuru tutan `StockTicker` tek sırada `StockTicker` başvuru tutan **IHubConnectionContext** için `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="bae94-152">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="bae94-153">Bu arabirim ile iletişim kurmak için kullandığı `StockTickerHub` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="bae94-153">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="bae94-154">(Daha fazla bilgi için bkz: [Server yayını ASP.NET SignalR ile](index.md).)</span><span class="sxs-lookup"><span data-stu-id="bae94-154">(For more information, see [Server Broadcast with ASP.NET SignalR](index.md).)</span></span>

<span data-ttu-id="bae94-155">Bu bağımlılıklar biraz untangle için size bir IOC kapsayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bae94-155">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="bae94-156">İlk olarak, şirketinizdeki basitleştirmek `StockTickerHub` ve `StockTicker` sınıfları.</span><span class="sxs-lookup"><span data-stu-id="bae94-156">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="bae94-157">Aşağıdaki kodda gerekmez, ı bölümleri açıklamalı.</span><span class="sxs-lookup"><span data-stu-id="bae94-157">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="bae94-158">Parametresiz bir kurucusu öğesinden Kaldır `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="bae94-158">Remove the parameterless constructor from `StockTicker`.</span></span> <span data-ttu-id="bae94-159">Bunun yerine, her zaman dı hub oluşturmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="bae94-159">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="bae94-160">StockTicker için tek örnek kaldırın.</span><span class="sxs-lookup"><span data-stu-id="bae94-160">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="bae94-161">IOC kapsayıcı StockTicker ömrünü denetlemek için daha sonra kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="bae94-161">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="bae94-162">Ayrıca, Oluşturucusu ortak olun.</span><span class="sxs-lookup"><span data-stu-id="bae94-162">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="bae94-163">Ardından, biz kodu için bir arabirimi oluşturarak yeniden düzenlemeniz `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="bae94-163">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="bae94-164">Bu arabirim ile aynı şekilde kullanacağız `StockTickerHub` gelen `StockTicker` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bae94-164">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="bae94-165">Visual Studio bu kolay tür düzenleme yapar.</span><span class="sxs-lookup"><span data-stu-id="bae94-165">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="bae94-166">StockTicker.cs dosyasını açın, sağ tıklayın `StockTicker` sınıf bildiriminin ve seçin **yeniden düzenlemeniz** ... **Ayıklama arabirimi**.</span><span class="sxs-lookup"><span data-stu-id="bae94-166">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="bae94-167">İçinde **arayüz** iletişim kutusunda, tıklatın **Tümünü Seç**.</span><span class="sxs-lookup"><span data-stu-id="bae94-167">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="bae94-168">Diğer Varsayılanları bırakın.</span><span class="sxs-lookup"><span data-stu-id="bae94-168">Leave the other defaults.</span></span> <span data-ttu-id="bae94-169">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="bae94-169">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="bae94-170">Visual Studio adlı yeni bir arabirim oluşturur `IStockTicker`ve ayrıca değiştirir `StockTicker` türetmek `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="bae94-170">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="bae94-171">IStockTicker.cs dosyasını açın ve arabirimine değiştirme **ortak**.</span><span class="sxs-lookup"><span data-stu-id="bae94-171">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="bae94-172">İçinde `StockTickerHub` sınıfı, iki örneğini değiştirmek `StockTicker` için `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="bae94-172">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="bae94-173">Oluşturma bir `IStockTicker` arabirimi kesinlikle gerekli değildir, ancak dı uygulamanızda bileşenleri arasındaki ilişki azaltmak için nasıl yardımcı olabileceğini gösteren istedik.</span><span class="sxs-lookup"><span data-stu-id="bae94-173">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="bae94-174">Ninject kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="bae94-174">Add the Ninject Library</span></span>

<span data-ttu-id="bae94-175">.NET için birçok açık kaynak IOC kapsayıcı vardır.</span><span class="sxs-lookup"><span data-stu-id="bae94-175">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="bae94-176">Bu öğretici için kullanmam [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="bae94-176">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="bae94-177">(Diğer popüler kitaplıkları dahil [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), ve [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="bae94-177">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="bae94-178">Yüklemek için NuGet Paket Yöneticisi'ni kullanın [Ninject Kitaplığı](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="bae94-178">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="bae94-179">Visual Studio'da gelen **Araçları** menüsünü seçin **kitaplık Paket Yöneticisi** | **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="bae94-179">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="bae94-180">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="bae94-180">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="bae94-181">SignalR bağımlılık çözümleyiciyi değiştirin</span><span class="sxs-lookup"><span data-stu-id="bae94-181">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="bae94-182">SignalR içinde Ninject kullanmak için türeyen bir sınıf oluşturun **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="bae94-182">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="bae94-183">Bu sınıf geçersiz kılmaları **GetService** ve **GetServices** yöntemlerinin **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="bae94-183">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="bae94-184">SignalR, SignalR tarafından dahili olarak kullanılan çeşitli hizmetlerin yanı sıra, hub örnekleri dahil olmak üzere çalışma zamanında çeşitli nesneleri oluşturmak için bu yöntem çağırır.</span><span class="sxs-lookup"><span data-stu-id="bae94-184">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="bae94-185">**GetService** yöntemi, bir türü tek bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bae94-185">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="bae94-186">Ninject çekirdek 's çağırmak için bu yöntemi geçersiz kılın **TryGet** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bae94-186">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="bae94-187">Bu yöntem null değeri döndürülürse, varsayılan çözümleyici geri.</span><span class="sxs-lookup"><span data-stu-id="bae94-187">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="bae94-188">**GetServices** yöntemi, belirtilen türdeki nesneleri koleksiyonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bae94-188">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="bae94-189">Varsayılan çözümleyici alınan sonuçlarla Ninject sonuçlarından birleştirmek için bu yöntemi geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="bae94-189">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="bae94-190">Ninject bağlamaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="bae94-190">Configure Ninject Bindings</span></span>

<span data-ttu-id="bae94-191">Şimdi türü bağlamaları bildirmek için Ninject kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="bae94-191">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="bae94-192">RegisterHubs.cs dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="bae94-192">Open the file RegisterHubs.cs.</span></span> <span data-ttu-id="bae94-193">İçinde `RegisterHubs.Start` yöntemi, Ninject çağıran Ninject kapsayıcı oluşturmak *çekirdek*.</span><span class="sxs-lookup"><span data-stu-id="bae94-193">In the `RegisterHubs.Start` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="bae94-194">Bizim özel bağımlılık Çözümleyicisi örneği oluşturun:</span><span class="sxs-lookup"><span data-stu-id="bae94-194">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="bae94-195">İçin bir bağlama oluşturun `IStockTicker` gibi:</span><span class="sxs-lookup"><span data-stu-id="bae94-195">Create a binding for `IStockTicker` as follows:</span></span>

[!code-html[Main](dependency-injection/samples/sample17.html)]

<span data-ttu-id="bae94-196">Bu kod iki şey söyleyen.</span><span class="sxs-lookup"><span data-stu-id="bae94-196">This code is saying two things.</span></span> <span data-ttu-id="bae94-197">Uygulamanın ihtiyacı olduğunda ilk bir `IStockTicker`, çekirdek örneği oluşturmalısınız `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="bae94-197">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="bae94-198">İkinci, `StockTicker` sınıfı singleton nesnesi olarak oluşturulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="bae94-198">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="bae94-199">Ninject nesnesinin bir örneğini oluşturun ve her istek için aynı örnek döndürülecektir.</span><span class="sxs-lookup"><span data-stu-id="bae94-199">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="bae94-200">İçin bir bağlama oluşturun **IHubConnectionContext** gibi:</span><span class="sxs-lookup"><span data-stu-id="bae94-200">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="bae94-201">Bu kod creatres döndüren anonim bir işlev bir **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="bae94-201">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="bae94-202">**WhenInjectedInto** yöntemi yalnızca oluşturulurken bu işlevi kullanmak için Ninject söyler `IStockTicker` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="bae94-202">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="bae94-203">SignalR oluşturduğu nedeni **IHubConnectionContext** dahili olarak, örnekler ve nasıl SignalR kendilerini oluşturan geçersiz kılmak istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="bae94-203">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="bae94-204">Bu işlev yalnızca uygular bizim `StockTicker` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="bae94-204">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="bae94-205">Bağımlılık çözümleyici uygulamasına geçirmek **MapHubs** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="bae94-205">Pass the dependency resolver into the **MapHubs** method:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="bae94-206">SignalR belirtilen çözümleyici kullanacağı artık **MapHubs**, yerine varsayılan çözümleyici.</span><span class="sxs-lookup"><span data-stu-id="bae94-206">Now SignalR will use the resolver specified in **MapHubs**, instead of the default resolver.</span></span>

<span data-ttu-id="bae94-207">İçin tam kod işte `RegisterHubs.Start`.</span><span class="sxs-lookup"><span data-stu-id="bae94-207">Here is the complete code listing for `RegisterHubs.Start`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="bae94-208">Visual Studio'da StockTicker uygulamayı çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="bae94-208">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="bae94-209">Tarayıcı penceresine gidin `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="bae94-209">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="bae94-210">Uygulamanın tam olarak aynı işlevselliği önce vardır.</span><span class="sxs-lookup"><span data-stu-id="bae94-210">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="bae94-211">(Bir açıklaması için bkz [Server yayını ASP.NET SignalR ile](index.md).) Biz davranışı değiştirmezsiniz; Yeni kod sınamak için korumak ve gelişmesi daha kolay uyguladı.</span><span class="sxs-lookup"><span data-stu-id="bae94-211">(For a description, see [Server Broadcast with ASP.NET SignalR](index.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
