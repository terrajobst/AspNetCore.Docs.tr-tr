---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "SignalR kullanıcıları eşlemek için bağlantıları | Microsoft Docs"
author: tfitzmac
description: "Bu konu, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri korumak nasıl gösterir. Bu konuda yazma CAN Fletcher'dan Yardım. Bu konuda kullanılan yazılım sürümleri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="c701b-105">Eşleme SignalR bağlantıları kullanıcılara</span><span class="sxs-lookup"><span data-stu-id="c701b-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="c701b-106">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c701b-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c701b-107">Bu konu, kullanıcılar ve kendi bağlantılarını hakkındaki bilgileri korumak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="c701b-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="c701b-108">Bu konuda yazma CAN Fletcher'dan Yardım.</span><span class="sxs-lookup"><span data-stu-id="c701b-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="c701b-109">Bu konuda kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="c701b-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="c701b-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c701b-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="c701b-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c701b-111">.NET 4.5</span></span>
> - <span data-ttu-id="c701b-112">SignalR sürüm 2</span><span class="sxs-lookup"><span data-stu-id="c701b-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="c701b-113">Bu konunun önceki sürümleri</span><span class="sxs-lookup"><span data-stu-id="c701b-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="c701b-114">SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="c701b-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="c701b-115">Sorularınız ve yorumlarınız</span><span class="sxs-lookup"><span data-stu-id="c701b-115">Questions and comments</span></span>
> 
> <span data-ttu-id="c701b-116">Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi.</span><span class="sxs-lookup"><span data-stu-id="c701b-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c701b-117">Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c701b-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="c701b-118">Giriş</span><span class="sxs-lookup"><span data-stu-id="c701b-118">Introduction</span></span>

<span data-ttu-id="c701b-119">Bir hub'a bağlanan her istemci benzersiz bağlantı kimliği geçirir. Bu değeri alabilir `Context.ConnectionId` hub bağlamını özelliği.</span><span class="sxs-lookup"><span data-stu-id="c701b-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="c701b-120">Uygulamanız için bağlantı kimliği kullanıcısıyla ve bu eşlemenin kalıcı hale getirmek gerekirse, aşağıdakilerden birini kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="c701b-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="c701b-121">Kullanıcı kimlik sağlayıcısı (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="c701b-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="c701b-122">[Bellek içi depolama](#inmemory), bir sözlük gibi</span><span class="sxs-lookup"><span data-stu-id="c701b-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="c701b-123">Her kullanıcı için SignalR grubu</span><span class="sxs-lookup"><span data-stu-id="c701b-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="c701b-124">[Kalıcı ve dış depolama](#database)gibi bir veritabanı tablosu veya Azure tablo depolaması</span><span class="sxs-lookup"><span data-stu-id="c701b-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="c701b-125">Bu uygulamaların her biri, bu konu başlığı altında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="c701b-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="c701b-126">Kullandığınız `OnConnected`, `OnDisconnected`, ve `OnReconnected` yöntemlerinin `Hub` kullanıcı bağlantı durumunu izlemek için sınıf.</span><span class="sxs-lookup"><span data-stu-id="c701b-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="c701b-127">Uygulamanız için en iyi yaklaşımı bağlıdır:</span><span class="sxs-lookup"><span data-stu-id="c701b-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="c701b-128">Uygulamanızı barındıran web sunucularının sayısı.</span><span class="sxs-lookup"><span data-stu-id="c701b-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="c701b-129">Bağlı durumda kullanıcıların listesini almak gerekmediğini.</span><span class="sxs-lookup"><span data-stu-id="c701b-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="c701b-130">Olup uygulama veya sunucu yeniden başlatıldığında grup ve kullanıcı bilgileri kalıcı olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="c701b-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="c701b-131">Bir dış sunucu çağırma gecikme süresi bir sorun olup olmadığı.</span><span class="sxs-lookup"><span data-stu-id="c701b-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="c701b-132">Bu noktalar için hangi yaklaşımın çalışır aşağıdaki tabloda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="c701b-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="c701b-133">Birden çok sunucu</span><span class="sxs-lookup"><span data-stu-id="c701b-133">More than one server</span></span> | <span data-ttu-id="c701b-134">Şu anda bağlı kullanıcıların listesini al</span><span class="sxs-lookup"><span data-stu-id="c701b-134">Get list of currently connected users</span></span> | <span data-ttu-id="c701b-135">Bilgileri yeniden başlatıldıktan sonra Sürdür</span><span class="sxs-lookup"><span data-stu-id="c701b-135">Persist information after restarts</span></span> | <span data-ttu-id="c701b-136">En iyi performans</span><span class="sxs-lookup"><span data-stu-id="c701b-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="c701b-137">UserId sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="c701b-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="c701b-138">Bellek içi</span><span class="sxs-lookup"><span data-stu-id="c701b-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="c701b-139">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="c701b-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="c701b-140">Kalıcı, dış</span><span class="sxs-lookup"><span data-stu-id="c701b-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="c701b-141">IUserID sağlayıcısı</span><span class="sxs-lookup"><span data-stu-id="c701b-141">IUserID provider</span></span>

<span data-ttu-id="c701b-142">Bu özellik, USERID belirtmek kullanıcılara bir Irequest IUserIdProvider yeni arabirimi aracılığıyla bağlı.</span><span class="sxs-lookup"><span data-stu-id="c701b-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="c701b-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="c701b-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="c701b-144">Varsayılan olarak, olacaktır kullanıcının kullandığı uygulaması `IPrincipal.Identity.Name` kullanıcı adı.</span><span class="sxs-lookup"><span data-stu-id="c701b-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="c701b-145">Bunu değiştirmek için uygulamanızı kaydetmek `IUserIdProvider` uygulamanız başladığında genel ana bilgisayar ile:</span><span class="sxs-lookup"><span data-stu-id="c701b-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="c701b-146">Gelen bir hub içinde bu kullanıcılara aşağıdaki API aracılığıyla iletileri göndermek kullanabileceksiniz:</span><span class="sxs-lookup"><span data-stu-id="c701b-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="c701b-147">**Belirli bir kullanıcı için bir ileti gönderme**</span><span class="sxs-lookup"><span data-stu-id="c701b-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="c701b-148">Bellek içi depolama</span><span class="sxs-lookup"><span data-stu-id="c701b-148">In-memory storage</span></span>

<span data-ttu-id="c701b-149">Aşağıdaki örneklerde, bağlantı ve kullanıcı bilgileri bellekte depolanır sözlükte tutulacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="c701b-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="c701b-150">Sözlük kullanan bir `HashSet` bağlantı kimliği depolamak için. Herhangi bir anda bir kullanıcı birden fazla bağlantı SignalR uygulamaya sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="c701b-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="c701b-151">Örneğin, birden çok aygıt ya da birden fazla tarayıcı sekmesi üzerinden bağlı bir kullanıcı birden fazla bağlantı kimliği gerekir.</span><span class="sxs-lookup"><span data-stu-id="c701b-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="c701b-152">Uygulama kapanırsa, tüm bilgileri kaybolur, ancak kullanıcıların kendi bağlantılarını yeniden oluşturmak gibi yeniden doldurulur.</span><span class="sxs-lookup"><span data-stu-id="c701b-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="c701b-153">Ortamınızda birden fazla web sunucusu varsa her sunucu bağlantıları ayrı koleksiyonu gerekir çünkü bellek içi depolama çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="c701b-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="c701b-154">İlk örnek bağlantıları kullanıcılara eşleme yöneten bir sınıfı gösterir.</span><span class="sxs-lookup"><span data-stu-id="c701b-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="c701b-155">Anahtar Hashset'i için kullanıcının adı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="c701b-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="c701b-156">Sonraki örnek bir hub'dan bağlantı eşleme sınıfının nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c701b-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="c701b-157">Sınıfının örneğini bir değişken adı depolanan `_connections`.</span><span class="sxs-lookup"><span data-stu-id="c701b-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="c701b-158">Tek kullanıcı grupları</span><span class="sxs-lookup"><span data-stu-id="c701b-158">Single-user groups</span></span>

<span data-ttu-id="c701b-159">Her kullanıcı için bir grup oluşturun ve yalnızca o kullanıcıyı erişmek istediğinizde bu gruba ileti gönderme.</span><span class="sxs-lookup"><span data-stu-id="c701b-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="c701b-160">Her grubun adı, kullanıcı adıdır.</span><span class="sxs-lookup"><span data-stu-id="c701b-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="c701b-161">Bir kullanıcının birden çok bağlantı varsa, her bağlantı kimliği kullanıcı grubuna eklenir.</span><span class="sxs-lookup"><span data-stu-id="c701b-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="c701b-162">Kullanıcı kestiğinde, el ile kullanıcı grubundan kaldırmamanız.</span><span class="sxs-lookup"><span data-stu-id="c701b-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="c701b-163">Bu eylem SignalR çerçevesi tarafından otomatik olarak gerçekleştirilir.</span><span class="sxs-lookup"><span data-stu-id="c701b-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="c701b-164">Aşağıdaki örnekte nasıl tek kullanıcı gruplarına uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c701b-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="c701b-165">Kalıcı ve dış depolama</span><span class="sxs-lookup"><span data-stu-id="c701b-165">Permanent, external storage</span></span>

<span data-ttu-id="c701b-166">Bu konuda nasıl bağlantı bilgilerini depolamak için bir veritabanı veya Azure tablo depolaması kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="c701b-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="c701b-167">Bu yaklaşım, her web sunucusu ile aynı veri deposu etkileşim kurabilen olduğundan, birden çok web sunucusu olduğunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="c701b-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="c701b-168">Web sunucuları çalışma veya uygulama yeniden başlatılmadan durdurursanız `OnDisconnected` yöntemi çağrılmaz.</span><span class="sxs-lookup"><span data-stu-id="c701b-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="c701b-169">Bu nedenle, veri deponuz artık geçerli olmayan bağlantı kimlikleri için kaydeder olduğundan mümkündür.</span><span class="sxs-lookup"><span data-stu-id="c701b-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="c701b-170">Yalnız bırakılmış kayıtlarının temizlemek için uygulamanız için uygun bir zaman çerçevesi dışında oluşturulmuş herhangi bir bağlantısı geçersiz kılmak isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="c701b-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="c701b-171">Bağlantı oluşturulduğunda, izleme için bir değer bu bölümdeki örnek verilebilir, ancak arka plan işlemi olarak yapmak isteyebilirsiniz çünkü eski kayıtları temizlemek nasıl gösterme.</span><span class="sxs-lookup"><span data-stu-id="c701b-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="c701b-172">Veritabanı</span><span class="sxs-lookup"><span data-stu-id="c701b-172">Database</span></span>

<span data-ttu-id="c701b-173">Aşağıdaki örnekler bir veritabanı bağlantısı ve kullanıcı bilgileri korumak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="c701b-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="c701b-174">Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte Entity Framework kullanarak modelleri tanımlamak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="c701b-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="c701b-175">Bu varlık modelleri veritabanı tabloları ve alanları karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="c701b-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="c701b-176">Veri yapısını uygulamanızın gereksinimlerine bağlı olarak önemli ölçüde farklılık.</span><span class="sxs-lookup"><span data-stu-id="c701b-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="c701b-177">İlk örnek birçok bağlantı varlıkları ile ilişkilendirilebilir bir kullanıcı varlığı tanımlamak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="c701b-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="c701b-178">Ardından, hub'ından aşağıda kod ile her bağlantının durumunu izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="c701b-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="c701b-179">Azure tablo depolaması</span><span class="sxs-lookup"><span data-stu-id="c701b-179">Azure table storage</span></span>

<span data-ttu-id="c701b-180">Aşağıdaki Azure tablo depolama örnek veritabanı örnektekine benzer.</span><span class="sxs-lookup"><span data-stu-id="c701b-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="c701b-181">Tüm Azure tablo depolama hizmeti ile çalışmaya başlamak için gereken bilgileri içermez.</span><span class="sxs-lookup"><span data-stu-id="c701b-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="c701b-182">Bilgi için bkz: [.NET tablo depolamasından kullanmayı](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="c701b-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="c701b-183">Aşağıdaki örnekte, bağlantı bilgilerini depolamak için bir tablo varlığı gösterir.</span><span class="sxs-lookup"><span data-stu-id="c701b-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="c701b-184">Kullanıcı adına göre verileri bölümler ve bir kullanıcı birden çok bağlantı herhangi bir zamanda çalışabilmeniz için her bir varlık bağlantı kimliğine göre tanımlar.</span><span class="sxs-lookup"><span data-stu-id="c701b-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="c701b-185">Hub'ı, her kullanıcının bağlantısının durumunu izler.</span><span class="sxs-lookup"><span data-stu-id="c701b-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
