---
title: ASP.NET Core Razor sayfalar yetkilendirme kuralları
author: guardrex
description: Kullanıcılara yetki vermek ve anonim kullanıcıların sayfa veya sayfaları klasör erişmesine izin veren kuralları ile sayfalarına erişimi denetlemeyi öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207270"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="9c612-103">ASP.NET Core Razor sayfalar yetkilendirme kuralları</span><span class="sxs-lookup"><span data-stu-id="9c612-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="9c612-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9c612-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9c612-105">Razor sayfaları uygulamanıza erişimi denetlemek için bir yolu, başlangıçta yetkilendirme kurallarını kullanmaktır.</span><span class="sxs-lookup"><span data-stu-id="9c612-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="9c612-106">Bu kuralları kullanıcılara yetki vermek ve anonim kullanıcıların tek tek sayfaları veya klasörleri sayfaların erişmesine olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9c612-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="9c612-107">Otomatik olarak bu konuda açıklanan kurallarını uygulamak [yetkilendirme filtrelerini](xref:mvc/controllers/filters#authorization-filters) erişimi denetleme.</span><span class="sxs-lookup"><span data-stu-id="9c612-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="9c612-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9c612-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9c612-109">Örnek uygulama kullandığı [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="9c612-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="9c612-110">Sabit kodlanmış uygulamasına kuramsal Maria Rodriguez, kullanıcı için kullanıcı hesabıdır.</span><span class="sxs-lookup"><span data-stu-id="9c612-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="9c612-111">E-posta kullanıcı adını kullanın "maria.rodriguez@contoso.com" ve kullanıcı oturum açmak için herhangi bir parola.</span><span class="sxs-lookup"><span data-stu-id="9c612-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="9c612-112">Kullanıcının kimliğinin `AuthenticateUser` yönteminde *Pages/Account/Login.cshtml.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="9c612-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="9c612-113">Gerçek hayatta kullanılan örnekte, bir veritabanında kullanıcı kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="9c612-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="9c612-114">ASP.NET Core kimliği kullanmak için sunulan yönergeleri [ASP.NET Core kimliği giriş](xref:security/authentication/identity) konu.</span><span class="sxs-lookup"><span data-stu-id="9c612-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="9c612-115">Bu konuda gösterilen örnekler ve kavramlar eşit olarak ASP.NET Core kimliği kullanan uygulamalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9c612-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="9c612-116">Bir sayfaya erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="9c612-116">Require authorization to access a page</span></span>

<span data-ttu-id="9c612-117">Kullanım [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda sayfasına:</span><span class="sxs-lookup"><span data-stu-id="9c612-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="9c612-118">Belirtilen yol bir uzantı ve yalnızca ileri eğik çizgi içeren olmadan Razor sayfaları kök göreli yolu görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="9c612-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="9c612-119">Bir [AuthorizePage aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9c612-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="9c612-120">Bir `AuthorizeFilter` sayfa modeli sınıfı ile uygulanabilir `[Authorize]` filtre özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9c612-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="9c612-121">Daha fazla bilgi için [Authorize filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="9c612-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="9c612-122">Sayfaların bir klasöre erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="9c612-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="9c612-123">Kullanım [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda bir klasördeki tüm sayfalar için:</span><span class="sxs-lookup"><span data-stu-id="9c612-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="9c612-124">Belirtilen Razor sayfaları kök göreli yol görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="9c612-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="9c612-125">Bir [AuthorizeFolder aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9c612-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="9c612-126">Bir alan sayfasına erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="9c612-126">Require authorization to access an area page</span></span>

<span data-ttu-id="9c612-127">Kullanım [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda alanı sayfası:</span><span class="sxs-lookup"><span data-stu-id="9c612-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="9c612-128">Sayfa adı sayfaları kök dizinine göreli bir uzantısı olmadan dosya belirtilen alan için yoludur.</span><span class="sxs-lookup"><span data-stu-id="9c612-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="9c612-129">Örneğin, sayfa adı dosya için *Areas/Identity/Pages/Manage/Accounts.cshtml* olduğu */Yönet/hesapları*.</span><span class="sxs-lookup"><span data-stu-id="9c612-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="9c612-130">Bir [AuthorizeAreaPage aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9c612-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="9c612-131">Alanların bir klasöre erişmek için yetkilendirme gerektirir</span><span class="sxs-lookup"><span data-stu-id="9c612-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="9c612-132">Kullanım [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) tüm alanlarda belirtilen yolda bir klasör:</span><span class="sxs-lookup"><span data-stu-id="9c612-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="9c612-133">Belirtilen alan sayfaları kök dizinine göreli bir klasör yolu klasör yoludur.</span><span class="sxs-lookup"><span data-stu-id="9c612-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="9c612-134">Örneğin, dosyalar için klasör yolu *alanlar/kimlik/sayfaları/Yönet/* olduğu */yönetme*.</span><span class="sxs-lookup"><span data-stu-id="9c612-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="9c612-135">Bir [AuthorizeAreaFolder aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9c612-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="9c612-136">Bir sayfaya anonim erişime izin ver</span><span class="sxs-lookup"><span data-stu-id="9c612-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="9c612-137">Kullanım [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir sayfaya:</span><span class="sxs-lookup"><span data-stu-id="9c612-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="9c612-138">Belirtilen yol bir uzantı ve yalnızca ileri eğik çizgi içeren olmadan Razor sayfaları kök göreli yolu görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="9c612-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="9c612-139">Sayfaların bir klasörde anonim erişime izin verin</span><span class="sxs-lookup"><span data-stu-id="9c612-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="9c612-140">Kullanım [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir klasördeki tüm sayfalar için:</span><span class="sxs-lookup"><span data-stu-id="9c612-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="9c612-141">Belirtilen Razor sayfaları kök göreli yol görünüm altyapısı yoludur.</span><span class="sxs-lookup"><span data-stu-id="9c612-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="9c612-142">Yetkili birleştirme ilgili not ve anonim erişim</span><span class="sxs-lookup"><span data-stu-id="9c612-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="9c612-143">Bir klasör sayfaların yetkilendirme gerektirir ve bu klasördeki bir sayfa anonim erişime izin verdiğini belirtin belirtmek için mükemmel bir şekilde geçerlidir:</span><span class="sxs-lookup"><span data-stu-id="9c612-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="9c612-144">Ters ancak geçerli değildir.</span><span class="sxs-lookup"><span data-stu-id="9c612-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="9c612-145">Sayfaların anonim erişim için bir klasör bildirmek olamaz ve içinde bir sayfa için yetkilendirme belirtin:</span><span class="sxs-lookup"><span data-stu-id="9c612-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="9c612-146">Özel sayfasında yetkilendirme gerektiren çalışmaz çünkü zaman hem `AllowAnonymousFilter` ve `AuthorizeFilter` filtreleri sayfaya uygulanır `AllowAnonymousFilter` WINS ve erişimi denetler.</span><span class="sxs-lookup"><span data-stu-id="9c612-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c612-147">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="9c612-147">Additional resources</span></span>

* [<span data-ttu-id="9c612-148">Razor sayfaları özel rota ve sayfa modeli sağlayıcılar</span><span class="sxs-lookup"><span data-stu-id="9c612-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="9c612-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) sınıfı</span><span class="sxs-lookup"><span data-stu-id="9c612-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
