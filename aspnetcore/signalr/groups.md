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
ms.openlocfilehash: 7e8c85dcbc46daa68988374f499f19a9d2cead47
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665867"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a>SignalR içinde kullanıcıları ve grupları yönetme

[Brennan Conroy](https://github.com/BrennanConroy) tarafından

SignalR, iletilerin belirli bir kullanıcıyla ilişkili tüm bağlantılara ve ayrıca adlandırılmış bağlantı gruplarına gönderilmesini sağlar.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [(nasıl indirileceği)](xref:index#how-to-download-a-sample)

## <a name="users-in-opno-locsignalr"></a>SignalR kullanıcılar

SignalR, belirli bir kullanıcıyla ilişkili tüm bağlantılara ileti göndermenizi sağlar. Varsayılan olarak, SignalR, Kullanıcı tanımlayıcısı olarak bağlantıyla ilişkili `ClaimsPrincipal` `ClaimTypes.NameIdentifier` kullanır. Tek bir kullanıcının SignalR uygulamasına birden fazla bağlantısı olabilir. Örneğin, bir Kullanıcı masaüstüne ve telefonlarına bağlanabilir. Her cihazın ayrı bir SignalR bağlantısı vardır, ancak hepsi aynı kullanıcıyla ilişkilendirilir. Kullanıcıya bir ileti gönderildiğinde, bu kullanıcıyla ilişkili tüm bağlantılar iletiyi alır. Bir bağlantı için Kullanıcı tanımlayıcısına, hub 'ınızdaki `Context.UserIdentifier` özelliği tarafından erişilebilir.

Aşağıdaki örnekte gösterildiği gibi, kullanıcı tanımlayıcısını hub yönteminizin `User` işleve geçirerek belirli bir kullanıcıya bir ileti gönderin:

> [!NOTE]
> Kullanıcı tanımlayıcısı büyük/küçük harfe duyarlıdır.

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a>SignalR gruplar

Grup, bir adla ilişkili bir bağlantı koleksiyonudur. İletiler, bir gruptaki tüm bağlantılara gönderilebilir. Gruplar uygulama tarafından yönetildiğinden, gruplar bir bağlantıya veya birden çok bağlantıya göndermek için önerilen yoldur. Bir bağlantı, birden fazla grubun üyesi olabilir. Bu, grupları her odanın bir grup olarak gösterilebileceği bir sohbet uygulaması gibi bir şey için ideal hale getirir. `AddToGroupAsync` ve `RemoveFromGroupAsync` yöntemleri aracılığıyla gruplardan bağlantılar eklenebilir veya gruplardan kaldırılabilir.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Bağlantı yeniden bağlandığında grup üyeliği korunmaz. Bağlantı, yeniden oluşturulduğunda gruba yeniden katılması gerekir. Uygulama birden çok sunucuya ölçeklendirildiyse bu bilgiler kullanılamadığından, bir grubun üyelerini saymak mümkün değildir.

Grupları kullanırken kaynaklara erişimi korumak için ASP.NET Core [kimlik doğrulaması ve yetkilendirme](xref:signalr/authn-and-authz) işlevini kullanın. Yalnızca bu grup için kimlik bilgileri geçerliyse bir gruba Kullanıcı eklerseniz, bu gruba gönderilen iletiler yalnızca yetkili kullanıcılara gider. Ancak, gruplar bir güvenlik özelliği değildir. Kimlik doğrulama talepleri, süre sonu ve iptal gibi grupların olmadığı özelliklere sahiptir. Bir kullanıcının gruba erişim izni iptal edildiğinde, bunu el ile tespit etmeniz ve gruptan kaldırmanız gerekir.

> [!NOTE]
> Grup adları büyük/küçük harfe duyarlıdır.

## <a name="related-resources"></a>İlgili kaynaklar

* [Başlarken](xref:tutorials/signalr)
* [Merkezler](xref:signalr/hubs)
* [Azure'a Yayımlama](xref:signalr/publish-to-azure-web-app)
