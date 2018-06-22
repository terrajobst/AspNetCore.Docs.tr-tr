---
title: ASP.NET Core Razor sayfalarının yetkilendirme kuralları
author: guardrex
description: Kullanıcıları yetkilendirmek ve anonim kullanıcıların sayfa veya sayfaları klasör erişmesine izin veren kuralları sayfalarıyla erişimi denetlemek öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 8856520bf43f2f62cc12c7e883485babdb43fb3e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272681"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="1fef8-103">ASP.NET Core Razor sayfalarının yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="1fef8-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="1fef8-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1fef8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1fef8-105">Razor sayfalarının uygulamanıza erişimi denetlemek için bir yolu, başlangıçta yetkilendirme kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="1fef8-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="1fef8-106">Bu kuralları kullanıcılarına yetki verme ve anonim kullanıcıların tek tek sayfa veya sayfaları klasörleri erişmesine izin verin.</span><span class="sxs-lookup"><span data-stu-id="1fef8-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="1fef8-107">Bu konuda açıklanan otomatik olarak kurallar geçerlidir [yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) erişimi denetlemek.</span><span class="sxs-lookup"><span data-stu-id="1fef8-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="1fef8-108">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1fef8-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1fef8-109">Örnek uygulama kullandığı [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="1fef8-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="1fef8-110">Uygulamada kodlanmış Maria Rodriguez kuramsal kullanıcı için kullanıcı hesabıdır.</span><span class="sxs-lookup"><span data-stu-id="1fef8-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="1fef8-111">E-posta kullanıcı adı kullanma "maria.rodriguez@contoso.com" ve kullanıcıyla oturum açmak için herhangi bir parola.</span><span class="sxs-lookup"><span data-stu-id="1fef8-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="1fef8-112">Kullanıcı, kimlik doğrulaması `AuthenticateUser` yönteminde *Pages/Account/Login.cshtml.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="1fef8-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="1fef8-113">Gerçek dünya örnekte, bir veritabanına karşı kullanıcı kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="1fef8-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="1fef8-114">ASP.NET Core kimlik kullanmak üzere yönergeleri [kimliği ASP.NET Core üzerinde giriş](xref:security/authentication/identity) konu.</span><span class="sxs-lookup"><span data-stu-id="1fef8-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="1fef8-115">Eşit oranda kavramları ve bu konuda gösterilen örnekleri ASP.NET Core kimliği kullanan uygulamalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="1fef8-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="1fef8-116">Bir sayfaya erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="1fef8-116">Require authorization to access a page</span></span>

<span data-ttu-id="1fef8-117">Kullanım [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda sayfasına:</span><span class="sxs-lookup"><span data-stu-id="1fef8-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="1fef8-118">Belirtilen yol bir uzantı ve yalnızca eğik içeren olmadan Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.</span><span class="sxs-lookup"><span data-stu-id="1fef8-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="1fef8-119">Bir [AuthorizePage aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="1fef8-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="1fef8-120">Bir `AuthorizeFilter` bir sayfa modeli sınıfla uygulanabilir `[Authorize]` filtre özniteliği.</span><span class="sxs-lookup"><span data-stu-id="1fef8-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="1fef8-121">Daha fazla bilgi için bkz: [Authorize filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="1fef8-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="1fef8-122">Sayfaların bir klasöre erişim yetkisi gerektirir</span><span class="sxs-lookup"><span data-stu-id="1fef8-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="1fef8-123">Kullanım [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda bir klasördeki tüm sayfalar için:</span><span class="sxs-lookup"><span data-stu-id="1fef8-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="1fef8-124">Belirtilen yol Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.</span><span class="sxs-lookup"><span data-stu-id="1fef8-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="1fef8-125">Bir [AuthorizeFolder aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="1fef8-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="1fef8-126">Bir sayfa anonim erişilebilsin</span><span class="sxs-lookup"><span data-stu-id="1fef8-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="1fef8-127">Kullanım [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir sayfaya:</span><span class="sxs-lookup"><span data-stu-id="1fef8-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="1fef8-128">Belirtilen yol bir uzantı ve yalnızca eğik içeren olmadan Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.</span><span class="sxs-lookup"><span data-stu-id="1fef8-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="1fef8-129">Sayfaların bir klasörde anonim erişime izin verin</span><span class="sxs-lookup"><span data-stu-id="1fef8-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="1fef8-130">Kullanım [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir klasördeki tüm sayfalar için:</span><span class="sxs-lookup"><span data-stu-id="1fef8-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="1fef8-131">Belirtilen yol Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.</span><span class="sxs-lookup"><span data-stu-id="1fef8-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="1fef8-132">Yetkili birleştirme göz önünde bulundurun ve anonim erişim</span><span class="sxs-lookup"><span data-stu-id="1fef8-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="1fef8-133">Bir klasör sayfaların yetkilendirme gerektirir ve bu klasördeki bir sayfa anonim erişime izin veren belirtir mükemmel geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="1fef8-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="1fef8-134">Ters ancak, doğru değil.</span><span class="sxs-lookup"><span data-stu-id="1fef8-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="1fef8-135">Sayfaların anonim erişim için bir klasör bildirme ve yetkilendirme için içinde bir sayfa belirtin:</span><span class="sxs-lookup"><span data-stu-id="1fef8-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="1fef8-136">Özel sayfasında yetkilendirme gerektiren çalışmayacak çünkü olduğunda hem `AllowAnonymousFilter` ve `AuthorizeFilter` filtreleri sayfaya uygulanan `AllowAnonymousFilter` WINS ve erişimi kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="1fef8-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1fef8-137">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="1fef8-137">Additional resources</span></span>

* [<span data-ttu-id="1fef8-138">Razor sayfalarının özel yolu ve sayfayı model sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="1fef8-138">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="1fef8-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) sınıfı</span><span class="sxs-lookup"><span data-stu-id="1fef8-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
