---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Öğretici: SignalR 2 ile sunucu yayını | Microsoft Docs'
author: tdykstra
description: Bu öğreticide, ASP.NET SignalR 2 sunucu yayın işlevselliği sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a243c78c7d552f1c82a88c6083871fcd16538618
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837435"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="7ad58-103">Öğretici: SignalR 2 ile yayın sunucusu</span><span class="sxs-lookup"><span data-stu-id="7ad58-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="7ad58-104">Bu öğreticide, ASP.NET SignalR 2 sunucu yayın işlevselliği sağlamak için kullanan bir web uygulaması oluşturma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="7ad58-105">Sunucu yayın sunucuyu istemcilere iletişimler başlatır anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="7ad58-106">Bu öğreticide oluşturacağınız uygulama bir bandı yayın sunucusu işlevselliği için tipik bir senaryo benzetimini yapar.</span><span class="sxs-lookup"><span data-stu-id="7ad58-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="7ad58-107">Düzenli olarak, sunucu rastgele hisse senedi fiyatlarına güncelleştirir ve bağlanan tüm istemciler için güncelleştirmeleri yayınlayın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="7ad58-108">Tarayıcı, sayılar ve semboller **değiştirme** ve **%** sütunları dinamik olarak değiştirme sunucudan yanıt bildirimleri.</span><span class="sxs-lookup"><span data-stu-id="7ad58-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="7ad58-109">Aynı URL'ye ek tarayıcılar açarsanız, bunların tümü aynı verilere ve verilere aynı değişiklikleri aynı anda gösterin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Web oluşturma](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="7ad58-111">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7ad58-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7ad58-112">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad58-112">Create the project</span></span>
> * <span data-ttu-id="7ad58-113">Sunucu kodunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="7ad58-113">Set up the server code</span></span>
> * <span data-ttu-id="7ad58-114">Sunucu kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="7ad58-114">Examine the server code</span></span>
> * <span data-ttu-id="7ad58-115">İstemci kodunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="7ad58-115">Set up the client code</span></span>
> * <span data-ttu-id="7ad58-116">İstemci kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="7ad58-116">Examine the client code</span></span>
> * <span data-ttu-id="7ad58-117">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="7ad58-117">Test the application</span></span>
> * <span data-ttu-id="7ad58-118">Günlü kaydını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="7ad58-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ad58-119">Uygulama oluşturma adımlarında size çalışmak istemiyorsanız, yeni bir boş bir ASP.NET Web uygulaması projesinde SignalR.Sample paketini yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="7ad58-120">Bu öğreticideki adımları gerçekleştirmeden NuGet paketi yüklerseniz, yönergeleri izlemelidir *readme.txt* dosya.</span><span class="sxs-lookup"><span data-stu-id="7ad58-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="7ad58-121">Bir OWIN başlangıç eklemeniz paketini çalıştırmak için hangi çağrıları sınıfı `ConfigureSignalR` yüklenen paketteki yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7ad58-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="7ad58-122">OWIN başlangıç sınıfı eklemezseniz bir hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="7ad58-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="7ad58-123">Bkz: [StockTicker örneği yüklemek](#install-the-stockticker-sample) bu makalenin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7ad58-124">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="7ad58-124">Prerequisites</span></span>

 * <span data-ttu-id="7ad58-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü.</span><span class="sxs-lookup"><span data-stu-id="7ad58-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="7ad58-126">Projeyi oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad58-126">Create the project</span></span>

<span data-ttu-id="7ad58-127">Bu bölümde, boş bir ASP.NET Web uygulaması oluşturmak için Visual Studio 2017'yi kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="7ad58-128">Visual Studio kullanarak ASP.NET Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7ad58-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Web oluşturma](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="7ad58-130">İçinde **yeni ASP.NET Web uygulaması - SignalR.StockTicker** penceresinde bırakın **boş** seçin ve seçilen **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="7ad58-131">Sunucu kodunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="7ad58-131">Set up the server code</span></span>

<span data-ttu-id="7ad58-132">Bu bölümde, sunucu üzerinde çalışan kodu ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="7ad58-133">Stok sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad58-133">Create the Stock class</span></span>

<span data-ttu-id="7ad58-134">Oluşturarak başlayın *hisse senedi* model depolamak ve stok hakkında bilgi iletmek için kullanacağınız sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7ad58-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="7ad58-135">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="7ad58-136">Sınıf adı *hisse senedi* ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="7ad58-137">Değiştirin *Stock.cs* Bu kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="7ad58-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="7ad58-138">Hisse oluştururken ayarladığınız iki özellik `Symbol` (örneğin, Microsoft MSFT) ve `Price`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="7ad58-139">Diğer özellikleri nasıl ve ne zaman ayarlamak bağımlı `Price`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="7ad58-140">İlk kez ayarladığınız `Price`, yayıldığı `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="7ad58-141">Bundan sonra ayarladığınızda `Price`, uygulamayı hesaplar `Change` ve `PercentChange` arasındaki fark temel özellik değerlerini `Price` ve `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="7ad58-142">StockTickerHub ve StockTicker sınıfları oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad58-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="7ad58-143">SignalR hub'ı API sunucusu istemci etkileşimi işlemek için kullanacaksınız.</span><span class="sxs-lookup"><span data-stu-id="7ad58-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="7ad58-144">A `StockTickerHub` türetilen sınıf `SignalRHub` sınıfı, bağlantı ve yöntem çağrıları istemcilerinden gelen işleyecek.</span><span class="sxs-lookup"><span data-stu-id="7ad58-144">A `StockTickerHub` class that derives from the `SignalRHub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="7ad58-145">Aynı zamanda Stok verileri korumak ve çalıştırmak gereken bir `Timer` nesne.</span><span class="sxs-lookup"><span data-stu-id="7ad58-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="7ad58-146">`Timer` Nesne düzenli aralıklarla fiyat güncelleştirmeleri istemci bağlantıları bağımsız tetikleyin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="7ad58-147">Bu işlevler konulamıyor bir `Hub` çünkü Hubs'a geçicidir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="7ad58-148">Uygulamayı oluşturur bir `Hub` bağlantıları ve istemciden sunucuya çağrılar gibi hub'ında her görev için sınıf örneği.</span><span class="sxs-lookup"><span data-stu-id="7ad58-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="7ad58-149">Bu nedenle ayrı bir sınıf içinde çalıştırmak stok verileri tutar, fiyatları güncelleştirir ve fiyat güncelleştirmeleri yayınlar mekanizması vardır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="7ad58-150">Sınıf adı `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-150">You'll name the class `StockTicker`.</span></span>

![Yayın StockTicker gelen](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="7ad58-152">Yalnızca bir örneğini istediğiniz `StockTicker` sınıfı, bu nedenle her bir başvuru ayarlamanız gerekir sunucuda çalıştırılacak `StockTickerHub` tekli örneğine `StockTicker` örneği.</span><span class="sxs-lookup"><span data-stu-id="7ad58-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="7ad58-153">`StockTicker` Sınıfında stok verileri içeren ve güncelleştirmeleri Tetikleyiciler olduğundan istemcilere yayın ancak `StockTicker` değil bir `Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7ad58-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="7ad58-154">`StockTicker` Sınıfında SignalR hub'ı bağlantı kapsamı nesnesine bir başvuru almak.</span><span class="sxs-lookup"><span data-stu-id="7ad58-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="7ad58-155">Bunu daha sonra SignalR bağlantı bağlam nesnesi istemcilere yayınlamak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="7ad58-156">Create StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="7ad58-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="7ad58-157">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7ad58-158">İçinde **Yeni Öğe Ekle - SignalR.StockTicker**seçin **yüklü** > **Visual C#**   >  **Web**  >  **SignalR** seçip **SignalR Hub sınıfı (v2)**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="7ad58-159">Sınıf adı *StockTickerHub* ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="7ad58-160">Bu adımda oluşturulur *StockTickerHub.cs* sınıf dosyası.</span><span class="sxs-lookup"><span data-stu-id="7ad58-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="7ad58-161">Eşzamanlı olarak projeye SignalR destekleyen bir dizi komut dosyaları ve derleme başvurularını ekler.</span><span class="sxs-lookup"><span data-stu-id="7ad58-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="7ad58-162">Değiştirin *StockTickerHub.cs* Bu kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="7ad58-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="7ad58-163">Dosyayı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-163">Save the file.</span></span>

<span data-ttu-id="7ad58-164">Uygulamanın kullandığı [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) istemcilerin sunucuda çağırabilir yöntemleri tanımlamak için sınıf.</span><span class="sxs-lookup"><span data-stu-id="7ad58-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="7ad58-165">Bir yöntem tanımlayacağınız: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="7ad58-166">Bir istemci sunucuya ilk kez bağlandığında, geçerli fiyatları ile stocks tümünün listesini almak için bu yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="7ad58-167">Yöntem zaman uyumlu olarak çalıştırabilir ve dönüş `IEnumerable<Stock>` nedeniyle bellekten veri döndürüyor.</span><span class="sxs-lookup"><span data-stu-id="7ad58-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="7ad58-168">Yöntem bir veritabanı araması veya bir web hizmeti çağrısı gibi bir bekleme kapsayacaktır bir şey yaparak verileri almak sahip olduğu belirtirsiniz `Task<IEnumerable<Stock>>` zaman uyumsuz işleme etkinleştirmek için dönüş değeri.</span><span class="sxs-lookup"><span data-stu-id="7ad58-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="7ad58-169">Daha fazla bilgi için [ASP.NET SignalR Hubs API Kılavuzu - sunucu - zaman zaman uyumsuz olarak yürütülecek](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="7ad58-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="7ad58-170">`HubName` Özniteliği, uygulama istemcide JavaScript kod hub'ı nasıl Bakacağınız belirtir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="7ad58-171">Bu öznitelik kullanmazsanız istemcideki varsayılan adı camelCase bu durumda sınıf adı sürümüdür `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="7ad58-172">Daha sonra göreceğiniz üzere oluşturduğunuzda `StockTicker` sınıfı uygulaması oluşturur, sınıf tekil örneğini, statik `Instance` özelliği.</span><span class="sxs-lookup"><span data-stu-id="7ad58-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="7ad58-173">Tekil örneğini `StockTicker` kaç adet istemcinin bağlanın veya bağlantıyı kesin ne olursa olsun bellek içindedir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="7ad58-174">Bu örneği nedir `GetAllStocks()` yöntemi geçerli hisse senedi bilgi almak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="7ad58-175">StockTicker.cs oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad58-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="7ad58-176">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="7ad58-177">Sınıf adı *StockTicker* ve projeye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="7ad58-178">Değiştirin *StockTicker.cs* Bu kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="7ad58-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="7ad58-179">Tüm iş parçacıklarının StockTicker kod'ın aynı örneğinin çalışacağı beri StockTicker sınıfı iş parçacığı açısından güvenli olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="7ad58-180">Sunucu kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="7ad58-180">Examine the server code</span></span>

<span data-ttu-id="7ad58-181">Sunucu kodu inceleyin, uygulamanın nasıl çalıştığını anlamanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="7ad58-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="7ad58-182">Statik bir alana tekil örneğini depolama</span><span class="sxs-lookup"><span data-stu-id="7ad58-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="7ad58-183">Statik kod başlatır `_instance` yedekler alan `Instance` özelliğiyle sınıfının örneği.</span><span class="sxs-lookup"><span data-stu-id="7ad58-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="7ad58-184">Oluşturucusu özel olduğundan, uygulama oluşturabileceğiniz sınıfın yalnızca örneğidir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="7ad58-185">Uygulamanın kullandığı [yavaş başlatma](/dotnet/framework/performance/lazy-initialization) için `_instance` alan.</span><span class="sxs-lookup"><span data-stu-id="7ad58-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="7ad58-186">Performansla ilgili nedenlerden dolayı değil.</span><span class="sxs-lookup"><span data-stu-id="7ad58-186">It's not for performance reasons.</span></span> <span data-ttu-id="7ad58-187">Bu örnek oluşturma iş parçacığı açısından güvenli olduğundan emin olmaktır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="7ad58-188">Bir istemci sunucuya her bağlandığında StockTicker tekil örnek içinde ayrı bir iş parçacığı çalıştırırken StockTickerHub sınıfının yeni bir örneğini alır `StockTicker.Instance` statik özelliği gördüğünüz daha önce `StockTickerHub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7ad58-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="7ad58-189">Bir ConcurrentDictionary stok verileri depolama</span><span class="sxs-lookup"><span data-stu-id="7ad58-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="7ad58-190">Oluşturucu başlatır `_stocks` bazı örnek verilerle stok, koleksiyon ve `GetAllStocks` hisse senetleri döner.</span><span class="sxs-lookup"><span data-stu-id="7ad58-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="7ad58-191">Daha önce bahsettiğim gibi Bu hisse topluluğu tarafından döndürülen `StockTickerHub.GetAllStocks`, bir sunucu yöntemde olduğu `Hub` istemcileri çağırabilirsiniz sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7ad58-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="7ad58-192">Hisse koleksiyon olarak tanımlanan bir [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) türü için iş parçacığı güvenliği.</span><span class="sxs-lookup"><span data-stu-id="7ad58-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="7ad58-193">Alternatif olarak, kullanabileceğinizi bir [sözlük](https://msdn.microsoft.com/library/xfhwa508.aspx) nesne ve ona değişiklikler yaptığınızda sözlük açıkça kilitleme.</span><span class="sxs-lookup"><span data-stu-id="7ad58-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="7ad58-194">Bu örnek uygulama için Tamam bellekte uygulama verilerini depolamak ve uygulama, siler, veri kaybına neden olduğu `StockTicker` örneği.</span><span class="sxs-lookup"><span data-stu-id="7ad58-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="7ad58-195">Gerçek bir uygulamada bir veritabanı gibi bir arka uç veri deposu ile işe yarar.</span><span class="sxs-lookup"><span data-stu-id="7ad58-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="7ad58-196">Hisse senedi fiyatlarına düzenli aralıklarla güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="7ad58-196">Periodically updating stock prices</span></span>

<span data-ttu-id="7ad58-197">Oluşturucu başlatılırken bir `Timer` nesnesini düzenli aralıklarla hisse senedi fiyatlarına rastgele olarak güncelleştiren yöntemleri çağırır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="7ad58-198">`Timer` çağrıları `UpdateStockPrices`, hangi state parametresi null geçirir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="7ad58-199">Uygulama kilidi vereceğine fiyatları güncelleştirmeden önce `_updateStockPricesLock` nesne.</span><span class="sxs-lookup"><span data-stu-id="7ad58-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="7ad58-200">Başka bir iş parçacığı zaten fiyatları güncelleştiriyor ve onu çağıran kodu denetler `TryUpdateStockPrice` listedeki her stoktaki.</span><span class="sxs-lookup"><span data-stu-id="7ad58-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="7ad58-201">`TryUpdateStockPrice` Yöntemi hisse senedi fiyatının değiştirip değiştirmemeye karar verir ve değiştirmek için ne kadar.</span><span class="sxs-lookup"><span data-stu-id="7ad58-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="7ad58-202">Hisse senedi fiyatının değişirse, uygulamayı çağırır `BroadcastStockPrice` bağlı istemcileri hisse senedi fiyatı değişiklik tüm yayın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="7ad58-203">`_updatingStockPrices` Atanan bayrağı [geçici](https://msdn.microsoft.com/library/x13ttww7.aspx) iş parçacığı açısından güvenli olduğundan emin olmak için.</span><span class="sxs-lookup"><span data-stu-id="7ad58-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="7ad58-204">Gerçek bir uygulamada `TryUpdateStockPrice` yöntemi fiyatı aramak için bir web hizmeti çağrısı.</span><span class="sxs-lookup"><span data-stu-id="7ad58-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="7ad58-205">Bu kodda, uygulama değişiklik rastgele bir rasgele sayı üreteci kullanır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="7ad58-206">Böylece StockTicker sınıfı istemcilere yayınlayabilirsiniz SignalR bağlamı alma</span><span class="sxs-lookup"><span data-stu-id="7ad58-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="7ad58-207">Fiyat değişiklikleri de burada kaynaklı olduğundan `StockTicker` nesnesini çağırmak için gereken nesne olduğu bir `updateStockPrice` bağlanan tüm istemciler yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7ad58-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="7ad58-208">İçinde bir `Hub` sınıf, sahip olduğunuz bir API İstemci yöntemleri çağırmak için ancak `StockTicker` öğesinden türetilen değil `Hub` sınıfı ve herhangi bir başvuru yoktur `Hub` nesne.</span><span class="sxs-lookup"><span data-stu-id="7ad58-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="7ad58-209">Bağlı istemciler için yayın `StockTicker` sınıfında SignalR bağlam örneğinin `StockTickerHub` sınıfı ve bu istemcilerde yöntemleri çağırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="7ad58-210">Başvuran geçişleri oluşturucuya tekil sınıfı örneğini oluşturduğunda, kod SignalR bağlamı için bir başvuru alır ve oluşturucu koyar `Clients` özelliği.</span><span class="sxs-lookup"><span data-stu-id="7ad58-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="7ad58-211">Yalnızca bir kez bağlamı almak istediğiniz neden iki nedeni vardır: bağlamı alma, pahalı bir görev ve yapar emin uygulamayı hedeflenen istemcilere gönderilen iletilerin sırasını korur. sonra alınıyor.</span><span class="sxs-lookup"><span data-stu-id="7ad58-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="7ad58-212">Başlama `Clients` bağlamı ve yerleştirme özellik `StockTickerClient` özelliği, içinde olduğu gibi aynı görünür istemci yöntemleri çağırmak için kod yazmanıza olanak sağlar bir `Hub` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7ad58-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="7ad58-213">Örneğin, tüm istemcilere, yayın yazabilirsiniz `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="7ad58-214">`updateStockPrice` Numarasını arıyoruz yöntemi `BroadcastStockPrice` henüz mevcut değil.</span><span class="sxs-lookup"><span data-stu-id="7ad58-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="7ad58-215">İstemcide çalışan kod yazdığınızda, daha sonra ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="7ad58-216">Başvurabilirsiniz `updateStockPrice` burada çünkü `Clients.All` uygulama çalışma zamanında ifade değerlendirme yani dinamik olduğundan.</span><span class="sxs-lookup"><span data-stu-id="7ad58-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="7ad58-217">Bu yöntem çağrısının yürütüldüğünde, SignalR yöntem adı ve parametre değeri istemciye gönderir ve istemci adlı yöntemi varsa `updateStockPrice`, uygulama bu yöntemi çağırın ve parametre değerini geçirin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="7ad58-218">`Clients.All` tüm istemcilere Gönder anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="7ad58-219">SignalR, hangi istemcilerin veya göndermek için istemci gruplarının belirtmek için diğer seçenekler sunar.</span><span class="sxs-lookup"><span data-stu-id="7ad58-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="7ad58-220">Daha fazla bilgi için [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="7ad58-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="7ad58-221">SignalR yol kaydetme</span><span class="sxs-lookup"><span data-stu-id="7ad58-221">Register the SignalR route</span></span>

<span data-ttu-id="7ad58-222">Sunucunun kesecek ve SignalR ile doğrudan hangi URL'sini de bilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="7ad58-223">Bunu yapmak için OWIN başlangıç sınıfı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7ad58-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="7ad58-224">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="7ad58-225">İçinde **Yeni Öğe Ekle - SignalR.StockTicker** seçin **yüklü** > **Visual C#**   >  **Web** ve ardından **OWIN başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="7ad58-226">Sınıf adı *başlangıç* seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="7ad58-227">Varsayılan kodda değiştirin *Startup.cs* Bu kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="7ad58-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="7ad58-228">Artık sunucu kodu ayarlama tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="7ad58-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="7ad58-229">Sonraki bölümde, istemci ayarlarsınız.</span><span class="sxs-lookup"><span data-stu-id="7ad58-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="7ad58-230">İstemci kodunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="7ad58-230">Set up the client code</span></span>

<span data-ttu-id="7ad58-231">Bu bölümde, istemcide çalışan kodu ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="7ad58-232">JavaScript dosyası ve HTML sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad58-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="7ad58-233">HTML sayfasını verileri görüntüler ve JavaScript dosyasını verileri düzenler.</span><span class="sxs-lookup"><span data-stu-id="7ad58-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="7ad58-234">StockTicker.html oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad58-234">Create StockTicker.html</span></span>

<span data-ttu-id="7ad58-235">İlk olarak, HTML istemcisi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="7ad58-236">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **HTML sayfası**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="7ad58-237">Dosya adı *StockTicker* seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="7ad58-238">Varsayılan kodda değiştirin *StockTicker.html* Bu kod dosyası:</span><span class="sxs-lookup"><span data-stu-id="7ad58-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="7ad58-239">HTML beş sütun, bir üst bilgi satırı ve tüm beş sütun kapsayan tek bir hücre içeren bir veri satırı bir tablo oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7ad58-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="7ad58-240">Veri satırı, "kısa bir süre içinde uygulama başlatıldığında yükleniyor..." gösterir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="7ad58-241">JavaScript kodu satır kaldırın ve sunucudan alınan stok verilerini kendi yerde satır ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="7ad58-242">Komut dosyası etiketlerini belirtin:</span><span class="sxs-lookup"><span data-stu-id="7ad58-242">The script tags specify:</span></span>

    * <span data-ttu-id="7ad58-243">JQuery betik dosyası.</span><span class="sxs-lookup"><span data-stu-id="7ad58-243">The jQuery script file.</span></span>

    * <span data-ttu-id="7ad58-244">SignalR çekirdek betik dosyası.</span><span class="sxs-lookup"><span data-stu-id="7ad58-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="7ad58-245">SignalR proxy'leri betik dosyası.</span><span class="sxs-lookup"><span data-stu-id="7ad58-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="7ad58-246">Daha sonra oluşturacağınız StockTicker betik dosyası.</span><span class="sxs-lookup"><span data-stu-id="7ad58-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="7ad58-247">Uygulama, dinamik olarak SignalR proxy'leri betik dosyası oluşturur.</span><span class="sxs-lookup"><span data-stu-id="7ad58-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="7ad58-248">"/ Signalr/hubs" URL'sini belirtir ve proxy için Hub sınıfına, bu durumda, üzerinde yöntemleri için yöntemlerini `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="7ad58-249">Tercih ederseniz, bu JavaScript dosyasını el ile kullanarak oluşturabileceğiniz [SignalR yardımcı programları](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="7ad58-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="7ad58-250">Dinamik dosya oluşturma, devre dışı bırakmak unutmayın `MapHubs` yöntem çağrısı.</span><span class="sxs-lookup"><span data-stu-id="7ad58-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="7ad58-251">İçinde **Çözüm Gezgini**, genişletme **betikleri**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="7ad58-252">JQuery ve SignalR için betik kitaplıkları projesinde görünür.</span><span class="sxs-lookup"><span data-stu-id="7ad58-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="7ad58-253">Paket Yöneticisi SignalR betikleri daha sonraki bir sürümünü yükler.</span><span class="sxs-lookup"><span data-stu-id="7ad58-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="7ad58-254">Komut dosyalarını projedeki sürümleri değerine karşılık gelen kod bloğundaki komut dosyası başvuruları güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="7ad58-255">İçinde **Çözüm Gezgini**, sağ *StockTicker.html*ve ardından **Başlangıç Sayfası Ayarla**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="7ad58-256">Create StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="7ad58-256">Create StockTicker.js</span></span>

<span data-ttu-id="7ad58-257">Artık JavaScript dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7ad58-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="7ad58-258">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **Ekle** > **JavaScript dosyası**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="7ad58-259">Dosya adı *StockTicker* seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="7ad58-260">Bu kodu ekleyin *StockTicker.js* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7ad58-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="7ad58-261">İstemci kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="7ad58-261">Examine the client code</span></span>

<span data-ttu-id="7ad58-262">İstemci kodu inceleyin, istemci kodu, iş uygulaması yapmak için sunucu kodu ile nasıl etkileşime gireceğini öğrenmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="7ad58-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="7ad58-263">Bağlantı başlatılıyor</span><span class="sxs-lookup"><span data-stu-id="7ad58-263">Starting the connection</span></span>

<span data-ttu-id="7ad58-264">`$.connection` SignalR proxy'leri için ifade eder.</span><span class="sxs-lookup"><span data-stu-id="7ad58-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="7ad58-265">Kod proxy için bir başvuru edinir `StockTickerHub` sınıfı ve bunu koyar `ticker` değişkeni.</span><span class="sxs-lookup"><span data-stu-id="7ad58-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="7ad58-266">Ara sunucu adını ayarlayın tarafından addır `HubName` özniteliği:</span><span class="sxs-lookup"><span data-stu-id="7ad58-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="7ad58-267">Tüm değişkenlerin ve işlevlerin tanımladıktan sonra dosyayı kodda son satırının SignalR bağlantı SignalR çağırarak başlatır `start` işlevi.</span><span class="sxs-lookup"><span data-stu-id="7ad58-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="7ad58-268">`start` İşlev zaman uyumsuz olarak yürütür ve döndürür bir [jQuery ertelenmiş nesne](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="7ad58-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="7ad58-269">Uygulama, zaman uyumsuz eylem tamamlandığında çağırılacak işlev belirtmek için yapılan işlevi çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="7ad58-270">Tüm stocks alma</span><span class="sxs-lookup"><span data-stu-id="7ad58-270">Getting all the stocks</span></span>

<span data-ttu-id="7ad58-271">`init` İşlev çağrılarında `getAllStocks` işlevi sunucuya ve sunucu Stok tablosu bilgileri kullanır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="7ad58-272">Varsayılan olarak, yöntem adı sunucu üzerinde pascal harfleri olsa bile camelCasing istemcide kullanmak zorunda olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="7ad58-273">CamelCasing kural yalnızca nesneleri yöntemleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="7ad58-274">Örneğin, başvurmanız `stock.Symbol` ve `stock.Price`değil `stock.symbol` veya `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="7ad58-275">İçinde `init` yöntemi uygulaması oluşturur HTML çağırarak sunucudan alınan stok her nesne için bir tablo satır için `formatStock` biçimi özelliklerine `stock` nesnesi ve ardından çağırarak `supplant` içindekiyertutucularıdeğiştirmekiçin`rowTemplate` değişken ile `stock` nesnesi özellik değerleri.</span><span class="sxs-lookup"><span data-stu-id="7ad58-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="7ad58-276">Sonuçta elde edilen HTML sonra stok tablosuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="7ad58-277">Çağırmanızı `init` bunu olarak geçirerek bir `callback` sonra zaman uyumsuz yürütülen işlevi `start` işlev biter.</span><span class="sxs-lookup"><span data-stu-id="7ad58-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="7ad58-278">Aradığınız varsa `init` arama sonra ayrı bir JavaScript ifadesi olarak `start`, bağlantı kurma tamamlanması için başlangıç işlevi beklenmeden hemen çalışır çünkü işlev başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="7ad58-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="7ad58-279">Bu durumda, `init` işlevi çağırmak deneyin `getAllStocks` uygulama sunucu bağlantısı kurmadan önce işlev.</span><span class="sxs-lookup"><span data-stu-id="7ad58-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="7ad58-280">Güncelleştirilmiş hisse senedi fiyatlarına alma</span><span class="sxs-lookup"><span data-stu-id="7ad58-280">Getting updated stock prices</span></span>

<span data-ttu-id="7ad58-281">Sunucu hisse ait fiyat değiştiğinde çağırdığı `updateStockPrice` bağlı istemciler üzerinde.</span><span class="sxs-lookup"><span data-stu-id="7ad58-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="7ad58-282">Uygulama istemci özelliğine işlevi ekler `stockTicker` çağrıları sunucudan kullanabilmesi için proxy.</span><span class="sxs-lookup"><span data-stu-id="7ad58-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="7ad58-283">`updateStockPrice` Sunucudan alınan bir tabloya bir stok satır olarak aynı şekilde işlev biçimleri `init` işlevi.</span><span class="sxs-lookup"><span data-stu-id="7ad58-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="7ad58-284">Tabloya satır ekleme, yerine bu tabloda hisse senedi'nın geçerli satırı bulur ve satır yenisiyle değiştirir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="7ad58-285">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="7ad58-285">Test the application</span></span>

<span data-ttu-id="7ad58-286">Emin olmak için uygulamayı test edebilirsiniz çalışmaktadır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="7ad58-287">Hisse senedi fiyatlarına geciktirmeye ile canlı Stok tablosu görüntülemek tüm tarayıcı pencerelerini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="7ad58-288">Araç çubuğunda, açma **betik hata ayıklamasını** ve ardından uygulamayı hata ayıklama modunda çalıştırmak için Yürüt düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Hata ayıklama modu ve Çalıştır'ı seçerek kullanıcı görüntüsü.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="7ad58-290">Görüntüleyen bir tarayıcı penceresi açılacaktır **hisse senedi tablo canlı**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="7ad58-291">Stok tablosu başlangıçta "yükleniyor..." satır, daha sonra kısa bir süre sonra uygulamayı ilk stok verileri gösterir ve değiştirmek hisse senedi fiyatlarına başlatın gösterir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="7ad58-292">Tarayıcıdan URL'yi kopyalayın, diğer iki tarayıcılar açın ve URL'leri adresi çubukları yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="7ad58-293">İlk stok görünen ilk tarayıcı ile aynıdır ve değişiklikler aynı anda gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="7ad58-294">Tüm tarayıcıları kapatın, yeni bir tarayıcı açın ve aynı URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="7ad58-295">Sunucuda çalıştırılacak StockTicker tekil nesnesi devam.</span><span class="sxs-lookup"><span data-stu-id="7ad58-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="7ad58-296">**Hisse senedi tablo canlı** stocks değiştirmeye devam gösterir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="7ad58-297">Rakamları değiştirmek tablonun ilk sıfır görmüyorum.</span><span class="sxs-lookup"><span data-stu-id="7ad58-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="7ad58-298">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="7ad58-299">Günlü kaydını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="7ad58-299">Enable logging</span></span>

<span data-ttu-id="7ad58-300">SignalR istemcide giderilmesine yardımcı olmak için etkinleştirebileceğiniz bir yerleşik günlük işleve sahiptir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="7ad58-301">Bu bölümde, günlüğü etkinleştirmek ve nasıl günlükleri SignalR kullanarak, aşağıdaki taşıma yöntemleri size gösteren örneklere bakın:</span><span class="sxs-lookup"><span data-stu-id="7ad58-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="7ad58-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), IIS 8 ve geçerli tarayıcılar tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="7ad58-303">[Sunucu tarafından gönderilen olayları](http://en.wikipedia.org/wiki/Server-sent_events), Internet Explorer dışındaki tarayıcıları tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="7ad58-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="7ad58-304">[Sonsuza kadar çerçeve](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), Internet Explorer tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="7ad58-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="7ad58-305">[AJAX uzun yoklama](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), tüm tarayıcılar tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="7ad58-306">Verilen herhangi bir bağlantısı için hem sunucu hem de istemci destekleyen en iyi bir aktarım yöntemi SignalR seçer.</span><span class="sxs-lookup"><span data-stu-id="7ad58-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="7ad58-307">Açık *StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="7ad58-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="7ad58-308">Vurgulanan bu dosyanın sonuna bağlantı başlatır kodundan hemen önce günlüğe kaydetmeyi etkinleştirmek için kod satırını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7ad58-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="7ad58-309">Tuşuna **F5** projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="7ad58-310">Tarayıcınızın geliştirici araçları penceresini açın ve konsol günlükleri görmek için seçin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="7ad58-311">SignalR aktarım yöntemi için yeni bir bağlantı anlaşması günlükleri görmek için sayfayı yenilemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="7ad58-312">Internet Explorer 10, Windows 8'de (IIS 8) çalıştırıyorsanız, aktarım yöntemdir **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="7ad58-313">Internet Explorer 10, Windows 7'de (IIS 7.5) çalıştırıyorsanız, aktarım yöntemdir **iframe**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="7ad58-314">Windows 8 (IIS 8)'de Firefox 19 çalıştırıyorsanız, aktarım yöntemdir **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="7ad58-315">' De Firefox, Firebug bir konsol penceresi almak için eklentisini yükleyin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="7ad58-316">Windows 7 (IIS 7.5)'de Firefox 19 çalıştırıyorsanız, aktarım yöntemdir **sunucu tarafından gönderilen** olayları.</span><span class="sxs-lookup"><span data-stu-id="7ad58-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="7ad58-317">StockTicker örnek yükleyin</span><span class="sxs-lookup"><span data-stu-id="7ad58-317">Install the StockTicker sample</span></span>

<span data-ttu-id="7ad58-318">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) StockTicker uygulamayı yükler.</span><span class="sxs-lookup"><span data-stu-id="7ad58-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="7ad58-319">NuGet paketini sıfırdan oluşturduğunuz Basitleştirilmiş sürümden daha fazla özellik içerir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="7ad58-320">Öğreticinin bu bölümünde NuGet paketini yükleyin ve yeni özellikleri ve bunları uygulayan kod gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7ad58-321">Bu öğreticinin önceki adımları gerçekleştirmeden paketi yüklerseniz, OWIN başlangıç sınıfı projenize eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="7ad58-322">Bu NuGet paketi için readme.txt dosyasını, bu adım açıklanır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="7ad58-323">SignalR.Sample NuGet paketini yükle</span><span class="sxs-lookup"><span data-stu-id="7ad58-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="7ad58-324">İçinde **Çözüm Gezgini**, projeye sağ tıklayıp seçin **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="7ad58-325">İçinde **NuGet Paket Yöneticisi: SignalR.StockTicker**seçin **Gözat**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="7ad58-326">Gelen **paket kaynağı**seçin **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="7ad58-327">Girin *SignalR.Sample* seçip arama kutusuna **Microsoft.AspNet.SignalR.Sample** > **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="7ad58-328">İçinde **Çözüm Gezgini**, genişletme *SignalR.Sample* klasör.</span><span class="sxs-lookup"><span data-stu-id="7ad58-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="7ad58-329">SignalR.Sample paket yükleme klasörü ve içeriği oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7ad58-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="7ad58-330">İçinde *SignalR.Sample* klasörü sağ tıklatın *StockTicker.html*ve ardından **başlangıç sayfası olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7ad58-331">Yükleme SignalR.Sample NuGet paketi uygulamanızdaki jQuery sürümü değişebilir, *betikleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="7ad58-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="7ad58-332">Yeni *StockTicker.html* paketi yükler dosya *SignalR.Sample* klasörü orijinal çalıştırmakistiyorsanızancakyükleyenpaket,jQuerysürümüileuyumluolacak*StockTicker.html* yeniden dosya, komut dosyası etiketi jQuery başvurusunda önce güncelleştirmeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="7ad58-333">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="7ad58-333">Run the application</span></span>

 <span data-ttu-id="7ad58-334">Tablonun ilk uygulamada gördüğünüz kullanışlı özellikler vardı.</span><span class="sxs-lookup"><span data-stu-id="7ad58-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="7ad58-335">Yeni özellikler tam borsa uygulama gösterilmektedir: gelmek ve kalan rengini değiştirmek stocks ve stok verilerini gösteren bir yatay kaydırma pencere.</span><span class="sxs-lookup"><span data-stu-id="7ad58-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="7ad58-336">Tuşuna **F5** uygulamayı çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="7ad58-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="7ad58-337">Uygulamayı ilk kez çalıştırdığınızda, "pazar" "kapalı" ve statik bir tablo ve kaydırma değil bir bandı penceresi görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="7ad58-338">Seçin **açık Pazar**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-338">Select **Open Market**.</span></span>

    ![Dinamik sayaç ekran görüntüsü.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="7ad58-340">**Canlı hisse senedi bandı** seçenekler yatay kaydırma yapmak ve düzenli aralıklarla hisse senedi fiyatı değişiklikleri rastgele olarak yayınlamak sunucuyu başlatır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="7ad58-341">Her zaman bir hisse senedi fiyatı değiştirir, hem de uygulama güncelleştirmeleri **hisse senedi tablo canlı** ve **Canlı hisse senedi bandı**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="7ad58-342">Pozitif bir hisse senedi ait fiyat değişikliği olduğunda, uygulama yeşil bir arka plana sahip stok gösterir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="7ad58-343">Negatif bir değişiklik olduğunda, uygulama kırmızı bir arka plan ile stok gösterir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="7ad58-344">Seçin **kapatmak Pazar**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="7ad58-345">Tablo durdurma güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-345">The table updates stop.</span></span>

    * <span data-ttu-id="7ad58-346">Bandı kaydırma durdurulur.</span><span class="sxs-lookup"><span data-stu-id="7ad58-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="7ad58-347">Seçin **sıfırlama**.</span><span class="sxs-lookup"><span data-stu-id="7ad58-347">Select **Reset**.</span></span>

    * <span data-ttu-id="7ad58-348">Tüm stok verileri sıfırlanır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-348">All stock data is reset.</span></span>

    * <span data-ttu-id="7ad58-349">Uygulamayı kullanmaya fiyat değişiklikleriyle önce ilk durumuna geri yükler.</span><span class="sxs-lookup"><span data-stu-id="7ad58-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="7ad58-350">Tarayıcıdan URL'yi kopyalayın, diğer iki tarayıcılar açın ve URL'leri adresi çubukları yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="7ad58-351">Her tarayıcıda aynı anda dinamik olarak güncelleştirilen aynı verileri görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="7ad58-352">Denetimlerden herhangi birini seçtiğinizde, tüm tarayıcılar aynı anda aynı şekilde yanıt verin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="7ad58-353">Canlı hisse senedi bandı görüntüleme</span><span class="sxs-lookup"><span data-stu-id="7ad58-353">Live Stock Ticker display</span></span>

<span data-ttu-id="7ad58-354">**Canlı hisse senedi bandı** görüntülenir, sırasız bir listesini bir `<div>` öğesi tek bir satıra CSS stillerinden biçimlendirilmiş.</span><span class="sxs-lookup"><span data-stu-id="7ad58-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="7ad58-355">Uygulama başlatır ve bandı tablo aynı şekilde güncelleştirir: içindeki yer tutucuları değiştirerek bir `<li>` şablonu dizesi ve dinamik olarak ekleme `<li>` öğelerine `<ul>` öğesi.</span><span class="sxs-lookup"><span data-stu-id="7ad58-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="7ad58-356">JQuery kullanarak kaydırma uygulamasını içeren `animate` sırasız liste içinde sol kenar boşluğu değişen işlevi `<div>`.</span><span class="sxs-lookup"><span data-stu-id="7ad58-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="7ad58-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="7ad58-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="7ad58-358">HTML kod borsa kodu:</span><span class="sxs-lookup"><span data-stu-id="7ad58-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="7ad58-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="7ad58-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="7ad58-360">CSS kodunu borsa kodu:</span><span class="sxs-lookup"><span data-stu-id="7ad58-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="7ad58-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="7ad58-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="7ad58-362">Kolaylaştırır jQuery kodu kaydırın:</span><span class="sxs-lookup"><span data-stu-id="7ad58-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="7ad58-363">İstemci çağırabilirsiniz sunucuda ek yöntemleri</span><span class="sxs-lookup"><span data-stu-id="7ad58-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="7ad58-364">Uygulamaya esneklik kazandırmak için uygulamayı çağırabilir ek yöntemleri vardır.</span><span class="sxs-lookup"><span data-stu-id="7ad58-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="7ad58-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="7ad58-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="7ad58-366">`StockTickerHub` Sınıfı istemci çağırabilirsiniz dört ek yöntemleri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="7ad58-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="7ad58-367">Uygulama çağrıları `OpenMarket`, `CloseMarket`, ve `Reset` yanıt sayfanın üstündeki düğmeleri olarak.</span><span class="sxs-lookup"><span data-stu-id="7ad58-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="7ad58-368">Bunlar, tüm istemcilere hemen yayılan durumundaki bir değişikliği tetikleyen bir istemci desenini gösterir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="7ad58-369">Bu yöntemlerin her biri bir yöntem çağrıları `StockTicker` Pazar durum değişikliği neden olur ve ardından yeni durum yayınlar sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7ad58-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="7ad58-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="7ad58-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="7ad58-371">İçinde `StockTicker` sınıfı, uygulama ile Pazar durumunu korur bir `MarketState` döndüren özellik bir `MarketState` sabit listesi değeri:</span><span class="sxs-lookup"><span data-stu-id="7ad58-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="7ad58-372">Pazar durum değişikliği yöntemlerin her biri bunu kilit bloğu içinde olduğundan `StockTicker` sınıfında iş parçacığı açısından güvenli olmak üzere:</span><span class="sxs-lookup"><span data-stu-id="7ad58-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="7ad58-373">Bu kod, iş parçacığı açısından güvenli olduğundan emin olmak için `_marketState` yedekler alan `MarketState` özelliği belirlenmiş `volatile`:</span><span class="sxs-lookup"><span data-stu-id="7ad58-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="7ad58-374">`BroadcastMarketStateChange` Ve `BroadcastMarketReset` dışında istemcide tanımlanan farklı yöntemleri çağırma yöntemleri zaten gördüğünüz BroadcastStockPrice yöntemine benzer:</span><span class="sxs-lookup"><span data-stu-id="7ad58-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="7ad58-375">Sunucu çağırabilirsiniz istemcide ek işlevler</span><span class="sxs-lookup"><span data-stu-id="7ad58-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="7ad58-376">`updateStockPrice` İşlevi artık işler borsa kodu görüntüleme ve tablodan ve kullandığı `jQuery.Color` kırmızı ve yeşil renk Flash.</span><span class="sxs-lookup"><span data-stu-id="7ad58-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="7ad58-377">Yeni işlevleri *SignalR.StockTicker.js* etkinleştirin ve Pazar durumuna bağlı düğmeleri devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="7ad58-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="7ad58-378">Bunlar ayrıca durdurmak veya başlatmak **Canlı hisse senedi bandı** yatay kaydırma.</span><span class="sxs-lookup"><span data-stu-id="7ad58-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="7ad58-379">Birçok işlev için eklendiği `ticker.client`, uygulamanın kullandığı [jQuery işlevi genişletmek](http://api.jquery.com/jQuery.extend/) bunları eklemek için.</span><span class="sxs-lookup"><span data-stu-id="7ad58-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="7ad58-380">Bağlantı kurulduktan sonra ek istemci kurulumu</span><span class="sxs-lookup"><span data-stu-id="7ad58-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="7ad58-381">İstemci bağlantı kurar sonra yapmak için bazı ek işleri vardır:</span><span class="sxs-lookup"><span data-stu-id="7ad58-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="7ad58-382">Pazar açık veya kapalı uygun çağırmak için olup olmadığını öğrenmek `marketOpened` veya `marketClosed` işlevi.</span><span class="sxs-lookup"><span data-stu-id="7ad58-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="7ad58-383">Düğmeleri sunucu yöntemi çağrıları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="7ad58-384">Uygulama bağlantı kurar sonra sunucu yöntemleri kadar düğmeleri kadar kablolu değildir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="7ad58-385">Kullanılabilir gelmeden önce kod server yöntemlerini çağıramazsınız olduğundan.</span><span class="sxs-lookup"><span data-stu-id="7ad58-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ad58-386">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7ad58-386">Additional resources</span></span>

<span data-ttu-id="7ad58-387">Bu öğreticide iletileri sunucudan bağlanan tüm istemciler için yayınlar bir SignalR uygulama programı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="7ad58-388">Artık düzenli aralıklarla ve herhangi bir istemciden gelen bildirimlere yanıt iletilerini yayınlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="7ad58-389">Çok iş parçacıklı tekil örneğini kavramı, çok oyunculu çevrimiçi oyun senaryolarda sunucu durumunu korumak üzere kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7ad58-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="7ad58-390">Bir örnek için bkz. [ShootR game SignalR tabanlı](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="7ad58-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="7ad58-391">Eşler arası iletişim senaryolarında gösteren öğreticiler için bkz. [SignalR ile çalışmaya başlama](introduction-to-signalr.md) ve [SignalR ile gerçek zamanlı güncelleştirme](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="7ad58-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="7ad58-392">SignalR hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="7ad58-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="7ad58-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="7ad58-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="7ad58-394">SignalR projesi</span><span class="sxs-lookup"><span data-stu-id="7ad58-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="7ad58-395">SignalR GitHub ve örnekleri</span><span class="sxs-lookup"><span data-stu-id="7ad58-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="7ad58-396">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="7ad58-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="7ad58-397">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="7ad58-397">Next steps</span></span>

<span data-ttu-id="7ad58-398">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="7ad58-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7ad58-399">Proje oluşturuldu</span><span class="sxs-lookup"><span data-stu-id="7ad58-399">Created the project</span></span>
> * <span data-ttu-id="7ad58-400">Sunucu kodunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="7ad58-400">Set up the server code</span></span>
> * <span data-ttu-id="7ad58-401">Sunucu kodu incelenir.</span><span class="sxs-lookup"><span data-stu-id="7ad58-401">Examined the server code</span></span>
> * <span data-ttu-id="7ad58-402">İstemci kodunu ayarlayın</span><span class="sxs-lookup"><span data-stu-id="7ad58-402">Set up the client code</span></span>
> * <span data-ttu-id="7ad58-403">İstemci dio</span><span class="sxs-lookup"><span data-stu-id="7ad58-403">Examined the client code</span></span>
> * <span data-ttu-id="7ad58-404">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="7ad58-404">Tested the application</span></span>
> * <span data-ttu-id="7ad58-405">Etkin günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="7ad58-405">Enabled logging</span></span>

<span data-ttu-id="7ad58-406">ASP.NET SignalR 2 kullanan bir gerçek zamanlı web uygulamasının nasıl oluşturulacağını öğrenmek için sonraki makaleye ilerleyin.</span><span class="sxs-lookup"><span data-stu-id="7ad58-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="7ad58-407">SignalR ile gerçek zamanlı bir web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7ad58-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)