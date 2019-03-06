---
title: ASP.NET Core Razor sayfalar yetkilendirme kuralları
author: guardrex
description: Kullanıcılara yetki vermek ve anonim kullanıcıların sayfa veya sayfaları klasör erişmesine izin veren kuralları ile sayfalarına erişimi denetlemeyi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 040d33eba7eaf7a3aece2eedcdef7343e52972af
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345515"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="63294-103">ASP.NET Core Razor sayfalar yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="63294-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="63294-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="63294-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="63294-105">Razor sayfaları uygulamanıza erişimi denetlemek için bir yolu, başlangıçta yetkilendirme kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="63294-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="63294-106">Bu kuralları kullanıcılara yetki vermek ve anonim kullanıcıların tek tek sayfaları veya klasörleri sayfaların erişmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="63294-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="63294-107">Otomatik olarak bu konuda açıklanan kurallarını uygulamak [yetkilendirme filtrelerini](xref:mvc/controllers/filters#authorization-filters) erişimi denetleme.</span><span class="sxs-lookup"><span data-stu-id="63294-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="63294-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="63294-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="63294-109">Örnek uygulama kullandığı [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="63294-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="63294-110">Bu konuda gösterilen örnekler ve kavramlar eşit olarak ASP.NET Core kimliği kullanan uygulamalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="63294-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="63294-111">ASP.NET Core kimliği kullanmak için sunulan yönergeleri <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="63294-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="63294-112">Bir sayfaya erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="63294-112">Require authorization to access a page</span></span>

<span data-ttu-id="63294-113">Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> belirtilen yolda sayfasına:</span><span class="sxs-lookup"><span data-stu-id="63294-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="63294-114">Belirtilen yol bir uzantı ve yalnızca ileri eğik çizgi içeren olmadan Razor sayfaları kök göreli yolu görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="63294-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="63294-115">Belirtmek için bir [yetkilendirme ilkesi](xref:security/authorization/policies), kullanan bir [AuthorizePage aşırı](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="63294-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="63294-116">Bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sayfa modeli sınıfı ile uygulanabilir `[Authorize]` filtre özniteliği.</span><span class="sxs-lookup"><span data-stu-id="63294-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="63294-117">Daha fazla bilgi için [Authorize filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="63294-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="63294-118">Sayfaların bir klasöre erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="63294-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="63294-119">Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> belirtilen yolda bir klasördeki tüm sayfalar için:</span><span class="sxs-lookup"><span data-stu-id="63294-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="63294-120">Belirtilen Razor sayfaları kök göreli yol görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="63294-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="63294-121">Belirtmek için bir [yetkilendirme ilkesi](xref:security/authorization/policies), kullanan bir [AuthorizeFolder aşırı](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="63294-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="63294-122">Bir alan sayfasına erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="63294-122">Require authorization to access an area page</span></span>

<span data-ttu-id="63294-123">Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> belirtilen yolda alanı sayfası:</span><span class="sxs-lookup"><span data-stu-id="63294-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="63294-124">Sayfa adı sayfaları kök dizinine göreli bir uzantısı olmadan dosya belirtilen alan için yoludur.</span><span class="sxs-lookup"><span data-stu-id="63294-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="63294-125">Örneğin, sayfa adı dosya için *Areas/Identity/Pages/Manage/Accounts.cshtml* olduğu */Yönet/hesapları*.</span><span class="sxs-lookup"><span data-stu-id="63294-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="63294-126">Belirtmek için bir [yetkilendirme ilkesi](xref:security/authorization/policies), kullanan bir [AuthorizeAreaPage aşırı](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="63294-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="63294-127">Alanların bir klasöre erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="63294-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="63294-128">Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> tüm alanlarda belirtilen yolda bir klasör:</span><span class="sxs-lookup"><span data-stu-id="63294-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="63294-129">Belirtilen alan sayfaları kök dizinine göreli bir klasör yolu klasör yoludur.</span><span class="sxs-lookup"><span data-stu-id="63294-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="63294-130">Örneğin, dosyalar için klasör yolu *alanlar/kimlik/sayfaları/Yönet/* olduğu */yönetme*.</span><span class="sxs-lookup"><span data-stu-id="63294-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="63294-131">Belirtmek için bir [yetkilendirme ilkesi](xref:security/authorization/policies), kullanan bir [AuthorizeAreaFolder aşırı](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="63294-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="63294-132">Bir sayfaya anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="63294-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="63294-133">Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> belirtilen yolda bir sayfaya:</span><span class="sxs-lookup"><span data-stu-id="63294-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="63294-134">Belirtilen yol bir uzantı ve yalnızca ileri eğik çizgi içeren olmadan Razor sayfaları kök göreli yolu görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="63294-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="63294-135">Sayfaların bir klasörde anonim erişime izin verin</span><span class="sxs-lookup"><span data-stu-id="63294-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="63294-136">Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> belirtilen yolda bir klasördeki tüm sayfalar için:</span><span class="sxs-lookup"><span data-stu-id="63294-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="63294-137">Belirtilen Razor sayfaları kök göreli yol görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="63294-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="63294-138">Yetkili birleştirme ilgili not ve anonim erişim</span><span class="sxs-lookup"><span data-stu-id="63294-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="63294-139">Belirtmek için geçerli olan bir klasörü yetkilendirme gerektiren sayfaları ve o klasördeki bir sayfa anonim erişime izin verdiğini belirtin:</span><span class="sxs-lookup"><span data-stu-id="63294-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="63294-140">Ancak bunun tersi geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="63294-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="63294-141">Sayfaların anonim erişim için bir klasör bildirmek olamaz ve sonra bir sayfa yetkilendirme gerektirir, klasörün içindeki belirtin:</span><span class="sxs-lookup"><span data-stu-id="63294-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="63294-142">Özel sayfasında gerektiren yetkilendirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="63294-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="63294-143">Hem <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ve <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sayfaya uygulanan <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> önceliklidir ve erişimi denetler.</span><span class="sxs-lookup"><span data-stu-id="63294-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="63294-144">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="63294-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
