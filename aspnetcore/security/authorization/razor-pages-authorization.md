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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>ASP.NET Core Razor sayfalar yetkilendirme kuralları

Tarafından [Luke Latham](https://github.com/guardrex)

Razor sayfaları uygulamanıza erişimi denetlemek için bir yolu, başlangıçta yetkilendirme kurallarını kullanmaktır. Bu kuralları kullanıcılara yetki vermek ve anonim kullanıcıların tek tek sayfaları veya klasörleri sayfaların erişmesine olanak sağlar. Otomatik olarak bu konuda açıklanan kurallarını uygulamak [yetkilendirme filtrelerini](xref:mvc/controllers/filters#authorization-filters) erişimi denetleme.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Örnek uygulama kullandığı [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması](xref:security/authentication/cookie). Bu konuda gösterilen örnekler ve kavramlar eşit olarak ASP.NET Core kimliği kullanan uygulamalar için geçerlidir. ASP.NET Core kimliği kullanmak için sunulan yönergeleri <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Bir sayfaya erişmek için yetkilendirme gerektirir

Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> belirtilen yolda sayfasına:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Belirtilen yol bir uzantı ve yalnızca ileri eğik çizgi içeren olmadan Razor sayfaları kök göreli yolu görünüm altyapısı yoludur.

Belirtmek için bir [yetkilendirme ilkesi](xref:security/authorization/policies), kullanan bir [AuthorizePage aşırı](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sayfa modeli sınıfı ile uygulanabilir `[Authorize]` filtre özniteliği. Daha fazla bilgi için [Authorize filtre özniteliği](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Sayfaların bir klasöre erişmek için yetkilendirme gerektirir

Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> belirtilen yolda bir klasördeki tüm sayfalar için:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Belirtilen Razor sayfaları kök göreli yol görünüm altyapısı yoludur.

Belirtmek için bir [yetkilendirme ilkesi](xref:security/authorization/policies), kullanan bir [AuthorizeFolder aşırı](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Bir alan sayfasına erişmek için yetkilendirme gerektirir

Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> belirtilen yolda alanı sayfası:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Sayfa adı sayfaları kök dizinine göreli bir uzantısı olmadan dosya belirtilen alan için yoludur. Örneğin, sayfa adı dosya için *Areas/Identity/Pages/Manage/Accounts.cshtml* olduğu */Yönet/hesapları*.

Belirtmek için bir [yetkilendirme ilkesi](xref:security/authorization/policies), kullanan bir [AuthorizeAreaPage aşırı](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Alanların bir klasöre erişmek için yetkilendirme gerektirir

Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> tüm alanlarda belirtilen yolda bir klasör:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Belirtilen alan sayfaları kök dizinine göreli bir klasör yolu klasör yoludur. Örneğin, dosyalar için klasör yolu *alanlar/kimlik/sayfaları/Yönet/* olduğu */yönetme*.

Belirtmek için bir [yetkilendirme ilkesi](xref:security/authorization/policies), kullanan bir [AuthorizeAreaFolder aşırı](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Bir sayfaya anonim erişime izin ver

Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> belirtilen yolda bir sayfaya:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Belirtilen yol bir uzantı ve yalnızca ileri eğik çizgi içeren olmadan Razor sayfaları kök göreli yolu görünüm altyapısı yoludur.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Sayfaların bir klasörde anonim erişime izin verin

Kullanım <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> kuralı aracılığıyla <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> eklemek için bir <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> belirtilen yolda bir klasördeki tüm sayfalar için:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Belirtilen Razor sayfaları kök göreli yol görünüm altyapısı yoludur.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Yetkili birleştirme ilgili not ve anonim erişim

Belirtmek için geçerli olan bir klasörü yetkilendirme gerektiren sayfaları ve o klasördeki bir sayfa anonim erişime izin verdiğini belirtin:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Ancak bunun tersi geçerli değil. Sayfaların anonim erişim için bir klasör bildirmek olamaz ve sonra bir sayfa yetkilendirme gerektirir, klasörün içindeki belirtin:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Özel sayfasında gerektiren yetkilendirme başarısız olur. Hem <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ve <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> sayfaya uygulanan <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> önceliklidir ve erişimi denetler.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
