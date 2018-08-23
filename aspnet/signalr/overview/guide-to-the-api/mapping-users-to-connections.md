---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: SignalR kullanıcılarını bağlantılarla eşleme | Microsoft Docs
author: tfitzmac
description: Bu konuda, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri saklamanın gösterilmektedir. Patrick Fletcher, bu konuda yazma yardımcı olmuştur. Bu konu başlığında kullanılan yazılım sürümleri...
ms.author: riande
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 765f85a4e07966d32bdfc9a0b533040f14a2843e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755523"
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="6f896-105">SignalR kullanıcılarını bağlantılarla eşleme</span><span class="sxs-lookup"><span data-stu-id="6f896-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="6f896-106">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6f896-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6f896-107">Bu konuda, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri saklamanın gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6f896-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="6f896-108">Patrick Fletcher, bu konuda yazma yardımcı olmuştur.</span><span class="sxs-lookup"><span data-stu-id="6f896-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="6f896-109">Bu konu başlığında kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="6f896-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="6f896-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6f896-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="6f896-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="6f896-111">.NET 4.5</span></span>
> - <span data-ttu-id="6f896-112">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="6f896-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="6f896-113">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="6f896-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="6f896-114">SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="6f896-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="6f896-115">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="6f896-115">Questions and comments</span></span>
> 
> <span data-ttu-id="6f896-116">Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="6f896-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="6f896-117">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="6f896-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="6f896-118">Giriş</span><span class="sxs-lookup"><span data-stu-id="6f896-118">Introduction</span></span>

<span data-ttu-id="6f896-119">Bir hub'a bağlanan her istemci benzersiz bağlantı kimliği geçirir. Bu değeri alabilirsiniz `Context.ConnectionId` hub bağlamını özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="6f896-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="6f896-120">Uygulamanız için bağlantı kimliği bir kullanıcı eşleme ve bu eşlemenin kalıcı hale getirmek gerekiyorsa, şunlardan birini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6f896-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="6f896-121">Kullanıcı kimlik sağlayıcısı (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="6f896-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="6f896-122">[Bellek içi depolama](#inmemory), bir sözlük gibi</span><span class="sxs-lookup"><span data-stu-id="6f896-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="6f896-123">Her kullanıcı için SignalR grubu</span><span class="sxs-lookup"><span data-stu-id="6f896-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="6f896-124">[Kalıcı, dış depolama](#database)bir veritabanı tablosu veya Azure tablo depolama gibi</span><span class="sxs-lookup"><span data-stu-id="6f896-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="6f896-125">Bu uygulamalardan her biri, bu konudaki gösterilir.</span><span class="sxs-lookup"><span data-stu-id="6f896-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="6f896-126">Kullandığınız `OnConnected`, `OnDisconnected`, ve `OnReconnected` yöntemlerinin `Hub` kullanıcı bağlantı durumunu izlemek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="6f896-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="6f896-127">Uygulamanız için en iyi yaklaşım bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="6f896-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="6f896-128">Uygulamanızı barındıran web sunucularının sayısı.</span><span class="sxs-lookup"><span data-stu-id="6f896-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="6f896-129">Olup şu anda bağlı olan kullanıcıların listesini almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6f896-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="6f896-130">Uygulama veya sunucu yeniden başlatıldığında, Grup ve kullanıcı bilgilerini kalıcı hale gerekip gerekmediğini.</span><span class="sxs-lookup"><span data-stu-id="6f896-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="6f896-131">Bir dış sunucu arama gecikme süresi bir sorun olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="6f896-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="6f896-132">Hangi yaklaşımın çalıştığı için bu konuları aşağıdaki tabloda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6f896-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="6f896-133">Birden fazla sunucu</span><span class="sxs-lookup"><span data-stu-id="6f896-133">More than one server</span></span> | <span data-ttu-id="6f896-134">Şu anda bağlı olan kullanıcıların listesini alma</span><span class="sxs-lookup"><span data-stu-id="6f896-134">Get list of currently connected users</span></span> | <span data-ttu-id="6f896-135">Yeniden başlatıldıktan sonra bilgilerini kalıcı hale</span><span class="sxs-lookup"><span data-stu-id="6f896-135">Persist information after restarts</span></span> | <span data-ttu-id="6f896-136">En iyi performans</span><span class="sxs-lookup"><span data-stu-id="6f896-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="6f896-137">Sağlayıcı kullanıcı kimliği</span><span class="sxs-lookup"><span data-stu-id="6f896-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="6f896-138">Bellek içi</span><span class="sxs-lookup"><span data-stu-id="6f896-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="6f896-139">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="6f896-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="6f896-140">Kalıcı, dış</span><span class="sxs-lookup"><span data-stu-id="6f896-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="6f896-141">IUserID sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="6f896-141">IUserID provider</span></span>

<span data-ttu-id="6f896-142">Bu özellik, USERID belirtmek kullanıcılara bir Irequest IUserIdProvider yeni arabirimi aracılığıyla temel.</span><span class="sxs-lookup"><span data-stu-id="6f896-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="6f896-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="6f896-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="6f896-144">Varsayılan olarak, olacaktır kullanıcının kullanan bir uygulama `IPrincipal.Identity.Name` kullanıcı adı.</span><span class="sxs-lookup"><span data-stu-id="6f896-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="6f896-145">Bunu değiştirmek için uygulamanız kaydetme `IUserIdProvider` , uygulamanız başlatıldığında genel konakla:</span><span class="sxs-lookup"><span data-stu-id="6f896-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="6f896-146">Bir hub içinde gelen bu kullanıcılara aşağıdaki API'si üzerinden ileti göndermek mümkün olacaktır:</span><span class="sxs-lookup"><span data-stu-id="6f896-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="6f896-147">**Belirli bir kullanıcıya bir ileti gönderme**</span><span class="sxs-lookup"><span data-stu-id="6f896-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="6f896-148">Bellek içi depolama</span><span class="sxs-lookup"><span data-stu-id="6f896-148">In-memory storage</span></span>

<span data-ttu-id="6f896-149">Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgileri bellekte depolanır bir sözlükte tutulacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6f896-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="6f896-150">Sözlük kullanan bir `HashSet` bağlantı kimliği depolamak için. Herhangi bir zamanda bir kullanıcı birden fazla bağlantı SignalR uygulamaya sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="6f896-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="6f896-151">Örneğin, birden çok cihaz ya da birden fazla tarayıcı sekmesinde bağlı bir kullanıcı birden fazla bağlantı kimliği gerekir.</span><span class="sxs-lookup"><span data-stu-id="6f896-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="6f896-152">Uygulama kapanır, tüm bilgileri kaybolur, ancak kullanıcıların kendi bağlantılarını yeniden oluşturma gibi yeniden doldurulur.</span><span class="sxs-lookup"><span data-stu-id="6f896-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="6f896-153">Ortamınızda birden fazla web sunucusu varsa her sunucuyu ayrı bir bağlantı koleksiyonu yeterli olacağından, bellek içi depolama çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="6f896-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="6f896-154">İlk örnek bağlantılar için kullanıcı eşleme yöneten bir sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="6f896-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="6f896-155">HashSet anahtarı kullanıcı adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6f896-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="6f896-156">Sonraki örnek, bir hub'ından bağlantı eşleme sınıfının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="6f896-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="6f896-157">Sınıfının örneğini, bir değişken adı ile depolanan `_connections`.</span><span class="sxs-lookup"><span data-stu-id="6f896-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="6f896-158">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="6f896-158">Single-user groups</span></span>

<span data-ttu-id="6f896-159">Her kullanıcı için bir grup oluşturun ve yalnızca bu kullanıcının erişmek istediğinizde bu gruba bir ileti gönderin.</span><span class="sxs-lookup"><span data-stu-id="6f896-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="6f896-160">Her grubun adı, kullanıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="6f896-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="6f896-161">Bir kullanıcının birden fazla bağlantı varsa, her bağlantı kimliği kullanıcı grubuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="6f896-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="6f896-162">Kullanıcı kesildiğinde, el ile kullanıcı grubundan kaldırmamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6f896-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="6f896-163">Bu eylem, SignalR framework tarafından otomatik olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6f896-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="6f896-164">Aşağıdaki örnek, tek kullanıcı gruplarına uygulanacağını gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6f896-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="6f896-165">Kalıcı, dış depolama</span><span class="sxs-lookup"><span data-stu-id="6f896-165">Permanent, external storage</span></span>

<span data-ttu-id="6f896-166">Bu konuda, bağlantı bilgilerini depolamak için bir veritabanı veya Azure tablo depolama kullanma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6f896-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="6f896-167">Bu yaklaşım, her web sunucusunun aynı veri deposu ile etkileşim kurabilir çünkü birden çok web sunucusu olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="6f896-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="6f896-168">Web sunucularınız çalışma ya da uygulama yeniden başlatılmadan durdurursanız `OnDisconnected` yöntemi çağrılmadı.</span><span class="sxs-lookup"><span data-stu-id="6f896-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="6f896-169">Bu nedenle, veri deponuz artık geçerli olmayan bir bağlantı kimlikleri için kayıtları olacağını mümkündür.</span><span class="sxs-lookup"><span data-stu-id="6f896-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="6f896-170">Artık bu kayıtları temizlemek için uygulamanız için uygun bir zaman çerçevesi dışında oluşturulmuş herhangi bir bağlantı geçersiz kılmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f896-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="6f896-171">Bu bölümdeki örneklerde bağlantıyı oluştururken izlemek için bir değer içerir ancak arka plan işlemi olarak bunu isteyebilirsiniz, çünkü eski kayıtları temizlemek nasıl gösterme.</span><span class="sxs-lookup"><span data-stu-id="6f896-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="6f896-172">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="6f896-172">Database</span></span>

<span data-ttu-id="6f896-173">Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgilerini bir veritabanında saklamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6f896-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="6f896-174">Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte, Entity Framework kullanarak modelleri tanımlamak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="6f896-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="6f896-175">Bu varlık modeli, veritabanı tabloları ve alanları karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="6f896-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="6f896-176">Data yapınız, uygulama gereksinimlerine bağlı olarak önemli ölçüde değişiklik gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="6f896-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="6f896-177">İlk örnek, birçok bağlantı varlıklar ile ilişkilendirilebilir bir kullanıcı varlığı tanımlamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6f896-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="6f896-178">Ardından, hub'ından aşağıda kod ile her bağlantının durumunu izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6f896-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="6f896-179">Azure tablo depolama</span><span class="sxs-lookup"><span data-stu-id="6f896-179">Azure table storage</span></span>

<span data-ttu-id="6f896-180">Aşağıdaki Azure tablo depolama örnek veritabanı örneğe benzerdir.</span><span class="sxs-lookup"><span data-stu-id="6f896-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="6f896-181">Tüm Azure tablo depolama hizmeti ile kullanmaya başlamak için gereken bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="6f896-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="6f896-182">Bilgi için [tablo Depolama'yı .NET kullanma](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="6f896-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="6f896-183">Aşağıdaki örnek, bağlantı bilgilerini depolamak için bir tablo varlığı gösterir.</span><span class="sxs-lookup"><span data-stu-id="6f896-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="6f896-184">Kullanıcı adına göre verileri bölümler ve kullanıcının herhangi bir zamanda birden çok bağlantı sahip olabileceği için bağlantı kimliği tarafından her varlık tanımlar.</span><span class="sxs-lookup"><span data-stu-id="6f896-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="6f896-185">Hub'ında her kullanıcının bağlantının durumunu izleyin.</span><span class="sxs-lookup"><span data-stu-id="6f896-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
