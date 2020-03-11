---
title: ASP.NET Core Razor Pages yetkilendirme kuralları
author: rick-anderson
description: Kullanıcılara yetki veren ve anonim kullanıcıların sayfalara veya sayfa klasörlerine erişmesine izin veren kurallara sahip sayfalara erişimi denetleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 00fc487c6ac802f213bcf83994ecc2b1a1468589
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662059"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="304b7-103">ASP.NET Core Razor Pages yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="304b7-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="304b7-104">Razor Pages uygulamanızda erişimi denetlemeye yönelik bir yol, başlangıçta yetkilendirme kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="304b7-104">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="304b7-105">Bu kurallar, kullanıcıları yetkilendirmeniz ve anonim kullanıcıların ayrı sayfalara veya sayfa klasörlerine erişmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="304b7-105">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="304b7-106">Bu konu başlığı altında açıklanan kurallar, erişimi denetlemek için otomatik olarak [Yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) uygular.</span><span class="sxs-lookup"><span data-stu-id="304b7-106">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="304b7-107">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="304b7-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="304b7-108">Örnek uygulama [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını](xref:security/authentication/cookie)kullanır.</span><span class="sxs-lookup"><span data-stu-id="304b7-108">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="304b7-109">Bu konu başlığında gösterilen kavramlar ve örnekler ASP.NET Core kimliği kullanan uygulamalar için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="304b7-109">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="304b7-110">ASP.NET Core kimliği kullanmak için <xref:security/authentication/identity>yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="304b7-110">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="304b7-111">Bir sayfaya erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="304b7-111">Require authorization to access a page</span></span>

<span data-ttu-id="304b7-112">Belirtilen yoldaki sayfaya bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-112">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="304b7-113">Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-113">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="304b7-114">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek Için bir [authorizepage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-114">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="304b7-115">Bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, `[Authorize]` Filter özniteliğiyle bir sayfa modeli sınıfına uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="304b7-115">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="304b7-116">Daha fazla bilgi için bkz. [Yetkilendirme filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="304b7-116">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="304b7-117">Bir sayfa klasörüne erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="304b7-117">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="304b7-118">Belirtilen yoldaki bir klasördeki tüm sayfalara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-118">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="304b7-119">Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-119">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="304b7-120">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [authorizefolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-120">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="304b7-121">Alan sayfasına erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="304b7-121">Require authorization to access an area page</span></span>

<span data-ttu-id="304b7-122">Belirtilen yoldaki alan sayfasına bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-122">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="304b7-123">Sayfa adı, belirtilen alanın sayfalar kök dizinine göre uzantısı olmayan dosyanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-123">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="304b7-124">Örneğin, dosya *alanı/Identity/Pages/Manage/accounts. cshtml* için sayfa adı */Manage/accounts*olur.</span><span class="sxs-lookup"><span data-stu-id="304b7-124">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="304b7-125">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareapage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-125">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="304b7-126">Bir alan klasörüne erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="304b7-126">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="304b7-127">Belirtilen yoldaki bir klasördeki tüm alanlara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-127">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="304b7-128">Klasör yolu, belirtilen alanın sayfalar kök dizinine göre klasörün yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-128">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="304b7-129">Örneğin, *bölgeler/kimlik/sayfalar/Yönet/* \ *Yönet*altındaki dosyalar için klasör yolu.</span><span class="sxs-lookup"><span data-stu-id="304b7-129">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="304b7-130">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareafolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-130">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="304b7-131">Bir sayfaya anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="304b7-131">Allow anonymous access to a page</span></span>

<span data-ttu-id="304b7-132">Belirtilen yoldaki bir sayfaya <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-132">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="304b7-133">Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-133">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="304b7-134">Bir sayfa klasörüne anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="304b7-134">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="304b7-135">Belirtilen yoldaki bir klasördeki tüm sayfalara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-135">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="304b7-136">Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-136">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="304b7-137">Yetkili ve anonim erişimi birleştirme hakkında</span><span class="sxs-lookup"><span data-stu-id="304b7-137">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="304b7-138">Bir sayfa klasörünün yetkilendirme gerektirdiğini ve sonra bu klasörün içindeki bir sayfanın adsız erişime izin verdiğini belirtmek için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="304b7-138">It's valid to specify that a folder of pages requires authorization and then specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="304b7-139">Ancak, tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="304b7-139">The reverse, however, isn't valid.</span></span> <span data-ttu-id="304b7-140">Anonim erişim için bir sayfa klasörü bildiremezsiniz ve ardından bu klasör içinde yetkilendirme gerektiren bir sayfa belirtemezsiniz:</span><span class="sxs-lookup"><span data-stu-id="304b7-140">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="304b7-141">Özel sayfada yetkilendirme gerektirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="304b7-141">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="304b7-142">Sayfaya <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ve <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> her ikisi de uygulandığında <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> önceliklidir ve erişimi denetler.</span><span class="sxs-lookup"><span data-stu-id="304b7-142">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="304b7-143">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="304b7-143">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="304b7-144">Razor Pages uygulamanızda erişimi denetlemeye yönelik bir yol, başlangıçta yetkilendirme kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="304b7-144">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="304b7-145">Bu kurallar, kullanıcıları yetkilendirmeniz ve anonim kullanıcıların ayrı sayfalara veya sayfa klasörlerine erişmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="304b7-145">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="304b7-146">Bu konu başlığı altında açıklanan kurallar, erişimi denetlemek için otomatik olarak [Yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) uygular.</span><span class="sxs-lookup"><span data-stu-id="304b7-146">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="304b7-147">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="304b7-147">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="304b7-148">Örnek uygulama [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını](xref:security/authentication/cookie)kullanır.</span><span class="sxs-lookup"><span data-stu-id="304b7-148">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="304b7-149">Bu konu başlığında gösterilen kavramlar ve örnekler ASP.NET Core kimliği kullanan uygulamalar için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="304b7-149">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="304b7-150">ASP.NET Core kimliği kullanmak için <xref:security/authentication/identity>yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="304b7-150">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="304b7-151">Bir sayfaya erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="304b7-151">Require authorization to access a page</span></span>

<span data-ttu-id="304b7-152">Belirtilen yoldaki sayfaya bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-152">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="304b7-153">Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-153">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="304b7-154">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek Için bir [authorizepage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-154">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="304b7-155">Bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, `[Authorize]` Filter özniteliğiyle bir sayfa modeli sınıfına uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="304b7-155">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="304b7-156">Daha fazla bilgi için bkz. [Yetkilendirme filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="304b7-156">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="304b7-157">Bir sayfa klasörüne erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="304b7-157">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="304b7-158">Belirtilen yoldaki bir klasördeki tüm sayfalara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-158">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="304b7-159">Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-159">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="304b7-160">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [authorizefolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-160">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="304b7-161">Alan sayfasına erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="304b7-161">Require authorization to access an area page</span></span>

<span data-ttu-id="304b7-162">Belirtilen yoldaki alan sayfasına bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-162">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="304b7-163">Sayfa adı, belirtilen alanın sayfalar kök dizinine göre uzantısı olmayan dosyanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-163">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="304b7-164">Örneğin, dosya *alanı/Identity/Pages/Manage/accounts. cshtml* için sayfa adı */Manage/accounts*olur.</span><span class="sxs-lookup"><span data-stu-id="304b7-164">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="304b7-165">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareapage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-165">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="304b7-166">Bir alan klasörüne erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="304b7-166">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="304b7-167">Belirtilen yoldaki bir klasördeki tüm alanlara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-167">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="304b7-168">Klasör yolu, belirtilen alanın sayfalar kök dizinine göre klasörün yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-168">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="304b7-169">Örneğin, *bölgeler/kimlik/sayfalar/Yönet/* \ *Yönet*altındaki dosyalar için klasör yolu.</span><span class="sxs-lookup"><span data-stu-id="304b7-169">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="304b7-170">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareafolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-170">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="304b7-171">Bir sayfaya anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="304b7-171">Allow anonymous access to a page</span></span>

<span data-ttu-id="304b7-172">Belirtilen yoldaki bir sayfaya <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-172">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="304b7-173">Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-173">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="304b7-174">Bir sayfa klasörüne anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="304b7-174">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="304b7-175">Belirtilen yoldaki bir klasördeki tüm sayfalara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> üzerinden <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> kuralını kullanın:</span><span class="sxs-lookup"><span data-stu-id="304b7-175">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="304b7-176">Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="304b7-176">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="304b7-177">Yetkili ve anonim erişimi birleştirme hakkında</span><span class="sxs-lookup"><span data-stu-id="304b7-177">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="304b7-178">Yetkilendirme gerektiren bir sayfa klasörünün ve bu klasörün içindeki bir sayfanın adsız erişime izin verdiğini belirtmek için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="304b7-178">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="304b7-179">Ancak, tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="304b7-179">The reverse, however, isn't valid.</span></span> <span data-ttu-id="304b7-180">Anonim erişim için bir sayfa klasörü bildiremezsiniz ve ardından bu klasör içinde yetkilendirme gerektiren bir sayfa belirtemezsiniz:</span><span class="sxs-lookup"><span data-stu-id="304b7-180">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="304b7-181">Özel sayfada yetkilendirme gerektirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="304b7-181">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="304b7-182">Sayfaya <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ve <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> her ikisi de uygulandığında <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> önceliklidir ve erişimi denetler.</span><span class="sxs-lookup"><span data-stu-id="304b7-182">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="304b7-183">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="304b7-183">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
