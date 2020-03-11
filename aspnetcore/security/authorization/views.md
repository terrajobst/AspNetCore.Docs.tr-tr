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
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="3756b-103">ASP.NET Core MVC 'de görünüm tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="3756b-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="3756b-104">Geliştirici genellikle geçerli kullanıcı kimliğine göre Kullanıcı arabirimini göstermek, gizlemek veya değiştirmek ister.</span><span class="sxs-lookup"><span data-stu-id="3756b-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="3756b-105">[Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla MVC görünümleri içindeki yetkilendirme hizmetine erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3756b-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3756b-106">Yetkilendirme hizmetini Razor görünümüne eklemek için `@inject` yönergesini kullanın:</span><span class="sxs-lookup"><span data-stu-id="3756b-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="3756b-107">Her görünümde yetkilendirme hizmeti istiyorsanız, `@inject` yönergesini *Görünümler* dizininin *_ViewImports. cshtml* dosyasına yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="3756b-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="3756b-108">Daha fazla bilgi için bkz. [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3756b-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="3756b-109">[Kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)sırasında kontrol yaptığınız şekilde `AuthorizeAsync` çağırmak için eklenen yetkilendirme hizmetini kullanın:</span><span class="sxs-lookup"><span data-stu-id="3756b-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

<span data-ttu-id="3756b-110">Bazı durumlarda, kaynak görünüm modeliniz olur.</span><span class="sxs-lookup"><span data-stu-id="3756b-110">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="3756b-111">[Kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative)sırasında `AuthorizeAsync` tam olarak kontrol ettiğiniz şekilde çağırın:</span><span class="sxs-lookup"><span data-stu-id="3756b-111">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

<span data-ttu-id="3756b-112">Yukarıdaki kodda, model, ilke değerlendirmesinin dikkate alınması gereken bir kaynak olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="3756b-112">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="3756b-113">Tek yetkilendirme denetimi olarak uygulamanızın kullanıcı arabirimi öğelerinin görünürlüğünü değiştirmeye güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="3756b-113">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="3756b-114">Bir kullanıcı arabirimi öğesini gizlemek, ilişkili denetleyici eylemine erişimi tamamen engellemeyebilir.</span><span class="sxs-lookup"><span data-stu-id="3756b-114">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="3756b-115">Örneğin, önceki kod parçacığındaki düğmesini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="3756b-115">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="3756b-116">Göreli kaynak URL 'SI */Document/Edit/1*olduğunu biliyorsa, Kullanıcı `Edit` Action metodunu çağırabilir.</span><span class="sxs-lookup"><span data-stu-id="3756b-116">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="3756b-117">Bu nedenle `Edit` Action yöntemi kendi yetkilendirme denetimini gerçekleştirmelidir.</span><span class="sxs-lookup"><span data-stu-id="3756b-117">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
