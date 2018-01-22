---
title: "ASP.NET Core MVC görünümü tabanlı yetkilendirme"
author: rick-anderson
description: "Bu belge, Ekle ve ASP.NET Core Razor görünüm içinde yetkilendirme hizmeti kullanma gösterilmektedir."
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 8f87ca90b77be1efd75688e8203cb57b1a3360ad
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="view-based-authorization"></a><span data-ttu-id="409ec-103">Görünüm tabanlı bir yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="409ec-103">View-based authorization</span></span>

<span data-ttu-id="409ec-104">Bir geliştirici genellikle gösterme, gizleme veya aksi halde geçerli kullanıcı kimliğine göre bir kullanıcı Arabirimi değiştirmek istiyor.</span><span class="sxs-lookup"><span data-stu-id="409ec-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="409ec-105">MVC görünümleri yetkilendirme Hizmeti'nde erişebilirsiniz [bağımlılık ekleme](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="409ec-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="409ec-106">Yetkilendirme hizmeti Razor görünüme eklemesine kullanmak `@inject` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="409ec-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="409ec-107">Her görünüm yetkilendirme hizmetinde istiyorsanız koyun `@inject` içine yönerge *_viewımports.cshtml* dosyasının *görünümleri* dizin.</span><span class="sxs-lookup"><span data-stu-id="409ec-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="409ec-108">Daha fazla bilgi için bkz: [bağımlılık ekleme görünümleri içine](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="409ec-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="409ec-109">Çağrılacak eklenen yetkilendirme hizmetini kullanmak `AuthorizeAsync` tam olarak denetleyin sırasında aynı şekilde [kaynak tabanlı bir yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="409ec-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="409ec-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="409ec-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="409ec-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="409ec-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="409ec-112">Bazı durumlarda, kaynak görünümü modelinizi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="409ec-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="409ec-113">Çağırma `AuthorizeAsync` tam olarak denetleyin sırasında aynı şekilde [kaynak tabanlı bir yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="409ec-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="409ec-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="409ec-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="409ec-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="409ec-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="409ec-116">Önceki kodda model dikkate ilke değerlendirmesi gerçekleştirmesi gereken bir kaynak olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="409ec-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="409ec-117">Uygulamanızın kullanıcı Arabirimi öğeleri tek yetkilendirme onay olarak geçiş görünürlüğünü güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="409ec-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="409ec-118">Bir kullanıcı Arabirimi öğesi gizleme tamamen erişim, ilişkilendirilen denetleyiciye eylemini engel.</span><span class="sxs-lookup"><span data-stu-id="409ec-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="409ec-119">Örneğin, yukarıdaki kod parçacığında düğmesini göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="409ec-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="409ec-120">Bir kullanıcının çağırabileceği `Edit` göreli kaynak biliyorsa eylem yöntemi URL'dir */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="409ec-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="409ec-121">Bu nedenle, `Edit` eylem yöntemi kendi yetkilendirme onay gerçekleştirmeniz.</span><span class="sxs-lookup"><span data-stu-id="409ec-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
