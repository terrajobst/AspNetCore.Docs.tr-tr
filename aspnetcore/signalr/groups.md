---
title: SignalR içinde kullanıcıları ve grupları yönetme
author: bradygaster
description: ASP.NET Core SignalR Kullanıcı ve grup yönetimine genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/groups
ms.openlocfilehash: 59e90042ecbaf936602643bbdc3965e036426b26
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963816"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a><span data-ttu-id="33331-103">SignalR içinde kullanıcıları ve grupları yönetme</span><span class="sxs-lookup"><span data-stu-id="33331-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="33331-104">[Brennan Conroy](https://github.com/BrennanConroy) tarafından</span><span class="sxs-lookup"><span data-stu-id="33331-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

SignalR<span data-ttu-id="33331-105">, iletilerin belirli bir kullanıcıyla ilişkili tüm bağlantılara ve ayrıca adlandırılmış bağlantı gruplarına gönderilmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="33331-105"> allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="33331-106">[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(nasıl indirileceği)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="33331-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-opno-locsignalr"></a><span data-ttu-id="33331-107">SignalR kullanıcılar</span><span class="sxs-lookup"><span data-stu-id="33331-107">Users in SignalR</span></span>

SignalR<span data-ttu-id="33331-108">, belirli bir kullanıcıyla ilişkili tüm bağlantılara ileti göndermenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="33331-108"> allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="33331-109">Varsayılan olarak, SignalR, Kullanıcı tanımlayıcısı olarak bağlantıyla ilişkili `ClaimsPrincipal` `ClaimTypes.NameIdentifier` kullanır.</span><span class="sxs-lookup"><span data-stu-id="33331-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="33331-110">Tek bir kullanıcının SignalR uygulamasına birden fazla bağlantısı olabilir.</span><span class="sxs-lookup"><span data-stu-id="33331-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="33331-111">Örneğin, bir Kullanıcı masaüstüne ve telefonlarına bağlanabilir.</span><span class="sxs-lookup"><span data-stu-id="33331-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="33331-112">Her cihazın ayrı bir SignalR bağlantısı vardır, ancak hepsi aynı kullanıcıyla ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="33331-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="33331-113">Kullanıcıya bir ileti gönderildiğinde, bu kullanıcıyla ilişkili tüm bağlantılar iletiyi alır.</span><span class="sxs-lookup"><span data-stu-id="33331-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="33331-114">Bir bağlantı için Kullanıcı tanımlayıcısına, hub 'ınızdaki `Context.UserIdentifier` özelliği tarafından erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="33331-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="33331-115">Aşağıdaki örnekte gösterildiği gibi, kullanıcı tanımlayıcısını hub yönteminizin `User` işleve geçirerek belirli bir kullanıcıya bir ileti gönderin:</span><span class="sxs-lookup"><span data-stu-id="33331-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="33331-116">Kullanıcı tanımlayıcısı büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="33331-116">The user identifier is case-sensitive.</span></span>

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a><span data-ttu-id="33331-117">SignalR gruplar</span><span class="sxs-lookup"><span data-stu-id="33331-117">Groups in SignalR</span></span>

<span data-ttu-id="33331-118">Grup, bir adla ilişkili bir bağlantı koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="33331-118">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="33331-119">İletiler, bir gruptaki tüm bağlantılara gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="33331-119">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="33331-120">Gruplar uygulama tarafından yönetildiğinden, gruplar bir bağlantıya veya birden çok bağlantıya göndermek için önerilen yoldur.</span><span class="sxs-lookup"><span data-stu-id="33331-120">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="33331-121">Bir bağlantı, birden fazla grubun üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="33331-121">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="33331-122">Bu, grupları her odanın bir grup olarak gösterilebileceği bir sohbet uygulaması gibi bir şey için ideal hale getirir.</span><span class="sxs-lookup"><span data-stu-id="33331-122">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="33331-123">`AddToGroupAsync` ve `RemoveFromGroupAsync` yöntemleri aracılığıyla gruplardan bağlantılar eklenebilir veya gruplardan kaldırılabilir.</span><span class="sxs-lookup"><span data-stu-id="33331-123">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="33331-124">Bağlantı yeniden bağlandığında grup üyeliği korunmaz.</span><span class="sxs-lookup"><span data-stu-id="33331-124">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="33331-125">Bağlantı, yeniden oluşturulduğunda gruba yeniden katılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="33331-125">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="33331-126">Uygulama birden çok sunucuya ölçeklendirildiyse bu bilgiler kullanılamadığından, bir grubun üyelerini saymak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="33331-126">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="33331-127">Grupları kullanırken kaynaklara erişimi korumak için ASP.NET Core [kimlik doğrulaması ve yetkilendirme](xref:signalr/authn-and-authz) işlevini kullanın.</span><span class="sxs-lookup"><span data-stu-id="33331-127">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="33331-128">Yalnızca bu grup için kimlik bilgileri geçerliyse bir gruba Kullanıcı eklerseniz, bu gruba gönderilen iletiler yalnızca yetkili kullanıcılara gider.</span><span class="sxs-lookup"><span data-stu-id="33331-128">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="33331-129">Ancak, gruplar bir güvenlik özelliği değildir.</span><span class="sxs-lookup"><span data-stu-id="33331-129">However, groups are not a security feature.</span></span> <span data-ttu-id="33331-130">Kimlik doğrulama talepleri, süre sonu ve iptal gibi grupların olmadığı özelliklere sahiptir.</span><span class="sxs-lookup"><span data-stu-id="33331-130">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="33331-131">Bir kullanıcının gruba erişim izni iptal edildiğinde, bunu el ile tespit etmeniz ve gruptan kaldırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="33331-131">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="33331-132">Grup adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="33331-132">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="33331-133">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="33331-133">Related resources</span></span>

* [<span data-ttu-id="33331-134">Kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="33331-134">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="33331-135">Merkezler</span><span class="sxs-lookup"><span data-stu-id="33331-135">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="33331-136">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="33331-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
