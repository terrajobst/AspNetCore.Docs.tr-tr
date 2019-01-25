---
uid: signalr/overview/advanced/dependency-injection
title: Signalr'da bağımlılık ekleme | Microsoft Docs
author: bradygaster
description: Yazılım sürümleri, sürüm 2 önceki sürümleri bu konunun önceki sürümleri hakkında bilgi için bu konu Visual Studio 2013 .NET 4.5 SignalR kullanılan...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 54e263e277852d2d478ce5bccd4164254498831a
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836616"
---
<a name="dependency-injection-in-signalr"></a><span data-ttu-id="3aee1-103">SignalR’da Bağımlılık Ekleme</span><span class="sxs-lookup"><span data-stu-id="3aee1-103">Dependency Injection in SignalR</span></span>
====================
<span data-ttu-id="3aee1-104">tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="3aee1-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3aee1-105">Bu konu başlığında kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="3aee1-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="3aee1-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3aee1-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="3aee1-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3aee1-107">.NET 4.5</span></span>
> - <span data-ttu-id="3aee1-108">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="3aee1-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="3aee1-109">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="3aee1-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="3aee1-110">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="3aee1-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="3aee1-111">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="3aee1-111">Questions and comments</span></span>
>
> <span data-ttu-id="3aee1-112">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="3aee1-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3aee1-113">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="3aee1-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="3aee1-114">Bağımlılık ekleme (sahte nesneler kullanılarak) test etmek için ya da bir nesnenin bağımlılıkları değiştirin veya çalışma zamanı davranışını değiştirmek için daha kolay hale getirme nesneleri arasındaki sabit kodlu bağımlılıkları kaldırmak için bir yoldur.</span><span class="sxs-lookup"><span data-stu-id="3aee1-114">Dependency injection is a way to remove hard-coded dependencies between objects, making it easier to replace an object's dependencies, either for testing (using mock objects) or to change run-time behavior.</span></span> <span data-ttu-id="3aee1-115">Bu öğreticide SignalR hub'larında bağımlılık ekleme yapmak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-115">This tutorial shows how to perform dependency injection on SignalR hubs.</span></span> <span data-ttu-id="3aee1-116">Ayrıca SignalR ile IOC kapsayıcıları kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-116">It also shows how to use IoC containers with SignalR.</span></span> <span data-ttu-id="3aee1-117">IOC kapsayıcı için bağımlılık ekleme genel bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-117">An IoC container is a general framework for dependency injection.</span></span>

## <a name="what-is-dependency-injection"></a><span data-ttu-id="3aee1-118">Bağımlılık ekleme nedir?</span><span class="sxs-lookup"><span data-stu-id="3aee1-118">What is Dependency Injection?</span></span>

<span data-ttu-id="3aee1-119">Zaten bağımlılık ekleme hakkında bilginiz varsa, bu bölümü atlayın.</span><span class="sxs-lookup"><span data-stu-id="3aee1-119">Skip this section if you are already familiar with dependency injection.</span></span>

<span data-ttu-id="3aee1-120">*Bağımlılık ekleme* (dı) olan bir desen burada nesneler kendi bağımlılıklar oluşturmaktan sorumlu değildir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-120">*Dependency injection* (DI) is a pattern where objects are not responsible for creating their own dependencies.</span></span> <span data-ttu-id="3aee1-121">DI becerisiyle basit bir örnek aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-121">Here is a simple example to motivate DI.</span></span> <span data-ttu-id="3aee1-122">İletilerini günlüğe kaydetmek için gereken bir nesne olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="3aee1-122">Suppose you have an object that needs to log messages.</span></span> <span data-ttu-id="3aee1-123">Günlüğe kaydetme arabirimi tanımlayabilir:</span><span class="sxs-lookup"><span data-stu-id="3aee1-123">You might define a logging interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="3aee1-124">Nesnesi içinde oluşturduğunuz bir `ILogger` iletilerini günlüğe kaydetmek için:</span><span class="sxs-lookup"><span data-stu-id="3aee1-124">In your object, you can create an `ILogger` to log messages:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="3aee1-125">Bu çalışır, ancak en iyi tasarım değil.</span><span class="sxs-lookup"><span data-stu-id="3aee1-125">This works, but it's not the best design.</span></span> <span data-ttu-id="3aee1-126">Değiştirmek istiyorsanız `FileLogger` diğeriyle `ILogger` uygulama, değiştirme gerekir `SomeComponent`.</span><span class="sxs-lookup"><span data-stu-id="3aee1-126">If you want to replace `FileLogger` with another `ILogger` implementation, you will have to modify `SomeComponent`.</span></span> <span data-ttu-id="3aee1-127">Diğer birçok kullanım nesnelerini kabul ederek şehirler `FileLogger`, bunların tümünün değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-127">Supposing that a lot of other objects use `FileLogger`, you will need to change all of them.</span></span> <span data-ttu-id="3aee1-128">Veya yapmaya karar verirseniz `FileLogger` tek Ayrıca uygulama boyunca değişiklik yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-128">Or if you decide to make `FileLogger` a singleton, you'll also need to make changes throughout the application.</span></span>

<span data-ttu-id="3aee1-129">Daha iyi bir yaklaşım "" bulunmaktır bir `ILogger` nesnesine — Örneğin, bir oluşturucu bağımsız değişkenini kullanarak tarafından:</span><span class="sxs-lookup"><span data-stu-id="3aee1-129">A better approach is to "inject" an `ILogger` into the object—for example, by using a constructor argument:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="3aee1-130">Nesne seçmek için sorumlu değildir. Şimdi `ILogger` kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="3aee1-130">Now the object is not responsible for selecting which `ILogger` to use.</span></span> <span data-ttu-id="3aee1-131">Swich yapabilecekleriniz `ILogger` bağımlı nesneleri değiştirmeden uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="3aee1-131">You can swich `ILogger` implementations without changing the objects that depend on it.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="3aee1-132">Bu düzeni olarak adlandırılır [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="3aee1-132">This pattern is called [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="3aee1-133">Başka bir ayarlayıcı eklemesi, ayarlayıcı yöntemi veya özelliği aracılığıyla bağımlılık ayarladığınız modelidir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-133">Another pattern is setter injection, where you set the dependency through a setter method or property.</span></span>

## <a name="simple-dependency-injection-in-signalr"></a><span data-ttu-id="3aee1-134">Signalr'da basit bağımlılık ekleme</span><span class="sxs-lookup"><span data-stu-id="3aee1-134">Simple Dependency Injection in SignalR</span></span>

<span data-ttu-id="3aee1-135">Sohbet uygulaması Öğreticisi göz önünde bulundurun [SignalR ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="3aee1-135">Consider the Chat application from the tutorial [Getting Started with SignalR](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="3aee1-136">Bu uygulama hub sınıfı şöyledir:</span><span class="sxs-lookup"><span data-stu-id="3aee1-136">Here is the hub class from that application:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="3aee1-137">Sohbet iletileri göndermeden önce sunucuda depolamak istediğinizi varsayalım.</span><span class="sxs-lookup"><span data-stu-id="3aee1-137">Suppose that you want to store chat messages on the server before sending them.</span></span> <span data-ttu-id="3aee1-138">Bu işlev soyutlayan bir arabirim tanımlayın ve arabirimine eklemesine DI kullanmak `ChatHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3aee1-138">You might define an interface that abstracts this functionality, and use DI to inject the interface into the `ChatHub` class.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="3aee1-139">Bir SignalR uygulama doğrudan hub'ları oluşturmaz yalnızca sorunudur; SignalR bunları sizin için oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3aee1-139">The only problem is that a SignalR application does not directly create hubs; SignalR creates them for you.</span></span> <span data-ttu-id="3aee1-140">Varsayılan olarak, bir hub sınıfı, parametresiz bir oluşturucu sağlamak için SignalR bekliyor.</span><span class="sxs-lookup"><span data-stu-id="3aee1-140">By default, SignalR expects a hub class to have a parameterless constructor.</span></span> <span data-ttu-id="3aee1-141">Ancak, kolayca hub örnekleri oluşturmak için bir işlev kaydetmek ve DI gerçekleştirmek için bu işlevi kullanın.</span><span class="sxs-lookup"><span data-stu-id="3aee1-141">However, you can easily register a function to create hub instances, and use this function to perform DI.</span></span> <span data-ttu-id="3aee1-142">İşlevini çağırarak kaydetmek **GlobalHost.DependencyResolver.Register**.</span><span class="sxs-lookup"><span data-stu-id="3aee1-142">Register the function by calling **GlobalHost.DependencyResolver.Register**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

<span data-ttu-id="3aee1-143">SignalR oluşturmanız gerektiğinde bu anonim işlev çağıracağı artık bir `ChatHub` örneği.</span><span class="sxs-lookup"><span data-stu-id="3aee1-143">Now SignalR will invoke this anonymous function whenever it needs to create a `ChatHub` instance.</span></span>

## <a name="ioc-containers"></a><span data-ttu-id="3aee1-144">IOC kapsayıcıları</span><span class="sxs-lookup"><span data-stu-id="3aee1-144">IoC Containers</span></span>

<span data-ttu-id="3aee1-145">Önceki kod, basit durumlar için uygundur.</span><span class="sxs-lookup"><span data-stu-id="3aee1-145">The previous code is fine for simple cases.</span></span> <span data-ttu-id="3aee1-146">Ancak yine de bu yazmanız gerekirdi:</span><span class="sxs-lookup"><span data-stu-id="3aee1-146">But you still had to write this:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

<span data-ttu-id="3aee1-147">Birçok bağımlılıkları olan karmaşık bir uygulamada bu "bağlantı" kod yazmanız gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-147">In a complex application with many dependencies, you might need to write a lot of this "wiring" code.</span></span> <span data-ttu-id="3aee1-148">Bu kod, özellikle bağımlılıkları iç içe geçmişse sağlamak zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-148">This code can be hard to maintain, especially if dependencies are nested.</span></span> <span data-ttu-id="3aee1-149">Ayrıca, birim testi de zordur.</span><span class="sxs-lookup"><span data-stu-id="3aee1-149">It is also hard to unit test.</span></span>

<span data-ttu-id="3aee1-150">Tek bir çözüm IOC kapsayıcı kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="3aee1-150">One solution is to use an IoC container.</span></span> <span data-ttu-id="3aee1-151">IOC kapsayıcı bağımlılıkları yönetmekten sorumlu olan bir yazılım bileşenidir. Türleri ile kapsayıcı kayıt ve sonra da kapsayıcı nesneleri oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="3aee1-151">An IoC container is a software component that is responsible for managing dependencies.You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="3aee1-152">Kapsayıcı bağımlılık ilişkileri otomatik olarak belirler.</span><span class="sxs-lookup"><span data-stu-id="3aee1-152">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="3aee1-153">Çok sayıda IOC kapsayıcı nesne yaşam süresi ve kapsamı gibi denetlemenize izin.</span><span class="sxs-lookup"><span data-stu-id="3aee1-153">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="3aee1-154">"IoC" anlamına gelir "tersine çevirme denetimi için", burada bir çerçeve koduna çağrı genel düzen olduğu.</span><span class="sxs-lookup"><span data-stu-id="3aee1-154">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="3aee1-155">IOC kapsayıcı nesnelerinizi sizin için "Normal denetim akışını tersine çevirir" oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3aee1-155">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


## <a name="using-ioc-containers-in-signalr"></a><span data-ttu-id="3aee1-156">SignalR öğesinde IOC kapsayıcıları kullanma</span><span class="sxs-lookup"><span data-stu-id="3aee1-156">Using IoC Containers in SignalR</span></span>

<span data-ttu-id="3aee1-157">Sohbet uygulaması büyük olasılıkla bir IOC kapsayıcısından yararlanmak basit bir işlemdir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-157">The Chat application is probably too simple to benefit from an IoC container.</span></span> <span data-ttu-id="3aee1-158">Bunun yerine, göz atalım [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) örnek.</span><span class="sxs-lookup"><span data-stu-id="3aee1-158">Instead, let's look at the [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) sample.</span></span>

<span data-ttu-id="3aee1-159">StockTicker örnek iki ana sınıf tanımlar:</span><span class="sxs-lookup"><span data-stu-id="3aee1-159">The StockTicker sample defines two main classes:</span></span>

- <span data-ttu-id="3aee1-160">`StockTickerHub`: Hub sınıfına istemci bağlantıları yönetir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-160">`StockTickerHub`: The hub class, which manages client connections.</span></span>
- <span data-ttu-id="3aee1-161">`StockTicker`: Hisse senedi fiyatlarına tutar ve bunları düzenli aralıklarla güncelleştiren bir tekli.</span><span class="sxs-lookup"><span data-stu-id="3aee1-161">`StockTicker`: A singleton that holds stock prices and periodically updates them.</span></span>

<span data-ttu-id="3aee1-162">`StockTickerHub` bir başvuru tutan `StockTicker` tekil, ancak `StockTicker` bir başvuru tutan **IHubConnectionContext** için `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="3aee1-162">`StockTickerHub` holds a reference to the `StockTicker` singleton, while `StockTicker` holds a reference to the **IHubConnectionContext** for the `StockTickerHub`.</span></span> <span data-ttu-id="3aee1-163">Bu arabirim ile iletişim kurmak için kullandığı `StockTickerHub` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="3aee1-163">It uses this interface to communicate with `StockTickerHub` instances.</span></span> <span data-ttu-id="3aee1-164">(Daha fazla bilgi için [ASP.NET SignalR ile sunucu yayını](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span><span class="sxs-lookup"><span data-stu-id="3aee1-164">(For more information, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).)</span></span>

<span data-ttu-id="3aee1-165">Bu bağımlılıklar biraz ayrıştırmaya IOC kapsayıcı kullanabiliriz.</span><span class="sxs-lookup"><span data-stu-id="3aee1-165">We can use an IoC container to untangle these dependencies a bit.</span></span> <span data-ttu-id="3aee1-166">İlk olarak, şimdi basitleştirmek `StockTickerHub` ve `StockTicker` sınıfları.</span><span class="sxs-lookup"><span data-stu-id="3aee1-166">First, let's simplify the `StockTickerHub` and `StockTicker` classes.</span></span> <span data-ttu-id="3aee1-167">Aşağıdaki kodda, ihtiyaç duymayacağımız olduğunu miyim bölümlerini açıklamalı.</span><span class="sxs-lookup"><span data-stu-id="3aee1-167">In the following code, I've commented out the parts that we don't need.</span></span>

<span data-ttu-id="3aee1-168">Parametresiz oluşturucusu öğesinden kaldırmak `StockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="3aee1-168">Remove the parameterless constructor from `StockTickerHub`.</span></span> <span data-ttu-id="3aee1-169">Bunun yerine, her zaman DI hub'ı oluşturmak için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="3aee1-169">Instead, we will always use DI to create the hub.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

<span data-ttu-id="3aee1-170">StockTicker için tekil örneğini kaldırın.</span><span class="sxs-lookup"><span data-stu-id="3aee1-170">For StockTicker, remove the singleton instance.</span></span> <span data-ttu-id="3aee1-171">Daha sonra IOC StockTicker ömrünü denetlemek için kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="3aee1-171">Later, we'll use the IoC container to control the StockTicker lifetime.</span></span> <span data-ttu-id="3aee1-172">Ayrıca, ortak oluşturucu olun.</span><span class="sxs-lookup"><span data-stu-id="3aee1-172">Also, make the constructor public.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

<span data-ttu-id="3aee1-173">Ardından, biz kodu için bir arabirim oluşturarak yeniden düzenleyebilirsiniz `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="3aee1-173">Next, we can refactor the code by creating an interface for `StockTicker`.</span></span> <span data-ttu-id="3aee1-174">Bu arabirim ayrıştırmak için kullanacağız `StockTickerHub` gelen `StockTicker` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3aee1-174">We'll use this interface to decouple the `StockTickerHub` from the `StockTicker` class.</span></span>

<span data-ttu-id="3aee1-175">Visual Studio bu kolay tür yeniden düzenleme sağlar.</span><span class="sxs-lookup"><span data-stu-id="3aee1-175">Visual Studio makes this kind of refactoring easy.</span></span> <span data-ttu-id="3aee1-176">StockTicker.cs dosyasını açın, sağ `StockTicker` sınıf bildiriminin ve seçin **yeniden düzenleme** ... **Ayıklama arabirimi**.</span><span class="sxs-lookup"><span data-stu-id="3aee1-176">Open the file StockTicker.cs, right-click on the `StockTicker` class declaration, and select **Refactor** ... **Extract Interface**.</span></span>

![](dependency-injection/_static/image1.png)

<span data-ttu-id="3aee1-177">İçinde **Arabirimi Ayıkla** iletişim kutusunda, tıklayın **Tümünü Seç**.</span><span class="sxs-lookup"><span data-stu-id="3aee1-177">In the **Extract Interface** dialog, click **Select All**.</span></span> <span data-ttu-id="3aee1-178">Diğer varsayılan değerleri bırakın.</span><span class="sxs-lookup"><span data-stu-id="3aee1-178">Leave the other defaults.</span></span> <span data-ttu-id="3aee1-179">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="3aee1-179">Click **OK**.</span></span>

![](dependency-injection/_static/image2.png)

<span data-ttu-id="3aee1-180">Visual Studio adlı yeni bir arabirim oluşturur `IStockTicker`ve ayrıca değiştirir `StockTicker` türetmek için `IStockTicker`.</span><span class="sxs-lookup"><span data-stu-id="3aee1-180">Visual Studio creates a new interface named `IStockTicker`, and also changes `StockTicker` to derive from `IStockTicker`.</span></span>

<span data-ttu-id="3aee1-181">IStockTicker.cs dosyasını açın ve değiştirmek için arabirimi **genel**.</span><span class="sxs-lookup"><span data-stu-id="3aee1-181">Open the file IStockTicker.cs and change the interface to **public**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

<span data-ttu-id="3aee1-182">İçinde `StockTickerHub` sınıfı, iki örneğinden birini değiştirmek `StockTicker` için `IStockTicker`:</span><span class="sxs-lookup"><span data-stu-id="3aee1-182">In the `StockTickerHub` class, change the two instances of `StockTicker` to `IStockTicker`:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

<span data-ttu-id="3aee1-183">Oluşturma bir `IStockTicker` arabirimi kesinlikle gerekli değildir, ancak DI uygulamanızın bileşenleri arasında eşleştirme yapmaktan azaltmak için nasıl yardımcı olabileceğini gösteren istedi.</span><span class="sxs-lookup"><span data-stu-id="3aee1-183">Creating an `IStockTicker` interface isn't strictly necessary, but I wanted to show how DI can help to reduce coupling between components in your application.</span></span>

## <a name="add-the-ninject-library"></a><span data-ttu-id="3aee1-184">Ninject kitaplığı Ekle</span><span class="sxs-lookup"><span data-stu-id="3aee1-184">Add the Ninject Library</span></span>

<span data-ttu-id="3aee1-185">.NET için çok sayıda açık kaynaklı IOC kapsayıcı vardır.</span><span class="sxs-lookup"><span data-stu-id="3aee1-185">There are many open-source IoC containers for .NET.</span></span> <span data-ttu-id="3aee1-186">Bu öğretici için kullanacağım [Ninject](http://www.ninject.org/).</span><span class="sxs-lookup"><span data-stu-id="3aee1-186">For this tutorial, I'll use [Ninject](http://www.ninject.org/).</span></span> <span data-ttu-id="3aee1-187">(Diğer popüler kitaplıkları içerir [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), ve [StructureMap ](http://docs.structuremap.net).)</span><span class="sxs-lookup"><span data-stu-id="3aee1-187">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), and [StructureMap](http://docs.structuremap.net).)</span></span>

<span data-ttu-id="3aee1-188">Yüklemek için NuGet Paket Yöneticisi'ni kullanın [Ninject Kitaplığı](https://nuget.org/packages/Ninject/3.0.1.10).</span><span class="sxs-lookup"><span data-stu-id="3aee1-188">Use NuGet Package Manager to install the [Ninject library](https://nuget.org/packages/Ninject/3.0.1.10).</span></span> <span data-ttu-id="3aee1-189">Visual Studio'da gelen **Araçları** menüsünü seçin **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="3aee1-189">In Visual Studio, from the **Tools** menu select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="3aee1-190">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="3aee1-190">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a><span data-ttu-id="3aee1-191">SignalR bağımlılık çözümleyiciyi değiştirin</span><span class="sxs-lookup"><span data-stu-id="3aee1-191">Replace the SignalR Dependency Resolver</span></span>

<span data-ttu-id="3aee1-192">SignalR içinde Ninject kullanmak için türetilen bir sınıf oluşturma **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="3aee1-192">To use Ninject within SignalR, create a class that derives from **DefaultDependencyResolver**.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

<span data-ttu-id="3aee1-193">Bu sınıf geçersiz kılmalar **GetService** ve **GetServices** yöntemlerinin **DefaultDependencyResolver**.</span><span class="sxs-lookup"><span data-stu-id="3aee1-193">This class overrides the **GetService** and **GetServices** methods of **DefaultDependencyResolver**.</span></span> <span data-ttu-id="3aee1-194">SignalR hub örneklerinin yanı sıra, SignalR tarafından dahili olarak kullanılan çeşitli Hizmetleri dahil olmak üzere çalışma zamanında, çeşitli nesneleri oluşturmak için bu yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="3aee1-194">SignalR calls these methods to create various objects at runtime, including hub instances, as well as various services used internally by SignalR.</span></span>

- <span data-ttu-id="3aee1-195">**GetService** yöntemi, bir türün tek bir örneğini oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3aee1-195">The **GetService** method creates a single instance of a type.</span></span> <span data-ttu-id="3aee1-196">Bu yöntemin Ninject çekirdek 's çağırmaya **TryGet** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="3aee1-196">Override this method to call the Ninject kernel's **TryGet** method.</span></span> <span data-ttu-id="3aee1-197">Bu yöntem null değeri döndürür, varsayılan çözümleyici için geri döner.</span><span class="sxs-lookup"><span data-stu-id="3aee1-197">If that method returns null, fall back to the default resolver.</span></span>
- <span data-ttu-id="3aee1-198">**GetServices** yöntemi, belirtilen bir türün nesnelerinin bir koleksiyonunu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3aee1-198">The **GetServices** method creates a collection of objects of a specified type.</span></span> <span data-ttu-id="3aee1-199">Ninject sonuçlardan varsayılan çözümleyici sonuçları ile birleştirmek için bu yöntemi yok sayın.</span><span class="sxs-lookup"><span data-stu-id="3aee1-199">Override this method to concatenate the results from Ninject with the results from the default resolver.</span></span>

## <a name="configure-ninject-bindings"></a><span data-ttu-id="3aee1-200">Ninject bağlamaları yapılandırma</span><span class="sxs-lookup"><span data-stu-id="3aee1-200">Configure Ninject Bindings</span></span>

<span data-ttu-id="3aee1-201">Artık türü bağlamaları bildirmek için Ninject kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="3aee1-201">Now we'll use Ninject to declare type bindings.</span></span>

<span data-ttu-id="3aee1-202">Uygulamanızın Startup.cs sınıfını açın (ya da el ile paket içindeki yönergeler doğrultusunda oluşturduğunuz `readme.txt`, veya oluşturulmuş projenize kimlik doğrulaması ekleyerek).</span><span class="sxs-lookup"><span data-stu-id="3aee1-202">Open your application's Startup.cs class (that you either created manually as per the package instructions in `readme.txt`, or that was created by adding authentication to your project).</span></span> <span data-ttu-id="3aee1-203">İçinde `Startup.Configuration` yöntemi çağıran Ninject Ninject kapsayıcı oluşturma *çekirdek*.</span><span class="sxs-lookup"><span data-stu-id="3aee1-203">In the `Startup.Configuration` method, create the Ninject container, which Ninject calls the *kernel*.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

<span data-ttu-id="3aee1-204">Bizim özel bağımlılık Çözümleyicisi örneği oluşturun:</span><span class="sxs-lookup"><span data-stu-id="3aee1-204">Create an instance of our custom dependency resolver:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

<span data-ttu-id="3aee1-205">İçin bir bağlama oluşturun `IStockTicker` gibi:</span><span class="sxs-lookup"><span data-stu-id="3aee1-205">Create a binding for `IStockTicker` as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

<span data-ttu-id="3aee1-206">Bu kod, iki şey diyor.</span><span class="sxs-lookup"><span data-stu-id="3aee1-206">This code is saying two things.</span></span> <span data-ttu-id="3aee1-207">İlki, uygulamayı gerektiğinde bir `IStockTicker`, çekirdek örneğini oluşturmalısınız `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="3aee1-207">First, whenever the application needs an `IStockTicker`, the kernel should create an instance of `StockTicker`.</span></span> <span data-ttu-id="3aee1-208">İkinci olarak, `StockTicker` sınıfı tekil nesnesi olarak oluşturulmuş olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="3aee1-208">Second, the `StockTicker` class should be a created as a singleton object.</span></span> <span data-ttu-id="3aee1-209">Ninject nesnesinin bir kopyasını oluşturmak ve her istek için aynı örnek döndürülecektir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-209">Ninject will create one instance of the object, and return the same instance for each request.</span></span>

<span data-ttu-id="3aee1-210">İçin bir bağlama oluşturun **IHubConnectionContext** gibi:</span><span class="sxs-lookup"><span data-stu-id="3aee1-210">Create a binding for **IHubConnectionContext** as follows:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

<span data-ttu-id="3aee1-211">Bu kod creatres döndüren bir anonim işlev bir **IHubConnection**.</span><span class="sxs-lookup"><span data-stu-id="3aee1-211">This code creatres an anonymous function that returns an **IHubConnection**.</span></span> <span data-ttu-id="3aee1-212">**WhenInjectedInto** yöntemi yalnızca oluştururken bu işlevi kullanmak için Ninject söyler `IStockTicker` örnekleri.</span><span class="sxs-lookup"><span data-stu-id="3aee1-212">The **WhenInjectedInto** method tells Ninject to use this function only when creating `IStockTicker` instances.</span></span> <span data-ttu-id="3aee1-213">SignalR oluşturduğu nedeni **IHubConnectionContext** dahili olarak, örnekler ve SignalR bunları nasıl oluşturduğunu geçersiz kılmak istemiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="3aee1-213">The reason is that SignalR creates **IHubConnectionContext** instances internally, and we don't want to override how SignalR creates them.</span></span> <span data-ttu-id="3aee1-214">Bu işlevi yalnızca uygular bizim `StockTicker` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3aee1-214">This function only applies to our `StockTicker` class.</span></span>

<span data-ttu-id="3aee1-215">Bağımlılık çözümleyiciyi içine geçirmek **MapSignalR** bir hub yapılandırmasını ekleyerek yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3aee1-215">Pass the dependency resolver into the **MapSignalR** method by adding a hub configuration:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

<span data-ttu-id="3aee1-216">Örnek kullanıcının başlangıç sınıfındaki Startup.ConfigureSignalR yöntemi yeni bir parametre ile güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="3aee1-216">Update the Startup.ConfigureSignalR method in the sample's Startup class with the new parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

<span data-ttu-id="3aee1-217">SignalR belirtilen çözümleyici kullanacağı artık **MapSignalR**, yerine varsayılan çözümleyici.</span><span class="sxs-lookup"><span data-stu-id="3aee1-217">Now SignalR will use the resolver specified in **MapSignalR**, instead of the default resolver.</span></span>

<span data-ttu-id="3aee1-218">İçin tam kodu işte `Startup.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="3aee1-218">Here is the complete code listing for `Startup.Configuration`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

<span data-ttu-id="3aee1-219">Visual Studio'da StockTicker uygulamayı çalıştırmak için F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="3aee1-219">To run the StockTicker application in Visual Studio, press F5.</span></span> <span data-ttu-id="3aee1-220">Tarayıcı penceresinde gidin `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span><span class="sxs-lookup"><span data-stu-id="3aee1-220">In the browser window, navigate to `http://localhost:*port*/SignalR.Sample/StockTicker.html`.</span></span>

![](dependency-injection/_static/image3.png)

<span data-ttu-id="3aee1-221">Uygulamayı önce olarak tam olarak aynı işlevlere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="3aee1-221">The application has exactly the same functionality as before.</span></span> <span data-ttu-id="3aee1-222">(Bir açıklaması için bkz. [ASP.NET SignalR ile sunucu yayını](../getting-started/tutorial-server-broadcast-with-signalr.md).) Davranışı değiştirdik henüz; yalnızca kod sınamak için korumak ve geliştirmek daha kolay.</span><span class="sxs-lookup"><span data-stu-id="3aee1-222">(For a description, see [Server Broadcast with ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).) We haven't changed the behavior; just made the code easier to test, maintain, and evolve.</span></span>
