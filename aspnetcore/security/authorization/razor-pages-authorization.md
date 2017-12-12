---
title: "ASP.NET Core Razor sayfalarının yetkilendirme kuralları"
author: guardrex
description: "Kullanıcıları yetkilendirmek ve anonim kullanıcıların tek tek sayfa veya sayfaları klasörleri erişmesine izin veren kuralları başlangıçta sayfalarıyla erişimi denetlemek öğrenin."
ms.author: riande
manager: wpickett
ms.date: 10/27/2017
ms.topic: article
ms.assetid: f65ad22d-9472-478a-856c-c59c8681fa71
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 36acf3c06a462882972c5f389d544d98cadc35f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core Razor sayfalarının yetkilendirme kuralları

Tarafından [Luke Latham](https://github.com/guardrex)

Razor sayfalarının uygulamanıza erişimi denetlemek için bir yolu, başlangıçta yetkilendirme kurallarını kullanmaktır. Bu kuralları kullanıcılarına yetki verme ve anonim kullanıcıların tek tek sayfa veya sayfaları klasörleri erişmesine izin verin. Bu konuda açıklanan otomatik olarak kurallar geçerlidir [yetkilendirme filtreleri](xref:mvc/controllers/filters#authorization-filters) erişimi denetlemek.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="require-authorization-to-access-a-page"></a>Bir sayfaya erişmek için yetkilendirme gerektirir

Kullanım [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda sayfasına:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

Belirtilen yol bir uzantı ve yalnızca eğik içeren olmadan Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.

Bir [AuthorizePage aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Sayfaların bir klasöre erişim yetkisi gerektirir

Kullanım [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yolda bir klasördeki tüm sayfalar için:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

Belirtilen yol Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.

Bir [AuthorizeFolder aşırı](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) kullanılabilmesi için bir yetkilendirme ilkesi belirtmeniz gerekir.

## <a name="allow-anonymous-access-to-a-page"></a>Bir sayfa anonim erişilebilsin

Kullanım [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir sayfaya:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

Belirtilen yol bir uzantı ve yalnızca eğik içeren olmadan Razor sayfalarının kök göreli yolu görünüm altyapısı yoldur.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Sayfaların bir klasörde anonim erişime izin verin

Kullanım [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) kuralı aracılığıyla [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) eklemek için bir [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yolda bir klasördeki tüm sayfalar için:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

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

## <a name="see-also"></a>Ayrıca bkz.

* [Razor sayfalarının özel yolu ve sayfayı model sağlayıcıları](xref:mvc/razor-pages/razor-pages-convention-features)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) sınıfı
