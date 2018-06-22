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
# <a name="manage-users-and-groups-in-signalr"></a>SignalR alanındaki kullanıcılar ve grupları yönetme

Tarafından [Brennan Conroy](https://github.com/BrennanConroy)

SignalR iletileri belirli bir kullanıcıyla ilişkili tüm bağlantıları gönderilmesi gibi adlı gruplarına bağlantılar sağlar.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(nasıl indirileceğini)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>SignalR kullanıcılar

SignalR belirli bir kullanıcıyla ilişkili tüm bağlantılara ileti göndermenizi sağlar. Varsayılan olarak SignalR kullanır `ClaimTypes.NameIdentifier` gelen `ClaimsPrincipal` kullanıcı tanımlayıcısı olarak bağlantı ile ilişkili. Tek bir kullanıcı bir SignalR uygulaması birden çok bağlantı olabilir. Örneğin, bir kullanıcının telefon yanı sıra Masaüstü üzerinde bağlı. Her cihaz için ayrı bir SignalR bağlantısı olsa da, aynı kullanıcıyla ilişkili tüm. Kullanıcıya bir ileti gönderilir, o kullanıcıyla ilişkili bağlantılarının tümünü iletiyi alır.

Kullanıcı tanımlayıcısı geçirerek belirli bir kullanıcıya bir ileti gönderme `User` aşağıdaki örnekte gösterildiği gibi hub yönteminin işlev:

> [!NOTE]
> Kullanıcı Kimliği büyük/küçük harf duyarlıdır.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

Kullanıcı tanımlayıcısı oluşturarak özelleştirilebilir bir `IUserIdProvider`ve içinde kaydetme `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> Özel SignalR hizmetlerinizi kaydetmeden önce AddSignalR çağrılmalıdır.

## <a name="groups-in-signalr"></a>SignalR grupları

Bir grup adı ile ilişkili bağlantıları koleksiyonudur. İletiler, bir grup içindeki tüm bağlantılar için gönderilebilir. Bir bağlantı veya birden çok bağlantı grupları uygulama tarafından yönetildiğinden göndermek için önerilen yöntem gruplarıdır. Bir bağlantı birden çok grupların üyesi olabilir. Bu gruplar bir sohbet uygulaması gibi bir şey için burada her alan bir grup olarak temsil edilebilir ideal hale getirir. Bağlantıları eklenebilir ya da grupları kaldırılmasını `AddToGroupAsync` ve `RemoveFromGroupAsync` yöntemleri.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Bir bağlantı bağlandığında grup üyeliği korunur değil. Bağlantı yeniden kurulduğunda gruba katılmaya gerekir. Bu bilgiler uygulamanın birden çok sunucuya ölçeklendirilir kullanılabilir olmadığından bir grubun üyelerini saymak mümkün değildir.

> [!NOTE]
> Grup adları büyük/küçük harfe duyarlıdır.

## <a name="related-resources"></a>İlgili kaynaklar

* [Kullanmaya başlama](xref:tutorials/signalr)
* [Merkezler](xref:signalr/hubs)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
