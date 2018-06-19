---
uid: signalr/overview/advanced/dependency-injection
title: SignalR öğesindeki bağımlılık ekleme | Microsoft Docs
author: MikeWasson
description: Yazılım sürümleri bu konuda Visual Studio 2013 .NET 4.5 SignalR önceki sürümleri hakkında bilgi için bu konuda sürüm 2 önceki sürümlerinde kullanılan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26565560"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="3a08d-103">SignalR öğesindeki bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="3a08d-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="3a08d-104">tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="3a08d-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3a08d-105">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="3a08d-105">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="3a08d-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3a08d-106">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="3a08d-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3a08d-107">.NET 4.5</span></span>
> - <span data-ttu-id="3a08d-108">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="3a08d-108">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="3a08d-109">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="3a08d-109">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="3a08d-110">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="3a08d-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="3a08d-111">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="3a08d-111">Questions and comments</span></span>
> 
> <span data-ttu-id="3a08d-112">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="3a08d-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3a08d-113">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="3a08d-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="3a08d-114">Bağımlılık ekleme, nesneler, nesnenin bağımlılıkları, (sahte nesneler kullanılarak) sınama ya da değiştirmek için ya da çalışma zamanı davranışını değiştirmek için daha kolay hale arasındaki sabit kodlu bağımlılıklar kaldırmak için bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="3a08d-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="3a08d-115">Bu öğretici SignalR hub'larında bağımlılık ekleme gerçekleştirme gösterir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="3a08d-116">Ayrıca SignalR ile IOC kapsayıcıları kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="3a08d-117">IOC kapsayıcı bağımlılık ekleme için genel bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="3a08d-118">Bağımlılık ekleme nedir?</span><span class="sxs-lookup"><span data-stu-id="3a08d-118">What is Dependency Injection?</span></span>

<span data-ttu-id="3a08d-119">Bağımlılık ekleme ile bilginiz varsa bu bölüm atlayın.</span><span class="sxs-lookup"><span data-stu-id="3a08d-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="3a08d-120">*Bağımlılık ekleme* (dı) olan bir desen burada nesneler kendi bağımlılıkları oluşturmaktan sorumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="3a08d-121">DI becerisiyle basit bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="3a08d-122">İletilerini günlüğe kaydetmek için gereken bir nesne olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="3a08d-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="3a08d-123">Günlük arabirim tanımlayabilir:</span><span class="sxs-lookup"><span data-stu-id="3a08d-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="3a08d-124">Nesnenizin içinde oluşturduğunuz bir `ILogger` iletilerini günlüğe kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="3a08d-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="3a08d-125">Bu çalışır, ancak en iyi tasarım değil.</span><span class="sxs-lookup"><span data-stu-id="3a08d-125">This works, but it's not the best design.</span></span> <span data-ttu-id="3a08d-126">Değiştirmek istiyorsanız `FileLogger` diğeriyle `ILogger` uygulaması, olacaktır değiştirmek `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="3a08d-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="3a08d-127">Diğer birçok kullanım nesneleri kabul `FileLogger`, bunların tümünün değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="3a08d-128">Veya yapmaya karar verirseniz `FileLogger` bir singleton Ayrıca uygulama boyunca değişiklik sağlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="3a08d-129">Daha iyi bir yaklaşım "eklemesine" olan bir `ILogger` nesnesine — oluşturucu bağımsız değişkeni kullanarak örneğin:</span><span class="sxs-lookup"><span data-stu-id="3a08d-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="3a08d-130">Nesne seçmek için sorumlu değildir artık `ILogger` kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="3a08d-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="3a08d-131">Swich yapabilecekleriniz `ILogger` bağımlı nesneleri değiştirmeden uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="3a08d-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="3a08d-132">Bu desen adlı [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="3a08d-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="3a08d-133">Başka bir düzen ayarlayıcı yöntemi veya özelliği aracılığıyla bağımlılık ayarladığınız ayarlayıcı ekleme yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="3a08d-134">SignalR öğesindeki basit bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="3a08d-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="3a08d-135">Öğretici sohbet uygulamadan göz önünde bulundurun [SignalR ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="3a08d-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="3a08d-136">Bu uygulama hub sınıfından şöyledir:</span><span class="sxs-lookup"><span data-stu-id="3a08d-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="3a08d-137">Sohbet iletileri göndermeden önce sunucuda saklamak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="3a08d-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="3a08d-138">Bu işlev soyutlar bir arabirim tanımlayın ve arabirimine eklemesine dı kullanmak `ChatHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3a08d-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="3a08d-139">Bir SignalR uygulaması doğrudan hub'ları oluşturmaz yalnızca sorunudur; SignalR bunları sizin için oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3a08d-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="3a08d-140">Varsayılan olarak, bir parametresiz oluşturucuya sahip olması için bir hub sınıf SignalR bekliyor.</span><span class="sxs-lookup"><span data-stu-id="3a08d-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="3a08d-141">Ancak, kolayca hub örnekleri oluşturmak için bir işlev kaydetmek ve DI gerçekleştirmek için bu işlevi kullanın.</span><span class="sxs-lookup"><span data-stu-id="3a08d-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="3a08d-142">İşlevini çağırarak kaydetmek **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="3a08d-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="3a08d-143">Oluşturması için gereken her SignalR bu anonim işlevi çağırma artık bir `ChatHub` örneği.</span><span class="sxs-lookup"><span data-stu-id="3a08d-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="3a08d-144">IOC kapsayıcıları</span><span class="sxs-lookup"><span data-stu-id="3a08d-144">IoC Containers</span></span>

<span data-ttu-id="3a08d-145">Önceki kod basit durumlar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="3a08d-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="3a08d-146">Ancak hala bu yazmak zorunda kaldı:</span><span class="sxs-lookup"><span data-stu-id="3a08d-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="3a08d-147">Birçok bağımlılıkları olan karmaşık bir uygulamada bu "kablolama" kod çok yazma gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="3a08d-148">Bu kod, özellikle bağımlılıkları iç içe geçmişse korumak zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="3a08d-149">Ayrıca, birim testi için de zordur.</span><span class="sxs-lookup"><span data-stu-id="3a08d-149">It is also hard to unit test.</span></span>

<span data-ttu-id="3a08d-150">Bir çözüm IOC kapsayıcı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="3a08d-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="3a08d-151">Bağımlılıklar yönetmekten sorumlu bir yazılım bileşeni bir IOC kapsayıcıdır. Kapsayıcıyla türleri kaydetmek ve nesneleri oluşturmak için kapsayıcı kullanın.</span><span class="sxs-lookup"><span data-stu-id="3a08d-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="3a08d-152">Kapsayıcı otomatik olarak bağımlılık ilişkileri rakamlar.</span><span class="sxs-lookup"><span data-stu-id="3a08d-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="3a08d-153">Birçok IOC kapsayıcıları nesne ömrü ve kapsam gibi denetlemenize izin.</span><span class="sxs-lookup"><span data-stu-id="3a08d-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="3a08d-154">"IoC" anlamına gelir "tersine çevirme denetimi için", olduğu genel bir desen olduğu bir çerçeve uygulama kodu çağırır.</span><span class="sxs-lookup"><span data-stu-id="3a08d-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="3a08d-155">IOC kapsayıcı nesnelerinizi sizin için hangi "Normal akış denetimi tersine çevirir" oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3a08d-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="3a08d-156">SignalR öğesinde IOC kapsayıcıları kullanma</span><span class="sxs-lookup"><span data-stu-id="3a08d-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="3a08d-157">Sohbet uygulaması büyük olasılıkla bir IOC kapsayıcıdan yararlanmak basit bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="3a08d-158">Bunun yerine, bakalım [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) örnek.</span><span class="sxs-lookup"><span data-stu-id="3a08d-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="3a08d-159">StockTicker örnek iki ana sınıf tanımlar:</span><span class="sxs-lookup"><span data-stu-id="3a08d-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="3a08d-160">`StockTickerHub`: İstemci bağlantıları yönetir hub sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3a08d-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="3a08d-161">`StockTicker`: Hisse senedi fiyatları tutar ve bunları düzenli aralıklarla güncelleştirir bir singleton.</span><span class="sxs-lookup"><span data-stu-id="3a08d-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="3a08d-162">`StockTickerHub`bir başvuru tutan `StockTicker` tek sırada `StockTicker` başvuru tutan **IHubConnectionContext** için `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="3a08d-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="3a08d-163">Bu arabirim ile iletişim kurmak için kullandığı `StockTickerHub` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="3a08d-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="3a08d-164">(Daha fazla bilgi için bkz: [Server yayını ASP.NET SignalR ile](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="3a08d-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="3a08d-165">Bu bağımlılıklar biraz untangle için size bir IOC kapsayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3a08d-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="3a08d-166">İlk olarak, şirketinizdeki basitleştirmek `StockTickerHub` ve `StockTicker` sınıfları.</span><span class="sxs-lookup"><span data-stu-id="3a08d-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="3a08d-167">Aşağıdaki kodda gerekmez, ı bölümleri açıklamalı.</span><span class="sxs-lookup"><span data-stu-id="3a08d-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="3a08d-168">Parametresiz bir kurucusu öğesinden Kaldır `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="3a08d-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="3a08d-169">Bunun yerine, her zaman dı hub oluşturmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="3a08d-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="3a08d-170">StockTicker için tek örnek kaldırın.</span><span class="sxs-lookup"><span data-stu-id="3a08d-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="3a08d-171">IOC kapsayıcı StockTicker ömrünü denetlemek için daha sonra kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="3a08d-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="3a08d-172">Ayrıca, Oluşturucusu ortak olun.</span><span class="sxs-lookup"><span data-stu-id="3a08d-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="3a08d-173">Ardından, biz kodu için bir arabirimi oluşturarak yeniden düzenlemeniz `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="3a08d-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="3a08d-174">Bu arabirim ile aynı şekilde kullanacağız `StockTickerHub` gelen `StockTicker` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3a08d-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="3a08d-175">Visual Studio bu kolay tür düzenleme yapar.</span><span class="sxs-lookup"><span data-stu-id="3a08d-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="3a08d-176">StockTicker.cs dosyasını açın, sağ tıklayın `StockTicker` sınıf bildiriminin ve seçin **yeniden düzenlemeniz** ... **Ayıklama arabirimi**.</span><span class="sxs-lookup"><span data-stu-id="3a08d-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="3a08d-177">İçinde **arayüz** iletişim kutusunda, tıklatın **Tümünü Seç**.</span><span class="sxs-lookup"><span data-stu-id="3a08d-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="3a08d-178">Diğer Varsayılanları bırakın.</span><span class="sxs-lookup"><span data-stu-id="3a08d-178">Leave the other defaults.</span></span> <span data-ttu-id="3a08d-179">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="3a08d-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="3a08d-180">Visual Studio adlı yeni bir arabirim oluşturur `IStockTicker`ve ayrıca değiştirir `StockTicker` türetmek `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="3a08d-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="3a08d-181">IStockTicker.cs dosyasını açın ve arabirimine değiştirme **ortak**.</span><span class="sxs-lookup"><span data-stu-id="3a08d-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="3a08d-182">İçinde `StockTickerHub` sınıfı, iki örneğini değiştirmek `StockTicker` için `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="3a08d-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="3a08d-183">Oluşturma bir `IStockTicker` arabirimi kesinlikle gerekli değildir, ancak dı uygulamanızda bileşenleri arasındaki ilişki azaltmak için nasıl yardımcı olabileceğini gösteren istedik.</span><span class="sxs-lookup"><span data-stu-id="3a08d-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="3a08d-184">Ninject kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="3a08d-184">Add the Ninject Library</span></span>

<span data-ttu-id="3a08d-185">.NET için birçok açık kaynak IOC kapsayıcı vardır.</span><span class="sxs-lookup"><span data-stu-id="3a08d-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="3a08d-186">Bu öğretici için kullanmam [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="3a08d-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="3a08d-187">(Diğer popüler kitaplıkları dahil [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), ve [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="3a08d-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="3a08d-188">Yüklemek için NuGet Paket Yöneticisi'ni kullanın [Ninject Kitaplığı](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="3a08d-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="3a08d-189">Visual Studio'da gelen **Araçları** menüsünü seçin **kitaplık Paket Yöneticisi** | **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="3a08d-189">In Visual Studio, from the **Tools** menu select **Library Package Manager** | **Package Manager Console**.</span></span> <span data-ttu-id="3a08d-190">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="3a08d-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="3a08d-191">SignalR bağımlılık çözümleyiciyi değiştirin</span><span class="sxs-lookup"><span data-stu-id="3a08d-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="3a08d-192">SignalR içinde Ninject kullanmak için türeyen bir sınıf oluşturun **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="3a08d-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="3a08d-193">Bu sınıf geçersiz kılmaları **GetService** ve **GetServices** yöntemlerinin **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="3a08d-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="3a08d-194">SignalR, SignalR tarafından dahili olarak kullanılan çeşitli hizmetlerin yanı sıra, hub örnekleri dahil olmak üzere çalışma zamanında çeşitli nesneleri oluşturmak için bu yöntem çağırır.</span><span class="sxs-lookup"><span data-stu-id="3a08d-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="3a08d-195">**GetService** yöntemi, bir türü tek bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3a08d-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="3a08d-196">Ninject çekirdek 's çağırmak için bu yöntemi geçersiz kılın **TryGet** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3a08d-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="3a08d-197">Bu yöntem null değeri döndürülürse, varsayılan çözümleyici geri.</span><span class="sxs-lookup"><span data-stu-id="3a08d-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="3a08d-198">**GetServices** yöntemi, belirtilen türdeki nesneleri koleksiyonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3a08d-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="3a08d-199">Varsayılan çözümleyici alınan sonuçlarla Ninject sonuçlarından birleştirmek için bu yöntemi geçersiz kılın.</span><span class="sxs-lookup"><span data-stu-id="3a08d-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="3a08d-200">Ninject bağlamaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3a08d-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="3a08d-201">Şimdi türü bağlamaları bildirmek için Ninject kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="3a08d-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="3a08d-202">Uygulamanızın haline sınıfı açın (ya da el ile paket yönergeler doğrultusunda oluşturduğunuz `readme.txt`, veya kimlik doğrulama projenize ekleme tarafından oluşturulan).</span><span class="sxs-lookup"><span data-stu-id="3a08d-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="3a08d-203">İçinde `Startup.Configuration` yöntemi, Ninject çağıran Ninject kapsayıcı oluşturmak *çekirdek*.</span><span class="sxs-lookup"><span data-stu-id="3a08d-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="3a08d-204">Bizim özel bağımlılık Çözümleyicisi örneği oluşturun:</span><span class="sxs-lookup"><span data-stu-id="3a08d-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="3a08d-205">İçin bir bağlama oluşturun `IStockTicker` gibi:</span><span class="sxs-lookup"><span data-stu-id="3a08d-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="3a08d-206">Bu kod iki şey söyleyen.</span><span class="sxs-lookup"><span data-stu-id="3a08d-206">This code is saying two things.</span></span> <span data-ttu-id="3a08d-207">Uygulamanın ihtiyacı olduğunda ilk bir `IStockTicker`, çekirdek örneği oluşturmalısınız `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="3a08d-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="3a08d-208">İkinci, `StockTicker` sınıfı singleton nesnesi olarak oluşturulmuş olabilir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="3a08d-209">Ninject nesnesinin bir örneğini oluşturun ve her istek için aynı örnek döndürülecektir.</span><span class="sxs-lookup"><span data-stu-id="3a08d-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="3a08d-210">İçin bir bağlama oluşturun **IHubConnectionContext** gibi:</span><span class="sxs-lookup"><span data-stu-id="3a08d-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="3a08d-211">Bu kod creatres döndüren anonim bir işlev bir **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="3a08d-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="3a08d-212">**WhenInjectedInto** yöntemi yalnızca oluşturulurken bu işlevi kullanmak için Ninject söyler `IStockTicker` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="3a08d-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="3a08d-213">SignalR oluşturduğu nedeni **IHubConnectionContext** dahili olarak, örnekler ve nasıl SignalR kendilerini oluşturan geçersiz kılmak istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="3a08d-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="3a08d-214">Bu işlev yalnızca uygular bizim `StockTicker` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3a08d-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="3a08d-215">Bağımlılık çözümleyici uygulamasına geçirmek **MapSignalR** bir hub yapılandırmasını ekleyerek yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3a08d-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="3a08d-216">Örnek 's başlangıç sınıfı Startup.ConfigureSignalR yönteminde yeni parametresiyle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="3a08d-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="3a08d-217">SignalR belirtilen çözümleyici kullanacağı artık **MapSignalR**, yerine varsayılan çözümleyici.</span><span class="sxs-lookup"><span data-stu-id="3a08d-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="3a08d-218">İçin tam kod işte `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="3a08d-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="3a08d-219">Visual Studio'da StockTicker uygulamayı çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="3a08d-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="3a08d-220">Tarayıcı penceresine gidin `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="3a08d-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="3a08d-221">Uygulamanın tam olarak aynı işlevselliği önce vardır.</span><span class="sxs-lookup"><span data-stu-id="3a08d-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="3a08d-222">(Bir açıklaması için bkz [Server yayını ASP.NET SignalR ile](../getting-started/tutorial-server-broadcast-with-signalr.md).) Biz davranışı değiştirmezsiniz; Yeni kod sınamak için korumak ve gelişmesi daha kolay uyguladı.</span><span class="sxs-lookup"><span data-stu-id="3a08d-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
