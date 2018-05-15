---
title: ASP.NET Core Razor sayfalarının yetkilendirme kuralları
author: guardrex
description: Kullanıcıları yetkilendirmek ve anonim kullanıcıların sayfa veya sayfaları klasör erişmesine izin veren kuralları sayfalarıyla erişimi denetlemek öğrenin.
manager: wpickett
ms.author: riande
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 2fd8cd444b1d774c387dc6426af5914bde9b8ae7
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="e6b02-103">ASP.NET Core Razor sayfalarının yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="e6b02-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="e6b02-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e6b02-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e6b02-105">Razor sayfalarının uygulamanıza erişimi denetlemek için bir yolu, başlangıçta yetkilendirme kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="e6b02-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="e6b02-106">Bu kuralları kullanıcılarına yetki verme ve anonim kullanıcıların tek tek sayfa veya sayfaları klasörleri erişmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="e6b02-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="e6b02-107">Bu konuda açıklanan otomatik olarak kurallar geçerlidir [yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) erişimi denetlemek.</span><span class="sxs-lookup"><span data-stu-id="e6b02-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="e6b02-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e6b02-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="e6b02-109">Bir sayfaya erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="e6b02-109">Require authorization to access a page</span></span>

<span data-ttu-id="e6b02-110">Kullanım [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda sayfasına:</span><span class="sxs-lookup"><span data-stu-id="e6b02-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="e6b02-111">Belirtilen yol bir uzantı ve yalnızca eğik içeren olmadan Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.</span><span class="sxs-lookup"><span data-stu-id="e6b02-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="e6b02-112">Bir [AuthorizePage aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e6b02-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="e6b02-113">Sayfaların bir klasöre erişim yetkisi gerektirir</span><span class="sxs-lookup"><span data-stu-id="e6b02-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="e6b02-114">Kullanım [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda bir klasördeki tüm sayfalar için:</span><span class="sxs-lookup"><span data-stu-id="e6b02-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="e6b02-115">Belirtilen yol Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.</span><span class="sxs-lookup"><span data-stu-id="e6b02-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="e6b02-116">Bir [AuthorizeFolder aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e6b02-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="e6b02-117">Bir sayfa anonim erişilebilsin</span><span class="sxs-lookup"><span data-stu-id="e6b02-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="e6b02-118">Kullanım [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir sayfaya:</span><span class="sxs-lookup"><span data-stu-id="e6b02-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="e6b02-119">Belirtilen yol bir uzantı ve yalnızca eğik içeren olmadan Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.</span><span class="sxs-lookup"><span data-stu-id="e6b02-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="e6b02-120">Sayfaların bir klasörde anonim erişime izin verin</span><span class="sxs-lookup"><span data-stu-id="e6b02-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="e6b02-121">Kullanım [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir klasördeki tüm sayfalar için:</span><span class="sxs-lookup"><span data-stu-id="e6b02-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="e6b02-122">Belirtilen yol Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.</span><span class="sxs-lookup"><span data-stu-id="e6b02-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="e6b02-123">Yetkili birleştirme göz önünde bulundurun ve anonim erişim</span><span class="sxs-lookup"><span data-stu-id="e6b02-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="e6b02-124">Bir klasör sayfaların yetkilendirme gerektirir ve bu klasördeki bir sayfa anonim erişime izin veren belirtir mükemmel geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="e6b02-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="e6b02-125">Ters ancak, doğru değil.</span><span class="sxs-lookup"><span data-stu-id="e6b02-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="e6b02-126">Sayfaların anonim erişim için bir klasör bildirme ve yetkilendirme için içinde bir sayfa belirtin:</span><span class="sxs-lookup"><span data-stu-id="e6b02-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="e6b02-127">Özel sayfasında yetkilendirme gerektiren çalışmayacak çünkü olduğunda hem `AllowAnonymousFilter` ve `AuthorizeFilter` filtreleri sayfaya uygulanan `AllowAnonymousFilter` WINS ve erişimi kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="e6b02-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="e6b02-128">Ayrıca bkz.</span><span class="sxs-lookup"><span data-stu-id="e6b02-128">See also</span></span>

* [<span data-ttu-id="e6b02-129">Razor sayfalarının özel yolu ve sayfayı model sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="e6b02-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* <span data-ttu-id="e6b02-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) sınıfı</span><span class="sxs-lookup"><span data-stu-id="e6b02-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
