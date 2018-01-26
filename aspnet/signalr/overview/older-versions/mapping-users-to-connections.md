---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "SignalR bağlantıları için SignalR kullanıcıları eşlemek 1.x | Microsoft Docs"
author: pfletcher
description: "Bu konu, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri korumak nasıl gösterir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 896bf4142ce090e39ed5697ff053cd56728318ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="210b9-103">SignalR bağlantıları için SignalR kullanıcıları eşlemek 1.x</span><span class="sxs-lookup"><span data-stu-id="210b9-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="210b9-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="210b9-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="210b9-105">Bu konu, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri korumak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="210b9-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="210b9-106">Giriş</span><span class="sxs-lookup"><span data-stu-id="210b9-106">Introduction</span></span>

<span data-ttu-id="210b9-107">Bir hub'a bağlanan her istemci benzersiz bağlantı kimliği geçirir. Bu değeri alabilir `Context.ConnectionId` hub bağlamını özelliği.</span><span class="sxs-lookup"><span data-stu-id="210b9-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="210b9-108">Uygulamanız için bağlantı kimliği kullanıcısıyla ve bu eşlemenin kalıcı hale getirmek gerekirse, aşağıdakilerden birini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="210b9-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="210b9-109">[Bellek içi depolama](#inmemory), bir sözlük gibi</span><span class="sxs-lookup"><span data-stu-id="210b9-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="210b9-110">Her kullanıcı için SignalR grubu</span><span class="sxs-lookup"><span data-stu-id="210b9-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="210b9-111">[Kalıcı ve dış depolama](#database)gibi bir veritabanı tablosu veya Azure tablo depolaması</span><span class="sxs-lookup"><span data-stu-id="210b9-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="210b9-112">Bu uygulamaların her biri, bu konu başlığı altında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="210b9-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="210b9-113">Kullandığınız `OnConnected`, `OnDisconnected`, ve `OnReconnected` yöntemlerinin `Hub` kullanıcı bağlantı durumunu izlemek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="210b9-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="210b9-114">Uygulamanız için en iyi yaklaşımı bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="210b9-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="210b9-115">Uygulamanızı barındıran web sunucularının sayısı.</span><span class="sxs-lookup"><span data-stu-id="210b9-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="210b9-116">Bağlı durumda kullanıcıların listesini almak gerekmediğini.</span><span class="sxs-lookup"><span data-stu-id="210b9-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="210b9-117">Olup uygulama veya sunucu yeniden başlatıldığında grup ve kullanıcı bilgileri kalıcı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="210b9-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="210b9-118">Bir dış sunucu çağırma gecikme süresi bir sorun olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="210b9-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="210b9-119">Bu noktalar için hangi yaklaşımın çalışır aşağıdaki tabloda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="210b9-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="210b9-120">Birden çok sunucu</span><span class="sxs-lookup"><span data-stu-id="210b9-120">More than one server</span></span> | <span data-ttu-id="210b9-121">Şu anda bağlı kullanıcıların listesini al</span><span class="sxs-lookup"><span data-stu-id="210b9-121">Get list of currently connected users</span></span> | <span data-ttu-id="210b9-122">Bilgileri yeniden başlatıldıktan sonra Sürdür</span><span class="sxs-lookup"><span data-stu-id="210b9-122">Persist information after restarts</span></span> | <span data-ttu-id="210b9-123">En iyi performans</span><span class="sxs-lookup"><span data-stu-id="210b9-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="210b9-124">Bellek içi</span><span class="sxs-lookup"><span data-stu-id="210b9-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="210b9-125">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="210b9-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="210b9-126">Kalıcı, dış</span><span class="sxs-lookup"><span data-stu-id="210b9-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="210b9-127">Bellek içi depolama</span><span class="sxs-lookup"><span data-stu-id="210b9-127">In-memory storage</span></span>

<span data-ttu-id="210b9-128">Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgileri bellekte depolanır sözlükte tutulacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="210b9-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="210b9-129">Sözlük kullanan bir `HashSet` bağlantı kimliği depolamak için. Herhangi bir anda bir kullanıcı birden fazla bağlantı SignalR uygulamaya sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="210b9-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="210b9-130">Örneğin, birden çok aygıt ya da birden fazla tarayıcı sekmesi üzerinden bağlı bir kullanıcı birden fazla bağlantı kimliği gerekir.</span><span class="sxs-lookup"><span data-stu-id="210b9-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="210b9-131">Uygulama kapanırsa, tüm bilgileri kaybolur, ancak kullanıcıların kendi bağlantılarını yeniden oluşturmak gibi yeniden doldurulur.</span><span class="sxs-lookup"><span data-stu-id="210b9-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="210b9-132">Ortamınızda birden fazla web sunucusu varsa her sunucu bağlantıları ayrı koleksiyonu gerekir çünkü bellek içi depolama çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="210b9-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="210b9-133">İlk örnek bağlantıları kullanıcılara eşleme yöneten bir sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="210b9-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="210b9-134">Anahtar Hashset'i için kullanıcının adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="210b9-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="210b9-135">Sonraki örnek bir hub'dan bağlantı eşleme sınıfının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="210b9-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="210b9-136">Sınıfının örneğini bir değişken adı depolanan `_connections`.</span><span class="sxs-lookup"><span data-stu-id="210b9-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="210b9-137">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="210b9-137">Single-user groups</span></span>

<span data-ttu-id="210b9-138">Her kullanıcı için bir grup oluşturun ve yalnızca o kullanıcıyı erişmek istediğinizde bu gruba ileti gönderme.</span><span class="sxs-lookup"><span data-stu-id="210b9-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="210b9-139">Her grubun adı, kullanıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="210b9-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="210b9-140">Bir kullanıcının birden çok bağlantı varsa, her bağlantı kimliği kullanıcı grubuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="210b9-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="210b9-141">Kullanıcı kestiğinde, el ile kullanıcı grubundan kaldırmamanız.</span><span class="sxs-lookup"><span data-stu-id="210b9-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="210b9-142">Bu eylem SignalR çerçevesi tarafından otomatik olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="210b9-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="210b9-143">Aşağıdaki örnekte nasıl tek kullanıcı gruplarına uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="210b9-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="210b9-144">Kalıcı ve dış depolama</span><span class="sxs-lookup"><span data-stu-id="210b9-144">Permanent, external storage</span></span>

<span data-ttu-id="210b9-145">Bu konuda nasıl bağlantı bilgilerini depolamak için bir veritabanı veya Azure tablo depolaması kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="210b9-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="210b9-146">Bu yaklaşım, her web sunucusu ile aynı veri deposu etkileşim kurabilen olduğundan, birden çok web sunucusu olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="210b9-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="210b9-147">Web sunucuları çalışma veya uygulama yeniden başlatılmadan durdurursanız `OnDisconnected` yöntemi çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="210b9-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="210b9-148">Bu nedenle, veri deponuz artık geçerli olmayan bağlantı kimlikleri için kaydeder olduğundan mümkündür.</span><span class="sxs-lookup"><span data-stu-id="210b9-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="210b9-149">Yalnız bırakılmış kayıtlarının temizlemek için uygulamanız için uygun bir zaman çerçevesi dışında oluşturulmuş herhangi bir bağlantısı geçersiz kılmak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="210b9-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="210b9-150">Bağlantı oluşturulduğunda, izleme için bir değer bu bölümdeki örnek verilebilir, ancak arka plan işlemi olarak yapmak isteyebilirsiniz çünkü eski kayıtları temizlemek nasıl gösterme.</span><span class="sxs-lookup"><span data-stu-id="210b9-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="210b9-151">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="210b9-151">Database</span></span>

<span data-ttu-id="210b9-152">Aşağıdaki örnekler bir veritabanı bağlantısı ve kullanıcı bilgileri korumak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="210b9-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="210b9-153">Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte Entity Framework kullanarak modelleri tanımlamak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="210b9-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="210b9-154">Bu varlık modelleri veritabanı tabloları ve alanları karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="210b9-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="210b9-155">Veri yapısını uygulamanızın gereksinimlerine bağlı olarak önemli ölçüde farklılık.</span><span class="sxs-lookup"><span data-stu-id="210b9-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="210b9-156">İlk örnek birçok bağlantı varlıkları ile ilişkilendirilebilir bir kullanıcı varlığı tanımlamak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="210b9-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="210b9-157">Ardından, hub'ından aşağıda kod ile her bağlantının durumunu izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="210b9-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="210b9-158">Azure tablo depolaması</span><span class="sxs-lookup"><span data-stu-id="210b9-158">Azure table storage</span></span>

<span data-ttu-id="210b9-159">Aşağıdaki Azure tablo depolama örnek veritabanı örnektekine benzer.</span><span class="sxs-lookup"><span data-stu-id="210b9-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="210b9-160">Tüm Azure tablo depolama hizmeti ile çalışmaya başlamak için gereken bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="210b9-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="210b9-161">Bilgi için bkz: [.NET tablo depolamasından kullanmayı](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="210b9-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="210b9-162">Aşağıdaki örnekte, bağlantı bilgilerini depolamak için bir tablo varlığı gösterir.</span><span class="sxs-lookup"><span data-stu-id="210b9-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="210b9-163">Kullanıcı adına göre verileri bölümler ve bir kullanıcı birden çok bağlantı herhangi bir zamanda çalışabilmeniz için her bir varlık bağlantı kimliğine göre tanımlar.</span><span class="sxs-lookup"><span data-stu-id="210b9-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="210b9-164">Hub'ı, her kullanıcının bağlantısının durumunu izler.</span><span class="sxs-lookup"><span data-stu-id="210b9-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
