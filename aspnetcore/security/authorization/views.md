---
title: "ASP.NET Core MVC görünümü tabanlı yetkilendirme"
author: rick-anderson
description: "Bu belge, Ekle ve ASP.NET Core Razor görünüm içinde yetkilendirme hizmeti kullanma gösterilmektedir."
keywords: ASP.NET Core, yetkilendirme, IAuthorizationService, Razor yetkilendirme
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a>Görünüm tabanlı bir yetkilendirme

Bir geliştirici genellikle gösterme, gizleme veya aksi halde geçerli kullanıcı kimliğine göre bir kullanıcı Arabirimi değiştirmek istiyor. MVC görünümleri yetkilendirme Hizmeti'nde erişebilirsiniz [bağımlılık ekleme](xref:fundamentals/dependency-injection#fundamentals-dependency-injection). Yetkilendirme hizmeti Razor görünüme eklemesine kullanmak `@inject` yönergesi:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Her görünüm yetkilendirme hizmetinde istiyorsanız koyun `@inject` içine yönerge *_viewımports.cshtml* dosyasının *görünümleri* dizin. Daha fazla bilgi için bkz: [bağımlılık ekleme görünümleri içine](xref:mvc/views/dependency-injection).

Çağrılacak eklenen yetkilendirme hizmetini kullanmak `AuthorizeAsync` tam olarak denetleyin sırasında aynı şekilde [kaynak tabanlı bir yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

Bazı durumlarda, kaynak görünümü modelinizi olacaktır. Çağırma `AuthorizeAsync` tam olarak denetleyin sırasında aynı şekilde [kaynak tabanlı bir yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

Önceki kodda model dikkate ilke değerlendirmesi gerçekleştirmesi gereken bir kaynak olarak geçirilir.

> [!WARNING]
> Uygulamanızın kullanıcı Arabirimi öğeleri tek yetkilendirme onay olarak geçiş görünürlüğünü güvenmeyin. Bir kullanıcı Arabirimi öğesi gizleme tamamen erişim, ilişkilendirilen denetleyiciye eylemini engel. Örneğin, yukarıdaki kod parçacığında düğmesini göz önünde bulundurun. Bir kullanıcının çağırabileceği `Edit` göreli kaynak biliyorsa eylem yöntemi URL'dir */Document/Edit/1*. Bu nedenle, `Edit` eylem yöntemi kendi yetkilendirme onay gerçekleştirmeniz.
