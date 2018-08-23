---
title: ASP.NET Core Razor sayfalar yetkilendirme kuralları
author: guardrex
description: Kullanıcılara yetki vermek ve anonim kullanıcıların sayfa veya sayfaları klasör erişmesine izin veren kuralları ile sayfalarına erişimi denetlemeyi öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: d3ecb41765da912df68aeb829350d27e4d087e3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752594"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core Razor sayfalar yetkilendirme kuralları

Tarafından [Luke Latham](https://github.com/guardrex)

Razor sayfaları uygulamanıza erişimi denetlemek için bir yolu, başlangıçta yetkilendirme kurallarını kullanmaktır. Bu kuralları kullanıcılara yetki vermek ve anonim kullanıcıların tek tek sayfaları veya klasörleri sayfaların erişmesine olanak sağlar. Otomatik olarak bu konuda açıklanan kurallarını uygulamak [yetkilendirme filtrelerini](xref:mvc/controllers/filters#authorization-filters) erişimi denetleme.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Örnek uygulama kullandığı [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması](xref:security/authentication/cookie). Sabit kodlanmış uygulamasına kuramsal Maria Rodriguez, kullanıcı için kullanıcı hesabıdır. E-posta kullanıcı adını kullanın "maria.rodriguez@contoso.com" ve kullanıcı oturum açmak için herhangi bir parola. Kullanıcının kimliğinin `AuthenticateUser` yönteminde *Pages/Account/Login.cshtml.cs* dosya. Gerçek hayatta kullanılan örnekte, bir veritabanında kullanıcı kimlik doğrulaması. ASP.NET Core kimliği kullanmak için sunulan yönergeleri [ASP.NET Core kimliği giriş](xref:security/authentication/identity) konu. Bu konuda gösterilen örnekler ve kavramlar eşit olarak ASP.NET Core kimliği kullanan uygulamalar için geçerlidir.

## <a name="require-authorization-to-access-a-page"></a>Bir sayfaya erişmek için yetkilendirme gerektirir

Kullanım [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda sayfasına:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Belirtilen yol bir uzantı ve yalnızca ileri eğik çizgi içeren olmadan Razor sayfaları kök göreli yolu görünüm altyapısı yoludur.

Bir [AuthorizePage aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> Bir `AuthorizeFilter` sayfa modeli sınıfı ile uygulanabilir `[Authorize]` filtre özniteliği. Daha fazla bilgi için [Authorize filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Sayfaların bir klasöre erişmek için yetkilendirme gerektirir

Kullanım [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda bir klasördeki tüm sayfalar için:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Belirtilen Razor sayfaları kök göreli yol görünüm altyapısı yoludur.

Bir [AuthorizeFolder aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>Bir alan sayfasına erişmek için yetkilendirme gerektirir

Kullanım [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda alanı sayfası:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Sayfa adı sayfaları kök dizinine göreli bir uzantısı olmadan dosya belirtilen alan için yoludur. Örneğin, sayfa adı dosya için *Areas/Identity/Pages/Manage/Accounts.cshtml* olduğu */Yönet/hesapları*.

Bir [AuthorizeAreaPage aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Alanların bir klasöre erişmek için yetkilendirme gerektirir

Kullanım [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) tüm alanlarda belirtilen yolda bir klasör:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Belirtilen alan sayfaları kök dizinine göreli bir klasör yolu klasör yoludur. Örneğin, dosyalar için klasör yolu *alanlar/kimlik/sayfaları/Yönet/* olduğu */yönetme*.

Bir [AuthorizeAreaFolder aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>Bir sayfaya anonim erişime izin ver

Kullanım [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir sayfaya:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Belirtilen yol bir uzantı ve yalnızca ileri eğik çizgi içeren olmadan Razor sayfaları kök göreli yolu görünüm altyapısı yoludur.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Sayfaların bir klasörde anonim erişime izin verin

Kullanım [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir klasördeki tüm sayfalar için:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Belirtilen Razor sayfaları kök göreli yol görünüm altyapısı yoludur.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Yetkili birleştirme ilgili not ve anonim erişim

Bir klasör sayfaların yetkilendirme gerektirir ve bu klasördeki bir sayfa anonim erişime izin verdiğini belirtin belirtmek için mükemmel bir şekilde geçerlidir:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Ters ancak geçerli değildir. Sayfaların anonim erişim için bir klasör bildirmek olamaz ve içinde bir sayfa için yetkilendirme belirtin:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Özel sayfasında yetkilendirme gerektiren çalışmaz çünkü zaman hem `AllowAnonymousFilter` ve `AuthorizeFilter` filtreleri sayfaya uygulanır `AllowAnonymousFilter` WINS ve erişimi denetler.

## <a name="additional-resources"></a>Ek kaynaklar

* [Razor sayfaları özel rota ve sayfa modeli sağlayıcılar](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) sınıfı
