---
title: SignalR öğesindeki kullanıcılar ve Gruplar'ı yönetme
author: bradygaster
description: ASP.NET Core SignalR kullanıcı ve Grup yönetimine genel bakış.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 45f2bb44e03a586b7fc186525fdd3a2645c820d5
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667758"
---
# <a name="manage-users-and-groups-in-signalr"></a>SignalR öğesindeki kullanıcılar ve Gruplar'ı yönetme

Tarafından [Brennan Conroy](https://github.com/BrennanConroy)

SignalR iletileri belirli bir kullanıcıyla ilişkili tüm bağlantıları gönderilmesi gibi adlı gruplarına bağlantılar sağlar.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(karşıdan yükleme)](xref:index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Signalr'da kullanıcılar

SignalR, belirli bir kullanıcıyla ilişkili tüm bağlantılara ileti göndermek sağlar. Varsayılan olarak, SignalR kullanır `ClaimTypes.NameIdentifier` gelen `ClaimsPrincipal` kullanıcı tanımlayıcısı olarak bağlantı ile ilişkili. Tek bir kullanıcı bir SignalR uygulama birden fazla bağlantı olabilir. Örneğin, bir kullanıcının telefon numaraları yanı sıra Masaüstü bağlı. Her cihaz için ayrı bir SignalR bağlantı olsa da, bunlar aynı kullanıcıyla ilişkili tüm. Kullanıcıya bir ileti gönderildiğinde, tüm bu kullanıcı ile ilişkili bağlantıları iletisi alırsınız. Kullanıcı tanımlayıcısı için bir bağlantı tarafından erişilebilen `Context.UserIdentifier` hub'ınızdaki özelliği.

Kullanıcı tanımlayıcısı için geçirerek belirli bir kullanıcıya bir ileti gönder `User` aşağıdaki örnekte gösterildiği gibi hub yönteminizi işlev:

> [!NOTE]
> Kullanıcı tanımlayıcısı büyük/küçük harf duyarlıdır.

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a>Signalr'da gruplarla

Bir grubu, bir ad ile ilişkili bağlantılar koleksiyonudur. Bir gruptaki tüm bağlantılara ileti gönderilebilir. Grupları bir bağlantı veya birden çok bağlantı grupları uygulama tarafından yönetildiği göndermek için önerilen yoldur. Bir bağlantı, birden çok grubunun bir üyesi olabilir. Bu grup için bir sohbet uygulaması gibi burada her odanın bir grup olarak temsil edilebilir ideal hale getirir. Bağlantılar için eklendiğinde veya grupları kaldırıldığında `AddToGroupAsync` ve `RemoveFromGroupAsync` yöntemleri.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Bir bağlantı bağlandığında, grup üyeliği korunmaz. Bağlantı yeniden kurulduğunda gruba katılabilir gerekir. Bu bilgiler uygulamanın birden çok sunucuya ölçeklendirilirse kullanılabilir olmadığından, bir grubun üyelerinin sayısı mümkün değildir.

Grupları kullanarak kaynaklarına erişimi korumak için [kimlik doğrulama ve yetkilendirme](xref:signalr/authn-and-authz) ASP.NET Core işlevselliği. Kimlik bilgilerini o grup için geçerli olduğunda, yalnızca bir gruba kullanıcı ekleme, bu gruba gönderilen iletileri yalnızca yetkili kullanıcıların gidin. Ancak, Grup bir güvenlik özelliği değildir. Kimlik doğrulaması talep süre sonu ve iptal etme gibi gruplar desteklemediği özelliklere sahip. Bir kullanıcı grubuna erişim izni iptal edilirse, el ile algılayan ve bunları gruptan kaldırmanız gerekir.

> [!NOTE]
> Grup adları büyük/küçük harfe duyarlıdır.

## <a name="related-resources"></a>İlgili kaynaklar

* [Kullanmaya başlama](xref:tutorials/signalr)
* [Merkezler](xref:signalr/hubs)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
