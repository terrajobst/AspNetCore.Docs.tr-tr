---
title: SignalR alanındaki kullanıcılar ve grupları yönetme
author: rachelappel
description: ASP.NET Core SignalR kullanıcı ve Grup Yönetimi genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: f7d60a906fc238f79c76fd2a4ee693417a348825
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272087"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="d5667-103">SignalR alanındaki kullanıcılar ve grupları yönetme</span><span class="sxs-lookup"><span data-stu-id="d5667-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="d5667-104">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="d5667-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="d5667-105">SignalR iletileri belirli bir kullanıcıyla ilişkili tüm bağlantıları gönderilmesi gibi adlı gruplarına bağlantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5667-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="d5667-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(nasıl indirileceğini)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="d5667-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="d5667-107">SignalR kullanıcılar</span><span class="sxs-lookup"><span data-stu-id="d5667-107">Users in SignalR</span></span>

<span data-ttu-id="d5667-108">SignalR belirli bir kullanıcıyla ilişkili tüm bağlantılara ileti göndermenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="d5667-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="d5667-109">Varsayılan olarak SignalR kullanır `ClaimTypes.NameIdentifier` gelen `ClaimsPrincipal` kullanıcı tanımlayıcısı olarak bağlantı ile ilişkili.</span><span class="sxs-lookup"><span data-stu-id="d5667-109">By default SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="d5667-110">Tek bir kullanıcı bir SignalR uygulaması birden çok bağlantı olabilir.</span><span class="sxs-lookup"><span data-stu-id="d5667-110">A single user can have multiple connections to a SignalR application.</span></span> <span data-ttu-id="d5667-111">Örneğin, bir kullanıcının telefon yanı sıra Masaüstü üzerinde bağlı.</span><span class="sxs-lookup"><span data-stu-id="d5667-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="d5667-112">Her cihaz için ayrı bir SignalR bağlantısı olsa da, aynı kullanıcıyla ilişkili tüm.</span><span class="sxs-lookup"><span data-stu-id="d5667-112">Each device has a separate SignalR connection, but they are all associated with the same user.</span></span> <span data-ttu-id="d5667-113">Kullanıcıya bir ileti gönderilir, o kullanıcıyla ilişkili bağlantılarının tümünü iletiyi alır.</span><span class="sxs-lookup"><span data-stu-id="d5667-113">If a message is sent to the user, all of the connections associated with that user will receive the message.</span></span>

<span data-ttu-id="d5667-114">Kullanıcı tanımlayıcısı geçirerek belirli bir kullanıcıya bir ileti gönderme `User` aşağıdaki örnekte gösterildiği gibi hub yönteminin işlev:</span><span class="sxs-lookup"><span data-stu-id="d5667-114">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="d5667-115">Kullanıcı Kimliği büyük/küçük harf duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="d5667-115">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="d5667-116">Kullanıcı tanımlayıcısı oluşturarak özelleştirilebilir bir `IUserIdProvider`ve içinde kaydetme `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d5667-116">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="d5667-117">Özel SignalR hizmetlerinizi kaydetmeden önce AddSignalR çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="d5667-117">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="d5667-118">SignalR grupları</span><span class="sxs-lookup"><span data-stu-id="d5667-118">Groups in SignalR</span></span>

<span data-ttu-id="d5667-119">Bir grup adı ile ilişkili bağlantıları koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="d5667-119">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="d5667-120">İletiler, bir grup içindeki tüm bağlantılar için gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="d5667-120">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="d5667-121">Bir bağlantı veya birden çok bağlantı grupları uygulama tarafından yönetildiğinden göndermek için önerilen yöntem gruplarıdır.</span><span class="sxs-lookup"><span data-stu-id="d5667-121">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="d5667-122">Bir bağlantı birden çok grupların üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="d5667-122">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="d5667-123">Bu gruplar bir sohbet uygulaması gibi bir şey için burada her alan bir grup olarak temsil edilebilir ideal hale getirir.</span><span class="sxs-lookup"><span data-stu-id="d5667-123">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="d5667-124">Bağlantıları eklenebilir ya da grupları kaldırılmasını `AddToGroupAsync` ve `RemoveFromGroupAsync` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="d5667-124">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="d5667-125">Bir bağlantı bağlandığında grup üyeliği korunur değil.</span><span class="sxs-lookup"><span data-stu-id="d5667-125">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="d5667-126">Bağlantı yeniden kurulduğunda gruba katılmaya gerekir.</span><span class="sxs-lookup"><span data-stu-id="d5667-126">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="d5667-127">Bu bilgiler uygulamanın birden çok sunucuya ölçeklendirilir kullanılabilir olmadığından bir grubun üyelerini saymak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="d5667-127">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="d5667-128">Grup adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="d5667-128">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="d5667-129">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="d5667-129">Related resources</span></span>

* [<span data-ttu-id="d5667-130">Kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="d5667-130">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="d5667-131">Merkezler</span><span class="sxs-lookup"><span data-stu-id="d5667-131">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="d5667-132">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="d5667-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
