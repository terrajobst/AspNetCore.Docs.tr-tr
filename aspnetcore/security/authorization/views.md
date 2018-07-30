---
title: ASP.NET Core MVC görünüm tabanlı yetkilendirme
author: rick-anderson
description: Bu belge, bir ASP.NET Core Razor görünümü içinde yetkilendirme hizmetine ekleme ve kullanma gösterilmektedir.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342542"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="8b085-103">ASP.NET Core MVC görünüm tabanlı yetkilendirme</span><span class="sxs-lookup"><span data-stu-id="8b085-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="8b085-104">Bir geliştirici genellikle görüntüleme, gizleme veya aksi halde geçerli kullanıcı kimliğine göre bir kullanıcı Arabirimi değiştirmek istiyor.</span><span class="sxs-lookup"><span data-stu-id="8b085-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="8b085-105">MVC görünümleri yetkilendirme hizmetinde erişebileceğiniz [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8b085-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="8b085-106">Razor görünüme yetkilendirme hizmet ekleme, kullanma `@inject` yönergesi:</span><span class="sxs-lookup"><span data-stu-id="8b085-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="8b085-107">Her görünüm yetkilendirme hizmetinde istiyorsanız koyun `@inject` içine yönerge *_viewımports.cshtml* dosya *görünümleri* dizin.</span><span class="sxs-lookup"><span data-stu-id="8b085-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="8b085-108">Daha fazla bilgi için [görünümlere bağımlılık ekleme](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8b085-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="8b085-109">Eklenen yetkilendirme hizmetini çağırmak için kullanın `AuthorizeAsync` tam olarak denetimi sırasında aynı şekilde [kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="8b085-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b085-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b085-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b085-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b085-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="8b085-112">Bazı durumlarda, kaynağın görünüm modelinizi olacaktır.</span><span class="sxs-lookup"><span data-stu-id="8b085-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="8b085-113">Çağırma `AuthorizeAsync` tam olarak denetimi sırasında aynı şekilde [kaynak tabanlı yetkilendirme](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="8b085-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8b085-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8b085-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8b085-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8b085-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="8b085-116">Önceki kodda, model dikkate ilke değerlendirmesi gerçekleştirmesi gereken bir kaynak olarak geçirilir.</span><span class="sxs-lookup"><span data-stu-id="8b085-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="8b085-117">Uygulamanızın kullanıcı Arabirimi öğeleri tek bir yetkilendirme onay olarak geçiş görünürlüğünü güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="8b085-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="8b085-118">UI öğesi gizleme tamamen erişim ilişkili denetleyici eylemi için engel.</span><span class="sxs-lookup"><span data-stu-id="8b085-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="8b085-119">Örneğin, yukarıdaki kod parçacığında düğmede göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="8b085-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="8b085-120">Bir kullanıcının çağırabileceği `Edit` göreli kaynak biliyorsa eylem yöntemine URL'dir */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="8b085-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="8b085-121">Bu nedenle, `Edit` eylem yöntemi kendi yetkilendirme onay gerçekleştirmeniz.</span><span class="sxs-lookup"><span data-stu-id="8b085-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
