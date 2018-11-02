---
title: SignalR öğesindeki kullanıcılar ve Gruplar'ı yönetme
author: tdykstra
description: ASP.NET Core SignalR kullanıcı ve Grup yönetimine genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 02db46f090c487a03171de244ff7ad0d5e9de0fa
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758173"
---
# <a name="manage-users-and-groups-in-signalr"></a><span data-ttu-id="020c3-103">SignalR öğesindeki kullanıcılar ve Gruplar'ı yönetme</span><span class="sxs-lookup"><span data-stu-id="020c3-103">Manage users and groups in SignalR</span></span>

<span data-ttu-id="020c3-104">Tarafından [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="020c3-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="020c3-105">SignalR iletileri belirli bir kullanıcıyla ilişkili tüm bağlantıları gönderilmesi gibi adlı gruplarına bağlantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="020c3-105">SignalR allows messages to be sent to all connections associated with a specific user, as well as to named groups of connections.</span></span>

<span data-ttu-id="020c3-106">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(karşıdan yükleme)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="020c3-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="users-in-signalr"></a><span data-ttu-id="020c3-107">Signalr'da kullanıcılar</span><span class="sxs-lookup"><span data-stu-id="020c3-107">Users in SignalR</span></span>

<span data-ttu-id="020c3-108">SignalR, belirli bir kullanıcıyla ilişkili tüm bağlantılara ileti göndermek sağlar.</span><span class="sxs-lookup"><span data-stu-id="020c3-108">SignalR allows you to send messages to all connections associated with a specific user.</span></span> <span data-ttu-id="020c3-109">Varsayılan olarak, SignalR kullanır `ClaimTypes.NameIdentifier` gelen `ClaimsPrincipal` kullanıcı tanımlayıcısı olarak bağlantı ile ilişkili.</span><span class="sxs-lookup"><span data-stu-id="020c3-109">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> <span data-ttu-id="020c3-110">Tek bir kullanıcı bir SignalR uygulama birden fazla bağlantı olabilir.</span><span class="sxs-lookup"><span data-stu-id="020c3-110">A single user can have multiple connections to a SignalR app.</span></span> <span data-ttu-id="020c3-111">Örneğin, bir kullanıcının telefon numaraları yanı sıra Masaüstü bağlı.</span><span class="sxs-lookup"><span data-stu-id="020c3-111">For example, a user could be connected on their desktop as well as their phone.</span></span> <span data-ttu-id="020c3-112">Her cihaz için ayrı bir SignalR bağlantı olsa da, bunlar aynı kullanıcıyla ilişkili tüm.</span><span class="sxs-lookup"><span data-stu-id="020c3-112">Each device has a separate SignalR connection, but they're all associated with the same user.</span></span> <span data-ttu-id="020c3-113">Kullanıcıya bir ileti gönderildiğinde, tüm bu kullanıcı ile ilişkili bağlantıları iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="020c3-113">If a message is sent to the user, all of the connections associated with that user receive the message.</span></span> <span data-ttu-id="020c3-114">Kullanıcı tanımlayıcısı için bir bağlantı tarafından erişilebilen `Context.UserIdentifier` hub'ınızdaki özelliği.</span><span class="sxs-lookup"><span data-stu-id="020c3-114">The user identifier for a connection can be accessed by the `Context.UserIdentifier` property in your hub.</span></span>

<span data-ttu-id="020c3-115">Kullanıcı tanımlayıcısı için geçirerek belirli bir kullanıcıya bir ileti gönder `User` aşağıdaki örnekte gösterildiği gibi hub yönteminizi işlev:</span><span class="sxs-lookup"><span data-stu-id="020c3-115">Send a message to a specific user by passing the user identifier to the `User` function in your hub method as shown in the following example:</span></span>

> [!NOTE]
> <span data-ttu-id="020c3-116">Kullanıcı tanımlayıcısı büyük/küçük harf duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="020c3-116">The user identifier is case-sensitive.</span></span>

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

<span data-ttu-id="020c3-117">Kullanıcı tanımlayıcısı oluşturarak özelleştirilebilir bir `IUserIdProvider`ve onu kaydetme `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="020c3-117">The user identifier can be customized by creating an `IUserIdProvider`, and registering it in `ConfigureServices`.</span></span>

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> <span data-ttu-id="020c3-118">AddSignalR özel SignalR hizmetlerinizi kaydetmeden önce çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="020c3-118">AddSignalR must be called before registering your custom SignalR services.</span></span>

## <a name="groups-in-signalr"></a><span data-ttu-id="020c3-119">Signalr'da gruplarla</span><span class="sxs-lookup"><span data-stu-id="020c3-119">Groups in SignalR</span></span>

<span data-ttu-id="020c3-120">Bir grubu, bir ad ile ilişkili bağlantılar koleksiyonudur.</span><span class="sxs-lookup"><span data-stu-id="020c3-120">A group is a collection of connections associated with a name.</span></span> <span data-ttu-id="020c3-121">Bir gruptaki tüm bağlantılara ileti gönderilebilir.</span><span class="sxs-lookup"><span data-stu-id="020c3-121">Messages can be sent to all connections in a group.</span></span> <span data-ttu-id="020c3-122">Grupları bir bağlantı veya birden çok bağlantı grupları uygulama tarafından yönetildiği göndermek için önerilen yoldur.</span><span class="sxs-lookup"><span data-stu-id="020c3-122">Groups are the recommended way to send to a connection or multiple connections because the groups are managed by the application.</span></span> <span data-ttu-id="020c3-123">Bir bağlantı, birden çok grubunun bir üyesi olabilir.</span><span class="sxs-lookup"><span data-stu-id="020c3-123">A connection can be a member of multiple groups.</span></span> <span data-ttu-id="020c3-124">Bu grup için bir sohbet uygulaması gibi burada her odanın bir grup olarak temsil edilebilir ideal hale getirir.</span><span class="sxs-lookup"><span data-stu-id="020c3-124">This makes groups ideal for something like a chat application, where each room can be represented as a group.</span></span> <span data-ttu-id="020c3-125">Bağlantılar için eklendiğinde veya grupları kaldırıldığında `AddToGroupAsync` ve `RemoveFromGroupAsync` yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="020c3-125">Connections can be added to or removed from groups via the `AddToGroupAsync` and `RemoveFromGroupAsync` methods.</span></span>

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

<span data-ttu-id="020c3-126">Bir bağlantı bağlandığında, grup üyeliği korunmaz.</span><span class="sxs-lookup"><span data-stu-id="020c3-126">Group membership isn't preserved when a connection reconnects.</span></span> <span data-ttu-id="020c3-127">Bağlantı yeniden kurulduğunda gruba katılabilir gerekir.</span><span class="sxs-lookup"><span data-stu-id="020c3-127">The connection needs to rejoin the group when it's re-established.</span></span> <span data-ttu-id="020c3-128">Bu bilgiler uygulamanın birden çok sunucuya ölçeklendirilirse kullanılabilir olmadığından, bir grubun üyelerinin sayısı mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="020c3-128">It's not possible to count the members of a group, since this information is not available if the application is scaled to multiple servers.</span></span>

<span data-ttu-id="020c3-129">Grupları kullanarak kaynaklarına erişimi korumak için [kimlik doğrulama ve yetkilendirme](xref:signalr/authn-and-authz) ASP.NET Core işlevselliği.</span><span class="sxs-lookup"><span data-stu-id="020c3-129">To protect access to resources while using groups, use [authentication and authorization](xref:signalr/authn-and-authz) functionality in ASP.NET Core.</span></span> <span data-ttu-id="020c3-130">Kimlik bilgilerini o grup için geçerli olduğunda, yalnızca bir gruba kullanıcı ekleme, bu gruba gönderilen iletileri yalnızca yetkili kullanıcıların gidin.</span><span class="sxs-lookup"><span data-stu-id="020c3-130">If you only add users to a group when the credentials are valid for that group, messages sent to that group will only go to authorized users.</span></span> <span data-ttu-id="020c3-131">Ancak, Grup bir güvenlik özelliği değildir.</span><span class="sxs-lookup"><span data-stu-id="020c3-131">However, groups are not a security feature.</span></span> <span data-ttu-id="020c3-132">Kimlik doğrulaması talep süre sonu ve iptal etme gibi gruplar desteklemediği özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="020c3-132">Authentication claims have features that groups do not, such as expiry and revocation.</span></span> <span data-ttu-id="020c3-133">Bir kullanıcı grubuna erişim izni iptal edilirse, el ile algılayan ve bunları gruptan kaldırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="020c3-133">If a user's permission to access the group is revoked, you have to manually detect that and remove them from the group.</span></span>

> [!NOTE]
> <span data-ttu-id="020c3-134">Grup adları büyük/küçük harfe duyarlıdır.</span><span class="sxs-lookup"><span data-stu-id="020c3-134">Group names are case-sensitive.</span></span>

## <a name="related-resources"></a><span data-ttu-id="020c3-135">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="020c3-135">Related resources</span></span>

* [<span data-ttu-id="020c3-136">Kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="020c3-136">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="020c3-137">Merkezler</span><span class="sxs-lookup"><span data-stu-id="020c3-137">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="020c3-138">Azure'a Yayımlama</span><span class="sxs-lookup"><span data-stu-id="020c3-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
