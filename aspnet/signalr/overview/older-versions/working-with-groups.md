---
uid: signalr/overview/older-versions/working-with-groups
title: "SignalR grupları ile çalışma 1.x | Microsoft Docs"
author: pfletcher
description: "Bu konu, grup üyeliği bilgileri Hub API ile kalıcı açıklar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 04da74f23663313e70e54fd4f2f9e5f005791cff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="f32ef-103">SignalR grupları ile çalışma 1.x</span><span class="sxs-lookup"><span data-stu-id="f32ef-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="f32ef-104">tarafından [CAN Fletcher'dan](https://github.com/pfletcher), [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f32ef-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f32ef-105">Bu konu, kullanıcıları gruplara ekleyin ve grup üyeliği bilgilerini kalıcı açıklar.</span><span class="sxs-lookup"><span data-stu-id="f32ef-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="f32ef-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="f32ef-106">Overview</span></span>

<span data-ttu-id="f32ef-107">SignalR gruplarında yayın iletileri belirtilen kümelerine bağlı istemciler için bir yöntem sağlar.</span><span class="sxs-lookup"><span data-stu-id="f32ef-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="f32ef-108">Bir grup herhangi bir sayıda istemcileri içerebilir ve bir istemci grupları herhangi bir sayıda üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="f32ef-109">Açıkça grupları oluşturmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="f32ef-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="f32ef-110">Etkin bir grup Groups.Add çağrıda adını belirtin ilk kez otomatik olarak oluşturulur ve bu üyelik son bağlantı kaldırdığınızda silinir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="f32ef-111">Grupları kullanma için giriş için bkz [Hub sınıfından grup üyeliğini yönetme](index.md) hub API'sindeki - Server Kılavuzu.</span><span class="sxs-lookup"><span data-stu-id="f32ef-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="f32ef-112">Bir grup üyeliği listesinin veya gruplarının bir listesini almak için hiçbir API yoktur.</span><span class="sxs-lookup"><span data-stu-id="f32ef-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="f32ef-113">SignalR ileti istemciler ve pub/alt modelini temel alan gruplar gönderir ve sunucu grupları ve grup üyeliklerine listelerini içermez.</span><span class="sxs-lookup"><span data-stu-id="f32ef-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="f32ef-114">Bir web grubu için bir düğüm eklediğinizde, yeni bir düğüme yayılması SignalR tutar herhangi bir durum olduğundan bu ölçeklenebilirlik, en üst düzeye çıkarmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="f32ef-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="f32ef-115">Kullanarak bir grup kullanıcı eklediğinizde `Groups.Add` yöntemi, kullanıcının geçerli bağlantı süresince bu gruba yönelik iletileri alır, ancak bu gruba kullanıcının üyelik geçerli bağlantıyı kalıcı yapılmaz.</span><span class="sxs-lookup"><span data-stu-id="f32ef-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="f32ef-116">Gruplar ve grup üyeliği bilgilerini kalıcı olarak korumak istiyorsanız, bir veritabanı veya Azure tablo depolama alanı gibi bir havuzda verileri depolamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="f32ef-117">Ardından, bir kullanıcının uygulamanıza her bağlandığında, depodan kullanıcının ait olduğu grupları almak ve el ile kullanıcıyı bu gruplara ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f32ef-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="f32ef-118">Geçici bozulma sonra yeniden bağlanmayı, kullanıcı daha önce atanan grupları otomatik olarak yeniden'e birleştirir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="f32ef-119">Otomatik olarak bir grup katılmanız yalnızca yeniden bağlanma, yeni bir bağlantı kurmadan olduğunda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="f32ef-120">Dijital olarak imzalanmış bir belirteç daha önce atanan grupları listesini içeren istemciden geçirilir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="f32ef-121">Kullanıcı istenen grubuna ait olup olmadığını doğrulamak istiyorsanız, varsayılan davranışı geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f32ef-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="f32ef-122">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="f32ef-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="f32ef-123">Ekleme ve kullanıcıları kaldırma</span><span class="sxs-lookup"><span data-stu-id="f32ef-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="f32ef-124">Bir grubun üyeleri çağırma</span><span class="sxs-lookup"><span data-stu-id="f32ef-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="f32ef-125">Grup üyeliği bir veritabanında saklamak</span><span class="sxs-lookup"><span data-stu-id="f32ef-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="f32ef-126">Azure tablo depolama içinde grup üyeliği</span><span class="sxs-lookup"><span data-stu-id="f32ef-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="f32ef-127">Grup üyeliği bağlanırken Yönlendirilme doğrulanıyor</span><span class="sxs-lookup"><span data-stu-id="f32ef-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="f32ef-128">Ekleme ve kullanıcıları kaldırma</span><span class="sxs-lookup"><span data-stu-id="f32ef-128">Adding and removing users</span></span>

<span data-ttu-id="f32ef-129">Eklemek veya kullanıcıların bir gruptan kaldırmak için arama [Ekle](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) veya [kaldırmak](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) yöntemleri ve kullanıcının bağlantı kimliği ve grubun adını parametreler olarak geçirin.</span><span class="sxs-lookup"><span data-stu-id="f32ef-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="f32ef-130">Bağlantı sona erdiğinde bir kullanıcıyı gruptan el ile kaldırmak zorunda değildir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="f32ef-131">Aşağıdaki örnekte gösterildiği `Groups.Add` ve `Groups.Remove` Hub yöntemlerinde kullanılan yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="f32ef-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="f32ef-132">`Groups.Add` Ve `Groups.Remove` zaman uyumsuz bir yöntem yürütülemez.</span><span class="sxs-lookup"><span data-stu-id="f32ef-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="f32ef-133">Bir istemci bir gruba eklemek ve hemen Grup seçeneğini kullanarak bir ileti istemciye göndermek istiyorsanız, Groups.Add yöntemi ilk tamamladığından emin yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="f32ef-134">Aşağıdaki kod örnekleri, .NET 4.5 ve .NET 4'te çalışan kod kullanarak bir tane çalışan kod kullanarak bir tane bunun nasıl yapılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="f32ef-135">Zaman uyumsuz .NET 4.5 örneği</span><span class="sxs-lookup"><span data-stu-id="f32ef-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="f32ef-136">Zaman uyumsuz .NET 4 örnek</span><span class="sxs-lookup"><span data-stu-id="f32ef-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="f32ef-137">Genel olarak, dahil `await` çağrılırken `Groups.Remove` yöntemi kaldırmaya çalıştığınız bağlantı kimliği artık kullanılabilir olabileceği için.</span><span class="sxs-lookup"><span data-stu-id="f32ef-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="f32ef-138">Bu durumda, `TaskCanceledException` istek zaman aşımına sonra atılır. Ekleyebileceğiniz uygulamanızı gruba bir iletiyi göndermeden önce kullanıcı grubundan kaldırıldı emin olmalısınız varsa, `await` Groups.Remove ve ardından catch önce `TaskCanceledException` durum özel durumu.</span><span class="sxs-lookup"><span data-stu-id="f32ef-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="f32ef-139">Bir grubun üyeleri çağırma</span><span class="sxs-lookup"><span data-stu-id="f32ef-139">Calling members of a group</span></span>

<span data-ttu-id="f32ef-140">İleti tüm bir grubu üyeleri veya yalnızca belirtilen grubunun üyeleri aşağıdaki örneklerde gösterildiği gibi gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f32ef-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="f32ef-141">**Tüm** bağlı istemciler belirtilen grubunda.</span><span class="sxs-lookup"><span data-stu-id="f32ef-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="f32ef-142">Tüm bağlı istemcileri belirtilen gruptaki **dışındaki belirtilen istemcilerin**, bağlantı kimliği tarafından tanımlanan</span><span class="sxs-lookup"><span data-stu-id="f32ef-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="f32ef-143">Tüm bağlı istemcileri belirtilen gruptaki **çağıran istemci dışındaki**.</span><span class="sxs-lookup"><span data-stu-id="f32ef-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="f32ef-144">Grup üyeliği bir veritabanında saklamak</span><span class="sxs-lookup"><span data-stu-id="f32ef-144">Storing group membership in a database</span></span>

<span data-ttu-id="f32ef-145">Aşağıdaki örnekler bir veritabanında grup ve kullanıcı bilgilerini korumak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="f32ef-146">Tüm veri erişim teknolojisi kullanabilirsiniz; Ancak, aşağıdaki örnekte Entity Framework kullanarak modelleri tanımlamak nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="f32ef-147">Bu varlık modelleri veritabanı tabloları ve alanları karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="f32ef-148">Veri yapısını uygulamanızın gereksinimlerine bağlı olarak önemli ölçüde farklılık.</span><span class="sxs-lookup"><span data-stu-id="f32ef-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="f32ef-149">Bu örnek adlı bir sınıf içerir `ConversationRoom` spor veya bahçesi oluşturma gibi farklı konularla ilgili görüşmeleri katılmak kullanıcıların sağlayan bir uygulama için benzersiz olacak.</span><span class="sxs-lookup"><span data-stu-id="f32ef-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="f32ef-150">Bu örnek ayrıca bağlantılar için bir sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="f32ef-151">Bağlantı sınıfı grup üyeliği izlemek için kesinlikle gerekli değildir ancak sık kullanıcılar izleme için güçlü bir çözüm bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="f32ef-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="f32ef-152">Sonra hub'ı, Grup ve kullanıcı bilgilerini veritabanından ve el ile kullanıcı uygun gruplara ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f32ef-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="f32ef-153">Örneğin, kullanıcı bağlantılarını izlemek için kod içermez.</span><span class="sxs-lookup"><span data-stu-id="f32ef-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="f32ef-154">Bu örnekte, `await` önce anahtar sözcüğü uygulanmaz `Groups.Add` grubunun üyeleri için bir ileti hemen gönderilmez olduğundan.</span><span class="sxs-lookup"><span data-stu-id="f32ef-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="f32ef-155">Yeni üye hemen ekledikten sonra grubunun tüm üyeleri için bir ileti göndermek istiyorsanız, uygulamak istediğiniz `await` zaman uyumsuz işlemi tamamlandı emin olmak için anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="f32ef-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="f32ef-156">Azure tablo depolama içinde grup üyeliği</span><span class="sxs-lookup"><span data-stu-id="f32ef-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="f32ef-157">Grup ve kullanıcı bilgilerini depolamak için Azure tablo depolaması kullanarak bir veritabanını kullanmaya benzer.</span><span class="sxs-lookup"><span data-stu-id="f32ef-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="f32ef-158">Aşağıdaki örnekte kullanıcı adı ve grup adı depolayan Tablo varlığı gösterir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="f32ef-159">Hub'ı, kullanıcı bağlandığında, atanan grupları alır.</span><span class="sxs-lookup"><span data-stu-id="f32ef-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="f32ef-160">Grup üyeliği bağlanırken Yönlendirilme doğrulanıyor</span><span class="sxs-lookup"><span data-stu-id="f32ef-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="f32ef-161">Varsayılan olarak, SignalR otomatik olarak bir kullanıcı uygun gruplara durumundan zaman bağlantı bırakılan ve bağlantı zaman aşımına uğramadan önce yeniden kurdu gibi geçici, yeniden bağlanmayı yeniden atar. Kullanıcı grubu bilgilerini bir belirteç içine bağlanırken Yönlendirilme geçirilir ve belirtecini sunucuda doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="f32ef-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="f32ef-162">Kullanıcı gruplarına yeniden katılma için doğrulama işlemi hakkında daha fazla bilgi için bkz: [bağlanırken Yönlendirilme gruplarına yeniden katılma](index.md).</span><span class="sxs-lookup"><span data-stu-id="f32ef-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="f32ef-163">Genel olarak, otomatik olarak gruplarında yeniden katılmanız varsayılan davranışını kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="f32ef-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="f32ef-164">SignalR grupları hassas verilere erişimi kısıtlamak için bir güvenlik mekanizması olarak tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="f32ef-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="f32ef-165">Ancak, uygulamanızın bir kullanıcının grup üyeliği bağlanırken Yönlendirilme denetleyin, varsayılan davranışı geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f32ef-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="f32ef-166">Her yeniden bağlanmak için yerine yalnızca kullanıcı bağlandığında, bir kullanıcının grup üyeliği alınmalıdır çünkü varsayılan davranışı değiştirme veritabanınız için bir yük ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="f32ef-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="f32ef-167">Grup üyeliği doğrulamanız gerekir yeniden bağlanma, atanan grupları listesini döndürür yeni bir hub ardışık düzen modül aşağıda gösterildiği gibi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f32ef-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="f32ef-168">Daha sonra bu modül hub ardışık düzendeki, aşağıda vurgulandığı gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f32ef-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
