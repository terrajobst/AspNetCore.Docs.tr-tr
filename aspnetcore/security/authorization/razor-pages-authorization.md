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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core Razor sayfalarının yetkilendirme kuralları

Tarafından [Luke Latham](https://github.com/guardrex)

Razor sayfalarının uygulamanıza erişimi denetlemek için bir yolu, başlangıçta yetkilendirme kurallarını kullanmaktır. Bu kuralları kullanıcılarına yetki verme ve anonim kullanıcıların tek tek sayfa veya sayfaları klasörleri erişmesine izin verin. Bu konuda açıklanan otomatik olarak kurallar geçerlidir [yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) erişimi denetlemek.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulama kullandığı [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması](xref:security/authentication/cookie). Uygulamada kodlanmış Maria Rodriguez kuramsal kullanıcı için kullanıcı hesabıdır. E-posta kullanıcı adı kullanma "maria.rodriguez@contoso.com" ve kullanıcıyla oturum açmak için herhangi bir parola. Kullanıcı, kimlik doğrulaması `AuthenticateUser` yönteminde *Pages/Account/Login.cshtml.cs* dosya. Gerçek dünya örnekte, bir veritabanına karşı kullanıcı kimlik doğrulaması. ASP.NET Core kimlik kullanmak üzere yönergeleri [kimliği ASP.NET Core üzerinde giriş](xref:security/authentication/identity) konu. Eşit oranda kavramları ve bu konuda gösterilen örnekleri ASP.NET Core kimliği kullanan uygulamalar için geçerlidir.

## <a name="require-authorization-to-access-a-page"></a>Bir sayfaya erişmek için yetkilendirme gerektirir

Kullanım [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda sayfasına:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Belirtilen yol bir uzantı ve yalnızca eğik içeren olmadan Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.

Bir [AuthorizePage aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Bir `AuthorizeFilter` bir sayfa modeli sınıfla uygulanabilir `[Authorize]` filtre özniteliği. Daha fazla bilgi için bkz: [Authorize filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Sayfaların bir klasöre erişim yetkisi gerektirir

Kullanım [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda bir klasördeki tüm sayfalar için:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Belirtilen yol Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.

Bir [AuthorizeFolder aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.

## <a name="allow-anonymous-access-to-a-page"></a>Bir sayfa anonim erişilebilsin

Kullanım [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir sayfaya:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Belirtilen yol bir uzantı ve yalnızca eğik içeren olmadan Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Sayfaların bir klasörde anonim erişime izin verin

Kullanım [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir klasördeki tüm sayfalar için:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Belirtilen yol Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Yetkili birleştirme göz önünde bulundurun ve anonim erişim

Bir klasör sayfaların yetkilendirme gerektirir ve bu klasördeki bir sayfa anonim erişime izin veren belirtir mükemmel geçerlidir:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Ters ancak, doğru değil. Sayfaların anonim erişim için bir klasör bildirme ve yetkilendirme için içinde bir sayfa belirtin:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Özel sayfasında yetkilendirme gerektiren çalışmayacak çünkü olduğunda hem `AllowAnonymousFilter` ve `AuthorizeFilter` filtreleri sayfaya uygulanan `AllowAnonymousFilter` WINS ve erişimi kontrol eder.

## <a name="additional-resources"></a>Ek kaynaklar

* [Razor sayfalarının özel yolu ve sayfayı model sağlayıcıları](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) sınıfı
