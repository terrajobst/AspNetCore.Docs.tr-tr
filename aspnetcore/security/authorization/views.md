---
title: ASP.NET Core MVC 'de görünüm tabanlı yetkilendirme
author: rick-anderson
description: Bu belgede, ASP.NET Core Razor görünümü içinde yetkilendirme hizmetinin nasıl ekleneceği ve kullanılacağı gösterilir.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/views
ms.openlocfilehash: fc03da9eb98d36ffdda932ee5b16f327c2be9f83
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663599"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>ASP.NET Core MVC 'de görünüm tabanlı yetkilendirme

Geliştirici genellikle geçerli kullanıcı kimliğine göre Kullanıcı arabirimini göstermek, gizlemek veya değiştirmek ister. [Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla MVC görünümleri içindeki yetkilendirme hizmetine erişebilirsiniz. Yetkilendirme hizmetini Razor görünümüne eklemek için `@inject` yönergesini kullanın:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Her görünümde yetkilendirme hizmeti istiyorsanız, `@inject` yönergesini *Görünümler* dizininin *_ViewImports. cshtml* dosyasına yerleştirin. Daha fazla bilgi için bkz. [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).

[Kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)sırasında kontrol yaptığınız şekilde `AuthorizeAsync` çağırmak için eklenen yetkilendirme hizmetini kullanın:

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

Bazı durumlarda, kaynak görünüm modeliniz olur. [Kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)sırasında `AuthorizeAsync` tam olarak kontrol ettiğiniz şekilde çağırın:

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

Yukarıdaki kodda, model, ilke değerlendirmesinin dikkate alınması gereken bir kaynak olarak geçirilir.

> [!WARNING]
> Tek yetkilendirme denetimi olarak uygulamanızın kullanıcı arabirimi öğelerinin görünürlüğünü değiştirmeye güvenmeyin. Bir kullanıcı arabirimi öğesini gizlemek, ilişkili denetleyici eylemine erişimi tamamen engellemeyebilir. Örneğin, önceki kod parçacığındaki düğmesini göz önünde bulundurun. Göreli kaynak URL 'SI */Document/Edit/1*olduğunu biliyorsa, Kullanıcı `Edit` Action metodunu çağırabilir. Bu nedenle `Edit` Action yöntemi kendi yetkilendirme denetimini gerçekleştirmelidir.
