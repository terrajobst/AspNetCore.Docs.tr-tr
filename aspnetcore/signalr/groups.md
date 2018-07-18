---
title: SignalR öğesindeki kullanıcılar ve Gruplar'ı yönetme
author: tdykstra
description: ASP.NET Core SignalR kullanıcı ve Grup yönetimine genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: d54ab2a113345f98e26425a88cad165d67b8d456
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095027"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="468a2-103">SignalR öğesindeki kullanıcılar ve Gruplar'ı yönetme</span><span class="sxs-lookup"><span data-stu-id="468a2-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="468a2-104">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="468a2-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="468a2-105">SignalR iletileri belirli bir kullanıcıyla ilişkili tüm bağlantıları gönderilmesi gibi adlı gruplarına bağlantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="468a2-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="468a2-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(karşıdan yükleme)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="468a2-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="468a2-107">Signalr'da kullanıcılar</span><span class="sxs-lookup"><span data-stu-id="468a2-107">Users in SignalR</span></span>

<span data-ttu-id="468a2-108">SignalR, belirli bir kullanıcıyla ilişkili tüm bağlantılara ileti göndermek sağlar.</span><span class="sxs-lookup"><span data-stu-id="468a2-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="468a2-109">Varsayılan olarak, SignalR kullanır `ClaimTypes.NameIdentifier` gelen `ClaimsPrincipal` kullanıcı tanımlayıcısı olarak bağlantı ile ilişkili.</span><span class="sxs-lookup"><span data-stu-id="468a2-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="468a2-110">Tek bir kullanıcı bir SignalR uygulama birden fazla bağlantı olabilir.</span><span class="sxs-lookup"><span data-stu-id="468a2-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="468a2-111">Örneğin, bir kullanıcının telefon numaraları yanı sıra Masaüstü bağlı.</span><span class="sxs-lookup"><span data-stu-id="468a2-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="468a2-112">Her cihaz için ayrı bir SignalR bağlantı olsa da, bunlar aynı kullanıcıyla ilişkili tüm.</span><span class="sxs-lookup"><span data-stu-id="468a2-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="468a2-113">Kullanıcıya bir ileti gönderildiğinde, tüm bu kullanıcı ile ilişkili bağlantıları iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="468a2-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="468a2-114">Kullanıcı tanımlayıcısı için bir bağlantı tarafından erişilebilen `Context.UserIdentifier` hub'ınızdaki özelliği.</span><span class="sxs-lookup"><span data-stu-id="468a2-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="468a2-115">Kullanıcı tanımlayıcısı için geçirerek belirli bir kullanıcıya bir ileti gönder `User` aşağıdaki örnekte gösterildiği gibi hub yönteminizi işlev:</span><span class="sxs-lookup"><span data-stu-id="468a2-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="468a2-116">Kullanıcı tanımlayıcısı büyük/küçük harf duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="468a2-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="468a2-117">Kullanıcı tanımlayıcısı oluşturarak özelleştirilebilir bir `IUserIdProvider`ve onu kaydetme `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="468a2-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="468a2-118">AddSignalR özel SignalR hizmetlerinizi kaydetmeden önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="468a2-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="468a2-119">Signalr'da gruplarla</span><span class="sxs-lookup"><span data-stu-id="468a2-119">Groups in SignalR</span></span>

<span data-ttu-id="468a2-120">Bir grubu, bir ad ile ilişkili bağlantılar koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="468a2-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="468a2-121">Bir gruptaki tüm bağlantılara ileti gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="468a2-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="468a2-122">Grupları bir bağlantı veya birden çok bağlantı grupları uygulama tarafından yönetildiği göndermek için önerilen yoldur.</span><span class="sxs-lookup"><span data-stu-id="468a2-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="468a2-123">Bir bağlantı, birden çok grubunun bir üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="468a2-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="468a2-124">Bu grup için bir sohbet uygulaması gibi burada her odanın bir grup olarak temsil edilebilir ideal hale getirir.</span><span class="sxs-lookup"><span data-stu-id="468a2-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="468a2-125">Bağlantılar için eklendiğinde veya grupları kaldırıldığında `AddToGroupAsync` ve `RemoveFromGroupAsync` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="468a2-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="468a2-126">Bir bağlantı bağlandığında, grup üyeliği korunmaz.</span><span class="sxs-lookup"><span data-stu-id="468a2-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="468a2-127">Bağlantı yeniden kurulduğunda gruba katılabilir gerekir.</span><span class="sxs-lookup"><span data-stu-id="468a2-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="468a2-128">Bu bilgiler uygulamanın birden çok sunucuya ölçeklendirilirse kullanılabilir olmadığından, bir grubun üyelerinin sayısı mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="468a2-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

> [!NOTE]
> <span data-ttu-id="468a2-129">Grup adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="468a2-129">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="468a2-130">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="468a2-130">Related resources</span></span>

* [<span data-ttu-id="468a2-131">Kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="468a2-131">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="468a2-132">Merkezler</span><span class="sxs-lookup"><span data-stu-id="468a2-132">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="468a2-133">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="468a2-133">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
