---
title: SignalR öğesindeki kullanıcılar ve Gruplar'ı yönetme
author: rachelappel
description: ASP.NET Core SignalR kullanıcı ve Grup yönetimine genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 3e5e310c84bc3ed5790d5b67a917bd54162ea163
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402531"
---
# <a name="manage-users-and-groups-in-signalr"></a>SignalR öğesindeki kullanıcılar ve Gruplar'ı yönetme

Tarafından [Brennan Conroy](https://github.com/BrennanConroy)

SignalR iletileri belirli bir kullanıcıyla ilişkili tüm bağlantıları gönderilmesi gibi adlı gruplarına bağlantılar sağlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(karşıdan yükleme)](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Signalr'da kullanıcılar

SignalR, belirli bir kullanıcıyla ilişkili tüm bağlantılara ileti göndermek sağlar. Varsayılan olarak, SignalR kullanır `ClaimTypes.NameIdentifier` gelen `ClaimsPrincipal` kullanıcı tanımlayıcısı olarak bağlantı ile ilişkili. Tek bir kullanıcı bir SignalR uygulama birden fazla bağlantı olabilir. Örneğin, bir kullanıcının telefon numaraları yanı sıra Masaüstü bağlı. Her cihaz için ayrı bir SignalR bağlantı olsa da, bunlar aynı kullanıcıyla ilişkili tüm. Kullanıcıya bir ileti gönderildiğinde, tüm bu kullanıcı ile ilişkili bağlantıları iletisi alırsınız. Kullanıcı tanımlayıcısı için bir bağlantı tarafından erişilebilen `Context.UserIdentifier` hub'ınızdaki özelliği.

Kullanıcı tanımlayıcısı için geçirerek belirli bir kullanıcıya bir ileti gönder `User` aşağıdaki örnekte gösterildiği gibi hub yönteminizi işlev:

> [!NOTE]
> Kullanıcı tanımlayıcısı büyük/küçük harf duyarlıdır.

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

Kullanıcı tanımlayıcısı oluşturarak özelleştirilebilir bir `IUserIdProvider`ve onu kaydetme `ConfigureServices`.

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> AddSignalR özel SignalR hizmetlerinizi kaydetmeden önce çağrılmalıdır.

## <a name="groups-in-signalr"></a>Signalr'da gruplarla

Bir grubu, bir ad ile ilişkili bağlantılar koleksiyonudur. Bir gruptaki tüm bağlantılara ileti gönderilebilir. Grupları bir bağlantı veya birden çok bağlantı grupları uygulama tarafından yönetildiği göndermek için önerilen yoldur. Bir bağlantı, birden çok grubunun bir üyesi olabilir. Bu grup için bir sohbet uygulaması gibi burada her odanın bir grup olarak temsil edilebilir ideal hale getirir. Bağlantılar için eklendiğinde veya grupları kaldırıldığında `AddToGroupAsync` ve `RemoveFromGroupAsync` yöntemleri.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Bir bağlantı bağlandığında, grup üyeliği korunmaz. Bağlantı yeniden kurulduğunda gruba katılabilir gerekir. Bu bilgiler uygulamanın birden çok sunucuya ölçeklendirilirse kullanılabilir olmadığından, bir grubun üyelerinin sayısı mümkün değildir.

> [!NOTE]
> Grup adları büyük/küçük harfe duyarlıdır.

## <a name="related-resources"></a>İlgili kaynaklar

* [Kullanmaya başlama](xref:tutorials/signalr)
* [Merkezler](xref:signalr/hubs)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
