---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: SignalR bağlantıları için SignalR kullanıcılarını eşleme 1.x | Microsoft Docs
author: bradygaster
description: Bu konuda, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri saklamanın gösterilmektedir.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 1e65a11e08a5b060cf8b096b5fe5b90eb8dc5b51
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836720"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="50baf-103">SignalR 1.x Sürümünde SignalR Kullanıcılarını Bağlantılarla Eşleme</span><span class="sxs-lookup"><span data-stu-id="50baf-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="50baf-104">tarafından [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="50baf-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="50baf-105">Bu konuda, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri saklamanın gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="50baf-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="50baf-106">Giriş</span><span class="sxs-lookup"><span data-stu-id="50baf-106">Introduction</span></span>

<span data-ttu-id="50baf-107">Bir hub'a bağlanan her istemci benzersiz bağlantı kimliği geçirir. Bu değeri alabilirsiniz `Context.ConnectionId` hub bağlamını özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="50baf-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="50baf-108">Uygulamanız için bağlantı kimliği bir kullanıcı eşleme ve bu eşlemenin kalıcı hale getirmek gerekiyorsa, şunlardan birini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="50baf-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="50baf-109">[Bellek içi depolama](#inmemory), bir sözlük gibi</span><span class="sxs-lookup"><span data-stu-id="50baf-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="50baf-110">Her kullanıcı için SignalR grubu</span><span class="sxs-lookup"><span data-stu-id="50baf-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="50baf-111">[Kalıcı, dış depolama](#database)bir veritabanı tablosu veya Azure tablo depolama gibi</span><span class="sxs-lookup"><span data-stu-id="50baf-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="50baf-112">Bu uygulamalardan her biri, bu konudaki gösterilir.</span><span class="sxs-lookup"><span data-stu-id="50baf-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="50baf-113">Kullandığınız `OnConnected`, `OnDisconnected`, ve `OnReconnected` yöntemlerinin `Hub` kullanıcı bağlantı durumunu izlemek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="50baf-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="50baf-114">Uygulamanız için en iyi yaklaşım bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="50baf-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="50baf-115">Uygulamanızı barındıran web sunucularının sayısı.</span><span class="sxs-lookup"><span data-stu-id="50baf-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="50baf-116">Olup şu anda bağlı olan kullanıcıların listesini almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="50baf-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="50baf-117">Uygulama veya sunucu yeniden başlatıldığında, Grup ve kullanıcı bilgilerini kalıcı hale gerekip gerekmediğini.</span><span class="sxs-lookup"><span data-stu-id="50baf-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="50baf-118">Bir dış sunucu arama gecikme süresi bir sorun olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="50baf-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="50baf-119">Hangi yaklaşımın çalıştığı için bu konuları aşağıdaki tabloda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="50baf-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="50baf-120">Birden fazla sunucu</span><span class="sxs-lookup"><span data-stu-id="50baf-120">More than one server</span></span> | <span data-ttu-id="50baf-121">Şu anda bağlı olan kullanıcıların listesini alma</span><span class="sxs-lookup"><span data-stu-id="50baf-121">Get list of currently connected users</span></span> | <span data-ttu-id="50baf-122">Yeniden başlatıldıktan sonra bilgilerini kalıcı hale</span><span class="sxs-lookup"><span data-stu-id="50baf-122">Persist information after restarts</span></span> | <span data-ttu-id="50baf-123">En iyi performans</span><span class="sxs-lookup"><span data-stu-id="50baf-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="50baf-124">Bellek içi</span><span class="sxs-lookup"><span data-stu-id="50baf-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="50baf-125">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="50baf-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="50baf-126">Kalıcı, dış</span><span class="sxs-lookup"><span data-stu-id="50baf-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="50baf-127">Bellek içi depolama</span><span class="sxs-lookup"><span data-stu-id="50baf-127">In-memory storage</span></span>

<span data-ttu-id="50baf-128">Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgileri bellekte depolanır bir sözlükte tutulacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="50baf-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="50baf-129">Sözlük kullanan bir `HashSet` bağlantı kimliği depolamak için. Herhangi bir zamanda bir kullanıcı birden fazla bağlantı SignalR uygulamaya sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="50baf-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="50baf-130">Örneğin, birden çok cihaz ya da birden fazla tarayıcı sekmesinde bağlı bir kullanıcı birden fazla bağlantı kimliği gerekir.</span><span class="sxs-lookup"><span data-stu-id="50baf-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="50baf-131">Uygulama kapanır, tüm bilgileri kaybolur, ancak kullanıcıların kendi bağlantılarını yeniden oluşturma gibi yeniden doldurulur.</span><span class="sxs-lookup"><span data-stu-id="50baf-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="50baf-132">Ortamınızda birden fazla web sunucusu varsa her sunucuyu ayrı bir bağlantı koleksiyonu yeterli olacağından, bellek içi depolama çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="50baf-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="50baf-133">İlk örnek bağlantılar için kullanıcı eşleme yöneten bir sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="50baf-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="50baf-134">HashSet anahtarı kullanıcı adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="50baf-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="50baf-135">Sonraki örnek, bir hub'ından bağlantı eşleme sınıfının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="50baf-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="50baf-136">Sınıfının örneğini, bir değişken adı ile depolanan `_connections`.</span><span class="sxs-lookup"><span data-stu-id="50baf-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="50baf-137">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="50baf-137">Single-user groups</span></span>

<span data-ttu-id="50baf-138">Her kullanıcı için bir grup oluşturun ve yalnızca bu kullanıcının erişmek istediğinizde bu gruba bir ileti gönderin.</span><span class="sxs-lookup"><span data-stu-id="50baf-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="50baf-139">Her grubun adı, kullanıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="50baf-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="50baf-140">Bir kullanıcının birden fazla bağlantı varsa, her bağlantı kimliği kullanıcı grubuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="50baf-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="50baf-141">Kullanıcı kesildiğinde, el ile kullanıcı grubundan kaldırmamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="50baf-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="50baf-142">Bu eylem, SignalR framework tarafından otomatik olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="50baf-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="50baf-143">Aşağıdaki örnek, tek kullanıcı gruplarına uygulanacağını gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="50baf-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="50baf-144">Kalıcı, dış depolama</span><span class="sxs-lookup"><span data-stu-id="50baf-144">Permanent, external storage</span></span>

<span data-ttu-id="50baf-145">Bu konuda, bağlantı bilgilerini depolamak için bir veritabanı veya Azure tablo depolama kullanma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="50baf-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="50baf-146">Bu yaklaşım, her web sunucusunun aynı veri deposu ile etkileşim kurabilir çünkü birden çok web sunucusu olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="50baf-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="50baf-147">Web sunucularınız çalışma ya da uygulama yeniden başlatılmadan durdurursanız `OnDisconnected` yöntemi çağrılmadı.</span><span class="sxs-lookup"><span data-stu-id="50baf-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="50baf-148">Bu nedenle, veri deponuz artık geçerli olmayan bir bağlantı kimlikleri için kayıtları olacağını mümkündür.</span><span class="sxs-lookup"><span data-stu-id="50baf-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="50baf-149">Artık bu kayıtları temizlemek için uygulamanız için uygun bir zaman çerçevesi dışında oluşturulmuş herhangi bir bağlantı geçersiz kılmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50baf-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="50baf-150">Bu bölümdeki örneklerde bağlantıyı oluştururken izlemek için bir değer içerir ancak arka plan işlemi olarak bunu isteyebilirsiniz, çünkü eski kayıtları temizlemek nasıl gösterme.</span><span class="sxs-lookup"><span data-stu-id="50baf-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="50baf-151">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="50baf-151">Database</span></span>

<span data-ttu-id="50baf-152">Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgilerini bir veritabanında saklamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="50baf-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="50baf-153">Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte, Entity Framework kullanarak modelleri tanımlamak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="50baf-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="50baf-154">Bu varlık modeli, veritabanı tabloları ve alanları karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="50baf-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="50baf-155">Data yapınız, uygulama gereksinimlerine bağlı olarak önemli ölçüde değişiklik gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="50baf-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="50baf-156">İlk örnek, birçok bağlantı varlıklar ile ilişkilendirilebilir bir kullanıcı varlığı tanımlamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="50baf-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="50baf-157">Ardından, hub'ından aşağıda kod ile her bağlantının durumunu izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="50baf-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="50baf-158">Azure tablo depolama</span><span class="sxs-lookup"><span data-stu-id="50baf-158">Azure table storage</span></span>

<span data-ttu-id="50baf-159">Aşağıdaki Azure tablo depolama örnek veritabanı örneğe benzerdir.</span><span class="sxs-lookup"><span data-stu-id="50baf-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="50baf-160">Tüm Azure tablo depolama hizmeti ile kullanmaya başlamak için gereken bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="50baf-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="50baf-161">Bilgi için [tablo Depolama'yı .NET kullanma](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="50baf-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="50baf-162">Aşağıdaki örnek, bağlantı bilgilerini depolamak için bir tablo varlığı gösterir.</span><span class="sxs-lookup"><span data-stu-id="50baf-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="50baf-163">Kullanıcı adına göre verileri bölümler ve kullanıcının herhangi bir zamanda birden çok bağlantı sahip olabileceği için bağlantı kimliği tarafından her varlık tanımlar.</span><span class="sxs-lookup"><span data-stu-id="50baf-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="50baf-164">Hub'ında her kullanıcının bağlantının durumunu izleyin.</span><span class="sxs-lookup"><span data-stu-id="50baf-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
