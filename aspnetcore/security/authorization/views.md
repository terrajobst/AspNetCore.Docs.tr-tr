---
title: ASP.NET Core MVC görünüm tabanlı yetkilendirme
author: rick-anderson
description: Bu belge, bir ASP.NET Core Razor görünümü içinde yetkilendirme hizmetine ekleme ve kullanma gösterilmektedir.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898652"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>ASP.NET Core MVC görünüm tabanlı yetkilendirme

Bir geliştirici genellikle görüntüleme, gizleme veya aksi halde geçerli kullanıcı kimliğine göre bir kullanıcı Arabirimi değiştirmek istiyor. MVC görünümleri yetkilendirme hizmetinde erişebileceğiniz [bağımlılık ekleme](xref:fundamentals/dependency-injection). Razor görünüme yetkilendirme hizmet ekleme, kullanma `@inject` yönergesi:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Her görünüm yetkilendirme hizmetinde istiyorsanız koyun `@inject` içine yönerge *_viewımports.cshtml* dosya *görünümleri* dizin. Daha fazla bilgi için [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).

Eklenen yetkilendirme hizmetini çağırmak için kullanın `AuthorizeAsync` tam olarak denetimi sırasında aynı şekilde [kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

Bazı durumlarda, kaynağın görünüm modelinizi olacaktır. Çağırma `AuthorizeAsync` tam olarak denetimi sırasında aynı şekilde [kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

Önceki kodda, model dikkate ilke değerlendirmesi gerçekleştirmesi gereken bir kaynak olarak geçirilir.

> [!WARNING]
> Uygulamanızın kullanıcı Arabirimi öğeleri tek bir yetkilendirme onay olarak geçiş görünürlüğünü güvenmeyin. UI öğesi gizleme tamamen erişim ilişkili denetleyici eylemi için engel. Örneğin, yukarıdaki kod parçacığında düğmede göz önünde bulundurun. Bir kullanıcının çağırabileceği `Edit` göreli kaynak biliyorsa eylem yöntemine URL'dir */Document/Edit/1*. Bu nedenle, `Edit` eylem yöntemi kendi yetkilendirme onay gerçekleştirmeniz.
