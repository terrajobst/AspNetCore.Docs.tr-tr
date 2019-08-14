---
title: ASP.NET Core Razor Pages yetkilendirme kuralları
author: guardrex
description: Kullanıcılara yetki veren ve anonim kullanıcıların sayfalara veya sayfa klasörlerine erişmesine izin veren kurallara sahip sayfalara erişimi denetleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: e0102ff64921a83f0330acb6f5d9bfd90f64ca7a
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994027"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="71001-103">ASP.NET Core Razor Pages yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="71001-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="71001-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="71001-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="71001-105">Razor Pages uygulamanızda erişimi denetlemeye yönelik bir yol, başlangıçta yetkilendirme kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="71001-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="71001-106">Bu kurallar, kullanıcıları yetkilendirmeniz ve anonim kullanıcıların ayrı sayfalara veya sayfa klasörlerine erişmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="71001-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="71001-107">Bu konu başlığı altında açıklanan kurallar, erişimi denetlemek için otomatik olarak [Yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) uygular.</span><span class="sxs-lookup"><span data-stu-id="71001-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="71001-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="71001-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="71001-109">Örnek uygulama [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını](xref:security/authentication/cookie)kullanır.</span><span class="sxs-lookup"><span data-stu-id="71001-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="71001-110">Bu konu başlığında gösterilen kavramlar ve örnekler ASP.NET Core kimliği kullanan uygulamalar için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="71001-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="71001-111">ASP.NET Core kimliği kullanmak için içindeki <xref:security/authentication/identity>yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="71001-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="71001-112">Bir sayfaya erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="71001-112">Require authorization to access a page</span></span>

<span data-ttu-id="71001-113">Belirtilen yoldaki sayfaya bir <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*></span><span class="sxs-lookup"><span data-stu-id="71001-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="71001-114">Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="71001-115">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek Için bir [authorizepage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="71001-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="71001-116">`[Authorize]` Filtre özniteliğiyle bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sayfa modeli sınıfına uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="71001-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="71001-117">Daha fazla bilgi için bkz. [Yetkilendirme filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="71001-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="71001-118">Bir sayfa klasörüne erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="71001-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="71001-119">Belirtilen yoldaki bir klasördeki <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> tüm sayfalara eklemek <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*></span><span class="sxs-lookup"><span data-stu-id="71001-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="71001-120">Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="71001-121">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [authorizefolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="71001-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="71001-122">Alan sayfasına erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="71001-122">Require authorization to access an area page</span></span>

<span data-ttu-id="71001-123">Belirtilen yoldaki alan sayfasına <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*></span><span class="sxs-lookup"><span data-stu-id="71001-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="71001-124">Sayfa adı, belirtilen alanın sayfalar kök dizinine göre uzantısı olmayan dosyanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="71001-125">Örneğin, dosya *alanı/Identity/Pages/Manage/accounts. cshtml* için sayfa adı */Manage/accounts*olur.</span><span class="sxs-lookup"><span data-stu-id="71001-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="71001-126">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareapage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="71001-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="71001-127">Bir alan klasörüne erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="71001-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="71001-128">Belirtilen yoldaki bir klasördeki <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> tüm alanlara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*></span><span class="sxs-lookup"><span data-stu-id="71001-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="71001-129">Klasör yolu, belirtilen alanın sayfalar kök dizinine göre klasörün yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="71001-130">Örneğin, *bölgeler/kimlik/sayfalar/Yönet/* \ *Yönet*altındaki dosyalar için klasör yolu.</span><span class="sxs-lookup"><span data-stu-id="71001-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="71001-131">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareafolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="71001-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="71001-132">Bir sayfaya anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="71001-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="71001-133">Belirtilen yoldaki bir sayfaya <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*></span><span class="sxs-lookup"><span data-stu-id="71001-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="71001-134">Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="71001-135">Bir sayfa klasörüne anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="71001-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="71001-136">Belirtilen yoldaki bir klasördeki <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> tüm sayfalara eklemek <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*></span><span class="sxs-lookup"><span data-stu-id="71001-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="71001-137">Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="71001-138">Yetkili ve anonim erişimi birleştirme hakkında</span><span class="sxs-lookup"><span data-stu-id="71001-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="71001-139">Yetkilendirme gerektiren bir sayfa klasörünün ve bu klasörün içindeki bir sayfanın adsız erişime izin verdiğini belirtmek için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="71001-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="71001-140">Ancak, tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="71001-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="71001-141">Anonim erişim için bir sayfa klasörü bildiremezsiniz ve ardından bu klasör içinde yetkilendirme gerektiren bir sayfa belirtemezsiniz:</span><span class="sxs-lookup"><span data-stu-id="71001-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="71001-142">Özel sayfada yetkilendirme gerektirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="71001-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="71001-143"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Ve sayfayaherikisideuygulandığında,,öncelikalırveerişimidenetler.<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter></span><span class="sxs-lookup"><span data-stu-id="71001-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71001-144">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="71001-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="71001-145">Razor Pages uygulamanızda erişimi denetlemeye yönelik bir yol, başlangıçta yetkilendirme kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="71001-145">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="71001-146">Bu kurallar, kullanıcıları yetkilendirmeniz ve anonim kullanıcıların ayrı sayfalara veya sayfa klasörlerine erişmesine izin verir.</span><span class="sxs-lookup"><span data-stu-id="71001-146">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="71001-147">Bu konu başlığı altında açıklanan kurallar, erişimi denetlemek için otomatik olarak [Yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) uygular.</span><span class="sxs-lookup"><span data-stu-id="71001-147">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="71001-148">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="71001-148">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="71001-149">Örnek uygulama [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını](xref:security/authentication/cookie)kullanır.</span><span class="sxs-lookup"><span data-stu-id="71001-149">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="71001-150">Bu konu başlığında gösterilen kavramlar ve örnekler ASP.NET Core kimliği kullanan uygulamalar için eşit oranda geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="71001-150">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="71001-151">ASP.NET Core kimliği kullanmak için içindeki <xref:security/authentication/identity>yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="71001-151">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="71001-152">Bir sayfaya erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="71001-152">Require authorization to access a page</span></span>

<span data-ttu-id="71001-153">Belirtilen yoldaki sayfaya bir <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*></span><span class="sxs-lookup"><span data-stu-id="71001-153">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="71001-154">Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-154">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="71001-155">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek Için bir [authorizepage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="71001-155">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="71001-156">`[Authorize]` Filtre özniteliğiyle bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sayfa modeli sınıfına uygulanabilir.</span><span class="sxs-lookup"><span data-stu-id="71001-156">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="71001-157">Daha fazla bilgi için bkz. [Yetkilendirme filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="71001-157">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="71001-158">Bir sayfa klasörüne erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="71001-158">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="71001-159">Belirtilen yoldaki bir klasördeki <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> tüm sayfalara eklemek <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*></span><span class="sxs-lookup"><span data-stu-id="71001-159">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="71001-160">Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-160">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="71001-161">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [authorizefolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="71001-161">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="71001-162">Alan sayfasına erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="71001-162">Require authorization to access an area page</span></span>

<span data-ttu-id="71001-163">Belirtilen yoldaki alan sayfasına <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*></span><span class="sxs-lookup"><span data-stu-id="71001-163">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="71001-164">Sayfa adı, belirtilen alanın sayfalar kök dizinine göre uzantısı olmayan dosyanın yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-164">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="71001-165">Örneğin, dosya *alanı/Identity/Pages/Manage/accounts. cshtml* için sayfa adı */Manage/accounts*olur.</span><span class="sxs-lookup"><span data-stu-id="71001-165">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="71001-166">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareapage aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="71001-166">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="71001-167">Bir alan klasörüne erişmek için yetkilendirme gerektir</span><span class="sxs-lookup"><span data-stu-id="71001-167">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="71001-168">Belirtilen yoldaki bir klasördeki <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> tüm alanlara bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> eklemek için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*></span><span class="sxs-lookup"><span data-stu-id="71001-168">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="71001-169">Klasör yolu, belirtilen alanın sayfalar kök dizinine göre klasörün yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-169">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="71001-170">Örneğin, *bölgeler/kimlik/sayfalar/Yönet/* \ *Yönet*altındaki dosyalar için klasör yolu.</span><span class="sxs-lookup"><span data-stu-id="71001-170">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="71001-171">Bir [Yetkilendirme İlkesi](xref:security/authorization/policies)belirtmek için, bir [Authorizeareafolder aşırı yüklemesi](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)kullanın:</span><span class="sxs-lookup"><span data-stu-id="71001-171">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="71001-172">Bir sayfaya anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="71001-172">Allow anonymous access to a page</span></span>

<span data-ttu-id="71001-173">Belirtilen yoldaki bir sayfaya <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> eklemek için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*></span><span class="sxs-lookup"><span data-stu-id="71001-173">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="71001-174">Belirtilen yol, uzantısı olmayan ve yalnızca eğik çizgi içeren Razor Pages kök göreli yolu olan görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-174">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="71001-175">Bir sayfa klasörüne anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="71001-175">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="71001-176">Belirtilen yoldaki bir klasördeki <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> tüm sayfalara eklemek <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> için aracılığıyla kuralınıkullanın:<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*></span><span class="sxs-lookup"><span data-stu-id="71001-176">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="71001-177">Belirtilen yol Razor Pages kök göreli yolu olan görüntüleme altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="71001-177">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="71001-178">Yetkili ve anonim erişimi birleştirme hakkında</span><span class="sxs-lookup"><span data-stu-id="71001-178">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="71001-179">Yetkilendirme gerektiren bir sayfa klasörünün ve bu klasörün içindeki bir sayfanın adsız erişime izin verdiğini belirtmek için geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="71001-179">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="71001-180">Ancak, tersi de geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="71001-180">The reverse, however, isn't valid.</span></span> <span data-ttu-id="71001-181">Anonim erişim için bir sayfa klasörü bildiremezsiniz ve ardından bu klasör içinde yetkilendirme gerektiren bir sayfa belirtemezsiniz:</span><span class="sxs-lookup"><span data-stu-id="71001-181">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="71001-182">Özel sayfada yetkilendirme gerektirme başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="71001-182">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="71001-183"><xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> Ve sayfayaherikisideuygulandığında,,öncelikalırveerişimidenetler.<xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter></span><span class="sxs-lookup"><span data-stu-id="71001-183">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71001-184">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="71001-184">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
